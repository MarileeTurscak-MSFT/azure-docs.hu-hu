---
title: Az Azure IoT Edge + Windows rövid útmutatója | Microsoft Docs
description: Az Azure IoT Edge kipróbálása elemzés futtatásával egy szimulált Edge-eszközön
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/02/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3b54a326fc648a443897a6e39c823d9c097cf1d3
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626382"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Rövid útmutató: Az első IoT Edge-modul üzembe helyezése az Azure Portal segítségével egy Windows-eszközön – előzetes verzió

Ebben a rövid útmutatóban előre összeállított kódokat fog távolról üzembe helyezni egy IoT Edge-eszközön az Azure IoT Edge felhőalapú interfészével. A feladat teljesítése érdekében először a Windows-eszközzel szimulálni fogja az IoT Edge-eszközt, amelyen aztán üzembe fog helyezni egy modult.

Ennek a rövid útmutatónak a segítségével megtanulhatja az alábbiakat:

1. IoT Hub létrehozása
2. IoT Edge-eszköz regisztrálása az IoT Hubon
3. Az IoT Edge-futtatókörnyezet telepítése és elindítása az eszközén.
4. Modul távoli üzembe helyezése IoT Edge-eszközön és Telemetria küldése az IoT Hubnak

![Bemutató architektúra][2]

A jelen rövid útmutatóban üzembe helyezett modul egy szimulált érzékelő, amely hőmérséklet-, páratartalom- és nyomásadatokat állít elő. A további Azure IoT Edge-oktatóanyagok az itt elvégzett munkára építkeznek olyan modulok üzembe helyezésével, amelyek a szimulált adatok elemzésével üzleti megállapításokat hoznak létre. 

>[!NOTE]
>Windows rendszeren az IoT Edge-futtatókörnyezet [nyilvános előzetes verzióban](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) érhető el.

Ha nem rendelkezik aktív Azure-előfizetéssel, kezdetnek hozzon létre egy [ingyenes fiókot][lnk-account].

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

A rövid útmutató számos lépésében az Azure CLI használatára van szükség, és az Azure IoT egyik bővítményével további funkciókhoz férhet hozzá. 

Adja hozzá az Azure IoT bővítményt a Cloud Shell-példányhoz.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Előfeltételek

Felhőerőforrások: 

* Egy erőforráscsoport a rövid útmutató során létrehozott összes erőforrás kezelésére. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

IoT Edge-eszköz: 

* Egy Windows rendszerű számítógép vagy virtuális gép, amely IoT Edge-eszközként szolgál majd. Használjon támogatott Windows-verziót:
   * Windows 10 vagy újabb
   * Windows Server 2016 vagy újabb
* Ha virtuális gépről van szó, engedélyezze a [beágyazott virtualizálást][lnk-nested], és foglaljon le legalább 2 GB memóriát. 
* Telepítse a [Windowshoz készült Dockert][lnk-docker], és ellenőrizze, hogy fut-e.

## <a name="create-an-iot-hub"></a>IoT Hub létrehozása

A rövid útmutató első lépéseként hozza létre az IoT Hubot az Azure CLI használatával. 

![IoT Hub létrehozása][3]

Ehhez a rövid útmutatóhoz az IoT Hub ingyenes csomagja is elegendő. Ha korábban már használta az IoT Hubot, és már létrehozott egy ingyenes központot, használhatja azt is. Mindegyik előfizetés csak egy ingyenes IoT-központtal rendelkezhet. 

A következő kód egy ingyenes **F1** központot hoz létre az **IoTEdgeResources** erőforráscsoportban. A *{hub_name}* elemet cserélje le az IoT Hub központ egyedi nevére.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Ha hibaüzenetet kap, mert az előfizetése már tartalmaz egy ingyenes központot, akkor módosítsa az SKU-t **S1**-re. 

## <a name="register-an-iot-edge-device"></a>IoT Edge-eszköz regisztrálása

Regisztráljon egy IoT Edge-eszközt az újonnan létrehozott IoT Hubon.
![Eszköz regisztrálása][4]

Hozzon létre egy eszközidentitást a szimulált eszközhöz, hogy az kommunikálhasson az IoT Hubbal. Az eszközidentitás a felhőben található, és egy egyedi eszközkapcsolati sztringgel társíthat fizikai eszközt az eszközidentitáshoz. 

Mivel az IoT Edge-eszközök másként viselkednek, mint a hagyományos IoT-eszközök, és kezelésük is másként történik, ezért IoT Edge-eszközként kell deklarálni a kezdetektől fogva. 

1. Az Azure Cloud Shellben a következő paranccsal hozza létre a **myEdgeDevice** nevű eszközt a központjában.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

1. Kérje le az eszköze kapcsolati sztringjét, amely összeköti a fizikai eszközt az IoT Hubban tárolt identitással. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Másolja és mentse a kapcsolati sztringet. Erre az értékre a következő szakaszban, az IoT Edge-futtatókörnyezet konfigurálásához lesz szükség. 

## <a name="install-and-start-the-iot-edge-runtime"></a>Az IoT Edge-futtatókörnyezet telepítése és elindítása

Telepítse az Azure IoT Edge-futtatókörnyezetet az IoT Edge-eszközön, és a konfigurálást eszközkapcsolati sztring használatával végezze el. 
![Eszköz regisztrálása][5]

Az IoT Edge-futtatókörnyezet minden IoT Edge-eszközön üzembe van helyezve. Három összetevőből áll. Az **IoT Edge biztonsági démon** az Edge-eszközök indulásakor lép működésbe, és az IoT Edge-ügynök elindításával elvégzi az eszköz rendszerindítását. Az **IoT Edge-ügynök** a modulok üzembe helyezését és monitorozását segíti az IoT Edge-eszközön, beleértve az IoT Edge-központot is. Az **IoT Edge-központ** az IoT Edge-eszközön lévő modulok, valamint az eszköz és az IoT Hub közötti kommunikációt kezeli. 

A futtatókörnyezet telepítése során a rendszer rá fog kérdezni az eszközkapcsolati sztringre. Ez esetben az Azure CLI-ről lekért sztringet használja. Ez a sztring társítja a fizikai eszközt az IoT Edge-eszköz identitásához az Azure-ban. 

Ebben a szakaszban az IoT Edge-futtatókörnyezet Linux-tárolókkal történő konfigurálásához talál útmutatást. Ha Windows-tárolókat kíván használni, tekintse meg az [Azure IoT Edge-futtatókörnyezet Windows rendszeren, Windows-tárolókhoz történő telepítését](how-to-install-iot-edge-windows-with-windows.md) ismertető cikket.

Hajtsa végre a következő lépéseket a Windows rendszerű számítógépen vagy az IoT Edge-eszközként előkészített virtuális gépen. 

### <a name="download-and-install-the-iot-edge-service"></a>Az IoT Edge-szolgáltatás letöltése és telepítése

Az IoT Edge-futtatókörnyezet letöltése és telepítése a PowerShell használatával történik. Az eszköz konfigurálásához az IoT Hubról lekért eszközkapcsolati sztringet használja. 

1. IoT Edge-eszközén futtassa a PowerShellt rendszergazdaként.

2. Töltse le és telepítse az IoT Edge-szolgáltatást az eszközre. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Linux
   ```

3. Ha a rendszer a **DeviceConnectionString** megadására kéri, akkor illessze be az előző szakaszból átmásolt sztringet. A kapcsolati sztring elé és után ne írjon idézőjeleket. 

### <a name="view-the-iot-edge-runtime-status"></a>Az IoT Edge-futtatókörnyezet állapotának megtekintése

Ellenőrizze, hogy a futtatókörnyezet megfelelően lett-e telepítve és konfigurálva.

1. Ellenőrizze az IoT Edge-szolgáltatás állapotát.

   ```powershell
   Get-Service iotedge
   ```

2. Ha hibaelhárításra van szükség, kérje le a szolgáltatás naplóit. 

   ```powershell
   # Displays logs from today, newest at the bottom.

   Get-WinEvent -ea SilentlyContinue `
    -FilterHashtable @{ProviderName= "iotedged";
      LogName = "application"; StartTime = [datetime]::Today} |
    select TimeCreated, Message |
    sort-object @{Expression="TimeCreated";Descending=$false} |
    format-table -autosize -wrap
   ```

3. Tekintse meg az IoT Edge-eszközön futó összes modult. Mivel első alkalommal indította el ezt a szolgáltatást, csak az **edgeAgent** modulnak szabad futnia. Az edgeAgent alapértelmezés szerint fut, és segíti az eszközön esetlegesen üzembe helyezett további modulok telepítését és indítását. 

   ```powershell
   iotedge list
   ```

   ![Egy modul megtekintése az eszközön](./media/quickstart/iotedge-list-1.png)

Ezzel konfigurálta az IoT Edge-eszközt. Az eszköz készen áll a felhőben üzembe helyezett modulok futtatására. 

## <a name="deploy-a-module"></a>Modul üzembe helyezése

Azure IoT Edge-eszközeit kezelheti a felhőből, és üzembe helyezhet egy olyan modult, amely telemetriaadatokat küld az IoT Hubra.
![Eszköz regisztrálása][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>A létrejött adatok megtekintése

Ebben a rövid útmutatóban létrehozott egy új IoT Edge-eszközt, és telepítette rajta az IoT Edge-futtatókörnyezetet. Ezután az Azure Portal segítségével úgy futtatta az IoT Edge-modult az eszközön, hogy magát az eszközt nem kellett módosítania. Ebben az esetben az Ön által továbbított modul az oktatóanyagokhoz használható környezeti adatokat hoz létre. 

Győződjön meg arról, hogy a felhőből üzembe helyezett modul fut az IoT Edge-eszközön. 

```powershell
iotedge list
```

   ![Három modul megtekintése az eszközön](./media/quickstart/iotedge-list-2.png)

Tekintse meg a tempSensor modul által a felhőbe küldött üzeneteket. 

```powershell
iotedge logs tempSensor -f
```

  ![A modulból származó adatok megtekintése](./media/quickstart/iotedge-logs.png)

Az IoT Hub által fogadott üzeneteket a [Visual Studio Code Azure IoT Toolkit bővítményével](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) is megtekintheti. 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha tovább szeretne dolgozni az IoT Edge-oktatóanyagokkal, használhatja az ebben a rövid útmutatóban regisztrált és létrehozott eszközt. Ha nem, törölheti a létrehozott Azure-erőforrásokat, és eltávolíthatja az IoT Edge-futtatókörnyezetet az eszközről. 

### <a name="delete-azure-resources"></a>Azure-erőforrások törlése

Ha a virtuális gépet és az IoT Hubot egy új erőforráscsoportban hozta létre, törölheti azt a csoportot és az összes társított erőforrást. Ha van valami abban az erőforráscsoportban, amit meg szeretne tartani, csak azokat a különálló erőforrásokat törölje, amelyektől meg szeretne szabadulni. 

Távolítsa el az **IoTEdgeResources** csoportot. 

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

### <a name="remove-the-iot-edge-runtime"></a>Az IoT Edge-futtatókörnyezet eltávolítása

Ha a jövőben tesztelésre szeretné használni az IoT Edge-eszközt, de nem szeretné, hogy a tempSensor modul adatokat küldjön az IoT Hubnak, amikor nincs használatban, a következő paranccsal állíthatja le az IoT Edge-szolgáltatást. 

   ```powershell
   Stop-Service iotedge -NoWait
   ```

Ha készen áll a tesztelés ismételt megkezdésére, újraindíthatja a szolgáltatást.

   ```powershell
   Start-Service iotedge
   ```

Ha el szeretné távolítani a telepítéseket az eszközéről, azt a következő parancsokkal teheti meg.  

Távolítsa el az IoT Edge-futtatókörnyezetet.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon
   ```

Ha eltávolította az IoT Edge-futtatókörnyezetet, az általa létrehozott tárolók leállnak, de továbbra is ott lesznek az eszközön. Tekintse meg az összes tárolót.

   ```powershell
   docker ps -a
   ```

Törölje azokat a tárolókat, amelyeket az IoT Edge-futtatókörnyezet hozott létre az eszközén. Ha más nevet adott neki, módosítsa a tempSensor tároló nevét. 

   ```powershell
   docker rm -f tempSensor
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```
   
## <a name="next-steps"></a>További lépések

Ebben a rövid útmutatóban létrehozott egy új IoT Edge-eszközt, és az Azure IoT Edge felhőalapú felülettel kódot telepített az eszközre. Most már van egy teszteszköze, amely nyers adatokat állít elő a környezetéről. 

Továbbléphet bármely másik oktatóanyagra, és megtudhatja, hogyan alakíthatja üzleti megállapításokká ezeket az adatokat a peremhálózaton az Azure IoT Edge segítségével.

> [!div class="nextstepaction"]
> [Érzékelők adatainak szűrése Azure Functions-függvényekkel](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png


<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az-iot-hub-delete


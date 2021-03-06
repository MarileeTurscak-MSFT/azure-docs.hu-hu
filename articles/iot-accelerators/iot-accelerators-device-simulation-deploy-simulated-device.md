---
title: IoT üzembe egyéni szimulált eszközök – Azure |} A Microsoft Docs
description: Ez az útmutató bemutatja, egyéni szimulált eszközök az Eszközszimuláció megoldásgyorsító üzembe helyezése.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/14/2018
ms.topic: conceptual
ms.openlocfilehash: e51d0330c3ed804bff8cdb06265bb8c39141733f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345062"
---
# <a name="deploy-a-new-simulated-device"></a>Új szimulált eszköz üzembe helyezése

A távoli figyelési és az Eszközszimuláció megoldásgyorsítók mindkét segítségével meghatározhatja a saját szimulált eszközök. Ez a cikk bemutatja az Eszközszimuláció megoldásgyorsító üzembe helyezése egy testre szabott hűtő eszköz típusa és a egy villanykörte új eszköztípus.

Ez a cikk lépései azt feltételezik, hogy befejezte a [létrehozása és a egy új szimulált eszköz teszt](iot-accelerators-remote-monitoring-create-simulated-device.md) útmutató útmutatót, és a fájlokat, amelyek meghatározzák a testre szabott hűtő és új villanykörte eszköz típusa.

Ez az útmutató lépései bemutatják, hogyan való:

1. Az SSH használata a fájlrendszer, a virtuális gép, amelyen az Eszközszimuláció megoldásgyorsító eléréséhez.

1. Állítsa be a Docker a Docker-tároló kívüli helyről az eszközmodell betölteni.

1. Egy egyéni modell-fájlok használatával szimuláció futtatása.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ez az útmutató a lépések elvégzéséhez, aktív Azure-előfizetés szükséges.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="prerequisites"></a>Előfeltételek

Ez az útmutató követéséhez lesz szüksége:

- Egy telepített példányát az [Eszközszimuláció megoldásgyorsító](https://www.azureiotsolutions.com/Accelerators#solutions/types/DS).
- A helyi **bash** rendszerhéj futtatásához a `ssh` és `scp` parancsokat. A Windows, egyszerűen telepítse **bash** telepítése [git](https://git-scm.com/download/win).
- Az egyéni modell fájlok, a ismertetettekhez [létrehozása és a egy új szimulált eszköz teszt](iot-accelerators-remote-monitoring-create-simulated-device.md).

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="configure-docker"></a>Docker konfigurálása

Ebben a szakaszban a Docker betölteni az eszköz modellje a konfigurálható a **/tmp/devicemodels** a virtuális gépen, nem pedig a mappa a Docker-tárolóban. Futtassa a parancsokat az ebben a szakaszban egy **bash** héjastól a helyi gépen:

1. Az SSH használatával csatlakozhat a virtuális gép az Azure-ban, a helyi gépen. Az alábbi parancs feltételezi, hogy a virtuális gép nyilvános IP-cím **vm-vikxv** van **104.41.128.108** – ezt az értéket cserélje le az előző szakaszban a virtuális gép nyilvános IP-címét:

   ```sh
    ssh azureuser@104.41.128.108
    ```

    Kövesse az utasításokat követve jelentkezzen be a virtuális gépet az előző szakaszban állítsa be a jelszót.

1. Az eszköz szimuláció szolgáltatás számára, hogy az eszköz modellek a tárolón kívül konfigurálja. Először nyissa meg a Docker-konfigurációs fájl:

    ```sh
    sudo nano /app/docker-compose.yml
    ```

    Keresse meg a beállításokat a a **devicesimulation** tároló és a Szerkesztés a **kötetek** beállítása az alábbi kódrészletben látható módon:

    ```yml
    # To mount custom device models, upload the files to the VM, edit and uncomment the following line:
          - /tmp/devicemodels:/app/webservice/data/devicemodels:ro
    # To mount a custom service configuration, create a custom file, edit and uncomment the following line:
    #     - /app/custom-device-simulation-appsettings.ini:/app/webservice/appsettings.ini:ro
    ```

    Mentse a módosításokat.

1. Hozza létre a JSON-t és a parancsfájl-fájlok tárolására szolgáló mappákat:

    ```sh
    sudo mkdir -p /tmp/devicemodels/scripts
    ```

    Tartsa a **bash** nyissa meg az SSH-munkamenet-ablakot.

1. Másolja az egyéni modell-fájlt a virtuális gép. Futtassa ezt a parancsot egy másik **bash** héjastól a számítógépen, ahol létrehozta a egyéni modellek. Először keresse meg a helyi mappát, amely az eszköz modellje JSON-fájlokat tartalmazza. Az alábbi parancs feltételezi, hogy a virtuális gép nyilvános IP-cím **104.41.128.108** – ezt az értéket cserélje le a virtuális gép nyilvános IP-címét. Adja meg a virtuális gép jelszavát, amikor a rendszer kéri:

    ```sh
    scp *json azureuser@104.41.128.108:/tmp/devicemodels
    cd scripts
    scp *js azureuser@104.41.128.108:/tmp/devicemodels/scripts
    ```

1. Indítsa újra az eszköz szimulálása Docker-tárolót az új eszköz modellek használata. Futtassa a következő parancsokat a **bash** héjastól együtt az open SSH-munkamenetből a virtuális géphez:

    ```sh
    sudo /app/start.sh
    ```

    Ha azt szeretné, megtekintheti a futó Docker-tárolók és a tároló azonosítók állapotát, használja a következő parancsot:

    ```sh
    docker ps
    ```

    Ha azt szeretné, az eszköz szimulálása a tárolóból a napló megtekintéséhez futtassa a következő parancsot. Cserélje le a tároló azonosítója az eszköz szimulálása tároló Azonosítóját:

    ```sh
    docker logs -f 5d3f3e78822e
    ```

## <a name="run-simulation"></a>Szimuláció futtatása

A szimuláció-eszközök egyéni modellek használatával most már futtathatja:

1. Indítsa el az Eszközszimuláció webes felhasználói felülete a [a Microsoft Azure IoT-Megoldásgyorsítók](https://www.azureiotsolutions.com/Accelerators#dashboard).

1. A webes felhasználói felületének használatával konfigurálhatja és a egy egyéni eszköz modelljeit egyikével szimuláció futtatása.

1. Ha a szimuláció futtatja, kattintson a **megtekintése az IoT Hub-mérőszámok az Azure Portalon** figyelése a szimuláció. Is használhatja az Azure CLI-parancsot az eszköz a felhőalapú forgalom figyelésére. Cserélje le az IoT hub nevét a központ neve:

    ```azurecli-interactive
    az iot hub monitor-events -n contoso-simulation9dc75
    ```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha tovább szeretne ismerkedni az eszközzel, hagyja üzembe helyezve az Eszközszimulációs megoldásgyorsítót.

Ha már nincs szüksége a megoldásgyorsító, törölje azt a [kiépített megoldások](https://www.azureiotsolutions.com/Accelerators#dashboard) lapon jelölje ki, majd **megoldás törlése**.

## <a name="next-steps"></a>További lépések

Ez az útmutató bemutatta az Eszközszimuláció megoldásgyorsító egyéni modellek üzembe helyezése. A javasolt következő lépésre az, hogy tudjon meg többet a [eszközmodell sémájának](iot-accelerators-device-simulation-device-schema.md).

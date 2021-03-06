---
title: Linux virtuális gép létrehozása az Azure CLI-vel az Azure Stackben |} A Microsoft Docs
description: Linux virtuális gép létrehozása a CLI használatával az Azure Stackben.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 6fa68fcc4732241b30609f476a09643e458065db
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377360"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Gyors útmutató: Linux rendszerű kiszolgáló virtuális gép létrehozása az Azure Stack az Azure parancssori felület használatával

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Létrehozhat egy Ubuntu Server 16.04 LTS virtuális gépet az Azure parancssori felület használatával. Kövesse a cikkben hozhat létre és használhat egy virtuális gépet. Ez a cikk is biztosít a lépéseket:

* Csatlakozzon a virtuális géphez a távoli ügyfélhez.
* Az NGINX-webkiszolgálót, és az alapértelmezett kezdőlap megtekintéséhez.
* A fel nem használt erőforrások törlése.

## <a name="prerequisites"></a>Előfeltételek

* **Egy Linuxos rendszerképet az Azure Stack piactéren**

   Az Azure Stack piactéren alapértelmezés szerint nem tartalmaz a Linux-rendszerképen. Adja meg az Azure Stack-operátorokról lekérése a **Ubuntu Server 16.04 LTS** rendszerképre van szüksége. Az operátor leírt lépéseket követve használhat a [Piactéri termékek letöltése az Azure-ból az Azure Stackhez](../azure-stack-download-azure-marketplace-item.md) cikk.

* Az Azure Stack egy adott verzióját az Azure CLI használatával az erőforrások létrehozásához és kezeléséhez szükséges. Ha nem rendelkezik az Azure CLI, Azure Stack konfigurált, jelentkezzen be a [development Kitet](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), vagy egy Windows-alapú külső ügyfél Ha [VPN-kapcsolaton keresztül csatlakozó](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) , és kövesse a lépéseket a [ Telepítse és konfigurálja az Azure CLI-vel](azure-stack-version-profiles-azurecli2.md).

* Egy nyilvános SSH-kulcsot a név id_rsa.pub mentése a Windows felhasználói profilja .ssh könyvtárában. Az SSH-kulcsok létrehozásával kapcsolatos részletes információkért lásd: [létrehozása SSH-kulcsok Windows](../../virtual-machines/linux/ssh-from-windows.md).

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Egy erőforráscsoport olyan logikai tároló, ahol üzembe helyezése és kezelése az Azure Stack-erőforrások. A fejlesztői készlet vagy az Azure Stack integrált rendszerek, futtassa a [az csoport létrehozása](/cli/azure/group#az-group-create) parancs használatával hozzon létre egy erőforráscsoportot.

>[!NOTE]
 Hitelesítésikód-példák az összes változót értékek vannak hozzárendelve. Megadhat azonban új értéket, ha szeretné.

Az alábbi példa egy helyi helyen myResourceGroup nevű erőforráscsoportot hoz létre.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

A virtuális gép létrehozása a [az virtuális gép létrehozása](/cli/azure/vm#az-vm-create) parancsot. Az alábbi példában a myVM nevű virtuális Gépet hoz létre. Ebben a példában használja Bemutatofelhasznalo rendszergazdanevet és Demouser@123 felhasználói jelszót. Ezeket az értékeket módosítsa úgy, hogy válasszon a környezetének megfelelő.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "UbuntuLTS" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

A nyilvános IP-címet adja vissza a **PublicIpAddress** paraméter. Jegyezze fel ezt a címet, mert szüksége lesz rá a virtuális gép eléréséhez.

## <a name="open-port-80-for-web-traffic"></a>A 80-as port megnyitása a webes adatforgalom számára

Mivel a virtuális gép futtatása az IIS-webkiszolgálón történik, meg kell nyitnia az internetes forgalmat a 80-as porton. A kívánt port megnyitásához használja az [az vm open-port](/cli/azure/vm#open-port) parancsot.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="use-ssh-to-connect-to-the-virtual-machine"></a>Csatlakozzon a virtuális géphez SSH-val

Az ssh-val telepített egy ügyfélszámítógépen a csatlakozás a virtuális géphez. Ha dolgozik, a Windows-ügyfélen, [Putty](http://www.putty.org/) a kapcsolat létrehozásához. A virtuális géphez csatlakozni, használja a következő parancsot:

```bash
ssh <publicIpAddress>
```

## <a name="install-the-nginx-web-server"></a>Az NGINX-webkiszolgáló telepítése

Csomag erőforrásokat, és telepítse a legújabb NGINX-csomagot, futtassa a következő szkriptet:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>Az NGINX kezdőlapjának megtekintése

Az NGINX telepítve van, és nyissa meg a virtuális gépen a 80-as porton a webkiszolgáló, a virtuális gép nyilvános IP-cím használatával is elérheti. Nyisson meg egy webböngészőt, és keresse meg a ```http://<public IP address>```.

![Az NGINX web server kezdőlap](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Az erőforrásokat, amelyeket többé nem kell törölni. Használhatja a [az csoport törlése](/cli/azure/group#az-group-delete) paranccsal eltávolítható az ezekhez az erőforrásokhoz. Az erőforráscsoport és az ahhoz tartozó összes erőforrás törléséhez futtassa a következő parancsot:

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>További lépések

Ez a rövid útmutatóban üzembe helyezett egy egyszerű Linux server virtuális géphez a webkiszolgáló. További információ a virtuális gépekről az Azure Stack, továbbra is [szempontok a virtuális gépek az Azure Stackben](azure-stack-vm-considerations.md).

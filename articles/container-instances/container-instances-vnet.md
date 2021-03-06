---
title: Egy Azure-beli virtuális hálózatban a tárolópéldányok üzembe helyezése
description: Ismerje meg, hogyan tárolócsoportok egy új vagy meglévő Azure virtuális hálózaton üzembe helyezni.
services: container-instances
author: mmacy
ms.service: container-instances
ms.topic: article
ms.date: 09/24/2018
ms.author: marsma
ms.openlocfilehash: bce1de5536eb26b48132bd3642eb780043e76231
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047590"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>Egy Azure-beli virtuális hálózatban a tárolópéldányok üzembe helyezése

[Az Azure Virtual Network](../virtual-network/virtual-networks-overview.md) nyújt biztonságos, privát hálózati, beleértve a szűrési, Útválasztás és társviszony-létesítést az Azure és helyszíni erőforrások. Tárolócsoportok üzembe egy Azure-beli virtuális hálózatban, a tárolók képesek kommunikálni biztonságosan a virtuális hálózatban lévő más erőforrásokra.

Egy Azure-beli virtuális hálózatban üzembe helyezett tárolócsoportok olyan szituációkra, mint engedélyezése:

* Az azonos alhálózaton található tárolócsoportok közötti közvetlen kapcsolat
* Küldés [feladatalapú](container-instances-restart-policy.md) számítási feladat kimenete container Instances szolgáltatásban a virtuális hálózatban lévő adatbázishoz
* A container Instances tartalom lekérése egy [szolgáltatásvégpont](../virtual-network/virtual-network-service-endpoints-overview.md) a virtuális hálózatban
* Tároló kommunikál a virtuális hálózatban lévő virtuális gépek
* Tároló kommunikál a helyszíni erőforrásokhoz keresztül egy [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md) vagy [ExpressRoute](../expressroute/expressroute-introduction.md)

> [!IMPORTANT]
> Ez a funkció jelenleg előzetes verzióban érhető el, és néhány [korlátozások érvényesek a](#preview-limitations). Az előzetes verziók azzal a feltétellel érhetők el, hogy Ön beleegyezik a [kiegészítő használati feltételekbe][terms-of-use]. A szolgáltatás néhány eleme megváltozhat a nyilvános rendelkezésre állás előtt.

## <a name="virtual-network-deployment-limitations"></a>Virtuális hálózat üzembe helyezési korlátozásoknak

Bizonyos korlátozások érvényesek, amikor üzembe helyezi a tárolócsoportok egy virtuális hálózatot.

* Windows-tárolók nem támogatottak.
* Tárolócsoportok telepíteni egy alhálózathoz, az alhálózat nem tartalmazhat más erőforrástípusok. Az összes meglévő erőforrások eltávolítása előtt tárolócsoportok hozzá egy meglévő alhálózatot, vagy hozzon létre egy új alhálózatot.
* Tárolócsoportok üzembe helyezni egy virtuális hálózatban jelenleg nem támogatják nyilvános IP-címe vagy DNS-név címke.
* További hálózati erőforrás, mert egy tárolócsoport telepítése egy virtuális hálózathoz általában némileg lassabb, mint a standard szintű tárolópéldány üzembe helyezése.

## <a name="preview-limitations"></a>Előzetes verzió korlátozásai

Bár ez a funkció előzetes verzióban érhető el, az alábbi korlátozások érvényesek a példányok üzembe helyezésekor tárolót egy virtuális hálózathoz.

**Támogatott** régiók:

* Nyugat-Európa (westeurope)
* USA nyugati RÉGIÓJA (westus)

**Nem támogatott** hálózati erőforrások:

* Hálózati biztonsági csoport
* Azure Load Balancer

**Hálózati erőforrás törlése** igényel [további lépéseket](#delete-network-resources) után tárolócsoportok helyezte a virtuális hálózathoz.

## <a name="required-network-resources"></a>Szükséges hálózati erőforrások

Három Azure Virtual Network erőforrás tárolócsoportok üzembe helyezéséhez a virtual Network szükséges: a [virtuális hálózati](#virtual-network) , egy [alhálózati delegált](#subnet-delegated) belül a virtuális hálózat és a egy [hálózati profil](#network-profile).

### <a name="virtual-network"></a>Virtuális hálózat

Virtuális hálózat határozza meg a címtartományt, amely egy vagy több alhálózatot hoz létre. Majd Azure-erőforrások (például a tárolócsoportok) üzembe helyezése az alhálózatok a virtuális hálózaton.

### <a name="subnet-delegated"></a>Alhálózat (delegált)

Alhálózatok, külön címterek használható a virtuális hálózat szegmentáljon az Azure-erőforrások bennük. Egy virtuális hálózaton belül egy vagy több alhálózatot hoz létre.

A tárolócsoportok használt alhálózat csak tárolócsoportok tartalmazhat. Első üzembe egy tárolócsoport egy alhálózathoz, az Azure ad az Azure Container Instances alhálózat. Meghatalmazott, miután az alhálózat csak a tárolócsoportok használható. Ha eltérő tárolócsoportok erőforrások üzembe helyezése a delegált alhálózathoz kísérli meg, a művelet sikertelen lesz.

### <a name="network-profile"></a>Hálózati profil

Hálózati profil a hálózati konfigurációs sablon az Azure-erőforrásokhoz. Azt adja meg az erőforrás, például az alhálózat, amelybe azt kell telepíteni, bizonyos hálózati tulajdonságok. Először egy tárolócsoport egy alhálózat (és így egy virtuális hálózat) üzembe helyezése, Azure hálózati profilt hoz létre az Ön számára. Ezután használhatja a hálózati profilban a későbbiekben az alhálózathoz.

Az alábbi ábrán több tárolóból álló csoportok az Azure Container Instances delegált alhálózathoz van telepítve. Miután üzembe helyezte egy tárolócsoport egy alhálózathoz, az azonos hálózati profil megadásával további tárolócsoportok telepítheti azt.

![Tárolócsoportok egy virtuális hálózaton belül][aci-vnet-01]

## <a name="deploy-to-virtual-network"></a>Virtuális hálózat üzembe helyezése

Tárolócsoportok egy új virtuális hálózaton üzembe helyezni, és engedélyezni az Azure az Ön számára a szükséges hálózati erőforrások létrehozása, vagy egy meglévő virtuális hálózaton üzembe helyezni.

### <a name="new-virtual-network"></a>Új virtuális hálózat

Egy új virtuális hálózaton üzembe helyezni, és automatikusan az Ön számára a hálózati erőforrások létrehozása az Azure rendelkezik, adja meg a következő végrehajtásakor [az tároló létrehozása][az-container-create]:

* Virtuális hálózat neve
* Virtuális hálózat címelőtagjához CIDR formátumban
* Alhálózat neve
* Alhálózat címelőtagot CIDR-formátumban

A virtuális hálózat és alhálózat címelőtagok a címterek a virtuális hálózatot és alhálózatot, illetve adja meg. Ezek az értékek szerepelnek az Classless Inter-Domain Routing (CIDR) formátumban, például `10.0.0.0/16`. Alhálózatok használatáról további információkért lásd: [hozzáadása, módosítása vagy törlése egy virtuális hálózat alhálózatához](../virtual-network/virtual-network-manage-subnet.md).

Miután üzembe helyezte az első tárolócsoport ezzel a módszerrel, telepíthet ugyanahhoz az alhálózathoz a virtuális hálózat és alhálózatok nevének, vagy az Azure automatikusan létrehozza az Ön számára a hálózati profil megadásával. Azure ad Azure Container Instances szolgáltatásban az alhálózaton, mert telepíthet *csak* tárolócsoportok az alhálózathoz.

### <a name="existing-virtual-network"></a>Meglévő virtuális hálózat

Az egy meglévő virtuális hálózaton üzembe helyezni egy tárolócsoport:

1. Hozzon létre egy alhálózatot a meglévő virtuális hálózaton belül, vagy egy meglévő alhálózat az üres *összes* más erőforrásokhoz
1. Üzembe helyezése egy tárolócsoportot [az tároló létrehozása] [ az-container-create] , és adja meg a következők egyikét:
   * Virtuális hálózat és alhálózat neve</br>
    vagy
   * Hálózati profil neve vagy azonosítója

Miután telepít egy meglévő alhálózat az első tárolócsoport, Azure ad az Azure Container Instances alhálózat. Már nem telepíthet tárolócsoportok naplóátvitelen kívüli egyéb erőforrásokra arra az alhálózatra.

A következő szakaszok ismertetik, hogyan helyezhet üzembe egy virtuális hálózatot az Azure CLI-vel tárolócsoportok. A parancspéldákban vannak formázva a **Bash** rendszerhéjat. Ha inkább egy másik, például a PowerShell vagy az parancssor rendszerhéj, ennek megfelelően módosítsa a folytatási karakterek.

## <a name="deploy-to-new-virtual-network"></a>Új virtuális hálózat üzembe helyezése

Először üzembe helyezése egy tárolócsoportot, és adjon meg egy új virtuális hálózatot és alhálózatot a paramétereket. Amikor megadja ezeket a paramétereket, az Azure a virtuális hálózatot és alhálózatot hoz létre erőforrásához biztosít az Azure Container Instances szolgáltatásban az alhálózat és is létrehoz egy hálózati profilt. Ezek az erőforrások létrejönnek, miután a tárolócsoport helyezünk üzembe az alhálózathoz.

Futtassa a következő [az tároló létrehozása] [ az-container-create] parancsot, amely egy új virtuális hálózatot és alhálózatot beállítást határoz meg. Ez a parancs üzembe helyezi a [microsoft/aci-helloworld] [ aci-helloworld] tároló, amely egy statikus weblapot kiszolgáló kis Node.js-webkiszolgálót futtat. A következő szakaszban fog üzembe helyezése egy második tárolócsoport ugyanahhoz az alhálózathoz, és a két tárolót a példányok közötti kommunikáció tesztelése.

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image microsoft/aci-helloworld \
    --vnet-name aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

Amikor telepít egy új virtuális hálózatra a metódus segítségével, az üzembe helyezés eltarthat néhány percig során jönnek létre a hálózati erőforrásokhoz. A kezdeti telepítés után további csoportban üzembe helyezett gyorsabban befejezéséhez.

## <a name="deploy-to-existing-virtual-network"></a>Meglévő virtuális hálózaton üzembe helyezni

Most, hogy telepítette a tárolócsoport egy új virtuális hálózatra, üzembe helyezése egy második tárolócsoport ugyanahhoz az alhálózathoz, és a két tárolót a példányok közötti kommunikáció ellenőrzéséhez.

Először kérje le az első tároló csoportot helyezett üzembe, IP-címét a *appcontainerben*:

```azurecli
az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
```

A kimenet a privát alhálózatra kell megjelenítenie a tárolócsoport IP-címét:

```console
$ az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
10.0.0.4
```

Most állítsa `CONTAINER_GROUP_IP` az IP-címhez, akkor kérhető le a `az container show` parancsot, majd hajtsa végre az alábbiakat `az container create` parancsot. A második tárolót *commchecker*, Alpine Linux-alapú lemezkép fut, és végrehajtja a `wget` szemben az első csoport tároló privát alhálózati IP-címet.

```azurecli
CONTAINER_GROUP_IP=<container-group-IP-here>

az container create \
    --resource-group myResourceGroup \
    --name commchecker \
    --image alpine:3.5 \
    --command-line "wget $CONTAINER_GROUP_IP" \
    --restart-policy never \
    --vnet-name aci-vnet \
    --subnet aci-subnet
```

A második tárolót üzembe helyezés befejeztével lekéréses a naplókat, így láthatja a kimenetét a `wget` , a végrehajtott parancs:

```azurecli
az container logs --resource-group myResourceGroup --name commchecker
```

Ha a második tárolót sikerült – az első, kimeneti hasonló lesz:

```console
$ az container logs --resource-group myResourceGroup --name commchecker
Connecting to 10.0.0.4 (10.0.0.4:80)
index.html           100% |*******************************|  1663   0:00:00 ETA
```

A kimenet kell megjelennie, amely `wget` tudta csatlakozhat, és töltse le az index fájlt az első tároló privát IP-címének használatával a helyi alhálózaton. A két tárolócsoportok közötti hálózati forgalmat a virtuális hálózaton belüli maradt.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

### <a name="delete-container-instances"></a>Tárolópéldányok törlése

Elkészült a container instances használata létrehozott, törölje mindkét az alábbi parancsokkal:

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
```

### <a name="delete-network-resources"></a>Hálózati erőforrások törlése

A kezdeti előzetes verzióját a funkció számos további parancsok a hálózati erőforrások törli a korábban létrehozott van szükség. Ha ez a cikk korábbi szakaszaiban a Példaparancsok használatban a virtuális hálózatot és alhálózatot létrehozni, majd használhatja a következő parancsfájl hálózati erőforrások törlése.

Parancsfájl végrehajtása előtt állítsa be a `RES_GROUP` változó neve lesz az erőforráscsoport, amely tartalmazza a virtuális hálózatot és alhálózatot, amelyet törölni kell. A szkript a Bash rendszerhéj van formázva. Ha inkább egy másik, például a PowerShell vagy az parancssor rendszerhéj, szüksége változó-hozzárendelés és leíró ennek megfelelően módosítsa.

> [!WARNING]
> Ez a parancsfájl törli az erőforrást! Törli a virtuális hálózat és az összes alhálózatot tartalmaz. Győződjön meg, hogy már nincs szüksége *bármely* az erőforrások a virtuális hálózatban, beleértve az esetleges olyan alhálózatokra, tartalmaz, ez a szkript futtatása előtt. Törölt, **ezeket az erőforrásokat is helyreállíthatatlan**.

```azurecli
# Replace <my-resource-group> with the name of your resource group
RES_GROUP=<my-resource-group>

# Get network profile ID
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Get the service association link (SAL) ID
SAL_ID=$(az network vnet subnet show --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --query id --output tsv)/providers/Microsoft.ContainerInstance/serviceAssociationLinks/default

# Delete the default SAL ID for the subnet
az resource delete --ids $SAL_ID --api-version 2018-07-01

# Delete the subnet delegation to Azure Container Instances
az network vnet subnet update --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --remove delegations 0

# Delete the subnet
az network vnet subnet delete --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet

# Delete virtual network
az network vnet delete --resource-group $RES_GROUP --name aci-vnet
```

## <a name="next-steps"></a>További lépések

Több virtuális hálózati erőforrások és szolgáltatások ebben a cikkben aktorcsoportot tárgyalt, ha rövid időre. Az Azure Virtual Network dokumentációja nagymértékben mutatja be az alábbi témakörök:

* [Virtuális hálózat](../virtual-network/manage-virtual-network.md)
* [Alhálózat](../virtual-network/virtual-network-manage-subnet.md)
* [Szolgáltatásvégpontok](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [ExpressRoute](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create

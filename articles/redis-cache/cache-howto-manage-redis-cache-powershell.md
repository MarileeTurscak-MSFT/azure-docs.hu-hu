---
title: Az Azure PowerShell-lel az Azure Redis Cache kezelése |} A Microsoft Docs
description: Ismerje meg, hogyan hajthat végre felügyeleti feladatokat Azure PowerShell-lel az Azure Redis Cache-hez.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: wesmc
ms.openlocfilehash: 980a183261c394bd83292170ab133fe17229013d
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/16/2018
ms.locfileid: "42059148"
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Az Azure PowerShell-lel az Azure Redis Cache kezelése
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
> 
> 

Ez a témakör bemutatja, hogyan hajtsa végre a gyakori feladatok például létrehozása, frissítése és méretezése az Azure Redis Cache-példány hozzáférési kulcsok újragenerálásával és a gyorsítótárak kapcsolatos információk megtekintése. Azure Redis Cache PowerShell-parancsmagok teljes listáját lásd: [Azure Redis Cache parancsmagok](https://docs.microsoft.com/powershell/module/azurerm.rediscache/?view=azurermps-6.6.0).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

A klasszikus üzemi modellel kapcsolatos további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellek és az erőforrások állapotának ismertetése](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="prerequisites"></a>Előfeltételek
Ha már telepítette az Azure PowerShell-lel, rendelkeznie kell Azure PowerShell-lel 1.0.0-s verziójának vagy újabb. Ellenőrizheti az Azure PowerShell, amelyen telepítve van a következő paranccsal, az Azure PowerShell-parancssorba verzióját.

    Get-Module azure | format-table version


Először be kell jelentkezni Azure az alábbi paranccsal.

    Connect-AzureRmAccount

A Microsoft Azure bejelentkezési párbeszédpanel, adja meg az e-mail-címét az Azure-fiókkal és annak jelszavát.

Ezután ha több Azure-előfizetéssel rendelkezik, akkor kell Azure-előfizetés beállítása. Az aktuális előfizetések listájának megtekintéséhez futtassa ezt a parancsot.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Válassza ki az előfizetést, futtassa a következő parancsot. A következő példában, az előfizetés nevét a `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Használatához Windows PowerShell az Azure Resource Manager, a következők szükségesek:

* Windows PowerShell 3.0 vagy 4.0-s verzióját. A Windows PowerShell-verzió megkereséséhez írja be a következőt:`$PSVersionTable` , és ellenőrizze a értékét `PSVersion` 3.0 vagy 4.0-s verzióját. Telepítenie egy kompatibilis verziót, tekintse meg a [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) vagy [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Ebben az oktatóanyagban látja parancsmagokhoz részletes segítséget kérhet, használja a Get-Help parancsmagot.

    Get-Help <cmdlet-name> -Detailed

Segítség kérése az a példában a `New-AzureRmRedisCache` parancsmagot, írja be:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a>Hogyan lehet csatlakozni más felhőkben
Az Azure alapértelmezés szerint a környezete `AzureCloud`, amely a globális Azure-felhő szolgáltató példányt jelenti. Egy másik példányhoz csatlakozni, használja a `Connect-AzureRmAccount` parancsot a `-Environment` vagy -`EnvironmentName` parancssori kapcsoló a kívánt környezetre, vagy a környezet nevét.

A rendelkezésre álló környezetek listájának megtekintéséhez futtassa a `Get-AzureRmEnvironment` parancsmagot.

### <a name="to-connect-to-the-azure-government-cloud"></a>Csatlakozás az Azure Government Cloud
Az Azure Government Cloud csatlakozni, használja a következő parancsok egyikét.

    Connect-AzureRmAccount -EnvironmentName AzureUSGovernment

vagy

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Gyorsítótár létrehozása az Azure Government cloud, használja az alábbi helyek egyikét.

* USGov Virginia
* USGov Iowa

Az Azure Government Cloud kapcsolatos további információkért lásd: [a Microsoft Azure Government](https://azure.microsoft.com/features/gov/) és [a Microsoft Azure Government fejlesztői útmutató](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Csatlakozás az Azure China Cloud
Az Azure China Cloud csatlakozni, használja a következő parancsok egyikét.

    Connect-AzureRmAccount -EnvironmentName AzureChinaCloud

vagy

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Gyorsítótár létrehozása az Azure China felhőben, használja az alábbi helyek egyikét.

* Kelet-Kína
* Észak-Kína

Az Azure China Cloud kapcsolatos további információkért lásd: [Kínában a 21Vianet által üzemeltetett Azure AzureChinaCloud](http://www.windowsazure.cn/).

### <a name="to-connect-to-microsoft-azure-germany"></a>Szeretne csatlakozni a Microsoft Azure Germanyben
A Microsoft Azure Germany csatlakozni, használja a következő parancsok egyikét.

    Connect-AzureRmAccount -EnvironmentName AzureGermanCloud


vagy

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

Gyorsítótár létrehozása a Microsoft Azure Germanyben, használja az alábbi helyek egyikét.

* Közép-Németország
* Északkelet-Németország

További információ a Microsoft Azure Germany: [Microsoft Azure Germany](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Azure Redis Cache PowerShell használt tulajdonságok
Az alábbi táblázat a tulajdonságok és a gyakran használt paramétereket a létrehozása és kezelése az Azure PowerShell-lel az Azure Redis Cache-példányokban leírását tartalmazza.

| Paraméter | Leírás | Alapértelmezett |
| --- | --- | --- |
| Name (Név) |A gyorsítótár neve | |
| Hely |A gyorsítótár helye | |
| ResourceGroupName |Erőforráscsoport neve, amelyben a gyorsítótár létrehozása | |
| Méret |A gyorsítótár méretét. Érvényes értékek a következők: P1, P2, P3, P4, C0 csomag, C1, C2, C3, C4, C5 csomag, C6 csomag, 250 MB-os, 1 GB-os, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB |1 GB |
| ShardCount |Hozzon létre egy prémium szintű gyorsítótár létrehozásakor a fürtözés engedélyezve van a szegmensek száma. Érvényes értékek a következők: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Adja meg a gyorsítótár-Termékváltozat. Érvényes értékek a következők: alapszintű, Standard, prémium szintű |Standard |
| RedisConfiguration |Itt adhatja meg a Redis-konfigurációs beállításokat. További információ az egyes beállítások: a következő [RedisConfiguration tulajdonságok](#redisconfiguration-properties) tábla. | |
| EnableNonSslPort |Azt jelzi, hogy engedélyezve van-e a nem SSL port. |False (Hamis) |
| MaxMemoryPolicy |Ez a paraméter elavult, – használja helyette a RedisConfiguration. | |
| StaticIP |Tárolásához a gyorsítótár egy virtuális hálózaton, adja meg egy egyedi IP-cím az alhálózat, a gyorsítótár. Ha nincs megadva, az egyik van kiválasztva, az alhálózatról. | |
| Alhálózat |Üzemelteti a gyorsítótár egy virtuális hálózaton, amikor megadja az alhálózaton, melyben szeretné üzembe helyezni a gyorsítótár nevére. | |
| VirtualNetwork |A gyorsítótár egy virtuális hálózaton tárolásához, a virtuális hálózat, melyben szeretné üzembe helyezni a gyorsítótár erőforrás Azonosítóját határozza meg. | |
| KeyType |Itt adhatja meg, melyik hívóbetű újragenerálni a hozzáférési kulcsok megújításakor. Érvényes értékek a következők: elsődleges, másodlagos | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration tulajdonságai
| Tulajdonság | Leírás | Árképzési szintek |
| --- | --- | --- |
| RDB-fájlba való biztonsági mentés engedélyezve |E [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) engedélyezve van |Csak prémium szinten |
| RDB-storage-connection-string |A storage-fiókhoz tartozó kapcsolati karakterlánc [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) |Csak prémium szinten |
| backup – gyakori RDB-fájlba való |A biztonsági mentési gyakorisága [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) |Csak prémium szinten |
| maxmemory-reserved |Konfigurálja a [szolgáltatás számára fenntartott memória](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) nem gyorsítótárazási folyamatok |Standard és Prémium |
| a maxmemory-házirend |Konfigurálja a [kiürítési szabályzatot](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) a gyorsítótár |Az összes tarifacsomag |
| értesítés-kulcstér-események |Konfigurálja a [kulcstérértesítések](cache-configure.md#keyspace-notifications-advanced-settings) |Standard és Prémium |
| hash-max-ziplist-entries |Konfigurálja a [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Prémium |
| hash-max-ziplist-value |Konfigurálja a [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Prémium |
| set-max-intset-bejegyzések |Konfigurálja a [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Prémium |
| zset-max-ziplist-entries |Konfigurálja a [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Prémium |
| zset-max-ziplist-value |Konfigurálja a [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Prémium |
| adatbázisok |Konfigurálja az adatbázisok száma. Ez a tulajdonság csak a cache létrehozásakor konfigurálható. |Standard és Prémium |

## <a name="to-create-a-redis-cache"></a>Egy Redis Cache létrehozása
Új Azure Redis Cache-példány használatával jön létre a [New-azurermrediscache parancsmag esetében](https://docs.microsoft.com/powershell/module/azurerm.rediscache/new-azurermrediscache?view=azurermps-6.6.0) parancsmagot.

> [!IMPORTANT]
> Egy Redis gyorsítótárhoz hoz létre az Azure portal használatával az előfizetés első alkalommal a portálon regisztrálja a `Microsoft.Cache` névteret biztosít az adott előfizetéshez. Ha megkísérli az első Redis gyorsítótár létrehozása a PowerShell használatával az előfizetés, először regisztrálnia kell a névtéren az alábbi paranccsal; egyéb parancsmagok például `New-AzureRmRedisCache` és `Get-AzureRmRedisCache` sikertelen.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `New-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of the redis cache to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.

        -Location <String>
            Location in which to create the redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Az alapértelmezett paraméterek gyorsítótár létrehozásához futtassa az alábbi parancsot.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, és `Location` kötelező paraméterek, de a többi nem kötelező, és az alapértelmezett értékek találhatók. Az előző parancs futtatásával létrehoz egy standard szintű Termékváltozat Azure Redis Cache-példány a megadott nevét, helyét és erőforráscsoport, amely 1 GB méretűnek és a nem SSL port le van tiltva.

A prémium gyorsítótár létrehozásához adjon meg egy P1 (6 GB – 60 GB), P2 (13 GB - 130 GB) méretű P3 (26 GB – 260 GB), vagy a P4 (53 GB - 530 GB). Fürtözésének engedélyezésére irányuló használatával megad egy szegmensek száma a `ShardCount` paraméter. Az alábbi példa 3 szegmensek prémium P1 szintű gyorsítótár hoz létre. Prémium P1 szintű gyorsítótár 6 GB méretű, és mivel meghatározott három szegmensek a teljes mérete 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Adja meg az értékeket, a `RedisConfiguration` paramétert, tegye az értékek belül `{}` , kulcs/érték párok, például `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Az alábbi példa létrehoz egy standard 1 GB-os gyorsítótár- `allkeys-random` konfigurált maxmemory házirend- és kulcstér értesítések `KEA`. További információkért lásd: [(Speciális beállítások) kulcstérértesítések](cache-configure.md#keyspace-notifications-advanced-settings) és [memória házirendek](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Az adatbázisok gyorsítótár létrehozása során beállítás konfigurálása
A `databases` beállítás csak a gyorsítótár létrehozása során konfigurálható. Az alábbi példa létrehoz egy prémium P3 (26 GB) gyorsítótár-48 adatbázisaihoz a [New-azurermrediscache parancsmag esetében](https://docs.microsoft.com/powershell/module/azurerm.rediscache/New-AzureRmRedisCache?view=azurermps-6.6.0) parancsmagot.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

További információ a `databases` tulajdonságot használja, lásd: [Azure Redis Cache alapértelmezett kiszolgálókonfiguráció](cache-configure.md#default-redis-server-configuration). További létrehozásával kapcsolatos információkat a gyorsítótár használatával a [New-azurermrediscache parancsmag esetében](https://docs.microsoft.com/powershell/module/azurerm.rediscache/new-azurermrediscache?view=azurermps-6.6.0) parancsmag, az előző [Redis gyorsítótárat létrehozni](#to-create-a-redis-cache) szakaszban.

## <a name="to-update-a-redis-cache"></a>Redis gyorsítótár frissítése
Az Azure Redis Cache-példány használatával frissíti a [Set-azurermrediscache parancsmag esetében](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Set-AzureRmRedisCache?view=azurermps-6.6.0) parancsmagot.

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `Set-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of the redis cache to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

A `Set-AzureRmRedisCache` tulajdonságok frissítéséhez, mint például a parancsmag is használható `Size`, `Sku`, `EnableNonSslPort`, és a `RedisConfiguration` értékeket. 

A következő parancs frissíti a maxmemory-házirend nevű myCache Redis Cache-hez.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-a-redis-cache"></a>Egy Redis cache méretezése
`Set-AzureRmRedisCache` segítségével méretezheti az Azure Redis cache-példány, ha a `Size`, `Sku`, vagy `ShardCount` tulajdonság módosítását mutatjuk be. 

> [!NOTE]
> Skálázás a PowerShell-lel gyorsítótár azonban ugyanazon korlátait és útmutatók az Azure Portalról gyorsítótár méretezése függvénye. A következő korlátozásokkal másik tarifacsomagra skálázhatja.
> 
> * Nem skálázhatja árképzési szint alacsonyabb tarifacsomagra.
> * A nem skálázhatja egy **prémium szintű** le a gyorsítótár egy **Standard** vagy egy **alapszintű** gyorsítótár.
> * A nem skálázhatja egy **Standard** le a gyorsítótár egy **alapszintű** gyorsítótár.
> * Méretezheti a egy **alapszintű** gyorsítótárba egy **Standard** gyorsítótár, de nem módosíthatja a méretét egyszerre. Ha más méretre van szüksége, érdemes egy későbbi skálázási műveletet, hogy a kívánt méretet.
> * A nem skálázhatja egy **alapszintű** közvetlenül a gyorsítótár egy **prémium** gyorsítótár. Meg lehessen méretezni kell **alapszintű** való **Standard** egy skálázási műveletet, majd a **Standard** való **prémium szintű** az ezt követő méretezése a művelet.
> * A nagyobb méretű le a nem skálázhatja a **C0 csomag (250 MB)** méretét.
> 
> További információkért lásd: [How to Azure Redis Cache méretezése](cache-how-to-scale.md).
> 
> 

Az alábbi példa bemutatja, hogyan nevű gyorsítótár méretezése `myCache` 2,5 GB-gyorsítótárhoz. Vegye figyelembe, hogy ez a parancs az alapszintű vagy standard szintű gyorsítótár is működik-e.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Ez a parancs kiadása után a gyorsítótár állapotát adja vissza (hívása hasonló `Get-AzureRmRedisCache`). Vegye figyelembe, hogy a `ProvisioningState` van `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Ha a skálázási művelet befejeződött, a `ProvisioningState` vált `Succeeded`. Ha meg kell győződnie, egy későbbi skálázási művelet, például a módosítás az alapszintű, Standard és a méretet, majd módosítja meg kell várnia, amíg az előző művelet befejeződik, vagy az alábbihoz hasonló hibaüzenetet kap.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>A Redis-gyorsítótár adatainak lekérése
Kérheti, hogy a gyorsítótár használatával kapcsolatos információkat a [Get-azurermrediscache parancsmag esetében](https://docs.microsoft.com/powershell/module/azurerm.rediscache/get-azurermrediscache?view=azurermps-6.6.0) parancsmagot.

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `Get-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Az aktuális előfizetésben gyorsítótárait kapcsolatos információkat ad vissza, futtassa `Get-AzureRmRedisCache` paraméterek nélkül.

    Get-AzureRmRedisCache

Egy adott erőforráscsoportban gyorsítótárait kapcsolatos információkat ad vissza, futtassa `Get-AzureRmRedisCache` együtt a `ResourceGroupName` paraméter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Egy megadott gyorsítótárfiók kapcsolatos információkat ad vissza, futtassa `Get-AzureRmRedisCache` együtt a `Name` paraméter a gyorsítótár nevét tartalmazó és a `ResourceGroupName` paramétert, hogy a gyorsítótár tartalmazó erőforráscsoportot.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>A Redis gyorsítótár elérési kulcsainak lekéréséhez
A gyorsítótár elérési kulcsainak lekéréséhez használja a [Get-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-6.6.0) parancsmagot.

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `Get-AzureRmRedisCacheKey`, futtassa a következő parancsot.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

A gyorsítótár a kulcsok lekéréséhez hívja meg a `Get-AzureRmRedisCacheKey` parancsmag paraméterével be a gyorsítótár nevét, amely tartalmazza a gyorsítótárat az erőforráscsoport nevét.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>A Redis gyorsítótár elérési kulcsainak újragenerálása
A gyorsítótár elérési kulcsainak újragenerálása, használhatja a [New-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/New-AzureRmRedisCacheKey?view=azurermps-6.6.0) parancsmagot.

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `New-AzureRmRedisCacheKey`, futtassa a következő parancsot.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates the access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Hozza létre újra az elsődleges vagy másodlagos kulcsot a gyorsítótárhoz, hívja meg a `New-AzureRmRedisCacheKey` parancsmagot, és adja meg a nevet, az erőforráscsoportot, és adja meg `Primary` vagy `Secondary` számára a `KeyType` paraméter. A következő példában a Cache-gyorsítótárhoz a másodlagos hozzáférési kulcs újbóli létrehozása.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Redis gyorsítótár törlése
Redis-gyorsítótár törléséhez használja a [Remove-azurermrediscache parancsmag esetében](https://docs.microsoft.com/powershell/module/azurerm.rediscache/remove-azurermrediscache?view=azurermps-6.6.0) parancsmagot.

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `Remove-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

A következő példában a cache nevű `myCache` törlődik.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Egy Redis gyorsítótárhoz importálása
Adatok importálása az Azure Redis Cache-példány használata a `Import-AzureRmRedisCache` parancsmagot.

> [!IMPORTANT]
> Importálási/exportálási lehetőség csak a [prémium szintű](cache-premium-tier-intro.md) gyorsítótárazza. Importálási/exportálási kapcsolatos további információkért lásd: [Azure Redis Cache az adatok importálása és exportálása](cache-how-to-import-export-data.md).
> 
> 

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `Import-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs to Azure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


A következő parancsot a blobból az Azure Redis Cache az SAS URI-t által meghatározott adatokat importálja.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Egy Redis gyorsítótárhoz exportálása
Egy Azure Redis Cache-példány használatával is exportálhat adatokat a `Export-AzureRmRedisCache` parancsmagot.

> [!IMPORTANT]
> Importálási/exportálási lehetőség csak a [prémium szintű](cache-premium-tier-intro.md) gyorsítótárazza. Importálási/exportálási kapcsolatos további információkért lásd: [Azure Redis Cache az adatok importálása és exportálása](cache-how-to-import-export-data.md).
> 
> 

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `Export-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Prefix <String>
            Prefix to use for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


A következő parancsot egy Azure Redis Cache-példányból a tárolóba, az SAS URI-t által megadott exportálja az adatokat.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Az újraindításhoz a Redis Cache-gyorsítótárhoz
Az Azure Redis Cache-példány használatával indíthatja újra a `Reset-AzureRmRedisCache` parancsmagot.

> [!IMPORTANT]
> Csak akkor érhető el, az újraindítás [prémium szintű](cache-premium-tier-intro.md) gyorsítótárazza. További információ a gyorsítótár újraindítása: [felügyeleti gyorsítótár - újraindítás](cache-administration.md#reboot).
> 
> 

A paramétereket, és a hozzájuk tartozó leírások listájának megtekintéséhez `Reset-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.

        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


A következő parancsot a megadott gyorsítótár mindkét csomópont újraindul.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>További lépések
A Windows PowerShell-lel az Azure-ral kapcsolatos további információkért lásd a következőket:

* [Az MSDN-en az Azure Redis Cache parancsmag dokumentációja](https://docs.microsoft.com/powershell/module/azurerm.rediscache/?view=azurermps-6.6.0)
* [Az Azure Resource Manager parancsmagjainak](http://go.microsoft.com/fwlink/?LinkID=394765): ismerje meg, a parancsmagok használatához az Azure Resource Manager modulban.
* [Erőforráscsoportok használata az Azure-erőforrások kezelése](../azure-resource-manager/resource-group-template-deploy-portal.md): ismerje meg, hogyan hozhat létre és kezelheti az erőforráscsoportok az Azure Portalon.
* [Azure-blogban](https://azure.microsoft.com/en-us/blog/): az Azure-ban új szolgáltatásainak megismerése.
* [Windows PowerShell-blog](http://blogs.msdn.com/powershell): a Windows PowerShell új szolgáltatásainak megismerése.
* ["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): valós tippeket és trükköket lekérése a Windows PowerShell-Közösségtől.


---
title: Az Azure Cosmos DB Automation - kezelés a PowerShell-lel |} A Microsoft Docs
description: Az Azure Powershell használata kezelheti az Azure Cosmos DB-fiókokhoz.
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 04/21/2017
ms.author: sngun
ms.openlocfilehash: 82ab30ebab1b69d5ae636702b3b56d3792c09010
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394573"
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a>Egy PowerShell-lel az Azure Cosmos DB-fiók létrehozása

Ez az útmutató azt ismerteti, automatizált felügyelete az Azure Powershell-lel az Azure Cosmos DB-adatbázisfiókhoz a parancsokat. Fiókkulcsok és a feladatátvételi prioritásokat a [többrégiós adatbázisfiókhoz] kezelésére szolgáló parancsokat is tartalmaz [terjesztése, adatok, globally.md]. Az adatbázis-fiók frissítése lehetővé teszi módosítása konzisztencia házirendek és régiók hozzáadása/eltávolítása. Platformfüggetlen kezelése érdekében az Azure Cosmos DB-fiókot, vagy használhatja [Azure CLI-vel](cli-samples.md), a [erőforrás-szolgáltató REST API][rp-rest-api], vagy a [Azure Portalon ](create-sql-api-dotnet.md#create-account).

## <a name="getting-started"></a>Első lépések

Kövesse a [telepítése és konfigurálása az Azure PowerShell-lel] [ powershell-install-configure] telepítéséhez, és jelentkezzen be az Azure Resource Manager-fiókjába, a PowerShellben.

### <a name="notes"></a>Megjegyzések

* Ha szeretné, hajtsa végre a következő parancsokat a felhasználó jóváhagyásának kérése nélkül, fűzze hozzá a `-Force` jelző a következő paranccsal.
* Az alábbi parancsokat a rendszer szinkron.

## <a id="create-documentdb-account-powershell"></a> Egy Azure Cosmos DB-fiók létrehozása

Ez a parancs lehetővé teszi, hogy hozzon létre egy Azure Cosmos DB-adatbázisfiók. Konfigurálja az új adatbázis-fiók vagy az egyetlen régióban, vagy az [többrégiós] [terjesztése, adatok, globally.md] egy bizonyos [konzisztencia-szabályzat](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>` A hely neve, az írási régió az adatbázisfiókot. Ezen a helyen kell rendelkeznie a feladatátvétel prioritási érték 0. Adatbázis-fiókonként pontosan egy írási régiót kell lennie.
* `<read-region-location>` Az olvasási régióban az adatbázisfiók hely neve. Ez a hely 0-nál nagyobb feladatátvételi prioritás értéke szükséges. Adatbázis-fiókonként egynél több olvasási régióval is lehet.
* `<ip-range-filter>` Megadja az IP-cím vagy IP-címtartományok része, mint az engedélyezett listára ügyfél IP-címek egy adott adatbázis fiókjához tartozó CIDR formátumban. IP-címek/címtartományok vesszővel elválasztott, és nem tartalmazhat szóközöket kell lennie. További információkért lásd: [Azure Cosmos DB Tűzfaltámogatásáról](firewall-support.md)
* `<default-consistency-level>` Az Azure Cosmos DB-fiók alapértelmezett konzisztencia szintjét. További információkért lásd: [Azure Cosmos DB-ben Konzisztenciaszintek](consistency-levels.md).
* `<max-interval>` Elavulás konzisztencia használata esetén ezt az értéket a idő mennyisége frissesség (másodpercben) megengedhető jelöli. Ez az érték elfogadható tartománya 1-100.
* `<max-staleness-prefix>` Elavulás konzisztencia használata esetén ezt az értéket a megengedhető elavult kérések számát jelöli. Ehhez az értékhez elfogadott tartomány: 1 – 2 147 483 647.
* `<resource-group-name>` Neve a [Azure-erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB-adatbázisfiók való tartozik.
* `<resource-group-location>` Az Azure-erőforráscsoportot, amelyhez az új Azure Cosmos DB adatbázis-fiók helyét, amelyhez tartozik.
* `<database-account-name>` Neve az Azure Cosmos DB adatbázis-fiókot létrehozni. Csak kisbetűket, számokat, képes használni az '-' karaktert, és 3 – 50 karakter között kell lennie.

Példa: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Megjegyzések
* Az előző példában egy adatbázis-fiókot hoz létre két régióban. Akkor is egy adatbázis-fiók létrehozása az egyik régió (amely ki van jelölve, az írási régió, és a feladatátvételi prioritási értéke 0) vagy a több mint két régióban. További információkért lásd: [többrégiós adatbázisfiókhoz] [terjesztése, adatok, globally.md].
* A helyek, amelyben az Azure Cosmos DB általánosan elérhető a régióban kell lennie. Aktuális régiók listája, a megadott a [Azure-régiók lap](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a> Frissítés az Azure Cosmos DB-adatbázisfiók

Ez a parancs lehetővé teszi az Azure Cosmos DB-adatbázis fiók tulajdonságainak frissítése. Ez magában foglalja a konzisztencia-szabályzat és a helyek, amely megtalálható az adatbázis-fiókot.

> [!NOTE]
> Ez a parancs lehetővé teszi, hogy hozzá és távolíthat el a régióban, de nem engedélyezi, hogy módosítsa a feladatátvételi prioritások. Feladatátvételi prioritások módosítása esetén lásd: [alább](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>` A hely neve, az írási régió az adatbázisfiókot. Ezen a helyen kell rendelkeznie a feladatátvétel prioritási érték 0. Adatbázis-fiókonként pontosan egy írási régiót kell lennie.
* `<read-region-location>` Az olvasási régióban az adatbázisfiók hely neve. Ez a hely 0-nál nagyobb feladatátvételi prioritás értéke szükséges. Adatbázis-fiókonként egynél több olvasási régióval is lehet.
* `<default-consistency-level>` Az Azure Cosmos DB-fiók alapértelmezett konzisztencia szintjét. További információkért lásd: [Azure Cosmos DB-ben Konzisztenciaszintek](consistency-levels.md).
* `<ip-range-filter>` Megadja az IP-cím vagy IP-címtartományok része, mint az engedélyezett listára ügyfél IP-címek egy adott adatbázis fiókjához tartozó CIDR formátumban. IP-címek/címtartományok vesszővel elválasztott, és nem tartalmazhat szóközöket kell lennie. További információkért lásd: [Azure Cosmos DB Tűzfaltámogatásáról](firewall-support.md)
* `<max-interval>` Elavulás konzisztencia használata esetén ezt az értéket a idő mennyisége frissesség (másodpercben) megengedhető jelöli. Ez az érték elfogadható tartománya 1-100.
* `<max-staleness-prefix>` Elavulás konzisztencia használata esetén ezt az értéket a megengedhető elavult kérések számát jelöli. Ehhez az értékhez elfogadott tartomány: 1 – 2 147 483 647.
* `<resource-group-name>` Neve a [Azure-erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB-adatbázisfiók való tartozik.
* `<resource-group-location>` Az Azure-erőforráscsoportot, amelyhez az új Azure Cosmos DB adatbázis-fiók helyét, amelyhez tartozik.
* `<database-account-name>` Neve az Azure Cosmos DB-adatbázisfiók frissíteni kell.

Példa: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a> Az Azure Cosmos DB-adatbázisfiók törlése

Ez a parancs lehetővé teszi, hogy egy meglévő Azure Cosmos DB-adatbázisfiók törlése.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>` Neve a [Azure-erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB-adatbázisfiók való tartozik.
* `<database-account-name>` Neve az Azure Cosmos DB-adatbázisfiók törölni.

Példa:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a> Az Azure Cosmos DB-adatbázisfiók tulajdonságainak beolvasása

Ez a parancs lehetővé teszi, hogy egy meglévő Azure Cosmos DB-adatbázisfiók tulajdonságainak beolvasása.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Neve a [Azure-erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB-adatbázisfiók való tartozik.
* `<database-account-name>` Az Azure Cosmos DB-adatbázisfiók neve.

Példa:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a> Frissítheti a címkéket az Azure Cosmos DB-adatbázisfiók

Az alábbi példa azt ismerteti, hogyan állíthatja be [Azure-erőforráscímkék] [ azure-resource-tags] adatbázis az Azure Cosmos DB-fiókot.

> [!NOTE]
> Ez a parancs kombinálva, a létrehozás vagy frissítés parancsok hozzáfűzésével a `-Tags` jelzőt mellékel a megfelelő paraméter.

Példa:

    $tags = @{"dept" = "Finance"; environment = "Production"}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDB/databaseAccounts"  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a> Fiók kulcsainak listázása

Az Azure Cosmos DB-fiók létrehozásakor a szolgáltatás két fő kulcsot, amely az Azure Cosmos DB-fiók elérésekor hitelesítéshez használt állít elő. Azáltal, hogy a két hozzáférési kulcsot, az Azure Cosmos DB lehetővé teszi a kulcsok az Azure Cosmos DB-fiók megszakítás nélkül. Írásvédett kulcsok hitelesítéséhez a csak olvasható műveletekhez is elérhetők. Nincsenek két írható és olvasható kulcsok (elsődleges és másodlagos) és a két csak olvasható kulcsok (elsődleges és másodlagos).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Neve a [Azure-erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB-adatbázisfiók való tartozik.
* `<database-account-name>` Az Azure Cosmos DB-adatbázisfiók neve.

Példa:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a> Kapcsolati karakterláncok listája

MongoDB-fiókok esetében a kapcsolati karakterláncot a MongoDB-alkalmazás csatlakozni az adatbázis-fiókot az alábbi parancs használatával lekérhetők.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Neve a [Azure-erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB-adatbázisfiók való tartozik.
* `<database-account-name>` Az Azure Cosmos DB-adatbázisfiók neve.

Példa:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a> Fiókkulcs újragenerálása

Az Azure Cosmos DB-fiókot, és rendszeres időközönként biztonságosabb fenntarthatja a kapcsolatokat, módosítania kell a hozzáférési kulcsokat. Lehetővé teszi az Azure Cosmos DB-fiók az egyik kulcs, míg más hozzáférési kulcs kapcsolatok fenntartásához két kulcsot vannak hozzárendelve.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>` Neve a [Azure-erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB-adatbázisfiók való tartozik.
* `<database-account-name>` Az Azure Cosmos DB-adatbázisfiók neve.
* `<key-kind>` A négy különböző típusú kulcs: ["Elsődleges" |} " Másodlagos "|}" PrimaryReadonly "|}" SecondaryReadonly"], amely, amelyet szeretne generálni.

Példa:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a> Egy Azure Cosmos DB-adatbázisfiók feladatátvétel prioritásának módosítása

A többrégiós adatbázisfiókhoz a különböző régiók, amely megtalálható az Azure Cosmos DB-adatbázisfiók feladatátvételi prioritását módosíthatja. Feladatátvétel a Azure Cosmos DB-fiókot a további információkért lásd: [globális adatterjesztés az Azure Cosmos DB][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>` A hely neve, az írási régió az adatbázisfiókot. Ezen a helyen kell rendelkeznie a feladatátvétel prioritási érték 0. Adatbázis-fiókonként pontosan egy írási régiót kell lennie.
* `<read-region-location>` Az olvasási régióban az adatbázisfiók hely neve. Ez a hely 0-nál nagyobb feladatátvételi prioritás értéke szükséges. Adatbázis-fiókonként egynél több olvasási régióval is lehet.
* `<resource-group-name>` Neve a [Azure-erőforráscsoport] [ azure-resource-groups] , amelyhez az új Azure Cosmos DB-adatbázisfiók való tartozik.
* `<database-account-name>` Az Azure Cosmos DB-adatbázisfiók neve.

Példa:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>További lépések

* Csatlakozás a .NET használatával, lásd: [.NET végzett csatlakozásról és lekérdezésről](create-sql-api-dotnet.md).
* Való csatlakozáshoz a Node.js használatával, tekintse meg [Node.js és MongoDB-alkalmazás végzett csatlakozásról és lekérdezésről](create-mongodb-nodejs.md).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/

---
title: 'Gyors útmutató: Számítási feladatok skálázása az Azure SQL Data Warehouse-ban – PowerShell | Microsoft Docs'
description: Azure SQL Data Warehouse-beli számítási feladatok skálázása a PowerShellben. Felskálázással a számítások teljesítménye növelhető, leskálázással a költségek csökkenthetők.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: d3a3b30539ed33bf3e8d21a8cc5e5f2692308f98
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43283192"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-powershell"></a>Gyors útmutató: Azure SQL Data Warehouse-beli számítási feladatok skálázása a PowerShellben

Azure SQL Data Warehouse-beli számítási feladatok skálázása a PowerShellben. [Felskálázással](sql-data-warehouse-manage-compute-overview.md) a számítások teljesítménye növelhető, leskálázással a költségek csökkenthetők.

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

Az oktatóanyaghoz az Azure PowerShell-modul 5.1.1-es vagy újabb verziója szükséges. A jelenleg rendelkezésére álló verzió azonosításához futtassa a következőt: `Get-Module -ListAvailable AzureRM`. Ha telepíteni vagy frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](/powershell/azure/install-azurerm-ps.md) ismertető cikket.

## <a name="before-you-begin"></a>Előkészületek

Ez a gyors útmutató feltételezi, hogy már rendelkezik méretezhető SQL Data Warehouse szolgáltatással. Ha létre kell hoznia egyet, kövesse a [Létrehozás és csatlakozás – portál](create-data-warehouse-portal.md) útmutatót egy **mySampleDataWarehouse** nevű adattárház létrehozásához.

## <a name="log-in-to-azure"></a>Jelentkezzen be az Azure-ba

Jelentkezzen be az Azure-előfizetésbe a [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) parancs használatával, és kövesse a képernyőn megjelenő útmutatást.

```powershell
Connect-AzureRmAccount
```

A használt előfizetés megtekintéséhez futtassa a [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) parancsot.

```powershell
Get-AzureRmSubscription
```

Ha nem az alapértelmezett előfizetést kívánja használni, futtassa a [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription) parancsot.

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Adattárház-információk megkeresése

Keresse meg a felfüggeszteni és folytatni tervezett adattárházhoz tartozó adatbázis nevét, a kiszolgáló nevét és az erőforráscsoportot.

Keresse meg adattárháza helyinformációit ezekkel lépésekkel.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).
2. Az Azure Portal bal oldali panelén kattintson az **SQL Data Warehouse-adattárházak** elemre.
3. Az **SQL adattárház** lapon jelölje ki a **mySampleDataWarehouse** elemet. Ezzel megnyílik az adattárház.

    ![Kiszolgálónév és erőforráscsoport](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Írja fel az adattárház nevét. Ezt fogja használni az adatbázis neveként. Ne feledje, hogy az adattárház az adatbázisok egy típusa. Írja fel a kiszolgáló nevét és az erőforráscsoportot is. Ezeket a felfüggesztő és folytató parancsokban fogja használni.
5. Ha a kiszolgáló valami.database.windows.net, csak az első részt használja kiszolgálónévként a PowerShell-parancsmagokban. A fenti ábrán a teljes kiszolgálónév newserver-20171113.database.windows.net. A PowerShell-parancsmagban a **newserver-20180430** részt használjuk a kiszolgáló neveként.

## <a name="scale-compute"></a>Számítások méretezése

Az SQL Data Warehouse-ban növelheti vagy csökkentheti a számítási erőforrásokat az adattárházegységek számának módosításával. A [Létrehozás és csatlakozás – portál](create-data-warehouse-portal.md) gyorsútmutató létrehozta a **mySampleDataWarehouse** adattárházat, és inicializálta azt 400 adattárházegységgel. Az alábbi lépésekkel módosíthatja a **mySampleDataWarehouse** adattárházban az adattárházegységek számát.

Az adattárházegységek számának módosítására használja a [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) PowerShell-parancsmagot. Az alábbi példa DW300-ra állítja be az adattárházegységek számát a **myResourceGroup** erőforráscsoportban a **mynewserver-20180430** kiszolgálón üzemeltetett **mySampleDataWarehouse** adatbázishoz.

```Powershell
Set-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
```

## <a name="check-data-warehouse-state"></a>Az adattárház állapotának ellenőrzése

Az adattárház aktuális állapotát megtekintheti a [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) PowerShell-parancsmag használatával. Ez a **myResourceGroup** erőforráscsoportban a **mynewserver-20180430.database.windows.net** kiszolgálón üzemeltetett **mySampleDataWarehouse** adatbázis állapotát adja vissza.

```powershell
$database = Get-AzureRmSqlDatabase -ResourceGroupName myResourceGroup -ServerName mynewserver-20171113 -DatabaseName mySampleDataWarehouse
$database
```

Az eredmény a következőhöz lesz hasonló:

```powershell
ResourceGroupName             : myResourceGroup
ServerName                    : mynewserver-20171113
DatabaseName                  : mySampleDataWarehouse
Location                      : North Europe
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/mynewserver-20171113/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

A kimenetben megjelenik az adatbázis **Állapota**. Ebben az esetben az látható, hogy az adatbázis online állapotban van.  Ennek a parancsnak a futtatáskor az Online, Pausing (felfüggesztés), Resuming (folytatás), Scaling (méretezés) vagy Paused (felfüggesztve) állapotértéket kell visszakapnia.

Az állapot önmagában való megjelenítéséhez használja a következő parancsot:

```powershell
$database | Select-Object DatabaseName,Status
```

## <a name="next-steps"></a>További lépések
Ebből az útmutatóból megtudhatta, hogyan tudja méterezni egy adattárház számítási kapacitását. Ha bővebb információra van szüksége az Azure SQL Data Warehouse-zal kapcsolatban, folytassa az adatok betöltésével foglalkozó oktatóanyaggal.

> [!div class="nextstepaction"]
>[Adatok betöltése az SQL Data Warehouse-okba](load-data-from-azure-blob-storage-using-polybase.md)

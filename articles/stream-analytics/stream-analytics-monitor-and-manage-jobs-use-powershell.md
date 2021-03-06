---
title: Figyelése és felügyelete az Azure Stream Analytics-feladatok PowerShell-lel
description: Ez a cikk ismerteti, hogyan használható az Azure PowerShell és a parancsmagok figyelése és felügyelete az Azure Stream Analytics-feladatok.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 6812298bc90446a7b7e136ec81a67641c2a9e18f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/05/2018
ms.locfileid: "43701608"
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Figyelheti és kezelheti a Stream Analytics-feladatok az Azure PowerShell-parancsmagok
Ismerje meg, hogyan figyelheti és a Stream Analytics-erőforrások kezelése a az Azure PowerShell-parancsmagok és a powershell-parancsprogramok, amelyek az alapszintű Stream Analytics-feladatok végrehajtásához.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-parancsmagok futtatása Stream Analytics előfeltételei
* Hozzon létre egy Azure-erőforráscsoportot az előfizetésében. Az alábbiakban látható egy minta Azure PowerShell-parancsfájlt. Azure PowerShell-információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview);  

Az Azure PowerShell 0.9.8-as verzióját használja:  

         # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Az Azure PowerShell 1.0-hoz:  

         # Log in to your Azure account
        Connect-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName "your sub" | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> Stream Analytics-feladatok programozott módon létrehozott nem rendelkezik a figyelés alapértelmezés szerint engedélyezve van.  Manuálisan engedélyezheti figyelése az Azure Portalon, a feladat figyelése oldalára, és az Engedélyezés gombra kattint, vagy ezt megteheti is programozott módon a helyen található lépéseket követve [Azure Stream Analytics – Stream Analytics-feladatok figyelése Amellyel programozott módon](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Stream Analytics az Azure PowerShell-parancsmagok
A következő Azure PowerShell-parancsmagok segítségével figyelheti és kezelheti az Azure Stream Analytics-feladatok. Vegye figyelembe, hogy az Azure PowerShell rendelkezik-e a különböző verziók. 
**Az első parancs van a felsorolt példák az Azure PowerShell 0.9.8-as verzióját használja, a második parancs van az Azure PowerShell 1.0-t.** Az Azure PowerShell 1.0-parancsok mindig lesz "AzureRM" parancsban.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Felsorolja az összes Stream Analytics-feladatok az Azure-előfizetés vagy a megadott erőforráscsoportban, vagy egy adott feladat egy erőforráscsoporton belül feladat adatainak beolvasása.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsJob

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsJob

A PowerShell-parancsot az Azure-előfizetésben a Stream Analytics-feladatok adatait adja vissza.

**2. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Ez a PowerShell-parancs StreamAnalytics-alapértelmezett – közép-USA erőforráscsoporthoz tartozik, a Stream Analytics-feladatok adatait adja vissza.

**3. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Ez a PowerShell-parancs StreamAnalytics-alapértelmezett – közép-USA erőforráscsoporthoz tartozik, a Stream Analytics-feladat StreamingJob adatait adja vissza.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Felsorolja az összes bemenet, amely egy megadott Stream Analytics-feladat vannak definiálva, vagy egy meghatározott bevitel adatainak beolvasása.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

A PowerShell-parancsot a feladat StreamingJob definiált összes bemeneti adatait adja vissza.

**2. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

A PowerShell-parancsot a feladat StreamingJob meghatározott EntryStream nevű bemeneti adatait adja vissza.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Felsorolja az összes, a kimeneteket, amely egy megadott Stream Analytics-feladat vannak definiálva, vagy egy adott kimeneti adatainak beolvasása.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

A PowerShell-parancsot a StreamingJob feladatban meghatározott kimenetek kapcsolatos információkat ad vissza.

**2. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

A PowerShell-parancsot a kimeneti StreamingJob feladatban meghatározott nevű kimeneti adatait adja vissza.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
A kvóta a streamelési egységek számát egy adott régióban adatainak beolvasása.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

A PowerShell-parancsot a kvóta-és folyamatos átviteli egységek használati adatokat az USA középső régiójában adja vissza.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Egy adott átalakítás definiálása a Stream Analytics-feladat adatainak beolvasása.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Az Azure PowerShell 1.0-hoz:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

A PowerShell-parancsot az átalakítást a feladat StreamingJob StreamingJob nevű kapcsolatos információkat ad vissza.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>New-AzureStreamAnalyticsInput | New-AzureRMStreamAnalyticsInput
Létrehoz egy új bemeneti Stream Analytics-feladat belül, vagy egy meglévő megadott bemeneti adatok frissítése.

A bemeneti neve adható meg a .json fájlt, vagy a parancssorban. Ha mindkettő meg van adva, a parancssorban a neve megegyezik egy, a fájl kell lennie.

Ha egy már létező bemeneti adja meg, és ne adja meg a – Force paraméterek, a parancsmag kéri-e a meglévő bemeneti cserélje le.

Ha megad a – Force paramétert, és adjon meg egy meglévő adjon meg nevet, a bemeneti váltja megerősítés nélkül.

A JSON-fájl szerkezete és annak tartalmát a részletes információkért tekintse meg a [bemeneti létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] szakaszában a [Stream Analytics felügyeleti REST API-referencia Szalagtár][stream.analytics.rest.api.reference].

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

A PowerShell-parancsot egy új bemenet Input.json fájlból hoz létre. Ha egy meglévő bemeneti a bemeneti definíciós fájlban megadott névvel már meg van adva, a parancsmag kéri-e, cserélje le a.

**2. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

A PowerShell-parancs hoz létre egy új bemenet a EntryStream nevű feladatban. Ha egy meglévő bemeneti ezen a néven már meg van adva, a parancsmag kéri-e, cserélje le a.

**3. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

A PowerShell-parancs lecseréli a meglévő bemeneti forrás a fájlból a definíció EntryStream nevű definíciója.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>New-AzureStreamAnalyticsJob | New-AzureRMStreamAnalyticsJob
Létrehoz egy új Stream Analytics-feladatot a Microsoft Azure-ban, vagy egy meglévő megadott feladat definíciójának frissítése.

A feladat nevére a .json fájlt, vagy a parancssorban adható meg. Ha mindkettő meg van adva, a parancssorban a neve megegyezik egy, a fájl kell lennie.

Ha egy már létező feladat nevét adja meg, és ne adja meg a – Force paraméterek, a parancsmag kéri-e cserélje le a meglévő feladat.

Ha megad a – Force paramétert, és adja meg egy meglévő feladat nevét, a feladat definíciója váltja megerősítés nélküli.

A JSON-fájl szerkezete és annak tartalmát a részletes információkért tekintse meg a [Stream Analytics-feladat létrehozása] [ msdn-rest-api-create-stream-analytics-job] szakaszában a [Stream Analytics felügyeleti REST API-referencia függvénytár] [stream.analytics.rest.api.reference].

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Ez a PowerShell-parancs létrehoz egy új feladatot a JobDefinition.json definícióból. Ha egy meglévő feladat a feladat-definíciós fájl a megadott néven már meg van adva, a parancsmag kéri-e, cserélje le a.

**2. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

A PowerShell-parancsot a feladat definíciója StreamingJob váltja fel.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>New-AzureStreamAnalyticsOutput | New-AzureRMStreamAnalyticsOutput
Létrehoz egy új kimeneti Stream Analytics-feladat belül, vagy egy meglévő kimeneti frissítése.  

A kimenet a neve a .json fájlt, vagy a parancssorban adható meg. Ha mindkettő meg van adva, a parancssorban a neve megegyezik egy, a fájl kell lennie.

Ha egy már létező kimenetet adja meg, és ne adja meg a – Force paraméterek, a parancsmag kéri-e cserélje le a meglévő kimenet.

Ha megad a – Force paramétert, és adjon meg egy meglévő kimeneti név, a kimeneti váltja megerősítés nélkül.

A JSON-fájl szerkezete és annak tartalmát a részletes információkért tekintse meg a [kimeneti létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] szakaszában a [Stream Analytics felügyeleti REST API-referencia Szalagtár][stream.analytics.rest.api.reference].

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

A PowerShell-paranccsal hoz létre a feladatban StreamingJob "kimeneti" nevű új kimenet. Ha egy meglévő kimeneti ezen a néven már meg van adva, a parancsmag kéri-e, cserélje le a.

**2. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Ez a PowerShell-parancs a "kimeneti" definíciója a feladat StreamingJob váltja fel.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>New-AzureStreamAnalyticsTransformation | New-AzureRMStreamAnalyticsTransformation
Létrehoz egy új átalakítás belül egy Stream Analytics-feladatot, vagy frissíti a meglévő átalakítást.

Az átalakítás neve adható meg a .json fájlt, vagy a parancssorban. Ha mindkettő meg van adva, a parancssorban a neve megegyezik egy, a fájl kell lennie.

Ha egy már létező átalakítás adja meg, és ne adja meg a – Force paraméterek, a parancsmag kéri-e cserélje le a meglévő átalakítást.

Ha megad a – Force paramétert, és adjon meg egy meglévő transzformációjának a neve, az átalakítás váltja megerősítés nélkül.

A JSON-fájl szerkezete és annak tartalmát a részletes információkért tekintse meg a [átalakítási létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] szakaszában a [Stream Analytics felügyeleti REST API Referenciatárában][stream.analytics.rest.api.reference].

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

A PowerShell-parancsot a feladat StreamingJob StreamingJobTransform nevű új átalakítást hoz létre. Ha egy meglévő átalakítást ezen a néven már definiálva van, a parancsmag kéri-e, cserélje le a.

**2. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Az Azure PowerShell 1.0-hoz:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 A PowerShell-parancsot a feladat StreamingJob StreamingJobTransform definíciója váltja fel.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Remove-AzureStreamAnalyticsInput | Remove-AzureRMStreamAnalyticsInput
A Microsoft Azure Stream Analytics-feladat aszinkron módon törli egy meghatározott bevitel.  
Ha megad a – Force paramétert a bemeneti megerősítés nélküli törlődni fog.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Az Azure PowerShell 1.0-hoz:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

A PowerShell-parancs eltávolítja a bemeneti EventStream StreamingJob feladatban.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Remove-AzureStreamAnalyticsJob | Remove-AzureRMStreamAnalyticsJob
Aszinkron módon törli egy adott Stream Analytics-feladatot a Microsoft Azure-ban.  
Ha megad a – Force paraméterek megerősítés nélküli törli a feladatot.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Az Azure PowerShell 1.0-hoz:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Ez a PowerShell-parancs eltávolítja a feladat StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Remove-AzureStreamAnalyticsOutput | Remove-AzureRMStreamAnalyticsOutput
Aszinkron módon törli egy adott kimeneti Stream Analytics-feladat, a Microsoft Azure-ban.  
Ha megad a – Force paraméterek megerősítés nélküli törli a kimenetet.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Az Azure PowerShell 1.0-hoz:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

A PowerShell paranccsal eltávolítja a kimenet a kimenetet a feldolgozás StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob
Aszinkron módon telepíti, és a egy Stream Analytics-feladat elindul a Microsoft Azure-ban.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Az Azure PowerShell 1.0-hoz:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Ez a PowerShell-parancs elindítja a feladatot, 2012. December 12., 12:12:12 beállítása egy egyéni kimeneti kezdési időponttal StreamingJob (UTC).

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob
Aszinkron módon történik a Microsoft Azure-ban futó Stream Analytics-feladat leáll, és felszabadítása erőforrásokhoz, volt használatban. A feladatdefiníció és azok metaadatait továbbra is elérhető marad az Azure Portalon és a felügyeleti API-k, az adott előfizetéshez tartozó, hogy a feladat szerkeszthető és újraindul. Nem kell fizetni a leállított állapotú feladat.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Az Azure PowerShell 1.0-hoz:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Ez a PowerShell-parancs leállítja a feladatot StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput
Ellenőrzi a Stream Analytics egy megadott bemeneti adatok csatlakozni.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Az Azure PowerShell 1.0-hoz:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

A PowerShell-parancs a kapcsolat állapota a bemeneti EntryStream szereplő StreamingJob teszteli.  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput
A Stream Analytics képessége szeretne csatlakozni a megadott kimeneti teszteli.

**1. példa**

Az Azure PowerShell 0.9.8-as verzióját használja:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Az Azure PowerShell 1.0-hoz:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

A PowerShell parancsot tesztek StreamingJob a kimenetet a kimeneti kapcsolati állapotát.  

## <a name="get-support"></a>Támogatás kérése
További segítségre van szüksége, próbálja meg [Azure Stream Analytics-fórumon](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>További lépések
* [Az Azure Stream Analytics bemutatása](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301


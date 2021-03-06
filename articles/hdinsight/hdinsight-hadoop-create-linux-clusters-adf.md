---
title: 'Oktatóanyag: Data Factory használatával az Azure HDInsight igény szerinti Hadoop-fürtök létrehozása '
description: Ismerje meg, igény szerinti Hadoop-fürtök létrehozása az Azure Data Factory használatával HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: jasonh
ms.openlocfilehash: 567bac8a12a841eed2df1467b94a2a91c86ff7b4
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666151"
---
# <a name="tutorial-create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Oktatóanyag: Igény szerinti Hadoop-fürtök létrehozása az Azure Data Factoryt használó HDInsight
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Ebből a cikkből megismerheti, hogyan igény szerint, az Azure Data Factory használatával az Azure HDInsight egy Hadoop-fürt létrehozásához. Ezt követően az adatfolyamatok az Azure Data Factory a Hive-feladatok futtatásához, majd törölheti a fürtöt. Ez az oktatóanyag végére elsajátíthatja, hogyan tesztelhetők a big Data típusú adatok feladatfuttatás ahol fürt létrehozását, a feladat futtatása és a fürt törlését végzett ütemezés szerint.

Ez az oktatóanyag a következő feladatokat mutatja be: 

> [!div class="checklist"]
> * Azure-tárfiók létrehozása
> * Megismerheti az Azure Data Factory-tevékenység
> * Létrehoz egy data factoryt az Azure portal használatával
> * Társított szolgáltatások létrehozása
> * Folyamat létrehozása
> * Folyamat aktiválása
> * Folyamat monitorozása
> * Kimenet ellenőrzése

Ha nem rendelkezik Azure-előfizetéssel, [hozzon létre egy ingyenes fiókot](https://azure.microsoft.com/free/) a feladatok megkezdése előtt.

## <a name="prerequisites"></a>Előfeltételek

- Azure PowerShell. Útmutatásért lásd: [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

- Egy Azure Active Directory-szolgáltatásnevet. Miután létrehozta az egyszerű szolgáltatás, ügyeljen arra, hogy lekérni a **Alkalmazásazonosító** és **hitelesítési kulcs** a hivatkozott cikk utasításai. Az oktatóanyag későbbi részében szüksége ezekre az értékekre. Ellenőrizzük, hogy az egyszerű szolgáltatás tagja a *közreműködői* szerepkört az előfizetés vagy az erőforráscsoport, amelyben a fürt létrejött. A szükséges értékek beolvasása és a megfelelő szerepköröket rendeljen útmutatásért lásd: [hozzon létre egy Azure Active Directory egyszerű szolgáltatás](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Azure-tárfiók létrehozása

Ebben a szakaszban hozzon létre egy tárfiókot, amelyet a HDInsight-fürtöt hoz létre, igény szerinti az alapértelmezett tárolóként használt. Ez a tárfiók is tartalmaz a minta HiveQL-parancsfájlt (**hivescript.hql**), amellyel a fürtön futó Hive mintafeladat szimulálásához.

Ebben a szakaszban egy Azure PowerShell-szkript hozhat létre a tárfiókot, másolja a szükséges fájlokat a storage-fiókon belül használja. Az Azure PowerShell példaszkript ebben a szakaszban a következő feladatokat hajtja végre:

1. Az Azure-bA a naplókat.
1. Létrehoz egy Azure-erőforráscsoportot.
1. Létrehoz egy Azure Storage-fiókot.
1. Hoz létre egy blobtárolót a tárfiókban
1. Másolja át a minta HiveQL-parancsfájlt (**hivescript.hql**) a Blob-tároló. A parancsfájl mindig elérhető legyen [ https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql ](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). A példaszkript egy másik nyilvános Blob-tárolóban már érhető el. Az alábbi PowerShell-szkript az Azure Storage-fiókot hoz létre, ezeket a fájlokat másolatot készít.


**Hozzon létre egy tárfiókot, és másolja a fájlokat az Azure PowerShell használatával:**
> [!IMPORTANT]
> Adja meg az Azure-erőforráscsoportot és a parancsfájl által létrehozott Azure storage-fiók nevét.
> Írja le **erőforráscsoport-név**, **tárfióknevet**, és **tárfiókkulcs** a parancsfájl által használt kimeneti adattípus. Szükséges a következő szakaszban.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
Login-AzureRmAccount
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

**Ellenőrizze a tárfiók létrehozása**

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
1. Válassza ki **erőforráscsoportok** a bal oldali panelen.
1. Kattintson duplán az erőforráscsoport nevét a PowerShell-parancsprogram létrehozott. Használja a szűrőt, ha túl sok erőforráscsoportja felsorolt.
1. Az a **erőforrások** csempe, megjelenik egy erőforrás szerepel, kivéve, ha az erőforráscsoport megosztása más projektek. Ennek az erőforrásnak a korábban megadott nevét a tárfiók. Válassza ki a tárfiók nevét.
1. Válassza ki a **Blobok** csempék.
1. Válassza ki a **adfgetstarted** tároló. Megjelenik egy nevű mappába **hivescripts**.
1. Nyissa meg a mappát, és ellenőrizze, hogy a minta parancsfájlt tartalmaz **hivescript.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Megismerheti az Azure Data Factory-tevékenység

[Az Azure Data Factory](../data-factory/introduction.md) amellyel előkészíthető és automatizálható az adatáthelyezési és -átalakítási adatok. Az Azure Data Factory hozhat létre egy HDInsight Hadoop cluster – igény egy bemeneti adatszelet feldolgozásához, és törölje a fürtöt, ha a feldolgozás befejeződött. 

Az Azure Data Factoryben adat-előállító egy vagy több adatfolyamatok is rendelkezik. Egy adatfolyamat rendelkezik egy vagy több tevékenység. A tevékenységek két típusa van:

- [Adattovábbítási tevékenységek](../data-factory/copy-activity-overview.md) -adattovábbítási tevékenységek használatával helyezze át az adatokat egy forrásadattárból egy céladattárba.
- [Adat-átalakítási tevékenységeket](../data-factory/transform-data.md). Adatátalakítási tevékenységek használatával adatok átalakíthatók/feldolgozhatók. HDInsight Hive-tevékenység az egyik a Data Factory által támogatott Adatátalakítási tevékenységeket. Ebben az oktatóanyagban használja a Hive átalakítási tevékenységet.

Ez a cikk a Hive-tevékenység, amely egy igény szerinti HDInsight Hadoop-fürt létrehozása fog konfigurálni. Ha a tevékenység futtatása az adatok feldolgozásához, ez történik:

1. Egy HDInsight Hadoop-fürt automatikusan jön létre, just-in-time dolgozza fel a szeletet. 

1. A bemeneti adatokat dolgozza fel a fürtön futó HiveQL-parancsfájlt. Ebben az oktatóanyagban a hive-tevékenység társított HiveQL-parancsfájlt a következő műveleteket hajtja végre:
    
    - Használja a meglévő táblázat (*hivesampletable*) egy másik tábla létrehozása **HiveSampleOut**.
    - Tölti fel a **HiveSampleOut** az eredeti csak adott oszlopokkal rendelkező tábla *hivesampletable*.
    
1. A HDInsight Hadoop-fürt törlődik a feldolgozás befejezése és a beállított időn (timeToLive-beállítást) üresjáratban a fürt után. A következő adatszelet feldolgozásra a timeToLive üresjárati idő érhető el, ha ugyanazon a fürtön segítségével dolgozza fel a szeletet.  

## <a name="create-a-data-factory"></a>Data factory létrehozása

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).

1. Az Azure Portalon válassza ki a **erőforrás létrehozása** > **adatok + analitika** > **adat-előállító**.

    ![A portálon az Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Azure Data Factory a portálon")

1. Adja meg vagy válassza ki az értékeket az alábbi képernyőképen látható módon:

    ![Az Azure portal segítségével Azure Data Factory létrehozása](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "létrehozása az Azure Data Factory az Azure portal használatával")

    Adja meg vagy válassza ki a következő értékeket:
    
    |Tulajdonság  |Leírás  |
    |---------|---------|
    |**Name (Név)** |  Adja meg az adat-előállító nevét. A névnek globálisan egyedinek kell lennie.|
    |**Előfizetés**     |  Válassza ki az Azure-előfizetését. |
    |**Erőforráscsoport**     | Válassza ki **meglévő** , és válassza ki az erőforráscsoportot, a PowerShell-szkripttel létrehozott. |
    |**Verzió**     | Válassza ki **V2 (előzetes verzió)** |
    |**Hely**     | A hely automatikusan értéke a korábban az erőforráscsoport létrehozásakor megadott helyre. A jelen oktatóanyag esetében a hely értéke **USA keleti RÉGIÓJA 2**. |
    

1. Válassza ki **rögzítés az irányítópulton**, majd válassza ki **létrehozás**. A portál irányítópultján egy új csempe jelenik meg **Üzembe helyezés beküldése** címmel. Adat-előállító létrehozása eltarthat bárhol 2 – 4 perc között.

    ![Sablon üzembe helyezés folyamatban](./media/hdinsight-hadoop-create-linux-clusters-adf/deployment-progress-tile.png "sablon üzembe helyezés folyamatban") 
 
1. Az adat-előállító létrehozása után a portál megjeleníti az adat-előállító áttekintésével.

    ![Az Azure Data Factory áttekintése](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Azure Data Factory – áttekintés")

1. Válassza ki **létrehozás és Monitorozás** az Azure Data Factory szerzői és figyelési portál elindításához.

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása

Ebben a szakaszban két társított szolgáltatást belül az adat-előállító létrehozásához.

- Egy **Azure Storage-beli társított szolgáltatást**, amely egy Azure Storage-fiókot társít az adat-előállítóhoz. Ezt a tárterületet csak az igény szerinti HDInsight-fürt használja. Ez tartalmazza a fürtön futtatott Hive-szkriptet is.
- Egy **igény szerinti HDInsight társított szolgáltatást**. Az Azure Data Factory automatikusan létrehoz egy HDInsight-fürtöt, és a Hive-parancsfájlt futtatja. Ezt követően pedig törli a HDInsight-fürtöt, miután a fürt egy előre meghatározott ideig tétlen volt.

###  <a name="create-an-azure-storage-linked-service"></a>Azure Storage-beli társított szolgáltatás létrehozása

1. A bal oldali ablaktáblán, a **első lépések** lapon válassza ki a **szerkesztése** ikonra.

    ![Az Azure Data Factoryhoz a társított szolgáltatás létrehozása](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "egy Azure Data Factoryhoz a társított szolgáltatás létrehozása")

1. Válassza ki **kapcsolatok** az ablakot, és válassza ki bal alsó sarkában **+ új**.

    ![Kapcsolatok létrehozása az Azure Data Factoryban](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "kapcsolatok létrehozása az Azure Data Factoryban")

1. Az a **új társított szolgáltatás** párbeszédpanelen jelölje ki **Azure Blob Storage** majd **Folytatás**.

    ![Létrehozás az Azure Storage társított szolgáltatás adat-előállító](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "létrehozása az Azure Storage társított szolgáltatás a Data Factory")

1. Adja meg a storage-beli társított szolgáltatás nevét, válassza ki a létrehozott Azure Storage-fiókot a PowerShell-szkript részeként, és válassza **Befejezés**.

    ![Adjon meg nevet az Azure Storage társított szolgáltatás](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "adjon meg nevet az Azure Storage társított szolgáltatás")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Igény szerinti HDInsight társított szolgáltatás létrehozása

1. Kattintson ismét az **+ Új** gombra egy további társított szolgáltatás létrehozásához.

1. Az **Új társított szolgáltatás** ablakban válassza a **Compute** > **Azure HDInsight** lehetőséget, majd kattintson a **Folytatás** gombra.

    ![Létrehozás HDInsight társított szolgáltatást az Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "létrehozása HDInsight társított szolgáltatás az Azure Data Factoryhoz")

1. Az a **új társított szolgáltatás** ablakban adja meg a szükséges értékeket.

    ![Adjon meg értékeket a HDInsight társított szolgáltatás](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "adjon meg értékeket a HDInsight társított szolgáltatás")

    Adja meg a következő értékeket, és az alapértelmezett a többit bízza.

    | Tulajdonság | Leírás |
    | --- | --- |
    | Name (Név) | Adjon meg egy nevet a HDInsight társított szolgáltatás |
    | Típus | Válassza ki **igény szerinti HDInsight** |
    | Azure Storage társított szolgáltatás | Válassza ki a korábban létrehozott Storage-beli társított szolgáltatást. |
    | Fürttípus | Válassza ki **hadoop** |
    | Élettartam | Adja meg az időtartamot, amelyek esetében szeretne a HDInsight-fürt automatikusan törlést megelőzően érhető el.|
    | Szolgáltatásnév azonosítója | Adja meg az Előfeltételek részeként létrehozott Azure Active Directory egyszerű szolgáltatás Alkalmazásazonosítója |
    | Szolgáltatásnév kulcsa | Adja meg a hitelesítési kulcsot az Azure Active Directory egyszerű szolgáltatás |
    | Fürt nevének előtagja | Egy érték, amely az előtaggal fog nyújtson a data factory által létrehozott összes fürttípusok |
    | Erőforráscsoport | Válassza ki a létrehozott erőforráscsoportot a korábban használt PowerShell-parancsfájl részeként| 
    | Fürt SSH-felhasználónév | Adjon meg egy SSH-felhasználónév |
    | Fürt SSH-jelszó | Adjon meg egy jelszót az SSH-felhasználó |

    Válassza a **Finish** (Befejezés) elemet.

## <a name="create-a-pipeline"></a>Folyamat létrehozása

1. Válassza a **+** (plusz) gombot, majd a **Folyamat** elemet.

    ![Folyamat létrehozása az Azure Data Factoryban](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "az Azure Data Factory-folyamat létrehozása")

1. Az a **tevékenységek** eszközkészletben bontsa ki a **HDInsight**, és húzza a **Hive** tevékenységet a folyamat tervezőfelületére. Az a **általános** lapra, adja meg a tevékenység nevét.

    ![Tevékenységek hozzáadása a Data Factory-folyamatot](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "tevékenységek hozzáadása a Data Factory-folyamatot")

1. Győződjön meg arról, hogy a Hive-tevékenység kiválasztva, válassza ki a **HDI-fürt** fülre, és a **HDInsight társított szolgáltatás** legördülő menüben válassza ki a korábban létrehozott HDInsight társított szolgáltatást.

    ![Adja meg a HDInsight-fürt részletes adatai a folyamat](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "adja meg a HDInsight fürt részletes adatai, a folyamat")

1. Válassza ki a **parancsfájl** lapra, és kövesse az alábbi lépéseket:
    
    1. A **parancsprogram társított szolgáltatás**válassza **HDIStorageLinkedService**. Ezt az értéket egy korábban létrehozott storage-beli társított szolgáltatás.
    
    1. A **fájl elérési útja**válassza **Browse Storage** , és keresse meg a helyet, ahol a minta Hive-parancsfájl áll rendelkezésre. Ha korábban a PowerShell-szkriptet futtatta, ezen a helyen kell lennie a `adfgetstarted/hivescripts/hivescript.hql`.
    
        ![Adja meg a folyamat Hive-szkript adatait](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "biztosítanak Hive-szkript adatait a folyamat")
    
    1. Alatt **speciális** > **paraméterek**válassza **automatikus kitöltés parancsprogram**. Ez a beállítás keres a Hive-parancsfájl a paramétereket, futásidőben értékekre van szükség. A parancsfájlt használja (**hivescript.hql**) rendelkezik egy **kimeneti** paraméter. Az érték a következő formátumban adja meg `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` , hogy az Azure Storage egy létező mappára mutasson. Az elérési út megkülönbözteti a kis- és nagybetűket. Ez az, hogy az a szkript kimenetének tárolására szolgáló elérési utat.
    
        ![Adja meg a Hive-parancsfájl paraméterek](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "paramétereket adja meg a Hive-parancsfájl")

1. Válassza ki **ellenőrzése** a folyamat érvényesítéséhez. Az érvényesítési ablak bezárásához kattintson a **>>** (jobbra mutató nyíl) gombra.

    ![Az Azure Data Factory-folyamat ellenőrzése](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "az Azure Data Factory-folyamat ellenőrzése")

1. Végül válassza **összes közzététele** összetevők közzététele az Azure Data Factory.

    ![Közzététel az Azure Data Factory-folyamatot](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "közzététele az Azure Data Factory-folyamatot")

## <a name="trigger-a-pipeline"></a>Folyamat aktiválása

1. A Tervező felületére az eszköztáron válassza **eseményindító** > **Aktiválás most**.

    ![Az Azure Data Factory-folyamat aktiválása](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "az Azure Data Factory-folyamat aktiválása")

1. Válassza ki **Befejezés** a felugró oldalsó sáv.

## <a name="monitor-a-pipeline"></a>Folyamat monitorozása

1. Váltson a bal oldali **Monitorozás** lapra. Ekkor a folyamat futása megjelenik a **Pipeline Runs** (Folyamatfuttatások) listában. Figyelje meg, hogy a futtatáshoz az állapotát a **állapot** oszlop.

    ![Az Azure Data Factory-folyamatok figyelése](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "az Azure Data Factory-folyamatok figyelése")

1. Az állapot frissítéséhez kattintson a **Frissítés** gombra.

1. Választhatja a **Tevékenységfuttatások megtekintése** a folyamat társított ikonra kattintva megtekintheti a tevékenység futtatásához. Az alábbi képernyőképen a futtatásához, mivel csak egy tevékenység a létrehozott folyamat csak egyetlen tevékenység látható. Váltás az előző nézethez való visszatéréshez, válassza ki a **folyamatok** felé a lap tetején.

    ![Az Azure Data Factory folyamat tevékenységének figyelése](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "a Azure Data Factory-folyamat tevékenységének figyelése")


## <a name="verify-the-output"></a>Kimenet ellenőrzése

1. A kimenet ellenőrzése az Azure Portalon lépjen a storage-fiókot, amelyet ebben az oktatóanyagban használt. A következő mappák vagy tárolók kell megjelennie:

    - Megjelenik egy **adfgerstarted/e** , amely tartalmazza a folyamat részeként futó Hive-szkript kimenetét.

    - Megjelenik egy **adfhdidatafactory -\<társított szolgáltatás neve >-\<időbélyeg >** tároló. Ez a tároló, a HDInsight-fürt, amely a folyamat futásának részeként lett létrehozva alapértelmezett tárolási helyét.

    - Megjelenik egy **adfjobs** tároló, amelyben az Azure Data Factory feladat naplózza.  

        ![Az Azure Data Factory-folyamat kimenet ellenőrzése](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "az Azure Data Factory-folyamat kimenet ellenőrzése")


## <a name="clean-up-the-tutorial"></a>Az oktatóanyag tartalmának törlése

-A a-deman HDInsight-fürt létrehozása, akkor nem kell explicit módon törli a HDInsight-fürt. A fürt törlődik a folyamat létrehozásakor megadott konfigurációja alapján. Azonban a fürt törlése után is a fürthöz társított storage-fiókok továbbra is létezik. Ez szándékosan van is, hogy az adatok sértetlenek. Azonban ha nem szeretné megőrizni az adatokat, törölheti a létrehozott tárfiókot.

Másik lehetőségként törölheti a teljes erőforráscsoport ebben az oktatóanyagban létrehozott. Ezzel törli a tárfiókot és az Azure Data Factory létrehozott.

### <a name="delete-the-resource-group"></a>Az erőforráscsoport törlése

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
1. Válassza ki **erőforráscsoportok** a bal oldali panelen.
1. Válassza ki az erőforráscsoport nevét a PowerShell-parancsprogram létrehozott. Használja a szűrőt, ha túl sok erőforráscsoportja felsorolt. Megnyitja az erőforráscsoportot.
1. Az a **erőforrások** csempe, kell rendelkeznie, az alapértelmezett tárfiókot és az adat-előállító felsorolt, kivéve, ha az erőforráscsoport megosztása más projektekhez.
1. Válassza az **Erőforráscsoport törlése** elemet. Ezzel törli a tárfiókot és a storage-fiókban tárolt adatok.

    ![Erőforráscsoport törlése](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "erőforráscsoport törlése")

1. Adja meg az erőforráscsoport nevét a törlés megerősítéséhez, majd válassza ki **törlése**.


## <a name="next-steps"></a>További lépések
Ebben a cikkben megtanulta, hogyan igény szerinti HDInsight-fürt létrehozása, és futtathat Hive-feladatokat az Azure Data Factory használatával. Folytassa a következő artciel megtudhatja, hogyan hozhat létre HDInsight-fürtök egyéni konfigurációval.

> [!div class="nextstepaction"]
>[Az Azure HDInsight-fürtök létrehozása egyéni konfiguráció](hdinsight-hadoop-provision-linux-clusters.md)



---
title: Az Azure Stack storage eszközök |} A Microsoft Docs
description: Ismerje meg az Azure Stack tárolási adatok adatátviteli eszközök
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 46bc043681bbba37a2dfb6476e1dd0784e1abf10
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966696"
---
# <a name="use-data-transfer-tools-for-azure-stack-storage"></a>Adatok átvitele tools for Azure Stack-tároló használata

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

A Microsoft Azure Stack a storage-szolgáltatás, a lemezek, blobok, táblák, üzenetsorok és fiók felügyeleti funkciókat biztosít. Az Azure storage-eszközöket is használhatja, ha szeretné kezelni, vagy helyezze át az adatokat, vagy az Azure Stack-storage-ból. Ez a cikk áttekintést a rendelkezésre álló eszközöket.

A követelmények határozza meg, amely a következő eszközök működik a legjobban az Ön számára:

* [AzCopy](#azcopy)

    Egy storage-specifikus, a parancssori segédprogram, amely adatokat másol egy objektumot egy másik objektumhoz tárfiókon belül vagy tárfiókok között letöltheti.

* [Azure PowerShell](#azure-powershell)

    Egy feladat, a parancssori rendszerhéj és parancsnyelv, amely kifejezetten.

* [Azure CLI](#azure-cli)

    Egy nyílt forráskódú, platformfüggetlen eszköz, amely az Azure és az Azure Stack platformok használata parancsokat tartalmaz.

* [Microsoft storage explorer használatával](#microsoft-azure-storage-explorer)

    Egy könnyen használható önálló alkalmazást egy felhasználói felületet.

* [Blobfuse ](#blobfuse)

    A virtuális fájlrendszer illesztőprogramja az Azure Blob Storage, amely lehetővé teszi, hogy a meglévő blokkblobok adataival a tárfiókban lévő hozzáférnie a Linux-fájlrendszer. 

Miatt a tárolási szolgáltatások az Azure és az Azure Stack közötti különbségeket van néhány konkrét követelmények az egyes eszközök a következő szakaszok ismertetik. Azure Stack storage és az Azure storage összehasonlításáért lásd: [Azure Stack-tároló: különbségek és szempontok](azure-stack-acs-differences.md).

## <a name="azcopy"></a>AzCopy

Az AzCopy parancssori segédprogram az adatmásolás egyszerű parancs használatával az optimális teljesítmény érdekében a Microsoft Azure blob és table storage szolgáltatásba vagy onnan tervezett. Másolhat adatokat egy objektumot a másikra tárfiókon belül vagy tárfiókok között.

### <a name="download-and-install-azcopy"></a>Töltse le és telepítse az Azcopyval

Az AzCopy segédprogram két verziója: a Windows és Linux rendszeren az AzCopy az AzCopy.

 - **AzCopy Windowson**
    - Töltse le az AzCopy támogatott verzióját az Azure Stackhez. Telepítse, és ugyanúgy, mint az Azure használata az AzCopy az Azure Stacken. További tudnivalókért lásd: [az AzCopy Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy).
        - Az 1802 frissítés vagy újabb verziók [töltse le az AzCopy 7.1.0](https://aka.ms/azcopyforazurestack20170417).
        - A korábbi verziók [töltse le az AzCopy 5.0.0](https://aka.ms/azcopyforazurestack20170417).

 - **AzCopy Linuxon**

    - AzCopy Linuxon futó Azure Stack 1802-es frissítés és újabb verzióin támogatja. Telepítse, és ugyanúgy, mint az Azure használata az AzCopy az Azure Stacken. További tudnivalókért lásd: [AzCopy linuxon](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux).

### <a name="azcopy-command-examples-for-data-transfer"></a>Adatátvitel az AzCopy parancs példák

Az alábbi példák hajtsa végre az adatok másolása, és az Azure Stack-blobokból jellemző forgatókönyvei. További tudnivalókért lásd: [az AzCopy Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) és [AzCopy linuxon](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux).

### <a name="download-all-blobs-to-a-local-disk"></a>Az összes blobokat a helyi lemez

**Windows**

````AzCopy
AzCopy.exe /source:https://myaccount.blob.local.azurestack.external/mycontainer /dest:C:\myfolder /sourcekey:<key> /S
````

**Linux**

````AzCopy
azcopy \
    --source https://myaccount.blob.local.azurestack.external/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
````

### <a name="upload-single-file-to-virtual-directory"></a>Virtuális könyvtár egyetlen fájl feltöltése

**Windows**

```AzCopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.local.azurestack.external/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

**Linux**

````AzCopy
azcopy \
    --source /mnt/myfiles/abc.txt \
    --destination https://myaccount.blob.local.azurestack.external/mycontainer/vd/abc.txt \
    --dest-key <key>
````

### <a name="move-data-between-azure-and-azure-stack-storage"></a>Adatok áthelyezése Azure és az Azure Stack-tárolás között

Az Azure storage és az Azure Stack közötti aszinkron adatátvitel nem támogatott. Meg kell adnia az átvitelt, az a **/SyncCopy** vagy **--szinkronizálási másolási** lehetőséget.

**Windows**

````AzCopy
Azcopy /Source:https://myaccount.blob.local.azurestack.external/mycontainer /Dest:https://myaccount2.blob.core.windows.net/mycontainer2 /SourceKey:AzSKey /DestKey:Azurekey /S /SyncCopy
````

**Linux**

````AzCopy
azcopy \
    --source https://myaccount1.blob.local.azurestack.external/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --sync-copy
````

### <a name="azcopy-known-issues"></a>Az Azcopy ismert problémák

 - Bármely fájl áruházbeli AzCopy művelet nem érhető el, mert a file storage még nem áll rendelkezésre az Azure Stackben.
 - Az Azure storage és az Azure Stack közötti aszinkron adatátvitel nem támogatott. Megadhatja, hogy az átvitel és a **/SyncCopy** lehetőség az adatok másolásához.
 - A Linux az Azcopy verzióval csak 1802 frissítés vagy újabb verzió. És nem támogatja a Table Storage-szolgáltatás.

## <a name="azure-powershell"></a>Azure PowerShell

Az Azure PowerShell modul parancsmagokat biztosít az Azure és az Azure Stack-szolgáltatások kezeléséhez az. Ez nem egy feladatalapú, parancssori rendszerhéj és parancsnyelv, amely kifejezetten.

### <a name="install-and-configure-powershell-for-azure-stack"></a>PowerShell telepítése és konfigurálása az Azure Stackhez

Az Azure Stack kompatibilis az Azure PowerShell-modulok használata az Azure Stack van szükség. További információkért lásd: [Azure Stack PowerShell telepítése](azure-stack-powershell-install.md) és [konfigurálása a PowerShell-környezet az Azure Stack felhasználói](azure-stack-powershell-configure-user.md) további.

### <a name="powershell-sample-script-for-azure-stack"></a>PowerShell példaszkript az Azure Stackhez 

Ez a minta azt feltételezik, hogy sikeresen [PowerShell telepítve van az Azure Stack](azure-stack-powershell-install.md). Ez a szkript segít a konfigurálás befejezéséhez, és kérje meg az Azure Stack-bérlő hitelesítő adatokat adja hozzá a helyi PowerShell környezetben. Ezt követően a szkript lesz az alapértelmezett Azure-előfizetés beállítása, hozzon létre egy új tárfiókot az Azure-ban, létrehozhat egy új tárolót az új tárfiók és meglévő képfájl (blob) feltöltése a tárolóba. Miután a parancsfájl listázza a tárolóban lévő összes BLOB, azt fogja hozzon létre egy új cél könyvtárat a helyi számítógépen, töltse le a rendszerképet.

1. Telepítés [Azure Stack-kompatibilis Azure PowerShell-modulok](azure-stack-powershell-install.md).
2. Töltse le a [az Azure Stack működéséhez szükséges eszközök](azure-stack-powershell-download.md).
3. Nyissa meg **Windows PowerShell ISE-ben** és **Futtatás rendszergazdaként**, kattintson a **fájl** > **új** hozhat létre egy új parancsfájlt.
4. Másolja az alábbi parancsfájlt, és illessze be az új parancsfájl-fájlba.
5. Frissítse a konfigurációs beállítások alapján a parancsfájl-változókat.
   > [!NOTE]
   > A szkriptre a gyökérkönyvtárban a futtatandó **AzureStack_Tools**.

```PowerShell  
# begin

$ARMEvnName = "AzureStackUser" # set AzureStackUser as your Azure Stack environemnt name
$ARMEndPoint = "https://management.local.azurestack.external" 
$GraphAudiance = "https://graph.windows.net/" 
$AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com" 

$SubscriptionName = "basic" # Update with the name of your subscription.
$ResourceGroupName = "myTestRG" # Give a name to your new resource group.
$StorageAccountName = "azsblobcontainer" # Give a name to your new storage account. It must be lowercase.
$Location = "Local" # Choose "Local" as an example.
$ContainerName = "photo" # Give a name to your new container.
$ImageToUpload = "C:\temp\Hello.jpg" # Prepare an image file and a source directory in your local computer.
$DestinationFolder = "C:\temp\downlaod" # A destination directory in your local computer.

# Import the Connect PowerShell module"
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
Import-Module .\Connect\AzureStack.Connect.psm1

# Configure the PowerShell environment
# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRmEnvironment -Name $ARMEvnName -ARMEndpoint $ARMEndPoint 

# Set the GraphEndpointResourceId value
Set-AzureRmEnvironment -Name $ARMEvnName -GraphEndpoint $GraphAudience

# Login
$TenantID = Get-AzsDirectoryTenantId -AADTenantName $AADTenantName -EnvironmentName $ARMEvnName
Add-AzureRmAccount -EnvironmentName $ARMEvnName -TenantId $TenantID 

# Set a default Azure subscription.
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a new Resource Group 
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Create a new storage account.
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS

# Set a default storage account.
Set-AzureRmCurrentStorageAccount -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName 

# Create a new container.
New-AzureStorageContainer -Name $ContainerName -Permission Off

# Upload a blob into a container.
Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

# List all blobs in a container.
Get-AzureStorageBlob -Container $ContainerName

# Download blobs from the container:
# Get a reference to a list of all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName

# Create the destination directory.
New-Item -Path $DestinationFolder -ItemType Directory -Force  

# Download blobs into the local destination directory.
$blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder

# end
````

### <a name="powershell-known-issues"></a>Ismert problémák PowerShell

Aktuális kompatibilis az Azure PowerShell modul verzióját az Azure Stack a felhasználói műveletek 1.2.11. Ez eltér az Azure PowerShell legújabb verzióját. Ez a különbség a tárolási szolgáltatások művelet hatással van:

* A visszaadott érték formátuma `Get-AzureRmStorageAccountKey` verzióban 1.2.11 kettő olyan tulajdonság tartozik: `Key1` és `Key2`, míg a jelenlegi Azure verziót tartalmazó összes fiókkulcsok tömböt ad vissza.

   ```
   # This command gets a specific key for a storage account, 
   # and works for Azure PowerShell version 1.4, and later versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Value[0]

   # This command gets a specific key for a storage account, 
   # and works for Azure PowerShell version 1.3.2, and previous versions.
   (Get-AzureRmStorageAccountKey -ResourceGroupName "RG01" `
   -AccountName "MyStorageAccount").Key1

   ```

   További információkért lásd: [Get-AzureRmStorageAccountKey](https://docs.microsoft.com/powershell/module/azurerm.storage/Get-AzureRmStorageAccountKey?view=azurermps-4.1.0).

## <a name="azure-cli"></a>Azure CLI

Az Azure CLI az Azure parancssori felülete Azure-erőforrások kezeléséhez. Vagy telepítheti macOS, Linux és Windows, és futtassa a parancssorból.

Az Azure CLI van optimalizálva, kezeléséhez és az Azure-erőforrások felügyelete a parancssorból, és az Azure Resource manageren automatizálási szkriptek létrehozására készült. Itt található az Azure Stack portálon, beleértve a részletes adatok elérésére ugyanazok a funkciók számos.

Az Azure Stack az Azure CLI 2.0-s vagy újabb verziójára van szükség. Telepítése és konfigurálása az Azure CLI az Azure Stack használatával kapcsolatos további információkért lásd: [telepítése és konfigurálása az Azure Stack parancssori felület](azure-stack-version-profiles-azurecli2.md). Több feladatait az Azure Stack tárfiókok található erőforrásokkal dolgozik az Azure CLI használatával kapcsolatos további információkért lásd: [és az Azure storage az Azure CLI használatával](../../storage/storage-azure-cli.md)

### <a name="azure-cli-sample-script-for-azure-stack"></a>Azure CLI-példa szkript az Azure Stackhez

Miután elvégezte a parancssori felület telepítése és konfigurálása, próbálkozzon az alábbi lépéseket egy kis rendszerhéj minta parancsfájl használatával kommunikálhat az Azure Stack tárolási erőforrások használata. A parancsfájl futása befejeződött, a következő műveleteket:

* Létrehoz egy új tárolót a storage-fiókban.
* A tároló feltölt egy már létező fájlt (a blob).
* Listázza a tárolóban lévő összes BLOB.
* Letölti a fájlt a helyi számítógépen, amely megad egy célhelyre.

A parancsfájl futtatása előtt győződjön meg arról, hogy képes sikeresen csatlakozni, és jelentkezzen be a cél Azure Stack.

1. Nyissa meg a kedvenc szövegszerkesztőjével, majd másolja és illessze be az előző parancsfájlt a szerkesztőbe.
2. A parancsfájl-változókat, hogy a konfigurációs beállítások frissítése.
3. A szükséges változók frissítése után mentse a parancsfájlt, és lépjen ki a saját szerkesztőben. A következő lépések azt feltételezik, hogy a parancsfájl nevű **my_storage_sample.sh**.
4. Megjelölés végrehajtható, mint a szkriptet, szükség esetén: `chmod +x my_storage_sample.sh`
5. A szkript végrehajtása. Ha például a Bash: `./my_storage_sample.sh`

```bash
#!/bin/bash
# A simple Azure Stack storage example script

export AZURESTACK_RESOURCE_GROUP=<resource_group_name>
export AZURESTACK_RG_LOCATION="local"
export AZURESTACK_STORAGE_ACCOUNT_NAME=<storage_account_name>
export AZURESTACK_STORAGE_CONTAINER_NAME=<container_name>
export AZURESTACK_STORAGE_BLOB_NAME=<blob_name>
export FILE_TO_UPLOAD=<file_to_upload>
export DESTINATION_FILE=<destination_file>

echo "Creating the resource group..."
az group create --name $AZURESTACK_RESOURCE_GROUP --location $AZURESTACK_RG_LOCATION

echo "Creating the storage account..."
az storage account create --name $AZURESTACK_STORAGE_ACCOUNT_NAME --resource-group $AZURESTACK_RESOURCE_GROUP --account-type Standard_LRS

echo "Creating the blob container..."
az storage container create --name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Uploading the file..."
az storage blob upload --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --file $FILE_TO_UPLOAD --name $AZURESTACK_STORAGE_BLOB_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME

echo "Listing the blobs..."
az storage blob list --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --output table

echo "Downloading the file..."
az storage blob download --container-name $AZURESTACK_STORAGE_CONTAINER_NAME --account-name $AZURESTACK_STORAGE_ACCOUNT_NAME --name $AZURESTACK_STORAGE_BLOB_NAME --file $DESTINATION_FILE --output table

echo "Done"
````

## <a name="microsoft-azure-storage-explorer"></a>A Microsoft Azure storage Explorerrel

A Microsoft Azure storage explorer egy önálló alkalmazás, a Microsoft. Ez lehetővé teszi, hogy egyszerűen dolgozhat Azure storage és az Azure Stack tárolási adatok Windows, macOS és Linux rendszerű számítógépek. Ha azt szeretné, hogy egy egyszerű módja annak, hogy az Azure Stack tárolási adatok kezelésére, majd fontolja meg a Microsoft Azure storage explorer használatával.

* Az Azure storage explorer használata az Azure Stack konfigurálásával kapcsolatos további tudnivalókért lásd: [storage explorer csatlakoztatása Azure Stack-előfizetéshez](azure-stack-storage-connect-se.md).
* A Microsoft Azure storage Explorerrel kapcsolatos további információkért lásd: [storage explorer használatának első lépései](../../vs-azure-tools-storage-manage-with-storage-explorer.md)

## <a name="blobfuse"></a>Blobfuse 

[Blobfuse](https://github.com/Azure/azure-storage-fuse) egy virtuális fájlrendszer illesztőprogramja van az Azure Blob Storage, amely lehetővé teszi, hogy a meglévő blokkblobok adataival a tárfiókban lévő hozzáférnie a Linux-fájlrendszer. Az Azure Blob Storage-objektum tárolási szolgáltatás, és így nem kell egy hierarchikus névtér. Blobfuse biztosít a névtér a virtuális direcectory séma használatával ferde használatával `/` elválasztóként. Blobfuse Azure és az Azure Stackben egyaránt használható. 

A Blob storage csatlakoztatása az Blobfuse fájlrendszerként Linux rendszeren kapcsolatos további információkért lásd: [Blobfuse a Blob-tároló csatlakoztatása fájlrendszerként hogyan](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux). 

Az Azure Stack **blobEndpoint** meg kell határozni mellett accountName, accountKey/sasToken, containerName, a lépésben előkészíti a csatlakoztatáshoz szükséges a Tárfiók hitelesítő adatainak konfigurálása közben. 

Az Azure Stack development Kit a blobEndpoint kell `myaccount.blob.local.azurestack.external`. Az Azure Stackkel integrált rendszereknél forduljon a felhő rendszergazdájához, ha nem biztos a végpont kapcsolatban. 

Vegye figyelembe, hogy accountKey és sasToken csak lehet konfigurált egyszerre csak egy. Tárfiók kulcsa van megadva, a hitelesítő adatok konfigurációs fájl esetén a következő formátumban: 

```text  
    accountName myaccount 
    accountKey myaccesskey== 
    containerName mycontainer 
    blobEndpoint myaccount.blob.local.azurestack.external
```

Megosztott hozzáférési jogkivonatot kap, amikor a következő formátumban kell megadni a hitelesítő adatok konfigurációs fájl:

```text  
    accountName myaccount 
    sasToken ?mysastoken 
    containerName mycontainer 
    blobEndpoint myaccount.blob.local.azurestack.external
```

## <a name="next-steps"></a>További lépések

* [Storage explorer csatlakoztatása Azure Stack-előfizetéshez](azure-stack-storage-connect-se.md)
* [Ismerkedés a storage Explorerben](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure-konzisztens tároló: különbségek és szempontok](azure-stack-acs-differences.md)
* [A Microsoft Azure storage bemutatása](../../storage/common/storage-introduction.md)

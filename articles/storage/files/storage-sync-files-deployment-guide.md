---
title: Az Azure File Sync üzembe helyezése |} A Microsoft Docs
description: Ismerje meg, hogyan helyezhet üzembe az Azure File Sync elejétől a végéig.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 261f8dfe41ece0cd56a4a71972e3142ef8440afb
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918109"
---
# <a name="deploy-azure-file-sync"></a>Az Azure File Sync üzembe helyezése
Az Azure File Sync használatával fájlmegosztásainak a szervezet az Azure Files között, miközben gondoskodik a rugalmasságát, teljesítményét és kompatibilitását a helyszíni fájlkiszolgálók. Az Azure File Sync Windows Server az Azure-fájlmegosztás gyors gyorsítótáraivá alakítja át. Helyileg, az adatok eléréséhez a Windows Serveren elérhető bármely protokollt használhatja, beleértve az SMB, NFS és FTPS. Tetszőleges számú gyorsítótárak világszerte igény szerint is rendelkezhet.

Javasoljuk, hogy olvasási [Azure Files üzembe helyezésének megtervezése](storage-files-planning.md) és [Azure File Sync üzembe helyezésének megtervezése](storage-sync-files-planning.md) a jelen cikkben ismertetett lépések végrehajtása előtt.

## <a name="prerequisites"></a>Előfeltételek
* Az Azure storage-fiók és egy Azure fájlmegosztás ugyanabban a régióban, amelyet szeretne az Azure File Sync üzembe helyezése. További információkért lásd:
    - [Régiónkénti elérhetőség](storage-sync-files-planning.md#region-availability) Azure File Sync.
    - [Hozzon létre egy tárfiókot](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) részletes leírása a storage-fiók létrehozása.
    - [Fájlmegosztás létrehozása](storage-how-to-create-file-share.md) részletes leírása a fájlmegosztás létrehozása.
* Legalább egy támogatott példány szinkronizálása az Azure File Sync használatával Windows Server vagy Windows Server-fürt. A Windows Server támogatott verzióival kapcsolatos további információkért lásd: [együttműködés a Windows Server](storage-sync-files-planning.md#azure-file-sync-system-requirements-and-interoperability).
* Győződjön meg arról, a Windows Server PowerShell 5.1 telepítve van. Windows Server 2012 R2 használatakor győződjön meg arról, hogy, vagy újabb rendszerű PowerShell 5.1. \*. Nyugodtan kihagyhatja ezt az ellenőrzést a Windows Server 2016, az alapértelmezett verzió out-of-box PowerShell 5.1-jébe. A Windows Server 2012 R2 ellenőrizheti, hogy a PowerShell 5.1 futnak. \* megnézzük az értékét a **PSVersion** tulajdonságát a **$PSVersionTable** objektum:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    Ha a PSVersion érték kisebb, mint az 5.1-es. \*, mint az eset áll fenn a legtöbb újonnan telepített Windows Server 2012 R2, letöltése és telepítése által könnyedén frissíthet [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). A megfelelő csomag letöltése és telepítése a Windows Server 2012 R2 **Win8.1AndW2K12R2 KB-os\*\*\*\*\*\*\*-x64.msu**.

    > [!Note]  
    > Az Azure File Sync még nem támogatja a PowerShell 6 Windows Server 2012 R2 vagy Windows Server 2016-ban.
* Az AzureRM PowerShell-modul a kiszolgálókon szeretné használni az Azure File Sync használatával. Az AzureRM PowerShell-modulok telepítésével kapcsolatos további információkért lásd: [Azure PowerShell telepítése és konfigurálása](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Javasoljuk, hogy mindig az Azure PowerShell-modulok legújabb verzióját használja. 

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Készítse elő a Windows Serverről az Azure File Sync használata
Tiltsa le minden olyan kiszolgálón, az Azure File Sync, többek között az egyes kiszolgáló-csomópont egy feladatátvevő fürtben használni kívánt **az Internet Explorer fokozott biztonsági beállításai**. Ez azért szükséges, csak a kezdeti kiszolgálói regisztrációhoz. Engedélyezheti újra azt követően a kiszolgáló regisztrálva van.

# <a name="portaltabportal"></a>[Portál](#tab/portal)
1. Nyissa meg a Kiszolgálókezelőt.
2. Kattintson a **helyi kiszolgáló**:  
    ![A Server Manager felhasználói felületén bal oldalán a "helyi kiszolgáló"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. Az a **tulajdonságok** subpane, válassza ki a hivatkozást **Internet Explorer fokozott biztonsági beállításai**.  
    ![A Server Manager felhasználói felületén a "Internet Explorer – fokozott biztonsági beállítások" panelen](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. Az a **az Internet Explorer fokozott biztonsági beállításai** párbeszédpanelen jelölje ki **ki** a **rendszergazdák** és **felhasználók**:  
    ![Az Internet Explorer fokozott biztonsági beállításai pop-ablak "Kikapcsolva" a kijelölt](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Az Internet Explorer fokozott biztonsági beállítások letiltása, egy rendszergazda jogú PowerShell-munkamenetben hajtsa végre az alábbiakat:

```PowerShell
# Disable Internet Explorer Enhanced Security Configuration 
# for Administrators
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Disable Internet Explorer Enhanced Security Configuration 
# for Users
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Force Internet Explorer closed, if open. This is required to fully apply the setting.
# Save any work you have open in the IE browser. This will not affect other browsers,
# including Edge.
Stop-Process -Name iexplore -ErrorAction SilentlyContinue
``` 

---

## <a name="install-the-azure-file-sync-agent"></a>Az Azure File Sync ügynök telepítése
Az Azure File Sync ügynök csomag egy letölthető, amely lehetővé teszi a Windows Serverről az Azure-fájlmegosztások való szinkronizálása megtörténik. 

# <a name="portaltabportal"></a>[Portál](#tab/portal)
Az ügynököt a letöltheti a [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Ha a letöltés befejeződött, kattintson duplán az MSI-csomag az Azure File Sync ügynök telepítésének indításához.

> [!Important]  
> Szeretne használni az Azure File Sync a feladatátvevő fürttel, ha az Azure File Sync ügynök telepítve kell lennie a fürt minden csomópontján. A fürt egyes csomópontjaihoz regisztrálva kell az Azure File Sync használatával működnek.

Javasoljuk, hogy tegye a következőket:
- Hagyja bejelölve az alapértelmezett telepítési útvonaltól (C:\Program Files\Azure\StorageSyncAgent), a hibaelhárítás és a kiszolgáló karbantartás leegyszerűsítésében.
- Engedélyezheti a Microsoft Update, hogy az Azure File Sync mindig naprakészek. Minden frissítés az Azure File Sync ügynök, a szolgáltatás-frissítéseket és gyorsjavításokat, beleértve a fordulhat elő, a Microsoft Update webhelyről. Azt javasoljuk, hogy a legújabb frissítés telepítése az Azure File Sync. További információkért lásd: [Azure File Sync-frissítési szabályzat](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Az Azure File Sync ügynök telepítésének befejezését követően a kiszolgáló regisztrációs felhasználói felület automatikusan megnyílik. Mielőtt regisztrálná; rendelkeznie kell a Társzinkronizálási szolgáltatás Tekintse meg a következő szakaszban a Társzinkronizálási szolgáltatás létrehozása.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Futtassa a következő PowerShell-kódot az operációs rendszer az Azure File Sync ügynök megfelelő verzióját töltse le és telepítse azt a rendszer.

> [!Important]  
> Szeretne használni az Azure File Sync a feladatátvevő fürttel, ha az Azure File Sync ügynök telepítve kell lennie a fürt minden csomópontján. A fürt egyes csomópontjaihoz regisztrálva kell az Azure File Sync használatával működnek.

```PowerShell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875004 `
        -OutFile "StorageSyncAgent.exe" 
}
elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875002 `
        -OutFile "StorageSyncAgent.exe" 
}
else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2 and Windows Server 2016")
}

# Extract the MSI from the install package
$tempFolder = New-Item -Path "afstemp" -ItemType Directory
Start-Process -FilePath ".\StorageSyncAgent.exe" -ArgumentList "/C /T:$tempFolder" -Wait

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "$($tempFolder.FullName)\StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.exe", ".\afstemp" -Recurse -Force
```

---

## <a name="deploy-the-storage-sync-service"></a>A Storage Sync Service telepítése 
Az üzembe helyezés az Azure File Sync kezdődik elhelyezése egy **Társzinkronizálási szolgáltatás** erőforrás egy erőforrás-csoportba, a kijelölt előfizetésben. Azt javasoljuk, ahogyan few ezek kiépítése igény szerint. A kiszolgálók és az erőforrás közötti megbízhatósági kapcsolat fog létrehozni, és csak egy kiszolgálót is regisztrálható egy Társzinkronizálási szolgáltatást. Emiatt javasoljuk, hogy annyi társzinkronizálási szolgáltatás szerint kiszolgálócsoportok szét kell telepíteni. Ne feledje, hogy a kiszolgálók a társzinkronizálási szolgáltatás különböző nem tudják szinkronizálni a egymással.

> [!Note]
> A Storage Sync Service a hozzáférési engedélyek öröklődnek az előfizetés és erőforráscsoport rendelkezik lett üzembe helyezve. Azt javasoljuk, hogy alaposan ellenőrizze ki férhet hozzá. Írási hozzáféréssel rendelkező entitások megkezdheti a kiszolgálók regisztrálva új fájlkészleteket szinkronizálása ezt a tárolót a szinkronizálási szolgáltatás és flow az Azure storage, amely hozzáférhető annak azokat az adatokat.

# <a name="portaltabportal"></a>[Portál](#tab/portal)
A Storage Sync Service telepítése, keresse fel a [az Azure portal](https://portal.azure.com/), kattintson a *új* , és keressen rá az Azure File Sync. A keresési eredmények között, válassza ki a **Azure File Sync**, majd válassza ki **létrehozása** megnyitásához a **Társzinkronizálási üzembe helyezése** fülre.

A megnyíló panelen adja meg a következőket:

- **Név**: egy egyedi nevet a Storage Sync Service (előfizetésenként).
- **Előfizetés**: az előfizetés, amelyben szeretné létrehozni a Storage Sync Service. A szervezet konfigurációs stratégia, attól függően előfordulhat, hogy rendelkezik egy vagy több előfizetés hozzáférést. Azure-előfizetés az a lehető legegyszerűbb tároló, számoljuk el (például az Azure Files) minden felhőszolgáltatás.
- **Erőforráscsoport**: egy erőforráscsoportot az Azure-erőforrások, például a storage-fiók vagy a Storage Sync Service logikai csoportjai. Hozzon létre egy új erőforráscsoportot, vagy használjon egy meglévő erőforráscsoportot az Azure File Sync. (Javasoljuk erőforráscsoportok tárolókként logikailag a szervezetben, például a HR-erőforrások vagy egy adott projekthez tartozó erőforrások csoportosítása erőforrások elkülönítése.)
- **Hely**: A régió, amelyben meg szeretné az Azure File Sync üzembe helyezése. Ebben a listában csak a támogatott régiók érhetők el.

Ha elkészült, válassza ki a **létrehozás** a Storage Sync Service telepítése.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Az Azure File Sync parancsmagokat implementálására, mielőtt szüksége lesz egy DLL-fájl importálását, és hozzon létre egy Azure-fájlszinkronizálás felügyeleti környezet. Ez azért szükséges, mert a parancsmagokat az Azure File Sync egyelőre nem része az AzureRM PowerShell-modul.

> [!Note]  
> A StorageSync.Management.PowerShell.Cmdlets.dll csomagot, amely az Azure File Sync parancsmagokat tartalmaz, (szándékosan) tartalmazza a parancsmag egy nem jóváhagyott művelettel (`Login`). A név `Login-AzureRmStorageSync` megfelelően lett kiválasztva a `Login-AzureRmAccount` az AzureRM PowerShell-modul az aliasát. A hibaüzenet jelenik meg (és a parancsmag) törlődni fog az Azure File Sync ügynök bekerül az AzureRM PowerShell-modul.

```PowerShell
$acctInfo = Login-AzureRmAccount

# The location of the Azure File Sync Agent. If you have installed the Azure File Sync 
# agent to a non-standard location, please update this path.
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzureRMAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzureRmLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzureRmResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzureRmResourceGroup -Name $resourceGroup -Location $region
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region
```

Az Azure File Sync-környezet létrehozása után a `Login-AzureRmStorageSync` parancsmaggal hozhat létre a Storage Sync Service. Ne felejtse el `<my-storage-sync-service>` a Storage Sync Service kívánt nevét.

```PowerShell
$storageSyncName = "<my-storage-sync-service>"
New-AzureRmStorageSyncService -StorageSyncServiceName $storageSyncName
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>Windows-kiszolgálót regisztrálja a Társzinkronizálási szolgáltatás
A Windows Server regisztrálása a Társzinkronizálási szolgáltatás létrehozza a kiszolgáló (vagy fürt) közötti megbízhatósági kapcsolat, és a Storage Sync Service. A kiszolgáló csak egy Társzinkronizálási szolgáltatás regisztrálhatók és szinkronizálhatók más kiszolgálók és az Azure-fájlmegosztások a azonos Storage Sync Service társított.

> [!Note]
> Kiszolgáló regisztrálása Azure hitelesítő adatait használja a Storage Sync Service és a Windows Server, azonban ezt követően a kiszolgáló hoz létre, és maga az identitást használja, amely érvényes mindaddig, amíg a kiszolgáló marad regisztrált közötti megbízhatósági kapcsolat létrehozásához és a aktuális közös hozzáférésű Jogosultságkód (SAS-tároló) token érvényes. Egy új SAS-jogkivonat nem lehet kiadni a kiszolgálón a kiszolgáló regisztrációjának törlése, így az a kiszolgáló elérését az Azure-fájlmegosztások, bármely szinkronizálás leállítása eltávolítása után.

# <a name="portaltabportal"></a>[Portál](#tab/portal)
A kiszolgáló regisztrációs felhasználói felület automatikusan megnyitja az Azure File Sync ügynök telepítése után. Ha nem, manuálisan is megnyithatja, a fájlok helyéről: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Amikor megnyílik a kiszolgáló regisztrációs felhasználói felületén, válassza ki a **bejelentkezési** megkezdéséhez.

Miután bejelentkezett, a rendszer felszólítja a következő információkat:

![A kiszolgáló regisztrációs felhasználói felület képernyőképe](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure-előfizetés**: az előfizetés, amely tartalmazza a Storage Sync Service (lásd: [a Storage Sync Service telepítése](#deploy-the-storage-sync-service)). 
- **Erőforráscsoport**: az erőforráscsoport, amely tartalmazza a Storage Sync Service.
- **Társzinkronizálási szolgáltatás**: a Storage Sync Service, amellyel a regisztrálni kívánt nevét.

Miután kiválasztotta a megfelelő információkat, válassza ki a **regisztrálása** a kiszolgáló regisztráció befejezéséhez. A regisztrációs folyamat részeként a rendszer kér egy további bejelentkezésre.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
$registeredServer = Register-AzureRmStorageSyncServer -StorageSyncServiceName $storageSyncName
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Szinkronizálási csoport és a felhőbeli végpont létrehozása
Szinkronizálási csoport határozza meg, hogy a fájlokat a sync-topológiát. Végpontok egy szinkronizálási csoportban vannak szinkronban egymással. Szinkronizálási csoport tartalmaznia kell egy felhőbeli végpont, amely az Azure-fájlmegosztások és a egy vagy több kiszolgáló végpontot jelenti. Kiszolgálói végpont egy regisztrált kiszolgáló elérési útját jelöli. Egy kiszolgáló több szinkronizálási csoporttal is lehet kiszolgálói végpontot. Tetszőleges számú szinkronizálási csoportok megfelelően írja le a kívánt szinkronizálási topológia igény szerint hozhat létre.

Felhőbeli végpont az Azure-fájlmegosztások mutató. Összes kiszolgálói végpontot a felhőbeli végpont, így a felhőbeli végpont a hub szinkronizálódnak. Az Azure-fájlmegosztást a storage-fiókját és a Storage Sync Service ugyanabban a régióban kell elhelyezkednie. A teljes Azure-fájlmegosztásba való szinkronizálása megtörténik, egy kivétellel: az NTFS-köteten található mappa rejtett "System Volume Information" hasonlítható speciális mappa lesznek üzembe helyezve. Ez a könyvtár neve ". SystemShareInformation". Nem szinkronizálódnak más végpontok szinkronizálási fontos metaadatokat tartalmaz. Ne használja, és törölje azt!

> [!Important]  
> Felhőbeli végpont vagy a szinkronizálási csoportban lévő kiszolgálói végpont módosításokat végezheti el, és a fájlok szinkronizálta már a szinkronizálási csoport végpontja. Ha módosítja a felhőbeli végponthoz (Azure-fájlmegosztás) közvetlenül, módosítások először az Azure File Sync módosítás észlelési feladat deríteniük. A módosítás észlelési feladat csak egyszer 24 óránként indul a felhőbeli végpont. További információkért lásd: [– gyakori kérdések az Azure Files](storage-files-faq.md#afs-change-detection).

# <a name="portaltabportal"></a>[Portál](#tab/portal)
A szinkronizálási csoport létrehozása a [az Azure portal](https://portal.azure.com/)nyissa meg a Storage Sync Service, és válassza ki **+ szinkronizálási csoport**:

![Új szinkronizálási csoport létrehozása az Azure Portalon](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

A megnyíló panelen adja meg a szinkronizálási csoport létrehozása a felhőbeli végpont a következő információkat:

- **Szinkronizálási csoport neve**: létrehozni a szinkronizálási csoport nevét. Ez a név a Storage Sync Service egyedinek kell lennie, de bármilyen nevet, amely logikus az Ön számára.
- **Előfizetés**: az előfizetés, amelybe telepítette a Storage Sync Service a [a Storage Sync Service telepítése](#deploy-the-storage-sync-service).
- **Storage-fiók**: Ha **válassza ki a tárfiókot**, egy másik ablaktáblán jelenik meg, amelyben kiválaszthatja a tárfiók, amely rendelkezik az Azure-fájlmegosztást, amelyet szeretne szinkronizálni.
- **Azure-fájlmegosztás**: a, amellyel szinkronizálni kívánt Azure-fájlmegosztás nevét.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
A szinkronizálási csoport létrehozásához hajtsa végre az alábbi PowerShell-lel. Ne felejtse el kicserélni `<my-sync-group>` a szinkronizálási csoport kívánt nevét.

```PowerShell
$syncGroupName = "<my-sync-group>"
New-AzureRmStorageSyncGroup -SyncGroupName $syncGroupName -StorageSyncService $storageSyncName
```

Ha a szinkronizálási csoport sikeresen létrejött, a felhőbeli végpont is létrehozhat. Ne felejtse el `<my-storage-account>` és `<my-file-share>` a várt értékkel.

```PowerShell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzureRmStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzureStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzureStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzureRmStorageSyncCloudEndpoint `
    -StorageSyncServiceName $storageSyncName `
    -SyncGroupName $syncGroupName ` 
    -StorageAccountResourceId $storageAccount.Id
    -StorageAccountShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>Kiszolgálói végpont létrehozása
Kiszolgálói végpont egy adott helyen, egy regisztrált kiszolgálón, például az egyik mappájába, egy kötetet jelöl. Kiszolgálói végpont egy regisztrált kiszolgáló (ahelyett, hogy egy csatlakoztatott megosztás) elérési útnak kell lennie, és a felhőbeli rétegezést használni, az elérési út egy nem rendszer-köteten kell lennie. Csatlakoztatott hálózati tárolóeszközök (NAS) nem támogatott.

# <a name="portaltabportal"></a>[Portál](#tab/portal)
Kiszolgálói végpont hozzáadásához nyissa meg a szinkronizálási újonnan létrehozott csoportot, majd **kiszolgálói végpont felvétele**.

![Új kiszolgálói végpontok hozzáadása a szinkronizálási csoport panelen](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

Az a **kiszolgálói végpont felvétele** panelen adja meg a kiszolgálói végpont létrehozása a következő információkat:

- **Regisztrált kiszolgáló**: a kiszolgáló vagy fürt, ahol szeretné létrehozni a kiszolgálói végpont nevét.
- **Elérési út**: a szinkronizálási csoport részeként szinkronizálja a Windows Server elérési útját.
- **A felhő adatrétegzés lehetőséget**: egy kapcsoló engedélyezheti vagy tilthatja le a felhőbeli rétegezés. A felhőbeli rétegezés, ritkán használt vagy használt fájlokat a is az Azure Files lesz rétegzett.
- **Szabad terület a köteten**: A minimális szabad területet a köteten, amelyen a kiszolgálói végpont is található fenntartására. Például ha egy köteten, amelyen egyetlen kiszolgálói végpont 50 % szabad terület a köteten van beállítva, körülbelül felét adatmennyiség többszintű az Azure Files. Függetlenül attól, hogy felhőbeli rétegezés engedélyezve van, az Azure-fájlmegosztást az adatok teljes másolata mindig szerepel a szinkronizálási csoport.

A kiszolgálói végpont hozzáadásához válassza **létrehozás**. Vannak, a fájlok mostantól szinkronban tartani az Azure-fájlmegosztást és a Windows Server. 

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Hajtsa végre a következő PowerShell-parancsokat a kiszolgálói végpont létrehozása, és ne felejtse el `<your-server-endpoint-path>` és `<your-volume-free-space>` a kívánt értékekkel.

```PowerShell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering $true `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
}
else {
    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="onboarding-with-azure-file-sync"></a>Bevezetés az Azure File Sync használatával
A javasolt lépéseket üzembe helyezni az Azure File Sync az első az állásidő nulla megőrzése a teljes fájl minőségben és hozzáférés-vezérlési lista (ACL) a következők:
 
1. A Storage Sync Service telepítése.
2. Szinkronizálási csoport létrehozása.
3. Az Azure File Sync-ügynök telepítése a kiszolgálón, a teljes adatkészlet.
4. Regisztrálja a kiszolgálót, és a kiszolgálói végpont létrehozása a megosztáson. 
5. Hajtsa végre a teljes feltöltése az Azure-fájlmegosztást (felhőbeli végpont) szinkronizálás segítségével.  
6. A kezdeti feltöltés befejeződése után telepítse az Azure File Sync ügynök összes többi kiszolgálóját.
7. A többi kiszolgáló minden egyes új fájlmegosztásokat hozhat létre.
8. Hozzon létre kiszolgálóvégpontok új fájlmegosztások felhőalapú rétegzési házirendet, ha szükséges. (Ebben a lépésben a kezdeti beállítás elérhető további tárterületet igényel.)
9. Lehetővé teszik az Azure File Sync ügynök ehhez a gyors visszaállítás, a teljes névtér a tényleges adatátviteli nélkül. A névtér teljes szinkronizálást követően a szinkronizálási motor betelik a helyi lemezterületet a felhő a kiszolgálói végpontot a rétegzési szabályzat alapján. 
10. Győződjön meg arról, szinkronizálása befejeződött, és tesztelje a topológia igény szerint. 
11. Átirányítja a felhasználókat és alkalmazásokat az új megosztásra.
12. Ismétlődő megosztásaitól, a kiszolgálókon szükség esetén törölheti.
 
Ha nem rendelkezik a kezdeti bevezetési extra tárterület, és szeretné csatlakoztatni a meglévő megosztások, előre, is feltöltheti az adatokat az Azure-fájlmegosztások. Ez a módszer javasolt, ha csak akkor fogadja el az állásidő és a feltétlenül garantálják az adatok nem változnak, a kiszolgálómegosztásokon a kezdeti bevezetési folyamat során. 
 
1. Győződjön meg arról, hogy bármely kiszolgálón lévő adatok a bevezetési folyamat során nem módosítható.
2. Előre vetőmag Azure-fájlmegosztások a kiszolgáló adatokkal eszközzel bármely adatok átvitele az SMB-n keresztül például Robocopy, a közvetlen SMB-példányt. Mivel az AzCopy nem tölt fel adatokat az SMB-n keresztül így előre beültetés esetében nem használható.
3. Hozzon létre az Azure File Sync-topológia a kívánt kiszolgálói végpontot a meglévő megosztások mutat.
4. Lehetővé teszik a szinkronizálás minden végponton egyeztetés folyamat befejezéséhez. 
5. Egyeztetés befejeződése után megnyithatja a megosztások változásait.
 
Felhívjuk a figyelmét arra, hogy jelenleg vonatkozik néhány korlátozás - előre beültetés megközelítés rendelkezik 
- Nem őrződik meg a fájlok teljes visszaadása. Például a fájlok ACL-EK és időbélyegek elvesznek.
- Adatok módosítása a előtt szinkronizálási topológia teljes körűen fel- és futó ütközések hatására a kiszolgáló végpontokon.  
- A felhőbeli végpont létrehozása után az Azure File Sync egy folyamat észleli a fájlokat a felhőben, a kezdeti szinkronizálás indítása előtt futtatja. Ez a folyamat befejezéséhez szükséges idő például a hálózati sebesség, a rendelkezésre álló sávszélességet és a fájlok és mappák száma különböző tényezőktől függően változik. Az előzetes kiadásban az Hozzávetőleges becslés az észlelési folyamat körülbelül 10 fájlokat/mp-ben található futtatja.  Ezért még akkor is, ha előre beültetés gyorsan lefut, beolvasni egy teljes mértékben működő rendszerben teljes időtartama sokkal hosszabb Ha adatokat nem lehet előzetesen a felhőben.

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>Áttelepítheti egy elosztott fájlrendszer replikációs szolgáltatása (DFS-R) környezetet az Azure File Sync
A DFS-R telepítési áttelepítése Azure File Sync:

1. Lecseréli a DFS-R-topológia ábrázolására szinkronizálási csoport létrehozása.
2. Indítsa el a kiszolgálón, amelyen az adatok teljes készlete a DFS-R topológia áttelepíteni. Telepítse az Azure File Sync az adott kiszolgálón.
3. Regisztrálja a kiszolgálót, és az első kiszolgáló kell áttelepíteni a kiszolgálói végpont létrehozásához. Ne engedélyezze a felhőbeli rétegezés.
4. Lehetővé teszik az összes az adatszinkronizálás az Azure-megosztás (felhőbeli végpont).
5. Telepíti és regisztrálja az Azure File Sync ügynök minden fennmaradó DFS-R-kiszolgálót.
6. Tiltsa le a DFS-R. 
7. A DFS-R Server-kiszolgálók mindegyike a kiszolgálói végpont létrehozásához. Ne engedélyezze a felhőbeli rétegezés.
8. Győződjön meg arról, szinkronizálása befejeződött, és tesztelje a topológia igény szerint.
9. A DFS-r kivonása
10. A felhő rétegezési május most már igény szerint bármely kiszolgálói végpont engedélyezhető.

További információkért lásd: [Azure File Sync együttműködési az elosztott fájlrendszer (DFS)](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>További lépések
- [És egy Azure File Sync kiszolgálói végpont törlése](storage-sync-files-server-endpoint.md)
- [Regisztráljon vagy az Azure File Sync-kiszolgáló regisztrációjának törlése](storage-sync-files-server-registration.md)

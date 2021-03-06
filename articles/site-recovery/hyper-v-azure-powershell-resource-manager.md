---
title: A PowerShell és az Azure Resource Manager a Hyper-V-Virtuálisgépek replikálása |} A Microsoft Docs
description: Automatizálhatja a replikálást a Hyper-V virtuális gépek az Azure-bA az Azure Site Recovery PowerShell és az Azure Resource Manager használatával.
services: site-recovery
author: bsiva
manager: abhiag
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 721bb725538b0b1f6eb0e7132b99e75491b6f969
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054473"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>Az Azure-bA vészhelyreállítás beállítása a Hyper-V virtuális gépekhez a PowerShell és Azure Resource Manager használatával

[Az Azure Site Recovery](site-recovery-overview.md) replikálásával segít a vállalatnak replikálása, feladatátvétele és helyreállítása Azure virtuális gépeken (VM), az üzletmenet-folytonossági és vészhelyreállítási (BCDR) stratégia megvalósításában, és a helyszíni virtuális gépek és fizikai kiszolgálók.

Ez a cikk ismerteti a Hyper-V virtuális gépek replikálása az Azure-ban a Windows PowerShell-lel, és az Azure Resource Manager használatával. A példában ez a cikk bemutatja, hogyan replikálható egyetlen virtuális Gépet a Hyper-V gazdagép, az Azure-bA.

## <a name="azure-powershell"></a>Azure PowerShell

Az Azure PowerShell kezelése az Azure-ban a Windows PowerShell-parancsmagokat kínál. Site Recovery PowerShell parancsmagokat, elérhető az Azure PowerShell-lel az Azure Resource Manager segítségével védhető és helyreállítható a kiszolgálók az Azure-ban.

Nem kell lennie egy PowerShell-lel szakértői ebből a cikkből, de az alapvető fogalmakkal, mint a modulok, a parancsmagok és a munkamenetek tisztában kell. Olvasási [első lépések a Windows PowerShell-lel](http://technet.microsoft.com/library/hh857337.aspx), és [az Azure PowerShell az Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> A Cloud Solution Provider (CSP) program Microsoft-partnerek konfigurálható és kezelhető a megfelelő CSP-előfizetésekben (bérlői előfizetések) ügyfél-kiszolgálók védelmének.
>
>

## <a name="before-you-start"></a>Előkészületek
Győződjön meg arról, hogy az előfeltételek teljesülnek:

* A [Microsoft Azure](https://azure.microsoft.com/) fiókot. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. Emellett itt olvashat [Azure Site Recovery Managert díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Ezen kiadásról és annak telepítéséről további információkért lásd: [Azure PowerShell 1.0-t.](https://azure.microsoft.com/)
* A [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) és [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modulok. Ezek a modulok a legújabb verziókat kap a [PowerShell-galéria](https://www.powershellgallery.com/)

Emellett az adott példa az ebben a cikkben leírt előfeltételei a következők:

* A Windows Server 2012 R2 vagy Microsoft Hyper-V Server 2012 R2 tartalmazó egy vagy több virtuális gépet futtató Hyper-V-gazdagép. A Hyper-V-kiszolgálók közvetlenül vagy proxyn keresztül csatlakozniuk az internethez.
* A replikálni kívánt virtuális gépek meg kell felelnie az [ezekről az előfeltételekről](hyper-v-azure-support-matrix.md#replicated-vms).

## <a name="step-1-sign-in-to-your-azure-account"></a>1. lépés: Jelentkezzen be az Azure-fiókkal

1. Nyisson meg egy PowerShell-konzolt, és jelentkezzen be az Azure-fiókkal, az alábbi paranccsal. A parancsmag kimenetei weblap kéri a hitelesítő adatait: **Connect-AzureRmAccount**.
    - Azt is megteheti, megadhatja a fiók hitelesítő adatait a paramétert a **Connect-AzureRmAccount** parancsmag használatával a **-hitelesítő adat** paraméter.
    - Ha Ön CSP-partner nevében egy bérlő dolgozik, adja meg az ügyfél egy bérlőt az elsődleges tartomány Bérlőazonosítója vagy a bérlő neve. Például: **Connect-AzureRmAccount-bérlő "fabrikam.com"**
2. Társítsa az előfizetést szeretné használni a fiókot, mert egy fiók több előfizetéssel is rendelkezik:

    `Select-AzureRmSubscription -SubscriptionName $SubscriptionName`

3. Győződjön meg arról, hogy az előfizetése regisztrálva van-e az Azure-szolgáltatók használata a Recovery Services és a Site Recovery az alábbi parancsokkal:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

4. A parancs kimeneténél ellenőrizze, hogy a **RegistrationState** értékre van állítva **regisztrált**, továbbléphet a 2. lépés. Ha nem kell regisztrálnia a hiányzó-szolgáltatót az előfizetésében, ezek a parancsok futtatásával:

    `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery` `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Győződjön meg arról, hogy a szolgáltatók regisztrálása sikeresen befejeződött, a következő parancsokkal:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-vault"></a>2. lépés: A tároló beállítása

1. Hozzon létre egy Azure Resource Manager-erőforráscsoportot, amelyben létrehozza a tárolót, vagy használjon egy meglévő erőforráscsoportot. Hozzon létre egy új erőforráscsoportot a következő. $ResourceGroupName változó tartalmazza a létrehozandó erőforráscsoport nevét, és a $Geo változó tartalmazza az Azure-régió, amelyben létrehozza az erőforráscsoportot (például "Dél-Brazília").

    `New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo` 

2. Az előfizetésében, futtassa az erőforráscsoportok listájának beszerzése a **Get-AzureRmResourceGroup** parancsmagot.
2. Következőképpen hozhat létre egy új Azure Recovery Services-tároló:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Kérheti le a meglévő tárolók listáját a **Get-AzureRmRecoveryServicesVault** parancsmagot.


## <a name="step-3-set-the-recovery-services-vault-context"></a>3. lépés: A Recovery Services vault környezet beállítása

A tárolási környezetet állítsa be a következőképpen:

`Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>4. lépés: Hozzon létre egy Hyper-V-hely

1. Következőképpen hozhat létre egy új Hyper-V-hely:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

2. Ez a parancsmag elindít egy Site Recovery-feladatot a webhely létrehozása, és a egy Site Recovery-feladatot objektumot ad vissza. Várjon, amíg a feladat befejeződik, majd ellenőrizze, hogy a feladat sikeresen befejeződött.
3. Használja a **Get-AzureRmSiteRecoveryJob parancsmag**lekérni a feladatobjektumot, majd a feladat aktuális állapotának ellenőrzéséhez.
4. Hozzon létre, és töltse le a regisztrációs kulcsot a helyhez, a következő:

    ```
    $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path
    ```

5. Másolja a letöltött kulcsot a Hyper-V-gazdagépen. Szüksége lesz a kulcs regisztrálja a Hyper-V gazdagépet a helyhez.

## <a name="step-5-install-the-provider-and-agent"></a>5. lépés: A Provider és agent telepítése

1. Töltse le a szolgáltató legújabb verzióját a telepítő [Microsoft](https://aka.ms/downloaddra).
2. Futtassa a telepítőt theHyper-V gazdagépen.
3. A telepítés végén továbbra is lépésben.
4. Amikor a rendszer kéri, adja meg a letöltött kulcsot, és a Hyper-V gazdagép a regisztráció befejezéséhez.
5. Győződjön meg arról, hogy a Hyper-V-gazdagép regisztrálva van a helyhez a következő:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy"></a>6. lépés: Replikációs házirend létrehozása

A Kezdés előtt vegye figyelembe, hogy a megadott tárfiók ugyanabban a régióban az Azure és a-tárolónak kell lennie, és rendelkeznie kell a georeplikáció engedélyezve van.

1. Replikációs szabályzat létrehozásához a következőképpen:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Tekintse meg a visszaadott feladat annak biztosítása érdekében, hogy a replikációs házirend létrehozása sikeres volt.

3. Kérje le a védelmi tároló, amely megfelel a helyhez, a következő:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. A védelmi tároló módon a replikációs házirendhez hozzárendelni:

     $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName   $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

4. Várjon, amíg a társítás feladat sikeresen befejeződik.

## <a name="step-7-enable-vm-protection"></a>7. lépés: A virtuális gép védelmének engedélyezése

1. Kérje le a védelmi entitás, amely megfelel a következő védeni kívánt virtuális géphez:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. A virtuális gép védelmét. Ha a védett virtuális gép csatlakoztatott egynél több lemezt tartalmaz, adja meg az operációsrendszer-lemez használatával a *OSDiskName* paraméter.

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

3. Várjon, amíg a virtuális gépek védett állapotba elérni a kezdeti replikációt követően. Ez eltarthat egy ideig, attól függően, például a replikálandó adatok mennyisége és a felsőbb rétegbeli rendelkezésre álló sávszélességet tényezők az Azure-bA. Ha egy védett állapotban van beállítva, a feladat állapotának és StateDescription frissítve lett a következő: 

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Frissítés recovery tulajdonságait (például a virtuális gépi szerepkör mérete,), és az Azure-hálózatot, amelyhez a feladatátvételt követően a virtuális gép hálózati Adaptert csatlakoztatni.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>8. lépés: Feladatátvételi teszt futtatása
1. Feladatátvételi teszt futtatása a következőképpen:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Győződjön meg arról, hogy a teszt virtuális gép létrehozása az Azure-ban. A teszt feladatátvételi feladatot az Azure-ban a teszt virtuális gép létrehozása után fel van függesztve.
3. Karbantartás és a feladatátvételi teszt elvégzése futtassa:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>További lépések
[További](https://docs.microsoft.com/powershell/module/azurerm.siterecovery) Azure Site Recoveryvel Azure Resource Manager PowerShell-parancsmagokkal kapcsolatos.

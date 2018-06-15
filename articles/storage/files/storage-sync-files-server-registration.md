---
title: Regisztrált kiszolgáló kezelése a Azure fájlszinkronizálás (előzetes verzió) |} Microsoft Docs
description: Megtudhatja, hogyan regisztrálásához és a Windows Server egy Azure szinkronizálás szinkronizálási tárhelyre.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: wgries
ms.openlocfilehash: 7385e8b84668facf8bf44f569a611e7dcdba9a1e
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738292"
---
# <a name="manage-registered-servers-with-azure-file-sync-preview"></a>Regisztrált kiszolgáló kezelése a Azure fájlszinkronizálás (előzetes verzió)
Az Azure File Sync (előzetes verzió) lehetővé teszi a szervezet Azure Files szolgáltatásban tárolt fájlmegosztásainak központosítását anélkül, hogy fel kellene adnia a helyi fájlkiszolgálók rugalmasságát, teljesítményét és kompatibilitását. Ennek érdekében a Windows-kiszolgálók átalakítása a Azure fájlmegosztás gyors gyorsítótárába. A Windows Server rendszeren elérhető bármely protokollt használhatja a fájlok helyi eléréséhez (pl. SMB, NFS vagy FTPS), és annyi gyorsítótára lehet világszerte, amennyire csak szüksége van.

A következő cikk bemutatja, hogyan regisztrálja, és a kiszolgáló egy tároló szinkronizálási szolgáltatás kezelése. Lásd: [Azure fájlszinkronizálás (előzetes verzió) telepítése](storage-sync-files-deployment-guide.md) fájlszinkronizálás Azure-végpontok telepítéséről információk.

## <a name="registerunregister-a-server-with-storage-sync-service"></a>Regisztrálása vagy a kiszolgáló regisztrációját az tároló szinkronizálási szolgáltatás
Windows Server és az Azure közötti megbízhatósági kapcsolat a kiszolgáló regisztrálása az Azure fájlszinkronizálás hoz létre. Ez a kapcsolat felhasználható létrehozásához *server végpontok* a kiszolgálón, amely képviseli, amelyek az Azure fájlmegosztások kell szinkronizálhatóak meghatározott mappákról (más néven egy *felhőbeli végpont*). 

### <a name="prerequisites"></a>Előfeltételek
Egy tároló szinkronizálási szolgáltatás regisztrálni a kiszolgálót, elő kell készítenie a kiszolgáló és az előfeltételei:

* A kiszolgálón futnia kell a Windows Server támogatott verziója. További információkért lásd: [Windows Server támogatott verziója](storage-sync-files-planning.md#supported-versions-of-windows-server).
* Győződjön meg arról, hogy a tároló szinkronizálási szolgáltatás van telepítve. Egy tároló szinkronizálási szolgáltatás telepítéséről további információkért lásd: [Azure fájlszinkronizálás (előzetes verzió) telepítése](storage-sync-files-deployment-guide.md).
* Győződjön meg arról, hogy a kiszolgáló csatlakozik-e az internethez, és hogy elérhető Azure.
* Tiltsa le az Internet Explorer fokozott biztonsági beállításai a rendszergazda a Kiszolgálókezelő felhasználói felületén.
    
    ![A Kiszolgálókezelő az Internet Explorer fokozott biztonsági beállításai a kijelölt felhasználói felület](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Győződjön meg arról, hogy a AzureRM PowerShell modul telepítve van-e a kiszolgálón. Ha a kiszolgáló egy feladatátvevő fürt tagja, a fürt minden csomópontja a AzureRM modul szükséges. További részletes információt a AzureRM modul telepítése található meg a [Azure PowerShell telepítése és konfigurálása](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > Azt javasoljuk, hogy egy kiszolgáló register/unregister legújabb verzióját, a AzureRM PowerShell-modul segítségével. Ha a AzureRM csomag korábban már telepítve van ezen a kiszolgálón (a PowerShell verziója ezen a kiszolgálón pedig 5.* vagy újabb), használhatja a `Update-Module` parancsmag frissíti a csomagot. 
* Ha hálózati proxykiszolgálót használják a környezetben, a kiszolgálón a sync-ügynök használatára a Proxybeállítások konfigurálása.
    1. A proxy IP-cím és port számának megállapításához
    2. Ezeket a fájlokat két szerkesztése:
        * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
        * C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
    3. Adja hozzá a sort az 1 (alatt ez a szakasz) a fenti két fájlban a megfelelő IP-címre (a név felülírandó 127.0.0.1) 127.0.0.1:8888 és a megfelelő port számát (a név felülírandó 8888) módosításával /System.ServiceModel konfigurációt:
    4. Állítsa be a WinHTTP-proxybeállítások parancssorból:
        * A proxy megjelenítése: netsh winhttp proxy megjelenítése
        * Állítsa be a proxy: netsh winhttp proxy 127.0.0.1:8888 beállítása
        * Alaphelyzetbe állítja a proxy: netsh winhttp proxy alaphelyzetbe állítása
        * Ha a telepítő az ügynök telepítése után, majd indítsa újra a sync-ügynök: net stop filesyncsvc
    
```XML
    Figure 1:
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
    </system.net>
```    

### <a name="register-a-server-with-storage-sync-service"></a>Regisztrálja a kiszolgálót tároló szinkronizálási szolgáltatás
Mielőtt a kiszolgáló is használható egy *végpontját* egy Azure fájlszinkronizálás a *szinkronizálású csoport*, szerepelnie kell a egy *tároló szinkronizálási szolgáltatás*. A kiszolgáló csak regisztrálható egy tárolási szinkronizálási szolgáltatás egyszerre.

#### <a name="install-the-azure-file-sync-agent"></a>Az Azure fájl Sync-ügynök telepítése
1. [Töltse le az Azure fájlszinkronizálás ügynököt](https://go.microsoft.com/fwlink/?linkid=858257).
2. Indítsa el az Azure fájlszinkronizálás ügynök telepítőjét.
    
    ![Az Azure fájlszinkronizálás ügynököt telepítő első oldalán](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Győződjön meg arról, lehetővé teszi az Azure fájlszinkronizálás ügynök a Microsoft Update használata frissítések. Ez azért fontos, mert a kritikus fontosságú biztonsági javítások és továbbfejlesztett szolgáltatásokat a kiszolgáló csomaghoz mellékeltük a Microsoft Update.

    ![Győződjön meg arról, a Microsoft Update ablaktábláján az Azure fájlszinkronizálás ügynök telepítőjét engedélyezve van a Microsoft Update](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Ha a kiszolgáló nem korábban regisztrálva, a kiszolgáló regisztrálása felhasználói felület jelenik meg azonnal a telepítés befejezése után.

> [!Important]  
> Ha a kiszolgáló egy feladatátvevő fürt tagja, az Azure fájl Sync-ügynök telepítve kell lennie a fürt minden csomópontján.

#### <a name="register-the-server-using-the-server-registration-ui"></a>Regisztrálja a kiszolgálót, a kiszolgáló regisztrálása felhasználói felület használatával
> [!Important]  
> Cloud Solution Provider (CSP) előfizetések nem használható a kiszolgáló regisztrálása felhasználói felületén. Ehelyett használjon PowerShell (alatt ez a szakasz).

1. Ha a kiszolgáló regisztrálása felhasználói felület nem indult el közvetlenül az Azure fájl Sync-ügynök telepítésének befejezése után, akkor is manuálisan kell elindítani a következő futtatásával `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Kattintson a *bejelentkezési* elérni az Azure-előfizetéshez. 

    ![Az a kiszolgáló regisztrálása felhasználói felület párbeszédpanel megnyitása](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Válassza ki a megfelelő előfizetés, erőforráscsoport és tároló szinkronizálási szolgáltatás a párbeszédpanelen.

    ![Szinkronizálási szolgáltatás tárolással](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. A képen egy további bejelentkezés szükséges a folyamat befejezéséhez. 

    ![Jelentkezzen be a párbeszédpanelt](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Ha a kiszolgáló egy feladatátvevő fürt tagja, minden olyan kiszolgálón kell futtatni a kiszolgáló regisztrálása. A regisztrált kiszolgálókat az Azure portálon megtekintésekor Azure fájlszinkronizálás automatikusan minden csomópont felismeri a ugyanazt a feladatátvevő fürt tagja, és a összevonja azokat megfelelően.

#### <a name="register-the-server-with-powershell"></a>A kiszolgáló regisztrálása a PowerShell használatával
Kiszolgáló regisztrálása a PowerShell használatával is elvégezheti. Ez az a kiszolgáló regisztrálása a Cloud Solution Provider (CSP) előfizetésekhez az egyetlen támogatott mód:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

### <a name="unregister-the-server-with-storage-sync-service"></a>Szüntesse meg a kiszolgáló, a tároló szinkronizálási szolgáltatás
Nincsenek számos lépést, a kiszolgáló regisztrációját az egy tároló szinkronizálási szolgáltatás szükséges. Vessen egy pillantást hogyan megfelelően az a kiszolgáló regisztrációját.

> [!Warning]  
> Ne kísérelje meg a regisztráció megszüntetését és a kiszolgáló regisztrálása vagy eltávolításával és újbóli létrehozása a kiszolgáló végpontok, kivéve, ha a Microsoft mérnöke explicit módon utasításainak szinkronizálási, rétegezéséhez felhő vagy bármely más szempontja, hogy Azure fájlszinkronizálás elhárítása. A kiszolgáló regisztrációjának törlése és eltávolítása a kiszolgáló végpontok egy felülíró műveletet, és rétegzett fájlok server-végpontokat tartalmazó kötetek nem "újra létrehozza" az Azure fájlmegosztás helyükre után a regisztrált kiszolgáló és a kiszolgáló-végpontok létre újból, és emiatt szinkronban hibák. Vegye figyelembe azt is, lehet, hogy a kiszolgáló-végpont névtér kívül megtalálható rétegzett fájlok végleg elvesznek. Rétegzett fájlok előfordulhat, hogy létezik-kiszolgálón belüli végpontok, még akkor is, ha a felhő rétegezéséhez sohasem volt engedélyezve.

#### <a name="optional-recall-all-tiered-data"></a>(Választható) Rétegzett adatainak visszaírásához
Ha szeretné, hogy a fájlokat, a rendszer jelenleg rétegzett (azaz azt a termelési, nem a tesztelési, környezet) Azure fájlszinkronizálás eltávolítása után elérhető legyen, visszahívása minden server-végpontokat tartalmazó köteten lévő összes fájlt. Tiltsa le az összes server-végpontokat tárolótömbökhöz felhő, és futtassa a következő PowerShell-parancsmagot:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <a-volume-with-server-endpoints-on-it>
```

> [!Warning]  
> Ha a helyi köteten, amelyen a kiszolgáló végpont nem rendelkezik elég szabad hely a rétegzett adatainak visszaírásához, a `Invoke-StorageSyncFileRecall` parancsmag sikertelen lesz.  

#### <a name="remove-the-server-from-all-sync-groups"></a>A kiszolgáló eltávolítása az összes szinkronizálási csoportok
A tároló szinkronizálási szolgáltatást a kiszolgáló regisztrációját, mielőtt összes server-végpontokat az adott kiszolgálón el kell távolítani. Ehhez az Azure-portálon:

1. Nyissa meg a tároló szinkronizálási szolgáltatást, ha a kiszolgáló regisztrálva van-e.
2. Távolítsa el a kiszolgáló összes server-végpontokat a tároló szinkronizálási szolgáltatás minden egyes szinkronizálás csoport. Ehhez kattintson a jobb gombbal a kívánt kiszolgálóra végpont a szinkronizálási ablakban.

    ![A kiszolgáló végpont szinkronizálási csoportból történő eltávolítása](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Ez egy egyszerű PowerShell-parancsfájllal is elvégezhető:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Connect-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

#### <a name="unregister-the-server"></a>Szüntesse meg a kiszolgáló
Most, hogy minden adat rendelkezik lett visszahívott, és a kiszolgáló összes szinkronizálási csoport el lett távolítva, a kiszolgáló regisztrációjának törlése is lehet. 

1. Az Azure-portálon lépjen a *regisztrálva kiszolgálók* a tároló szinkronizálási szolgáltatás szakasza.
2. Kattintson a jobb gombbal a kiszolgáló regisztrációját, és kattintson a "Kiszolgáló regisztrációjának törlése".

    ![Kiszolgáló regisztrációjának törlése](media/storage-sync-files-server-registration/unregister-server-1.png)

## <a name="ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter"></a>Biztosítani kell Azure fájlszinkronizálás egy jó szomszédos az adatközpontban található 
Mivel Azure fájlszinkronizálás ritkán az egyetlen szolgáltatás, az adatközpontban fut, érdemes lehet a hálózati és tárolási Azure fájlszinkronizálás-használatát korlátozása.

> [!Important]  
> Ha túl alacsonyra korlátok Azure fájlszinkronizálás szinkronizálás és a visszaírási hatással lesz.

### <a name="set-azure-file-sync-network-limits"></a>Az Azure fájlszinkronizálás hálózati vonatkozó korlátok beállítása
A hálózathasználat Azure fájlszinkronizálás segítségével szabályozhatja a `StorageSyncNetworkLimit` parancsmagok. 

Például létrehozhat egy új szabályozási korlát annak érdekében, hogy Azure fájlszinkronizálás nem használja több mint 10 MB/s közötti 9 óra és 5 (17:00 h) a munkahelyi hét során: 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
New-StorageSyncNetworkLimit -Day Monday, Tuesday, Wednesday, Thursday, Friday -StartHour 9 -EndHour 17 -LimitKbps 10000
```

A következő parancsmag használatával megtekintheti a korlátot:

```PowerShell
Get-StorageSyncNetworkLimit # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

Hálózati korlátok eltávolításához használja `Remove-StorageSyncNetworkLimit`. Például a következő parancs eltávolítja az összes hálózati korlátok:

```PowerShell
Get-StorageSyncNetworkLimit | ForEach-Object { Remove-StorageSyncNetworkLimit -Id $_.Id } # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

### <a name="use-windows-server-storage-qos"></a>Használja a Windows Server tárolási QoS 
Ha Azure fájlszinkronizálás egy Windows Server virtualizálási állomáson futó virtuális gépen, a tárolási szolgáltatásminőség (tárolási szolgáltatásminőség) segítségével storage IO-használat szabályozása. A tárolási QoS-házirendet a maximális (és a határt, például hogyan kényszeríti ki StorageSyncNetwork korlát feletti), vagy a minimális (vagy a foglalás) állítható be. Legfeljebb helyett legalább beállítás lehetővé teszi, hogy a fájl szinkronizálás Azure-kapacitásnövelés a rendelkezésre álló tár sávszélesség használatára, ha másik munkaterhelés sem használja. További információkért lásd: [tárolási szolgáltatásminőség](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview).

## <a name="see-also"></a>Lásd még
- [Egy Azure fájlszinkronizálás (előzetes verzió) telepítésének tervezése](storage-sync-files-planning.md)
- [Azure fájlszinkronizálás (előzetes verzió) telepítése](storage-sync-files-deployment-guide.md) 
- [Hibaelhárítás az Azure fájlszinkronizálás (előzetes verzió)](storage-sync-files-troubleshoot.md)

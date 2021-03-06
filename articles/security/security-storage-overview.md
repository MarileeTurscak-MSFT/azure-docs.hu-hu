---
title: Használható az Azure Storage biztonsági funkciói |} A Microsoft Docs
description: Ez a cikk az alapvető áttekintést nyújt az Azure biztonsági funkciókat, amelyeket az Azure Storage is használható.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2018
ms.author: terrylan
ms.openlocfilehash: 4ed4e73348db8cfffb6e79afaa9d196e242d7488
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054520"
---
# <a name="azure-storage-security-overview"></a>Az Azure Storage biztonsági áttekintése

Az Azure Storage az ügyfelek igényeihez alkalmazkodó, modern, a tartósságra, rendelkezésre állásra és méretezhetőségre tervezett alkalmazásokhoz legmegfelelőbb felhőalapú tárolómegoldás. Az Azure Storage egy átfogó biztonsági képességeket biztosít. A következőket teheti:

* A tárfiók biztonságos szerepköralapú hozzáférés-vezérlés (RBAC) és az Azure Active Directory használatával.
* Gondoskodhat az alkalmazás és az Azure között átvitt adatok ügyféloldali titkosítás, a HTTPS vagy az SMB 3.0-s.
* Állítsa be az adatokat automatikusan titkosítja a Storage Service Encryption segítségével az Azure Storage-írás.
* Készlet operációs rendszer és az adatlemezek virtuális gépek (VM) által használt titkosítását az Azure Disk Encryption használatával.
* Az Azure Storage az adatobjektumok delegált hozzáférési jogot a közös hozzáférésű jogosultságkódok (saas-alapú) használatával.
* Analytics segítségével nyomon követheti a tároló elérésekor valaki által használt hitelesítési módszert.

Az Azure Storage biztonsági részletesebb tekintse meg, lásd: a [Azure Storage biztonsági útmutatóját](../storage/common/storage-security-guide.md). Ez az útmutató egy Azure Storage biztonsági funkcióinak részletes ismertetése. Ilyen például a tárfiók kulcsaihoz, adattitkosítás átvitel, mind REST-en, és a storage analytics.

Ez a cikk az Azure Storage használható az Azure security funkcióit. Cikkek hivatkozásainak egyes szolgáltatások részletei adjon meg, hogy többet is megtudhat.

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés

Segíthet a szerepköralapú hozzáférés-vezérlés használatával tegye biztonságossá tárfiókját. A hozzáférés korlátozása alapján a [ismernie kell](https://en.wikipedia.org/wiki/Need_to_know) és [legalacsonyabb jogosultsági](https://en.wikipedia.org/wiki/Principle_of_least_privilege) biztonsági alapelveket elengedhetetlen, szervezetek által adatok elérésére vonatkozó biztonsági szabályzatok kikényszerítéséhez. A hozzáférési jogosultságokat szerint a megfelelő RBAC-szerepkörök hozzárendelése a csoportokhoz és alkalmazásokhoz egy bizonyos hatókörben. Használhat [beépített RBAC-szerepkör](../role-based-access-control/built-in-roles.md), például a Tárfiók-közreműködő, jogosultságok hozzárendelése a felhasználókhoz.

További információ:

* [Az Azure Active Directory szerepköralapú hozzáférés-vezérlés](../role-based-access-control/role-assignments-portal.md)

## <a name="delegated-access-to-storage-objects"></a>Tárolási objektumok való delegált hozzáférés

Közös hozzáférésű jogosultságkód a tárfiókban található erőforrások delegált hozzáférést biztosít. Az SAS, az azt jelenti, hogy Ön is az ügyfeleknek a tárfiókban lévő objektumokra vonatkozó adott ideig, és az engedélyek bizonyos készletét. Ezekkel a korlátozott engedélyekkel biztosíthat a hozzáférési kulcsainak megosztása nélkül. 

A SAS URI, amely a lekérdezési paraméterek magában foglalja a tárolási erőforrásokhoz való hitelesített hozzáférés szükséges összes információt az. A SAS-adattároló erőforrások elérésére, az ügyfél csak kell adja meg a megfelelő konstruktor vagy a metódus az SAS.

További információ:

* [A SAS-modell ismertetése](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Létrehozása és SAS használata a Blob storage használatával](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Titkosítás az átvitel során

Titkosítás az átvitel során egy mechanizmust, az adatok védelme, amikor azok elküldése hálózatokon keresztül. Az Azure Storage biztosíthatja az adatok:

* [Átviteli szintű titkosítást](../storage/common/storage-security-guide.md#encryption-in-transit), például a HTTPS, vagy onnan máshová az Azure Storage-adatok átvitel során.
* [Vezetékes titkosítás](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), például az SMB 3.0 titkosítás, az Azure-fájlmegosztások esetén.
* [Ügyféloldali titkosítás](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), a Storage-bA továbbított előtt titkosíthatja az adatokat, és az adatok visszafejtéséhez követően elfogyott a tárterület.

További információ az ügyféloldali titkosítás:

* [A Microsoft Azure Storage ügyféloldali titkosítása](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [A cloud security sorozat szabályozza: az átvitt adatok titkosítása](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Titkosítás inaktív állapotban

A legtöbb szervezet számára [adattitkosítás inaktív](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) kötelező lépés az adatok adatvédelmi, megfelelőségi és az adatok elkülönítése felé. Három Azure-funkciókról, aktívan nem használt adatok titkosításának adja meg:

* [A Storage Service Encryption](../storage/common/storage-security-guide.md#encryption-at-rest) lehetővé teszi, hogy a storage szolgáltatás automatikusan adatok titkosítása írásakor, az Azure Storage-kérelem.
* [Ügyféloldali titkosítás](../storage/common/storage-security-guide.md#client-side-encryption) az inaktív adatok titkosítását a szolgáltatást is nyújt.
* [Az Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) lehetővé teszi az operációsrendszer-lemezek és a egy IaaS virtuális gépen használó adatlemezek titkosítását.

További információk a Storage Service Encryption szolgáltatással:

* [Az Azure Storage Service Encryption](https://azure.microsoft.com/services/storage/) érhető el [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/). További információ a más az Azure storage-típusok: [Azure Files](https://azure.microsoft.com/services/storage/files/), [lemez (prémium szintű Storage)](https://azure.microsoft.com/services/storage/premium-storage/), [Table storage](https://azure.microsoft.com/services/storage/tables/), és [Queue storage](https://azure.microsoft.com/services/storage/queues/).
* [Az Azure Storage Service Encryption for Data at Rest](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption

A virtuális gépek az Azure Disk Encryption lehetővé teszi, szervezeti biztonsági és megfelelőségi követelmények is. A Virtuálisgép-lemezek (beleértve a rendszerindító és az adatlemezek), az titkosítja a kulcsok és szabályzatok, hogy az [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

Lemeztitkosítás virtuális gépekhez a Linux és Windows operációs rendszerek esetében működik. Emellett a Key Vault segítségével biztonságos, kezelése és naplózása a lemeztitkosítási kulcsok használatát. Virtuálisgép-lemezen lévő összes adatot az Azure storage-fiókokhoz az iparági szabványnak megfelelő titkosítási technológia használatával titkosítása. A Disk Encryption megoldás a Windows alapján [Microsoft BitLocker Meghajtótitkosításon](https://technet.microsoft.com/library/cc732774.aspx), és a Linux-megoldás alapján [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Részletek

* [Az Azure Disk Encryption Windows és Linux rendszerű IaaS virtuális gépek](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-storage-firewalls-and-virtual-networks"></a>Az Azure Storage-tűzfalak és virtuális hálózatok

Az Azure storage lehetővé teszi, hogy a tárfiókok tűzfalszabályok engedélyezése. Egyszer engedélyezett, akkor blokkolja a beérkező adatokat, beleértve a más Azure-szolgáltatásokhoz érkező kérelmeket. Konfigurálhatja a kivételeket úgy, hogy engedélyezze a forgalmat. Előfordulhat, hogy engedélyezni kell a tűzfalszabályokat, a meglévő tárfiókok vagy létrehozás közben.

Ez a funkció engedélyezett hálózatok egy meghatározott készletének a storage-fiókok védelméhez használjon.

További információ az Azure storage-tűzfalak és virtuális hálózatok tekintse át a [konfigurálása az Azure Storage-tűzfalak és virtuális hálózatok](../storage/common/storage-network-security.md)

## <a name="azure-key-vault"></a>Azure Key Vault

Használja az Azure Disk Encryption [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) segítségével vezérelheti és felügyelheti a lemeztitkosítási kulcsokat és titkos kulcsokat a key vault-előfizetés. Emellett biztosítja, hogy a virtuálisgép-lemezeken lévő összes adatot is titkosítása az Azure Storage-ban. A Key Vault naplózását a kulcsokat és a házirend-használati használjon.

Részletek

* [Mi az Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md)

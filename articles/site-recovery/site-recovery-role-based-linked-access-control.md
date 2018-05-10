---
title: Szerepköralapú hozzáférés-vezérlés használatával kezelheti az Azure Site Recovery |} Microsoft Docs
description: Ez a cikk ismerteti, hogyan alkalmazza, és szerepköralapú hozzáférés-vezérlést (RBAC) használata az Azure Site Recovery központi telepítések felügyeletéhez szükséges
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 4618d449410656f742cff40cf3cc190a0ce9c5e4
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/10/2018
---
# <a name="use-role-based-access-control-to-manage-azure-site-recovery-deployments"></a>Azure Site Recovery központi telepítések felügyeletéhez szükséges szerepköralapú hozzáférés-vezérlés használatával

Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletes hozzáférés-vezérlést biztosít az Azure-hoz. Az RBAC használata, feladatkörök elkülönítse a munkacsoporton belül, és csak különleges hozzáférési engedélyeket a felhasználóknak adott feladatok elvégzéséhez szükség szerint.

Az Azure Site Recovery 3 beépített szerepkörök, amelyekkel szabályozhatja a Site Recovery felügyeleti műveleteket biztosít. További információ az [Azure beépített RBAC-szerepköreivel](../role-based-access-control/built-in-roles.md) kapcsolatban.

* [Site Recovery-közreműködő](../role-based-access-control/built-in-roles.md#site-recovery-contributor) – Ez a szerepkör minden olyan engedéllyel rendelkezik, amelyek az Azure Site Recovery-műveletek Recovery Services-tárolókban történő kezeléséhez szükségesek. Az ezzel a szerepkörrel rendelkező felhasználók nem hozhatnak létre, illetve nem törölhetnek Recovery Services-tárolókat, és nem adhatnak hozzáférési jogokat más felhasználóknak. A szerepkör legmegfelelőbb vész helyreállítási rendszergazdák, akik engedélyezése és kezelése a esettől vész-helyreállítási az alkalmazások vagy a teljes szervezet számára.
* [Helyreállítási operátor hely](../role-based-access-control/built-in-roles.md#site-recovery-operator) -ezt a szerepkört jogosult végrehajtása és feladatátvétel és a feladat-visszavétel műveleteinek a felügyeletét. A felhasználói szerephez nem tudja engedélyezni vagy tiltsa le a replikációt, létrehozása vagy törlése a tárolóból, új infrastruktúra regisztrálása vagy más felhasználók hozzáférési jogosultságokat rendelhet hozzá. Ezt a szerepkört egy feladatátvételi virtuális gépek is vész helyreállítási kezelők a legalkalmasabb vagy alkalmazások, ha arra utasította az alkalmazástulajdonosok és a rendszergazdák egy tényleges vagy szimulált katasztrófa helyzetben, például egy vész-Helyreállítási részletezést. POST a katasztrófa feloldását, a vész-Helyreállítási operátor ismételt védelemmel láthatná és a feladat-visszavételt a virtuális gépek.
* [Site Recovery-olvasó](../role-based-access-control/built-in-roles.md#site-recovery-reader) – Ez a szerepkör a Site Recovery összes felügyeleti műveletének a megtekintésére rendelkezik engedélyekkel. Ezt a szerepkört olyan egy informatikai felügyeleti vezető, akik védelmi aktuális állapotának figyelése és támogatási jegyek ablakába, ha szükséges.

Ha még nagyobb mértékben vezérelheti a saját szerepköröket definiál, lásd: hogyan [egyéni szerepkörök létrehozása](../role-based-access-control/custom-roles.md) az Azure-ban.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Új virtuális gépek replikáció engedélyezése szükséges engedélyek
Ha Azure-bA az Azure Site Recovery segítségével új virtuális gép replikálódik, a kapcsolódó felhasználó a hozzáférési szintek érvényesítése annak érdekében, hogy a felhasználó rendelkezik-e a szükséges engedélyekkel a Site Recovery számára a megadott Azure-erőforrások használatára.

Ahhoz, hogy egy új virtuális gép replikálását, a felhasználóknak rendelkezniük kell:
* A virtuális gép létrehozása a kijelölt erőforráscsoportban engedély
* Engedéllyel a kiválasztott virtuális hálózatban lévő virtuális gép létrehozása
* A kiválasztott tárolási fiók írásához engedély

A felhasználó nem tudja befejezni a replikációt az új virtuális gépek a következő engedélyeket.

> [!IMPORTANT]
>Győződjön meg arról, hogy a megfelelő engedélyeket kerülnek-e a telepítési modell / (erőforrás-kezelő / klasszikus) erőforrás-telepítéshez használt.

| **Erőforrás típusa** | **Telepítési modell** | **Engedély** |
| --- | --- | --- |
| Compute | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Klasszikus | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Network (Hálózat) | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Klasszikus | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Storage | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Klasszikus | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Erőforráscsoport | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

Fontolja meg a "Virtuális gép közreműködő" és "Klasszikus virtuális gép közreműködő" [beépített szerepkörök](../role-based-access-control/built-in-roles.md) a Resource Manager és klasszikus telepítési modellek kulcsattribútumokkal.

## <a name="next-steps"></a>További lépések
* [Szerepköralapú hozzáférés-vezérlés](../role-based-access-control/role-assignments-portal.md): az RBAC első lépései az Azure portálon.
* Útmutató: a hozzáférés kezelése:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Szerepköralapú hozzáférés-vezérlés hibaelhárítási](../role-based-access-control/troubleshooting.md): kapcsolatos gyakori hibák elhárítására vonatkozó javaslatok beolvasása.

---
title: Az Azure Security Center biztonsági házirendjének beállításai |} A Microsoft Docs
description: Az Azure Security Center biztonsági házirend-beállítások konfigurálása.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: ab8a289ea0de263871b76788514052c09a6bf4da
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/10/2018
ms.locfileid: "44295737"
---
# <a name="security-policy-settings"></a>Biztonsági szabályzat beállításai
Ez a cikk áttekintést biztonsági házirendjének beállításai a Security Center.

## <a name="what-are-security-policies"></a>Mik azok a biztonsági szabályzatok?
A biztonsági szabályzat határozza meg a számítási feladatokhoz tartozó kívánt konfigurációkat, és segít biztosítani a vállalati vagy hatósági követelményeknek való megfelelést. Az Azure Security Centerben az Azure-előfizetésekre vonatkozó szabályzatokat határozhat meg, és testre szabni azokat a számítási feladatok típusának vagy az adatok érzékenysége. A szabályozott adatokat, például személyazonosításra alkalmas adatokat használó alkalmazások például szükség lehet egy magasabb biztonsági szintet, mint a többi munkaterhelését.

A következő biztonsági házirend szerint állíthatja be:

- **Az adatgyűjtés**: meghatározza az ügynökkiépítés és [adatgyűjtés](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) beállításait.
- **Biztonsági házirend**: azt határozza meg, amely szabályozza a Security Center figyeli, és javasolja. Szerkesztheti a [biztonsági házirend](security-center-policies.md) a Security Centerben. Is [Azure Policy](security-center-azure-policy.md) létrehozhat új meghatározásokat, meghatározhat további szabályzatokat, és hozzárendelhet szabályzatokat a felügyeleti csoportok. 
- **E-mail-értesítések**: meghatározza, hogy a biztonsági felelősök kapcsolati adatait, és [e-mailes értesítés](security-center-provide-security-contact-details.md) beállításait.
- **Tarifacsomag**: határozza meg az ingyenes vagy standard [díjszabás kiválasztása](security-center-pricing.md). A kiválasztott csomag határozza meg, hogy a Security Center mely szolgáltatásai érhetők el a hatókörbe eső erőforrásokhoz. Megadhat egy szintet előfizetésekhez, erőforráscsoportokhoz és munkaterületekhez.

> [!NOTE]
> Mindegyik említett előfizetésenként állíthatja be. A munkaterületek beállíthatja a csak az adatok gyűjtése és a tarifacsomag. Erőforráscsoportok csak tarifacsomag állíthatók be.
>


## <a name="who-can-edit-security-policies"></a>Ki szerkeszthet biztonsági szabályzatok?
A Security Center szerepköralapú hozzáférés-vezérlés (RBAC), amely biztosít beépített szerepkörök, felhasználók, csoportok és Azure-szolgáltatások rendelhető használ. A Security Center megnyitásakor látnak csak ők is hozzáférhetnek az erőforrásokhoz kapcsolódó információkat. Ami azt jelenti, hogy a felhasználók vannak hozzárendelve a szerepköre *tulajdonosa*, *közreműködői*, vagy *olvasó* a erőforrás tartozik előfizetés vagy az erőforrás-csoporthoz. Ezen szerepkörök mellett két speciális Security Center-szerepkör van:

- **Biztonsági olvasó**: rendelkezik megtekintése a Security Centernek, amely tartalmazza a javaslatok, riasztások, a házirend és egészségügyi, rights, de azok nem végezhet módosításokat.
- **Biztonsági rendszergazda**: megegyező nézet jogokkal rendelkezik *biztonsági olvasó*, és azok is a biztonsági házirend módosítása és javaslatok és riasztások bezárása.


## <a name="next-steps"></a>További lépések
Ebben a cikkben megismerkedett az Azure Security Centerben a biztonsági szabályzatokat. Azure Security Centerrel kapcsolatos további tudnivalókért tekintse meg a következő cikkeket:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md): ismerje meg, hogyan konfigurálhat biztonsági házirendeket az Azure-előfizetések és -erőforráscsoportok.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md): megtudhatja, hogyan Security Center javaslatait az Azure-erőforrások védelme.
* [Biztonsági állapotmonitorozás az Azure Security Centerben](security-center-monitoring.md): Útmutató az Azure-erőforrások állapotának monitorozásához.
* [Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md): A biztonsági riasztások kezelése és az azokra való reagálás.
* [Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md): Útmutató a partneri megoldások biztonsági állapotának monitorozásához.
- [Az Azure Security Center által nyújtott adatbiztonság](security-center-data-security.md): Útmutató a Security Center felügyeli, és gondoskodik az adatok.
* [Azure Security Center – gyakori kérdések](security-center-faq.md): Választ találhat a szolgáltatás használatával kapcsolatos gyakori kérdésekre.
* [Az Azure Security blog](http://blogs.msdn.com/b/azuresecurity/): az Azure security legújabb híreket és információkat.

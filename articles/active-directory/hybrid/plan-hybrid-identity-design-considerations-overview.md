---
title: Az Azure Active Directory hibrid identitás kialakításával kapcsolatos szempontok – áttekintés |} A Microsoft Docs
description: Áttekintése és hibrid identitás kialakítási szempontokat ismertető útmutató tartalom térképe
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6d916e884e7764a1ba5ebaf224750f38e7d7673e
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46313893"
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory hibrid identitáskezelés – kialakítási szempontok
Fogyasztói-alapú eszközök vannak proliferating a vállalati világban, és felhő alapú szoftver--szolgáltatásként (SaaS) alkalmazások könnyen fogad el. Ennek eredményeképpen belső adatközpontokra és felhőalapú platformon alkalmazás-hozzáférés felhasználók feletti irányítás is kihívást jelent.  

A Microsoft identitáskezelési megoldásai kiterjednek a helyszíni és felhőalapú képességekre is, így egyetlen felhasználói identitás jön létre hitelesítés és engedélyezés céljából az összes erőforráshoz, helytől függetlenül. A fogalom hibrid identitás néven ismert. Különböző kialakítási és konfigurációs beállításokat a Microsoft-megoldások hibrid identitás, és néhány esetben nehéz lenne meghatározni, melyik kombináció a legjobb lehet a szervezet igényeinek. 

A hibrid Identitáskezelés – kialakítási szempontokat útmutató segítségével megismerheti, hogyan tervezhet hibrid identitáskezelési megoldás, amely leginkább megfelel az üzleti és technológiai a szervezet igényeinek.  Ez az útmutató részletesen ismerteti azokat a lépéseket és feladatokat, amelyek segítséget nyújtanak a szervezet egyedi igényeinek megfelelő hibrid identitáskezelési megoldás tervezése. A lépések és feladatok során az útmutató a releváns technológiák bemutatásához lesz, és lehetőségeket, amelyek funkcionális és szolgáltatásminőségi (például a rendelkezésre állási, skálázhatósági, teljesítmény, kezelhetőségi és biztonsági) funkció szint követelmények. 

Pontosabban a hibrid identitás tervezési szempontok az útmutató célok a következő kérdések megválaszolásával: 

* Milyen kérdéseket kell kérdéseit vagy válaszait egy hibrid identitás-specifikus egy technológiai vagy problématerületnek tartomány leginkább megfelelő kialakítás rendelkezésemre a követelmények teljesítéséhez alapjául?
* Mely tevékenységek sorozatát kell el tervezhet a technológiai vagy problématerületnek tartomány hibrid identitáskezelési megoldás? 
* Milyen hibrid identitás technológiai és konfigurációs lehetőségek segítik a rendelkezésemre a követelmények teljesítéséhez érhetők el? Mik azok a-hátrányokkal járnak ezek a lehetőségek, hogy a legjobb választás az üzleti is választott?

## <a name="who-is-this-guide-intended-for"></a>Kinek szól a Ez az útmutató?
 CIO, CITO, vezető identitás Architects, nagyvállalati Rendszertervezőknek és IT-tervezők tervezéséért felelősek hibrid identitáskezelési megoldás közepes vagy nagy szervezetek számára.

## <a name="how-can-this-guide-help-you"></a>Hogyan Ez az útmutató segít Önnek?
Ez az útmutató segítségével megtudhatja, hogyan tervezhet, amely egy felhőalapú identitáskezelési rendszerének integrálhatják a jelenlegi helyszíni identitáskezelési megoldás hibrid identitáskezelési megoldás. 

A következő ábrán látható egy példa egy hibrid identitáskezelési megoldás, amely lehetővé teszi az informatikai rendszergazdák kezelhetik a jelenlegi Windows Server Active Directory megoldással a helyszíni integrálása a Microsoft Azure Active Directory-felhasználók számára (egyszeri bejelentkezés engedélyezése Egyszeri bejelentkezés) található a felhőben és helyszíni alkalmazásokban.

![Példa](media/plan-hybrid-identity-design-considerations/hybridID-example.png)

A fenti ábrán egy példa egy hibrid identitáskezelési megoldás, amely a helyszíni szolgáltatásokkal való integráció révén, annak érdekében, hogy a végfelhasználói hitelesítés a folyamathoz egyetlen felületet biztosít, és elősegítsék a felhőalapú szolgáltatások kihasználása az informatikai azok kezelése az erőforrásokat. Bár ez a példa lehet egy gyakori forgatókönyv, minden cég hibrid identitás tervezési valószínű, hogy az eltérő követelmények miatt az 1. ábrán bemutatott példa eltérőnek lennie. 

Ez az útmutató azokat a lépéseket és feladatokat, amelyekkel a szervezet egyedi igényeinek megfelelő hibrid identitáskezelési megoldás tervezésekor több ismertet. A következő lépések és feladatok során az útmutató mutatja be a releváns technológiákat és beállítási lehetőségeket, a funkcionális és a szolgáltatás szolgáltatásminőségi szintre vonatkozó követelményeinek a szervezet.

**Feltételezések**: rendelkezik némi tapasztalattal a Windows Server, az Active Directory Domain Services és az Azure Active Directory. Ebben a dokumentumban feltételezzük, hogyan ezeket a megoldásokat is az üzleti igényeinek önállóan vagy az integrált megoldást keres.

## <a name="design-considerations-overview"></a>Kialakítási szempontok áttekintése
Ez a dokumentum lépéseket és feladatokat, amelyekkel az igényeinek legjobban megfelelő hibrid identitáskezelési megoldás tervezéséhez nyújt. A lépések bemutatják sorrendben követik egymást. Szempontok a későbbi lépésekben hozott döntések felülvizsgálatát a korábbi lépések azonban tervezési döntések is lehet szükség. Minden kísérlet történik a dokumentum lehetséges tervezési ütközésekre riasztást küld. 

A tervezési, hogy igényeknek leginkább megfelelő csak ha annyiszor megy végig a lépéseken, ahányszor szükséges a dokumentumban ismertetett szempontok érkezik. 

| Hibrid identitás fázis | A témakör listája |
| --- | --- |
| Identitással kapcsolatos követelmények meghatározása |[Üzleti igények meghatározása](plan-hybrid-identity-design-considerations-business-needs.md)<br> [Határozza meg a címtár-szinkronizálás követelményei](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [A multi-factor authentication-követelmények meghatározása](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [A hibrid identitás bevezetési stratégia kidolgozása](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Adatbiztonság erős identitása-megoldáson keresztül továbbfejlesztésének tervezése |[Az adatvédelmi követelmények meghatározása](plan-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [A Tartalomkezelés követelmények meghatározása](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Hozzáférés-vezérlési követelményeinek meghatározása](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Incidensválasz-követelmények meghatározása](plan-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Data protection stratégia kidolgozása](plan-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Hibrid identitás-életciklus megtervezése |[Hibrid identitáskezelési feladatokat meghatározása](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Szinkronizálás kezelése](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hibrid identitás-kezelés bevezetési stratégiájának meghatározása](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="download-this-guide"></a>Ez az útmutató letöltése
A hibrid identitás tervezési szempontjait ismertető útmutató a pdf-verzióját letöltheti a [Technet gallery](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 


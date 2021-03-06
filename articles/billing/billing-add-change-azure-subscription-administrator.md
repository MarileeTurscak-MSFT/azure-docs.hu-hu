---
title: Azure-rendszergazdai előfizetés szerepkörök hozzáadása vagy módosítása |} A Microsoft Docs
description: Ismerteti, hogyan lehet felvenni vagy módosítani Azure társ-rendszergazda, szolgáltatás-rendszergazda és a fiók rendszergazdája
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: cwatson
ms.openlocfilehash: 821d263856f21897915ba7954487b4d029cc4ed0
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395245"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Adja hozzá, vagy az Azure-előfizetések rendszergazdáinak módosításáról

Az Azure-erőforrások hozzáférésének kezeléséhez megfelelő rendszergazdai szerepkörrel kell rendelkeznie. Ez a cikk bemutatja, hogyan hozzáadásához vagy módosításához a rendszergazda szerepkör egy felhasználó az előfizetés szintjén.

> [!div class="nextstepaction"]
> [Segítsen az Azure számlázási dokumentumok fejlesztésében](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="what-administrator-role-do-i-use"></a>Milyen rendszergazdai szerepkör használni?

Az Azure számos különböző szerepkörrel rendelkezik. Erőforrásokhoz való hozzáférés kezelésére használható a hagyományos előfizetés rendszergazdai szerepköröket, például a szolgáltatás-rendszergazda és a társ-rendszergazdaként vagy egy újabb engedélyezési rendszer nevezik szerepköralapú hozzáférés-vezérlés (RBAC). Jobb ellenőrzésének biztosítása érdekében, és a hozzáférés-kezelés egyszerűsítése érdekében javasoljuk, hogy minden hozzáférés-felügyeleti igényeinek megfelelő RBAC használatát. Ha lehetséges javasoljuk, hogy újrakonfigurálta a meglévő hozzáférési szabályzatokat az RBAC használatával. További információkért lásd: [Mi a szerepköralapú hozzáférés-vezérlés (RBAC)](../role-based-access-control/overview.md) és [megismerheti az Azure-ban a különféle szerepkörök](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Az RBAC tulajdonosi egy előfizetés hozzáadása az Azure Portalon 

Ha valakit egy Azure-előfizetés rendszergazdájaként szeretne hozzáadni, rendelje hozzá a [Tulajdonos](../role-based-access-control/built-in-roles.md#owner) szerepkört (RBAC-szerepkör) az előfizetés hatókörében. A tulajdonosi szerepkör kezelni tudja a hozzárendelt előfizetésben lévő erőforrásokat, és nem rendelkezik hozzáférési jogosultságokkal más előfizetésekhez.

1. Látogasson el [ **előfizetések** az Azure Portalon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Válassza ki azt az előfizetést, amelynek hozzáférést szeretne adni.
3. Válassza a **Hozzáadás** lehetőséget.
   (Ha hiányzik a Hozzáadás gomb, nincs engedélye engedélyek hozzáadására.)
4. Válassza a listában a **Hozzáférés-vezérlés (IAM)** elemet.
5. A **Szerepkör** mezőben válassza a **Tulajdonos** elemet. 
6. A **Hozzáférés hozzárendelése** mezőben válassza az **Azure AD, felhasználó, csoport vagy alkalmazás** elemet. 
7. A **Kiválasztás** mezőbe írja be annak a felhasználónak az e-mail-címét, amelyet tulajdonosként szeretne hozzáadni. Válassza ki a felhasználót, majd válassza a **Mentés** lehetőséget.

    ![A tulajdonosi szerepkörrel, kijelölt bemutató képernyőkép](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Ez hozzáférést biztosít a felhasználó teljes az összes olyan erőforrásokhoz, beleértve a jogot arra, hogy mások való hozzáférés delegálására. Látogasson el a hozzáférést egy másik hatókörben, egy erőforráscsoportot, mint például a **hozzáférés-vezérlés (IAM)** panel az adott hatókörnél.

## <a name="add-or-change-co-administrator"></a>Adja hozzá, vagy módosítsa a társ-rendszergazdaként

Csak [Tulajdonos](../role-based-access-control/built-in-roles.md#owner) adható hozzá társadminisztrátorként. A más, például [közreműködői](../role-based-access-control/built-in-roles.md#contributor) és [olvasói](../role-based-access-control/built-in-roles.md#reader) szerepkörrel rendelkező felhasználók nem adhatók hozzá társadminisztrátorokként.

> [!TIP]
> Csak ki kell a tulajdonos hozzáadása társadminisztrátorként, ha a felhasználónak van szüksége az Azure klasszikus üzembe helyezés kezelése. Azt javasoljuk, hogy az RBAC használatával más célokra.

1. Ha még nem tette, személy tulajdonosként hozzáadni a fenti utasítások.
2. **Kattintson a jobb gombbal** a tulajdonos felhasználó, a most hozzáadott, és válassza **Hozzáadás társadminisztrátorként**. Ha nem látja a **Hozzáadás társadminisztrátorként** lehetőségét, frissítse az oldalt, vagy próbálkozzon egy másik webböngészőben. 

    ![Képernyőkép, amely hozzáadja a társ-rendszergazdaként](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    A társ-rendszergazdaként engedély eltávolítása **kattintson a jobb gombbal** a társ-rendszergazda felhasználót, majd **társadminisztrátor eltávolítása**.

    ![Képernyőkép, amely eltávolítja a társ-rendszergazdaként](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Az Azure-előfizetés szolgáltatás-rendszergazda módosítása

Csak a fiók rendszergazdája módosíthatja az előfizetés szolgáltatás-rendszergazda. Alapértelmezés szerint amikor regisztrál, a szolgáltatás-rendszergazda megegyezik a fiók rendszergazdája. Ha a szolgáltatás-rendszergazda egy másik felhasználó megváltozik, majd a fiók rendszergazdája elveszítette a hozzáférését az Azure Portalra. Azonban a fiók rendszergazdája mindig segítségével Fiókközpontban módosíthatja a szolgáltatás-rendszergazda vissza a saját magukat.

1. Ellenőrizze, hogy a forgatókönyvet úgy a [korlátok a szolgáltatás-rendszergazdák változó](#limits).
1. Jelentkezzen be a [Fiókközpontban](https://account.windowsazure.com/subscriptions) rendszergazdaként.
1. Válasszon egy előfizetést.
1. A jobb oldalon válassza ki a **előfizetés részleteinek szerkesztése**.

    ![Képernyőfelvétel: a Szerkesztés előfizetés gomb Account Center webhelyen](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. Az a **szolgáltatás-rendszergazda** mezőbe írja be az új szolgáltatás-rendszergazda e-mail-címét.

    ![Képernyőfelvétel: a box, a szolgáltatás-rendszergazda e-mail-cím módosítása](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Szolgáltatás-rendszergazdák módosítása vonatkozó korlátozások

* Minden egyes előfizetés társítva az Azure AD-címtárral. Az előfizetés társítva a könyvtárban találja, [ **előfizetések**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), majd válassza ki az előfizetés megtekintéséhez a címtárban.
* Ha be van jelentkezve a munkahelyi vagy iskolai fiókkal jelentkezik be, más fiókokat hozzáadhatja szolgáltatás-rendszergazdaként a cégnél. Ha például abby@contoso.com adhat hozzá bob@contoso.com , szolgáltatás-rendszergazda, de nem adható hozzá john@notcontoso.com , kivéve, ha john@notcontoso.com jelenlét rendelkezik a contoso.com címtárban. Jelentkezik be a munkahelyi vagy iskolai fiókok felhasználók továbbra is a Microsoft Account felhasználók hozzáadása a szolgáltatás-rendszergazdaként.

  | Bejelentkezési módszer | A Microsoft Account felhasználó hozzáadása a szolgáltatás-rendszergazdaként? | Adja hozzá munkahelyi vagy iskolai fiók ugyanazon a szervezeten belül, szolgáltatás-rendszergazdaként? | Munkahelyi vagy iskolai fiók hozzáadása a másik szervezet szolgáltatás-rendszergazdaként? |
  | --- | --- | --- | --- |
  |  Microsoft-fiók |Igen |Nem |Nem |
  |  Munkahelyi vagy iskolai fiókkal |Igen |Igen |Nem |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>A fiókadminisztrátor az Azure-előfizetés módosítása

A fiók rendszergazdája a felhasználót, hogy kezdetben az Azure-előfizetést regisztrált, és felelős a számlázási tulajdonosa az előfizetés. Az előfizetés fiókadminisztrátori módosításához lásd [Azure-előfizetés tulajdonjogának átruházása másik fiókra](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Nem tudom, akik a a fiók rendszergazdája?** Kövesse az alábbi lépéseket:

1. Látogasson el [ **előfizetések** az Azure Portalon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Válassza ki az előfizetést, gombra, majd keresse meg a kívánt **beállítások**.
1. Válassza ki **tulajdonságok**. Az előfizetés fiókadminisztrátori fiókjával jelenik meg a **Fiókadminisztrátor** mezőbe.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>További információ az erőforrás-hozzáférés-vezérlés és az Active Directory

* RBAC kapcsolatos további információkért lásd: [Mi a szerepköralapú hozzáférés-vezérlés (RBAC)?](../role-based-access-control/overview.md)
* Az Azure-ban minden szerepkörökkel kapcsolatos további tudnivalókért lásd: [megismerheti az Azure-ban a különféle szerepkörök](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Az Azure Active Directory szolgáltatással kapcsolatos további információkért lásd: [kapcsolódnak az Azure-előfizetések az Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) és [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Segítség Forduljon az ügyfélszolgálathoz.

Ha továbbra is segítségre van szüksége, [forduljon az ügyfélszolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma gyors megoldása érdekében.

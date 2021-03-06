---
title: A bejelentkezési kockázati szabályzat beállítása az Azure Active Directory Identity Protection |} A Microsoft Docs
description: Ismerje meg az Azure AD Identity Protection bejelentkezési kockázati szabályzat konfigurálása.
services: active-directory
keywords: az Azure active directory identity protection a következőket cloud app discovery szolgáltatást, alkalmazások, biztonság, kockázati, kockázati szint, biztonsági rést, biztonsági házirend kezelése
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: bae71c55bdea838d0ef5e0ae3acbac3e98a92fa0
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055051"
---
# <a name="how-to-configure-the-sign-in-risk-policy"></a>Útmutató: A bejelentkezési kockázati szabályzat beállítása

Az Azure Active Directory észleli [kockázati események típusairól](../reports-monitoring/concept-risk-events.md#risk-event-types) a valós idejű és offline állapotban van. Minden kockázati esemény észlelt egy bejelentkezhet a felhasználó hozzájárul a kockázatos bejelentkezési neve egy logikai fogalom. Kockázatos bejelentkezés egy mutató a egy bejelentkezési kísérlet, amely előfordulhat, hogy nem hajtott végre egy felhasználói fiók jogos tulajdonosa.


## <a name="what-is-the-sign-in-risk-policy"></a>Mi az a bejelentkezési kockázati házirend?

Azure ad-ben elemzi az egyes bejelentkezési felhasználói. Az elemzés célja, észleli a gyanús tevékenységeket és a bejelentkezési származnak. Ha például a bejelentkezési kész névtelen IP-cím használatával, vagy egy ismeretlen helyről kezdeményezett bejelentkezési? Az Azure ad-ben a rendszer képes észlelni a gyanús tevékenységeket is ismertek kockázati események. A kockázati események, amely az észlelt a bejelentkezés során, az Azure AD kiszámítja egy érték alapján. Az érték a valószínűség (alacsony, közepes, magas), hogy a bejelentkezés nem történik meg a jogos felhasználója jelöli. A neve annak a valószínűsége **bejelentkezési kockázati szint**.

A bejelentkezési kockázati házirend egy automatikus válasz egy konkrét bejelentkezési kockázati szint konfigurálható. A válaszban az erőforrásokhoz való hozzáférés letiltása, vagy átadja a többtényezős hitelesítés (MFA) kihívást eléréséhez szükséges.

   
## <a name="how-do-i-access-the-sign-in-risk-policy"></a>Hogyan érhetem el a bejelentkezési kockázati házirend?
   
A bejelentkezési kockázati házirend van a **konfigurálása** szakaszában a [Azure AD Identity Protection lapról](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Bejelentkezési kockázati házirend](./media/howto-sign-in-risk-policy/1014.png "bejelentkezési kockázati házirend")


## <a name="policy-settings"></a>Szabályzat beállításai

A bejelentkezési kockázati házirend konfigurálásakor kell beállítani:

- A felhasználók és csoportok, a szabályzat vonatkozik:

    ![Felhasználók és csoportok](./media/howto-sign-in-risk-policy/11.png)

- A bejelentkezési kockázati szintet, amely elindítja a szabályzat:

    ![Bejelentkezési kockázati szint](./media/howto-sign-in-risk-policy/12.png)

- Milyen típusú hozzáférést szeretne kényszeríti ki a bejelentkezési kockázati szint teljesülésekor:  

    ![Hozzáférés](./media/howto-sign-in-risk-policy/13.png)

- A szabályzat állapotát:

    ![Szabályzat kényszerítése](./media/howto-sign-in-risk-policy/14.png)


A szabályzat konfigurációs párbeszédpanel újrakonfigurálás hatásának megbecsüléséhez lehetőséget biztosít.

![Becsült hatás](./media/howto-sign-in-risk-policy/15.png)

## <a name="what-you-should-know"></a>Tudnivalók

Konfigurálhatja a bejelentkezési kockázat biztonsági házirend többtényezős hitelesítés megkövetelése:

![MFA megkövetelése](./media/howto-sign-in-risk-policy/16.png)

Azonban biztonsági okokból a beállítás csak akkor működik, amely a multi-factor Authentication regisztrált felhasználók számára. Identity protection a többtényezős hitelesítés követelménye rendelkező felhasználók letiltja a nem regisztrált multi-factor Authentication még.

Ha azt szeretné, a többtényezős hitelesítés megkövetelése a kockázatos bejelentkezések, a következőket kell elvégeznie:

1. Engedélyezze a [többtényezős hitelesítési regisztrációs házirend](#multi-factor-authentication-registration-policy) az érintett felhasználók számára.

2. Az érintett felhasználókat, hogy jelentkezzen be egy kockázatos munkamenetben az MFA-regisztráció végrehajtásához szükséges.

Ezen lépések elvégzése biztosítja, hogy a többtényezős hitelesítés szükséges a kockázatos bejelentkezés.

A bejelentkezési kockázati házirend van:

- Böngésző forgalom és a modern hitelesítést használó bejelentkezések alkalmazza.

- Tiltsa le a WS-Trust végpont a következő az összevont Identitásszolgáltató, például az AD FS korábbi biztonsági protokollokat használó alkalmazások nem alkalmazza.


A kapcsolódó felhasználói szolgáltatások áttekintését lásd:

* [Kockázatos bejelentkezési helyreállítási](flows.md#risky-sign-in-recovery)
* [Kockázatos bejelentkezés blokkolva](flows.md#risky-sign-in-blocked)  
* [Az Azure AD Identity Protection a bejelentkezési élmény](flows.md)  

## <a name="best-practices"></a>Ajánlott eljárások

Kiválasztása egy **magas** küszöbérték csökkenti a szabályzat akkor aktiválódik, és minimalizálja a felhasználók számát.  

Azonban nem tartalmazza **alacsony** és **Közepes** a szabályzatot, amely esetleg blokkolja egy támadó a sérült biztonságú identitás kihasználhassa a kockázatosként megjelölt bejelentkezések.

A házirend beállításakor

- Felhasználók, akik nem / nem lehet többtényezős hitelesítés kihagyása

- Zárja ki a területi beállításokat, amennyiben a szabályzat engedélyezése nem célszerű a felhasználók (például segélyszolgálat nincs hozzáférés)

- Zárja ki a felhasználók, akik várhatóan sok vakriasztások (a fejlesztők, adatbiztonsági elemzők) létrehozása

- Használja a **magas** küszöbérték kezdeti szabályzat bevezetés során, vagy ha kell minimálisra csökkenthető a végfelhasználók által látott kihívásokat.

- Használja a **alacsony** küszöbértéket, ha a szervezet megköveteli a nagyobb biztonság. Válassza a **alacsony** küszöbérték vezet be a további felhasználói bejelentkezési kihívásokat, de a nagyobb biztonság.

A legtöbb szervezet számára ajánlott alapértelmezés szerint a rendszer a szabály konfigurálása egy **Közepes** küszöbérték egyensúlyozni használhatósági és a biztonság között.






## <a name="next-steps"></a>További lépések

Az Azure AD Identity Protection áttekintést kaphat, tekintse meg a [áttekintése az Azure AD Identity Protection](overview.md).

---
title: A B2B együttműködés meghívót tartalmazó e - Azure Active Directory elemeinek |} A Microsoft Docs
description: Az Azure Active Directory B2B együttműködés meghívó e-mail sablon
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: cbb81f62bcea43c3c30abff6b5f76e3a89347813
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985396"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>A B2B együttműködés meghívót tartalmazó e - Azure Active Directory elemei

Meghívó e-mailek minősítés kulcsfontosságú ahhoz, hogy a partnerek hajón B2B együttműködés felhasználóként az Azure ad-ben. A címzett megbízhatóság növelése érdekében használhatja őket. jogtisztasága adhat hozzá, és az e-mailt, ellenőrizze, hogy a címzett, a közösségi koncepció kapcsolatcsoporté kijelölése magabiztosan a **Ismerkedés** gombra kattintva fogadja el a meghívást. Ebben a megbízhatósági kapcsolatban egy kulcsot azt jelenti, hogy csökkentse a megosztási fennakadások nélkül használható. És szeretné is, hogy az e-mailben remekül!

![Az Azure AD B2b meghívó e-mail](media/invitation-email-elements/invitation-email.png)

## <a name="explaining-the-email"></a>Az e-mailben tájékoztatja
Tekintsünk meg néhány elemét az e-mailt, hogy tudja, hogyan lehet a legjobban a képességeinek használata érdekében.

### <a name="subject"></a>Tárgy
Az e-mail tárgyában a következő mintát követi: meghívót kapott a &lt;tenantname&gt; szervezet

### <a name="from-address"></a>Feladó címe
A feladó címe LinkedIn-hez hasonló mintát használjuk.  Legyen egyértelmű a meghívót küldő személy, aki és amely vállalati és is elmagyarázza, hogy az e-mail érkezik egy Microsoft e-mail-címét. A formátum: &lt;megjelenített nevét, a meghívót küldő személy&gt; a &lt;tenantname&gt; (a Microsofton keresztül) <invites@microsoft.com>

### <a name="reply-to"></a>Válasz címzettje
A válasz az e-mailben, ha elérhető, a meghívót küldő személy e-mail van beállítva, hogy e-mail-válasz egy e-mailt küld vissza a meghívót küldő személy.

### <a name="branding"></a>Védjegyezés
A bérlő használja a meghívói e-mailekről a vállalati arculat megjelenítése, amikor lehetséges, hogy beállította a bérlő számára. Ha azt szeretné, ez a funkció kihasználásához [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) konfigurálásának részleteiért vannak. A szalagcímembléma jelenik meg az e-mailben. Hajtsa végre a lemezkép mérete és minőségi utasítások [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) a legjobb eredmények elérése érdekében. Emellett a vállalat nevét is megjelenik-e a művelet meghívásához.

### <a name="call-to-action"></a>Művelet hívása
A művelet meghívásához két részből áll: Miért érdemes a címzett megkapta az e-mailt, és amit a címzett van a rendszer arról tájékoztatja.
- A "Miért" szakaszt a következő minta használatával is kezelhetők: az alkalmazás-hozzáférési meghívót kapott a &lt;tenantname&gt; szervezet

- És a "Mi folyamatban kéri, hogy tegye" szakasz jelenlétét jelzi a **Ismerkedés** gombra. Meghívók nélkül hozzáadása után a címzett, ez a gomb nem jelenik meg.

### <a name="inviters-information"></a>Meghívója tartozó információk
Az e-mail tartalmazza a meghívót küldő személy megjelenített neve. És emellett, ha beállította az Azure AD-fiókot a profilhoz tartozó fotó, a meghívó e-mailben szerepelni fog is, hogy a kép. Mindkét célja, hogy az e-mailt a címzett bizalom erősítése érdekében.

Ha még állított be a profilképét, a meghívót küldő személy monogramja a kép helyett egy ikon jelenik meg:

  ![Megjelenítés a meghívót küldő személy monogramja](media/invitation-email-elements/inviters-initials.png)

### <a name="body"></a>Törzs
A törzse tartalmazza az üzenetet, amely a meghívót küldő személy composes, vagy a meghívó API keresztülmegy. Egy szövegterület, így nem dolgozza fel biztonsági okokból HTML-címkéket.

### <a name="footer-section"></a>Lábléc szakasz
A lábléc tartalmazza a Microsoft vállalati márka, és lehetővé teszi, hogy a címzett tudja, ha a rendszer elküldte az e-mail címről küldtük. Különleges eset:

- A meghívót küldő személy nem rendelkezik egy e-mail-címet a meghívó bérlőben

  ![képe a meghívót küldő személy nem rendelkezik egy e-mail-címet a meghívó bérlőben](media/invitation-email-elements/inviter-no-email.png)


- A címzett nem kell beváltani a meghívót

  ![Amikor a címzett nem szüksége van a meghívó beváltása](media/invitation-email-elements/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>További lépések

Az Azure AD B2B együttműködés a következő cikkekben talál:

- [Mi az Azure AD B2B együttműködés](what-is-b2b.md)
- [Hogyan hozzá az Azure Active Directory-rendszergazdák B2B együttműködési felhasználókat?](add-users-administrator.md)
- [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködési felhasználókat?](add-users-information-worker.md)
- [B2B együttműködés vendégmeghívás beváltása](redemption-experience.md)
- [Adja hozzá a B2B-együttműködés felhasználók meghívás nélkül](add-user-without-invite.md)

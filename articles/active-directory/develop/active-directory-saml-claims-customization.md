---
title: A SAML-jogkivonat a vállalati alkalmazások az Azure Active Directoryban kiállított jogcímek testreszabása |} Microsoft Docs
description: A SAML-jogkivonat a vállalati alkalmazások az Azure Active Directoryban kiállított jogcímek testreszabása
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: jeedes
ms.custom: aaddev
ms.openlocfilehash: db529bf1e8ea4363c84cb365444ca367d428b162
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318420"
---
# <a name="customizing-claims-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>A SAML-jogkivonat a vállalati alkalmazások az Azure Active Directoryban kiállított jogcímek testreszabása
Ma az Azure Active Directory a legtöbb vállalati alkalmazás, így mindkét az Azure AD-alkalmazásgyűjtemény, valamint egyéni alkalmazások előre integrált alkalmazás támogatja a egyszeri bejelentkezéshez. Amikor egy felhasználó hitelesíti magát egy alkalmazást a Azure AD a SAML 2.0 protokoll segítségével, az Azure AD egy token küld az alkalmazás (HTTP POST). És ezt követően az alkalmazás ellenőrzi és használja a jogkivonatot a felhasználót a nem kér felhasználónevet és jelszót. Ezeket a SAML-jogkivonatokat a "jogcímek" néven ismert felhasználóval kapcsolatos információt tartalmazza.

Az identitás-beszéd, a "jogcímek" információ arról, hogy az identitásszolgáltató azokat, hogy a felhasználó számára kiadják a tokenen belül a felhasználó. A [SAML-jogkivonat](http://en.wikipedia.org/wiki/SAML_2.0), az adatokat általában a SAML Attribute utasítást tartalmaz. A SAML tulajdonos, más néven azonosítója a általában jelzi a felhasználó egyedi azonosítója.

Alapértelmezés szerint a Azure Active Directory SAML-jogkivonatból állít ki az alkalmazáshoz, amely tartalmazza a NameIdentifier jogcím értéke az a felhasználónév (AKA egyszerű felhasználónév) Azure AD-ben. Ez az érték a felhasználó egyedileg azonosíthatja. A SAML-jogkivonat a felhasználó e-mail címét, Utónév és Vezetéknév tartalmazó további jogcímeket is tartalmaz.

Megtekinteni vagy szerkeszteni a SAML-jogkivonat az alkalmazás által kiadott jogcímeket, nyissa meg az alkalmazás Azure-portálon. Válassza ki a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzet a **felhasználói attribútumok** szakaszban az alkalmazás.

![Felhasználói attribútumok szakasz][1]

Ennek oka két miért szükség lehet a SAML-jogkivonat kiadott jogcímeket szerkesztése:
* Az alkalmazás eltérő szabályzatkészletet jogcím URI-azonosítók, vagy a jogcímértékek van írva.
* Az alkalmazás telepítve lett a NameIdentifier jogcímet, mint az Azure Active Directoryban tárolt felhasználónév (AKA egyszerű felhasználónév) igényel.

Az alapértelmezett jogcímértékek tudja szerkeszteni. Jelölje ki a jogcímet az SAML-jogkivonat attribútumok táblában. Ekkor megnyílik a **attribútum szerkesztése** szakaszt, és ezután szerkesztheti jogcím neve, értékét és a jogcímek használata.

![Felhasználói attribútum szerkesztése][2]

Is eltávolíthatja a jogcímek (eltérő NameIdentifier) a helyi menü, amely kattintva megnyílik a **...**  ikonra. Új jogcímek is hozzáadhat a **Hozzáadás attribútum** gombra.

![Felhasználói attribútum szerkesztése][3]

## <a name="editing-the-nameidentifier-claim"></a>A NameIdentifier jogcímszabályok szerkesztése
Megoldani a problémát, ahol az alkalmazás már telepített egy másik felhasználónévvel, kattintson a a **felhasználói azonosító** legördülő listán a **felhasználói attribútumok** szakasz. Ez a művelet több, különböző beállításokkal párbeszédpanel biztosítja:

![Felhasználói attribútum szerkesztése][4]

Válassza ki a legördülő **user.mail** a NameIdentifier jogcímet a felhasználó e-mail címét a könyvtárban kell beállítani. Vagy válassza ki **user.onpremisessamaccountname** felhasználó által a SAM-fiók neve, amely a helyszíni Azure AD szinkronizálása megtörtént.

Használhatja a speciális **ExtractMailPrefix()** a tartományi utótag eltávolítja az e-mail cím, a SAM-neve vagy a felhasználó egyszerű felhasználóneve. Ez kibontása csak a továbbítja a folyamatban felhasználónév első része (például "joe_smith" helyett joe_smith@contoso.com).

![Felhasználói attribútum szerkesztése][5]

Most is jelentek meg a **join()** függvény a felhasználói azonosító értékét az ellenőrzött tartományhoz való csatlakozáshoz. Ha bejelöli a join() függvényt a **felhasználói azonosító** először válassza ki a felhasználói azonosító hasonló e-mail cím vagy a felhasználó egyszerű felhasználónév és a második legördülő listán válassza az ellenőrzött tartomány. Ha az e-mail cím ellenőrzött tartományával választja, akkor az Azure AD kibontja a felhasználónév az első érték joe_smith a joe_smith@contoso.com és a contoso.onmicrosoft.com fűzi azokat. Tekintse meg a következő példát:

![Felhasználói attribútum szerkesztése][6]

## <a name="adding-claims"></a>Jogcím hozzáadása
Jogcím hozzáadása egy szabálykészlethez, amikor az attribútumnév (amely nem feltétlenül kell követniük a SAML-specifikáció szerint URI mintát) is megadhat. Adja meg az értéket bármely felhasználói attribútumot, amelyet a könyvtárban tárolnak.

![Felhasználói attribútum hozzáadása][7]

Például szeretné küldeni a osztály, a felhasználó tagja a szervezetek (mint például a Sales) jogcímszabályként. Adja meg a jogcím nevét, az alkalmazás által várt, és válassza **felhasználó.részleg** értékeként.

> [!NOTE]
> Ha a megadott felhasználó nincs a kiválasztott attribútumnak tárolt érték, majd, hogy a jogcím nem küldése történik a tokenben.

> [!TIP]
> A **user.onpremisesecurityidentifier** és **user.onpremisesamaccountname** csak akkor támogatott, ha a felhasználói adatok szinkronizálása a helyszíni Active Directory használatával a [az Azure AD Csatlakozás eszköz](../active-directory-aadconnect.md).

## <a name="restricted-claims"></a>Korlátozott jogcímek

SAML néhány korlátozott jogcímek szerepelnek. Ha ezeket a jogcímeket, majd az Azure AD nem küld ezeket a jogcímeket. Az alábbiakban a SAML-alapú korlátozott jogcímkészlethez:

    | Jogcím típusa (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expired |
    | http://schemas.microsoft.com/identity/claims/accesstoken |
    | http://schemas.microsoft.com/identity/claims/openid2_id |
    | http://schemas.microsoft.com/identity/claims/identityprovider |
    | http://schemas.microsoft.com/identity/claims/objectidentifier |
    | http://schemas.microsoft.com/identity/claims/puid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier[MR1] |
    | http://schemas.microsoft.com/identity/claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groups |
    | http://schemas.microsoft.com/claims/groups.link |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/role |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/identity/claims/scope |

## <a name="next-steps"></a>További lépések
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](../active-directory-apps-index.md)
* [Egyszeri bejelentkezés konfigurálása az Azure Active Directory alkalmazáskatalógusában nem szereplő alkalmazásokhoz](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Hibaelhárítási SAML-alapú egyszeri bejelentkezést.](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png

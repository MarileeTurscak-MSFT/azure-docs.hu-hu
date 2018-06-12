---
title: Összevonási tanúsítványok kezelése az Azure AD |} Microsoft Docs
description: Ismerje meg, hogyan szabhatja testre a összevonási tanúsítványok lejárati dátuma és rövidesen lejáró tanúsítvány megújítása.
services: active-directory
documentationcenter: ''
author: jeevansd
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2017
ms.author: jeedes
ms.openlocfilehash: c4d812db6371a4cd1fcc701f7eae2c913c29fb6b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303598"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Összevont egyszeri bejelentkezés az Azure Active Directoryban tanúsítványainak kezelése
Ez a cikk ismerteti a gyakori kérdéseket és a tanúsítványok, az Azure Active Directory (Azure AD) hoz létre az SaaS-alkalmazások létrehozásához összevont egyszeri bejelentkezést (SSO) kapcsolatos adatokat. Az Azure AD-alkalmazásgyűjtemény vagy egy alkalmazás nem galéria sablon használatával, vegyen fel alkalmazásokat. Az alkalmazás beállítása az összevont egyszeri bejelentkezési beállítás használatával.

Ez a cikk fontos csak olyan alkalmazásokat, amelyek használatára konfigurált Azure AD SSO keresztül SAML összevonási, a következő példában látható módon:

![Az Azure AD-egyszeri bejelentkezés](./media/manage-certificates-for-federated-single-sign-on/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automatikusan létrehozott tanúsítvány gyűjteménye, és nem galéria-alkalmazásokhoz
Új alkalmazás felvétele a gyűjteményből, és egy SAML-alapú bejelentkezés konfigurálásakor, az Azure AD a létrehoz egy tanúsítványt a három évig érvényes alkalmazás. Ez a tanúsítvány a programot letöltheti a **SAML-aláíró tanúsítványa** szakasz. Gyűjteményelem alkalmazások Ez a szakasz egy lehetőség, hogy töltse le a tanúsítványt, vagy a metaadatok, attól függően, hogy az alkalmazás követelmény előfordulhat, hogy megjelenítése.

![Az Azure AD egyszeri bejelentkezést.](./media/manage-certificates-for-federated-single-sign-on/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Testre szabhatja a összevonási tanúsítvány lejárati dátuma, és új tanúsítványt váltása
Alapértelmezés szerint a tanúsítványok állítottak három év után lejár. A tanúsítvány egy másik lejárati dátuma a következő lépések végrehajtásával szerint is választhat.
A képernyőfelvételek Salesforce használja az példa, de ezeket a lépéseket alkalmazhatja bármely összevont SaaS-alkalmazásokhoz.

1. Az a [Azure-portálon](https://aad.portal.azure.com), kattintson a **vállalati alkalmazás** a bal oldali ablaktáblán, és kattintson a **új alkalmazás** a a **áttekintése** lap:

   ![Nyissa meg az egyszeri bejelentkezés beállítása varázsló](./media/manage-certificates-for-federated-single-sign-on/enterprise_application_new_application.png)

2. Keresse meg a gyűjtemény alkalmazás, és válassza ki a hozzáadni kívánt alkalmazást. Ha a szükséges alkalmazás nem található, adja hozzá az alkalmazás használatával az **nem-gyűjtemény alkalmazás** lehetőséget. Ez a funkció csak az Azure AD Premium (P1 és P2) SKU érhető el.

    ![Az Azure AD egyszeri bejelentkezést.](./media/manage-certificates-for-federated-single-sign-on/add_gallery_application.png)

3. Kattintson a **egyszeri bejelentkezés** hivatkozásra a bal oldali ablaktáblán, és módosítsa **egyszeri bejelentkezés mód** való **SAML-alapú bejelentkezés**. Ezt a lépést az alkalmazás három év tanúsítványt hoz létre.

4. Hozzon létre egy új tanúsítványt, kattintson a **hozzon létre új tanúsítvány** hivatkozásra a **SAML-aláíró tanúsítványa** szakasz.

    ![Új tanúsítvány létrehozása](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

5. A **hozzon létre egy új tanúsítványt** a hivatkozás megnyitja a havinaptár-vezérlőben. Beállíthatja bármely dátum és idő az aktuális dátum utáni legfeljebb három év. A kiválasztott dátum és idő az új lejárati dátum és idő az új tanúsítvány. Kattintson a **Save** (Mentés) gombra.

    ![Töltse le, majd a tanúsítvány feltöltése](./media/manage-certificates-for-federated-single-sign-on/certifcate_date_selection.PNG)

6. Most már az új tanúsítvány áll le. Kattintson a **tanúsítvány** innen tölthető le azt. Ezen a ponton a tanúsítvány nem lesz aktív. Ha szeretné ezt a tanúsítványt a váltása, válassza ki a **új tanúsítvány aktiválásához** jelölőnégyzetet, majd kattintson **mentése**. Ettől a ponttól az Azure AD elindítja az új tanúsítványt használja a válasz az aláíráshoz.

7.  Megtudhatja, hogyan tölthet fel a tanúsítvány az adott SaaS-alkalmazáshoz, kattintson a **nézet konfigurációs oktatóanyag** hivatkozásra.

## <a name="certificate-expiration-notification-email"></a>Tanúsítvány lejáratának értesítő e-mailt

Az Azure AD elküld egy e-mailek értesítés 60, 30 és 7 nap SAML-tanúsítvány érvényességének lejárta előtt. Az e-mail címet az értesítés küldési helyének megadása:

- Az Azure Active Directory alkalmazás egyszeri bejelentkezés lapon nyissa meg az értesítő e-mailt mező.
- Adja meg a tanúsítvány lejárati értesítő e-mailt kell kapnia, e-mail címét. Alapértelmezés szerint ez a mező a rendszergazda, aki hozzá az alkalmazás e-mail címét használja.

Az értesítő e-mailt fog kapni aadnotification@microsoft.com. Az e-mailt a levélszemét-helyre is elkerülése érdekében ügyeljen arra, hogy adja hozzá az e-mailt az ügyfelekhez. 

## <a name="renew-a-certificate-that-will-soon-expire"></a>Rövidesen lejáró tanúsítvány megújítása
A következő megújítási lépéseket kell eredményeznie jelentős állásidő nélkül a felhasználók számára. Ez a szakasz a szolgáltatás Salesforce példaként, de ezeket a lépéseket a képernyőképek bármely összevont SaaS-alkalmazás is alkalmazhatja.

1. Az a **Azure Active Directory** alkalmazás **egyszeri bejelentkezés** lapon, az alkalmazás új tanúsítvány jön létre. Ehhez kattintson a **hozzon létre új tanúsítvány** hivatkozásra a **SAML-aláíró tanúsítványa** szakasz.

    ![Új tanúsítvány létrehozása](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

2. Válassza ki a kívánt lejárati dátum és idő az új tanúsítvány, és kattintson a **mentése**.

3. Töltse le a tanúsítványt a **SAML-aláíró tanúsítványa** lehetőséget. Töltse fel az új tanúsítványt az SaaS-alkalmazás egyszeri bejelentkezés konfigurálására szolgáló képernyőn. Megtudhatja, hogyan hajthatja végre az adott SaaS-alkalmazáshoz, kattintson a **nézet konfigurációs oktatóanyag** hivatkozásra.
   
4. Az Azure ad-ben az új tanúsítvány aktiválásához jelölje be a **új tanúsítvány aktiválásához** jelölőnégyzetet, majd kattintson a **mentése** gombra az oldal tetején. Ez összegzi az új tanúsítvány keresztül, az Azure AD oldalán. A tanúsítvány állapota a **új** való **aktív**. Ettől a ponttól az Azure AD elindítja az új tanúsítványt használja a válasz az aláíráshoz. 
   
    ![Új tanúsítvány létrehozása](./media/manage-certificates-for-federated-single-sign-on/new_certificate_download.png)

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](../active-directory-saas-tutorial-list.md)
* [Alkalmazások kezelése az Azure Active Directoryban cikk indexe](../active-directory-apps-index.md)
* [Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](what-is-single-sign-on.md)
* [Hibaelhárítási SAML-alapú egyszeri bejelentkezést.](../develop/active-directory-saml-debugging.md)

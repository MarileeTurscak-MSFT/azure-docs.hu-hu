---
title: A probléma összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása |} Microsoft Docs
description: Összevont egyszeri bejelentkezést az egyéni SAML-alkalmazás, amely nem szerepel az Azure AD Application Gallery konfigurálása során felmerülő gyakori problémák megoldása
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 36262320a5a8457b22cbe9fe9d902fda26b6609c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/23/2018
ms.locfileid: "36331104"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>A probléma összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása

Ha probléma merül fel, ha az alkalmazások konfigurálásáról. Ellenőrizze, hogy a cikkben szereplő összes lépésekkel [egyszeri bejelentkezés alkalmazásokhoz, amelyek nincsenek rajta az Azure Active Directory alkalmazáskatalógusában konfigurálása.](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery)

## <a name="cant-add-another-instance-of-the-application"></a>Az alkalmazás egy másik példánya nem vehető fel.

Adja hozzá az alkalmazás második példányát, kell tennie:

-   Konfigurálja a második példány egyedi azonosítója. Az első példánynál használt ugyanezzel az azonosítóval nem konfigurálható.

-   Az első példánynál használttól eltérő tanúsítvány konfigurálása.

Ha az alkalmazás nem támogatja az előző bármely, egy második példányt nem konfigurálható.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Ha állította be a entityid beállítást (felhasználói azonosító) formátumát

Nem választhat ki, hogy az Azure AD elküldi az alkalmazásnak a válaszban szereplő felhasználók hitelesítése után entityid beállítást (felhasználói azonosító) formátuma.

Az Azure AD választ, a a NameID attribútum (felhasználói azonosító) a kijelölt érték alapján vagy formátumban a SAML AuthRequest az alkalmazás által kért. További információ látogasson el a [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) szakaszban NameIDPolicy,

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Ha az alkalmazás metaadatainak vagy a tanúsítvány lekérése az Azure AD

Töltse le a metaadatokat vagy a tanúsítvány az Azure AD, kövesse az alábbi lépéseket:

1.  Nyissa meg a [ **Azure-portálon** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **minden szolgáltatás** a fő bal oldali navigációs menü tetején.

3.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.

   * Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes Alkalmazások.**

6.  Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.

7.  Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.

8.  Ugrás a **SAML-aláíró tanúsítványa** területen, majd kattintson **letöltése** oszlop értékét. Attól függően, hogy milyen az alkalmazáshoz az szükséges, az egyszeri bejelentkezés konfigurálása lásd: a metaadatok XML-kód letöltése beállítás, vagy a tanúsítvány.

Az Azure AD nem biztosít a metaadatok beolvasása URL-CÍMÉT. A metaadatok XML-fájlként csak olvasható.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Nem tudjuk kérelmet küldött SAML-jogcímek testreszabása

Megtudhatja, hogyan szabhatja testre a SAML attribútum típusú jogcímek az alkalmazás számára, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.

## <a name="next-steps"></a>További lépések
[Alkalmazások kezelése az Azure Active Directoryval](manage-apps/what-is-application-management.md)

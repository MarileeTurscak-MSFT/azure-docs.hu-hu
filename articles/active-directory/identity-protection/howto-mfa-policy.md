---
title: A többtényezős hitelesítési regisztrációs házirend konfigurálása az Azure Active Directory Identity Protection |} A Microsoft Docs
description: Ismerje meg, hogyan konfigurálhatja az Azure AD Identity Protection többtényezős hitelesítési regisztrációs házirend.
services: active-directory
keywords: az Azure active directory identity protection a következőket cloud app discovery szolgáltatást, alkalmazások, biztonság, kockázati, kockázati szint, biztonsági rést, biztonsági házirend kezelése
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 792a1fc2403e672c973577efd7a05c9c81d45ad4
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47054081"
---
# <a name="how-to-configure-the-multi-factor-authentication-registration-policy"></a>Útmutató: A többtényezős hitelesítési regisztrációs házirend konfigurálása

Az Azure AD Identity Protection segítséget az üzembe helyezést (MFA) többtényezős hitelesítési regisztrációs házirend konfigurálásával. Ez a cikk leírja, mi a házirend használható egy konfigurálásának.

## <a name="what-is-the-multi-factor-authentication-registration-policy"></a>Mi az a többtényezős hitelesítési regisztrációs házirend?

Az Azure multi-factor authentication a egy módszer annak ellenőrzése, akik, amely több, mint felhasználónév és jelszó szükséges. Biztosít egy második biztonsági szintként, felhasználói bejelentkezéseket és tranzakciókat.  

Azt javasoljuk, hogy az Azure multi-factor authentication a felhasználók bejelentkezési folyamatába igényel el, mert azt:

- Szolgáltatás egyszerű ellenőrzési lehetőség számos szigorú hitelesítést végez

- A szervezet védelme és helyreállítása a fiók feltörések előkészítése a kulcsfontosságú szerepet játszik az


További részletekért lásd: [Mi az Azure multi-factor Authentication?](../authentication/multi-factor-authentication.md)


## <a name="how-do-i-access-the-mfa-registration-policy"></a>Hogyan érhetem el az MFA regisztrációs szabályzatának?
   
Az MFA regisztrációs szabályzatának szerepel a **konfigurálása** szakaszában a [Azure AD Identity Protection lapról](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Többtényezős hitelesítési szabályzat](./media/howto-mfa-policy/1014.png)




## <a name="policy-settings"></a>Szabályzat beállításai

A bejelentkezési kockázati házirend konfigurálásakor kell beállítani:

- A felhasználók és csoportok, a szabályzat vonatkozik:

    ![Felhasználók és csoportok](./media/howto-mfa-policy/11.png)

- Milyen típusú hozzáférést szeretne érvényesíteni:  

    ![Felhasználók és csoportok](./media/howto-mfa-policy/12.png)

- A szabályzat állapotát:

    ![Szabályzat kényszerítése](./media/howto-mfa-policy/14.png)


A szabályzat konfigurációs párbeszédpanel a konfiguráció hatásának megbecsüléséhez lehetőséget biztosít.

![Becsült hatás](./media/howto-mfa-policy/15.png)




## <a name="user-experience"></a>Felhasználói élmény


A kapcsolódó felhasználói szolgáltatások áttekintését lásd:

* [A multi-factor authentication regisztrációs folyamat](flows.md#multi-factor-authentication-registration).  
* [Bejelentkezési élmény az Azure AD Identity Protection](flows.md).  



## <a name="next-steps"></a>További lépések

Az Azure AD Identity Protection áttekintést kaphat, tekintse meg a [áttekintése az Azure AD Identity Protection](overview.md).

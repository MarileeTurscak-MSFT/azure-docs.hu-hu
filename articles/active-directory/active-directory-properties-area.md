---
title: Adja hozzá a szervezet adatvédelmi információ az Azure ad-ben |} A Microsoft Docs
description: Azt ismerteti, hogyan adja hozzá a szervezet adatvédelmi információ az Azure Active Directory (Azure AD) tulajdonságok területére.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro
ms.openlocfilehash: a34fa2b8c2d966af108664c219a222fb9a5b7abc
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/12/2018
ms.locfileid: "35923517"
---
# <a name="how-to-add-your-organizations-privacy-info-in-azure-active-directory"></a>Útmutató: Az Azure Active Directoryban adja hozzá a szervezet adatvédelmi információ
Ez a cikk bemutatja, hogyan Bérlői rendszergazda adhat hozzá adatvédelemmel kapcsolatos adatokat a szervezet Azure Active Directory (Azure AD) bérlő az Azure Portalon keresztül.

Erősen ajánlott hozzáadása a globális adatvédelmi partner és a szervezet adatvédelmi nyilatkozata is, így belső alkalmazottai és külső vendégek tekintheti át a szabályzatokat. Mivel egyedileg létrehozott és az egyes üzleti szabott adatvédelmi nyilatkozatai, erősen ajánlott, segítségért forduljon jogászhoz.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="access-the-properties-area-to-add-your-privacy-info"></a>Hozzáférés az tulajdonságai területen adja hozzá az adatvédelmi információ

1.  Jelentkezzen be a bérlő rendszergazdájaként az Azure Portalon.

2.  A bal oldali navigációs sávon válassza **Azure Active Directory**, majd válassza ki **tulajdonságok**.

    A **tulajdonságok** területen jelenik meg.

    ![Az Azure AD tulajdonságok területen az adatvédelmi információs terület kiemelése](./media/active-directory-properties-area/properties-area.png)

3.  Adja hozzá az adatvédelmi információ az alkalmazottak számára:

    - **Elérhetőség – technikai.** Forduljon a technikai támogatás a szervezeten belüli személy e-mail címét írja be.
    
    - **Globális adatvédelmi partner.** Írja be a lekérdezések a személyes adatok védelméről a kapcsolattartó személy e-mail címet. Ezt a személyt az is, akik a Microsoft kapcsolatba lép az adatok megsértése esetén. Ha nem az itt felsorolt személy, a Microsoft felveszi a kapcsolatot a globális rendszergazdák.

    - **Adatvédelmi nyilatkozat URL-címe.** Írja be a hivatkozást, amely leírja, hogyan a szervezet kezeli mindkét dokumentum a szervezet belső és külső Vendég az adatvédelem.

        >[!Important]
        >Ha nem adja meg a saját adatvédelmi nyilatkozat vagy az adatvédelmi partner, a külső vendégek jelenik meg a szöveg a **felülvizsgálati engedélyek** szöveget,  **< _a szervezet neve_> nem biztosított, hogy áttekinthesse feltételekre mutató hivatkozásokat**. Vendégfelhasználó például megjelenik ez az üzenet, amikor egy szervezet hozzáférnie a B2B-együttműködés meghívót kapnak.

        ![B2B együttműködés áttekintése engedélyek üzenettel](./media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  Kattintson a **Mentés** gombra.

## <a name="next-steps"></a>További lépések
- [Az Azure Active Directory B2B együttműködés vendégmeghívás beváltása](https://aka.ms/b2bredemption)
- [Adja hozzá, vagy az Azure Active Directory felhasználó profilinformációinak módosítása](fundamentals/active-directory-users-profile-azure-portal.md)
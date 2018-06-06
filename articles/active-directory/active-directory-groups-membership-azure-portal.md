---
title: A csoportok a csoport beletartozik az Azure Active Directoryban kezelése |} Microsoft Docs
description: Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban. Megtudhatja, hogyan kezelheti az adott tagságok.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 9865f871f925b4ab1dd290b0afba3258581f0bf6
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34714323"
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Kezelheti az Azure Active Directory-bérlőhöz tartozik egy csoport mely csoportok
Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban. Megtudhatja, hogyan kezelheti az adott tagságok.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>Hogyan találhatom meg, amely a csoport tagja a csoportok?
1. Jelentkezzen be az [Azure AD felügyeleti központba](https://aad.portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **felhasználók és csoportok**.

   ![Nyitó felhasználók és csoportok kép](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. Válassza ki **összes csoport**.

   ![Csoportok lemezkép kiválasztása](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. Válasszon ki egy csoportot.
2. Válassza ki **csoporttagságok**.

   ![Nyitó csoport tagságát kép](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. A csoport tagja másik csoportnak, hozzáadja a a **Group - csoporttagságok** panelen válassza a **Hozzáadás** parancsot.
2. Válasszon csoportot a **csoport kijelölése** panelt, és válassza a **válasszon** gomb a panel alján. Egyszerre csak egy csoporthoz adhat hozzá a csoporthoz. A **felhasználói** mezőben szűrése a képernyőt a megfelelő felhasználó vagy eszköz nevek a bejegyzést. Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.

   ![A csoporttagság hozzáadása](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. A csoport eltávolításához egy másik csoport tagjaként a a **Group - csoporttagságok** panelen válasszon ki egy csoportot.
9. Válassza ki a **eltávolítása** parancsot, és erősítse meg választását a parancssorba.

   ![Távolítsa el a tagság parancs](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Amikor befejezte a csoport csoporttagság módosítása, válassza ki a **mentése**.

## <a name="additional-information"></a>További információ
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Tekintse meg a meglévő csoportok](active-directory-groups-view-azure-portal.md)
* [Hozzon létre egy új csoportot és tagok hozzáadása](active-directory-groups-create-azure-portal.md)
* [Egy csoport beállításainak kezelése](active-directory-groups-settings-azure-portal.md)
* [A csoport tagjai kezelése](active-directory-groups-members-azure-portal.md)
* [A csoport dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)

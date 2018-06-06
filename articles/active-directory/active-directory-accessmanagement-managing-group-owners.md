---
title: További lépések a csoportok használatával access Management szolgáltatáshoz |} Microsoft Docs
description: Hogyan speciális-a következőre vonatkozó biztonsági csoportok és erőforrásokhoz való hozzáférés kezelése ezek a csoportok használatával történő kezelése.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.custom: it-pro
ms.openlocfilehash: 0f379c22f1bad522a2d9bff399bbf7006f7c4920
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713279"
---
# <a name="managing-owners-for-a-group"></a>Egy csoport tulajdonosainak kezelése
Amennyiben az erőforrás tulajdonosa hozzá van rendelve hozzáférést egy erőforráshoz egy Azure Active Directory-csoportba, a csoport tagsága kezeli a csoport tulajdonosa. Az erőforrás tulajdonosa hatékonyan felhasználók hozzárendelése az erőforrást a csoport tulajdonosa engedélyt ad.

## <a name="add-an-owner-to-a-group"></a>Egy olyan tulajdonost hozzáadása csoporthoz

1. Az a [az Azure AD felügyeleti központban](https://aad.portal.azure.com), jelölje be **felhasználók és csoportok**.
2. Válassza ki **összes csoport**, majd nyissa meg a csoportot, amelyhez hozzá szeretné adni a tulajdonosai számára.
3. Válassza ki **adja hozzá a tulajdonosok**.
4. Az a **adja hozzá a tulajdonosok** lapon, válassza ki a felhasználót, hogy ez a csoport tulajdonosa hozzá, és győződjön meg arról, hogy ez a név a **kijelölt** ablaktáblán.

## <a name="remove-an-owner-from-a-group"></a>Egy olyan tulajdonost eltávolítása egy csoportból

1. Az a [az Azure AD felügyeleti központban](https://aad.portal.azure.com), jelölje be **felhasználók és csoportok**.
2. Válassza ki **összes csoport**, majd nyissa meg a csoport, amelyről el szeretné távolítani a tulajdonosai.
3. Válassza ki a **tulajdonosok** fülre.
4. Válassza ki a csoportból eltávolítani, majd válassza ki a kívánt tulajdonosát **eltávolítása**.

## <a name="additional-information"></a>További információ
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal](active-directory-manage-groups.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

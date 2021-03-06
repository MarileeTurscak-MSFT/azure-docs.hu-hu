---
title: Hagyja meg vendégként – Azure Active Directory-szervezet |} A Microsoft Docs
description: Bemutatja, hogyan egy Azure AD B2B vendégfelhasználó hagyhatja egy szervezet a hozzáférési panelen.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: cea882bd1ba2ba12d34690fb47ec1afd6edf5c4c
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982170"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Egy szervezet meg vendégként

Egy Azure Active Directory (Azure AD) B2B vendégfelhasználó dönt, hogy egy szervezet bármikor hagyja, ha már nincs szükségük használják az alkalmazásokat, hogy a szervezet vagy karbantartása van társítva. A felhasználó hagyhatja egy szervezet saját, forduljon a rendszergazdához, nélkül.

## <a name="leave-an-organization"></a>Egy szervezet

Egy szervezet felhasználóként jelentkezett be, hogy a [hozzáférési Panel](https://myapps.microsoft.com), tegye a következőket:

1. Ha még nem jelentkezett elhagyja a szervezet, a jobb felső sarokban válassza ki a nevét, majd kattintson a elhagyja a szervezetet.
2. A jobb felső sarokban válassza ki a nevét.
3. A **szervezetek**, válassza a beállítások (fogaskerék) ikonra.
 
   ![Képernyőfelvétel a hozzáférési panelen felhasználói beállításokat:](media/leave-the-organization/UserSettings.png) 

3. A **szervezetek**, keresse meg a szervezet, amely hagyja, és válassza ki a kívánt **szervezet**.

   ![Képernyőfelvétel: a szervezet beállítást hagyja a felhasználói felületen](media/leave-the-organization/LeaveOrg.png)

4. Ha a rendszer kéri, erősítse meg, válassza ki a **hagyja**. 

## <a name="account-removal"></a>Fiók eltávolítása

Amikor egy felhasználó elhagyja a szervezetet, a felhasználói fiók "törölték" a címtárban. Alapértelmezés szerint a felhasználói objektum átkerül a **törölt felhasználók** terület az Azure ad-ben de ez nem végleges törlése 30 napig. A helyreállítható törlés lehetővé teszi a rendszergazdák állítsa vissza a felhasználói fiók (beleértve a csoportok és engedélyek), ha a felhasználó kérést küld a fiók visszaállítása a 30 napos időszakon belül.

Igény szerint egy Bérlői rendszergazda véglegesen törölheti a fiókot a 30 napos időszakban bármikor. Ehhez tegye a következőket:

1. Az a [az Azure portal](https://portal.azure.com)válassza **Azure Active Directory**.
2. A **kezelés**válassza **felhasználók**.
3. Válassza ki **törölt felhasználók**.
4. A törölt felhasználó melletti jelölőnégyzetet, majd válassza ki és **véglegesen törli a**.

Ha egy felhasználó véglegesen törli, ez a művelet nem módosítható.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>További lépések

- Az Azure AD B2B áttekintését lásd: [Mi az Azure AD B2B együttműködés?](what-is-b2b.md)




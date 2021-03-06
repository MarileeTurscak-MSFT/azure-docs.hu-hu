---
title: Oktatóanyag – Azure Active Directory B2C-bérlő létrehozása |} A Microsoft Docs
description: Ismerje meg, hogyan készíti elő a Regisztrálás az alkalmazások létrehozásával az Azure Active Directory B2C-bérlő az Azure portal használatával.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7571e5f4d95320ab92fa3b69b0ea1f05ff9c771f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408402"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Oktatóanyag: Azure Active Directory B2C-bérlő létrehozása

Az alkalmazások az Azure Active Directory (Azure AD) B2C-vel kezelheti, mielőtt azok szerepelnie kell egy felügyelt bérlőhöz.

Ebben a cikkben az alábbiakkal ismerkedhet meg:

> [!div class="checklist"]
> * Azure AD B2C bérlő létrehozása
> * A bérlő az előfizetés mutató hivatkozás

Megismerheti, hogyan kell regisztrálni egy alkalmazást a következő oktatóanyagban.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C bérlő létrehozása

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).
2. Győződjön meg arról, hogy a könyvtár, amely tartalmazza az előfizetés kattintva használ, a **címtár és előfizetés-szűrő** a felső menüben, és az azt tartalmazó könyvtár kiválasztása. Ez az egy másik címtárban, amely nem tartalmazza az Azure AD B2C-bérlőben.

    ![Váltás előfizetési könyvtár](./media/tutorial-create-tenant/switch-directory-subscription.png)

3. Válasszon **erőforrás létrehozása** az Azure portal bal felső sarkában.
4. Keresse meg és válassza **Active Directory B2C**, és kattintson a **létrehozás**.
5. Válasszon **hozzon létre egy új Azure AD B2C-bérlő**, adjon meg egy nevet és kezdeti tartománynév, ami a bérlő nevét, válassza ki az ország (azt később már nem módosítható), és kattintson **létrehozás**.

    ![Bérlő létrehozása](./media/tutorial-create-tenant/create-tenant.png)

    Ebben a példában a bérlő neve a következő contoso0926Tenant.onmicrosoft.com

6. Az a **új B2C-bérlő létrehozása vagy csatolása egy létező bérlőhöz** lapon a **hivatkozásra egy meglévő Azure AD B2C-bérlőt szeretne aktiválni az Azure-előfizetésem**, válassza ki az Ön által létrehozott bérlőt, válassza ki az előfizetését, kattintson a **Új létrehozása** , és adjon meg egy nevet az erőforráscsoport, amely tartalmazni fogja a bérlő, válassza ki azt a helyet, majd kattintson a **létrehozás**.
7. Az új bérlő használatának megkezdéséhez, ellenőrizze, hogy használja az Azure AD B2C-bérlő kattintva tartalmazó könyvtárba a **címtár és előfizetés-szűrő** a felső menüben, és az azt tartalmazó könyvtár kiválasztása.

    ![Váltás a directory-bérlőben](./media/tutorial-create-tenant/switch-directories.png)

## <a name="next-steps"></a>További lépések

Ebben a cikkben megtanulta, hogyan lehet:

> [!div class="checklist"]
> * Azure AD B2C bérlő létrehozása
> * A bérlő az előfizetés mutató hivatkozás

> [!div class="nextstepaction"]
> [Egy webalkalmazás, fiókokkal történő hitelesítésének engedélyezése](active-directory-b2c-tutorials-web-app.md)
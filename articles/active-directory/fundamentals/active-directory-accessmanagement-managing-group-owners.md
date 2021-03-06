---
title: Hogyan lehet hozzáadni, vagy távolítsa el az Azure Active Directory csoporttulajdonosok |} A Microsoft Docs
description: Megtudhatja, hogyan hozzáadása vagy eltávolítása a csoportok tulajdonosainak Azure Active Directory használatával.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro
ms.openlocfilehash: f546ea5b5f9288849334d27cd1721f0c22fb8806
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46297775"
---
# <a name="how-to-add-or-remove-group-owners-in-azure-active-directory"></a>Hogyan: hozzáadása vagy eltávolítása a csoportok tulajdonosainak az Azure Active Directoryban
Az Azure Active Directory (Azure AD-) csoportok tulajdonosai és kezeli a csoport tulajdonosai. Csoporttulajdonosok rendeli hozzá egy csoportot és annak tagjait kezelheti az erőforrás tulajdonosa (rendszergazda). Csoporttulajdonosok lehetnek a csoport tagjai, nem szükséges. Miután a csoport tulajdonosa hozzá lett rendelve, csak egy erőforrás tulajdonosa hozzáadhat vagy eltávolíthat tulajdonosai.

Bizonyos esetekben a rendszergazda dönthet nem rendelhet hozzá a csoport tulajdonosával. Ebben az esetben a csoport tulajdonosa lesz. Tulajdonosok hozzárendelését is elvégezheti további tulajdonosok folyamatokhoz a csoporthoz, kivéve, ha korlátozott Ez a csoport beállításainál.

## <a name="add-an-owner-to-a-group"></a>Tulajdonos hozzáadása csoporthoz
További csoporttulajdonosok hozzáadása egy csoporthoz, az Azure AD használatával.

### <a name="to-add-a-group-owner"></a>Csoporttulajdonos hozzáadása
1. Jelentkezzen be a [az Azure portal](https://portal.azure.com) a címtár egy globális rendszergazdai fiók használatával.

2. Válassza ki **Azure Active Directory**, jelölje be **csoportok**, majd válassza ki a csoport, amelynek tulajdonosa hozzá szeretne (ebben a példában _mobileszköz-kezelési szabályzat – Nyugat-India_).

3. Az a **mobileszköz-kezelési szabályzat – Nyugat-India – áttekintés** lapon jelölje be **tulajdonosok**.

    ![Mobileszköz-kezelési szabályzat – Nyugat-India – áttekintés oldalra a tulajdonosok opció kiemelésével](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. A a **mobileszköz-kezelési szabályzat – Nyugat - tulajdonosok** lapon jelölje be **tulajdonosok hozzáadása**, és keressen rá, és válassza ki a felhasználót, hogy az új csoport tulajdonosa legyen, és válassza a **kiválasztása**.

    ![Mobileszköz-kezelési szabályzat – Nyugat - tulajdonosok lap Hozzáadás tulajdonosok lehetőséggel kiemelve](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Miután kiválasztotta az új tulajdonos, frissítheti a **tulajdonosok** lapon, és tekintse meg a nevét, a tulajdonosok listája hozzá.

## <a name="remove-an-owner-from-a-group"></a>Tulajdonos eltávolítása egy csoportból
Tulajdonos eltávolítása az Azure AD-csoport.

### <a name="to-remove-an-owner"></a>Tulajdonos eltávolítása
1. Jelentkezzen be a [az Azure portal](https://portal.azure.com) a címtár egy globális rendszergazdai fiók használatával.

2. Válassza ki **Azure Active Directory**, jelölje be **csoportok**, majd válassza ki a csoport, amelynek tulajdonosa hozzá szeretne (ebben a példában _mobileszköz-kezelési szabályzat – Nyugat-India_).

3. Az a **mobileszköz-kezelési szabályzat – Nyugat-India – áttekintés** lapon jelölje be **tulajdonosok**.

    ![Mobileszköz-kezelési szabályzat – Nyugat-India – áttekintés oldalra a tulajdonosok opció kiemelésével](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. Az a **mobileszköz-kezelési szabályzat – Nyugat - tulajdonosok** lapra, jelölje be a csoporttulajdonos eltávolítása, válassza a kívánt felhasználó **eltávolítása** a felhasználó adatai lap, és válassza a **Igen** megerősítéséhez a döntés.

    ![Az Eltávolítás opció kiemelésével felhasználó információi lap](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    Miután eltávolította a tulajdonos, visszatérhet a **tulajdonosok** lapon, és tekintse meg a nevét, a tulajdonosok listája lett távolítva.

## <a name="next-steps"></a>További lépések
- [Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal](active-directory-manage-groups.md)

- [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](../users-groups-roles/groups-settings-cmdlets.md)

- [Csoportok használata hozzáférések hozzárendelése az integrált SaaS-alkalmazások](../users-groups-roles/groups-saasapps.md)

- [Helyszíni identitások integrálása az Azure Active Directoryval](../hybrid/whatis-hybrid-identity.md)

- [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](../users-groups-roles/groups-settings-v2-cmdlets.md)

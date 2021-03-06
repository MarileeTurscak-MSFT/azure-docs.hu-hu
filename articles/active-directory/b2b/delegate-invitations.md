---
title: Az Azure Active Directory B2B együttműködés meghívók delegálása |} A Microsoft Docs
description: Az Azure Active Directory B2B együttműködés felhasználói tulajdonságok konfigurálható
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 6389a4987c590cd2d0f1dc648f9d003581102265
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984775"
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Az Azure Active Directory B2B együttműködés meghívók delegálása

Az Azure Active Directory (Azure AD) vállalatközi (B2B) együttműködés nem rendelkezik a globális rendszergazda meghívót küldhet. Ehelyett házirendekkel és a felhasználók számára, akiknek szerepkörök lehetővé számukra, hogy meghívók küldése meghívók delegálása. Vendégfelhasználói meghívókat delegálására egy fontos új lehetőség van a Vendég meghívója szerepkörön keresztül.

## <a name="guest-inviter-role"></a>A Vendégmeghívó szerepkörrel
A felhasználó a Vendégmeghívó szerepkörrel meghívók küldése hozzárendelheti azt. Nem lehet meghívók küldése a globális rendszergazdai szerepkör tagjává kell. Alapértelmezés szerint rendszeres felhasználók is hívhat meg a meghívás API-t, ha egy globális rendszergazdai le van tiltva a meghívót a normál felhasználók számára. A felhasználó is hívhat meg az API-t az Azure portal vagy a PowerShell használatával.

A következő példa bemutatja, hogyan használhatja a Powershellt a felhasználó hozzáadása a Vendégmeghívó szerepkörrel:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Szabályozza, ki küldhetnek meghívót

![Vezérelheti, hogyan hívhat meg](media/delegate-invitations/control-who-to-invite.png)

Az Azure AD B2B együttműködés Bérlői rendszergazda állíthatja be a következő meghívó házirendek:

- Meghívók kikapcsolása
- Csak rendszergazdák és a Vendégmeghívó szerepkörű felhasználók küldhetnek meghívót
- A rendszergazdák, a Vendégmeghívó szerepkörrel és tagok küldhetnek meghívót
- Minden felhasználó,-Vendégek esetén is küldhetnek meghívót

Alapértelmezés szerint a bérlők # 4 vannak beállítva. (Minden felhasználó, beleértve a vendégeket, meghívhatja a B2B-felhasználók.)

## <a name="next-steps"></a>További lépések

Az Azure AD B2B együttműködés a következő cikkekben talál:

- [Mi az az Azure AD B2B együttműködés?](what-is-b2b.md)
- [B2B együttműködés meghívó nélküli vendégfelhasználók hozzáadása](add-user-without-invite.md)
- [B2B-együttműködés felhasználó hozzáadása szerepkörhöz](add-guest-to-role.md)



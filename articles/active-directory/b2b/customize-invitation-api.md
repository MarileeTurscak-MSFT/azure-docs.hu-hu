---
title: Az Azure Active Directory B2B együttműködés API és testreszabás |} A Microsoft Docs
description: Az Azure Active Directory B2B együttműködés a vállalatokon átívelő kapcsolatok támogatása érdekében lehetővé teszi, hogy az üzleti partnerek szelektíven érhessék el a vállalati alkalmazásokat
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: reference
ms.date: 04/11/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 20c824da82d6e3e66bfa2d7447c8a9573cbdce69
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985813"
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Az Azure Active Directory B2B együttműködés API és testreszabás

Ossza meg velünk, hogy szeretné-e a meghívási folyamatot úgy, hogy a szervezetek számára a leginkább testre szabhatja a számos ügyfél volt. Az API-val, egyszerűen megteheti. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>A meghívó API képességeit
Az API-t a következő lehetőségeket biztosítja:

1. A külső felhasználó meghívása *bármely* e-mail-címét.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Testre szabhatja, ha azt szeretné, hogy a felhasználók elfogadják a meghívót után kerül.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Válassza a keresztül velünk a kapcsolatot a standard meghívó e-mail küldése

    ```
    "sendInvitationMessage": true
    ```

  egy üzenet, amely testre szabható címzettnek

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. Másolatot kap majd: személyek meg szeretné tartani a kapcsolatos a figyelmét a közreműködő hurokba került.

5. Vagy teljes mértékben testre szabhatja a meghívó és a regisztrációs munkafolyamat nem, ha az Azure AD-n keresztül értesítéseket küldeni.

    ```
    "sendInvitationMessage": false
    ```

  Ebben az esetben vissza a beváltási URL-címe az API-val, amely ágyazható be egy e-mail sablon, IM vagy más tetszőleges terjesztési mód.

6. Végül ha Ön rendszergazda, ha szeretné, a felhasználó nevében meghívása.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Engedélyezési modellje
Az API-t a következő hitelesítési mód környezetben is futtatható:

### <a name="app--user-mode"></a>Alkalmazás + felhasználói mód
Ebben a módban személy, aki használja az API-t kell B2B meghívók lehet létrehozni a engedélyekkel kell rendelkeznie.

### <a name="app-only-mode"></a>Egyetlen alkalmazás mód
Alkalmazás csak a környezetben az alkalmazás a meghívás sikeres User.Invite.All hatóköre van szüksége.

További információkért tekintse meg: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Már lehetséges a PowerShell használatával adja hozzá, és a egy szervezet külső felhasználók könnyedén meghívása. Hozzon létre egy meghívást arra parancsmag segítségével:

```
New-AzureADMSInvitation
```

Használhatja a következő beállításokat:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Emellett megtekinthet a meghívó API-referencia [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>További lépések

- [Mi az az Azure AD B2B együttműködés?](what-is-b2b.md)
- [Az elemek a B2B együttműködés meghívó e-mail](invitation-email-elements.md)
- [B2B együttműködés vendégmeghívás beváltása](redemption-experience.md)
- [Adja hozzá a B2B-együttműködés felhasználók meghívás nélkül](add-user-without-invite.md)


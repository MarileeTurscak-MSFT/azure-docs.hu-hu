---
title: fájl belefoglalása
description: fájl belefoglalása
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7f086c44abf6c9002c47904dc722294e046528f7
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293469"
---
## <a name="test-your-app"></a>Az alkalmazás tesztelése

1. Futtassa az eszközt vagy emulátort a kódot.

2. Próbálja meg jelentkezzen be egy Azure Active Directory-fiók (munkahelyi vagy iskolai fiók) vagy a Microsoft-fiókkal (live.com, outlook.com). 

    ![Az alkalmazás tesztelése](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Adja meg a felhasználónevet és jelszót](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="consent-to-your-app"></a>Hozzájárulás az alkalmazáshoz
Először egy felhasználó bejelentkezik az alkalmazás kérni fogja, beleegyezik abba, hogy az engedélyeket az alkalmazás igényeinek megfelelően, amint az itt látható: 

![Adja meg az Ön hozzájárul az alkalmazás-hozzáférés](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="success"></a>Sikerült!
Után jelentkezzen be a & beleegyezik, az alkalmazás a Microsoft Graph API-válasz jelenik meg. Ez a hívás, hogy a **/me** végpont pedig visszaadja a [felhasználói profil](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_get). Más Microsoft Graph-végpontok listáját lásd: [Microsoft Graph API fejlesztői dokumentáció](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="scopes-and-delegated-permissions"></a>Hatókörök és delegált engedélyek

A Microsoft Graph API megköveteli a *User.Read* hatókörrel, hogy a felhasználói profil olvasása. Ebben a hatókörben automatikusan szerepel a minden alkalmazás, amely regisztrálva van az alkalmazásregisztrációs portálon. Más API-k további hatókörökkel lesz szükség. Ha például a Microsoft Graph API megköveteli a *Calendars.Read* hatókörrel, hogy a felhasználók naptáraiban listázása. 

A felhasználók naptáraiban eléréséhez, adja hozzá a *Calendars.Read* delegált engedéllyel az alkalmazás regisztrációs adatok. Adja hozzá a *Calendars.Read* a hatókört a `acquireTokenSilent` hívja. 

>[!NOTE]
>Ha megváltoztatja az alkalmazás regisztrációját a felhasználók felkérheti további beleegyezést.

<!--end-collapse-->

[!INCLUDE [Help and support](active-directory-develop-help-support-include.md)]

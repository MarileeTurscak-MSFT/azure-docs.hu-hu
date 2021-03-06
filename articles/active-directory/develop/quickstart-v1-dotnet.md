---
title: A felhasználók és a Microsoft Graph API hívása egy .NET (WPF) asztali alkalmazásból |} A Microsoft Docs
description: Megtudhatja, hogyan integrálható az Azure AD-bejelentkezés Windows asztali .NET-alkalmazás létrehozása, és meghívja az Azure AD által védett API-k az OAuth 2.0 használatával.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f9389a7c0e80f075c01f2236fa1bdf9dc9544ac6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987441"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Gyors útmutató: A felhasználók Bejelentkeztetéséhez és a Microsoft Graph API hívása egy .NET (WPF) asztali alkalmazásból

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

.NET natív ügyfelek védett erőforrások elérését igénylő az Azure Active Directory (Azure AD) az Active Directory Authentication Library (ADAL) biztosít. Adal-t egyszerűen az alkalmazás hozzáférési. 

Ebből a gyorsútmutatóból megtudhatja, hogyan hozhat létre egy .NET WPF feladatlista-alkalmazás, amely:

* Lekérdezi hozzáférési jogkivonatokat az Azure AD Graph API-t az OAuth 2.0 hitelesítési protokoll hívásakor.
* A felhasználók számára egy adott aliasú könyvtár keres.
* Tünetek felhasználók ki.

Hozhat létre a teljes, működő alkalmazást, a következőket kell tennie:

1. Regisztrálja az alkalmazást az Azure ad-ben.
2. Telepítse és konfigurálja az adal-t.
3. Adal-t használó tokenekhez Azure AD-ből való.

## <a name="prerequisites"></a>Előfeltételek

Első lépésként hajtsa végre az Előfeltételek:

* [Töltse le az alkalmazás skeleton](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) vagy [töltse le az elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)
* Amennyiben felhasználók létrehozása, és regisztrálni egy alkalmazást az Azure AD-bérlővel rendelkezik. Ha még nem rendelkezik egy bérlő [megtudhatja, hogyan tehet szert egy](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>1. lépés: A DirectorySearcher alkalmazás regisztrálása

Ahhoz, hogy az alkalmazást, hogy a jogkivonatok lekérésére, az alkalmazás regisztrálása az Azure AD-bérlője, és engedélyezi azt az Azure AD Graph API eléréséhez:

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Az a felső menüsávon válassza ki a fiókját és a **Directory** menüben válassza ki az Active Directory-bérlőt, az alkalmazásokat regisztrálni szeretne.
3. Válassza ki a **minden szolgáltatás** a bal oldali navigációs válassza **Azure Active Directory**.
4. A **alkalmazásregisztrációk**, válassza a **Hozzáadás**.
5. Kövesse az utasításokat, és hozzon létre egy új **natív** ügyfélalkalmazás.
    * A **neve** az alkalmazás ismerteti az alkalmazást a végfelhasználók számára
    * A **átirányítási URI-t** sémát és karakterlánc kombinációja, amely az Azure AD a jogkivonatválaszok visszaadására fog használni. Adja meg például egy adott értéket az alkalmazás `http://DirectorySearcher`.

6. Miután végrehajtotta a regisztráció, AAD rendeli az alkalmazást egy egyedi azonosítóját. Ez az érték kell a következő szakaszokban, ezért másolja ki az alkalmazás oldaláról.
7. Az a **beállítások** lapon a **szükséges engedélyek** válassza **Hozzáadás**. Válassza ki **Microsoft Graph** az API-ként, majd a **delegált engedélyek** adja hozzá a **címtáradatok olvasása** engedéllyel. Ez az engedély beállítása lehetővé teszi, hogy az alkalmazás a felhasználók számára a Graph API lekérdezéséhez.

## <a name="step-2-install-and-configure-adal"></a>2. lépés: Telepítse és konfigurálja az adal-t

Most, hogy egy alkalmazás az Azure ad-ben, telepítheti az adal-t és az identitással kapcsolatos kód írása. Az ADAL-kommunikálni az Azure ad-ben meg kell adnia azt az alkalmazás regisztrációs bizonyos információkat.

1. Kezdje az ADAL hozzáadásával a `DirectorySearcher` projektet, a Package Manager konzol segítségével.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

1. Az a `DirectorySearcher` projektben nyissa meg `app.config`.
1. Cserélje le az értékeket az elemek a `<appSettings>` szakasz az értékekhez adjon meg az Azure Portalra. A kód ezeket az értékeket hivatkozik, amikor az adal-t használ.
  * A `ida:Tenant` a tartomány az Azure AD-bérlő, például contoso.onmicrosoft.com
  * A `ida:ClientId` a portálról kimásolt az alkalmazás ügyfél-azonosítója.
  * A `ida:RedirectUri` a portál regisztrált átirányítási URL-címe.

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>3. lépés: Használja az ADAL-ra tokenekhez Azure AD-ből

Az alapelv ADAL mögött, hogy minden alkalommal, amikor az alkalmazás-hozzáférési jogkivonat van szüksége, az alkalmazás egyszerűen meghívja `authContext.AcquireTokenAsync(...)`, ADAL végzi.

1. Az a `DirectorySearcher` projektben nyissa meg `MainWindow.xaml.cs`.
1. Keresse meg a `MainWindow()` metódust. 
1. Az alkalmazás inicializálása `AuthenticationContext` -ADAL által elsődleges osztály. `AuthenticationContext` van, ahol adja át az adal-t a koordináták kommunikálni az Azure ad-ben, és mondja el, hogyan gyorsítótárazza a jogkivonatok szükséges.

    ```csharp
    public MainWindow()
    {
        InitializeComponent();

        authContext = new AuthenticationContext(authority, new FileCache());

        CheckForCachedToken();
    }
    ```

1. Keresse meg a `Search(...)` metódus, která bude volána választva a felhasználó a **keresési** gombra az alkalmazás felhasználói felületén. Ez a módszer egy GET kéréssel teszi az Azure AD Graph API lekérdezéséhez, a felhasználók számára, akiknek UPN kezdődik-e a megadott keresési kifejezés.
1. Lekérdezése a Graph API, adjon meg egy access_token a a `Authorization` a kérelem fejlécében azaz ahol ADAL érhető el.

    ```csharp
    private async void Search(object sender, RoutedEventArgs e)
    {
        // Validate the Input String
        if (string.IsNullOrEmpty(SearchText.Text))
        {
            MessageBox.Show("Please enter a value for the To Do item name");
            return;
        }

        // Get an Access Token for the Graph API
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
            UserNameLabel.Content = result.UserInfo.DisplayableId;
            SignOutButton.Visibility = Visibility.Visible;
        }
        catch (AdalException ex)
        {
            // An unexpected error occurred, or user canceled the sign in.
            if (ex.ErrorCode != "access_denied")
                MessageBox.Show(ex.Message);

            return;
        }

        ...
    }
    ```

    Ha az alkalmazás tokent kér meghívásával `AcquireTokenAsync(...)`, ADAL megkísérli egy token vissza a felhasználói hitelesítő adatok kérése nélkül.
    * Ha adal-t, hogy a felhasználónak kell-e jogkivonat beszerzéséhez jelentkezzen be, azt fog egy bejelentkezési párbeszédpanel megjelenítése, a felhasználó hitelesítő adatainak gyűjtése és sikeres hitelesítést követően jogkivonat. 
    * Ha adal-t nem adható vissza a jogkivonat bármilyen okból, hogy kivételt fogja kijelezni egy `AdalException`.

1. Figyelje meg, hogy a `AuthenticationResult` az objektum tartalmaz egy `UserInfo` objektum, amely használható az alkalmazás szükségessé kapcsolatos információk összegyűjtéséhez. Az a DirectorySearcher `UserInfo` szabható testre az alkalmazás felhasználói felületén, a felhasználói azonosítóval.
1. Amikor a felhasználó kiválasztja az **Kijelentkezés** gombra kattint, győződjön meg arról, hogy a következőre irányuló hívás `AcquireTokenAsync(...)` kéri a felhasználót, hogy jelentkezzen be. Könnyedén megteheti ezt az adal-t a tokengyorsítótárral jelölésének törlésével:

    ```csharp
    private void SignOut(object sender = null, RoutedEventArgs args = null)
    {
        // Clear the token cache
        authContext.TokenCache.Clear();

        ...
    }
    ```

    Ha a felhasználónak nem szabad a **Kijelentkezés** gomb, biztosítani kell a felhasználói munkamenet a következő biztonsági a DirectorySearcher futnak. Az alkalmazás indításakor adal-t a jogkivonatok gyorsítótárát egy meglévő token ellenőrizheti és a felhasználói felületen ennek megfelelően frissülnek.

1. Az a `CheckForCachedToken()` módszer, egy másik hívás `AcquireTokenAsync(...)`, ezúttal ad át a `PromptBehavior.Never` paraméter. `PromptBehavior.Never` adal-t jelzi, hogy a felhasználó nem kéri a bejelentkezéshez, és az adal-t kell helyette kivételt, ha nem tudja jogkivonat.

    ```csharp
    public async void CheckForCachedToken() 
    {
        // As the application starts, try to get an access token without prompting the user. If one exists, show the user as signed in.
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "user_interaction_required")
            {
                // An unexpected error occurred.
                MessageBox.Show(ex.Message);
            }

            // If user interaction is required, proceed to main page without singing the user in.
            return;
        }

        // A valid token is in the cache
        SignOutButton.Visibility = Visibility.Visible;
        UserNameLabel.Content = result.UserInfo.DisplayableId;
    }
    ```

Gratulálunk! Most már rendelkezik egy működő, felhasználók hitelesítése, biztonságos webes API-kat, az OAuth 2.0 használatával, és alapvető információkat szeretne a felhasználó első .NET WPF-alkalmazás. Ha még nem tette, most már az az idő, az egyes felhasználók a bérlő feltölti. A DirectorySearcher alkalmazás futtatásához, és jelentkezzen be egy ezen felhasználók. Keresse meg a más felhasználók, az egyszerű felhasználónév alapján. Zárja be az alkalmazást, és újra futtathatja. Figyelje meg, hogyan a felhasználói munkamenet érintetlen marad. Jelentkezzen ki, és a egy másik felhasználóként jelentkezzen be.

Adal-t egyszerűen ezeket a gyakori identitáskezelési funkciókat építhet be az alkalmazás. Ez gondoskodik a dirty munka, beleértve a gyorsítótár kezeléséhez, OAuth-protokoll támogatása, bemutatása a felhasználó egy bejelentkezési felhasználói felület, lejárt, és további frissítése. Feltétlenül szükséges tudnia, csak egyetlen API hívással `authContext.AcquireTokenAsync(...)`.

Ha, olvassa el az elkészült mintát (a konfigurációs értékek) nélkül [a Githubon](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).

## <a name="next-steps"></a>További lépések

Ismerje meg, hogyan védheti meg a webes API-t OAuth 2.0 tulajdonosi hozzáférési jogkivonatok használatával.
> [!div class="nextstepaction"]
> [.NET webes API Azure AD-vel biztonságos >>](quickstart-v1-dotnet-webapi.md)

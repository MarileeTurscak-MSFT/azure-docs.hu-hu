---
title: Jelentkezzen be egy Azure Active Directory-fiókot az Azure Active Directory B2C-vel egyéni szabályzatok használatával beállítása |} A Microsoft Docs
description: Jelentkezzen be az Azure Active Directory-fiókkal az Azure Active Directory B2C használatával egyéni szabályzatok beállításához.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 77f552f94df6e003f6504991f9617a080acdd768
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47180512"
---
# <a name="set-up-sign-in-with-an-azure-active-directory-account-using-custom-policies-in-azure-active-directory-b2c"></a>Állítsa be a bejelentkezést egy Azure Active Directory-fiókot az Azure Active Directory B2C-vel egyéni szabályzatok használatával 

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ez a cikk bemutatja, hogyan bejelentkezés engedélyezése a felhasználók számára a szervezet Azure Active Directory (Azure AD) használatával [egyéni szabályzatok](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C-ben.

## <a name="prerequisites"></a>Előfeltételek

Hajtsa végre a [az Azure Active Directory B2C-vel egyéni szabályzatok – első lépések](active-directory-b2c-get-started-custom.md).

## <a name="register-an-application"></a>Egy alkalmazás regisztrálása

Ahhoz, hogy jelentkezzen be egy meghatározott felhasználók számára az Azure AD a szervezeten belül, kell regisztrálni egy alkalmazást a szervezeti Azure AD-bérlővel.

>[!NOTE]
>`Contoso.com` a szervezeti használt Azure AD-bérlő és `fabrikamb2c.onmicrosoft.com` használja a következő utasítások az Azure AD B2C-bérlő.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Győződjön meg arról, hogy használja a könyvtárat, amely tartalmazza a szervezeti Azure AD-bérlő (contoso.com) kattintva a **címtár és előfizetés-szűrő** a felső menüben, és a könyvtár, amely tartalmazza a bérlő kiválasztása.
3. Válasszon **minden szolgáltatás** az Azure Portalon, és majd keresse meg és válassza a bal felső sarkában lévő **alkalmazásregisztrációk**.
4. Válassza az **Új alkalmazás regisztrálása** elemet.
5. Adjon meg egy nevet az alkalmazásnak. Például: `Azure AD B2C App`.
6. Az a **alkalmazástípus**válassza `Web app / API`.
7. A a **bejelentkezési URL-**, adja meg a következő URL-cím csupa kisbetűvel, ahol `your-tenant` váltja fel az Azure AD B2C-bérlő (fabrikamb2c.onmicrosoft.com) nevére:

    ```
    https://yourtenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp
    ```

8. Kattintson a **Create** (Létrehozás) gombra. Másolás a **Alkalmazásazonosító** későbbi felhasználás céljából.
9. Válassza ki az alkalmazást, és válassza **beállítások**.
10. Válassza ki **kulcsok**, adja meg a kulcs leírása, és válassza ki az időtartamot, majd kattintson a **mentése**. Másolja későbbi felhasználás jelenik meg a kulcs értékét.

## <a name="create-a-policy-key"></a>Hozzon létre egy házirendjének kulcsa

Kell tárolni, amelyet az Azure AD B2C-bérlő az alkalmazáskulcsot.

1. Győződjön meg arról, hogy használja az Azure AD B2C-bérlő kattintva tartalmazó könyvtárba a **címtár és előfizetés-szűrő** a felső menüben, és a könyvtár, amely tartalmazza a bérlő kiválasztása.
2. Válasszon **minden szolgáltatás** az Azure Portalon, és majd keresse meg és válassza a bal felső sarkában lévő **Azure AD B2C-vel**.
3. Az Áttekintés oldalon válassza ki a **identitás-kezelőfelületi keretrendszer – előzetes verzió**.
4. Válassza ki **Szabályzatbejegyzések** majd **Hozzáadás**.
5. A **beállítások**, válassza a `Manual`.
6. Adjon meg egy **neve** a házirend-kulcs. Például: `ContosoAppSecret`.  Az előtag `B2C_1A_` automatikusan hozzáadódik a kulcs neve.
7. A **titkos**, adja meg a korábban feljegyzett alkalmazáskulcsot.
8. A **kulcshasználat**válassza `Signature`.
9. Kattintson a **Create** (Létrehozás) gombra.

## <a name="add-a-claims-provider"></a>A jogcímeket szolgáltató hozzáadása

Ha azt szeretné, hogy a felhasználók számára, hogy jelentkezzen be az Azure AD használatával, definiálhatja egy jogcímszolgáltatótól, amely az Azure AD B2C-végponton keresztül képes kommunikálni az Azure AD szeretne. A végpont ellenőrzése, hogy egy adott felhasználó rendelkezik hitelesítése Azure AD B2C által használt jogcímeket biztosít. 

Meghatározhatja az Azure AD egy jogcímszolgáltatótól, az Azure AD-hozzáadásával a **ClaimsProvider** elem a bővítmény fájlban a szabályzat.

1. Nyissa meg a *TrustFrameworkExtensions.xml*.
2. Keresse meg a **ClaimsProviders** elemet. Ha nem létezik, adja hozzá a legfelső szintű elem alatt.
3. Vegyen fel egy új **ClaimsProvider** módon:

    ```XML
    <ClaimsProvider>
      <Domain>Contoso</Domain>
      <DisplayName>Login using Contoso</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="ContosoProfile">
          <DisplayName>Contoso Employee</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <OutputTokenFormat>JWT</OutputTokenFormat>
          <Metadata>
            <Item Key="METADATA">https://login.windows.net/your-tenant/.well-known/openid-configuration</Item>
            <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="response_types">id_token</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Alatt a **ClaimsProvider** elem, frissítse az értéket a **tartomány** egyedi értéket, hogy megkülönböztesse a más identitásszolgáltatókkal is használható.
5. Alatt a **ClaimsProvider** elem, frissítse az értéket a **DisplayName** , egy rövid nevet a jogcímszolgáltató. Ez az érték jelenleg nem használja.

### <a name="update-the-technical-profile"></a>A technikai profil frissítése

Egy token beszerzése az Azure AD-végpontról, adja meg a protokoll, amely az Azure AD B2C-vel való kommunikációhoz, az Azure ad-vel kell használnia kell. Ez a **TechnicalProfile** eleme **ClaimsProvider**.

1. Frissítés azonosítója a **TechnicalProfile** elemet. Ez az azonosító segítségével tekintse meg a házirend egyéb részei a technikai profil.
2. Frissítse az értéket a **DisplayName**. Ez az érték a Bejelentkezés gombra a bejelentkezési képernyőn jelennek meg.
3. Frissítse az értéket a **leírás**.
4. Azure ad-ben az OpenID Connect protokollt használja, ezért ügyeljen arra, hogy az érték **protokoll** van `OpenIdConnect`.
5. Állítsa a **METAADATOK** való `https://login.windows.net/your-tenant/.well-known/openid-configuration`, ahol `your-tenant` van az Azure AD-bérlő neve (contoso.com).
6. Nyissa meg a böngészőt, és nyissa meg a **METAADATOK** imént frissített, keresse meg az URL-címet a **kibocsátó** objektumot, másolja és illessze be az értéket értéke **ProviderName** az XML-fájlban.
8. Állítsa be **client_id** és **IdTokenAudience** való az alkalmazás regisztrációja Alkalmazásazonosítója.
9. A **CryptograhicKeys**, frissítse az értéket a **StorageReferenceId** meghatározott házirend kulcshoz. Például: `ContosoAppSecret`.

### <a name="upload-the-extension-file-for-verification"></a>Az ellenőrzéshez a bővítmény-fájl feltöltése

Már konfigurálta a szabályzatot, hogy az Azure AD B2C-vel képes kommunikálni az Azure AD-címtárhoz. A szabályzat csak, győződjön meg arról, hogy minden problémát, amennyiben nem rendelkezik a bővítmény fájlt töltsön fel.

1. Az a **egyéni szabályzatok** az Azure AD B2C-bérlő, válassza a lap **szabályzat feltöltése**.
2. Engedélyezése **szabályzat felülírása, ha létezik**, és keresse meg és válassza ki a *TrustFrameworkExtensions.xml* fájlt.
3. Kattintson a **Feltöltés** gombra.

## <a name="register-the-claims-provider"></a>A jogcím-szolgáltató regisztrálása

Ezen a ponton az identitásszolgáltató be lett állítva, de nem érhető el az összes regisztrálási-regisztrálási vagy bejelentkezési képernyőt. Elérhető legyen, hozzon létre egy meglévő sablon felhasználói interakciósorozat másolatát, és ezután módosítsa, hogy az Azure ad-ben identitásszolgáltató is rendelkezik:

1. Nyissa meg a *TrustFrameworkBase.xml* az alapszintű csomag fájlt.
2. Keresse meg és másolja ki a teljes tartalmát a **UserJourney** , amely tartalmazza az elem `Id="SignUpOrSignIn"`.
3. Nyissa meg a *TrustFrameworkExtensions.xml* , és keresse meg a **UserJourneys** elemet. Ha az elem nem létezik, adjon hozzá egyet.
4. Illessze be a teljes tartalmát a **UserJourney** gyermekeként kimásolt elem a **UserJourneys** elemet.
5. Nevezze át a felhasználói út azonosítója. Például: `SignUpSignInContoso`.

### <a name="display-the-button"></a>A gomb megjelenítése

A **hiányzik a ClaimsProviderSelection** elem ehhez hasonló regisztrálási-regisztrálási vagy bejelentkezési képernyőn egy identitás szolgáltató a gombhoz. Ha hozzáad egy **hiányzik a ClaimsProviderSelection** elem az Azure ad-ben, egy új gomb megjelenik-e, amikor egy felhasználó hajtanak végre az oldalon.

1. Keresse meg a **OrchestrationStep** , amely tartalmazza az elem `Order="1"` az Ön által létrehozott felhasználói interakciósorozat.
2. A **ClaimsProviderSelects**, adja hozzá a következő elemet. Állítsa az értékét **TargetClaimsExchangeId** egy megfelelő értéket, például a `ContosoExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>A gomb összekapcsolása egy műveletet

Most, hogy egyetlen helyen, amelyekkel hozzákapcsolhatja egy műveletet kell. A művelet, ebben az esetben pedig az Azure AD B2C-vel való kommunikációhoz, az Azure ad-vel fogadhatnak jogkivonatot. A gomb összekapcsolása egy műveletet az Azure ad-ben jogcím-szolgáltatói összekapcsolásának a technikai profil:

1. Keresse meg a **OrchestrationStep** tartalmazó `Order="2"` a felhasználói interakciósorozatban szereplő.
2. Adja hozzá a következő **ClaimsExchange** gondoskodik róla, hogy ugyanazt az értéket használt elem **azonosító** során használt **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```
    
    Frissítse az értéket a **TechnicalProfileReferenceId** , a **azonosító** a korábban létrehozott technikai profil. Például: `ContosoProfile`.

3. Mentse a *TrustFrameworkExtensions.xml* fájlt, és töltse fel újra az ellenőrzéshez.

## <a name="update-and-test-the-relying-party-file"></a>Frissítse és a függő entitás fájl tesztelése

Frissítse a függő entitásonkénti (RP) fájl, amely az Ön által létrehozott felhasználói interakciósorozat kezdeményezi.

1. Készítsen másolatot *SignUpOrSignIn.xml* a munkakönyvtárban, és nevezze át. Például nevezze át, hogy *SignUpSignInContoso.xml*.
2. Nyissa meg az új fájlt, és frissítse az értéket, a **PolicyId** az attribútum **TrustFrameworkPolicy** egyedi értékkel. Például: `SignUpSignInContoso`.
3. Frissítse az értéket a **PublicPolicyUri** URI-a szabályzat. Ha például`http://contoso.com/B2C_1A_signup_signin_contoso`
4. Frissítse az értéket, a **hivatkozásazonosító** attribútum **DefaultUserJourney** megfelelően (SignUpSignInContoso) létrehozott új felhasználói interakciósorozat azonosítója.
5. Mentse a módosításokat, a fájl feltöltéséhez, tesztelje azt megnyitásával, és kattintson a **Futtatás most**.


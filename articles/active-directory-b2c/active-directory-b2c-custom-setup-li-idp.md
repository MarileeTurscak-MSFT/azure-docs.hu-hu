---
title: Jelentkezzen be a LinkedIn-fiók az Azure Active Directory B2C-vel egyéni szabályzatok használatával beállítása |} A Microsoft Docs
description: Állítson be egy Google-fiókkal az Azure Active Directory B2C-vel egyéni szabályzatok használatával.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4817ad918af66080cec1faead96c6c9448387556
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181210"
---
# <a name="set-up-sign-in-with-a-linkedin-account-using-custom-policies-in-azure-active-directory-b2c"></a>Állítsa be a bejelentkezést a LinkedIn-fiók az Azure Active Directory B2C-vel egyéni szabályzatok használatával

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ez a cikk bemutatja, hogyan bejelentkezés engedélyezése a felhasználók számára a LinkedIn-fiók használatával [egyéni szabályzatok](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C-ben.

## <a name="prerequisites"></a>Előfeltételek

- Hajtsa végre a [az Azure Active Directory B2C-vel egyéni szabályzatok – első lépések](active-directory-b2c-get-started-custom.md).
- Ha a LinkedIn-fiók már nincs, hozzon létre egyet, a [LinkedIn a regisztrációs oldalra](https://www.linkedin.com/start/join).
- A LinkedIn-alkalmazás az alkalmazást képviselő 80-as X 80-as pixeles embléma kép megadását igényli.

## <a name="create-an-application"></a>Alkalmazás létrehozása

LinkedIn használni Identitásszolgáltatóként az Azure AD B2C-ben, meg kell a LinkedIn-alkalmazás létrehozása.

1. Jelentkezzen be a [LinkedIn Alkalmazáskezelés](https://www.linkedin.com/secure/developer?newapp=) a LinkedIn-fiók hitelesítő adataival.
2. Válassza ki **-alkalmazás létrehozása**.
3. Adja meg a **cég neve**, egy **alkalmazásnév**, és a egy **alkalmazásleírás**.
4. Töltse fel a **alkalmazás emblémája** létrehozott.
5. Válasszon egy **alkalmazások által használható** a megadott listáról.
6. A **webhely URL-címe**, adja meg `https://your-tenant.b2clogin.com`.  Cserélje le `your-tenant` az Azure AD B2C-bérlő nevével. Ha például contoso.b2clogin.com.
7. Adja meg a **üzleti E-mail** cím és **munkahelyi telefon** számát.
8. A lap alján, olvassa el és fogadja el a használati feltételeket, és válassza ki **küldés**.
9. Válassza ki **hitelesítési**, és jegyezze fel a **ügyfél-azonosító** és **titkos Ügyfélkód** értékeket későbbi használatra.
10. A **átirányítási URL-címek engedélyezett**, adja meg `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp`. Cserélje le `your-tenant-name` a bérlő nevével. Kisbetűk használhatók, ha akkor is, ha a bérlő Azure AD B2C-ben nagybetűk van definiálva, írja be a bérlő nevét kell. 
11. Válassza ki **frissítés**.
12. Válassza ki **beállítások**, módosítsa a **az alkalmazás állapota** való **élő**, majd válassza ki **frissítés**.

## <a name="create-a-policy-key"></a>Hozzon létre egy házirendjének kulcsa

Az ügyfél titkos kulcsát, az Azure AD B2C-bérlő korábban rögzített tárolni kívánt.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).
2. Győződjön meg arról, hogy használja az Azure AD B2C-bérlő kattintva tartalmazó könyvtárba a **címtár és előfizetés-szűrő** a felső menüben, és a könyvtár, amely tartalmazza a bérlő kiválasztása.
3. Válasszon **minden szolgáltatás** az Azure Portalon, és majd keresse meg és válassza a bal felső sarkában lévő **Azure AD B2C-vel**.
4. Az Áttekintés oldalon válassza ki a **identitás-kezelőfelületi keretrendszer – előzetes verzió**.
5. Válassza ki **Szabályzatbejegyzések** majd **Hozzáadás**.
6. A **beállítások**, válassza a `Manual`.
7. Adjon meg egy **neve** a házirend-kulcs. Például: `LinkedInSecret`. Az előtag `B2C_1A_` automatikusan hozzáadódik a kulcs neve.
8. A **titkos**, adja meg a korábban feljegyzett ügyfélkulcsot.
9. A **kulcshasználat**válassza `Signature`.
10. Kattintson a **Create** (Létrehozás) gombra.

## <a name="add-a-claims-provider"></a>A jogcímeket szolgáltató hozzáadása

Ha azt szeretné, hogy a felhasználók a LinkedIn-fiók használatával, definiálhatja a fiók, amely az Azure AD B2C-végponton keresztül kommunikálhat egy jogcímszolgáltatótól szeretne. A végpont ellenőrzése, hogy egy adott felhasználó rendelkezik hitelesítése Azure AD B2C által használt jogcímeket biztosít. 

Meghatározhatja a LinkedIn-fiók egy jogcímszolgáltatótól, adja hozzá, hogy a **ClaimsProviders** elem a bővítmény fájlban a szabályzat.

1. Nyissa meg a *TrustFrameworkExtensions.xml*.
2. Keresse meg a **ClaimsProviders** elemet. Ha nem létezik, adja hozzá a legfelső szintű elem alatt.
3. Vegyen fel egy új **ClaimsProvider** módon:

    ```xml
    <ClaimsProvider>
      <Domain>linkedin.com</Domain>
      <DisplayName>LinkedIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LinkedIn-OAUTH">
          <DisplayName>LinkedIn</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">linkedin</Item>
            <Item Key="authorization_endpoint">https://www.linkedin.com/oauth/v2/authorization</Item>
            <Item Key="AccessTokenEndpoint">https://www.linkedin.com/oauth/v2/accessToken</Item>
            <Item Key="ClaimsEndpoint">https://api.linkedin.com/v1/people/~:(id,first-name,last-name,email-address,headline)</Item>
            <Item Key="ClaimsEndpointAccessTokenName">oauth2_access_token</Item>
            <Item Key="ClaimsEndpointFormatName">format</Item>
            <Item Key="ClaimsEndpointFormat">json</Item>
            <Item Key="scope">r_emailaddress r_basicprofile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your LinkedIn application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_LinkedInSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="emailAddress" />
            <!--<OutputClaim ClaimTypeReferenceId="jobTitle" PartnerClaimType="headline" />-->
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="linkedin.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Cserélje le a értékét **client_id** a korábban feljegyzett ügyfél-azonosító.
5. Mentse a fájlt.

### <a name="upload-the-extension-file-for-verification"></a>Az ellenőrzéshez a bővítmény-fájl feltöltése

Már konfigurálta a szabályzatot, hogy az Azure AD B2C-vel képes kommunikálni a LinkedIn-fiók. A szabályzat csak, győződjön meg arról, hogy minden problémát, amennyiben nem rendelkezik a bővítmény fájlt töltsön fel.

1. Az a **egyéni szabályzatok** az Azure AD B2C-bérlő, válassza a lap **szabályzat feltöltése**.
2. Engedélyezése **szabályzat felülírása, ha létezik**, és keresse meg és válassza ki a *TrustFrameworkExtensions.xml* fájlt.
3. Kattintson a **Feltöltés** gombra.

## <a name="register-the-claims-provider"></a>A jogcím-szolgáltató regisztrálása

Ezen a ponton az identitásszolgáltató be lett állítva, de nem érhető el az összes regisztrációs vagy bejelentkezési képernyőt. Elérhető legyen, hozzon létre egy meglévő sablon felhasználói interakciósorozat másolatát, és ezután módosítsa, hogy a LinkedIn-identitásszolgáltató is rendelkezik.

1. Nyissa meg a *TrustFrameworkBase.xml* az alapszintű csomag fájlt.
2. Keresse meg és másolja ki a teljes tartalmát a **UserJourney** , amely tartalmazza az elem `Id="SignUpOrSignIn"`.
3. Nyissa meg a *TrustFrameworkExtensions.xml* , és keresse meg a **UserJourneys** elemet. Ha az elem nem létezik, adjon hozzá egyet.
4. Illessze be a teljes tartalmát a **UserJourney** gyermekeként kimásolt elem a **UserJourneys** elemet.
5. Nevezze át a felhasználói út azonosítója. Például: `SignUpSignInLinkedIn`.

### <a name="display-the-button"></a>A gomb megjelenítése

A **hiányzik a ClaimsProviderSelection** elem ehhez hasonló regisztrálási vagy bejelentkezési képernyőn egy identitás szolgáltató a gombhoz. Ha hozzáad egy **hiányzik a ClaimsProviderSelection** elem a LinkedIn-fiók, egy új gomb megjelenik-e, amikor egy felhasználó hajtanak végre az oldalon.

1. Keresse meg a **OrchestrationStep** , amely tartalmazza az elem `Order="1"` az Ön által létrehozott felhasználói interakciósorozat.
2. A **ClaimsProviderSelects**, adja hozzá a következő elemet. Állítsa az értékét **TargetClaimsExchangeId** egy megfelelő értéket, például a `LinkedInExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>A gomb összekapcsolása egy műveletet

Most, hogy egyetlen helyen, amelyekkel hozzákapcsolhatja egy műveletet kell. A művelet, ebben az esetben pedig az Azure AD B2C-vel való kommunikációhoz a LinkedIn-fiók kaphat jogkivonatot.

1. Keresse meg a **OrchestrationStep** tartalmazó `Order="2"` a felhasználói interakciósorozatban szereplő.
2. Adja hozzá a következő **ClaimsExchange** gondoskodik róla, hogy ugyanazt az értéket használt elem **azonosító** során használt **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    ```
    
    Frissítse az értéket a **TechnicalProfileReferenceId** , a **azonosító** a korábban létrehozott technikai profil. Például: `LinkedIn-OAUTH`.

3. Mentse a *TrustFrameworkExtensions.xml* fájlt, és töltse fel újra az ellenőrzéshez.

## <a name="update-and-test-the-relying-party-file"></a>Frissítse és a függő entitás fájl tesztelése

Frissítse a függő entitásonkénti (RP) fájl, amely az Ön által létrehozott felhasználói interakciósorozat kezdeményezi.

1. Készítsen másolatot *SignUpOrSignIn.xml* a munkakönyvtárban, és nevezze át. Például nevezze át, hogy *SignUpSignInLinkedIn.xml*.
2. Nyissa meg az új fájlt, és frissítse az értéket, a **PolicyId** az attribútum **TrustFrameworkPolicy** egyedi értékkel. Például: `SignUpSignInLinkedIn`.
3. Frissítse az értéket a **PublicPolicyUri** URI-a szabályzat. Ha például`http://contoso.com/B2C_1A_signup_signin_linkedin`
4. Frissítse az értéket, a **hivatkozásazonosító** attribútum **DefaultUserJourney** megfelelően (SignUpSignLinkedIn) létrehozott új felhasználói interakciósorozat azonosítója.
5. Mentse a módosításokat, a fájl feltöltéséhez, tesztelje azt megnyitásával, és kattintson a **Futtatás most**.

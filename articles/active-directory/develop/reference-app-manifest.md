---
title: Az Azure Active Directory alkalmazásjegyzék értelmezése |} A Microsoft Docs
description: Részletes lefedettségét az Azure Active Directory alkalmazásjegyzék, amely jelenti egy alkalmazás identitás konfigurálása az Azure AD-bérlő, és az OAuth hitelesítési, jóváhagyási felületen és egyéb megkönnyítésére szolgál.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja
ms.openlocfilehash: 74bcd1e795186eba652d2da986c290e1989dd1d1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041574"
---
# <a name="azure-active-directory-app-manifest"></a>Az Azure Active Directory-manifest aplikace

Alkalmazások integrálása az Azure Active Directory (Azure AD) az Azure AD-bérlő regisztrálva kell lennie. Beállíthatja, hogy az alkalmazás a [az Azure portal](https://portal.azure.com) kiválasztásával **alkalmazásregisztrációk** alatt **Azure Active Directory**, szeretne konfigurálni, az alkalmazás kiválasztása, majd kiválasztásával **Manifest**.

## <a name="manifest-reference"></a>Jegyzékfájl referencia

> [!NOTE]
> Ha nem látja a **példaérték** oszlop után a **leírás**, állítsa teljes méretre a böngészőablakot, és görgessen/pöccintsen, amíg megjelenik a **példaérték** oszlop.

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]

| Kulcs  | Érték típusa | Leírás  | Példaérték |
|---------|---------|---------|---------|
| `accessTokenAcceptedVersion` | Nullázható Int32 | Adja meg az elfogadott hozzáférési token verzió a jelenlegi API-erőforrásban. A lehetséges értékek: 1, 2, NULL értékű. Az alapértelmezett érték null, amely 2, lesznek kezelve. | `2` |
| `allowPublicClient` | logikai | A tartalék alkalmazás típusát határozza meg. Az Azure AD alapértelmezés szerint az alkalmazás típusát, a replyUrlsWithType kikövetkezteti. Vannak bizonyos helyzetekben, ahol az Azure AD nem tudja megállapítani az ügyféloldali alkalmazás típusa (pl. [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3) HTTP-kérelem egy URL-átirányítás nélkül történik, ahol a folyamat). Ezekben az esetekben az Azure AD fog értelmezni az alkalmazás típusát, ez a tulajdonság értéke alapján. Ha ezt az értéket a tartalék alkalmazástípus igaz értékre van beállítva, például mobileszközön futó telepített alkalmazás nyilvános ügyfél van beállítva. Az alapértelmezett érték: false ami azt jelenti, hogy a tartalék alkalmazástípus bizalmas ügyfél, például webes alkalmazás. | `false` |
| `appId` | Azonosító karakterlánc | Itt adhatja meg az alkalmazás-alkalmazásokba az Azure AD által hozzárendelt egyedi azonosítója. | `"601790de-b632-4f57-9523-ee7cb6ceba95"` |
| `appRoles` | A tömb típusa | Itt adhatja meg, hogy egy alkalmazás deklarálhat szerepkörök gyűjteménye. Ezeket a szerepköröket rendelhet felhasználókhoz, csoportokhoz és egyszerű szolgáltatásokat. További példákat és info [alkalmazás-szerepkörök hozzáadása az alkalmazásban, és fogadhatók a jogkivonat](howto-add-app-roles-in-azure-ad-apps.md) | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  |
| `groupMembershipClaims` | sztring | Bitmaszk, amely beállítja a `groups` egy felhasználó vagy az OAuth 2.0 hozzáférési jogkivonatban, amelyeket az alkalmazás vár a kiállított jogcímet. A bitmaszk értékei a következők:<br>0: nincs<br>1: biztonsági csoportok és az Azure AD-szerepkörök<br>2: fenntartva<br>4: fenntartva<br>A bitmaszk beállítása a 7 minden a biztonsági csoportok, terjesztési csoportok és az Azure AD-címtárbeli szerepkörök, amelyek a bejelentkezett felhasználó tagja kap. | `1` |
| `optionalClaims` | sztring | A választható jogcímeket adja vissza a jogkivonat a biztonsági jogkivonat szolgáltatás az adott alkalmazás. További információ: [választható jogcímek](active-directory-optional-claims.md). | `null` |
| `id` | Azonosító karakterlánc | A címtárban lévő alkalmazás egyedi azonosítója. Ez az azonosító nem áll az alkalmazás bármely protokoll tranzakció azonosításához használt azonosító. A hivatkozik az objektumra a címtárlekérdezés használatos. | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `identifierUris` | Karakterlánc-tömbben | Felhasználó által definiált URI(s), amely egyedi azonosításához az Azure AD-bérlővel, vagy egyetlen ellenőrzött egyéni tartománynak egy webalkalmazás, ha az alkalmazás több-bérlős. | <code>[<br>&nbsp;&nbsp;"https://MyRegistererdApp"<br>]</code> |
| `informationalUrls` | sztring | Adja meg a hivatkozásokat az alkalmazás és az adatvédelmi nyilatkozat feltételeit. A felhasználók számára a felhasználói jóváhagyási felületen keresztül illesztett és az adatvédelmi nyilatkozat feltételeit. További információ: [hogyan: regisztrált Azure AD-alkalmazások és az adatvédelmi nyilatkozat feltételeit bővíteni](howto-add-terms-of-service-privacy-statement.md). | <code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> |
| `keyCredentials` | A tömb típusa | Hitelesítő alkalmazás által hozzárendelt, a karakterlánc-alapú közös titkos kulcsot és az X.509-tanúsítványokat mutató hivatkozásokat tartalmazza. Ezeket a hitelesítő adatokat használt hozzáférési jogkivonatokat kérésekor (ha az alkalmazás egy ügyfél hálózataként erőforrásként ahelyett hogy). | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> |
| `knownClientApplications` | A tömb típusa | Jóváhagyás kötegelése használatos, ha van olyan megoldás, amely tartalmazza a két részből áll: egy ügyfélalkalmazást és a egy egyéni webes API-alkalmazás. Ha ezt az értéket ad meg az ügyfél-alkalmazás alkalmazásazonosítója, a felhasználó csak kell hozzájárulás megadása után az ügyfélalkalmazás. Az Azure AD tudni fogja, hogy hozzájárul ahhoz, hogy az ügyfél azt jelenti, hogy az implicit módon beleegyezik abba, hogy a webes API-t és az ügyfél és a webes API-k egyszerű szolgáltatásokat automatikusan kiépül egy időben. Az ügyfél és a webes API-alkalmazás ugyanabban a bérlőben regisztrálni kell. | `[GUID]` |
| `logoUrl` | sztring | Csak a CDN URL-címe a portálon feltöltött embléma mutató érték értelmezése. | `https://MyRegisteredAppLogo` |
| `logoutUrl` | sztring | Jelentkezzen ki az alkalmazásból az URL-címe. | `https://MyRegisteredAppLogout` |
| `name` | sztring | Az alkalmazás megjelenített neve. | `MyRegisteredApp` |
| `oauth2AllowImplicitFlow` | logikai | Itt adhatja meg, hogy ez a webalkalmazás kérhet OAuth2.0 implicit folyamat hozzáférési jogkivonatok. Az alapértelmezett érték FALSE (hamis). Ez a jelző böngészőalapú alkalmazások, például a Javascript egyoldalas alkalmazások szolgál. További tudnivalókért írja be a `OAuth 2.0 implicit grant flow` a tartalomjegyzékben, és az implicit folyamat kapcsolatos témakörökben talál. | `false` |
| `oauth2AllowIdTokenImplicitFlow` | logikai | Itt adhatja meg, hogy ez a webalkalmazás kérhet OAuth2.0 implicit folyamat azonosító-jogkivonatokat. Az alapértelmezett érték FALSE (hamis). Ez a jelző böngészőalapú alkalmazások, például a Javascript egyoldalas alkalmazások szolgál. | `false` |
| `oauth2Permissions` | A tömb típusa | Adja meg az OAuth 2.0-engedélyhatókörök, amely a webes API (erőforrás) alkalmazás közzéteszi az ügyfélalkalmazások gyűjteménye. Ezek engedélyhatókörök ügyfélalkalmazások során hozzájárulási nyújtható. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"u| Adja meg az OAuth 2.0-engedélyhatókörök, amely a webes API (erőforrás) alkalmazás közzéteszi az ügyfélalkalmazások gyűjteménye. Ezek engedélyhatókörök ügyfélalkalmazások során hozzájárulási nyújtható. ser_impersonation"<br>&nbsp;&nbsp;}<br>]</code> |
| `oauth2RequiredPostResponse` | logikai | Megadja, hogy, az OAuth 2.0 jogkivonat-kérelmeket részeként az Azure AD engedélyezi POST kéréseket, GET-kérések szemben. Az alapértelmezett érték FALSE (hamis), amely megadja, hogy csak a GET-kérések engedélyezett lesz. | `false` |
| `parentalControlSettings` | sztring | `countriesBlockedForMinors` Itt adhatja meg, amelyben az alkalmazás le van tiltva kiskorúak országok.<br>`legalAgeGroupRule` Adja meg a jogi korcsoport csoport szabály, amely az alkalmazás felhasználóinak vonatkozik. Állítható `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids`, vagy `BlockMinors`.  | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> |
| `passwordCredentials` | A tömb típusa | A leírást lásd a `keyCredentials` tulajdonság. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> |
| `preAuthorizedApplications` | A tömb típusa | Alkalmazások és az beleegyezése kért engedélyeket sorolja fel. Szükséges egy rendszergazdát, hogy az alkalmazás hozzájárulást adott. preAuthorizedApplications nem kéri a felhasználótól, hogy engedélyt adjanak az a kért engedélyeket. PreAuthorizedApplications felsorolt engedélyek nem szükséges felhasználói beleegyezés. Azonban nem szerepel az preAuthorizedApplications további kért engedélyeket szükséges felhasználói beleegyezés. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `replyUrlsWithType` | Karakterlánc-tömbben | Ez több érték a tulajdonság, amely elfogadja az Azure ad-ben regisztrált redirect_uri értékek listáját tartalmazza a célként jogkivonatok visszaadásakor. Az egyes uri-értékek tartalmaznia kell egy társított alkalmazás típusú értékké. Támogatott típusú értékek a következők: `Web`, `InstalledClient`. | <code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> |
| `requiredResourceAccess` | A tömb típusa | A dinamikus hozzájárulásával `requiredResourceAccess` meghajtók, a rendszergazdai jóváhagyási felületen és a felhasználói jóváhagyási felületen statikus hozzájárulási használó felhasználók számára. Azonban ez nem a meghajtó a felhasználói jóváhagyási felületen az Általános eset.<br>`resourceAppId` az egyedi azonosítója az erőforrást, az alkalmazás hozzáférésre van szüksége van. Ezt az értéket megadva a cél erőforrás-alkalmazás alkalmazásazonosítója egyenlőnek kell lennie.<br>`resourceAccess` van egy tömb, amely felsorolja a OAuth2.0-engedélyhatókörök, és az alkalmazás a megadott erőforrás igénylő alkalmazás-szerepkörök. Tartalmazza a `id` és `type` értékeket a megadott erőforrások. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>] </code> |
| `samlMetadataUrl` | sztring | Az alkalmazás SAML-metaadataira mutató URL-címe. | `https://MyRegisteredAppSAMLMetadata` |
| `signInUrl` | sztring | Adja meg az URL-cím, az alkalmazás kezdőlapjára. | `https://MyRegisteredApp` |
| `signInAudience` | sztring | Itt adhatja meg, milyen microsoft-fiókok az aktuális alkalmazás használata támogatott. Támogatott értékei a következők:<ul><li>**AzureADMyOrg** – a Microsoft a felhasználók munkahelyi vagy iskolai fiók a szervezet Azure AD-bérlőben (azaz egybérlős)</li><li>**AzureADMultipleOrgs** – a Microsoft a felhasználók munkahelyi vagy iskolai fiók bármely szervezet az Azure AD-bérlőben (azaz több bérlős)</li> <li>**AzureADandPersonalMicrosoftAccount** -felhasználók személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókot bármely szervezet az Azure AD-bérlőben</li></ul> | `AzureADandPersonalMicrosoftAccount` |
| `tags` | Karakterlánc-tömbben | Egyéni karakterláncok kategorizálásához és az alkalmazás azonosításához használható. | <code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code> |

## <a name="next-steps"></a>További lépések

* Az alkalmazás alkalmazás- és szolgáltatásnév objektumok közötti kapcsolatot a további információk: [alkalmazás és egyszerű szolgáltatási objektumok Azure AD-ben](app-objects-and-service-principals.md).
* Tekintse meg a [Azure AD fejlesztői szószedetét](developer-glossary.md) az alapvető Azure Active Directory (AD) fejlesztői fogalmak némelyike definíciói.

Használja a következő megjegyzéseket visszajelzést, amellyel pontosíthatja, és a tartalom formázása.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

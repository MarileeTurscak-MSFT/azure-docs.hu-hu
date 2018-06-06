---
title: Az egyéni házirendeket az Azure Active Directory B2C használatával megjegyzések fejlesztők számára |} Microsoft Docs
description: Megjegyzések fejlesztők konfigurálásának és fenntartásának az Azure AD B2C egyéni házirendek számára.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 10/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ce323972dcdbf673311b407f427bc452fbe6dc3a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709726"
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Az Azure Active Directory B2C egyéni házirend nyilvános előzetes kibocsátási megjegyzései
Az egyéni házirend szolgáltatáskészlet már elérhető az összes Azure Active Directory B2C előzetes verzióját nyilvános értékelésre (az Azure AD B2C) ügyfelek. E szolgáltatáskészlet speciális identitás fejlesztők a legösszetettebb identitáskezelési megoldások kialakításának irányul.  

E szolgáltatáskészlet jelenleg a identitás élmény keretrendszer keresztül közvetlenül XML-fájl szerkesztésével konfigurálhatja a fejlesztők van szükség. A konfigurációs módszer hatékony és összetett. Speciális identitás identitás élmény keretrendszerrel fejlesztők kell terveznie a fektetnek útmutatók befejezése és a hivatkozás dokumentumok olvasási időt. 

## <a name="features-included-in-this-public-preview"></a>A nyilvános előzetes szereplő szolgáltatások
A nyilvános előzetes verziójában bevezetett új funkciókkal fejlesztők számára a következő feladatokat végezheti el:<br>

* Szerző és feltöltése az egyéni hitelesítési felhasználói utak egyéni házirendekkel. 
   * Felhasználói utak cseréjét, részletes Jogcímszolgáltatók közötti ismertetik. 
   * Határozza meg azt a felhasználói utak feltételes ugorhat. 
* Az egyéni hitelesítési felhasználói utak szolgáltatások REST API-t integrálja.  
* Adja hozzá az Identitásszolgáltatók, amelyek megfelelnek a szabványos OpenIDConnect összevonásának. <br>
* Adja hozzá az összevonás az identitás-szolgáltatóktól igazodnia kell a SAML 2.0 protokoll. 

## <a name="terms-of-the-public-preview"></a>A nyilvános előzetes feltételeit.

* Javasoljuk, hogy az új szolgáltatások használatához csak tesztelési célokra.<br>
* Az új szolgáltatások nem feltétlenül egy éles környezetben való használathoz.<br>
* Szolgáltatásszint-szerződések (SLA) nem vonatkoznak az új szolgáltatásokat. <br>
* Támogatási kérelmek nyilvántartásához rendszeres támogatási csatornákon keresztül. <br>
* Nincs megadva általánosan rendelkezésre álló Ígért dátum.<br>
* Saját belátása szerint, és bármilyen okból Microsoft is jelzőt és elutasítása vagy forgatókönyvek és felhasználói útvonal be, amelyek mérete meghaladja az Azure AD B2C termék okleveles felhasználói identitások és hozzáférések (CIAM) felügyeleti platform, mely a körét korlátozza.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Egyéni házirend szolgáltatáskészlet fejlesztők feladatai
Kézi házirend konfigurálása az Azure AD B2C az alapul szolgáló platform alacsonyabb szintű hozzáférést biztosít, és egyedi, teljes mértékben testreszabható megbízhatósági keretrendszer létrehozását eredményezi. Ezek lehetséges kombinációinak egyéni Identitásszolgáltatók, megbízhatósági kapcsolatokat, külső szolgáltatásokkal, és lépésről lépésre munkafolyamatok Integrációk fel őket a speciális fejlesztők nagyobb iránti igények kielégítése érdekében helyezze el.

Teljes mértékben kihasználják a nyilvános előzetes verzióhoz, javasoljuk, hogy a fejlesztők az egyéni házirend szolgáltatáskészlet fel felelnie a következő irányelveket:
* Ismerkedjen meg az identitás élmény keretrendszer és a kulcs vagy titkos kulcsok kezelése konfigurációs nyelve.
* Saját tulajdonba forgatókönyveket és a használatot.
* A forgatókönyv módszeres tesztek végrehajtása.
* Hajtsa végre a szoftverek fejlesztési és átmeneti legalább egy fejlesztési és tesztelési környezetben az ajánlott eljárásokról és egy éles környezetben.
* Maradjon naprakész az identitás-szolgáltatóktól és integrálja szolgáltatások új fejlesztéseiről. Például nyomon követheti, változások a titkos kulcsok és a szolgáltatás ütemezett és nem ütemezett módosításait.
* Aktív figyelés beállítása, és figyelje a figyelt termelési környezetben.
* Az Azure-előfizetésben naprakészen kapcsolattartási e-mail címet, és válaszol a Microsoft live hely team e-mailek marad.
* Ha ehhez a Microsoft live hely csapat által ajánlott időben művelet végrehajtása. 

## <a name="features-by-stage-and-known-issues"></a>Szolgáltatások szakasz és ismert problémák
Egyéni házirend/identitás élmény keretrendszer képességek folyamatos és gyors fejlesztés alatt áll.  Ebben a táblában funkciók/összetevő rendelkezésre állása index.

Kérdéseit felteheti a Stack Overflow [https://aka.ms/aadb2cso](https://aka.ms/aadb2cso)


### <a name="identity-providers-tokens-protocols"></a>Identitás-szolgáltatók jogkivonatokat, protokollok
Külső összetevőket és alkalmazások rendelkező csatolók

| Szolgáltatás | Fejlesztés | Előzetes verzió | FE | Megjegyzések |
|---------------------------------------------|-------------|---------|----|-------|
| IDP-OpenIDConnect |  | x |  | például Google + |
| IDP-OAUTH2 |  | x |  | Ha például a Facebook-on  |
| IDP-OAUTH1 |  | x |  | például a Twitter |
| IDP-SAML |  | x |  | például Salesforce, az AD FS |
| IDP-WSFED | x |  |  |  |
| Függő entitás OAUTH |  | x |  |  |
| Függő entitás OIDC |  | x |  |  |
| Függő entitás SAML | x |  |  |  |
| Függő entitás WSFED | x |  |  |  |
| Az alapszintű és a művelet auth REST API-n |  | x |  | például az Azure Functions |


### <a name="component-support"></a>Összetevő támogatása


| Szolgáltatás | Fejlesztés | Előzetes verzió | FE | Megjegyzések |
|-------------------------------------------|-------------|---------|----|-------|
| Azure Multi Factor Authentication |  | x |  |  |
| Az Azure Active Directory helyi könyvtár |  | x |  |  |
| E-mailek Azure alrendszere 2FA |  | x |  |  |
| Több nyelv támogatása|  | x |  |  |
| Jelszó bonyolultsága | x |  |  |  |


### <a name="content-definition"></a>Tartalom meghatározása

| Szolgáltatás | Fejlesztés | Előzetes verzió | FE | Megjegyzések |
|-----------------------------------------------------------------------------|-------------|---------|----|-------|
|   Hibalap, api.error |  | x |  |  |
|   IDP kiválasztása lapon, a api.idpselections |  | x |  |  |
|   Előfizetési, IDP telepítendő api.idpselections.signup |  | x |  |  |
|   Forgot Password, api.localaccountpasswordreset |  | x |  |  |
|   Helyi fiók bejelentkezhet, api.localaccountsignin |  | x |  |  |
|   Helyi fiók előfizetési, api.localaccountsignup |  | x |  |  |
|   Többtényezős hitelesítés lap, api.phonefactor |  | x |  |  |
|   Önálló magas – például közösségi fiók sig felpörgetéshez api.selfasserted |  | x |  |  |
|   Önálló magas profil frissítéséhez api.selfasserted.profileupdate |  | x |  |  |
|   Egyesített előfizetési vagy a bejelentkezési oldal, a api.signuporsignin |  | x |  |  |


### <a name="app-ief-integration"></a>Alkalmazás-IEF integráció
| Szolgáltatás | Fejlesztés | Előzetes verzió | FE | Megjegyzések |
|--------------------------------------------------|-------------|---------|----|-------------------------------------------------|
| Lekérdezési karakterlánc paraméter id_token_hint | x |  |  |  |
| Lekérdezési karakterlánc paraméter domain_hint |  | x |  | rendelkezésre álló jogcímként, átadhatók IDP |
| Lekérdezési karakterlánc paraméter login_hint |  | x |  | rendelkezésre álló jogcímként, átadhatók IDP |
| UserJourney keresztül client_assertion JSON beillesztése | x |  |  | elavulttá válik |
| UserJourney id_token_hint, JSON beszúrása | x |  |  | Nyissa meg továbbítás megközelítés felelt meg a JSON |


### <a name="session-management"></a>Munkamenet-kezelés

| Szolgáltatás | Fejlesztés | Előzetes verzió | FE | Megjegyzések |
|---------------------------------|-------------|---------|----|-------|
| Egyszeri bejelentkezési munkamenet-szolgáltató |  | x |  |  |
| Külső bejelentkezési munkamenet-szolgáltató |  | x |  |  |
| SAML SSO munkamenet-szolgáltató |  | x |  |  |


### <a name="security"></a>Biztonság
| Szolgáltatás | Fejlesztés | Előzetes verzió | FE | Megjegyzések |
|---------------------------------------------|-------------|---------|----|-------|
| Házirend kulcsok létrehozása, manuális, feltöltése |  | x |  |  |
| Házirend kulcsok - RSA/Cert, titkos kulcsok |  | x |  |  |


### <a name="developer-interface"></a>Fejlesztői felület
| Szolgáltatás | Fejlesztés | Előzetes verzió | FE | Megjegyzések |
|---------------------------------------------|-------------|---------|----|-------|
| Az Azure portál – IEF UX |  | x |  |  |
| Application Insights UserJourney naplók  |  | x |  |  |
| Application Insights-eseménynaplók |x|  |  |  |



## <a name="next-steps"></a>További lépések
[Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).

---
title: Az Azure Active Directory (Azure AD) naplózott tevékenységeire vonatkozó referencia | Microsoft Docs
description: Tekintse át az Azure Active Directoryban (Azure AD) naplókban rögzíthető tevékenységeket.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 04/19/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 818f4f4d8ea51cb278190d6e41953f0b04b2e995
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/09/2018
ms.locfileid: "42060800"
---
# <a name="azure-ad-audit-activity-reference"></a>Azure AD naplózott tevékenységekre vonatkozó referencia

Az Azure Portalon az Azure Active Directory (Azure AD) jelentéskészítési funkciójával minden szükséges információhoz hozzájuthat a környezetével kapcsolatban.

Az Azure AD jelentéskészítési architektúrája a következő elemekből áll:

- **Tevékenység** 
    - **Bejelentkezési tevékenységek** – A felügyelt alkalmazások használatával és a felhasználók bejelentkezési tevékenységeivel kapcsolatos információk
    - **Auditnaplók** – az Azure AD-n belül különböző szolgáltatások által végrehajtott összes módosításra vonatkozó nyomkövetési naplókat biztosít. A naplók például rögzítik bármely erőforrás módosításait az Azure AD-n belül, úgymint: felhasználók, alkalmazások, csoportok, szerepkörök, szabályzatok, hitelesítések stb.
- **Biztonság** 
    - **Kockázatos bejelentkezések** – A kockázatos bejelentkezés egy olyan bejelentkezési kísérletet jelöl, amelyet elképzelhető, hogy olyan személy hajtott végre, aki nem a felhasználói fiók jogos tulajdonosa. További részletek: Kockázatos bejelentkezések.
    - **Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága. További részletek: Kockázatosként megjelölt felhasználók.

Ez a cikk a naplókban rögzíthető tevékenységeket sorolja fel.
 


## <a name="access-reviews"></a>Hozzáférési felülvizsgálatok

|Naplózási kategória|Tevékenység|
|---|---|
|Címtárkezelés|Adminisztratív egység hozzáadása|
|Címtárkezelés|Tag hozzáadása adminisztratív egységhez|
|Címtárkezelés|Adminisztratív egység törlése|
|Címtárkezelés|Tag eltávolítása az adminisztratív egységből|
|Címtárkezelés|Adminisztratív egység frissítése|
|Címtárkezelés|Adminisztráció|
|Felhasználókezelés|Címtárművelet|
|Felhasználókezelés|Exportálás|
|Felhasználókezelés|Importálás|
|Felhasználókezelés|Egyéb|
|Felhasználókezelés|Folyamatletét|
|Felhasználókezelés|Szinkronizálási szabályművelet|
|Felhasználókezelés|Alkalmazás hozzáadása|
|Felhasználókezelés|Alkalmazás törlése|
|Felhasználókezelés|Az alkalmazás frissítése|
|Felhasználókezelés|Alkalmazás frissítése: egyszeri bejelentkezés üzemmód|
|Felhasználókezelés|Automatizált jelszóváltás|
|Felhasználókezelés|V2 alkalmazásengedélyek hozzáadása|
|Felhasználókezelés|V1 alkalmazás létrehozása|
|Felhasználókezelés|V2 alkalmazás létrehozása|
|Felhasználókezelés|V1 alkalmazás törlése|
|Felhasználókezelés|V2 alkalmazás törlése|
|Felhasználókezelés|V2 alkalmazás engedélymegadásának törlése|
|Felhasználókezelés|V1 és V2 alkalmazások lekérése|
|Felhasználókezelés|V1 alkalmazás lekérése|
|Felhasználókezelés|V1 alkalmazások lekérése|
|Felhasználókezelés|V2 alkalmazás lekérése|
|Felhasználókezelés|V2 alkalmazások lekérése|




## <a name="account-provisioning"></a>Fiók kiépítése

|Naplózási kategória|Tevékenység|
|---|---|
|Alkalmazáskezelés|V2 alkalmazás engedélymegadásainak beolvasása|
|Alkalmazáskezelés|V2 alkalmazások szolgáltatásnevének lekérése az aktuális bérlőben|
|Alkalmazáskezelés|V1 alkalmazás frissítése|
|Alkalmazáskezelés|V2 alkalmazás frissítése|
|Alkalmazáskezelés|V2 alkalmazás engedélymegadásának frissítése|
|Alkalmazáskezelés|Oauth2Permissiongrant hozzáadása|
|Alkalmazáskezelés|Alkalmazásszerepkör-hozzárendelés hozzáadása a szolgáltatásnévhez|

## <a name="application-proxy"></a>Alkalmazásproxy

|Naplózási kategória|Tevékenység|
|---|---|
|Alkalmazáskezelés|Alkalmazás hozzáadása|
|Alkalmazáskezelés|Tulajdonos hozzáadása alkalmazáshoz|
|Alkalmazáskezelés|Tulajdonos hozzáadása szolgáltatásnévhez|
|Alkalmazáskezelés|Szabályzat hozzáadása szolgáltatásnévhez|
|Címtárkezelés|Szolgáltatásnév hozzáadása|
|Címtárkezelés|Szolgáltatásnév hitelesítő adatainak hozzáadása|
|Címtárkezelés|Hozzájárulás az alkalmazáshoz|
|Címtárkezelés|Alkalmazás törlése|
|Címtárkezelés|Alkalmazás végleges törlése|
|Címtárkezelés|Az Oauth2Permissiongrant eltávolítása|
|Címtárkezelés|Alkalmazásszerepkör-hozzárendelés eltávolítása a szolgáltatásnévből|
|Címtárkezelés|Tulajdonos eltávolítása az alkalmazásból|
|Erőforrás|Tulajdonos eltávolítása az egyszerű szolgáltatásnévből|
|Erőforrás|Szabályzat eltávolítása az egyszerű szolgáltatásnévből|
|Erőforrás|Szolgáltatásnév eltávolítása|


## <a name="automated-password-rollover"></a>Automatizált jelszóváltás

|Naplózási kategória|Tevékenység|
|---|---|
|Alkalmazáskezelés|Egyszerű szolgáltatásnév hitelesítő adatainak eltávolítása|


## <a name="b2c"></a>B2C

|Naplózási kategória|Tevékenység|
|---|---|
|Alkalmazáskezelés|Alkalmazás visszaállítása|
|Alkalmazáskezelés|Jóváhagyás visszavonása|
|Alkalmazáskezelés|Az alkalmazás frissítése|
|Alkalmazáskezelés|Külső titkos kódok frissítése|
|Alkalmazáskezelés|Szolgáltatásnév frissítése|
|Alkalmazáskezelés|Hozzáférési jogkivonat kiadása az alkalmazásnak|
|Alkalmazáskezelés|Hitelesítési kód kiadása az alkalmazásnak|
|Alkalmazáskezelés|id_token kiadása az alkalmazásnak|
|Alkalmazáskezelés|Helyi fiók hitelesítő adatainak ellenőrzése|
|Alkalmazáskezelés|Felhasználóhitelesítés ellenőrzése|
|Alkalmazáskezelés|V2 alkalmazásengedélyek hozzáadása|
|Alkalmazáskezelés|ASCII-titkosításon alapuló kulcs hozzáadása CPIM-kulcstárolóhoz|
|Alkalmazáskezelés|Kulcs hozzáadása CPIM-kulcstárolóhoz|
|Alkalmazáskezelés|AdminPolicyDatas-SetResources|
|Alkalmazáskezelés|AdminUserJourneys-GetResources|
|Alkalmazáskezelés|AdminUserJourneys-RemoveResources|
|Hitelesítés|AdminUserJourneys-SetResources|
|Hitelesítés|IdentityProvider létrehozása|
|Hitelesítés|V1 alkalmazás létrehozása|
|Hitelesítés|V2 alkalmazás létrehozása|
|Hitelesítés|Egyéni tartomány létrehozása a bérlőben|
|Engedélyezés|Új AdminUserJourney létrehozása|
|Engedélyezés|Honosított json-erőforrás létrehozása|
|Engedélyezés|Új egyéni identitásszolgáltató létrehozása|
|Engedélyezés|Új identitásszolgáltató létrehozása|
|Engedélyezés|B2C címtárerőforrás létrehozása vagy frissítése|
|Engedélyezés|Szabályzat létrehozása|
|Engedélyezés|trustFramework szabályzat létrehozása|
|Engedélyezés|Konfigurálható előtaggal rendelkező trustFramework szabályzat létrehozása|
|Engedélyezés|Felhasználói attribútum létrehozása|
|Engedélyezés|CreateTrustFrameworkPolicy|
|Engedélyezés|Új AdminUserJourney létrehozása vagy frissítése|
|Engedélyezés|Identitásszolgáltató törlése|
|Engedélyezés|IdentityProvider törlése|
|Engedélyezés|V1 alkalmazás törlése|
|Engedélyezés|V2 alkalmazás törlése|
|Engedélyezés|V2 alkalmazás engedélymegadásának törlése|
|Engedélyezés|B2C címtárerőforrás törlése|
|Engedélyezés|CPIM-kulcstároló törlése|
|Engedélyezés|trustFramework szabályzat törlése|
|Engedélyezés|Felhasználói attribútum törlése|
|Engedélyezés|B2C funkció engedélyezése|
|Engedélyezés|Előfizetéshez tartozó B2C címtárerőforrások lekérése|
|Engedélyezés|Egyéni identitásszolgáltató lekérése|
|Engedélyezés|Identitásszolgáltató lekérése|
|Engedélyezés|V1 és V2 alkalmazások lekérése|
|Engedélyezés|V1 alkalmazás lekérése|
|Engedélyezés|V1 alkalmazások lekérése|
|Engedélyezés|V2 alkalmazás lekérése|
|Engedélyezés|V2 alkalmazások lekérése|
|Engedélyezés|B2C címtárerőforrás lekérése|
|Engedélyezés|A bérlőben lévő egyéni tartományok listájának lekérése|
|Engedélyezés|Felhasználói út lekérése|
|Engedélyezés|A felhasználói út engedélyezett alkalmazásjogcímeinek lekérése|
|Engedélyezés|A felhasználói út engedélyezett önellenőrzött jogcímeinek lekérése|
|Engedélyezés|A szabályzat engedélyezett önellenőrzött jogcímeinek lekérése|
|Engedélyezés|Elérhető kimeneti jogcímek listájának lekérése|
|Engedélyezés|A felhasználói út tartalomdefinícióinak lekérése|
|Engedélyezés|Adott rendszergazdai folyamathoz tartozó identitásszolgáltatók lekérése|
|Engedélyezés|Kulcstároló aktív kulcsaihoz tartozó metaadatok lekérése JWK-ban|
|Engedélyezés|Az összes rendszergazdai folyamat|
|Engedélyezés|Az összes felhasználó összes rendszergazdai folyamatához tartozó címkék listájának lekérése|
|Engedélyezés|Felhasználó bérlői listájának lekérése|
|Engedélyezés|Helyi fiókok önellenőrző jogcímeinek lekérése|
|Engedélyezés|Honosított json-erőforrás lekérése|
|Engedélyezés|A Microsoft.AzureActiveDirectory erőforrás-szolgáltató műveleteinek lekérése|
|Engedélyezés|Szabályzatok lekérése|
|Engedélyezés|Szabályzat lekérése|
|Engedélyezés|Bérlő erőforrás-tulajdonságainak lekérése|
|Engedélyezés|Támogatott identitásszolgáltatók listájának lekérése|
|Engedélyezés|A felhasználói út támogatott identitásszolgáltatói listájának lekérése|
|Engedélyezés|Bérlői adatok lekérése|
|Engedélyezés|Bérlő számára engedélyezett funkciók lekérése|
|Engedélyezés|Bérlő által definiált egyéni identitásszolgáltatók listájának lekérése|
|Engedélyezés|Bérlő által definiált identitásszolgáltatók listájának lekérése|
|Engedélyezés|Bérlő által definiált helyi identitásszolgáltató-lista lekérése|
|Engedélyezés|Felhasználó bérlői információinak lekérése erőforrások létrehozásához|
|Engedélyezés|Bérlők listájának lekérése|
|Engedélyezés|tenantDomains lekérése|
|Engedélyezés|A CPIM alapértelmezett támogatott kultúrájának lekérése|
|Engedélyezés|Rendszergazdai folyamathoz tartozó információk lekérése|
|Engedélyezés|A bérlőhöz tartozó UserJourneys lista lekérése|
|Engedélyezés|A CPIM-hez tartozó elérhető, támogatott kultúrák készletének lekérése|
|Engedélyezés|TrustFramework szabályzat lekérése|
|Engedélyezés|TrustFramework szabályzat lekérése xml-ként|
|Engedélyezés|Felhasználói attribútum lekérése|
|Engedélyezés|Felhasználói attribútumok lekérése|
|Engedélyezés|Felhasználói utak listájának lekérése|
|Engedélyezés|GetIEFPolicies|
|Engedélyezés|GetIdentityProviders|
|Engedélyezés|GetTrustFrameworkPolicy|
|Engedélyezés|Lekérdez egy CPIM-kulcstárolót jwk formátumban|
|Engedélyezés|Lekérdezi a bérlőben lévő kulcstárolók listáját|
|Engedélyezés|Lekérdezi a bérlő típusát|
|Engedélyezés|MigrateTenantMetadata|
|Engedélyezés|IdentityProvider javítása|
|Engedélyezés|PutTrustFrameworkPolicy|
|Engedélyezés|PutTrustFrameworkpolicy|
|Engedélyezés|Felhasználói út eltávolítása|
|Engedélyezés|CPIM-kulcstároló biztonsági másolatának visszaállítása|
|Engedélyezés|V2 alkalmazás engedélymegadásainak beolvasása|
|Engedélyezés|V2 alkalmazások szolgáltatásnevének lekérése az aktuális bérlőben|
|Engedélyezés|Egyéni identitásszolgáltató frissítése|
|Engedélyezés|Identitásszolgáltató frissítése|
|Engedélyezés|Helyi identitásszolgáltató frissítése|
|Engedélyezés|V1 alkalmazás frissítése|
|Engedélyezés|V2 alkalmazás frissítése|
|Engedélyezés|V2 alkalmazás engedélymegadásának frissítése|
|Engedélyezés|Szabályzat frissítése|
|Engedélyezés|Felhasználói attribútum frissítése|
|Engedélyezés|CPIM titkosított kulcs feltöltése|
|Engedélyezés|Felhasználói hitelesítés: az API le van tiltva a bérlő funkciókészletéhez|
|Engedélyezés|Felhasználói hitelesítés: A felhasználó bérlői rendszergazdaként kap hozzáférést|
|Engedélyezés|Felhasználói hitelesítés: Felhasználó hitelesített felhasználóként kapott hozzáférési jogokat|
|Engedélyezés|B2C funkció engedélyezésének ellenőrzése|
|Engedélyezés|Ellenőrizze, hogy a funkció engedélyezve van-e|
|Engedélyezés|Program létrehozása|
|Engedélyezés|Program törlése|
|Engedélyezés|Programvezérlő csatolása|
|Engedélyezés|Azure AD hozzáférési felülvizsgálatok előkészítése|
|Engedélyezés|Programvezérlő leválasztása|
|Engedélyezés|Program frissítése|
|Engedélyezés|Asztali egyszeri bejelentkezés letiltása|
|Engedélyezés|Asztali egyszeri bejelentkezés letiltása adott tartományon|
|Engedélyezés|Alkalmazásproxy letiltása|
|Engedélyezés|Átmenő hitelesítés letiltása|
|Engedélyezés|Asztali egyszeri bejelentkezés engedélyezése|
|Címtárkezelés|Asztali egyszeri bejelentkezés engedélyezése adott tartományhoz|
|Címtárkezelés|Alkalmazásproxy engedélyezése|
|Címtárkezelés|Átmenő hitelesítés engedélyezése|
|Címtárkezelés|Egyéni tartomány létrehozása a bérlőben|
|Címtárkezelés|B2C funkció engedélyezése|
|Címtárkezelés|A bérlőben lévő egyéni tartományok listájának lekérése|
|Címtárkezelés|Bérlő erőforrás-tulajdonságainak lekérése|
|Címtárkezelés|Bérlői adatok lekérése|
|Címtárkezelés|Bérlő számára engedélyezett funkciók lekérése|
|Címtárkezelés|tenantDomains lekérése|
|Kulcs|Lekérdezi a bérlő típusát|
|Kulcs|B2C funkció engedélyezésének ellenőrzése|
|Kulcs|Ellenőrizze, hogy a funkció engedélyezve van-e|
|Kulcs|Partner hozzáadása vállalathoz|
|Kulcs|Nem ellenőrzött tartomány hozzáadása|
|Kulcs|Ellenőrzött tartomány hozzáadása|
|Kulcs|Vállalat létrehozása|
|Kulcs|Vállalati beállítások létrehozása|
|Kulcs|Vállalati beállítások törlése|
|Kulcs|Partner lefokozása|
|Kulcs|Címtár törölve|
|Egyéb|Címtár véglegesen törölve|
|Egyéb|Ez a címtár törlésre van ütemezve|
|Erőforrás|Vállalat előléptetése partnerré|
|Erőforrás|Rights Management-tulajdonságok végleges törlése|
|Erőforrás|Partner eltávolítása a vállalatból|
|Erőforrás|Nem ellenőrzött tartomány eltávolítása|
|Erőforrás|Ellenőrzött tartomány eltávolítása|
|Erőforrás|Cégadatok beállítása|
|Erőforrás|A DirSync funkció beállítása|
|Erőforrás|A DirSyncEnabled jelölő beállítása|
|Erőforrás|Partnerség beállítása|
|Erőforrás|Véletlen törlés küszöbértékének beállítása|
|Erőforrás|Vállalati adatok engedélyezett helyének beállítása|
|Erőforrás|Vállalati nemzetközi szolgáltatás engedélyezésének beállítása|
|Erőforrás|Címtárszolgáltatás beállítása a bérlőn|
|Erőforrás|Tartomány hitelesítésének beállítása|
|Erőforrás|Tartomány összevonási beállításainak beállítása|
|Erőforrás|Jelszószabályzat beállítása|
|Erőforrás|Rights Management tulajdonságainak beállítása|
|Erőforrás|Vállalat frissítése|
|Erőforrás|Vállalati beállítások frissítése|
|Erőforrás|Frissítési tartomány|
|Erőforrás|Tartomány ellenőrzése|
|Erőforrás|E-mailek ellenőrzött tartományának ellenőrzése|
|Erőforrás|Előkészítés|
|Erőforrás|Riasztási beállítások frissítése|
|Erőforrás|Heti összefoglaló beállításainak frissítése|
|Erőforrás|Jelszóvisszaíró letiltása a címtárhoz|
|Erőforrás|Jelszóvisszaíró engedélyezése a címtárhoz|
|Erőforrás|Alkalmazásszerepkör-hozzárendelés hozzáadása csoporthoz|
|Erőforrás|Csoport hozzáadása|
|Erőforrás|Tag hozzáadása csoporthoz|
|Erőforrás|Tulajdonos hozzáadása csoporthoz|
|Erőforrás|Csoportbeállítások létrehozása|
|Erőforrás|Csoport törlése|
|Erőforrás|Csoportbeállítások törlése|
|Erőforrás|Csoportalapú licenc felhasználókra való alkalmazásának befejezése|
|Erőforrás|Csoport végleges törlése|
|Erőforrás|Alkalmazás szerepkör-hozzárendelésének eltávolítása a csoportból|
|Erőforrás|Tag eltávolítása a csoportból|
|Erőforrás|Tulajdonos eltávolítása a csoportból|
|Erőforrás|Csoport visszaállítása|
|Erőforrás|Csoportlicenc beállítása|
|Erőforrás|Csoport beállítása felhasználó által kezeltként|
|Erőforrás|Csoportalapú licenc felhasználókra való alkalmazásának megkezdése|
|Erőforrás|Csoportlicenc újraszámolásának aktiválása|
|Erőforrás|Csoport frissítése|
|Erőforrás|Csoportbeállítások frissítése|
|Erőforrás|Tag hozzáadása|
|Erőforrás|Csoport létrehozása|
|Erőforrás|Csoport törlése|
|Erőforrás|Tag eltávolítása|
|Erőforrás|Csoport frissítése|
|Erőforrás|Csoporthoz való csatlakoztatásra vonatkozó, függő kérelem jóváhagyása|
|Erőforrás|Csoporthoz való csatlakoztatásra vonatkozó, függő kérelem visszavonása|
|Erőforrás|Életciklus-kezelési szabályzat létrehozása|
|Erőforrás|Csoporthoz való csatlakoztatásra vonatkozó, függő kérelem törlése|
|Erőforrás|Csoporthoz való csatlakoztatásra vonatkozó, függő kérelem elutasítása|
|Erőforrás|Csoport megújítása|
|Erőforrás|Csoporttagság kérése|
|Erőforrás|Dinamikus csoport tulajdonságainak beállítása|
|Erőforrás|Életciklus-kezelési szabályzat frissítése|
|Erőforrás|ASCII-titkosításon alapuló kulcs hozzáadása CPIM-kulcstárolóhoz|
|Erőforrás|Kulcs hozzáadása CPIM-kulcstárolóhoz|
|Erőforrás|CPIM-kulcstároló törlése|
|Erőforrás|Kulcstároló törlése|
|Erőforrás|Kulcstároló aktív kulcsaihoz tartozó metaadatok lekérése JWK-ban|
|Erőforrás|Kulcstároló metaadatainak lekérése|
|Erőforrás|Lekérdez egy CPIM-kulcstárolót jwk formátumban|
|Erőforrás|Lekérdezi a bérlőben lévő kulcstárolók listáját|
|Erőforrás|CPIM-kulcstároló biztonsági másolatának visszaállítása|
|Erőforrás|Kulcstároló mentése|
|Erőforrás|CPIM titkosított kulcs feltöltése|
|Erőforrás|Hitelesítési kód kiadása az alkalmazásnak|
|Erőforrás|id_token kiadása az alkalmazásnak|


## <a name="core-directory"></a>Alapvető könyvtár

|Naplózási kategória|Tevékenység|
|---|---|
|Adminisztratív egységek kezelése|Egy kockázatiesemény-típus letöltése|
|Adminisztratív egységek kezelése|Rendszergazdák és a heti összefoglalók letöltésének jóváhagyása|
|Adminisztratív egységek kezelése|Összes kockázatiesemény-típus letöltése|
|Adminisztratív egységek kezelése|Ingyenes felhasználói kockázati események letöltése|
|Adminisztratív egységek kezelése|Kockázatosként megjelölt felhasználók letöltése|
|Alkalmazáskezelés|Feldolgozott kötegelt meghívók|
|Alkalmazáskezelés|Kötegelt meghívók feltöltve|
|Alkalmazáskezelés|Tulajdonos hozzáadása szabályzathoz|
|Alkalmazáskezelés|Házirend hozzáadása|
|Alkalmazáskezelés|Szabályzat törlése|
|Alkalmazáskezelés|Szabályzat hitelesítő adatainak eltávolítása|
|Alkalmazáskezelés|Szabályzat frissítése|
|Alkalmazáskezelés|Az MFA regisztrációs szabályzatának beállítása|
|Alkalmazáskezelés|Bejelentkezési kockázati szabályzat beállítása|
|Alkalmazáskezelés|Felhasználói kockázati szabályzat beállítása|
|Alkalmazáskezelés|Használati feltételek elfogadása|
|Alkalmazáskezelés|Használati feltételek létrehozása|
|Alkalmazáskezelés|Használati feltételek elutasítása|
|Alkalmazáskezelés|Használati feltételek törlése|
|Alkalmazáskezelés|Használati feltételek szerkesztése|
|Alkalmazáskezelés|Használati feltételek közzététele|
|Alkalmazáskezelés|Használati feltételek közzétételének visszavonása|
|Alkalmazáskezelés|Alkalmazás SSL-tanúsítványának hozzáadása|
|Alkalmazáskezelés|SSL-kötés törlése|
|Alkalmazáskezelés|Összekötő regisztrálása|
|Alkalmazáskezelés|AdminPolicyDatas-RemoveResources|
|Alkalmazáskezelés|AdminPolicyDatas-SetResources|
|Alkalmazáskezelés|AdminUserJourneys-GetResources|
|Címtárkezelés|AdminUserJourneys-RemoveResources|
|Címtárkezelés|AdminUserJourneys-SetResources|
|Címtárkezelés|IdentityProvider létrehozása|
|Címtárkezelés|Új AdminUserJourney létrehozása|
|Címtárkezelés|Honosított json-erőforrás létrehozása|
|Címtárkezelés|Új egyéni identitásszolgáltató létrehozása|
|Címtárkezelés|Új identitásszolgáltató létrehozása|
|Címtárkezelés|B2C címtárerőforrás létrehozása vagy frissítése|
|Címtárkezelés|Szabályzat létrehozása|
|Címtárkezelés|trustFramework szabályzat létrehozása|
|Címtárkezelés|Konfigurálható előtaggal rendelkező trustFramework szabályzat létrehozása|
|Címtárkezelés|Felhasználói attribútum létrehozása|
|Címtárkezelés|CreateTrustFrameworkPolicy|
|Címtárkezelés|Identitásszolgáltató törlése|
|Címtárkezelés|IdentityProvider törlése|
|Címtárkezelés|B2C címtárerőforrás törlése|
|Címtárkezelés|trustFramework szabályzat törlése|
|Címtárkezelés|Felhasználói attribútum törlése|
|Címtárkezelés|Erőforráscsoporthoz tartozó B2C címtárerőforrások lekérése|
|Címtárkezelés|Előfizetéshez tartozó B2C címtárerőforrások lekérése|
|Címtárkezelés|Egyéni identitásszolgáltató lekérése|
|Címtárkezelés|Identitásszolgáltató lekérése|
|Címtárkezelés|B2C címtárerőforrás lekérése|
|Címtárkezelés|Felhasználói út lekérése|
|Címtárkezelés|A felhasználói út engedélyezett alkalmazásjogcímeinek lekérése|
|Címtárkezelés|A felhasználói út engedélyezett önellenőrzött jogcímeinek lekérése|
|Címtárkezelés|A szabályzat engedélyezett önellenőrzött jogcímeinek lekérése|
|Címtárkezelés|Elérhető kimeneti jogcímek listájának lekérése|
|Címtárkezelés|A felhasználói út tartalomdefinícióinak lekérése|
|Címtárkezelés|Adott rendszergazdai folyamathoz tartozó identitásszolgáltatók lekérése|
|Címtárkezelés|Az összes rendszergazdai folyamat|
|Címtárkezelés|Az összes felhasználó összes rendszergazdai folyamatához tartozó címkék listájának lekérése|
|Csoportkezelés|Felhasználó bérlői listájának lekérése|
|Csoportkezelés|Helyi fiókok önellenőrző jogcímeinek lekérése|
|Csoportkezelés|Honosított json-erőforrás lekérése|
|Csoportkezelés|A Microsoft.AzureActiveDirectory erőforrás-szolgáltató műveleteinek lekérése|
|Csoportkezelés|Szabályzatok lekérése|
|Csoportkezelés|Szabályzat lekérése|
|Csoportkezelés|Támogatott identitásszolgáltatók listájának lekérése|
|Csoportkezelés|A felhasználói út támogatott identitásszolgáltatói listájának lekérése|
|Csoportkezelés|Bérlő által definiált egyéni identitásszolgáltatók listájának lekérése|
|Csoportkezelés|Bérlő által definiált identitásszolgáltatók listájának lekérése|
|Csoportkezelés|Bérlő által definiált helyi identitásszolgáltató-lista lekérése|
|Csoportkezelés|Felhasználó bérlői információinak lekérése erőforrások létrehozásához|
|Csoportkezelés|A CPIM alapértelmezett támogatott kultúrájának lekérése|
|Csoportkezelés|Rendszergazdai folyamathoz tartozó információk lekérése|
|Csoportkezelés|A bérlőhöz tartozó UserJourneys lista lekérése|
|Csoportkezelés|A CPIM-hez tartozó elérhető, támogatott kultúrák készletének lekérése|
|Csoportkezelés|TrustFramework szabályzat lekérése|
|Csoportkezelés|TrustFramework szabályzat lekérése xml-ként|
|Csoportkezelés|Felhasználói attribútum lekérése|
|Szabályzatkezelés|Felhasználói attribútumok lekérése|
|Szabályzatkezelés|Felhasználói utak listájának lekérése|
|Szabályzatkezelés|GetIEFPolicies|
|Szabályzatkezelés|GetIdentityProviders|
|Szabályzatkezelés|GetTrustFrameworkPolicy|
|Erőforrás|MigrateTenantMetadata|
|Erőforrás|Erőforrások áthelyezése|
|Erőforrás|IdentityProvider javítása|
|Erőforrás|PutTrustFrameworkPolicy|
|Erőforrás|PutTrustFrameworkpolicy|
|Erőforrás|Felhasználói út eltávolítása|
|Erőforrás|Egyéni identitásszolgáltató frissítése|
|Erőforrás|Identitásszolgáltató frissítése|
|Erőforrás|Helyi identitásszolgáltató frissítése|
|Erőforrás|B2C címtárerőforrás frissítése|
|Erőforrás|Szabályzat frissítése|
|Erőforrás|Előfizetés állapotának frissítése|
|Szerepkörkezelés|Felhasználói attribútum frissítése|
|Szerepkörkezelés|Erőforrások áthelyezésének ellenőrzése|
|Szerepkörkezelés|Eszköz hozzáadása|
|Szerepkörkezelés|Eszközkonfiguráció hozzáadása|
|Szerepkörkezelés|Regisztrált felhasználó hozzáadása eszközhöz|
|Szerepkörkezelés|Regisztrált felhasználók hozzáadása eszközhöz|
|Szerepkörkezelés|Eszköz törlése|
|Szerepkörkezelés|Eszköz konfigurációjának törlése|
|Szerepkörkezelés|Az eszköz már nem megfelelő|
|Szerepkörkezelés|Az eszköz már nincs felügyelve|
|Felhasználókezelés|Regisztrált felhasználó eltávolítása az eszközről|
|Felhasználókezelés|Regisztrált felhasználók eltávolítása az eszközről|
|Felhasználókezelés|Eszköz frissítése|
|Felhasználókezelés|Eszköz konfigurációjának frissítése|
|Felhasználókezelés|Jogosult tag hozzáadása szerepkörhöz|
|Felhasználókezelés|Tag hozzáadása szerepkörhöz|
|Felhasználókezelés|Szerepkör-hozzárendelés hozzáadása szerepkör-definícióhoz|
|Felhasználókezelés|Szerepkör hozzáadása sablonból|
|Felhasználókezelés|Hatókörön belüli tag hozzáadása a szerepkörhöz|
|Felhasználókezelés|Jogosult tag eltávolítása a szerepkörből|
|Felhasználókezelés|Tag eltávolítása szerepkörből|
|Felhasználókezelés|Szerepkör-hozzárendelés eltávolítása a szerepkör-definícióból|
|Felhasználókezelés|Hatókörön belüli tag eltávolítása a szerepkörből|
|Felhasználókezelés|Szerepkör frissítése|
|Felhasználókezelés|AccessReview_Review|
|Felhasználókezelés|AccessReview_Update|
|Felhasználókezelés|ActivationAborted|
|Felhasználókezelés|ActivationApproved|
|Felhasználókezelés|ActivationCanceled|
|Felhasználókezelés|ActivationRequested|
|Felhasználókezelés|Hozzáadva|
|Felhasználókezelés|Hozzárendelés|


## <a name="identity-protection"></a>Identitásvédelem

|Naplózási kategória|Tevékenység|
|---|---|
|Címtárkezelés|Jogosultságszint emelése|
|Címtárkezelés|Eltávolítva|
|Címtárkezelés|Szerepkör-beállítások módosításai|
|Egyéb|ScanAlertsNow|
|Egyéb|Regisztráció|
|Egyéb|Jogosultságszint csökkentése|
|Egyéb|UpdateAlertSettings|
|Egyéb|UpdateCurrentState|
|Szabályzatkezelés|Hozzáférési felülvizsgálat befejezve|
|Szabályzatkezelés|Jóváhagyó hozzáadása a kérelem-jóváhagyáshoz|
|Szabályzatkezelés|Felülvizsgáló hozzáadása a hozzáférési felülvizsgálathoz|
|Felhasználókezelés|Hozzáférési felülvizsgálat alkalmazása|
|Felhasználókezelés|Hozzáférési felülvizsgálat létrehozása|


## <a name="invited-users"></a>Meghívott felhasználók

|Naplózási kategória|Tevékenység|
|---|---|
|Egyéb|Kérelem-jóváhagyás létrehozása|
|Egyéb|Hozzáférési felülvizsgálat törlése|
|Felhasználókezelés|Felülvizsgáló eltávolítása a hozzáférési felülvizsgálatból|
|Felhasználókezelés|Felülvizsgálat-alkalmazás eredményének kérése|
|Felhasználókezelés|Felülvizsgálat leállításának kérése|
|Felhasználókezelés|Alkalmazás-hozzárendelés felülvizsgálata|
|Felhasználókezelés|Csoporttagság felülvizsgálata|
|Felhasználókezelés|RBAC-szerepkörtagság felülvizsgálata|


## <a name="microsoft-identity-manager-mim"></a>Microsoft Identity Manager (MIM)

|Naplózási kategória|Tevékenység|
|---|---|
|Csoportkezelés|Kérelem-jóváhagyási kérelem felülvizsgálata|
|Csoportkezelés|Hozzáférési felülvizsgálat frissítése|
|Csoportkezelés|Hozzáférési felülvizsgálati e-mail-értesítési beállításainak frissítése|
|Csoportkezelés|Hozzáférési felülvizsgálat ismétlődésszám-beállításának frissítése|
|Csoportkezelés|Hozzáférési felülvizsgálat napokban megadott ismétlődési időtartamának frissítése|
|Felhasználókezelés|Hozzáférési felülvizsgálat ismétlődési befejezéstípusának frissítése|
|Felhasználókezelés|Hozzáférési felülvizsgálat ismétlődéstípus-beállításának frissítése|



## <a name="privileged-identity-management"></a>Privileged Identity Management

|Naplózási kategória|Tevékenység|
|---|---|
|Szerepkörkezelés|Hozzáférési felülvizsgálati emlékeztető beállításainak frissítése|
|Szerepkörkezelés|Kérelem-jóváhagyás frissítése|
|Szerepkörkezelés|Alkalmazásszerepkör-hozzárendelés megadása a felhasználónak|
|Szerepkörkezelés|Felhasználó hozzáadása|
|Szerepkörkezelés|Felhasználók erős hitelesítési telefonos alkalmazásra vonatkozó információinak hozzáadása|
|Szerepkörkezelés|Felhasználói licenc módosítása|
|Szerepkörkezelés|Felhasználói jelszó módosítása|
|Szerepkörkezelés|Összevont felhasználó konvertálása felügyeltre|
|Szerepkörkezelés|Alkalmazásjelszó létrehozása a felhasználó számára|
|Szerepkörkezelés|Felhasználó alkalmazásjelszavának törlése|
|Szerepkörkezelés|Felhasználó törlése|
|Szerepkörkezelés|Fiók letiltása|
|Szerepkörkezelés|Erős hitelesítés engedélyezése|
|Szerepkörkezelés|Felhasználó végleges törlése|
|Szerepkörkezelés|Alkalmazásszerepkör-hozzárendelés eltávolítása a felhasználóból|
|Szerepkörkezelés|Felhasználók erős hitelesítési telefonos alkalmazásra vonatkozó információinak eltávolítása|



## <a name="self-service-group-management"></a>Önkiszolgáló csoportkezelés

|Naplózási kategória|Tevékenység|
|---|---|
|Csoportkezelés|Új felhasználói jelszó kérése|
|Csoportkezelés|Felhasználó visszaállítása|
|Csoportkezelés|Felhasználói jelszó kényszerített módosításának beállítása|
|Csoportkezelés|Felhasználókezelő beállítása|
|Csoportkezelés|A felhasználók oath token metaadatainak engedélyezettre állítása|
|Csoportkezelés|StsRefreshTokenValidFrom időbélyeg frissítése|
|Csoportkezelés|Külső titkos kódok frissítése|
|Csoportkezelés|Felhasználó frissítése|
|Csoportkezelés|A rendszergazda ideiglenes jelszót hoz létre|


## <a name="self-service-password-management"></a>Önkiszolgáló jelszókezelés

|Naplózási kategória|Tevékenység|
|---|---|
|Címtárkezelés|A rendszergazda kötelezővé teszi a felhasználó számára az új jelszó kérését|
|Címtárkezelés|Külső felhasználó hozzárendelése alkalmazáshoz|
|Felhasználókezelés|E-mail nincs elküldve, a felhasználó leiratkozott|
|Felhasználókezelés|Külső felhasználó meghívása|
|Felhasználókezelés|Külső felhasználó meghívásának beváltása|
|Felhasználókezelés|Bérlő ugrásszerű létrehozása|
|Felhasználókezelés|Felhasználó ugrásszerű létrehozása|
|Felhasználókezelés|Felhasználói jelszó regisztrációja|
|Felhasználókezelés|Új felhasználói jelszó kérése|
|Felhasználókezelés|Új jelszó önkiszolgáló kérésének blokkolása|


## <a name="terms-of-use"></a>Használati feltételek

|Naplózási kategória|Tevékenység|
|---|---|
|Szabályzatkezelés|Jelszó módosítása (önkiszolgáló)|
|Szabályzatkezelés|Jelszó átállítása (a rendszergazda által)|
|Szabályzatkezelés|Új jelszó kérése (önkiszolgáló)|
|Szabályzatkezelés|Új jelszó önkiszolgáló kérésének tevékenységállapota|
|Szabályzatkezelés|Új jelszó önkiszolgáló kérésének tevékenységállapota|
|Szabályzatkezelés|Felhasználói fiók zárolásának feloldása (önkiszolgáló)|
|Szabályzatkezelés|A felhasználó új jelszó önkiszolgáló kérésére regisztrált|




## <a name="next-steps"></a>További lépések

Az áttekintéseket a következőképpen érheti el:

- Jelentéskészítés: lásd az [Azure Active Directory jelentéskészítési útmutatót](overview-reports.md).

- Naplózott tevékenységekre vonatkozó jelentések: lásd a [naplózott tevékenységekre vonatkozó jelentésekről szóló útmutatót az Azure Active Directory portálon](concept-audit-logs.md). 


---
title: PaaS üzemelő példányainak biztonságossá tétele |} A Microsoft Docs
description: " Ismerje meg a biztonsági előnyöket, paas és más felhőalapú szolgáltatás, és biztonságossá tétele az Azure PaaS üzemelő példány vonatkozó ajánlott eljárások megismerése. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: techlake
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: terrylan
ms.openlocfilehash: 35650eec65fa9181d035c52e6b466985b483500c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036507"
---
# <a name="securing-paas-deployments"></a>PaaS üzemelő példányainak biztonságossá tétele

Ez a cikk, amely segítséget nyújt információkat:

- Megismerheti az alkalmazások felhőbeli üzemeltetéséhez biztonsági előnyeit
- A platform előnyeinek értékelődik ki a platformszolgáltatás (PaaS) és más cloud service-modellek
- Módosítja a biztonsági fókusz egy hálózati-központú egy identitás-központú szegélyhálózat-alapú biztonsági módszer
- Általános PaaS biztonsági ajánlott eljárások javaslatok megvalósítása

## <a name="cloud-security-advantages"></a>Felhőbeli biztonsági előnyeit
Nincsenek biztonsági előnyök, hogy a felhőben. A helyszíni környezetben, valószínűleg a szervezet rendelkezik a teljesítetlen feladatokról, valamint korlátozott erőforrások biztonsági, amely létrehoz egy környezetet, ahol a támadók képesek minden rétegen biztonsági réseit támogatásán érhető el.

![Felhő korszakának biztonsági előnyeit][1]

Olyan vállalatok, amelyek egy szolgáltató felhőalapú biztonsági képességeket és a felhőbeli intelligens technológiák használatával növelhető a fenyegetések észlelése és a válaszidőket.  A felhőszolgáltató feladatkörök kapacitásértékek szervezetek további biztonsági lefedettség, amely lehetővé teszi, hogy foglalja le újból a biztonsággal kapcsolatos információforrások és más üzleti prioritásokhoz költségvetés kérheti le.

## <a name="division-of-responsibility"></a>A felelősségi körzet
Fontos tudni, hogy az osztálynak a között, és a Microsoft felelőssége. A helyszínen, a teljes verem a saját, de a felhőre váltáskor látnia néhány feladatkört átvitele a Microsoft. A következő feladata mátrix jeleníti meg a verem környezetben SaaS, PaaS és IaaS, amelynek Ön a felelős, és a Microsoft felelős.

![Felelősség zónák][2]

Az összes felhőbeli központi telepítési típusok meg saját adatait és identitások. Ön felelős az adatait és identitások, a helyszíni erőforrások és a felhő összetevők védelme szabályozni (amely a szolgáltatás típusa szerint változó).

Feladatai, amelyek mindig maradnak, függetlenül a típusú telepítés, akkor a következők:

- Adatok
- Végpontok
- Fiók
- Hozzáférés-kezelés

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Egy a PaaS előnyei biztonsági cloud service-modell
Ugyanaz a felelősségi mátrix használja, tekintsük át azokat egy Azure PaaS üzemelő példány és a helyszíni biztonsági előnyeit.

![A PaaS biztonsági előnyei][3]

A veremben, a fizikai infrastruktúra alján indítása Microsoft csökkenti gyakori kockázatok és feladatokról. Mivel a Microsoft folyamatosan figyeli a Microsoft cloud, meglehetősen nehéz a támadásokkal szemben. A támadó szerezni a cél a Microsoft cloud nincs értelme. Kivéve, ha a támadó rendelkezik a nagy mennyiségű költséget takaríthat meg és erőforrásokat, a támadó valószínűleg áthelyezése egy másik cél.  

A verem közepén nem helyszíni és a egy PaaS üzemelő példány között nincs különbség. Az alkalmazási rétegre és a fiók és a hozzáférési réteg hogy hasonló kockázatokat. Következő lépések szakaszban Ez a cikk végigvezeti az ajánlott eljárások a elkerüléséhez, vagy ez a kockázat minimalizálása.

A stack, az adatszabályozást, és a rights management tetején, végrehajtása a egy kockázata, hogy a kulcskezelő enyhíthetők. (Ajánlott eljárások a kulcskezelés vonatkozik.) Egy további felelősségére, amíg egy PaaS üzemelő példány már nem kell erőforrásokat is át a kulcskezelés kezelheti a területek rendelkezik.

Az Azure platform is biztosít erős a DDoS protection különféle a hálózatalapú technológiák használatával. Mindenfajta hálózati alapú DDoS elleni védelmi módszerek azonban azok korlátokkal rendelkeznek a hivatkozás és adatközpont alapján. Nagy DDoS-támadások hatásainak elkerülése érdekében, kihasználhatja az Azure core felhőalapú méretezhetőségi, lehetővé téve a gyors és automatikus horizontális felskálázási DDoS-támadásokkal szembeni védelmét. További részletek a hogyan ezt megteheti az ajánlott eljárásokat cikkekben olvashatja.

## <a name="modernizing-the-defenders-mindset"></a>A defender így modernizálhatja
A paas üzemelő példányok az általános megközelítés a shift jár biztonsági. Át a vezérlőelem minden saját kezűleg felelőssége megosztása a Microsofttal kellene.

Egy másik jelentős PaaS és a hagyományos helyszíni üzemelő példányok közötti különbség egy új nézetet, mi határozza meg az elsődleges biztonsági határt. Hagyományosan az elsődleges helyszíni biztonsági határ a hálózat és a legtöbb a helyszíni biztonsági tervek az elsődleges biztonsági pivot használja a hálózati. PaaS üzemelő példányok akkor jobban által kiszolgált identitás, az elsődleges biztonsági határt is figyelembe véve.

## <a name="adopt-a-policy-of-identity-as-the-primary-security-perimeter"></a>Az elsődleges biztonsági határt, egy házirendet az identitás elfogadása
A felhő-számítástechnika a széleskörű hálózati hozzáférés, ami lehetővé teszi a hálózat-központú felhőmegoldásokat kevésbé jelentős öt alapvető jellemzőt egyikét. A cél az a felhő-számítástechnika lehetővé teszi a felhasználók hozzáférhessenek az erőforrásokhoz, helytől függetlenül jelentős részét. A felhasználók többsége számára azok helyétől fog kell valahol az interneten.

Az alábbi ábra bemutatja, hogyan a biztonsági határt alakult az a hálózat pereme, az identitás szegélyhálózat-alapú. Biztonsági kapcsolatban nyújt a hálózat védelmébe, és további információk az adatok védelme, valamint a biztonság, az alkalmazások és felhasználók kezelése válik. A legfontosabb különbség, hogy szeretné-e biztonsági közelebb leküldése milyen fontos, hogy a vállalat.

![Új biztonsági határt azonosító][4]

Kezdetben az Azure PaaS-szolgáltatások (például a webes szerepkörök és az Azure SQL) megadott alig vagy egyáltalán nem hagyományos hálózati szegélyhálózat-alapú védelem. Volt egyetértés, hogy az elem célja volt az internethez (webes szerepkörből) kell tenni, és ennek a hitelesítésnek biztosít az új szegélyhálózat-alapú (például BLOB vagy Azure SQL).

A modern biztonsági eljárások azt feltételezik, hogy a támadó áthatolt a hálózat szegélyén. Ezért a modern defense eljárások identitás helyezte át. Szervezetek számára, létre kell hoznia egy biztonsági azonosító-alapú szegélyhálózati az erős hitelesítési és engedélyezési higiéniai (ajánlott).

Alapelvek és a hálózat szegélyén mintákat volt elérhető évtizedes. Ezzel szemben az iparágaknak is megvannak a viszonylag kevesebb az identitást használja, mint az elsődleges biztonsági határt felhasználói. Az adott mondta hogy rendelkezik halmozott elegendő élményt biztosít, amely a mezőben bizonyítottan és a alkalmazni kell szinte az összes PaaS-szolgáltatások általános ajánlásokat.

Az alábbiakban az identitás peremhálózati kezelésére vonatkozó ajánlott eljárások.

**Ajánlott eljárás**: a kulcsokat és a PaaS üzemelő példány biztonságos hitelesítő adatok védelme.   
**Részletes**: kulcsok és hitelesítő adatok elvesztése gyakori probléma. Egy központi megoldás használható, ahol kulcsok és titkos kulcsok tárolhatók a hardveres biztonsági modulok. Az Azure biztosít a felhőben a hardveres biztonsági modul [Azure Key Vault](../key-vault/key-vault-whatis.md).

**Ajánlott eljárás**: hitelesítő adatok és egyéb titkok nem állítja, a forráskód vagy a GitHub.   
**Részletes**: csak rosszabb, mint a kulcsok és hitelesítő adatok elvesztése kellene a jogosulatlan személyek érni őket. A támadók kihasználhatja a robot technológiák kulcsok és titkok forráskódtárakból, például a GitHub tárolt található. Ne tegye kulcs és titkos kulcsok az alábbi nyilvános kódtárházakhoz.

**Ajánlott eljárás**: a virtuális gép kezelőfelületek, a hibrid PaaS és IaaS-szolgáltatások, amely lehetővé teszi a távoli kezelőfelület használatával ezek a virtuális gépek közvetlenül kezelhetnek.   
**Részletes**: távoli felügyeleti protokollok, mint például [SSH](https://en.wikipedia.org/wiki/Secure_Shell), [RDP](https://support.microsoft.com/kb/186607), és [PowerShell távoli eljáráshívás](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) is használható. Általában azt javasoljuk, hogy nem engedélyezi a közvetlen távelérési virtuális géphez az internetről.

Ha lehetséges használjon alternatív módszerek, például a virtuális magánhálózatok használata az Azure-beli virtuális hálózathoz. Ha alternatív módszerek nem érhetők el, győződjön meg arról, összetett jelszavak és a kétfaktoros hitelesítés használatát (például [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md)).

**Ajánlott eljárás**: erős hitelesítési és engedélyezési platformot használja.   
**Részletes**: használja az összevont identitások kialakítása helyett egyéni felhasználói tárolókkal Azure AD-ben. Ha összevont identitások kialakítása használja, a platform-alapú megközelítés előnyeit, és delegálja a partnerek számára engedélyezett azonosítók kezelését. Egy összevont identitáskezelési megközelítés különösen fontos, amikor az alkalmazottak megszűnik, és információkat kell több identitáskezelési és engedélyezési rendszer hatással lehet.

Használja a platform által biztosított hitelesítési és engedélyezési mechanizmusokkal egyéni kód helyett. A hiba oka egyéni kód fejlesztése, hogy a hibalehetőségeket rejt magában. A fejlesztők a legtöbb nem biztonsági szakértők és nem valószínű, hogy az apró és a legújabb fejlemények a hitelesítés és engedélyezés. Kereskedelmi-kódokat (például Microsoft), gyakran alaposan tekintse át biztonsági.

A kétfaktoros hitelesítést használni. A kétfaktoros hitelesítés hitelesítés és engedélyezés a jelenlegi standard azért nem szükséges a felhasználónév és jelszó típusú hitelesítés járó biztonsági gyenge. A (portal/távoli PowerShell) az Azure felügyeleti felületek és az ügyfelek által használt szolgáltatások is hozzáférést kell kell megtervezni és használatára konfigurált [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md).

Szabványon alapuló hitelesítési protokollok, például OAuth2 és a Kerberos használata. Ezeket a protokollokat nagymértékben lett felülvizsgálva társ, és valószínűleg valósíthatók meg a platform-kódtárak a hitelesítéshez és engedélyezéshez részeként.

## <a name="use-threat-modeling-during-application-design"></a>Alkalmazás-tervezés során modellezési threat használata
A Microsoft [biztonsági fejlesztési életciklus](https://www.microsoft.com/en-us/sdl) Megadja, hogy a fenyegetések modellezése a tervezési fázis során nevű folyamat csapatok kell végezhetnek. Ez a folyamat megkönnyítése érdekében a Microsoft hozott létre a [SDL Threat Modeling Tool](../security/azure-security-threat-modeling-tool.md). Az alkalmazás-tervezés modellezési és számbavétele [STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy) fenyegetések közötti összes megbízhatósági határait használhatják fel őket tervezési hibák már a legelején.

Az alábbi táblázat STRIDE fenyegetések listázása, és bizonyos Azure-funkciókat használó példa megoldások biztosít. Ezek a megoldások minden helyzetben nem fog működni.

| Fenyegetés | Biztonsági tulajdonság | A lehetséges az Azure platform migrálása |
| --- | --- | --- |
| Címek hamisítása | Hitelesítés | HTTPS-kapcsolatokat igényelnek. |
| Illetéktelen módosítása | Integritás | SSL-tanúsítványok ellenőrzése. |
| Letagadhatóság | Letagadhatatlanság | Engedélyezze az Azure [monitorozási és diagnosztikai](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). |
| Információk felfedése | Bizalmas | Az inaktív bizalmas adatok titkosítása az [szolgáltatási tanúsítványok](https://docs.microsoft.com/rest/api/appservice/certificates). |
| Szolgáltatásmegtagadás | Rendelkezésre állás | Teljesítmény-mérőszámok a lehetséges szolgáltatásmegtagadás-feltételek figyelése Kapcsolat szűrők megvalósításához. |
| Jogok kiterjesztése | Engedélyezés | Használat [Privileged Identity Management](../active-directory/privileged-identity-management/subscription-requirements.md). |

## <a name="develop-on-azure-app-service"></a>Fejlesztés az Azure App Service-ben
[Az Azure App Service](../app-service/app-service-web-overview.md) egy PaaS-ajánlat, amely lehetővé teszi a webes és mobilalkalmazásokat bármilyen platformra vagy eszközre létrehozása, és bárhol kapcsolódhatnak az adatokhoz, a felhőben vagy helyszíni. App Service-ben a webes és mobil funkciókkal, amelyeket a rendszer korábban külön-külön tartalmazott Azure-webhelyek és Azure Mobile Services tartalmazza. Ezenkívül új lehetőségek is elérhetők az üzleti folyamatok automatizálásához és felhőalapú API-k üzemeltetéséhez. Egyetlen integrált szolgáltatásként az App Service lehetőségek gazdag tárházaként segíti a lehetőségeket a webes, mobil és integrációs feladatokhoz.

Az alábbiakban az App Service használatának ajánlott eljárásai.

**Ajánlott eljárás**: [hitelesítés Azure Active Directoryn keresztül](../app-service/app-service-authentication-overview.md).   
**Részletes**: App Service-ben az identitásszolgáltató az OAuth 2.0 szolgáltatást biztosít. OAuth 2.0 ügyfél fejlesztői egyszerűség művelet során gondoskodik a webalkalmazások, asztali alkalmazások és mobiltelefonon adott engedélyezési folyamatok tárgyalja. Az Azure AD OAuth 2.0 használja ahhoz, hogy engedélyezze a hozzáférést mobil- és webalkalmazásokat.

**Ajánlott eljárás**: meg kell tudnia és a legalacsonyabb jogosultsági biztonsági alapelveket alapján korlátozhatja a hozzáférést.   
**Részletes**: hozzáférés korlátozása elengedhetetlen a szervezetek által adatok elérésére vonatkozó biztonsági szabályzatok kikényszerítéséhez. Az RBAC használatával engedélyek hozzárendelése a felhasználók, csoportok és alkalmazások egy bizonyos hatókörben. Felhasználók alkalmazásokhoz való hozzáférésének engedélyezésére vonatkozó további tudnivalókért lásd: [hozzáférés-kezelés – első lépések](../role-based-access-control/overview.md).

**Ajánlott eljárás**: Ön kulcsainak védelmét.   
**Részletes**: Azure Key Vault segít a kriptográfiai kulcsokat és titkos kódok, amelyek felhőalapú alkalmazásokat és szolgáltatásokat használja. A Key Vault titkosíthatók az kulcsok és titkos kulcsokat (például hitelesítési kulcsokat, a tárfiók kulcsait, az adattitkosítási kulcsokat. PFX-fájlok és jelszavak) hardveres biztonsági modulokban (HSM) által védett kulcsok használatával. A még nagyobb biztonság érdekében lehetőség van arra is, hogy kulcsokat importáljon és generáljon a hardveres biztonsági modulokban. Lásd: [Azure Key Vault](../key-vault/key-vault-whatis.md) további. A Key Vault használatával kezelheti a TLS-tanúsítványok az automatikus megújítás.

**Ajánlott eljárás**: bejövő forrás IP-címek korlátozása.   
**Részletes**: [App Service Environment-környezet](../app-service/environment/intro.md) rendelkezik, amely segítséget nyújt a hálózati biztonsági csoportokon keresztül bejövő forrás IP-címek korlátozása virtuális hálózati integráció funkcióval. Virtuális hálózatok lehetővé teszik Azure-erőforrások elhelyezni, hogy ki férhet hozzá az internet, az irányítható hálózatban. További tudnivalókért lásd: [az alkalmazás integrálása az Azure-beli virtuális hálózathoz](../app-service/web-sites-integrate-with-vnet.md).

**Ajánlott eljárás**: az App Service Environment-környezetek biztonsági állapotának figyelése.   
**Részletes**: az App Service Environment-környezetek figyelése az Azure Security Center. A Security Center azonosítja a potenciális biztonsági réseket, amikor létrehozza [javaslatok](../security-center/security-center-virtual-machine-recommendations.md) , amely végigvezeti Önt a szükséges vezérlők konfigurálásának folyamatán.

> [!NOTE]
> Monitoring App Service-ben van és elérhető előzetes verzióban kizárólag a a [Standard szintű](../security-center/security-center-pricing.md) , a Security Center.
>
>

## <a name="install-a-web-application-firewall"></a>Webalkalmazási tűzfal telepítése
A webalkalmazások egyre inkább ki vannak téve rosszindulatú támadásoknak, amelyek az ismert biztonsági réseket használják ki. Az ilyen jellegű támadások között például gyakoriak az SQL-injektálásos és a webhelyek közötti, parancsprogramot alkalmazó támadások. Az ilyen támadások megakadályozása az alkalmazás kódjában kihívást jelenthet, és szigorú felügyeletet, javítást és megfigyelést igényelhet az alkalmazás topológiájának számos rétegén. A központosított webalkalmazási tűzfal egyszerűbbé teszi a biztonságfelügyeletet, és segít az alkalmazás-rendszergazdáknak a fenyegetések vagy a behatolások elleni védekezésben. Emellett a WAF-megoldás gyorsabban képes kezelni a biztonsági fenyegetéseket azáltal, hogy kijavítja az ismert biztonsági réseket egy központi helyen, ahelyett hogy az egyes webalkalmazások védelmét biztosítaná. A meglévő alkalmazásátjárókat egyszerűen át lehet alakítani webalkalmazási tűzfallal rendelkező alkalmazásátjárókká.

[Webalkalmazási tűzfal (WAF)](../application-gateway/waf-overview.md) egy szolgáltatás, Application Gateway, amelyek a webalkalmazásoknak a gyakori támadások és biztonsági rések központi védelmet nyújt. WAF-szabályai alapján a [Open Web Application Security Project (OWASP) alapvető szabálykészletek](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0-s vagy 2.2.9-es verzióinak.

## <a name="monitor-the-performance-of-your-applications"></a>Az alkalmazások teljesítményének figyelése
Figyelés gyűjtése és elemzése a teljesítmény, egészségügyi és az alkalmazás rendelkezésre állásának meghatározása az adatokat. A hatékony monitorozási stratégia pontos információkat nyújt az alkalmazás összetevőinek működéséről. Ez segít növelni az üzemidőt értesítése a kritikus fontosságú problémákról, így fel lehet számolni őket, mielőtt azok veszélyeztetnék. Ez ráadásul segít jobban észlelje a rendellenességeket, amelyekre kapcsolatos biztonsági lehet.

Használat [Azure Application Insights](http://azure.microsoft.com/documentation/services/application-insights) figyelése rendelkezésre állását, teljesítményét és használatát az alkalmazás, akár a felhőben vagy a helyszínen vannak tárolva. Application Insights segítségével gyorsan azonosíthatja és diagnosztizálhatja a hibákat az alkalmazásban egy felhasználó jelenti azokat várakozás nélkül. A gyűjtött információk alapján megalapozott döntéseket hozhat az alkalmazás karbantartásával és továbbfejlesztésével kapcsolatban.

Az Application Insights kiterjedt eszközkészlettel rendelkezik az általa gyűjtött adatok feldolgozásához. Az Application Insights egy általános adattárban tárolja az adatait. Képes a különféle közös funkciók, például a riasztások, irányítópultok és részletes elemzés előnyeinek kihasználására a Log Analytics lekérdezési nyelvének használatával.



## <a name="next-steps"></a>További lépések
Ebben a cikkben összpontosítottunk egy Azure PaaS üzemelő példány és a felhőalapú alkalmazásokhoz ajánlott biztonsági eljárások a biztonsági előnyöket. Következő lépésként megtanulhatja, ajánlott eljárások a PaaS webes és mobil megoldások használata az adott Azure-szolgáltatások biztosításához. Kezdjük az Azure App Service-ben, az Azure SQL Database és az Azure SQL Data Warehouse és az Azure Storage. Amint elérhetővé válnak más Azure-szolgáltatások az ajánlott eljárásokat cikkeket, hivatkozások lesznek közzétéve, az alábbi lista:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Az Azure SQL Database és az Azure SQL Data warehouse-bA](security-paas-applications-using-sql.md)
- [Azure Storage](security-paas-applications-using-storage.md)
- Az Azure REDIS Cache
- Azure Service Bus
- Webalkalmazás-tűzfalak

Lásd: [Azure ajánlott biztonsági eljárások és minták](security-best-practices-and-patterns.md) további ajánlott biztonsági eljárások szeretne használni, amikor a tervezése, telepítése, és a felhőalapú megoldások kezelése az Azure használatával.

Az alábbi forrásanyagokat biztosít az Azure biztonsági és a kapcsolódó Microsoft-szolgáltatások kapcsolatos általános információk érhetők el:
* [Az Azure Security csapat blogja](https://blogs.msdn.microsoft.com/azuresecurity/) – az Azure Security legújabb naprakész információk
* [A Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) – Ha a jelenteni lehet a Microsoft biztonsági réseket, beleértve a problémák az Azure-ral, vagy az e-mailen keresztül secure@microsoft.com

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png

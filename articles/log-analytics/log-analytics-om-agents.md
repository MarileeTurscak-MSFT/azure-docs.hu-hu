---
title: Az Operations Manager csatlakoztatása szolgáltatáshoz |} Microsoft Docs
description: A meglévő befektetések a System Center Operations Manager karbantartása, és kiterjesztett képességek használata Naplóelemzési, integrálható az Operations Manager a munkaterületen.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: magoedte
ms.openlocfilehash: 84eabef06b4d2ad71e6d9a947a77589f9159e030
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/08/2018
---
# <a name="connect-operations-manager-to-log-analytics"></a>Adatforrások csatlakoztatása az Operations Manager szolgáltatáshoz
A meglévő befektetések a System Center Operations Manager karbantartása, és kiterjesztett képességek használata Naplóelemzési, integrálható az Operations Manager Naplóelemzési munkaterületet.  Ez lehetővé teszi a Naplóelemzési lehetőségek használja ki, miközben továbbra is az Operations Manager használja:

* Az Operations Manager az informatikai szolgáltatások állapotának figyelése
* A támogatási incidensek és a probléma felügyeleti ITSM megoldásaival való integráció fenntartása
* A helyszíni és a nyilvános felhő infrastruktúra-szolgáltatási virtuális gépek, amelyek a figyelheti az Operations Manager telepített ügynökök életciklusának kezelését

System Center Operations Manager integrálása értéket ad hozzá a szolgáltatási műveletek stratégia a sebesség és a Log Analyticshez összegyűjtésének, tárolásának és az Operations Manager alkalmazásból adatok elemzése a hatékonyságát.  Jelentkezzen Analytics segítségével korrelálja, és a hibák, a problémák azonosítása és felszínre hozza az ismétlődések a meglévő probléma felügyeleti folyamat támogatásához.  A teljesítmény-, esemény- és adatok gazdag irányítópultok és az adatok közzétételét ésszerű módszerrel, a jelentéskészítési lehetőségeket mutatja be a erőssége Naplóelemzési riasztás vizsgálata keresőmotor rugalmasan számos lehetőséget kínál az Operations Manager complimenting.

Az Operations Manager felügyeleti csoportnak az ügynök adatokat gyűjteni a Log Analyticshez adatforrások és engedélyezve van a munkaterület megoldások alapján a kiszolgálók.  Attól függően, a megoldások engedélyezve van, az adatokat vagy kerülnek közvetlenül egy Operations Manager felügyeleti kiszolgálóról a szolgáltatás, vagy az ügynök által felügyelt rendszeren gyűjtött adatok mennyisége miatt Naplóelemzési küldött közvetlenül az ügynöktől. A felügyeleti kiszolgáló továbbítja az adatokat közvetlenül a szolgáltatás; Soha ne írás a működési vagy adatok adatraktár-adatbázisba.  Amikor egy felügyeleti kiszolgáló Log Analyticshez való kapcsolata megszakad, azt gyorsítótárazza az adatokat helyileg, amíg a kommunikációs helyreáll, a Naplóelemzési.  Ha a felügyeleti kiszolgáló előre tervezett karbantartások vagy nem tervezett leállás miatt offline állapotban, a felügyeleti csoport egy másik felügyeleti kiszolgálóra folytatja csatlakozni a szolgáltatáshoz.  

Az alábbi ábra mutatja a felügyeleti kiszolgálók és az ügynökök közötti kapcsolat a System Center Operations Manager felügyeleti csoport és a Naplóelemzési, beleértve a irányát és portjait is.   

![OMS-műveletek-manager-integráció-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

Ha az IT-biztonsági házirendeknek nem engedélyezi a számítógépek a hálózat csatlakozik az internethez, felügyeleti kiszolgáló beállítható úgy, hogy az OMS-átjárón, attól függően, hogy engedélyezve van a megoldások összegyűjtött adatok küldésére és fogadására a konfigurációs adatokat.  További információt és az Operations Manager felügyeleti csoportjának kommunikációja áthaladjon egy OMS-átjáró a Log Analytics szolgáltatás konfigurálásával kapcsolatos lépéseket, [számítógépeket csatlakoztatni az OMS Szolgáltatáshoz az OMS-átjáró](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Rendszerkövetelmények
Megkezdése előtt tekintse át az alábbi részleteket megfelel a szükséges előfeltételek ellenőrzése.

* A Naplóelemzési csak akkor támogatja a System Center Operations Manager 1801, Operations Manager 2016-Operations Manager 2012 SP1 UR6 és nagyobb, és az Operations Manager 2012 R2 UR2 és nagyobb.  A proxytámogatás az Operations Manager 2012 SP1 UR 7-es és az Operations Manager 2012 R2 UR 3-as verziójában jelent meg.
* Az összes Operations Manager-ügynökök meg kell felelnie a minimális támogatási követelményeknek. Győződjön meg arról, hogy ügynökök a minimális frissítéskor, ellenkező esetben a Windows-ügynök forgalom sikertelen lehet, hogy és sok hiba előfordulhat, hogy töltse ki az Operations Manager eseménynaplójában.
* A Naplóelemzési munkaterület.  További információkért tekintse át [Ismerkedés a Naplóelemzési](log-analytics-get-started.md).

### <a name="network"></a>Network (Hálózat)
Az alábbi lista a proxy és tűzfal konfigurációs adatokat, az Operations Manager-ügynök, felügyeleti kiszolgálók és operatív konzol Naplóelemzési folytatott kommunikációhoz szükséges információt.  Minden egyes összetevő forgalmát a Naplóelemzési szolgáltatás a hálózatról kimenő.     

|Erőforrás | Portszám| HTTP-ellenőrzés kihagyása|  
|---------|------|-----------------------|  
|**Ügynök**|||  
|\*.ods.opinsights.azure.com| 443 |Igen|  
|\*.oms.opinsights.azure.com| 443|Igen|  
|\*.blob.core.windows.net| 443|Igen|  
|\*.azure-automation.net| 443|Igen|  
|**Felügyeleti kiszolgáló**|||  
|\*.service.opinsights.azure.com| 443||  
|\*.blob.core.windows.net| 443| Igen|  
|\*.ods.opinsights.azure.com| 443| Igen|  
|*.azure-automation.net | 443| Igen|  
|**Operations Manager konzolján az OMS-be**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80-as és 443-as||  
|\*.microsoft.com| 80-as és 443-as||  
|\*.microsoftonline.com| 80-as és 443-as||  
|\*.mms.microsoft.com| 80-as és 443-as||  
|login.windows.net| 80-as és 443-as||  
|portal.loganalytics.io| 80-as és 443-as||
|api.loganalytics.io| 80-as és 443-as||
|docs.loganalytics.io| 80-as és 443-as||  

## <a name="connecting-operations-manager-to-log-analytics"></a>Csatlakozás az Operations Manager szolgáltatáshoz
Hajtsa végre az alábbi lépéseket az Operations Manager felügyeleti csoportjának csatlakozni a Naplóelemzési munkaterület egyik konfigurálásának lépéseit.

Ha első alkalommal regisztrál az Operations Manager felügyeleti csoportjának a Naplóelemzési munkaterület, és a felügyeleti kiszolgálók kell a szolgáltatás a proxy vagy az OMS átjárókiszolgáló, a beállítással adhatja meg a webproxy-konfigurációja keresztül kommunikálnak a felügyeleti csoport nem érhető el az operatív konzolon.  A felügyeleti csoportot ki kell sikeresen regisztrálva a szolgáltatásban érhető el ez a beállítás előtt.  Frissítenie kell a rendszer proxykonfigurációt a Netsh a rendszer a fut, az operatív konzol a konfigurálására-integráció és az összes felügyeleti kiszolgáló a felügyeleti csoportban.  

1. Nyisson meg egy rendszergazda jogú parancssort.
   a. Ugrás a **Start** és típus **cmd**.
   b. Kattintson a jobb gombbal **parancssor** , és jelölje ki futtató rendszergazda **.
2. Adja meg a következő parancsot, és nyomja le az **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Log Analytics integrálja a következő lépések végrehajtását követően eltávolíthatja a konfigurációs rendszert futtató `netsh winhttp reset proxy` , majd a **proxy kiszolgáló konfigurálása** az operatív konzolon a beállítást annak megadásához, a proxy vagy az OMS Szolgáltatáshoz Átjárókiszolgáló. 

1. Az Operations Manager konzolon válassza a **felügyeleti** munkaterületen.
2. Bontsa ki az Operations Management Suite csomópontot, és kattintson a **kapcsolat**.
3. Kattintson a **Regisztrálás az Operations Management Suite** hivatkozásra.
4. Az a **Operations Management Suite előkészítési varázslója: hitelesítés** lapon, az e-mail címet vagy telefonszámot, és az OMS-előfizetéshez társított rendszergazdai fiók jelszavát, majd kattintson **bejelentkezés**.
5. Miután Ön hitelesítése sikeres, a a **Operations Management Suite előkészítési varázslója: Munkaterület kiválasztása** lap, kéri a Naplóelemzési munkaterület kiválasztása.  Ha egynél több munkaterületen, válassza ki a munkaterület regisztrálása az Operations Manager felügyeleti csoportot a legördülő listából, és kattintson a kívánt **következő**.
   
   > [!NOTE]
   > Az Operations Manager csak egyszerre egy Naplóelemzési munkaterület támogat. A kapcsolat és a számítógépek az előző munkaterület szolgáltatáshoz regisztrált Naplóelemzési törlődnek.
   > 
   > 
6. Az a **Operations Management Suite előkészítési varázslója: Összegzés** lapon hagyja jóvá a beállításokat, ha azok a helyes-e, kattintson **létrehozása**.
7. Az a **Operations Management Suite előkészítési varázslója: Befejezés** kattintson **Bezárás**.

### <a name="add-agent-managed-computers"></a>Ügynök által felügyelt számítógépek hozzáadása
Integrációjának konfigurálása után az Naplóelemzési munkaterületet, ez csak a szolgáltatás kapcsolatot létesít, nincs adatgyűjtés az ügynök a felügyeleti csoportban. Ez nem fordulhat elő, amíg mely adott ügynök által felügyelt számítógépek adatokat gyűjt a Log Analyticshez konfigurálása után. Kiválaszthatja a számítógép-objektumok önállóan, vagy kiválaszthat egy csoportot, amely tartalmazza a Windows számítógép-objektumok. Nem választhat ki egy csoportot, amely egy másik osztály, például logikai lemezek vagy az SQL-adatbázis példányait tartalmazza.

1. Nyissa meg az Operations Manager-konzolt, és válassza ki az **Administration** (Adminisztráció) munkaterületet.
2. Bontsa ki az Operations Management Suite csomópontot, és kattintson a **kapcsolat**.
3. Kattintson a **egy számítógépcsoport hozzáadásához** alatt a műveletek a jobb oldali ablaktábla a fejléc.
4. Az a **számítógép keresése** párbeszédpanelen is kereshet számítógépek vagy csoportok az Operations Manager általi megfigyelés alatt. Válassza ki a számítógépek vagy csoportok érheti el a szolgáltatáshoz, kattintson **Hozzáadás**, és kattintson a **OK**.

Megtekintheti a számítógépeket és csoportokat szeretné az adatgyűjtést az Operations Management Suite a felügyelt számítógépek csomópontjából konfigurálva a **felügyeleti** munkaterület az operatív konzol.  Itt adja hozzá, vagy távolítsa el a számítógép- és szükség szerint.

### <a name="configure-proxy-settings-in-the-operations-console"></a>Proxybeállítások konfigurálása az operatív konzolon
Ha a felügyeleti csoport és a napló Analytics szolgáltatás között egy belső proxykiszolgáló, hajtsa végre az alábbi lépéseket.  Ezek a beállítások központilag a felügyeleti csoportból felügyelt és ügynök által felügyelt rendszerek, adatok gyűjtéséhez Naplóelemzési hatókörében szereplő kiosztva.  Ez az egyes megoldások kihagyása a felügyeleti kiszolgáló és adatküldéshez közvetlenül a szolgáltatás előnyös.

1. Nyissa meg az Operations Manager-konzolt, és válassza ki az **Administration** (Adminisztráció) munkaterületet.
2. Bontsa ki az Operations Management Suite, majd a **kapcsolatok**.
3. Az OMS Connection (OMS-kapcsolat) nézetben kattintson a **Configure Proxy Server** (Proxykiszolgáló konfigurálása) lehetőségre.
4. A **Operations Management Suite varázslója: proxykiszolgáló** lapon jelölje be **proxykiszolgáló használata az Operations Management Suite eléréséhez**, és írja be az URL-cím, egy portszám, például http://corpproxy:80 majd **Befejezés**.

Ha a proxykiszolgálóhoz hitelesítés szükséges, hajtsa végre a következő lépésekkel állíthatja be a hitelesítő adatok és beállítások propagálódik az OMS jelent a felügyeleti csoportban lévő felügyelt számítógépeket.

1. Nyissa meg az Operations Manager-konzolt, és válassza ki az **Administration** (Adminisztráció) munkaterületet.
2. A **RunAs Configuration** (RunAs-konfiguráció) területen válassza a **Profiles** (Profilok) lehetőséget.
3. Nyissa meg a **System Center Advisor Run As Profile Proxy** (System Center Advisor RunAs-profiljának proxyja) profilt.
4. Az a futtató profil varázsló kattintson a Hozzáadás gombra a Futtatás mint fiók használatára. Létrehozhat egy [Futtatás mint fiók](https://technet.microsoft.com/library/hh321655.aspx) vagy egy meglévő fiókot használjon. Ennek a fióknak rendelkeznie kell a megfelelő engedélyekkel a proxykiszolgálón való áthaladáshoz.
5. Kezelheti a fiók beállításához válassza **egy kijelölt osztály, csoport vagy objektum**, kattintson a **válasszon...** majd **csoporthoz...** lehetőségre a **csoport keresési** mezőbe.
6. Keresse meg és válassza ki **Microsoft System Center Advisor figyelési kiszolgáló csoport**.  Kattintson a **OK** zárja be a csoport kijelölése után a **csoport keresési** mezőbe.
7. Kattintson a **OK** bezárásához a **hozzáadása egy futtató fiókot** mezőbe.
8. Kattintson a **mentése** a varázsló befejezéséhez és a módosítások mentéséhez.

Után a kapcsolat jön létre, és mely ügynökök gyűjt és adatokat küldeni a Naplóelemzési konfigurál, a következő konfiguráció alkalmazása a felügyeleti csoport nem feltétlenül sorrendben:

* A futtató fiók **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** jön létre.  Az a futtató profilhoz társítva **Microsoft System Center Advisor Futtatás mint profil Blob** célzott két osztály - és **gyűjtési kiszolgáló** és **Operations Manager felügyeleti csoport**.
* Két összekötők jönnek létre.  Az első nevű **Microsoft.SystemCenter.Advisor.DataConnector** és a-előfizetést, amely továbbítja a felügyeleti csoport összes osztályok példányai szolgáltatáshoz generált összes riasztás automatikusan konfigurálja. A második összekötő **az Advisor Connector**, amely felelős az OMS webes szolgáltatással folytatott kommunikáció és az adatok megosztása.
* Az ügynökök és a felügyeleti csoportban szereplő adatok gyűjtéséért felelős ügyfélfeladatot kiválasztott csoportok hozzáadódik a **Microsoft System Center Advisor figyelési kiszolgáló csoport**.

## <a name="management-pack-updates"></a>Felügyeleti csomagok frissítéséhez
Konfiguráció befejezése után az Operations Manager felügyeleti csoport kapcsolatot hoz létre a Naplóelemzés szolgáltatással.  A felügyeleti kiszolgáló a webszolgáltatás szinkronizálja, és engedélyezte a megoldások, amelyekbe beépül az Operations Manager felügyeleti csomagok formájában fogadják a frissített konfigurációs adatokat.   Az Operations Manager ellenőrzi a felügyeleti csomagok és az automatikus frissítések letöltése, és importálja azokat, ha azok elérhetők.  Két szabály van különösen ami szabályozhatja, hogy ez a viselkedés:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -frissíti az alap Naplóelemzési felügyeleti csomagokat. Alapértelmezés szerint 12 óránként fut.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** -frissíti a megoldás felügyeleti csomagok engedélyezve van a munkaterületen. Alapértelmezés szerint öt (5) percenként fut.

Ha szeretné felülbírálni az e két szabályok automatikus letöltés tiltása letiltásával őket, vagy módosíthat, milyen gyakran szinkronizálja a felügyeleti kiszolgáló határozzák meg, ha egy új felügyeleti csomag elérhető, és le kell tölteni az OMS gyakoriságát.  Kövesse a lépéseket [egy szabály vagy figyelő felülbírálása](https://technet.microsoft.com/library/hh212869.aspx) módosítani a **gyakoriság** paraméter másodpercben. a szinkronizálási ütemezés módosításához, vagy módosítsa a **engedélyezve** letiltja a szabályok paraméter.  A felülbírálások az Operations Manager felügyeleti csoport osztály összes objektumához célként.

Ha folytatja a meglévő vezérlő folyamatának szabályozása a üzemi felügyeleti csoportban található felügyeleti csomag kiadásokban követően, a szabályok letiltása, és lehetővé teszi az adott időben, amikor a frissítések engedélyezettek. Ha rendelkezik olyan fejlesztési vagy QA felügyeleti csoporthoz a környezetben, és képes kapcsolódni az internetre, felügyeleti csoportnál. Ez a forgatókönyv támogatásához a Naplóelemzési munkaterület konfigurálható.  Ez lehetővé teszi, hogy ellenőrizze és értékelje a Log Analyticshez felügyeleti csomagok iteratív kiadásaiban őket az üzemi felügyeleti csoportjába feloldása előtt.

## <a name="switch-an-operations-manager-group-to-a-new-log-analytics-workspace"></a>Az Operations Manager csoport váltani egy új Naplóelemzési munkaterület
1. Jelentkezzen be az Azure Portalra a [https://portal.azure.com](https://portal.azure.com) címen.
2. Az Azure Portalon kattintson a bal alsó sarokban található **További szolgáltatások** elemre. Az erőforrások listájába írja be a **Log Analytics** kifejezést. Ahogy elkezd gépelni, a lista a beírtak alapján szűri a lehetőségeket. Válassza ki **Naplóelemzési** , majd hozza létre a munkaterületén.  
3. Az Operations Manager konzol megnyitásához olyan fiókkal, amely az Operations Manager-Rendszergazdák szerepkör tagja, és válassza ki a **felügyeleti** munkaterületen.
4. Bontsa ki az Operations Management Suite, és válassza **kapcsolatok**.
5. Válassza ki a **művelet Management Suite újrakonfigurálása** hivatkozás a közel-oldalon, ha a panel.
6. Kövesse a **Operations Management Suite előkészítési varázslója** , és adja meg e-mail címét vagy telefonszámát számát és az új Naplóelemzési munkaterület társított rendszergazdai fiók jelszavát.
   
   > [!NOTE]
   > A **Operations Management Suite előkészítési varázslója: Munkaterület kiválasztása** lap megadja a már meglévő munkaterület, amely használatban van.
   > 
   > 

## <a name="validate-operations-manager-integration-with-log-analytics"></a>Az Operations Manager integrációja Naplóelemzési ellenőrzése
Ellenőrizheti, hogy az Operations Manager-integrálást Naplóelemzési sikeres néhány különböző módja van.

### <a name="to-confirm-integration-from-the-azure-portal"></a>Integráció az Azure portálról megerősítéséhez
1. Az Azure Portalon kattintson a bal alsó sarokban található **További szolgáltatások** elemre. Az erőforrások listájába írja be a **Log Analytics** kifejezést. Ahogy elkezd gépelni, a lista a beírtak alapján szűri a lehetőségeket.
2. A Naplóelemzési munkaterület, jelölje ki a megfelelő munkaterületre.  
3. Válassza ki **speciális beállítások**, jelölje be **csatlakoztatott források**, majd válassza ki **a System Center**. 
4. A tábla a System Center Operations Manager szakaszban meg kell jelennie az ügynökök és az állapot számú felsorolt adatok utolsó fogadásakor felügyeleti csoport neve.
   
   ![oms-settings-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

### <a name="to-confirm-integration-from-the-operations-console"></a>Integráció az operatív konzolról megerősítéséhez
1. Nyissa meg az Operations Manager-konzolt, és válassza ki az **Administration** (Adminisztráció) munkaterületet.
2. Válassza ki **felügyeleti csomagok** és a a **keres:** szöveg írja be a következőt **Advisor** vagy **Eszközintelligencia**.
3. Attól függően, hogy engedélyezte a megoldások tekintse meg a megfelelő felügyeleti csomag szerepel a keresési eredmények között.  Például ha engedélyezte a riasztási felügyeleti megoldás, a felügyeleti csomag Microsoft System Center Advisor Riasztáskezelési szerepel a listában.
4. Az a **figyelés** megtekintéséhez nyissa meg a **Operations Management Suite\Health állapot** nézet.  Jelöljön ki egy felügyeleti csoportban a **felügyeleti kiszolgáló állapota** ablaktáblán, majd a a **részletes nézet** ablaktáblán, erősítse meg a tulajdonság értékét **hitelesítési szolgáltatás URI** megegyezik a napló Analytics munkaterület azonosítója.
   
   ![oms-opsmgr-mg-authsvcuri-property-ms](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-log-analytics"></a>Távolítsa el a Log Analyticshez való integráció
Ha már nem szükséges az Operations Manager felügyeleti csoport és a Naplóelemzési munkaterület közötti integráció, nincs több lépésre van szükség a kapcsolat és a konfiguráció megfelelően eltávolítani a felügyeleti csoport. Az alábbi eljárás van a Naplóelemzési munkaterület frissítése törölni kell a hivatkozás a felügyeleti csoport, törölje a Log Analytics-összekötőt, és ezután törölje az integráció a szolgáltatás a támogató felügyeleti csomagok.   

Felügyeleti csomagok megoldásainak engedélyezve van, amelyekbe beépül az Operations Manager és a felügyeleti csomagokat a Naplóelemzés szolgáltatás integrálása támogatásához szükséges könnyen nem törölhető a felügyeleti csoportból.  Ennek az az oka a Naplóelemzési felügyeleti csomagok némelyike más kapcsolódó felügyeleti csomagoktól tartalmazhat függőségeket.  Törölje a felügyeleti csomagokat függőség rendelkező más felügyeleti csomagoktól függ, töltse le a parancsfájl [függőségekkel rendelkező felügyeleti csomag eltávolítása](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) TechNet Script Center.  

1. Nyissa meg az Operations Manager parancs-rendszerhéja egy olyan fiókkal, amely az Operations Manager-Rendszergazdák szerepkör tagja.
   
    > [!WARNING]
    > Ellenőrizze, nem rendelkezik a neve, a folytatás előtt a word Advisor vagy IntelligencePack bármilyen egyéni felügyeleti csomagok, ellenkező esetben a következő lépések törölje ezeket a felügyeleti csoportból.
    > 

2. A parancs-rendszerhéj parancssorában gépelje be `Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
3. Következő típusa `Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
4. A felügyeleti csomagok fennmaradó, amely más System Center Advisor felügyeleti csomagoktól tartalmazhat függőség eltávolításához használja a parancsfájl *RecursiveRemove.ps1* korábban letöltött a TechNet Script Center.  
 
    > [!NOTE]
    > Ne törölje a Microsoft System Center Advisor vagy a Microsoft System Center Advisor belső felügyeleti csomag.  
    >  

5. Nyissa meg az Operations Manager operatív konzol egy olyan fiókkal, amely az Operations Manager-Rendszergazdák szerepkör tagja.
6. A **felügyeleti**, jelölje be a **felügyeleti csomagok** csomópont és a a **keres:** mezőbe írja be **Advisor** , és ellenőrizze a következő felügyeleti csomagokat továbbra is az Ön felügyeleti csoportjába importált:
   
   * A Microsoft System Center Advisor
   * A Microsoft System Center Advisor belső
7. Az OMS-portálon kattintson a **Beállítások** csempére.
8. Válassza ki **csatlakoztatott adatforrások**.
9. A tábla a System Center Operations Manager szakaszban meg kell jelennie a felügyeleti csoport el szeretné távolítani a munkaterület neve.  Az oszlop alatt **utolsó adatok**, kattintson a **eltávolítása**.  
   
    > [!NOTE]
    > A **eltávolítása** hivatkozás nem lesz elérhető 14 nap elteltével nem észlelhető a csatlakoztatott felügyeleti csoportból tevékenység esetén.  
    > 

10. Megjelenik egy ablak, amelyben meg kell erősítenie, hogy szeretné-e az eltávolítás folytatásához.  Kattintson a **Igen** a folytatáshoz. 

Törölje a két összekötőt - Microsoft.SystemCenter.Advisor.DataConnector és az Advisor Connector, mentse a számítógépre a PowerShell parancsfájl alatt, és hajtsa végre az alábbi példák segítségével:

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> A számítógépen futtatja a parancsfájlt, ha nem a felügyeleti kiszolgáló, az Operations Manager parancshéjat, attól függően, hogy a felügyeleti csoport verziójának telepítve kell rendelkeznie.
> 
> 

```
    param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

A jövőben Ha újracsatlakozni a felügyeleti csoport egy Naplóelemzési munkaterület tervezi, akkor importálja újra a `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` felügyeleticsomag-fájlt.  Attól függően, hogy a System Center Operations Manager a környezetében telepített verziója Ez a fájl is található a következő helyen:

* A forrás-adathordozóján a `\ManagementPacks` mappa a System Center 2016 - Operations Manager és újabb rendszer.
* A legújabb kumulatív alkalmazza a felügyeleti csoportban.  Operations Manager 2012, a forrásmappa értéke` %ProgramFiles%\Microsoft System Center 2012\Operations Manager\Server\Management Packs for Update Rollups` és a 2012 R2-ben található `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups`.

## <a name="next-steps"></a>További lépések
Funkciók hozzáadása és az adatgyűjtésre [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).



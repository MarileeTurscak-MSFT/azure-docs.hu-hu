---
title: Hogyan teszi lehetővé az Azure Cosmos DB a kulcsrakész globális disztribúció |} A Microsoft Docs
description: Ismerje meg a globális georeplikáció, több főkiszolgálós, feladatátvétel és data recovery global Database-adatbázisok az Azure Cosmos DB egy globálisan elosztott, többmodelles adatbázis-szolgáltatás használatával.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: 1aa0fa3d4d9e1821a6980d9334456a78eba7bfbc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956914"
---
# <a name="how-azure-cosmos-db-enables-turnkey-global-distribution"></a>Hogyan teszi lehetővé az Azure Cosmos DB a kulcsrakész globális terjesztés
Az Azure Cosmos DB az alábbi képességekkel ahhoz, hogy könnyen globálisan terjesztett alkalmazások írása. Ezek a képességek keresztül érhető el az Azure Cosmos DB erőforrás-szolgáltató alapú [REST API-k](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) és az Azure Portalon.

## <a id="GlobalDistribution"></a>Globális terjesztés

### <a id="RegionalPresence"></a>Széles körben használt területi jelenlét 
Az Azure folyamatosan nő a földrajzi jelenlét oldhatnak [új régiók](https://azure.microsoft.com/regions/) online. Az Azure Cosmos DB sorolja be a rendszer egy *eligazodást szolgáltatás* az Azure-ban, és alapértelmezés szerint minden új Azure régióban érhető el. Ez lehetővé teszi, hogy rendelje hozzá a Azure Cosmos DB-adatbázisfiók egy földrajzi régiót, amint az Azure megnyitja az új üzleti régiót.

### <a id="MultiMasterSupport"></a>Több főkiszolgálós támogatást
Az Azure Cosmos DB natív, kiszolgálóoldali támogatást nyújt több fő régióban bárhol írási modell részét képező egyaránt. Ez a támogatás biztosít < 10 ms írási késés és 99,999 %-os rendelkezésre állás írási [pénzügyileg támogatott SLA-k](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Több főkiszolgálós érhető el, a, beleértve az összes API [SQL](sql-api-introduction.md), [MongoDB](mongodb-introduction.md), [Cassandra](cassandra-introduction.md), [Graph](graph-introduction.md), és [tábla](table-introduction.md) a Cosmos DB az összes SDK nyelvre és nyelvről. Az Azure Cosmos DB támogatja a 4 különböző konzisztenciaszintet (korlátozott frissesség, munkamenet, konzisztens előtag és végleges) képesség több elsajátítására rendelkező fiókok esetében.

### <a id="UnlimitedRegionsPerAccount"></a>Korlátlan számú régióban társítása az Azure Cosmos DB-adatbázisfiók
Az Azure Cosmos DB segítségével tetszőleges számú Azure-régiók társítani a Azure Cosmos DB-fiókot. Geokerítés-korlátozásokat (például, Kína, Németország) kívül vannak, amelyek az Azure Cosmos DB-adatbázisfiókhoz társított régiók számának vonatkozóan nincs korlátozás. A következő ábrán látható, úgy 25 Azure-régióban span adatbázisfiók:

![Az Azure Cosmos DB adatbázisfiókkal 25 Azure-régiók](./media/distribute-data-globally-turnkey/spanning-regions.png)


### <a id="PolicyBasedGeoFencing"></a>Csoportházirend-alapú geokerítés-
Az Azure Cosmos DB támogatja a házirendalapú geokerítés-célja. Geokerítés-adatok irányítási és megfelelőségi korlátozások biztosításához fontos összetevője, és előfordulhat, hogy egy adott régióban társítását ahhoz a fiókhoz. Példák a geokerítés-tartalmazhatnak (de nem korlátozódnak) felmerülő globális disztribúciót, a régiók (például Kínában és Németországban) szuverén felhő vagy kormányzati adózási határ (például Ausztrália) belül. A szabályzatok az Azure-előfizetés metaadatait segítségével szabályozhat.

### <a id="DynamicallyAddRegions"></a>Dinamikusan hozzáadhat és eltávolíthat a régiók
Az Azure Cosmos DB teszi lehetővé (társítás) hozzáadása vagy eltávolítása (társításának megszüntetése) régióban adatbázis-fiókjából bármely időpontra, (tekintse meg [előző ábrán](#UnlimitedRegionsPerAccount)). Több partícióra kiterjedő adatok párhuzamosan replikálása,-hez tartozó Azure Cosmos DB garantálja, hogy egy új régió hozzáadása, esetén műveletek 30 percen belül bárhol a világon (feltéve, hogy az adatok a 100 TB-osra bővül vagy kisebb). 

### <a id="FailoverPriorities"></a>Feladatátvételi prioritások
Használatakor nem több főkiszolgálós az ügyfelek szabályozhatják az azokban az esetekben szolgáltatáskimaradások aktivált regionális feladatátvétel sorozata pontos, Azure Cosmos DB lehetővé teszi, társíthatja a egy *prioritású* a különböző, az adatbázisfiókhoz társított régiók (lásd a az alábbi ábra). Az Azure Cosmos DB biztosítja, hogy az automatikus feladatátvételre kerül sor a prioritásuk szerinti sorrendben történik a megadott. Régiónkénti feladatátvétel kapcsolatos további információkért lásd: [automatikus aktivált regionális feladatátvétel az Azure Cosmos DB az üzletmenet folytonosságának](regional-failover.md). A következő kép bemutatja hogyan a bérlő egy Azure Cosmos DB-ben beállíthatja a feladatátvétel prioritási sorrendben (jobb oldali ablaktábla)-adatbázisfiókhoz társított régiók:

![Az Azure Cosmos DB feladatátvételi prioritások konfigurálása](./media/distribute-data-globally-turnkey/failover-priorities.png)

### <a id="ConsistencyLevels"></a>Globálisan elosztott adatbázis több, jól definiált konzisztenciamodell
Az Azure Cosmos DB támogatja a [több jól definiált, intuitív és gyakorlati konzisztenciamodell](consistency-levels.md) szavatolják. Kiválaszthatja, hogy egy adott konzisztenciájú modellt (a beállítások listájának érhető el) függően a számítási feladatok forgatókönyveket.

### <a id="TunableConsistency"></a>Hangolható konzisztencia globálisan replikált adatbázisok
Az Azure Cosmos DB lehetővé teszi, hogy programozott módon felülbírálása és az alapértelmezett konzisztencia választás futásidőben egy kérelem alapon enyhítése.

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dinamikusan konfigurálhatók olvasási és írási régiók
Az Azure Cosmos DB lehetővé teszi, hogy a régiók (az adatbázishoz kapcsolódó) konfigurálása "olvasási", "write" vagy "Olvasás/írás" régiója.

### <a id="ElasticallyScaleThroughput"></a>Átviteli sebesség rugalmasan méretezése az Azure-régiók között
Rugalmasan méretezheti egy Azure Cosmos DB-tároló üzembe helyezési átviteli sebesség szerint programozott módon. Az átviteli sebességet alkalmazza a rendszer az Azure Cosmos DB-tároló oszlik el az összes régióban.

### <a id="GeoLocalReadsAndWrites"></a>GEO-helyi írások és olvasások
A kulcs egy globálisan elosztott adatbázis előnye, hogy a világ bármely pontján található adatok alacsony késleltetésű elérést biztosít. Az Azure Cosmos DB közel valós idejű kínál (< 10 ezredmásodperc) olvassa be, és az írási műveleteknél világszerte 99. Ez biztosítja, hogy a szolgáltatás (helyi) a legközelebbi régióból szolgálja ki olvasások. Az olvasási kérelem kiszolgálására, a helyi régió, amelyben jelenik meg az olvasási kvórum szolgál. Ugyanez vonatkozik, az írási műveletek. Az írási elfogadott, csak azt követően replikák többsége rendelkezik tartósan véglegesítve lettek az írási helyileg, de éppen által kezdeményezett távoli replikákon gombra annak megerősítéséhez, az írási műveletek nélkül. Másképp helyezni, hogy a replikációs protokoll az Azure Cosmos DB, az olvasási és írási határozatképességére mindig helyi a régió, ahol a kérelem van kiadva, feltételezve működik.

### <a id="ManualFailover"></a>Manuális feladatátvétel
Az Azure Cosmos DB lehetővé teszi, hogy elindíthatja a feladatátvételt egy adatbázis-fiók ellenőrzése a *teljes körű* a teljes alkalmazás (kívül az adatbázishoz kapcsolódó) rendelkezésre állási tulajdonságait. A biztonsági, mind a hiba észlelése és vezető vezetőválasztási liveness tulajdonságainak garantált, mivel az Azure Cosmos DB garantálja *nulla adatvesztés* egy bérlő által kezdeményezett manuális feladatátvétel művelethez.

### <a id="AutomaticFailover"></a>Automatikus feladatátvétel
Az Azure Cosmos DB támogatja az Automatikus feladatátvétel egy vagy több regionális leállás. Regionális feladatátvétel során a Azure Cosmos DB tárolja, az olvasási késés, hasznos üzemidőt rendelkezésre állási, konzisztencia és átviteli sebesség SLA-k. Az Azure Cosmos DB felső határt biztosít az Automatikus feladatátvétel a művelet végrehajtásához időtartama. Ez a potenciális adatvesztés az ablakban egy regionális kimaradás során.

### <a id="GranularFailover"></a>Másik feladatátvételi granularitással tervezve
Jelenleg az automatikus és manuális feladatátvételt képességek érhetők el az adatbázisfiók részletességgel. Vegye figyelembe, hogy belsőleg az Azure Cosmos DB arra tervezték, hogy *automatikus* egy adatbázist, egy tárolót, vagy akár egy partíció (egy tároló kulcsainak számos tulajdonos) kifinomultabb részletességét a feladatátvétel. 

### <a id="MultiHomingAPIs"></a>Az Azure Cosmos DB többkiszolgálós

Egy egyszeri forgatókönyvben, ahol van egy írási régiót és több olvasási-replikával, az Azure Cosmos DB lehetővé teszi, hogy az adatbázis logikai (régiófüggetlen) vagy fizikai (régióspecifikus) végpontok használatával. Logikai végpont használata biztosítja, hogy az alkalmazás átlátható módon lehet többhelyű feladatátvétel esetén. Az utóbbi, fizikai végpontnak, szabályozhatja az alkalmazás átirányítási olvasások és írások adott régiókba történő. Több engedélyezett írási régióval forgatókönyveket, a felhasználók kell megadnia a régióspecifikus végpontok hozzá használatát, és átirányítja a prioritási sorrendet a régióban.

Olvasási beállításainak konfigurálása az információkat a [SQL API](../cosmos-db/tutorial-global-distribution-sql-api.md), [Table API](../cosmos-db/tutorial-global-distribution-table.md), és [MongoDB API-val](../cosmos-db/tutorial-global-distribution-mongodb.md) ezekben a cikkekben.

### <a id="TransparentSchemaMigration"></a>Átlátható és egységes adatbázis sémákat és indexeket áttelepítése 
Azure Cosmos DB teljes mértékben a [sémafüggetlen](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Az adatbázismotor egyedi kialakítása automatikusan lehetővé teszi, hogy az Azure Cosmos DB- és párhuzamos indexelését beolvasást követően az adatok anélkül, hogy bármiféle séma vagy a felhasználó másodlagos indexekkel kellene foglalkoznia. Ez lehetővé teszi, hogy a globálisan elosztott alkalmazás gyorsan iterálni aggódniuk az adatbázis-migrálás sémákat és indexeket és összehangolása a többfázisú alkalmazás kibocsátások sémamódosítások nélkül. Az Azure Cosmos DB garantálja, hogy módosítania kellene az indexelési szabályzatok tett, nem eredményez teljesítmény vagy a rendelkezésre állási teljesítménycsökkenést.  

### <a id="ComprehensiveSLAs"></a>Átfogó SLA-k (túl magas rendelkezésre állás)
Azure Cosmos DB egy globálisan elosztott adatbázis-szolgáltatásként kínál a jól definiált és átfogó SLA-k, a **rendelkezésre állási**, **késés**, **átviteli**, és **konzisztencia** az adatbázis fut, globális méretű, az adatbázishoz társított régiók számától függetlenül.  

## <a id="LatencyGuarantees"></a>Késési garancia
A legfontosabb előnye, például az Azure Cosmos DB egy globálisan elosztott adatbázis-szolgáltatás, hogy kis késésű hozzáférést az adataihoz, a világ bármely pontján található ajánlat. Az Azure Cosmos DB kínál a garantált alacsony késés 99 százalékon különböző adatbázis-műveletekhez. A replikációs protokollt használ az Azure Cosmos DB biztosítja, hogy az adatbázis-műveletek (olvasási és írási) mindig megtörténik, valamint az ügyfél helyi régióban. A késés SLA-t az Azure Cosmos DB olvasási, a (szinkron) az indexelt írások és lekérdezések 99 százalékon garanciát biztosít a különböző kérések és válaszok méretű. A késési garancia az írások tartós többségi kvóruma véglegesítéseket helyi régióban tartalmazza.

### <a id="LatencyAndConsistency"></a>Késés a kapcsolat konzisztencia 
Egy globálisan elosztott szolgáltatás kínál az erős konzisztencia az egy globálisan elosztott környezetben azt kell szinkrón replikálása az írásokat, illetve régiók közötti olvasási szinkron módon végrehajtani. Világos és a nagy kiterjedésű hálózat megbízhatóságát sebességétől előírja, hogy erős konzisztencia magasabb késleltetésű és eredményez az adatbázis-műveletek romlik a rendelkezésre állás. Ezért annak érdekében, hogy garantált a 99. percentilis és 99,99 %-os rendelkezésre állás minden egyrégiós és minden többrégiós fiókok Könnyített konzisztencia és 99,999 %-os rendelkezésre állás minden többrégiós adatbázisfiókhoz, a szolgáltatás alacsony késés érdekében az aszinkron replikáció kell elérnie. Ez a-kapcsolja van szükség, hogy a szolgáltatás is kell-e biztosítani [konzisztencia jól definiált, Könnyített a modellek](consistency-levels.md) – gyengébb, mint az erős (az alacsony késés és rendelkezésre állási garancia ajánlat), és ideális erősebb "konzisztenciát" (az egy intuitív programozási modellhez).

Az Azure Cosmos DB biztosítja, hogy lépjen kapcsolatba a replikákat több régióban, hogy egy adott szintű konzisztenciagaranciának nem szükséges egy olvasási művelet. Hasonlóképpen, biztosítja, hogy írási művelet nem blokkolt alatt replikálja az adatokat, amíg minden régióban, írások rendszer aszinkron módon régiók közötti replikálására). Többrégiós adatbázisfiókhoz mindkét erős, valamint több Könnyített konzisztenciaszintek érhetők el. 

### <a id="LatencyAndAvailability"></a>Késés a kapcsolatot a rendelkezésre állás 
Késés és rendelkezésre állás az azonos érme két oldalán. Egy műveletet a stabil állapotot és a hibák és a hálózati partíciókat a rendelkezésre állási késését beszélünk. Az alkalmazás szempontból lassan futó adatbázis-művelet megkülönböztetni egy adatbázis, amely nem érhető el. 

Elérhetetlensége megkülönböztetni a nagy a késés, az Azure Cosmos DB abszolút felső határt biztosít különböző adatbázis-műveletek késése. Az adatbázis-művelet végrehajtásához a felső határérték hosszabb időt vesz igénybe, ha az Azure Cosmos DB időtúllépési hiba adja vissza. Az Azure Cosmos DB SLA szerinti rendelkezésre állást biztosítja, hogy a rendelkezésre állási SLA-t az időtúllépések okának is beleszámít. 

### <a id="LatencyAndThroughput"></a>Késés a kapcsolat sebessége
Az Azure Cosmos DB nem derül választania kell a késleltetés és az átviteli sebesség között. SLA-mindkét késés 99 százalékon figyelembe veszi, és azt az Ön által kiépített átviteli sebességet biztosít. 

## <a id="ConsistencyGuarantees"></a>Garantált adatkonzisztenciát biztosítanak
Bár a [erős konzisztencia modell](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) az arany szabvány, az adatok programozhatóság, a nagyobb késés (az egyenletes állapotú) nagy ahhoz áron érhető el, és csökkenteni a rendelkezésre állás (érdekében a meghibásodások esetén). 

Az Azure Cosmos DB, jól definiált programozási modellt biztosít a replikált adatok konzisztencia vonatkozó döntések meghozatalát. Annak érdekében, hogy engedélyezi, amellyel egyszerűen készíthet globálisan elosztott alkalmazások többkiszolgálós képesség, az Azure Cosmos DB által elérhetővé tett konzisztenciamodell tervezték őket régiófüggetlen és független a régió, ahol az olvasási és írási folyamatban van szolgálja ki. 

Az Azure Cosmos DB konzisztencia SLA garantálja, hogy az olvasási kérések 100 %-ban az Ön (vagy az adatbázis-fiókot, vagy a kérelem szint) által meghatározott konzisztenciamodellt megfelelő konzisztenciagaranciának felel meg. Egy olvasási kérést minősül teljesül-e a konzisztencia SLA-t, ha teljesülnek a konzisztenciaszint társított összes konzisztenciagaranciákat. Az alábbi táblázat a konzisztenciájú modellt, és a konzisztencia kezességvállalást rögzíti. Ez konzisztenciájú modellt mindegyike garantálja, hogy egy 100 %-os SLA-t. 

|Konzisztenciamodell  | Jellemzők  |
|---------|---------|
|Erős |  Linearizable  |
|Korlátozott frissesség  |  Monoton Olvasás (ugyanabban a régióban), konzisztens előtag, kötött elavulás &lt; K, T   |
|Munkamenet  |  Olvassa el a saját írási, Monotonic olvasni, konzisztens előtag    |
|Konzisztens előtag  | Konzisztens előtag  |

### <a id="ConsistencyAndAvailability"></a>Rendelkezésre állási konzisztencia a kapcsolattal
A [lehetséges eredmény](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) , a [CAP-tétel](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) igazolja, hogy valóban nem lehetséges, a rendszer segítségével is elérhető marad, illetve linearizable konzisztencia érdekében a meghibásodások esetén. Az adatbázis-szolgáltatás ki kell választania a CP vagy a hozzáférési pont, a CP rendszerek ahol leírtnál kisebb a rendelkezésre állási linearizable konzisztencia értéke a közben az AP rendszerek leírtnál kisebb [linearizable konzisztencia](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) rendelkezésre állási értéke. Az Azure Cosmos DB soha nem a kért konzisztenciájú modellt, ezáltal hivatalos CP rendszer sérti. A gyakorlatban konzisztencia viszont nem mindent vagy semmit javaslatának felvétele; nincsenek több jól definiált konzisztenciamodellekkel konzisztenciaspektrumot linearizable és végleges konzisztencia között. Az Azure Cosmos DB számos Könnyített konzisztenciamodellel, amely a valós életből vett alkalmazható és intuitív használandó azonosítja. Az Azure Cosmos DB a kompromisszumot kínál a konzisztencia-rendelkezésre állási navigál, azzal egy [több enyhe még jól definiált konzisztenciamodellekkel](consistency-levels.md) és a egy 99,99 %-os rendelkezésre állás minden egyszeri adatbázis egyrégiós és 99,999 %-os olvasási és írási rendelkezésre állás minden többrégiós adatbázisfiókhoz számára. 

### <a id="ConsistencyAndAvailability"></a>Késés konzisztencia a kapcsolattal
A CAP-tétel átfogóbb változata nevezzük [elérhető, PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), amely is késés és konzisztencia kompromisszumot kínál a egyenletes állapotban használt fiókok. Megállapítja, hogy az egy stabil állapotot egy adatbázis-rendszer kell válassza ki a konzisztencia és a késés. Több Könnyített konzisztenciamodell (aszinkron replikáció és a helyi olvasási és írási határozatképességére által támogatott), az Azure Cosmos DB biztosítja, hogy minden olvasási és írási helyi, az olvasási, és rendre írási régió. Ez lehetővé teszi az Azure Cosmos DB-ajánlat alacsony késési garancia az adott konzisztenciamodellt kínál a régión belül.  

### <a id="ConsistencyAndThroughput"></a>Konzisztencia a kapcsolat sebessége
Mivel a kiválasztott függ, hogy egy adott konzisztenciájú modell megvalósítását egy [kvórum típus](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), átviteli sebesség is a kiválasztott egy konzisztencia-modell alapján változik. Például az Azure Cosmos DB, erősen konzisztens olvasás RU díját van nagyjából *dupla* , amely végül konzisztens olvasások. Ebben az esetben kell double az azonos átviteli teljesítmény elérése érdekében a RUs kiépítéséhez. Az alábbi képen látható a kapcsolat olvasási kapacitást biztosít egy adott konzisztenciájú modellt az Azure Cosmos DB:

![Konzisztencia- és átviteli sebesség közötti kapcsolat](./media/distribute-data-globally-turnkey/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Átviteli sebesség garanciák 
Az Azure Cosmos DB lehetővé teszi az átviteli sebességet (ahogy, tárolás), rugalmasan tetszőleges számú egyéni igények vagy igény szerinti régió között. Az alábbi képen látható egy egyetlen Azure Cosmos DB-tároló (között egy adott régión belül három erőforrás partíciók) vízszintesen particionált, és majd globálisan elosztott, három Azure-régiók között:

![Az Azure Cosmos DB elosztott és a particionált tárolók](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

Egy Azure Cosmos DB-tárolók beolvasása elosztott kétféleképpen (i) egy adott régión belül és régiók között (ii). Ezt a következőképpen teheti meg: 

* **Helyi terjesztési**: egy adott régión belül egy Azure Cosmos DB-tárolók vízszintesen horizontálisan, hogy *erőforrás-partíciók*. Minden egyes erőforrás-partíció kezeli az olyan kulcsokat, és erősen konzisztens, és magas rendelkezésre állású fizikailag négy replikák által képviselt más néven egy *replikakészlethez* és állapotú gép replikáció többek között ezeket a replikákat. Az Azure Cosmos DB egy rendszer teljes erőforrás-szolgáltatás, ahol egy erőforrás-partíció feladata, hogy az átviteli sebesség a költségvetés rendszererőforrás számára lefoglalt részét. A felhasználók számára átlátható, egy Azure Cosmos DB-tárolók méretezését. Az Azure Cosmos DB kezeli az erőforrás-partíciókat és bontja és egyesíti azokat tárolóként igény szerint, és átviteli sebesség követelmények változnak. 
* **Globális terjesztés**: Ha egy többrégiós adatbázis, az erőforrás-partíciók mindegyike majd között oszlanak meg ezekben a régiókban. Erőforrás-partíciók között különböző régiókban űrlap ugyanazokat a kulcsok tulajdonos *set particionálása* (lásd: [előző ábrán](#ThroughputGuarantees)).  Erőforrás-partíciókat, a partíción belül koordinált több régióban, az adatbázishoz kapcsolódó állapotú gép replikáció használatával. Attól függően, a konfigurált konzisztencia szintjét az erőforrás-partíciókat, a partíción belül vannak konfigurálva, segítségével dinamikusan különböző topológiákat (például csillag, lánckapcsolt, fa stb.). 

Egy partíció válaszidejű felügyeleti, terheléselosztás és szigorú erőforrás-szabályozás az Azure Cosmos DB lehetővé teszi rugalmasan méretezhesse az átviteli sebességet egy Azure Cosmos DB-tárolók vagy az adatbázis társított több Azure-régiók között. Kiosztott átviteli sebesség módosítása az Azure Cosmos DB modul jogosultsághoz. Hasonlóan más adatbázis-műveletek, az Azure Cosmos DB garantálja, hogy a kiosztott átviteli sebesség módosításához a kérés késését abszolút felső határnál. Például a következő ábrán látható egy ügyfél tároló és az igény szerint rugalmasan kiosztott átviteli (és 1M - 10 millió kéréseinek száma másodpercenként a két régióban is).

![Az Azure Cosmos DB rugalmasan kiosztott átviteli sebesség](./media/distribute-data-globally-turnkey/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Az átviteli sebesség a kapcsolat konzisztencia 
Ugyanaz a [konzisztencia a kapcsolat sebessége](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Az átviteli sebesség a kapcsolatot a rendelkezésre állás
Az Azure Cosmos DB továbbra is a magas rendelkezésre állás fenntartása, amikor a kiosztott átviteli sebesség végrehajtott módosítások. Az Azure Cosmos DB átláthatóan kezeli az erőforrás-partíciók (és felosztása, egyesítése és a Klónozás hajtja végre műveletek), és gondoskodik róla, hogy a műveletek nem csökkentheti a teljesítmény vagy a rendelkezésre állás, amíg az alkalmazás rugalmasan növeli vagy csökkenti a teljesítményt. 

## <a id="AvailabilityGuarantees"></a>Rendelkezésre állási garanciák
Az Azure Cosmos DB 99,99 %-os rendelkezésre SLA minden egyrégiós adatbázisfiókhoz, és minden többrégiós fiókok kínál Könnyített konzisztenciáját, és 99,999 %-os rendelkezésre állás minden többrégiós adatbázisfiókhoz. A korábban ismertetett Azure Cosmos DB garantált rendelkezésre állás tartalmaznia kell egy abszolút felső korlátja minden adat- és vezérlési síkjával végzett műveletek késése. Ne változtassa meg a rendelkezésre állási garanciák régióban vagy régiók közötti földrajzi távolságtól számával. Rendelkezésre állási garanciák függetlenek vonatkozó megállapodást manuális és is automatikus feladatátvételt. Az Azure Cosmos DB transzparens többkiszolgálós API-kat győződjön meg arról, hogy az alkalmazás logikai végpontokon is működik, és transzparens módon irányíthatja a kérelmeket egy új régióban feladatátvétel esetén, vagy adott regionális végpontok a prioritási sorrendet is kínál használat. Az alkalmazás nem abban az esetben egy regionális feladatátvétel és a rendelkezésre állási SLA-k mindig megőrződik újratelepítése szükséges.

### <a id="AvailabilityAndConsistency"></a>Rendelkezésre állási a kapcsolatot a konzisztencia, a késés és átviteli sebesség
Rendelkezésre állási a kapcsolatot a konzisztencia, a késés és átviteli sebesség a szakaszokban ismertetett [konzisztencia a kapcsolatot a rendelkezésre állás](#ConsistencyAndAvailability), [késés a kapcsolatot a rendelkezésre állás](#LatencyAndAvailability) és [Átviteli sebesség a kapcsolatot a rendelkezésre állás](#ThroughputAndAvailability). 

## <a id="CustomerFacingSLAMetrics"></a>Ügyfelek által használt SLA-metrika
Az Azure Cosmos DB transzparens módon az átviteli sebesség, a késés, a konzisztencia és a rendelkezésre állási metrikák tesz elérhetővé. Ezek a metrikák érhetők el, programozott és az Azure Portalon keresztül (lásd az alábbi ábra). Riasztások az Azure Application Insights használatával különböző küszöbértékeket is állíthatja. Az alábbi képen látható, konzisztencia, késés, átviteli sebességet és rendelkezésre állási metrikák minden bérlő számára transzparens módon elérhető:
 
![Az Azure Cosmos DB ügyfél látható SLA-metrika](./media/distribute-data-globally-turnkey/customer-slas.png)

## <a id="Next Steps"></a>Következő lépések

* [Az Azure Cosmos DB globális terjesztésének előnyei](distribute-data-globally-benefits.md)

* [Azure Cosmos DB globális adatbázis-replikáció konfigurálása](tutorial-global-distribution-sql-api.md)

* [Azure Cosmos DB-fiókok több főkiszolgálós engedélyezése](enable-multi-master.md)

* [Az Azure Cosmos DB automatikus és kézi feladatátvételi működése](regional-failover.md)

* [Az Azure Cosmos DB ütközésfeloldás ismertetése](multi-master-conflict-resolution.md)

* [több főkiszolgálós használata a nyílt forráskódú NoSQL-adatbázisok](multi-master-oss-nosql.md)


## <a id="References"></a>Hivatkozások
1. Eric sörgyár. [Nagy teljesítményű, elosztott rendszerek felé](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric sörgyár. [Tengelysapka később – alkalmazásának tizenkét éve hogyan változtak a szabályok](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Sörgyár&#39;s feltevésen és egységes, elérhető, megvalósíthatósági, partíció hibatűrő Web Services](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Konzisztencia kompromisszumot kínál a Modern elosztott adatbázis-rendszerek kialakítása](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Állítsa le az adatbázisok CP vagy AP hívása](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis és mások. [A gyakorlati részleges határozatképességére valószínűségi korlátozott frissesség (PBS)](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor és gyapjú. [Betöltés, a kapacitás és a rendelkezésre állási a kvórum rendszerek](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy és a következő. [Lineralizability: Helyességét feltétel párhuzamos objektumok](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [Az Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/)

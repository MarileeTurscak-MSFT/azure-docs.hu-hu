---
title: Magas rendelkezésre állás – Azure SQL Database szolgáltatás |} A Microsoft Docs
description: További tudnivalók az Azure SQL Database szolgáltatás magas rendelkezésre állás és a szolgáltatások
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 9c06a028df098874a1ec12d83a362e01a5f4a711
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161896"
---
# <a name="high-availability-and-azure-sql-database"></a>Magas rendelkezésre állású és az Azure SQL Database

Az Azure SQL Database magas rendelkezésre állású adatbázis szolgáltatás, amely garantálja, hogy az adatbázis mentése és futó 99,99 %-os aggódniuk a karbantartás és leállás nélkül platformmegbízhatósági. Ez a egy teljes körűen felügyelt SQL Server adatbázismotor-folyamat, amely biztosítja, hogy az SQL Server-adatbázis mindig frissíteni vagy javítani anélkül, hogy ez hatással lenne a számítási feladatok Azure-felhőben lévő üzemeltetett. Ha egy példány van-e javítani, vagy átadja a feladatokat, az állásidő az általában nem noticable Ha, [újrapróbálkozási logikát alkalmazni](sql-database-develop-overview.md#resiliency) az alkalmazásban. Ha a feladatátvétel elvégzéséhez szükséges idő 60 másodpercnél hosszabb, nyisson meg egy támogatási esetet. Az Azure SQL Database még akkor is, biztosítva, hogy az adatok mindig elérhető a kritikus fontosságú esetekben állíthatja helyre.

Az Azure platform teljes körű kezeli az Azure SQL-adatbázisok és adatvesztés nélkül, és a magas százalékos adatok rendelkezésre állását garantálja. Az Azure automatikusan kezeli a javítás, biztonsági mentések, replikációs, hibaészlelés, lehetséges alapul szolgáló hardver, szoftvereket vagy hálózati hibák, üzembe helyezése hibajavításokat tartalmaz, feladatátvételi teszteket, adatbázis-frissítés és más karbantartási feladatokhoz. Az SQL Server-mérnökök végrehajtották a legismertebb eljárások annak biztosítása, hogy a karbantartási műveleteket végezhető el kisebb, mint az adatbázis élettartama során idő 0,01 %. Ez az architektúra célja annak biztosítása érdekében, hogy véglegesített adatokat soha nem elvész, és hogy karbantartási műveleteket anélkül, hogy ez hatással lenne a számítási feladatok. Nincsenek, a karbantartási időszakok vagy állásidőt eredményezhetett, miközben az adatbázis frissítve vagy fenntartott, állítsa le a számítási feladatok elvégzéséhez szükséges. Beépített magas rendelkezésre állás az Azure SQL Database garantálja, hogy az adatbázis soha nem lesznek a szoftver az architektúrában hibaérzékeny pont.

Az Azure SQL Database az SQL Server adatbázismotor architektúra, amely 99,99 %-os rendelkezésre állását, még akkor is, az infrastruktúra-hibák esetekben biztosítása érdekében a felhőalapú környezet módosul alapul. Az Azure SQL Database által használt két magas rendelkezésre állású architektúra modellje (mindkettő 99,99 %-os rendelkezésre állás biztosítása):
- Standard/általános célú szolgáltatást többrétegű modell, amely a számítási és tárolási szétválasztása alapul. Ez a modell architekturális támaszkodik a magas rendelkezésre állás és megbízhatóság a tárolási szint, de előfordulhat, hogy néhány esetleges teljesítménycsökkenés karbantartásának idejére.
- Prémium vagy üzleti kritikus szolgáltatás többrétegű modell, amely egy olyan fürtjét, adatbázis-motor folyamatainak alapul. Ez a modell architekturális a tény, hogy nem mindig elérhető database engine csomópont és minimális hatása van a számítási feladatokra karbantartásának idejére is támaszkodik.

Az Azure frissíti, és javítások alapjául szolgáló operációs rendszert, illesztőprogramokat és az SQL Server adatbázismotor transzparens módon az a minimális-ideje a végfelhasználók számára. Az Azure SQL Database az SQL Server adatbázismotor és a Windows operációs rendszer legújabb stabil verziója fut, és a felhasználók többsége nem észre, hogy a frissítések folyamatosan történik.

## <a name="standardgeneral-purpose-availability"></a>Standard/általános célú rendelkezésre állása

Standard szintű rendelkezésre állás 99,99 %-os SLA-t, amely a Standard és alapszintű vagy általános célú csomagban kérik hivatkozik. Ebben a modellben architekturális magas rendelkezésre állású számítási és tárolási rétegek elkülönítése és az adatok tárolási szinten a replikáció érhető el.

A következő ábrán látható négy csomópont elkülönített számítási és tárolási rétegekkel standard architekturális modellben.

![Számítási és tárolási szétválasztása](media/sql-database-managed-instance/general-purpose-service-tier.png)

A standard szintű rendelkezésre állás modellben az alábbi két réteg:

- Egy állapot nélküli számítási rétegben, amely a sqlserver.exe folyamat fut, és csak átmeneti és a gyorsítótárazott adatokat (például – tervgyorsítótárból, pufferkészletben, oszlop store készlet) tartalmazza. Ez az állapot nélküli SQL Server-csomóponton, inicializálja a folyamatot, a csomópont állapotát vezérli és hajt végre feladatátvételt egy másik helyre, szükség esetén az Azure Service Fabric szolgáltatást.
- Állapot-nyilvántartó adatrétege-adatbázisfájlokat (.mdf/.ldf), amely az Azure Premium Storage szolgáltatásban tárolódnak. Az Azure Storage garantálja, hogy az bármilyen adatbázis-fájlban lévő bármelyik rekordra adatvesztés nélkül lesz. Az Azure Storage rendelkezik beépített rendelkezésre állása/adatredundancia, amely biztosítja, hogy a naplófájl minden rekord vagy adatfájlban lap megőrzi még akkor is, ha az SQL Server-folyamat leáll.

Alapul szolgáló infrastruktúra egy része sikertelen lesz, amikor az adatbázis-kezelő vagy az operációs rendszer frissítve van, vagy az Sql Server-folyamat kritikus probléma észlelése esetén az Azure Service Fabric az állapot nélküli SQL Server-folyamat áthelyezi egy másik az állapot nélküli számítási csomóponton. Nincs meg az új számítási szolgáltatás feladatátvétel esetén futtassa a feladatátvételi idő minimalizálása érdekében váró tartalék csomópontok. Nincs hatással az Azure Storage-rétegből az adatokat, és adatokat vagy naplófájlokat csatolt újrainicializált SQL Server folyamatán. Ez a folyamat garantálja a 99,99 %-os rendelkezésre állást, de előfordulhat, hogy rendelkezik néhány teljesítményére nagy terhelést, amely miatt a váltás ideje fut-e a, és azt a tényt az új SQL Server-csomópont hideg gyorsítótáras kezdődik.

## <a name="premiumbusiness-critical-availability"></a>Prémium és üzletileg kritikus fontosságú rendelkezésre állása

Prémium szintű rendelkezésre állás engedélyezve van az Azure SQL Database prémium szintű, és arra tervezték, intenzív számítási feladatokhoz, amelyek működését zavarják a teljesítmény romlása a folyamatban lévő karbantartási tevékenységek miatt.

A prémium szintű modellben az Azure SQL database egyesíti a számítási és a egy csomóponton. Magas rendelkezésre állású architektúra ebben a modellben valósítja meg az üzembe helyezett 4 csomópontos (SQL Server adatbázismotor folyamat) számítási és tárolási (helyileg csatlakoztatott SSD) replikációs [Always On rendelkezésre állási csoportok](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) fürt.

![Database engine fürtben](media/sql-database-managed-instance/business-critical-service-tier.png)

Az SQL Server adatbázismotor folyamat és a mögöttes mdf/ldf-fájlok kerülnek ugyanazon a csomóponton a helyileg csatlakoztatott SSD-tárolás biztosítja a számítási feladathoz alacsony késést biztosít. Magas rendelkezésre állású standard használatával lett megvalósítva [Always On rendelkezésre állási csoportok](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Minden adatbázis, a fürt egy elsődleges adatbázis, amely elérhető ügyfél számítási feladata, és a egy három másodlagos folyamat adatok másolatát tartalmazó adatbázis-csomópont. Az elsődleges csomópont folyamatosan leküldi a módosítások a másodlagos csomópontot annak érdekében, hogy az adatok elérhető legyen a másodlagos replikákon. Ha bármilyen okból leáll az elsődleges csomópont. Feladatátvétel az SQL Server adatbázismotor kezeli – egy másodlagos másodpéldány az elsődleges csomópont válik, és annak biztosítására, elég a fürtben található csomópontok jön létre egy új másodlagos replikára. A számítási feladatok a rendszer automatikusan átirányítja az új elsődleges csomópontra.

Emellett a kritikus fontosságú üzleti fürt biztosít beépített csak olvasható csomópont használt csak olvasási lekérdezések futtatása (például a jelentések), amely az elsődleges számítási feladat teljesítményét nem befolyásolja. 

## <a name="zone-redundant-configuration-preview"></a>Zóna redundáns configuration (előzetes verzió)

Alapértelmezés szerint a kvórum-set a replikákat a helyi tárolási konfigurációk jönnek létre ugyanabban az adatközpontban. Bevezetésével [Azure-beli rendelkezésre állási zónák](../availability-zones/az-overview.md), lehetősége nyílik a különböző replikába helyezni, a kvórum-készletek a különböző rendelkezésre állási zónák ugyanabban a régióban. Kiküszöbölése a meghibásodási pont, a vezérlő körgyűrűs is duplikálódnak több zónában, három átjárókiszolgáló körök (GW). Egy adott átjáró kör útválasztást vezérlik [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). A zóna redundáns konfiguráció nem hoz létre az adatbázis-redundancia, mert a rendelkezésre állási zónák (előzetes verzió) használatát a prémium szintű és az üzletileg kritikus szolgáltatási szinten érhető el részeként költség. A zóna redundáns adatbázis kiválasztásával teheti a prémium szintű és az üzletileg kritikus adatbázisokat rugalmas sokkal nagyobb körét a hiba, katasztrofális adatközpont valamilyen okból kimaradás lép, az alkalmazáslogika módosítása nélkül. A zóna redundáns konfiguráció bármely meglévő prémium és az üzletileg kritikus adatbázisokat vagy készleteket is konvertálható.

A redundáns kvórum-zónakészlet replikák néhány távolsága a különböző adatközpontokban van, mert a hálózati késés növelheti a véglegesítés ideje, és így hatással az egyes OLTP számítási feladatok teljesítményére. Mindig visszatérhet az Egyzónás konfiguráció a zóna redundancia beállítás letiltásával. Ez a folyamat egy adatművelet méretét, és hasonló a normál szolgáltatási szint frissítését. A folyamat végén az adatbázis vagy készlet való áttelepítése a zóna redundáns kört a zónában kört vagy fordítva.

> [!IMPORTANT]
> Zóna zónaredundáns adatbázisok és rugalmas készletek jelenleg csak a prémium szintű szolgáltatáscsomagban támogatott. Nyilvános előzetes verziója, a biztonsági mentések és a naplózási során a rekordok az RA-GRS storage tárolja, és ezért nem lehet automatikusan elérhető egy zóna kiterjedő szolgáltatáskimaradás esetén. 

A magas rendelkezésre állású architektúra redundáns zóna verziója által az alábbi ábra mutatja be:
 
![magas rendelkezésre állású architektúra zónaredundáns](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Felskálázás olvasása
Leírt, a prémium és üzletileg kritikus szolgáltatási szintek a magas rendelkezésre álláshoz egyetlen zóna és a redundáns zónabeállítások is használja ki a kvórum beállítása és AlwaysOn technológia. AlwaysOn előnyeinek egyik célja, hogy a replika mindig a tranzakciós szempontból konzisztens állapotban van. Mivel a replikák számítási mérete megegyezik az elsődleges, az alkalmazás kihasználhatja, hogy további kapacitást a csak olvasható számítási feladatok részeként karbantartási költségek (olvasási horizontális felskálázás). Ezzel a módszerrel a csak olvasási lekérdezések elkülönül a fő olvasási és írási számítási feladatok, és nem lesz hatással a teljesítményét. Olvasási horizontális felskálázás funkció célja az alkalmazások, amelyek logikailag tartalmaznak csak olvasható feladatokhoz, mint például az elemzési választják, és ezért hasznosíthatja a további kapacitások anélkül, hogy az elsődleges csatlakozna. 

Az olvasási horizontális Felskálázás funkció használatához, hogy adott adatbázissal, explicit módon aktiválnia kell az adatbázis létrehozásakor vagy később a PowerShell használatával meghívásával konfiguráció módosítása a [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) vagy a [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) parancsmagok vagy az Azure Resource Manager REST API használatával a [- adatbázisok létrehozása vagy frissítése](/rest/api/sql/databases/createorupdate) metódust.

Olvasási horizontális Felskálázás egy adatbázis engedélyezését követően, hogy az adatbázis csatlakozó alkalmazások lesznek irányítva, vagy az írható-olvasható replika, vagy egy csak olvasható replika adatbázis szerint a `ApplicationIntent` tulajdonság, az alkalmazás konfigurálása kapcsolati karakterlánc. Információk a `ApplicationIntent` tulajdonságot használja, lásd: [adja meg az alkalmazások szándékáról](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Ha olvasási kibővített le van tiltva, vagy a ReadScale tulajdonsága egy nem támogatott szolgáltatási rétegben, minden kapcsolat a rendszer átirányítja az írható-olvasható replika, a független a `ApplicationIntent` tulajdonság.

## <a name="conclusion"></a>Összegzés
Az Azure SQL Database és az Azure platform mélyen integrált, és nagymértékben függ, a Service Fabric hiba észlelése és a helyreállítás, az Azure Storage-Blobokból a data protection és a hibatűrés magasabb rendelkezésre állási zónák. Egy időben az Azure SQL database teljes mértékben kihasználja az Always On rendelkezésre állási csoport technológia a replikációt és feladatátvételt az SQL Server box termékből. Ezek a technológiák kombinációja lehetővé teszi, hogy az alkalmazásokat a teljes mértékben a vegyes tárolási modell előnyeinek és a legnagyobb erőforrás-igényű SLA-kat támogatja. 

## <a name="next-steps"></a>További lépések

- Ismerje meg [Azure-beli rendelkezésre állási zónák](../availability-zones/az-overview.md)
- Ismerje meg [Service Fabric](../service-fabric/service-fabric-overview.md)
- Ismerje meg [az Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 

---
title: Az Azure Load Balancer áttekintése |} A Microsoft Docs
description: Az Azure Load Balancer funkciók, architektúra és megvalósítási áttekintése. Ismerje meg a terheléselosztó működését, és használja ki azt a felhőben.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 618b00906a799e1b8cfcfac5ee6bcc3a714c2f87
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918742"
---
# <a name="what-is-azure-load-balancer"></a>Mi az Azure Load Balancer?

Az Azure Load Balancerrel méretezheti az alkalmazásait, és hozzon létre magas rendelkezésre állás a szolgáltatásokhoz. Load Balancer bejövő és kimenő eseteket támogatja az biztosít alacsony késéssel és nagy átviteli sebességű és méretezhető akár több milliónyi összes TCP és UDP-alkalmazás.  

Terheléselosztó osztja el az új bejövő folyamatok, amelyek a terheléselosztó előtérbeli háttérbeli címkészlet példányok, szabályok és az állapotadat-mintavételek alapján az ügyfélszámítógépekre érkeznek. 

Emellett nyilvános Load Balancer biztosíthat kimenő kapcsolatokat a virtuális gépek (VM) a virtuális hálózatban lévő nyilvános IP-címek a magánhálózati IP-címek fordítása.

Az Azure Load Balancer kétféle termékváltozatban érhető el: alap és Standard. A méretezési csoport, a funkciók és a díjszabás különbségek vannak. Bármilyen forgatókönyvhöz, amely az alapszintű Load Balancer lehetőségeinek is létrehozhatók a Standard Load Balancer, habár a megközelítések némileg eltérőek lehetnek. Tudnivalók a Load Balancer, fontos való ismerkedésre az alapelvek betartására és a Termékváltozat-specifikus különbségeket.

## <a name="why-use-load-balancer"></a>Miért érdemes használni a Load Balancer? 

Használhatja az Azure Load Balancerrel együttműködve:

* Terheléselosztás a bejövő internetes forgalmat a virtuális gépek. Ebben a konfigurációban más néven egy [nyilvános Load Balancer](#publicloadbalancer).
* Terheléselosztás a virtuális hálózatban lévő virtuális gépek között forgalmat. Egy terheléselosztó előtérrendszerhez egy hibrid forgatókönyvben egy a helyszíni hálózatról is elérheti. Mindkét forgatókönyv szerint konfigurációját használja egy [belső Load Balancer](#internalloadbalancer).
* Port előre forgalmat egy adott portot a bejövő hálózati címfordítási (NAT) szabályait használó meghatározott virtuális gépeken.
* Adja meg [kimenő kapcsolat](load-balancer-outbound-connections.md) nyilvános Load Balancer használatával a virtuális hálózaton belüli virtuális gépek számára.


>[!NOTE]
> Az Azure teljeskörűen felügyelt terheléselosztási megoldások együttesét biztosítja a különböző forgatókönyvekre. Ha Transport Layer Security (TLS) protokoll-lezárást („SSL-kiszervezés”) vagy per-HTTP/HTTPS kérelmeket vagy alkalmazásréteg-feldolgozást keres, tekintse át az [Application Gatewayt](../application-gateway/application-gateway-introduction.md) ismertető cikket. Ha a globális DNS-terheléselosztás, tekintse át [Traffic Manager](../traffic-manager/traffic-manager-overview.md). A végpontok közötti forgatókönyvek esetében előnyt jelenthet ezen megoldások igény szerinti kombinációja.

## <a name="what-are-load-balancer-resources"></a>Mik a Load Balancer-erőforrások?

Egy terheléselosztó-erőforrást egy nyilvános terheléselosztó vagy egy belső terheléselosztó létezhet. A Load Balancer erőforrásfüggvények előtér, egy szabályt, az állapotfigyelő mintavételező és a háttérkiszolgáló-készlet definíciója fejezik ki. A háttérkészlet virtuális gépek helyezze a virtuális gépről a háttérkészlet megadásával.

Load Balancer-erőforrások olyan objektumok, amelyen belül fejezhető ki hogyan Azure kell program, a több-bérlős-infrastruktúrát a létrehozni kívánt forgatókönyvben. Nincs terheléselosztó-erőforrások és a tényleges infrastruktúra közötti közvetlen kapcsolat. A terheléselosztó nem hoz létre egy példányt, és mindig érhető el a kapacitás. 

## <a name="fundamental-load-balancer-features"></a>Load Balancer alapvető funkciók

Load Balancer a TCP és UDP-alkalmazások esetén az alábbi alapvető képességeket biztosítja:

* **Terheléselosztás**

    Azure Load balancerrel hozzon létre egy terheléselosztási szabályt, amely a készlet háttérpéldányokkal előtérbeli érkező forgalom elosztása. Load Balancer bejövő forgalom megoszlása a kivonat-alapú algoritmust használ, és ennek megfelelően átírja a fejlécek, a háttérbeli címkészlet példányok flow. Egy kiszolgáló érhető el, hogy új folyamatok kapjon az állapotfigyelő mintavételező azt jelzi, hogy egy megfelelően működő háttér-végpontot.
    
    Alapértelmezés szerint a Load Balancer folyamatok leképezése a rendelkezésre álló kiszolgálók használja egy 5-ször több kivonatoló mikroszolgáltatásokból álló, a forrás IP-címe, forrásport, cél IP-címe, célport és IP-protokoll számát. Ha szeretné, hozzon létre egy adott IP-forráscím való affinitását 2 vagy 3 rekordos kivonatot díjazásával egy adott szabály által. Az adott csomag-adatfolyam minden csomagok mögött az elosztott terhelésű előtér ugyanazon az ügyfélszámítógépekre érkeznek. Ha az ügyfél kezdeményezi egy új folyamatot az azonos kiindulási IP, a portot az adatforrás-módosításokkal. Ennek eredményeképpen az 5-ször több okozhat a forgalom egy másik háttérszolgáltatás végponthoz meg.

    További információkért lásd: [Load Balancer elosztási módjának](load-balancer-distribution-mode.md). Az alábbi képen látható a kivonat-alapú terjesztési:

    ![Kivonat-alapú terjesztési](./media/load-balancer-overview/load-balancer-distribution.png)

    *Ábra: Kivonat-alapú terjesztési*

* **Porttovábbítást**

    A terheléselosztó létrehozhat egy bejövő NAT-szabály a port forgalom az adott háttérrendszer-példány a virtuális hálózaton belül egy adott portot egy adott előtérbeli IP-címet az adott port. Ez történik, a terheléselosztás azonos kivonat-alapú terjesztési által is. Ez a funkció általános forgatókönyvek távoli asztal protokoll (RDP) vagy a Secure Shell (SSH) munkamenetek Azure virtuális hálózatban lévő egyes Virtuálisgép-példányokhoz. Több belső végpont leképezheti a különböző portok azonos előtérbeli IP-címen. Távoli felügyeletéhez a virtuális gépek az interneten egy további jump boxon nélkül használhatja őket.

* **Független és átlátható alkalmazás**

    Load Balancer közvetlenül nem működik együtt a TCP vagy UDP vagy az alkalmazásrétegre, és bármely TCP vagy UDP-alkalmazás megoldás.  Terheléselosztó nem leáll vagy folyamatok származnak, interaktívan, és protokoll kézfogások mindig közvetlenül az ügyfél és a háttérkészletpéldányt között fordul elő a folyamatot, a hasznos application layer átjáró függvény kínál.  Egy bejövő folyamathoz választ, mindig egy virtuális gép válaszát.  Érkezésekor a folyamat a virtuális gépen, a rendszer is megőrzi az eredeti forrás IP-címet.  Következzen két példa további megmutatják, átláthatóság:
    - Minden végpont csak egy virtuális gép által megválaszolták.  Például egy TCP-kézfogás mindig akkor fordul elő, az ügyfél és a kiválasztott háttérbeli virtuális gép között.  Egy kérelem egy előtérrendszerhez válasz háttérbeli virtuális gép által generált válasz. Ha sikeresen időtúllépést kapcsolódni, meg vannak a teljes körű létesített kapcsolatok ellenőrzése legalább egy háttérbeli virtuális gép.
    - Alkalmazás is észleltünk adattartalmakat transzparens terheléselosztó és minden olyan UDP vagy TCP-alkalmazás támogatja. A HTTP-kérelem feldolgozása vagy alkalmazás réteget is észleltünk adattartalmakat kezelését igénylő számítási feladatokhoz (például a HTTP URL-címek elemzés), például a 7. rétegbeli terheléselosztó használjon [Application Gateway](https://azure.microsoft.com/services/application-gateway).
    - Mert független a TCP hasznos Load Balancer és TLS kiszervezése ("SSL") nincs megadva, a teljes körű titkosított forgatókönyveket Load Balancer használatával hozhat létre, és lezárja az TLS-kapcsolatot a virtuális gépre a nagy horizontális felskálázási TLS alkalmazások kaphatnak.  A TLS-munkamenet kulcskezelési kapacitás például csak korlátozott típusa és a háttérkiszolgáló-készlethez hozzáadott virtuális gépek száma alapján.  Ha "SSL-kiürítés", az alkalmazás réteg kezelés vagy a kívánja delegálni a tanúsítványok kezelése az Azure-bA van szüksége, használja az Azure-7. rétegbeli terheléselosztó [Application Gateway](https://azure.microsoft.com/services/application-gateway) helyette.
        

* **Automatikus újrakonfigurálásig**

    Load Balancer azonnal újrakonfigurálása Ha felfelé vagy lefelé méretezés példányra. Hozzáadásával vagy eltávolításával a virtuális gépek a a háttérkészlet újrakonfigurálja a terheléselosztó a terheléselosztó erőforrás további műveletek nélkül.

* **Állapotadat-mintavételek**

    Annak megállapításához, a háttérkészletben példányok állapotát, a terheléselosztó állapot-mintavételei, Ön által meghatározott használja. Ha egy mintavételező nem válaszol, akkor a terheléselosztó nem küld új kapcsolatokat a nem megfelelő állapotú példányokat. Meglévő kapcsolatok nem változnak, és akkor is, amíg az alkalmazás leállítja a folyamatot, egy üresjárati időtúllépés történik, vagy a virtuális gép leállt.
     
    Load Balancer biztosít [különböző egészségügyi mintavételi típusok](load-balancer-custom-probe-overview.md#types) a TCP, HTTP vagy HTTPS-végpontokat.

    Ezenkívül klasszikus a cloud services használata esetén egy további típusa engedélyezett: [vendégügynök](load-balancer-custom-probe-overview.md#guestagent).  Ez kell lennie, fontolja meg az állapotfigyelő mintavételező az utolsó mentsvára lennie, és nem ajánlott, amikor az egyéb lehetőségek is kivitelezhető.
    
* **Kimenő kapcsolatok (SNAT)**

    Minden kimenő forgalom a magánhálózati IP-címek a virtuális hálózaton belül az interneten nyilvános IP-címeket is fordítható le a terheléselosztó előtérbeli IP-címre. Nyilvános előtéri egy háttérbeli virtuális Gépről, de a terheléselosztási szabály van kötve, amikor Azure programok automatikusan lefordítani a nyilvános előtérbeli IP-cím a kimenő kapcsolatokat.

    * Engedélyezheti a egyszerű frissítés és a szolgáltatások, vészhelyreállítás, mert az előtér dinamikusan rendelhetők a szolgáltatás egy másik példánya.
    * Egyszerűbb hozzáférés-vezérlési lista (ACL) felügyeletet. ACL-ek kifejezni, az előtérbeli IP-címek nem változik, ahogy szolgáltatások felfelé vagy lefelé vagy újratelepítése beolvasása.  Fordítja le a kimenő kapcsolatokat IP-címek kevesebb, mint a gépek csökkentheti az engedélyezési terhe.

    További információkért lásd: [kimenő kapcsolatok](load-balancer-outbound-connections.md).

A standard Load Balancer ezek – alapok mellett további Termékváltozat-specifikus képességeket tartalmaz. Tekintse át a részleteket a cikk további részében.

## <a name="skus"></a> Load Balancer Termékváltozat összehasonlítása

Load Balancer támogatja sem az alapszintű, Standard termékváltozatok, mindegyik eltérő forgatókönyv méretezési csoportban, szolgáltatásait, és a díjszabással. Bármilyen forgatókönyvhöz, amely az alapszintű Load Balancer lehetőségeinek, valamint a Standard Load Balancer hozható létre. Valójában mindkét termékváltozatok esetében az API-k olyan hasonló, és a Termékváltozat meghatározását keresztül meghívott. Az API az SKU-k támogatása a terheléselosztó és a nyilvános IP-cím lett bevezetve a 2017-08-01-es API-val. Mindkét változatában elérhető, az azonos általános API és a struktúra.

Azonban attól függően, melyik SKU választja, a teljes forgatókönyv konfigurációs eltérhetnek. Load Balancer – dokumentáció meghívja, ha egy cikk csak egy adott Termékváltozat vonatkozik. Összehasonlítása, és különbségek megismeréséhez lásd az alábbi táblázatot. További információkért lásd: [Standard Load Balancer áttekintése](load-balancer-standard-overview.md).

>[!NOTE]
> Új minták el kell fogadnia a Standard Load Balancer. 

Önálló virtuális gépeket, a rendelkezésre állási csoportok és a virtual machine scale sets csak egy termékváltozatra, soha nem is csatlakoztathatók. Azokat a nyilvános IP-címeket használ, amikor Load Balancer és a nyilvános IP-cím Termékváltozatának meg kell egyeznie. Load Balancer és a nyilvános IP-termékváltozatok amelyek nem módosítható.

_Ajánlott explicit módon, adja meg a termékváltozatok annak ellenére, hogy ez még nem kötelező._  Jelenleg a szükséges módosításokat tartják minimális. Ha nincs megadva a Termékváltozat, kerül értelmezésre egy szándékát, hogy az alapszintű Termékváltozat API 2017-08-01-es verzióját használja.

>[!IMPORTANT]
>A standard Load Balancer egy új terheléselosztó terméket, és nagymértékben felülbírálja az alapszintű Load Balancert. Fontos és szándékos között van különbség a két termék. A Standard Load Balancer bármilyen végpontok közötti forgatókönyv, amely az alapszintű Load Balancer lehetőségeinek is létrehozhatók. Ha már jártas alapszintű terheléselosztót, meg kell ismerkednie a Standard Load Balancer a legutóbbi változtatásokat a Standard és alapszintű és a hatásuk között viselkedés megismeréséhez. Ebben a szakaszban javasolt gondosan áttekinteni.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

További információkért lásd: [szolgáltatási korlátozásaival terheléselosztó](https://aka.ms/lblimits). A Standard Load Balancer részletekért lásd: [áttekintése](load-balancer-standard-overview.md), [díjszabás](https://aka.ms/lbpricing), és [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Alapelvek

### <a name = "publicloadbalancer"></a>Nyilvános Load Balancer

Nyilvános Load Balancer nyilvános IP-cím és port számát a bejövő forgalom rendel a virtuális gépről a magánhálózati IP-cím és port számát a virtuális gép, és ez fordítva is igaz a válasz forgalomhoz. Terheléselosztási szabályok alkalmazásával adott típusú forgalom szét több virtuális gép vagy szolgáltatás. Ha például a webes kérelem forgalom terhelését is terjednek több webkiszolgáló között.

A következő ábrán látható egy elosztott terhelésű végpont a webes forgalmat, amelyet a nyilvános és a 80-as porton három virtuális gépet használ. Ezek három virtuális gépet egy elosztott terhelésű készlet vannak.

![Nyilvános Load Balancer – példa](./media/load-balancer-overview/IC727496.png)

*Ábra: Webes forgalom terheléselosztása betöltheti a nyilvános Load Balancer*

Amikor internetes ügyfelek weblap kérést küld a 80-as TCP-porton webalkalmazás nyilvános IP-címét, az Azure Load Balancer a kérelmeket elosztja a három virtuális gépet az elosztott terhelésű készletben. Terheléselosztó algoritmusok kapcsolatos további információkért lásd: a [terheléselosztó funkciók](load-balancer-overview.md##fundamental-load-balancer-features) című szakaszát.

Alapértelmezés szerint az Azure Load Balancer hálózati forgalmat a Virtuálisgép-példányok között osztja el. Munkamenet-affinitás is konfigurálhatja. További információkért lásd: [Load Balancer elosztási módjának](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> belső Load Balancer

Egy belső terheléselosztót arra utasítja a forgalom csak az erőforrások, amelyek egy virtuális hálózaton belül, vagy egy VPN használatával, amely Azure-infrastruktúra eléréséhez. Ebben a tekintetben egy belső Load Balancer nyilvános Load Balancer eltér. Azure-infrastruktúra korlátozza a hozzáférést egy virtuális hálózati terheléselosztással előtérbeli IP-címét. előtérbeli IP-címek és virtuális hálózatok közvetlenül soha nem érhetők el egy internet-végponthoz. Belső üzleti alkalmazások futtatása az Azure-ban, és érhetők el Azure-ban vagy a helyszíni erőforrásokhoz.

Belső Load Balancer lehetővé teszi, hogy a következő típusú terheléselosztási funkciók:

* **Egy virtuális hálózaton belüli**: terheléselosztás virtuális gépek a virtuális hálózat ugyanabban a virtuális hálózatban található virtuális gépek készletét.
* **Létesítmények közötti virtuális hálózatok**: terheléselosztás a helyszíni számítógépek számára egy ugyanazon virtuális hálózaton található virtuális gépek. 
* **A többrétegű alkalmazások**: internetkapcsolattal rendelkező többrétegű alkalmazásokban, ahol a háttérbeli rétegekből amelyek nem internetes elérésű terheléselosztását. A háttérbeli rétegekből szükséges forgalom terhelését elosztja az internetre irányuló réteg (lásd a következő ábra).
* **Az üzletági alkalmazások**: üzleti alkalmazásokhoz, amelyek az Azure-ban anélkül, hogy további load balancer hardveres vagy szoftveres terheléselosztást. Ebben a forgatókönyvben a számítógép, amelynek forgalma az elosztott terhelésű készlet a helyszíni kiszolgálókat tartalmazza.

![Belső Load Balancer – példa](./media/load-balancer-overview/IC744147.png)

*Ábra: Terheléselosztási Többrétegű alkalmazások nyilvános, mind a belső Load Balancer használatával*

## <a name="pricing"></a>Díjszabás
Standard Load Balancer használatáért beállított terheléselosztási szabályok száma és a feldolgozott bejövő és kimenő adatok mennyisége alapján. A Standard Load Balancer díjszabási információkért keresse fel a [Load Balancer díjszabása](https://azure.microsoft.com/pricing/details/load-balancer/) lapot.

Alapszintű Load Balancer díjmentesen érhető el.

## <a name="sla"></a>SLA

Standard Load Balancer SLA-ra vonatkozó további információért látogasson el a [Load Balancer SLA](https://aka.ms/lbsla) lapot. 

## <a name="limitations"></a>Korlátozások

- Terheléselosztó, amely a TCP vagy UDP-terheléselosztás és porttovábbítást a konkrét IP-protokollt.  Terheléselosztási szabályok és bejövő NAT-szabályokat a TCP és UDP támogatott és más IP-protokollok, beleértve az ICMP esetében nem támogatott. Terheléselosztó nem zárja be, válaszol, vagy ellenkező esetben interaktívan UDP vagy TCP-folyamat a hasznos. Akkor sem proxy. Sikeres érvényesítésének feltétele lesz egy előtérbeli kapcsolatot végre kell hajtania sávon hely ugyanazt a protokollt egy terheléselosztási és bejövő NAT-szabályt (TCP vagy UDP) használt a _és_ a virtuális gépek legalább egyikének kell létrehozni a választ egy ügyfél számára Tekintse meg a frontend alhálózatból választ.  Sávon belüli választ nem fogad a terheléselosztó előtérbeli azt jelzi, hogy nincsenek olyan virtuális gépek is tud válaszolni.  Nem alkalmas a terheléselosztó előtérbeli nélkül tud válaszolni a virtuális gépek kezelése.  Ugyanez vonatkozik a kimenő kapcsolatok ahol [port helyettesítő SNAT](load-balancer-outbound-connections.md#snat) van csak a TCP és UDP; támogatott más IP-protokollok többek között az ICMP is sikertelenek lesznek.  Példányszintű nyilvános IP-címet hozzárendelni csökkentése érdekében.
- Eltérően ez nyilvános Terheléselosztók [kimenő kapcsolatok](load-balancer-outbound-connections.md) átállás magánhálózati IP-címek a virtuális hálózaton belül, a nyilvános IP-címeket, ha belső Terheléselosztók nem fordítandó kimenő származik magánhálózati IP-címtér kapcsolatokat az előtér egy belső terheléselosztó is vannak.  Ezzel elkerülhető a lehetséges SNAT portfogyás belül egyedi belső IP-címtér ahol fordítása nem kötelező.  A mellékhatása, hogy ha egy kimenő folyam a háttérkészletben virtuális gépből próbál egy folyamatot a belső terheléselosztó, mely a készletben található előtérbeli _és_ le van képezve vissza magát, a folyamat mindkét alsó nem felelnek meg, és a flow meghiúsulnak.  Ha a folyamat volt nem feleltethető meg vissza ugyanazon a háttérkészletben, amely létrehozta a folyamatot, az előtérbeli virtuális gép, a folyamat sikeres lesz.   Amikor a folyamat vissza magát a maps oly módon, hogy a virtuális gépek a frontend, megjelenik a kimenő folyam és oly módon, hogy a virtuális gép saját maga a megfelelő bejövő forgalom jelenik meg. A vendég operációs rendszer szempontjából a bejövő és kimenő részeit ugyanezt a folyamatot a virtuális gépen belül nem egyezik. A TCP protokollkészlet nem ismerik fel ezeket a feleket az adott adatfolyam részeként ugyanezt a folyamatot, a forrás- és nem egyeznek.  A folyamat bármely más virtuális Gépet a háttérkészletben képez le, amikor a folyamat a feleket egyezni fog, és a virtuális gép sikeresen válaszolhat a folyamatot.  Ebben a forgatókönyvben a jelenség nem állandó hálózati kapcsolat időtúllépése esetén ugyanarra a háttérrendszerre beszúrásra a folyamatot a flow adja vissza. Is számos gyakori megoldás megbízhatóan elérése érdekében ez a forgatókönyv (a háttérkészlet, a háttérbeli készletek megfelelő belső Load Balancer előtérbeli folyamatok származó) többek között a belső terheléselosztó mögötti proxy réteg vagy beszúrási vagy [DSR stílusszabályok használatával](load-balancer-multivip-overview.md).  Ügyfelek kombinálhatják egy belső Load Balancer bármilyen 3. fél proxy- vagy belső helyettesítő [Application Gateway](../application-gateway/application-gateway-introduction.md) korlátozódik, HTTP/HTTPS, proxy forgatókönyvek esetén. Nyilvános Load Balancer csökkentése érdekében használhat, miközben a hibalehetőség-e az eredményül kapott forgatókönyv [SNAT Erőforrásfogyás](load-balancer-outbound-connections.md#snat) és el kell kerülni, kivéve, ha körültekintően felügyelt.

## <a name="next-steps"></a>További lépések

Most már az Azure Load Balancer áttekintése. Ismerkedés a Load Balancer használatával, hozzon létre egyet, virtuális gép létrehozása egyéni IIS-bővítmény telepítve van, és elosztani a kéréseket a webes alkalmazás a virtuális gépek között. További információ a [alapszintű Load Balancer létrehozása](quickstart-create-basic-load-balancer-portal.md) rövid.

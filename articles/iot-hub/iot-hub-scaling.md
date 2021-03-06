---
title: Méretezés az Azure IoT Hub |} A Microsoft Docs
description: Hogyan skálázhatja az IoT hub a várható üzeneteinek átviteli sebessége és a kívánt funkciók támogatásához. A támogatott, az egyes szintek átviteli és a horizontális skálázási lehetőségek összegzését tartalmazza.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: 6ae0217ed4b8833eb42a4719a1f2525461f9dcdd
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143648"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>A megoldás a megfelelő IoT Hub-csomag kiválasztása

Minden IoT-megoldás eltér, ezért a tarifacsomag és méret alapján többféle lehetőséget kínál az Azure IoT Hub. Ez a cikk az adott segítségével kiértékelheti az IoT Hub igényeinek. További díjszabási információk az IoT Hub szintekről [IoT Hub díjszabása](https://azure.microsoft.com/pricing/details/iot-hub). 

Döntse el, melyik az IoT Hub-szint a legmegfelelőbb megoldás, tegye fel magának két kérdéseket:

**Milyen funkciókat tervezhetem használni?**
Az Azure IoT Hub réteget kínál a két, alapszintű és standard szintű, az általuk támogatott funkciókról számában eltérő. Ha az adatok gyűjtése eszközökről, és központilag elemezheti őket az IoT-megoldás alapul majd az alapszintű csomag valószínűleg Önnek ideális. Ha speciális konfigurációk használatával távolról vezérelheti az IoT-eszközök, vagy eloszthatja az alakzatot magukhoz az eszközökhöz tevékenységprofilok egy része szeretne majd érdemes a standard szintre. A funkciókat, amelyek az egyes szintek tartalmaz részletes információkat továbbra is [alapszintű és standard csomagokról](#basic-and-standard-tiers).

**Mennyi adatot tervezhetem naponta áthelyezni?**
Minden IoT Hub-csomag érhető el három méretben alapú körül mennyi adat átviteli bármelyik nap, képes kezelni. 1, 2 és 3 numerikusan azonosítja ezeket a méreteket. Például egy 1. szintű IoT-központ minden egység képes kezelni napi 400 ezer üzenetet, míg a 3. szintre egység 300 millió képes kezelni. Az adatok irányelvek kapcsolatos további információért folytassa [üzeneteinek átviteli sebessége](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Alapszintű és standard csomagokról

A standard szintű IoT-központ összes funkciókat tesz elérhetővé, és kötelező megadni minden olyan IoT-megoldások, győződjön meg arról, hogy a kívánt kétirányú kommunikációt képességeket használhatja. Az alapszintű csomag lehetővé teszi, hogy az funkciók egy részét, és csak egyirányú kommunikációs eszközökről a felhőbe igénylő IoT-megoldások szól. Mindkét rétegei ugyanazokat a biztonsági és hitelesítési szolgáltatásokat.

Miután létrehozta az IoT hub a frissíthető az alapszintű csomag a standard szintre a meglévő operations megszakítása nélkül. További információkért lásd: [az IoT hub frissítése](iot-hub-upgrade.md). Vegye figyelembe, hogy a partíciók maximális korlátot, az alapszintű csomag az IoT Hub 8 és a standard csomag az 32. A legtöbb IoT-központok csak 4 partíciók van szükség. A partíciós korlát akkor kell kiválasztani, amikor az IoT Hub jön létre, és az eszköz – felhő üzeneteket vonatkozik, az ezeket az üzeneteket az egyidejű olvasók. Ez az érték az alapszintű csomag a standard szintű csomag áttelepítésekor változatlan marad. Azt is vegye figyelembe, hogy csak egyféle típusú [edition](https://azure.microsoft.com/pricing/details/iot-hub/) szinten belüli választható ki az IoT Hub száma. Létrehozhat például egy IoT hubot, több S1-egységet, de nem különböző kiadásait, például az S1 és B3, vagy S1 és S2 egységek vegyesen.

| Képesség | Alapszintű csomag | Standard csomag |
| ---------- | ---------- | ------------- |
| [Eszköz – felhő telemetriát](iot-hub-devguide-messaging.md) | Igen | Igen |
| [Eszközönkénti identitás](iot-hub-devguide-identity-registry.md) | Igen | Igen |
| [Üzenet-útválasztása](iot-hub-devguide-messages-read-custom.md) és [Event Grid-integrációt](iot-hub-event-grid.md) | Igen | Igen |
| [HTTP, AMQP és MQTT protokoll](iot-hub-devguide-protocols.md) | Igen | Igen |
| [Eszközök kiépítési szolgáltatáshoz](../iot-dps/about-iot-dps.md) | Igen | Igen |
| [Monitorozás és diagnosztika](iot-hub-monitor-resource-health.md) | Igen | Igen |
| [Üzenetküldés a felhőből az eszközre](iot-hub-devguide-c2d-guidance.md) |   | Igen |
| [Ikereszközök](iot-hub-devguide-device-twins.md), [ikermodulokkal](iot-hub-devguide-module-twins.md) és [Eszközkezelés](iot-hub-device-management-overview.md) |   | Igen |
| [Azure IoT Edge](../iot-edge/about-iot-edge.md) |   | Igen |

Az IoT Hub is biztosít egy ingyenes csomag, amelynek szinkronban tesztelés és értékelés céljából használják. Rendelkezik a standard szintű, de korlátozott üzenetkezelési keretek összes funkcióját. Az ingyenes vagy alapszintű vagy standard szintű nem verzióról. 

### <a name="iot-hub-rest-apis"></a>IoT Hub REST API-k

A támogatott képességek közötti különbség az alapszintű és standard csomagokról az IoT Hub azt jelenti, hogy egyes API-hívások nem működik az alapszintű csomag hubs használatával. Az alábbi táblázat azt mutatja, melyik API-k érhetők el: 

| API | Alapszintű csomag | Standard csomag |
| --- | ---------- | ------------- |
| [Eszköz törlése](https://docs.microsoft.com/rest/api/iothub/service/deletedevice) | Igen | Igen |
| [Eszköz](https://docs.microsoft.com/rest/api/iothub/service/getdevice) | Igen | Igen |
| Modul törlése | Igen | Igen |
| Modul beolvasása | Igen | Igen |
| [Beállításjegyzék statisztikájának beolvasása](https://docs.microsoft.com/rest/api/iothub/service/getdeviceregistrystatistics) | Igen | Igen |
| [Szolgáltatások statisztikájának beolvasása](https://docs.microsoft.com/rest/api/iothub/service/getservicestatistics) | Igen | Igen |
| [Hozzon létre vagy az eszköz frissítése](https://docs.microsoft.com/rest/api/iothub/service/createorupdatedevice) | Igen | Igen |
| A PUT-modul | Igen | Igen |
| [Az IoT Hub lekérdezése](https://docs.microsoft.com/rest/api/iothub/service/queryiothub) | Igen | Igen |
| Lekérdezés-modulok | Igen | Igen |
| [Fájl feltöltése SAS URI létrehozása](https://docs.microsoft.com/rest/api/iothub/device/createfileuploadsasuri) | Igen | Igen |
| [Értesítés e kötve eszköz](https://docs.microsoft.com/rest/api/iothub/device/receivedeviceboundnotification) | Igen | Igen |
| [Esemény küldése](https://docs.microsoft.com/rest/api/iothub/device/senddeviceevent) | Igen | Igen |
| A modul esemény küldése | Igen | Igen |
| [Fájlfeltöltés állapota frissítése](https://docs.microsoft.com/rest/api/iothub/device/updatefileuploadstatus) | Igen | Igen |
| [A tömeges eszköz művelet](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/bulkoperation) | Igen, kivéve az IoT Edge-képességek | Igen | 
| [A parancs üzenetsor kiürítése](https://docs.microsoft.com/rest/api/iothub/service/purgecommandqueue) |   | Igen |
| [Ikereszköz beolvasása](https://docs.microsoft.com/rest/api/iothub/service/gettwin) |   | Igen |
| Ikermodul beolvasása |   | Igen |
| [Eszközmetódus meghívása](https://docs.microsoft.com/rest/api/iothub/service/invokedevicemethod) |   | Igen |
| [Ikereszköz frissítése](https://docs.microsoft.com/rest/api/iothub/service/updatetwin) |   | Igen | 
| Ikermodul frissítése |   | Igen | 
| [Értesítési eszköz kötött Abandon](https://docs.microsoft.com/rest/api/iothub/device/abandondeviceboundnotification) |   | Igen |
| [Teljes eszközhöz kötött értesítés](https://docs.microsoft.com/rest/api/iothub/device/completedeviceboundnotification) |   | Igen |
| [Feladat megszakítása](https://docs.microsoft.com/rest/api/iothub/service/canceljob) |   | Igen |
| [Feladat létrehozása](https://docs.microsoft.com/rest/api/iothub/service/createjob) |   | Igen |
| [Feladat beolvasása](https://docs.microsoft.com/rest/api/iothub/service/getjob) |   | Igen |
| [Lekérdezés feladatok](https://docs.microsoft.com/rest/api/iothub/service/queryjobs) |   | Igen |

## <a name="message-throughput"></a>Üzeneteinek átviteli sebessége

A legjobb módszer a következő méretre egy IoT Hub-megoldás, hogy a forgalmat egy egységenként kiértékeléséhez. Különösen fontolja meg a szükséges maximális átviteli sebesség a műveletek az alábbi kategóriákban:

* Az eszközről a felhőbe irányuló üzenetek
* Felhőből az eszközre irányuló üzenetek
* Identitásjegyzék műveletei

Forgalom egységenkénti alapon, nem / hub mérjük. Egy 1 vagy 2-es szintű IoT Hub-példány lehet akár 200 egységre lenne szüksége társítva. 3. szintre IoT Hub-példány legfeljebb 10 egység lehet. Az IoT hub létrehozása után módosíthatja az egységek számát, vagy helyezze át az 1, 2 és a egy adott szinten belül 3 méretet között zavarja meg a meglévő operations. További információkért lásd: [az IoT Hub frissítése](iot-hub-upgrade.md).

Például az egyes szintek forgalom képességeit eszköz – felhő üzeneteket az alábbiakra vendégteljesítmény:

| Szint | Folyamatos teljesítményt | Tartós küldések |
| --- | --- | --- |
| B1, S1 |Akár 1111 KB/perc / egység<br/>(1,5 GB/nap/egység) |Átlagosan 278 üzenetek/perc / egység<br/>(400 000 üzenet/nap / egység) |
| B2, S2 |Legfeljebb 16 MB/perc / egység<br/>(22.8 GB/nap/egység) |Átlagosan 4,167 üzenetek/perc / egység<br/>(6 millió üzenet/nap / egység) |
| B3, S3 |Akár 814 MB/perc / egység<br/>(1144.4 GB/nap/egység) |Átlagosan 208,333 üzenetek/perc / egység<br/>(300 millió üzenet/nap / egység) |

Mellett az átviteli sebességgel kapcsolatos információkat lásd: [az IoT Hub kvótái és szabályozások] [ IoT Hub quotas and throttles] , és ennek megfelelően a megoldás megtervezése.

### <a name="identity-registry-operation-throughput"></a>Identitás beállításjegyzék műveleti teljesítménye
Az IoT Hub identitásjegyzék műveletei vannak nem lehet futásidejű művelet, mivel ezek többnyire kapcsolódó eszközök üzembe helyezését.

Adott burst teljesítményszámokhoz lásd [az IoT Hub kvótái és szabályozások][IoT Hub quotas and throttles].

## <a name="auto-scale"></a>Automatikus méretezés
Ha Ön hamarosan eléri az engedélyezett üzenetkorlát az IoT hubon, akkor használhatja ezeket [automatikusan elvégzi a méretezést lépéseket](https://azure.microsoft.com/resources/samples/iot-hub-dotnet-autoscale/) egy IoT Hub egységet az IoT hubbal megegyező szinten növekszik.

## <a name="sharding"></a>Sharding
Egyetlen IoT hubra eszközök millióira méretezhetők, míg egyes esetekben a megoldáshoz szükséges specifikus teljesítményt nyújt, amely nem garantálja az egyetlen IoT hubra. Ebben az esetben az eszközök particionáló több IoT hubon keresztül. Több IoT hubon forgalomnövekedések smooth, és szerezze be a szükséges kapacitás vagy a művelet díjakat, amelyek szükségesek.

## <a name="next-steps"></a>További lépések

* Az IoT Hub képességek és a teljesítményadatok kapcsolatos további információkért lásd: [az IoT Hub díjszabás] [hivatkozás díjszabás] vagy [az IoT Hub kvótái és szabályozások][IoT Hub quotas and throttles].
* Az IoT Hub-csomag módosításához kövesse [az IoT hub frissítése](iot-hub-upgrade.md).

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md

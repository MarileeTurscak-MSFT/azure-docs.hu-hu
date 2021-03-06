---
title: Event Grid, az IoT Hub útválasztásának összehasonlítása |} A Microsoft Docs
description: Az IoT Hub lehetővé teszi a saját üzenet-útválasztási szolgáltatás, de is integrálható az Event GRID használatával az események közzététele. Hasonlítsa össze a két funkció.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 13cf5861bf39cdd9c192586979b95192a31e9399
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978675"
---
# <a name="compare-message-routing-and-event-grid-for-iot-hub"></a>Az IoT Hub üzenet-útválasztással és az Event Grid összehasonlítása

Az Azure IoT Hub lehetővé teszi, hogy streamelhet adatokat a csatlakoztatott eszközökből, és integrálhatja az adatokat az üzleti alkalmazások. Az IoT Hub IoT-események integrálása más Azure-szolgáltatások vagy alkalmazások kétféle módszert biztosít. Ez a cikk ismerteti a két funkció ezt a funkciót biztosító, így kiválaszthatja, melyik lehetőség a forgatókönyv esetén ajánlott használni.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

* **[IoT Hub üzenet-útválasztása](iot-hub-devguide-messages-d2c.md)**: Ez az IoT Hub szolgáltatás lehetővé teszi a felhasználók irányíthatja a Szolgáltatásvégpontok Azure Storage-tárolók, az Event Hubs, Service Bus-üzenetsorok és Service Bus-üzenettémakörök például eszköz – felhő üzeneteket. Útválasztás is lehetőséget biztosít egy lekérdezési szűrje az adatokat az Útválasztás, a végpontok előtt. Eszköz telemetriai adatok mellett is küldhet [nem telemetria-eseményeinek](iot-hub-devguide-messages-d2c.md#non-telemetry-events) , amely a műveletek indításához használható. 
* **Az IoT Hub Event Grid-integráció**: Azure Event Grid egy teljes körűen felügyelt esemény-útválasztó szolgáltatás közzétételi használó rendszer-előfizetési modellt. Az IoT Hub-és Event Grid [IoT Hub-események integrálása az Azure és az Azure-szolgáltatások](iot-hub-event-grid.md), közel valós időben. 

## <a name="similarities-and-differences"></a>Különbségeket

Üzenettovábbítás és Event Grid riasztási konfiguráció engedélyezése, amíg vannak a kettő közötti fontosabb különbségeket. Tekintse meg az alábbi táblázat részleteket:

| Szolgáltatás | IoT Hub üzenet-útválasztása | Az IoT Hub Event Grid-integráció |
| ------- | --------------- | ---------- |
| **Az eszközre** | Igen, az üzenetirányítással használható telemetriai adatokat. | Nem, Event Grid csak használható nem telemetriát az IoT Hub-események. |
| **Esemény típusa** | Igen, az üzenetirányítással jelenthetik ikereszköz módosításokat és eszköz-életciklussal kapcsolatos események. | Event Grid jelentheti Igen, ha az eszközök létrehozni, törölni, csatlakoznak, és csatlakozik az IoT Hub |
| **Rendezés** | Igen, az események rendezése fenntartani.  | Események sorrendje nem, nem garantált. | 
| **Maximális üzenetméret** | 256 KB-os, eszközről a felhőbe | 64 KB |
| **Szűrés** | Gazdag szűrést az üzenet alkalmazás tulajdonságait, üzenet Rendszertulajdonságok, üzenet szövegét, device twin címkék és az eszköz ikereszköz tulajdonságait. Példák: [üzenet útválasztás lekérdezési szintaxis](iot-hub-devguide-routing-query-syntax.md). | Szűrés utótag/előtagja alapján az eszköz azonosítóját, amely jól működik a hierarchikus szolgáltatásokhoz, például a storage. |
| **Végpontok** | <ul><li>Event Hubs</li> <li>Azure Blob Storage</li> <li>Service Bus-üzenetsor</li> <li>Service Bus-témák</li></ul><br>Fizetős IoT Hub termékváltozatok (S1, S2 és S3) korlátozva, 10 egyéni végpontok. az IoT Hub 100 útvonal hozható létre. | <ul><li>Azure Functions</li> <li>Azure Automation</li> <li>Event Hubs</li> <li>Logic Apps</li> <li>Storage Blob</li> <li>Egyéni témakörök</li> <li>Külső szolgáltatásokat, Webhookok segítségével</li></ul><br>A végpontok legfrissebb listájáért lásd: [Event Grid eseménykezelők](../event-grid/overview.md#event-handlers). |
| **Költségek** | Nem jár külön az üzenettovábbításhoz. Csak a bejövő telemetriát az IoT Hub díjat számítunk fel. Például ha egy üzenet, három különböző végpontokra irányítva, akkor számlázása csak egy üzenetet. | Nem jár az IoT hubról. Event Grid nyújt az első 100 000 művelet havonta ingyen, és ezután $0,60 ezt követően 1 millió műveletenként. |

Az IoT Hub üzenet-útválasztással és az Event Grid Hasonlóságok túl, amelyek némelyike részletezi az alábbi táblázatban:

| Szolgáltatás | IoT Hub üzenet-útválasztása | Az IoT Hub Event Grid-integráció |
| ------- | --------------- | ---------- |
| **Megbízhatóság** | Magas: Kézbesíti a minden üzenet legalább egyszer minden egyes útvonal végpontját. Összes üzenet, amely még nem lett kézbesítve egy órán belül lejár. | Magas: Kézbesíti a minden üzenet legalább egyszer minden egyes előfizetés esetén a webhook. Összes esemény, amely még nem lett kézbesítve 24 órán belül lejár. | 
| **Méretezhetőség** | Nagy: Támogatja a több millió egyszerre kapcsolódó eszközről küld milliárdos nagyságrendű üzenetet optimalizálva. | Nagy: Régiónként másodpercenként, útválasztási 10,000,000 képes. |
| **Késés** | Alacsony: Közel valós időben. | Alacsony: Közel valós időben. |
| **Több végpont küldése** | Igen, küldjön egy üzenet, több végpontot. | Igen, küldjön egy üzenet, több végpontot.  | 
| **Biztonság** | IOT Hub eszközönkénti identitás- és visszavonható hozzáférés-vezérlést biztosít. További információkért lásd: a [az IoT Hub-hozzáférés-vezérlés](iot-hub-devguide-security.md). | Event Grid érvényesítés három pontot tartalmaz: webhook eseménykézbesítés, esemény-előfizetések és események közzététele. További információkért lásd: [Event Grid biztonsági és hitelesítési](../event-grid/security-authentication.md). |

## <a name="how-to-choose"></a>Segítség a választáshoz

Az IoT Hub üzenet-útválasztással és a IoT Hub és az Event Grid integráció hasonló eredmény elérése érdekében különböző műveleteket hajt végre. Ezek is igénybe vehet az IoT Hub-megoldásból származó adatokat, és adja át azt, hogy más szolgáltatások reagálhat. Igen, hogyan megállapítani, hogy melyiket érdemes használni? Mellett az előző szakaszban az adatokat használja a döntés segít a következő kérdéseket: 

* **Milyen típusú adatokat, küldenek a végpontokhoz?**

   Használja az IoT Hub üzenet-útválasztása Ha rendelkezik a telemetriai adatokat küldeni a más szolgáltatásokhoz. Üzenetirányítás is lehetővé teszi, hogy az alkalmazás- és üzenettulajdonságok, az üzenet törzse, device twin címkék és eszköz-ikertulajdonságok lekérdezéséhez.

   A IoT Hub és az Event Grid integráció működik az IoT Hub szolgáltatásban előforduló eseményeket. Az IoT Hub-események közé tartozik a létrehozása, törlése, csatlakoztathatók, illetve. 

* **Milyen végpontokat kell ezeket az információkat kapni?**

   IoT Hub üzenet-útválasztása támogatja a korlátozott végpontokat, de irányítsa át az adatokat és eseményeket további végpontokat az összekötőket hozhat létre. Támogatott végpontok teljes listáját lásd: a táblázat az előző szakaszban. 

   Az IoT Hub-integráció az Event GRID használatával további végpontokat támogatja. Is működik, a webhookok bővíteni, Útválasztás kívül az Azure-szolgáltatás ökoszisztéma, valamint a külső üzleti alkalmazások. 

* **Nem számít, hogy az adatok sorrendben érkeznek?**

   A sorrendet, amely üzeneteket küld, IoT Hub üzenet-útválasztása tart fenn, így ugyanúgy vizsgálatát.

   Event Grid nem garantálja, hogy végpontok kapja, hogy azok történt ugyanabban a sorrendben. Azonban az esemény sémája a következő időbélyeget segítségével azonosíthatja a rendelés után az esemény érkezik a végponton tartalmazza. 

## <a name="next-steps"></a>További lépések

* Tudjon meg többet [IoT Hub üzenet-útválasztása](iot-hub-devguide-messages-d2c.md) és a [IoT Hub-végpontok](iot-hub-devguide-endpoints.md).
* Tudjon meg többet [Azure Event Griddel](../event-grid/overview.md)
* Üzenet útvonalak létrehozásával kapcsolatban lásd: a [folyamat az IoT Hub eszköz – felhő üzenetek útvonalak](../iot-hub/tutorial-routing.md) oktatóanyag.
* Próbálja ki az Event Grid integráció által [küldése e-mailes értesítést az Azure IoT Hub-események Logic Apps használatával](../event-grid/publish-iot-hub-events-to-logic-apps.md)
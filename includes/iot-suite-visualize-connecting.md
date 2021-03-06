---
title: fájl belefoglalása
description: fájl belefoglalása
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 5702c6e9c9d75c6cccb82f1c57684ef7b9898c34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34666009"
---
## <a name="view-device-telemetry"></a>Eszköztelemetria megtekintése

Az eszközről küldött telemetriai adatok megtekintheti a **eszközök** lap a megoldásban.

1. Válassza ki az eszközt, az eszközök listájában kiépítve a **eszközök** lap. A panel az eszközt, beleértve a telemetriát rajzot információit jeleníti meg:

    ![Tekintse meg az eszköz részletei](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Válasszon **nyomás** telemetriai megjelenítésének módosítása:

    ![Nézet nyomás telemetriai adat](media/iot-suite-visualize-connecting/devicespressure.png)

1. Az eszköz diagnosztikai adatainak megtekintésére, görgessen le a **diagnosztika**:

    ![Nézet eszköz diagnosztika](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Az eszközön működésre

Az eszközök metódusok meghívása, használja a **eszközök** a távoli figyelésére szolgáló megoldás lapján. Például a távoli figyelésére szolgáló megoldás a **hűtő** eszközök valósítja meg a **FirmwareUpdate** metódust.

1. Válasszon **eszközök** lehetőségre, és navigáljon a **eszközök** lap a megoldásban.

1. Válassza ki az eszközt, az eszközök listájában kiépítve a **eszközök** lap:

    ![Válassza ki a fizikai eszköz](media/iot-suite-visualize-connecting/devicesselect.png)

1. A metódusok hívása az eszközön listájának megjelenítéséhez kattintson **feladatok**, majd **Run metódus**. Ütemezni a feladatot több eszközön futtassa, jelölje ki több eszközre a listában. A **feladatok** panelen láthatók metódus általános a kiválasztott eszközök.

1. Válasszon **FirmwareUpdate**, a feladat neve **UpdatePhysicalChiller**. Állítsa be **belsővezérlőprogram-verziónként** való **2.0.0**, beállíthatja **belső vezérlőprogram URI** való **http://contoso.com/updates/firmware.bin**, és válassza a **alkalmaz**:

    ![A belső vezérlőprogram-frissítés ütemezése](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Üzenetek sorozatát jeleníti meg a konzolon, a kód fut, miközben a szimulált eszköz kezeli a metódust.

1. Ha a frissítés befejeződött, a belső vezérlőprogram verziójának megjeleníti a **eszközök** lap:

    ![A frissítés befejeződött](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> A megoldásban a feladat állapotának nyomon követése, válassza a **nézet**.

## <a name="next-steps"></a>További lépések

A cikk [testre szabhatja a távoli megfigyelési megoldásgyorsító](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) testre szabhatja a megoldásgyorsító néhány módját ismerteti.
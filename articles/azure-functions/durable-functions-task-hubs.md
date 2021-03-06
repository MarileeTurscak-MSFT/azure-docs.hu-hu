---
title: Durable Functions – az Azure a feladatközpontok
description: Ismerje meg, milyen feladatot a központ egy az Azure Functions Durable Functions-bővítményben. Ismerje meg, hogyan konfigurálhatja feladatközpontok konfigurálása.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 53c70d4407222a80a9bc948db51294cd3e4e1bb4
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092835"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Feladatközpontok a tartós függvények (az Azure Functions)

A *feladat hub* a [Durable Functions](durable-functions-overview.md) vezénylések használt Azure Storage-erőforrások logikai tárolója. Orchestrator, illetve a tevékenység funkciók tartoznak, a tevékenység egy központban csak is kapcsolatba egymással.

Minden függvényalkalmazáshoz egy külön feladat központ rendelkezik. Ha több függvényalkalmazás egy tárfiókot, a storage-fiókot a több feladatközpontok tartalmazza. A következő ábra szemlélteti egy feladat hub függvényalkalmazást a közös vagy dedikált storage-fiókok száma.

![Bemutató ábra. Ez a megosztott, és a storage-fiókok dedikált.](media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Az Azure Storage-erőforrások

Egy feladat hub a következő tároló-erőforrások áll: 

* Egy vagy több vezérlő üzenetsorok.
* Egy munkaelem várólistát.
* Egy előzménytábla.
* Példányok egy olyan táblát.
* Egy storage-tárolót tartalmazó egy vagy több bérletet blobot.

Összes ilyen erőforrásról jönnek létre automatikusan az alapértelmezett Azure Storage-fiókban az orchestrator vagy tevékenység függvények futtatása vagy ütemezésére. A [teljesítmény és méretezhetőség](durable-functions-perf-and-scale.md) a cikk azt ismerteti, hogyan használhatók a ezeket az erőforrásokat.

## <a name="task-hub-names"></a>A feladat értesítésiközpont-nevek

Feladatközpontok van deklarálva a név azonosítja a *host.json* fájljához a következő példában látható módon:

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

A feladat értesítésiközpont-nevek kell betűvel kezdődhet, és csak betűkből és számokból állhat. Ha nincs megadva, az alapértelmezett név az **DurableFunctionsHub**.

> [!NOTE]
> A név egy feladat hub másik kódjába, ha több feladatközpontok megosztott tárfiók található. Ha egy megosztott tárfiókot megosztása több függvényalkalmazás, hogy konfigurálnia minden egyes feladat hub, a különböző neveket a *host.json* fájlokat.

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [Ismerje meg, hogyan kezelje a verziókezelés](durable-functions-versioning.md)

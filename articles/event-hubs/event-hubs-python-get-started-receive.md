---
title: Események fogadása az Azure Event Hubs Python használatával |} A Microsoft Docs
description: Események fogadása az Event Hubsból pythonnal – első lépések
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/26/2018
ms.author: sethm
ms.openlocfilehash: f5388f2de599d94f68a1d24a7d701a2cb4795915
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703664"
---
# <a name="receive-events-from-event-hubs-using-python"></a>Események fogadása az Event Hubs Python használatával

Az Azure Event Hubs szolgáltatás egy kiválóan méretezhető eseménykezelő rendszer, amely képes kezelni a több millió csatlakoztatott eszközök és más rendszerek által létrehozott hatalmas adatmennyiségek elemzését teszi az alkalmazások engedélyezése események száma másodpercenként. Adatoknak egy eseményközpontba való összegyűjtését, miután kap, és zpracovávat události használatával a folyamaton belüli kezelők vagy más elemzési rendszerek továbbítással.

Az Event Hubs kapcsolatos további információkért tekintse meg a [Event Hubs – áttekintés][Event Hubs overview].

Ez az oktatóanyag ismerteti a események fogadása az eseményközpontba egy pythonban írt alkalmazás. Eseményeket küldeni, tekintse meg [a megfelelő küldési cikk](event-hubs-python-get-started-send.md).

Jelen oktatóanyagban szereplő kód nézetéből [ezeket a GitHub-minták](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), amely a teljes megtekintéséhez tekintse meg működő alkalmazást, beleértve az importálási utasításokat és változódeklarációkat. További példák GitHub ugyanabban a mappában érhetők el.

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag teljesítéséhez a következő előfeltételekre lesz szüksége:

- Python 3.4-es vagy újabb.
- Egy meglévő Event Hubs névtér és az eseményközpont. Ezek az entitások utasításait követve hozhat létre [Ez a cikk](event-hubs-create.md). 

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]


## <a name="install-python-package"></a>Python-csomag telepítése

Az Event Hubs a Python-csomag telepítéséhez nyisson meg egy parancssort, amelynek az elérési út Python, és futtassa ezt a parancsot: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-receive-events"></a>Hozzon létre egy Python-szkriptet eseményeket szeretne fogadni.

Ezután hozzon létre egy Python-alkalmazás, amely fogadja az eseményeket egy adott eseményközpontból:

1. Nyissa meg a Python kedvenc szerkesztőjében, például [Visual Studio Code][Visual Studio Code].
2. Hozzon létre egy parancsfájlt nevű **recv.py**.
3. Recv.py, és cserélje le a címet, a felhasználó és a kulcs értékeit az Azure Portalról az előző szakaszban beszerzett illessze be a következő kódot: 

```python
import os
import sys
import logging
import time
from azure.eventhub import EventHubClient, Receiver, Offset

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"
CONSUMER_GROUP = "$default"
OFFSET = Offset("-1")
PARTITION = "0"

total = 0
last_sn = -1
last_offset = "-1"
client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
try:
    receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
    client.run()
    start_time = time.time()
    for event_data in receiver.receive(timeout=100):
        last_offset = event_data.offset
        last_sn = event_data.sequence_number
        print("Received: {}, {}".format(last_offset, last_sn))
        total += 1

    end_time = time.time()
    client.stop()
    run_time = end_time - start_time
    print("Received {} messages in {} seconds".format(total, run_time))

except KeyboardInterrupt:
    pass
finally:
    client.stop()
```

## <a name="receive-events"></a>Események fogadása

A szkript futtatásához nyisson meg egy parancssort, amelynek az elérési út Python, és futtassa ezt a parancsot:

```bash
start python recv.py
```
 
## <a name="next-steps"></a>További lépések

Eseményeket küldeni, tekintse meg [a megfelelő küldési cikk](event-hubs-python-get-started-send.md).

Látogasson el a következő oldalakon további információt az Event Hubsról:

* [Event Hubs – áttekintés][Event Hubs overview]
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

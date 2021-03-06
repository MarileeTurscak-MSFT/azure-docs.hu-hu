---
title: fájl belefoglalása
description: fájl belefoglalása
services: signalr
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 97b8ef5e260a853ecdffb3c502f8a5051dd0e143
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006258"
---
## <a name="create-an-azure-signalr-service-instance"></a>Azure SignalR Service-példány létrehozása

Az alkalmazás egy SignalR Service-példányhoz fog csatlakozni az Azure-ban.

1. Kattintson az Azure Portal bal felső sarkában található Új gombra. Az Új képernyőn írja be a keresőmezőbe a *SignalR Service* kifejezést, majd nyomja le az Enter billentyűt.

    ![SignalR Service keresése](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. A keresési eredmények között válassza ki a **SignalR Service** elemet, majd a **Létrehozás** lehetőséget.

1. Adja meg az alábbi beállításokat.

    | Beállítás      | Ajánlott érték  | Leírás                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Erőforrás neve** | Globálisan egyedi név | Az új SignalR Service-példányt azonosító név. Érvényes karakterek: `a-z`, `0-9` és `-`.  | 
    | **Előfizetés** | Az Ön előfizetése | Az új SignalR Service-példány létrehozásához használt előfizetés. | 
    | **[Erőforráscsoport](../../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Az új erőforráscsoport neve, amelyben létrehozza a SignalR Service-példányt. | 
    | **Hely** | USA nyugati régiója | Válasszon egy Önhöz közeli [régiót](https://azure.microsoft.com/regions/). |
    | **Tarifacsomag** | Ingyenes | Az Azure SignalR Service-t ingyenesen is kipróbálhatja. |
    | **Egységek száma** |  Nem alkalmazható | Az egységek száma azt határozza meg, hogy a SignalR Service-példány hány kapcsolatot tud elfogadni. Ez a beállítás csak a Standard szinten konfigurálható. |

    ![SignalR szolgáltatás létrehozása](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. A SignalR Service-példány üzembe helyezésének megkezdéséhez kattintson a **Létrehozás** gombra.

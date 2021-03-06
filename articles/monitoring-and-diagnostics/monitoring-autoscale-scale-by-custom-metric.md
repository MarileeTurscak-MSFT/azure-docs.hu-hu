---
title: Az automatikus méretezés egyéni metrikával Azure-ban
description: Ismerje meg, hogy az erőforrás méretezése az Azure-beli egyéni metrika szerint.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 9df587d92b9e35db496c787186ff2945db7965ce
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987815"
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Ismerkedés az automatikus skálázás az Azure-beli egyéni metrika szerint
Ez a cikk azt ismerteti, hogy az erőforrás méretezése az Azure Portalon egy egyéni metrika szerint.

Az Azure Monitor automatikus skálázása csak érvényes [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - webalkalmazások](https://azure.microsoft.com/services/app-service/web/), és [APIManagement-szolgáltatások](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

# <a name="lets-get-started"></a>Lehetővé teszi, hogy első lépései
Ez a cikk feltételezi, hogy egy webes alkalmazást az application insights segítségével a konfigurálása. Ha Ön nem rendelkezik ilyennel, [Application Insights beállítása ASP.NET-webhely][1]

- Nyissa meg [Azure Portalon][2]
- Kattintson az Azure Monitor ikonra a bal oldali navigációs ablaktáblán.
  ![Indítsa el az Azure Monitor][3]
- Kattintson az automatikus skálázási beállítás számára, amelyen az automatikus skálázási kell alkalmazni, az automatikus skálázási aktuális állapotával együtt az összes erőforrás megtekintését ![Fedezze fel az Azure monitor automatikus méretezés][4]
- Az Azure monitorban "Automatikus" panel megnyitásához, és válasszon ki egy erőforrást méretezésére
> Megjegyzés: Az alábbi lépéseket az app service-csomag, amely rendelkezik az app insights konfigurált webalkalmazás társított használja.
- Az erőforrás a skálázási beállítás panelen láthatja, hogy a jelenlegi példányszám 1. Kattintson az "Automatikus méretezés engedélyezése".
  ![Új webalkalmazás számára a skálázási beállítás][5]
- Adjon meg egy nevet a skálázási beállítás, majd kattintson a "Hozzáadás szabály". Figyelje meg, hogy a környezet ablaktáblát a jobb oldalon megnyíló skálázási szabály beállításainak. Alapértelmezés szerint méretezheti a példányok száma 1, ha az erőforrás a CPU percetage meghaladja a 70 %-os lehetőség beállítása. Módosítsa a metrikaforrás felső "Application Insights". a "Erőforrás" legördülő listában válassza ki az app insights-erőforrás, majd válassza ki az egyéni metrika alapján a, amelyre vonatkozóan szeretné méretezni.
  ![Méretezhető, egyéni metrika][6]
- Az a fenti lépésben adja a skálázási szabályhoz, amely méretezhető és csökkentése méretezési száma 1, ha az egyéni metrika kisebb egy küszöbértéknél.
  ![A cpu kihasználtságához][7]
- Állítsa be az Ön példányszámkorlátoknál. Például ha szeretne horizontális függően az egyéni metrika ingadozások által megkövetelt 2 – 5-példányok között, állítsa a "2', 'maximális", "5" és "default", "2" "minimum"
> Megjegyzés: abban az esetben az erőforrás-metrikák olvasása során, és a jelenlegi kapacitás nem éri el az alapértelmezett kapacitásértéket, majd az erőforrások rendelkezésre állásának biztosításához maximumára skálázza ki az alapértelmezett értékre. Ha a jelenlegi kapacitás még magasabb, mint az alapértelmezett kapacitásértéket, az automatikus skálázási nem lesz skálázva a.
- Kattintson a "Mentés"

Gratulálunk! A webalkalmazás, egy egyéni metrika alapján méretezhető, hogy most a méretezési csoport automatikus beállítás létrehozása sikeresen megtörtént.

> Megjegyzés: A ugyanazokat a lépéseket kell alkalmazni a VMSS vagy felhőalapú szolgáltatás szerepkör használatának első lépései.

<!--Reference-->
[1]: https://docs.microsoft.com/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png

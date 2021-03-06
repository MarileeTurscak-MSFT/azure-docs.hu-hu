---
title: Az Azure tárolók monitorozásának áttekintése |} A Microsoft Docs
description: Ez a cikk áttekintést tárolók az Azure-ban könnyen felismerhető, a fürt állapotának és rendelkezésre állás monitorozása az Azure-ban elérhető különböző módszereket.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: db85f85011154dcc7adfa9d569e9015a9c5c33ca
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055059"
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Az Azure-beli tárolók áttekintése
Az Azure-ban, hatékonyan monitorozni és kezelni a számítási feladatok Azure-tárolók Kubernetes vagy a Docker telepítve. Fontos tudni, hogyan tárolók, mikroszolgáltatások több alkalmazással rendelkező végez annak érdekében, hogy egy megbízható szolgáltatás biztosításához ipari méretekben, és a figyelési csomag támogatja. Ez a cikk röviden áttekinti az a felügyeleti és monitorozási képességeket nyújt az Azure segítségével megismerheti azokat, és amelyek megfelelőek az igényei alapján.

Használatával [-tárolókhoz az Azure Monitor](monitoring-container-insights-overview.md), a teljesítmény és a Linux tároló-infrastruktúra állapotát megtekintheti egy pillantással és gyorsan problémák kivizsgálásában. A telemetriai adatokat a Log Analytics-munkaterületen tárolja, és szűrése az Azure Portalon, ahol megismerheti, integrált és szegmens összesített adatok az irányítópultok a terhelés, teljesítmény és állapotának figyeléséhez.  

Az üzemeltetett Azure-beli Kubernetes-szolgáltatás, a Log Analytics-on kívül futó tárolók [Windows- és Docker-tároló megoldás](../log-analytics/log-analytics-containers.md) megtekinthető és kezelhető a Windows és a Docker-tároló gazdagépek segít. A Log Analytics-munkaterületen, megtekintheti a környezet leltáradatait, teljesítmény és a csomópontok és a tárolók eseményeket. Részletes naplózási adatait megjelenítő parancsok használják tárolókhoz is megtekintheti, és a tárolók háríthatóak el megtekintésével és központosított naplók keresése távolról elérni a Docker és a Windows gazdagépekre nélkül.

Átfogó, vagy – teljes körű figyelést az alkalmazás elérése érdekében minden olyan függőséget, az Azure vagy a helyszíni erőforráshoz, hogy ellenőrizni kell az Azure Monitor vagy a Log Analytics.  Az alkalmazási rétegre tartalmaznia kell egy további rétegét állapotának figyelése, az Application Insights használatával platform és az alkalmazás szintjén egyaránt hozzáadásához. A platform szintjén vannak az Application Insights SDK-k [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/), és [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights). Mikroszolgáltatás-alkalmazások esetén nincs támogatása [Java](../application-insights/app-insights-java-get-started.md), [Node.js](../application-insights/app-insights-nodejs-quick-start.md), [.Net](../application-insights/app-insights-asp-net.md), [.Net Core](../application-insights/app-insights-asp-net-core.md), valamint számos más [nyelvek és keretrendszerek](../application-insights/app-insights-platforms.md). 

Ellenkező esetben problémák azonosítatlan kerül, amely hatással lehet az alkalmazás rendelkezésre állásának és a szolgáltatási szintű célok nem teljesül.  

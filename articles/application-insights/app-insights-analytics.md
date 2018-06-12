---
title: Elemzés - a hatékony keresési és a lekérdezés eszközt az Azure Application Insights |} Microsoft Docs
description: 'Elemzés, a hatékony diagnosztikai eszköz az Application Insights áttekintése. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: 170cd76c72e8aeb5de48c711ae4637a0244742fb
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294200"
---
# <a name="analytics-in-application-insights"></a>Az Application Insightsban elemzés
Elemzés a hatékony keresési és lekérdezés eszköz [Application Insights](app-insights-overview.md). Analytics egy webes eszköz, így nem szükséges. Ha már konfigurálta az Application Insights egy, az alkalmazások, akkor az alkalmazás adatainak elemezheti a alkalmazás Analytics megnyitásával [áttekintése panel](app-insights-dashboards.md).

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics/001.png)

Használhatja a [Analytics playground](https://go.microsoft.com/fwlink/?linkid=859557) Ez nagy mennyiségű adatot tartalmazó egy szabad bemutató környezetben.
<br>
<br>
> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

## <a name="query-data-in-analytics"></a>Az Analytics lekérdezési adatok
Egy tipikus lekérdezés kezdődik-e egy tábla nevét, majd több *operátorok* elválasztott `|`.
Például keressük meg hogy hány kérésnek a különböző országokból az utolsó 3 óra során kapott alkalmazást:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

A táblanév először *kérelmek* , és adja hozzá a védőeszközön elemek szükség szerint.  Először igazolnia idő kapcsolódó szűrő megadásához. csak az utolsó 3 órát rekordok áttekintéséhez.
Azt, majd száma az egyes országok rekordok száma (, hogy az oszlopban található adat *client_CountryOrRegion*). Végül azt jeleníti meg a tortadiagram.
<br>

![Lekérdezés eredményei](./media/app-insights-analytics/030.png)

A nyelv számos vonzó lehetőséggel rendelkezik:

* [Szűrő](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) által a mezőket, beleértve az egyéni tulajdonságok és a metrikák a nyers app telemetriai adatokat.
* [Csatlakozás](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/join-operator) több táblázatot – korrelálja Lapmegtekintések, függőségi hívások esetében, kivételeket és naplókivonatokat kéri.
* Hatékony statisztikai [összesítések](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions).
* Közvetlen és erőteljes képi megjelenítések.
* [REST API](https://dev.applicationinsights.io/) használható lekérdezések futtatása programozott módon, például a Powershellből.

A [teljes nyelvi referencia](https://go.microsoft.com/fwlink/?linkid=856079) minden támogatott parancs adatokat, és rendszeresen frissíti.

## <a name="next-steps"></a>További lépések
* Az első lépései a [Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587)
* Első lépések [lekérdezések írásáról](https://go.microsoft.com/fwlink/?linkid=856078)
* Tekintse át a [SQL-felhasználók lap cheat](https://aka.ms/sql-analytics) a leggyakrabban használt idioms kifejezés fordítását.
* Kipróbálása Analytics a a [playground](https://analytics.applicationinsights.io/demo) Ha az alkalmazás nem adatok küldése az Application Insights még.
* Tekintse meg a [bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).
---
title: Speciális lekérdezések az Azure Log Analyticsben |} A Microsoft Docs
description: Ez a cikk nyújt segítséget az Analytics-portál használatával kell lekérdezéseket írni a Log Analyticsben.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 2f9868abd0eb8bf96928aeba6f96c10bcb91c4e2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958557"
---
# <a name="writing-advanced-queries-in-log-analytics"></a>Lekérdezések a Log Analytics speciális írása

> [!NOTE]
> Hajtsa végre [az Analytics-portál – első lépések](get-started-analytics-portal.md) és [Ismerkedés a lekérdezések](get-started-queries.md) ebben a leckében befejezése előtt.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

## <a name="reusing-code-with-let"></a>Újbóli felhasználása az lehetővé teszik a kódot
Használat `let` eredmények hozzárendelése egy változóhoz, és később a lekérdezésben hivatkoznak rá:

```Kusto
// get all events that have level 2 (indicates warning level)
let warning_events=
Event
| where EventLevel == 2;
// count the number of warning events per computer
warning_events
| summarize count() by Computer 
```

Állandó értékek változókat is rendelhet. Ez támogat egy metódust, amely minden alkalommal, amikor a lekérdezés végrehajtása módosítani kell a mezők paramétereinek beállítása. Szükség szerint módosítsa ezeket a paramétereket. Ha például kiszámítása a szabad lemezterület és a szabad memória (a százalékos érték), egy adott időtartományban:

```Kusto
let startDate = datetime(2018-08-01T12:55:02);
let endDate = datetime(2018-08-02T13:21:35);
let FreeDiskSpace =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Logical Disk" and CounterName=="Free Megabytes"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
let FreeMemory =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Memory" and CounterName=="Available MBytes Memory"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
union FreeDiskSpace, FreeMemory
```

Ez megkönnyíti a befejezési idő a lekérdezés a következő futtatásakor kezdete módosítása.

### <a name="local-functions-and-parameters"></a>Helyi funkciók és paraméterek
Használat `let` használható függvények létrehozása az ugyanabból a lekérdezés utasításokat. Például adja meg a függvény, amely egy dátum/idő mezőt (az UTC-formátum) tart, és a egy Egyesült Államokbeli szabványos formátumra konvertálja. 

```Kusto
let utc_to_us_date_format = (t:datetime)
{
  strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
  bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
};
Event 
| where TimeGenerated > ago(1h) 
| extend USTimeGenerated = utc_to_us_date_format(TimeGenerated)
| project TimeGenerated, USTimeGenerated, Source, Computer, EventLevel, EventData 
```

## <a name="functions"></a>Functions
Egy függvény aliasa a lekérdezés mentheti, így más lekérdezések által hivatkozott. Például a következő standard lekérdezés visszaadja az elmúlt egy napon belül jelentett összes hiányzó biztonsági frissítések:

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Lekérdezés mentése függvényében, és adjon meg egy aliast például _security_updates_last_day_. Ezután használhatja azt egy másik lekérdezésbe SQL kapcsolódó szükséges frissítések kereséséhez:

```Kusto
security_updates_last_day | where Title contains "SQL"
```

Függvényében, egy lekérdezés mentéséhez válassza a **mentése** a portálon és a Módosítás gombra **Mentés másként** való _függvény_. A függvény aliasa betűket, számokat és aláhúzásjeleket tartalmazhat, de betűvel vagy aláhúzásjellel kell kezdődnie.

> [!NOTE]
> Függvény mentése a Log Analytics-lekérdezéseket, de jelenleg az Application Insights-lekérdezések esetében nem lehetséges.


## <a name="print"></a>Nyomtatás
`print` csak egy oszlop, és a egy számítás eredménye megjelenítése egyetlen sor tartalmazó táblát adja vissza. Ez azokban az esetekben, ahol meg kell egy egyszerű calcuation gyakran használják. Ha például az aktuális idő PST megkereséséhez, és adjon hozzá egy oszlopot a keleti téli idő:

```Kusto
print nowPst = now()-8h
| extend nowEst = nowPst+3h
```

## <a name="datatable"></a>A DataTable
`datatable` lehetővé teszi, hogy meghatározza az adatok egy készletét. Adjon meg egy séma- és a egy értékhalmazt, és majd irányítsa a táblázat bármely más lekérdezés elemek be. Hozzon létre egy táblát a memóriahasználatot és azok óránkénti átlagos érték kiszámításához, például:

```Kusto
datatable (TimeGenerated: datetime, usage_percent: double)
[
  "2018-06-02T15:15:46.3418323Z", 15.5,
  "2018-06-02T15:45:43.1561235Z", 20.2,
  "2018-06-02T16:16:49.2354895Z", 17.3,
  "2018-06-02T16:46:44.9813459Z", 45.7,
  "2018-06-02T17:15:41.7895423Z", 10.9,
  "2018-06-02T17:44:23.9813459Z", 24.7,
  "2018-06-02T18:14:59.7283023Z", 22.3,
  "2018-06-02T18:45:12.1895483Z", 25.4
]
| summarize avg(usage_percent) by bin(TimeGenerated, 1h)
```

DataTable szerkezeteket akkor is hasznos, ha egy keresési táblázat létrehozása. Például képezze le táblaadatok eseményazonosítók, például a _SecurityEvent_ tábla, eseménytípusok felsorolt máshol, hozzon létre egy keresési táblázat használatával eseménytípusokat `datatable` , és ez a datatable  _SecurityEvent_ adatokat:

```Kusto
let eventCodes = datatable (EventID: int, EventType:string)
[
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4672, "Access",
    4624, "Account activity",
    4799, "Access management",
    4798, "Access management",
    5059, "Key operation",
    4648, "A logon was attempted using explicit credentials",
    4768, "Access management",
    4662, "Other",
    8002, "Process action",
    4904, "Security event management",
    4905, "Security event management",
];
SecurityEvent
| where TimeGenerated > ago(1h) 
| join kind=leftouter (
  eventCodes
) on EventID
| project TimeGenerated, Account, AccountType, Computer, EventType
```

## <a name="next-steps"></a>További lépések
Tekintse meg a Log Analytics lekérdezési nyelv segítségével a többi leckék:

- [Karakterlánc-műveletek](string-operations.md)
- [Dátum és idő műveletek](datetime-operations.md)
- [Összesítésfüggvények](aggregations.md)
- [Speciális összesítések](advanced-aggregations.md)
- [JSON és adatstruktúrák](json-data-structures.md)
- [Illesztés](joins.md)
- [Diagramok](charts.md)
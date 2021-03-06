---
title: Az Azure Log Analytics-lekérdezések összesítések |} A Microsoft Docs
description: Ismerteti, összesítési függvények a Log Analytics-lekérdezéseket, amelyek kínálnak hasznos módszer az adatok elemzéséhez.
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
ms.openlocfilehash: 764c43a382442096a5d130334e54afdc135ba419
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966680"
---
# <a name="aggregations-in-log-analytics-queries"></a>Összesítések a Log Analytics-lekérdezések

> [!NOTE]
> Hajtsa végre [az Analytics-portál – első lépések](get-started-analytics-portal.md) és [Ismerkedés a lekérdezések](get-started-queries.md) ebben a leckében befejezése előtt.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Ez a cikk ismerteti az összesítő függvények a Log Analytics-lekérdezéseket, amelyek kínálnak hasznos módszer az adatok elemzéséhez. Ezek a függvények minden dolgozni a `summarize` operátort, amelynek összesített eredmények a bemeneti tábla egy táblát hoz létre.

## <a name="counts"></a>Száma

### <a name="count"></a>count
Szűrők alkalmazása után az eredményhalmazban sorok számát. Az alábbi példa adja vissza a sorok száma a _Teljesítményoptimalizált_ táblát az elmúlt 30 percben. Az eredményt adja vissza egy adott nevű oszlopban *count_* , kivéve, ha egy adott nevét rendelje hozzá:


```Kusto
Perf
| where TimeGenerated > ago(30m) 
| summarize count()
```

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| summarize num_of_records=count() 
```

Lehet, hogy idővel a trendek kíváncsi idődiagramját Vizualizációk:

```Kusto
Perf 
| where TimeGenerated > ago(30m) 
| summarize count() by bin(TimeGenerated, 5m)
| render timechart
```

A jelen példa kimenetében teljesítményoptimalizált rekordszám trendvonalat 5 perc időközök látható:

![Száma – trendek](media/aggregations/count-trend.png)


### <a name="dcount-dcountif"></a>DCount, dcountif
Használat `dcount` és `dcountif` külön értékek egy adott oszlopban. A következő lekérdezés kiértékeli, hogy hány egyedi számítógépek számlálása az elmúlt órában:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcount(Computer)
```

Csak a Linux rendszerű számítógépek számlálása használjuk `dcountif`:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcountif(Computer, OSType=="Linux")
```

### <a name="evaluating-subgroups"></a>Alcsoportok kiértékelése
Egy szám vagy más összesítéseket végre az adatok az alcsoportok, használja a `by` kulcsszót. Például különböző Linux rendszerű számítógépek számlálása az egyes országok számát:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry
```

|RemoteIPCountry  | distinct_computers  |
------------------|---------------------|
|Egyesült Államok    | 19                  |
|Kanada           | 3                   |
|Írország          | 0                   |
|Egyesült Királyság   | 0                   |
|Hollandia      | 2                   |


Elemezheti az adatokat még kisebb alcsoportok, adjon hozzá további oszlopok az `by` szakaszban. Például előfordulhat, hogy szeretné az egyes országból / OSType különböző számítógépek száma:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry, OSType
```

## <a name="percentiles-and-variance"></a>Percentiliseinek és eltérés
Numerikus értékek kiértékelésekor általános gyakorlat, hogy azok átlagos használatával `summarize avg(expression)`. Átlagokat rendkívüli értékek csak néhány esetben írhatók le vannak hatással. A probléma megoldásához használhatja kevésbé érzékeny funkciók például `median` vagy `variance`.

### <a name="percentile"></a>Percentilis
A középérték megkereséséhez használja a `percentile` függvény megadása a PERCENTILIS értékkel:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 50) by Computer
```

Megadhat egy összesített eredményre beolvasni az egyes különböző percentilisei is:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 25, 50, 75, 90) by Computer
```

Ez előfordulhat, hogy mutatja, hogy néhány számítógépen processzorok értékük hasonló medián, de néhány állandó középértékének körül, amíg más számítógépek jelentett sokkal kisebb és nagyobb CPU értékeket, ami azt jelenti, adatforgalmi csúcsokhoz lépett fel.

### <a name="variance"></a>Variancia
Érték varianciáját közvetlenül kiértékelni, használja a szórás és eltérés módszerek:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), variance(CounterValue) by Computer
```

Egy jó stabilitását, a CPU-használat elemzése módja stdev kombinálva a középérték számítás:

```Kusto
Perf
| where TimeGenerated > ago(130m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), percentiles(CounterValue, 50) by Computer
```

Tekintse meg a Log Analytics lekérdezési nyelv segítségével a többi leckék:

- [Karakterlánc-műveletek](string-operations.md)
- [Dátum és idő műveletek](datetime-operations.md)
- [Speciális összesítések](advanced-aggregations.md)
- [JSON és adatstruktúrák](json-data-structures.md)
- [Speciális lekérdezés írása](advanced-query-writing.md)
- [Illesztés](joins.md)
- [Diagramok](charts.md)

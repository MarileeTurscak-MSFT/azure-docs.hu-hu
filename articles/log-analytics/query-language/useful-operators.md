---
title: Az Azure Log Analytics-lekérdezések hasznos operátorok |} A Microsoft Docs
description: Közös funkciók használata a Log Analytics-lekérdezéseket a különböző helyzetekhez.
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
ms.date: 08/21/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: ce397b1ba8d77d2916caa2798c0161ba55f51dbb
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/22/2018
ms.locfileid: "42447483"
---
# <a name="useful-operators-in-log-analytics-queries"></a>A Log Analytics-lekérdezések hasznos operátorok

Az alábbi táblázat néhány gyakori függvények használata a Log Analytics-lekérdezéseket a különböző helyzetekhez.

## <a name="useful-operators"></a>Hasznos operátorok

Kategória                                |Megfelelő analitikai függvény
----------------------------------------|----------------------------------------
Kijelölés és oszlop aliasok            |`project`, `project-away`, `extend`
Az ideiglenes táblák és állandók          |`let scalar_alias_name = …;` <br> `let table_alias_name = (){ … &#124; … &#124; … };`
Összehasonlítás és karakterlánc-operátorok         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Karakterlánc-függvények                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Közös matematikai függvények                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Szöveg elemzése                            |`extract()`, `extractjson()`, `parse`, `split()`
Kimeneti korlátozása                         |`take`, `limit`, `top`, `sample`
Dátumfüggvények                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Csoportosítás és összesítés                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Illesztés és illesztésre                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Rendezési sorrend                             |`sort`, `order` 
Dinamikus objektum (JSON és array)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Logikai operátorok                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Gépi tanulás                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>További lépések

- A lecke meg a [lekérdezések írása a Log Analytics](get-started-queries.md).
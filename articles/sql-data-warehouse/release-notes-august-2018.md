---
title: Az Azure SQL Data Warehouse kibocsátási megjegyzések 2018 augusztus |} A Microsoft Docs
description: Kibocsátási megjegyzések az Azure SQL Data warehouse-hoz.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/13/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: c52fcd544e22c274d0e9053da93d2b9dee0df695
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286632"
---
# <a name="whats-new-in-azure-sql-data-warehouse-august-2018"></a>Mi az új Azure SQL Data warehouse? 2018. augusztus
Az Azure SQL Data Warehouse fejlesztései folyamatosan kap. Ez a cikk ismerteti az új szolgáltatásokat és változásokat tartalmazza, amelyek a 2018 augusztus.

## <a name="automatic-intelligent-insights"></a>Automatikus Intelligent Insights
A Microsoft vezetett be [automatikus intelligent insights](https://azure.microsoft.com/blog/automatic-intelligent-insights-to-optimize-performance-with-sql-data-warehouse/) az adatraktárhoz Automation felhőalapú megkönnyítse. Már nem figyelheti az adattárházat, az adatok torzulása és az optimálisnál rosszabb Táblastatisztika kell. További költségek nélkül az SQL Data Warehouse feltárásával intelligent insights összes Gen2-példánya számára. Integrálásával [az Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), automatikusan megkapja a aktív számítási feladatok teljesítményének javítása érdekében kapcsolatos bevált gyakorlatokat. Az SQL Data Warehouse a számítási feladatok, és felület javaslatokat, a használat alapján elemzi. Ez az elemzés történik napi lehetővé teszi a használati jelentések és javaslatok fejlesztései a számítási feladat figyeléséhez.

A javaslatok az Advisor az Azure Portalon is megtekintheti: ![Azure Advisor portál javaslatokat az Azure SQL Data warehouse-hoz](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/4e205b6d-df04-48db-8eec-d591f2592cf4.png)

Részletesen is megtekintheti az adott riasztásra vonatkozó javaslatok láthatók kategóriákhoz: ![Azure Advisor portál javaslat részletei az Azure SQL Data warehouse-hoz](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/3c42426e-6969-46e3-9025-c34c0755a302.png)


## <a name="bug-fixes"></a>Hibajavítások

| Beosztás | Leírás |
|:---|:---|
| **A felosztás száma meghaladja a maximális korlátot lehetséges lekérdezés-hibák** |A felső határa 1 millió fájl-felosztás korlát túllépésekor nem kezelt kivételt okozott az SQL-kezelő kiírása, és az összes lekérdezés nem sikerült. A javítás címmel a probléma a kivétel megfelelően kezeli, és anélkül, hogy ez a lekérdezések hibát adnak vissza. |
| **ExternalMoveReadersPerNode alapértelmezett hasznosításával betöltési teljesítmény javítása érdekében** |A probléma a ExternalMoveReadersPerNode tulajdonság beállítása folyamatban van a szinkronizált beállítása service fabric okozta. A regresszió miatt egy csökkentett teljesítményű Gen2 betöltési teljesítmény. A javítás Gen2 betöltési teljesítmény optimalizált tervezési paraméterek belül újra elérhetővé teszi.|


## <a name="next-steps"></a>További lépések
Most, hogy jobban megismerte az SQL Data warehouse, megtudhatja, hogyan lehet gyorsan [hozzon létre egy SQL Data Warehouse][create a SQL Data Warehouse]. Ha az Azure új felhasználója, hasznosnak találhatja az [Azure szószedetét][Azure glossary], amikor az új fogalmakkal ismerkedik. Vagy tekintsen meg néhányat a többi SQL Data Warehouse-erőforrás közül.  

* [Ügyfelek sikertörténetei]
* [Blogok]
* [Funkciókérések]
* [Videók]
* [Az ügyféltanácsadói csapat blogjai]
* [Stack Overflow-fórum]
* [Twitter]


[Blogok]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Az ügyféltanácsadói csapat blogjai]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Ügyfelek sikertörténetei]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Funkciókérések]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-fórum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videók]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
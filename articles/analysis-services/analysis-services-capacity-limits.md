---
title: Az Azure Analysis Services-erőforrás és objektum korlátozásai |} A Microsoft Docs
description: Azure Analysis Services erőforrás- és korlátokat ismerteti.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4c2cebe2225e475ccd40460e7b10a6ba3ed428d5
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724040"
---
# <a name="analysis-services-resource-and-object-limits"></a>Analysis Services erőforrás- és objektum korlátozásai

Ez a cikk bemutatja az erőforrás és a modell korlátok objektumot.

## <a name="tier-limits"></a>Csomag

### <a name="developer-tier"></a>Fejlesztői szint

Ezt a szintet kiértékeléshez, valamint fejlesztési és tesztelési forgatókönyvekhez ajánljuk. Egyetlen csomagban tartalmazza a standard szintű csomagéval megegyező funkciókat, de korlátozott feldolgozási teljesítménnyel, QPU-val és memóriamérettel rendelkezik. Kibővített lekérdezésreplika ehhez a szinthez *nem érhető el*. Ehhez a szinthez nem tartozik SLA.

|Felkészülés  |QPU-k  |Memória (GB)  |
|---------|---------|---------|
|D1    |    20     |    3     |


### <a name="basic-tier"></a>Alapszintű csomag

Ezt a szintet olyan éles környezetben való használatra ajánlunk, amelyben kis méretű táblázatos modellek, korlátozott mennyiségű párhuzamos felhasználó és egyszerűbb adatfrissítési követelmények szerepelnek. Kibővített lekérdezésreplika ehhez a szinthez *nem érhető el*. A perspektívák, a több partíció használata és a DirectQuery táblázatosmodell-funkciók *nem támogatottak* ezen a szinten.  

|Felkészülés  |QPU-k  |Memória (GB)  |
|---------|---------|---------|
|B1    |    40     |    10     |
|B2    |    80     |    20     |

### <a name="standard-tier"></a>Standard csomag

Ez a szint olyan létfontosságú, éles környezetben használt alkalmazásokhoz ideális, amelyek rugalmasságot követelnek meg a párhuzamos felhasználói tevékenységekre vonatkozóan, és amelyek gyorsan növekvő adatmodelleket használnak. Támogatja a speciális adatfrissítést az adatmodellek közel valós idejű frissítése érdekében, valamint az összes táblázatos modellezési funkciót is.

|Felkészülés  |QPU-k  |Memória (GB)  |
|---------|---------|---------|
|S1    |    40     |    10     |
|S2    |    100     |    25     |
|S3    |    200     |    50     |
|S4    |    400     |    100     |
|S8*    |    320     |    200     |
|S9*    |    640    |    400     |

\* Nem érhető el az összes régióban.  

## <a name="object-limits"></a>Objektum korlátok

Ezek a korlátok elméleti. Teljesítmény fog csökkenteni kell a alacsonyabb sorszámúak címen.

|Objektum|Maximális méret és számok|  
|------------|----------------------------|  
|Adatbázis-példány|16,000|  
|Táblák és oszlopok adatbázisban összesített száma|16,000|  
|Egy tábla sorainak|Korlátlan<br /><br /> **Figyelmeztetés:** a korlátozás, hogy nincs egyetlen oszlop a tábla több mint 1,999,999,997 különböző értékekkel rendelkezhet.|  
|Egy tábla hierarchiák|15,999|  
|A hierarchia szintek|15,999|  
|Kapcsolatok|8,000|  
|Az összes tábla oszlopainak kulcs|15,999|  
|A táblákban mértékek|2 ^ 31 – 1 = 2 147 483 647|  
|Egy lekérdezés által visszaadott cellák|2 ^ 31 – 1 = 2 147 483 647|  
|Az adatforrás-lekérdezés rögzítése mérete|64K|  
|Objektum nevének hossza|512 karakter|  



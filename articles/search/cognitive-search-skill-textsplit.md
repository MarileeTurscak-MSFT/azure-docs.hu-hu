---
title: Szöveg felosztása a kognitív keresés szakértelem (Azure Search) |} A Microsoft Docs
description: Szöveg felosztása adattömbökre vagy a szöveget az Azure Search-felderítési bővítést folyamatban hossza alapján.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 583d2ac5a8ac4c236612cdfe78595da1812c56fa
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730766"
---
#   <a name="text-split-cognitive-skill"></a>Szöveg felosztása cognitive szakértelem

A **szöveg felosztása** szakértelem működésképtelenné válik a szöveg adattömbökbe szöveg. Megadhatja, hogy szeretné-e a szöveg kezdetét mondatokat vagy egy adott hosszúságú oldalak. Ez szakértelem, különösen hasznosak, ha olyan szöveg maximális aktiválásához megszerzett képességeit a hosszúsági követelményeknek. 

> [!NOTE]
> A kognitív keresés nyilvános előzetes verzióban érhető el. Képességcsoport végrehajtási, és a lemezkép kinyerése és a normalizálási jelenleg rendelkezésre állnak az ingyenes. Később az ezen funkciók díjszabásáról jelentjük be. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Ismeretek paraméterek

A paraméterei a kis-és nagybetűket.

| Paraméter neve     | Leírás |
|--------------------|-------------|
| textSplitMode      | "Lapok" vagy "mondatok" | 
| maximumPageLength | Ha textSplitMode "lapok" értékre van állítva, ez vonatkozik a mért maximális hossza `String.Length`. A minimális érték 100. | 
| defaultLanguageCode   | (nem kötelező) A következő nyelvi kódok: `da, de, en, es, fi, fr, it, ko, pt`. Alapértelmezés szerint angol (en). Érdemes figyelembe venni néhány dolgot:<ul><li>Ha egy languagecode-countrycode formátum, a formátum csak a languagecode részét szolgál.</li><li>A nyelv nem szerepel a fenti listán, ha a felosztás szakértelem karakter határokat, működésképtelenné válik a szöveg.</li><li>Biztosít egy nyelvi kód hasznos darabolás szó nem terület nyelveken, például a kínai, japán és koreai megfelezése elkerülése érdekében.</li></ul>  |


## <a name="skill-inputs"></a>Ismeretek bemenetek

| Paraméter neve       | Leírás      |
|----------------------|------------------|
| szöveg  | Szöveg felosztása karakterláncrészletet. |
| languageCode  | (Nem kötelező) A dokumentum nyelvkód.  |

## <a name="skill-outputs"></a>Ismeretek kimenetek 

| Paraméter neve     | Leírás |
|--------------------|-------------|
| textItems | Egy tömb, amely a könyvtárban találhatók karakterláncrészletek. |


##  <a name="sample-definition"></a>Minta-definíció

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
        }
    ]
}
```

##  <a name="sample-input"></a>Minta beviteli

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
                "languageCode": "en"
            }
        }
    ]
}
```

##  <a name="sample-output"></a>Kimeneti példa

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Hibák esetén
A nyelv nem támogatott, ha generál a rendszer figyelmeztetést, és a szöveg van felosztva, karakter határok.

## <a name="see-also"></a>Lásd még

+ [Előre megadott képesség](cognitive-search-predefined-skills.md)
+ [Hogyan képességcsoport megadása](cognitive-search-defining-skillset.md)

---
title: Szöveg egyesítési cognitive search szakértelem (Azure Search) |} A Microsoft Docs
description: A mezők gyűjteménye szöveg egyesítése egy konszolidált mezőt. Cognitive szakértelem használja az Azure Search-felderítési bővítést folyamatban.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 5bd1b534386f38f8dd3a78bbd98ffd012875351f
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736155"
---
#    <a name="text-merge-cognitive-skill"></a>Szöveg egyesítési cognitive szakértelem

A **szöveg egyesítése** szakértelem összesíti a szöveget a mezők gyűjteménye egyetlen mezőbe. 

> [!NOTE]
> A kognitív keresés nyilvános előzetes verzióban érhető el. Képességcsoport végrehajtási, és a lemezkép kinyerése és a normalizálási jelenleg rendelkezésre állnak az ingyenes. Később az ezen funkciók díjszabásáról jelentjük be. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.MergeSkill

## <a name="skill-parameters"></a>Ismeretek paraméterek

A paraméterei a kis-és nagybetűket.

| Paraméter neve     | Leírás |
|--------------------|-------------|
| insertPreTag  | Szerepelnie, előtt minden behelyettesítendő karakterlánc. Az alapértelmezett érték `" "`. Hagyja ki a helyet, állítsa az értékét `""`.  |
| insertPostTag | Karakterlánc minden Beszúrás után fogja tartalmazni. Az alapértelmezett érték `" "`. Hagyja ki a helyet, állítsa az értékét `""`.  |


##  <a name="sample-input"></a>Minta beviteli
JSON-dokumentumok biztosítása használható bemeneti szakértelem lehet:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The brown fox jumps over the dog" ,
             "itemsToInsert": ["quick", "lazy"],
             "offsets": [3, 28],
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Példa kimenet
Ez a példa bemutatja a korábbi bemenet, feltéve, hogy a kimenet a *insertPreTag* értékre van állítva `" "`, és *insertPostTag* értékre van állítva `""`. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="extended-sample-skillset-definition"></a>A kiterjesztett minta indexmezők definíciója

Egy általános forgatókönyv a szöveg egyesítési be egy dokumentumot content mezőjének a képek (-OCR szakértelem, vagy a kép felirata szöveg) értéket képviselő szöveges alak egyesíteni. 

A következő példa indexmezők OCR szakértelem szövegeket nyerhet ki képekből a beágyazása a dokumentumba használ. Ezután létrehoz egy *merged_text* eredeti és az egyes rendszerképek OCRed szöveget tartalmazó mezőbe. 

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " "
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```
A fenti példa feltételezi, hogy létezik-e a normalized-lemezképek mező. Normalized-lemezképek mező kapni, állítsa be a *imageAction* az indexelő meghatározását, hogy a konfigurációs *generateNormalizedImages* alább látható módon:

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>Lásd még

+ [Előre megadott képesség](cognitive-search-predefined-skills.md)
+ [Hogyan képességcsoport megadása](cognitive-search-defining-skillset.md)
+ [Az indexelő (REST) létrehozása](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

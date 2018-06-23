---
title: Előre elkészített LUIS entitások elavulnak referencia - Azure |} Microsoft Docs
titleSuffix: Azure
description: A cikkben kora előre elkészített entitás információ a nyelvi ismertetése (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 59732469cf0d1e55643f3977958ec34a887130d3
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321987"
---
# <a name="age-entity"></a>Kor entitás
Az előre elkészített kora entitás a kora egyaránt numerikus és tekintetében nap, hét, hónap és év rögzíti. Az entitás már be van tanítva, mert nem kell az alkalmazás leképezések korra tartalmazó példa utterances hozzáadása. Támogatott a kor entitás [számos kultúra](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Kor típusai
Kor felügyelete a [felismerő szöveges](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) Github-adattár

## <a name="resolution-for-prebuilt-age-entity"></a>Előre elkészített kora entitás felbontás
A következő példa bemutatja a feloldása a **builtin.age** entitás.

```JSON
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8236133
    }
  ],
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

## <a name="next-steps"></a>További lépések

További tudnivalók a [pénznem](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), és [dimenzió](luis-reference-prebuilt-dimension.md) entitásokat. 
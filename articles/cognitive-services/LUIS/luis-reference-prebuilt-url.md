---
title: A LUIS előre összeállított entitások URL-hivatkozással – Azure |} A Microsoft Docs
titleSuffix: Azure
description: Ez a cikk URL-címet tartalmaz az előre összeállított Entitásadatok a Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 86989abab1dcf64384b8b26b9484bc508f2ce31f
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236788"
---
# <a name="url-entity"></a>URL-cím entitás
Entitás URL-cím tartománynév vagy IP-címek URL-címek kibontása. Az entitás már be van tanítva, mert nem kell az alkalmazás URL-címeket tartalmazó példa beszédmódok hozzáadása. URL-cím entitás támogatott `en-us` culture csak. 

## <a name="types-of-urls"></a>URL-címeinek típusai
URL-cím felügyelje a [felismerő szöveges](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) Github-adattár

## <a name="resolution-for-prebuilt-url-entity"></a>Előre összeállított URL-cím entitás feloldása
Az alábbi példa bemutatja a feloldása a **builtin.url** entitás.

```JSON
{
  "query": "http://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "http://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>További lépések

További információ a [sorszámnál](luis-reference-prebuilt-ordinal.md), [szám](luis-reference-prebuilt-number.md), és [hőmérséklet](luis-reference-prebuilt-temperature.md) entitásokat.
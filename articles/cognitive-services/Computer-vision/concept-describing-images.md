---
title: Ismertető a rendszerképek – Computer Vision
titleSuffix: Azure Cognitive Services
description: Ismertető a képek a Computer Vision API használatával kapcsolatos fogalmakat.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: a65e3ea2fb28ca8a2250fb3e39860eb5e08c18f4
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981984"
---
# <a name="describing-images"></a>Képek leírása

Számítógépes Látástechnológiai algoritmus elemezheti a tartalmat a képet. Ez az elemzés egy "leírás" teljes mondatokban emberek számára olvasható nyelveként megjelenik a alapját képezi. A leírás összefoglalja, hogy mi az a képen található. Számítógépes Látástechnológiai algoritmus alapuló a vizuális jellemzőket azonosítja a kép különböző leírások létrehozásához. Leírás ki lesz értékelve, és a egy megbízhatósági pontszám jönnek létre. A megbízhatósági pontszámok listája csökkenő sorrendbe rendezve érkezik vissza.

## <a name="image-description-example"></a>Kép leírását bemutató példa

A következő JSON-választ mutatja be, milyen számítógépes Látástechnológiai ad vissza, ha a alapuló a vizuális jellemzőket példaképen leíró.

![B & W épületek](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

A robot, amely ezt a technológiát használja, a lemezkép feliratok létrehozása egy példát találhat [Itt](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption).  

## <a name="next-steps"></a>További lépések

Fogalmak ismertetése [rendszerképek címkézése](concept-tagging-images.md) és [lemezképek kategorizálásához](concept-categorizing-images.md).
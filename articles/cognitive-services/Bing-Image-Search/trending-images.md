---
title: A felkapott képek – a Bing Image Search API webes keresés
titleSuffix: Azure Cognitive Services
description: Ismerje meg, hogyan kereshet a felkapott képek weben a Bing Image Search API használatával.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 00d992da1d061ddbbee59e93898a92b6701f1ffb
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296311"
---
# <a name="get-trending-images-from-the-bing-image-search-api"></a>A Bing Image Search API a felkapott képek bekérése

A mai népszerű képek lekéréséhez a következő GET kérelmet küldeni:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

A népszerű képek API jelenleg csak a következő piacok támogatja:  

- en-US (angol nyelven, Egyesült Államok)  
- en-hitelesítésszolgáltató (angol nyelven, Kanada)  
- en-Ausztrália (angol nyelven, Ausztrália)  
- zh-CN (kínai, Kína)

A válasz tartalmaz egy [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#trendingimages) objektum, amely felsorolja a képek kategória szerint. A kategória `title` a képeket a felhasználói élmény a csoporthoz. A kategóriák a napi változhatnak.  

```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  

Minden csempe kép és a kapcsolódó képek első beállításokat tartalmazza. A kapcsolódó képek bekérése, használhatja a lekérdezés `text` hívja a [Image Search API](./search-the-web.md) és a kapcsolódó képek megjelenítése, saját magának. Vagy használhatja az URL-címet `webSearchUrl` érvénybe a felhasználót, hogy a Bing lemezképek keresési eredmények oldal, amely a kapcsolódó lemezképet is tartalmaz.

Ha felhívja a képkeresési API, a kapcsolódó képek bekérése, állítsa be a [azonosító](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#id) lekérdezési paraméter az azonosítót a `id` mező. Az azonosító megadása, hogy a válasz tartalmazza-e a lemezkép (legyen az első képet a válaszban) és a kapcsolódó képek biztosítja. Emellett állítsa a [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#q) lekérdezési paraméter, a szöveget a `query` objektum `text` mező.

Az alábbi példa bemutatja, hogyan végezheti a lemezkép-azonosító az előző népszerű képek API-válasz Mr. Smith kapcsolódó rendszerképeket.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

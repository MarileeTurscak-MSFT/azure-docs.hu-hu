---
title: 'Bing Custom Search: Oldalon keresztül elérhető weblapjait |} A Microsoft Docs'
description: Bemutatja, hogyan végig az összes Bing adhatnak vissza a weblapok lapon.
services: cognitive-services
author: brapel
manager: ehansen
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 5692776736090c55547c2fe934e2c0aaf840d3c5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982358"
---
# <a name="paging-webpages"></a>Lapozófájl weblapok 

A Custom Search API hívásakor a Bing eredmények listáját adja vissza. A lista egy teljes száma, amely fontos lehet a lekérdezési eredmények egy részét. Az első rendelkezésre álló válaszokat becsült teljes száma, a válasz-objektum eléréséhez [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#totalestimatedmatches) mező.  
  
A következő példa bemutatja a `totalEstimatedMatches` mező, amely egy webes válasz tartalmazza.  
  
```  
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```  
  
Oldalon keresztül a rendelkezésre álló weboldalakat, használja a [száma](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#count) és [eltolás](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#offset) lekérdezési paramétereket.  
  
A `count` paraméter adja meg a válaszban visszaadott eredmények száma. Az eredmények, amelyek a válaszban vonatkozó kérések maximális száma érték az 50. Az alapértelmezett érték 10. A tényleges szám i lehet kisebb, mint a kért.

A `offset` paraméter adja meg a kihagyandó eredmények száma. A `offset` nulláról induló, és lehet kisebb, mint (`totalEstimatedMatches` - `count`).  
  
Ha azt szeretné, egy-egy lapon 15 megjelenítésénél, így állíthatja `count` 15-re és `offset` 0 beolvasni az eredmények első oldala. Minden ezt követő laphoz, akkor növelni `offset` 15 (például 15, 30).  
  
Az alábbiakban látható egy példa, amely a kérelmek 45 eltolástól kezdve 15 weblapok.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>
Host: api.cognitive.microsoft.com  
```  

Ha az alapértelmezett `count` érték a megvalósítás esetében működik, csak meg kell adnia a `offset` lekérdezési paraméter.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>  
Host: api.cognitive.microsoft.com  
```  


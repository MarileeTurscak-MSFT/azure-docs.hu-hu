---
title: Hírek Search API – első lépések |} Microsoft Docs
description: Első lépések a Bing hírek Search API használatával jeleníti meg.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 9CF6EAF3-42D8-4321-983C-4AC3896E8E03
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 2aa1b3de07bd61eebfc1d410cda76c32fc7c958e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35349195"
---
# <a name="your-first-news-search-query"></a>Az első hírek keresési lekérdezés

Mielőtt az első hívás, kell egy kognitív szolgáltatások előfizetés kulcs beszerzése. Ahhoz, hogy a kulcs, lásd: [kognitív szolgáltatások próbálja](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api).

Ahhoz, hogy csak hírek találatok, elküldése egy GET kérelmet a következő végponthoz:

```http
https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

A kérelmet HTTPS protokollt kell használnia.

Azt javasoljuk, hogy az összes kérelem-kiszolgálótól származik. A kulcs terjesztése ügyfélalkalmazás részeként rosszindulatú harmadik fél elérésére több lehetőséget biztosít a. Is, a kiszolgálótól érkező hívás egyetlen frissítési pontot biztosít az API-t a jövőbeli verzióiban.

A kérelemnek meg kell adnia a [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query) lekérdezési paraméter, amely tartalmazza a felhasználó keresési kifejezés. Bár nem kötelező, a kérelem is kell megadnia a [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#mkt) lekérdezési paraméter, amely azonosítja a piacon, ha azt szeretné, hogy az eredmények származnia. Nem kötelező listájának lekérdezési paramétert, mint `freshness` és `textDecorations`, lásd: [lekérdezésparamétereket](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters). Az összes lekérdezés paraméterértékek URL-kódolású kell lennie.

A kérelemnek meg kell adnia a [Ocp-Apim-előfizetés-kulcs](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#subscriptionkey) fejléc. Bár nem kötelező, meg hosszúan is adja meg a következő fejléc:

- [Felhasználói ügynök](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#clientid)
- [X-keresési-Ügyfélip](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#clientip)
- [X keresése](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#location)

Az ügyfél IP-cím és a hely fejlécek fontosak hely kompatibilis tartalom visszaküldésével.

Az összes kérés- és válaszfejlécekről listájáért lásd: [fejlécek](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#headers).

## <a name="the-request"></a>A kérelem

Az alábbi példában látható, amely tartalmazza a javasolt lekérdezés-paraméterek és a fejlécek hírek kérelmet. Ha most először hívja a Bing API-k bármelyikét, az ügyfél-azonosító fejléc nem tartalmaz. Az ügyfél-azonosító csak tartalmazzák, ha korábban már hívott a Bing API és a Bing visszaadott egy ügyfél-Azonosítót, a felhasználó és eszköz kombinációja.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

## <a name="the-response"></a>A válasz

Az alábbiakban látható a korábbi kérelemre adott válasz. A példában is látható a Bing-specifikus response fejlécekkel együtt.

```http
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "News",
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",
    "totalEstimatedMatches" : 88400,
    "value" : [{
        "name" : "Sailing Vies for Four Trophies",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.9C23AA5...",
                "width" : 650,
                "height" : 341
            }
        },
        "description" : "Sailing College Rankings, presented by Zim...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "contoso.com"
        }],
        "datePublished" : "2017-04-14T15:28:00"
    },
    {
        "name" : "Reunion at Fabrikam Lakes Sailing Club, celebrates 50 years...",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891F...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.38210...",
                "width" : 650,
                "height" : 366
            }
        },
        "description" : "The reunion on April 29, at Fabrikam Lakes Sailing...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "Contoso"
        }],
        "datePublished" : "2017-04-14T13:08:00"
    },
    {
        "name" : "Sailing Club to host Dinghy Sailing World...",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891FE99...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.364AB41...",
                "width" : 448,
                "height" : 300
            }
        },
        "description" : "The sailing club that trained Olympian Ben...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "Contoso"
        }],
        "datePublished" : "2017-04-04T11:02:00",
        "category" : "Sports"
    },
    {
        "name" : "A 24-Carat Dinghy",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891F...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.6CC2...",
                "width" : 700,
                "height" : 466
            }
        },
        "description" : "“Hard dinghies are for purists, the kind of people who...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "contoso.com"
        }],
        "datePublished" : "2017-04-03T12:14:00",
        "category" : "Politics"
    }]
}
```

## <a name="next-steps"></a>További lépések

Próbálja ki az API-t. Ugrás a [hírek keresési API-tesztelési konzol](https://dev.cognitive.microsoft.com/docs/services/56b43f72cf5ff8098cef380a/operations/56f02400dbe2d91900c68553).

A válasz objektumok fel kapcsolatos részletekért lásd: [Bing hírek Search újdonságai?](./search-the-web.md). További információ a következő gyakori műveletei is találhat:

- [A mai híreket beolvasása](./search-the-web.md#getting-todays-top-news)
- [Első Hírek kategória szerint](./search-the-web.md#getting-news-by-category)
- [Első trendekkel hírek](./search-the-web.md#getting-trending-news)

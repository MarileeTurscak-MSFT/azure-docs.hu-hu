---
title: Alapértelmezett címke insights |} Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Az alapértelmezett insights Visual keresési visszaadó kép ismerteti.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/17/2018
ms.author: scottwhi
ms.openlocfilehash: c0bf51ab86e2ba99aeb859ea415e1afd355a86f2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35348702"
---
# <a name="default-insights-tag"></a>Az alapértelmezett insights címke

Az alapértelmezett insights címke, a másikat a `displayName` mező üres karakterláncot kell beállítani. Az alábbi példa alapértelmezett insights (műveletek) lehetséges listáját jeleníti meg.

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {
          "_type" : "ImageModuleAction",
          "actionType" : "PagesIncluding",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageShoppingSourcesAction",
          "actionType" : "ShoppingSources",
          "data" : {
            "offers" : [...]
          }
        },
        {
          "image" : {...},
          "actionType" : "MoreSizes"
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "VisualSearch",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRecipesAction",
          "actionType" : "Recipes",
          "data" : {
            "value" : [...]
          }
        },
        {
          "image" : {...},
          "actionType" : "ImageById"
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "ProductVisualSearch",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRelatedSearchesAction",
          "actionType" : "RelatedSearches",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRelatedSearchesAction",
          "actionType" : "DocumentLevelSuggestions",
          "data" : {
            "value" : [...]
          }
        }
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

## <a name="pagesincluding-insight"></a>PagesIncluding felmérése

A PagesIncluding insight, amelyek közé tartoznak a lemezkép listáját tartalmazza. Ténylegesen kép objektumok listájának és a `hostPageUrl` mező tartalmaz ahhoz a weboldalhoz, amely tartalmazza a kép URL-CÍMÉT. Példa a használatra, lásd: [PagesIncluding példa](./bing-insights-usage.md#pagesincluding-insight-example). 

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "PagesIncluding",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https://www.bing.com\/images\/search?",
              "name" : "Today's smoking hot country",
              "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP...",
              "datePublished" : "2017-09-20T12:00:00.0000000Z",
              "contentUrl" : "http:\/\/contoso.com\/wordstuff",
              "hostPageUrl" : "http:\/\/contoso.com\/2017\/09\/20\/car",
              "contentSize" : "122540 B",
              "encodingFormat" : "jpeg",
              "hostPageDisplayUrl" : "contoso.com\/2017\/09\/20\/car",
              "width" : 894,
              "height" : 1200,
              "thumbnail" : {
                "width" : 474,
                "height" : 636
              },
              "imageInsightsToken" : "ccid_CO5GEthj*mid_5323B1",
              "insightsMetadata" : {
                "pagesIncludingCount" : 12,
                "availableSizesCount" : 7
              },
              "imageId" : "5323B1900FB9087B6B45D176D234E1F2F23CD3A5",
              "accentColor" : "55585B"
            }
          ]
        }
      }
```

## <a name="shoppingsources-insight"></a>ShoppingSources felmérése

A ShoppingSources insight webhelyek, ha a felhasználó vásárolhatja meg az elemet, az ábrán látható listáját tartalmazza. A weblap, ahol a felhasználó elem megvásárlásához előfordulhat, hogy az URL-cím, az ár, az elem és minősítési vagy felülvizsgálati adatok közé tartoznak ajánlatok listája Példa a használatra, lásd: [ShoppingSources példa](./bing-insights-usage.md#shoppingsources-insight-example).

```json
      {
        "_type" : "ImageShoppingSourcesAction",
        "actionType" : "ShoppingSources",
        "data" : {
          "offers" : [
            {
              "name" : "Apple Pie",
              "url" : "https:\/\/contoso.com\/product_p\/l10.htm",
              "description" : "A taste of the crust, apple, and pie filling...",
              "seller" : {
                "name" : "Contoso"
              },
              "price" : 3.99,
              "priceCurrency" : "USD",
              "aggregateRating" : {
                "ratingValue" : 5
              },
              "lastUpdated" : "2018-04-16T00:00:00.0000000"
            }
          ]
        }
      }
```


## <a name="moresizes-insight"></a>MoreSizes felmérése

A MoreSizes insight – a Bing az interneten található kép méretét (kisebb vagy nagyobb) száma (lásd a `availableSizesCount` mező).

```json
      {
        "image" : {
          "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detai...",
          "name" : "Making Apple Pie",
          "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP....",
          "datePublished" : "2013-06-21T12:00:00.0000000Z",
          "contentUrl" : "http:\/\/contoso.com\/content\/uploads\/2013\/06\/apple-pie.jpg",
          "hostPageUrl" : "http:\/\/contoso.com\/2013\/06\/21\/making-apple-pie\/",
          "contentSize" : "134847 B",
          "encodingFormat" : "jpeg",
          "hostPageDisplayUrl" : "contoso.com\/2013\/06\/21\/making-apple-pie",
          "width" : 1050,
          "height" : 765,
          "thumbnail" : {
            "width" : 474,
            "height" : 345
          },
          "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CF...",
          "insightsMetadata" : {
            "recipeSourcesCount" : 6,
            "pagesIncludingCount" : 103,
            "availableSizesCount" : 28
          },
          "imageId" : "D12339146CFEDF3D409CC7A66D2C98D0D71904D4",
          "accentColor" : "3A0B01"
        },
        "actionType" : "MoreSizes"
      },
```

## <a name="visualsearch-insight"></a>VisualSearch felmérése

A VisualSearch insight tartalmaz olyan lemezképkészlet, amellyel vizuálisan hasonlóak az eredeti kép (tartalmazza a tartalmat az eredeti ábrának hasonló). Példa a használatra, lásd: [VisualSearch példa](./bing-insights-usage.md#visualsearch-insight-example).

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "VisualSearch",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
              "name" : "An apple pie...",
              "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.z...",
              "datePublished" : "2017-03-18T00:17:00.0000000Z",
              "contentUrl" : "http:\/\/contoso.net\/images\/8\/8a\/an_apple_pie.png",
              "hostPageUrl" : "http:\/\/contoso.com\/wiki\/an_apple_pie.png",
              "contentSize" : "87930 B",
              "encodingFormat" : "png",
              "hostPageDisplayUrl" : "contoso.com\/wiki\/an_apple_pie.png",
              "width" : 263,
              "height" : 192,
              "thumbnail" : {
                "width" : 474,
                "height" : 346
              },
              "imageInsightsToken" : "ccid_zhRxfGkI*mid_1DCBA7AA6D231...",
              "insightsMetadata" : {
                "recipeSourcesCount" : 6,
                "pagesIncludingCount" : 103,
                "availableSizesCount" : 28
              },
              "imageId" : "1DCBA7AA6D23147F9DD06D47DB3A38EB25389",
              "accentColor" : "3E0D01"
            }
          ]
        }
      }
```

## <a name="recipes-insight"></a>Receptet felmérése

A receptet insight, amelyek közé tartozik egy receptet, hogy az ábrán látható a étele listáját tartalmazza. Példa a használatra, lásd: [receptet példa](./bing-insights-usage.md#recipes-insight-example).

```json
      {
        "_type" : "ImageRecipesAction",
        "actionType" : "Recipes",
        "data" : {
          "value" : [
            {
              "name" : "Granny's Apple Pie",
              "url" : "http:\/\/contoso.com\/recipes\/appetizer\/apple-pie.html",
              "description" : "I love Granny's apple pie. Sooooo delicious...",
              "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=A63002cd9",
              "creator" : {
                "_type" : "Person",
                "name" : "Charlene Whitney"
              },
              "aggregateRating" : {
                "text" : "5",
                "ratingValue" : 5,
                "bestRating" : 5,
                "reviewCount" : 1
              },
              "cookTime" : "PT45M",
              "prepTime" : "PT1H",
              "totalTime" : "PT1H45M"
            }
          ]
        }
      }
```


## <a name="imagebyid-insight"></a>ImageById felmérése

A ImageById betekintést nyújt egy `Image` a kép insights a kért objektum.

```json
      {
        "image" : {
          "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=deta...",
          "name" : "Making Apple Pie",
          "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP...",
          "datePublished" : "2013-06-21T12:00:00.0000000Z",
          "contentUrl" : "http:\/\/contoso.com\/content\/uploads\/2013\/06\/apple-pie.jpg",
          "hostPageUrl" : "http:\/\/contoso.com\/2013\/06\/21\/making-apple-pie\/",
          "contentSize" : "134847 B",
          "encodingFormat" : "jpeg",
          "hostPageDisplayUrl" : "contoso.com\/2013\/06\/21\/making-apple-pie",
          "width" : 1050,
          "height" : 765,
          "thumbnail" : {
            "width" : 474,
            "height" : 345
          },
          "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFE...",
          "insightsMetadata" : {
            "recipeSourcesCount" : 6,
            "pagesIncludingCount" : 103,
            "availableSizesCount" : 28
          },
          "imageId" : "D12339146CFEDF3D409A66D2C98D0D71904D4",
          "accentColor" : "3A0B01"
        },
        "actionType" : "ImageById"
      },
```


## <a name="productvisualsearch-insight"></a>ProductVisualSearch felmérése

A ProductVisualSearch insight a lemezképek az eredeti ábrának termékek kijelölése hasonló termékek listáját tartalmazza. A `insightsMetadata` mező tartalmazhat ajánlatokat, ahol a termék- és a termék vásárolhat. 

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "ProductVisualSearch",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detail...",
              "name" : "Contoso 4-Piece Kitchen Package...",
              "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.l9hzaabu-RJd...",
              "datePublished" : "2017-07-16T04:28:00.0000000Z",
              "contentUrl" : "https:\/\/www.contoso.com\/assets\/media\/images\/prod...",
              "hostPageUrl" : "https:\/\/www.contoso.com\/4-piece-kitchen-package...",
              "contentSize" : "13594 B",
              "encodingFormat" : "jpeg",
              "hostPageDisplayUrl" : "https:\/\/www.contoso.com\/4-piece-kitchen-package...",
              "width" : 450,
              "height" : 332,
              "thumbnail" : {
                "width" : 474,
                "height" : 349
              },
              "imageInsightsToken" : "ccid_l9hzaabu*mid_70A8B616355D681DB9A5A...",
              "insightsMetadata" : {
                "shoppingSourcesCount" : 1,
                "recipeSourcesCount" : 0,
                "aggregateOffer" : {
                  "offers" : [
                    {
                      "name" : "4-Piece Kitchen Package with...",
                      "url" : "https:\/\/www.fabrikam.com\/1234.html?ref=bing",
                      "description" : "This 36 Frenchdoor refrigerator by...",
                      "seller" : {
                        "name" : "Fabrikam",
                        "image" : {
                          "url" : "https:\/\/tse1.mm.bing.net\/th?id=A818f811..."
                        }
                      },
                      "price" : 2756,
                      "priceCurrency" : "USD",
                      "availability" : "InStock",
                      "lastUpdated" : "2018-02-20T00:00:00.0000000"
                    }
                  ]
                },
                "pagesIncludingCount" : 4,
                "availableSizesCount" : 2
              },
              "imageId" : "70A8B616355D681DA5980A8D0514BCC995A3",
              "accentColor" : "60646B"
            }
          ]
        }
      }
```


## <a name="relatedsearches-insight"></a>RelatedSearches felmérése

A RelatedSearches insight kapcsolódó mások számára (a többi felhasználó keresési kifejezések alapján) végrehajtott keresések listáját tartalmazza. Példa a használatra, lásd: [RelatedSearches példa](./bing-insights-usage.md#relatedsearches-insight-example).


```json
      {
        "_type" : "ImageRelatedSearchesAction",
        "actionType" : "RelatedSearches",
        "data" : {
          "value" : [
            {
              "text" : "Homemade Apple Pies Recipes",
              "displayText" : "Homemade Apple Pies Recipes",
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Homemade...",
              "thumbnail" : {
                "url" : "https:\/\/tse1.mm.bing.net\/th?q=Homemade+Apple+Pies"
              }
            }
          ]
        }
      }
```


## <a name="documentlevelsuggestions-insight"></a>DocumentLevelSuggestions felmérése

A DocumentLevelSuggestions insight kép tartalma alapján javasolt kifejezéseket tartalmaz. 

```json
      {
        "_type" : "ImageRelatedSearchesAction",
        "actionType" : "DocumentLevelSuggestions",
        "data" : {
          "value" : [
            {
              "text" : "American Apple Pie",
              "displayText" : "American Apple Pie",
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=American",
              "thumbnail" : {
                "url" : "https:\/\/tse3.mm.bing.net\/th?q=American+Apple+Pie."
              }
            }
          ]
        }
      }
```



## <a name="next-steps"></a>További lépések

Tekintse meg a példák hogyan jelenjen meg a Bing előfordulhat, hogy a visual elemzéseket (lásd: [példák a Bing insights – használat](bing-insights-usage.md)).

Használatának gyors megkezdése az első kérelem, tekintse meg a quickstarts: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md).

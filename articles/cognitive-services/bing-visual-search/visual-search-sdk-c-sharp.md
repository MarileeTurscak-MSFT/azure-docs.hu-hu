---
title: Visual keresési SDK C# gyors üzembe helyezés |} Microsoft Docs
description: A telepítő SDK C#-konzolalkalmazást a Visual kereséshez.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 05/16/2018
ms.author: v-gedod
ms.openlocfilehash: 65102862caf3ff2af0d4d7bfdb26d720d17c50ea
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37018842"
---
# <a name="visual-search-sdk-c-quickstart"></a>Visual keresési SDK C# gyors üzembe helyezés

A Bing Visual keresési SDK használja, a REST API a webes kérelmek és elemzési eredmények.
A [forráskód C# Bing Visual keresési SDK minták](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVisualSearch) érhető el a Git központ.

Kód forgatókönyvek szerepelnek, az alábbi kategóriákban:
* [Visual keresési ügyfél](#client)
* [Teljes Konzolalkalmazás](#complete)
* [Kép bináris post rendelkező cropArea](#binary-crop)
* [KnowledgeRequest paraméter](#knowledge-req)
* [Címkék, műveletek és művelettípus](#tags-actions)
* [Címkék, műveletek és első művelettípus száma száma](#num-tags-actions)

## <a name="prerequisites"></a>Előfeltételek
* Visual Studio 2017. Ha szükséges, hogy ingyenes közösségi verziója letölthető innen: https://www.visualstudio.com/vs/community/.
* SDK-hívások hitelesítése egy kognitív szolgáltatások API-kulcs szükséges. Regisztráljon egy [ingyenes próba kulcs](https://azure.microsoft.com/try/cognitive-services/?api=search-api-v7). A Próbakulcs jó másodpercenként egy hívás a hét napja. Éles telepítési forgatókönyvhöz [hozzáférési kulcs vásárlása](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7). Lásd még: [árakról](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/visual/).
* A .NET core SDK, .net core 1.1-es alkalmazások futási képességét. Itt kaphat mag, a keretrendszer és a futtatókörnyezet: https://www.microsoft.com/net/download/.

## <a name="application-dependencies"></a>Alkalmazásfüggőségek

Állítsa be a Konzolalkalmazás, a Bing webes keresés SDK használatával, keresse meg a `Manage NuGet Packages` lehetőséget a Visual Studio megoldáskezelőjében.  Adja hozzá a `Microsoft.Azure.CognitiveServices.Search.VisualSearch` csomag.

Telepíti a [NuGet webes keresés SDK csomag](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VisualSearch/1.0) is telepíti a függőségek, beleértve:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

<a name="client"></a> 
## <a name="visual-search-client"></a>Visual keresési ügyfél
Egy példányának létrehozása a `VisualSearchAPI` ügyfelet, adja hozzá az irányelvek segítségével:
```
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;

```
Az ügyfél ezután példányosítható:
```
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
Az ügyfél képek kereséséhez használni:
```
 System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open;
 // The knowledgeRequest parameter is not required if an image binary is passed in the request body
 var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;

```
Az előző lekérdezés az eredményeket elemezni:
```
    // Visual Search results
    if (visualSearchResults.Image?.ImageInsightsToken != null)
    {
        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
    }
    else
    {
        Console.WriteLine("Couldn't find image insights token!");
    }

    // List of tags
    if (visualSearchResults.Tags.Count > 0)
    {
        var firstTagResult = visualSearchResults.Tags.First();
        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

        // List of actions in first tag
        if (firstTagResult.Actions.Count > 0)
        {
            var firstActionResult = firstTagResult.Actions.First();
            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
        }
        else
        {
            Console.WriteLine("Couldn't find tag actions!");
        }

```

<a name="complete"></a> 
## <a name="complete-console-application"></a>Teljes Konzolalkalmazás

A következő Konzolalkalmazás végrehajtja a korábban meghatározott lekérdezést, és elemzi az eredményeket:
```
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using System;
using System.IO;
using System.Linq;

namespace VisualSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            String subKey = "YOUR-SUBSCRIPTION-KEY";

            VisualSearchImageBinary(subKey);
            //VisualSearchImageBinaryWithCropArea(subKey);
            //VisualSearchUrlWithFilters(subKey);
            //VisualSearchInsightsTokenWithCropArea(subKey);
            //VisualSearchUrlWithJson(subKey);

            Console.WriteLine("\r\nAny key to quit...");
            Console.ReadKey();
        }


        // This will send an image binary in the body of the post request and print out the imageInsightsToken, 
        //  the number of tags, the number of actions, and the first actionType.

        public static void VisualSearchImageBinary(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                using (System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
                {
                    // The knowledgeRequest parameter is not required if an image binary is passed in the request body
                    var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
                    Console.WriteLine("\r\nVisual search request with binary of image");

                    if (visualSearchResults == null)
                    {
                        Console.WriteLine("No visual search result data.");
                    }
                    else
                    {
                        // Visual Search results
                        if (visualSearchResults.Image?.ImageInsightsToken != null)
                        {
                            Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image insights token!");
                        }

                        // List of tags
                        if (visualSearchResults.Tags.Count > 0)
                        {
                            var firstTagResult = visualSearchResults.Tags.First();
                            Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                            // List of actions in first tag
                            if (firstTagResult.Actions.Count > 0)
                            {
                                var firstActionResult = firstTagResult.Actions.First();
                                Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                                Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tag actions!");
                            }

                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image tags!");
                        }
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}

```

Az SDK különböző funkciókat fogunk bemutatni, a Bing keresési minták.  A következő funkciók hozzáadása a korábban meghatározott `VisualSrchSDK` osztály.

<a name="binary-crop"></a>
## <a name="image-binary-post-with-croparea"></a>Kép bináris post rendelkező cropArea

Az alábbi kód elküldi a post-kérelmet, és egy cropArea objektum törzsét bináris kép.  Ezután hogy kiírja a imageInsightsToken, a címkék száma, a műveletek száma és az első művelettípus.

```
        public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                using (FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
                {
                    // The ImageInfo struct contains a crop area specifying a region to crop in the uploaded image
                    CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
                    ImageInfo ImageInfo = new ImageInfo(cropArea: CropArea);
                    VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);

                    // The knowledgeRequest here holds additional information about the image, which is passed in in binary form
                    var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: VisualSearchRequest).Result;
                    Console.WriteLine("\r\nVisual search request with binary of image with knowledgeRequest of CropArea");

                    if (visualSearchResults == null)
                    {
                        Console.WriteLine("No visual search result data.");
                    }
                    else
                    {
                        // Visual Search results
                        if (visualSearchResults.Image?.ImageInsightsToken != null)
                        {
                            Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image insights token!");
                        }

                        // List of tags
                        if (visualSearchResults.Tags.Count > 0)
                        {
                            var firstTagResult = visualSearchResults.Tags.First();
                            Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                            // List of actions in first tag
                            if (firstTagResult.Actions.Count > 0)
                            {
                                var firstActionResult = firstTagResult.Actions.First();
                                Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                                Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tag actions!");
                            }

                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image tags!");
                        }
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
<a name="knowledge-req"></a>
## <a name="knowledgerequest-parameter"></a>KnowledgeRequest paraméter

Az alábbi kód elküldi egy kép URL-címe a `knowledgeRequest` paraméter, valamint egy \"webhely: www.bing.com\" szűrő.  Ezt követően, hogy kiírja a `imageInsightsToken`, a címkék száma, a műveletek száma és az első művelettípus.
```
        public static void VisualSearchUrlWithFilters(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via URL, in the ImageInfo object
                var ImageUrl = "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80";
                ImageInfo ImageInfo = new ImageInfo(url: ImageUrl);

                // Optional filters inside the knowledgeRequest will restrict similar products and images to certain domains
                Filters Filters = new Filters(site: "www.bing.com");
                KnowledgeRequest KnowledgeRequest = new KnowledgeRequest(filters: Filters);

                // An image binary is not necessary here, as the image is specified via URL
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo, knowledgeRequest: KnowledgeRequest);
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with url of image and knowledgeRequest based on filters");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
                        }

                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
<a name="tags-actions"></a>
## <a name="tags-actions-and-actiontype"></a>Címkék, műveletek és művelettípus

A következő kód egy kép insights jogkivonatot a knowledgeRequest paraméter együtt egy cropArea objektum küldi el.  Ezután hogy kiírja a imageInsightsToken, a címkék száma, a műveletek száma és az első művelettípus.

```
        public static void VisualSearchInsightsTokenWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via an insights token, in the ImageInfo object
                var ImageInsightsToken = "bcid_113F29C079F18F385732D8046EC80145*ccid_oV/QcH95*mid_687689FAFA449B35BC11A1AE6CEAB6F9A9B53708*thid_R.113F29C079F18F385732D8046EC80145";

                // An optional crop area can be passed in to define a region of interest in the image
                CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
                ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: ImageInsightsToken, cropArea: CropArea);

                // An image binary is not necessary here, as the image is specified via insights token
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with ImageInsightsToken and knowledgeRequest based on imageInfo of cropArea");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
                        }

                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
```
<a name="num-tags-actions"></a>
## <a name="number-of-tags-number-of-actions-and-first-actiontype"></a>Címkék, műveletek és első művelettípus száma száma
A következő kódot küld egy kép URL-címét a knowledgeRequest paraméterrel együtt egy vágási területen.  A imageInsightsToken, a címkék száma, a műveletek száma és az első művelettípus nyomtatását.
```
        public static void VisualSearchUrlWithJson(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {

                //The visual search request can be passed in as a JSON string
                //The image is specified via URL in the ImageInfo object, along with a crop area as shown below:
                
                //     "imageInfo": {
                //         "url": "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80",
                //     "cropArea": {
                //           "top": 0.1,
                //           "bottom": 0.5,
                //           "left": 0.1,
                //           "right": 0.9
                //         }
                //     },
                //     "knowledgeRequest": {
                //        "filters": {
                //            "site": "www.bing.com"
                //        }              
                //     }

                var VisualSearchRequestJSON = "{\"imageInfo\":{\"url\":\"https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80\",\"cropArea\":{\"top\":0.1,\"bottom\":0.5,\"left\":0.1,\"right\":0.9}},\"knowledgeRequest\":{\"filters\":{\"site\":\"www.bing.com\"}}}";

                // An image binary is not necessary here, as the image is specified by URL in JSON text
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequestJSON).Result;
                Console.WriteLine("\r\nVisual search request with image url specified by JSON text");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
                        }

                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
```

## <a name="next-steps"></a>További lépések

[Kognitív Services .NET SDK-minták](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7).
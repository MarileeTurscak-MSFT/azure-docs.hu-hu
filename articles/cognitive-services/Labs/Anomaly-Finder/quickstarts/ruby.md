---
title: Az Anomáliadetektálási kereső API használata a Ruby - a Microsoft Cognitive Services |} A Microsoft Docs
description: Get information és kód minták segítségével gyorsan használatának első lépései a Ruby és Anomáliadetektálási kereső API-val a Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 6eb559f8971583afe9619fb41fe331bd3013bb69
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/15/2018
ms.locfileid: "41987537"
---
# <a name="use-the-anomaly-finder-api-with-ruby"></a>A Anomáliaészlelő API-t használja a Ruby használatával

Ez a cikk bemutatja, és kódminták segítségével gyorsan az Anomáliadetektálási kereső API-használatának Ruby a feladatnak az anomáliadetektálási az észlelés eredménye az idősorozat-adatok beolvasása.

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-ruby"></a>Anomáliadetektálási pontok lekérdezése a Ruby használatával Anomáliadetektálási kereső API-val 
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Idősorozat-adatok – példa
Az adatsorozat adatpontjainak idő formátuma a következő,

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-ruby-example"></a>Adatok elemzése és anomáliadetektálási pontot kap Ruby példa

A példa lépései a következők.

1. Telepítés [rest-ügyfél](https://github.com/rest-client/rest-client) 'gem-telepítés rest-ügyfél' futtatásával.
2. Mentés alábbi kód .rb-fájlként.
3. Cserélje le a `[YOUR_SUBSCRIPTION_KEY]` értéke az érvényes előfizetési kulccsal végzett.
4. Cserélje le a `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` a példa vagy a saját adatpont.
5. Hajtsa végre, és ellenőrizze a választ.

```ruby
# https://github.com/rest-client/rest-client
require 'rest_client'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = '[YOUR_SUBSCRIPTION_KEY]';

endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

# Replace the request data with your real data.
requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

header = {
    :content_type => 'application/json',
    :'Ocp-Apim-Subscription-Key' => subscription_key
}

response = RestClient::Request.execute(
    :url => endpoint,
    :method => :post,
    :verify_ssl => true,
    :payload => requestData,
    :header => header)

# You will see the response with anomaly results
puts response.body
```

### <a name="example-response"></a>Példaválasz

A sikeres válasz JSON-fájlban. Mintaválasz a következőképpen történik.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [REST API – referencia](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)

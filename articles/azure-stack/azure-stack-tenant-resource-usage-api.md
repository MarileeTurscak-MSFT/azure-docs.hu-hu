---
title: Bérlői erőforrás-használati API |} A Microsoft Docs
description: Hivatkozás az erőforrás-használati API, amely az Azure Stack használati információk lekéréséhez.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: ab5dad550e590cd70f54ad5c8d4727d0f6370190
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379712"
---
# <a name="tenant-resource-usage-api"></a>Bérlői erőforrás-használati API

A bérlőt a bérlői API használatával a bérlő saját erőforrás-használati adatok megtekintéséhez. (Jelenleg a privát előzetes verzió) az Azure-használati API összhangban az API-t.

A Windows PowerShell-parancsmagot is használhatja **Get-UsageAggregates** beolvasni a használati adatok, például az Azure-ban.

## <a name="api-call"></a>API-hívás
### <a name="request"></a>Kérés
A kérést a kért előfizetéseket és a kért időkeretet felhasználási részletek beolvasása. Nincs nincs a kérelem törzsében.

| **Metódus** | **Kérés URI-ja** |
| --- | --- |
| GET |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumentumok
| **Argumentum** | **Leírás** |
| --- | --- |
| *armendpoint* |Az Azure Stack-környezet az Azure Resource Manager végpontját. Az Azure Stack egyezmény, hogy az Azure Resource Manager-végpont neve szerepel-e a formátum `https://management.{domain-name}`. Ha például a development Kit tartománynév local.azurestack.external, akkor a Resource Manager-végpont `https://management.local.azurestack.external`. |
| *subId* |A felhasználó, aki a hívás előfizetés-azonosítója. Az API csak le kell kérdeznie egy egyetlen előfizetés használata is használhat. Szolgáltatók összes bérlőre vonatkozóan a szolgáltató Erőforrás kihasználtsága API-t – lekérdezések használata is használhatja. |
| *reportedStartTime* |A lekérdezés kezdete. Az érték *DateTime* kell lennie, és például 13:00 óra kezdetén (UTC). A napi aggregációs éjfélkor (UTC) értékre állítja. A formátum *escape-karakterrel* ISO 8601, például 2015-06-16T18 % 3a53 % 3a11 % 2b00 % 3a00Z, ahol kettőspont escape-karakterrel megjelölve a(z) % 3a, és plusz escape-karakterrel megjelölve, % 2/b. úgy, hogy az URI rövid. |
| *reportedEndTime* |A lekérdezés vége. A korlátozásokat, amelyek a alkalmazni *reportedStartTime* ezt az argumentumot is vonatkoznak. Az érték *reportedEndTime* a jövőben nem lehet. |
| *aggregationGranularity* |Nem kötelező paraméter, amely két különálló lehetséges értéke van: napi vagy óránkénti. Javasolt értékek, mint egy napi részletességgel az adatot adja vissza, a másik pedig egy óránkénti megoldás. A napi beállítás az alapértelmezett érték. |
| *api-version* |Ez a kérés használt protokoll verziója. 2015-06-01-preview kell használnia. |
| *continuationToken* |Token lekért legutóbbi hívásának a Usage API-t szolgáltató. Ez a token van szükség, ha választ értéke nagyobb, mint 1000 sort, és úgy működik, mint egy könyvjelzőt a folyamat előrehaladását. Ha nem található, az adatok lekérésének forrása a nap kezdete vagy órát, a részletesség átadott. |

### <a name="response"></a>Válasz
GET /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00&reportedEndTime=2015-06-01T00%3a00%3a00%2b00%3a00&aggregationGranularity=Daily&api-version=1.0

```json
{
"value": [
{

"id":
"/subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",
"name": "sub1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

…
```

### <a name="response-details"></a>Válasz részletei
| **Argumentum** | **Leírás** |
| --- | --- |
| *id* |A használati összesítés egyedi azonosítója |
| *name* |A használati összesítés neve |
| *type* |Erőforrás-definíció |
| *subscriptionId* |Az Azure-felhasználó az előfizetés-azonosító |
| *usageStartTime* |A használati gyűjtőhöz, amelyhez a használat összesítés tartozik, kezdete (UTC) |
| *usageEndTime* |A használati gyűjtőhöz, amelyhez a használat összesítés tartozik befejezési időpontja (UTC) |
| *instanceData* |Példány adatai (a új formátum) kulcs-érték párt:<br>  *resourceUri*: teljesen minősített erőforrás-azonosító, beleértve az erőforráscsoportok és a példány neve <br>  *hely*: a régió, amelyben ez a szolgáltatás volt futtatva <br>  *a címkék*: a felhasználó által az erőforráscímkék <br>  *További információ*: operációs rendszer verziója vagy a kép típusa a felhasznált, például az erőforrással kapcsolatos további részletek |
| *Mennyiség* |Ezen az időn a következő erőforrás-használat mennyisége |
| *MeterId* |A felhasznált erőforrás egyedi azonosítója (más néven *ResourceID*) |


## <a name="next-steps"></a>További lépések
[Szolgáltatói erőforrás-használati API](azure-stack-provider-resource-api.md)

[Használattal kapcsolatos gyakori kérdések](azure-stack-usage-related-faq.md)


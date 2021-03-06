---
title: Az Azure Container Registry webhookok adatbázisséma hivatkozása
description: Webhook kérés JSON hasznos referencia az Azure Container Registry.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: marsma
ms.openlocfilehash: 87fe978416c29b50abeef0e0a6624d7440dd87ef
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096893"
---
# <a name="azure-container-registry-webhook-reference"></a>Az Azure Container Registry webhookok leírása

Is [webhookok konfigurálása](container-registry-webhook.md) a tároló-beállításjegyzékhez, amelyek létrehozzák az eseményeket, amikor bizonyos műveleteket alapján történik. Például engedélyezheti a webhookok, a tároló rendszerképét kiváltó `push` és `delete` műveleteket. Webhook akkor aktiválódik, amikor az Azure Container Registry bocsát ki, adja meg a végpont az eseménnyel kapcsolatos adatokat tartalmazó HTTP vagy HTTPS-kérést. A végpont ezután a webhook feldolgozni, és azoknak megfelelő cselekvést.

Az alábbi szakaszok a séma támogatott események által webhook-kérelmet. Az esemény szakaszok az esemény típusa, egy példa kérelem hasznos adatai, és aktiválja a webhookot egy vagy több Példaparancsok tartalom sémáját.

Az Azure container registry webhookok konfigurálásával kapcsolatos további információkért lásd: [használata Azure Container Registry webhookokat](container-registry-webhook.md).

## <a name="webhook-requests"></a>Webhook-kérelmek

### <a name="http-request"></a>HTTP-kérelem

Aktivált webhook lehetővé teszi egy olyan HTTP `POST` kérése az URL-végpontot, a webhook konfigurálásakor megadott.

### <a name="http-headers"></a>HTTP-fejlécek

Webhook kérések tartalmazzák egy `Content-Type` , `application/json` Ha nem adott meg egy `Content-Type` a webhook egyéni fejlécet.

A kérelem ezek egyéni fejlécek, előfordulhat, hogy a megadott a webhook mellett egyéb fejlécek kerülnek.

## <a name="push-event"></a>Leküldéses esemény

A Webhook aktiválódik, ha egy adattár át lett helyezve egy tárolórendszerképet.

### <a name="push-event-payload"></a>Leküldéses eseménytartalom

|Elem|Típus|Leírás|
|-------------|----------|-----------|
|`id`|Sztring|A webhook-esemény azonosítója.|
|`timestamp`|DateTime|Az idő, amikor a webhook-esemény lett elindítva.|
|`action`|Sztring|A művelet, amely kiváltotta a webhook-esemény.|
|[Cél](#target)|Komplex típus|A webhook-esemény kiváltó esemény célját.|
|[Kérelem](#request)|Komplex típus|A kérelem, ami a webhook-esemény jön létre.|

### <a name="target"></a>cél

|Elem|Típus|Leírás|
|------------------|----------|-----------|
|`mediaType`|Sztring|A hivatkozott objektum MIME-típusát.|
|`size`|Int32|A tartalom bájtok száma. Ugyanaz, mint a Hossz mezőben.|
|`digest`|Sztring|A tartalom, ahogyan a beállításjegyzék V2 HTTP API-specifikációnak a kivonat.|
|`length`|Int32|A tartalom bájtok száma. Ugyanaz, mint mérete mező.|
|`repository`|Sztring|A tárház nevét.|
|`tag`|Sztring|A kép címke neve.|

### <a name="request"></a>kérelem

|Elem|Típus|Leírás|
|------------------|----------|-----------|
|`id`|Sztring|Az esemény által kezdeményezett kérelem azonosítója.|
|`host`|Sztring|A kívülről elérhető-példányának gazdagépnevét a beállításjegyzék, a bejövő kérelem HTTP-állomásfejlécet által megadott.|
|`method`|Sztring|A kérelmi metódust, ami az esemény jön létre.|
|`useragent`|Sztring|A felhasználói ügynök fejléc a kérelem.|

### <a name="payload-example-push-event"></a>Adattartalom-példa: leküldéses esemény

```JSON
{
  "id": "cb8c3971-9adc-488b-bdd8-43cbb4974ff5",
  "timestamp": "2017-11-17T16:52:01.343145347Z",
  "action": "push",
  "target": {
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "size": 524,
    "digest": "sha256:80f0d5c8786bb9e621a45ece0db56d11cdc624ad20da9fe62e9d25490f331d7d",
    "length": 524,
    "repository": "hello-world",
    "tag": "v1"
  },
  "request": {
    "id": "3cbb6949-7549-4fa1-86cd-a6d5451dffc7",
    "host": "myregistry.azurecr.io",
    "method": "PUT",
    "useragent": "docker/17.09.0-ce go/go1.8.3 git-commit/afdb6d4 kernel/4.10.0-27-generic os/linux arch/amd64 UpstreamClient(Docker-Client/17.09.0-ce \\(linux\\))"
  }
}
```

Példa [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) parancsot, amely elindítja a **leküldéses** event webhook:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="delete-event"></a>Esemény törlése

A Webhook aktiválódik, ha a tárház vagy az jegyzékfájl törlődik. Nem aktiválódik, ha egy címke törlése.

### <a name="delete-event-payload"></a>Eseménytartalom törlése

|Elem|Típus|Leírás|
|-------------|----------|-----------|
|`id`|Sztring|A webhook-esemény azonosítója.|
|`timestamp`|DateTime|Az idő, amikor a webhook-esemény lett elindítva.|
|`action`|Sztring|A művelet, amely kiváltotta a webhook-esemény.|
|[Cél](#delete_target)|Komplex típus|A webhook-esemény kiváltó esemény célját.|
|[Kérelem](#delete_request)|Komplex típus|A kérelem, ami a webhook-esemény jön létre.|

### <a name="delete_target"></a> Cél

|Elem|Típus|Leírás|
|------------------|----------|-----------|
|`mediaType`|Sztring|A hivatkozott objektum MIME-típusát.|
|`digest`|Sztring|A tartalom, ahogyan a beállításjegyzék V2 HTTP API-specifikációnak a kivonat.|
|`repository`|Sztring|A tárház nevét.|

### <a name="delete_request"></a> Kérelem

|Elem|Típus|Leírás|
|------------------|----------|-----------|
|`id`|Sztring|Az esemény által kezdeményezett kérelem azonosítója.|
|`host`|Sztring|A kívülről elérhető-példányának gazdagépnevét a beállításjegyzék, a bejövő kérelem HTTP-állomásfejlécet által megadott.|
|`method`|Sztring|A kérelmi metódust, ami az esemény jön létre.|
|`useragent`|Sztring|A felhasználói ügynök fejléc a kérelem.|

### <a name="payload-example-delete-event"></a>Adattartalom-példa: esemény törlése

```JSON
{
    "id": "afc359ce-df7f-4e32-bdde-1ff8aa80927b",
    "timestamp": "2017-11-17T16:54:53.657764628Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:80f0d5c8786bb9e621a45ece0db56d11cdc624ad20da9fe62e9d25490f331d7d",
      "repository": "hello-world"
    },
    "request": {
      "id": "3d78b540-ab61-4f75-807f-7ca9ecf559b3",
      "host": "myregistry.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  }
```

Példa [Azure CLI-vel](/cli/azure/acr) parancsok, az eseményindító egy **törlése** event webhook:

```azurecli
# Delete repository
az acr repository delete -n MyRegistry --repository MyRepository

# Delete manifest
az acr repository delete -n MyRegistry --repository MyRepository --tag MyTag --manifest
```

## <a name="next-steps"></a>További lépések

[Azure Container Registry webhookok használata](container-registry-webhook.md)
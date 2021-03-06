---
title: Az Azure Functions – HTTP-eseményindítók és kötések
description: Megtudhatja, hogyan használja a HTTP-eseményindítók és kötések az Azure Functions szolgáltatásban.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: az Azure functions, függvények, esemény feldolgozása, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra, HTTP-n API-t, REST
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: e989152ece19168138597a96d1246ec64498ce69
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227554"
---
# <a name="azure-functions-http-triggers-and-bindings"></a>Az Azure Functions – HTTP-eseményindítók és kötések

Ez a cikk ismerteti a HTTP-eseményindítók és a kimeneti kötések az Azure Functions használata.

HTTP-trigger válaszolni a testre szabható [webhookok](https://en.wikipedia.org/wiki/Webhook).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="packages---functions-1x"></a>Csomagok – 1.x függvények

A HTTP-kötések szerepelnek a [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet-csomag verziója 1.x. A csomag forráskódja a [azure-webjobs-sdk-bővítmények](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http) GitHub-adattárban.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Csomagok – 2.x függvények

A HTTP-kötések szerepelnek a [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet-csomag verziója 3.x. A csomag forráskódja a [azure-webjobs-sdk-bővítmények](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub-adattárban.

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Eseményindító

A HTTP-eseményindítóval lehetővé teszi a HTTP-kérést függvény hívása. HTTP-trigger használatával hozhat létre kiszolgáló nélküli API-kat és webhookokat válaszol. 

Alapértelmezés szerint HTTP-trigger adja vissza HTTP 200 OK az funkciók egy üres szövegtörzzsel 1.x vagy HTTP 204 Nincs tartalom az funkciók egy üres szövegtörzzsel 2.x. Módosítsa a válasz, állítson be egy [HTTP kimeneti kötésének](#http-output-binding).

## <a name="trigger---example"></a>Az eseményindító – példa

Tekintse meg az adott nyelvű példa:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

### <a name="trigger---c-example"></a>Eseményindító - C#-példa

A következő példa bemutatja egy [C#-függvény](functions-dotnet-class-library.md) keres, amely egy `name` paraméter a lekérdezési karakterlánc vagy a HTTP-kérelem törzse. Figyelje meg, hogy a visszaadott érték szolgál a kimeneti kötést, de a visszaadott érték attribútum nem szükséges.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequestMessage req, 
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

### <a name="trigger---c-script-example"></a>Eseményindító - C#-szkript példa

Az alábbi példa bemutatja a trigger kötés egy *function.json* fájl és a egy [C#-szkriptfüggvény](functions-reference-csharp.md) , amely a kötés használja. A függvény megkeresi egy `name` paraméter a lekérdezési karakterlánc vagy a HTTP-kérelem törzse.

Íme a *function.json* fájlt:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

A [konfigurációs](#trigger---configuration) szakasz mutatja be ezeket a tulajdonságokat.

Íme a C#-szkriptkódot kötődő `HttpRequestMessage`:

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

Helyett egyéni objektumot kell kötni `HttpRequestMessage`. Ez az objektum létrejön a JSON-ként értelmezni a kérelem törzséből. Hasonlóképpen egy kimeneti kötést, és a válasz törzse együtt 200 állapotkódot adja vissza a HTTP-válasz adható át.

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
```

### <a name="trigger---f-example"></a>Eseményindító - F #-példa

Az alábbi példa bemutatja a trigger kötés egy *function.json* fájl és a egy [F #-függvény](functions-reference-fsharp.md) , amely a kötés használja. A függvény megkeresi egy `name` paraméter a lekérdezési karakterlánc vagy a HTTP-kérelem törzse.

Íme a *function.json* fájlt:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

A [konfigurációs](#trigger---configuration) szakasz mutatja be ezeket a tulajdonságokat.

Az F #-kód itt látható:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Kell egy `project.json` fájlt, amely NuGet használatával hivatkozhat a `FSharp.Interop.Dynamic` és `Dynamitey` szerelvényeket, az alábbi példában látható módon:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

### <a name="trigger---javascript-example"></a>Eseményindító - JavaScript-példa

Az alábbi példa bemutatja a trigger kötés egy *function.json* fájl és a egy [JavaScript-függvény](functions-reference-node.md) , amely a kötés használja. A függvény megkeresi egy `name` paraméter a lekérdezési karakterlánc vagy a HTTP-kérelem törzse.

Íme a *function.json* fájlt:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

A [konfigurációs](#trigger---configuration) szakasz mutatja be ezeket a tulajdonságokat.

A következő JavaScript-kódot:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

### <a name="trigger---java-example"></a>Eseményindító - Java-példában

Az alábbi példa bemutatja a trigger kötés egy *function.json* fájl és a egy [Java függvény](functions-reference-java.md) , amely a kötés használja. A függvény HTTP-állapot 200-as kód válaszban előtagok a riasztást kiváltó kérés törzse egy "Helló," az üdvözlőszöveget kéréstörzs adja vissza.


Íme a *function.json* fájlt:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "anonymous",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

A Java-kód itt látható:

```java
@FunctionName("hello")
public HttpResponseMessage<String> hello(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS), Optional<String> request,
                        final ExecutionContext context) 
    {
        // default HTTP 200 response code
        return String.format("Hello, %s!", request);
    }
}
```

## <a name="trigger---attributes"></a>Eseményindító - attribútumok

A [C#-osztálykódtárakat](functions-dotnet-class-library.md), használja a [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) attribútum.

Beállíthatja az engedélyezési engedélyezett és a egy HTTP-metódusok attribútum konstruktor paramétereket, és a webhook típusa és útvonal-sablon tulajdonságait. Ezekkel a beállításokkal kapcsolatos további információkért lásd: [eseményindító - konfiguráció](#trigger---configuration). Íme egy `HttpTrigger` attribútumot a podpis metody:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous)] HttpRequestMessage req)
{
    ...
}
 ```

Egy teljes példa: [eseményindító – C#-példa](#trigger---c-example).

## <a name="trigger---configuration"></a>Eseményindító - konfiguráció

A következő táblázat ismerteti a megadott kötés konfigurációs tulajdonságaiban a *function.json* fájlt, és a `HttpTrigger` attribútum.

|Function.JSON tulajdonság | Attribútum tulajdonsága |Leírás|
|---------|---------|----------------------|
| **type** | n/a| Kötelező – kell állítani `httpTrigger`. |
| **direction** | n/a| Kötelező – kell állítani `in`. |
| **name** | n/a| Kötelező – a a függvény kódját a kérelem vagy a kérelem törzsében használt változó neve. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Meghatározza, hogy milyen kulcsok, az esetleges kell jelen lennie ahhoz, hogy a függvény hívása a kérésre. A jogosultsági szinteket a következő értékek egyike lehet: <ul><li><code>anonymous</code>&mdash;Egyetlen API-kulcs nem szükséges.</li><li><code>function</code>&mdash;Egy adott API-kulcs megadása kötelező. Ez az az alapértelmezett érték, ha egyiket sem.</li><li><code>admin</code>&mdash;A fő kulcsot kötelező megadni.</li></ul> További információkért lásd a szakasz [engedélyezési kulcsok](#authorization-keys). |
| **Módszerek** |**Módszerek** | A HTTP-metódusok, amelyre a függvény válasza tömbje. Ha nincs megadva, a függvény az összes HTTP-metódusok válaszol. Lásd: [testre szabhatja a http-végpontot](#customize-the-http-endpoint). |
| **útvonal** | **útvonal** | Meghatározza az útvonalsablonhoz, szabályozásával, amelyhez a kérés URL-címeket, a függvény válasza. Az alapértelmezett érték, ha egyiket sem `<functionname>`. További információkért lásd: [testre szabhatja a http-végpontot](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** | _Csak az a verzió 1.x futásidejű támogatott._<br/><br/>A HTTP-eseményindítóval, hogy működjön, konfigurálja a [webhook](https://en.wikipedia.org/wiki/Webhook) fogadót a megadott szolgáltatón. Nincs beállítva a `methods` tulajdonságot, ha ezzel a tulajdonsággal. A webhook típusa a következő értékek egyike lehet:<ul><li><code>genericJson</code>&mdash;Egy általános célú webhook-végpontot egy szolgáltató logika nélkül. Ez a beállítás korlátozza a kérelmek Ha csak a HTTP-n keresztül, közzététel és az a `application/json` tartalom típusa.</li><li><code>github</code>&mdash;A függvény válaszol [GitHub-webhookok](https://developer.github.com/webhooks/). Ne használja a _authLevel_ tulajdonság GitHub-webhookok használatával. További információkért tekintse meg a GitHub-webhookok szakaszban Ez a cikk későbbi részében.</li><li><code>slack</code>&mdash;A függvény válaszol [webhookok Slack](https://api.slack.com/outgoing-webhooks). Ne használja a _authLevel_ tulajdonság Slack webhookok használatával. További információkért tekintse meg a Slack webhookok szakaszt, a cikk későbbi részében.</li></ul>|

## <a name="trigger---usage"></a>Eseményindító - használat

A C# és az F # függvény, a bemeneti adatokat lehet az eseményindító típusú deklarálhatnak `HttpRequestMessage` vagy egy egyéni típus. Ha úgy dönt, `HttpRequestMessage`, a kérelem objektum teljes hozzáférést kap. Egyéni írja be a következőt a modul megpróbálja elemezni az objektum tulajdonságainak JSON-kérelem törzse.

A JavaScript-függvények a Functions futtatókörnyezete biztosít, a kérelem törzsében a támogatásikérelem-objektum helyett. További információkért lásd: a [JavaScript eseményindító példa](#trigger---javascript-example).


### <a name="customize-the-http-endpoint"></a>A HTTP-végpontot testreszabása

Alapértelmezés szerint létrehozott egy függvényt a HTTP-trigger, a függvény a megcímezhető az útvonal a következő formában:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Ez az útvonal nem kötelező testre szabható `route` tulajdonsága a HTTP-trigger bemeneti kötést. Tegyük fel, a következő *function.json* fájl határozza meg a `route` HTTP-trigger tulajdonságát:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Használja ezt a konfigurációt, a függvény már címezhetővé válnak, a következő útvonal az eredeti útvonal helyett.

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

Ez lehetővé teszi a függvénykódot a címet, két paramétert támogató _kategória_ és _azonosító_. Bármilyen [webes API útvonal megkötés](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) a paraméterekkel. Az alábbi C#-függvénykódot mindkét paraméter használnak.

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

Íme a Node.js-függvény kódot, amely útvonal ugyanazokat a paramétereket használja.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Alapértelmezés szerint az összes funkció útvonal van fűzve előtagként *api*. Is testreszabhatja, vagy távolítsa el a előtagot használja a `http.routePrefix` tulajdonságot a [host.json](functions-host-json.md) fájlt. A következő példában eltávolítjuk a *api* útválasztási előtagot a előtag, az üres karakterlánc használatával a *host.json* fájlt.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="authorization-keys"></a>Hitelesítési kulcsok

Functions-kulcsok használata a HTTP-függvény végpontjainak eléréséhez a fejlesztés során nehezebb teszi lehetővé.  Egy normál HTTP-eseményindító ilyen egy API-kulcsot kell a kérelemben szereplő lehet szükség. 

> [!IMPORTANT]
> Kulcsok segítségére lehetnek a HTTP-végpontokat rejtse fejlesztése során, amíg azok nem tartozhat arra, hogy biztonságos HTTP-trigger, éles környezetben. További tudnivalókért lásd: [egy HTTP-végpontot, éles környezetben biztonságos](#secure-an-http-endpoint-in-production).

> [!NOTE]
> Az a funkciók 1.x-futtatókörnyezet a webhook-szolgáltatók a kulcsok használatával előfordulhat, hogy többféle módon, attól függően, a szolgáltató támogatja a kérelmek engedélyezését végzi. Ez foglalkozik [Webhookok és kulcsok](#webhooks-and-keys). A verzió 2.x verziójú futtatókörnyezet nem tartalmaz beépített támogatást nyújt a webhook-szolgáltatók.

Kulcsok két típusa van:

* **Gazdagép kulcsok**: belül a függvényalkalmazás a függvények által megosztott ezeket a kulcsokat. Ha egy API-kulcsot, ezek a függvényalkalmazás belül függvényeket elérését teszi lehetővé.
* **Funkcióbillentyűk**: csak a konkrét funkciók, amelyek szerint vannak definiálva a alkalmazni ezeket a kulcsokat. Ha egy API-kulcsot, ezek csak való hozzáférés engedélyezése, hogy a függvény.

Minden egyes kulcs neve referenciaként, és a függvény és a gazdagép szintjén van ("alapértelmezett" nevű) alapértelmezett kulcs. Funkcióbillentyűk elsőbbséget élveznek a gazdagép-kulcsokat. Két kulcs van megadva ugyanazzal a névvel, a függvény kulcsát mindig használja.

Minden függvényalkalmazáshoz is rendelkezik egy speciális **főkulcs**. Ezt a kulcsot nem nevű gazdagép kulcs `_master`, amely a futtatókörnyezeti API-k rendszergazdai hozzáférést biztosít. Ezt a kulcsot nem vonható vissza. Beállításakor egy engedélyezési szintű `admin`, kérelmek kell használnia a főkulcs; bármilyen más kulcs engedélyezési hiba eredményez.

> [!CAUTION]  
> Miatt az emelt szintű engedélyekkel a főkulcs által nyújtott a függvényalkalmazásban nem kell ezt a kulcsot megoszthatja harmadik féllel vagy osztja el a natív ügyfélalkalmazások számára. Körültekintően járjon el a rendszergazdai jogosultsági szint kiválasztásakor.

### <a name="obtaining-keys"></a>Kulcsok beszerzése

Kulcsok az Azure-ban a függvényalkalmazás részeként tárolja, és inaktív. A kulcsok megtekintéséhez, újakat hozhat létre, vagy a kulcsok állni az új értékek, keresse meg az egyik a HTTP-eseményindítóval aktivált függvény a a [az Azure portal](https://portal.azure.com) válassza **kezelés**.

![Funkcióbillentyűk a portálon kezelheti.](./media/functions-bindings-http-webhook/manage-function-keys.png)

Nem támogatott API-t olyan programozott módon a függvény kulcsok beszerzéséhez.

### <a name="api-key-authorization"></a>Hitelesítési API

A legtöbb HTTP-eseményindító sablonok a kérelem API-kulcs szükséges. Így a HTTP-kérés általában a következő URL-cím hasonlóan néz ki:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

A kulcs tartalmazhat egy lekérdezési karakterlánc változóban nevű `code`, a fentiek szerint. Azt is képezheti egy `x-functions-key` HTTP-fejléc. A kulcsnak az értéke lehet bármely függvénykulcs, a függvény definiálva, vagy bármely állomás kulcsát.

Névtelen kérések, amelyek nem igénylik a kulcsok engedélyezheti. A főkulcs használatát is megkövetelheti. Az alapértelmezett hitelesítési szint módosítása használatával a `authLevel` JSON kötésében tulajdonság. További információkért lásd: [eseményindító - konfiguráció](#trigger---configuration).

> [!NOTE]
> Ha helyileg futtatja a functions, engedélyezési le van tiltva, a megadott hitelesítési szint beállítástól függetlenül. Az Azure-bA közzétételt követően a `authLevel` az eseményindító a beállítás lép életbe.



### <a name="secure-an-http-endpoint-in-production"></a>Biztonságos egy HTTP-végpontot, éles környezetben

Teljes körűen biztonságossá tételéhez a függvény végpontok éles környezetben, érdemes megfontolni megvalósítását függvény alkalmazásszintű biztonság az alábbi lehetőségek közül:

* Kapcsolja be az App Service-hitelesítés / engedélyezés a függvényalkalmazás számára. Az App Service platform lehetővé teszi az Azure Active Directory (AAD) és több külső identitásszolgáltató használatával ügyfelek hitelesítéséhez. Ezzel a Functions egyéni engedélyezési szabályok megvalósításához, és használhatja a felhasználói adatokat a függvénykódban. További tudnivalókért lásd: [hitelesítése és engedélyezése Azure App Service-ben](../app-service/app-service-authentication-overview.md).

* Az Azure API Management (APIM) használatával-kérések hitelesítéséhez. APIM API biztonsági beállítások a bejövő kéréseket széles skáláját kínálja. További tudnivalókért lásd: [az API Management a hitelesítési házirendek](../api-management/api-management-authentication-policies.md). Az APIM-helyen konfigurálhatja a függvényalkalmazás csak az IP-címet az APIM-példány érkező kéréseket fogadják. További tudnivalókért lásd: [IP-címkorlátozások](ip-addresses.md#ip-address-restrictions).

* A függvényalkalmazás, egy Azure App Service Environment (ASE) üzembe helyezése. ASE a függvények futtatására dedikált üzemeltetési környezetet biztosít. ASE lehetővé teszi, hogy az összes bejövő kérések hitelesítéséhez használhatja egyetlen előtér-átjáró konfigurálását. További információkért lásd: [egy webalkalmazási tűzfal (WAF) konfigurálása App Service Environment-környezet](../app-service/environment/app-service-app-service-environment-web-application-firewall.md).

Ezek függvény alkalmazási szintű biztonsági módszer használata esetén állítsa be a HTTP-eseményindítóval aktivált függvényt hitelesítési szintet `anonymous`.

### <a name="webhooks"></a>Webhookok

> [!NOTE]
> Webhook mód csak verzió érhető el a Functions futtatókörnyezet 1.x.

Webhook mód a webhook hasznos adat található további ellenőrzést biztosít. A verzió 2.x, az alapszintű HTTP-eseményindító továbbra is működik, és a webhookok az ajánlott eljárás.

#### <a name="github-webhooks"></a>GitHub-webhookok

GitHub-webhookok válaszolni, először a függvény HTTP-Trigger létrehozása, és állítsa be a **webHookType** tulajdonságot `github`. Az URL-CÍMÉT és API-kulcsát, majd másolja a **webhook hozzáadása** a GitHub-adattár oldalát. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Példaként tekintse meg a [GitHub-webhookok által aktivált függvények létrehozását](functions-create-github-webhook-triggered-function.md).

#### <a name="slack-webhooks"></a>Slack-webhookok

A Slack webhook állít elő, helyett adja meg, így konfigurálnia kell egy adott kulcs a jogkivonatot a Slack, ami lehetővé teszi egy tokent az Ön számára. Lásd: [engedélyezési kulcsok](#authorization-keys).

### <a name="webhooks-and-keys"></a>Webhookok és kulcsok

Webhook engedélyezési kezelje a webhook fogadó összetevő, a HTTP-eseményindítóval része, és a mechanizmus a webhook típusa alapján változik. Minden egyes mechanizmus támaszkodik egy kulcsot. Alapértelmezés szerint az "alapértelmezett" nevű függvény kulcsot használja. Egy másik kulcsot használatához adja meg a webhook-szolgáltatót, hogy a kulcs nevét, a kérés küldése a következő módszerek valamelyikével:

* **Lekérdezési karakterlánc**: A szolgáltató adja át a kulcs nevét a `clientid` például a lekérdezési sztring paramétereként, `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
* **Kérelem fejléce**: A szolgáltató adja át a kulcs nevét a `x-functions-clientid` fejléc.

## <a name="trigger---limits"></a>Eseményindító - korlátok

A HTTP-kérelem hossza legfeljebb 100 MB-ra (104,857,600 bájt), és az URL-cím hossza legfeljebb 4 KB-os (4096 bájt). Ezek a korlátok határozza meg a `httpRuntime` elem a futtatókörnyezet [Web.config fájl](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

Ha egy függvényt, amely használja a HTTP-eseményindító nem befejezéséhez körülbelül 2,5 percen belül, az átjáró időtúllépés és HTTP 502-es hibát adhat vissza. A funkció továbbra is fut, de nem lehet egy HTTP-választ adja vissza. Hosszú lefutású funkciók azt javasoljuk, hogy hajtsa végre az aszinkron minták, és visszaadja azt a helyet, ahol megpingelheti a kérés állapotát. Mennyi ideig futhat egy függvény kapcsolatos információkért lásd: [méretezés és üzemeltetés – Használatalapú csomagban](functions-scale.md#consumption-plan). 

## <a name="trigger---hostjson-properties"></a>Eseményindító - host.json tulajdonságai

A [host.json](functions-host-json.md) fájl HTTP-eseményindító viselkedését vezérlő beállításokat tartalmaz.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Kimenet

A HTTP-kimenet válaszol a HTTP-kérést küldő kötés használja. A kötés HTTP-trigger igényel, és lehetővé teszi, hogy a válasz az eseményindító kéréshez társított testreszabása. Ha a kimeneti kötés egy HTTP nincs nincs megadva, a HTTP-trigger HTTP 200 OK az funkciók egy üres szövegtörzzsel adja vissza. 1.x vagy HTTP 204 Nincs tartalom az funkciók egy üres szövegtörzzsel 2.x.

## <a name="output---configuration"></a>Kimenete – konfiguráció

A következő táblázat ismerteti a megadott kötés konfigurációs tulajdonságaiban a *function.json* fájlt. A C#-osztálykódtárakat, ezeket nem attribútum tulajdonságok vannak *function.json* tulajdonságait. 

|Tulajdonság  |Leírás  |
|---------|---------|
| **type** |Meg kell `http`. |
| **direction** | Meg kell `out`. |
|**name** | A függvény kódját a a választ, a használt változó neve vagy `$return` a visszatérési érték használatát. |

## <a name="output---usage"></a>Kimenet – használat

HTTP-választ küldeni, a nyelv – standard válasz minták használatával. C# vagy C#-szkript, győződjön meg arról, a függvény visszatérési típusa `HttpResponseMessage` vagy `Task<HttpResponseMessage>`. A C# a visszatérési érték attribútum nem szükséges.

Például a válaszokat, tekintse meg a [eseményindító példa](#trigger---example).

## <a name="next-steps"></a>További lépések

[Tudjon meg többet az Azure functions eseményindítók és kötések](functions-triggers-bindings.md)

---
title: Az Azure API Management, az Event Hubs és a Runscope API-k figyelése |} A Microsoft Docs
description: Kapcsolódó Azure API Management, Azure Event Hubs és a Runscope HTTP-naplózás és figyelés a log-eventhub házirend bemutatására mintaalkalmazás
services: api-management
documentationcenter: ''
author: darrelmiller
manager: erikre
editor: ''
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2018
ms.author: apimpm
ms.openlocfilehash: 4c58be8f501e72027e1692ceb73552a3f252f92a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38603178"
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Az Azure API Management, az Event Hubs és a Runscope API-k monitorozása
A [API Management-szolgáltatás](api-management-key-concepts.md) javíthatják a HTTP API-nak küldött HTTP-kérések feldolgozását számos funkciót kínál. Létezik-e a kérelmek és válaszok azonban csak átmeneti. A kérelmet, és biztosítani az API Management szolgáltatás számára a háttérrendszeri API segítségével. Az API-t feldolgozza a kérést és választ keresztüláramló vissza az API fogyasztó számára. Az API Management szolgáltatás tartja néhány Megjelenítés az API-k leíró fontos statisztikákat az Azure portal irányítópultján, de után eltűnnek róla, hogy a részletek.

A napló eventhub házirend az API Management szolgáltatás használatával is elküldheti minden adatát a kérelem és válasz egy [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md). Nincsenek számos okból, miért érdemes eseményeket küld az API-k HTTP-üzenetek hoznak létre. Néhány példa: napló a frissítéseket, használatelemzési információkat, kivétel riasztási és külső Integrációk.   

Ez a cikk bemutatja, hogyan a teljes HTTP kérés- és üzenet rögzítése. küldje el az Eseményközpontok felé, majd továbbítja az, hogy egy külső szolgáltatás, amely HTTP-naplózás és -szolgáltatások figyelésének üzenetet.

## <a name="why-send-from-api-management-service"></a>Miért küldött az API Management szolgáltatás?
Akkor lehet írni, amely HTTP-kérések és válaszok rögzítése, és továbbíthatja azokat, naplózás és figyelés rendszerek HTTP API-keretrendszert csatlakoztatható HTTP közbenső szoftverek. Ennek a módszernek a hátránya a HTTP-közbenső integrálható a háttérrendszeri API kell, és meg kell egyeznie az API-platform. Ha több API-t, a pedig külön kell telepítenie a közbenső szoftver. Gyakran oka miért háttérrendszeri API-khoz nem lehet frissíteni.

Naplózás infrastruktúra integrálása az Azure API Management szolgáltatás használatával központosított és platformfüggetlen megoldást kínál. Akkor is méretezhető, részben oka az, hogy a [georeplikációs](api-management-howto-deploy-multi-region.md) Azure API Management képességeit.

## <a name="why-send-to-an-azure-event-hub"></a>Miért érdemes az Azure-Eseményközpontba küldeni?
Egy ésszerű kérje meg, miért érdemes létrehozni egy szabályzatot, amely kifejezetten az Azure Event Hubs? Nincsenek számos olyan helyet, ahol előfordulhat, hogy szeretnék saját kérések naplózása. Miért nem csupán a kérések küldéséhez közvetlenül a végső rendeltetési?  Ez a lehetőség. Azonban így az API management-szolgáltatás bejelentkezési kérelmek, esetén figyelembe kell venni, milyen hatással van az API teljesítményének naplózási üzeneteket. Növelje a rendszer összetevőinek rendelkezésre álló példányát vagy georeplikációs előnyeit kihasználva tudja kezelni a terhelés fokozatos növekedését. Azonban a rövid csúcsterhelésekkel később, ha a naplózás infrastruktúra kérések túllépik az terhelés alatt lassú kérelmek okozhat.

Az Azure Event Hubs célja, hogy hatalmas mennyiségű bejövő adat, az események sokkal nagyobb számú foglalkoznak, mint a HTTP-kérések száma a legtöbb API-k folyamat kapacitással. Az Event Hubs kifinomult között az API management-szolgáltatás és az infrastruktúra, amely tárolja és feldolgozza az üzeneteket egy közvetítő funkcionál. Ez biztosítja, hogy az API teljesítményének nem romlani fog a naplózás infrastruktúra miatt.  

Miután az adatokat egy eseményközpontba lett átadva, rendszer megőrzi, és várakozik az Eseményközpont fogyasztói feldolgozni azt. Az Event Hubs nem fontos a feldolgozásának módja, azt csak ügyel gondoskodik róla, hogy az üzenet sikeresen kézbesíteni.     

Az Event Hubs képes az események streamelése több felhasználói csoporthoz. Ez lehetővé teszi a különböző rendszerek által feldolgozandó események. Ez lehetővé teszi, hogy számos integrációs forgatókönyveket támogató nem helyezi emellett késések belül az API Management szolgáltatás API-kérelem feldolgozása csak egy esemény létrejön igényei szerint.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Alkalmazás/http-üzenetek küldése egy házirend
Egy Eseményközpont eseményadatok egyszerű karakterláncként fogad el. A karakterlánc tartalma szerint strukturálhatja. Az, hogy becsomagolhatja a HTTP-kérést és a mobilszolgáltatásokba az Event Hubs, formázó karakterlánc, a kérelem vagy válasz információkkal kell. Ehhez hasonló helyzeteket, ha egy meglévő formátum felhasználhatja azt, majd azt nem lehet írni a saját kód elemzése. Kezdetben szeretnék venni használatával a [HAR](http://www.softwareishard.com/blog/har-12-spec/) küld el a HTTP-kérelmeket és válaszokat. Azonban ez a formátum tárolására van optimalizálva sorozata, HTTP-kérések egy JSON-alapú formátumban. A forgatókönyvhöz a HTTP üzenet átadja a hálózaton keresztül, a szükségtelen összetettséget hozzáadott kötelező elemek számos tartalmazott.  

Egy másik lehetséges volt, hogy használja a `application/http` media írja be a HTTP-specifikációnak leírtak szerint [RFC 7230](http://tools.ietf.org/html/rfc7230). Ez az adathordozó típusát pontosan ugyanazt a formátumot használja, amely ténylegesen a hálózaton keresztül a HTTP-üzenetek küldésére szolgál, de a teljes üzenetet lehet helyezni egy másik HTTP-kérelem törzse. Ebben az esetben az imént fogjuk a szervezet használja az üzenet küldése az Event Hubsba. Kényelmesen, van egy elemző, amely létezik a [Microsoft ASP.NET webes API 2.2-es ügyfél](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) kódtárakat, amely ebben a formátumban értelmezni és átalakíthatja a natív `HttpRequestMessage` és `HttpResponseMessage` objektumokat.

Hogy ezt az üzenetet létrehozni, használja ki a C#-alapú kell [házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx) az Azure API Management szolgáltatásban. Íme a szabályzatot, amely HTTP-kérelem üzenetet küld az Azure Event Hubs.

```xml
<log-to-eventhub logger-id="conferencelogger" partition-id="0">
@{
   var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                               context.Request.Method,
                                               context.Request.Url.Path + context.Request.Url.QueryString);

   var body = context.Request.Body?.As<string>(true);
   if (body != null && body.Length > 1024)
   {
       body = body.Substring(0, 1024);
   }

   var headers = context.Request.Headers
                          .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                          .ToArray<string>();

   var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

   return "request:"   + context.Variables["message-id"] + "\n"
                       + requestLine + headerString + "\r\n" + body;
}
</log-to-eventhub>
```

### <a name="policy-declaration"></a>A házirend deklarace
Van néhány adott beállítást érdemes illesztőivel kapcsolatban a házirend-kifejezés. A napló eventhub házirend az API Management szolgáltatáson belül létrehozott naplózó nevére hivatkozik naplózó-azonosító néven attribútummal rendelkezik. A dokumentum részletesen ismerteti az API Management szolgáltatásban egy Eseményközpont naplózó beállításához található [eseménynaplózás az Azure Event Hubs az Azure API Management hogyan](api-management-howto-log-event-hubs.md). A második attribútumot egy nem kötelező paraméter, amely arra utasítja az Event hubs szolgáltatás, amely tárolja az üzenetet a partíció. Event Hubs engedélyezése a méretezhetőséget, és legalább két szükséges partíciók használja. Az üzenetek kézbesítését rendezett csak garantált egy partíción belül. A Microsoft arra utasítani az Event Hubs melyik partícióba helyezi el az üzenetet, ha egy Ciklikus időszeleteléses algoritmust használ a terhelés. Azonban, hogy okozhat a sorrendben feldolgozandó üzenetek egy része.  

### <a name="partitions"></a>Partíciók
Annak érdekében, az üzenetek közvetítését a rendelés kézbesítési, és kihasználhatja a terhelés elosztása funkció a partíciók, kiválasztott HTTP-kérési üzeneteket küldhet egy partíciót és a egy második partícióra HTTP-válaszüzenetek. Ez biztosítja, hogy egy még akkor is, terheléselosztást és garantáljuk, hogy összes kérelem sorrendben fognak használni, és minden válasz felhasznált sorrendben. Lehetséges, hogy a megfelelő kérést előtt fogják választ, de ez nem probléma, hogy egy másik mechanizmust korrelálásához kéri, hogy válaszokat, és tudjuk, hogy kérések mindig előttinek válaszok.

### <a name="http-payloads"></a>HTTP is észleltünk adattartalmakat.
Létrehozása után a `requestLine`, hogy ellenőrizze, hogy ha a kérelem törzsében csonkolva lesz. A kérelem törzsében csak 1024 csonkítja. Ez növekedhet, azonban az egyes Event Hub-üzenetek korlátozva, legfeljebb 256 KB, akkor valószínű, hogy bizonyos HTTP-üzenet szervek lesz nem elférjen egyetlen üzenetben. Naplózás és elemzési visszamehet jelentős mennyiségű olyan információt is alaptípusokból származtathatók, csak a HTTP kérelem-sor és a fejlécet. Is sok API-k kérése csak visszaadott kis szervezetek és így információinak értékét a nagy szervezetek csonkolására elvesztését viszonylag minimális támogatáshoz képest korlátozottabb az átvitel, feldolgozási és tárolási költségeket, így minden szervezet tartalmat csökkentése. A szervezet feldolgozási utolsó Megjegyzés: a, hogy át kell `true` , az As<string>() metódus túlterhelését, mert azt általában azért olvassák el a törzs tartalma, de volt is szeretett volna a háttérrendszer API-t a szervezet olvashatja. Ezzel a módszerrel igaz értéket ad, hogy a törzset, hogy elolvashatja a második alkalommal kell pufferelt miatt. Ez azért fontos tudni, ha egy API-t, amely rendelkezik, a nagy méretű fájlok feltöltését, vagy hosszú lekérdezési használja. Ezekben az esetekben a szervezet minden olvasási elkerülése érdekében ajánlott lenne.   

### <a name="http-headers"></a>HTTP-fejlécek
HTTP-fejléceket is továbbítja az üzenet formátumba egyszerű kulcs/érték pár formátuma. Választottuk ki feleslegesen a hitelesítő adatok kiszivárgását elkerülése érdekében bizonyos biztonsági időérzékeny mezőinek. Nem valószínű, hogy API-kulcsok és más hitelesítő adatokat használni kívánt elemzés céljából. Kívánjuk osztani a elemzéseket végezhet a felhasználó és az adott termék használják, akkor a kapunk lehetett az, hogy a `context` objektumot, és adja hozzá, amely az üzenetet.     

### <a name="message-metadata"></a>Üzenet-metaadatok
A teljes üzenetet küldeni az event hubs felépítése, amikor az első sor nincs valójában része a `application/http` üzenet. Az első sorban, hogy az üzenet-e a kérés vagy a válaszüzenet és a egy üzenet azonosítója, amelyet összekapcsolását kérelmek által küldött válaszokhoz álló további metaadatokat. Az Üzenetazonosító jön létre egy másik szabályzattal, a következőhöz hasonló:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Azt volt a kérelemüzenet létrehozott, egy változó tárolja, amíg a válaszban visszaadott és a küldi el a rendszer a kérések és válaszok egy üzenetnek számít. Azonban a kérés-válasz küldése egymástól függetlenül, és a egy üzenetazonosító használatával korrelációját, ha a két, kapunk egy kicsit nagyobb rugalmasságot biztosít az üzenet mérete, lehetővé teszi több partíciót előnyeinek kihasználása, ugyanakkor biztosítják az üzenetek sorrendje és a kérés jelenik meg a a naplózási irányítópulton hamarabb. Emellett előfordulhat néhány forgatókönyvet, ahol érvényes válasz soha nem továbbítja az event hubs, valószínűleg az API Management szolgáltatásban, egy kérelem végzetes hiba miatt, de még mindig van egy rekordot, a kérelem.

A válasz HTTP-üzenet küldése a szabályzat hasonlít a kérelmet, és így nézhet ki a teljes házirend-konfigurációt:

```xml
<policies>
  <inbound>
      <set-variable name="message-id" value="@(Guid.NewGuid())" />
      <log-to-eventhub logger-id="conferencelogger" partition-id="0">
      @{
          var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                      context.Request.Method,
                                                      context.Request.Url.Path + context.Request.Url.QueryString);

          var body = context.Request.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Request.Headers
                               .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                               .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                               .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "request:"   + context.Variables["message-id"] + "\n"
                              + requestLine + headerString + "\r\n" + body;
      }
  </log-to-eventhub>
  </inbound>
  <backend>
      <forward-request follow-redirects="true" />
  </backend>
  <outbound>
      <log-to-eventhub logger-id="conferencelogger" partition-id="1">
      @{
          var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                              context.Response.StatusCode,
                                              context.Response.StatusReason);

          var body = context.Response.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Response.Headers
                                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                          .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "response:"  + context.Variables["message-id"] + "\n"
                              + statusLine + headerString + "\r\n" + body;
     }
  </log-to-eventhub>
  </outbound>
</policies>
```

A `set-variable` házirend által egyaránt elérhető értéket hoz létre a `log-to-eventhub` szabályzat a `<inbound>` szakaszban, és a `<outbound>` szakaszban.  

## <a name="receiving-events-from-event-hubs"></a>Az események fogadását az Event Hubsról
Események az Azure Event Hubs használatával fogadott a [AMQP protokoll](http://www.amqp.org/). A Microsoft Service Bus csapatával ügyfél hajtott végre a fogyasztó események könnyebbé tenni az elérhető szalagtárak. Támogatott két többféle módon, egy folyamatban van egy *közvetlen fogyasztói* és egyéb használ a `EventProcessorHost` osztály. A két megközelítést példái megtalálhatók a [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md). A különbségek rövid verziója, `Direct Consumer` teljes körű felügyeletet biztosít, és a `EventProcessorHost` nem közé bekötése, az viszont lehetővé teszi bizonyos feltételezéseket hogyan dolgozza fel ezeket az eseményeket.  

### <a name="eventprocessorhost"></a>EventProcessorHost
Ebben a példában használjuk a `EventProcessorHost` az egyszerűség kedvéért mindazonáltal, előfordulhat, hogy ebben a konkrét esetben nem a legjobb választás. `EventProcessorHost` nem ellenőrizte, hogy nem kell aggódnia threading belül egy adott processzor eseményosztály problémák bonyolult feladatát. Azonban ebben az esetben, hogy egyszerűen konvertálás az üzenet egy másik formátumba és átadásával mentén, és egy másik szolgáltatás async módszert használ. Hiba esetén nem kell megosztott állapotot, és ezért problémák threading kockázata frissítéséhez. A legtöbb esetben `EventProcessorHost` valószínűleg a legjobb választás, és természetesen a könnyebb beállítás.     

### <a name="ieventprocessor"></a>IEventProcessor
A központi fogalma használatakor `EventProcessorHost` megvalósítását létrehozása a `IEventProcessor` felület, amely tartalmazza a metódus `ProcessEventAsync`. Ez a módszer lényege itt jelenik meg:

```csharp
async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
{

   foreach (EventData eventData in messages)
   {
       _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

       try
       {
           var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
           await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
       }
       catch (Exception ex)
       {
           _Logger.LogError(ex.Message);
       }
   }
    ... checkpointing code snipped ...
}
```

A metódusnak átadott EventData objektum listáját, és azt, hogy a lista ciklustevékenység. Az egyes módszerek bájt HttpMessage objektumba elemzi, és az objektum egy példányát IHttpMessageProcessor átadott.

### <a name="httpmessage"></a>HttpMessage
A `HttpMessage` -példány az adatok három darabot tartalmaz:

```csharp
public class HttpMessage
{
   public Guid MessageId { get; set; }
   public bool IsRequest { get; set; }
   public HttpRequestMessage HttpRequestMessage { get; set; }
   public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

A `HttpMessage` példány tartalmaz egy `MessageId` GUID, amely lehetővé teszi számunkra, hogy a HTTP-kérelem csatlakozhat a megfelelő HTTP-válasz és a egy logikai értéket, ha az objektum tartalmaz egy HttpRequestMessage és HttpResponseMessage egy példányát azonosítja. Az osztályok a beépített HTTP `System.Net.Http`, előnyeit képes voltam az `application/http` elemzés a kódot, amely része a `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
A `HttpMessage` megvalósítását, majd továbbítja a példány `IHttpMessageProcessor`, azaz különítse el a forgalmat fogadó és az Azure Event Hubs az esemény értelmezését és a tényleges feldolgozását, létrehozott egy felületet.

## <a name="forwarding-the-http-message"></a>A HTTP üzenet továbbítása
Ehhez a mintához kifejezetten érdekes paranccsal küldje le a HTTP-kérelem keresztül lenne [Runscope](http://www.runscope.com). Runscope egy felhőalapú szolgáltatás, amely HTTP-hibakeresés, naplózás és figyelés. Egy ingyenes szinttel, így könnyen próbálja ki, és lehetővé teszi számunkra, hogy tekintse meg a HTTP-kérések valós időben az API Management szolgáltatáson keresztül áramló rendelkeznek.

A `IHttpMessageProcessor` megvalósítási néz ki,

```csharp
public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
{
   private HttpClient _HttpClient;
   private ILogger _Logger;
   private string _BucketKey;
   public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
   {
       _HttpClient = httpClient;
       var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
       _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
       _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
       _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
       _Logger = logger;
   }

   public async Task ProcessHttpMessage(HttpMessage message)
   {
       var runscopeMessage = new RunscopeMessage()
       {
           UniqueIdentifier = message.MessageId
       };

       if (message.IsRequest)
       {
           _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
           runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
       }
       else
       {
           _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
           runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
       }

       var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
       messagesLink.BucketKey = _BucketKey;
       messagesLink.RunscopeMessage = runscopeMessage;
       var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
       _Logger.LogDebug("Request sent to Runscope");
   }
}
```

Képes kihasználni az voltam egy [meglévő ügyféloldali kódtára a Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) , amely megkönnyíti leküldéses `HttpRequestMessage` és `HttpResponseMessage` példányok be a szolgáltatásba. A Runscope API eléréséhez szüksége egy fiókot és a egy API-kulcsot. Útmutató az első API-kulcs található a [hozzáférés Runscope API-alkalmazások létrehozása](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) képernyőfelvétel.

## <a name="complete-sample"></a>Teljes minta
A [forráskódját](https://github.com/darrelmiller/ApimEventProcessor) és tesztek a minta a Githubon érhetők el. Kell egy [API Management szolgáltatás](get-started-create-service-instance.md), [a csatlakoztatott Eseményközpontot](api-management-howto-log-event-hubs.md), és a egy [Tárfiók](../storage/common/storage-create-storage-account.md) maga a minta futtatásához.   

A minta akkor csak egy egyszerű konzolalkalmazást, amely figyeli az adott eseményközpontból érkező események alakítja át őket egy `HttpRequestMessage` és `HttpResponseMessage` objektumokat, és ezután továbbítja őket a Runscope API be.

A következő animált ábrán láthatja a webkiszolgálónak küldött a fejlesztői portál, az üzenet alatt érkezett, feldolgozott és továbbított Konzolalkalmazás majd a kérés és válasz jelenik meg a Runscope Traffic inspectorra API-kérelem.

![A kérelem lesznek továbbítva a Runscope bemutatója](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Összegzés
Az Azure API Management-szolgáltatás szempontokból ideális hely utazik, és az API-k HTTP-forgalom rögzítése biztosít. Az Azure Event Hubs egy rugalmasan méretezhető, költséghatékony megoldás, hogy a forgalom rögzítése és elágazó, naplózás, figyelés és más kifinomult elemzési rendszerek másodlagos feldolgozás. Csatlakozás a külső forgalmat, Runscope nem egyszerű: mindössze néhány tucat sornyi kóddal rendszerek figyelése.

## <a name="next-steps"></a>További lépések
* További információ az Azure Event Hubs
  * [Az Azure Event Hubs használatának első lépései](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Üzenetek fogadása az EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programozási útmutató](../event-hubs/event-hubs-programming-guide.md)
* További információ az API Management és az Event Hubs-integráció
  * [Eseménynaplózás az Azure Event Hubs az Azure API Management hogyan](api-management-howto-log-event-hubs.md)
  * [Naplózó entitások bemutatása](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [napló eventhub házirend-referencia](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)

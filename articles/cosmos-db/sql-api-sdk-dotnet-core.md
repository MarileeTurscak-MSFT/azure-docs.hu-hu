---
title: 'Az Azure Cosmos DB: SQL .NET Core API, SDK és -erőforrások |} A Microsoft Docs'
description: Mindent megtudhat a SQL .NET Core API és az SDK, beleértve a kiadási dátum, kivezetési dátum és az Azure Cosmos DB .NET Core SDK minden verziója között végrehajtott módosítások.
services: cosmos-db
author: rnagpal
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7000fc13a792f4f6def592e78dc759c8cf165881
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451918"
---
# <a name="azure-cosmos-db-net-core-sdk-for-sql-api-release-notes-and-resources"></a>Az Azure Cosmos DB .NET Core SDK-t az SQL API-hoz: kibocsátási megjegyzések és erőforrások
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-módosítási hírcsatorna](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Aszinkron Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST erőforrás-szolgáltató](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor – .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor – Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**SDK letöltése**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**API-dokumentáció**</td><td>[.NET API dokumentációja](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Minták**</td><td>[.NET platformra írt kódmintái](sql-api-dotnet-samples.md)</td></tr>

<tr><td>**Első lépések**</td><td>[Az Azure Cosmos DB .NET Core SDK használatának első lépései](sql-api-dotnetcore-get-started.md)</td></tr>

<tr><td>**Alapú webappokról szóló oktatóanyagunkat**</td><td>[Webalkalmazás-fejlesztés az Azure Cosmos DB használatával](sql-api-dotnet-application.md)</td></tr>

<tr><td>**Aktuális támogatott keretrendszer**</td><td>[.NET standard 1.6-os és a .NET Standard 1.5-ös](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések

Az Azure Cosmos DB .NET Core SDK legújabb verziójában funkcióparitás rendelkezik a [Azure Cosmos DB .NET SDK](sql-api-sdk-dotnet.md).

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1

* Többrégiós kérés átmeneti hibák hozzá további rugalmasságának biztosításával.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0

* A hozzáadott többrégiós támogatási írni.
* Adatbázisközi partíció lekérdezési teljesítménnyel kapcsolatos fejlesztések a TOP és MaxBufferedItemCount.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0

* Új kérelem visszavonás támogatása.
* A hozzáadott SetCurrentLocation való ConnectionPolicy, amely automatikusan feltölti az elsődleges helyek a régióhoz igazodik.
* Kijavítva a hiba a partíció-lekérdezésekben Adatbázisközi minimális/maximális és a egy szűrőt, amely megegyezik az egyetlen dokumentum egyes partíción.
* DocumentClient módszerek IDocumentClient paritás most már rendelkezik.
* Frissített közvetlen TCP átviteli stack létesített kapcsolatok számának csökkentése érdekében.
* Támogatás hozzáadva a közvetlen mód TCP nem Windows-ügyfelek számára.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Új kérelem visszavonás támogatása.
* A hozzáadott SetCurrentLocation való ConnectionPolicy, amely automatikusan feltölti az elsődleges helyek a régióhoz igazodik.
* Kijavítva a hiba a partíció-lekérdezésekben Adatbázisközi minimális/maximális és a egy szűrőt, amely megegyezik az egyetlen dokumentum egyes partíción.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* DocumentClient módszerek IDocumentClient paritás most már rendelkezik.
* Frissített közvetlen TCP átviteli stack létesített kapcsolatok számának csökkentése érdekében.
* Támogatás hozzáadva a közvetlen mód TCP nem Windows-ügyfelek számára.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0

* FeedOptions hozzáadott ConsistencyLevel tulajdonság.
* A hozzáadott JsonSerializerSettings RequestOptions és FeedOptions.
* A hozzáadott EnableReadRequestsFallback ConnectionPolicy való.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1

* Rögzített KeyNotFoundException a partíció rendelés közötti esetekben a lekérdezések által.
* Kijavítva a hiba, a select záradékban a LINQ-lekérdezések JsonPropery attribútum lett nem folyamatban figyelembe véve.

### <a name="a-name182182"></a><a name="1.8.2"/>1.8.2

* Kijavítva a hiba, amely bizonyos fajta feltételek elérte az eredmények az időszakos "Microsoft.Azure.Documents.NotFoundException: az olvasási munkamenet nem érhető el a bemeneti munkamenet-jogkivonat" hibák munkamenet konzisztenciaszint használatakor.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1

* Rögzített regressziós ahol FeedOptions.MaxItemCount = -1-System.ArithmeticException okozott: oldalméret értéke negatív.
* QueryMetrics hozzá egy új ToString() függvényét.
* Közzétett partíció statisztikai gyűjtemények olvasása.
* ChangeFeedOptions hozzáadott PartitionKey tulajdonság.
* Kisebb hibajavítások.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
 
 * Hozzáadja a DocumentCollection UniqueKeyPolicy tulajdonság használatával a dokumentumok egyedi indexek megadásának lehetőségét.
 * Kijavítva a hiba, amelyben az egyéni JsonSerializer beállítások lettek nem folyamatban figyelembe véve az egyes lekérdezések és a tárolt eljárás végrehajtása.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
 
 * Azure Cosmos DB API-referencia az Azure documentdb módosítás márkajelzési, dokumentáció, szerelvényekben metaadat-információkat és a NuGet-csomagot. 
 * Tegye elérhetővé a diagnosztikai adatok és a késés közvetlen kapcsolódás móddal küldött kérelmeket a válaszból. A tulajdonságnevek megkülönböztetik a RequestDiagnosticsString és RequestLatency ResourceResponse osztályhoz.
 * Az SDK-verziója letölthető Azure Cosmos DB Emulatort elérhető legújabb verziója szükséges https://aka.ms/cosmosdb-emulator.
 
### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Több megbízhatóság javításai és továbbfejlesztései hozzá.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1 

* Kapcsolódó belső változások támogató [Microsoft.Azure.Graphs](https://docs.microsoft.com/azure/cosmos-db/graph-sdk-dotnet)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Egy lekérdezés eredményeit a kulcs-egy adott partíció tartományértéke sorolására FeedOption, PartitionKeyRangeId támogatása. 
* Támogatás hozzáadva a StartTime, hogy elkezdhessük megvizsgálni a módosítások az időnek az elteltével a ChangeFeedOption. 

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Kijavítva a JsonSerializable osztály, amely a stack overflow kivételt okozhat.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Támogatás hozzáadva az egyéni JsonSerializerSettings megadása közben hárítható el egy [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) példány.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   A cél keretrendszereket egyik támogatása a .NET Standard 1.5-ös.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Kijavítva, hogy x64 érintett gépek, amelyek nem támogatják a SSE4 utasítást, és throw SEHException, amikor az Azure Cosmos DB-lekérdezések futtatásához.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Egy új konzisztenciaszint támogatása az új ConsistentPrefix nevezik.
*   Az egyes partíciók lekérdezési metrikák támogatása.
*   Támogatás hozzáadva a lekérdezések a folytatási méretét korlátozó.
*   Sikertelen kérelmek nyomkövetésének részletesebb támogatása.
*   Bizonyos teljesítménnyel kapcsolatos fejlesztések készült SDK-ban.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Kijavítva, hogy a rendszer figyelmen kívül hagyja a PartitionKey értéket FeedOptions megadott összesítő lekérdezésekhez.
* Javítva lett egy probléma transzparens kezelését a partíció felügyeleti közepes repülési a partíciók közti Order By záradék a lekérdezés végrehajtása során.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Rögzített holtpontok az API-k, az ASP.NET-környezetben való használat async néhány okozó problémát.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Javít, hogy rugalmasabb SDK bizonyos körülmények között az automatikus feladatátvételre.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Egy WebException alkalmanként okozó probléma: A távoli név nem oldható fel.
* A beírt dokumentumok közvetlenül olvasásakor ReadDocumentAsync API új hozzáadásával támogatása hozzáadva.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* LINQ támogatása összesítési lekérdezések (száma, MIN, MAX, SUM és átlagos).
* Javítás a memóriavesztés az oka eseménykezelő ConnectionPolicy objektum számára.
* Javít egy problémát UpsertAttachmentAsync viselkedésmintáit nem dolgozott ETag használatakor.
* Javítsa ki egy problémát a viselkedésmintáit közötti partíció az order by lekérdezések folytatása nem dolgozott rendezésekor karakterlánc típusú.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Összesítés lekérdezések (száma, MIN, MAX, SUM és átlagos) támogatása. Lásd: [összesítési támogatási](sql-api-sql-query.md#Aggregates).
* Süllyesztett minimálisan 2500 RU/s 10,100 RU/s a particionált gyűjtemények átviteli sebességet.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

Az Azure Cosmos DB .NET Core SDK lehetővé teszi, hogy hozhat létre gyors és platformfüggetlen [ASP.NET Core](https://www.asp.net/core) és [.NET Core](https://www.microsoft.com/net/core#windows) Windows, Mac és Linux rendszereken futó alkalmazások. Az Azure Cosmos DB .NET Core SDK legújabb kiadása teljesen van [Xamarin](https://www.xamarin.com) kompatibilis és használható iOS, Android és Mono (Linux) készített alkalmazások fejlesztésére.  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

Az Azure Cosmos DB .NET Core előzetes SDK lehetővé teszi, hogy hozhat létre gyors és platformfüggetlen [ASP.NET Core](https://www.asp.net/core) és [.NET Core](https://www.microsoft.com/net/core#windows) Windows, Mac és Linux rendszereken futó alkalmazások.

Az Azure Cosmos DB .NET Core előzetes SDK funkcióparitás legújabb verziójával rendelkezik a [Azure Cosmos DB .NET SDK](sql-api-sdk-dotnet.md) és a következőket támogatja:
* Az összes [csatlakozási mód](performance-tips.md#networking): az átjáró mód, közvetlen TCP és közvetlen HTTPs. 
* Az összes [konzisztenciaszintek](consistency-levels.md): erős, munkamenet, korlátozott frissesség és végleges.
* [A particionált gyűjtemények](partition-data.md). 
* [Többrégiós adatbázisfiókhoz és georeplikáció útján](distribute-data-globally.md).

Ha ez az SDK kapcsolatos kérdése van, küldése a [StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), vagy be a problémát a [github-adattár](https://github.com/Azure/azure-documentdb-dotnet/issues). 

## <a name="release--retirement-dates"></a>Állapot tárolá & kivezetési dátum

| Verzió | Kiadás dátuma | Visszavonás dátuma |
| --- | --- | --- |
| [2.1.1](#2.1.1) |2018. szeptember 27. |--- |
| [2.1.0](#2.1.0) |2018. szeptember 21. |--- |
| [2.0.0](#2.0.0) |2018. szeptember 07. |--- |
| [1.9.1](#1.9.1) |2018. március 09. |--- |
| [1.8.2](#1.8.2) |2018. február 21. |--- |
| [1.8.1](#1.8.1) |2018. február 05 |--- |
| [1.7.1](#1.7.1) |2017. november 16. |--- |
| [1.7.0](#1.7.0) |2017. november 10. |--- |
| [1.6.0](#1.6.0) |2017. október 17. |--- |
| [1.5.1](#1.5.1) |2017. október 02 |--- |
| [1.5.0](#1.5.0) |2017. augusztus 10. |--- | 
| [1.4.1](#1.4.1) |2017. augusztus 07. |--- |
| [1.4.0](#1.4.0) |2017. augusztus 02 |--- |
| [1.3.2](#1.3.2) |2017. június 12. |--- |
| [1.3.1](#1.3.1) |2017. május 23. |--- |
| [1.3.0](#1.3.0) |2017. május 10. |--- |
| [1.2.2](#1.2.2) |2017. április 19. |--- |
| [1.2.1](#1.2.1) |2017. március 29. |--- |
| [1.2.0](#1.2.0) |2017. március 25. |--- |
| [1.1.2](#1.1.2) |2017. március 20. |--- |
| [1.1.1](#1.1.1) |2017. március 14. |--- |
| [1.1.0](#1.1.0) |2017. február 16. |--- |
| [1.0.0](#1.0.0) |2016. december 21-én |--- |
| [0.1.0-preview](#0.1.0-preview) |2016. november 15-én |2016. december 31-ig. |

## <a name="see-also"></a>Lásd még:
Cosmos DB kapcsolatos további információkért lásd: [a Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján. 


---
title: Node.js-példák az Azure Cosmos DB-hez | Microsoft Docs
description: A GitHubon Node.js-példákat találhat az Azure Cosmos DB-beli gyakori feladatokhoz, köztük a CRUD-műveletekhez.
keywords: node.js-példák
services: cosmos-db
author: moderakh
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: sample
ms.date: 05/23/2017
ms.author: moderakh
ms.openlocfilehash: 0abe743f9afceea936a55a13d7004a3094ddb5b3
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/14/2018
ms.locfileid: "45575269"
---
# <a name="azure-cosmos-db-nodejs-examples"></a>Azure Cosmos DB Node.js-példák
> [!div class="op_single_selector"]
> * [.NET-példák](sql-api-dotnet-samples.md)
> * [Java-példák](sql-api-java-samples.md)
> * [Aszinkron Java-példák](sql-api-async-java-samples.md)
> * [Node.js-példák](sql-api-nodejs-samples.md)
> * [Node.js-példák – 2.0-s verzió (előzetes verzió)](sql-api-nodejs-samples-preview.md)
> * [Python-példák](sql-api-python-samples.md)
> * [Azure Kódminta-katalógus](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

CRUD-műveleteket és más gyakori műveleteket Azure Cosmos DB-erőforrásokon végrehajtó megoldásminták is megtalálhatók az [azure-documentdb-nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) GitHub-tárházban. Ez a cikk a következő információkat tartalmazza:

* Az egyes Node.js-példák projektfájljaiban a feladatokra mutató hivatkozások.
* A kapcsolódó API-referenciatartalmak hivatkozásai.

**Előfeltételek**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [Aktiválhatja Visual Studio-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): A Visual Studio-előfizetés minden hónapban biztosít Önnek jóváírást, amelyekkel fizetős Azure-szolgáltatásokat használhat.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Szüksége lesz a [Node.js SDK](sql-api-sdk-node.md)-ra is.
   
   > [!NOTE]
   > Minden minta önálló, elvégzi a saját beállításait és eltávolítja a nyomait. Éppen ezért a minták többször is meghívják a [DocumentClient.createCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createcollection-databaselink--body--options--callback-
) metódust. Előfizetését minden ilyen esetben a létrehozott gyűjtemény teljesítményszintjének megfelelő, 1 órai használati díj terheli.
   > 
   > 

## <a name="database-examples"></a>Adatbázis-példák
A [DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) projekt [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) fájlja a következő feladatok végrehajtását mutatja be.

| Tevékenység | API-referencia |
| --- | --- |
| [Adatbázis létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) |[DocumentClient.createDatabase](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createdatabase-body--options--callback-) |
| [Fiók lekérdezése egy adatbázishoz](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) |[DocumentClient.queryDatabases](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#querydatabases-query--options-) |
| [Adatbázis olvasása Id (azonosító) alapján](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) |[DocumentClient.readDatabase](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdatabase-databaselink--options--callback-) |
| [Egy fiókhoz tartozó adatbázisok listázása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) |[DocumentClient.readDatabases](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdatabases-options-) |
| [Adatbázis törlése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) |[DocumentClient.deleteDatabase](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#deletedatabase-databaselink--options--callback-) |

## <a name="collection-examples"></a>Gyűjtemény-példák
A [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) projekt [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) fájlja a következő feladatok végrehajtását mutatja be.

| Tevékenység | API-referencia |
| --- | --- |
| [Gyűjtemény létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) |[DocumentClient.createCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createcollection-databaselink--body--options--callback-) |
| [Az egy adatbázisban lévő összes gyűjtemény listájának olvasása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) |[DocumentClient.readCollections](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readcollections-databaselink--options-) |
| [Gyűjtemény beolvasása a by_self mező alapján](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) |[DocumentClient.readCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readcollection-collectionlink--options--callback-) |
| [Gyűjtemény beolvasása Id (azonosító) alapján](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) |[DocumentClient.readCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readcollection-collectionlink--options--callback-) |
| [Gyűjtemény teljesítményszintjének lekérése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) |[DocumentQueryable.queryOffers](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#queryoffers-query--options-) |
| [Gyűjtemény teljesítményszintjének módosítása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) |[DocumentClient.replaceOffer](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#replaceoffer-offerlink--offer--callback-) |
| [Gyűjtemény törlése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) |[DocumentClient.deleteCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#deletecollection-collectionlink--options--callback-) |

## <a name="document-examples"></a>Dokumentum-példák
A [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) projekt [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) fájlja a következő feladatok végrehajtását mutatja be.

| Tevékenység | API-referencia |
| --- | --- |
| [Dokumentumok létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) |[DocumentClient.createDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createdocument-documentsfeedordatabaselink--body--options--callback-) |
| [Gyűjtemény dokumentum-csatornájának olvasása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) |[DocumentClient.readDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdocument-documentlink--options--callback-) |
| [Dokumentum olvasása ID (azonosító) alapján](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) |[DocumentClient.readDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdocument-documentlink--options--callback-) |
| [Dokumentum olvasása csak a dokumentum módosítása esetén](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) |[DocumentClient.readDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#readdocument-documentlink--options--callback-)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.requestoptions.accesscondition?view=azure-dotnet#Microsoft_Azure_Documents_Client_RequestOptions_AccessCondition) |
| [Dokumentumok lekérdezése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) |[DocumentClient.queryDocuments](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#querydocuments-documentsfeedordatabaselink--query--options-) |
| [Dokumentum cseréje](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |[DocumentClient.replaceDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#replacedocument-documentlink--document--options--callback-) |
| [Dokumentum cseréje feltételes ETag-ellenőrzéssel](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |[DocumentClient.replaceDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#replacedocument-documentlink--document--options--callback-)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.requestoptions.accesscondition?view=azure-dotnet#Microsoft_Azure_Documents_Client_RequestOptions_AccessCondition) |
| [Dokumentum törlése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) |[DocumentClient.deleteDocument](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#deletedocument-documentlink--options--callback-) |

## <a name="indexing-examples"></a>Indexelési példák
Az [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) projekt [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) fájlja a következő feladatok végrehajtását mutatja be.

| Tevékenység | API-referencia |
| --- | --- |
| [Gyűjtemény létrehozása alapértelmezett indexeléssel](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L657-L701) |[DocumentClient.createCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createcollection-databaselink--body--options--callback-) |
| [Meghatározott dokumentum manuális indexelése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) |[RequestOptions.indexingDirective: 'include'](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective?view=azure-dotnet#Microsoft_Azure_Documents_Client_RequestOptions_IndexingDirective) |
| [Meghatározott dokumentum manuális kizárása az indexből](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) |[RequestOptions.indexingDirective: 'exclude'](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective?view=azure-dotnet#Microsoft_Azure_Documents_Client_RequestOptions_IndexingDirective) |
| [Szakaszolt indexelés használata tömeges importáláshoz és nagy gyűjtemények beolvasásához](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) |[IndexingMode.Lazy](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.indexingmode?view=azure-dotnet) |
| [Dokumentum megadott elérési útjainak befoglalása az indexelés során](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) |[IndexingPolicy.IncludedPaths](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.indexingpolicy.includedpaths?view=azure-dotnet#Microsoft_Azure_Documents_IndexingPolicy_IncludedPaths) |
| [Bizonyos elérési utak kizárása az indexelésből](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) |[IndexingPolicy.ExcludedPath](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.indexingpolicy.excludedpaths?view=azure-dotnet#Microsoft_Azure_Documents_IndexingPolicy_ExcludedPaths) |
| [Karakterlánc-útvonalon végzett keresés engedélyezése tartományi művelethez](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347) |[FeedOptions.EnableScanInQuery](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.feedoptions.enablescaninquery?view=azure-dotnet#Microsoft_Azure_Documents_Client_FeedOptions_EnableScanInQuery) |
| [Tartományindex létrehozása karakterlánc-útvonalon](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) |[IndexKind.Range](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.indexkind?view=azure-dotnet#Microsoft_Azure_Documents_IndexKind_Range), [IndexingPolicy](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.indexingpolicy?view=azure-dotnet), [DocumentClient.queryDocument](https://docs.microsoft.com/javascript/api/documentdb/documentclient?view=azure-node-latest) |
| [Gyűjtemény létrehozása alapértelmezett indexPolicy-vel, majd ennek online frissítése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) |[DocumentClient.createCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createcollection-databaselink--body--options--callback-)<br> [DocumentClient.replaceCollection#replaceCollection](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#replacecollection-collectionlink--collection--options--callback-) |

Az indexeléssel kapcsolatban az [Azure Cosmos DB indexelési szabályzatok](indexing-policies.md) című cikkben talál bővebb információt.

## <a name="server-side-programming-examples"></a>Kiszolgálóoldali programozási példák
A [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) projekt [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) fájlja a következő feladatok végrehajtását mutatja be.

| Tevékenység | API-referencia |
| --- | --- |
| [Tárolt eljárás létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) |[DocumentClient.createStoredProcedure](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#createstoredprocedure-collectionlink--sproc--options--callback-) |
| [Tárolt eljárás végrehajtása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) |[DocumentClient.executeStoredProcedure](https://docs.microsoft.com/javascript/api/documentdb/DocumentClient?view=azure-node-latest#executestoredprocedure-sproclink--params--options--callback-) |

A kiszolgálóoldali programozásról az [Azure Cosmos DB kiszolgálóoldali programozása: Tárolt eljárások, adatbázis-eseményindítók és felhasználói függvények](programming.md) című cikkben talál bővebb információt.

## <a name="partitioning-examples"></a>Particionálási példák
Az [Partitioning](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) projekt [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) fájlja a következő feladatok végrehajtását mutatja be.

| Tevékenység | API-referencia |
| --- | --- |
| [A HashPartitionResolver használata](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) |[HashPartitionResolver](https://docs.microsoft.com/javascript/api/documentdb/HashPartitionResolver?view=azure-node-latest) |

Az adatok Azure Cosmos DB-beli particionálásáról az [Adatok particionálása és méretezése az Azure Cosmos DB-ben](partition-data.md) című cikkben talál további információt.


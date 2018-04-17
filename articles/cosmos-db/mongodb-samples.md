---
title: MongoDB API-k segítségével az Azure Cosmos DB alkalmazás létrehozása |} Microsoft Docs
description: Ez az oktatóanyag az Azure Cosmos DB API-k használatával mongodb egy online adatbázist hoz létre.
keywords: mongodb-példák
services: cosmos-db
author: AndrewHoh
manager: kfile
editor: ''
documentationcenter: ''
ms.assetid: fb38bc53-3561-487d-9e03-20f232319a87
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: anhoh
ms.openlocfilehash: 81eff479c94af938918e6a221d45184ca1a84aef
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/16/2018
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Egy Azure Cosmos DB létrehozása: API-t a MongoDB-alkalmazásokhoz Node.js használatával
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Node.js MongoDB-hez](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [C++](sql-api-cpp-get-started.md)
>  
>

Ez a példa bemutatja, hogyan hozhat létre egy Azure Cosmos DB: API-t a MongoDB-Konzolalkalmazás Node.js segítségével.

Ebben a példában használatához az alábbiak szükségesek:

* [Hozzon létre](create-mongodb-dotnet.md#create-account) egy Azure Cosmos DB: API-t a MongoDB-fiók.
* A MongoDB beolvasása [kapcsolati karakterlánc](connect-mongodb-account.md) információkat.

## <a name="create-the-app"></a>Az alkalmazás létrehozása

1. Hozzon létre egy *app.js* fájl és másolja és illessze be az alábbi kódot.

    ```nodejs
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<username>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';

    var insertDocument = function(db, callback) {
    db.collection('families').insertOne( {
            "id": "AndersenFamily",
            "lastName": "Andersen",
            "parents": [
                { "firstName": "Thomas" },
                { "firstName": "Mary Kay" }
            ],
            "children": [
                { "firstName": "John", "gender": "male", "grade": 7 }
            ],
            "pets": [
                { "givenName": "Fluffy" }
            ],
            "address": { "country": "USA", "state": "WA", "city": "Seattle" }
        }, function(err, result) {
        assert.equal(err, null);
        console.log("Inserted a document into the families collection.");
        callback();
    });
    };
    
    var findFamilies = function(db, callback) {
    var cursor =db.collection('families').find( );
    cursor.each(function(err, doc) {
        assert.equal(err, null);
        if (doc != null) {
            console.dir(doc);
        } else {
            callback();
        }
    });
    };
    
    var updateFamilies = function(db, callback) {
    db.collection('families').updateOne(
        { "lastName" : "Andersen" },
        {
            $set: { "pets": [
                { "givenName": "Fluffy" },
                { "givenName": "Rocky"}
            ] },
            $currentDate: { "lastModified": true }
        }, function(err, results) {
        console.log(results);
        callback();
    });
    };
    
    var removeFamilies = function(db, callback) {
    db.collection('families').deleteMany(
        { "lastName": "Andersen" },
        function(err, results) {
            console.log(results);
            callback();
        }
    );
    };
    
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    **Nem kötelező**: használata a **MongoDB Node.js 2.2 illesztőprogram**, cserélje le a következő kódrészletet:

    Eredeti:

    ```nodejs
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    Le kell cserélni:

    ```nodejs
    MongoClient.connect(url, function(err, db) {
    assert.equal(null, err);
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                db.close();
            });
        });
        });
    });
    });
    ```
    
2. A következő változók módosítása a *app.js* fiókbeállításokban / fájl (keresése a [kapcsolati karakterlánc](connect-mongodb-account.md)):

    > [!IMPORTANT]
    > A **MongoDB Node.js 3.0 illesztőprogram** Cosmos DB jelszóban szereplő speciális karakterek kódolás szükséges. Ügyeljen arra, hogy a "=" karakterek kódolása % 3D
    >
    > Példa: A jelszó *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv ==* kódolja *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv 3D % 3D*
    >
    > A **MongoDB Node.js 2.2 illesztőprogram** nincs szükség a Cosmos DB jelszót különleges karakterek kódolása.
    >
    >
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Nyissa meg kedvenc terminálját, és futtassa **npm mongodb--telepítése Mentés**, majd futtassa az alkalmazás a **csomópont app.js**

## <a name="next-steps"></a>További lépések
* Megtudhatja, hogyan [MongoChef használja](mongodb-mongochef.md) együtt az Azure Cosmos DB: API-t a MongoDB-fiók.

---
title: Node.js-webalkalmazás létrehozása az Azure Cosmos DB-hez | Microsoft Docs
description: Ez a Node.js-oktatóanyag bemutatja, hogyan tárolhatja és érheti el az Azure Websitesban tárolt Node.js Express-webalkalmazások adatait a Microsoft Azure Cosmos DB segítségével.
keywords: Alkalmazásfejlesztés, adatbázis-oktatóanyag, a node.js megismerése, node.js-oktatóanyag
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 03/23/2018
ms.author: sngun
ms.openlocfilehash: f7f41e9d77e0687c6c8b25a4163348a7310aa40c
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697323"
---
# <a name="_Toc395783175"></a>Node.js-webalkalmazás létrehozása az Azure Cosmos DB használatával

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Node.js – v2](sql-api-nodejs-application-preview.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)
> 

Ez a Node.js-oktatóanyag bemutatja, miként tárolhatja és érheti el az Azure Websitesban tárolt Node.js Express-alkalmazás adatait az Azure Cosmos DB és az SQL API segítségével. Olyan egyszerű webalapú teendőkezelő alkalmazást, todo appot fog létrehozni, amellyel feladatokat készíthet, kérhet le, és végezhet el. A feladatokat JSON-dokumentumok formájában tárolja az Azure Cosmos DB. Ez az oktatóanyag bemutatja az alkalmazás létrehozásának és üzembe helyezésének lépéseit, valamint hogy mi történik az egyes kódrészletekben.

![Képernyőfelvétel a jelen Node.js oktatóanyag során készített My Todo List (Saját teendőlista) alkalmazásról](./media/sql-api-nodejs-application/cosmos-db-node-js-mytodo.png)

Nincs ideje elvégezni az oktatóanyagot, és csak hozzá szeretne jutni a teljes megoldáshoz? Semmi gond, a teljes mintamegoldást beszerezheti a [GitHubról][GitHub]. Az alkalmazás futtatásához szükséges útmutatást az [Olvass el](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) fájlban találja.

## <a name="_Toc395783176"></a>Előfeltételek
> [!TIP]
> Ez a Node.js-oktatóanyag feltételezi, hogy rendelkezik némi tapasztalattal a Node.js és az Azure Websites használatát illetően.
> 
> 

A jelen cikkben lévő utasítások követése előtt rendelkeznie kell a következőkkel:

* Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js]-verzió: 0.10.29-es vagy újabb. A Node.js 6.10 vagy újabb verzióját javasoljuk.
* [Express generátor](http://www.expressjs.com/starter/generator.html) (az `npm install express-generator -g` segítségével telepítheti)
* [Git][Git].

## <a name="_Toc395637761"></a>1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása
Először hozzon létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik fiókkal, vagy az oktatóanyagban az Azure Cosmos DB Emulatort használja, továbbléphet a [2. lépés: Új Node.js-alkalmazás létrehozása](#_Toc395783178) című lépésre.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>2. lépés: Új Node.js-alkalmazás létrehozása
Most megtanulhatja, hogyan hozhat létre egy alapszintű Hello World Node.js-projektet az [Express](http://expressjs.com/)-keretrendszer használatával.

1. Nyissa meg kedvenc terminálját, például a Node.js parancssort.
2. Keresse meg azt a könyvtárat, amelyben tárolni szeretné az új alkalmazást.
3. Az Express generátor használatával hozzon létre egy új alkalmazást **todo** (teendők) néven.

   ```bash
   express todo
   ```
4. Nyissa meg az új **todo** könyvtárat, és telepítse a függőségeket.

   ```bash
    cd todo
    npm install
   ```
5. Futtassa az új alkalmazást.

   ```bash
   npm start
   ```
6. Az új alkalmazás megtekintéséhez navigáljon a böngészőben a következő címre: [http://localhost:3000](http://localhost:3000).
   
    ![A Node.js megismerése – Képernyőfelvétel a Hello World alkalmazásról egy böngészőablakban](./media/sql-api-nodejs-application/cosmos-db-node-js-express.png)

    Ezt követően az alkalmazás leállításához nyomja le a CTRL+C billentyűkombinációt a terminálablakban, majd Windows rendszerű gépek esetén a kötegelt feladat leállításához kattintson az **y** elemre.

## <a name="_Toc395783179"></a>3. lépés: További modulok telepítése
A **package.json** fájl egyike azon fájloknak, amelyek a projekt gyökérmappájában létrejönnek. Ez a fájl tartalmazza a Node.js-alkalmazáshoz szükséges további modulok listáját. Később, amikor az Azure Websitesra telepíti az alkalmazást, a rendszer ennek a fájlnak a segítségével határozza meg, hogy melyik modulokat kell az Azure-ban telepíteni ahhoz, hogy működjön az alkalmazás. A jelen oktatóanyag befejezéséhez még két csomag telepítésére van szükség.

1. A terminálban telepítse az **async** modult az npm segítségével.

   ```bash
   npm install async --save
   ```
2. Telepítse a **DocumentDB** modult az npm segítségével. Ez az a modul, amelyben az Azure Cosmos DB-vel kapcsolatos csodák történnek.

   ```bash
   npm install documentdb --save
   ```

## <a name="_Toc395783180"></a>4. lépés: Az Azure Cosmos DB szolgáltatás használata Node.js-alkalmazásokban
Ezzel a kezdeti beállítás és konfiguráció készen is van. Ideje elkezdeni a kódírást az Azure Cosmos DB használatával.

### <a name="create-the-model"></a>A modell létrehozása
1. A projektkönyvtáron belül hozzon létre egy új könyvtárat **models** (modellek) néven, a package.json fájllal egy könyvtárban.
2. A **models** könyvtárban hozzon létre egy új fájlt **task-model.js** néven. Ez a fájl tartalmazza majd a modellt az alkalmazás által létrehozott feladatok számára.
3. Ugyanabban a **models** könyvtárban hozzon létre egy másik új fájlt **cosmosdb-manager.js** néven. Ez a fájl néhány hasznos, újrafelhasználható, az alkalmazás minden területén használt kódot tartalmaz majd. 
4. Másolja be az alábbi kódot a **cosmosdb-manager.js** fájlba
    ```nodejs
    let DocumentDBClient = require('documentdb').DocumentClient;

    module.exports = {
    getOrCreateDatabase: (client, databaseId, callback) => {
        let querySpec = {
        query: 'SELECT * FROM root r WHERE r.id = @id',
        parameters: [{ name: '@id', value: databaseId }]
        };

        client.queryDatabases(querySpec).toArray((err, results) => {
        if (err) {
            callback(err);
        } else {
            if (results.length === 0) {
            let databaseSpec = { id: databaseId };
            client.createDatabase(databaseSpec, (err, created) => {
                callback(null, created);
            });
            } else {
            callback(null, results[0]);
            }
        }
        });
    },

    getOrCreateCollection: (client, databaseLink, collectionId, callback) => {
        let querySpec = {
        query: 'SELECT * FROM root r WHERE r.id=@id',
        parameters: [{ name: '@id', value: collectionId }]
        };

        client.queryCollections(databaseLink, querySpec).toArray((err, results) => {
        if (err) {
            callback(err);
        } else {
            if (results.length === 0) {
            let collectionSpec = { id: collectionId };
            client.createCollection(databaseLink, collectionSpec, (err, created) => {
                callback(null, created);
            });
            } else {
            callback(null, results[0]);
            }
        }
        });
    }
    };
    ```
5. Mentse és zárja be a **cosmosdb-manager.js** fájlt.
6. A **task-model.js** fájl elejéhez adja hozzá a következő kódot a **DocumentDBClient**-ügyfélre és a fentiekben létrehozott **cosmosdb-manager.js** fájlra való hivatkozáshoz: 

    ```nodejs
    let DocumentDBClient = require('documentdb').DocumentClient;
    let docdbUtils = require('./cosmosdb-manager.js');
    ```
7. Ezután adja hozzá a feladatobjektum meghatározására és exportálására használt kódot. Ez felelős a feladatobjektum elindításáért, valamint a használni kívánt adatbázis és dokumentumgyűjtemény beállításáért.  

    ```nodejs
    function TaskModel(documentDBClient, databaseId, collectionId) {
      this.client = documentDBClient;
      this.databaseId = databaseId;
      this.collectionId = collectionId;
   
      this.database = null;
      this.collection = null;
    }
   
    module.exports = TaskModel;
    ```
8. Ezután adja hozzá a következő kódot a feladatobjektumokhoz további metódusok meghatározásához, amelyek lehetővé teszik majd az Azure Cosmos DB-ben tárolt adatokkal folytatott interakciót.

    ```nodejs
    let DocumentDBClient = require('documentdb').DocumentClient;
    let docdbUtils = require('./cosmosdb-manager');

    function TaskModel(documentDBClient, databaseId, collectionId) {
    this.client = documentDBClient;
    this.databaseId = databaseId;
    this.collectionId = collectionId;

    this.database = null;
    this.collection = null;
    }

    TaskModel.prototype = {
    init: function(callback) {
        let self = this;

        docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function(err, db) {
        if (err) {
            callback(err);
        } else {
            self.database = db;
            docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function(err, coll) {
            if (err) {
                callback(err);
            } else {
                self.collection = coll;
            }
            });
        }
        });
    },

    find: function(querySpec, callback) {
        let self = this;

        self.client.queryDocuments(self.collection._self, querySpec).toArray(function(err, results) {
        if (err) {
            callback(err);
        } else {
            callback(null, results);
        }
        });
    },

    addItem: function(item, callback) {
        let self = this;

        item.date = Date.now();
        item.completed = false;

        self.client.createDocument(self.collection._self, item, function(err, doc) {
        if (err) {
            callback(err);
        } else {
            callback(null, doc);
        }
        });
    },

    updateItem: function(itemId, callback) {
        let self = this;

        self.getItem(itemId, function(err, doc) {
        if (err) {
            callback(err);
        } else {
            doc.completed = true;

            self.client.replaceDocument(doc._self, doc, function(err, replaced) {
            if (err) {
                callback(err);
            } else {
                callback(null, replaced);
            }
            });
        }
        });
    },

    getItem: function(itemId, callback) {
        let self = this;
        let querySpec = {
        query: 'SELECT * FROM root r WHERE r.id = @id',
        parameters: [{ name: '@id', value: itemId }]
        };

        self.client.queryDocuments(self.collection._self, querySpec).toArray(function(err, results) {
        if (err) {
            callback(err);
        } else {
            callback(null, results[0]);
        }
        });
    }
    };

    module.exports = TaskModel;
    ```
9. Mentse és zárja be a **task-model.js** fájlt. 

### <a name="create-the-controller"></a>A vezérlő létrehozása
1. A projekt **routes** könyvtárában hozzon létre egy új fájlt **tasklist.js** néven. 
2. Adja hozzá a következő kódot a **tasklist.js** fájlhoz. Ez betölti a **tasklist.js** fájl által használt DocumentDBClient és async modult. Emellett a **TaskList** (Feladatlista) függvényt is meghatározta, amelyet a rendszer továbbad a **Task** (Feladat) objektum korábban meghatározott példányának:
   
    ```nodejs
    let DocumentDBClient = require('documentdb').DocumentClient;
    let async = require('async');

    function TaskList(taskModel) {
    this.taskModel = taskModel;
    }

    module.exports = TaskList;
    ```
3. Folytassa a **tasklist.js** fájlhoz való hozzáadást a **showTasks, addTasks** és **completeTasks** által használt metódusok hozzáadásával.
   
   ```nodejs
    TaskList.prototype = {
    showTasks: function(req, res) {
        let self = this;

        let querySpec = {
        query: 'SELECT * FROM root r WHERE r.completed=@completed',
        parameters: [
            {
            name: '@completed',
            value: false
            }
        ]
        };

        self.taskModel.find(querySpec, function(err, items) {
        if (err) {
            throw err;
        }

        res.render('index', {
            title: 'My ToDo List ',
            tasks: items
        });
        });
    },

    addTask: function(req, res) {
        let self = this;
        let item = req.body;

        self.taskModel.addItem(item, function(err) {
        if (err) {
            throw err;
        }

        res.redirect('/');
        });
    },

    completeTask: function(req, res) {
        let self = this;
        let completedTasks = Object.keys(req.body);

        async.forEach(
        completedTasks,
        function taskIterator(completedTask, callback) {
            self.taskModel.updateItem(completedTask, function(err) {
            if (err) {
                callback(err);
            } else {
                callback(null);
            }
            });
        },
        function goHome(err) {
            if (err) {
            throw err;
            } else {
            res.redirect('/');
            }
        }
        );
    }
    };
    ```        
4. Mentse és zárja be a **tasklist.js** fájlt.

### <a name="add-configjs"></a>A config.js fájl hozzáadása
1. A projektkönyvtárban hozzon létre egy új fájlt **config.js** néven.
2. Adja hozzá a következőket a **config.js** fájlhoz. Ez meghatározza az alkalmazáshoz szükséges konfigurációs beállításokat és értékeket.
   
    ```nodejs
    let config = {}
   
    config.host = process.env.HOST || "[the URI value from the Azure Cosmos DB Keys page on http://portal.azure.com]";
    config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the Azure Cosmos DB Keys page on http://portal.azure.com]";
    config.databaseId = "ToDoList";
    config.collectionId = "Items";
   
    module.exports = config;
    ```
3. A **config.js** fájlban frissítse a HOST és az AUTH_KEY értékeket azokkal az értékekkel, amelyeket a [Microsoft Azure Portalon](https://portal.azure.com) lévő Azure Cosmos DB-fiókjának Kulcsok oldalán talál.
4. Mentse és zárja be a **config.js** fájlt.

### <a name="modify-appjs"></a>Az app.js fájl módosítása
1. A projekt könyvtárában nyissa meg az **app.js** fájlt. Ez a fájl korábban, az Express-webalkalmazás létrehozásakor jött létre.
2. Adja hozzá a következő kódot az **app.js** fájl elejéhez:
   
    ```nodejs
    var DocumentDBClient = require('documentdb').DocumentClient;
    var config = require('./config');
    var TaskList = require('./routes/tasklist');
    var TaskModel = require('./models/task-model');
    ```
3. Ez a kód fogja meghatározni a használni kívánt konfigurációs fájlt, és kiolvasni belőle az értékeket néhány változóhoz, amelyekre hamarosan szüksége lesz.
4. Cserélje ki az **app.js** fájl alábbi két sorát:
   
    ```nodejs
    app.use('/', index);
    app.use('/users', users); 
    ```
   
    a következő kódtöredékre:
   
    ```nodejs
    let docDbClient = new DocumentDBClient(config.host, {
        masterKey: config.authKey
    });
    let taskModel = new TaskModel(docDbClient, config.databaseId, config.collectionId);
    let taskList = new TaskList(taskModel);
    taskModel.init();
   
    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    app.set('view engine', 'jade');
    ```
5. Ezek a sorok meghatározzák a **TaskModel** objektum egy új példányát, amely egy új (a **config.js** fájlból kiolvasott értékek felhasználásával létesített) kapcsolattal csatlakozik az Azure Cosmos DB-adatbázishoz. Továbbá ezek inicializálják a feladatobjektumot, majd társítanak űrlapműveleteket a metódusokhoz a **TaskList**-vezérlőn. 
6. Végül mentse és zárja be az **app.js** fájlt. És már majdnem készen is van.

## <a name="_Toc395783181"></a>5. lépés: Felhasználói felület létrehozása
Most térjünk át a felhasználói felület létrehozására, hogy a felhasználók ténylegesen használatba vehessék az alkalmazást. A létrehozott Express-alkalmazás a **Jade** megjelenítési motort használja. A Jade motorral kapcsolatos további információkért lásd: [http://jade-lang.com/](http://jade-lang.com/).

1. A rendszer a **views** (nézetek) könyvtárban található **layout.jade** fájlt használja a többi **.jade** fájl globális sablonjaként. Ebben a lépésben ezt a sablont a [Twitter Bootstrap](https://github.com/twbs/bootstrap) eszközkészletre módosítja majd, amellyel könnyen tervezhet tetszetős webhelyeket. 
2. Nyissa meg a **views** (nézetek) mappában található **layout.jade** fájlt, és cserélje ki annak tartalmát a következőre:

    ```
    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body
        nav.navbar.navbar-inverse.navbar-fixed-top
          div.navbar-header
            a.navbar-brand(href='#') My Tasks
        block content
        script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
        script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
    ```

    Ez gyakorlatilag megmondja a **Jade** motornak, hogy rendereljen HTML-kódot az alkalmazás számára, és létrehoz egy **content** (tartalom) nevű **blokkot**, ahol megadhatja a tartalomoldalak elrendezését.

    Mentse és zárja be a **layout.jade** fájlt.

3. Most nyissa meg az **index.jade** fájlt, az alkalmazás által használt nézetet, és cserélje ki a fájl tartalmát az alábbira:
   
        extends layout
        block content
           h1 #{title}
           br
        
           form(action="/completetask", method="post")
             table.table.table-striped.table-bordered
               tr
                 td Name
                 td Category
                 td Date
                 td Complete
               if (typeof tasks === "undefined")
                 tr
                   td
               else
                 each task in tasks
                   tr
                     td #{task.name}
                     td #{task.category}
                     - var date  = new Date(task.date);
                     - var day   = date.getDate();
                     - var month = date.getMonth() + 1;
                     - var year  = date.getFullYear();
                     td #{month + "/" + day + "/" + year}
                     td
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn.btn-primary(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             .form-group
               label(for="name") Item Name:
               input.form-control(name="name", type="textbox")
             .form-group
               label(for="category") Item Category:
               input.form-control(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   

Ez kibővíti az elrendezést, és tartalmat biztosít a **layout.jade** fájlban az imént látott **content** (tartalom) helyőrző számára.
   
Ebben az elrendezésben két HTML-űrlapot hoztunk létre.

Az első űrlap az adatok táblázatát, valamint egy gombot tartalmaz, amely lehetővé teszi az elemek frissítését úgy, hogy elküldi azokat a vezérlő **/completetask** metódusának.
    
A második űrlap két beviteli mezőt és egy gombot tartalmaz, amely lehetővé teszi új elemek létrehozását úgy, hogy elküldi azokat a vezérlő **/addtask** metódusának.

Az alkalmazás működéséhez csak ennyire van szükség.

## <a name="_Toc395783181"></a>6. lépés: Az alkalmazás helyileg történő futtatása
1. Ha a helyi gépén szeretné tesztelni az alkalmazást, futtassa az `npm start` parancsot a terminálon az alkalmazás elindításához, majd frissítse a [http://localhost:3000](http://localhost:3000) böngészőoldalt. Az oldalnak most úgy kell kinéznie, ahogy az alábbi képen látható:
   
    ![Képernyőfelvétel a My Todo List (Saját teendőlista) alkalmazásról egy böngészőablakban](./media/sql-api-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > Ha olyan hibaüzenetet kap, amely a layout.jade fájlban vagy az index.jade fájlban lévő behúzásra vonatkozik, győződjön meg arról, hogy az első két sor mindkét fájlban balra zárt, és nem tartalmaz szóközt. Ha szóközök kerültek az első két sor elé, távolítsa el őket, mentse mindkét fájlt, és frissítse a böngészőablakot. 

2. Adjon meg egy új feladatot az Item (Elem), az Item Name (Elem neve) és a Category (Kategória) mezőkben, majd kattintson az **Add Item** (Elem hozzáadása) lehetőségre. Ez egy új dokumentumot hoz létre az Azure Cosmos DB-ben a megadott tulajdonságokkal. 
3. Az oldal ekkor frissül, és megjeleníti az újonnan létrehozott elemet a teendőlistában.
   
    ![Képernyőfelvétel az alkalmazásról és a teendőlista új eleméről](./media/sql-api-nodejs-application/cosmos-db-node-js-added-task.png)
4. A feladatok elvégzéséhez egyszerűen jelölje be a jelölőnégyzetet a Complete (Elvégezve) oszlopban, majd kattintson az **Update tasks** (Feladatok frissítése) lehetőségre. Ez frissíti a már létrehozott dokumentumot, és eltávolítja azt a nézetből.

5. Az alkalmazás leállításához nyomja le a CTRL+C billentyűkombinációt a terminálablakban, majd a kötegelt feladat leállításához kattintson az **Y** elemre.

## <a name="_Toc395783182"></a>7. lépés: Az alkalmazásfejlesztési projekt üzembe helyezése az Azure Websites-ban
1. Ha még nem tette meg, engedélyezzen egy Git-tárházat az Azure Websites számára. Ehhez a következő témakörben találhat útmutatót: [Local Git Deployment to Azure App Service](../app-service/app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).
2. Adja hozzá Azure-webhelyét távoli Git-elemként.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Helyezze üzembe a tárházat a távoli mappához küldéssel.
   
        git push azure master
4. Néhány másodpercen belül a Git befejezi a webalkalmazás közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-on futó munkáját!

    Gratulálunk! Létrehozta az első Node.js Express-webalkalmazását az Azure Cosmos DB használatával, és közzétette azt az Azure Websitesban.

    Az oktatóanyaghoz a teljes referenciaalkalmazás letölthető a [GitHubról][GitHub].

## <a name="_Toc395637775"></a>Következő lépések

* Méret- és teljesítménytesztelést szeretne végezni az Azure Cosmos DB használatával? Tekintse meg a következőt: [Teljesítmény- és mérettesztelés az Azure Cosmos DB használatával](performance-testing.md)
* Ismerje meg, hogyan [figyelhet egy Azure Cosmos DB-fiókot](monitor-accounts.md).
* Futtasson lekérdezéseket a minta-adatkészleteken a [Query Playground](https://www.documentdb.com/sql/demo) (Tesztlekérdezések) használatával.
* Tekintse át az [Azure Cosmos DB-dokumentációt](https://docs.microsoft.com/azure/cosmos-db/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app


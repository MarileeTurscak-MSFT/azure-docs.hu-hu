---
title: Rövid útmutató az Azure Redis Cache Node.js környezettel történő használatához | Microsoft Docs
description: Ebből a rövid útmutatóból megtudhatja, hogyan használhatja az Azure Redis Cache-t a Node.js és a node_redis alkalmazásával.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: quickstart
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/21/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 8f71feb610884af29bdfbf170cfc411f32c50233
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38700782"
---
# <a name="quickstart-how-to-use-azure-redis-cache-with-nodejs"></a>Rövid útmutató: Az Azure Redis Cache használata a Node.js környezettel



Az Azure Redis Cache hozzáférést biztosít egy biztonságos, dedikált Redis Cache gyorsítótárhoz, amelyet a Microsoft felügyel. A gyorsítótár a Microsoft Azure összes alkalmazásából elérhető.

Ez a témakör segítséget nyújt az első lépések megtételében az Azure Redis Cache használatakor Node.js környezetben. 

A rövid útmutató lépései bármilyen szövegszerkesztővel elvégezhetők. A [Visual Studio Code](https://code.visualstudio.com/) például jó választás lehet, és Windows, macOS és Linux platformokon is használható.

![Kész gyorsítótár-alkalmazás](./media/cache-nodejs-get-started/cache-app-complete.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Előfeltételek
Telepítse a [node_redis](https://github.com/mranney/node_redis) ügyfelet:

    npm install redis

Ez az oktatóanyag a [node_redis](https://github.com/mranney/node_redis) ügyfelet használja. Az egyéb Node.js-ügyfeleket használó példákért tekintse meg az egyes Node.js-ügyfelek dokumentációját a [Node.js Redis-ügyfeleket](http://redis.io/clients#nodejs) felsoroló weblapon.


## <a name="create-a-cache"></a>Gyorsítótár létrehozása
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-access-keys](../../includes/redis-cache-access-keys.md)]


Adjon hozzá környezeti változókat a **GAZDAGÉP NEVE** és **Elsődleges** hozzáférési kulcs megadásához. A kód ezen változóit fogja használni ahelyett, hogy közvetlenül a kódban tárolná a bizalmas információkat.

```
set REDISCACHEHOSTNAME=contosoCache.redis.cache.windows.net
set REDISCACHEKEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```


## <a name="connect-to-the-cache"></a>Csatlakozás a gyorsítótárhoz

A [node_redis](https://github.com/mranney/node_redis) legújabb buildjei támogatást nyújtanak az Azure Redis Cache-hez SSL használatával való kapcsolódáshoz. Az alábbi példa bemutatja, hogyan csatlakozhat az Azure Redis Cache-hez a 6380-as SSL-végpont használatával. 

```js
var redis = require("redis");

// Add your cache name and access key.
var client = redis.createClient(6380, process.env.REDISCACHEHOSTNAME,
    {auth_pass: process.env.REDISCACHEKEY, tls: {servername: process.env.REDISCACHEHOSTNAME}});
```

Ne hozzon létre új kapcsolatokat a kód minden műveletéhez. Ehelyett a lehető legtöbbször hasznosítsa újra a kapcsolatokat. 

## <a name="create-a-new-nodejs-app"></a>Új Node.js-alkalmazás létrehozása

Hozzon létre egy *redistest.js* nevű új szkriptfájlt.

Adja hozzá a következő példa JavaScriptet a fájlhoz. Ez a kód bemutatja, hogyan csatlakozhat egy Azure Redis Cache-példányhoz a gyorsítótár gazdagépének nevével és a legfontosabb környezeti változókkal. A kód emellett tárolja és lekéri gyorsítótár egyik sztringértékét. A rendszer a `PING` és a `CLIENT LIST` parancsot is végrehajtja. További példák a Redis használatára a [node_redis](https://github.com/mranney/node_redis) ügyféllel: [http://redis.js.org/](http://redis.js.org/).

```js
var redis = require("redis");
var bluebird = require("bluebird");

bluebird.promisifyAll(redis.RedisClient.prototype);
bluebird.promisifyAll(redis.Multi.prototype);

async function testCache() {

    // Connect to the Redis cache over the SSL port using the key.
    var cacheConnection = redis.createClient(6380, process.env.REDISCACHEHOSTNAME, 
        {auth_pass: process.env.REDISCACHEKEY, tls: {servername: process.env.REDISCACHEHOSTNAME}});
        
    // Perform cache operations using the cache connection object...

    // Simple PING command
    console.log("\nCache command: PING");
    console.log("Cache response : " + await cacheConnection.pingAsync());

    // Simple get and put of integral data types into the cache
    console.log("\nCache command: GET Message");
    console.log("Cache response : " + await cacheConnection.getAsync("Message"));    

    console.log("\nCache command: SET Message");
    console.log("Cache response : " + await cacheConnection.setAsync("Message",
        "Hello! The cache is working from Node.js!"));    

    // Demostrate "SET Message" executed as expected...
    console.log("\nCache command: GET Message");
    console.log("Cache response : " + await cacheConnection.getAsync("Message"));    

    // Get the client list, useful to see if connection list is growing...
    console.log("\nCache command: CLIENT LIST");
    console.log("Cache response : " + await cacheConnection.clientAsync("LIST"));    
}

testCache();
```

Futtassa a szkriptet a Node.js-sel.

```
node redistest.js
```

Az alábbi példában a `Message` kulcsot láthatja. A kulcsnak korábban gyorsítótárazott értéke volt, amely az Azure Portalon, a Redis Console használatával lett beállítva. Az alkalmazás frissítette ezt a gyorsítótárazott értéket. Az alkalmazás továbbá végrehajtotta a `PING` és a `CLIENT LIST` parancsot.

![Kész gyorsítótár-alkalmazás](./media/cache-nodejs-get-started/cache-app-complete.png)


## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha azt tervezi, hogy a következő oktatóanyaggal folytatja, megtarthatja és újból felhasználhatja az ebben a rövid útmutatóban létrehozott erőforrásokat.

Ha azonban befejezte az oktatóanyag mintaalkalmazásának használatát, a díjak elkerülése érdekében törölheti az ebben a rövid útmutatóban létrehozott Azure-erőforrásokat. 

> [!IMPORTANT]
> Az erőforráscsoport törlése nem vonható vissza; az erőforráscsoport és a benne foglalt erőforrások véglegesen törlődnek. Figyeljen arra, hogy ne töröljön véletlenül erőforráscsoportot vagy erőforrásokat. Ha a jelen minta üzemeltetését végző erőforrásokat egy meglévő, megtartani kívánt erőforrásokat tartalmazó erőforráscsoportban hozta létre, az erőforrásokat az erőforráscsoport törlése helyett külön-külön törölheti a megfelelő panelekről.
>

Jelentkezzen be az [Azure Portalra](https://portal.azure.com), és kattintson az **Erőforráscsoportok** elemre.

A **Szűrés név alapján...** mezőbe írja be az erőforráscsoport nevét. A jelen cikk utasításai egy *TestResources* nevű erőforráscsoportot használtak. Az eredménylistában kattintson a **…** ikonra az erőforráscsoport mellett, majd kattintson az **Erőforráscsoport törlése** elemre.

![Törlés](./media/cache-nodejs-get-started/cache-delete-resource-group.png)

A rendszer az erőforráscsoport törlésének megerősítését fogja kérni. A megerősítéshez írja be az erőforráscsoport nevét, és kattintson a **Törlés** gombra.

A rendszer néhány pillanaton belül törli az erőforráscsoportot és a benne foglalt erőforrásokat.



## <a name="next-steps"></a>További lépések

Ebből a rövid útmutatóból megtudhatta, hogyan használhatja az Azure Redis Cache-t egy Node.js-alkalmazásból. Folytassa a következő rövid útmutatóval a Redis Cache ASP.NET-webalkalmazással történő használatához.

> [!div class="nextstepaction"]
> [Egy Azure Redis Cache-t használó ASP.NET-webalkalmazás létrehozása](./cache-web-app-howto.md)




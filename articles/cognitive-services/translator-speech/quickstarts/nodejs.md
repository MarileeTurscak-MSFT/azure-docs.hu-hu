---
title: NODE.js gyors üzembe helyezés az Azure kognitív szolgáltatások esetében a Microsoft Translator Diktálásfelismerési API |} Microsoft Docs
description: Get információkat és a kód minták segítségével gyorsan Ismerkedés a Microsoft Translator Diktálásfelismerési API használatával a Microsoft Azure kognitív Services.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: d469fa008ba8acaf505fa09596dd739d5cc7744c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35347466"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-nodejs"></a>A Microsoft Translator Diktálásfelismerési API-t Node.js gyors üzembe helyezés 
<a name="HOLTop"></a>

Ez a cikk bemutatja, hogyan használja a Microsoft Translator Diktálásfelismerési API lefordítani a WAV-fájlokban szóbeli szavakat.

## <a name="prerequisites"></a>Előfeltételek

Szüksége [Node.js 6](https://nodejs.org/en/download/) futtatásához ezt a kódot.

Telepítenie kell a [Websocket csomag](https://www.npmjs.com/package/websocket) a Node.js.

A mappában, amelyben a végrehajtható fájl az alábbi kód fordítása "speak.wav" nevű WAV-fájl szükséges. A WAV-fájl szabványos PCM, 16 bites, 16 kHz Monó formátumban kell megadni. Ezt úgy szerezheti be ilyen .wav fájl a [fordító szöveg-beszéd API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

Rendelkeznie kell egy [kognitív szolgáltatások API-fiók](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) rendelkező **Microsoft Translator Diktálásfelismerési API**. A szolgáltatás fizetős kulcs van szüksége a [Azure irányítópult](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Beszéd fordítása

A következő kódot az eszköz beszédfelismerés egyik nyelvről a másikra.

1. Hozzon létre egy új Node.js-projektet a kedvenc ide.
2. Adja hozzá az alábbi kódot.
3. Cserélje le a `key` hívóbetű érvényes az előfizetéshez tartozó értéket.
4. Futtassa a programot.

```nodejs
/* To install this dependency, run:
npm install websocket
*/
var wsClient = require('websocket').client;
var fs = require('fs');
/* To install this dependency, run:
npm install stream-buffers
*/
var streamBuffers = require('stream-buffers');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let key = 'ENTER KEY HERE';

let host = 'wss://dev.microsofttranslator.com';
let path = '/speech/translate';
let params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa';
let uri = host + path + params;

/* The input .wav file is in PCM 16bit, 16kHz, mono format.
You can obtain such a .wav file using the Translator Text Speak API. See:
http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak
*/
let input_path = 'speak.wav';

let output_path = 'speak2.wav';

function receive(message) {
    console.log ("Received message of type: " + message.type + ".");

    if (message.type == 'utf8') {
        var result = JSON.parse(message.utf8Data)
        console.log("Response type: " + result.type + ".");
        console.log("Recognized input as: " + result.recognition);
        console.log("Translation: " + result.translation);
    }
    else if (message.type == 'binary') {
/* This is needed to make sure message.binaryData is defined. See:
https://stackoverflow.com/questions/17546953/cant-access-object-property-even-though-it-exists-returns-undefined
*/
        let binary_data = JSON.parse (JSON.stringify (message.binaryData));

        if (binary_data.type == 'Buffer') {
/* Put the binary data in a Buffer - do not write it directly to the file. */
            let buffer = new Buffer(binary_data.data, 'binary');
/* Write the contents of the Buffer to the file. */
            fs.writeFile (output_path, buffer, 'binary', function (error) {
/* fs.writeFile calls the error-handling function even if there is no error, in which case error === null. See:
https://stackoverflow.com/questions/44473868/node-js-fs-writefile-err-returns-null
*/
                if (error !== null) {
                    console.log ("Error: " + error);
                }
            });
        }
    }
}

function send(connection, filename) {
    
    var myReadableStreamBuffer = new streamBuffers.ReadableStreamBuffer({
        frequency: 100,
        chunkSize: 32000
    });

/* Make sure the audio file is followed by silence.
This lets the service know that the audio input is finished. */
    myReadableStreamBuffer.put(fs.readFileSync(filename));
    myReadableStreamBuffer.put(new Buffer(3200000));
    myReadableStreamBuffer.stop();

    myReadableStreamBuffer.on('data', function (data) {
        connection.sendBytes(data);
    });

    myReadableStreamBuffer.on('end', function () {
        console.log('Closing connection.');
        connection.close(1000);
    });
}

function connect() {
    var ws = new wsClient();

    ws.on('connectFailed', function (error) {
        console.log('Connection error: ' + error.toString());
    });
                            
    ws.on('connect', function (connection) {
        console.log('Connected.');

        connection.on('message', receive);
        
        connection.on('close', function (reasonCode, description) {
            console.log('Connection closed: ' + reasonCode);
        });

        connection.on('error', function (error) {
            console.log('Connection error: ' + error.toString());
        });
        
        send(connection, input_path);
    });

    ws.connect(uri, null, null, { 'Ocp-Apim-Subscription-Key' : key });
}

connect();
```

**Beszéd válasz fordítása**

Egy sikeres eredménye "speak2.wav" nevű fájl létrehozása. A fájl tartalmazza a fordítás szóbeli a "speak.wav" szó.

[Lap tetejére](#HOLTop)

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [A fordító beszéd oktatóanyag](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Lásd még 

[A fordító beszéd áttekintése](../overview.md)
[API-referencia](http://docs.microsofttranslator.com/speech-translate.html)

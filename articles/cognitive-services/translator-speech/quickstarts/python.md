---
title: Python gyors üzembe helyezés az Azure kognitív szolgáltatások esetében a Microsoft Translator Diktálásfelismerési API |} Microsoft Docs
description: Get információkat és a kód minták segítségével gyorsan Ismerkedés a Microsoft Translator Diktálásfelismerési API használatával a Microsoft Azure kognitív Services.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: 29722b3fa7fe61aff0b2406002453020d999ea41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35347411"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-python"></a>A Microsoft Translator Diktálásfelismerési API-t Python gyors üzembe helyezés 
<a name="HOLTop"></a>

Ez a cikk bemutatja, hogyan használja a Microsoft Translator Diktálásfelismerési API lefordítani a WAV-fájlokban szóbeli szavakat.

## <a name="prerequisites"></a>Előfeltételek

Szüksége lesz [Python 3.x](https://www.python.org/downloads/) futtatásához ezt a kódot.

Telepíteni kell a [websocket-ügyfélcsomag](https://pypi.python.org/pypi/websocket-client) a Python.

Szüksége lesz egy .wav fájlt "speak.wav" a végrehajtható fájl az alábbi kód fordítása ugyanabban a mappában. A WAV-fájl szabványos PCM, 16 bites, 16kHz Monó formátumban kell megadni. Ezt úgy szerezheti be ilyen .wav fájl a [fordító szöveg-beszéd API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

Rendelkeznie kell egy [kognitív szolgáltatások API-fiók](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) rendelkező **Microsoft Translator Diktálásfelismerési API**. Szüksége lesz egy fizetős kulcsot a [Azure irányítópult](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Beszéd fordítása

A következő kódot az eszköz beszédfelismerés egyik nyelvről a másikra.

1. A kedvenc IDE egy új Python-projekt létrehozása
2. Adja hozzá az alábbi kódot.
3. Cserélje le a `key` hívóbetű érvényes az előfizetéshez tartozó értéket.
4. Futtassa a programot.

```python
# -*- coding: utf-8 -*-

# To install this package, run:
#   pip install websocket-client
import websocket

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'wss://dev.microsofttranslator.com'
path = '/speech/translate';
params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa'
uri = host + path + params

input_file = 'speak.wav'
output_file = 'speak2.wav'

output = bytearray ()

def on_open (client):
    print ("Connected.")

# r = read. b = binary.
    with open (input_file, mode='rb') as file:
        data = file.read()

    print ("Sending audio.")
    client.send (data, websocket.ABNF.OPCODE_BINARY)
# Make sure the audio file is followed by silence.
# This lets the service know that the audio input is finished.
    print ("Sending silence.")
    client.send (bytearray (32000), websocket.ABNF.OPCODE_BINARY)

def on_data (client, message, message_type, is_last):
    global output
    if (websocket.ABNF.OPCODE_TEXT == message_type):
        print ("Received text data.")
        print (message)
# For some reason, we receive the data as type websocket.ABNF.OPCODE_CONT.
    elif (websocket.ABNF.OPCODE_BINARY == message_type or websocket.ABNF.OPCODE_CONT == message_type):
        print ("Received binary data.")
        print ("Is last? " + str(is_last))
        output = output + message
        if (True == is_last):
# w = write. b = binary.
            with open (output_file, mode='wb') as file:
                file.write (output)
                print ("Wrote data to output file.")
            client.close ()
    else:
        print ("Received data of type: " + str (message_type))

def on_error (client, error):
    print ("Connection error: " + str (error))

def on_close (client):
    print ("Connection closed.")

client = websocket.WebSocketApp(
    uri,
    header=[
        'Ocp-Apim-Subscription-Key: ' + key
    ],
    on_open=on_open,
    on_data=on_data,
    on_error=on_error,
    on_close=on_close
)

print ("Connecting...")
client.run_forever()
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

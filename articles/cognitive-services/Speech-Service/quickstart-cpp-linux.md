---
title: 'Gyors útmutató: Ismeri fel a beszéd, a C++, a Cognitive Services beszédfelismerő SDK használatával Linux rendszeren'
titleSuffix: Microsoft Cognitive Services
description: Ismerje meg, hogyan ismerhetik fel a beszéd, a C++ használatával a Cognitive Services beszédfelismerő SDK Linux rendszeren
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 92bd5980ac2e6befbe352df6ddf8644f04d37d34
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126865"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-using-the-speech-sdk"></a>Rövid útmutató: Ismeri fel a beszéd, a C++, a beszéd SDK használatával Linux rendszeren

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Ebből a cikkből elsajátíthatja, hogyan C++ Konzolalkalmazás létrehozása a linuxon (Ubuntu 16.04) alapuló átírás Beszédfelismerés a Cognitive Services beszédfelismerő SDK segítségével.

## <a name="prerequisites"></a>Előfeltételek

* A beszédfelismerési szolgáltatás egy előfizetési kulcsot. Lásd: [próbálja ki ingyenesen a speech service](get-started.md).
* Egy Ubuntu 16.04 működő mikrofon PC.
* Hozza létre és futtassa ezt a mintát, futtassa a következő szükséges csomagok telepítéséhez:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="get-the-speech-sdk"></a>A beszédfelismerés SDK beszerzése

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

A Cognitive Services beszédfelismerő SDK jelenlegi verziója `0.6.0`.

A Cognitive Services beszédfelismerő SDK Linux rendszeren érhető el a 64 bites és 32-bit-es alkalmazások létrehozásához.
A szükséges fájlokat a tar-fájlként letölthető https://aka.ms/csspeech/linuxbinary.
SDK letöltése és telepítése az alábbiak szerint:

1. Hol szeretné elhelyezni a Speech SDK bináris fájljait és a fejlécek válasszon egy könyvtárat (abszolút elérési utat).
   Például válassza ki az elérési út `speechsdk` a kezdőkönyvtár alatt:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Ha még nem létezik a következő könyvtár létrehozásakor:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Töltse le és csomagolja ki a `.tar.gz` a Speech SDK bináris fájljait az archívum:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. A kibontott csomagot, a legfelső szintű könyvtár tartalmának ellenőrzése:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Harmadik felekre vonatkozó megjegyzés és licencfájlok, kell megjelennie, valamint egy `include` fejlécek könyvtárat és a egy `lib` kódtárak címtárat.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-the-sample-code"></a>A mintakód hozzáadása

1. Adja hozzá a következő kód egy fájlt `helloworld.cpp`:

  [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Cserélje le a karakterláncot `YourSubscriptionKey` az előfizetési kulccsal végzett.

1. Cserélje le a karakterláncot `YourServiceRegion` együtt a [régió](regions.md) az előfizetéséhez tartozó (például `westus` az ingyenes próba-előfizetésre).

## <a name="building"></a>Létrehozás

> [!NOTE]
> Ügyeljen arra, hogy másolja és illessze be a build parancsok alább, egy _egysoros_.

* Az egy **x64** a számítógépen, az alkalmazás a következő parancsot:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Az egy **x86** a számítógépen, az alkalmazás a következő parancsot:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-sample"></a>Minta futtatása

1. A konfigurálás a betöltő könyvtárelérési utat átirányítása a Speech SDK-könyvtár konfigurálása.

   * Az egy **x64** gépen futtassa:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * Az egy **x86** gépen futtassa:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Futtassa az alkalmazást az alábbiak szerint:

   ```sh
   ./helloworld
   ```

1. Ehhez hasonló kimenetnek kell megjelennie:

   ```text
   Say something...
   We recognized: What's the weather
   ```

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Keresse meg az ehhez a mintához a `quickstart/cpp-linux` mappát.

## <a name="next-steps"></a>További lépések

* [A minták letöltése](speech-sdk.md#get-the-samples)

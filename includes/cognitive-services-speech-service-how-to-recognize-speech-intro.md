---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 3394fc2f6395799a252b645635d982b4573296c6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47000723"
---
<!-- N.B. no header, no intents here, language-agnostic -->

A Cognitive Services [beszéd SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) nyújt a használatának legegyszerűbb módja **Speech to Text** az összes funkciójával az alkalmazásban.

1. Hozzon létre egy beszéd-konfigurációt, és adja meg a Speech service előfizetési kulcs (vagy egy engedélyezési jogkivonatot) és a egy [régió](~/articles/cognitive-services/speech-service/regions.md) paraméterekként. Szükség szerint változtassa meg a konfigurációt. Például adjon meg egy egyéni végpontot, adjon meg egy nem szabványos végpontot, vagy válassza ki a beszélt nyelv vagy a kimeneti formátum.

1. Hozzon létre egy beszédfelismerő a speech-konfigurációból. Adjon meg egy hang-konfigurációt, ha azt szeretné, hogy ismeri fel az alapértelmezett mikrofon (például hang stream vagy hangfájl) eltérő forrásból.

1. Az események aszinkron művelethez lefoglalhatnak, ha szükséges. A felismerő ekkor meghívja az eseménykezelőket, átmeneti és végső eredményt, ha. Ellenkező esetben az alkalmazás csak a végleges átírást eredményt kap.

1. Indítsa el a felismerése. Egylépéses recognition, felismerés parancsot vagy lekérdezést, például használja a `RecognizeOnceAsync()` metódust. Ez a módszer az első felismert utterance (kifejezés) adja vissza. A hosszú ideig futó felismerés például beszédátírási, használja a `StartContinuousRecognitionAsync()` metódust. Az események aszinkron felismerési eredményeket lefoglalhatnak.

Tekintse meg az alábbi kódrészleteket speech recognition forgatókönyvek, amelyek a beszéd SDK-val.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]

---
title: Ismerkedés a Bing Speech Recognition API-val klienskódtárak használatával |} A Microsoft Docs
titlesuffix: Azure Cognitive Services
description: A Microsoft Cognitive Services a Bing Speech klienskódtárak használatával hozhat létre alkalmazásokat, amelyek a beszélt hangot képes szöveggé alakítani.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX
ms.openlocfilehash: a0fa11633efc610407755ebc109649f3fefdcb55
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585815"
---
# <a name="get-started-with-bing-speech-service-client-libraries"></a>Bing – beszédszolgáltatás klienskódtárak használatának első lépései

Amellett, hogy a REST API-n keresztül közvetlen HTTP-kérelem indítására, a Bing Speech Service nyújt a fejlesztőknek Speech klienskódtárak különböző nyelveken. A beszédfelismerés klienskódtárai:

- Támogatja a speciális képességek beszédfelismerés, például a valós idejű, mennyi ideig hang stream (legfeljebb 10 perc) és folyamatos felismerés köztes eredményeket.
- Adjon meg egy egyszerű és bármilyen API-t a prioritási nyelven.
- Az alsó szintű kommunikációs Részletek elrejtése.

Jelenleg a következő Bing Speech-ügyfélkódtárak érhetők el:

- [C# asztali könyvtár](GetStartedCSharpDesktop.md)
- [C# szolgáltatási kódtár](GetStartedCSharpServiceLibrary.md)
- [JavaScript-kódtár](GetStartedJSWebsockets.md)
- [Androidhoz készült Java-kódtár](GetStartedJavaAndroid.md)
- [IOS-es Objective-C-függvénytár](Get-Started-ObjectiveC-iOS.md)

> [!NOTE] 
2018 szeptember, az új óta [beszédszolgáltatás](../../speech-service/index.yml) általánosan elérhetővé vált. Javasoljuk, hogy [ingyenes kipróbálása](../../speech-service/get-started.md). 

## <a name="additional-resources"></a>További források

- A [minták](../samples.md) lap Speech-klienskódtárakkal teljes mintáit tartalmazza.
- Ha egy ügyféloldali kódtár, amely még nem támogatott van szüksége, létrehozhat saját SDK-t. Alkalmazzon a [Speech WebSocket protokoll](../API-Reference-REST/websocketprotocol.md) a platform és a tetszőleges nyelv használata.

## <a name="license"></a>Licenc

Az összes Cognitive Services SDK-k és minták az MIT-licenccel rendelkező rendelkezik licenccel. További információkért lásd: [licenc](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).


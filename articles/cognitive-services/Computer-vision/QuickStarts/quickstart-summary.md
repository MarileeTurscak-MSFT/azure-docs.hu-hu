---
title: A Computer Vision rövid útmutatóinak összefoglalása | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Ezekben a rövid útmutatókban képet elemezhet, miniatűrt hozhat létre, valamint nyomtatott és kézzel írott szöveget nyerhet ki a Computer Visionnel a Cognitive Servicesben.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 94424de3f175e82cf8490bad98f4a775761979e4
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770306"
---
# <a name="quickstart-summary"></a>Rövid útmutató: Összefoglalás

Ezek a rövid útmutatók információval és kódmintákkal szolgálnak, hogy megismerkedjen a Computer Vision használatának első lépéseivel.

A példa közvetlen HTTP-hívásokat indít a Computer Vision API felé. Az *SDK rövid útmutatókban* található példák a Computer Vision ügyfélkódtárait használják, amelyek egyszerűsített metódusokat biztosítanak a HTTP-hívások burkolására.

A Computer Vision API-kkal való gyors kísérletezéshez próbálja ki az [Open API-tesztkonzolt](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

A Computer Vision szolgáltatást az alábbi feladatok elvégzésére használhatja:

* Távoli rendszerkép elemzése
* Helyi rendszerkép elemzése
* Hírességek és nevezetességek felismerése (tartománymodellek)
* Miniatűr intelligens létrehozása
* Nyomtatott szöveg felismerése és kinyerése (OCR) képekből
* Kézzel írt szöveg felismerése és kinyerése képekből

A példákban használt kódok hasonlóak. Azonban a Computer Vision különböző funkcióit emelik ki, valamint különböző technikákat a szolgáltatással való adatcseréhez, úgymint:

* A _miniatűr létrehozása_ bájttömbként ad vissza egy képet a válasz törzsében.
* A _helyi kép elemzéséhez_ a kérésnek bájttömbként tartalmaznia kell a képet.
* A _kézzel írt szöveg kinyeréséhez_ két hívás szükséges.

## <a name="summary"></a>Összegzés

| Első lépések               | A kérés paraméterei                          | Válasz          |
| ------------------------ | ------------------------------------------- | ----------------  |
| Távoli rendszerkép elemzése   | visualFeatures=Categories,Description,Color | JSON-sztring       |
| Helyi rendszerkép elemzése    | data=image_data (byte array)                | JSON-sztring       |
| Hírességek felismerése       | model=celebrities                           | JSON-sztring       |
| Miniatűr létrehozása     | width=200&height=150&smartCropping=true     | bájttömb        |
| Nyomtatott szöveg kinyerése     | language=unk&detectOrientation=true         | JSON-sztring       |
| Kézzel írt szöveg kinyerése | handwriting=true                            | URL-cím, JSON-sztring  |

## <a name="next-steps"></a>További lépések

Ismerje meg a Computer Vision API-kat, amelyekkel képeket elemezhet, hírességeket és nevezetességeket azonosíthat rajtuk, valamint miniatűrt hozhat létre, illetve nyomtatott és kézzel írott szövegeket nyerhet ki belőlük.

> [!div class="nextstepaction"]
> [Ismerkedjen meg a Computer Vision API-k működésével](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

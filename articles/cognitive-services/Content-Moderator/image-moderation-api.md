---
title: Lemezkép - moderálás a Content Moderator
titlesuffix: Azure Cognitive Services
description: Közepes kívánatos képek képmoderálás használata
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: 6c5fed78c67f974a2af11efd133e9a79ec52124b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219651"
---
# <a name="image-moderation"></a>Képmoderálás

Használja a Content Moderator a gépi támogatású képmoderálás és [emberi vizsgálóeszköz](Review-Tool-User-Guide/human-in-the-loop.md) képek a felnőtt és szexuális tartalom mérsékelt. Szöveges tartalom kiszűrhető a szöveg kinyerése és arcok észlelése. Megfelelő lemezképek egyéni listájával, és további műveleteket végrehajtania.

## <a name="evaluating-for-adult-and-racy-content"></a>A felnőtt és szexuális tartalom kiértékelése

A **Evaluate** művelet 0 és 1 közötti konfidencia-pontszám adja vissza. Emellett adja vissza, logikai adatok egyenlő true vagy FALSE (hamis). Ezek az értékek előre jelezni, hogy a rendszerkép tartalmazza-e a potenciális felnőtt vagy pikáns tartalmak. Az API hívásakor a lemezképpel (a fájl vagy URL-cím) a visszaadott válasz tartalmazza a következő információkat:

    "ImageModeration": {
      .............
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      ............
      ],

> [!NOTE]

> - `isImageAdultClassified` a rendszerkép található, előfordulhat, hogy figyelembe kell venni, nyíltan explicit vagy bizonyos esetekben felnőtt lehetséges jelenléte jelöli.
> - `isImageRacyClassified` a rendszerkép található, előfordulhat, hogy figyelembe kell venni, nyíltan kétértelmű vagy bizonyos esetekben érett lehetséges jelenléte jelöli.
> - A pontszámok 0 és 1 között. Minél nagyobb a pontszám, annál nagyobb a modell becslése, hogy a kategóriához lehet alkalmazni. Ebben az előzetes verzióban manuálisan kódolt kimenetek helyett egy statisztikai modellt használ. Azt javasoljuk, hogy a saját határozza meg, hogyan az egyes kategóriák az igényeinek megfelelően igazítja a tartalmat a teszteléshez.
> - A logikai értékek a következők: IGAZ vagy hamis attól függően, a belső pontszám küszöbértékeket. Ügyfelek fel kell mérnie, hogy e használja ezt az értéket, vagy hozza meg, a tartalom házirendek alapján egyedi küszöbértékeket.
>

## <a name="detecting-text-with-optical-character-recognition-ocr"></a>Az optikai karakterfelismerés (OCR) szöveg észlelése

A **optikai karakterfelismerés (OCR)** művelet előrejelzi a képet a szöveges tartalom jelenlétét, és kinyeri a szöveg moderálása, egyebek mellett a. A nyelv is megadhat. Ha nem adja meg a nyelvet, az észlelés alapértelmezés szerint angol.

A válasz tartalmazza a következő információkat:
- Az eredeti szöveg.
- Az észlelt szöveges elemek a megbízhatósági eredményeiket.

Példa kivonat:

    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      .........
      "language": "eng",
      "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
      "candidates": []
    },


## <a name="detecting-faces"></a>Arcfelismerés

Arcok észlelése segít észlelni a személyes azonosításra alkalmas adatokat (PII) például az arcok a képeken. Az egyes rendszerképek észlelése lehetséges arcokat és a lehetséges arcok számát.

A válasz ezeket az információkat tartalmazza:

- Arcok száma
- Felismert arc-helyek listája

Példa kivonat:


    "FaceDetection": {
       ......
      "result": true,
      "count": 2,
      "advancedInfo": [
      .....
      ],
      "faces": [
        {
          "bottom": 598,
          "left": 44,
          "right": 268,
          "top": 374
        },
        {
          "bottom": 620,
          "left": 308,
          "right": 532,
          "top": 396
        }
      ]
    }

## <a name="creating-and-managing-custom-lists"></a>Létrehozásába és kezelésébe az egyedi listákkal

Számos online Közösség Miután a felhasználók képeket vagy egyéb típusú tartalmak feltöltése sértő elemek előfordulhat, hogy első többször keresztül megosztott a következő napokon, hetek vagy hónapok. Ismételt vizsgálata és kiszűri a ugyanazt a lemezképet, vagy több helyen a lemezképet még kis mértékben módosított verzióját, a költségek költséges és sok hibalehetőséget rejtő lehet.

Ahelyett, hogy ugyanazt a lemezképet többször moderálása, hozzáadja a sértő képek a listán szereplő letiltott tartalom. Ezzel a módszerrel a tartalom-jóváhagyás rendszer összeveti az egyedi listákkal bejövő lemezképek hasonlítja össze, és leállítja a további feldolgozás.

> [!NOTE]
> A maximális korlát **5 kép listák** az egyes lista **nem haladhatja meg a 10 000 lemezképek**.
>

A Content Moderator biztosít teljes [Image List felügyeleti API](try-image-list-api.md) műveletekkel egyéni rendszerképek listájának kezeléséhez. Kezdje a [kép sorolja fel API-konzol](try-image-list-api.md) , és a REST API-Kódminták. Emellett olvassa el a [kép lista .NET – rövid útmutató](image-lists-quickstart-dotnet.md) Ha ismeri a Visual Studio és C#.

## <a name="matching-against-your-custom-lists"></a>Egyező összeveti az egyedi listákkal

A Match művelet lehetővé teszi, hogy az egyedi listákkal, létrehozni és felügyelni a listázási műveletek használatával bármely bejövő képek intelligens megfelelő.

Ha van egyezés, a művelet az azonosító és a moderálás címkék a megfelelést kiváltó kép adja vissza. A válasz ezeket az információkat tartalmazza:

- Egyeztetés pontszámát (0 és 1) között
- Egyeztetett kép
- Címkéket (előző moderálás során hozzárendelt)
- Kép címkék

Példa kivonat:

    {
    ..............,
    "IsMatch": true,
    "Matches": [
        {
            "Score": 1.0,
            "MatchId": 169490,
            "Source": "169642",
            "Tags": [],
            "Label": "Sports"
        }
    ],
    ....
    }

## <a name="human-review-tool"></a>Emberi vizsgálóeszköz

További precízebb, használja a Content Moderator [vizsgálóeszköz](Review-Tool-User-Guide/human-in-the-loop.md) és annak API-t a az emberi moderátorok vonatkozó felülvizsgálat tartalmának és a moderálás eredmények surface. Tekintse át a számítógéphez hozzárendelt címkék ezeket, és erősítse meg a végső döntéshozatal.

![Az emberi moderátorok kép áttekintése](images/moderation-reviews-quickstart-dotnet.PNG)

## <a name="next-steps"></a>További lépések

Próbálja ki a [kép moderálási API-konzol](try-image-api.md) , és a REST API-Kódminták. Emellett olvassa el a [kép moderálás .NET – rövid útmutató](image-moderation-quickstart-dotnet.md) Ha ismeri a Visual Studio és C#.

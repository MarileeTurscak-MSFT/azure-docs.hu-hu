---
title: Az Azure media intelligence |} A Microsoft Docs
description: Az Azure Media Services használata esetén az audio- és contnet AudioAnalyzerPreset és VideoAnalyzerPreset használatával elemezhetők.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/24/2018
ms.author: juliako
ms.openlocfilehash: a428f76f1239e7e67b99d05b96d26abd601e89c6
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498691"
---
# <a name="media-intelligence"></a>Médiaintelligencia

Az Azure Media Services REST v3 API lehetővé teszi hang- és tartalmakat elemezhet. A tartalom elemzéséhez, hozzon létre egy **átalakítása** , és küldje el egy **feladat** , hogy ezen készletek egyikét használja: **AudioAnalyzerPreset** vagy **VideoAnalyzerPreset** . 

## <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

**AudioAnalyzerPreset** lehetővé teszi, hogy egy hang- vagy fájlból származó több hang információk kinyerése. A kimenet egy JSON-fájlt (az összes az insights) és a hang átiratok VTT fájlt tartalmazza. Ezt a beállítást olyan tulajdonságot, amely meghatározza a bemeneti fájl nyelvét formájában fogad el egy [BCP47](https://tools.ietf.org/html/bcp47) karakterlánc. A hang insights tartalmazza:

* Hanganyag átírása – egy szöveges időbélyegzőnél kimondott szó. Több nyelvet támogat
* Hangszóró indexelő – előadó és a megfelelő szövegrészeket és szavakat leképezése
* Beszéd hangulatelemzés – hangulatelemzést végzett a hanganyag átírása kimenete
* Kulcsszavak – a hanganyag átírása kinyert kulcsszavakat.

## <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

**VideoAnalyzerPreset** lehetővé teszi elemzési adatokat nyerhet több audio- és video-fájlból. A kimenet egy JSON-fájlt (az összes az insights), a videóátiratot, és a egy gyűjtemény miniatűrök VTT fájlt tartalmazza. Ezt a beállítást is fogad egy [BCP47](https://tools.ietf.org/html/bcp47) tulajdonságként (a videó nyelvének képviselő) karakterláncot. A videó insights az összes fent említett hang insights és az alábbi elemeket tartalmazza:

* Face követési – az idő, mely során arcok jelen a videóban. Minden rendelkezik, a face id és a egy megfelelő gyűjtemény miniatűrök
* Vizuális szöveg – a szöveg, optikai karakterfelismerés keresztül észlelt. A szöveg megjelölve, és segítségével kulcsszavakkal (mellett az audio átiratot) is
* Kulcsképek – a videó kinyert kulcsképek gyűjteménye
* Vizuális tartalom-jóváhagyás – a felnőtt vagy pikáns jellegű megjelölt videókat része
* Jegyzet – alapján egy előre meghatározott hálózatiobjektum-modellt a videók jegyzetkészítés eredménye

##  <a name="insightsjson-elements"></a>insights.JSON elemek

A parancs kimenete egy JSON-fájlt (insights.json) észlelt a rendszer a videó vagy hang összes betekintést. A json a következő elemeket tartalmazza:

### <a name="transcript"></a>a szövegben

|Name (Név)|Leírás|
|---|---|
|id|A sor azonosítója.|
|szöveg|Az átirat magát.|
|Nyelv|A szöveges nyelvet. Szánt támogatásához, átirat, ahol az egyes sorok rendelkezhet egy másik nyelvet.|
|példányok|Amikor ezt a sort jelent meg időt a tartományok listája. Ha a példány a szövegben, 1 példánnyal fog rendelkezni.|

Példa:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

### <a name="ocr"></a>optikai karakterfelismerés

|Name (Név)|Leírás|
|---|---|
|id|Az optikai Karakterfelismerés sor azonosítója.|
|szöveg|Az optikai Karakterfelismerés szöveg.|
|magabiztosan|Elismerés magabiztosan.|
|Nyelv|Az optikai Karakterfelismerés nyelv.|
|példányok|Amikor jelent meg az optikai Karakterfelismeréssel időt tartományok listája (az azonos OCR többször is megjelenhetnek).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

### <a name="faces"></a>arcok

|Name (Név)|Leírás|
|---|---|
|id|A face azonosítója.|
|név|A face neve. "Ismeretlen #0", az azonosított hírességek vagy ügyfél betanított személy is lehet.|
|magabiztosan|Arcok azonosítása magabiztosan.|
|leírás|A hírességek leírása. |
|thumbnalId|A miniatűr képét, arc azonosítója.|
|knownPersonId|Ha ez egy ismert személy, a belső azonosítója.|
|a referenceid megadása|Ha a Bing hírességek, a Bing-azonosító.|
|referenceType|Jelenleg csak a Bing.|
|cím|Ha egy hírességek, akkor annak címe (például: "a Microsoft Vezérigazgatójával").|
|imageUrl|Ha a hírességek, a kép URL-címe.|
|példányok|Ezek a példányok, ahol az arcfelismerés jelent meg a megadott időtartományban. Minden példány egy thumbnailsId is tartalmaz. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

### <a name="shots"></a>helyességének

|Name (Név)|Leírás|
|---|---|
|id|A képernyőkép-azonosítót.|
|Kulcsképek|Kulcs keretek belül a képernyőkép-készítés (megvannak-azonosító és példányok idő tartományok listája) listája. Kulcs keretek példányok rendelkeznek egy thumbnailId mezőt, a Kulcsképkocka miniatűr-azonosítóját.|
|példányok|A képernyőkép-készítés ideje tartományainak listáját (a helyességének 1 példánnyal rendelkezik).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
                "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
            "thumbnailId": "00000000-0000-0000-0000-000000000000",  
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
                "thumbnailId": "00000000-0000-0000-0000-000000000000",      
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
      "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

### <a name="statistics"></a>statisztika

|Name (Név)|Leírás|
|---|---|
|CorrespondenceCount|A videóban megfelelések száma.|
|WordCount|A speaker kiszolgálónként szavak számát.|
|SpeakerNumberOfFragments|A videó szerepel a beszélő töredék mennyisége.|
|SpeakerLongestMonolog|A beszélő leghosszabb monolog. Ha a beszélő belül a monolog silences része. Csend elején és végén a monolog törlődik.| 
|SpeakerTalkToListenRatio|A számítás időt a speaker monolog (nélkül a csend köztes) elosztva a videó teljes időtartama alapján. Az idő a harmadik tizedesvesszőtől lesz kerekítve.|


### <a name="sentiments"></a>hangulati

Összesítjük hangulati azok sentimentType mező (pozitív/Neutral/negatív) szerint. Például, 0 – 0.1, 0.2-0.1-es.

|Name (Név)|Leírás|
|---|---|
|id|A róluk szóló véleményeket azonosítója.|
|Átlag |Az összes pontszámok szoftverpéldányok száma a róluk szóló véleményeket típusa – pozitív/Neutral/negatív átlaga|
|példányok|Amikor a véleményt jelent meg időt a tartományok listája.|
|sentimentType |A típus "Pozitív", "Semleges" vagy "Negatív" lehet.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

### <a name="labels"></a>címkék

|Name (Név)|Leírás|
|---|---|
|id|A címke azonosítója.|
|név|A címke nevét (például "Számítógép", "TV").|
|Nyelv|A címke neve nyelv, (Ha a fordítás melyik változatot). BCP-47|
|példányok|Amikor ezt a címkét jelent meg időt a tartományok listája (címkék többször is megjelenhetnek). Minden példány egy megbízhatósági mező tartozik. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

### <a name="keywords"></a>a kulcsszavak

|Name (Név)|Leírás|
|---|---|
|id|A kulcsszó-azonosítót.|
|szöveg|A kulcsszó szöveg.|
|magabiztosan|A kulcsszó felismerés magabiztosan.|
|Nyelv|A kulcsszó nyelv (Ha a fordítás melyik változatot).|
|példányok|Amikor ezt a kulcsszót jelent meg időt a tartományok listája (kulcsszó többször is megjelenhetnek).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

A visualContentModeration blokk tartalmaz idő-címtartományokat, amelyek a Video Indexer, potenciálisan rendelkezik a felnőtt tartalom található. Ha visualContentModeration üres, nincs nincs felnőtt tartalom azonosított.

Lehet, hogy videókat, amelyek tartalmazzák a felnőtt vagy pikáns tartalmak csak a privát nézetben érhető el. Felhasználók van arra, hogy a tartalom, amelyben a IsAdult attribútum eset tartalmazni fogja az emberi ellenőrző eredménye egy emberi vizsgálóeszközt igényelnie.

|Name (Név)|Leírás|
|---|---|
|id|A vizuális tartalom-jóváhagyás azonosítója.|
|adultScore|A felnőtt pontszám (a content moderator).|
|racyScore|A pikáns pontszám (a tartalom-jóváhagyás).|
|példányok|Amikor jelent meg a vizuális tartalom-jóváhagyás időtartományok listája.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```
## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [Oktatóanyag: Az Azure Media Services videók elemzése](analyze-videos-tutorial-with-api.md)

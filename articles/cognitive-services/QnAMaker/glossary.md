---
title: Szószedet – QnA Maker
titleSuffix: Azure Cognitive Services
description: Szószedet
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: b22ec27b2999d322945e37c5a38d2b1d1532e7e3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166044"
---
# <a name="glossary"></a>Szószedet

## <a name="qna-maker-service"></a>A QnA Maker szolgáltatás
A QnA Maker szolgáltatás olyan olyan előfeltételt, QnA Maker használatához. A QnA Maker csomag vásárlása beállítja az erőforrások létrehozására és kezelésére a Tudásbázis Azure-előfizetésében. A QnA Maker felhasználói fiókokat hozhat létre több QnA Maker szolgáltatás Azure-előfizetésében.

## <a name="knowledge-base"></a>Tudásbázis
Tudásbázis az adattár kérdések, valamint választ ad a, karbantartani, és a QnA Maker keresztül használt. Az egyes QnA Maker szintek több tudásbázisok használható.

## <a name="endpoint"></a>Végpont
A REST-alapú HTTP-végpontot a Tudásbázis-tartalmat, amely integrálható az alkalmazásba, általában egy csevegőrobot karbantartás. 

## <a name="test-knowledge-base"></a>Tudásbázis tesztelése
Tudásbázis kétállapotú - teszt rendelkezik, és közzé. A teszt Tudásbázis verziója szerkesztett mentett és tesztelt, a pontosság és a válaszok teljességéről folyamatban van. A teszt Tudásbázisban végzett módosítások nem érintik a végfelhasználó számára az alkalmazás-/ csevegőrobot.

## <a name="published-knowledge-base"></a>Közzétett Tudásbázis
Tudásbázis kétállapotú - vizsgálat és egy közzétett rendelkezik.  A közzétett Tudásbázis verziója, amely a csevegési robot vagy alkalmazás használatban van. Tudásbázis közzététele művelet a teszt Tudásbázis tartalmát a Tudásbázis közzétett verziójának helyezi. Mivel a közzétett Tudásbázis a verzió, az alkalmazás által használt a végponton keresztül, annak érdekében, hogy a tartalom-e a megfelelő és tesztelt gondot kell fordítani.

## <a name="query"></a>Lekérdezés
Felhasználói lekérdezés, amely rákérdez, a végfelhasználói vagy tesztelőként a kérdést, a Tudásbázis. A lekérdezés gyakran egy természetes nyelvi formátumban vagy a kérdés képviselő néhány kulcsszavak nem.

## <a name="response"></a>Válasz
A válasz a válasz veszi át a Tudásbázis következő, a legmegfelelőbb az adott felhasználó lekérdezés alapján.

## <a name="confidence-score"></a>Megbízhatósági pontszám
A megbízhatósági pontszám a választ, akkor egy numerikus értéket 0 és 100, folyamatban van egy pontos lekérdezés egyeznek felhasználói lekérdezés és a egy Tudásbázis, amely a válasz kiszolgált kérdést 100 között a helyes, a megfelelő választ az egy adott felhasználó lekérdezés. Válaszok általában a megbízhatósági pontszám szerinti sorrendben, és a magasabb megbízhatósági pontszám rendelkezőt biztosítja az alapértelmezett válaszként.

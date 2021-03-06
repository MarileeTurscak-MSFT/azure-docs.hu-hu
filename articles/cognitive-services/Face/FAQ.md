---
title: Gyakori kérdések – Face API
titlesuffix: Azure Cognitive Services
description: Az alábbiakban a Face API szolgáltatás legnépszerűbb kérdésekre adott válaszok.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: 9b30fa0fbbd655c03800dadb19cc2568d404204d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129556"
---
# <a name="face-api-frequently-asked-questions"></a>A Face API – gyakori kérdések

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Ha nem találja a kérdésekre adott válaszok a gyakori kérdések, próbálja meg, forduljon a Face API közösségéhez a [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) , vagy forduljon a Súgó és támogatás a [UserVoice](https://cognitive.uservoice.com/).

-----
**Kérdés**: milyen tényezők csökkentheti a Face API pontossága Recognition, ellenőrző vagy hasonló?

**Válasz**: általában az azonos esetekben, amikor az emberek azonosítása valaki; például nehéz
* Akadályok egyik vagy mindkét szemet blokkolása
* Háttérvilágítás szélsőséges világító, például: súlyos
* Stílus és videofelismerést, arc haj haj módosításai
* Módosítások miatt kora
* Szélsőséges arckifejezések (pl. screaming)

A Face API gyakran ilyen esetekben kihívást sikeres, de pontossága csökkenthető. Elismerési robusztusabb és ezek a kihívások megoldása, a fényképeket, amelyek tartalmazzák a sokféle szögek és megvilágítási rendelkező személy betanításához.

-----
**Kérdés**: I vagyok átadja a bináris adatok, de a "érvénytelen arcképét" hibaüzenetet kapok.

**Válasz**: Ez azt jelenti, hogy az algoritmus a lemezkép-elemzés hiba történt. Okok a következők:
* A bemeneti formátum támogatott JPEG, PNG, (az első képkocka) GIF és BMP tartalmazza.
* Kép fájlmérete kell nem lehet nagyobb, mint 4MB
* Az észlelhető oldallal mérete tartománya 36 x 36, 4096 x 4096 képpont. A tartományon arcok észlelése nem történik meg
* Néhány arcok észlelése helytelen lehet kihívások, például a nagyon nagy face szög (head-testtartás), mert nagy hangelnyelés. Elülső és a közel-elülső arcok rendelkezik a legjobb eredmények elérése érdekében

-----

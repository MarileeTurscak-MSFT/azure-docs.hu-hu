---
title: Beszélgetés Learner alapértelmezett konfiguráció – a Microsoft Cognitive Services |} A Microsoft Docs
titleSuffix: Azure
description: Ismerje meg az alapértelmezett Beszélgetéstanuló konfigurációt.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: b048509f38356b19d52507f4dfb5bfa61122f9f6
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576425"
---
# <a name="default-values-and-boundaries"></a>Alapértelmezett értékek és határok

Ez a dokumentum ismerteti az alapértelmezett konfiguráció Beszélgetéstanuló és a kulcs a szolgáltatás határain.

## <a name="default-configuration"></a>Alapértelmezett konfiguráció

Paraméter | Alapértelmezett érték
--- | --- 
Alapértelmezett munkamenet időkorlátja | 30 perc

## <a name="boundaries"></a>Határok

Paraméter | Korlát
--- | --- 
Szerzői API, maximális HTTP hívások / hó | 5M
Szerzői API, maximális HTTP-hívások | 25
Munkamenet API-t, a maximális HTTP-hívások / hó | 500 000
Munkamenet API-t, a maximális HTTP-hívások / másodperc | 10
Egyéni (nem programozott) entitás modell / maximális száma | Lásd: [LUIS határok doc](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries); a gyakorlatban, tényleges szám valamivel kisebb is lehet.
Egyes modellek előre összeállított entitások maximális száma | Lásd: [LUIS határok doc](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries)
Entitások (összesen) egyes modellek maximális száma | 100
Egyes modellek műveletek maximális száma | 32
Egyes modellek train párbeszédpanelek maximális száma | 1000
Felhasználók maximális száma fordulat / train párbeszédpanel | 100
Egyes modellek log párbeszédpanelek maximális száma | Nincs előre beállított korlát, de a napló párbeszédpanelek csak megmaradnak megsemmisülne fix időszakra.  A beszélgetés Learner felhasználói felület ezenkívül megjelenítése a 100 log párbeszédpanelek egyszerre. 
Modellek felhasználónkénti maximális száma | Nincs előre beállított korlát
Nem várakozási soros műveletek maximális száma | 5 (*)

(*) 5 egymást követő nem várakozási művelet után minden nem várakozási művelet maszkolva, és Beszélgetéstanuló elérhető várakozási műveletek közül fog választani.

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [Beszélgetéstanuló használatának első lépései](./quickstart.md)

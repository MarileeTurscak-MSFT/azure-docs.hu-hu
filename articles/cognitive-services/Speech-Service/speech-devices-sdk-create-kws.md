---
title: Hozzon létre egy egyéni ébresztési word
description: Ismerje meg, hogyan hozzon létre egy egyéni ébresztési szót a Speech Devices SDK-val.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 5b5a13c5a655260bde0496cb2289aec8a55e4b21
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/02/2018
ms.locfileid: "48017037"
---
# <a name="create-a-custom-wake-word-by-using-the-speech-service"></a>Hozzon létre egy egyéni ébresztési szót a Speech szolgáltatással

Az eszköz mindig figyeli a hálózati ébresztési szó (vagy kifejezés). Például a "Hey Cortana" a Cortana Segéd az ébresztési szó lesz. Amikor a felhasználó szöveget az ébresztési a word, az eszköz küld minden későbbi hang a felhőbe, mindaddig, amíg a felhasználó leállítja a különféle. Hatékony módja az eszköz megkülönböztetéséhez és erősítse a márkajelzési beállításokat az ébresztési word testreszabása.

Ebből a cikkből megismerheti, hogyan hozhat létre egy egyéni ébresztési word, az eszköz.

## <a name="choose-an-effective-wake-word"></a>Válasszon egy érvényes ébresztési word

Ha úgy dönt, hogy ébresztési szót, ügyeljen a következőkre:

* Az ébresztési word egy angol nyelvű szót vagy kifejezést kell lennie. Érdemes igénybe legfeljebb két másodpercen övezi.

* A 4-7 szótagokat határoznak szavak működnek a legjobban. Például a "Hey, számítógép" egy jó ébresztési szót. Csak a "Hey", adott gyenge.

* Ébresztési szavak közös angol nyelvű írásmódja szabályok kövessék.

* Egy egyedi vagy akár egy közös angol nyelvű írásmódja szabályok a következő feldolgozott szó előfordulhat, hogy csökkenti a vakriasztások. Például a "computerama" egy jó ébresztési szó lehet.

* Ne adja meg egy gyakori szót. Például "étkezési" és "go" olyan szavak, amelyek az emberek gyakran tegyük fel, a szokásos beszélgetésekben. Az eszköz hamis eseményindítók lehetnek.

* Ne használja, előfordulhat, hogy egyéb kiejtés ébresztési szót. Felhasználók a saját eszköz válaszol "megfelelő" kiejtés ismernie kell. Ha például olyan "509" is ejtsd, "öt nulla nine," "öt hoppá kilenc," vagy "öt száz és kilenc." "R.E.I." "az r-e-i" vagy "ray." ejtsd is Is lehet ejtsd a "Live", "/līv/" vagy "/liv/".

* Ne használja a speciális karaktereket, szimbólumok és számjegy. Például "Go #" és "20 + macskák" nem lenne jó ébresztési szavakat. Azonban "Ugrás éles" vagy "húsz macskák plusz" működhetnek. Továbbra is a szimbólumok használata a márkajelzési beállításokat, és marketing- és dokumentáció segítségével a megfelelő kiejtés megerősítése.

> [!NOTE]
> Ha az ébresztési Word Védjegyezett szót, győződjön meg, hogy Ön a tulajdonosa, a védjegyekre, illetve, hogy Ön jogosult a védjegy tulajdonosától a Word alkalmazást. A Microsoft nem vállal jogi Ön választhatja ki a hálózati ébresztési word esetlegesen felmerülő problémákat.

## <a name="create-your-wake-word"></a>Az ébresztési word létrehozása

Egy egyéni ébresztési szót az eszköz használata előtt létre kell hoznia az ébresztési szó a Microsoft egyéni ébresztési Word létrehozó szolgáltatások használatával. Miután megadta a ébresztési word-, a szolgáltatás állít elő a fájlnevet, amely a szoftverfejlesztői készlet ahhoz, hogy az ébresztési word, az eszközön telepítheti.

1. Nyissa meg a [Custom Speech service portal](https://cris.ai/).

1. Hozzon létre egy új fiókot, amellyel az Azure Active Directory a meghívót kapott e-mail-címmel. 

    ![Új fiók létrehozása](media/speech-devices-sdk/wake-word-1.png)
 
1.  Miután bejelentkezett, töltse ki az űrlapot, és válassza ki **elkezdheti saját**.

    ![sikeresen jelentkezett be](media/speech-devices-sdk/wake-word-3.png)
 
1. A **egyéni ébresztési Word** lap nem érhető el nyilvánosan, így nem áll fenn, amely vesz igénybe, hogy közvetlen kapcsolat. A Custom Speech funkcióhoz egy Azure-előfizetések, de nem az egyéni ébresztési Word funkció. Ha kapott a **nem találhatók előfizetések.** hibalap, csak cserélje le a **"előfizetések? errorMessage nincs % 20Subscriptions % 20found ="** az "**customkws**" a URL-cím és a találatok adjon meg. Az URL-cím ezek egyike lehet: https://westus.cris.ai/customkws, https://eastasia.cris.ai/customkws vagy https://northeurope.cris.ai/customkws, attól függően, ahol a régióban van.

    ![Az egyéni ébresztési Word oldal rejtett](media/speech-devices-sdk/wake-word-4.png)
 
1. Írja be a választott ébresztési szót, és válassza ki **küldje el a word**.

    ![Adja meg az ébresztési word](media/speech-devices-sdk/wake-word-5.png)
 
1. Eltarthat néhány percig, létrejön a fájlokat. Megjelenik egy forgó kör a böngészőablakban. Ezután egy információs sáv megjelenik, rákérdez arra, hogy töltse le a .zip-fájlként.

    ![A .zip fájl fogadása](media/speech-devices-sdk/wake-word-6.png)

1. Mentse a .zip-fájlt a számítógépre. Ez a csomag központi telepítése az egyéni ébresztési word-fájl szükséges. Az egyéni ébresztési szó üzembe helyezéséhez kövesse a [a Speech eszközök SDK használatának első lépései](speech-devices-sdk-qsg.md).

1. Válassza ki **jelentkezzen ki.**

## <a name="next-steps"></a>További lépések

Első lépésként kérje le egy [ingyenes Azure-fiók](https://azure.microsoft.com/free/) és iratkozzon fel a Speech Devices SDK-val.

> [!div class="nextstepaction"]
> [Iratkozzon fel a Speech Devices SDK-val](get-speech-devices-sdk.md)


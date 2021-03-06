---
title: Oktatóanyag – Kulcskifejezéseket visszaadó LUIS-alkalmazás létrehozása – Azure | Microsoft Docs
description: Az oktatóanyagból megtudhatja, hogyan adhat hozzá, illetve kaphat vissza keyPhrase entitást a LUIS-alkalmazából a kulcstémák kimondott szövegének elemzéséhez.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: ef7a1c81f453a8d4ff9526a4844518782e152c4f
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159486"
---
# <a name="tutorial-8-add-keyphrase-entity"></a>Oktatóanyag: 8. keyPhrase entitás hozzáadása 
Ebben az oktatóanyagban egy alkalmazást használhat, amely bemutatja, hogyan nyerhetők ki kulcstémák a kimondott szövegekből.

<!-- green checkmark -->
> [!div class="checklist"]
> * Ismerkedés a keyPhrase entitásokkal 
> * A LUIS-alkalmazás használata az emberi erőforrások (HR) tartományban 
> * Egy keyPhrase entitás hozzáadása a kimondott szöveg tartalmának kinyeréséhez
> * Alkalmazás betanítása és közzététele
> * Alkalmazás végpontjának lekérdezése a LUIS által visszaadott JSON-válasz megtekintéséhez, beleértve a kulcskifejezéseket is

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Előkészületek
Ha még nincs meg az Emberi erőforrások alkalmazása az [egyszerű entitás](luis-quickstart-primary-and-secondary-data.md) oktatóanyagából, [importálja](luis-how-to-start-new-app.md#import-new-app) a JSON-t egy új alkalmazásba a [LUIS](luis-reference-regions.md#luis-website) webhelyén. Az importálandó alkalmazás a [LUIS-minták](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-simple-HumanResources.json) GitHub-adattárban található.

Ha meg szeretné tartani az eredeti Emberi erőforrások alkalmazást, klónozza a [Settings](luis-how-to-manage-versions.md#clone-a-version) (Beállítások) lapon a verziót, és adja neki a következő nevet: `keyphrase`. A klónozás nagyszerű mód, hogy kísérletezhessen a különböző LUIS-funkciókkal anélkül, hogy az az eredeti verzióra hatással lenne. 

## <a name="keyphrase-entity-extraction"></a>A keyPhrase entitás kinyerése
A kulcstémát az előre összeállított entitás, a **keyPhrase** biztosítja. Ez az entitás a kimondott szöveg kulcstémáját adja vissza.

Az alábbi kimondott szövegek például szolgáló kulcskifejezései:

|Kimondott szöveg|keyPhrase entitásértékek|
|--|--|
|Lesz jövőre olyan egészségbiztosítási csomag, amelynek alacsonyabb lesz az önrésze?|„alacsonyabb önrész”<br>„új egészségbiztosítási csomag”<br>„jövőre”|
|A nagyobb önrészű egészségbiztosítási csomag kiterjed a látáskorrekciós vizsgálatokra?|„nagyobb önrészű egészségbiztosítási csomag”<br>„látáskorrekciós vizsgálat”|

Az ügyfélalkalmazás használhatja ezeket az értékeket csakúgy, mint más kinyert entitásokat a beszélgetés következő lépésének megállapításához.

## <a name="add-keyphrase-entity"></a>keyPhrase entitás hozzáadása 
A kimondott szöveg témájának kinyeréséhez adja hozzá az előre összeállított keyPhrase entitást.

1. Győződjön meg arról, hogy az Emberi erőforrások alkalmazás a LUIS **Build** (Létrehozás) szakaszában van. Ha erre a szakaszra szeretne lépni, válassza a jobb felső menüsávon a **Build** (Létrehozás) elemet. 

2. Válassza az **Entities** (Entitások) elemet a bal oldali menüben.

    [ ![Az összeállítási szakasz bal oldali navigációs panelének képernyőképe kiemelt entitásokkal](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png#lightbox)

3. Válassza a **Manage prebuilt entities** (Előre összeállított entitások kezelése) lehetőséget.

    [ ![Az entitáslistát tartalmazó előugró párbeszédpanel képernyőképe](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png#lightbox)

4. Az előugró párbeszédpanelen válassza ki a **keyPhrase** elemet, majd a **Done** (Kész) lehetőséget. 

    [ ![Az entitáslistát tartalmazó előugró párbeszédpanel képernyőképe](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->
5. A bal oldali menüben válassza az **Intents** (Szándékok) lehetőséget, majd a **Utilities.Confirm** szándékot. A keyPhrase-entitás számos kimondott szövegben meg van címkézve. 

    [ ![Képernyőkép a Utilities.Confirm szándékról, a keyPhrases megcímkézve a kimondott szövegben](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>A LUIS-alkalmazás betanítása

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Alkalmazás közzététele a végponton

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]


## <a name="query-the-endpoint-with-an-utterance"></a>A kimondott szöveget tartalmazó végpont lekérdezése

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Lépjen az URL-cím végéhez, és írja be a következőt: `does form hrf-123456 cover the new dental benefits and medical plan`. Az utolsó lekérdezésisztring-paraméter `q`, a kimondott szöveg pedig a **query**. 

```
{
  "query": "does form hrf-123456 cover the new dental benefits and medical plan",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.9300641
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.9300641
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0359598845
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0141798034
    },
    {
      "intent": "MoveEmployee",
      "score": 0.0112197418
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00507669244
    },
    {
      "intent": "None",
      "score": 0.00238501839
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00202810857
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00102957746
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0008688423
    },
    {
      "intent": "Utilities.Confirm",
      "score": 3.557994E-05
    }
  ],
  "entities": [
    {
      "entity": "hrf-123456",
      "type": "HRF-number",git 
      "startIndex": 10,
      "endIndex": 19
    },
    {
      "entity": "new dental benefits",
      "type": "builtin.keyPhrase",
      "startIndex": 31,
      "endIndex": 49
    },
    {
      "entity": "medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 55,
      "endIndex": 66
    },
    {
      "entity": "hrf",
      "type": "builtin.keyPhrase",
      "startIndex": 10,
      "endIndex": 12
    },
    {
      "entity": "-123456",
      "type": "builtin.number",
      "startIndex": 13,
      "endIndex": 19,
      "resolution": {
        "value": "-123456"
      }
    }
  ]
}
```

Az űrlap keresése közben a felhasználó több információt adott meg, mint amennyi az űrlap megtalálásához szükséges. A további információkat **builtin.keyPhrase** elemként adja vissza a rendszer. Az ügyfélalkalmazás használhatja ezeket a további információkat egy következő kérdéshez, például „Szeretne egy emberierőforrások-képviselővel beszélni az új fogászati juttatásokról?”, vagy egy több lehetőséget tartalmazó menüt adhat meg, amely például tartalmazza a következőt: „További információk az új fogászati juttatásokról vagy orvosi tervről”.

## <a name="what-has-this-luis-app-accomplished"></a>Milyen műveleteket végzett el a LUIS-alkalmazás?
A keyPhrase entitás észlelésére képes alkalmazás azonosított egy természetes nyelvi lekérdezési szándékot, és visszaadta a kinyert adatokat, közöttük a fő témát is. 

A csevegőrobot már elegendő információval rendelkezik a beszélgetés következő lépésének megállapításához. 

## <a name="where-is-this-luis-data-used"></a>Hol vannak használatban ezek a LUIS-adatok? 
A LUIS végzett ezzel a kéréssel. A hívó alkalmazás, például egy csevegőrobot, felhasználhatja a topScoringIntent eredményt és a kimondott szöveg keyPhrase adatait a következő lépés végrehajtásához. A LUIS nem végzi el ezt a programozható munkát a csevegőrobotnak vagy a hívó alkalmazásnak. A LUIS csak azt határozza meg, hogy mi a felhasználó szándéka. 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [Hangulatelemzés hozzáadása az alkalmazáshoz](luis-quickstart-intent-and-sentiment-analysis.md)
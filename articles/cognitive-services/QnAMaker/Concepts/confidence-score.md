---
title: Megbízhatósági pontszám – a Microsoft Cognitive Services |} A Microsoft Docs
titleSuffix: Azure
description: Elmagyarázza, megbízhatósági pontszám
services: cognitive-services
author: tulasim88
manager: pchoudh
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 09/27/2018
ms.author: tulasim
ms.openlocfilehash: e878da5f6741b1a4c31874af05b7a37f6dee21df
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586223"
---
# <a name="confidence-score"></a>Megbízhatósági pontszám
Ha egy felhasználó lekérdezése Tudásbázis van, a QnA Maker azokra adott válaszokat, és a egy magabiztossági pontszámot ad vissza. Ezt az értéket, hogy a válasz-e a megfelelő egyezik a megadott felhasználói lekérdezés magabiztosan jelzi. 

A megbízhatósági pontszám értéke 0 és 100 közötti szám. A pontszám: 100, valószínűleg pontos egyezést, egy pontszám, a 0 azt jelenti, nem egyező válasz nem található, miközben. Minél nagyobb a pontszám - a a választ a nagyobb biztonsággal. Egy adott lekérdezésre vonatkozó lehet több választ ad vissza. Ebben az esetben a válaszokat a rendszer magabiztossági pontszámot csökkenő sorrendben adja vissza.

Az alábbi példában látható QnA egy entitást, 2 kérdése van. 


![Minta kérdés-válasz párt](../media/qnamaker-concepts-confidencescore/ranker-example-qna.png)

A fenti példa-pontszámok, például a minta pontszám tartomány az alábbi – a különböző típusú felhasználói lekérdezések várható:


![Rangsorolás pontszám tartomány](../media/qnamaker-concepts-confidencescore/ranker-score-range.png)


Az alábbi táblázat azt jelzi, hogy egy adott pontszám kapcsolódó tipikus magabiztosan.

|Pontszám értéke|Pontszám jelentése|. Példalekérdezés|
|--|--|--|
|90 - 100|A felhasználó lekérdezése és a egy KB-os kérdést pontos egyezés közelében|"A módosításokat nem frissíti a Tudásbázis közzététel után"|
|> 70|Magas megbízhatóság – általában egy jó választ, amely teljesen ad választ a felhasználó lekérdezése|"A KB-os közzétett, de nem frissül"|
|50 - 70|Közepes megbízhatósági – általában egy viszonylag jó választ, amely elsődleges célja, a felhasználó lekérdezése kell választ adni.|"Kell menteni a frissítés előtt közzé saját KB?"|
|30 – 50|Alacsony megbízhatósági – általában egy kapcsolódó választ, a felhasználói szándékot részleges válaszok|"Mire a Mentés és a vonat?"|
|< 30|Nagyon alacsony megbízhatósági – általában nem felel meg a felhasználó lekérdezése, de van néhány egyező szavak vagy kifejezések |"Ha is hozzáadhatok szinonimák saját KB"|
|0|Nincs egyezés, így a válasz nem ad vissza.|"A szolgáltatás mennyibe"|

## <a name="choose-a-score-threshold"></a>Válassza ki a pontszám küszöbértéket
A fenti táblázatban a legtöbb Tudásbázis mutatja, amelyek várhatóan pontszámokat. Azonban mivel minden KB különböző, és rendelkezik a különböző típusú szavak, leképezések és célok – javasoljuk, teszteléséhez, és válassza ki a küszöbérték, amely a legjobban működik az Ön számára. Az alapértelmezett és ajánlott küszöbértéket, amely a legtöbb Tudásbázis működnie kell az **50**.

A küszöbérték kiválasztásakor tartsa szem előtt a pontosság és lefedettség közötti egyensúly, és a Teljesítménybeállítások az igényei alapján küszöbértékét.

- Ha **pontossága** (vagy a pontosság) tartalomtovábbításának fontosabb, akkor növelje a küszöbértéket. Így minden alkalommal, amikor visszatér a választ, csak egy sokkal több CONFIDENT megkülönbözteti a kis, és sokkal valószínűleg a válasz felhasználókat keres. Ebben az esetben előfordulhat, hogy végül hagyja a további kérdések, megválaszolatlan be. *Például:* a küszöbérték győződjön meg arról, ha **70**, elkerülheti a figyelmét néhány nem egyértelmű példák kedvelések, "Mi mentse és betanítunk?".

- Ha **lefedettség** (vagy visszaírási) több fontos - és megválaszolásában, mint sok kérdésre, lehetséges, még akkor is, ha a felhasználó kérdés – csak részleges kapcsolat majd CSÖKKENTHETI a küszöbértéket. Ez azt jelenti, hogy ott lehet további olyan esetekben, ahol a válasz nem felel meg a felhasználó a tényleges lekérdezés, de néhány egyéb némileg kapcsolódó választ ad. *Például:* a küszöbérték győződjön meg arról, ha **30**, előfordulhat, hogy engedélyezi a hasonlóan, a fenti példa a válasz nem nagyon kapcsolódó válaszokat, lekérdezések, például a "hol módosítható a KB-os?"


## <a name="improve-confidence-scores"></a>Megbízhatósági pontszámukat
A megbízhatósági pontszám, egy felhasználó adott válaszban javítása érdekében adhat hozzá a felhasználó lekérdezése a Tudásbázis következő, egy másik kérdésre adott válasz.


## <a name="similar-confidence-scores"></a>Hasonló megbízhatósági pontszámok
Ha a többszöri válaszadást hasonló konfidencia-pontszám, valószínű, hogy a lekérdezés túl általános, és ezért egyező, több választ tartalmazó valószínűségét volt-e. Próbálja ki a QnA-tudásbázisok jobban építse fel, hogy minden QnA entitás rendelkezik egy különálló szándékot.


## <a name="confidence-score-differences"></a>Megbízhatósági pontszám különbségek
A megbízhatósági pontszám válasz változhatnak elhanyagolható mértékben a tesztelés és a Tudásbázis közzétett verziója között akkor is, ha a tartalom azonos. Ennek oka az, a teszt- és a közzétett Tudásbázis tartalmát a különböző Azure Search-indexek találhatók.
Itt látható a [közzététele](../How-To/publish-knowledge-base.md) művelet működését.


## <a name="no-match-found"></a>Nincs találat.
Nem megfelelő talál egyezést szerint a rangsorolás, amikor a 0,0 vagy "None" megbízhatósági pontszámot ad vissza, és az alapértelmezett válasz "nem szerepel jó a KB-ban található". Ez a robot vagy alkalmazás kód, a végpontot hív-e alapértelmezett válasz felül lehet bírálni. Azt is megteheti a felülbírálás válasz állítsa be az Azure-ban, és a egy adott QnA Maker szolgáltatást üzembe helyezett összes tudásbázisok az alapértelmezett értékre változik.

1. Nyissa meg a [az Azure portal](http://portal.azure.com) , és keresse meg az erőforráscsoport, amely a QnA Maker szolgáltatást létrehozott jelöli.

2. Ide kattintva megnyithatja a **App Service-ben**.

    ![Hozzáférés az App service](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Kattintson a **Alkalmazásbeállítások** és szerkesztheti a **DefaultAnswer** a kívánt alapértelmezett válasz mező. Kattintson a **Save** (Mentés) gombra.

    ![Alapértelmezett válasz módosítása](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Indítsa újra az App service

    ![A QnA Maker az App Service-újraindítás](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>További lépések
> [!div class="nextstepaction"]
> [Támogatott adatforrások](./data-sources-supported.md)
## <a name="see-also"></a>Lásd még 
[A QnA Maker áttekintése](../Overview/overview.md)

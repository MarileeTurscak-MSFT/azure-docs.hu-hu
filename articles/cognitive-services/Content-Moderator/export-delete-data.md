---
title: Exportálja, vagy törölje a data - Content Moderator
titlesuffix: Azure Cognitive Services
description: Ismerje meg, hogyan exportálhatja, vagy a Content Moderator-adatok törlése.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 9e671fa9735406ec425b105bb121d3a029ef19dc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227231"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Exportálja, vagy a Content Moderator a felhasználói adatok törlése

Content Moderator gyűjti a felhasználói adatokat a szolgáltatás, de az ügyfelek teljes hozzáféréssel rendelkeznek megtekintése, exportálása és törlése, hogy az adatok használatával a [felülvizsgálati felhasználói felület](http://contentmoderator.cognitive.microsoft.com/) és a [API-k](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Hogyan exportálhatja, és törli a felhasználói adatokat a Content Moderator további információkért lásd az alábbi táblázatot.

| Adatok | Az exportálási művelet | Törlési művelet |
| ---- | ---------------- | ---------------- |
| Fiók adatai (előfizetési kulcsok) | – | Törlés az Azure portal (az Azure-előfizetések) használatával. Vagy használja a **törlése csapat** gombra a [felülvizsgálati felhasználói felület](http://contentmoderator.cognitive.microsoft.com/) Team-beállítások lapon. |
| Az egyéni egyező képek | [Lemezkép-azonosítók beolvasása](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Rendszerképek tárolása a szellemi tulajdont képező egyirányú kivonat formátumban történik, és nem lehet kibontani a tényleges lemezképeket. | [Törölje az összes lemezképet](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Vagy törli a Content Moderator az erőforrást az Azure portal használatával. |
| Egyéni egyezési feltételei | [Minden használati](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Az összes feltételek törlése](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Vagy törli a Content Moderator az erőforrást az Azure portal használatával. |
| Címkék | – | Használja a **törlése** ikon minden címke a tekintse át a felhasználói felület címke beállítások oldalon érhető el. Vagy használja a **törlése csapat** gombra a [felülvizsgálati felhasználói felület](http://contentmoderator.cognitive.microsoft.com/) Team-beállítások lapon. |
| Felülvizsgálatok | [Felülvizsgálat beolvasása](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Használja a **törlése csapat** gombra a [felülvizsgálati felhasználói felület](http://contentmoderator.cognitive.microsoft.com/) Team-beállítások lapon.
| Felhasználók | – | Használja a **törlése** ikonra a minden felhasználó számára elérhető a [felülvizsgálati felhasználói felület](http://contentmoderator.cognitive.microsoft.com/) Team-beállítások lapon. Vagy használja a **törlése csapat** gombra a [felülvizsgálati felhasználói felület](http://contentmoderator.cognitive.microsoft.com/) Team-beállítások lapon. |


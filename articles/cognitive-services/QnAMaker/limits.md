---
title: Korlátozások és határok – QnA Maker
titleSuffix: Azure Cognitive Services
description: A QnA Maker között korlátok átfogó listáját.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: efdcf57fb0f175be51e6b9f8e72e4a02fcbeb5f3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162321"
---
# <a name="qna-maker-limits"></a>A QnA Maker korlátok
A QnA Maker között korlátok átfogó listáját.

## <a name="knowledge-bases"></a>Tudásbázisok

* Tudásbázisok maximális száma alapján [Azure Search szint korlátok](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Az Azure Search szolgáltatásréteg** | **Ingyenes** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Engedélyezett közzétett tudásbázisok maximális számát (indexek maximális száma – 1 (fenntartott teszt)|2|14|49|199|199|2999|

## <a name="extraction-limits"></a>Kibontási korlátok
* Eseménystreamek olvashatóak fájlok maximális számát és méretét: lásd: [QnAMaker díjszabása](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* – Gyakori kérdések HTML-lapok a a kivonási QnA-tudásbázisok bejárható részletes hivatkozások maximális száma: 20

## <a name="metadata-limits"></a>Metaadat-korlátok
* Metaadatokat tartalmazó mezőket egy Tudásbázis, maximális száma alapján [Azure Search szint korlátok](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Az Azure Search szolgáltatásréteg** | **Ingyenes** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximális metaadatokat tartalmazó mezőket a QnA Maker szolgáltatás (között az összes Tudásbázis)|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Tudásbázis-tartalmat korlátok
Általános korlátozások a tartalmat a Tudásbázis:
* Válasz szöveg hossza: 250000
* Kérdés szöveg hossza: 1000
* Metaadatok kulcs/érték szöveg hossza: 100
* A metaadat neve által támogatott karakterek: betűk, számjegyek és _  
* A metaadat-értékben által támogatott karakterek: mindenhol, kivéve: és |} 
* Fájl nevének hossza: 200
* Támogatott fájlformátumok: ".tsv", ".pdf", "txt", ".docx", ".xlsx".
* Alternatív kérdések maximális számát: 100
* Kérdés-válasz párt maximális számát: függ a [Azure Search szolgáltatásréteg](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) kiválasztása 

## <a name="create-knowledge-base-call-limits"></a>Hozza létre a Tudásbázis hívás korlátai:
Ezen felelnek a minden korlátokat Tudásbázis művelet; létrehozása azt jelenti, kattintson a *létrehozása KB* vagy a CreateKnowledgeBase API-t hívná.
* Alternatív kérdésnek válasz maximális számát: 100
* URL-ek maximális száma: 10
* Fájlok maximális száma: 10

## <a name="update-knowledge-base-call-limits"></a>Tudásbázis hívás vonatkozó korlátok módosítása
Ezek képviselik a frissítési műveletek; korlátai azt jelenti, kattintson a *mentéséhez és betanítunk* vagy a UpdateKnowledgeBase API-t hívná.
* Minden adatforrás nevének hossza: 300
* Alternatív kérdések felvett vagy törölt maximális számát: 100
* Metaadatok mezők maximális száma felvett vagy törölt: 10
* Frissíthető URL-címek maximális száma: 5

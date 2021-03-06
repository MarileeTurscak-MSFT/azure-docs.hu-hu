---
title: Az Azure-beli hitelesítő adatait a Content Moderator |} A Microsoft Docs
description: A Content Moderator hitelesítő adatokat, az API-k használata kezelheti.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 6477879953dc2bb2c7503eb0b2d4b5effa7b6a11
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/06/2018
ms.locfileid: "44024655"
---
# <a name="manage-credentials"></a>Hitelesítő adatok kezelése

A Content Moderator hitelesítő adataival jönnek létre a következő helyeken:

- [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [A Content Moderator felülvizsgálati eszköz](http://contentmoderator.cognitive.microsoft.com/)

Ez a cikk ismerteti, hogy hol található őket, és hogyan kapcsolódnak egymáshoz.

## <a name="the-azure-portal"></a>Az Azure Portal

Az Azure portal irányítópultján válassza a Content Moderator fiókját. A **erőforrás-kezelés**válassza **kulcsok**. Másolja a kulcsot, kattintson a jobb oldalán a kulcs ikonra.

![Content Moderator kulcsok az Azure Portalon](images/credentials-azure-portal-keys.PNG)

### <a name="use-the-azure-account-with-the-review-tool-and-review-api"></a>Az Azure-fiók használata a vizsgálóeszközt, és tekintse át az API-t
A felülvizsgálati API-k az Azure-kulcs használata, másolja az erőforrás-azonosító szerepel a **tulajdonságok** az alábbi képernyőképen képernyőn, majd írja be a hitelesítő adatok képernyőn a vizsgálóeszköz a **szerepel az engedélyezési listán erőforrás-azonosító(k)** , ahogyan az alábbi mezők **erőforrás-azonosító** szakaszban. 

> [!NOTE]
> A Content Moderator előfizetéshez tartozó régiót meg kell egyeznie a felülvizsgálati csapat régió ahhoz, hogy a csapat ismeri fel és a csapat az adatokkal. Például ezen a lapon a képeken az **USA nyugati RÉGIÓJA** régió **(4)** tartalmaz a Content Moderator Azure-előfizetés és a csapatától.
>
> Cserélje a két helyén a vizsgálóeszköz a kulcs és az erőforrás-azonosító az Azure-előfizetésből, miután a **próbaverzió Ocp-Apim-Subscription-Key** jelenik meg a hitelesítő adatok képernyőn már nincs használatban, de mindig elérhető.
> A próbaverzió kulcs legfeljebb 5 000 tranzakció havonta, 1 kérelem / másodperc (RPS) korlátozza.

![Content Moderator erőforrás-azonosító az Azure Portalon](images/credentials-azure-portal-resourceid.PNG)

### <a name="use-the-azure-account-with-the-workflows-in-the-review-tool"></a>Az Azure-fiók használata a munkafolyamatokat a felülvizsgálati eszköz

Az Azure key használata a munkafolyamatokhoz elérhető a Content Moderator, írja be a a **Ocp-Apim-Subscription-Key** mezőbe a **munkafolyamat-beállítások** , ahogyan az alábbi szakasz  **A munkafolyamatok** szakaszban. Nyomja le az **"+"** menteni az erőforrás-azonosítója.

![Content Moderator munkafolyamat hitelesítő adatait a felülvizsgálati eszköz](images/credentials-workflow.PNG)

## <a name="the-review-tool"></a>A felülvizsgálati eszköz

A tekintse át az eszköz irányítópultján a **beállítások** lapon jelölje be **hitelesítő adatok**.

![Content Moderator hitelesítő adatait a felülvizsgálati eszköz](images/credentials-trial-resource-workflow.PNG)

Az alábbi szakasz megvizsgálja az előző képen részletesebben:

### <a name="api"></a>API

Az első rész listák a **tekintse át az API-végpont**, **csapatazonosító**, és a **Ocp-Apim-Subscription-Key (a Content Moderator Próbakulcs)** a csapatától részeként állítja elő létrehozását. Ezek segítségével az összes Content Moderator API-k, beleértve a felülvizsgálati API-t hívjuk.

Azt is vegye figyelembe az API-végpont a régió azonosítója. Ha például **westus** a régió, a "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0"

![Content Moderator kulcsot a felülvizsgálati eszköz](images/credentials-trialkey.PNG)

### <a name="resource-id"></a>Erőforrás-azonosító

Az ebben a szakaszban tárgyalt témaköröket a [Azure-fiókjával a felülvizsgálati eszköz-és API-val](credentials.md#how-to-use-your-azure-account-with-the-review-tool) szakaszban. A mező kitöltése általában üresen, kivéve, ha az ad hozzá az Azure erőforrás-azonosító ebben a mezőben az előző szakaszban leírtak szerint.

### <a name="workflows"></a>Munkafolyamatok

Ez a mezők halmaza alapján az előző szakaszban tárgyalt témaköröket a [a munkafolyamatok futtatásához az Azure-kulcs használatával](credentials.md#use-the-azure-account-with-the-workflows-in-the-review-tool). Alapértelmezés szerint a vizsgálóeszközt az automatikusan létrehozott próbaverziós kulcsot használja a munkafolyamatok futtatásához, és ez milyen jelenik meg először. A két mező lehetővé teszi a meghatározott időszak és a lemezkép listák képernyő szöveges és képi kiértékelése operatív jelölik.

## <a name="next-steps"></a>További lépések

* A Content Moderator hitelesítő adatok használata a [munkafolyamatok](workflows.md).

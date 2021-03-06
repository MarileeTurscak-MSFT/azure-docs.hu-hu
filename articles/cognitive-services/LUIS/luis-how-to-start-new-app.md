---
title: Új alkalmazás létrehozása az intelligens hangfelismerési szolgáltatással
description: Létrehozhat és kezelhet az alkalmazások, a Language Understanding (LUIS) weblapon.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 401c4fb8942aee73c036ae2b248a030eaea4917a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031254"
---
# <a name="create-an-app"></a>Alkalmazás létrehozása
Nincsenek többféle módon LUIS-alkalmazások létrehozásához. A LUIS-alkalmazásokon hozhat létre a [LUIS](https://www.luis.ai) portál, vagy a LUIS szerzői [API-k](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

## <a name="using-the-luis-portal"></a>A LUIS-portál használatával
A LUIS portálon többféle módon hozhat létre egy új alkalmazást:

* [Indítsa el](#create-new-app) egy üres alkalmazást és szándék fog vonatkozni, utterances és entitások létrehozásához.
* [Indítsa el](#create-new-app) egy üres alkalmazást, és adja hozzá a [előre összeállított tartomány](luis-how-to-use-prebuilt-domains.md).
* [A LUIS alkalmazás importálása](#import-new-app) , amely már tartalmazza a szándék fog vonatkozni, utterances és entitások JSON-fájlból.

## <a name="using-the-authoring-apis"></a>Az Authoring Tool API-k használatával
Az Authoring Tool API-khoz többféle módon hozhat létre egy új alkalmazást:

* [Indítsa el](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) egy üres alkalmazást és szándék fog vonatkozni, utterances és entitások létrehozásához.
* [Indítsa el](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) egy előre elkészített tartománnyal.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Új alkalmazás létrehozása a LUIS

1. A **saját alkalmazások** lapon jelölje be **új alkalmazás létrehozása**.

    ![LUIS-alkalmazások listája](./media/luis-create-new-app/apps-list.png)


2. A párbeszédpanelen adjon nevet az alkalmazás "TravelAgent".

    ![Hozzon létre új alkalmazás párbeszédpanel](./media/luis-create-new-app/create-app.png)

3. Válassza ki az alkalmazás kulturális környezet (TravelAgent alkalmazások esetén válassza az angol nyelvű), majd válassza ki **kész**. 

    > [!NOTE]
    > A kulturális környezet az alkalmazás létrehozása után nem módosítható. 

    

<!--

## Import new app
You can set the name (50 char max), version (10 char max), and description of an app in the JSON file. Examples of application JSON files are available at [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. On **My Apps** page, select **Import new app**.
2. In the **Import new app** dialog, select the JSON file defining the LUIS app.

    ![Import a new app dialog](./media/luis-create-new-app/import-app.png)

## Export app
1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row.

    [![](media/luis-create-new-app/apps-list.png "Screenshot of pop-up dialog of per-app actions")](media/luis-create-new-app/three-dots.png#lightbox)

2. Select **Export app** from the menu. 

## Rename app

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Rename** from the menu.
3. Enter the new name of the app and select **Done**.

## Delete app

> [!CAUTION]
> You are deleting the app for all collaborators and the owner. [Export](#export-app) the app before deleting it. 

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Delete** from the menu.
3. Select **Ok** in the confirmation window.

## Export endpoint logs
The logs contain the Query, UTC time, and LUIS JSON response.

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Export endpoint logs** from the menu.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```
-->
## <a name="next-steps"></a>További lépések

Az első az alkalmazás feladata [leképezések hozzáadása](luis-how-to-add-intents.md).
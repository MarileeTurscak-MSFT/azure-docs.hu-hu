---
title: A QnA Maker szolgáltatás – QnA Maker frissítése
titleSuffix: Azure Cognitive Services
description: A QnA Maker verem egyes összetevőinek frissítésére a kezdeti létrehozás után kiválaszthatja.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 8542b1f6dfe031de58ea6eeb931027ee03bd81f2
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030965"
---
# <a name="upgrade-your-qna-maker-service"></a>QnA Maker-szolgáltatás frissítése
A QnA Maker verem egyes összetevőinek frissítésére a kezdeti létrehozás után kiválaszthatja. A részletek a függő összetevők és a termékváltozat [Itt](https://aka.ms/qnamaker-docs-capacity).

## <a name="upgrade-qna-maker-management-sku"></a>A QnA Maker felügyeleti Termékváltozat frissítése
A QnA Maker felügyeleti Termékváltozat frissítése:
1. Nyissa meg a QnA Maker erőforrást az Azure Portalon, és válassza ki **tarifacsomag**.

    ![A QnA Maker erőforrás](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Válassza ki a megfelelő Termékváltozat és nyomja le a **kiválasztása**.

    ![A QnA Maker – díjszabás](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Az App service frissítése
Is [vertikális felskálázás](https://docs.microsoft.com/azure/app-service/web-sites-scale) - vagy leskálázás az App Service-ben.

1. Nyissa meg az App service-erőforrást az Azure Portalon, és válassza ki **vertikális felskálázás** vagy **vertikális leskálázás** szükség szerint lehetőség.

    ![A QnA Maker alkalmazás szolgáltatásskálázás](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Az Azure Search szolgáltatás frissítése
Jelenleg nem hajtható végre egy a hely frissítését az Azure keresési Termékváltozat. Azonban az Azure search új erőforrás létrehozása a kívánt Termékváltozat, állítsa vissza az adatokat az új erőforráshoz, és összekapcsolása a QnA Maker verem.

1. Az Azure search új erőforrás létrehozása az Azure Portalon, és válassza ki a kívánt Termékváltozatot.

    ![A QnA Maker Azure search-erőforrás](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. Az indexek visszaállítása az eredeti az Azure search-erőforrás az újat. Tekintse meg a biztonsági mentés visszaállítási mintakód [Itt](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Az adatok visszaállítása után nyissa meg az új Azure search erőforrás válassza **kulcsok**, és jegyezze fel a **neve** és a **adminisztrációs kulcsot**.

    ![A QnA Maker Azure search-kulcsok](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Az új Azure search-erőforrás összekapcsolása a QnA Maker vermet, nyissa meg a QnA Maker App Service-ben.

    ![Az App Service a QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Válassza ki **Alkalmazásbeállítások** , és cserélje le a **AzureSearchName** és **AzureSearchAdminKey** mezőket a 3. lépés.

    ![A QnA Maker az App Service-beállítás](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. Indítsa újra az App Service-ben.

    ![A QnA Maker az App Service-újraindítás](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [A QnA Maker API használata](../Quickstarts/csharp.md)

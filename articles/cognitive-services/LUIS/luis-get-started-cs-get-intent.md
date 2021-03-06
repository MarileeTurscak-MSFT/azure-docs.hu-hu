---
title: Természetes nyelvű szöveg elemzése a Language Understanding (LUIS) szolgáltatásban C# nyelven – Azure Cognitive Services | Microsoft Docs
description: Ebben a rövid útmutatóban elérhető nyilvános LUIS-alkalmazással határozza meg egy felhasználó szándékát egy beszélgetés szövegéből. C# nyelven küldje el szövegként a felhasználó szándékát a nyilvános alkalmazás HTTP-előrejelzési végpontjára. A LUIS a végpontnál a nyilvános alkalmazás modelljét alkalmazza a természetes nyelvű szövegen a jelentés elemzése érdekében, amellyel meghatározza az általános szándékot, valamint kinyeri az alkalmazás témájában releváns adatokat.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: b6ddd48fc6bfa5c099e42f3717a2113f871b4f9a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163260"
---
# <a name="quickstart-analyze-text-using-c"></a>Rövid útmutató: Szövegelemzés C# használatával

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Előfeltételek

* [A Visual Studio Community 2017-es kiadása](https://visualstudio.microsoft.com/vs/community/)
* C# programozási nyelv (a VS Community 2017 része)
* A df67dcdb-c37d-46af-88e1-8b97951ca1c2 azonosítójú nyilvános alkalmazás


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS-kulcs lekérése

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Elemzés böngészővel

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-c"></a>Szövegelemzés C# használatával 

Ha C# nyelven lekérdezi a GET [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) előrejelzési végpontot, ugyanazokat az eredményeket kapja, mint amelyeket az előző szakaszban látott a böngészőablakban. 

1. Hozzon létre egy új konzolalkalmazást a Visual Studióban. 

    ![A LUIS felhasználói beállítások menü elérése](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. A Visual Studio projekt Megoldáskezelő ablakában válassza az **Add reference** (Referencia hozzáadása) elemet, majd válassza a **System.Web** lehetőséget az Assemblies (Szerelvények) lapon.

    ![A LUIS felhasználói beállítások menü elérése](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Írja felül a Program.cs fájlt a következő kóddal:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Cserélje le a `YOUR_KEY` értéket a LUIS-kulcsra.

5. Buildelje és futtassa a konzolalkalmazást. Megjelenik a korábban a böngészőablakban látott JSON.

    ![A LUIS által visszaadott JSON-eredményt megjelenítő konzolablak](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="luis-keys"></a>LUIS-kulcsok

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Amikor végzett ezzel a rövid útmutatóval, zárja be a Visual Studio-projektet, és távolítsa el a projekt könyvtárát a fájlrendszerből. 

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [Kimondott szövegek hozzáadása és betanítás a C# használatával](luis-get-started-cs-add-utterance.md)
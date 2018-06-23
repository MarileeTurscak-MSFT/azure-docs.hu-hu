---
title: Egyéni kifejezés listákkal Azure tartalom moderátor mérsékelt |} Microsoft Docs
description: Az egyéni kifejezés mérsékelt hogyan sorolja fel, Azure tartalom moderátor SDK for .NET használatával.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/11/2018
ms.author: sajagtap
ms.openlocfilehash: 6da72ad070d9c3a6be38e24626dff77b52fed852
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/23/2018
ms.locfileid: "35347182"
---
# <a name="moderate-with-custom-term-lists-in-net"></a>A .NET egyéni kifejezés listákkal közepes szintű

Az alapértelmezett globális listáját Azure tartalom moderátor elegendő-e a tartalom moderálás igényeinek. Azonban szükség lehet a szervezet vonatkozó feltételek képernyőn. Például előfordulhat, hogy kívánt versenytársnak címkenevek további ellenőrzésre. 

A tartalom moderátor SDK for .NET hozhat létre egyéni listájának feltételeket, hogy a szöveg moderálás API-t használja.

> [!NOTE]
> Nincs a jelenlegi maximális műveletszámot **5 kifejezés listája** minden listájával, hogy **legfeljebb 10 000 feltételek**.
>

Ez a cikk információkat tartalmazza, és mintakódok segítséget első lépései a .NET-hez, hogy a tartalom moderátor SDK használatával:
- Lista létrehozása.
- Feltételek hozzáadása a listához.
- Képernyőfelvétel a feltételek listáját feltételeit ellen.
- Feltételek törlése a listáról.
- Listájának törlése.
- Lista adatainak szerkesztése.
- Frissítse az index, hogy a lista módosításainak szerepelnek az új vizsgálat.

Ez a cikk feltételezi, hogy Ön már ismeri a Visual Studio és a C#.

## <a name="sign-up-for-content-moderator-services"></a>Iratkozzon fel a tartalom moderátor szolgáltatások

Tartalom moderátor-szolgáltatások díjairól a REST API-t vagy az SDK használata előtt be kell egy előfizetési kulcsot.

A tartalom moderátor irányítópulton található az Előfizetés kulcs **beállítások** > **hitelesítő adatok** > **API**  >  **Próba Ocp-Apim-előfizetés-kulcs**. További információkért lásd: [áttekintése](overview.md).

## <a name="create-your-visual-studio-project"></a>A Visual Studio-projekt létrehozása

1. Adjon hozzá egy új **Konzolalkalmazás (.NET-keretrendszer)** -projektet a megoldásban.

1. Nevet a projektnek **TermLists**. Jelölje ki a projektet a megoldásban egyetlen kiindulási projektként.

1. Adjon hozzá egy hivatkozást, a **ModeratorHelper** szerelvény a projektre a [tartalom moderátor ügyfél segítő gyors üzembe helyezés](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Szükséges csomagok telepítése

A következő NuGet-csomagok a TermLists projekt telepítése:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Frissítés a program által utasítások segítségével.

Módosítsa a program által using utasításokat.

    using System;
    using System.Threading;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;

### <a name="add-private-properties"></a>Adja hozzá a saját tulajdonságait

A következő személyes tulajdonságok hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// The language of the terms in the term lists.
    /// </summary>
    private const string lang = "eng";

    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Content Moderator APIs.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against a the list.
    /// </summary>
    private const double latencyDelay = 0.5;

## <a name="create-a-term-list"></a>Egy kifejezés lista létrehozása

A kifejezés lista létrehozása **ContentModeratorClient.ListManagementTermLists.Create**. Az első paraméterben **létrehozása** karakterlánc, amely tartalmazza a MIME-típust, amely "application/json" kell lennie. További információkért lásd: a [API-referencia](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). A második paraméter egy **törzs** objektum, amely nevét és leírását, az új kifejezés listáját tartalmazza.

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

> [!NOTE]
> A tartalom moderátor szolgáltatási kulcs egy kérelmek / második (RPS) sávszélesség-korlátjának rendelkezik, és ha meghaladja a korlátot, az SDK 429-es jelű hibakód kivételt jelez. 
>
> Ingyenes szint kulcs egy RPS sávszélesség-korlátjának rendelkezik.

    /// <summary>
    /// Creates a new term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The term list ID.</returns>
    static string CreateTermList (ContentModeratorClient client)
    {
        Console.WriteLine("Creating term list.");

        Body body = new Body("Term list name", "Term list description");
        TermList list = client.ListManagementTermLists.Create("application/json", body);
        if (false == list.Id.HasValue)
        {
            throw new Exception("TermList.Id value missing.");
        }
        else
        {
            string list_id = list.Id.Value.ToString();
            Console.WriteLine("Term list created. ID: {0}.", list_id);
            Thread.Sleep(throttleRate);
            return list_id;
        }
    }

## <a name="update-term-list-name-and-description"></a>Frissítés kifejezés lista nevét és leírását

A kifejezés lista információinak frissítéséhez **ContentModeratorClient.ListManagementTermLists.Update**. Az első paraméterben **frissítés** kifejezés lista-azonosító. A második paraméter egy MIME-típust, amely "application/json" kell lennie. További információkért lásd: a [API-referencia](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). A harmadik paraméter egy **törzs** objektum, amely tartalmazza az új nevét és leírását.

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// Update the information for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="name">The new name for the term list.</param>
    /// <param name="description">The new description for the term list.</param>
    static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
    {
        Console.WriteLine("Updating information for term list with ID {0}.", list_id);
        Body body = new Body(name, description);
        client.ListManagementTermLists.Update(list_id, "application/json", body);
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-term-to-a-term-list"></a>A kifejezés listához adni egy kifejezés

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// Add a term to the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="term">The term to add to the term list.</param>
    static void AddTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
        client.ListManagementTerm.AddTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

## <a name="get-all-terms-in-a-term-list"></a>Minden feltételek kifejezés listaként beolvasása

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// Get all terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to get all terms.</param>
    static void GetAllTerms(ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
        Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
        TermsData data = terms.Data;
        foreach (TermsInList term in data.Terms)
        {
            Console.WriteLine(term.Term);
        }
        Thread.Sleep(throttleRate);
    }

## <a name="add-code-to-refresh-the-search-index"></a>Adja hozzá a kódot, hogy a keresési index frissítése

Kifejezés listájának módosítása után frissítenie a módosítások meg lehet adni a következő képernyő szöveg kifejezés lista használatakor a search-index. Ez hasonlít hogyan egy keresőmotor (Ha engedélyezve van), az asztalon vagy egy webes keresőmotor folyamatosan frissíti új fájlok vagy lapok indexét.

Egy kifejezés lista keresési indexszel rendelkező frissíti **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// Refresh the search index for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to refresh.</param>
    static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
        client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
        Thread.Sleep((int)(latencyDelay * 60 * 1000));
    }

## <a name="screen-text-using-a-term-list"></a>Képernyő szöveg kifejezés lista használatával

Egy kifejezés lista használatával képernyőn meg **ContentModeratorClient.TextModeration.ScreenText**, amely a következő paraméterek fogadja el.

- A kifejezés listája a kifejezések nyelv.
- A MIME-típust, amely "text/html", "text/xml", "text/markdown" vagy "text/plain" lehet.
- A képernyő szöveg.
- Logikai érték. Ez a mező beállítása **igaz** automatikus javítási előtt szűrési azt a szöveget.
- Logikai érték. Ez a mező beállítása **igaz** azonosíthatók a szöveg a személyes azonosításra alkalmas adatokat (PII).
- A kifejezés listaazonosító.

További információkért lásd: a [API-referencia](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** adja vissza egy **képernyő** objektum, amely rendelkezik egy **feltételek** tulajdonság, amely tartalmazza a feltételeket, hogy a tartalom moderátor észlelt a szűrés. Ha a tartalom moderátor nem talált meg a feltételeket a szűrés során vegye figyelembe, hogy a **feltételek** tulajdonság értéke **null**.

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// Screen the indicated text for terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to use to screen the text.</param>
    /// <param name="text">The text to screen.</param>
    static void ScreenText (ContentModeratorClient client, string list_id, string text)
    {
        Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
        Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
        if (null == screen.Terms)
        {
            Console.WriteLine("No terms from the term list were detected in the text.");
        }
        else
        {
            foreach (DetectedTerms term in screen.Terms)
            {
                Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
            }
        }
        read.Sleep(throttleRate);
    }

## <a name="delete-terms-and-lists"></a>Törli a feltételek és listák

A kifejezés vagy egy lista törlése nem egyértelmű. Az SDK segítségével hajtsa végre a következő feladatokat:

- Egy kifejezés törlése. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- Egy listán szereplő összes feltétel törlése a lista törlése nélkül. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Egy listát, és annak teljes tartalmát törli. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### <a name="delete-a-term"></a>Egy kifejezés törlése

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// Delete a term from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete the term.</param>
    /// <param name="term">The term to delete.</param>
    static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
        client.ListManagementTerm.DeleteTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-all-terms-in-a-term-list"></a>Egy kifejezés listán szereplő összes feltétel törlése

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// Delete all terms from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete all terms.</param>
    static void DeleteAllTerms (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
        client.ListManagementTerm.DeleteAllTerms(list_id, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-a-term-list"></a>Kifejezés listájának törlése

A következő metódusdefiníciót hozzáadása a névtérhez TermLists, osztály Program.

    /// <summary>
    /// Delete the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to delete.</param>
    static void DeleteTermList (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Deleting term list with ID {0}.", list_id);
        client.ListManagementTermLists.Delete(list_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>A teljes kép

Adja hozzá a **fő** metódusdefiníciót is névtér TermLists, a Program osztályban. Végül zárja be a Program és a TermLists névtér.

    static void Main(string[] args)
    {
        using (var client = Clients.NewClient())
        {
            string list_id = CreateTermList(client);

            UpdateTermList(client, list_id, "name", "description");
            AddTerm(client, list_id, "term1");
            AddTerm(client, list_id, "term2");

            GetAllTerms(client, list_id);

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            string text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteTerm(client, list_id, "term1");

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteAllTerms(client, list_id);
            DeleteTermList(client, list_id);

            Console.WriteLine("Press ENTER to close the application.");
            Console.ReadLine();
        }
    }

## <a name="run-the-application-to-see-the-output"></a>A kimenet megtekintéséhez az alkalmazás futtatása

A kimenet lesz a következő sorokban, de az adatok változhat.

    Creating term list.
    Term list created. ID: 252.
    Updating information for term list with ID 252.
    
    Adding term "term1" to term list with ID 252.
    Adding term "term2" to term list with ID 252.
    
    Getting terms in term list with ID 252.
    term1
    term2
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term1" from list ID 252 at index 32.
    Found term: "term2" from list ID 252 at index 46.
    
    Removed term "term1" from term list with ID 252.
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term2" from list ID 252 at index 46.
    
    Removing all terms from term list with ID 252.
    Deleting term list with ID 252.
    Press ENTER to close the application.
    
## <a name="next-steps"></a>További lépések

[Töltse le a Visual Studio megoldás](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) ennél és egyéb tartalom moderátor quickstarts a .NET-hez, és az integráció a kezdéshez.

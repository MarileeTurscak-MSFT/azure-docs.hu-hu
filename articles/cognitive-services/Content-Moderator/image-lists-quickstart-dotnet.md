---
title: Az egyéni rendszerkép listák az Azure Content Moderator mérsékelt |} A Microsoft Docs
description: Hogyan lehet egyéni rendszerképpel rendelkező közepes sorolja fel, az Azure Content Moderator SDK használatával a .NET-hez.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/14/2018
ms.author: sajagtap
ms.openlocfilehash: 855d89af70e080e2837fe2c0b66ea66c188dbd61
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182688"
---
# <a name="moderate-with-custom-image-lists-in-net"></a>Üzletmenetre gyakorolt közepes egyéni rendszerkép listákkal a .NET-ben

Ez a cikk nyújt információt, és kódminták segítségével történő használatának első lépései a [Content Moderator SDK for .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) való:
- Hozzon létre egy egyéni rendszerkép
- Hozzáadhat és eltávolíthat a lemezképek a listából
- Az összes rendszerkép azonosítóinak beszerzését a listában
- A lekérésére és frissítésére listametaadatok
- A lista keresési index frissítése
- Képernyőképek a listában található rendszerképek ellen
- Törölje az összes lemezképet a listából
- Az egyéni lista törlése

> [!NOTE]
> A maximális korlát **5 kép listák** az egyes lista **nem haladhatja meg a 10 000 lemezképek**.
>

Ebben a rövid útmutatóban a konzolalkalmazást szimulálja a feladatokat hajthat végre a képlista API.

Ez a cikk azt feltételezi, hogy már ismeri a Visual Studio és C#.

## <a name="sign-up-for-content-moderator-services"></a>Iratkozzon fel a Content Moderator szolgáltatások

A REST API-t vagy az SDK-t a Content Moderator szolgáltatások használata előtt szüksége van egy előfizetési kulcsot.
Tekintse meg a [rövid](quick-start.md) megtudhatja, hogyan szerezheti be a kulcsot.

## <a name="create-your-visual-studio-project"></a>A Visual Studio-projekt létrehozása

1. Vegyen fel egy új **Console app (.NET Framework)** projektet a megoldáshoz.

   A mintakód adja a projektnek **ImageLists**.

1. Jelölje ki a projektet a megoldáshoz egyetlen indítási projektként.

### <a name="install-required-packages"></a>Szükséges csomagok telepítése

A következő NuGet-csomagok telepítéséhez:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Frissítés a program által utasítások segítségével.

Módosítsa a program által utasítások segítségével.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>A Content Moderator ügyfél létrehozása

Adja hozzá a következő kódot a Content Moderator ügyfélbeállítások az előfizetéshez.

> [!IMPORTANT]
> Frissítés a **AzureRegion** és **CMSubscriptionKey** mezőket a régió azonosítója és az Előfizetés kulcs értékét.


    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "YOUR API REGION";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"https://{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR API KEY";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.Endpoint = AzureBaseURL;
            return client;
        }
    }


### <a name="initialize-application-specific-settings"></a>Alkalmazás-specifikus beállítások inicializálása

Adja hozzá a következő osztályok és a statikus mezők a **Program** osztály a program.cs fájlban.

    /// <summary>
    /// The minimum amount of time, im milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against the list.
    /// </summary>
    private const double latencyDelay = 0.5;

    /// <summary>
    /// Define constants for the labels to apply to the image list.
    /// </summary>
    private class Labels
    {
        public const string Sports = "Sports";
        public const string Swimsuit = "Swimsuit";
    }

    /// <summary>
    /// Define input data for images for this sample.
    /// </summary>
    private class Images
    {
        /// <summary>
        /// Represents a group of images that all share the same label.
        /// </summary>
        public class Data
        {
            /// <summary>
            /// The label for the images.
            /// </summary>
            public string Label;

            /// <summary>
            /// The URLs of the images.
            /// </summary>
            public string[] Urls;
        }

        /// <summary>
        /// The initial set of images to add to the list with the sports label.
        /// </summary>
        public static readonly Data Sports = new Data()
        {
            Label = Labels.Sports,
            Urls = new string[] {
                "https://moderatorsampleimages.blob.core.windows.net/samples/sample4.png",
                "https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png",
                "https://moderatorsampleimages.blob.core.windows.net/samples/sample9.png"
            }
        };

        /// <summary>
        /// The initial set of images to add to the list with the swimsuit label.
        /// </summary>
        /// <remarks>We're adding sample16.png (image of a puppy), to simulate
        /// an improperly added image that we will later remove from the list.
        /// Note: each image can have only one entry in a list, so sample4.png
        /// will throw an exception when we try to add it with a new label.</remarks>
        public static readonly Data Swimsuit = new Data()
        {
            Label = Labels.Swimsuit,
            Urls = new string[] {
                "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg",
                "https://moderatorsampleimages.blob.core.windows.net/samples/sample3.png",
                "https://moderatorsampleimages.blob.core.windows.net/samples/sample4.png",
                "https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png"
            }
        };

        /// <summary>
        /// The set of images to subsequently remove from the list.
        /// </summary>
        public static readonly string[] Corrections = new string[] {
            "https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png"
        };
    }

    /// <summary>
    /// The images to match against the image list.
    /// </summary>
    /// <remarks>Samples 1 and 4 should scan as matches; samples 5 and 16 should not.</remarks>
    private static readonly string[] ImagesToScreen = new string[] {
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg",
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample4.png",
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png"
    };

    /// <summary>
    /// A dictionary that tracks the ID assigned to each image URL when 
    /// the image is added to the list.
    /// </summary>
    /// <remarks>Indexed by URL.</remarks>
    private static readonly Dictionary<string, int> ImageIdMap =
        new Dictionary<string, int>();

    /// <summary>
    /// The name of the file to contain the output from the list management operations.
    /// </summary>
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private static string OutputFile = "ListOutput.log";

    /// <summary>
    /// A static reference to the text writer to use for logging.
    /// </summary>
    private static TextWriter writer;

    /// <summary>
    /// A copy of the list details.
    /// </summary>
    /// <remarks>Used to initially create the list, and later to update the
    /// list details.</remarks>
    private static Body listDetails;
   

> [!NOTE]
> A Content Moderator Szolgáltatáskulcs rendelkezik egy második (RPS) sávszélesség-korlátjának kérelemre, és ha túllépi a korlátot, az SDK kivételt 429 hibakód. 
>
> Ingyenes szint kulcs esetében egy függő Entitás sebessége.


## <a name="create-a-method-to-write-messages-to-the-log-file"></a>A naplófájl üzeneteket írni metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Writes a message to the log file, and optionally to the console.
    /// </summary>
    /// <param name="message">The message.</param>
    /// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
    private static void WriteLine(string message = null, bool echo = false)
    {
        writer.WriteLine(message ?? String.Empty);

        if (echo)
        {
            Console.WriteLine(message ?? String.Empty);
        }
    }

## <a name="create-a-method-to-create-the-custom-list"></a>Egyéni lista létrehozása metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Creates the custom list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The response object from the operation.</returns>
    private static ImageList CreateCustomList(ContentModeratorClient client)
    {
        // Create the request body.
        listDetails = new Body("MyList", "A sample list",
            new BodyMetadata("Acceptable", "Potentially racy"));

        WriteLine($"Creating list {listDetails.Name}.", true);

        var result = client.ListManagementImageLists.Create(
            "application/json", listDetails);
        Thread.Sleep(throttleRate);

        WriteLine("Response:");
        WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        return result;
    }

## <a name="create-a-method-to-add-a-collection-of-images-to-the-list"></a>Képek gyűjteménye hozzáadása a listához metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust.

Ez a rövid útmutató nem bemutatják, hogyan lehet a listában található rendszerképek címkékkel. 

    /// <summary>
    /// Adds images to an image list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="listId">The list identifier.</param>
    /// <param name="imagesToAdd">The images to add.</param>
    /// <param name="label">The label to apply to each image.</param>
    /// <remarks>Images are assigned content IDs when they are added to the list.
    /// Track the content ID assigned to each image.</remarks>
    private static void AddImages(
    ContentModeratorClient client, int listId,
    IEnumerable<string> imagesToAdd, string label)
    {
        foreach (var imageUrl in imagesToAdd)
        {
            WriteLine();
            WriteLine($"Adding {imageUrl} to list {listId} with label {label}.", true);
            try
            {
                var result = client.ListManagementImage.AddImageUrlInput(
                    listId.ToString(), "application/json", new BodyModel("URL", imageUrl), null, label);

                ImageIdMap.Add(imageUrl, Int32.Parse(result.ContentId));

                WriteLine("Response:");
                WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));
            }
            catch (Exception ex)
            {
                WriteLine($"Unable to add image to list. Caught {ex.GetType().FullName}: {ex.Message}", true);
            }
            finally
            {
                Thread.Sleep(throttleRate);
            }
        }
    }

## <a name="create-a-method-to-remove-images-from-the-list"></a>Képek eltávolítása a listából metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Removes images from an image list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="listId">The list identifier.</param>
    /// <param name="imagesToRemove">The images to remove.</param>
    /// <remarks>Images are assigned content IDs when they are added to the list.
    /// Use the content ID to remove the image.</remarks>
    private static void RemoveImages(
        ContentModeratorClient client, int listId,
        IEnumerable<string> imagesToRemove)
    {
        foreach (var imageUrl in imagesToRemove)
        {
            if (!ImageIdMap.ContainsKey(imageUrl)) continue;
            int imageId = ImageIdMap[imageUrl];

            WriteLine();
            WriteLine($"Removing entry for {imageUrl} (ID = {imageId}) from list {listId}.", true);

            var result = client.ListManagementImage.DeleteImage(
                listId.ToString(), imageId.ToString());
            Thread.Sleep(throttleRate);

            ImageIdMap.Remove(imageUrl);

            WriteLine("Response:");
            WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));
        }
    }

## <a name="create-a-method-to-get-all-of-the-content-ids-for-images-in-the-list"></a>A listában található rendszerképek lekérése azonosítók tartalom mindegyikét metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Gets all image IDs in an image list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="listId">The list identifier.</param>
    /// <returns>The response object from the operation.</returns>
    private static ImageIds GetAllImageIds(
        ContentModeratorClient client, int listId)
    {
        WriteLine();
        WriteLine($"Getting all image IDs for list {listId}.", true);

        var result = client.ListManagementImage.GetAllImageIds(listId.ToString());
        Thread.Sleep(throttleRate);

        WriteLine("Response:");
        WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        return result;
    }

## <a name="create-a-method-to-update-the-details-of-the-list"></a>A részletek a lista frissítéséhez metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Updates the details of an image list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="listId">The list identifier.</param>
    /// <returns>The response object from the operation.</returns>
    private static ImageList UpdateListDetails(
        ContentModeratorClient client, int listId)
    {
        WriteLine();
        WriteLine($"Updating details for list {listId}.", true);

        listDetails.Name = "Swimsuits and sports";

        var result = client.ListManagementImageLists.Update(
            listId.ToString(), "application/json", listDetails);
        Thread.Sleep(throttleRate);

        WriteLine("Response:");
        WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        return result;
    }

## <a name="create-a-method-to-retrieve-the-details-of-the-list"></a>A lista adatai olvashatók be metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust.

    /// <summary>
    /// Gets the details for an image list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="listId">The list identifier.</param>
    /// <returns>The response object from the operation.</returns>
    private static ImageList GetListDetails(
        ContentModeratorClient client, int listId)
    {
        WriteLine();
        WriteLine($"Getting details for list {listId}.", true);

        var result = client.ListManagementImageLists.GetDetails(listId.ToString());
        Thread.Sleep(throttleRate);

        WriteLine("Response:");
        WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        return result;
    }

## <a name="create-a-method-to-refresh-the-search-index-of-the-list"></a>A search-index a lista frissítéséhez metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust.

Bármikor listáját, frissítenie kell a keresési index frissítése előtt listájának képernyőképek a használatával.

    /// <summary>
    /// Refreshes the search index for an image list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="listId">The list identifier.</param>
    /// <returns>The response object from the operation.</returns>
    private static RefreshIndex RefreshSearchIndex(
        ContentModeratorClient client, int listId)
    {
        WriteLine();
        WriteLine($"Refreshing the search index for list {listId}.", true);

        var result = client.ListManagementImageLists.RefreshIndexMethod(listId.ToString());
        Thread.Sleep(throttleRate);

        WriteLine("Response:");
        WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        return result;
    }

## <a name="create-a-method-to-match-images-against-the-list"></a>Képek a lista alapján megfelelő metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Matches images against an image list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="listId">The list identifier.</param>
    /// <param name="imagesToMatch">The images to screen.</param>
    private static void MatchImages(
        ContentModeratorClient client, int listId,
        IEnumerable<string> imagesToMatch)
    {
        foreach (var imageUrl in imagesToMatch)
        {
            WriteLine();
            WriteLine($"Matching image {imageUrl} against list {listId}.", true);

            var result = client.ImageModeration.MatchUrlInput(
                    "application/json", new BodyModel("URL", imageUrl), listId.ToString());
            Thread.Sleep(throttleRate);

            WriteLine("Response:");
            WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));
        }
    }

## <a name="create-a-method-to-delete-all-images-from-the-list"></a>Az összes rendszerkép törlése a listáról metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Deletes all images from an image list.
    /// </summary>
    /// <param name="client">The Content Modertor client.</param>
    /// <param name="listId">The list identifier.</param>
    private static void DeleteAllImages(
        ContentModeratorClient client, int listId)
    {
        WriteLine();
        WriteLine($"Deleting all images from list {listId}.", true);

        var result = client.ListManagementImage.DeleteAllImages(listId.ToString());
        Thread.Sleep(throttleRate);

        WriteLine("Response:");
        WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));
    }

## <a name="create-a-method-to-delete-the-list"></a>A lista törlése metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Deletes an image list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="listId">The list identifier.</param>
    private static void DeleteCustomList(
        ContentModeratorClient client, int listId)
    {
        WriteLine();
        WriteLine($"Deleting list {listId}.", true);

        var result = client.ListManagementImageLists.Delete(listId.ToString());
        Thread.Sleep(throttleRate);

        WriteLine("Response:");
        WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));
    }

## <a name="create-a-method-to-retrieve-ids-for-all-image-lists"></a>Az összes rendszerképlisták azonosítók lekéréséhez metódus létrehozása

Adja hozzá a **Program** osztályhoz a következő metódust. 

    /// <summary>
    /// Gets all list identifiers for the client.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The response object from the operation.</returns>
    private static IList<ImageList> GetAllListIds(ContentModeratorClient client)
    {
        WriteLine();
        WriteLine($"Getting all image list IDs.", true);

        var result = client.ListManagementImageLists.GetAllImageLists();
        Thread.Sleep(throttleRate);

        WriteLine("Response:");
        WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        return result;
    }

## <a name="add-code-to-simulate-the-use-of-an-image-list"></a>Adja hozzá a kódot egy képlista használatának szimulálása

Adja hozzá a következő kódot a **fő** metódust.

Ez a kód szimulálja számos, a művelet, amely a definiálása és kezelése a listában, valamint listájának képernyőképek a használatával elvégezhető. A naplózási szolgáltatások lehetővé teszi a válaszobjektumok hozta a Content Moderator Service SDK-hívásokat.

    // Create the text writer to use for logging, and cache a static reference to it.
    using (StreamWriter outputWriter = new StreamWriter(OutputFile))
    {
        writer = outputWriter;

        // Create a Content Moderator client.
        using (var client = Clients.NewClient())
        {
            // Create a custom image list and record the ID assigned to it.
            var creationResult = CreateCustomList(client);
            if (creationResult.Id.HasValue)
            {
                // Cache the ID of the new image list.
                int listId = creationResult.Id.Value;

                // Perform various operations using the image list.
                AddImages(client, listId, Images.Sports.Urls, Images.Sports.Label);
                AddImages(client, listId, Images.Swimsuit.Urls, Images.Swimsuit.Label);
                        
                GetAllImageIds(client, listId);
                UpdateListDetails(client, listId);
                GetListDetails(client, listId);

                // Be sure to refresh search index
                RefreshSearchIndex(client, listId);

                // WriteLine();
                WriteLine($"Waiting {latencyDelay} minutes to allow the server time to propagate the index changes.", true);
                Thread.Sleep((int)(latencyDelay * 60 * 1000));

                // Match images against the image list.
                MatchImages(client, listId, ImagesToMatch);

                // Remove images
                RemoveImages(client, listId, Images.Corrections);

                // Be sure to refresh search index
                RefreshSearchIndex(client, listId);

                WriteLine();
                WriteLine($"Waiting {latencyDelay} minutes to allow the server time to propagate the index changes.", true);
                Thread.Sleep((int)(latencyDelay * 60 * 1000));

                // Match images again against the image list. The removed image should not get matched.
                MatchImages(client, listId, ImagesToMatch);

                // Delete all images from the list.
                DeleteAllImages(client, listId);

                // Delete the image list.
                DeleteCustomList(client, listId);

                // Verify that the list was deleted.
                GetAllListIds(client);
                }
            }

            writer.Flush();
            writer.Close();
            writer = null;
    }

    Console.WriteLine();
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();

## <a name="run-the-program-and-review-the-output"></a>Futtassa a programot, és tekintse át a kimenetet

A lista azonosítója és a tartalom és más kép minden alkalommal, amikor az alkalmazás futtatásához.
A naplófájl a program által írt rendelkezik a következő kimenet:

    Creating list MyList.
    Response:
    {
        "Id": 169642,
        "Name": "MyList",
        "Description": "A sample list",
        "Metadata": {
            "Key One": "Acceptable",
            "Key Two": "Potentially racy"
        }
    }

    Adding https://moderatorsampleimages.blob.core.windows.net/samples/sample4.png to list 169642 with label Sports.
    Response:
    {
        "ContentId": "169490",
        "AdditionalInfo": [
        {
            "Key": "Source",
            "Value": "169642"
        },
        {
            "Key": "ImageDownloadTimeInMs",
            "Value": "233"
        },
        {
            "Key": "ImageSizeInBytes",
            "Value": "2945548"
        }
        ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
            },
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_b4d3e20a-0751-4760-8829-475e5da33ce8"
    }

    Adding https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png to list 169642 with label Sports.
    Response:
    {
        "ContentId": "169491",
        "AdditionalInfo": [
        {
            "Key": "Source",
            "Value": "169642"
        },
        {
            "Key": "ImageDownloadTimeInMs",
            "Value": "215"
        },
        {
            "Key": "ImageSizeInBytes",
            "Value": "2440050"
        }
        ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
            },
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_cc1eb6af-2463-4e5e-9145-2a11dcecbc30"
    }

    Adding https://moderatorsampleimages.blob.core.windows.net/samples/sample9.png to list 169642 with label Sports.
    Response:
    {
        "ContentId": "169492",
        "AdditionalInfo": [
        {
            "Key": "Source",
            "Value": "169642"
        },
        {
            "Key": "ImageDownloadTimeInMs",
            "Value": "98"
        },
        {
            "Key": "ImageSizeInBytes",
            "Value": "1631958"
        }
        ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
            },
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_01edc1f2-b448-48cf-b7f6-23b64d5040e9"
    }

    Adding https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg to list 169642 with label Swimsuit.
    Response:
    {
        "ContentId": "169493",
        "AdditionalInfo": [
        {
            "Key": "Source",
            "Value": "169642"
        },
        {
            "Key": "ImageDownloadTimeInMs",
            "Value": "27"
        },
        {
            "Key": "ImageSizeInBytes",
            "Value": "17280"
        }
        ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
            },
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_41f7bc6f-8778-4576-ba46-37b43a6c2434"
    }

    Adding https://moderatorsampleimages.blob.core.windows.net/samples/sample3.png to list 169642 with label Swimsuit.
    Response:
    {
        "ContentId": "169494",
        "AdditionalInfo": [
        {
            "Key": "Source",
            "Value": "169642"
        },
        {
            "Key": "ImageDownloadTimeInMs",
            "Value": "129"
        },
        {
            "Key": "ImageSizeInBytes",
            "Value": "1242855"
        }
        ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
            },
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_61a48f33-eb55-4fd9-ac97-20eb0f3622a5"
    }

    Adding https://moderatorsampleimages.blob.core.windows.net/samples/sample4.png to list 169642 with label Swimsuit.
    Unable to add image to list. Caught Microsoft.CognitiveServices.ContentModerator.Models.APIErrorException: Operation returned an invalid status code 'Conflict'

    Adding https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png to list 169642 with label Swimsuit.
    Response:
    {
        "ContentId": "169495",
        "AdditionalInfo": [
        {
            "Key": "Source",
            "Value": "169642"
        },
        {
            "Key": "ImageDownloadTimeInMs",
            "Value": "65"
        },
        {
            "Key": "ImageSizeInBytes",
            "Value": "1088127"
        }
        ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
            },
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_1c1f3de4-58b9-4aa8-82fa-1b0f479f6d7c"
    }

    Getting all image IDs for list 169642.
    Response:
    {
        "ContentSource": "169642",
        "ContentIds": [
            169490,
            169491,
            169492,
            169493,
            169494,
            169495
    ],
    "Status": {
        "Code": 3000,
        "Description": "OK",
        "Exception": null
        },
    "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_0d017deb-38fa-4701-a7b1-5b6608c79da2"
    }

    Updating details for list 169642.
    Response:
    {
        "Id": 169642,
        "Name": "Swimsuits and sports",
        "Description": "A sample list",
        "Metadata": {
            "Key One": "Acceptable",
            "Key Two": "Potentially racy"
        }
    }

    Getting details for list 169642.
    Response:
    {
        "Id": 169642,
        "Name": "Swimsuits and sports",
        "Description": "A sample list",
        "Metadata": {
            "Key One": "Acceptable",
            "Key Two": "Potentially racy"
        }
    }

    Refreshing the search index for list 169642.
    Response:
    {
        "ContentSourceId": "169642",
        "IsUpdateSuccess": true,
        "AdvancedInfo": [],
        "Status": {
            "Code": 3000,
            "Description": "RefreshIndex successfully completed.",
            "Exception": null
            },
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_c72255cd-55a0-415e-9c18-0b9c08a9f25b"
    }
    Waiting 0.5 minutes to allow the server time to propagate the index changes.

    Matching image https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg against list 169642.
    Response:
    {
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_ec384878-dbaa-4999-9042-6ac986355967",
        "CacheID": null,
        "IsMatch": true,
        "Matches": [
            {
                "Score": 1.0,
                "MatchId": 169493,
                "Source": "169642",
                "Tags": [],
                "Label": "Swimsuit"
            }
        ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        }
    }

    Matching image https://moderatorsampleimages.blob.core.windows.net/samples/sample4.png against list 169642.
    Response:
    {
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_e9db4b8f-3067-400f-9552-d3e6af2474c0",
        "CacheID": null,
        "IsMatch": true,
        "Matches": [
            {
                "Score": 1.0,
                "MatchId": 169490,
                "Source": "169642",
                "Tags": [],
                "Label": "Sports"
            }
        ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
            }
    }

    Matching image https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png against list 169642.
    Response:
    {
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_25991575-05da-4904-89db-abe88270b403",
        "CacheID": null,
        "IsMatch": false,
        "Matches": [],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        }
    }

    Matching image https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png against list 169642.
    Response:
    {
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_c65d1c91-0d8a-4511-8ac6-814e04adc845",
        "CacheID": null,
        "IsMatch": true,
        "Matches": [
            {
                "Score": 1.0,
                "MatchId": 169495,
                "Source": "169642",
                "Tags": [],
                "Label": "Swimsuit"
            }
            ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        }
    }

    Removing entry for https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png (ID = 169495) from list 169642.
    Response:
    ""

    Refreshing the search index for list 169642.
    Response:
    {
        "ContentSourceId": "169642",
        "IsUpdateSuccess": true,
        "AdvancedInfo": [],
        "Status": {
            "Code": 3000,
            "Description": "RefreshIndex successfully completed.",
            "Exception": null
            },
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_b55a375e-30a1-4612-aa7b-81edcee5bffb"
    }

    Waiting 0.5 minutes to allow the server time to propagate the index changes.

    Matching image https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg against list 169642.
    Response:
    {
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_00544948-2936-489c-98c8-b507b654bff5",
        "CacheID": null,
        "IsMatch": true,
        "Matches": [
            {
                "Score": 1.0,
                "MatchId": 169493,
                "Source": "169642",
                "Tags": [],
                "Label": "Swimsuit"
            }
            ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        }
    }

    Matching image https://moderatorsampleimages.blob.core.windows.net/samples/sample4.png against list 169642.
    Response:
    {
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_c36ec646-53c2-4705-86b2-d72b5c2273c7",
        "CacheID": null,
        "IsMatch": true,
        "Matches": [
            {
                "Score": 1.0,
                "MatchId": 169490,
                "Source": "169642",
                "Tags": [],
                "Label": "Sports"
            }
            ],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        }
    }

    Matching image https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png against list 169642.
    Response:
    {
        TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_22edad74-690d-4fbc-b7d0-bf64867c4cb9",
        "CacheID": null,
        "IsMatch": false,
        "Matches": [],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        }
    }

    Matching image https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png against list 169642.
    Response:
    {
        "TrackingId": "WE_f0527c49616243c5ac65e1cc3482d390_ContentModerator.Preview_abd4a178-3238-4601-8e4f-cf9ee66f605a",
        "CacheID": null,
        "IsMatch": false,
        "Matches": [],
        "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        }
    }

    Deleting all images from list 169642.
    Response:
    "Reset Successful."

    Deleting list 169642.
    Response:
    ""

    Getting all image list IDs.
    Response:
    []


## <a name="next-steps"></a>További lépések

Első a [Content Moderator .NET SDK-val](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) és a [Visual Studio-megoldás](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) ennél és a többi a Content Moderator rövid útmutató a .NET-hez, és az integrációval kapcsolatos első lépések.

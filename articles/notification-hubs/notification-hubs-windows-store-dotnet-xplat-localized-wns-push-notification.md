---
title: Honosított értesítések küldése Windows-alkalmazásokba az Azure Notification Hubs használatával | Microsoft Docs
description: Ebből az oktatóanyagból megtudhatja, hogy hogyan küldhet honosított értesítéseket a friss hírekről az Azure Notification Hubs használatával.
services: notification-hubs
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 0e78b00e49b2ef468e693abfdcde8a138313ba63
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/15/2018
ms.locfileid: "41919141"
---
# <a name="tutorial-push-localized-notifications-to-windows-apps-by-using-azure-notification-hubs"></a>Oktatóanyag: Honosított leküldéses értesítések küldése Windows-alkalmazásokba az Azure Notification Hubs használatával
> [!div class="op_single_selector"]
> * [Windows Áruház C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt mutatja be, hogy hogyan lehet honosított leküldéses értesítéseket küldeni a Notification Hubs szolgáltatásban regisztrált mobileszközökre. Ebben az oktatóanyagban az [Oktatóanyag: Értesítések küldése adott eszközökre (Univerzális Windows-platform)](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) oktatóanyag során létrehozott alkalmazásokat fogja frissíteni az alábbi forgatókönyvek támogatásához: 

- A Windows Áruházbeli alkalmazás lehetővé teszi az ügyféleszközök számára nyelvek megadását, valamint a feliratkozást a különböző frisshír-kategóriákra.
- A háttéralkalmazás értesítéseket küld, az Azure Notification Hubs **címke** és **sablon** funkcióját használva.

Az oktatóanyag befejezését követően a mobilalkalmazás lehetővé teszi a felhasználót érdeklő kategóriákra való regisztrálást, valamint azt, hogy a felhasználó, milyen nyelven szeretné fogadni az értesítéseket. A háttéralkalmazás nyelv és eszköz alapján honosított értesítéseket küld. 

Eben az oktatóanyagban az alábbiakkal fog megismerkedni: 

> [!div class="checklist"]
> * A Windows-alkalmazás frissítése a területi beállítási adatok támogatására
> * A háttéralkalmazás frissítése honosított értesítések küldéséhez
> * Az alkalmazás tesztelése

## <a name="prerequisites"></a>Előfeltételek
Végezze el az [Oktatóanyag: Értesítések küldése adott eszközökre (Univerzális Windows-platform)](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) című oktatóanyagot. 

Az [Oktatóanyag: Értesítések küldése adott eszközökre (Univerzális Windows-platform)](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) oktatóanyag során létrehozott egy alkalmazást, amely **címkék** használatával iratkozott fel a hírek különböző **kategóriáira** vonatkozó értesítésekre. Ebben az oktatóanyagban a Notification Hubs **sablon** funkcióját használva fog **honosított** értesítéseket küldeni a friss hírekkel kapcsolatban.

A sablonokkal lényegében azt a formátumot adhatja meg, amelyben egy adott eszköznek fogadnia kell az értesítéseket. A sablon meghatározza a hasznos adatok pontos formátumát a háttéralkalmazás által küldött üzenet részét képező tulajdonságokra hivatkozva. Ebben az oktatóanyagban a háttéralkalmazás egy területi beállításoktól független üzenetet küld el, amely minden támogatott nyelvet tartalmaz:

```json
{
    "News_English": "...",
    "News_French": "...",
    "News_Mandarin": "..."
}
```

Az eszközök egy sablonnal regisztrálnak, amely a megfelelő tulajdonságra hivatkozik. A bejelentési üzeneteket angol nyelven fogadni kívánó Windows Áruházbeli alkalmazás például az alábbi sablonra regisztrál a megfelelő címkékkel:

```xml
<toast>
    <visual>
    <binding template=\"ToastText01\">
        <text id=\"1\">$(News_English)</text>
    </binding>
    </visual>
</toast>
```

A sablonokkal kapcsolatos további információkért tekintse meg a [Sablonok](notification-hubs-templates-cross-platform-push-messages.md) című cikket. 

## <a name="update-windows-app-to-support-locale-information"></a>A Windows-alkalmazás frissítése a területi beállítási adatok támogatására

1. Nyissa meg az [Oktatóanyag: Értesítések küldése adott eszközökre (Univerzális Windows-platform)](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) című oktatóanyaghoz létrehozott Visual Studio-megoldást. 
2. Frissítse a **MainPage.xaml** fájlt, hogy az tartalmazza a területi beállítások kombinált listáját:

    ```xml
    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>
    ```
2. A **Notifications** osztályban adjon hozzá egy területi beállítási paramétert a **StoreCategoriesAndSubscribe** és a **SubscribeToCateories** metódushoz.

    ```csharp   
    public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
    {
        ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
        ApplicationData.Current.LocalSettings.Values["locale"] = locale;
        return await SubscribeToCategories(categories);
    }

    public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        if (categories == null)
        {
            categories = RetrieveCategories();
        }

        // Using a template registration. This makes supporting notifications across other platforms much easier.
        // Using the localized tags based on locale selected.
        string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

        return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
    }
    ```

    A *RegisterNativeAsync* metódus meghívása helyett hívja meg a *RegisterTemplateAsync* metódust. Egy adott értesítésformátumra kell regisztrálnia, amelyben a sablon a területi beállítástól függ. A sablont el is kell neveznie (például: „localizedWNSTemplateExample”), mert érdemes egynél több sablont regisztrálni (például egyet a bejelentési értesítésekhez és egyet a csempékhez). A sablonok elnevezése a frissítésükhöz és törlésükhöz is szükséges.
   
    Ha egy eszköz több, ugyanazon címkével rendelkező sablont is regisztrál, az adott címkét célzó bejövő üzenetekről több értesítés is érkezik az adott eszközre (egy-egy mindegyik sablon esetén). Ez a működés akkor hasznos, ha egyetlen logikai üzenetnek több látható értesítést kell eredményeznie, például egy jelvény megjelenítését és egy bejelentést a Windows Áruházbeli alkalmazásban.
3. Adja hozzá az alábbi metódust a tárolt területi beállítás lekéréséhez:
   
    ```csharp
    public string RetrieveLocale()
    {
        var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
        return locale != null ? locale : "English";
    }
    ```

4. A **MainPage.xaml.cs** fájlban frissítése a gombkattintás-kezelőt a Locale (Területi beállítás) kombinált lista aktuális értékének lekérésével, és az értéknek a Notifications osztály meghívásában való megadásával az alább látható módon:
   
    ```csharp
    private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
    {
        var locale = (string)Locale.SelectedItem;

        var categories = new HashSet<string>();
        if (WorldToggle.IsOn) categories.Add("World");
        if (PoliticsToggle.IsOn) categories.Add("Politics");
        if (BusinessToggle.IsOn) categories.Add("Business");
        if (TechnologyToggle.IsOn) categories.Add("Technology");
        if (ScienceToggle.IsOn) categories.Add("Science");
        if (SportsToggle.IsOn) categories.Add("Sports");

        var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                categories);

        var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
            string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    ```
4. Végül az App.xaml.cs fájlban frissítse az `InitNotificationsAsync` metódust a területi beállítás lekérésére, és a feliratkozáskor való használatára:

    ```csharp   
    private async void InitNotificationsAsync()
    {
        var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

        // Displays the registration ID so you know it was successful
        if (result.RegistrationId != null)
        {
            var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

## <a name="send-localized-notifications-from-your-back-end"></a>Honosított értesítések küldése a háttérrendszerből
Sablonértesítések küldésekor csak egy tulajdonságkészletet kell megadnia. Ebben az oktatóanyagban a háttéralkalmazás az aktuális hírek honosított verzióját tartalmazó tulajdonságkészletet küldi el, például:

```json
{
    "News_English": "World News in English!",
    "News_French": "World News in French!",
    "News_Mandarin": "World News in Mandarin!"
}
```

Ebben a szakaszban frissíteni fogja a megoldás konzolalkalmazás projektjét. Módosítsa a `SendTemplateNotificationAsync` metódust a korábban létrehozott konzolalkalmazásban az alábbi kóddal: 

> [!IMPORTANT]
> A kódban adja meg az értesítési központhoz teljes hozzáférést biztosító nevet és kapcsolati sztringet. 


```csharp
private static async void SendTemplateNotificationAsync()
{
    // Define the notification hub.
    NotificationHubClient hub = 
        NotificationHubClient.CreateClientFromConnectionString(
            "<connection string with full access>", "<hub name>");

    // Sending the notification as a template notification. All template registrations that contain 
    // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
    // This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string, string> templateParams = new Dictionary<string, string>();

    // Create an array of breaking news categories.
    var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
    var locales = new string[] { "English", "French", "Mandarin" };

    foreach (var category in categories)
    {
        templateParams["messageParam"] = "Breaking " + category + " News!";

        // Sending localized News for each tag too...
        foreach( var locale in locales)
        {
            string key = "News_" + locale;

            // Your real localized news content would go here.
            templateParams[key] = "Breaking " + category + " News in " + locale + "!";
        }

        await hub.SendTemplateNotificationAsync(templateParams, category);
    }
}
```

Ez az egyszerű hívás platformtól függetlenül az **összes** eszközre elküldi a honosított híreket, mivel az értesítési központ a létrehozott natív hasznos adatokat az összes olyan eszköz számára létrehozza és elküldi, amely feliratkozott egy adott címkére.

## <a name="test-the-app"></a>Az alkalmazás tesztelése
1. Futtassa a Windows Áruházbeli univerzális alkalmazást. Várjon, amíg megjelenik a **Registration successful** (Sikeres regisztráció) üzenet.

    ![Mobilalkalmazás és regisztráció](./media/notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification/registration-successful.png)
1. Jelölje ki a **kategóriákat** és a **területi beállítást**, majd kattintson a **Subscribe** (Feliratkozás) lehetőségre. Az alkalmazás címkékké alakítja át a kiválasztott kategóriákat, és új eszközregisztrációt kezdeményez az értesítési központban a kiválasztott címkékre vonatkozóan.

    ![Mobilalkalmazás](./media/notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification/mobile-app.png)
2.  Egy **megerősítő** üzenet jelenik meg a **feliratkozásokkal** kapcsolatban. 

    ![Feliratkozási üzenet](./media/notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification/subscription-message.png)
1. Ha megkapta a megerősítést, futtassa a **konzolalkalmazást**, amellyel értesítéseket küldhet az egyes kategóriákra vonatkozóan az összes támogatott nyelven. Ellenőrizze, hogy csak azon kategóriákra vonatkozóan kap-e értesítéseket, amelyekre feliratkozott, és hogy az üzenetek megfelelnek-e a kiválasztott területi beállításnak. 

    ![Értesítési üzenetek](./media/notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification/notifications.png)
 

## <a name="next-steps"></a>További lépések
Ebben az oktatóanyagban elsajátította, hogy hogyan küldhet honosított értesítéseket olyan adott eszközökre, amelyek a regisztrációjukhoz társított címkékkel rendelkeznek. Ha szeretné megtudni, hogy hogyan küldhet értesítéseket adott felhasználóknak, akik egynél több eszközt is használhatnak, lépjen tovább a következő oktatóanyagra: 

> [!div class="nextstepaction"]
>[Leküldéses értesítések küldése adott felhasználóknak](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md)


<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Notify users with Notification Hubs: Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs/notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx

---
title: Az ASP.NET core Azure Linux webalkalmazások Application Insights Profilkészítő profil |} Microsoft Docs
description: A koncepció áttekintése és a részletes útmutatót engedélyezheti a
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: 63a7ceacffe1ee33227d3a8272dda7de7b3b1135
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/23/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Profil ASP.NET Core Azure Linux-webalkalmazásokban az Application Insights Profilkészítő

Ez a funkció jelenleg előzetes verzió

Annak megállapítása, mennyi időt töltött az egyik módszer az élő webes alkalmazás használatakor [Application Insights](app-insights-overview.md). Profilkészítő már elérhető az alkalmazásszolgáltatások Linux üzemeltetett ASP.NET core webes alkalmazásokhoz. Ez az útmutató lépésről lépésre utasítás a szolgáltatásprofil-elemzői adat hogyan gyűjthetők össze az ASP.NET core Linux web Apps.

Ez a forgatókönyv befejezése után az alkalmazás az alábbi képernyőképet hasonló szolgáltatásprofil-elemzői adat fog gyűjteni. Ebben a példában a Profilkészítő nyomkövetési azt jelzi, egy adott webes kérelem nem lassú, mert a legtöbb idő telhet el a várakozási. A gyakran használt adatok elérési útja a kódban, amely az alkalmazás lelassítják a lángmegelőzési ikon előtt. Ez a példa bemutatja `About` metódus a `HomeController` lassítsa le a webalkalmazást, mert ez volt hívja `Thread.Sleep`.

![Szolgáltatásprofil-elemzői adat](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="pre-requisites"></a>Előfeltételek
Az alábbi útmutató a Windows, Linux és Mac fejlesztői környezetek alkalmazza:

* Telepítés [.NET core SDK 2.1.2 vagy újabb verzió](https://www.microsoft.com/net/download/windows/build)
* Telepítse a Git található utasítások segítségével: a következő [első lépések – Git telepítése](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## <a name="setup-project-locally"></a>A telepítő-projekt helyi küldése

1. Nyisson meg egy parancssort a számítógépen. Az alábbi utasításokat az összes Windows, Linux és Mac fejlesztői környezetben működik.

2. Az ASP.NET core MVC webalkalmazás létrehozása
    ```
    dotnet new mvc -n LinuxProfilerTest
    ```
3. Módosítsa a könyvtárat a parancssort a projekt gyökérmappájába

4. Adja hozzá a Nuget-csomagot a szolgáltatásprofil-elemzői adat gyűjtéséhez.
    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```
5. Adja hozzá a kódsort a HomeController.cs néhány másodpercen belül véletlenszerű késleltetése

    ```csharp
        using System.Threading;
        ...

        public IActionResult About()
            {
                Random r = new Random();
                int delay = r.Next(5000, 10000);
                Thread.Sleep(delay);
                return View();
            }
    ```
6. A helyi tárházba mentéséhez és a módosítások végrehajtásához

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-azure-app-service-for-hosting-your-project"></a>Azure App Service létrehozása a projekthez üzemeltetéséhez
1. App Services Linux környezet létrehozása

    ![Hozzon létre Linux App Service szolgáltatások](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Hozza létre a központi telepítési hitelesítő adatokat. Jegyezze fel a jelszót később szüksége lesz a az alkalmazás központi telepítésekor.

    ![Üzembe helyezési hitelesítő adatok létrehozása](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Válassza ki a központi telepítési lehetőség. Állítson be egy helyi Git-tárházat az Azure-portál utasításai web app alkalmazásban. A Git-tárház automatikusan jön létre.

    ![Git-tárház beállítása](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Több központi telepítési beállítások érhetők el [Itt](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type)

## <a name="deploy-your-project"></a>A projekt telepítése

1. A parancssorban keresse meg a projekt legfelső szintű mappát. Mutasson a alkalmazásszolgáltatások ezen távoli Git-tárház hozzáadása:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```
    * Használja a "create telepítési hitelesítő adatokhoz." lépés a "username" adat.
    * Használja az "app service létrehozása" lépésben (app name)

2. A módosítások Azure küldésével a projekt telepítése

    ```
    git push azure master
    ```
A következőhöz hasonló kimenetet fog látni:

    ```
    Counting objects: 9, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (9/9), 1.78 KiB | 911.00 KiB/s, done.
    Total 9 (delta 3), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id 'd7369a99d7'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling ASP.NET Core Web Application deployment.
    remote: ......
    remote:   Restoring packages for /home/site/repository/EventPipeExampleLinux.csproj...
    remote: .
    remote:   Installing Newtonsoft.Json 10.0.3.
    remote:   Installing Microsoft.ApplicationInsights.Profiler.Core 1.1.0-LKG
    …

    ```

## <a name="add-application-insights-to-monitor-your-web-apps"></a>A webalkalmazások figyeléséhez az Application Insights hozzáadása
1. [Az Application Insights-erőforrás létrehozása](./app-insights-create-new-resource.md)
2. Az Application Insights-erőforrás iKey másolja, majd adja meg a következő beállításokat az App Service szolgáltatások

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![Alkalmazás beállítása](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    Beállítások módosítása a hely automatikusan újraindul. Miután az új beállításokat alkalmazzák, a Profilkészítő azonnal 2 percig futása elindul. azt a rendszer futtatja a 2 perces óránként.

3. A webhely néhány adatforgalmat generálnak. A webhely is frissítheti ```About``` lap néhány alkalommal.

4. Várjon, amíg a 2 – 5 percig, az eseményeket az Application Insights összesíthetők.

5. Nyissa meg az Application Insights teljesítmény ablaktáblán az Azure portálon. Szolgáltatásprofil-elemzői adat érhető el a jobb alsó sarokban jelenik meg.

    ![Nézetben nyomainak](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Ismert problémák

### <a name="enable-button-in-profiler-configuration-pane-does-not-work"></a>A Profilkészítő konfigurációs ablaktáblában nem működik gomb
**Ha az App Services Linux használó alkalmazás működteti, nem kell újra engedélyezni Profilkészítő App Insights portálon találja meg teljesítmény paneljén. Beleértve a NuGet-csomagot a projekt és az App Insights beállítása iKey az alkalmazás beállításai megfelelőek Profilkészítő engedélyezése**.

Ha követi a [App Insights Profilkészítő for Windows](./app-insights-profiler.md) engedélyezése a munkafolyamatot kattintson **engedélyezése** konfigurálása Profilkészítő ablaktáblában, hibaüzenetet kap, a gomb megpróbálja telepíteni a Windows-verzión Profilkészítő ügynök a Linux-környezeten.

A engedélyezése élményt nyújt a probléma megoldásán dolgozunk.

![Nem működik a Linux alkalmazásszolgáltatások Profilkészítő el újra a teljesítmény ablaktáblán a Profilkészítő engedélyezni kell](./media/app-insights-profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>További lépések
Ha egyéni tárolók alkalmazásszolgáltatások által üzemeltetett használ, kövesse az utasításokat a [ szolgáltatásprofil-elemzőben engedélyezése indexelése ASP.NET Core alkalmazás](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) App Insights Profilkészítő engedélyezése

Ha bármely problémák vagy a javaslatok, jelentse a github-adattár: [ApplicationInsights-Profiler-AspNetCore: problémák](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues)

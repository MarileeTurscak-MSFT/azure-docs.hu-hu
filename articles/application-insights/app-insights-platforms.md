---
title: 'Application Insights: nyelvek, platformok és integrációk| Microsoft Docs'
description: Az Application Insightshoz elérhető nyelvek, platformok és integrációk
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 09/01/2016
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 4f474ad234c80a0dcb5a9f704a263a97e7df0cc1
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174366"
---
# <a name="developer-analytics-languages-platforms-and-integrations"></a>Fejlesztői elemzések: nyelvek, platformok és integrációk
Ezen elemek az [Application Insights](app-insights-overview.md) azon megvalósításai, amelyekről hallottunk, beleértve néhány harmadik fél által létrehozottat.

## <a name="languages---officially-supported-by-application-insights-team"></a>Az Application Insights csapata által hivatalosan támogatott nyelvek
* [C#|VB (.NET)](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [JavaScript-weblapok](app-insights-javascript.md)
* [Node.JS](app-insights-nodejs.md)

## <a name="languages---community-supported"></a>Közösség által támogatott nyelvek
* [F#](https://safe-stack.github.io/docs/template-azure-ai/)
* [PHP](https://github.com/Microsoft/ApplicationInsights-PHP)
* [Python](https://pypi.python.org/pypi/applicationinsights/0.1.0)
* [Ruby](https://rubygems.org/gems/application_insights)
* [Bármi más](#projects)

## <a name="platforms-and-frameworks"></a>Platformok és keretrendszerek
* [ASP.NET](app-insights-asp-net.md)
* [ASP.NET – már élő alkalmazásokhoz](app-insights-monitor-performance-live-website-now.md)
* [ASP.NET Core](app-insights-asp-net-core.md)
* [Android](app-insights-mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [Angular](https://github.com/MarkPieszak/angular-application-insights)
* [Azure Web Apps](app-insights-azure-web-apps.md)
* [Azure Cloud Services](app-insights-cloudservices.md)&#151;beleértve a webes és a feldolgozói szerepköröket
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample)
* [Docker](app-insights-docker.md)
* [Glimpse](https://azure.microsoft.com/blog/glimpse-application-insights/)
* [iOS](app-insights-mobile-center-quickstart.md) (App Center)
* [Ionic](https://github.com/SoftwarePioniere/ionic-application-insights)
* [iOS](https://github.com/Microsoft/ApplicationInsights-iOS) (App Center)
* [J2EE](app-insights-java-get-started.md)
* [J2EE – már élő alkalmazásokhoz](app-insights-java-live.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [OSX](https://github.com/Microsoft/ApplicationInsights-OSX)
* [SAFE Stack](https://safe-stack.github.io/docs/template-azure-ai/)
* [Spring](http://joe.blog.freemansoft.com/2015/12/enabling-microsoft-application-insight.html)
* [Univerzális Windows-alkalmazás](app-insights-mobile-center-quickstart.md) (App Center)
* [WCF](https://github.com/Microsoft/ApplicationInsights-SDK-Labs/blob/master/WCF/readme.md)
* [Asztali Windows-alkalmazások, szolgáltatások és feldolgozói szerepkörök](app-insights-windows-desktop.md)
* [Bármi más](#projects)

## <a name="logging-frameworks"></a>Naplózási keretrendszerek
* [Log4Net, NLog, vagy System.Diagnostics.Trace](app-insights-asp-net-trace-logs.md)
* [Java, Log4J, vagy Logback](app-insights-java-trace-logs.md)
* [Szemantikus naplózás (SLAB)](https://github.com/fidmor89/SLAB_AppInsights) – integrálható a [szemantikus naplózási alkalmazásblokkal](https://msdn.microsoft.com/library/dn440729.aspx)
* [Felhőalapú terheléses tesztelés](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/30/getting-application-insights-counters-with-cloud-based-load-testing.aspx)
* [LogStash beépülő modul](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)
* [Logary](https://www.nuget.org/packages/Logary.Targets.AppInsights/)
* [Logrus](https://github.com/jjcollinge/logrus-appinsights)

## <a name="content-management-systems"></a>Tartalomkezelő rendszerek
* [Concrete](https://github.com/fidmor89/appInsights-Concrete)
* [Drupal](https://github.com/fidmor89/AppInsights-Drupal)
* [Joomla](https://github.com/fidmor89/AppInsights-Joomla)
* [Orchard](https://azure.microsoft.com/blog/integrating-application-insights-into-a-modular-cms-and-a-multi-tenant-public-saas/preview/)
* [SharePoint](app-insights-sharepoint.md)
* [WordPress](https://wordpress.org/plugins/application-insights/)

## <a name="export-and-data-analysis"></a>Exportálás és adatelemzés
* [Alooma](https://www.alooma.com/blog/application-insights-amazon-redshift)
* [Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](app-insights-export-power-bi.md)

## <a name="projects"></a> Saját SDK kialakítása
Ha még nem készült SDK az Ön nyelvéhez vagy platformjához, akár Ön is létrehozhat egyet. Tekintse meg a meglévő SDK-k kódjainak listáját a [GitHubon elérhető Application Insights SDK-projektben](https://github.com/Microsoft/AppInsights-Home).

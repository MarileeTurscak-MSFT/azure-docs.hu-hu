---
title: Még több előny az Azure Application Insights |} Microsoft Docs
description: Első lépések az Application insights szolgáltatással, miután ez ismerje meg a szolgáltatások összegzését.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: 2f03083367de4e818bdc953ab76c28ff687f0a48
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294336"
---
# <a name="more-telemetry-from-application-insights"></a>Az Application Insights további telemetria
Miután [Application Insights hozzá az ASP.NET-kódban](app-insights-asp-net.md), néhány dolgot még több telemetriai adatokat is van. 

| Műveletek | Amihez jut|
|---|---|
|(IIS-kiszolgálókkal) [Állapotmonitor telepítése](http://go.microsoft.com/fwlink/?LinkId=506648) a minden egyes kiszolgáló-számítógépén.<br/>(Az azure web apps) A webalkalmazás Azure Vezérlőpult nyissa meg az Application Insights panelt.| [**Teljesítményszámlálók**](app-insights-performance-counters.md)<br/>[**Kivételek** ](app-insights-asp-net-exceptions.md) – részletes kivételeseményekhez megadhat veremkiíratásokat,<br/>[**Függőségek**](app-insights-asp-net-dependencies.md)|
|[A JavaScript részlet felvétele a weblapok](app-insights-javascript.md)|[Teljesítmény lapon](app-insights-web-track-usage.md), böngésző kivételeket, az AJAX-teljesítmény. Az ügyféloldali egyéni telemetriai adatokat.|
|[Rendelkezésre állási webteszt létrehozása](app-insights-monitor-web-app-availability.md)|Riasztásokat kaphat, ha a webhely nem érhető el.|
|[Győződjön meg arról buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) MSBuild állítja elő|[Jegyzetek metrika diagramokban összeállítása](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Egyéni események és metrikák írása](app-insights-api-custom-events-metrics.md)|Az üzleti eseményeket és metrikákat száma, nyomon követheti a részletes használati és még sok más.|
|[Élő webhelyét profilját](https://aka.ms/AIProfilerPreview)|Az élő webalkalmazását részletes függvény időzítés|







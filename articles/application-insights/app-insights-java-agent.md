---
title: Az Azure Application Insights Java-webalkalmazások alkalmazásteljesítmény-figyelés |} A Microsoft Docs
description: A kiterjesztett teljesítmény és használat monitorozása az Application insights szolgáltatással Java webhelyét.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: 30983e283f47761d103829f02b02bc281bd785ee
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091932"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Függőségek, kivételek kivétel történt, és metódus végrehajtási időpontok a Java-webalkalmazások monitorozása


Ha rendelkezik [kialakítva az Application Insights Java-webalkalmazását][java], a Java ügynököt segítségével részletesebb elemzéseket kódváltoztatás nélkül:

* **Függőségek:** hívások egyéb összetevőivel, például az alkalmazás által adatait:
  * **REST-hívások** HttpClient keresztül történik, OkHttp és RestTemplate (Spring) rögzítve lesznek.
  * **Redis Cache** a Jedis ügyfél-n keresztül végzett hívások rögzítve lesznek.
  * **[JDBC-hívások](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, az SQL Server és Oracle DB parancsok automatikusan rögzítve lesznek. A MySQL a hívás hosszabb időt vesz igénybe, mint a 10s, ha az ügynök jelentéseket küld a lekérdezésterv.
* **Kivétel történt:** a kód által kezelt kivételek kapcsolatos információkat.
* **Metódus végrehajtási idő:** bizonyos eljárások végrehajtásához szükséges kapcsolatos információkat.

A Java ügynököt használatához telepítheti a kiszolgálón. A web apps kell lesznek tagolva a [Application Insights Java SDK][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>A Javához készült Application Insights-ügynök telepítése
1. A gépen, amelyen fut a Java kiszolgáló [töltse le az ügynököt](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). Ellenőrizze, hogy az azonos verson Java-ügynök az Application Insights Java SDK core és a web csomag letöltéséhez.
2. Az alkalmazás-kiszolgáló indítási parancsfájl szerkesztése, és adja hozzá az alábbi JVM:
   
    `javaagent:`*az ügynök JAR-fájl teljes elérési útja*
   
    Például a Linux rendszerű gépen Tomcat:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Indítsa újra az alkalmazáskiszolgáló.

## <a name="configure-the-agent"></a>Az ügynök konfigurálása
Hozzon létre egy fájlt `AI-Agent.xml` és helyezze ugyanabba a mappába, az ügynök JAR-fájlt.

Állítsa be az XML-fájl tartalmát. Szerkessze a következő példa a azt szeretné, vagy hagyja ki a szolgáltatásokat.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Jelentések kivétel- és az egyes módszerek metódus időzítési engedélyeznie kell.

Alapértelmezés szerint `reportExecutionTime` IGAZ és `reportCaughtExceptions` false (hamis).

## <a name="view-the-data"></a>Az adatok megtekintése
Összesített távoli függőség- és metódus végrehajtási időpontok jelenik meg az Application Insights-erőforrást [alatt a teljesítmény csempéje][metrics].

Keresse meg az egyes példányok függőségi, kivételek és módszer jelentéseknek, nyissa meg a [keresési][diagnostic].

[Diagnosztizálás függőségi kapcsolatos problémák – további](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Kérdései vannak? Problémákat tapasztal?
* Nincs adat? [Végezze el a tűzfalkivételek beállítása](app-insights-ip-addresses.md)
* [A Java hibaelhárítása](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md

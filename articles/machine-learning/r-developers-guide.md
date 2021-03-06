---
title: R – fejlesztői útmutató az Azure-bA – az R programozási |} A Microsoft Docs
description: Ez a cikk áttekintést a különféle módokon, hogy az adatszakértők az R-rel kihasználhatják a meglévő szaktudásukkal programozási nyelv, az Azure-ban. Az Azure számos olyan szolgáltatás, amelyek az R-fejlesztők kihasználhatják a saját adatok adatelemzési számítási feladatok a felhőbe kiterjeszteni kínál.
services: machine-learning
documentationcenter: ''
author: AnalyticJeremy
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: R
ms.topic: article
ms.date: 09/12/2018
ms.author: jepeach
ms.openlocfilehash: bc00bd3b61398355c663d133c0c9a66c2a52aa8d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047634"
---
# <a name="r-developers-guide-to-azure"></a>R – fejlesztői útmutató az Azure-bA
<img src="media/r-developers-guide/logo_r.svg" alt="R logo" align="right" width="200" />

Számos adatszakértők foglalkoznak, egyre növekvő adatmennyiségek módon használja ki a felhő-számítástechnika a elemzések keres.  Ez a cikk a különféle módokon, hogy az adatszakértők terén már megszerzett tudásukat áttekintést nyújt a [R programozási nyelv](https://www.r-project.org) az Azure-ban.

A Microsoft rendelkezik teljes elfogadott az R programozási nyelv kiváló eszköz adatszakértők számára.  Azáltal, hogy több lehetőség is R fejlesztők saját kód futtatásához az Azure-ban, a vállalat van engedélyezése az adatszakértők, az adatok adatelemzési számítási feladatok a felhőbe kiterjeszteni, nagyszabású projektekhez hozzáadásával.

Most vizsgálja meg a különböző lehetőségek és az legfigyelemreméltóbb forgatókönyveket minden egyes.

## <a name="azure-services-with-r-language-support"></a>Az R nyelv támogatása az Azure-szolgáltatások
Ez a cikk ismerteti a következő Azure-szolgáltatások, amelyek támogatják az R nyelv:

|Szolgáltatás                                                          |Leírás                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
|[Adatelemző virtuális gép](#data-science-virtual-machine)    |a data science munkaállomás vagy egy egyéni számítás célként használandó egyéni virtuális gép|
|[A HDInsight Machine Learning-szolgáltatások](#ml-services-on-hdinsight)            |fürt-alapú rendszer R elemzések futtatása nagyméretű adathalmazok számos csomópont között   |
|[Az Azure Databricks](#azure-databricks)                            |együttműködési a Spark környezet, amely támogatja az R- és más nyelveken               |
|[Az Azure Machine Learning Studióban](#azure-machine-learning-studio)  |az Azure machine learning-példakísérleteket az egyéni R-szkriptek futtatása                      |
|[Azure Batch](#azure-batch)                                      |számos lehetőséget kínál a gazdasági szempontból a fürt több csomópontján futó R-kód|
|[Az Azure notebookok](#azure-notebooks)                              |egy ingyenes (de legfeljebb) felhőalapú verziója Jupyter notebookok                  |
|[Azure SQL Database](#azure-sql-database)                        |az SQL Server adatbázismotor alapjait az R-szkriptek futtatása                            |

## <a name="data-science-virtual-machine"></a>Adatelemzési virtuális gép
A [adatelemző virtuális gép](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) (DSVM) egy személyre szabott Virtuálisgép-lemezkép a Microsoft Azure felhőalapú platformon, amelyet kifejezetten adatelemzésre. Számos népszerű adatelemzési eszközök, többek között van:
* [A Microsoft R Open](https://mran.microsoft.com/open/)
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
* [Az RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop)
* [Az RStudio Server](https://www.rstudio.com/products/rstudio/#Server)

A DSVM üzembe lehet helyezni Windows vagy Linux operációs rendszert.  A dsvm-hez két különböző módon használható: egy interaktív munkaállomás vagy egy egyéni fürt egy számítási platform.

### <a name="as-a-workstation"></a>Munkaállomásként
Ha szeretne – első lépések az R a felhőben gyorsan és egyszerűen, akkor a legjobb megoldás.  A környezet számára, akik a helyi munkaállomáson dolgozott az r nyelv ismerősek lesznek.  Azonban helyett használja a helyi erőforrások, az R-környezetben futtat egy virtuális Gépet a felhőben.  Ha az adatok tárolása már az Azure-ban, ez rendelkezik-e további előnye, hogy az R-szkriptek futtatása "az adatok közelebb." Az adatok továbbítására az interneten keresztül, az adatok elérhetők az Azure belső hálózaton keresztül, amely sokkal gyorsabb hozzáférést biztosít.

A DSVM R fejlesztői csapatok különösen hasznos lehet.  Portfóliónk fejlesztésén hatékony munkaállomásokat minden fejlesztő részére, és megköveteli a csapat tagjai a fogják használni, különböző szoftvercsomagok mely verzióin szinkronizálni, helyett minden fejlesztő hozhatók létre újabb webpéldányok egy példányát a dsvm-hez, amikor szükséges.

### <a name="as-a-compute-platform"></a>Számítási platform
Mellett munkaállomásként használja, a dsvm-hez is használatos egy rugalmasan méretezhető számítási platform az R-projektekhez.  Használatával a <code>[AzureDSVM](https://github.com/Azure/AzureDSVM)</code> R-csomag létrehozását és törlését a DSVM-példányok programozott módon szabályozhatja.  A példányok egy fürtbe alkotnak, és üzembe helyezése egy elosztott elemzési hajtható végre, a felhőben.  A teljes folyamatot szabályozhatja a helyi munkaállomáson futó R-kód.

A DSVM kapcsolatos további információkért tekintse meg a ["Bevezetés az Azure adatelemzési virtuális gép Linux és Windows."](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview)

## <a name="ml-services-on-hdinsight"></a>ML-szolgáltatások a HDInsighton
[Microsoft Machine Learning Services](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview) biztosít az adatszakértők, statisztikusok és R-programozók skálázható, igény szerinti hozzáféréssel rendelkező elosztott elemzési a HDInsight módszereket.  A megoldás a legújabb funkciókat az R-alapú elemzési biztosít nagy méretű adatkészletek gyakorlatilag bármilyen méretű, az Azure Blob vagy a Data Lake storage betöltve.

Ez az egy vállalati szintű megoldás, amely lehetővé teszi az R-kódokat a fürt méretezése érdekében.  A Microsoft a functions használatával
<code>[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)</code> csomag, az R-szkriptek HDInsight adatfeldolgozási funkciók párhuzamosan futtatható a fürt több csomópontján.  Ez lehetővé teszi az R sokkal nagyobb méretekben több, mint lehetséges egyszálas r-adatok elemzése a hatékonyabb játékelemzésért munkaállomáson futó.

Vertikális ezáltal gépi Tanulási szolgáltatásokat, a HDInsight nagyszerű lehetőséget kínál a fejlesztőknek R hatalmas adatkészletekre.  Rugalmas és skálázható platformot biztosít az R-szkriptek futtatása a felhőben.

A egy Machine Learning-szolgáltatások fürt létrehozásával kapcsolatos lépésenkénti útmutatóért lásd: a ["Első lépések a Machine Learning-szolgáltatások az Azure HDInsight"](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started) cikk.

## <a name="azure-databricks"></a>Azure Databricks
[Az Azure Databricks](https://azure.microsoft.com/services/databricks/) egy a Microsoft Azure cloud services platformra optimalizált Apache Spark-alapú elemzési platform.  Az Apache Spark alapítóival közösen tervezett Databricks integrálva van az Azure-ral, így egyetlen kattintással beállítható, zökkenőmentes munkafolyamatokat tesz lehetővé, valamint olyan interaktív munkaterületet biztosít, amellyel lehetővé válik az adatszakértők, az adatmérnökök és az üzleti elemzők közötti együttműködés.

A Databricks az együttműködés a platform notebook rendszer engedélyezve van.  Felhasználók létrehozása, megosztása és jegyzetfüzetek szerkesztése a rendszerek más felhasználókkal.  Ezeket a notebookokat engedélyezése a felhasználóknak, amely végrehajtja a ellen a Spark-fürtök a Databricks környezetben felügyelt kód írására.  Ezeket a notebookokat teljes körűen támogatja az R, és hozzáférést biztosít a felhasználóknak a Spark segítségével is a `SparkR` és `sparklyr` csomagokat.

Databricks Spark épül, és erős hangsúlyt az együttműködést, mivel a platform gyakran használják az adatszakértők, amelyek együttműködve összetett elemzéseket, nagy méretű adatkészleteket a csapatok.  Mivel a jegyzetfüzetet a Databricksben támogatja az R kívül más nyelveken, különösen hasznos a teams ahol elemzők elsődleges munkahelyi különböző nyelvet használják.

A cikk ["Mi az az Azure Databricks?"](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks)
a platform és elsajátíthatja további részleteket is nyújtanak.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[Az Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/) egy együttműködést támogató, fogd és vidd eszköz használatával hozhat létre, tesztelheti és üzembe helyezése a felhőbeli prediktív elemzési megoldások.  Lehetővé teszi az újonnan felbukkanó adatszakértők létrehozni és üzembe helyezni a gépi tanulási modellek sok kód írása nélkül.

Machine Learning Studio támogatja az R- és Python.  R két módon használhatja a Machine Learning Studio használatával.

### <a name="custom-r-scripts-in-your-experiments"></a>A kísérletek az egyéni R-szkriptek
Először is kiterjesztheti az adatkezelés és a machine learning, Machine Learning Studio képességeiről egyéni R-parancsfájlok írásával.
Bár az ML Studio számos különböző modulok az előkészítés és adatelemzés tartalmaz, nem lehet azonos az érett Nyelv például r képességeit  Ezért a szolgáltatás tervezték, hogy vezessen be a saját egyéni R-szkriptek azokban az esetekben, ahol a megadott modul nem felel meg az igényeinek.

Ezzel a funkcióval a áthúzása egy "Hajtsa végre az R-parancsfájl" modul a kísérletbe.  Majd használja a Kódszerkesztő a "Tulajdonságok" panelen egy új R-szkriptet írni vagy illessze be egy meglévő parancsfájl.  A szkriptben hivatkozhat külső R-csomagokat.  A parancsfájlt is használhatja, manipulálják az adatokat, vagy összetett gépi Tanulási modellek, amelyek nem részei a standard szintű Machine Learning Studio modell könyvtár.

Az R használata a Machine Learning Studio-kísérletek belül alaposan megismerni, tekintse meg a ["Gyors üzembe helyezési oktatóanyag az R programozási nyelv, az Azure Machine Learning."](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart)

### <a name="create-manage-and-deploy-experiments-from-your-local-r-environment"></a>Létrehozása, kezelése és üzembe helyezése a helyi R-környezetben a kísérletek
A másik lehetőség, hogy használhatja a Machine Learning Studio R az, hogy használja a
<code>[AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)</code> a csomag monitorozását és ellenőrzését a Kísérletezési folyamat az R programozási környezetet.  Ez a csomag, amely a Microsoft által karbantartott, fel-és letölteni az adatkészleteket az Azure gépi Tanulási kísérletek közzététele az Azure ML-webszolgáltatások, mint az R-funkciók és a R data futtassa a meglévő webszolgáltatásokon keresztül, és lekérése a kimeneti logikát teszi lehetővé.

Ez a csomag egyszerűen alkalmazhatók az Azure ML skálázható platformként az R-kód számára.  Kattintással, és a felhasználói felületen húzással, helyett a teljes telepítési folyamatot a már jól ismert eszközök használatával automatizálható.

## <a name="azure-batch"></a>Azure Batch
A nagy méretű az R-munkák használhatja [Azure Batch](https://azure.microsoft.com/services/batch/).  Ezt a szolgáltatást felhőléptékű feladatütemezés és számításkezelés felhőméretben-kezelést biztosít, így az R számítási feladatok több tíz, száz vagy ezer virtuális gép között is méretezheti.  Mivel ez egy általánosított számítástechnikai platformja, van néhány lehetőség az R-munkák futtatása Azure Batch.

Az egyik lehetőség, hogy használja a Microsoft <code>[doAzureParallel](https://github.com/Azure/doAzureParallel)</code> csomagot.  Az R-csomag egy párhuzamos háttéralkalmazásának a `foreach` csomagot.  Minden egyes megismételt lehetővé teszi a `foreach` hurkot, és az Azure Batch-fürtön belül egy csomóponton párhuzamosan futnak.  A csomag szeretné megismerni, olvassa el a ["doAzureParallel: kihasználhatja az Azure rugalmas számítási közvetlenül az R-munkamenet az"](https://azure.microsoft.com/blog/doazureparallel/) blogbejegyzést.

Az R-szkriptek futtatásához az Azure Batch egy másik lehetőség, hogy a kód "RScript.exe" a csomagot, egy Batch-alkalmazások az Azure Portalon.  Részletes útmutatót szeretne, olvassa el a ["R számítási feladatait az Azure Batch."](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/)

Egy harmadik lehetőség a [Azure elosztott adatok mérnöki eszközkészlet](https://github.com/Azure/aztk) (AZTK), amely lehetővé teszi, hogy az Azure Batchben a Docker-tárolók használatával igény szerinti Spark-fürtök kiépítése.  Ez biztosítja, hogy a Spark-feladatok futtatása az Azure-ban gazdaságos megoldást kínál.  Használatával [AZTK a SparklyR](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK), az R-szkriptek kiterjeszthető a felhőben egyszerűen, és gazdaságosan.

## <a name="azure-notebooks"></a>Azure Notebooks
[Az Azure notebookok](https://notebooks.azure.com) van egy kis költségigényű, alacsony terhelésű metódus R notebookok való munkavégzés előnyben részesítő fejlesztőknek saját kód használata az Azure-bA.  Egy ingyenes szolgáltatás mindenki számára a fejleszthet és futtathat kódot a böngésző használatával [Jupyter](https://jupyter.org/), amely egy nyílt forráskódú projekt, amely lehetővé teszi a markdown prose weboldalak százmilliárdjainak biztosítják a, a végrehajtható kódok és a egy egyetlen vászonra grafikus.

Noha az Azure notebookok semmibe projektek egy kivitelezhető lehetőség, rendelkezik bizonyos korlátozások, amelyek megkönnyítik a nagy léptékű adatelemzési projektek nem megfelelő.  Jelenleg a szolgáltatás korlátozza az egyes notebook folyamat 4 GB memória, és adatkészletek csak 1 GB-os lehet.  A kisebb méretű elemzések közzététele, ez azonban egy egyszerű és ingyenes lehetőséget.

## <a name="azure-sql-database"></a>Azure SQL Database
[Az Azure SQL Database](https://azure.microsoft.com/services/sql-database/) a Microsoft intelligens, teljes körűen felügyelt relációs adatbázis-szolgáltatás.  Ez lehetővé teszi, hogy használja a teljes SQL Server nem kell minden olyan infrastruktúra nélkül.  Ez magában foglalja [Machine Learning-szolgáltatások](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017), egyike a a legújabb SQL Service kiegészítéseit.

Ezt a szolgáltatást kínál egy beágyazott, prediktív elemzési és adatelemző motor tárolt eljárások, T-SQL-parancsfájlok R utasításokat tartalmazó vagy R-kód, amely tartalmazza a T-SQL, R-kód egy SQL Server-adatbázison belül végrehajtható.  Adatok kinyerése az adatbázisból, és hogy betöltené őket az R-környezetben, helyett betölteni az R-kód közvetlenül az adatbázisba, és biztosítani, hogy az adatok mellett jobbra futtassa.

Míg a Machine Learning-szolgáltatások már része a helyszíni SQL Server 2016 óta, akkor viszonylag új, az Azure SQL Database.  Jelenleg éppen [korlátozott előzetes verzió](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap) , de továbbra is fejlesztheti tovább.


### <a name="next-steps"></a>További lépések
* [Azure-ban, az mrsdeploy futtatja az R-kód](http://blog.revolutionanalytics.com/2017/03/running-your-r-code-azure.html)
* [A Machine Learning szolgáltatás a felhőben](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-in-the-cloud)
* [További források a Machine Learning-kiszolgáló és a Microsoft R](https://docs.microsoft.com/machine-learning-server/resources-more)
* [Az Azure-ban R](https://github.com/yueguoguo/r-on-azure) – egy csomagok, eszközök és az R használata az Azure-ral esettanulmányok áttekintése

---

<sub>Az R-embléma &copy; 2016 The R Foundation feltételei szolgál, és a [Creative Commons Attribution-ShareAlike 4.0 nemzetközi licenc](https://creativecommons.org/licenses/by-sa/4.0/).</sub>

---
title: Presto telepítése Linux-alapú Azure HDInsight-fürtökön
description: Megtudhatja, hogyan Presto és Airpal telepítése Linux-alapú HDInsight Hadoop-fürtökön parancsfájlműveletekkel.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: b9ac9c49e633906e47244eedcb18a4cda4a6228d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978953"
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Telepítheti és használhatja Presto HDInsight Hadoop-fürtök

Ebből a dokumentumból megismerheti, hogyan Presto telepítése HDInsight Hadoop-fürtökön Parancsfájlműveletekkel használatával. Azt is megtudhatja, hogyan Airpal telepítése egy meglévő Presto HDInsight-fürtön.

> [!IMPORTANT]
> A jelen dokumentumban leírt lépések szükséges egy **HDInsight 3.5-ös Hadoop-fürt** , Linux rendszert használ. A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható. További információkért lásd: [HDInsight-verziók](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Presto mi?
[Presto](https://prestodb.io/overview.html) gyors elosztott SQL lekérdezési motorja big Data-van. A presto ideális interaktív több petabájtnyi adat lekérdezését. Presto, és hogyan működnek együtt az összetevőkről további információkért lásd: [Presto fogalmak](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> A HDInsight-fürthöz megadott összetevők teljes mértékben támogatottak, és a Microsoft Support fog help elkülönítésére, és ezeket az összetevőket kapcsolatos problémák megoldásához.
> 
> Egyéni összetevők, például Presto, annak érdekében, hogy a probléma további hibaelhárításához üzletileg ésszerű támogatást kapnak. Emiatt előfordulhat, hogy a probléma megoldásához vagy rákérdez arra, hogy a nyílt forráskódú technológiák, ahol található részletes szakértelmét, hogy a technológiát a rendelkezésre álló csatorna léphet. Számos, használható, például közösségi helyek vannak, például: [HDInsight az MSDN-fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Is Apache projektek rendelkeznek projekt helyek [ http://apache.org ](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Szkriptműveletek használatával a Presto telepítése

Ez a szakasz útmutatást a minta parancsfájl használatával, amikor egy új fürt létrehozása az Azure portal használatával. 

1. Indítsa el a fürt kiépítése a lépéseket követve [Provision Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md). Győződjön meg arról, létrehozhatja a fürtöt a **egyéni** a fürt létrehozási folyamat. A fürt az alábbi követelményeknek kell megfelelnie.

    * Hadoop-fürt a HDInsight 3.6-os verzióját kell lennie.

    * Azure Storage azt kell használnia, mint az adattárban. Presto egy fürtön, amely a beállítást használja az Azure Data Lake Store használata még nem támogatott. 

    ![HDInsight-fürt létrehozása egyéni beállítások használatával](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Az a **speciális beállítások** területen válassza **Parancsfájlműveletek**, és adja meg az alábbi adatokat:
   
   * **NÉV**: Adjon egy rövid nevet a parancsprogram-művelet.
   * **Bash-szkript URI azonosítója**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **A fő**: ezt a beállítást választva
   * **FELDOLGOZÓ**: ezt a beállítást választva
   * **ZOOKEEPER**: törölje a jelet a jelölőnégyzetből.
   * **PARAMÉTEREK**: hagyja üresen a mezőt


3. Alsó részén a **Parancsfájlműveletek** területen kattintson a **válassza** gombra a konfiguráció mentéséhez. Végül kattintson a **kiválasztása** gomb alsó részén a **speciális beállítások** terület menteni a konfigurációs adatokat.

4. A fürt kiépítése a leírtak szerint folytassa [Provision Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Az Azure PowerShell, az Azure klasszikus parancssori felület, a HDInsight .NET SDK vagy az Azure Resource Manager-sablonok a alkalmazni parancsfájlműveletekkel is használható. Már fut a fürtök parancsfájlműveletekkel is alkalmazhat. További információkért lásd: [testreszabása HDInsight fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>A Presto használata a HDInsight

A Presto egy HDInsight-fürtön, használja az alábbi lépéseket:

1. Csatlakozzon SSH-val a HDInsight-fürthöz:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. A Presto rendszerhéj, a következő paranccsal indítsa el.
   
        presto --schema default

3. -Lekérdezést futtathat egy minta tábla **hivesampletable**, alapértelmezés szerint minden HDInsight fürtön elérhető.
   
        select count (*) from hivesampletable;
   
    Alapértelmezés szerint [Hive](https://prestodb.io/docs/current/connector/hive.html) és [TPCH](https://prestodb.io/docs/current/connector/tpch.html) Presto összekötők már konfigurálva vannak. Hive-összekötő az alapértelmezés szerint telepített Hive telepítést használ, így a Hive a táblák automatikus láthatók lesznek a Presto van konfigurálva.

    További információkért lásd: [Presto dokumentáció](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>A Presto Airpal használata

[Airpal](https://github.com/airbnb/airpal#airpal) Presto egy nyílt forráskódú webes lekérdezési felületet szól. Airpal további információkért lásd: [Airpal dokumentáció](https://github.com/airbnb/airpal#airpal).

Az alábbi lépések segítségével Airpal telepíteni az élcsomóponton:

1. SSH-val, csatlakozás az átjárócsomóponthoz, amely Presto telepített HDInsight-fürt:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Miután csatlakozott, futtassa a következő parancsot.

        sudo slider registry  --name presto1 --getexp presto 
   
    A következő JSON hasonló kimenet jelenik meg:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. A kimenetben, vegye figyelembe az értéket a **érték** tulajdonság. A fürt élcsomópontok Airpal telepítése során kell ezt az értéket. A fenti kimenetben szükséges értéke **10.0.0.12:9090**.

4. A sablon használatához **[Itt](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** hozhat létre egy HDInsight-fürt élcsomópontok, és adja meg az értékeket az alábbi képernyőképen látható módon.

    ![HDInsight telepítés Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Kattintson a **Purchase** (Vásárlás) gombra.

6. Miután a fürt konfigurációját a módosítások is vonatkozik, elérheti a Airpal webes felületén az alábbi lépések segítségével.

    1. A fürt párbeszédpanelen kattintson a **alkalmazások**.

        ![HDInsight indítási Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    2. Az a **telepített alkalmazások** területen kattintson a **portál** airpal ellen.

        ![HDInsight indítási Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    3. Amikor a rendszer kéri, adja meg, amely a HDInsight Hadoop-fürt létrehozásakor megadott rendszergazdai hitelesítő adatait.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>A Presto telepítése HDInsight-fürt testreszabása

A telepítés testreszabásához használja az alábbi lépéseket:

1. SSH-val, csatlakozás az átjárócsomóponthoz, amely Presto telepített HDInsight-fürt:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. A konfigurációs módosításokat a fájlban `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. A Presto konfigurációs további információkért lásd: [YARN-alapú fürtök Presto konfigurációs](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Állítsa le és kill Presto aktuális futó példányát.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Indítsa el a Presto egy új példányát a testreszabási beállításokkal.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Várjon, amíg az új példány készen áll, és jegyezze fel presto koordinátor-címét.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Számításiteljesítmény-mérési adatok Presto futtató HDInsight-fürtök létrehozása

TPC-DS-ben az iparági szabvány számos döntést támogatási rendszerek, beleértve a big data-rendszereket a teljesítmény méréséhez. Segítségével Presto adatokat hozhat létre és kiértékelheti, hogyan viszonyul a saját HDInsight számításiteljesítmény-mérési adatokkal. További információkért lásd: [Itt](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Lásd még
* [Telepítse, és a Hue használata a HDInsight-fürtökön](hdinsight-hadoop-hue-linux.md). A hue webes felhasználói felület, amely megkönnyíti a szeretne létrehozni, futtassa, és mentse a Pig and Hive-feladatok.

* [A Giraph telepítése HDInsight-fürtökön](hdinsight-hadoop-giraph-install-linux.md). Fürt testreszabása használatával a Giraph telepítése HDInsight Hadoop-fürtökön. A Giraph lehetővé teszi a Hadoop használatával diagramfeldolgozási végrehajtásához, és használható az Azure HDInsight.

* [A Solr telepítése HDInsight-fürtökön](hdinsight-hadoop-solr-install-linux.md). Fürt testreszabása használatával a Solr telepítése HDInsight Hadoop-fürtökön. A Solr lehetővé teszi a tárolt adatok hatékony keresési műveletek végrehajtásához.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

---
title: Egy böngészőben – Azure HDInsight Hadoop-fürtök létrehozása
description: Ismerje meg, Hadoop, HBase, Storm vagy Spark-fürtök létrehozása Linux rendszeren a HDInsight egy webböngészőt, és az Azure betekintő portál használatával.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: 77f4b8e8826dab014b81fdb6847755630ac44508
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/27/2018
ms.locfileid: "43104942"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Linux-alapú fürtök létrehozása a HDInsight az Azure portal használatával
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Az Azure Portalon egy webalapú felügyeleti eszköz, szolgáltatások és erőforrások a Microsoft Azure-felhő üzemeltet. Ebben a cikkben megismerheti, hogyan hozhat létre Linux-alapú HDInsight-fürtök a portál használatával.

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **A modern webböngésző**. Az Azure Portalon használja a HTML5-alapú és a Javascript, és előfordulhat, hogy nem működik megfelelően régebbi böngészőkben.

## <a name="create-clusters"></a>Fürtök létrehozása
Az Azure Portalon a tulajdonságait a legtöbb tesz elérhetővé. Azure Resource Manager-sablonnal számos Részletek elrejtése. További információkért lásd: [Linux-alapú Hadoop-fürtök a HDInsight az Azure Resource Manager-sablonok használatával](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Jelentkezzen be az [Azure Portal](https://portal.azure.com) webhelyre.
2. Kattintson a **+**, kattintson a **intelligencia és elemzés**, és kattintson a **HDInsight**.
   
    ![Új fürt létrehozása az Azure Portalon](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "új fürt létrehozása az Azure Portalon")

3. Az a **HDInsight** panelen kattintson a **egyéni (méret, beállítások, alkalmazások)**, kattintson a **alapjai**, majd adja meg a következő információkat.

    ![Új fürt létrehozása az Azure Portalon](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "új fürt létrehozása az Azure Portalon")

    * A **Fürt neve** mezőben adja meg a fürt nevét, amelynek globálisan egyedinek kell lennie.

    * Az a **előfizetés** legördülő menüben válassza ki az Azure-előfizetést, amely a fürt szolgál.

    * Kattintson a **fürt típusa**, majd válassza ki a létrehozni kívánt fürt (Hadoop, Spark, stb.). A **operációs rendszer**, kattintson a **Linux** , majd egy verziót. Alapértelmezett verzióját használja, ha nem tudja, melyik a megfelelő. További tájékoztatás a [HDInsight cluster versions](hdinsight-component-versioning.md) (A HDInsight-fürtök verziói) című cikkben olvasható.

        Hadoop, Spark- és interaktív lekérdezési fürt esetében választhatja telepítéséhez a **vállalati biztonsági csomag**. Vállalati biztonsági csomag lehetővé teszi, hogy a biztonsági funkciókat az Azure Active Directory-integráció és az Apache Ranger a fürtök. További információkért lásd: [az Azure HDInsight vállalati biztonsági csomag](./domain-joined/apache-domain-joined-introduction.md).

        ![Engedélyezi a vállalati biztonsági csomag](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-enable-enterprise-security-package.png "engedélyezése a vállalati biztonsági csomag")
     
        > [!IMPORTANT]
        > HDInsight fürtök különböző típusú, amelyek megfelelnek a számítási feladatok vagy technológia, amely a fürt van hangolva származnak. Nincs támogatott módszer, amely ötvözi az több típus, például a Storm és a egy fürtön a HBase-fürt létrehozásához. 
        > 
        > 
        
    * A **fürt bejelentkezési felhasználóneve** és **fürt bejelentkezési jelszavának**, adja meg a rendszergazdai felhasználó felhasználónevét és jelszavát.

    * Adjon meg egy **SSH-felhasználónév** , és ha szeretné engedélyezni az SSH-jelszó ugyanaz, mint a korábban megadott rendszergazdai jelszót, jelölje be a **használja ugyanazt a jelszót, mint a fürtbe való bejelentkezésekor** jelölőnégyzetet. Ha nem, adja meg, vagy egy **jelszó** vagy **nyilvános kulcs**, az SSH-felhasználó hitelesítéséhez használandó. A Microsoft a nyilvános kulcs használatát javasolja. A hitelesítő adatok konfigurációjának mentéséhez kattintson az oldal alján található **Kiválasztás** elemre.
   
    További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

    * Az **Erőforráscsoport** alatt adja meg, hogy új erőforráscsoportot kíván-e létrehozni, vagy egy meglévőt szeretne használni.

    * Adjon meg egy adatközpont **hely** ahol a fürt létrejött.

    * Kattintson a **Tovább** gombra.

4. A **tárolási**, adja meg, hogy az Azure Storage (WASB) vagy a Data Lake Storage az alapértelmezett tárolóként. Tekintse meg az alábbi táblázatban további információt.

    ![Új fürt létrehozása az Azure Portalon](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "új fürt létrehozása az Azure Portalon")

    | Storage                                      | Leírás |
    |----------------------------------------------|-------------|
    | **Az Azure Storage-Blobokkal, az alapértelmezett tároló**   | <ul><li>A **elsődleges tárolási típusok**válassza **Azure Storage**. Ezt követően a **kijelöléséről**, választhat **saját előfizetések** szeretné-e az Azure-előfizetés részét képező tárfiókot adjon meg, és jelölje ki a tárfiókot. Ellenkező esetben kattintson a **hívóbetű** , és adja meg az adatokat a tárfiók, válassza ki a kívánt kívül az Azure-előfizetés.</li><li>A **alapértelmezett tároló**, ha szeretné, lépjen a portál által javasolt alapértelmezett tároló nevét, vagy saját maga hozza létre.</li><li>Ha a WASB használja alapértelmezett tárolóként, (opcionálisan) kattintson **további Tárfiókok** társítása a fürt további tárfiókok megadására. A **Azure-Tárkulcsok**, kattintson a **tárkulcs hozzáadása**, és ezután megadhat egy storage-fiókot az Azure-előfizetésekre vagy más előfizetésekből származó (azáltal, hogy a tárfiók hozzáférési kulcsát).</li><li>Ha a WASB használja alapértelmezett tárolóként, (opcionálisan) kattintson **Data Lake Store-hozzáférés** , adja meg az Azure Data Lake Storage kiegészítő tárolóként. További információkért lásd: [a rövid útmutató: a HDInsight-fürtök beállítása](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
    | **Az Azure Data Lake Storage alapértelmezett tárolóként** | A **elsődleges tárolási típusok**válassza **Azure Data Lake Storage Gen1** vagy **Azure Data Lake Storage Gen2 (előzetes verzió)** , majd a cikk [a rövid útmutató : Állítsa be a HDInsight-fürtök](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) útmutatást. |
    | **Külső metaadattárak**                      | Szükség esetén az SQL-adatbázis, a fürthöz társított Hive-val és az Oozie-metaadatok mentése is megadhat. A **SQL-adatbázis kiválasztása a Hive** SQL-adatbázis kiválasztása, és adja meg a felhasználónév/jelszó az adatbázishoz. Ismételje meg ezeket a lépéseket Oozie-metaadatok.<br><br>Néhány szempontot metaadattárakat az Azure SQL database használatakor. <ul><li>Az Azure SQL database, a metaadattár használt más Azure-szolgáltatásokkal, mint az Azure HDInsight való csatlakozást engedélyezhetik. Az Azure SQL database irányítópulton a jobb oldalon kattintson a kiszolgálónévre. Ez az a kiszolgáló, amelyen az SQL database-példány fut-e. Miután a nézetet, kattintson a **konfigurálása**, majd **Azure-szolgáltatások**, kattintson a **Igen**, és kattintson a **mentése**.</li><li>Amikor metaadattárának, ne használja az adatbázis nevét, amely tartalmazza a kötőjelet vagy kötőjelet tartalmazhat, ez okozhat a Fürtlétrehozási folyamat sikertelen lesz.</li></ul> |

    Kattintson a **Tovább** gombra. 

    > [!WARNING]
    > A rendszer nem támogatja további tárfiókok használatát a HDInsight-fürtön kívül eső helyeken.

5. Másik lehetőségként kattinthat **alkalmazások** telepíthet alkalmazásokat, hogy működik a HDInsight-fürtökkel. Ezek az alkalmazások lehetnek a Microsoft, független szoftvergyártók (ISV-k) vagy a felhasználók fejlesztései. További információkért lásd: [telepítése HDInsight-alkalmazások](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Kattintson a **fürtméret** a csomópontokat a fürthöz használt információit jeleníti meg. Állítsa be, amelyekre szüksége van a fürt munkavégző csomópontok számát. A fürt futtatása becsült költsége is látható.
   
    ![Csomópont árképzési szintjei](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "adja meg a fürt csomópontok száma")
   
   > [!IMPORTANT]
   > Ha azt tervezi, hogy több mint 32 feldolgozó csomópontokat, a fürt létrehozáskor vagy a fürt létrehozása után felskálázásával majd választania kell egy fő csomópont mérete legalább 8 maggal és 14 GB RAM-MAL rendelkező.
   > 
   > További információ a csomópontméretekről és a velük járó költségekről: [A HDInsight díjszabása](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
   Kattintson a **tovább** mentse a csomópontok árképzésének konfigurációját.

7. Kattintson a **speciális beállítások** például egyéb nem kötelező beállítások konfigurálása **Parancsfájlműveletek** egyéni összetevők vagy az összekötő telepítése egy fürt testreszabása egy **virtuálishálózat**. Tekintse meg az alábbi táblázatban további információt.

    ![Csomópont árképzési szintjei](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "adja meg a fürt csomópontok száma")

    | Beállítás | Leírás |
    |--------|-------------|
    | **A Parancsfájlműveletek** | Használja ezt a beállítást, ha azt szeretné, egyéni parancsfájl használata a fürt testreszabása, a fürt létre lesz hozva. A szkriptműveletek kapcsolatos további információkért lásd: [testreszabása HDInsight-fürtök szkriptműveletekkel](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Virtual Network** | Válassza ki a Azure-beli virtuális hálózathoz és az alhálózatot, ha el szeretné helyezni a fürt egy virtuális hálózatban. HDInsight használatával egy virtuális hálózathoz, beleértve a virtuális hálózathoz megadott konfigurációs követelményekkel kapcsolatos információkat lásd: [HDInsight kiterjesztése képességek az Azure Virtual Network használatával](hdinsight-extend-hadoop-virtual-network.md). |

    Kattintson a **Tovább** gombra.

8. A **összefoglalás**, ellenőrizze a korábban megadott adatokat, majd kattintson **létrehozás**.

    ![Csomópont árképzési szintjei](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "adja meg a fürt csomópontok száma")
    
    > [!NOTE]
    > Némi várakozás a fürt létrehozása, általában körülbelül 15 percet vesz igénybe. A csempe használatához a Kezdőpulton, vagy a **értesítések** bal oldalán a lapra, és ellenőrizze a kiépítési folyamat a bejegyzés.
    > 
    > 
12. A létrehozási folyamat befejezése után kattintson a kezdőpultján a fürthöz tartozó csempére. A fürt ablakban a következő adatokat tartalmazza.
    
    ![Fürt felület](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "fürt tulajdonságai")
    
    A következő segítségével megismerheti az ikonok tetején.
    
    * **Áttekintés** lapon például a nevét, az erőforráscsoportot, amelyhez tartozik, a helyet, az operációs rendszer, a fürt irányítópultja stb URL-CÍMÉT a fürt összes nélkülözhetetlen információkat nyújt.
    * **Irányítópult** a fürthöz társított Ambari portálra irányítja.
    * **Secure Shell**: a fürthöz SSH használatával eléréséhez szükséges információkat.
    * **Fürt méretezése** lehetővé teszi, hogy a fürthöz társított munkavégző csomópontok számának növelése.
    * **Törlés**: törli a HDInsight-fürtöt.
    

## <a name="customize-clusters"></a>Fürtök személyre szabása
* Lásd: [Bootstrap használatával testre szabhatja a HDInsight-fürtök](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Lásd: [testreszabása HDInsight-fürtök szkriptműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>A fürt törlése
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>További lépések
Most, hogy sikeresen létrehozott egy HDInsight-fürtöt, megtudhatja, hogyan használható a fürt a következő használatával:

### <a name="hadoop-clusters"></a>Hadoop-fürtök
* [A Hive használata a HDInsightban](hadoop/hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hadoop/hdinsight-use-pig.md)
* [A MapReduce használata a HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-fürtök
* [A HDInsighton futó HBase használatának első lépései](hbase/apache-hbase-tutorial-get-started-linux.md)
* [A HDInsight hbase Java-alkalmazások fejlesztése](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-fürtök
* [Java-topológiák fejlesztése HDInsight alatt futó Stormmal](storm/apache-storm-develop-java-topology.md)
* [Python-összetevők használata a HDInsight alatt futó Stormmal](storm/apache-storm-develop-python-topology.md)
* [Telepítheti és figyelheti a HDInsight Storm-topológiák](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>A Spark-fürtök
* [Önálló alkalmazás létrehozása a Scala használatával](spark/apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](spark/apache-spark-livy-rest-interface.md)
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](spark/apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére](spark/apache-spark-machine-learning-mllib-ipython.md)


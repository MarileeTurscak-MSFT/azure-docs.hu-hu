---
title: Üres élcsomópontok használata a HDInsight - Azure Hadoop-fürtök
description: Hogyan lehet üres élcsomópontot hozzáadni egy HDInsight-fürtöt, amely ügyfélként is használható, és ezután teszt/állomás a HDInsight-alkalmazások.
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: jasonh
ms.openlocfilehash: 1111f3c21e3c3718a9a010284a42ea469e04473d
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/27/2018
ms.locfileid: "43090388"
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a>Üres élcsomópontok használata a HDInsight Hadoop-fürtök

Ismerje meg, hogyan lehet üres élcsomópontot hozzáadni egy HDInsight-fürt. Üres élcsomópontot egy Linuxos virtuális gép ugyanazokat az ügyfél eszközöket telepíteni és konfigurálni az átjárócsomópontokkal hasonlóan, de nincs futó Hadoop-szolgáltatásokhoz. A peremhálózati csomópont a fürthöz hozzáférő, az ügyfél alkalmazásokat kíván tesztelni, és az ügyfélalkalmazásokat üzemeltető is használhatja. 

Egy meglévő HDInsight-fürtön, egy új fürt a fürt létrehozásakor egy üres élcsomópontot is hozzáadhat. Üres élcsomópontot hozzáadása történik az Azure Resource Manager-sablon használatával.  A következő példa bemutatja, hogyan történik egy sablon használatával:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Ahogyan azt a mintát, igény szerint meghívhatja egy [műveleti parancsfájl](hdinsight-hadoop-customize-cluster-linux.md) további konfigurálást, például a telepítés végrehajtásához [Apache Hue](hdinsight-hadoop-hue-linux.md) az élcsomóponthoz. A parancsfájl parancsfájlművelet kell lennie a weben nyilvánosan elérhető.  Például ha a parancsfájl az Azure storage-ban, használjon nyilvános tárolók vagy nyilvános blobok.

A peremhálózati csomópont virtuális gépének mérete a HDInsight fürt munkavégző csomópont virtuális gép mérete követelményeknek kell megfelelnie. A javasolt munkavégző csomópont virtuálisgép-méretek, lásd: [Hadoop-fürtök létrehozása a HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

Miután létrehozta az élcsomóponthoz, az élcsomóponthoz SSH használatával csatlakozhat, és futtassa az ügyfél eszközök eléréséhez a HDInsight Hadoop-fürtöt.

> [!WARNING] 
> Egyéni telepített összetevőinek támogatásához az élcsomóponti operacionalizáláshoz üzletileg ésszerű támogatást kapni a Microsofttól. Emiatt előfordulhat, hogy a felmerülő problémák elhárításához. Vagy előfordulhat, hogy említett közösségi erőforrások további segítségért. A következő néhány a legtöbb aktív helyek segítség kérése a Közösségtől:
>
> * [A HDInsight MSDN-fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [http://stackoverflow.com](http://stackoverflow.com).
>
> Ha egy Apache-okat használ, akkor lehet a projekt helyek megtalálhatja a segítséget az Apache [ http://apache.org ](http://apache.org), mint például a [Hadoop](http://hadoop.apache.org/) hely.

> [!NOTE]
> Ugyanaz, mint a fürtöket, élcsomópontok is rendelkezésre állnak managed a javítást.  További információkért lásd: [operációs rendszer javításai a HDInsight](./hdinsight-os-patching.md).

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Egy peremhálózati csomópont hozzáadása meglévő fürthöz
Ebben a szakaszban a Resource Manager-sablonnal egy meglévő HDInsight-fürt egy élcsomópontot hozzáadandó használhatja.  A Resource Manager-sablon található [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-add-edge-node/). A Resource Manager-sablon meghívja a helyen található szkriptműveletet https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. A parancsfájl nem más műveletet végrehajtani.  A Resource Manager-sablonnal hívó parancsfájlművelet bemutatásához.

**Üres élcsomópontot hozzáadása egy meglévő fürthöz**

1. Kattintson az alábbi képre kattintva jelentkezzen be az Azure-ba, és nyissa meg az Azure Resource Manager-sablon az Azure Portalon. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Adja meg a következő tulajdonságokkal:
   
   * **Előfizetés**: válassza ki a fürt létrehozásához használt Azure-előfizetés.
   * **Erőforráscsoport**: válassza ki az erőforráscsoportot, a meglévő HDInsight-fürt használja.
   * **Hely**: válassza ki a meglévő HDInsight-fürt helyét.
   * **Fürt neve**: Adjon meg egy meglévő HDInsight-fürt nevére.
   * **Csomópontméret él**: válassza ki a Virtuálisgép-méretek egyikét. A virtuálisgép-méretet, meg kell felelnie a munkavégző csomópont virtuálisgép-méret követelmények. A javasolt munkavégző csomópont virtuálisgép-méretek, lásd: [Hadoop-fürtök létrehozása a HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).
   * **Csomópont-előtag él**: az alapértelmezett érték **új**.  Az az alapértelmezett érték, a peremhálózati csomópont nevét a következő **új élcsomópontok**.  Testre szabhatja az előtag a portálról. A teljes nevet, a sablon alapján is testreszabhatja.

4. Ellenőrizze **elfogadom a feltételeket és a fenti feltételeket**, és kattintson a **beszerzési** hozhat létre az élcsomóponthoz.

>[!IMPORTANT]
> Ellenőrizze, hogy válassza ki az Azure-erőforráscsoportot a meglévő HDInsight-fürt.  Ellenkező esetben a következő üzenet jelenik "nem hajtható végre, a beágyazott erőforrás kért műveletet. Szülőerőforrás "&lt;ClusterName >" nem található. "

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Adja hozzá az élcsomóponthoz, a fürt létrehozásakor
Ebben a szakaszban egy Resource Manager-sablon a HDInsight-fürt létrehozása a élcsomópontot használja.  A Resource Manager-sablon megtalálható a [Azure gyorsindítási sablonok katalógusában](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/). A Resource Manager-sablon meghívja a helyen található szkriptműveletet https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. A parancsfájl nem más műveletet végrehajtani.  A Resource Manager-sablonnal hívó parancsfájlművelet bemutatásához.

**Az élcsomópont egy HDInsight-fürt létrehozása**

1. Ha még nincs ilyen, hozzon létre egy HDInsight-fürtön.  Lásd: [Hadoop első lépései a HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).
2. Kattintson az alábbi képre kattintva jelentkezzen be az Azure-ba, és nyissa meg az Azure Resource Manager-sablon az Azure Portalon. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Adja meg a következő tulajdonságokkal:
   
   * **Előfizetés**: válassza ki a fürt létrehozásához használt Azure-előfizetés.
   * **Erőforráscsoport**: hozzon létre egy új erőforráscsoportot, a fürthöz.
   * **Hely**: Válasszon egy helyet az erőforráscsoportnak.
   * **Fürt neve**: Adjon meg egy nevet az új fürt létrehozásához.
   * **A fürt bejelentkezési név**: Adja meg a Hadoop HTTP-felhasználó nevét.  Az alapértelmezett név az **admin**.
   * **A fürt bejelentkezési jelszavának**: Adja meg a Hadoop HTTP-felhasználó jelszavát.
   * **Ssh felhasználónév**: Adja meg az SSH-felhasználónév. Alapértelmezés szerint ez **sshuser**.
   * **Ssh jelszó**: Adja meg az SSH-felhasználói jelszóra.
   * **Telepítse a Script Action**: tartsa az oktatóanyag az alapértelmezett értéket.
     
     Egyes tulajdonságok szoftveresen kötöttek a sablonban: fürt típusa, a fürt munkavégző csomópontok száma, a peremhálózati csomópont méretét és a peremhálózati csomópont nevét.
4. Ellenőrizze **elfogadom a feltételeket és a fenti feltételeket**, és kattintson a **beszerzési** a fürt létrehozásához az élcsomóponthoz.

## <a name="add-multiple-edge-nodes"></a>Több peremhálózati csomópont hozzáadása

Egy HDInsight-fürt több élcsomópontok adhat hozzá.  A több peremhálózati csomópont konfiguráció kizárólag valósítható meg az Azure Resource Manager-sablonok használatával.  Tekintse meg a sablon mintát, ez a cikk elején.  Frissítenie kell a **targetInstanceCount** , hogy a létrehozni kívánt edge csomópontok számát.

## <a name="access-an-edge-node"></a>Az élcsomópont eléréséhez
Az élcsomópont ssh végpont van &lt;EdgeNodeName >.&lt; ClusterName >-ssh.azurehdinsight.net:22.  Ha például új-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Az élcsomópont egy alkalmazást, az Azure-portálon jelenik meg.  A portál eléréséhez az élcsomópont SSH-val a információkat biztosít.

**A peremhálózati csomópont SSH-végpont ellenőrzése**

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Nyissa meg a HDInsight-fürt egy élcsomópontot.
3. Kattintson a **alkalmazások**. Meg kell jelennie az élcsomóponthoz.  Alapértelmezés szerint ez **új élcsomópontok**.
4. Kattintson az élcsomóponthoz. Meg kell jelennie az SSH-végpont.

**A Hive használata az élcsomópontra**

1. Csatlakozzon az élcsomóponthoz SSH használatával. További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Miután csatlakozott az élcsomóponthoz SSH használatával, a Hive konzolt használja a következő parancsot:
   
        hive
3. A következő parancsot a fürt megjelenítése a Hive-táblákat:
   
        show tables;

## <a name="delete-an-edge-node"></a>Élcsomópont törlése
Élcsomópont törölheti az Azure Portalról.

**Az élcsomópont eléréséhez**

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Nyissa meg a HDInsight-fürt egy élcsomópontot.
3. Kattintson a **alkalmazások**. Az élcsomópontok listáját gondoskodnak.  
4. Kattintson a jobb gombbal az élcsomóponthoz, törlése, és kattintson a kívánt **törlése**.
5. Kattintson a **Yes** (Igen) gombra a megerősítéshez.

## <a name="next-steps"></a>További lépések
Ebben a cikkben megismerkedett hozzáadása az élcsomóponthoz, és hogyan lehet az élcsomópont eléréséhez. További tudnivalókért tekintse meg a következő cikkeket:

* [HDInsight-alkalmazások telepítése](hdinsight-apps-install-applications.md): Megtudhatja, hogyan telepíthet HDInsight-alkalmazásokat a fürtjeire.
* [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): útmutató HDInsight közzé nem tett HDInsight-alkalmazás üzembe helyezése.
* [HDInsight-alkalmazások közzététele](hdinsight-apps-publish-applications.md): Megtudhatja, hogyan teheti közzé egyéni HDInsight-alkalmazásait az Azure Piactéren.
* [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: HDInsight-alkalmazás telepítése): Megtudhatja, hogyan adhat meg HDInsight-alkalmazásokat.
* [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md) (Linux-alapú HDInsight-fürtök testreszabása parancsfájlműveletek segítségével): megtudhatja, hogyan telepíthet további alkalmazásokat parancsfájlműveletek használatával.
* [Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md) (Linux-alapú Hadoop-fürtök létrehozása a HDInsightban Resource Manager-sablonok segítségével): Megtudhatja, hogyan hívhat meg Resource Manager-sablonokat HDInsight-fürtök létrehozásához.


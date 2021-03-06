---
title: Azure parancssori felületével klasszikus – Azure HDInsight Hadoop-fürtök kezelése
description: Útmutató az Azure HDInsight Hadoop-fürtök kezelése az Azure klasszikus parancssori felület használatával.
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: 2586b9219eb145b2033fe2d8fc64b8ae72f34eda
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958288"
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-classic-cli"></a>A klasszikus Azure CLI-vel HDInsight Hadoop-fürtök kezelése
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Ismerje meg, hogyan használható a [Azure klasszikus parancssori felület](../cli-install-nodejs.md) Azure HDInsight Hadoop-fürtök kezeléséhez. A klasszikus parancssori felület implementálva van a node.js-ben. Használható bármilyen platformon, amely támogatja a Node.js-t, beleértve a Windows, Mac és Linux platformokat.

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

## <a name="prerequisites"></a>Előfeltételek
A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Az Azure klasszikus parancssori felület** – lásd: [telepítése és konfigurálása a klasszikus Azure-CLI](../cli-install-nodejs.md) telepítési és konfigurációs információt.
* **Csatlakozás az Azure-bA**, a következő paranccsal:

    ```cli
    azure login
    ```
  
    Munkahelyi vagy iskolai fiók segítségével történő hitelesítésről további információkért lásd: [csatlakozhat az Azure-előfizetés az Azure klasszikus parancssori felületen](/cli/azure/authenticate-azure-cli).
* **Váltson Azure Resource Manager módra** az alábbi paranccsal:
  
    ```cli
    azure config mode arm
    ```

Segítséget szeretne kérni, használja a **-h** váltani.  Példa:

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>Fürtök létrehozása a CLI-vel
Lásd: [fürtök létrehozása a klasszikus Azure CLI-vel HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Listázása és megjelenítése a fürt részletes adatai
A következő parancsok használatával listázása és megjelenítése a fürt részletes adatai:

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![Parancssori nézete fürtlista][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Fürtök törlése
Használja a következő parancsot a fürt törléséhez:

```cli
azure hdinsight cluster delete <Cluster Name>
```

A fürt törölheti az erőforráscsoportot, amely tartalmazza a fürt törlésével is. Megjegyzés: Ez a művelet törli a csoportot, beleértve az alapértelmezett tárfiók található összes erőforrást.

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>Fürtök méretezése
A Hadoop-fürt méretének módosítása:

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>Fürt HTTP-hozzáférés engedélyezése vagy letiltása

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Fürt RDP-hozzáférés engedélyezése vagy letiltása

```cli
azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
azure hdinsight cluster disable-rdp-access [options] <Cluster Name>
```

## <a name="next-steps"></a>További lépések
Ebben a cikkben megtanulhatta, hogyan hajthat végre különböző HDInsight fürt felügyeleti feladatokat. További tudnivalókért tekintse meg a következő cikkeket:

* [HDInsight felügyelete az Azure Portal használatával][hdinsight-admin-portal]
* [HDInsight felügyelete az Azure PowerShell használatával][hdinsight-admin-powershell]
* [Azure HDInsight – első lépések][hdinsight-get-started]
* [Az Azure klasszikus parancssori felület használata][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Fürtök listázása és megjelenítése"

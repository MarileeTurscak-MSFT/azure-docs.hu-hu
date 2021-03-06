---
title: A log Analytics-beli Apache kafka – Azure HDInsight
description: Ismerje meg, hogyan lehet az Apache Kafka-fürtöt az Azure HDInsight-naplók elemzése a Log Analytics használatával.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: d07c37803c94d7fa39d398a97796fecbad373af4
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047826"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>A HDInsight-beli Apache kafka-naplók elemzése

Ismerje meg, hogyan lehet Apache Kafka on HDInsight által előállított naplók elemzése a Log Analytics használatával.

## <a name="enable-log-analytics-for-kafka"></a>A Kafka a Log Analytics engedélyezése

A Log Analytics engedélyezése a HDInsight lépései megegyeznek az összes HDInsight-fürt. Az alábbi hivatkozások segítségével megtudhatja, hogyan hozhat létre és konfigurálja a szükséges szolgáltatások:

1. Hozzon létre egy Log Analytics-munkaterületet. További információkért lásd: a [Ismerkedés a Log Analytics-munkaterület](https://docs.microsoft.com/azure/log-analytics) dokumentumot.

2. Hozzon létre egy Kafka HDInsight-fürtön. További információkért lásd: a [a HDInsight Apache Kafka használatának első lépései](apache-kafka-get-started.md) dokumentumot.

3. A Kafka-fürt használata a Log Analytics konfigurálása. További információkért lásd: a [Log Analytics használata a HDInsight figyelése](../hdinsight-hadoop-oms-log-analytics-tutorial.md) dokumentumot.

    > [!NOTE]
    > A fürt használni a Log Analytics használatával is konfigurálhatja a `Enable-AzureRmHDInsightOperationsManagementSuite` parancsmagot. Ez a parancsmag megköveteli a következő információkat:
    >
    > * A HDInsight-fürt neve.
    > * A Log Analytics munkaterület azonosítója. A munkaterület-Azonosítót a Log Analytics-munkaterületen találhatja meg.
    > * A Log Analytics-kapcsolat elsődleges kulcsának. Keresse meg az elsődleges kulcs, nyissa meg a munkaterület az Azure Portalon, válassza a __speciális beállítások__ a bal oldali menüből. Speciális beállításainak megadásához válassza __csatlakoztatott források__>__Linux-kiszolgálók__.


> [!IMPORTANT]
> Mielőtt adatokat a Log Analytics körülbelül 20 percet igénybe vehet.

## <a name="query-logs"></a>Lekérdezések naplói

1. Az a [az Azure portal](https://portal.azure.com), válassza ki a Log Analytics-munkaterületre.

2. Válassza ki __naplóbeli keresés__. Itt megkeresheti a Kafka begyűjtött adatokat. Az alábbiakban néhány példa keresések:

    * Használat: `Perf | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * CPU-használat: `Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Másodpercenkénti bejövő üzenetek: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)`

    * Bejövő bájtok másodpercenként: `metrics_kafka_CL | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    * Kimenő bájtok másodpercenként: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    > [!IMPORTANT]
    > A lekérdezési értékeket cserélje le a fürtre jellemző információkat. Ha például `ClusterName_s` kell állítani a fürt nevére. `HostName_s` a fürt egyik munkavégző csomópontjához tartomány nevére kell állítani.

    Is megadhat `*` naplózott összes típusok keresése. Jelenleg a következő naplók kapcsolódnak érhetők el a lekérdezésekhez:

    | Napló típusa | Leírás |
    | ---- | ---- |
    | napló\_kafkaserver\_CL | A Kafka-közvetítő Server.log elérési úton található |
    | napló\_kafkacontroller\_CL | A Kafka-közvetítő controller.log |
    | metrikák\_kafka\_CL | A Kafka JMX-metrikák |

    ![A CPU-használat keresés képe](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>További lépések

A Log Analytics további információkért lásd: a [Ismerkedés a Log Analytics-munkaterület](../../log-analytics/log-analytics-get-started.md) dokumentumot.

A Kafka használatáról további információ a következő dokumentumokban talál:

 * [Tükrözött Kafka HDInsight-fürtök között](apache-kafka-mirroring.md)
 * [A HDInsight-beli Kafka méretezhetőségének javítása](apache-kafka-scalability.md)
 * [A Spark Stream (DStreams) a Kafka használata](../hdinsight-apache-spark-with-kafka.md)
 * [Használja a Spark strukturált Stream használata a Kafkával](../hdinsight-apache-kafka-spark-structured-streaming.md)

---
title: Nézet kubelet-naplók az Azure Kubernetes Service (AKS)
description: Hibaelhárítási információk megjelenítése az Azure Kubernetes Service (AKS) csomópontjáról kubelet-naplók
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/21/2018
ms.author: iainfou
ms.openlocfilehash: aeab24685f3663ba2c50205344d33db3d34676c2
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/22/2018
ms.locfileid: "42441948"
---
# <a name="get-kubelet-logs-from-azure-kubernetes-service-aks-cluster-nodes"></a>Az Azure Kubernetes Service (AKS)-fürt csomópontjának kubelet-naplók beszerzése

Bizonyos esetekben szükség lehet első *kubelet* hibaelhárítás céljából az Azure Kubernetes Service (AKS) csomópont naplóinak. Ez a cikk bemutatja, hogyan használhatja `journalctl` megtekintéséhez a *kubelet* naplókat.

## <a name="create-an-ssh-connection"></a>Hozzon létre egy SSH-kapcsolatot

Először hozzon létre egy SSH-kapcsolatot a csomóponton, amelyen meg kell *kubelet* naplókat. Ez a művelet részleteit a a [SSH-t az Azure Kubernetes Service (AKS) fürtcsomópontok] [ aks-ssh] dokumentumot.

## <a name="get-kubelet-logs"></a>Kubelet-naplók beszerzése

A csomóponthoz csatlakozás után futtassa a következő parancsot a *kubelet* naplók:

```console
sudo journalctl -u kubelet -o cat
```

Az alábbi minta kimenet megjelenítése a *kubelet* adatok:

```
I0508 12:26:17.905042    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:27.943494    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:28.920125    8672 server.go:796] GET /stats/summary: (10.370874ms) 200 [[Ruby] 10.244.0.2:52292]
I0508 12:26:37.964650    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:47.996449    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:58.019746    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:05.107680    8672 server.go:796] GET /stats/summary/: (24.853838ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:27:08.041736    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:18.068505    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:28.094889    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:38.121346    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:44.015205    8672 server.go:796] GET /stats/summary: (30.236824ms) 200 [[Ruby] 10.244.0.2:52588]
I0508 12:27:48.145640    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:58.178534    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:05.040375    8672 server.go:796] GET /stats/summary/: (27.78503ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:28:08.214158    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:18.242160    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:28.274408    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:38.296074    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:48.321952    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:58.344656    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
```

## <a name="next-steps"></a>További lépések

Ha további információkat talál a Kubernetes-főkiszolgálóról van szüksége, tekintse meg [megtekintheti a Kubernetes főcsomópont naplózza az aks-ben][aks-master-logs].

<!-- LINKS - internal -->
[aks-ssh]: ssh.md
[aks-master-logs]: view-master-logs.md

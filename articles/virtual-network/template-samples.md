---
title: Azure Resource Manager-sablonminták virtuális hálózathoz | Microsoft Docs
description: Ismerje meg az Azure virtuális hálózatok üzembe helyezéséhez használható különféle Azure Resource Manager-sablonokat.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/13/2018
ms.author: jdial
ms.openlocfilehash: af0affc549afd8b63fe0d566fac198de054554c1
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/18/2018
ms.locfileid: "31528987"
---
# <a name="azure-resource-manager-template-samples-for-virtual-network"></a>Azure Resource Manager-sablonminták virtuális hálózathoz

Az alábbi táblázatban hivatkozásokat találhat az elérhető Azure Resource Manager-sablonmintákra. A sablonokat az Azure [Portal](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), az Azure [CLI](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy az Azure [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json) segítségével helyezheti üzembe. Saját sablonok létrehozásáról [Az első saját sablon létrehozása](../azure-resource-manager/resource-manager-create-first-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json) és [Az Azure Resource Manager-sablonok szerkezetének és szintaxisának megértése](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json) című cikkben tájékozódhat.


| | |
|----|----|
|[Virtuális hálózat létrehozása két alhálózattal](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)| Létrehoz egy virtuális hálózatot két alhálózattal.|
|[Forgalom irányítása hálózati virtuális készüléken keresztül](https://github.com/Azure/azure-quickstart-templates/tree/master/201-userdefined-routes-appliance)| Létrehoz egy virtuális hálózatot három alhálózattal. Üzembe helyez egy virtuális gépet minden egyes alhálózatban. Létrehoz egy útvonaltáblát, mely a forgalmat az egyik alhálózatból egy másikba irányítja a harmadik alhálózatban futó virtuális gépen keresztül. Hozzárendeli az útvonaltáblát az alhálózatok egyikéhez.|
|[Virtuális hálózati szolgáltatásvégpont létrehozása az Azure Storage szolgáltatáshoz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-2subnets-service-endpoints-storage-integration)|Létrehoz egy új virtuális hálózatot két alhálózattal, mindkét alhálózatban egy-egy hálózati adapterrel. Engedélyez egy Azure Storage-szolgáltatásvégpontot az egyik alhálózathoz, és új tárfiókot is biztosít az adott alhálózat számára.|
|[Két virtuális hálózat csatlakoztatása](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering)| Létrehoz két virtuális hálózatot, illetve egy virtuális hálózati társviszonyt a két hálózat között.|
|[Virtuális gép létrehozása több IP-címmel](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-multiple-ipconfig)| Létrehoz egy Windows vagy Linux rendszerű virtuális gépet több IP-címmel.|

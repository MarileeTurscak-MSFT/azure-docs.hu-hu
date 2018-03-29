---
title: Azure PowerShell-példaszkript – Service Fabric-fürt létrehozása | Microsoft Docs
description: Azure PowerShell-példaszkript – Egy három csomópontos Service Fabric-tesztfürt létrehozása.
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: a03362ebd4b8502f12b7c7bb9aadc558f6a073d2
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/23/2018
---
# <a name="create-a-three-node-test-service-fabric-cluster"></a>Egy három csomópontos Service Fabric-tesztfürt létrehozása

Ez a példaszkript létrehoz egy három csomópontos Service Fabric-tesztfürtöt, melyet egy X.509 tanúsítvány tesz biztonságossá. A három csomópontos fürtkonfiguráció azért támogatott fejlesztési és tesztelési célokra, mert azzal már biztonságosan elvégezhetők frissítések, és átvészelhetők az egyes csomópontok meghibásodásai (feltéve, hogy nem egyszerre következnek be). Az éles fürtök legalább öt vagy több csomópontot igényelnek ahhoz, hogy rugalmasak legyenek az egyszerre előforduló hibákkal szemben.  

A parancs létrehoz egy önaláírt tanúsítványt, és feltölti azt egy új kulcstartóba, amely ugyanabban az erőforráscsoportban jön létre, mint a fürt. A rendszer emellett a tanúsítványt egy helyi könyvtárba is átmásolja.  Állítsa be úgy az *-OS* paramétert, hogy a fürtcsomópontokon futó Windows vagy Linux verzióját válassza.  Szabja testre a paramétereket szükség szerint.

Szükség esetén telepítse az Azure PowerShellt az [Azure PowerShell útmutatójának](/powershell/azure/overview) utasításait követve, majd a `Login-AzureRmAccount` futtatásával hozza létre a kapcsolatot az Azure-ral. 

## <a name="sample-script"></a>Példaszkript

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-test-cluster/create-test-cluster.ps1 "Create a test Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

A példaszkript futtatása után a következő paranccsal távolítható el az erőforráscsoport, a fürt és az összes kapcsolódó erőforrás.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Szkript ismertetése

A szkript a következő parancsokat használja. A táblázatban lévő összes parancs a hozzá tartozó dokumentációra hivatkozik.

| Parancs | Megjegyzések |
|---|---|
| [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Létrehoz egy új Service Fabric-fürtöt. |

## <a name="next-steps"></a>További lépések

Az Azure PowerShell modullal kapcsolatos további információért lásd az [Azure PowerShell dokumentációját](/powershell/azure/overview).

További Azure Powershell-példákat az Azure Service Fabrichez az [Azure PowerShell-példák](../service-fabric-powershell-samples.md) között találhat.

---
title: Azure PowerShell-minták – Alapszintű virtuálisgép-méretezési csoport létrehozása | Microsoft Docs
description: Azure PowerShell-minták
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 9ce9c7b2f0834913a22c66062d3e1b4b264b4ae5
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452188"
---
# <a name="create-a-basic-virtual-machine-scale-set-with-powershell"></a>Alapszintű virtuálisgép-méretezési csoport létrehozása a PowerShell-lel
Ez a szkript létrehoz egy Windows Server 2016 rendszert futtató virtuálisgép-méretezési csoportot. A szkript futtatása után a virtuálisgép-példányokat RDP-kapcsolaton keresztül érheti el.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Példaszkript
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/simple-scale-set/simple-scale-set.ps1 "Create a simple virtual machine scale set")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása
Az alábbi paranccsal eltávolítható az erőforráscsoport, a méretezési csoport és az összes kapcsolódó erőforrás.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Szkript ismertetése
A szkript a következő parancsokat használja az üzemelő példány létrehozásához. A táblázatban lévő összes elem a hozzá tartozó dokumentációra hivatkozik.

| Parancs | Megjegyzések |
|---|---|
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) | Létrehozza a virtuálisgép-méretezési csoportot és az összes támogató erőforrást, beleértve a virtuális hálózatot, a terheléselosztót és a NAT-szabályokat. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Eltávolít egy erőforráscsoportot és az összes abban található erőforrást. |

## <a name="next-steps"></a>További lépések
Az Azure PowerShell modullal kapcsolatos további információért lásd az [Azure PowerShell dokumentációját](/powershell/azure/overview).

A virtuálisgép-méretezési csoportokhoz használható további PowerShell-példaszkripteket az [Azure virtuálisgép-méretezési csoportokra vonatkozó dokumentációjában](../powershell-samples.md) találhat.
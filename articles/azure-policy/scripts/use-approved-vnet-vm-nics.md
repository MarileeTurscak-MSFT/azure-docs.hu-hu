---
title: Azure Policy JSON-minta – Jóváhagyott virtuális hálózat megkövetelése a virtuális gépek hálózati adapterén | Microsoft Docs
description: Ez a JSON-mintaszabályzat megköveteli egy jóváhagyott virtuális hálózat használatát a hálózati adaptereken.
services: azure-policy
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 10/30/2017
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: c2e6170ca81deeab7a692e1588c6975e8a6812b5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34602346"
---
# <a name="use-approved-vnet-for-vm-network-interfaces"></a>Jóváhagyott virtuális hálózat megkövetelése a virtuális gépek hálózati adapterén

Ez a szabályzat megköveteli egy jóváhagyott virtuális hálózat használatát a hálózati adaptereken. A jóváhagyott virtuális hálózat azonosítóját Ön határozza meg.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Példasablon

[!code-json[main](../../../policy-templates/samples/Network/vm-creation-in-approved-vnet/azurepolicy.json "Use approved vNet for VM network interfaces")]

A sablon az [Azure Portal](#deploy-with-the-portal), a [PowerShell](#deploy-with-powershell) vagy az [Azure CLI](#deploy-with-azure-cli) használatával helyezhető üzembe.

## <a name="deploy-with-the-portal"></a>Üzembe helyezés a portállal

[![Üzembe helyezés az Azure-ban](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fvm-creation-in-approved-vnet%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Üzembe helyezés a PowerShell-lel

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "vm-creation-in-approved-vnet" -DisplayName "Use approved vNet for VM network interfaces" -description "This policy enforces VM network interfaces to use vNet." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/vm-creation-in-approved-vnet/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/vm-creation-in-approved-vnet/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -vNetId <vNet Id> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>PowerShell-üzembehelyezés eltávolítása

Az alábbi paranccsal eltávolítható az erőforráscsoport, a virtuális gép és az összes kapcsolódó erőforrás.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Üzembe helyezés az Azure CLI-vel

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'vm-creation-in-approved-vnet' --display-name 'Use approved vNet for VM network interfaces' --description 'This policy enforces VM network interfaces to use vNet.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/vm-creation-in-approved-vnet/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/vm-creation-in-approved-vnet/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "vm-creation-in-approved-vnet"
```

### <a name="clean-up-azure-cli-deployment"></a>Az Azure CLI üzemelő példányának eltávolítása

Az alábbi paranccsal eltávolítható az erőforráscsoport, a virtuális gép és az összes kapcsolódó erőforrás.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>További lépések

- További példák [Az Azure Policy sablonjai](../json-samples.md) oldalon.
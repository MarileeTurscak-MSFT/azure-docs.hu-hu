---
title: Azure CLI-példaszkript – Operációsrendszer-lemez csatolása | Microsoft Docs
description: Azure CLI-példaszkript – Operációsrendszer-lemez csatolása
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: bd1bb1dc6222e5abdc03bb9dc5da93751c54da77
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932306"
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Egy virtuális gép operációsrendszer-lemezének hibaelhárítása

Ez a szkript egy hibás vagy problémás virtuális gép operációsrendszer-lemezét csatolja hozzá adatlemezként egy második virtuális géphez. A használata lemezproblémák hibaelhárításához és adatok helyreállításához javallott.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Példaszkript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Szkript ismertetése

A szkript a következő parancsokat használja egy erőforráscsoport, egy virtuális gép és minden kapcsolódó erőforrás létrehozásához. A táblázatban lévő összes parancs a hozzá tartozó dokumentációra hivatkozik.

| Parancs | Megjegyzések |
|---|---|
| [az vm show](https://docs.microsoft.com/cli/azure/vm#az_vm_show) | Visszaadja a virtuális gépek listáját. Jelen esetben a rendszer a lekérdezési beállítással adja vissza a virtuális gép operációsrendszer-lemezét. Ezt az értéket ezután hozzáadja egy „uri” nevű változóhoz. |
| [az vm delete](https://docs.microsoft.com/cli/azure/vm#az_vm_delete) | Töröl egy virtuális gépet. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Létrehoz egy virtuális gépet.  |
| [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) | Csatlakoztat egy lemezt egy virtuális géphez. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az_vm_list_ip_addresses) | Visszaadja egy virtuális gép IP-címeit. |

## <a name="next-steps"></a>További lépések

Az Azure CLI-vel kapcsolatos további információért lásd az [Azure CLI dokumentációját](https://docs.microsoft.com/cli/azure).

A virtuális gépekhez kapcsolódó további CLI-példaszkripteket az [Azure Linux rendszerű virtuális gépekre vonatkozó dokumentációjában](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) találhat.

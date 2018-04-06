---
title: Az Azure CLI minták |} Microsoft Docs
description: Az Azure CLI-minták
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 938e61a7ab831b6f0632d0426afe4d0f947af223
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/05/2018
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>A Linux virtuális gépek Azure CLI-minták

A következő táblázat az Azure CLI használatával létrehozott bash parancsfájlokra mutató hivatkozásokat tartalmaz.

| | |
|---|---|
|**Virtuális gépek létrehozása**||
| [Virtuális gép létrehozása](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | A Linux virtuális gép minimális konfigurációs hoz létre. |
| [A teljesen konfigurált virtuális gép létrehozása](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások.|
| [Magas rendelkezésre állású virtuális gépek létrehozása](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre. |
| [A Docker engedélyezve van a virtuális gép létrehozása](./../scripts/virtual-machines-linux-cli-sample-create-docker-host.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy virtuális gépet, és konfigurálja a virtuális gép egy Docker-gazdagépként egy NGINX tároló futtatja. |
| [Hozzon létre egy virtuális Gépet, és futtassa a konfigurációs parancsfájl](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy virtuális gépet, és használja az Azure egyéni parancsprogramok futtatására szolgáló bővítmény NGINX telepítéséhez. |
| [Hozzon létre egy virtuális gép WordPress telepítése](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy virtuális gépet, és az Azure egyéni parancsprogramok futtatására szolgáló bővítmény segítségével WordPress telepítése. |
| [A felügyelt operációsrendszer-lemez a virtuális gép létrehozása](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet egy meglévő kezelt lemez az operációs rendszer lemezeként csatolásával. |
| [Hozzon létre egy virtuális Gépet egy pillanatképből](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet egy pillanatképből, először hoz létre egy felügyelt lemezes pillanatképből, és majd az operációs rendszer lemezeként az új kezelt lemez csatolása. |
|**Tárhely kezelése**||
| [Felügyelt lemezes virtuális merevlemez létrehozása](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Egy felügyelt lemezt hoz létre, egy speciális olyan operációs rendszert tartalmazó virtuális Merevlemezt vagy egy virtuális Merevlemezt adatlemez a.  |
| [Hozzon létre egy felügyelt lemezes egy pillanatképből](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Felügyelt lemezes pillanatképet hoz létre. |
| [Felügyelt lemezmásolás ugyanazon vagy másik előfizetésbe](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Másolatot ugyanazon vagy másik előfizetés lemez felügyelt, de ugyanabban a régióban, mint a szülő kezelt lemezre. 
| [Pillanatkép virtuális merevlemez tárfiókba exportálása](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Felügyelt pillanatkép exportálja, virtuális Merevlemezt egy másik régióban található tárfiókot. |
| [Azonos vagy azoktól eltérő előfizetés pillanatkép másolása](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Pillanatkép másolatokat ugyanazon vagy másik előfizetésbe, de a szülő pillanatkép ugyanabban a régióban. |
|**Virtuális gépek hálózati**||
| [Virtuális gépek közötti hálózati forgalmának biztonságossá tétele](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Két virtuális gép minden kapcsolódó erőforrások és egy belső és külső hálózati biztonsági csoportokkal (NSG) hoz létre. |
|**Virtuális gépek védelme**||
| [A Virtuálisgép- és adatlemezek titkosítása](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy Azure Key Vault, a titkosítási kulcs és a szolgáltatás egyszerű, majd a virtuális gépek titkosítja. |
|**Virtuális gépek figyelése**||
| [Virtuális gép és az Operations Management Suite figyelése](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy virtuális gépet, telepíti az Operations Management Suite-ügynököt, és regisztrálja az OMS-munkaterület virtuális gép.  |
|**Virtuális gépek hibaelhárítása**||
| [A virtuális gépek operációs rendszer lemezhiba elhárítása](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | Csatlakoztatja az operációsrendszer-lemez egy virtuális géptől adatlemezt, a második virtuális gép. |
| | |

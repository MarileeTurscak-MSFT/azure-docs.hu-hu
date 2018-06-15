---
title: Linux virtuális gépet az Azure CLI 2.0 másolása |} Microsoft Docs
description: Útmutató egy példányát az Azure Linux virtuális gép Azure CLI 2.0 és a felügyelt lemezek létrehozásához.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 09/25/2017
ms.author: cynthn
ms.openlocfilehash: 66f2789d717816f5be3fd8b298819825f8cd87f7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30905010"
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a>Hozzon létre egy Linux virtuális gép egy másolatát Azure CLI 2.0 és felügyelt lemezek


Ez a cikk bemutatja, hogyan hozzon létre egy másolatot az Azure CLI 2.0 és az Azure Resource Manager telepítési modell segítségével Linux operációs rendszert futtató Azure virtuális gép (VM). Az [Azure CLI 1.0-s](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.

Emellett [töltse fel, és hozzon létre egy virtuális gép olyan virtuális merevlemezről](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Előfeltételek


-   Telepítés [Azure CLI 2.0](/cli/azure/install-az-cli2)

-   Jelentkezzen be Azure-fiókkal rendelkező [az bejelentkezési](/cli/azure/reference-index#az_login).

-   Egy Azure virtuális Gépen, a másolás forrásaként használandó rendelkezik.

## <a name="step-1-stop-the-source-vm"></a>1. lépés: A forrás virtuális gép leállítása


A forrás virtuális gép felszabadítása használatával [az virtuális gép felszabadítása](/cli/azure/vm#az_vm_deallocate).
Az alábbi példa felszabadítja a nevű virtuális gép **myVM** erőforráscsoportban **myResourceGroup**:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

## <a name="step-2-copy-the-source-vm"></a>2. lépés: A forrás virtuális gép másolása


A virtuális gépek másolásához az alapul szolgáló virtuális merevlemez másolatának létrehozása. A folyamat egy speciális virtuális Merevlemezt, amely felügyelt lemezként, mint a forrás virtuális gép konfigurációs és a beállításokat tartalmazó hoz létre.

További információ az Azure Managed Disksről: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md). 

1.  A lemez minden virtuális gép és az operációs rendszer neve lista [az vm lista](/cli/azure/vm#az_vm_list). Az alábbi példa felsorolja az összes virtuális gép nevű erőforráscsoportban **myResourceGroup**:
    
    ```azurecli
    az vm list -g myResourceGroup \
         --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' \
         --output table
    ```

    A kimenet a következő példához hasonló:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Másolás a lemez hozzon létre egy új lemez használatával kezelhetők [az lemez létrehozása](/cli/azure/disk#az_disk_create). Az alábbi példakód létrehozza nevű lemez **myCopiedDisk** nevű felügyelt lemezről **myDisk**:

    ```azurecli
    az disk create --resource-group myResourceGroup \
         --name myCopiedDisk --source myDisk
    ``` 

1.  Ellenőrizheti a felügyelt lemezek most az erőforráscsoportban [az Lemezlista](/cli/azure/disk#az_disk_list). Az alábbi példa felsorolja a felügyelt lemezek nevű erőforráscsoportban **myResourceGroup**:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```


## <a name="step-3-set-up-a-virtual-network"></a>3. lépés: Virtuális hálózat beállítása


A következő lépések hozzon létre egy új virtuális hálózat, az alhálózat, a nyilvános IP-cím és a virtuális hálózati kártya (NIC).

A virtuális gép másolása hibaelhárítási célokra, vagy további központi telepítéseket, előfordulhat, hogy nem használni kívánt virtuális gép egy meglévő virtuális hálózatban.

Ha szeretne létrehozni egy virtuális hálózati infrastruktúra a másolt virtuális gépek, hajtsa végre a következő néhány lépést. Ha nem szeretné létrehozni a virtuális hálózatot, folytassa a [4. lépés: virtuális gép létrehozása](#step-4-create-a-vm).

1.  A virtuális hálózat létrehozása a [az hálózati vnet létrehozása](/cli/azure/network/vnet#az_network_vnet_create). Az alábbi példa létrehoz egy virtuális hálózatot nevű **myVnet** és nevű alhálózat **mySubnet**:

    ```azurecli
    az network vnet create --resource-group myResourceGroup \
        --location eastus --name myVnet \
        --address-prefix 192.168.0.0/16 \
        --subnet-name mySubnet \
        --subnet-prefix 192.168.1.0/24
    ```

1.  Hozzon létre egy nyilvános IP-cím használatával [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#az_network_public_ip_create). Az alábbi példa létrehoz egy nyilvános IP-cím nevű **myPublicIP** a DNS-nevét **mypublicdns**. (A DNS-név kell egyedinek lennie, így adjon meg egy egyedi nevet.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup \
        --location eastus --name myPublicIP --dns-name mypublicdns \
        --allocation-method static --idle-timeout 4
    ```

1.  Hozzon létre a hálózati adapter által használt [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#az_network_nic_create).
    Az alábbi példa létrehoz egy hálózati adapter nevű **myNic** , amely csatolva van a **mySubnet** alhálózati:

    ```azurecli
    az network nic create --resource-group myResourceGroup \
        --location eastus --name myNic \
        --vnet-name myVnet --subnet mySubnet \
        --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a>4. lépés: Virtuális gép létrehozása

Mostantól létrehozhat egy virtuális gép használatával [az virtuális gép létrehozása](/cli/azure/vm#az_vm_create).

Adja meg a másolt felügyelt lemezt kell használni az operációs rendszer lemezeként (--csatolása operációsrendszer-lemez), az alábbiak szerint:

```azurecli
az vm create --resource-group myResourceGroup \
    --name myCopiedVM --nics myNic \
    --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>További lépések

Azure CLI használata az új virtuális gép kezelése című témakörben talál [Azure parancssori felület parancsait az Azure Resource Manager](../azure-cli-arm-commands.md).

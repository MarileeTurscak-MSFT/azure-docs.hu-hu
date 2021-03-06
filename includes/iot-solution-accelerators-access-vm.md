---
title: fájl belefoglalása
description: fájl belefoglalása
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b2b4bfc6aa03039a7eca402f7a9af083a44f0829
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345874"
---
## <a name="access-the-virtual-machine"></a>Hozzáférés a virtuális gép

Az alábbi lépések az a `az` parancsot az Azure Cloud Shellben. Igény szerint is [Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli) a fejlesztést a gépet, és helyileg futtassa a parancsokat.

A következő lépések bemutatják, hogyan konfigurálhatja az Azure virtuális gépen, hogy **SSH** hozzáférést. Látható lépései azt feltételezik a megoldásgyorsító a kiválasztott **contoso-szimuláció** – ezt az értéket cserélje le az üzemelő példány neve:

1. A megoldás gyorsító erőforrásokat tartalmazó erőforráscsoportot tartalmának listázásához:

    ```azurecli-interactive
    az resource list -g contoso-simulation -o table
    ```

    Jegyezze fel a virtuális gépet, a nyilvános IP-cím és a hálózati biztonsági csoport neve – később szüksége lesz ezekre az értékekre.

1. Frissítse a hálózati biztonsági csoportot, hogy az SSH-hozzáférést. Az alábbi parancs feltételezi, hogy a hálózati biztonsági csoport neve **contoso-szimuláció-nsg** – ezt az értéket cserélje le a hálózati biztonsági csoport neve:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name contoso-simulation-nsg -g contoso-simulation --access Allow -o table
    ```

    Fejlesztés és tesztelés során csak engedélyezze az SSH-hozzáférést. Ha engedélyezi az SSH- [újra minél hamarabb tiltsa le,](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices#disable-rdpssh-access-to-azure-virtual-machines).

1. Frissítse a jelszavát a **azureuser** tudja fiók egy jelszót a virtuális gépen. Válassza ki a saját jelszavát, a következő parancs futtatásakor:

    ```azurecli-interactive
    az vm user update --name vm-vikxv --username azureuser --password YOURSECRETPASSWORD  -g contoso-simulation
    ```

1. Keresse meg a virtuális gép nyilvános IP-címét. Az alábbi parancs feltételezi, hogy a virtuális gép neve **vm-vikxv** – ezt az értéket cserélje le a korábban végrehajtott egy megjegyzés, virtuális gép neve:

    ```azurecli-interactive
    az vm list-ip-addresses --name vm-vikxv -g contoso-simulation -o table
    ```

    Jegyezze fel a virtuális gép nyilvános IP-címét.

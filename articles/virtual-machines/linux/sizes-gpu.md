---
title: Azure Linux virtuális gép méretének - GPU |} Microsoft Docs
description: A különböző GPU listák az Azure-ban a Linux virtuális gépekhez elérhető méretek optimalizált. Vcpu, adatlemezek és hálózati adapterek, valamint tárolási átviteli sebesség és a hálózati sávszélesség az a sorozat-méretek számát tartalmazza.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jonbeck
ms.openlocfilehash: c4704dd461ae96600fa812fdfe8d9b0e59e93d72
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/06/2018
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU optimalizált virtuálisgép-méretek

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Illesztőprogram telepítése és ellenőrzési lépések, tekintse meg [N-series illesztőprogram telepítése Linux](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* X ne telepítse kiszolgáló vagy a más rendszerekkel, amelyek használják a `Nouveau` illesztőprogram Ubuntu NC virtuális gépeken. NVIDIA GPU-illesztőprogramok a telepítés előtt le kell tiltania a `Nouveau` illesztőprogram.  

## <a name="other-sizes"></a>Más méretek
- [Általános célú](sizes-general.md)
- [Számításra optimalizált](sizes-compute.md)
- [Memóriaoptimalizált](sizes-memory.md)
- [Tárolásra optimalizált](sizes-storage.md)
- [Nagy teljesítményű számítás](sizes-hpc.md)

## <a name="next-steps"></a>További lépések
További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.
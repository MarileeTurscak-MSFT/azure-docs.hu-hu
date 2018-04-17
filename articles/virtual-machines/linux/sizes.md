---
title: Linux virtuális gép méretének az Azure-ban |} Microsoft Docs
description: A Linux virtuális gépek Azure-ban elérhető különböző méretű sorolja fel.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jonbeck
ms.openlocfilehash: 06dc8075318e3de10a75e7fc1c22e2bdf61afffc
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/16/2018
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Az Azure Linux virtuális gépek méretei
Ez a cikk ismerteti az elérhető méretek és a beállítások a Linux-alkalmazások és munkafolyamatok futtatásához használhatja az Azure virtuális gépekhez. Telepítési szempontok érdemes figyelembe vennie, amikor arra készül használni ezeket az erőforrásokat is biztosít. Ez a cikk érhető el is [Windows virtuális gépek](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| Típus                     | Méretek           |    Leírás       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Általános célú](sizes-general.md)          | B, Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7,  | Kiegyensúlyozott processzor-memória arány. Ideális választás tesztelési-fejlesztési feladatokhoz, kis és közepes méretű adatbázisokhoz, valamint kis és közepes adatforgalmú webkiszolgálókhoz. |
| [Számításra optimalizált](sizes-compute.md)        | Fsv2, Fs, F             | Magas processzor-memória arány. Alkalmas közepes adatforgalmú webkiszolgálók, hálózati berendezések, kötegfolyamatok és alkalmazáskiszolgálók számára.        |
| [Memóriaoptimalizált](sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Magas memória-CPU aránya. Ideális választás relációs adatbázis-kiszolgálókhoz, közepes és nagy gyorsítótárakhoz és memóriabeli elemzésekhez.                 |
| [Tárolásra optimalizált](sizes-storage.md)        | Ls                | Magas lemez-adatátviteli és I/O-műveleti jellemzők. Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.                                                         |
| [GPU](sizes-gpu.md)            | Portok HV, NC, NCv2, NCv3, ND            | Speciális virtuális gépek nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt, valamint a modell betanítási és inferencing (NT) részletes Learning segítségével... Egy vagy több Feldolgozóegységekkel érhető el.       |
| [Nagy teljesítményű számítás](sizes-hpc.md) | H, A8-11          | A leggyorsabb és leghatékonyabb processzorral rendelkező virtuális gépeink, választható nagy átviteli sebességű (távoli közvetlen memória-hozzáférést lehetővé tevő) hálózati adapterrel. 

<br>

- A különböző méretű árazással kapcsolatos információkért lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Az Azure-régiók Virtuálisgép-méretek rendelkezésre állását, lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/).
- Általános korlátozások az Azure virtuális gépeken futó, olvassa el [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).
- További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.


## <a name="rest-api"></a>REST API

A lekérdezés REST API-t használ a Virtuálisgép-méretek információkért tekintse át a következőket:

- [Átméretezéséhez elérhető virtuálisgép-méretek listázása](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [Az előfizetéshez elérhető virtuálisgép-méretek listázása](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [A rendelkezésre állási csoportok elérhető virtuálisgép-méretek listázása](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a>ACU

További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.

## <a name="benchmark-scores"></a>Teljesítményteszt pontszámok

Ismerje meg többet a számítási teljesítmény Linux virtuális gépek használatához a [CoreMark teljesítményteszt pontszámok](compute-benchmark-scores.md).

## <a name="next-steps"></a>További lépések

További információ a különböző Virtuálisgép-méretek érhetők el:
- [Általános célú](sizes-general.md)
- [Számításra optimalizált](sizes-compute.md)
- [Memóriaoptimalizált](sizes-memory.md)
- [Tárolásra optimalizált](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Nagy teljesítményű számítás](sizes-hpc.md)




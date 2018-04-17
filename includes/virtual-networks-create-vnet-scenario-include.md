---
title: fájl belefoglalása
description: fájl belefoglalása
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 29b2ac1a5dc128028dbd40e683c1b45e6208d124
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/16/2018
---
## <a name="scenario"></a>Forgatókönyv

Hozzon létre egy Vnetet és alhálózatok létrehozásával, ez a dokumentum az alábbi forgatókönyvet használja:

![VNet-forgatókönyv](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Ebben a forgatókönyvben a neve VNet létrehozása **TestVNet**, és egy fenntartott CIDR-blokkja a **192.168.0.0./16**. A VNet a következő alhálózatokat tartalmaz: 

* **FrontEnd**, amelynek a CIDR-blokkja **192.168.1.0/24** lesz.
* **BackEnd**, amelynek a CIDR-blokkja **192.168.2.0/24** lesz.


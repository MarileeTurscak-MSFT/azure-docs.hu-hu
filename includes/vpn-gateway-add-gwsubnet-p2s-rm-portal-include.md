---
title: fájl belefoglalása
description: fájl belefoglalása
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a8dec00373d991a7aeaf11f1a4d95463ab71d9a2
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197086"
---
1. Keresse meg a [portálon](http://portal.azure.com) azt a Resource Manager-alapú virtuális hálózatot, amelyhez létre kíván hozni egy virtuális hálózati átjárót.
2. A VNet lap **Beállítások** részén az **Alhálózatok** elemre kattintva bontsa ki az **Alhálózatok** oldalt.
3. Az **Alhálózatok** lap **+Átjáróalhálózat** elemére kattintva nyissa meg az **Alhálózat hozzáadása** lapot. 

  ![Az átjáró alhálózatának hozzáadása](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Az átjáró alhálózatának hozzáadása")
4. Az alhálózat **nevénél** automatikusan megjelenik a „GatewaySubnet” érték. Ez az érték szükséges ahhoz, hogy az Azure felismerje, hogy az alhálózat egy átjáró alhálózata. Módosítsa úgy a **címtartomány** automatikusan kitöltött értékeit, hogy megfeleljenek a konfigurációs követelményeinek, majd a lap alján található **OK** gombra kattintva hozza létre az alhálózatot.

  ![Az alhálózat hozzáadása](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Az alhálózat hozzáadása")
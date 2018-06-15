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
ms.openlocfilehash: 1c3103889bd11df45710cc0d6afaeeba022c9ace
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197720"
---
Az [az network vpn-connection show](/cli/azure/network/vpn-connection#show) paranccsal ellenőrizheti, sikeres volt-e a kapcsolat. A példában a „--name” a tesztelni kívánt kapcsolat nevére utal. Ha a kapcsolat létrehozás alatt áll, a kapcsolat állapota „Connecting”. Ha a kapcsolat létrejött, az állapot „Connected” értékűre változik.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
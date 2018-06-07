---
title: Azure-leképezések forgalom megjelenítése |} Microsoft Docs
description: A Javascript térképen forgalmi adatok megjelenítése
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6ff7a0270509c244fc97bd04d8ba648fd262dc58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600119"
---
# <a name="show-traffic-on-the-map"></a>A térképen forgalom megjelenítése

Ez a cikk bemutatja, hogyan forgalom és események információkat jelenít meg a térképen. 

## <a name="understand-the-code"></a>A kód értelmezése

<iframe height='456' scrolling='no' title='A térképen forgalom megjelenítése' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Tekintse meg a toll <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>forgalom megjelenítése a térképen</a> Azure leképezésekhez (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) a <a href='https://codepen.io'>CodePen</a>.
</iframe>

A fenti kódot az első kódblokkot egy térkép objektumot hoz létre. Látható [térkép létrehozásához](map-create.md) utasításokat.

A második kódblokkot használ [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic) függvény a térkép osztály a forgalom és események a térképen.

## <a name="next-steps"></a>További lépések

További tudnivalók az osztályok és az ebben a cikkben használt módszerek: 
* [térkép](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic)

További kód hozzáadása a maps, tekintse meg a következő cikkeket: 
* [A térkép – egér események való interakció](./map-events.md)
* [Egy elérhető leképezés létrehozása](./map-accessibility.md)

Tekintse meg a [minta kódlap](http://aka.ms/AzureMapsSamples) több leképezés forgatókönyvek esetén.

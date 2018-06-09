---
title: Azure-készletben szolgáltatásokat kínáló |} Microsoft Docs
description: Felhő operátorként a felhasználók számára szolgáltatást is kínál.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 042e65cfe350cb61124ed8920ae3616502e6553d
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248843"
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Az ajánlat Azure verem szolgáltatások áttekintése

*A következőkre vonatkozik: Azure verem integrált rendszerek és az Azure verem szoftverfejlesztői készlet*

[A Microsoft Azure verem](azure-stack-poc.md) egy hibrid felhő platform, amely lehetővé teszi az adatközpontban a szolgáltatások nyújtásához. Szolgáltató szolgáltatást is kínál a bérlők számára. Egy üzleti vagy kormányzati ügynökség kínálhat alkalmazottai számára a helyszíni szolgáltatásokhoz. A szolgáltatások által biztosított foglalja magába, de nem korlátozódik:

- Platform (PaaS) szolgáltatás, például a alkalmazásszolgáltatások, az API Apps, API-függvények, SQL és MySQL.

Még akkor is kombinálhatja services integrálására és összetett megoldások a különböző felhasználók számára.

Ezeket a szolgáltatásokat telepíthet a felhasználók számára, létre kell hoznia [tervek ajánlatok és kvóták](azure-stack-plan-offer-quota-overview.md). A felhasználók majd előfizethetnek a ajánlatok szolgáltatások használatával.

## <a name="plan-your-service-offers"></a>A szolgáltatási ajánlatok tervezése

Amikor arra készül, az ajánlatokat, vegye figyelembe a következő szempontokat:

**Próbaajánlatokra**: új felhasználók, akik ezután frissítheti a további szolgáltatásokat vonzerőt próbaajánlatokra is használhatja. Próbaverziós ajánlat, hozzon létre egy kis [referenciaterv](azure-stack-plan-offer-quota-overview.md#base-plan) egy választható nagyobb bővítmény tervvel.

**Kapacitástervezés**: Előfordulhat, hogy a felhasználók, erőforrások és a rendszer az összes felhasználó eltömődés nagy mennyiségű adása. Teljesítmény érdekében is [konfigurálása a tervek kvótával rendelkező](azure-stack-plan-offer-quota-overview.md#plans) cap-használatra.

**Meghatalmazott szolgáltatók**: biztosíthat mások képes ajánlatok létrehozni a környezetben. Például ha egy szolgáltató, akkor [delegálása](azure-stack-delegated-provider.md) ezt a lehetőséget a viszonteladók számára. Vagy, ha egy szervezet, delegálhatja más osztályok leányvállalatai.

## <a name="next-steps"></a>További lépések

[Hozzon létre egy ajánlatot Azure verem](azure-stack-create-offer.md)

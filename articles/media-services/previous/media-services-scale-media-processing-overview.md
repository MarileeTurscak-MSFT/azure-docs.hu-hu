---
title: Skálázás Media feldolgozása – áttekintés |} A Microsoft Docs
description: Ez a témakör a méretezési Media feldolgozására az Azure Media Services nyújt áttekintést.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 780ef5c2-3bd6-4261-8540-6dee77041387
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: juliako
ms.openlocfilehash: 698a85244d5341224dd9f513c5617b9086e36844
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033073"
---
# <a name="scaling-media-processing-overview"></a>Skálázás Media feldolgozása – áttekintés
Ezen a lapon nyújt áttekintést, és miért médiafeldolgozás méretezését. 

## <a name="overview"></a>Áttekintés
A Media Services-fiókok Fenntartott egység típussal vannak társítva, amely meghatározza a médiafeldolgozási feladatok feldolgozásának sebességét. A következő Fenntartott egység típusok közül választhat: **S1**, **S2** vagy **S3**. Ugyanaz a kódolási feladat például gyorsabban fut, amikor az **S2** Fenntartott egység típust használja az **S1** típus helyett. További információkért lásd: a [szolgáltatás számára fenntartott egység típusok](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

A szolgáltatás számára fenntartott egység típusának meghatározása, mellett megadhatja fiókja ellátása szolgáltatás számára fenntartott egységek. A megadott Fenntartott egységek száma határozza meg az egy adott fiókon egy időben feldolgozható médiafeladatok számát. Például ha a fiókja rendelkezik öt fenntartott egységet, majd öt feldolgozható médiafeladatok fog futni, amíg egy időben, amennyi a feldolgozandó feladatok. A hátralévő feladatok a várólistában várakozik, és feldolgozására egymás után futtatott feladat végeztével lesznek első mértékének. Ha a fiók nem rendelkezik minden üzembe helyezett szolgáltatás számára fenntartott egységeket, majd feladatok fog felvenni, egymás után. Ebben az esetben egy feladat befejeződik, és a következő egy kezdő között a várakozási idő függ az erőforrások rendelkezésre állása a rendszerben.

## <a name="choosing-between-different-reserved-unit-types"></a>Más szolgáltatás számára fenntartott egység típusok közötti választáshoz
Az alábbi táblázat segít a különböző kódolási sebességű közötti kiválasztásakor döntéseket. Teljesítményteszt néhány esetben is kínál, a [letöltheti a videó](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) saját teszteket végrehajtásához:

| Forgatókönyvek | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| A felhasználási esetet |Egyszeres átviteli sebességű kódolás. <br/>SD vagy az alatti megoldások, a fájlok nem idő-és nagybetűket, alacsony költségű. |Egyféle sávszélességű, és több átviteli sebességű kódolás.<br/>Normál használati SD és a HD Encoding. |Egyféle sávszélességű, és több átviteli sebességű kódolás.<br/>Teljes HD és 4K felbontása videókat. Idő-és nagybetűket, a gyorsabb ügyintézés kódolást. |
| A 7 perces videó a teljesítményteszt |Meghatározott kódolásból egy egyféle sávszélességű MP4-fájlt, az azonos felbontású körülbelül 5 percet vesz igénybe. |Idő szükséges kódolásának a "H264 egyféle sávszélességű 720p" előbeállítást körülbelül 8 perc.<br/><br/>Kódolás az "H264 Multiple Bitrate 720p" beállításkészletet körülbelül 16.8 percig tart. |Idő szükséges kódolásának a "H264 egyetlen Bitrate 1080p" előbeállítást megközelítőleg 4 perc.<br/><br/>Kódolás a "H264 Multiple Bitrate 1080p" előbeállítás körülbelül 8 percet vesz igénybe. |


## <a name="considerations"></a>Megfontolandó szempontok
> [!IMPORTANT]
> Tekintse át a jelen szakaszban ismertetett szempontok.  
> 
> 

* A hangelemzés Videóelemzés feladatok és a Media Services v3-as vagy a Video Indexer által aktivált, az S3-egység típusa erősen ajánlott.
* Megosztott tárolókészletet használja, azt jelenti, anélkül, hogy bármilyen szolgáltatás számára fenntartott egységeket, majd encode feladatait van-e ugyanazokat a méretezhetőségi, az S1 szintű fenntartott egységek. Azonban nincs felső korlátja, az idő a tevékenységek várólistára Bajos dolgaival, és a egy adott időpontban legfeljebb csak egy tevékenység fog futni.

## <a name="billing"></a>Számlázás

A számlázás a Media szolgáltatás számára fenntartott egységek tényleges percalapú használatán alapul. – Gyakori kérdések című szakaszában talál részletes magyarázatát, a [Media Services-díjszabás](https://azure.microsoft.com/pricing/details/media-services/) lapot.   

## <a name="quotas-and-limitations"></a>Kvóták és korlátozások
Kvóták és korlátozások és a egy támogatási jegyet kapcsolatos információkért lásd: [kvóták és korlátozások](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Következő lépés
A méretezési media feldolgozási feladatot egy technológiák elérése: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portál](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> A Java SDK legújabb verziójának beszerzéséhez és a Java-fejlesztés megkezdéséhez tekintse meg [Az Azure Media Services Java ügyfél-SDK használatának megkezdése](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use) című cikket. <br/>
> A legfrissebb Media Services PHP SDK letöltéséhez keresse meg a Microsoft Azure-csomag 0.5.7-es verzióját a [Packagist-adattárban](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]


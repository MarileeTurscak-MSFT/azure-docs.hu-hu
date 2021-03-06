---
title: Az Azure IoT Edge-modulok
description: Az IoT Edge modul kínálnak az Azure piactéren elérhető alkalmazás és szolgáltatás-közzétevők.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, IoT Edge module offer
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/22/2018
ms.author: qianw211
ms.openlocfilehash: ecc892a38d5e86a089085cd67a78ce7d00c86fd8
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181116"
---
# <a name="iot-edge-modules"></a>IoT Edge-modulok

A [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) platform az Azure-felhő alapját.  A platform lehetővé teszi a felhasználók közvetlenül IoT-eszközök futtathatnak a felhőalapú számítási feladatok üzembe helyezéséhez.  Egy IoT Edge-modul offline számítási feladatok futtatásához és a helyi adatok elemzéseket végezhet. Ez az ajánlat típusát segít a sávszélességet, a helyi és a bizalmas adatok védelme érdekében, és közel valós idejű válaszidő kínál.  Most már rendelkezik ilyen előre elkészített munkaterhelések előnyeinek kihasználása érdekében a beállításokat. Eddig csak a Microsoft megoldásai kevés volt elérhető.  Kellett az időt és erőforrásokat fektet be a saját egyéni IoT-megoldások létrehozásához.

Bevezetésével a [IoT Edge-modulok az Azure Marketplace-en](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1), hogy most már megvannak a kiadók számára közzé és értékesítheti megoldásaikat az IoT-olvasóknak videóanyagai egyetlen. IoT fejlesztői végső soron találhat és vásárolhat képességek a megoldás fejlesztésének felgyorsításához.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>IoT Edge-modulok az Azure Marketplace-en legfontosabb előnyei:

| **A kiadók számára**    | **Az ügyfelek (IoT fejlesztői)**  |
| :------------------- | :-------------------|
| A fejlesztők létrehozása és üzembe helyezése IoT Edge-megoldások szeretne elérni.  | Állítsa össze az IoT Edge megoldás tudatban, hogy biztonságos és tesztelt összetevőket használ. |
| Közzététele után, és futtathatja a tárolókat támogató IoT Edge hardvereket. | Csökkentse a piacra jutási időt, keresése és üzembe helyezése 1. és 3. fél IoT Edge-modulok igényeinek. |
| A rugalmas számlázási lehetőségeket értékesítheti <ul> <li> Az ingyenes és a saját licencét (BYOL). </li> </ul> | Az Ön által választott számlázási modellek-vásárlásokhoz. <ul> <li> Az ingyenes és a saját licencét (BYOL). </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>Mi az IoT Edge-modul?

Az Azure IoT Edge lehetővé teszi, hogy központi telepítésére és felügyeletére az üzleti logikát az Edge-modulok formájában. Az Azure IoT Edge-modulok IoT Edge által kezelt legkisebb számítási egységek, és a Microsoft-szolgáltatások (például az Azure Stream Analytics), 3. fél szolgáltatásokat, illetve a saját megoldásspecifikus kódját is tartalmazhat. IoT Edge-modulok kapcsolatos további információkért lásd: [megismerheti az Azure IoT Edge-modulok](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).

**Mi a különbség egy tároló ajánlattípusra és a egy IoT Edge-modul ajánlattípusra?**

Az IoT Edge-modul ajánlattípusra egy adott típusú IoT Edge-eszköz futó tárolók. Az IoT Edge-környezetben történő futtatásra alapértelmezett konfigurációs beállításokat tartalmaz, és szükség esetén használja az IoT Edge-modul SDK-t integrálni az IoT Edge-futtatókörnyezet.

## <a name="publishing-your-iot-edge-module"></a>Az IoT Edge-modul közzététele

**A jobb oldali kirakat kiválasztása**

IoT Edge-modulok csak az Azure Marketplace-en közzétett, az appsource-ban nem vonatkozik.  A különbségeket és a célközönség kirakattípus között további információkért lásd: [a közzétételi lehetőség a megoldás meghatározása](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type).
 
**Számlázási módok**

A Piactér jelenleg támogatja **ingyenes** és **Bring Your saját licences (BYOL)** számlázási lehetőségekkel, az IoT Edge-modulok.
 
**Közzétételi beállítások**

Minden esetben az IoT Edge-modulok kell választania a **Transact** közzététel lehetőséget.  Lásd: [válasszon egy közzétételi lehetőség](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) közzétételi beállítások további részleteiért.  

## <a name="eligibility-criteria"></a>Megfelelőségi feltételeket

Házirend és a Microsoft Azure Marketplace-en szerződéseket a feltételeket az IoT Edge-modul ajánlatok vonatkoznak.  Emellett nincsenek előfeltételeket és az IoT Edge-modulok vonatkozó technikai követelményeit.  

**Előfeltételek**

Egy IoT Edge-modul az Azure piactéren való közzétételéhez szeretne megfelelnek a következő előfeltételeknek:

- A Cloud Partner portálra (CPP) való hozzáférést. További információkért lásd: [közzétételi útmutató az Azure Marketplace és AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Az IoT Edge-modul egy Azure Container Registry a gazdagép. 
- Az IoT Edge modul metaadatok készen áll, például a (nem teljes lista) rendelkezik: 
    - Cím
    - Egy leírást (HTML formátumban)
    - Emblémakép (PNG formátumú és 40x40px, 90x90px, 115x115px, 255x115px többek között a rögzített lemezkép mérete)
    - Egy kifejezés feltételei és adatvédelmi szabályzat
    - Alapértelmezett modul konfigurációjának (útvonalat, ikereszköz kívánt tulajdonságait, createOptions, a környezeti változók)
    - Dokumentáció
    - Támogatási kapcsolattartók

**Technikai követelmények**

Az IoT Edge-modul, ahhoz, hogy hitelesített, és az Azure Marketplace-en közzétett, az elsődleges műszaki követelményeknek a részletes leírást talál a [IoT Edge-modul minősítési folyamat](https://cloudpartner.azure.com/#documentation/iot-edge-module-certification-process) a a [felhő közzétételi Portál](https://cloudpartner.azure.com/).  

## <a name="documentation-and-resources"></a>Dokumentáció és források

Ha bejelentkezett érhetők el az alábbi cikkeket a [felhő közzétételi portál](https://cloudpartner.azure.com/):

- [Hozzon létre egy IoT Edge-modul ajánlatot](https://cloudpartner.azure.com/#documentation/create-iot-edge-module-offer) – – a felhő közzétételi portál kínálnak a lépéseket egy új IoT Edge-modul közzétételéhez.
- [IoT Edge-modul minősítési folyamat](https://cloudpartner.azure.com/#documentation/iot-edge-module-certification-process) – a lépéseket és a egy IoT Edge-modul tanúsítása összegzését.
- [IoT Edge-modul – gyakori kérdések](https://cloudpartner.azure.com/#documentation/iot-edge-module-faq) – – IoT Edge-modulok kapcsolatos gyakori kérdések listáját.

## <a name="next-steps"></a>További lépések

Ha ezt még nem tette meg,

- Regisztrálja a [Microsoft Partner Network](https://partner.microsoft.com/membership).
- Hozzon létre egy [Microsoft Account](https://account.microsoft.com/account/) (szükséges az Azure Marketplace-en transact ajánlatok; mások számára ajánlott).
- Küldje el a [Marketplace regisztrációs űrlap](https://azuremarketplace.microsoft.com/sell/signup).

Ha regisztrálva van és a egy új ajánlat létrehozása vagy egy meglévő, dolgozik

- Jelentkezzen be a [Cloud Partner Portalon](https://cloudpartner.azure.com/) létrehozni, vagy végezze el az ajánlatot.

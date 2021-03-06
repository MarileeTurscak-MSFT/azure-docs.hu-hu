---
title: Virtuális merevlemez létrehozása közben gyakori hibáinak elhárítása |} A Microsoft Docs
description: Válaszok a gyakori hibaelhárítási kérdések és problémák virtuális merevlemez létrehozása közben.
services: Azure Marketplace
documentationcenter: ''
author: HannibalSII
manager: ''
editor: ''
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c4e88a9fbb15dd90d619b159ae1065dfacc1907f
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39713397"
---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a>Virtuális merevlemez létrehozása során tapasztalt gyakori hibáinak elhárítása
Ez a cikk segít az Azure Marketplace kiadókra és/vagy társ-rendszergazdaként, előfordulhat, hogy egy probléma vagy gyakori kérdései közzététele, vagy a virtuális gép megoldások kezelése során megadott.

1. Hogyan változtatható meg a gazdagép neve?
   
    Virtuális gép létrehozása után a felhasználók a gazdagép neve nem lehet frissíteni.
2. Hogyan lehet a távoli asztali szolgáltatás vagy a bejelentkezési jelszavának alaphelyzetbe állítása?
   
   * [Referencia a Windows virtuális gép](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Linux rendszerű virtuális gép referenciája](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Hogyan hozhat létre új ssh-tanúsítványok?
   
   Tekintse meg a hivatkozást: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Hogyan lehet a nyitott VPN-tanúsítványának konfigurálása?
   
   Tekintse meg a hivatkozást: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. Mi a Microsoft kiszolgálói szoftverek futtatásához a Microsoft Azure virtuális gép környezetben (infrastruktúra--szolgáltatásként) a támogatási házirenddel?
   
   Tekintse meg a hivatkozást: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Rendelkezik a virtuális gépek minden egyedi azonosítója?
   
   Azure kódol Azure virtuális gép egyedi azonosítója az összes virtuális Géphez. Ebben a blogbejegyzésben és dokumentáció részletek megtekintéséhez.
7. A virtuális gép hogyan kezelhetők az egyéni szkriptek bővítményét az indítási feladat?
   
   Tekintse meg a hivatkozást: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Virtuális gép létrehozása az Azure Portalon, a prémium szintű tárolóba feltöltött virtuális merevlemez használatával hogyan?
   
   Ez a funkció még nem támogatott.
9. Támogatja a 32 bites alkalmazást az Azure piactéren?
   
   Tekintse meg a hivatkozást a támogatási házirend: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. Minden alkalommal, amikor a tapasztalataimat szeretném-lemezkép készítése saját virtuális merevlemezek, a hibaüzenet jelenik meg: ". Virtuális merevlemez már regisztrálva van a lemezképtárban erőforrásként"a PowerShellben. Nem tudok hozott létre bármilyen kép előtt sem volt található ilyen nevű képet az Azure-ban. Hogyan oldhatom fel ez?
    
    Ez általában fordulhat elő, ha a felhasználó egy virtuális Gépet a VHD-vel kiosztott és zár van a virtuális Merevlemezt. Ellenőrizze, hogy nincs-e nincs virtuális gép foglalja le a VHD-t. Ha a hiba továbbra is fennállnak, majd hozzon létre egy támogatási jegyet, ezen hivatkozás használatával vagy a közzétételi portál kapcsolatos ez (részletek 11 kérdés-válasz van megadva).
---
title: Hyper-V virtuális gépek vészhelyreállítása az Azure-bA a helyszíni Hyper-V kiszolgáló előkészítése |} A Microsoft Docs
description: Ismerje meg, hogyan készíti elő az Azure Site Recovery szolgáltatással az Azure-bA vész-helyreállítási System Center VMM által nem felügyelt helyszíni Hyper-V virtuális gépeket.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 09/12/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: f1899817ee2d0efec4ab561a64f24e49cb173c29
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720769"
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Vészhelyreállítás az Azure-bA a helyszíni Hyper-V kiszolgálók előkészítése

Ez az oktatóanyag bemutatja, hogyan készítse elő a helyszíni Hyper-V infrastruktúra, amikor a Hyper-V virtuális gépek replikálása az Azure-bA a vész-helyreállítási alkalmazásában szeretné. A Hyper-V-gazdagépek a System Center Virtual Machine Manager (VMM) által kezelhető, de ez nem szükséges.  Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Tekintse át a Hyper-V követelményeinek, és a VMM-ben követelmények, ha van ilyen.
> * Készítse elő a VMM-ben, ha van ilyen
> * Az Azure-helyen internet-hozzáférés ellenőrzése
> * Az Azure-bA a feladatátvételt követően érhetik el azokat, hogy a virtuális gépek előkészítése

Ez a sorozat második oktatóanyaga. Győződjön meg arról, hogy [beállította az Azure-összetevőket](tutorial-prepare-azure.md) az előző oktatóanyagban leírtak szerint.



## <a name="review-requirements-and-prerequisites"></a>Tekintse át a követelmények és Előfeltételek

Győződjön meg arról, hogy a Hyper-V-gazdagépek és virtuális gépek megfelelnek a követelményeinek.

1. [Győződjön meg arról](hyper-v-azure-support-matrix.md#on-premises-servers) helyszíni kiszolgáló követelményei.
2. [A követelmények ellenőrzéséhez](hyper-v-azure-support-matrix.md#replicated-vms) az Azure-bA replikálni kívánt Hyper-V virtuális gépek számára.
3. Ellenőrizze a Hyper-V-gazdagép [hálózati](hyper-v-azure-support-matrix.md#hyper-v-network-configuration); és a gazdagép és vendég [tárolási](hyper-v-azure-support-matrix.md#hyper-v-host-storage) támogatása a helyszíni Hyper-V-gazdagépek.
4. Ellenőrizze az Azure támogatott [hálózati](hyper-v-azure-support-matrix.md#azure-vm-network-configuration-after-failover), [tárolási](hyper-v-azure-support-matrix.md#azure-storage) és [számítási](hyper-v-azure-support-matrix.md#azure-compute-features) lehetőségeit a feladatátvételt követően.
5. Az Azure-ba replikált helyszíni virtuális gépeknek meg kell felelniük az [Azure virtuális gépekre vonatkozó feltételeinek](hyper-v-azure-support-matrix.md#azure-vm-requirements).


## <a name="prepare-vmm-optional"></a>Készítse elő a VMM-ben (nem kötelező)

Ha Hyper-V-gazdagépek a VMM által felügyelt, akkor készítse elő a helyszíni VMM-kiszolgáló. 

- Ellenőrizze, hogy a VMM-kiszolgáló rendelkezik egy legalább egy felhő, egy vagy több gazdagépcsoportot. A Hyper-V-gazdagép, amelyen a virtuális gépek futnak a felhőben található kell lennie.
- Hálózatleképezés előkészítése a VMM-kiszolgálón.

### <a name="prepare-vmm-for-network-mapping"></a>Hálózatleképezés előkészítése a VMM-ben

VMM-ben használata [hálózatleképezés](site-recovery-network-mapping.md) helyszíni VMM-Virtuálisgép-hálózatok és az Azure virtuális hálózatok közötti leképezéseket. Leképezés biztosítja, hogy az Azure virtuális gépek csatlakoznak a megfelelő hálózathoz, a feladatátvételt követően felálló.

Hálózatleképezés módon előkészítése a VMM-ben:

1. Ellenőrizze, hogy egy [VMM logikai hálózata](https://docs.microsoft.com/system-center/vmm/network-logical) társított, amely a felhő, amelyben a Hyper-V-gazdagépek találhatók.
2. Ellenőrizze, hogy egy [Virtuálisgép-hálózat](https://docs.microsoft.com/system-center/vmm/network-virtual) a logikai hálózathoz társított.
3. A Virtuálisgép-hálózatot a virtuális gépek csatlakozni a VMM-ben.

## <a name="verify-internet-access"></a>Internet-hozzáférés ellenőrzése

1. Az oktatóanyag az alkalmazásában a legegyszerűbb konfiguráció van, a Hyper-V-gazdagépek és a VMM-kiszolgáló közvetlen internet-hozzáféréssel rendelkezik egy proxykiszolgáló használata nélkül. 
2. Győződjön meg arról, hogy a Hyper-V-gazdagépek, és ha szükséges, a VMM-kiszolgáló férhet hozzá a szükséges URL-címek az alábbi.   
3. Ha IP-cím alapján szabályozására van, ellenőrizze, hogy:
    - Csatlakozhat IP-cím-alapú tűzfalszabályainak [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) portot.
    - Lehetővé teszik az IP-címtartományok esetében az előfizetés Azure-régió.
    
### <a name="required-urls"></a>Kötelező URL-címek


[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Felkészülés az Azure virtuális gépekhez való kapcsolódásra a feladatátvételt követően

Feladatátviteli forgatókönyv során érdemes a replikált helyszíni hálózathoz való csatlakozáshoz.

Windows virtuális géphez való kapcsolódásra a feladatátvételt követően RDP segítségével, hozzáférés engedélyezése a következő:

1. Ha internetes hozzáférést kíván használni, engedélyezze az RDP-t a helyszíni virtuális gépen a feladatátvétel előtt. Ellenőrizze, hogy a **Nyilvános** résznél felvette-e a listára a TCP- és UDP-szabályokat, valamint hogy a **Windows tűzfal** > **Engedélyezett alkalmazások** területén az összes profil számára engedélyezve van-e az RDP.
2. Ha helyek közötti VPN-kapcsolatot kíván használni, engedélyezze az RDP-t a helyszíni gépen. Engedélyezze az RDP-t a **Windows tűzfal** -> **Engedélyezett alkalmazások és szolgáltatások** területén a **Tartomány és Privát** hálózatok számára.
   Ellenőrizze, hogy az operációs rendszer tárolóhálózati szabályzata **OnlineAll** értékre van-e állítva. [További információk](https://support.microsoft.com/kb/3031135). A virtuális gépen nem lehetnek függőben lévő Windows-frissítések a feladatátvétel elindításakor. Ha vannak, a frissítés befejeződéséig nem fog tudni bejelentkezni a virtuális gépre.
3. A feladatátvételt követően ellenőrizze a **Rendszerindítási diagnosztika** részt a Windows Azure virtuális gépen a virtuális gép képernyőképének megtekintéséhez. Ha nem sikerül, ellenőrizze, hogy fut-e a virtuális gép, majd tekintse át a [hibaelhárítási tippeket](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

A feladatátvételt követően az Azure virtuális gépek ugyanazon IP-címet használja a replikált helyszíni virtuális gép vagy egy másik IP-címmel érheti el. [További](concepts-on-premises-to-azure-networking.md) feladatátvételi IP-címzés beállítása.

## <a name="next-steps"></a>További lépések

> [!div class="nextstepaction"]
> [Az Azure-bA vészhelyreállítás beállítása a Hyper-V virtuális gépek](tutorial-hyper-v-to-azure.md)
> [vészhelyreállítás beállítása az Azure-bA Hyper-V virtuális gépek VMM-felhőkben](tutorial-hyper-v-vmm-to-azure.md)

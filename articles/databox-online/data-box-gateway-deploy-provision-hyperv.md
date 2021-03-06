---
title: 'Oktatóanyag: Az Azure Data Box Gateway üzembe helyezése a Hyper-V-ben | Microsoft Docs'
description: Az Azure Data Box Gateway üzembe helyezésével foglalkozó második oktatóanyag, amely a virtuális eszközök Hyper-V-ben való üzembe helyezését tárgyalja.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: bf744d2aaab168b8ce918f7b776d8855cdc5ad16
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975243"
---
# <a name="tutorial-provision-azure-data-box-gateway-in-hyper-v-preview"></a>Oktatóanyag: Az Azure Data Box Gateway üzembe helyezése a Hyper-V-ben

## <a name="overview"></a>Áttekintés

Az oktatóanyag azt mutatja be, hogyan lehet üzembe helyezni a Data Box Gatewayt egy gazdarendszeren, amely Windows Server 2016-on, Windows Server 2012 R2-n vagy Windows Server 2012-n futtat Hyper-V-t. 

A virtuális eszközök üzembe helyezéséhez és konfigurálásához rendszergazdai jogosultság szükséges. Az üzembe helyezés és a kezdeti beállítás körülbelül 10 percet vesz igénybe.

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * A gazdagép minimális eszközkövetelményeknek való megfelelésének ellenőrzése
> * Virtuális eszköz üzembe helyezése a hipervizorban
> * A virtuális eszköz elindítása és az IP-cím lekérése

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

> [!IMPORTANT]
> - A Data Box Gateway előzetes verzióban érhető el. A megoldás megrendelése és üzembe helyezése előtt tekintse át az [Azure előzetes verziókra vonatkozó szolgáltatási feltételeit](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Előfeltételek

A virtuális eszközök a Hyper-V-t Windows Server 2016 vagy Windows Server 2012 R2 rendszeren futtató gazdarendszeren való üzembe helyezésének előfeltételei a következők.

### <a name="for-the-data-box-gateway-resource"></a>Data Box Gateway-erőforrás esetén

Mielőtt hozzákezd, győződjön meg az alábbiakról:

* [Az Azure Portal a Data Box Gatewayhez való előkészítését](data-box-gateway-deploy-prep.md) ismertető szakaszban foglalt összes lépést végrehajtotta.
* Letöltötte a virtuális eszköz Hyper-V-rendszerképét az Azure Portalról [a portál a Data Box Gatewayhez való előkészítését](data-box-gateway-deploy-prep.md) ismertető szakaszban leírtak szerint.

  > [!IMPORTANT]
  > A Data Box Gatewayen futó szoftver csak a Data Box Gateway-erőforrással használható.
 
### <a name="for-the-data-box-gateway-virtual-device"></a>Virtuális Data Box Gateway-eszköz esetén

Az eszköz üzembe helyezése előtt győződjön meg az alábbiakról:

* Rendelkezik hozzáféréssel a Hyper-V-t Windows Server 2012 R2 vagy újabb rendszeren futtató olyan gazdarendszerhez, amely eszközök üzembe helyezésére is használható.
* A gazdarendszer képes elkülöníteni az alábbi erőforrásokat a virtuális eszköz üzembe helyezése érdekében:

  * Legalább 4 mag.
  * Legalább 8 GB RAM. 
  * Egy hálózati adapter.
  * 250 GB-os operációsrendszer-lemez.
  * 2 TB-os virtuális lemez az adatok számára.

### <a name="for-the-network-in-the-datacenter"></a>Az adatközpont hálózata esetén

Előkészületek:

- Tekintse át a Data Box Gateway üzembe helyezésére vonatkozó hálózati követelményeket, és azoknak megfelelően konfigurálja az adatközponti hálózatot. További információkért lásd [a Data Box Gateway hálózati követelményeit](data-box-gateway-system-requirements.md#networking-requirements) ismertető szakaszt.
- Az eszköz optimális működéséhez gondoskodjon róla, hogy az internetes sávszélesség legalább 20 Mb/s legyen.


## <a name="check-the-host-system"></a>A gazdarendszer ellenőrzése

Virtuális eszköz létrehozásához a következőkre lesz szüksége:

* Egy Hyper-V-szerepkörre, amely Windows Server 2016, Windows Server 2012 R2 vagy Windows Server 2012 rendszerre van telepítve.
* Egy, a gazdagépre csatlakoztatott Microsoft Windows-ügyfélen futó Microsoft Hyper-V-kezelőre.
* Bizonyosodjon meg róla, hogy a mögöttes hardver (a gazdarendszer), amelyen a virtuális eszközt létrehozza, képes elkülöníteni az alábbi erőforrásokat a virtuális eszköz számára:

    * Legalább 4 mag.
    * Legalább 8 GB RAM.
    * Egy, a hálózatra csatlakozó hálózati adapter, amely képes a forgalmat az internetre irányítani. .
    * 250 GB-os operációsrendszer-lemez.
    * 2 TB-os virtuális lemez a rendszeradatok számára.

## <a name="provision-a-virtual-device-in-hypervisor"></a>Virtuális eszköz üzembe helyezése a hipervizorban

A következő lépések végrehajtásával helyezzen üzembe egy eszközt a hipervizoron.

1. A Windows Server-gazdagépen másolja a virtuális eszköz rendszerképét a helyi meghajtóra. Ezt a VHDX-rendszerképet az Azure Portalról töltötte le. Jegyezze fel a helyet, ahová a rendszerképet másolta, mivel az eljárás későbbi szakaszában szükség lesz rá.
2. Nyissa meg a **Kiszolgálókezelőt**. A jobb felső sarokban kattintson az **Eszközök** gombra, és válassza a **Hyper-V kezelője** lehetőséget.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image1.png)  
  
3. A **Hyper-V kezelője** hatókörpanelén kattintson jobb gombbal a rendszercsomópontra, majd a megnyíló helyi menüben kattintson az **Új** > **Virtuális gép** elemre.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image2.png)
4. Az Új virtuális gép varázsló **Előkészületek** lapján kattintson a **Tovább** gombra.
5. A **Név és hely megadása** lapon adja meg a virtuális eszköz **nevét**. Kattintson a **Tovább** gombra.
   
   > [!IMPORTANT]
   > Ebben a kiadásban csak nagybetűs karaktereket használhat a virtuális eszköz nevében.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image3.png)
6. A **Generáció beállítása** lapon válassza a **2. generáció** lehetőséget a .vhdx-eszköz rendszerképének típusaként, majd kattintson a **Tovább** gombra.    

   ![](./media/data-box-gateway-deploy-provision-hyperv/image4.png)
7. A **Memória hozzárendelése** lapon adjon meg egy legalább **8192 MB** méretű **indítási memóriát**, ne engedélyezze a dinamikus memóriát, majd kattintson a **Tovább** gombra.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image5.png) 
8. A **Hálózatkezelés beállítása** lapon adja meg az internetre csatlakoztatott virtuális kapcsolót, majd kattintson a **Tovább** gombra.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image6.png)
9. A **Virtuális merevlemez csatlakoztatása** lapon válassza a **Meglévő virtuális merevlemez használata** lehetőséget, adja meg a virtuális eszköz rendszerképének helyét, majd kattintson a **Tovább** gombra.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image7.png)
10. Tekintse át az **összefoglalást**, majd kattintson a **Befejezés** gombra a virtuális gép létrehozásához.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image8.png)
11. 4 magra van szükség ahhoz, hogy meg tudjon felelni a minimális követelményeknek. A 4 virtuális processzor hozzáadásához válassza ki a gazdarendszert a **Hyper-V kezelő** ablakában. A jobb oldali panel **Virtuális gépek** listájában keresse meg az imént létrehozott virtuális gépet. Jelölje ki, kattintson a jobb gombbal a gép nevére, majd válassza a **Beállítások** elemet.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image9.png)
12. A **Beállítások** lap bal oldali panelén kattintson a **Processzor** lehetőségre. A jobb oldali panelen állítsa a **virtuális processzorok számát** 4-re (vagy többre). Kattintson az **Alkalmaz** gombra.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image10.png)
13. Ahhoz, hogy megfeleljen a minimális követelményeknek, egy 2 TB méretű virtuális adatlemezt is hozzá kell adnia. A **Beállítások** lapon:

    1. A bal oldali panelen válassza az **SCSI-vezérlő** lehetőséget.
    2. A jobb oldali panelen válassza a **Merevlemez** lehetőséget, és kattintson a **Hozzáadás** gombra.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image11.png)
14. A **Merevlemez** lapon válassza a **Virtuális merevlemez** lehetőséget, és kattintson az **Új** gombra. Megnyílik az **Új virtuális merevlemez varázsló**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image12.png)
1. Az Új virtuális merevlemez varázsló **Előkészületek** lapján kattintson a **Tovább** gombra.
2. A **Lemezformátum kiválasztása** lapon fogadja el az alapértelmezett **VHDX** formátumot. Kattintson a **Tovább** gombra.
   
17. A **Lemeztípus kiválasztása** lapon állítsa a virtuális merevlemez típusát a **Dinamikusan bővülő** típusra (ajánlott). A **Rögzített méretű** lemez is működhet, de előfordulhat, hogy túl sokat kellene várnia. Azt javasoljuk, hogy ne alkalmazza a **Különbség** beállítást. Kattintson a **Tovább** gombra. 

    ![](./media/data-box-gateway-deploy-provision-hyperv/image13.png)
18. A **Név és hely megadása** lapon adja meg az adatlemez **nevét** és **helyét** (ez utóbbit tallózva is megkeresheti). Kattintson a **Tovább** gombra.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image14.png)
19. A **Lemez beállítása** lapon válassza az **Új üres virtuális merevlemez létrehozása** lehetőséget, és adjon meg legalább **2 TB** méretet. Bár a minimális méretkövetelmény a 2 TB, mindig megadhat ennél nagyobb méretű lemezt is. Vegye figyelembe, hogy a lemez mérete az üzembe helyezést követően nem csökkenthető,  egy adatlemez hozzáadásával azonban bővíteni lehet. Kattintson a **Tovább** gombra.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image15.png)
20. Az **Összefoglalás** lapon tekintse át a virtuális adatlemez adatait, és ha elégedett, kattintson a **Befejezés** gombra a lemez létrehozásához. A varázsló bezáródik, és a rendszer hozzáadja a virtuális merevlemezt a géphez.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image16.png)
21. Lépjen vissza a **Beállítások** lapra. Az **OK** gombra kattintva zárja be a **Beállítások** lapot, és lépjen vissza a Hyper-V kezelőjének ablakára.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image17.png)

## <a name="start-the-virtual-device-and-get-the-ip"></a>A virtuális eszköz elindítása és az IP-cím lekérése
Az alábbi lépések végrehajtásával indítsa el a virtuális eszközt, és csatlakozzon hozzá.

#### <a name="to-start-the-virtual-device"></a>A virtuális eszköz indítása
1. Indítsa el a virtuális eszközt.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image18.png)
2. Amikor az eszköz már fut, jelölje ki, kattintson rá a jobb gombbal, és válassza a **Csatlakozás** lehetőséget.

3. 10–15 percig is eltarthat, amíg az eszköz elkészül. A folyamat előrehaladtát egy állapotüzenet jelzi a konzolon. Ha az eszköz kész, lépjen a **Művelet** területre. A `Ctrl + Alt + Delete` billentyűparanccsal jelentkezzen be a virtuális eszközre. Az alapértelmezett felhasználó az *EdgeUser*, az alapértelmezett jelszó pedig a *Password1*.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image21.png)
4. Biztonsági okokból az eszköz rendszergazdai jelszava az első bejelentkezéskor lejár. A rendszer megkéri a jelszó módosítására.

   Olyan jelszót adjon meg, amely legalább 8 karakterből áll. A jelszónak a következő négy típusból legalább hármat tartalmaznia kell: kisbetűk, nagybetűk, számok és speciális karakterek. Írja be ismét a jelszót a megerősítéséhez. A rendszer értesíti, hogy a jelszó megváltozott.
   
5. A jelszó sikeres módosítását követően előfordulhat, hogy a virtuális eszköz újraindul. Várjon, amíg az eszköz elindul.  Megjelenik az eszköz Windows PowerShell-konzolja és egy folyamatjelző.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image22.png)

6. A 6–8. lépést csak akkor kell végrehajtani, ha nem DHCP-környezetben végzi a rendszerindítást. Ha DHCP-környezetben van, hagyja ki ezeket a lépéseket, és folytassa a 9. lépéssel. Ha nem DHCP-környezetben indította az eszközt, erről egy üzenet tájékoztatja.
    
7. A hálózat konfigurálásához a `Get-HcsIpAddress` parancs használatával listázza ki a virtuális eszközön engedélyezett hálózati adaptereket. Ha az eszközön egyetlen hálózati adapter van engedélyezve, az ehhez az adapterhez rendelt alapértelmezett név az `DATA1`.

8. A `Set-HcsIpAddress` parancsmaggal konfigurálhatja a hálózatot. Lásd a következő példát:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`
    
9. Miután a kezdeti beállítás befejeződött és az eszköz elindult, az eszköz szalagcímének szövege jelenik meg. Jegyezze fel a szalagcímen megjelenő IP- és URL-címet az eszköz kezeléséhez. Az IP-címmel csatlakozhat a virtuális készülék webes kezelőfelületéhez, ahol elvégezheti a helyi beállítást és az aktiválást.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image23.png)
      

Ha az eszköz nem felel meg a minimális konfigurációs követelményeknek, egy hibaüzenet jelenik meg a szalagcím szövegében. Módosítsa az eszköz konfigurációját, hogy a gép elegendő erőforrással rendelkezzen a minimális követelmények kielégítéséhez. Ezután újraindíthatja az eszközt, és csatlakozhat hozzá. A minimális konfigurációs követelményeket az [1. lépésben tekintheti meg, amely ismerteti, hogyan ellenőrizheti a gazdarendszer megfelelését a virtuális eszközökre vonatkozó minimális követelményeknek](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

<!--If you face any other error during the initial configuration using the local web UI, refer to the following workflows:

* Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).-->

## <a name="next-steps"></a>További lépések

Ebben az oktatóanyagban a Data Box Gatewayjel kapcsolatos alábbi témakörökkel ismerkedett meg:

> [!div class="checklist"]
> * A gazdagép minimális eszközkövetelményeknek való megfelelésének ellenőrzése
> * Virtuális eszköz üzembe helyezése a hipervizorban
> * A virtuális eszköz elindítása és az IP-cím lekérése

A következő oktatóanyag a virtuális eszköz csatlakoztatását, beállítását és aktiválását mutatja be.

> [!div class="nextstepaction"]
> [A Data Box Gateway csatlakoztatása és beállítása](./data-box-gateway-deploy-connect-setup-activate.md)



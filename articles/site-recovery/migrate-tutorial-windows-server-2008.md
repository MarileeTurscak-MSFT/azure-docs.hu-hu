---
title: A helyszíni Windows Server 2008 kiszolgálók áttelepítése az Azure-bA az Azure Site Recoveryvel |} A Microsoft Docs
description: Ez a cikk ismerteti, hogyan telepítheti át a helyszíni Windows Server 2008 gépek az Azure-ba, az Azure Site Recovery használatával.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/22/2018
ms.author: bsiva
ms.openlocfilehash: d15a5b62a148e971c0740f01744fce308e502340
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47056036"
---
# <a name="migrate-servers-running-windows-server-2008-to-azure"></a>Az Azure-ban Windows Server 2008 rendszerű kiszolgálók áttelepítése

Az oktatóanyag bemutatja, hogyan telepítheti át a Windows Server 2008 vagy 2008 R2 rendszerű Azure-bA a helyszíni kiszolgálók Azure Site Recovery használatával. Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Készítse elő a helyszíni környezetet az áttelepítésre
> * A célkörnyezet beállítása
> * Replikációs szabályzat beállítása
> * A replikáció engedélyezése
> * A várnak megfelelő működés ellenőrzése egy áttelepítési teszt futtatásával
> * Feladatátvétel az Azure-bA és az áttelepítés befejezése

A korlátozások és ismert problémák szakaszban néhány korlátozás és a lehetséges megoldások az ismert problémák, amikor listák You may encounter közben áttelepítése Windows Server 2008 számítógépek, az Azure-bA. 


## <a name="supported-operating-systems-and-environments"></a>Támogatott operációs rendszerek és környezetek


|Operációs rendszer  | A helyszíni környezetben  |
|---------|---------|
|A Windows Server 2008 SP2 - 32 bites és 64 bites (IA-32 és x86-64)</br>– Standard</br>– Enterprise</br>-Adatközpont   |     VMware virtuális gépek, Hyper-V virtuális gépek és fizikai kiszolgálók    |
|A Windows Server 2008 R2 SP1 – 64 bites</br>– Standard</br>– Enterprise</br>-Adatközpont     |     VMware virtuális gépek, Hyper-V virtuális gépek és fizikai kiszolgálók|

> [!WARNING]
> - Futó Server Core kiszolgálók áttelepítése nem támogatott.
> - Gondoskodjon arról, hogy a legújabb szervizcsomaggal és való migrálás előtt telepített Windows-frissítések.


## <a name="prerequisites"></a>Előfeltételek

A Kezdés előtt hasznos lehet az Azure Site Recovery architektúrájának áttekintése [VMware-alapú és fizikai kiszolgálók áttelepítésének](vmware-azure-architecture.md) vagy [Hyper-V virtuális gép áttelepítése](hyper-v-azure-architecture.md) 

Windows Server 2008 vagy Windows Server 2008 R2 rendszert futtató Hyper-V virtuális gépek áttelepítését, kövesse a [a helyszíni gépek áttelepítése az Azure-bA](migrate-tutorial-on-premises-azure.md) oktatóanyag.

Ez az oktatóanyag további részeinek bemutatja, hogyan telepíthet át a helyszíni VMware virtuális gépek és a Windows Server 2008 vagy 2008 R2 rendszert futtató fizikai kiszolgálókat.


## <a name="limitations-and-known-issues"></a>Korlátozások és ismert problémák

- A konfigurációs kiszolgáló, a további folyamatkiszolgálók és a mobilitási szolgáltatás áttelepítése a Windows Server 2008 SP2 használt kell futnia a 9.19.0.0 verzió vagy újabb, az Azure Site Recovery szoftver.

- Alkalmazás-konzisztens helyreállítási pontok és a virtuális gépre kiterjedő konzisztencia funkció nem támogatott a Windows Server 2008 SP2 rendszert futtató kiszolgálók replikálása. A Windows Server 2008 SP2 kiszolgálók át kell egy összeomlás-konzisztens helyreállítási pont. Összeomlás-konzisztens helyreállítási pontok alapértelmezés szerint 5 percenként jönnek létre. Replikációs házirend használata a konfigurált alkalmazáshoz a alkalmazáskonzisztens pillanatkép készítésének gyakorisága, kapcsolja be az alkalmazás-konzisztens helyreállítási pontok hiánya miatt kritikus fontosságú replikációs állapotot okoz. Vakriasztások elkerülése érdekében állítsa az alkalmazáskonzisztens pillanatkép készítésének gyakorisága "Kikapcsolva" a replikációs házirendben.

- Az áttelepítés alatt álló kiszolgálók rendelkeznie kell a .NET Framework 3.5 Service Pack 1 a mobilitási szolgáltatás működéséhez.

- Ha a kiszolgáló dinamikus lemezzel rendelkezik, bizonyos konfigurációk esetében, hogy ezek a lemezek megfigyelheti a kiszolgáló keresztül vannak megjelölve az offline vagy külső lemezként látható. Előfordulhat továbbá, a dinamikus lemezek között tükrözött kötetek tükrözött set állapota "Nem sikerült a redundancia" van megjelölve. A diskmgmt.msc probléma megoldásához manuálisan importálja ezeket a lemezeket, és újra őket.

- Az áttelepítés alatt álló kiszolgálók vmstorfl.sys illesztőprogram kell rendelkeznie. Feladatátvétel sikertelen lehet, ha az illesztőprogram nem szerepel az áttelepítés alatt álló kiszolgáló. 
  > [!TIP]
  >Ellenőrizze, hogy az illesztőprogram megtalálható-e a "C:\Windows\system32\drivers\vmstorfl.sys". Ha az illesztőprogram nem található, hozzon létre egy üres fájlt helyben is megkerülheti a problémát. 
  >
  > Nyissa meg a parancssort (Futtatás > cmd), és futtassa a következő: "másolható nul c:\Windows\system32\drivers\vmstorfl.sys"

- Előfordulhat, hogy nem lehet RDP-hez a Windows Server 2008 SP2-kiszolgálókra a 32 bites operációs rendszer, a feladatátvétel után azonnal vagy tesztelési feladatátvétel az Azure-bA. Indítsa újra a a feladatátviteli virtuális géphez az Azure Portalról, és próbáljon meg újra. Ha még nem lehet csatlakozni, ellenőrizze, ha a kiszolgáló konfigurálva van a távoli asztali kapcsolatok, és győződjön meg arról, hogy nincsenek-e tűzfalszabályok vagy blokkolja a kapcsolatot a hálózati biztonsági csoportok. 
  > [!TIP]
  > Feladatátvételi teszt való migrálás előtt erősen ajánlott kiszolgálók. Győződjön meg arról, hogy legalább egy sikeres feladatátvételi teszt végzett áttelepíteni kívánt minden egyes kiszolgálón. A feladatátvételi teszt részeként a teszt átvevő gépen csatlakozhat, és győződjön meg, hogy elemek az elvárt módon működnek.
  >
  >A teszt feladatátvételi műveletet zavart nem okozó, amely segítséget nyújt a virtuális gépek létrehozását a választott elkülönített hálózatban áttelepítések teszteléséhez. Ellentétben a feladatátvételi művelet, a teszt feladatátvételi művelet során az adatreplikációt progres továbbra is. Számos teszt feladatátvételi teszteket, mielőtt, készen áll a migrálásra tetszés szerinti hajthat végre. 
  >
  >


## <a name="getting-started"></a>Első lépések

A következő feladatokat az Azure előfizetés és a helyszíni VMware-ről/fizikai környezet előkészítése:

1. [Az Azure előkészítése](tutorial-prepare-azure.md)
2. Készítse elő a helyszíni [VMware](vmware-azure-tutorial-prepare-on-premises.md)


## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

1. Jelentkezzen be az [Azure Portal](https://portal.azure.com) > **Recovery Services** szolgáltatásba.
2. Kattintson az **Erőforrás létrehozása** > **Figyelés + felügyelet** > **Backup és Site Recovery** lehetőségre.
3. A **neve**, adja meg a rövid név **W2K8 áttelepítési**. Ha egynél több előfizetéssel rendelkezik, válassza ki ezek közül a megfelelőt.
4. Hozzon létre egy erőforráscsoportot **w2k8migrate**.
5. Válassza ki a kívánt Azure-régiót. A támogatott régiók megtekintéséhez olvassa el az [Azure Site Recovery – Díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/) című cikknek a földrajzi elérhetőséggel foglalkozó részét.
6. Ha gyors hozzáférést szeretne a tárolóhoz az irányítópultról, kattintson a **Rögzítés az irányítópulton**, majd a **Létrehozás** gombra.

   ![Új tároló](media/migrate-tutorial-windows-server-2008/migrate-windows-server-2008-vault.png)

Az új tároló megjelenik az **Irányítópult** **Minden erőforrás** részében, illetve a központi **Recovery Services-tárolók** oldalon.


## <a name="prepare-your-on-premises-environment-for-migration"></a>Készítse elő a helyszíni környezetet az áttelepítésre

- A Windows Server 2008 virtuális gépek VMware,-en futó áttelepítését [beállítása a helyszíni konfigurációs kiszolgáló VMware-en](vmware-azure-tutorial.md#set-up-the-source-environment).
- Ha a konfigurációs kiszolgáló VMware virtuális gépként, a telepítő nem lehet [a konfigurációs kiszolgálót, egy a helyszíni fizikai kiszolgáló vagy virtuális gépén](physical-azure-disaster-recovery.md#set-up-the-source-environment).

## <a name="set-up-the-target-environment"></a>A célkörnyezet beállítása

Válassza ki és ellenőrizze a célerőforrásokat.

1. Kattintson az **Infrastruktúra előkészítése** > **Cél** elemre, majd válassza ki a használni kívánt Azure-előfizetést.
2. Adja meg a Resource Manager-alapú üzemi modell beállítást.
3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.


## <a name="set-up-a-replication-policy"></a>Replikációs szabályzat beállítása

1. Hozzon létre egy új replikációs házirendet, kattintson a **Site Recovery-infrastruktúra** > **replikációs házirendek** > **+ replikációs házirend**.
2. A **replikációs házirend létrehozása**, adja meg a szabályzat nevét.
3. A **helyreállítási Időkorlát küszöbértéke**, adja meg a helyreállítási pont célkitűzés (RPO) vonatkozó korlátozás. Riasztást generál, ha a kapcsolódó replikáció helyreállítási időkorlátja túllépi ezt a korlátot.
4. A **helyreállítási pont megőrzése**, adja meg, hogy mennyi ideig (órákban) adatmegőrzési időtartama az egyes helyreállítási pontok. A replikált virtuális gépek ezen az időtartamon belül bármikor helyreállíthatók. 24 óra megőrzési támogat a premium storage és a standard szintű tárolóra vonatkozó 72 óra replikált gépek esetében.
5. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg **ki**. A szabályzat létrehozásához kattintson az **OK** gombra.

A szabályzat automatikusan társítva lesz a konfigurációs kiszolgálóval.

> [!WARNING]
> Adjon meg **OFF** a az alkalmazáskonzisztens pillanatkép gyakorisága beállítás a replikációs házirend. Csak összeomlás-konzisztens helyreállítási pontjait replikálásakor a Windows Server 2008 rendszerű kiszolgálók támogatottak. Használható az üzembe semmilyen más érték az alkalmazáskonzisztens pillanatkép készítésének gyakorisága eredményez téves riasztások kikapcsolásával a kiszolgáló replikációs állapota kritikus alkalmazáskonzisztens helyreállítási pontok hiánya miatt.

   ![Replikációs házirend létrehozása](media/migrate-tutorial-windows-server-2008/create-policy.png)

## <a name="enable-replication"></a>A replikáció engedélyezése

[Engedélyezze a replikációt](physical-azure-disaster-recovery.md#enable-replication) a Windows Server 2008 SP2 vagy Windows Server 2008 R2 SP1 server kell áttelepíteni.
   
   ![Fizikai kiszolgáló hozzáadása](media/migrate-tutorial-windows-server-2008/Add-physical-server.png)

   ![A replikáció engedélyezése](media/migrate-tutorial-windows-server-2008/Enable-replication.png)

## <a name="run-a-test-migration"></a>Migrálási teszt futtatása

Kiszolgálók replikálása után a kezdeti replikálás befejeződött, és a kiszolgáló állapotának kerül, a feladatátvételi teszt elvégzése **védett**.

A [rest failover](tutorial-dr-drill-azure.md) parancs Azure-ban történő futtatásával győződjön meg arról, hogy minden a vártnak megfelelően működik-e.

   ![Feladatátvételi teszt](media/migrate-tutorial-windows-server-2008/testfailover.png)


## <a name="migrate-to-azure"></a>Áttelepítés az Azure-ba

Futtasson egy feladatátvételt a migrálni kívánt gépen.

1. A **Beállítások** > **Replikált elemek** területen kattintson a gépre > **Feladatátvétel** ikonra.
2. A **Feladatátvétel** területen válassza ki a **Helyreállítási pontot** a feladatok átvételéhez. Válassza a legutóbbi helyreállítási pontot.
3. Válassza a **Gép leállítása a feladatátvétel megkezdése előtt** lehetőséget. A Site Recovery megkísérli a feladatátvétel indítása előtt állítsa le a kiszolgálót. A feladatátvételi akkor is folytatódik, ha a leállítás meghiúsul. A feladatátvételi folyamatot a **Feladatok** lapon követheti nyomon.
4. Ellenőrizze, hogy az Azure-beli virtuális gép a várt módon jelenik-e meg az Azure-ban.
5. A **Replikált elemek** listában kattintson a jobb gombbal a virtuális gépre, majd kattintson a **Migrálás befejezése** parancsra. Ez befejezi a migrálási folyamatot, valamint leállítja a virtuális gép replikálását és a virtuális gép Site Recovery-számlázását.

   ![Az áttelepítés befejezése](media/migrate-tutorial-windows-server-2008/complete-migration.png)


> [!WARNING]
> **Ne szakítsa meg a folyamatban lévő feladatátvételt**: A feladatátvétel indítása előtt a virtuális gép replikációja leáll. Ha megszakítja a folyamatban lévő feladatátvételt, az leáll, a virtuális gép replikációja azonban nem folytatódik.

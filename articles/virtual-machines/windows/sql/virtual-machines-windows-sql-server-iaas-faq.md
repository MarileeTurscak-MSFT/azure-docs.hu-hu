---
title: SQL Server a Windows Azure-beli virtuális gépek – gyakori kérdések |} A Microsoft Docs
description: Ez a cikk ismerteti az Azure virtuális gépeken futó SQL Server gyakran feltett kérdésekre adott válaszok.
services: virtual-machines-windows
documentationcenter: ''
author: v-shysun
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: v-shysun
ms.openlocfilehash: 48df858095cb867954460ec858567e41ed330063
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009265"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-azure-virtual-machines"></a>Windows Azure-beli virtuális gépeken futó SQL Server kapcsolatos gyakori kérdések

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

Ez a cikk ismerteti a futó kapcsolatos leggyakoribb kérdésekre adott válaszokat [Windows Azure virtuális gépeken futó SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> Ez a cikk a Windows virtuális gépeken futó SQL Serverre konkrét problémák összpontosít. Ha az SQL Server Linuxos virtuális gépeken futnak, tekintse meg a [Linux – gyakori kérdések](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Képek

1. **Milyen az SQL Server virtuálisgép-katalógus rendszerképek érhetők el?**

   Azure Windows és Linux rendszerű SQL Server összes kiadásában az összes támogatott fő kiadásához tartozó virtuálisgép-lemezképek tart. További részletekért tekintse meg a teljes listáját [Windows Virtuálisgép-rendszerképek](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) és [Linuxos Virtuálisgép-rendszerkép](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **Frissülnek a meglévő SQL Server virtuálisgép-katalógus rendszerképek?**

   Minden két hónapra, SQL Server-rendszerképeket a virtuálisgép-katalógus frissülnek a legújabb Windows és Linux-frissítések. Windows-rendszerképeket ilyenek például a Windows Update, beleértve a fontos az SQL Server biztonsági frissítések és szervizcsomagok fontosként megjelölt frissítéseket. Linux-lemezképekhez ilyenek például a rendszerek legújabb frissítéseit. SQL Server összegző frissítések a Linux és Windows másképp kezeli. A Linux SQL Server összegző frissítései is megtalálhatók a frissítést. Azonban jelenleg Windows virtuális gépek nem frissíti az SQL Server vagy Windows Server összegző frissítései.

1. **Az SQL Server virtuálisgép-rendszerképek lekérése távolíthatók el a katalógusban?**

   Igen. Az Azure csak egy kép / fő verziójú és kiadású tart fenn. Például amikor egy új SQL Server szervizcsomag megjelenik, az Azure ad hozzá egy új rendszerképet a katalógusban, hogy a service Pack. Az SQL Server-lemezképet az előző service Pack azonnal törlődik az Azure Portalról. Azonban továbbra is elérhető, a következő három hónapban a Powershellből kiépítéshez. Három hónap elteltével az előző képen a service pack már nem érhető el. Ez a házirend eltávolítása is alkalmazni, ha egy SQL Server-verzió nem támogatott válik, amikor az életciklusa végére ér.

1. **Lehetséges egy VHD-lemezképet készíteni egy SQL Server virtuális Gépet?**

   Igen, de van néhány szempontot. Ha a virtuális merevlemez telepít egy új virtuális géphez az Azure-ban, ezt megteheti ge nem az SQL Server-konfigurációs szakasz a portálon. Ezután a Powershellen keresztül az SQL Server-konfigurációs beállításokat kell kezelni. Emellett meg kell fizetni a díj az SQL virtuális gép a lemezkép eredetileg alapján. Ez igaz, akkor is, ha eltávolítja az SQL Server virtuális merevlemez üzembe helyezése előtt. 

1. **Az állítható be a virtuálisgép-katalógus (a + az SQL Server 2012 például Windows 2008 R2) nem látható konfigurációk?**

   Nem. A virtuális gép katalógus-lemezképek, amelyek tartalmazzák az SQL Server esetében jelöljön ki egy megadott rendszerképet.

## <a name="creation"></a>Létrehozás

1. **Hogyan hozhatok létre Azure virtuális gépeken az SQL Server?**

   A legegyszerűbb megoldás, hogy hozzon létre egy virtuális gépet, amely tartalmazza az SQL Server. Az Azure-regisztráció és a egy SQL virtuális gép létrehozása a portálról, olvassa el [az Azure Portalon az SQL Server virtuális gép kiépítése](virtual-machines-windows-portal-sql-server-provision.md). Választhatja a használatalapú másodpercenkénti SQL Server licencelési használó virtuálisgép-lemezkép, vagy olyan lemezképet, amely lehetővé teszi, hogy a saját SQL Server-licencét is használhatja. Emellett lehetősége manuálisan az SQL Server telepítése a virtuális gép mindkettővel szabadon licenccel rendelkező kiadásra (Developer és Express) vagy az a helyi licencek újrafelhasználásával. Ha a saját licenc használata, rendelkeznie kell [az Azure frissítési garancián keresztüli Licenchordozhatóság](https://azure.microsoft.com/pricing/license-mobility/). További információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Hogyan is a helyszíni SQL Server-adatbázis migrálása a felhőbe?**

   Először hozzon létre egy Azure virtuális gép egy SQL Server-példányhoz. Majd telepítse át a helyszíni adatbázisok-példányhoz. Adatok migrálási stratégiák, lásd: [SQL Server-adatbázis áttelepítése az SQL Server Azure virtuális gép](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Licencelés

1. **Hogyan telepíthetem saját licenccel rendelkező példány az SQL Server-beli virtuális gépen?**

   Ehhez két módja van. Egyik kioszthatja a [virtuálisgép-lemezképek, amely támogatja a licencek](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), amely bring-your-saját licenc (használata BYOL) is nevezik. Egy másik lehetőség, hogy egy Windows Server rendszerű virtuális gép az SQL Server telepítési adathordozóról másolja, majd telepítse az SQL Server virtuális gépen. Azonban ha manuálisan telepíti az SQL Server, nincs portál integrációjához és az SQL Server IaaS-ügynök bővítmény nem támogatott, így például automatikus biztonsági mentés és az automatikus javítás nem fog működni ebben a forgatókönyvben. Ebből kifolyólag javasoljuk, hogy a BYOL-katalógus rendszerképek közül. Egy Azure-beli virtuális gépen (BYOL) és a saját SQL Server-adathordozó használatához rendelkeznie kell [az Azure frissítési garancián keresztüli Licenchordozhatóság](https://azure.microsoft.com/pricing/license-mobility/). További információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Módosíthatja a saját SQL Server-licencét használja, ha létrehozták a használatalapú fizetéses katalógus rendszerképek közül egy virtuális Gépet?**

   Nem. Másodpercenkénti használatalapú licencelés saját licenc használata nem válthat. Az egyik új Azure virtuális gép létrehozása a [BYOL-lemezképeknek](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), majd telepítse át az adatbázisokat az új kiszolgálóra a standard [adatáttelepítési eljárásokkal](virtual-machines-windows-migrate-sql.md).

1. **Rendelkeznie kell fizetnie, ha csak van használatban a készenléti/feladatátvételi az licenc SQL Server-beli virtuális gépen?**

   Ha rendelkezik frissítési garanciával, és használják a Licenchordozhatósági programot leírtak szerint [virtuális gép licencelés – gyakori kérdések](http://azure.microsoft.com/pricing/licensing-faq/) akkor nem kell fizetnie egy magas rendelkezésre ÁLLÁSÚ telepítésben passzív másodlagos másodpéldányként részt vevő egy SQL Server licencelése. Ellenkező esetben kell fizetnie, licenceket.


## <a name="administration"></a>Adminisztráció

1. **Telepíthetem egy második példányt az SQL Server ugyanazon a virtuális Gépen? Módosíthatja a telepített szolgáltatások az alapértelmezett példány?**

   Igen. Az SQL Server telepítési adathordozóján található egy mappában található a **C** meghajtó. Futtatás **Setup.exe** arról a helyről az új SQL Server-példányok hozzáadása vagy módosítása másik telepített funkciók az SQL Server a gépen. Vegye figyelembe, hogy egyes funkciók, például az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció, csak az alapértelmezett példány működése.

1. **Eltávolíthatja az SQL Server alapértelmezett példányát?**

   Igen, de van néhány szempontot. Amint azt az előző választ, Funkciók, amelyek működéséhez a [SQL Server IaaS-ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md) csak működnek az alapértelmezett példányon található. Ha az alapértelmezett példányt távolítja el, a bővítmény továbbra is fennáll, akkor a keresett és Eseménynapló hibák léphetnek fel. Ezeket a hibákat a következő két forrásból származnak: **Microsoft SQL Server hitelesítőadat-kezelés** és **Microsoft SQL Server IaaS-ügynök**. A hibák egyike az alábbihoz hasonló lehet:

      A hálózattal kapcsolatos vagy példányspecifikus hiba történt az SQL Server-kapcsolat létrehozásakor. A kiszolgáló nem található vagy nem érhető el.

   Ha úgy dönt, az alapértelmezett példány eltávolítása, is eltávolítja a [SQL Server IaaS-ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md) is.

1. **Eltávolítható az SQL Server teljesen az SQL virtuális gép?**

   Igen, de továbbra is az SQL virtuális gép kell fizetnie, leírtak szerint [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md). Ha már nincs szüksége az SQL Server, az új virtuális gépek üzembe helyezése, és az adatai és alkalmazásai számára az új virtuális gép áttelepítése. Az SQL Server virtuális gép távolíthatja el.
   
## <a name="updating-and-patching"></a>Frissítések és javítások

1. **Hogyan frissíthetem az SQL Server egy Azure-beli virtuális gépen új verzióra vagy kiadásra?**

   Jelenleg nincs Azure-beli virtuális gépen futó SQL Server helyszíni frissítését. Új Azure virtuális gép létrehozása a kívánt SQL Server verziójához/kiadásához, és ezután az adatbázisokat az új kiszolgálóra a standard [adatáttelepítési eljárásokkal](virtual-machines-windows-migrate-sql.md).

1. **Frissítések és szervizcsomagok alkalmazási módja egy SQL Server virtuális gépen?**

   Virtuális gépek számára is kézben tarthatja a gazdagép frissítéséből, beleértve a mikor és hogyan alkalmazni a frissítéseket. Az operációs rendszer esetén manuálisan alkalmazhatja a windows-frissítéseket, vagy engedélyezheti a nevű ütemezési szolgáltatást [automatikus javítás](virtual-machines-windows-sql-automated-patching.md). Az Automatikus javítás minden fontosként megjelölt frissítést telepít, így az e kategóriába eső SQL Server-frissítéseket is. Az SQL Server egyéb nem kötelező frissítéseit manuálisan kell telepíteni.

## <a name="general"></a>Általános kérdések

1. **Azure virtuális gépeken támogatottak az SQL Server feladatátvevő fürt példány (FCI)?**

   Igen. Is [Windows feladatátvevő fürt létrehozása a Windows Server 2016-on](virtual-machines-windows-portal-sql-create-failover-cluster.md) , és a fürttároló közvetlen tárolóhelyek (S2D) használja. Másik lehetőségként használhatja külső fürtszolgáltatás vagy a tárolási megoldások leírtak szerint [az SQL Server Azure virtuális gépek magas rendelkezésre állás és vészhelyreállítás helyreállítási](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > Jelenleg a [SQL Server IaaS-ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md) nem támogatott az SQL Server FCI az Azure-ban. Azt javasoljuk, hogy az FCI-ben részt vevő virtuális gépek távolítsa el a bővítményt. Ez a bővítmény szolgáltatásai, például az automatikus biztonsági mentés és a javítás és a egy portál funkciók az SQL támogatja. Ezek a funkciók nem működnek az SQL virtuális gépek, az ügynök eltávolítása után.

1. **Mi a különbség az SQL virtuális gépek és az SQL Database szolgáltatás?**

   Elméleti szinten fut az SQL Server-beli virtuális gépen eltér, hogy nem fut az SQL Server egy távoli adatközpontban. Ezzel szemben a [SQL Database](../../../sql-database/sql-database-technical-overview.md) kínál az adatbázis--szolgáltatásként. Az SQL Database nem rendelkezik hozzáféréssel a az adatbázisokat üzemeltető gépeken. Teljes összehasonlításáért lásd: [felhőalapú SQL Server option: Azure SQL (PaaS) adatbázis vagy SQL Server Azure virtuális gépeken (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hogyan kell telepíteni az SQL Data eszközök a saját Azure virtuális gépen**

    Töltse le és telepítse az SQL Data eszközöket [Microsoft SQL Server Data Tools – Business Intelligence Visual Studio 2013-hoz készült](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>További források

**Windows virtuális gépek**:

* [Egy Windows virtuális gép az SQL Server használatának áttekintése](virtual-machines-windows-sql-server-iaas-overview.md).
* [Egy SQL Server Windows virtuális gép kiépítése](virtual-machines-windows-portal-sql-server-provision.md)
* [Egy Azure virtuális Gépen futó SQL Server-adatbázis áttelepítése](virtual-machines-windows-migrate-sql.md)
* [Magas rendelkezésre állás és vészhelyreállítás Azure-beli virtuális gépeken az SQL Serverhez](virtual-machines-windows-sql-high-availability-dr.md)
* [Ajánlott eljárások az SQL Server teljesítményének Azure Virtual Machines szolgáltatásbeli növeléséhez](virtual-machines-windows-sql-performance.md)
* [Azure-beli virtuális gépeken futó SQL Server – alkalmazásminták és fejlesztési stratégiák](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux rendszerű virtuális gépek**:

* [Linux rendszerű virtuális gép az SQL Server használatának áttekintése](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Az SQL Server Linux virtuális gép kiépítése](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Gyakori kérdések (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [A Linux rendszeren futó SQL Server dokumentációja](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

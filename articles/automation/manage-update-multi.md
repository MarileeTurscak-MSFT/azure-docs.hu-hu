---
title: Több Azure-beli virtuális gép frissítéseinek kezelése
description: Ez a cikk ismerteti, hogyan lehet Azure-beli virtuális gépek frissítéseinek kezeléséhez.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 23f86581b5ecc5257ccb246c7199eef4246efb08
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498232"
---
# <a name="manage-updates-for-multiple-machines"></a>Frissítések kezelése több gép esetén

Az Update Management megoldás segítségével kezelheti a Windows és Linux rendszerű virtuális gépek frissítéseit és javításait. Az [Azure Automation](automation-offering-get-started.md)-fiókból a következőket végezheti el:

- Virtuális gépek előkészítése
- Felmérheti az elérhető frissítések állapotát
- Kötelező frissítések telepítésének ütemezése
- Tekintse át a telepítési eredményeket, hogy ellenőrizze, hogy frissítéseket sikeresen alkalmazta-e az összes virtuális gép, amelynek az Update Management engedélyezve van

## <a name="prerequisites"></a>Előfeltételek

A frissítéskezelés használatához a következők szükségesek:

- Azure Automation futtató fiók. Megtudhatja, hogyan hozhat létre egyet, tekintse meg a [Ismerkedés az Azure Automation](automation-offering-get-started.md).
- Egy támogatott operációs rendszert futtató virtuális gépre vagy számítógépre.

## <a name="supported-operating-systems"></a>Támogatott operációs rendszerek

Az Update Management a következő operációs rendszereken támogatott:

|Operációs rendszer  |Megjegyzések  |
|---------|---------|
|A Windows Server 2008, Windows Server 2008 R2 RTM    | Támogatja az értékelések csak frissíteni.         |
|A Windows Server 2008 R2 SP1 és újabb verziók     |Windows PowerShell 4.0-s vagy újabb szükség. ([Töltse le a WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))</br> Windows PowerShell 5.1 megbízhatóság növelése érdekében ajánlott. ([Töltse le a WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) és 7 (x64)      | A Linux-ügynököknek hozzáféréssel kell rendelkezniük valamely frissítési tárházhoz.        |
|Red Hat Enterprise 6 (x86/x64) és 7 (x64)     | A Linux-ügynököknek hozzáféréssel kell rendelkezniük valamely frissítési tárházhoz.        |
|SUSE Linux Enterprise Server 11 (x86/x64) és 12 (x64)     | A Linux-ügynököknek hozzáféréssel kell rendelkezniük valamely frissítési tárházhoz.        |
|Ubuntu 12.04 LTS, 14.04 LTS és 16.04 LTS (x86/x64)      |A Linux-ügynököknek hozzáféréssel kell rendelkezniük valamely frissítési tárházhoz.         |

> [!NOTE]
> Ahhoz, hogy Ubuntu rendszeren elkerülje a karbantartási időszakon kívüli frissítéstelepítést, konfigurálja újra az Unattended-Upgrade csomagot az automatikus frissítések letiltásához. További információt az [Ubuntu kiszolgáló kézikönyvének Automatikus frissítések témakörében](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) talál.

A Linux-ügynököknek hozzáféréssel kell rendelkezniük valamely frissítési tárházhoz.

Ez a megoldás nem támogatja egy Log Analytics-ügynök Linux rendszerekre, amely több Azure Log Analytics-munkaterületnek való jelentésre van konfigurálva.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Az Update Management engedélyezése az Azure-beli virtuális gépek

Az Azure Portalon nyissa meg az Automation-fiókját, és válassza **frissítéskezelés**.

Válassza ki **adja hozzá az Azure virtuális gépek**.

![Azure-beli Virtuálisgép-lap hozzáadása](./media/manage-update-multi/update-onboard-vm.png)

Válassza ki az előkészíteni kívánt virtuális gépet. 

A **Update Management engedélyezése**válassza **engedélyezése** bevezetni a virtuális gép.

![Frissítéskezelés engedélyezése párbeszédpanel](./media/manage-update-multi/update-enable.png)

Előkészítés végeztével az Update Management engedélyezve van a virtuális gép.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Nem Azure-beli virtuális gépeken és számítógépeken a frissítéskezelés engedélyezése

Az Update Management engedélyezése az Azure Windows virtuális gépek és számítógépek kezelésével kapcsolatos információkért lásd: [a Log Analytics szolgáltatás az Azure-ban való csatlakozáshoz Windows számítógépek](../log-analytics/log-analytics-windows-agent.md).

Az Update Management engedélyezése nem Azure-beli Linuxos virtuális gépek és számítógépek kezelésével kapcsolatos információkért lásd: [Linux rendszerű számítógépek csatlakoztatása a Log Analytics](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Az Automation-fiókhoz csatlakoztatott számítógépek megtekintése

Miután engedélyezte a Frissítéskezelést a gépek, adatai kiválasztásával megtekintheti **számítógépek**. Információkat tekintheti meg *gépnév*, *megfelelőségi állapot*, *környezet*, *operációs rendszer típusa*, *kritikus és biztonsági frissítések*, *más telepített frissítések*, és *frissítési ügynök készültsége* számítógépek.

  ![Számítógépek megtekintése lap](./media/manage-update-multi/update-computers-tab.png)

Számítógépek nemrég lett engedélyezve az Update Management, előfordulhat, hogy nem felmérését még. Azon számítógépek esetén a megfelelőségi állapot állapota **nincs értékelve**. Ez egy lista a megfelelőségi állapot a lehetséges értékek:

- **Megfelelő**: számítógépek, amelyről nem hiányzik kritikus vagy biztonsági frissítések.

- **Nem megfelelő**: számítógépek, amelyek hiányzik legalább egy kritikus vagy biztonsági frissítés.

- **Nincs értékelve**: A frissítés kiértékelésének adatai nem érkeztek időkereten belül a számítógépről. A Linux rendszerű számítógépek esetében a várt időtartamon az elmúlt 3 órában van. A Windows-számítógépek esetében az elmúlt 12 órában időkereten van.

Az ügynök állapotának megtekintéséhez válassza ki a hivatkozásra a **frissítési ÜGYNÖK KÉSZÜLTSÉGE** oszlop. Ezzel a beállítással megnyílik a **hibrid feldolgozó** ablaktáblán, és a hibrid feldolgozó állapotát jeleníti meg. Az alábbi képen egy példa olyan ügynök, amely nem csatlakozik az Update Management hosszabb idő:

![Számítógépek megtekintése lap](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Frissítésfelmérés megtekintése

Az Update Management engedélyezése után megnyílik az **Update Management** ablaktábla. A **Hiányzó frissítések** lapon a hiányzó frissítések listája látható.

## <a name="collect-data"></a>Adatok gyűjtése

A virtuális gépeken és számítógépeken telepített ügynökök frissítésével kapcsolatos adatokat gyűjteni. Az ügynökök elküldi az adatokat az Azure az Update Management.

### <a name="supported-agents"></a>Támogatott ügynökök

A következő táblázat ismerteti a megoldás által támogatott csatlakoztatott forrásokat:

| Csatlakoztatott forrás | Támogatott | Leírás |
| --- | --- | --- |
| Windows-ügynökök |Igen |A frissítéskezelés begyűjti a Windows ügynököktől a rendszerfrissítésekről szóló információkat, és ezután kezdeményezi a szükséges frissítések telepítését. |
| Linux-ügynökök |Igen |Az Update Management Linux-ügynököktől a rendszerfrissítésekről szóló információkat gyűjti, és ezután kezdeményezi a támogatott disztribúciókon szükséges frissítések telepítését. |
| Az Operations Manager felügyeleti csoportja |Igen |Az Update Management egy csatlakoztatott felügyeleti csoportban lévő ügynököktől gyűjti a rendszerfrissítésekről szóló információkat. |
| Azure Storage-fiók |Nem |Az Azure Storage nem tartalmazza a rendszerfrissítésekről szóló információkat. |

### <a name="collection-frequency"></a>A gyűjtés gyakorisága

Felügyelt Windows-számítógépek esetében naponta kétszer fut vizsgálat. 15 percenként, a Windows API hívása az utolsó frissítésének időpontját, hogy módosultak-e az állapot meghatározására a lekérdezéshez. Ha az állapot megváltozik, megfelelőségi vizsgálatot kezdeményez. Linux-számítógépek 3 óránként fut vizsgálat.

30 perc és a felügyelt számítógépekből származó frissített adatok megjelennek az irányítópulton 6 óráig is eltarthat.

## <a name="schedule-an-update-deployment"></a>Frissítéstelepítés ütemezése

A frissítések telepítéséhez ütemezzen egy olyan telepítést, amely megfelel a kiadási ütemtervnek és a szolgáltatási ablak. Kiválaszthatja, hogy a telepítés milyen típusú frissítéseket tartalmazzon. Például hozzáadhatja a kritikus vagy a biztonsági frissítéseket, és kizárhatja a kumulatív frissítéseket.

Egy új frissítéstelepítést egy vagy több virtuális gépet, a ütemezése **frissítéskezelés**válassza **frissítések központi telepítésének ütemezése**.

Az a **új frissítéstelepítés** panelen adja meg a következő információkat:

- **Név**: Adjon meg egy egyedi nevet a frissítéstelepítés azonosításához.
- **Operációs rendszer**: válasszon **Windows** vagy **Linux**.
- **A csoportok frissítése (előzetes verzió)**: előfizetés, erőforráscsoport, helyek és címkék felvenni az üzembe helyezés az Azure-beli virtuális dinamikus csoportot hozhat létre kombinációja alapján egy lekérdezést határoz meg. További tudnivalókért tekintse meg, [dinamikus csoportok](automation-update-management.md#using-dynamic-groups)
- **Frissítendő gépek**: válassza ki, mentett keresést, importált csoporthoz, vagy válassza ki a gépeket, válassza ki a frissíteni kívánt gépeket. Ha a **Gépek** lehetőséget választotta, a gép állapota az **ÜGYNÖK KÉSZÜLTSÉGÉNEK FRISSÍTÉSE** oszlopban látható. A frissítéstelepítés ütemezése előtt látható a gép állapotát. A számítógépcsoportok Log Analyticsben lévő létrehozásának különböző módszereivel kapcsolatos további információkért tekintse meg a [Log Analytics számítógépcsoportjait](../log-analytics/log-analytics-computer-groups.md) ismertető részt

  ![Új frissítés üzembe helyezési panel](./media/manage-update-multi/update-select-computers.png)

- **Frissítési besorolás**: válassza ki a szoftver szerepeljenek a központi telepítési típusokat. A választható besorolási típusok leírását lásd: [frissítési besorolások](automation-update-management.md#update-classifications). A választható besorolási típusok a következők:
  - Kritikus frissítések
  - Biztonsági frissítések
  - Kumulatív frissítések
  - Funkciócsomagok
  - Szervizcsomagok
  - Definíciófrissítések
  - Eszközök
  - Frissítések

- **Belefoglalási és kizárási frissítései** -ekkor megnyílik a **beszámítása vagy kihagyása** lap. A felügyelt vagy kizárt frissítések olyan külön lapon. A belefoglalási kezelésének további információkért lásd: [belefoglalási viselkedés](automation-update-management.md#inclusion-behavior)

- **Ütemezési beállítások**: Elfogadhatja az alapértelmezett időpontot, amely a 30 perccel az aktuális idő utáni időpont, Megadhat egy másik időpontot is.

   Azt is megadhatja, hogy a telepítés egyszer történjen meg, vagy ismétlődjön. Egy ismétlődő ütemezés beállításához a **ismétlődési**válassza **ismétlődő**.

   ![Ütemezési beállítások párbeszédpanel](./media/manage-update-multi/update-set-schedule.png)

- **Előre parancsfájlok + utáni parancsfájlok**: válassza ki a parancsfájlok futtatása előtt és után a központi telepítés. További tudnivalókért lásd: [kezelése előtti és utáni parancsfájlokat](pre-post-scripts.md).
- **Karbantartási időszak (perc)**: Adja meg azt az időszakot, hogy azt szeretné, hogy a frissítés telepítése megtörténjen. Ez a beállítás biztosítja, hogy a módosítások a megadott szolgáltatási időkereten belül menjenek végbe.

- **Indítsa újra a vezérlő** – Ez a beállítás azt határozza meg, hogyan kezelje a központi telepítési újraindítások.

   |Beállítás|Leírás|
   |---|---|
   |Szükség esetén újraindítás| **(Alapértelmezett)**  Szükség esetén újraindítás kezdeményezték, ha lehetővé teszi, hogy a karbantartási időszak.|
   |Mindig induljon újra|Függetlenül attól, hogy az egyik szükséges újraindítást kezdeményez. |
   |Soha ne induljon újra|Függetlenül attól, hogy ha a számítógép újraindítása szükséges, újraindítások le lesznek tiltva.|
   |Csak újraindítás – frissítések nem lesznek telepítve|Ezzel a beállítással figyelmen kívül hagyja a frissítés telepítése, és csak az újraindítást kezdeményez.|

Ha elkészült, az ütemezés konfigurálását, válassza ki a **létrehozás** gombra kattintva térjen vissza az állapot-irányítópultra. A **ütemezett** táblázat mutatja a központi telepítési ütemezés létrehozott.

## <a name="view-results-of-an-update-deployment"></a>Frissítéstelepítés eredményeinek megtekintése

Miután az ütemezett telepítés elindult, a **Frissítéskezelés** panel **Frissítéstelepítések** lapján láthatóvá válik a telepítés állapota.

Ha a telepítés fut, az állapota **Folyamatban**. A telepítés sikeres befejezését követően a állapota **sikeres**.

Ha a telepítésben lévő frissítések közül egy vagy több meghiúsul, a telepítés állapota **Részben sikertelen**.

![A frissítéstelepítés állapota](./media/manage-update-multi/update-view-results.png)

Adott frissítéstelepítés irányítópultjának megtekintéséhez válassza ki a befejezett telepítést.

A **frissítés eredményei** ablaktábla megjeleníti azokat a frissítéseket és a telepítési eredményeket, a virtuális gép teljes száma. A jobb oldali tábla minden egyes frissítés és a telepítés eredményét részletes áttekintést nyújt. A telepítési eredmények a következő értékek lehetnek:

- **Nem lett megkísérelve**: A frissítés nem lett telepítve, mert nincs elég ideje volt elérhető a megadott karbantartási időszak alapján.
- **Sikeres**: A frissítés sikeres volt.
- **Sikertelen**: A frissítés sikertelen volt.

A telepítés által létrehozott összes naplóbejegyzés megtekintéséhez válassza a **Minden napló** elemet.

Amely a cél virtuális gépen a frissítések telepítését kezeli a runbook feladatstreamjének megtekintéséhez válassza ki a kimeneti csempét.

A telepítés közben felmerülő hibák részletes információinak megtekintéséhez válassza a **Hibák** elemet.

## <a name="next-steps"></a>További lépések

- További információk a Frissítéskezelésről, beleértve a naplókat, kimeneti és hibát, [frissítéskezelési megoldás az Azure-ban](../operations-management-suite/oms-solution-update-management.md).

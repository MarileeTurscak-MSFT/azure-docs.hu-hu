---
title: Az Azure Security Centerben a biztonsági konfigurációk javítása |} A Microsoft Docs
description: Ez a dokumentum bemutatja, hogyan valósíthat meg az Azure Security Center javaslatait, az "Szervizelése biztonsági konfigurációkat."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2018
ms.author: terrylan
ms.openlocfilehash: 74bfc3435ad6d247dabd3a7cbf2910ede5f8c8ca
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296242"
---
# <a name="remediate-security-configurations-in-azure-security-center"></a>Az Azure Security Centerben a biztonsági konfigurációk javítása
Az Azure Security Center naponta elemzi az operációs rendszer (OS) a virtuális gépek (VM) és a egy konfigurációjához, amely a teheti a virtuális gépek és számítógépek támadásokkal szemben. Biztonsági rések feloldása, amikor az operációs rendszer konfigurációja nem egyezik a javasolt biztonsági konfigurációs szabályok, és ezek a biztonsági rések megszüntetését elősegítő konfigurációmódosításokat javasolja a Security Center javasolja.

Által figyelt konfigurációkkal kapcsolatos további információkért lásd: a [ajánlott konfigurációs szabályok](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Biztonsági konfiguráció értékelések testreszabása kapcsolatban lásd: [testreszabása operációs rendszer biztonsági konfigurációival az Azure Security Centerben (előzetes verzió)](security-center-customize-os-security-config.md).

## <a name="implement-the-recommendation"></a>A javaslatok megvalósítása
"A biztonsági konfigurációk javítása" jeleníti meg a Security Center egy javaslatot. Az ajánlás alatt jelenik meg **javaslatok** > **számítás és alkalmazások**.

Ebben a példában magában foglalja a "Biztonsági konfigurációk javítása" Javaslat alatt **számítás és alkalmazások**.
1. A Security Centerben, a bal oldali panelén válassza **számítás és alkalmazások**.  
  A **számítás és alkalmazások** ablak nyílik meg.

   ![Biztonsági konfigurációk javítása][1]

2. Válassza ki **biztonsági konfigurációk javítása**.  
  A **biztonsági konfigurációk** ablak nyílik meg.

   ![A "Biztonsági konfigurációk" ablak][2]

  Az irányítópult megjeleníti felső részén:

  - **Nem teljesített szabályok súlyosság szerint**: szabályokat, hogy az operációs rendszer konfigurációja sikertelen volt a virtuális gépek és számítógépek, súlyosság szerint bontottuk ki teljes száma.
  - **Nem teljesített szabályok típus szerinti**: szabályokat, hogy az operációs rendszer konfigurációja sikertelen volt a virtuális gépek és számítógépek, típus szerint bontottuk ki teljes száma.
  - **Nem sikerült a Windows szabályok**: szabályok teljes száma a Windows operációs rendszer beállításai alapján nem sikerült.
  - **Sikertelen Linuxos szabályok**: szabályok száma szerint a Linux operációsrendszer-konfigurációk nem sikerült.

  Az irányítópult alsó részén felsorolja az összes sikertelen szabályok a virtuális gépek és számítógépek és a hiányzó frissítés súlyosságát. A listán a következő elemeket tartalmazza:

  - **CCEID**: a szabály a CCE egyedi azonosítója. A Security Center egyedi azonosítók hozzárendelése a konfigurációs szabályok Common Configuration Enumeration (CCE) használja.
  - **Név**: a nem teljesített szabály nevét.
  - **Szabály típusa**: A *beállításkulcs*, *biztonsági házirend*, *naplózási házirend*, vagy *IIS* szabály típusa.
  - **nem. virtuális gépek és számítógépek**: virtuális gépek és a sikertelen szabályt alkalmazó számítógépek teljes száma.
  - **Súlyosság szabály**: A CCE érték *kritikus*, *fontos*, vagy *figyelmeztetés*.
  - **Állapot**: a javaslat aktuális állapota:

    - **Open** (Nyitott): a javaslattal egyelőre még nem foglalkoztak.
    - **Folyamatban lévő**: A javaslat jelenleg alkalmaznak az erőforrásokhoz, és ilyenkor Ön nem szükséges.
    - **Megoldott**: A javaslat alkalmazásának. Ha a probléma megoldódott, a bejegyzés halványan jelenik meg.

3. Nem teljesített szabály részleteinek megtekintéséhez válassza ki a listából.

   ![Sikertelen konfigurációs szabály részletes nézete][3]

   A részletes nézetben jeleníti meg a következő információkat:

   - **Név**: a szabály nevét.
   - **CCIED**: a szabály a CCE egyedi azonosítója.
   - **Operációsrendszer-verzió**: a virtuális gép vagy a számítógép az operációs rendszer verzióját.
   - **Súlyosság szabály**: A CCE érték *kritikus*, *fontos*, vagy *figyelmeztetés*.
   - **Teljes leírás**: a szabály leírását.
   - **Biztonsági rés**: biztonsági rés vagy a kockázat, ha a szabályt a rendszer nem alkalmazza a magyarázatot.
   - **Lehetséges hatás**: az üzletmenetre gyakorolt hatás, amikor a szabály vonatkozik.
   - **Ellenintézkedés**: hibaelhárítási lépéseket.
   - **Várt érték**: az érték, ha a Security Center elemzi a virtuális gép operációs rendszer konfigurációja, szemben a szabály vár.
   - **Tényleges érték**: A visszaadott érték, szemben a szabály a virtuális gép operációsrendszer-konfiguráció elemzése után.
   - **A művelet szabály**: A szabály műveletet, amelyet a Security Center ellen a szabály a virtuális gép operációsrendszer-konfiguráció elemzése során.

4. A részletes nézet ablak tetején válassza **keresési**.  
  Keresés megnyitása a munkaterületeket, amelyeken virtuális gépek és a kiválasztott biztonsági konfigurációkat eltérő rendelkező számítógépek listáját. A munkaterület kiválasztása látható csak ha a kiválasztott szabály több virtuális gép különböző munkaterületekhez kapcsolódó vonatkozik.

   ![Felsorolt munkaterületek][4]

5. Jelöljön ki egy munkaterületet.  
  A Log Analytics keresési lekérdezés megnyitja a munkaterületet a biztonsági konfigurációk eltérés a szűrt.

   ![Az operációs rendszer biztonsági rése munkaterület][5]

6. Válasszon ki egy számítógépet a listában.  
  Csak az adott számítógépen a szűrt adatokat egy új keresési eredmény nyílik meg.

   ![A kiválasztott számítógép kapcsolatos részletes információk][6]

## <a name="next-steps"></a>További lépések
Ez a cikk bemutatta, hogyan valósíthat meg a Security Center javaslatait "Szervizelése biztonsági konfigurációkat." Biztonsági konfiguráció értékelések testreszabása kapcsolatban lásd: [testreszabása operációs rendszer biztonsági konfigurációival az Azure Security Centerben (előzetes verzió)](security-center-customize-os-security-config.md).

Által figyelt konfigurációkkal áttekintéséhez lásd: [ajánlott konfigurációs szabályok](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). A Security Center egyedi azonosítók hozzárendelése a konfigurációs szabályok Common Configuration Enumeration (CCE) használja. További információkért látogasson el a [CCE](https://nvd.nist.gov/cce/index.cfm) hely.

A Security Centerrel kapcsolatos további tudnivalókért lásd a következőket:

* Támogatott Windows és Linux rendszerű virtuális gépek listáját lásd: [az Azure Security Center által támogatott platformok](security-center-os-coverage.md).
* Az Azure-előfizetések és -erőforráscsoportok vonatkozó biztonsági szabályzatok konfigurálásával kapcsolatban lásd: [biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md).
* Megtudhatja, hogyan javaslatok az Azure-erőforrások védelméhez, tekintse meg [biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md).
* Az Azure-erőforrások állapotának figyelése kapcsolatban lásd: [biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md).
* Ismerje meg, hogyan kezelése és válaszadás a biztonsági riasztásokra, lásd: [kezelése és válaszadás a biztonsági riasztásokra az Azure Security Center](security-center-managing-and-responding-alerts.md).
* A partnermegoldások állapotának figyelése kapcsolatban lásd: [partneri megoldások monitorozása az Azure Security Center](security-center-partner-solutions.md).
* A szolgáltatás használatával kapcsolatban gyakran feltett kérdésekre adott válaszokat, lásd: [Azure Security Center – gyakori kérdések](security-center-faq.md).
* Azure biztonsági és megfelelőségi információ blogbejegyzések, lásd: [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/).

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png

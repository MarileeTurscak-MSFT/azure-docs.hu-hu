---
title: Az Azure Automation forgatókönyveinek típusai
description: 'Ismerteti a runbookok, amelyek is használhatja az Azure Automation és szempontokat, akkor figyelembe kell venniük annak meghatározása, amelynek használatához írja be a különböző típusú. '
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ad24f53c7ca58756aa4028c8af2e4a83cfcfe76a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984322"
---
# <a name="azure-automation-runbook-types"></a>Az Azure Automation forgatókönyveinek típusai

Az Azure Automation az alábbi táblázatban számos különböző típusú, amely röviden ismerteti a runbookok támogatja.  Az alábbi szakaszok további információt arról, hogy az egyes mikor szempontok többek között.

| Típus | Leírás |
|:--- |:--- |
| [Grafikus](#graphical-runbooks) |A Windows PowerShell és a létrehozott és szerkesztett teljes egészében az Azure Portalon grafikus szerkesztő alapján. |
| [Grafikus PowerShell-munkafolyamat](#graphical-runbooks) |Windows PowerShell-munkafolyamaton alapuló és a létrehozott és szerkesztésük teljes egészében az Azure Portalon a grafikus szerkesztőben. |
| [PowerShell](#powershell-runbooks) |Szöveges forgatókönyv Windows PowerShell-szkript alapján. |
| [PowerShell-munkafolyamat](#powershell-workflow-runbooks) |Windows PowerShell-munkafolyamaton alapuló szöveges forgatókönyv. |
| [Python](#python-runbooks) |Python-alapú szöveges forgatókönyv. |

## <a name="graphical-runbooks"></a>Grafikus runbookokban

[Grafikus](automation-runbook-types.md#graphical-runbooks) és a grafikus PowerShell-munkafolyamati runbookok létrehozása és az Azure Portalon a grafikus szerkesztő szerkeszthetők.  Egy fájlba exportálhatja, és ezután importálja őket egy másik automation-fiók, de nem hozható létre vagy szerkessze őket egy másik eszközzel.  Grafikus runbookok létrehozása a PowerShell-kódot, de közvetlenül nem tekinthetők meg és módosíthatja a kódot. Grafikus runbookok nem lehet konvertálni az egyik a [szöveges formátumokból](automation-runbook-types.md), sem grafikus formátum konvertálható szöveges runbookok. Grafikus runbookok konvertálható grafikus PowerShell-munkafolyamati runbookok importálása és fordítva.

### <a name="advantages"></a>Előnyök

* Vizuális szerzői műveletekhez részben modell insert-kapcsolat konfigurálása  
* Hogyan áramlanak keresztül az adatok a folyamat összpontosíthat  
* Vizuálisan képviselik a felügyeleti folyamatok  
* Gyermek runbookok magas szintű munkafolyamatokat hozhat létre közé tartozik egy más runbookok  
* Arra ösztönzi a moduláris programozás  

### <a name="limitations"></a>Korlátozások

* Az Azure Portalon kívül a runbook nem szerkeszthető.
* PowerShell-kód végrehajtására összetett logikát tartalmazó kóddal végzett tevékenység lehet szükség.
* Nem lehet megtekintése, vagy közvetlenül szerkesztheti a grafikus munkafolyamat által létrehozott PowerShell-kódot. Vegye figyelembe, hogy a kód tevékenységeket hoz létre a kódot is megtekintheti.

## <a name="powershell-runbooks"></a>PowerShell-forgatókönyvek

PowerShell-forgatókönyvek Windows Powershellen alapulnak.  Közvetlenül szerkesztheti a kódot a runbook a szövegszerkesztő használatával az Azure Portalon.  Minden olyan kapcsolat nélküli szövegszerkesztőben is használhatja, és [importálja a forgatókönyvet](automation-creating-importing-runbook.md) az Azure Automationbe.

### <a name="advantages"></a>Előnyök

* PowerShell-kóddal további vesződni PowerShell-munkafolyamat minden összetett logikát alkalmazzák.
* Runbook, mint a PowerShell-munkafolyamati runbookok gyorsabban indul, mivel nincs szüksége futtatása előtt kell összeállítani.

### <a name="limitations"></a>Korlátozások

* PowerShell-parancsprogramok ismernie kell.
* Nem használható [párhuzamos feldolgozási](automation-powershell-workflow.md#parallel-processing) párhuzamosan több művelet végrehajtásához.
* Nem használható [ellenőrzőpontok](automation-powershell-workflow.md#checkpoints) hiba esetén a runbook folytatása.
* PowerShell-munkafolyamati runbookok és a grafikus runbookok csak szerepelhetnek gyermek runbookként a Start-AzureAutomationRunbook parancsmaggal, amely létrehoz egy új feladatot.

### <a name="known-issues"></a>Ismert problémák

Az alábbiakban a PowerShell-runbookok jelenlegi ismert problémái.

* PowerShell-forgatókönyvek nem tudja lekérni egy nem titkosított [változóeszköz](automation-variables.md) null értékű.
* Nem sikerült beolvasni a PowerShell-forgatókönyvek a [változóeszköz](automation-variables.md) a *~* a nevében.
* Get-Process egy hurokba, és a egy PowerShell runbook körülbelül 80 közelítő összeomolhat.
* PowerShell-runbook sikertelen lehet, ha egyszerre egy nagyon nagy mennyiségű adatot írni a kimeneti stream megkísérli.   Általában is használhatja a probléma megoldásához szerint kiírta volna csak a szükséges információkat, amikor nagy objektumok használata által.  Helyett például szerint kiírta volna valami hasonló *Get-Process*, akkor is csak a kötelező mezőkbe a kimeneti *Get-Process |} Válassza ki a Folyamatnév, CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell-munkafolyamati runbookok

PowerShell-munkafolyamati runbookok alapuló szöveges runbookok [Windows PowerShell-munkafolyamat](automation-powershell-workflow.md).  Közvetlenül szerkesztheti a kódot a runbook a szövegszerkesztő használatával az Azure Portalon.  Minden olyan kapcsolat nélküli szövegszerkesztőben is használhatja, és [importálja a forgatókönyvet](automation-creating-importing-runbook.md) az Azure Automationbe.

### <a name="advantages"></a>Előnyök

* A PowerShell-munkafolyamati kód minden összetett logikát alkalmazzák.
* Használat [ellenőrzőpontok](automation-powershell-workflow.md#checkpoints) hiba esetén a runbook folytatása.
* Használat [párhuzamos feldolgozási](automation-powershell-workflow.md#parallel-processing) párhuzamosan több művelet végrehajtásához.
* Lehetnek más grafikus runbookok és a PowerShell-munkafolyamati runbookok gyermek runbookként magas szintű munkafolyamatok létrehozását.

### <a name="limitations"></a>Korlátozások

* Szerző PowerShell-munkafolyamat ismernie kell.
* A Runbook például a PowerShell-munkafolyamat további összetettsége kell foglalkozniuk [objektumok deszerializálni](automation-powershell-workflow.md#code-changes).
* A Runbook elindításához, mint a PowerShell-forgatókönyvek, mivel fordítható futtatása előtt kell hosszabb időt vesz igénybe.
* PowerShell-forgatókönyvek csak lehet része gyermek runbookok a Start-AzureAutomationRunbook parancsmaggal, amely létrehoz egy új feladatot.

## <a name="python-runbooks"></a>Python-runbookok

Python runbookok összeállításához, a Python 2.  Közvetlenül szerkesztheti a kódot a runbook a szövegszerkesztő használatával az Azure Portalon, vagy használhat offline szövegszerkesztőben, és [importálja a forgatókönyvet](automation-creating-importing-runbook.md) az Azure Automationbe.

### <a name="advantages"></a>Előnyök

* A robusztus Python-kódtárakat használni.

### <a name="limitations"></a>Korlátozások

* Python-szkriptek ismernie kell.
* Csak a Python 2 támogatott abban a pillanatban, ami azt jelenti, adott funkciók Python 3 sikertelen lesz.
* Annak érdekében, hogy felhasználja az külső gyártó kódtárait kell [a csomag importálása](python-packages.md) Automation-fiókba a hozzá tartozó használható.

## <a name="considerations"></a>Megfontolandó szempontok

Akkor figyelembe kell vennie az alábbi további szempontok annak meghatározása, amely egy adott forgatókönyvhöz használatához írja be.

* A grafikus runbookok nem alakítható át szöveges típusú vagy fordítva.
* Különböző típusú runbookok használatával egy gyermek runbook korlátozások is.  Lásd: [az Azure Automation runbookok](automation-child-runbooks.md) további információt.

## <a name="next-steps"></a>További lépések

* Grafikus forgatókönyv-készítési kapcsolatos további információkért lásd: [grafikus létrehozás az Azure Automationben](automation-graphical-authoring-intro.md)
* A PowerShell és a PowerShell közötti különbségek megértéséhez munkafolyamatokat a runbookok, lásd: [Windows PowerShell-munkafolyamat megismerése](automation-powershell-workflow.md)
* Hozzon létre vagy importáljon egy Runbook való további információkért lásd: [létrehozása vagy egy Runbook importálása](automation-creating-importing-runbook.md)

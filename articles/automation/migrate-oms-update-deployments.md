---
title: Az OMS Frissítéstelepítések migrálása az Azure-bA
description: Ez a cikk bemutatja, hogyan telepítheti át az OMS frissítéstelepítések az Azure-bA
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 07/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d0b380aa6046daa235098516a8c93d3ba72533a6
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/15/2018
ms.locfileid: "42056670"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>Az OMS Frissítéstelepítések migrálása az Azure-bA

Folyamatban van, az Operations Management Suite (OMS) portal [elavult](../log-analytics/log-analytics-oms-portal-transition.md). Az OMS-portálon az Update Management elérhető összes funkciót az Azure Portalon érhető el. Ez a cikk az Azure Portalra való migrálásához szükséges információkat.

## <a name="key-information"></a>Fontos információk

* Meglévő telepítések továbbra is működni fog. Miután az üzembe helyezés az Azure-ban kell újból, törölheti a régi központi telepítési az OMS-ből.
* Az OMS-ben már az összes meglévő funkciók érhetők el az Azure-ban, a frissítések kezelésével kapcsolatos további tudnivalókért lásd: [az Update Management áttekintése](automation-update-management.md).

## <a name="access-the-azure-portal"></a>Az Azure portal eléréséhez

Az OMS-munkaterületen kattintson **nyissa meg az Azure-ban**. Ugrás a Log Analytics-munkaterületet, amelyet OMS használni.

![Nyissa meg az Azure-ban – OMS-portálon](media/migrate-oms-update-deployments/link-to-azure-portal.png)

Az Azure Portalon kattintson a **Automation-fiók**

![Log Analytics](media/migrate-oms-update-deployments/log-analytics.png)

Az Automation-fiókban kattintson **az Update Management** az Update Management megnyitásához.

![Frissítéskezelés](media/migrate-oms-update-deployments/azure-automation.png)

A jövőben megnyithatja közvetlenül az Azure Portalon, a **minden szolgáltatás**, jelölje be **Automation-fiókok** alatt **kezelőeszközök**, válassza ki a megfelelő Automation Fiókja, majd kattintson a **az Update Management**.

## <a name="recreate-existing-deployments"></a>Hozza létre újra a meglévő telepítések

Az OMS-portálon létrehozott összes telepítést rendelkezik egy [-beli mentett keresés](../log-analytics/log-analytics-computer-groups.md) , a frissítések telepítését, hogy létezik azonos nevű számítógépcsoport néven is ismert. A mentett keresés volt a frissítéstelepítés ütemezett gépek listáját tartalmazza.

![Frissítéskezelés](media/migrate-oms-update-deployments/oms-deployment.png)

A meglévő mentett keresés használatához kövesse az alábbi lépéseket:

Hozzon létre egy új frissítési telepítés, nyissa meg az Azure Portalon válassza szolgál, és kattintson az Automation-fiókot **frissítéskezelési**. Kattintson a **frissítések központi telepítésének ütemezése**.

![Frissítések központi telepítésének ütemezése](media/migrate-oms-update-deployments/schedule-update-deployment.png)

A **új frissítéstelepítés** panel nyílik meg. Adja meg az értékeket az alábbi táblázatban leírt tulajdonságokkal, és kattintson a **létrehozás**:

Frissítendő gépek válassza ki a mentett keresésnek, használja a meglévő OMS-telepítés.

| Tulajdonság | Leírás |
| --- | --- |
|Name (Név) |A frissítéstelepítést beazonosító egyedi név. |
|Operációs rendszer| Válassza ki **Linux** vagy **Windows**.|
|Frissítendő gépek |Válassza ki, mentett keresést, importált csoporthoz, vagy a legördülő listából válassza ki a gépet, és válassza ki az egyes gépek. Ha úgy dönt, **gépek**, jelenik meg a gép készen áll-e a **frissítési ÜGYNÖK KÉSZÜLTSÉGE** oszlop.</br> Számítógépcsoportok létrehozását a Log Analytics különböző módszereivel kapcsolatos további információkért lásd: [Log Analytics számítógépcsoportjaival](../log-analytics/log-analytics-computer-groups.md) |
|Frissítési besorolások|Válassza ki az összes szükséges. CentOS nem támogatja a beépített.|
|Kihagyandó frissítések|Adja meg a kihagyandó frissítések. Windows, adja meg a tudásbáziscikkben nélkül a **KB-os** előtag. A Linux rendszerre adja meg a csomag nevét, vagy helyettesítő karaktert használni.  |
|Ütemezési beállítások|Válassza ki a kezdési idejét, és válassza vagy **egyszer** vagy **ismétlődő** az ismétlődés.|| Karbantartási időszak |Frissítések beállított percek száma. Az érték nem lehet kisebb, mint 30 perc vagy 6 óra. |
| Karbantartási időszak |Frissítések beállított percek száma. Az érték lehet nem lehet kisebb, mint 30 perc és legfeljebb 6 óra |
| Vezérlő újraindítása| Detemines újraindítások kezelésének módját.</br>Rendelkezésre álló lehetőségek közül választhat:</br>(Alapértelmezett) szükség esetén újraindítás</br>Mindig újraindítás</br>Soha ne újraindítás</br>Csak újraindítás - nem telepíti a frissítéseket|

Kattintson a **ütemezett frissítéstelepítések** a újonnan létrehozott központi telepítési állapotának megtekintéséhez.

![új frissítéstelepítés](media/migrate-oms-update-deployments/new-update-deployment.png)

Ahogy korábban említettük, az új üzemelő példányok az Azure Portalon keresztül konfigurálása után, a meglévő központi telepítéseknél eltávolíthatók az OMS-portálon.

## <a name="next-steps"></a>További lépések

Az Azure-beli kezelésével kapcsolatos további tudnivalókért lásd: [az Update Management](automation-update-management.md)
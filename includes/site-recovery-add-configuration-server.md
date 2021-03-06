---
title: fájl belefoglalása
description: fájl belefoglalása
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/06/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 2ca4916d48da6fe8a2c061056a1ea0fed9a78bb6
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/06/2018
ms.locfileid: "44058303"
---
1. Futtassa az egyesített telepítő fájlját.
2. A **alapismeretek**válassza **a konfigurációs kiszolgáló és a folyamatkiszolgáló telepítése**.

    ![Előkészületek](./media/site-recovery-add-configuration-server/combined-wiz1.png)

3. A **Külső szoftver licence** területen kattintson az **Elfogadom** elemre a MySQL letöltéséhez és telepítéséhez.

    ![Külső gyártótól származó szoftverek](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. A **Regisztráció** területen válassza ki a kulcstartóból letöltött regisztrációs kulcsot.

    ![Regisztráció](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. Az **Internetbeállítások** területen adja meg, hogy a konfigurációs kiszolgálón futó Provider hogyan csatlakozzon az Azure Site Recoveryhez az interneten keresztül. Győződjön meg arról, hogy engedélyezte a szükséges URL-címek.

    - Ha szeretne csatlakozni, amely jelenleg be van állítva a számítógépen, válassza a proxy **csatlakozás az Azure Site Recoveryhez proxykiszolgálóval**.
    - Ha azt szeretné, hogy a szolgáltatót, hogy közvetlenül kapcsolódjon, válassza ki a **közvetlen csatlakozás az Azure Site Recoveryhez proxykiszolgáló nélkül**.
    - Ha a meglévő proxy hitelesítést igényel, vagy ha egyéni proxyt használ a Provider csatlakoztatásához, válassza ki a kívánt **csatlakozás egyéni proxybeállításokkal**, és adja meg a cím, port és hitelesítő adatokat.
     ![Tűzfal](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. Az **Előfeltételek ellenőrzése** területen a telepítő ellenőrzi, hogy a telepítés végrehajtható-e. Ha megjelenik egy figyelmeztetés a **globális időszinkron ellenőrzéséről**, ellenőrizze, hogy a rendszeróra ideje (a **Dátum és idő** beállítások) megegyeznek-e az időzónával.

    ![Előfeltételek](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. A **MySQL-konfiguráció** területen hozza létre a telepített MySQL-kiszolgálópéldányra való bejelentkezéshez szükséges hitelesítő adatokat.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. A **környezet részletei**, jelölje be, nem, ha az Azure Stack-beli virtuális vagy fizikai kiszolgálókat replikál. 
9. A **Telepítés helye** területen válassza ki, hová szeretné telepíteni a bináris fájlokat, és hol kívánja tárolni a gyorsítótárat. A kiválasztott meghajtón legalább 5 GB szabad lemezterületre van szükség, de javasoljuk, hogy a gyorsítótárazáshoz használt lemezen legyen legalább 600 GB szabad hely.

    ![Telepítés helye](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. A **Hálózat kiválasztása** területen adja meg a figyelőt (hálózati adaptert és SSL-portot), amelyen keresztül a konfigurációs kiszolgáló küldi és fogadja a replikált adatokat. A 9443-as port a replikációs forgalom küldésére és fogadására használt alapértelmezett port, ez azonban a környezeti követelményektől függően módosítható. A 9443-as port mellett a 443-as portot is megnyitjuk, amelyen keresztül egy webkiszolgáló a replikálási műveleteket vezényli. Ne használja a 443-as porton küldése vagy fogadása a replikációs forgalom számára.

    ![Hálózat kiválasztása](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. Az **Összefoglalás** területen ellenőrizze az adatokat, majd kattintson a **Telepítés** gombra. A telepítés után a rendszer létrehoz egy hozzáférési kódot. Erre szüksége lesz, amikor engedélyezi a replikálást, ezért másolja le, és tárolja biztonságos helyen.

    ![Összegzés](./media/site-recovery-add-configuration-server/combined-wiz10.png)

A regisztráció befejezését követően a kiszolgáló megjelenik a tároló **Beállítások** > **Kiszolgálók** paneljén.

---
title: Biztonsági mentése az Exchange-kiszolgáló Azure Backup szolgáltatás az Azure Backup Server
description: 'Útmutató: biztonsági mentése az Exchange-kiszolgáló Azure Backup szolgáltatás használatával az Azure Backup Server'
services: backup
author: pvrk
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: d64c273a189b1fe2337c4430b156874e0adf54b2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605960"
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-azure-backup-server"></a>Biztonsági mentése az Exchange-kiszolgáló Azure Backup szolgáltatás az Azure Backup Server
Ez a cikk ismerteti, hogyan konfigurálása a Microsoft Azure Backup Server (MABS) biztonsági mentése egy Microsoft Exchange-kiszolgálóhoz az Azure-bA.  

## <a name="prerequisites"></a>Előfeltételek
A folytatás előtt győződjön meg arról, hogy Azure Backup Server [telepítve és előkészített](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>MABS védelmi ügynök
Az Exchange kiszolgálón a MABS védelmi ügynök telepítéséhez kövesse az alábbi lépéseket:

1. Győződjön meg arról, hogy a tűzfalak konfigurációja megfelelő. Lásd: [tűzfalkivétel konfigurálása az ügynök számára](https://technet.microsoft.com/library/Hh758204.aspx).
2. Az Exchange-kiszolgálóhoz gombra kattintva a ügynök telepíthető **felügyeleti > ügynökök > telepítése** MABS felügyeleti konzolon. Lásd: [a MABS védelmi ügynök telepítése](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) a részletes lépéseket.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Az Exchange-kiszolgáló védelmi csoport létrehozása
1. A MABS felügyeleti konzolon kattintson a **védelmi**, és kattintson a **új** megnyitásához az eszközsávon a **új védelmi csoport létrehozása** varázsló.
2. Az a **üdvözlő** képernyőjén kattintson a varázsló **következő**.
3. Az a **védelmi csoport típusának kiválasztása** képernyőn, jelölje be **kiszolgálók** kattintson **következő**.
4. Válassza ki az Exchange server-adatbázis védelmét, és kattintson a kívánt **következő**.

   > [!NOTE]
   > Ha az Exchange 2013 védelmét, ellenőrizze a [Exchange 2013 előfeltételei](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    A következő példában az Exchange 2010 adatbázis van kiválasztva.

    ![Csoporttagok kiválasztása](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Az adatvédelmi módszer kiválasztása.

    A védelmi csoport neve, majd válassza ki a következők mindegyikét:

   * Rövid távú lemezes védelmet szeretnék.
   * Online védelmet szeretnék.
6. Kattintson a **Tovább** gombra.
7. Válassza ki a **Eseutil futtatása az adatok sértetlenségének ellenőrzéséhez** lehetőséget, ha azt szeretné, hogy az Exchange Server-adatbázisok sértetlenségének ellenőrzéséhez.

    Miután kiválasztotta ezt a beállítást, biztonsági mentés konzisztencia-ellenőrzést fog futni a MABS az i/o-forgalmat futtatásával elkerülése érdekében a **eseutil** parancsot az Exchange-kiszolgálón.

   > [!NOTE]
   > Használja ezt a beállítást, át kell másolnia az Ese.dll és az Eseutil.exe fájloknak a C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin könyvtárához a MAB. Ellenkező esetben aktiválódik, a következő hibával:  
   > ![az Eseutil hiba](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Kattintson a **Tovább** gombra.
9. Válassza ki az adatbázist a **másolásos biztonsági mentésre**, és kattintson a **következő**.

   > [!NOTE]
   > Ha nincs bejelölve a "Teljes biztonsági másolat" adatbázis másolatának legalább egy DAG, naplók nem lesznek csonkolva.
   >
   >
10. Konfigurálja a célokat **rövid távú biztonsági mentés**, és kattintson a **következő**.
11. Tekintse át a rendelkezésre álló lemezterület, majd a **következő**.
12. Válassza ki azt az időpontot, ahol a MAB kiszolgáló létrehoz a kezdeti replikálást, és kattintson a **következő**.
13. A konzisztencia-ellenőrzési beállítások kiválasztása, és kattintson **következő**.
14. Adja meg az adatbázis biztonsági mentése az Azure-ba, és kattintson a kívánt **következő**. Példa:

    ![Online védelem adatainak megadása](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Adja meg a ütemezését **Azure biztonsági mentés**, és kattintson a **tovább**. Példa:

    ![Adja meg az online biztonsági mentés ütemezése](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Jegyezze fel az Online helyreállítási pontok alapuló Expressz teljes helyreállítási pontokat. Az online helyreállítási pontot, ezért úgy kell ütemeznie után az idő megadott az expressz teljes helyreállítási pont.
    >
    >
16. Konfigurálja az adatmegőrzési **Azure biztonsági mentés**, és kattintson a **következő**.
17. Válasszon egy online replikációs lehetőséget, és kattintson a **következő**.

    Ha nagy adatbázis, a kezdeti biztonsági másolatot a hálózaton keresztül a létrehozandó hosszú ideig eltarthat. A probléma elkerülése érdekében, létrehozhat egy offline biztonsági másolat.  

    ![Online megőrzési szabály megadása](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Hagyja jóvá a beállításokat, és kattintson a **csoport létrehozása**.
19. Kattintson a **Bezárás** gombra.

## <a name="recover-the-exchange-database"></a>Az Exchange-adatbázis helyreállítása
1. Exchange-adatbázis helyreállítása, kattintson a **helyreállítási** MABS felügyeleti konzolján.
2. Keresse meg a helyreállítani kívánt Exchange-adatbázis.
3. Válassza ki az online helyreállítási pontot a *helyreállításkor* legördülő listából.
4. Kattintson a **helyreállítása** elindítani a **helyreállítási varázsló**.

Az online helyreállítási pontok, öt helyreállítási típusa van:

* **Helyreállítás az eredeti Exchange-kiszolgálón:** fogja visszaállítani az adatokat az eredeti Exchange-kiszolgálóhoz.
* **Helyreállítás az Exchange-kiszolgáló egy másik adatbázisba:** fogja visszaállítani az adatokat egy másik egy másik Exchange server-adatbázisba.
* **Helyreállítás helyreállítási adatbázisba:** lesz az adatok helyreállítása egy Exchange helyreállítási adatbázis (Rekordadatbázis).
* **Másolás hálózati mappába:** fogja visszaállítani az adatokat egy hálózati mappába.
* **Másolás szalagra:** egy szalagtárat vagy egy önálló szalagos meghajtót csatlakoztatott, de MABS konfigurálva van, ha a helyreállítási pont egy szabad szalagra kerülnek-e.

    ![Válassza ki az online replikációs](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>További lépések
* [Az Azure biztonsági mentési – gyakori kérdések](backup-azure-backup-faq.md)

---
title: Migrálás minimális állásidővel az Azure Database for MySQL-hez
description: Ez a cikk bemutatja, hogyan áttelepítése egy minimális állásidővel MySQL-adatbázis, Azure database for MySQL-hez az Azure Database Migration Service használatával.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 55d5cf97225508d6c7c490347cfe21ced832300e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091717"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migrálás minimális állásidővel az Azure Database for MySQL-hez
Hajthat végre MySQL migrálást az Azure Database for MySQL-hez minimális állásidővel segítségével az újonnan bevezetett **folyamatos szinkronizálás képesség** számára a [Azure Database Migration Service](https://aka.ms/get-dms) (DMS). Ez a funkció az alkalmazás által felmerülő állásidő mennyiségét korlátozza.

## <a name="overview"></a>Áttekintés
A DMS hajt végre egy kezdeti betöltése a helyszíni az Azure Database for MySQL-hez, és ezután közben az alkalmazás továbbra is futni fog folyamatosan szinkronizálja az Azure-ban új tranzakciók. Után a cél Azure oldaláról behozza az adatokat, akkor állítsa le az alkalmazást egy pillanatra (minimális üzemkimaradással) számára, várja meg a legutóbbi köteg adatok (az az idő, amíg az alkalmazás hatékonyan érhető el minden olyan új forgalom érvénybe állítsa le az alkalmazást) olvasásra beállítása a célkiszolgálón, és frissítse a kapcsolati karakterláncot, mutasson az Azure-bA. Ha elkészült, az alkalmazás lesz élő Azure-ban!

![Az Azure Database Migration Service folyamatos szinkronban](./media/howto-migrate-online/ContinuousSync.png)

- A MySQL-források DMS áttelepítési jelenleg nyilvános előzetes verzióban érhető el. Ha szeretné kipróbálni a szolgáltatás a MySQL-alapú számítási feladatok migrálása, lépjen tovább a portálon. A visszajelzés felbecsülhetetlen abban, hogy a szolgáltatás további fejlesztéséhez.

## <a name="next-steps"></a>További lépések
- A videó megtekintésének [egyszerűen migrálhatja a MySQL/PostgreSQL a felügyelt alkalmazások az Azure-bA](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), amely tartalmaz egy MySQL-alkalmazások migrálása az Azure Database for MySQL-hez való ismertető bemutató.
- [MySQL migrálása az Azure Database for MySQL-hez a DMS használatával online] (https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).

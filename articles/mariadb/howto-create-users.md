---
title: Felhasználók létrehozása az Azure Database for MariaDB-kiszolgáló
description: Ez a cikk bemutatja, hogyan kommunikálhat egy Azure Database for MariaDB-kiszolgáló új felhasználói fiókokat hozhat létre.
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 50154a7fee63eb3ff9e08155123f9e5962bbfcf0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946116"
---
# <a name="create-users-in-azure-database-for-mariadb"></a>Felhasználók létrehozása az Azure Database for MariaDB 
Ez a cikk bemutatja, hogyan hozhat létre felhasználókat az Azure Database for MariaDB.

Első létrehozásakor az Azure Database for MariaDB, megadott egy kiszolgálói rendszergazda felhasználónevét és jelszavát. További információkért kövesse a [rövid](quickstart-create-mariadb-server-database-using-azure-portal.md). Megkeresheti a kiszolgáló rendszergazdai bejelentkezési felhasználónevet, az Azure Portalról.

A kiszolgáló-rendszergazdai felhasználó néhány jogosultsággal, a kiszolgáló lekérdezi, lásd: válassza ki, BESZÚRÁSA, frissítése, törlése, hozzon létre és töltse be újra, folyamat, hivatkozások, INDEX, ALTER, ADATBÁZISOK megjelenítése, az ideiglenes táblák létrehozása, ZÁROLÁS táblák, hajtsa végre, replikációs alárendelt, replikációs dobja el ÜGYFÉL, NÉZET LÉTREHOZÁSA, MEGJELENÍTÉSE, LÉTREHOZÁSA RUTIN, RUTIN ALTER, FELHASZNÁLÓI, ESEMÉNY-ESEMÉNYINDÍTÓ LÉTREHOZÁSA

Az Azure Database for MariaDB-kiszolgáló létrehozása után további felhasználók létrehozása az első kiszolgálói rendszergazdai felhasználói fiók segítségével, rendszergazdai hozzáférést biztosít számukra. Emellett a kiszolgálói rendszergazdai fiókkal, amelyhez hozzáférése egyes adatbázissémák kevesebb jogosultsággal rendelkező felhasználók létrehozásához használható.

## <a name="create-additional-admin-users"></a>További rendszergazdai felhasználók létrehozása
1. Kérje le a kapcsolati információkat és a rendszergazdai felhasználó nevét.
   Az adatbázis-kiszolgálóhoz való csatlakozáshoz szüksége van a teljes kiszolgálónévre és a rendszergazdai bejelentkezési hitelesítő adatokra. Megtalálhatja a kiszolgáló nevét és bejelentkezési adatait a kiszolgálóról **áttekintése** lap vagy az **tulajdonságok** oldal az Azure Portalon. 

2. A rendszergazdai fiókot és jelszót használja az adatbázis-kiszolgálóhoz való kapcsolódáshoz. Használja az ügyfél előnyben részesített eszközt, például a MySQL Workbench, mysql.exe, HeidiSQL vagy mások. 
   Ha biztos benne, hogy hogyan kapcsolódhat, [való csatlakozás és adatlekérdezés a MySQL Workbench](./connect-workbench.md)

3. Szerkessze és futtassa a következő SQL-kódot. Cserélje le a helyőrző értékét az új felhasználói nevet `new_master_user`. Ezt a szintaxist biztosít a listában szereplő összes adatbázissémák jogosultságokkal (*.*), a felhasználó nevét (ebben a példában new_master_user). 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. A jogok ellenőrzése 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="create-database-users"></a>Adatbázis-felhasználók létrehozása

1. Kérje le a kapcsolati információkat és a rendszergazdai felhasználó nevét.
   Az adatbázis-kiszolgálóhoz való csatlakozáshoz szüksége van a teljes kiszolgálónévre és a rendszergazdai bejelentkezési hitelesítő adatokra. Megtalálhatja a kiszolgáló nevét és bejelentkezési adatait a kiszolgálóról **áttekintése** lap vagy az **tulajdonságok** oldal az Azure Portalon. 

2. A rendszergazdai fiókot és jelszót használja az adatbázis-kiszolgálóhoz való kapcsolódáshoz. Használja az ügyfél előnyben részesített eszközt, például a MySQL Workbench, mysql.exe, HeidiSQL vagy mások. 
   Ha biztos benne, hogy hogyan kapcsolódhat, [való csatlakozás és adatlekérdezés a MySQL Workbench](./connect-workbench.md)

3. Szerkessze és futtassa a következő SQL-kódot. A helyőrző értékét cserélje le `db_user` a kívánt új felhasználónevet és a helyőrző értékét `testdb` a saját adatbázis nevére.

   Az sql-kódot szintaxis létrehoz egy új adatbázist testdb nevű példa célokra. Egy új felhasználót hoz létre az Azure Database for MariaDB-szolgáltatás, és az új adatbázis-séma az összes jogosultságot biztosít (testdb.\*) az adott felhasználó. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Ellenőrizze a engedélyezi az adatbázison belül.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Jelentkezzen be a kiszolgálóra, és adja meg a kijelölt adatbázis, az új felhasználónévvel és jelszóval. Ez a példa bemutatja a mysql parancssorból. Ezzel a paranccsal kéri a felhasználónévhez tartozó jelszót. Cserélje le a saját kiszolgáló nevét, az adatbázis neve és a felhasználó neve.

   ```bash
   mysql --host mydemoserver.mariadb.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```
Fiókkezeléssel kapcsolatos további információkért lásd: MariaDB dokumentációját [felhasználóifiók-kezelés](https://mariadb.com/kb/en/library/user-account-management/), [GRANT szintaxis](https://mariadb.com/kb/en/library/grant/), és [jogosultságokkal](https://mariadb.com/kb/en/library/grant/#privilege-levels).

## <a name="next-steps"></a>További lépések
Az új felhasználók gépek, hogy azok tudjanak csatlakozni az IP-címek számára megnyitja a tűzfalat: [hozzon létre és kezelhető az Azure Database for MariaDB tűzfalszabályok az Azure portal használatával](howto-manage-firewall-portal.md)  

<!--or [Azure CLI](howto-manage-firewall-using-cli.md).-->

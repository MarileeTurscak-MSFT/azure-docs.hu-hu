---
title: Létrehozása és kezelése a MariaDB tűzfalszabályok az Azure Database for MariaDB
description: Hozzon létre és kezelhető az Azure Database for MariaDB tűzfalszabályok az Azure portal használatával
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a43c47a1ac143014a7b36f64d72d20bf73c05c92
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950284"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-portal"></a>Hozzon létre és kezelhető az Azure Database for MariaDB tűzfalszabályok az Azure portal használatával
Kiszolgálószintű tűzfalszabályok engedélyezése a rendszergazdák számára, hogy a MariaDB-kiszolgáló Azure-adatbázis elérését a megadott IP-cím vagy egy IP-címtartományt. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Kiszolgálószintű tűzfalszabály létrehozása az Azure Portalon

1. A MariaDB kiszolgáló, a beállítások lapon szakaszban kattintson **kapcsolatbiztonság** a kapcsolatbiztonság lap megnyitásához az Azure Database for MariaDB-hez.

   ![Az Azure portal – kapcsolatbiztonság kattintson](./media/howto-manage-firewall-portal/1-connection-security.png)

2. Kattintson a **adjon hozzá saját IP-címet** az eszköztáron. Ez automatikusan hoz létre egy tűzfalszabályt a számítógép nyilvános IP-címét, az Azure rendszer által érzékelt.

   ![Az Azure portal – kattintson a Hozzáadás saját IP-cím](./media/howto-manage-firewall-portal/2-add-my-ip.png)

3. Az IP-cím ellenőrzése a konfiguráció mentése előtt. Bizonyos esetekben az Azure portal által megfigyelt IP-cím eltér az internetes és az Azure-kiszolgálókkal való elérésekor használt IP-cím. Emiatt előfordulhat, hogy módosítani szeretné a kezdő IP- és a záró IP-cím, hogy a szabály az elvárt módon működnek.

   Egy keresőmotor vagy egyéb online eszközt használja a saját IP-cím ellenőrzése. Például keresés, "Mi az saját IP-cím".

4. További címtartomány hozzáadásához. A tűzfalszabályok az Azure Database for MariaDB, az egyetlen IP-címet vagy címtartományt is megadhat. Ha szeretné korlátozni a szabályt, hogy egyetlen IP-címet, írja be ugyanazt a címet a kezdő IP- és a záró IP-cím mezőt. A tűzfal megnyitása után lehetővé teszi a rendszergazdák, a felhasználók és az alkalmazás eléréséhez, amelyhez érvényes hitelesítő adatokkal rendelkeznek a MariaDB-kiszolgáló bármely olyan adatbázisába.

   ![Az Azure portal - tűzfalszabályok ](./media/howto-manage-firewall-portal/4-specify-addresses.png)

5. Kattintson a **mentése** gombra az eszköztárban, a kiszolgálószintű tűzfalszabály mentéséhez. Várjon, amíg a visszaigazolás, hogy létrejött-e a tűzfalszabályok a frissítést.

   ![Az Azure portal – kattintson a Mentés gombra](./media/howto-manage-firewall-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Csatlakozás az Azure-ból
Ahhoz, hogy az Azure-alkalmazások az Azure Database for MariaDB-kiszolgálóhoz való kapcsolódáshoz, engedélyezni kell az Azure-kapcsolatokat. Ha például üzemeltetése az Azure Web Apps-alkalmazáshoz, vagy egy Azure-beli virtuális gépen futó alkalmazást, vagy egy Azure Data Factory az adatkezelési átjáró csatlakozni. Az erőforrások nem kell ugyanazon a virtuális hálózaton (VNet) vagy a tűzfalszabály erőforráscsoportjának ahhoz, hogy ezeket a kapcsolatokat lehet. Amikor egy Azure-alkalmazás megkísérel csatlakozni az adatbázis-kiszolgálóhoz, a tűzfal ellenőrzi, hogy az Azure-kapcsolatok engedélyezve vannak-e. Van néhány módszerek ilyen típusú kapcsolatok engedélyezéséhez. A 0.0.0.0 kezdő- és zárócímet tartalmazó tűzfalbeállítás jelzi, hogy ezek a kapcsolatok engedélyezettek. Beállíthatja azt is megteheti, a **Azure-szolgáltatásokhoz való hozzáférés engedélyezése** beállítást **ON** a portálon a **kapcsolatbiztonság** panelt, és kattintson **mentése**. A csatlakozási kísérlet nem engedélyezett, ha a kérés nem éri el az Azure Database for MariaDB-kiszolgáló.

> [!IMPORTANT]
> Ez a beállítás konfigurálja a tűzfalat arra, hogy engedélyezzen minden, az Azure felől érkező kapcsolatot, beleértve a más ügyfelek előfizetéseiből érkező kapcsolatokat is. Ezen beállítás kiválasztásakor győződjön meg arról, hogy a bejelentkezési és felhasználói engedélyei a hozzáféréseket az arra jogosult felhasználókra korlátozzák.
> 

## <a name="manage-existing-firewall-rules-in-the-azure-portal"></a>Az Azure Portalon a meglévő tűzfalszabályok kezelése
Ismételje meg a tűzfalszabályok kezelésére.
* Az aktuális számítógép hozzáadásához kattintson **+ Hozzáadás a saját IP-címet**. Kattintson a **Mentés** gombra a módosítások mentéséhez.
* További IP-címek hozzáadásához írja be a **SZABÁLYNÉV**, **KEZDŐ IP-**, és **záró IP-cím**. Kattintson a **Mentés** gombra a módosítások mentéséhez.
* Meglévő szabály módosításához kattintson a szabály a mezők, és módosítsa. Kattintson a **Mentés** gombra a módosítások mentéséhez.
* Meglévő szabály törléséhez kattintson a három pont [...], és kattintson **törlése**. Kattintson a **Mentés** gombra a módosítások mentéséhez.

<!--
## Next steps
 - Similarly, you can script to [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](howto-manage-firewall-using-cli.md).
- For help in connecting to an Azure Database for MariaDB server, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md) -->

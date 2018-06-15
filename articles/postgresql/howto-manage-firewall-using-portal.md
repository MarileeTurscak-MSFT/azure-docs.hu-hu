---
title: Hozzon létre és kezelheti az Azure adatbázis tűzfalszabályok PostgreSQL
description: Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok az Azure portál használatával
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: bef927cff49d957728a2a12362786d48d60e61b7
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/28/2018
ms.locfileid: "29690339"
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a>Hozzon létre és kezelheti az Azure-adatbázis PostgreSQL-tűzfalszabályok az Azure portál használatával
Kiszolgálószintű tűzfal-szabályok lehetővé teszik a rendszergazdák az Azure-adatbázisának eléréséhez PostgreSQL-kiszolgáló a megadott IP-cím vagy az IP-címek. 

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató Útmutató lépéseit, az alábbiak szükségesek:
- A kiszolgáló [PostgreSQL az Azure-adatbázis létrehozása](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Kiszolgálószintű tűzfalszabály létrehozása az Azure Portalon
1. PostgreSQL-kiszolgáló beállításai lapon elemcsoportban kattintson **kapcsolatbiztonsági** PostgreSQL megnyitandó a kapcsolat biztonsági beállításait tartalmazó lapot az Azure-adatbázishoz.

  ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Kattintson a **hozzáadása a saját IP** az eszköztáron. Ez automatikusan létrehozza a tűzfalszabályok a számítógép a nyilvános IP-címmel, az Azure rendszer által érzékelt.

  ![Azure portál – kattintson a saját IP-cím hozzáadása](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Az IP-cím ellenőrzése a konfiguráció mentéséhez. Bizonyos esetekben az IP-cím, az Azure portál által megfigyelt eltér az internetes és az Azure-kiszolgálók eléréséhez használt IP-címét. Emiatt előfordulhat, hogy módosítani szeretné a kezdő IP- és a záró IP-, a várt módon szabály függvény végrehajtásához.
Egy keresőmotor vagy más online eszközt használja a saját IP-cím ellenőrzése. Keressen például, "Mi az saját IP."

  ![Mi az a saját IP Bing keresése](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Adjon hozzá további címtartományok. Az adatbázisra vonatkozó tűzfalszabályok az Azure a PostgreSQL megadhatja a egyetlen IP-címet, vagy címtartományokat. Ha szeretné korlátozni a szabály, amely egyetlen IP-címnek, írja be ugyanazt a címet a mezőben a kezdő IP- és a záró IP. Megnyitásáról a tűzfal lehetővé teszi a rendszergazdák, a felhasználók és az alkalmazások a bejelentkezéshez a PostgreSQL-kiszolgálón, amelyhez bármely adatbázisra érvényes hitelesítő adatokkal rendelkezik.

  ![Azure portál – tűzfalszabályok ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Kattintson a **mentése** az eszköztáron menteni a kiszolgálószintű tűzfalszabály. Várja meg, hogy a tűzfalszabályok frissítése sikeres volt-e a jóváhagyást.

  ![Azure portál – válassza a Mentés](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Csatlakozás az Azure-ból
Lehetővé teszik az alkalmazások az Azure-ból az Azure-adatbázis PostgreSQL-kiszolgálóhoz való kapcsolódáshoz, Azure-kapcsolatok engedélyezni kell. Például egy Azure Web Apps alkalmazást vagy olyan alkalmazás, amely egy Azure virtuális gép üzemeltetésére, vagy csatlakoztassa egy Azure Data Factory az adatkezelési átjáró. Az erőforrások nem kell ugyanazt a virtuális hálózatot (VNet) vagy a tűzfalszabály tartozó erőforráscsoport ahhoz, hogy ezeket a kapcsolatokat. Amikor egy Azure-alkalmazás megkísérel csatlakozni az adatbázis-kiszolgálóhoz, a tűzfal ellenőrzi, hogy az Azure-kapcsolatok engedélyezve vannak-e. Többféle módszer ahhoz, hogy ilyen típusú kapcsolatokat. A 0.0.0.0 kezdő- és zárócímet tartalmazó tűzfalbeállítás jelzi, hogy ezek a kapcsolatok engedélyezettek. Beállíthatja azt is megteheti, a **Azure-szolgáltatásokhoz való hozzáférés engedélyezése** lehetőséggel **ON** a portálon a **kapcsolatbiztonsági** ablaktáblán, és nyomja le **mentése**. A kapcsolódási kísérlet nem engedélyezett, ha a kérelem nem érte el az Azure-adatbázishoz PostgreSQL-kiszolgáló.

> [!IMPORTANT]
> Ez a beállítás konfigurálja a tűzfalat arra, hogy engedélyezzen minden, az Azure felől érkező kapcsolatot, beleértve a más ügyfelek előfizetéseiből érkező kapcsolatokat is. Ezen beállítás kiválasztásakor győződjön meg arról, hogy a bejelentkezési és felhasználói engedélyei a hozzáféréseket az arra jogosult felhasználókra korlátozzák.
> 

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Meglévő kiszolgálószintű tűzfalszabályok kezelése az Azure Portalon
Ismételje meg a tűzfal-szabályok kezelése.
* Az aktuális számítógép hozzáadásához kattintson a gombra kattintva + **hozzáadása a saját IP**. Kattintson a **Mentés** gombra a módosítások mentéséhez.
* További IP-címek hozzáadásához adja meg a Szabály neve, a Kezdő IP-cím és a Záró IP-cím értékét. Kattintson a **Mentés** gombra a módosítások mentéséhez.
* Meglévő szabály módosításához kattintson a szabály valamelyik mezőjére, és adja meg a módosításokat. Kattintson a **Mentés** gombra a módosítások mentéséhez.
* Meglévő szabály törléséhez kattintson a három pont [...], és kattintson a **törlése** a szabály eltávolításához. Kattintson a **Mentés** gombra a módosítások mentéséhez.

## <a name="next-steps"></a>További lépések
- Ehhez hasonlóan is, a parancsfájl [hozzon létre és kezelheti az Azure-adatbázis Azure parancssori felület használatával PostgreSQL tűzfalszabályok](howto-manage-firewall-using-cli.md).
- Egy PostgreSQL-kiszolgáló Azure-adatbázishoz szeretne csatlakozni a témakörben talál segítséget [PostgreSQL az Azure-adatbázis adatkapcsolattárak](concepts-connection-libraries.md).

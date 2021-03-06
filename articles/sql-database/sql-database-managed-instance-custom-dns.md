---
title: Az Azure SQL Database felügyelt példány egyéni DNS |} A Microsoft Docs
description: Ez a témakör ismerteti egy egyéni DNS-ben az Azure SQL Database felügyelt példánya a konfigurációs beállításait.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/23/2018
ms.openlocfilehash: f26ea763d48d03fe7e981b7abbbe64e573ec0b3a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224273"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Egy egyéni DNS konfigurálása az Azure SQL Database felügyelt példánya

Kell egy Azure-ban üzembe helyezni egy Azure SQL Database felügyelt példányába [virtuális hálózat (VNet)](../virtual-network/virtual-networks-overview.md). Néhány forgatókönyv (azaz a csatolt kiszolgálók más SQL-példányok a felhőalapú vagy hibrid környezetben) fel kell oldani a felügyelt példányon a privát állomásnevek igénylő közül választhat. Ebben az esetben, konfigurálnia kell egy egyéni DNS az Azure-on belül. Felügyelt példány az azonos DNS-t használ a belső működésével, mivel a virtuális hálózat DNS-konfigurációt kell lennie a felügyelt példány kompatibilis. 

Olyan egyéni DNS-konfigurációs van kompatibilis a felügyelt példány, meg kell: 
- Egyéni DNS-kiszolgáló konfigurálása, hogy legyen képes feloldani a tartományneveket 
- Helyezze az Azure rekurzív feloldó DNS IP-cím 168.63.129.16 a virtuális hálózat DNS-lista végén 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Egyéni DNS-kiszolgálók konfigurációjának beállítása

1. Az Azure Portalon keresse meg az egyéni DNS-beállítás a virtuális hálózat számára.

   ![egyéni DNS-beállítás](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Egyéni váltson, és adja meg az egyéni DNS-kiszolgáló IP-címét, valamint az Azure rekurzív feloldók IP-cím 168.63.129.16. 

   ![egyéni DNS-beállítás](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > A DNS-listája nem beállítása az Azure rekurzív feloldó okozhat a felügyelt példány hibás állapotba lép, ha valamilyen okból a egyéni DNS-kiszolgáló nem érhető el. Utáni helyreállítás állapot megkövetelhetik, hogy hozzon létre új példányt a megfelelő hálózati szabályzatok rendelkező virtuális hálózaton, példány szintű adatok létrehozásához, és az adatbázisok visszaállítása. Az Azure rekurzív feloldó beállítást, mivel biztosítja, hogy a legutóbbi bejegyzést a DNS-listában, akkor is, ha az összes egyéni DNS-kiszolgáló meghibásodik, nyilvános nevek továbbra is feloldható. Lásd: [VNet-konfiguráció](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>További lépések

- Áttekintéséhez lásd: [mit jelent a felügyelt példány](sql-database-managed-instance.md)
- Bemutatja, hogyan hozzon létre egy új felügyelt példány oktatóanyagért lásd: [hozzon létre egy felügyelt példányt](sql-database-managed-instance-get-started.md).
- A felügyelt példányhoz egy virtuális hálózat konfigurálásával kapcsolatos további információkért lásd: [felügyelt példányok VNet-konfiguráció](sql-database-managed-instance-vnet-configuration.md)

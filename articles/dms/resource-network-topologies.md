---
title: Hálózati topológia az Azure SQL adatbázis felügyelt példány áttelepítéshez az Azure-adatbázis áttelepítése szolgáltatással |} Microsoft Docs
description: Ismerje meg, hogy a forrás és cél konfigurációk adatbázis áttelepítési szolgáltatás.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 9fcee103854209016d73e29b598c9f33d35c4b6c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316867"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Az Azure SQL DB felügyelt példány áttelepítéshez az Azure-adatbázis áttelepítése szolgáltatással hálózati topológiák
A cikkből megtudhatja, hogy átfogó áttelepítési környezetet biztosítson a helyszíni SQL Server-kiszolgálók az Azure SQL adatbázis felügyelt példányra együttműködik az Azure-adatbázis áttelepítési szolgáltatás különböző hálózati topológiák.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Az Azure SQL adatbázis felügyelt példány konfigurálva hibrid munkaterhelések 
Ez a topológia akkor használja, ha az Azure SQL adatbázis-felügyelt példány a helyi hálózathoz csatlakozik. Ezt a módszert biztosít a leggyakrabban egyszerűsített hálózati, és adatok legnagyobb átviteli sebességet eredményez az áttelepítés során.

![A hibrid munkaterhelések hálózati topológia](media\resource-network-topologies\hybrid-workloads.png)

**Követelmények**
- Ebben a forgatókönyvben az Azure SQL adatbázis-felügyelt példány és az Azure adatbázis áttelepítési szolgáltatáspéldány ugyanazon Azure virtuális jönnek létre, de különböző alhálózatokon használnak.  
- Az ebben a forgatókönyvben használt VNET is csatlakozik-e a helyszíni hálózat használatával vagy [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) vagy [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Az Azure SQL adatbázis felügyelt példány különítve a helyszíni hálózat
A hálózati topológia használja, ha a környezetében szükséges legalább egy, a következő esetekben:
- Az Azure SQL adatbázis-felügyelt példány el különítve a helyi kapcsolat, de az Azure adatbázis áttelepítési szolgáltatáspéldány a helyszíni hálózathoz csatlakozik.
- Ha a szerepköralapú hozzáférés vezérlés (RBAC) házirendek legyenek érvényben, és ugyanahhoz az előfizetéshez, amelyen az Azure SQL adatbázis felügyelt példány elérése a felhasználók korlátozása kell.
- A Vnetek az Azure SQL adatbázis-felügyelt példány használja, és az Azure-adatbázis áttelepítési szolgáltatás különböző előfizetésekhez.

![A helyszíni hálózat különítve felügyelt példány hálózati topológia](media\resource-network-topologies\mi-isolated-workload.png)

**Követelmények**
- A VNET Azure adatbázis áttelepítési szolgáltatás által használt ebben a forgatókönyvben is kapcsolódnia kell a helyszíni hálózat használatával (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) vagy [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Állítson be [virtuális hálózat hálózati társviszony-létesítés](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) a VNETBEN használt Azure SQL adatbázis felügyelt példányhoz, és az Azure-adatbázis áttelepítési szolgáltatás között.


## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Felhő felhő áttelepítések: megosztott virtuális hálózat

Ebben a topológiában használja, ha a forrás SQL Server egy Azure virtuális gép üzemel, és osztja meg ugyanazt a virtuális Hálózatot az Azure SQL Database felügyelt-példányt és az Azure-adatbázis áttelepítési szolgáltatás.

![Hálózati topológia felhő felhő áttelepítések egy megosztott virtuális hálózaton](media\resource-network-topologies\cloud-to-cloud.png)

**Követelmények**
- Nincsenek további követelmények.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Felhő áttelepítésre felhőbeli: elkülönített hálózatok

A hálózati topológia használja, ha a környezetében szükséges legalább egy, a következő esetekben:
- Az Azure SQL adatbázis-felügyelt példány egy elszigetelt virtuális lett kiépítve.
- Ha a szerepköralapú hozzáférés vezérlés (RBAC) házirendek legyenek érvényben, és ugyanahhoz az előfizetéshez, amelyen az Azure SQL adatbázis felügyelt példány elérése a felhasználók korlátozása kell.
- Az Azure SQL Database felügyelt-példányt és az Azure-adatbázis áttelepítési szolgáltatás Vnetekhez különböző előfizetésekhez vannak.

![Hálózati topológia felhő felhő áttelepítések egy elszigetelt virtuális hálózaton](media\resource-network-topologies\cloud-to-cloud-isolated.png)

**Követelmények**
- Állítson be [virtuális hálózat hálózati társviszony-létesítés](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) a VNETBEN használt Azure SQL adatbázis felügyelt példányhoz, és az Azure-adatbázis áttelepítési szolgáltatás között.


## <a name="see-also"></a>Lásd még:
- [Az Azure SQL adatbázis felügyelt példány SQL-kiszolgáló áttelepítése](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Az Azure-adatbázis áttelepítési szolgáltatás használatára vonatkozó előfeltételek áttekintése](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Virtuális hálózat létrehozása az Azure Portallal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>További lépések
Az Azure-adatbázis áttelepítése szolgáltatás és a nyilvános előzetes regionális rendelkezésre állási áttekintését lásd: a cikk [Mi az az Azure adatbázis áttelepítési szolgáltatás előzetes](dms-overview.md). 
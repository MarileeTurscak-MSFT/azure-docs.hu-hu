---
title: SQL virtuális gépeken (klasszikus) felügyeleti feladatok automatizálására |} Microsoft Docs
description: Ez a témakör az SQL Server agent-kiterjesztés, automatizálja az adott SQL Server felügyeleti feladatok kezelését ismerteti. Ezek közé tartoznak az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció. Ez a témakör a klasszikus üzembe helyezési módot használ.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/07/2018
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3ff9a8b91b0359c57fae5b1a01b5d895ab9a1685
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/09/2018
ms.locfileid: "29851758"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-classic"></a>Azure virtuális gépeken kiterjesztésű SQL Server Agent (klasszikus) felügyeleti feladatok automatizálásához
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Klasszikus](../classic/sql-server-agent-extension.md)
> 
>
 
Az SQL Server IaaS ügynök Extension (SQLIaaSAgent) fut az Azure virtuális gépeken felügyeleti feladatok automatizálására. Ez a témakör a bővítményt, valamint a vonatkozó telepítési, állapot és eltávolítási utasításokat támogatja a szolgáltatások áttekintését.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk a klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja. Ez a cikk erőforrás-kezelő verziójának megtekintése: [SQL Server Agent bővítményt az SQL Server virtuális gépek erőforrás-kezelő](../sql/virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Támogatott szolgáltatások
Az SQL Server IaaS ügynöke bővítmény a következő felügyeleti feladatokat támogatja:

| Felügyeleti szolgáltatás | Leírás |
| --- | --- |
| **SQL automatikus biztonsági mentés** |Automatizálja az ütemezés a biztonsági mentések az összes olyan adatbázis az alapértelmezett példány az SQL Server, a virtuális gépen. További információkért lásd: [automatikus biztonsági mentés az SQL Server az Azure virtuális gépek (klasszikus)](../classic/sql-automated-backup.md). |
| **SQL automatikus javítás** |Konfigurálja a karbantartási időszak során, ami fontos Windows-frissítéseket a virtuális gép akkor kerül sor, a munkaterhelés csúcsidőben frissítések elkerülése érdekében. További információkért lásd: [automatikus javítás az SQL Server az Azure virtuális gépek (klasszikus)](../classic/sql-automated-patching.md). |
| **Azure Key Vault-integráció** |Lehetővé teszi, hogy automatikusan telepítse és konfigurálja az Azure Key Vault az SQL Server virtuális gép. További információkért lásd: [konfigurálása Azure Key Vault-integráció az SQL Server Azure virtuális gépeken (klasszikus)](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Előfeltételek
A virtuális gép az SQL Server IaaS ügynöke bővítmény használatára vonatkozó követelményeket:

### <a name="operating-system"></a>Operációs rendszer:
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

### <a name="sql-server-versions"></a>SQL Server-verziók:
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:
[Töltse le és konfigurálja a legújabb Azure PowerShell-parancsok](/powershell/azure/overview).

Indítsa el a Windows Powershellt, és csatlakoztassa az Azure-előfizetése a **Add-AzureAccount** parancsot.

    Add-AzureAccount

Ha több előfizetéssel rendelkezik, **válasszon-AzureSubscription** válassza ki az előfizetést, amely tartalmazza a cél a klasszikus virtuális gép.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Ezen a ponton a klasszikus virtuális gépek és a társított szolgáltatás nevük beszerezheti a **Get-AzureVM** parancsot.

    Get-AzureVM

## <a name="installation"></a>Telepítés
Klasszikus virtuális gépek az SQL Server IaaS ügynöke bővítmény telepítése és konfigurálása annak társított szolgáltatásaihoz PowerShell kell használnia. Használja a **Set-AzureVMSqlServerExtension** PowerShell-parancsmag a bővítmény telepítéséhez. A következő parancs például a bővítmény telepítését a Windows Server virtuális gép (klasszikus), és nevét, az "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Ha az SQL-infrastruktúra-szolgáltatási ügynök bővítmény a legújabb verzióra frissíti, a bővítmény frissítése után újra kell indítania a virtuális gép.

> [!NOTE]
> Klasszikus virtuális gépek nem rendelkeznek egy lehetőség, hogy telepítse és konfigurálja az SQL IaaS-ügynök bővítmény a portálon keresztül.
> 
> 

## <a name="status"></a>status
Egy győződjön meg arról, hogy telepítve van-e a bővítmény módja az ügynök állapotát megtekintheti az Azure portálon. Válasszon ki egy virtuális gépet a virtuális gépek paneljét, szerepel, és kattintson a **bővítmények**. Megjelenik a **SQLIaaSAgent** felsorolt bővítmény.

![SQL Server IaaS-ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Használhatja a **Get-AzureVMSqlServerExtension** Azure Powershell-parancsmagot.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Eltávolítás
Az Azure portálon, eltávolíthatja a bővítményt a három pont parancsával a **bővítmények** panelen található a virtuálisgép-tulajdonságokat. Kattintson a **Eltávolítás**.

![Távolítsa el az SQL Server IaaS-ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Használhatja a **Remove-AzureVMSqlServerExtension** Powershell-parancsmagot.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>További lépések
A bővítmény által támogatott szolgáltatások használatának megkezdéséhez. A hivatkozott témakörökben talál további részleteket a [támogatott szolgáltatások](#supported-services) című szakaszát.

További információ az SQL Servert futtató Azure virtuális gépeken: [SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md).


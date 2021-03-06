---
title: A virtuális gépek előkészítése az Azure Monitor |} A Microsoft Docs
description: Ez a cikk azt ismerteti, hogyan fogunk előkészíteni és az Azure Monitor konfigurálása a virtuális gépek, így az elosztott alkalmazás hogyan működik, és milyen állapotbeli problémák azonosított ismertetése.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: 2f0568064eed556429675ffb34c84d588ac670d5
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47064356"
---
# <a name="how-to-onboard-the-azure-monitor-for-vms"></a>Hogyan üzembe helyezni az Azure figyelheti a virtuális gépek 
Ez a cikk ismerteti, hogyan állítható be az Azure Monitor az Azure-beli virtuális gépek operációs rendszer állapotának figyelésére és felderítése és képezze le az alkalmazás függőségeit, amelyek rajtuk üzemeltethető virtuális gépek számára.  

Az Azure Monitor engedélyezése a virtuális gépek az alábbi módszerek egyikének használatával történik, és az egyes módszerek használatával részletei is szerepelnek a cikk későbbi részében.  

* Kiválasztásával egy Azure virtuális gépen **Insights (előzetes verzió)** közvetlenül a virtuális gépről.
* Több Azure virtuális gépeken az Azure Policy segítségével annak biztosítása érdekében a meglévő és új virtuális gépek értékeli ki a szükséges függőségek telepítve van és megfelelően legyenek konfigurálva.  Így eldöntheti, függően mi nem megfelelőként, hogyan szeretné javítani a nem megfelelő virtuális gépek jelenti.  
* Több Azure virtuális gépek vagy a virtuális gép méretezési csoportok között egy adott előfizetésen vagy erőforráscsoport PowerShell használatával.

## <a name="prerequisites"></a>Előfeltételek
A Kezdés előtt győződjön meg arról, hogy rendelkezik-e a következő alárendelt az alábbi szakaszokban leírtak szerint.

### <a name="log-analytics"></a>Log Analytics 

Log Analytics-munkaterület az alábbi régiókban jelenleg támogatja:

  - USA nyugati középső régiója  
  - USA keleti régiója  
  - Nyugat-Európa  
  - Délkelet-Ázsia<sup>1</sup>  

<sup>1</sup> ebben a régióban jelenleg nem támogatja az Azure Monitor állapota funkcióját a virtuális gépek   

Ha nem rendelkezik egy munkaterületet, akkor is hozhat létre keresztül [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md)segítségével, [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), vagy a a [az Azure portal](../log-analytics/log-analytics-quick-create-workspace.md).  

A megoldás engedélyezéséhez, kell lennie a Log Analytics közreműködő szerepkör tagja. A Log Analytics-munkaterülethez való hozzáférésének kapcsolatos további információkért lásd: [munkaterületeinek kezeléséhez](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

### <a name="supported-operating-systems"></a>Támogatott operációs rendszerek

Az alábbi táblázat a virtuális gépek az Azure monitorban támogatott Windows- és Linux operációs rendszerek listája.  A teljes listát, részletesen ismertetve a fő- és alverzió Linux operációs rendszer kiadási és a kernel verziója támogatott alább ebben a szakaszban.

|Operációs rendszer verziója |Teljesítmény |Maps |Állapot |  
|-----------|------------|-----|-------|  
|A Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|RHEL 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |  
|Cent operációs rendszer Linux 7, 6 | X | X | X |  
|SLES 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> a teljesítmény a szolgáltatás az Azure Monitor-beli virtuális gépek csak az Azure Monitor érhető el, akkor esetén nem érhető el, akkor a bal oldali ablaktáblán, az Azure virtuális gépek közvetlenül elérhessék.  

>[!NOTE]
>Az alábbi adatokat a következőkre vonatkozik a Linux operációs rendszer támogatja:  
> - Csak az alapértelmezett és az SMP Linux kernelű kiadások támogatottak.  
> - A nem szabványos kernelű kiadások, például a PAE és a Xen nem támogatottak semmilyen Linux-disztribúció esetén. Ha például kiadási karakterlánc "2.6.16.21-0.8-xen", a rendszer nem támogatott.  
> - Az egyéni kernelek, beleértve a standard kernelek újrafordításait, nem támogatottak.  
> - A CentOSPlus kernel szintén nem támogatott.  


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Operációs rendszer verziója | Kernel verziója |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Operációs rendszer verziója | Kernel verziója |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |
| 6.9 | 2.6.32-696 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Operációs rendszer verziója | Kernel verziója |
|:--|:--|
| Ubuntu 18.04 | kernel 4.15. * |
| Ubuntu 16.04.3 | kernel 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="oracle-enterprise-linux-6-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 6 szoros vállalati kernel
| Operációs rendszer verziója | Kernel verziója
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-enterprise-linux-5-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 5 szoros vállalati kernel

| Operációs rendszer verziója | Kernel verziója
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 vállalati kiszolgáló

| Operációs rendszer verziója | Kernel verziója
|:--|:--|
|12 SP2 | 4.4. * |
|12 SP3 | 4.4. * |

### <a name="hybrid-environment-connected-sources"></a>Hibrid környezetben csatlakoztatott források
Virtuális gépek térkép az Azure Monitor az adatok lekérése a Microsoft Dependency agent. A függőségi ügynök a Log Analytics-ügynököket a Log Analytics-kapcsolat támaszkodik. Ez azt jelenti, hogy a rendszer kell rendelkeznie a Log Analytics-ügynököket telepíteni és konfigurálni a függőségi ügynök.  A következő táblázat ismerteti a térkép funkció támogatja a hibrid környezetben összekapcsolt forrásokról.

| Csatlakoztatott forrás | Támogatott | Leírás |
|:--|:--|:--|
| Windows-ügynökök | Igen | Mellett a [Log Analytics-ügynököket for Windows](../log-analytics/log-analytics-concept-hybrid.md), Windows-ügynökök a Microsoft Dependency agent szükséges. A támogatott operációsrendszer-verziók teljes listáját megtekintheti a [támogatott operációs rendszerek](#supported-operating-systems) szakaszban. |
| Linux-ügynökök | Igen | Mellett a [Linuxhoz készült Log Analytics-ügynök](../log-analytics/log-analytics-concept-hybrid.md), Linux-ügynökök a Microsoft Dependency agent szükséges. A támogatott operációsrendszer-verziók teljes listáját megtekintheti a [támogatott operációs rendszerek](#supported-operating-systems) szakaszban. |
| System Center Operations Manage felügyeleti csoport | Nem | |  

A Windows, a Microsoft Monitoring Agent (MMA) segítségével a System Center Operations Manager és a Log Analytics összegyűjtése és küldése figyelési adatok. A System Center Operations Manager és a Log Analytics biztosít különböző ki a box verziói az ügynököt. Ezek a verziók jelenthetnek a Log Analyticsnek, a System Center Operations Managernek vagy mindkettőnek.  

A Linux, a Linux-összegyűjti és figyelés a Log Analytics az adatokat küld a Log Analytics-ügynököket.   

Ha a Windows vagy Linux rendszerű számítógépek közvetlenül nem lehet csatlakozni a szolgáltatáshoz, a Log Analytics-ügynök csatlakoztatása a Log Analytics használatával az OMS-átjáró konfigurálása szeretné. Hogyan telepítheti és konfigurálhatja az OMS-átjáró további információkért lásd: [számítógépek csatlakoztatását az OMS-átjáró Internet-hozzáférés nélküli](../log-analytics/log-analytics-oms-gateway.md).  

A függőségi ügynök a következő helyről lehet letölteni.

| Fájl | Operációs rendszer | Verzió | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.1 | 55030ABF553693D8B5112569FB2F97D7C54B66E9990014FC8CC43EFB70DE56C6 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.1 | 43C75EF0D34471A0CBCE5E396FFEEF4329C9B5517266108FA5D6131A353D29FE |

## <a name="diagnostic-and-usage-data"></a>Diagnosztika és használati adatok
A Microsoft automatikusan gyűjt keresztül az Azure Monitor szolgáltatás használatának és teljesítményének adatait. A Microsoft ezeket az adatokat adja meg, és a minőségének, biztonságának és integritásának a szolgáltatás javítására használja. Adja meg a pontos és hatékony hibaelhárítási képességeket kínál, a térkép a szolgáltatás adatokat például az operációs rendszer és verzió, IP-cím, DNS-nevet és munkaállomás-neve, a szoftver konfigurációjára vonatkozó információkat tartalmaz. A Microsoft nem gyűjti a neveket, címeket és egyéb kapcsolattartási adatait.

Az adatok gyűjtésével és használatával kapcsolatos további információkért tekintse meg a [Microsoft Online Services adatvédelmi nyilatkozata](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="performance-counters-enabled"></a>A teljesítményszámlálók engedélyezve
A virtuális gépek az Azure Monitor konfigurálása a Log Analytics-munkaterületet a megoldás által használt teljesítményszámlálók adatainak összegyűjtése.  A következő táblázat felsorolja azokat az objektumokat és a megoldás által konfigurált számlálói gyűjtött minden 60 másodpercben.

### <a name="windows-performance-counters"></a>Windows-teljesítményszámlálók

|Objektumnév |Számláló neve |  
|------------|-------------|  
|Logikai lemez |% Szabad terület |  
|Logikai lemez |Átl. Lemez mp/Olvasás |  
|Logikai lemez |Átl. Lemez mp/átvitel |  
|Logikai lemez |Átl. Lemez mp/írás |  
|Logikai lemez |Bájt/mp |  
|Logikai lemez |Lemezolvasási sebesség (bájt/s) |  
|Logikai lemez |Lemezolvasások/mp |  
|Logikai lemez |Átvitel/mp |  
|Logikai lemez |Lemezírási sebesség (bájt/s) |  
|Logikai lemez |Lemezírások/mp |  
|Logikai lemez |Szabad hely MB-ban |  
|Memory (Memória) |Rendelkezésre álló memória |  
|Hálózati Adapter |Fogadott bájtok/mp |  
|Hálózati Adapter |Küldött bájtok/s |  
|Processzor |Processzoridő |  

### <a name="linux-performance-counters"></a>Linux-teljesítményszámlálók

|Objektumnév |Számláló neve |  
|------------|-------------|  
|Logikai lemez |Foglalt hely % |  
|Logikai lemez |Lemezolvasási sebesség (bájt/s) |  
|Logikai lemez |Lemezolvasások/mp |  
|Logikai lemez |Átvitel/mp |  
|Logikai lemez |Lemezírási sebesség (bájt/s) |  
|Logikai lemez |Lemezírások/mp |  
|Logikai lemez |Szabad hely MB-ban |  
|Logikai lemez |Logikai lemez bájt/mp |  
|Memory (Memória) |Rendelkezésre álló memória |  
|Network (Hálózat) |Fogadott bájtok teljes száma |  
|Network (Hálózat) |Küldött bájtok száma összesen |  
|Processzor |Processzoridő |  

## <a name="enable-from-the-azure-portal"></a>Az Azure Portalról engedélyezése
Ha engedélyezni szeretné az Azure Portalon az Azure Virtuálisgép-monitorozási, tegye a következőket:

1. Az Azure Portalon válassza ki a **virtuális gépek**. 
2. Válasszon ki egy virtuális gépet a listából. 
3. A virtuális gép lapon található a **figyelés** szakaszban jelölje be **Insights (előzetes verzió)**.
4. Az a **Insights (előzetes verzió)** lapon jelölje be **kipróbálása**.

    ![A virtuális gépek az Azure Monitor engedélyezése a virtuális gép](./media/monitoring-vminsights-onboard/enable-vminsights-vm-portal-01.png)

5. Az a **Azure Monitor Insights felvételi** lapon, ha rendelkezik egy meglévő Log Analytics munkaterület ugyanabban az előfizetésben, válassza a legördülő listában.  A lista preselects az alapértelmezett munkaterületre és a helyre, amely a virtuális gép telepítve van az előfizetésben. 

    >[!NOTE]
    >Ha azt szeretné, a virtuális gépről a monitorozási adatok tárolására szolgáló új Log Analytics-munkaterület létrehozásához kövesse a [hozzon létre egy Log Analytics-munkaterület](../log-analytics/log-analytics-quick-create-workspace.md) a támogatott régiók egyikében korábban felsorolt.   

Miután engedélyezte a figyelés, a virtuális gép mérőszámok megtekintéséhez nagyjából 10 percet igénybe vehet. 

![Az Azure Monitor engedélyezése a virtuális gépek figyelése a központi telepítés feldolgozása](./media/monitoring-vminsights-onboard/onboard-vminsights-vm-portal-status.png)

## <a name="enable-using-azure-policy"></a>Engedélyezze az Azure Policy használata
A megoldás több Azure virtuális gép, amely biztosítja az egységes megfelelőségi és az új virtuális gépek kiépítése, automatikus engedélyezést engedélyezéséhez [Azure Policy](../azure-policy/azure-policy-introduction.md) ajánlott.  A következő előnyöket az Azure Policy használata a megadott házirendek biztosít az új virtuális gépek:

* Az Azure Monitor engedélyezése a virtuális gépek minden egyes virtuális gépéhez a meghatározott hatóköre
* Log Analytics-ügynök telepítése 
* Az alkalmazásfüggőségek felderítése, és jelenítse meg a térkép a függőségi ügynök telepítése
* Ha az Azure virtuális gép operációsrendszer-lemezkép a szabályzatdefinícióban egy előre meghatározott listában naplózása  
* Ha bejelentkezik az Azure virtuális gép megadott egyikéből munkaterület naplózása
* Jelentés a megfelelőségi eredmények 
* Szervizelési nem kompatibilis virtuális gépek támogatásához

A bérlő számára a szolgáltatás aktiválásához az ehhez a folyamathoz szükséges:

- A felsorolt lépéseket itt használatával Log Analytics-munkaterület konfigurálása
- Importálja a kezdeményezési definíciót a bérlőhöz (a felügyeleti csoport vagy az előfizetés szintjén)
- Hozzárendeli a szabályzatot a kívánt hatókörhöz
- Tekintse át a megfelelőségi eredményeit

### <a name="add-the-policies-and-initiative-to-your-subscription"></a>A házirendek és a kezdeményező hozzáadása az előfizetéshez
A szabályzatok használatához használhatja egy megadott PowerShell-szkript – [Add-VMInsightsPolicy.ps1](https://www.powershellgallery.com/packages/Add-VMInsightsPolicy/1.2) folytatva befejezheti a feladatot az Azure PowerShell-galériából érhető el. A parancsfájl hozzáad a házirendek és a egy kezdeményezés az előfizetéshez.  A következő lépésekkel konfigurálhatja az Azure Policy az előfizetésében. 

1. PowerShell-szkript letöltése a helyi fájlrendszerbe.

2. A mappában a következő PowerShell-parancs használatával adja hozzá a szabályzatok. A szkript az alábbi választható paramétereket támogatja: 

    ```powershell
    -UseLocalPolicies [<SwitchParameter>]
      <Optional> Load the policies from a local folder instead of https://raw.githubusercontent.com/dougbrad/OnBoardVMInsights/Policy/Policy/

    -SubscriptionId <String>
      <Optional> SubscriptionId to add the Policies/Initiatives to
    -ManagementGroupId <String>
      <Optional> Management Group Id to add the Policies/Initiatives to

    -Approve [<SwitchParameter>]
      <Optional> Gives the approval to add the Policies/Initiatives without any prompt
    ```  

    >[!NOTE]
    >Megjegyzés: Ha azt tervezi, a kezdeményezés szabályzat hozzárendelése több előfizetést, a definíciók kell tárolni a felügyeleti csoportban, amely tartalmazza a rendszer hozzárendeli a szabályzatot az előfizetéseket. Ezért a - ManagementGroupID paramétert kell használnia.
    >
   
    Paraméterek nélkül. példa:  `.\Add-VMInsightsPolicy.ps1`

### <a name="create-a-policy-assignment"></a>Szabályzat-hozzárendelés létrehozása
Futtatása után a `Add-VMInsightsPolicy.ps1` PowerShell-parancsfájl, a következő kezdeményezésére és a szabályzatok hozzáadásakor:

* **Log Analytics-ügynököket Windows virtuális gépek – előzetes verzió telepítése**
* **Linux rendszerű virtuális gépek – előzetes verzió Log Analytics-ügynök telepítése**
* **A függőségi ügynököt Windows virtuális gépek – előzetes verzió telepítése**
* **Függőségi ügynök Linux rendszerű virtuális gépek – előzetes verzió telepítése**
* **Audit Log Analytics ügynök üzembe helyezés – virtuális gép rendszerkép (OS) listán nem szereplő – előzetes verzió**
* **Naplózási függőségi ügynök üzembe helyezés – virtuális gép rendszerkép (OS) listán nem szereplő – előzetes verzió**

A következő kezdeményezésparaméter hozzáadása megtörténik:

- **Bejelentkezés Analytice munkaterület** (meg kell adnia az erőforrás-azonosító, a munkaterület, ha alkalmazása egy hozzárendelést a PowerShell vagy parancssori felület használatával)

    Virtuális gép található, nem megfelelő, a naplózási házirend **virtuális gépek nem az operációs rendszer hatókör...**  a feltételek a központi telepítési házirend csak a jól ismert Azure-beli Virtuálisgép-rendszerképeket üzembe helyezett virtuális gépeket tartalmazza. Ellenőrizze a dokumentációban, ha a virtuális gép operációs rendszerének vagy nem támogatott.  Ha nem érhető el, majd kell ismétlődő a központi telepítésre vonatkozó házirendet és frissítési/módosíthatja azt, hogy a kép legyen hatókörben.

A következő opcionális önálló-házirend hozzáadása:

- **Virtuális gép konfigurálva van a nem egyező Log Analytics-munkaterület - előzetes verzió**

    A már konfigurált virtuális gépek azonosításához használható a [Virtuálisgép-bővítménnyel](../virtual-machines/extensions/oms-windows.md), azonban egy másik munkaterülettel vannak konfigurálva, mint (a szabályzat-hozzárendelés szerint). Ez a munkaterület azonosítója paraméter szükséges időt.

A jelen kezdeti kiadás csak az Azure Portalon hozhat létre a szabályzat-hozzárendelés. Megtudhatja, hogyan hajtsa végre ezeket a lépéseket, tekintse meg [szabályzat-hozzárendelés létrehozása az Azure Portalról](../azure-policy/assign-policy-definition.md).

## <a name="enable-with-powershell"></a>Engedélyezze a PowerShell-lel
Engedélyezze az Azure Monitor-beli virtuális gépek több virtuális gép vagy Virtuálisgép-méretezési csoportokban, használhatja egy megadott PowerShell-szkript – [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0) folytatva befejezheti a feladatot az Azure PowerShell-galériából érhető el.  Ez a szkript algoritmusa minden virtuális gép és a virtuális gép méretezési csoportot az előfizetésében, a megadott hatókörön belüli erőforráscsoportban *ResourceGroup*, vagy egy egyetlen virtuális gép vagy a méretezési csoport által megadott *neve*.  Minden virtuális gép vagy a virtuális gép méretezési csoportot a parancsfájl ellenőrzi, ha a Virtuálisgép-bővítmény már telepítve van, és ha nem, telepítse újra a kísérletet.  Ellenkező esetben halad a Log Analytics és a függőségi ügynök Virtuálisgép-bővítmények telepítése.   

A szkriptnek szüksége van az Azure PowerShell 5.7.0 modul verzió vagy újabb. A verzió azonosításához futtassa a következőt: `Get-Module -ListAvailable AzureRM`. Ha frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ismertető cikket. Ha helyileg futtatja a PowerShellt, akkor emellett a `Connect-AzureRmAccount` futtatásával kapcsolatot kell teremtenie az Azure-ral.

A parancsfájllal kapcsolatos segítséget szeretne kérni, futtathatja `Get-Help` argumentum részletes és gyakorlati példa listáját.   

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and Dependency Agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VM's and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VM's/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VM's are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong to

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VM's to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VM's/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Suported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VM's in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to ReInstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to ReInstall (move to a new workspace)
```

A következő példa bemutatja a mappában található a PowerShell-parancsok segítségével engedélyezése az Azure Monitor-beli virtuális gépek és a várt kimeneti megismerése:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VM's or VM ScaleSets matching criteria specified

VM's or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency Agent extensions on above 2 VM's or VM Scale Sets.
VM's in a non-running state will be skipped.
Extension will not be re-installed if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="enable-for-hybrid-environment"></a>A hibrid környezet engedélyezése
Ez a szakasz ismerteti, hogyan előkészítheti a virtuális gépeket vagy fizikai számítógépek üzemeltetett az adatközpontban vagy egyéb felhőkörnyezet által az Azure Monitor-beli virtuális gépek figyelésére.  

Az Azure Monitor, virtuális gépek térkép függőségi ügynök nem továbbít adatokat magát, és nem igényel tűzfalak és a portok módosítása. Az adatok térképen mindig továbbítása a Log Analytics-ügynököket, az Azure Monitor szolgáltatásba, vagy közvetlenül vagy keresztül a [OMS-átjáró](../log-analytics/log-analytics-oms-gateway.md) Ha az informatikai biztonsági szabályzatok nem engedélyezik a számítógépeken a hálózat csatlakozik az internethez.

Tekintse át a követelményeket és a központi telepítési módszerek a [Log Analytics Linux és Windows-ügynök](../log-analytics/log-analytics-concept-hybrid.md).

Összesített lépéseket:

1. Log Analytics-ügynököket telepíteni Windows vagy Linux
2. Az Azure Monitor telepítése a virtuális gépek térkép függőségi ügynök
3. Teljesítményszámlálók gyűjtésének engedélyezéséhez
4. A virtuális gépek előkészítése az Azure Monitor

### <a name="install-the-dependency-agent-on-windows"></a>Telepítse a függőségi ügynököt Windows 
A függőségi ügynök telepítheti manuálisan a Windows-számítógépeken futó `InstallDependencyAgent-Windows.exe`. Ha a végrehajtható fájl kapcsolók nélkül futtatja, a telepítővarázsló, amelyek telepítéséhez az interaktív módon elindítja.  

>[!NOTE]
>Az ügynök telepítéséhez vagy eltávolításához rendszergazdai jogosultság szükséges.

Az alábbi táblázat a parancssorból az ügynök a telepítő által támogatott konkrét paraméterei emeli ki.  

| Paraméter | Leírás |
|:--|:--|
| /? | A parancssori kapcsolók listáját adja vissza. |
| /S | Felhasználói beavatkozás nélküli telepítés végrehajtásához. |

Ha például az a telepítőprogram futtatásához a `/?` írja be a paraméter `InstallDependencyAgent-Windows.exe /?`

A Windows a függőségi ügynök fájlok vannak telepítve a `C:\Program Files\Microsoft Dependency Agent` alapértelmezés szerint.  Ha a függőségi ügynök nem a telepítés befejezése után indul el, ellenőrizze a hibával kapcsolatos részletes információk a naplókat. A naplózási könyvtár `%Programfiles%\Microsoft Dependency Agent\logs`. 

### <a name="install-the-dependency-agent-on-linux"></a>A függőségi ügynök telepítése Linux rendszeren
A függőségi ügynök telepítve van a Linux-kiszolgálókon `InstallDependencyAgent-Linux64.bin`, azt a héjparancsfájlt, egy önkicsomagoló bináris. A fájl használatával futtathatja `sh` vagy adjon hozzá végrehajtási engedélyeket magát a fájlt.

>[!NOTE]
> Az ügynök telepítéséhez vagy eltávolításához gyökérszintű hozzáférés szükséges.
> 

| Paraméter | Leírás |
|:--|:--|
| – Súgó | A parancssori kapcsolók listájának lekérése. |
| -s | Beavatkozás nélküli telepítés a felhasználónak szóló üzenetek nélkül. |
| --ellenőrzése | Az engedélyek és az operációs rendszer ellenőrzése, az ügynök telepítése nélkül. |

Ha például az a telepítőprogram futtatása a `-help` paraméterként `InstallDependencyAgent-Linux64.bin -help`.

A Linux függőségi ügynök telepítése a rendszergazdaként a következő parancs futtatásával `sh InstallDependencyAgent-Linux64.bin`
    
Ha a függőségi ügynök nem indul el, ellenőrizze a hibával kapcsolatos részletes információk a naplókat. A Linux-ügynökök, a naplózási könyvtár van `/var/opt/microsoft/dependency-agent/log`.

A függőségi ügynök fájlok kerülnek a következő könyvtárak:

| Fájlok | Hely |
|:--|:--|
| Alapvető fájlok | /opt/microsoft/dependency-agent |
| Naplófájlok | /var/opt/microsoft/dependency-agent/log |
| Konfigurációs fájlok | /etc/opt/microsoft/dependency-agent/config |
| Szolgáltatás végrehajtható fájljai | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Bináris tárolófájlok | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Teljesítményszámlálók engedélyezése
Ha a Log Analytics-munkaterületet a megoldás által hivatkozott már összegyűjtéséhez a teljesítményszámlálókat, a megoldás által igényelt nincs konfigurálva, akkor kell engedélyezni kell. Ez manuálisan leírt módon valósítható [Itt](../log-analytics/log-analytics-data-sources-performance-counters.md), vagy pedig letöltésével és futtatásával egy PowerShell-parancsprogram elérhető [Azure Powershell-galériából](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1).
 
### <a name="onboard-azure-monitor-for-vms"></a>A virtuális gépek előkészítése az Azure Monitor
Ez a módszer egy JSON-sablon ahhoz, hogy a megoldás-összetevőket a Log Analytics-munkaterület konfigurációját tartalmazza.  

Ha ismeri a sablon segítségével üzembe helyezni erőforrásokat fogalmát, lásd:
* [Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure PowerShell-lel](../azure-resource-manager/resource-group-template-deploy.md)
* [Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) 

Ha az Azure CLI-vel, akkor először helyi telepítése és használata a parancssori felület. Kell futnia az Azure CLI 2.0.27-es vagy újabb. A verzió azonosításához futtassa `az --version`. Ha telepíteni vagy frissíteni szeretné az Azure CLI, lásd: kell [az Azure CLI telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Hozzon létre, és hajtsa végre a sablon

1. Másolja és illessze be a következő JSON-szintaxist a létrehozott fájlba:

    ```json
    {

    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "WorkspaceName": {
            "type": "string"
        },
        "WorkspaceLocation": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-15-preview",
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('WorkspaceName')]",
            "location": "[parameters('WorkspaceLocation')]",
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },

                    "plan": {
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                        "promotionCode": ""
                    }
                },
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },
                    "plan": {
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                        "promotionCode": ""
                    }
                }
            ]
        }
    ]
    ```

2. Mentse a fájlt **installsolutionsforvminsights.json** egy helyi mappába.
3. Értékeinek szerkesztéséhez **WorkspaceName**, **ResourceGroupName**, és **WorkspaceLocation**.  Értéke **WorkspaceName** van a Log Analytics-munkaterület, amely magában foglalja a munkaterület neve és értéke a teljes erőforrás-Azonosítójára van **WorkspaceLocation** a régió, a munkaterület van definiálva. a.
4. Készen áll a sablon a következő PowerShell-paranccsal üzembe helyezésére:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    A konfiguráció módosításának befejezése néhány percet is igénybe vehet. Ha elkészült, megjelenik egy üzenet, amely a következő példához hasonló, és az eredmény tartalmazza:

    ```powershell
    provisioningState       : Succeeded
    ```
Miután engedélyezte a figyelés, állapota és a hibrid számítógép mérőszámok megtekintéséhez nagyjából 10 percet vehet igénybe. 

## <a name="next-steps"></a>További lépések

A figyelés engedélyezve van a virtuális gép, ezt az információt és az Azure Monitor-beli virtuális gépek elemzési érhető el.  Az állapotfigyelő szolgáltatás használatával kapcsolatban lásd: [a virtuális gépek állapotának megtekintése az Azure Monitor](monitoring-vminsights-health.md), vagy a felderített alkalmazások függőségeinek megtekintése: [megtekintése az Azure Monitor virtuális gépeket a térképen](monitoring-vminsights-maps.md).  
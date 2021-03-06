---
title: Hálózati biztonsági csoport Eseménynapló és a szabály az Azure diagnosztikai naplók számláló |} A Microsoft Docs
description: Ismerje meg, hogyan esemény- és a szabály a számláló diagnosztikai naplók engedélyezése Azure-beli hálózati biztonsági csoport számára.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2018
ms.author: jdial
ms.openlocfilehash: 5635998eb72f08ddc665793e77008890b2cdb05d
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/20/2018
ms.locfileid: "42060843"
---
# <a name="diagnostic-logging-for-a-network-security-group"></a>A hálózati biztonsági csoport diagnosztikai naplózás

Hálózati biztonsági csoport (NSG) tartalmazza a szabályokat, amelyek engedélyezik vagy megtagadják a forgalmat a virtuális hálózat alhálózatán, a hálózati adapter vagy mindkettőt. Ha engedélyezte a diagnosztikai naplózás az NSG-t, bejelentkezhet a következő kategóriákba tartozó információkat:

* **Esemény:** kerülnek naplózásra bejegyzések melyik NSG-t a virtuális gépek, a MAC-cím alapján szabályok érvényesek. Ezek a szabályok állapota gyűjtött minden 60 másodpercben.
* **A szabály a számláló:** hány alkalommal minden NSG tartalmaz bejegyzést a alkalmaznak-forgalom engedélyezése vagy megtagadása szabály.

Diagnosztikai naplók az Azure Resource Manager-alapú üzemi modellel üzembe helyezett hálózati biztonsági csoportok csak érhetők el. A klasszikus üzemi modellel üzembe helyezett hálózati biztonsági csoportok diagnosztikai naplózás nem engedélyezhető. Szeretné jobban megismerni a két modell, lásd: [ismertetése Azure üzemi modelljeinek](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Diagnosztikai naplózás külön-külön engedélyezve van az *egyes* NSG-t szeretne gyűjteni a diagnosztikai adatokat. Ha érdekli az operatív, vagy inkább naplózza a tevékenységét, tekintse meg az Azure [naplózása](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enable-logging"></a>Naplózás engedélyezése

Használhatja a [az Azure Portal](#azure-portal), [PowerShell](#powershell), vagy a [Azure CLI-vel](#azure-cli) a diagnosztikai naplózás engedélyezése.

### <a name="azure-portal"></a>Azure Portal

1. Jelentkezzen be a [portálra](https://portal.azure.com).
2. Válassza ki **minden szolgáltatás**, majd írja be a *hálózati biztonsági csoportok*. Amikor **hálózati biztonsági csoportok** jelennek meg a keresési eredmények közül válassza ki azt.
3. Válassza ki a kívánt naplózásának engedélyezése az NSG.
4. Alatt **figyelés**válassza **diagnosztikai naplók**, majd válassza ki **diagnosztika bekapcsolása**, ahogy az alábbi képen is látható:
 
    ![Diagnosztika bekapcsolása](./media/virtual-network-nsg-manage-log/turn-on-diagnostics.png)

5. A **diagnosztikai beállítások**, adja meg, vagy válassza ki a következő adatokat, és válassza **mentése**:

    | Beállítás                                                                                     | Érték                                                          |
    | ---------                                                                                   |---------                                                       |
    | Name (Név)                                                                                        | Egy tetszőleges nevet.  Például: *myNsgDiagnostics*      |
    | **Archiválás tárfiókba**, **egy eseményközpontba Stream**, és **Küldés a Log Analyticsnek** | Kiválaszthatja, hogy annyi célok kiválasztása közben. Minden egyes kapcsolatos további információkért lásd: [destinations jelentkezzen](#log-destinations).                                                                                                                                           |
    | NAPLÓ                                                                                         | Válassza ki az egyik vagy mindkét naplókategóriák. Az egyes kategóriákhoz tartozó naplózott adatok kapcsolatos további információkért lásd: [kategóriába jelentkezzen](#log-categories).                                                                                                                                             |
6. Naplók megtekintése és elemzése. További információkért lásd: [megtekintése és -naplók elemzése](#view-and-analyze-logs).

### <a name="powershell"></a>PowerShell

A következő parancsokat futtathat a [Azure Cloud Shell](https://shell.azure.com/powershell), vagy a számítógépről futtatja a Powershellt. Az Azure Cloud Shell olyan ingyenes interaktív kezelőfelület. A fiókjával való használat érdekében a gyakran használt Azure-eszközök már előre telepítve és konfigurálva vannak rajta. Ha futtatja a PowerShell a számítógépről, akkor a *AzureRM* PowerShell-modult, 6.1.1 verzió vagy újabb. Futtatás `Get-Module -ListAvailable AzureRM` a számítógépen, a telepített verzió azonosításához. Ha frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](/powershell/azure/install-azurerm-ps) ismertető cikket. Ha futtatja a Powershellt helyileg, is futtatni szeretné `Login-AzureRmAccount` bejelentkezni az Azure-fiókkal, amely rendelkezik a [szükséges engedélyek](virtual-network-network-interface.md#permissions)].

Diagnosztikai naplózás engedélyezése kell egy létező NSG azonosítója. Ha nem rendelkezik egy létező NSG-t, létrehozhat egyet a [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).

Engedélyezni szeretné a diagnosztikai naplózás a hálózati biztonsági csoport lekérése [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup). Például egy NSG-t lekéréséhez nevű *myNsg* nevű erőforráscsoportot, amely létezik *myResourceGroup*, adja meg a következő parancsot:

```azurepowershell-interactive
$Nsg=Get-AzureRmNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

Diagnosztikai naplók háromféle cél írhatja. További információkért lásd: [destinations jelentkezzen](#log-destinations). Ebben a cikkben naplókat küld a *Log Analytics* cél példaként. Egy meglévő Log Analytics-munkaterülethez való lekéréséhez [Get-azurermoperationalinsightsworkspace parancsmagok](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace). Például egy meglévő munkaterületet lekéréséhez nevű *myWorkspace* nevű erőforráscsoportból *myWorkspaces*, adja meg a következő parancsot:

```azurepowershell-interactive
$Oms=Get-AzureRmOperationalInsightsWorkspace `
  -ResourceGroupName myWorkspaces `
  -Name myWorkspace
```

Ha nem rendelkezik egy meglévő munkaterületet, létrehozhat egyet a [New-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace).

Két kategóriába sorolhatók a naplózási engedélyezheti a naplókat. További információkért lásd: [kategóriába jelentkezzen](#log-categories). Az NSG-t a diagnosztikai naplózás engedélyezése [Set-azurermdiagnosticsetting parancshoz](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting). Az alábbi példában a munkaterület esemény- és számláló kategória-adatokat egy NSG-t az azonosítók használata az NSG-t és a munkaterület korábban lekért jelentkezik:

```azurepowershell-interactive
Set-AzureRmDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

Ha csak egy kategóriát vagy a másik, nem pedig mind az adatok naplózása, vegye fel a `-Categories` lehetőséget az előző parancsot, utána pedig *NetworkSecurityGroupEvent* vagy *NetworkSecurityGroupRuleCounter*. Ha szeretne bejelentkezni egy másik [cél](#log-destinations) , mint a Log Analytics-munkaterületet, használja a megfelelő paramétereket az Azure-beli [tárfiók](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Eseményközpont](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Naplók megtekintése és elemzése. További információkért lásd: [megtekintése és -naplók elemzése](#view-and-analyze-logs).

### <a name="azure-cli"></a>Azure CLI

A következő parancsokat futtathat a [Azure Cloud Shell](https://shell.azure.com/bash), vagy futtatja az Azure CLI a számítógépről. Az Azure Cloud Shell olyan ingyenes interaktív kezelőfelület. A fiókjával való használat érdekében a gyakran használt Azure-eszközök már előre telepítve és konfigurálva vannak rajta. A számítógépről futtatja a parancssori Felületet, ha szüksége van-e 2.0.38 verzió vagy újabb. Futtatás `az --version` a számítógépen, a telepített verzió azonosításához. Ha frissíteni szeretne, olvassa el [Azure CLI telepítése](/cli/azure/install-azure-cli?view=azure-cli-latest). Ha helyileg futtatja a parancssori felület, is futtatni szeretné `az login` bejelentkezni az Azure-fiókkal, amely rendelkezik a [szükséges engedélyek](virtual-network-network-interface.md#permissions).

Diagnosztikai naplózás engedélyezése kell egy létező NSG azonosítója. Ha nem rendelkezik egy létező NSG-t, létrehozhat egyet a [az network nsg létrehozása](/cli/azure/network/nsg#az-network-nsg-create).

Engedélyezni szeretné a diagnosztikai naplózás a hálózati biztonsági csoport lekérése [az network nsg show](/cli/azure/network/nsg#az-network-nsg-show). Például egy NSG-t lekéréséhez nevű *myNsg* nevű erőforráscsoportot, amely létezik *myResourceGroup*, adja meg a következő parancsot:

```azurecli-interactive
nsgId=$(az network nsg show \
  --name myNsg \
  --resource-group myResourceGroup \
  --query id \
  --output tsv)
```

Diagnosztikai naplók háromféle cél írhatja. További információkért lásd: [destinations jelentkezzen](#log-destinations). Ebben a cikkben naplókat küld a *Log Analytics* cél példaként. További információkért lásd: [kategóriába jelentkezzen](#log-categories). 

Az NSG-t a diagnosztikai naplózás engedélyezése [az monitor diagnostic-settings létrehozása](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create). Az alábbi példa esemény- és számláló kategória adatokat naplózza nevű meglévő munkaterülethez *myWorkspace*, amely létezik a nevű erőforráscsoport *myWorkspaces*, és az NSG-t lekért azonosítója korábban:

```azurecli-interactive
az monitor diagnostic-settings create \
  --name myNsgDiagnostics \
  --resource $nsgId \
  --logs '[ { "category": "NetworkSecurityGroupEvent", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "NetworkSecurityGroupRuleCounter", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --workspace myWorkspace \
  --resource-group myWorkspaces
```

Ha nem rendelkezik egy meglévő munkaterületet, is létrehozhat egyet a [az Azure portal](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [PowerShell](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace). Két kategóriába sorolhatók a naplózási engedélyezheti a naplókat. 

Ha csak egy kategória vagy egyéb adatok, távolítsa el a kategória nem szeretne adatokat jelentkezzen be az előző parancs. Ha szeretne bejelentkezni egy másik [cél](#log-destinations) , mint a Log Analytics-munkaterületet, használja a megfelelő paramétereket az Azure-beli [tárfiók](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Eseményközpont](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Naplók megtekintése és elemzése. További információkért lásd: [megtekintése és -naplók elemzése](#view-and-analyze-logs).

## <a name="log-destinations"></a>Napló célhelyek

Diagnosztikai adatok lehetnek:
- [Azure Storage-fiókot írt](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), a naplózási és manuális ellenőrzést. Megadhatja, hogy a megőrzési időtartam (napban) használata az erőforrás diagnosztikai beállításait.
- [Az Eseményközpontok felé, folyamatos](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) egy külső szolgáltatás, vagy egyéni elemzési megoldás, mint a Power bi támogatunk.
- [Az Azure Log Analytics írt](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-diagnostics-direct-to-log-analytics).

## <a name="log-categories"></a>Naplókategóriák

Az alábbi naplózási kategóriákban írja JSON-formátumú adatokat:

### <a name="event"></a>Esemény

Az Eseménynapló tartalmaz információt arról, hogy mely virtuális gépek, a MAC-cím alapján az NSG-szabályok érvényesek. A következő adatokat naplózza az egyes eseményekhez. A következő példában a rendszer adatokat naplózza a virtuális gép 192.168.1.4 IP-cím és MAC-címét, 00-0D-3A-92-6A-7C:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "priority":[PRIORITY-SPECIFIED-IN-RULE],
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "conditions":{
            "protocols":"[PROTOCOLS-SPECIFIED-IN-RULE]",
            "destinationPortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourcePortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourceIP":"[SOURCE-IP-OR-RANGE-SPECIFIED-IN-RULE]",
            "destinationIP":"[DESTINATION-IP-OR-RANGE-SPECIFIED-IN-RULE]"
            }
        }
}
```

### <a name="rule-counter"></a>A szabály a számláló

A szabály számlálót minden egyes szabály alkalmazása az erőforrásokra vonatkozó információkat tartalmaz. A következő példa adatokat a rendszer naplózza minden alkalommal, amikor egy szabály alkalmazásakor. A következő példában a rendszer adatokat naplózza a virtuális gép 192.168.1.4 IP-cím és MAC-címét, 00-0D-3A-92-6A-7C:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> A forrás IP-cím a kommunikáció a program nem naplózza. Engedélyezheti a [NSG csoportforgalom naplózása](../network-watcher/network-watcher-nsg-flow-logging-portal.md) az NSG-t azonban, amely naplózza az összes, a szabály teljesítményszámláló adatai, valamint a forrás IP-cím által kezdeményezett kommunikáció. A rendszer az NSG-folyamatnapló adatait egy Azure Storage-fiókba fogja írni. Elemezheti az adatokat a a [traffic analytics](../network-watcher/traffic-analytics.md) Azure Network Watcher képességét.

## <a name="view-and-analyze-logs"></a>Naplók megtekintése és elemzése

Diagnosztikai naplóadatokat megtekintése kapcsolatban lásd: [Azure diagnosztikai naplók áttekintése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Ha diagnosztikai adatokat küld:
- **Log Analytics**: használhatja a [hálózati biztonsági csoport elemzési](../log-analytics/log-analytics-azure-networking-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-log-analytics
) megoldást kínál a bővített lehetőségeket. A megoldás biztosítja a Vizualizációk az NSG-szabályok, amelyek engedélyezik vagy megtagadják a forgalmat, egy MAC-címét, a hálózati adaptert egy virtuális gépen.
- **Az Azure Storage-fiók**: adatok írása a PT1H.json fájlt. Annak a:
    - Eseménynapló a következő elérési úton: `insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
    - A szabály a számláló napló a következő elérési úton: `insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>További lépések

- Tudjon meg többet [naplózása](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), aggregated, korábban naplózási vagy a műveleti naplókban. Tevékenység naplózása alapértelmezés szerint engedélyezve van a hálózati biztonsági csoportok vagy Azure üzemi modellel lett létrehozva. Annak megállapításához, hogy mely műveletek a tevékenységnaplóban az NSG-k befejeződött, keresse meg a következő erőforrástípusok tartalmazó bejegyzéseket:
    - Microsoft.ClassicNetwork/networkSecurityGroups
    - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
    - Microsoft.Network/networkSecurityGroups
    - Microsoft.Network/networkSecurityGroups/securityRules
- További diagnosztikai adatokat, a forrás IP-cím az egyes folyamatok bejelentkezéshez: [NSG csoportforgalom naplózása](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

---
title: Nézet létrehozása és kezelése az Azure Monitor tevékenységnapló-riasztások
description: Az Azure Portal, az erőforrás-sablon és a PowerShell a tevékenységnapló-riasztások létrehozásának módjáról.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 526c50fa4d261a30738c3f24d537fe5e0d765f6d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951304"
---
# <a name="create-view-and-manage-activity-log-alerts-using-azure-monitor"></a>Létrehozása, megtekintése és használata az Azure Monitor tevékenységnapló-riasztások kezelése  

## <a name="overview"></a>Áttekintés
A tevékenységnapló-riasztások a riasztásokat, amelyek aktiválni, ha egy új tevékenységnapló eseményéhez történik, amely megfelel a feltételeknek, a riasztásban feltüntetett.

Ezek a riasztások az Azure-erőforrásokhoz, van egy Azure Resource Manager-sablon segítségével hozható létre. Akkor is képes lehet létrehozott, frissített vagy törölt az Azure Portalon. Általában akkor tevékenységnapló-riasztások létrehozása, hogy értesítést kapjon, ha adott változás történik az Azure-előfizetésében, gyakran hatóköre az adott erőforrás-csoportok vagy az erőforrás-erőforrások. Például érdemes lehet értesítést kap minden erőforráscsoportban (minta) virtuális gép **myProductionResourceGroup** törlik, vagy előfordulhat, hogy szeretne kaphat, ha egyetlen új szerepkör hozzá van rendelve egy felhasználó az előfizetésében.

> [!IMPORTANT]
> Riasztások a szolgáltatás állapotával kapcsolatos értesítés nem hozható létre a tevékenység log riasztás létrehozása a felületen keresztül. Című cikk nyújt tájékoztatást, létrehozásával és használatával a szolgáltatás állapotával kapcsolatos értesítésekre, [tevékenységnapló-riasztások a szolgáltatás állapotára vonatkozó értesítések fogadásához](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="manage-alert-rules-for-activity-log-using-azure-portal"></a>Az Azure portal használatával tevékenységnapló riasztási szabályok kezelése

> [!NOTE]

>  A riasztási szabályok létrehozásakor a következőket biztosítja:

> - A hatókör előfizetés különbözik nem az előfizetés ahol a riasztás létrejött.
- Feltételek kell lennie a szint/állapot/hívó / erőforráscsoport vagy erőforrás-azonosító / erőforrás típusa / az eseménykategóriát, amelyen a riasztás úgy van konfigurálva.
- Nem "anyOf" feltétel vagy a beágyazott feltételeknek a riasztás konfigurálásakor JSON (alapvetően csak egy allOf esetén megengedett semmilyen további allOf anyOf).
- Ha a kategória: "rendszergazda". A fenti feltételek legalább egyikének a riasztásban szereplő adjon meg. Nem hozható létre egy riasztást, amely minden alkalommal, amikor létrejön egy esemény a Tevékenységnaplókban aktiválja.

### <a name="create-an-alert-rule-for-an-activity-log-using-azure-portal"></a>Az Azure portal használatával egy tevékenységnapló-riasztási szabály létrehozása

Kövesse az alábbi eljárást:

1. Az Azure Portalon válassza ki a **figyelő** > **riasztások**
2. Kattintson a **Új riasztási szabály** felső részén a **riasztások** ablak.

     ![Új riasztási szabály](./media/monitor-alerts-unified/AlertsPreviewOption.png)

     A **létrehozás szabály** ablak jelenik meg.

      ![Új riasztási szabály-beállítások](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule-options.png)

3. **Adja meg a riasztási feltétel alapján** adja meg a következő adatokat, és kattintson a **kész**.

    - **Riasztási cél:** megtekintheti, és jelölje ki a cél az új riasztás, használja a **előfizetés-szűrő** / **szűrés erőforrástípus szerint** , és válassza ki az erőforrás vagy erőforráscsoport a a lista jelenik meg.

    > [!NOTE]

    > kiválaszthatja, hogy egy erőforrás, erőforráscsoport vagy egy tevékenységnaplóbeli a teljes előfizetés.

    **Riasztás cél minta nézet** ![cél kiválasztása](./media/monitoring-activity-log-alerts-new-experience/select-target.png)

    - A **cél feltételek**, kattintson a **adja meg a feltételeket** és a cél az összes rendelkezésre álló jelek jelennek meg, beleértve azokat a különböző kategóriák **tevékenységnapló**; eszközkategória-név hozzáfűzi a **Monitor Service** nevét.

    - A jel kijelölhet a listában megjelenik a különböző típusú lehetséges műveletek **tevékenységnapló**.

    A naplózási előzmények ütemterv és a cél jel a megfelelő riasztási logika választhatja ki:

    **Feltételek képernyő hozzáadása**

    ![feltételek hozzáadása](./media/monitoring-activity-log-alerts-new-experience/add-criteria.png)

    **Előzmények idő**: elérhető a kiválasztott művelet események algoritmus keresztül az elmúlt 6 és 12 vagy 24 óra (vagy) az elmúlt héten.

    **Riasztási logika**:

     - **Eseményszint**-esemény súlyossági szintje. _Részletes_, _tájékoztató_, _figyelmeztetés_, _hiba_, vagy _kritikus_.
     - **Állapot**: az esemény állapota. _Lépések_, _sikertelen_, vagy _sikeres_.
     - **Esemény kezdeményezője**: a hívó; néven is ismert Az e-mail címet vagy az Azure Active Directory a műveletet végrehajtó felhasználó azonosítója.

        Minta jel gráf riasztási logika a alkalmazni:

        ![ kijelölt feltétel](./media/monitoring-activity-log-alerts-new-experience/criteria-selected.png)

4. A **riasztási szabályok részletei**, adja meg a következő adatokat:

    - **Riasztási szabály neve** – az Új riasztási szabály neve
    - **Leírás** – az Új riasztási szabály leírása
    - **A riasztás mentése erőforráscsoportba** – válassza ki az erőforráscsoport, amelyben az új szabály menteni szeretné.

5. A **műveletcsoport**, a legördülő menüben adja meg a műveletcsoport, amelyet szeretne hozzárendelni az Új riasztási szabály. Másik lehetőségként [hozzon létre egy új műveletcsoportot](monitoring-action-groups.md) , és rendelje az új szabályt. Új csoport létrehozásához kattintson a **+ új csoport**.

6. A szabályok létrehozása után engedélyezéséhez kattintson **Igen** a **engedélyezése a szabály létrehozásakor** lehetőséget.
7. Kattintson a **riasztási szabály létrehozása**.

    A tevékenységnapló Új riasztási szabály jön létre, és tetején megjelenik egy megerősítő üzenetet, az ablak jobb.

    Engedélyezi, tiltsa le, szerkesztése vagy egy szabály törlése. [További](#view-and-manage-activity-log-alert-rules-in-azure-portal) tevékenység szabály kezelésével kapcsolatban.


Azt is megteheti, egy egyszerű analógia ismertetése feltételek, amelyeken riasztási szabályok hozható létre a tevékenységnapló-, a rendszer és felfedezni vagy szűrheti az eseményeket a [tevékenységnapló az Azure Portalon](monitoring-overview-activity-logs.md#query-the-activity-log-in-the-azure-portal). Az Azure Monitor - tevékenységnapló-, egy is szűrheti vagy keresse meg szükséges esemény és majd a használatával hozzon létre egy riasztást a **tevékenységnapló-riasztás hozzáadása** gombra; majd lépésekkel 4 és újabb verziók esetében az oktatóanyag a fenti leírtak alapján.
    
 ![ riasztás hozzáadása a tevékenységnaplóról](./media/monitoring-activity-log-alerts-new-experience/add-activity-log.png)
    

### <a name="view-and-manage-activity-log-alert-rules-in-azure-portal"></a>Megtekintheti és kezelheti a tevékenység naplóriasztási szabály az Azure Portalon

1. Az Azure Portalon kattintson **figyelő** > **riasztások** kattintson **szabályok kezelése** , az ablak bal felső.

    ![ Riasztási szabályok kezelése](./media/monitoring-activity-log-alerts-new-experience/manage-alert-rules.png)

    Megjelenik a rendelkezésre álló szabályok listája.

2. Keressen rá a módosítani kívánt tevékenység log szabály.

    ![ keresési tevékenység naplóriasztási szabály](./media/monitoring-activity-log-alerts-new-experience/searth-activity-log-rule-to-edit.png)

    A rendelkezésre álló szűrők - _előfizetés_, _erőforráscsoport_, _erőforrás_, _jeltípus_, vagy _állapota_  a szerkeszteni kívánt tevékenység szabály található.

    > [!NOTE]

    > Csak akkor szerkeszthető **leírás** , **feltételek cél** és **Műveletcsoportok**.

3.  Válassza ki a szabályt, és kattintson duplán a szabály beállítások szerkesztéséhez. Végezze el a szükséges módosításokat, és kattintson a **mentése**.

    ![ Riasztási szabályok kezelése](./media/monitoring-activity-log-alerts-new-experience/activity-log-rule-edit-page.png)

4.  Letiltása, engedélyezése vagy egy szabály törlése. Válassza ki a 2. lépésben leírtaknak megfelelően a szabály kiválasztása után a megfelelő lehetőséget az ablak tetején.


## <a name="manage-alert-rules-for-activity-log-using-azure-resource-template"></a>Az Azure Resource-sablonnal tevékenységnapló riasztási szabályok kezelése
Tevékenységnapló-riasztás létrehozása a Resource Manager-sablon használatával, a típusú erőforrás létrehozása `microsoft.insights/activityLogAlerts`. Ezután, töltse ki az összes kapcsolódó tulajdonságok. Íme egy sablont, amely létrehoz egy tevékenységnapló-riasztás.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
A fenti json-mintaadatok menthető, ez az útmutató céljából (például:) sampleActivityLogAlert.json és telepíthetők [Azure Resource Manager az Azure Portalon](../azure-resource-manager/resource-group-template-deploy-portal.md).

> [!NOTE]
> Akár 5 percig is eltarthat a egy új tevékenység riasztási szabály aktiválása

## <a name="manage-alert-rules-for-activity-log-using-powershell-cli-or-api"></a>A tevékenységnaplóban a PowerShell, a parancssori felület vagy az API riasztási szabályok kezelése
[Az Azure Monitor - tevékenység Log riasztások API](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) egy REST API-t, és teljes mértékben kompatibilisek az Azure Resource Manager REST API-val. Ezért azt is használható a Powershell használatával a Resource Manager egy parancsmagjához, valamint az Azure CLI-n keresztül.

A használati minta erőforrás sablon korábban bemutatott (sampleActivityLogAlert.json) Azure Resource Manager PowerShell-parancsmaggal keresztül alább látható a [erőforrás sablonszakasznak](#manage-alert-rules-for-activity-log-using-azure-resource-template) :
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```
Viselkedésmintáit a sampleActivityLogAlert.parameters.json rendelkezik a riasztási szabály létrehozásához szükséges paraméterek megadott érték.

A használati minta erőforrás sablon korábban bemutatott (sampleActivityLogAlert.json) az Azure CLI Azure Resource Manager parancs-n keresztül alább látható a [erőforrás sablonszakasznak](#manage-alert-rules-for-activity-log-using-azure-resource-template) :

```azurecli
az group deployment create --resource-group myRG --template-file sampleActivityLogAlert.json --parameters @sampleActivityLogAlert.parameters.json
```
Viselkedésmintáit a sampleActivityLogAlert.parameters.json rendelkezik a riasztási szabály létrehozásához szükséges paraméterek megadott érték.


## <a name="next-steps"></a>További lépések

- [Tevékenységi naplóit a Webhook sémáról](monitoring-activity-log-alerts-webhook.md)
- [A Tevékenységnaplók áttekintése](monitoring-activity-log-alerts.md) 
- Tudjon meg többet [Műveletcsoportok](monitoring-action-groups.md).  
- Ismerje meg [szolgáltatás állapotára vonatkozó értesítések](monitoring-service-notifications.md).

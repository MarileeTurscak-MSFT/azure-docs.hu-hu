---
title: Az Azure szolgáltatási értesítésekhez tevékenységnapló-riasztások fogadása
description: Értesítés SMS, e-mailben vagy webhook Azure-szolgáltatás esetén.
author: shawntabrizi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: shtabriz
ms.component: alerts
ms.openlocfilehash: 221434a391f963a764ef36b9533cc8cfd0e16c01
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123448"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Tevékenységnapló-riasztások létrehozása szolgáltatási értesítésekhez
## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan szolgáltatás állapotára vonatkozó értesítések a tevékenységnapló-riasztások beállítása az Azure portal használatával.  

Riasztást is küld, ha az Azure service health értesítéseket küld az Azure-előfizetéshez. A riasztás alapján konfigurálhatja:

- A szolgáltatás állapotával kapcsolatos értesítés (szolgáltatási problémák, tervezett karbantartás, állapot-tanácsadási információk) osztályát.
- Az érintett előfizetés.
- Az érintett szolgáltatásokat.
- Az érintett régió(k).

> [!NOTE]
> Szolgáltatás állapotára vonatkozó értesítések nem küld riasztást erőforrásról hálózatállapot-események.

Is lehet konfigurálni kell a riasztást küldő:

- Válasszon ki egy meglévő művelet.
- Hozzon létre egy új műveletcsoportot (amely a jövőbeni riasztásokhoz használható).

A műveletcsoportokkal kapcsolatban további információt a [műveletcsoportok létrehozásáról és kezeléséről](monitoring-action-groups.md) szóló cikkben talál.

Health értesítési szolgáltatásriasztások konfigurálása Azure Resource Manager-sablonok használatával kapcsolatos információkért lásd: [Resource Manager-sablonok](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Az Azure portal segítségével hozzon létre egy riasztást a szolgáltatás állapotával kapcsolatos értesítés az új műveletcsoport
1. Az a [portál](https://portal.azure.com)válassza **Service Health**.

    ![A "Service" szolgáltatása](./media/monitoring-activity-log-alerts-on-service-notifications/home-servicehealth.png)

1. Az a **riasztások** szakaszban jelölje be **állapotriasztások**.

    ![A "Állapotriasztások" lap](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades-sh.png)

1. Válassza ki **szolgáltatásállapot-riasztás létrehozása** , és töltse ki a mezőket.

    ![A "Létrehozás szolgáltatásállapot-riasztás" parancs](./media/monitoring-activity-log-alerts-on-service-notifications/service-health-alert.png)

1. Válassza ki a **előfizetés**, **szolgáltatások**, és **régiók** szeretne riasztást létrehozni.

    ![A "Tevékenységnapló-riasztás hozzáadása" párbeszédpanel](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Ebben az előfizetésben a tevékenységnapló-riasztás mentéséhez használatos. A riasztási erőforrás ehhez az előfizetéshez van telepítve, és a tevékenységnaplóban eseményeket figyeli.

1. Válassza ki a **eseménytípusok** szeretne riasztást létrehozni: *szolgáltatási probléma*, *tervezett karbantartás*, és *állapottanácsadási információk* 

1. Írja be a riasztás részleteinek megadása egy **riasztásiszabály-névnek** és **leírás**.

1. Válassza ki a **erőforráscsoport** hol szeretné menteni a riasztást.

1. Hozzon létre egy új műveletcsoportot kiválasztásával **új műveletcsoport**. Adjon meg egy nevet a a **műveletcsoport neve** mezőbe, majd adjon meg egy nevet a a **rövid, nevet** mezőbe. Ez a riasztás aktiválódásakor küldött értesítések hivatkozik rövid nevét.

    ![Új műveletcsoport létrehozása](./media/monitoring-activity-log-alerts-on-service-notifications/action-group-creation.png)

1. Adja meg a fogadók listáját azáltal, hogy a fogadó:

    a. **Név**: Adja meg a fogadó nevét, alias vagy azonosítója.

    b. **Művelet típusa**: válassza ki az SMS, e-mailt, webhookot, az Azure app és több.

    c. **Részletek**: a választott művelet típusa alapján, adja meg, egy telefonszám, e-mail címét, webhook URI-t, stb.

1. Válassza ki **OK** a műveletcsoport létrehozásához, majd **riasztási szabály létrehozása** a riasztás végrehajtásához.

Néhány percen belül a riasztás aktív, és megkezdi aktiválása a létrehozása során megadott feltételek alapján.

Ismerje meg, hogyan [konfigurálása webhook-értesítésekkel meglévő probléma felügyeleti rendszerek](../service-health/service-health-alert-webhook-guide.md). A tevékenységnapló-riasztások a webhook sémáról kapcsolatos tudnivalókat lásd: [Webhookok az Azure-tevékenységi naplóriasztások](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>A műveletcsoport meghatározott ezeket a lépéseket az újrafelhasználható, az összes jövőbeli riasztásdefiníciók meglévő műveletcsoport.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Riasztás létrehozása a meglévő műveletcsoport számára a szolgáltatás állapotával kapcsolatos értesítés az Azure portal használatával

1. Végezze el az 1 – 7 hozhat létre a szolgáltatás állapotával kapcsolatos értesítés az előző szakaszban. 

1. Alatt **definiálása műveletcsoport**, kattintson a **válassza műveletcsoport** gombra. Válassza ki a megfelelő műveletet.

1. Válassza ki **Hozzáadás** hozzáadása a műveletcsoport, majd **riasztási szabály létrehozása** a riasztás végrehajtásához.

Néhány percen belül a riasztás aktív, és megkezdi aktiválása a létrehozása során megadott feltételek alapján.

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-resource-manager-templates"></a>Az Azure Resource Manager-sablonok segítségével hozzon létre egy riasztást a szolgáltatás állapotával kapcsolatos értesítés az új műveletcsoport

Az alábbiakban látható egy példa, amely a műveletcsoport hoz létre egy e-mail-tárolóhoz, és lehetővé teszi, hogy az összes szolgáltatás állapotára vonatkozó értesítések a célként megadott előfizetés esetében.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "actionGroups_name": {
            "defaultValue": "SubHealth",
            "type": "String"
        },
        "activityLogAlerts_name": {
            "defaultValue": "ServiceHealthActivityLogAlert",
            "type": "String"
        },
        "emailAddress":{
            "type":"string"
        }
    },
    "variables": {
        "alertScope":"[concat('/','subscriptions','/',subscription().subscriptionId)]"
    },
    "resources": [
        {
            "comments": "Action Group",
            "type": "microsoft.insights/actionGroups",
            "name": "[parameters('actionGroups_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "groupShortName": "[parameters('actionGroups_name')]",
                "enabled": true,
                "emailReceivers": [
                    {
                        "name": "[parameters('actionGroups_name')]",
                        "emailAddress": "[parameters('emailAddress')]"
                    }
                ],
                "smsReceivers": [],
                "webhookReceivers": []
            },
            "dependsOn": []
        },
        {
            "comments": "Service Health Activity Log Alert",
            "type": "microsoft.insights/activityLogAlerts",
            "name": "[parameters('activityLogAlerts_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "scopes": [
                    "[variables('alertScope')]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ServiceHealth"
                        },
                        {
                            "field": "properties.incidentType",
                            "equals": "Incident"
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]",
                            "webhookProperties": {}
                        }
                    ]
                },
                "enabled": true,
                "description": ""
            },
            "dependsOn": [
                "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]"
            ]
        }
    ]
}
```

## <a name="manage-your-alerts"></a>A riasztások kezelése

Miután létrehozta a riasztást, legyen látható a **riasztások** szakaszában **figyelő**. Válassza ki a kezelni kívánt riasztás:

* Szerkesztheti.
* Törölje azt.
* Letiltani vagy engedélyezni, ha szeretné ideiglenesen leállítani, vagy folytathatja a riasztás-mailjeire.

## <a name="next-steps"></a>További lépések
- Ismerje meg, hogyan [konfigurálása webhook-értesítésekkel meglévő probléma felügyeleti rendszerek](../service-health/service-health-alert-webhook-guide.md).
- Ismerje meg [szolgáltatás állapotára vonatkozó értesítések](monitoring-service-notifications.md).
- Ismerje meg [értesítési sebességkorlátozással](monitoring-alerts-rate-limiting.md).
- Tekintse át a [tevékenység log riasztási webhookséma](monitoring-activity-log-alerts-webhook.md).
- Get- [tevékenységnapló-riasztások áttekintése](monitoring-overview-alerts.md), és a riasztások fogadása. 
- Tudjon meg többet [Műveletcsoportok](monitoring-action-groups.md).

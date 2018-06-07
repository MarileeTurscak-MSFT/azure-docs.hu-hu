---
title: Azure-erőforrás nem található a hibák |} Microsoft Docs
description: Ismerteti, hogyan javítsa a hibákat, ha egy erőforrás nem található.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/06/2018
ms.author: tomfitz
ms.openlocfilehash: 494526ae2084053f23bb3a096ac7d089c47a731a
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823435"
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Nem található az Azure-erőforrások hibák megoldása

Ez a cikk ismerteti a hibákat, láthatja, ha egy erőforrás nem található a telepítés során.

## <a name="symptom"></a>Jelenség

Ha a sablont, amely nem oldható fel egy erőforrás nevét tartalmazza, hasonló hibaüzenetet kap:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Ha használja a [hivatkozás](resource-group-template-functions-resource.md#reference) vagy [listKeys](resource-group-template-functions-resource.md#listkeys) működik együtt a erőforrása, amely nem oldható fel, a következő hibaüzenetet kapja:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Ok

Erőforrás-kezelő kell beolvasni az erőforrás tulajdonságait, de nem tudja azonosítani az erőforrást az előfizetésében.

## <a name="solution-1---set-dependencies"></a>1 - megoldás függőségek beállítása

Ha a megszüntetni kívánt központi telepítése a hiányzó erőforrást a sablonban, ellenőrizze, hogy szükséges-e hozzáadjon egy függőséget. Erőforrás-kezelő optimalizálja a központi telepítési erőforrások létrehozásával párhuzamosan, amikor lehetséges. Ha egy erőforrást egy másik erőforrás után kell telepíteni, szeretné-e használni a **dependsOn** elem a sablonban. Például a webes alkalmazás telepítésekor az App Service-csomag léteznie kell. Ha nem adott meg, hogy a webalkalmazás az App Service-csomag függ-e, erőforrás-kezelő mindkét erőforrás egyszerre hoz létre. Hibaüzenetet kap arról, hogy az App Service csomag erőforrás nem található, mert azt még nem létezik az egyik tulajdonságának beállítása a webalkalmazás megkísérlésekor. Megakadályozható, hogy ezt a hibát úgy, hogy a függőséget az web app alkalmazásban.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

De szeretné elkerülni azt, amelyik nem szükséges függőségek beállítása. Ha szükségtelen függőségek, erőforrások, amelyek nem a párhuzamos telepített egymástól függenek, hogy meghosszabbítani az üzemelő példány időtartama. Ezenkívül létrehozhat körkörös függőségek, amelyek a központi telepítés letiltása. A [hivatkozás](resource-group-template-functions-resource.md#reference) függvény és [lista *](resource-group-template-functions-resource.md#listkeys-listsecrets-and-list) funkciók egy implicit függőség a hivatkozott erőforrás hoz létre, ha adott erőforrás ugyanabban a sablonban és a név (nem erőforrás-azonosító által hivatkozott ). Emiatt előfordulhat, hogy további függőségek, mint a függőségek megadott a **dependsOn** tulajdonság. A [resourceId](resource-group-template-functions-resource.md#resourceid) függvény nem létrehozása egy implicit függőség, vagy ellenőrizze, hogy létezik-e az erőforrást. A [hivatkozás](resource-group-template-functions-resource.md#reference) függvény és [lista *](resource-group-template-functions-resource.md#listkeys-listsecrets-and-list) funkciók nem létrehozása egy implicit függőség, amikor az erőforrás hivatkozik az erőforrás-azonosító. Egy implicit függőség létrehozásához adja át a ugyanazt a sablont üzembe helyezett erőforrás nevét.

Amikor megjelenik a függőségi problémák, kell betekintést erőforrás telepítési sorrendjét. A telepítési műveletek sorrendjét megtekintése:

1. Válassza ki az üzembe helyezési előzményeket az erőforráscsoport számára.

   ![Válassza ki a központi telepítés előzményei](./media/resource-manager-not-found-errors/select-deployment.png)

2. Jelölje ki a központi telepítés előzményeit, és válassza ki **események**.

   ![Válassza ki a központi telepítési eseményeket](./media/resource-manager-not-found-errors/select-deployment-events.png)

3. Vizsgálja meg az egyes erőforrások események sorozatát. Nagy figyelmet fordítani az egyes művelet állapotát. Például a következő kép bemutatja, amelynek a telepítése három storage-fiókok párhuzamosan. Figyelje meg, hogy a három storage-fiókok egy időben vannak-e indítva.

   ![Párhuzamos üzembe helyezés](./media/resource-manager-not-found-errors/deployment-events-parallel.png)

   A következő kép bemutatja, három tárfiókok nem telepített párhuzamosan. A második tárfiók attól függ, az első tárfiók, és a harmadik tárfiók attól függ, a második tárfiókot. Az első tárfiók elindult, elfogadott, és befejeződött a következő megkezdése előtt.

   ![szekvenciális központi telepítés](./media/resource-manager-not-found-errors/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Megoldás 2 - erőforrás lekérése másik erőforráscsoportban található

Amikor az erőforrás létezik-e egy másik erőforráscsoportban található, mint a telepítendő, használja a [resourceId függvény](resource-group-template-functions-resource.md#resourceid) el az erőforrás teljesen minősített neve.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Megoldás 3 - ellenőrzés hivatkozás függvény

Keressen egy kifejezés, amely magában foglalja a [hivatkozás](resource-group-template-functions-resource.md#reference) függvény. Megadja az értékeket e az erőforrás van ugyanazon a sablonban, erőforráscsoport és előfizetés függően változhat. A ellenőrizze, hogy van-e a szükséges paraméterértékeket biztosítása a forgatókönyvéhez. Ha az erőforrás egy másik erőforráscsoportban található, adja meg a teljes erőforrás-azonosító. Például hivatkozhat egy másik erőforráscsoportban található a tárfiók, használja:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```
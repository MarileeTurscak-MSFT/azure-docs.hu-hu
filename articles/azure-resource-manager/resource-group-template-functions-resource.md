---
title: Az Azure Resource Manager sablonfüggvényei - erőforrások |} A Microsoft Docs
description: A funkciók az Azure Resource Manager-sablon használatával lekérheti az erőforrásokra vonatkozó értékeket ismerteti.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: tomfitz
ms.openlocfilehash: 4e454030f77a22236da18c8d8a5215667784f7c5
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301440"
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Erőforrás-funkciók az Azure Resource Manager-sablonok

Resource Manager az alábbi funkciókat biztosít erőforrás-értékeinek beolvasása:

* [listAccountSas](#list)
* [listkeys műveletének](#listkeys)
* [listSecrets](#list)
* [lista *](#list)
* [Szolgáltatók](#providers)
* [reference](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [előfizetést](#subscription)

Paraméterek, változókat, vagy a jelenlegi üzemelő példány lekérjük az értékeket, lásd: [központi telepítési érték funkciók](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listaccountsas-listkeys-listsecrets-and-list"></a>listAccountSas, listkeys műveletének, listSecrets és lista *
`listAccountSas(resourceName or resourceIdentifier, apiVersion, functionValues)`

`listKeys(resourceName or resourceIdentifier, apiVersion)`

`listSecrets(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Minden erőforrás típusa, amely támogatja a list művelet értékeit adja vissza. A leggyakoribb használatokban vannak `listKeys` és `listSecrets`. 

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| resourceName vagy resourceIdentifier |Igen |sztring |Az erőforrás egyedi azonosítója. |
| apiVersion |Igen |sztring |API-verzió erőforrás futásidejű állapot. Általában a következő formátumban **éééé-hh-nn**. |
| functionValues |Nem |objektum | A függvény értékekkel rendelkező objektum. Csak adja meg ezt az objektumot az funkciók, amelyek támogatják a paraméterértékeket, rendelkező objektum például fogadása **listAccountSas** a storage-fiók. | 

### <a name="return-value"></a>Vrácená hodnota

A visszaadott objektum listkeys műveletének formátuma a következő:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Más lista függvények, különböző visszaadott formátumokat. Szeretné megtekinteni a függvényt formátumát, foglalja bele a kimeneti szakasz ahogy a példában a sablonban. 

### <a name="remarks"></a>Megjegyzések

Minden művelet, amely kezdődik **lista** a sablonban függvény is használható. Az elérhető műveletek közé tartozik, nem csak listkeys műveletének, de az is, például az operations `list`, `listAdminKeys`, és `listStatus`. A [lista fiók SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) művelethez szükséges a kérelem törzsében paraméterek, például *signedExpiry*. A funkció használatához a sablonban, adja meg a objektum a szervezet a paraméterértékeket.

Annak megállapításához, hogy mely erőforrástípusokat list művelettel rendelkezik, a következő lehetőségek állnak rendelkezésére:

* Nézet a [REST API-műveleteket](/rest/api/) erőforrás-szolgáltató, és keresse a műveletek listázása. Például, hogy a storage-fiókok a [listkeys műveletének művelet](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Használja a [Get-azurermprovideroperation Parancsmagból](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-parancsmagot. Az alábbi példa lekéri az összes listázási műveletek storage-fiókok:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* A következő Azure CLI-parancs segítségével a listában csak a műveletek szűrése:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Az erőforrás neve használatával adja meg az erőforrás vagy a [resourceId függvény](#resourceid). Ha ugyanazt a sablont, amely a hivatkozott erőforrás telepíti ezt a funkciót használ, használja az erőforrás neve.

### <a name="example"></a>Példa

A következő [példasablonja](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) mutatja be az elsődleges és másodlagos kulcsok vissza a kimeneti szakasz egy storage-fiókból. Emellett a tárfiók SAS-jogkivonatát adja vissza. A jogkivonat beszerzéséhez objektum listAccountSas függvény továbbítja. Ebben a példában a listát funkciók használatát mutatják funkcionalitást. Általában, lenne erőforrás értékét a SAS-jogkivonat használata helyett egy kimeneti értéket, küldje vissza. Kimeneti értékeket az üzembe helyezési előzmények vannak tárolva, és nem biztonságos. Meg kell adnia egy lejárati ideje a jövőben az üzembe helyezés befejeződik.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagename": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus"
        },
        "requestContent": {
            "type": "object",
            "defaultValue": {
                "signedServices": "b",
                "signedResourceType": "c",
                "signedPermission": "r",
                "signedExpiry": "2018-08-20T11:00:00Z",
                "signedResourceTypes": "s"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('storagename')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": false,
                "accessTier": "Hot",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "keys": {
            "type": "object",
            "value": "[listKeys(parameters('storagename'), '2018-02-01')]"
        },
        "accountSAS": {
            "type": "object",
            "value": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('requestContent'))]"
        }
    }
}
``` 

Az Azure CLI-vel ebben a példában sablon üzembe helyezéséhez használja:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json --parameters storagename=<your-storage-account>
```

Ez a PowerShell használatával például a sablon üzembe helyezéséhez használja:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json -storagename <your-storage-account>
```

<a id="providers" />

## <a name="providers"></a>Szolgáltatók
`providers(providerNamespace, [resourceType])`

Erőforrás-szolgáltató és a támogatott erőforrástípusok kapcsolatos információkat ad vissza. Ha nem ad meg egy erőforrás típusa, a függvény a támogatott típusok az erőforrás-szolgáltató adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| providerNamespace |Igen |sztring |A szolgáltató Namespace |
| resourceType |Nem |sztring |Erőforrás típusa, az adott névtérben. |

### <a name="return-value"></a>Vrácená hodnota

Minden támogatott típus a következő formátumban adja vissza: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

A visszaadott értékekhez tömb rendezése nem garantált.

### <a name="example"></a>Példa

A következő [példasablonja](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) bemutatja, hogyan használja a szolgáltató függvényt:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "providerNamespace": {
            "type": "string"
        },
        "resourceType": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers(parameters('providerNamespace'), parameters('resourceType'))]",
            "type" : "object"
        }
    }
}
```

Az a **Microsoft.Web** erőforrás-szolgáltató és **helyek** erőforrás típusa, az előző példában egy objektumot ad vissza a következő formátumban:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

Az Azure CLI-vel ebben a példában sablon üzembe helyezéséhez használja:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json --parameters providerNamespace=Microsoft.Web resourceType=sites
```

Ez a PowerShell használatával például a sablon üzembe helyezéséhez használja:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json -providerNamespace Microsoft.Web -resourceType sites
```

<a id="reference" />

## <a name="reference"></a>Hivatkozás
`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Az erőforrások futásidejű állapotot képviselő objektumot adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| resourceName vagy resourceIdentifier |Igen |sztring |Név vagy erőforrás egyedi azonosítója. |
| apiVersion |Nem |sztring |A megadott erőforrás API-verzió. Adja meg az értékét üzembe helyezésekor az erőforrás nem található ugyanazt a sablont. Általában a következő formátumban **éééé-hh-nn**. |
| "Teljes" |Nem |sztring |Érték, amely megadja, hogy a teljes erőforrás-objektumot ad vissza. Ha nem ad meg `'Full'`, csak az erőforrás tulajdonságai objektumot ad vissza. A teljes objektum például az erőforrás-azonosító és a hely értékeket tartalmaz. |

### <a name="return-value"></a>Vrácená hodnota

Minden erőforrástípus a referencia-függvény különböző tulajdonságait adja vissza. A függvény nem ad vissza egy egyetlen, előre definiált formátumot. A visszaadott érték is, a teljes objektum megadott attól függően eltér. Egy erőforrás-típus tulajdonságainak megtekintéséhez a objektumot ad vissza, a kimeneti szakaszban, a példában látható módon.

### <a name="remarks"></a>Megjegyzések

A referencia-funkció az értékét a futásidejű állapot osztályból, és ezért nem használható a változók szakaszban. A sablon kimeneti szakasz a használat vagy [hivatkozott sablonnak](resource-group-linked-templates.md#link-or-nest-a-template). A kimenetek szakaszában nem használható egy [beágyazott sablont](resource-group-linked-templates.md#link-or-nest-a-template). Az értékeket egy üzembe helyezett erőforrás visszaadása egy beágyazott sablont, váltson egy hivatkozott sablonnak a beágyazott sablont. 

A referencia-függvény használatával akkor implicit módon deklarálja, hogy egy erőforrás függ-e egy másik erőforrás, ha a hivatkozott erőforrás kiosztása belül ugyanazt a sablont, és a nevét (nem erőforrás-azonosító) az erőforrás hivatkozik. Emellett a dependsOn tulajdonság használatához nincs szükség. A függvény nem kerül kiértékelésre, a hivatkozott erőforrás üzembe helyezési befejeződéséig.

Tekintse meg a nevét és a egy erőforrástípushoz értékeit, hozzon létre egy sablont, amely az objektumot ad vissza, a kimeneti szakaszban. Ha az adott típusú erőforrással rendelkezik, a sablon bármely új erőforrások üzembe helyezése nélkül adja vissza az objektumot. 

Általában használni a **referencia** funkció egy adott érték visszaadása egy objektumot, például a blob-végpont URI-t vagy teljesen minősített tartománynevét.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

Használat `'Full'` erőforrás értékek, amelyek nem részei a Tulajdonságok séma számíthat. Például a kulcstartó hozzáférési házirendjeinek beállítása, kérje le a tulajdonságok egy virtuális géphez.

```json
{
  "type": "Microsoft.KeyVault/vaults",
  "properties": {
    "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
    "accessPolicies": [
      {
        "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
        "objectId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.principalId]",
        "permissions": {
          "keys": [
            "all"
          ],
          "secrets": [
            "all"
          ]
        }
      }
    ],
    ...
```

A fenti sablon teljes példa: [Key vault Windows](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo08.msiWindowsToKeyvault.json). Egy hasonló példa érhető el az [Linux](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo07.msiLinuxToArm.json).

### <a name="example"></a>Példa

A következő [példasablonja](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) üzembe helyez egy erőforrást, és adott erőforrásra hivatkozik.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      },
      "fullReferenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'), '2016-12-01', 'Full')]"
      }
    }
}
``` 

Az előző példában a két objektum adja vissza. A Tulajdonságok objektumában a következő formátumban kell megadni:

```json
{
   "creationTime": "2017-10-09T18:55:40.5863736Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

A teljes objektum a következő formátumban kell megadni:

```json
{
  "apiVersion":"2016-12-01",
  "location":"southcentralus",
  "sku": {
    "name":"Standard_LRS",
    "tier":"Standard"
  },
  "tags":{},
  "kind":"Storage",
  "properties": {
    "creationTime":"2017-10-09T18:55:40.5863736Z",
    "primaryEndpoints": {
      "blob":"https://examplestorage.blob.core.windows.net/",
      "file":"https://examplestorage.file.core.windows.net/",
      "queue":"https://examplestorage.queue.core.windows.net/",
      "table":"https://examplestorage.table.core.windows.net/"
    },
    "primaryLocation":"southcentralus",
    "provisioningState":"Succeeded",
    "statusOfPrimary":"available",
    "supportsHttpsTrafficOnly":false
  },
  "subscriptionId":"<subscription-id>",
  "resourceGroupName":"functionexamplegroup",
  "resourceId":"Microsoft.Storage/storageAccounts/examplestorage",
  "referenceApiVersion":"2016-12-01",
  "condition":true,
  "isConditionTrue":true,
  "isTemplateResource":false,
  "isAction":false,
  "provisioningOperation":"Read"
}
```

Az Azure CLI-vel ebben a példában sablon üzembe helyezéséhez használja:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json --parameters storageAccountName=<your-storage-account>
```

Ez a PowerShell használatával például a sablon üzembe helyezéséhez használja:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json -storageAccountName <your-storage-account>
```

A következő [példasablonja](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) hivatkozik egy tárfiókot, amelyet az ebben a sablonban nincs telepítve. A tárfiók már létezik belül ugyanabban az erőforráscsoportban.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

Az Azure CLI-vel ebben a példában sablon üzembe helyezéséhez használja:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json --parameters storageAccountName=<your-storage-account>
```

Ez a PowerShell használatával például a sablon üzembe helyezéséhez használja:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json -storageAccountName <your-storage-account>
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a>Erőforráscsoport
`resourceGroup()`

Az aktuális erőforráscsoport képviselő objektumot adja vissza. 

### <a name="return-value"></a>Vrácená hodnota

A visszaadott objektum a következő formátumban kell megadni:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a>Megjegyzések

A resourceGroup függvény egyik gyakori felhasználási hozhat létre erőforrásokat az erőforráscsoport ugyanazon a helyen. Az alábbi példában az erőforráscsoport helyét egy webhely a hely hozzárendeléséhez.

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a>Példa

A következő [példasablonja](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) az erőforráscsoport tulajdonságait adja vissza.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "resourceGroupOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

Az előző példában egy objektumot ad vissza a következő formátumban:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Az Azure CLI-vel ebben a példában sablon üzembe helyezéséhez használja:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json
```

Ez a PowerShell használatával például a sablon üzembe helyezéséhez használja:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json 
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Adja vissza egy adott erőforrás egyedi azonosítója. A függvény akkor használható, ha az erőforrás neve nem egyértelmű vagy ugyanabban a sablonban nincs kiépítve. 

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nem |karakterlánc (a GUID formátumban) |Alapértelmezett érték az aktuális előfizetésben. Adja meg ezt az értéket, amikor szüksége van egy másik előfizetésben egy erőforrás lekérése. |
| resourceGroupName |Nem |sztring |Alapértelmezett érték az aktuális erőforráscsoportban. Adja meg ezt az értéket, amikor szüksége van egy másik erőforráscsoportban található egy erőforrás lekérése. |
| resourceType |Igen |sztring |Felhasznált erőforrás típusa, beleértve az erőforrás-szolgáltató névtere. |
| resourceName1 |Igen |sztring |Erőforrás neve. |
| resourceName2 |Nem |sztring |Következő erőforrás neve szegmens Ha erőforrás van beágyazva. |

### <a name="return-value"></a>Vrácená hodnota

Az azonosító a következő formátumban adja vissza:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Megjegyzések

Az erőforrás-e az azonos előfizetésben és erőforráscsoportban tartozik, mint a jelenlegi üzemelő példány a megadott paraméterértékek függenek.

Egy tárfiók ugyanabban az előfizetésben és erőforráscsoportban található az erőforrás-azonosító lekéréséhez használja:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

A tárfiók ugyanahhoz az előfizetéshez, de egy másik erőforráscsoportban található az erőforrás-azonosító lekéréséhez használja:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Az erőforrás-azonosítója egy storage-fiók egy másik előfizetésben és erőforráscsoportban, amelyet:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Egy adatbázis egy másik erőforráscsoportban található az erőforrás-azonosító lekéréséhez használja:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Gyakran kell használatakor ez a függvény egy storage-fiók vagy a virtuális hálózat egy másik erőforráscsoportban. Az alábbi példa bemutatja, hogyan könnyen használható egy erőforrás egy külső erőforrás-csoportból:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a>Példa

A következő [példasablonja](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) adja vissza az erőforráscsoportot egy storage-fiók erőforrás-azonosító:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('11111111-1111-1111-1111-111111111111', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

Az alapértelmezett értékeket az előző példa kimenete a következő:

| Name (Név) | Típus | Érték |
| ---- | ---- | ----- |
| sameRGOutput | Sztring | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Sztring | /subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Sztring | /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Sztring | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName |

Az Azure CLI-vel ebben a példában sablon üzembe helyezéséhez használja:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json
```

Ez a PowerShell használatával például a sablon üzembe helyezéséhez használja:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json 
```

<a id="subscription" />

## <a name="subscription"></a>előfizetést
`subscription()`

Az előfizetés, a jelenlegi üzemelő példány részleteit adja vissza. 

### <a name="return-value"></a>Vrácená hodnota

A függvény a következő formátumban adja vissza:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Példa

A következő [példasablonja](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) mutat be az előfizetés függvény meghívta a kimeneti szakasz. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

Az Azure CLI-vel ebben a példában sablon üzembe helyezéséhez használja:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json
```

Ez a PowerShell használatával például a sablon üzembe helyezéséhez használja:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json 
```

## <a name="next-steps"></a>További lépések
* A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* A megadott számú alkalommal újrafuttathatja egy adott típusú erőforrás létrehozásakor, lásd: [több erőforráspéldány létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* A létrehozott sablon üzembe helyezése, olvassa el [alkalmazás üzembe helyezése Azure Resource Manager-sablonnal](resource-group-template-deploy.md).


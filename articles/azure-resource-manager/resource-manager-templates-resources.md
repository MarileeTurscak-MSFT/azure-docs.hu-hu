---
title: Az Azure Resource Manager-sablon erőforrásainak |} A Microsoft Docs
description: Ismerteti az Azure Resource Manager-sablonok deklaratív JSON-szintaxist használva erőforrás szakaszába.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2018
ms.author: tomfitz
ms.openlocfilehash: 6723cf8cc18637c157b295361425357e1c47ec2e
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007161"
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Erőforrások szakaszában található Azure Resource Manager-sablonok

Az erőforrások szakaszban meghatározhatja az erőforrásokat, amelyek telepítése vagy frissítése. Ez a szakasz is kapott bonyolult, mert ismernie kell a típusok, helyezi üzembe, adja meg a megfelelő értékeket.

## <a name="available-properties"></a>Rendelkezésre álló tulajdonságok

Az alábbi struktúra használatával erőforrásokat határoz meg:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Elem neve | Szükséges | Leírás |
|:--- |:--- |:--- |
| feltétel | Nem | Logikai érték, amely azt jelzi, hogy az erőforrás jön létre a központi telepítés során. Amikor `true`, az erőforrás létrehozása az üzembe helyezés során. Amikor `false`, az erőforrást a rendszer kihagyta a központi telepítéshez. |
| apiVersion |Igen |Az erőforrás létrehozásához használt REST API-verzió. |
| type |Igen |Az erőforrás típusát. Ezt az értéket a névteret, az erőforrás-szolgáltató és az erőforrástípus kombinációja (például **Microsoft.Storage/storageAccounts**). |
| név |Igen |Az erőforrás neve. A név RFC3986 meghatározott URI-összetevőt korlátozásokat kell követnie. Emellett az Azure-szolgáltatások elérhetővé az erőforrás neve kívüli felek ellenőrzése, hogy a név nem egy másik identitását meghamisítását tett kísérlet. |
| location |Változó |Támogatott a megadott erőforráscsoport földrajzi helyét. Az elérhető helyek közül választhat, de általában logikus válasszon egyet a felhasználók közelében van. Általában is logikus helyezni erőforrásokat, amelyek ugyanabban a régióban léphetnek kapcsolatba egymással. A legtöbb erőforrástípusok szüksége egy olyan helyre, de bizonyos típusú (például a szerepkör-hozzárendelés) egy olyan helyre nem igényelnek. |
| tags |Nem |Az erőforráshoz tartozó címkék. Hogy logikusan rendszerezhesse az erőforrások az előfizetésen címkékkel. |
| Megjegyzések |Nem |Dokumentálja a sablonban az erőforrásokat a megjegyzések |
| másolás |Nem |Ha több példány van szükség, az erőforrások létrehozásához számát. Az alapértelmezett mód párhuzamos. Adja meg a soros módra, amikor nem szeretné, hogy az összes vagy egy időben üzembe helyezendő erőforrásokat. További információkért lásd: [több erőforráspéldány létrehozása az Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nem |Az erőforrások telepíteni kell az erőforrás üzembe van helyezve. Resource Manager kiértékeli az erőforrások közti függőségeket, és a megfelelő sorrendben telepíti azokat. Ha az erőforrások nem függ egymástól, hogy helyezésük párhuzamosan. Az érték lehet egy erőforrás vesszővel elválasztott listáját nevét vagy az erőforrás egyedi azonosítók. Ez a sablon üzembe helyezett erőforrások csak listája. A sablonban nem meghatározott erőforrások már léteznie kell. Kerülje a szükségtelen függőségek hozzáadása a telepítéshez lelassíthatja, és hozzon létre körkörös függőségi. Beállítás függőségekkel kapcsolatos útmutatásért lásd: [függőségek meghatározása az Azure Resource Manager-sablonok](resource-group-define-dependencies.md). |
| properties |Nem |Erőforrás-specifikus konfigurációs beállításokat. A tulajdonságok értékei ugyanazok, mint a REST API-művelet (PUT metódust) az erőforrás létrehozásához nyújt a kérelem törzsében szereplő értékek. Megadhat egy másolási tömböt egy tulajdonságot több példányát is. |
| sku | Nem | Bizonyos erőforrások üzembe helyezéséhez a Termékváltozat definiáló engedélyezése. Ha például a tárfiókok a redundancia típusát is megadhat. |
| típusa | Nem | Bizonyos erőforrások lehetővé teszik egy értéket, amely meghatározza a telepít erőforrás típusát. Ha például a Cosmos DB létrehozása típusát is megadhat. |
| csomag | Nem | Bizonyos erőforrások lehetővé teszik az értékek, amelyek meghatározzák a csomag telepítéséhez. Ha például egy virtuális gépen a Marketplace-beli rendszerképét is megadhat. | 
| erőforrások |Nem |Gyermek erőforrások, amelyek a definiált erőforrás függenek. Csak adja meg a séma a szülő erőforrás által számukra engedélyezett erőforrástípusok. Teljesen minősített erőforrás típusa, a gyermek tartalmazza a szülő erőforrás típusa, például **Microsoft.Web/sites/extensions**. A szülőerőforrás függőség nem implicit. Meg kell határoznia, hogy a függőséget explicit módon. |

## <a name="condition"></a>Állapot

Ha el kell döntenie, üzembe helyezés során e hozzon létre egy erőforrást, használja a `condition` elemet. Ez az elem értéke IGAZ vagy hamis értéket mutat. Ha az értéke true, az erőforrás jön létre. Ha a beállítás értéke FALSE (hamis), az erőforrás nem lesz létrehozva. Általában ezt az értéket használja, ha azt szeretné, hozzon létre egy új erőforrást, vagy használjon egy meglévőt. Például adja meg, hogy van-e telepítve egy új tárfiókot, vagy egy meglévő tárfiókot használja, használja:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Egy teljes példát sablon által használt a `condition` elem, lásd: [egy új vagy meglévő virtuális hálózati, tárolási és nyilvános IP-Címmel rendelkező virtuális gép](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

## <a name="resource-specific-values"></a>Erőforrás-specifikus értékeket

A **API-verzió**, **típus**, és **tulajdonságok** elemek különböznek az egyes erőforrástípusok. A **termékváltozat**, **kind**, és **terv** elemei bizonyos erőforrástípusok esetében elérhető, de nem minden. Ezek a tulajdonságok értékeit megállapításához lásd: [sablonreferenciája](/azure/templates/).

## <a name="resource-names"></a>Erőforrás neve

Általában a Resource Managerben erőforrásnevek három típusú dolgozni:

* Erőforrás nevének egyedinek kell lennie.
* Nem kell egyedinek lennie tartalmazó erőforrásneveket, de válassza ki, amelyek segítségével azonosíthatja az erőforrás nevének megadásához.
* Lehet, hogy általános erőforrásnevet.

### <a name="unique-resource-names"></a>Egyedi erőforrásnevek

Adja meg a minden erőforrás típusa, amely rendelkezik egy adat-hozzáférési végpont egyedi erőforráscsoport nevét. Néhány gyakori erőforrástípusok igénylő egy egyedi nevet a következők:

* Azure Storage<sup>1</sup> 
* Web Apps funkció az Azure App Service-ben
* SQL Server
* Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> tárfiókneveket is kisbetűnek kell lennie, 24 karakter vagy kevesebb, és nem kell minden kötőjel.

A név megadásakor, manuálisan hozzon létre egy egyedi nevet, vagy használja a [uniqueString()](resource-group-template-functions-string.md#uniquestring) függvény használatával létrehoz egy nevet. Érdemes azt is adjon hozzá egy előtagot vagy az utótag az **uniqueString** eredményt. Az egyedi név módosítását segítségével további könnyen azonosíthatja az erőforrás típusa, a neve. Ha például egy storage-fiók egy egyedi nevet is létrehozhat használatával a következő változót:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Az azonosításhoz erőforrásnevek
Bizonyos erőforrástípusok, neve, de a nevük érdemes nem rendelkezik egyedinek kell lennie. Ezek erőforrástípusok megadhat egy nevet, amely azonosítja az erőforrás-környezet és az erőforrás típusát egyaránt.

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

### <a name="generic-resource-names"></a>Általános erőforrás neve
Minden erőforrás esetében, amelyet leginkább keresztül érhető el egy másik erőforrás egy általános nevet, amely nem változtatható a sablonban is használhatja. Például beállíthatja egy szabványos, általános tűzfalszabályokat nevet egy SQL-kiszolgálón:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Hely
Sablon üzembe helyezésekor, meg kell adnia az egyes erőforrások helyét. Különböző típusú különböző helyeken támogatottak. Az előfizetéshez, az adott erőforrástípushoz elérhető helyek listájának megtekintéséhez használja az Azure PowerShell vagy az Azure CLI. 

Az alábbi példa a PowerShell segítségével kéri le a helyek a `Microsoft.Web\sites` erőforrás típusa:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Az alábbi példa az Azure CLI segítségével kéri le a helyek a `Microsoft.Web\sites` erőforrás típusa:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

Annak meghatározásához, hogy az erőforrások a támogatott helyek, miután a sablonban beállított adott helyen. Ezt az értéket a legegyszerűbb módja az, hogy hozzon létre egy erőforráscsoportot egy helyen, amely támogatja a erőforrástípusok, és állítsa be a táblázatnak `[resourceGroup().location]`. Erőforráscsoportok különböző helyeken a sablon újbóli telepítése, és nem sem tudják módosítani a sablonban vagy a paraméterek. 

Az alábbi példa bemutatja egy ugyanazon a helyen az erőforráscsoport üzembe helyezett tárfiókot:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Ha szüksége kódba foglalni a helyet a sablonban, adja meg a támogatott régiók közül a nevét. Az alábbi példa bemutatja egy storage-fiókot, amelyet mindig USA északi középső Régiója:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="tags"></a>Címkék
[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

### <a name="add-tags-to-your-template"></a>Címkék hozzáadása a sablonhoz

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Gyermek-erőforrás

Bizonyos erőforrástípusok belül is meghatározhat gyermekerőforrásait tömbjét. Gyermek erőforrások olyan erőforrások, csak egy másik erőforrás keretén belül léteznek. Például egy SQL-adatbázis nem létezhet anélkül, hogy egy SQL server, az adatbázis a kiszolgáló gyermek. Az adatbázis a kiszolgáló meghatározásán határozhatja meg.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Beágyazott, ha a típus értéke `databases` , de a teljes erőforrás típusa `Microsoft.Sql/servers/databases`. Nem ad meg `Microsoft.Sql/servers/` mivel feltételezzük, hogy az erőforrás típusa. A gyermek-erőforrás neve értékre van állítva `exampledatabase` , de a teljes nevet tartalmazza a szülő neve. Nem ad meg `exampleserver` mivel feltételezzük, hogy a szülő erőforrás.

A gyermek erőforrástípus formátuma: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

A gyermek-erőforrás neve formátuma: `{parent-resource-name}/{child-resource-name}`

Azonban nem kell az adatbázist a kiszolgálón belül meghatározásához. Megadhatja, hogy a gyermek-erőforrás, a legfelső szinten. Előfordulhat, hogy ezt a módszert használja, ha ugyanazt a sablont a szülő erőforrás nincs telepítve, vagy ha szeretné használni `copy` több gyermek-erőforrás létrehozása. Ezt a módszert használja adja meg a teljes erőforrás típusa, és a gyermek-erőforrás neve a szülő erőforrás nevét tartalmazza.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Egy teljesen minősített erőforrás hivatkozást létrehozásánál ahhoz, hogy típusa és neve a szegmensek egyesítése a nem egyszerűen csak az erősebbet összefűzésével. Után a névtér, használjon inkább egy sorozatát *típusnév/* párok a legkevésbé nejvíce specifické:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Példa:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` helyes `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` nem megfelelő

## <a name="recommendations"></a>Javaslatok
A következő információkat az erőforrásokkal való munka során lehet hasznos:

* Adjon meg más közreműködőkkel az erőforrás rendeltetésének megismerése érdekében **megjegyzések** az egyes erőforrások a sablonban:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Ha egy *nyilvános végpontot* (például egy Azure Blob storage nyilvános végpont), a sablonban *do nem rögzített kód* a névtér. Használja a **referencia** függvény dinamikusan beolvasni a névteret. Ez a módszer használatával a végpont a sablonban manuális módosítása nélkül helyezheti üzembe a sablont a különböző nyilvános névtér-környezetekben. Az API-verzió beállítása ugyanarra a verzióra, amely a storage-fiókot használja a sablonban:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Ha ugyanazt a sablont hoz létre a storage-fiók van telepítve, nem kell a szolgáltatói névtér adja meg, ha az erőforrás hivatkozik. Az alábbi példa bemutatja az egyszerűsített Szintaxis:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Ha más nyilvános névtér használatára beállított értékeket a sablonban, módosítsa ezeket az értékeket, hogy azonos **referencia** függvény. Például beállíthatja a **storageUri** a virtuális gép diagnosztikai profiljának tulajdonságát:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Egy meglévő tárfiókot, amely egy másik erőforráscsoportban található is lehet hivatkozni:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Nyilvános IP-címek hozzárendelése a virtuális gép csak akkor, ha egy alkalmazás írja elő. Ha csatlakozni szeretne egy virtuális gépet (VM) a hibakereséshez, vagy a felügyeleti vagy felügyeleti célokra, használja a bejövő NAT-szabályokat, a virtuális hálózati átjáró vagy a jumpbox.
   
     Virtuális gépekhez való csatlakozás kapcsolatos további információkért lásd:
   
   * [Virtuális gépek futtatása egy N szintű architektúrához az Azure-ban](../guidance/guidance-compute-n-tier-vm.md)
   * [A WinRM-elérés beállítása virtuális gépekhez az Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [A virtuális gép külső hozzáférés engedélyezése az Azure portal használatával](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [A virtuális gép külső hozzáférés engedélyezése a PowerShell használatával](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [A Linux rendszerű virtuális gép külső hozzáférés engedélyezése az Azure CLI-vel](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* A **domainNameLabel** nyilvános IP-címek tulajdonsága egyedinek kell lennie. A **domainNameLabel** értéket kell csak 3 és 63 karakter közötti lehet, és kövesse a reguláris kifejezés által meghatározott szabályok: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Mivel a **uniqueString** függvény létrehoz egy karakterlánc, amely 13 karakterből, a **dnsPrefixString** paraméter értéke legfeljebb 50 karakter hosszúságú lehet:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Amikor jelszót ad hozzá egy egyéni szkriptek futtatására szolgáló bővítmény, használja a **commandToExecute** tulajdonságot a **protectedSettings** tulajdonság:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > Győződjön meg arról, hogy a titkok titkosítását, ha a azok paraméterként a virtuális gépek és a bővítményeket, használja a **protectedSettings** tulajdonságát a megfelelő bővítményeket.
   > 
   > 


## <a name="next-steps"></a>További lépések
* A különböző megoldástípusokhoz használható teljes sablonok megtekintéséhez lásd: [Azure gyorsindítási sablonok](https://azure.microsoft.com/documentation/templates/).
* A sablonon belül használhatja függvényeivel kapcsolatos részletekért lásd: [Azure Resource Manager-Sablonfüggvények](resource-group-template-functions.md).
* Egynél több sablon üzembe helyezése során használatához lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* Szükség lehet a belül egy másik erőforráscsoportban található erőforrások. Ebben a forgatókönyvben nem gyakori, ha a storage-fiókok vagy a virtuális hálózatokat, amelyek több erőforráscsoportok vannak megosztva. További információkért lásd: a [resourceId függvény](resource-group-template-functions-resource.md#resourceid).
* Erőforrás neve vonatkozó megkötésekkel kapcsolatos további információkért lásd: [ajánlott az Azure-erőforrások elnevezési konvenciói](../guidance/guidance-naming-conventions.md).
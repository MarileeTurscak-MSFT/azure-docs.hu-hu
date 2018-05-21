---
title: Hozzon létre egy alkalmazás webalkalmazási tűzfal - Azure parancssori Felülettel |} Microsoft Docs
description: Megtudhatja, hogyan hozzon létre egy alkalmazás webalkalmazási tűzfal az Azure parancssori felület használatával.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: victorh
ms.openlocfilehash: 1a0691f51071ec4e983727967790367507ef40fd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/20/2018
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-cli"></a>Hozzon létre egy alkalmazást az Azure parancssori felület használatával webalkalmazási tűzfal

Az Azure parancssori felület használatával hozzon létre egy [Alkalmazásátjáró](application-gateway-introduction.md) rendelkező egy [webalkalmazási tűzfal](application-gateway-web-application-firewall-overview.md) (waf-ot), amely használ egy [virtuálisgép-méretezési csoport](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). A WAF használatát [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) szabályokat, hogy az alkalmazás védelme. Ezek a szabályok közé tartoznak például az SQL-injektálás támadások, a többhelyes parancsfájlok futtatására és a munkamenet kihasználásának elleni védelem. 

Ebben a cikkben az alábbiakkal ismerkedhet meg:

> [!div class="checklist"]
> * A hálózat beállítása
> * Hozzon létre egy alkalmazás WAF engedélyezve
> * Virtuálisgép-méretezési csoport létrehozása
> * Hozzon létre egy tárfiókot, és diagnosztika konfigurálása

![Webes alkalmazás tűzfal – példa](./media/application-gateway-web-application-firewall-cli/scenario-waf.png)

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítését és használatát választja, akkor ehhez az oktatóanyaghoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség. A verzió megkereséséhez futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Az erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. Hozzon létre egy Azure erőforráscsoport nevű *myResourceGroupAG* rendelkező [az csoport létrehozása](/cli/azure/group#az_group_create).

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Hálózati erőforrások létrehozása

A virtuális hálózat és az alhálózatokat használják az Alkalmazásátjáró és a kapcsolódó erőforrások hálózati kapcsolatot biztosít. Nevű a virtuális hálózat létrehozása *myVNet* és nevű alhálózat *myAGSubnet* rendelkező [az hálózati vnet létrehozása](/cli/azure/network/vnet#az_network_vnet_create) és [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Hozzon létre egy nyilvános IP-cím nevű *myAGPublicIPAddress* rendelkező [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#az_network_public_ip_create).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myBackendSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myAGSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway-with-a-waf"></a>Hozzon létre egy alkalmazást egy WAF

Használhat [az hálózati Alkalmazásátjáró létrehozása](/cli/azure/application-gateway#az_application_gateway_create) nevű Alkalmazásátjáró létrehozása *myAppGateway*. Alkalmazásátjáró az Azure parancssori felület használatával hoz létre, amikor konfigurációs adatokat, például, sku, és a HTTP-beállításait adja meg. Az Alkalmazásátjáró hozzá van rendelve *myAGSubnet* és *myPublicIPSddress* , amelyet korábban hozott létre.

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --capacity 2 \
  --sku WAF_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
az network application-gateway waf-config set \
  --enabled true \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --firewall-mode Detection \
  --rule-set-version 3.0
```

Az alkalmazás-átjáró hozható létre több percig is eltarthat. Az Alkalmazásátjáró létrehozása után megtekintheti az új szolgáltatásokat is:

- *appGatewayBackendPool* -Alkalmazásátjáró rendelkeznie kell legalább egy háttér címkészletet.
- *appGatewayBackendHttpSettings* – Megadja, hogy 80-as porton, és olyan HTTP protokollt használja a kommunikációhoz.
- *appGatewayHttpListener* -a társított alapértelmezett figyelő *appGatewayBackendPool*.
- *appGatewayFrontendIP* -hozzárendel *myAGPublicIPAddress* való *appGatewayHttpListener*.
- *Szabály1* - útválasztási szabály társított alapértelmezett *appGatewayHttpListener*.

## <a name="create-a-virtual-machine-scale-set"></a>Virtuálisgép-méretezési csoport létrehozása

Ebben a példában hozzon létre egy virtuálisgép-méretezési csoport, amely a háttérkészletének az Alkalmazásátjáró két kiszolgáló biztosítja. A méretezési csoportban lévő virtuális gépek társított a *myBackendSubnet* alhálózat. A skála létrehozásához állítsa be, használhatja [az vmss létrehozása](/cli/azure/vmss#az_vmss_create).

```azurecli-interactive
az vmss create \
  --name myvmss \
  --resource-group myResourceGroupAG \
  --image UbuntuLTS \
  --admin-username azureuser \
  --admin-password Azure123456! \
  --instance-count 2 \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --vm-sku Standard_DS2 \
  --upgrade-policy-mode Automatic \
  --app-gateway myAppGateway \
  --backend-pool-name appGatewayBackendPool
```

### <a name="install-nginx"></a>Az NGINX telepítése

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroupAG \
  --vmss-name myvmss \
  --settings '{ "fileUris": ["https://raw.githubusercontent.com/davidmu1/samplescripts/master/install_nginx.sh"],"commandToExecute": "./install_nginx.sh" }'
```

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Hozzon létre egy tárfiókot, és diagnosztika konfigurálása

Ebben az oktatóanyagban az Alkalmazásátjáró tárfiók adatok használ felderítésére és megelőzésére célokra. Log Analytics vagy az Eseményközpont kiválasztásával adatok rögzítéséhez is használhatja. 

### <a name="create-a-storage-account"></a>Create a storage account

Hozzon létre egy tárfiókot, nevű *myagstore1* rendelkező [az storage-fiók létrehozása](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create).

```azurecli-interactive
az storage account create \
  --name myagstore1 \
  --resource-group myResourceGroupAG \
  --location eastus \
  --sku Standard_LRS \
  --encryption blob
```

### <a name="configure-diagnostics"></a>Diagnosztika konfigurálása

Erőforrásrekord-adatokat a diagnosztika konfigurálja azokat a ApplicationGatewayAccessLog ApplicationGatewayPerformanceLog és ApplicationGatewayFirewallLog naplók. Helyettesítő `<subscriptionId>` az előfizetés-azonosítóval, majd konfigurálja a diagnosztika [az figyelő diagnosztikai-beállítások létrehozása](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az_monitor_diagnostic_settings_create).

```azurecli-interactive
appgwid=$(az network application-gateway show --name myAppGateway --resource-group myResourceGroupAG --query id -o tsv)
storeid=$(az storage account show --name myagstore1 --resource-group myResourceGroupAG --query id -o tsv)
az monitor diagnostic-settings create --name appgwdiag --resource $appgwid \
  --logs '[ { "category": "ApplicationGatewayAccessLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayPerformanceLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "ApplicationGatewayFirewallLog", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --storage-account $storeid
```

## <a name="test-the-application-gateway"></a>Az Alkalmazásátjáró tesztelése

A nyilvános IP-cím, az alkalmazás-átjáró használatához [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#az_network_public_ip_show). Másolja a nyilvános IP-címet, majd illessze be a böngésző címsorába.

```azurepowershell-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

![Az alkalmazás átjáró alap URL-cím tesztelése](./media/application-gateway-web-application-firewall-cli/application-gateway-nginxtest.png)

## <a name="next-steps"></a>További lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * A hálózat beállítása
> * Hozzon létre egy alkalmazás WAF engedélyezve
> * Virtuálisgép-méretezési csoport létrehozása
> * Hozzon létre egy tárfiókot, és diagnosztika konfigurálása

További információt a alkalmazásátjárót és a kapcsolódó erőforrások, továbbra is a útmutatókat.
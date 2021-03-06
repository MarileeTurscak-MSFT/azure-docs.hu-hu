---
title: Az Azure Resource Manager elérése Windows VM-beli, felhasználó által hozzárendelt felügyelt identitással
description: Oktatóanyag, amely végigvezeti az Azure Resource Manager Windows VM-beli, felhasználó által hozzárendelt felügyelt identitással való elérésének folyamatán.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: daveba
ms.openlocfilehash: b94c0764816fb0eb15ede7cd74dc74adada5bc9b
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106122"
---
# <a name="tutorial-use-a-user-assigned-managed-identity-on-a-windows-vm-to-access-azure-resource-manager"></a>Oktatóanyag: Az Azure Resource Manager elérése Windows VM-beli, felhasználó által hozzárendelt felügyelt identitással

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Ez az oktatóanyag ismerteti, hogyan hozhat létre felhasználó által hozzárendelt identitást, hogyan csatolhatja azt Windows rendszerű virtuális géphez, majd hogyan használhatja ezt az identitást az Azure Resource Manager API eléréséhez. A felügyeltszolgáltatás-identitások kezelését az Azure automatikusan végzi. Ezek segítségével anélkül végezhet hitelesítést az Azure AD-hitelesítést támogató szolgáltatásokban, hogy be kellene ágyaznia a hitelesítő adatokat a kódba. 

Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Felhasználó által hozzárendelt felügyelt identitás létrehozása
> * Felhasználó által hozzárendelt identitás hozzárendelése Windows rendszerű virtuális géphez
> * Hozzáférés engedélyezése a felhasználó által hozzárendelt identitás számára az Azure Resource Manager erőforráscsoportjához 
> * Hozzáférési jogkivonat lekérése a felhasználó által hozzárendelt identitás használatával, majd az Azure Resource Manager meghívása a jogkivonat használatával 
> * Erőforráscsoport tulajdonságainak olvasása

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Bejelentkezés az Azure Portalra](https://portal.azure.com)

- [Windows rendszerű virtuális gép létrehozása](/azure/virtual-machines/windows/quick-create-portal)

- A jelen oktatóanyag elvégzéséhez szükséges erőforrás-létrehozási és szerepkör-felügyeleti lépések végrehajtásához a fiókjának „Tulajdonos” jogosultságokkal kell rendelkeznie a megfelelő hatókörben (az előfizetésben vagy az erőforráscsoportban). Ha segítségre van szüksége a szerepkör-hozzárendeléssel kapcsolatban, tekintse meg [Az Azure-előfizetések erőforrásaihoz való hozzáférés kezelése szerepköralapú hozzáférés-vezérléssel](/azure/role-based-access-control/role-assignments-portal) részben leírtakat.
- Ha a PowerShell helyi telepítése és használata mellett dönt, az oktatóanyaghoz az Azure PowerShell-modul 5.7.0-s vagy újabb verziójára lesz szükség. A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`. Ha frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](/powershell/azure/install-azurerm-ps) ismertető cikket. 
- Ha helyileg futtatja a PowerShellt, akkor emellett a következőket kell tennie: 
    - Futtassa a `Login-AzureRmAccount` parancsot, hogy kapcsolatot hozzon létre az Azure-ral.
    - Telepítse a [PowerShellGet legújabb verzióját](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Futtassa a következőt: `Install-Module -Name PowerShellGet -AllowPrerelease` a `PowerShellGet` modul kiadás előtti verziójának eléréséhez (előfordulhat, hogy a parancs futtatása után ki kell lépnie (`Exit`) az aktuális PowerShell-munkamenetből, hogy telepíteni tudja az `AzureRM.ManagedServiceIdentity` modult).
    - Futtassa az `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` parancsot az `AzureRM.ManagedServiceIdentity` modul kiadás előtti verziójának telepítéséhez, hogy elvégezhesse a cikkben szereplő, felhasználó által hozzárendelt identitással kapcsolatos műveleteket.

## <a name="create-a-user-assigned-identity"></a>Felhasználó által hozzárendelt identitás létrehozása

A felhasználó által hozzárendelt identitás különálló Azure-erőforrásként jön létre. Az Azure a [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) parancsmaggal egy identitást hoz létre az Azure AD-bérlőben, amely egy vagy több Azure-szolgáltatáspéldányhoz rendelhető hozzá.

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

A válasz tartalmazza az imént létrehozott, felhasználó által hozzárendelt identitás részleteit, az alábbi példához hasonlóan. Jegyezze fel a felhasználó által hozzárendelt identitás `Id` és `ClientId` értékét, mivel ezeket a következő lépésekben használni fogja:

```azurepowershell
{
Id: /subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1
ResourceGroupName : myResourceGroupVM
Name: ID1
Location: westus
TenantId: 733a8f0e-ec41-4e69-8ad8-971fc4b533f8
PrincipalId: e591178e-b785-43c8-95d2-1397559b2fb9
ClientId: af825a31-b0e0-471f-baea-96de555632f9
ClientSecretUrl: https://control-westus.identity.azure.net/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1/credentials?tid=733a8f0e-ec41-4e69-8ad8-971fc4b533f8&oid=e591178e-b785-43c8-95d2-1397559b2fb9&aid=af825a31-b0e0-471f-baea-96de555632f9
Type: Microsoft.ManagedIdentity/userAssignedIdentities
}
```

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Felhasználó által hozzárendelt identitás hozzárendelése Windows rendszerű virtuális géphez

A felhasználó által hozzárendelt identitást az ügyfelek több Azure-erőforrás esetében is használhatják. Az alábbi parancsokkal rendelhet felhasználó által hozzárendelt identitást egyetlen virtuális géphez. Ehhez használja az előző lépésben az `-IdentityID` paraméter esetében visszaadott `Id` tulajdonságot.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Hozzáférés engedélyezése a felhasználó által hozzárendelt identitás számára az Azure Resource Managerben lévő erőforráscsoporthoz 

Az Azure-erőforrások felügyelt identitásai segítségével a kód hozzáférési jogkivonatokat tud lekérni az olyan erőforrás API-k hitelesítéséhez, amelyek támogatják az Azure AD-hitelesítést. Ebben az oktatóanyagban a kódot az Azure Resource Manager API-jának eléréséhez használjuk. 

Ehhez azonban még engedélyeznie kell az identitás számára a hozzáférést az Azure Resource Manager erőforrásaihoz. Ebben az esetben arról az erőforráscsoportról van szó, amelyben a virtuális gép megtalálható. A környezetnek megfelelően frissítse a `<SUBSCRIPTION ID>` értékét.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

A válasz tartalmazza az imént létrehozott szerepkör-hozzárendelés részleteit, az alábbi példához hasonlóan:

```azurepowershell
RoleAssignmentId: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM/providers/Microsoft.Authorization/roleAssignments/f9cc753d-265e-4434-ae19-0c3e2ead62ac
Scope: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM
DisplayName: ID1
SignInName:
RoleDefinitionName: Reader
RoleDefinitionId: acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId: e591178e-b785-43c8-95d2-1397559b2fb9
ObjectType: ServicePrincipal
CanDelegate: False
```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Hozzáférési jogkivonat lekérése a VM identitásával, majd a Resource Manager meghívása a használatával 

Az oktatóanyag további részében a korábban létrehozott virtuális gépről dolgozunk.

1. Jelentkezzen be az Azure Portalra a [https://portal.azure.com](https://portal.azure.com) webhelyen

2. A portálon lépjen a **Virtuális gépek** szakaszra, lépjen a Windows rendszerű virtuális géphez, és az **Áttekintés** területen kattintson a **Csatlakozás** elemre.

3. Írja be a Windows rendszerű virtuális gép létrehozásakor használt **felhasználónevet** és **jelszót**.

4. Most, hogy létrehozott egy **távoli asztali kapcsolatot** a virtuális géppel, nyissa meg a **PowerShellt** a távoli munkamenetben.

5. A Powershell `Invoke-WebRequest` parancsával küldjön kérést az Azure-erőforrások helyi felügyeltidentitási végpontjára, hogy lekérjen egy hozzáférési jogkivonatot az Azure Resource Managerhez.  A `client_id` értéket a [felhasználó által hozzárendelt felügyelt identitás létrehozásakor](#create-a-user-assigned-identity) kapja vissza.

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=af825a31-b0e0-471f-baea-96de555632f9&resource=https://management.azure.com/' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Erőforráscsoport tulajdonságainak olvasása

Az Azure Resource Managert az előző lépésben lekért hozzáférési jogkivonattal érheti el, illetve azzal olvashatja annak az erőforráscsoportnak a tartalmát, amelyhez a felhasználó által hozzárendelt identitás számára hozzáférést adott. Cserélje le a(z) <SUBSCRIPTION ID> kifejezést a környezet előfizetés-azonosítójára.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
A válasz tartalmazza az adott erőforráscsoport adatait, az alábbi példához hasonlóan:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/myResourceGroupVM","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>További lépések

Ebben az oktatóanyagban megismerte, hogy hogyan hozhat létre felhasználó által hozzárendelt identitást, majd csatolhatja azt egy Azure-beli virtuális géphez az Azure Resource Manager API eléréséhez.  További információ az Azure Resource Managerről:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)
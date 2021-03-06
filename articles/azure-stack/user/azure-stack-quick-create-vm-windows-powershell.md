---
title: Windows virtuális gép létrehozása PowerShell használatával az Azure Stackben |} A Microsoft Docs
description: Windows virtuális gép létrehozása a PowerShell-lel az Azure Stackben.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 4795de2126a34907ecdec69e87a059dbadd0c3d0
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380386"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-by-using-powershell-in-azure-stack"></a>Gyors útmutató: Windows Server virtuális gép létrehozása az Azure Stack PowerShell használatával

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Létrehozhat egy Windows Server 2016 virtuális gép az Azure Stack PowerShell használatával. Kövesse a cikkben hozhat létre és használhat egy virtuális gépet. Ez a cikk is biztosít a lépéseket:

* Csatlakozzon a virtuális géphez a távoli ügyfélhez.
* Az IIS-webkiszolgáló telepítéséhez, és az alapértelmezett kezdőlap megtekintéséhez.
* Az erőforrások törlése.

>[!NOTE]
 Ha egy VPN-kapcsolaton keresztül kapcsolódik, ez a cikk az Azure Stack fejlesztői készletet, vagy a külső ügyfél Windows-alapú leírt lépések futtatásával.

## <a name="prerequisites"></a>Előfeltételek

* Győződjön meg arról, hogy az Azure Stack-operátorokról e hozzáadni a **Windows Server 2016** lemezképet az Azure Stack piactéren.

* Az Azure Stack egy adott verzióját az Azure PowerShell-lel az erőforrások létrehozásához és kezeléséhez szükséges. Ha nincs konfigurálva az Azure Stack PowerShell, kövesse a lépéseket [telepítése](azure-stack-powershell-install.md) PowerShell.

* Az Azure Stack PowerShell használatával állítsa be szüksége lesz az Azure Stack környezettel való kapcsolódáshoz. További útmutatást lásd: [csatlakozhat az Azure Stack PowerShell felhasználói](azure-stack-powershell-configure-user.md).

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Egy erőforráscsoport olyan logikai tároló, amely az Azure Stack erőforrások üzembe helyezése és felügyelt. A fejlesztői készlet vagy az Azure Stackkel integrált rendszereknél futtassa a következő kódblokk egy erőforráscsoport létrehozásához. A jelen dokumentum összes változót értékek vannak hozzárendelve, használja ezeket az értékeket, vagy rendelje hozzá az új értékekkel.

```powershell
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Tároló-erőforrások létrehozása

Hozzon létre egy tárfiókot, és a egy storage-tároló, a Windows Server 2016-ban tárolja.

```powershell
# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

# Create a storage container to store the virtual machine image
$containerName = 'osdisks'
$container = New-AzureStorageContainer `
  -Name $containerName `
  -Permission Blob
```

## <a name="create-networking-resources"></a>Hálózati erőforrások létrehozása

Hozzon létre egy virtuális hálózatot, egy alhálózatot és egy nyilvános IP-címet. Ezeket az erőforrásokat a virtuális gép hálózati csatlakoztatásának biztosítására szolgálnak.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Hálózati biztonsági csoport és hálózati biztonsági csoportszabály létrehozása

A hálózati biztonsági csoport bejövő és kimenő szabályok használatával teszi biztonságossá a virtuális gépet. Lehetővé teszi, hogy hozzon létre egy bejövő szabályt a 3389-es bejövő távoli asztali kapcsolatok engedélyezése és a egy bejövő szabály a bejövő webes forgalom engedélyezéséhez a 80-as porton.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRuleRDP `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRuleWWW `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Hálózati kártya létrehozása a virtuális géphez

A hálózati kártya csatlakoztatja a virtuális gépet egy alhálózathoz, egy hálózati biztonsági csoporthoz és egy nyilvános IP-címhez.

```powershell
# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

Hozzon létre egy virtuálisgép-konfigurációt. Ez a konfiguráció a virtuális gép üzembe helyezése során használt beállításait tartalmazza. Például: hitelesítő adatok, mérete és a virtuálisgép-lemezkép.

```powershell
# Define a credential object to store the username and password for the virtual machine
$UserName='demouser'
$Password='Password@123'| ConvertTo-SecureString -Force -AsPlainText
$Credential=New-Object PSCredential($UserName,$Password)

# Create the virtual machine configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_A1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Windows `
  -ComputerName "MainComputer" `
  -Credential $Credential

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "MicrosoftWindowsServer" `
  -Offer "WindowsServer" `
  -Skus "2016-Datacenter" `
  -Version "latest"

$osDiskName = "OsDisk"
$osDiskUri = '{0}vhds/{1}-{2}.vhd' -f `
  $StorageAccount.PrimaryEndpoints.Blob.ToString(),`
  $vmName.ToLower(), `
  $osDiskName

# Sets the operating system disk properties on a virtual machine.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the virtual machine.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-virtual-machine"></a>Csatlakozás a virtuális géphez

Hogy távolról jelentkezzen be a virtuális gépet az előző lépésben létrehozott nyilvános IP-címének kell. A következő parancsot a virtuális gép nyilvános IP-címének lekéréséhez:

```powershell
Get-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName | Select IpAddress
```

A következő paranccsal hozzon létre egy távoli asztali munkamenetet a virtuális géppel. Cserélje le az IP-címet a virtuális gépe nyilvános IP-címére. Amikor a rendszer kéri, adja meg a felhasználónevet és jelszót, amely a virtuális gép létrehozásakor használt.

```powershell
mstsc /v <publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>IIS telepítése a PowerShell használatával

Miután bejelentkezett az Azure-beli virtuális gépre, egyetlen PowerShell-utasítással telepítheti az IIS-t, és engedélyezheti, hogy a helyi tűzfalszabály átengedje a webforgalmat. Nyisson meg egy PowerShell-parancssort, és futtassa a következő parancsot:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>Az IIS kezdőlapjának megtekintése

A telepített IIS-t, és nyissa meg a virtuális Gépen a 80-as porton egy tetszőleges webböngészőben használhatja az alapértelmezett IIS kezdőlapjának megtekintése. Használja a *publicIpAddress* az előző szakaszban, és látogasson el az alapértelmezett oldalt a dokumentált.

![Alapértelmezett IIS-webhely](./media/azure-stack-quick-create-vm-windows-powershell/default-iis-website.png)

## <a name="delete-the-virtual-machine"></a>A virtuális gép törlése

Ha már nincs rá szükség, a következő paranccsal eltávolítható az erőforráscsoport, amely tartalmazza a virtuális gép és a kapcsolódó erőforrásokkal:

```powershell
Remove-AzureRmResourceGroup `
  -Name $ResourceGroupName
```

## <a name="next-steps"></a>További lépések

Ez a rövid útmutatóban egy egyszerű Windows virtuális gépet helyezte. További információ a virtuális gépekről az Azure Stack, továbbra is [szempontok a virtuális gépek az Azure Stackben](azure-stack-vm-considerations.md).

---
title: Windows Server az Azure hibrid juttatása |} Microsoft Docs
description: Útmutató a Windows Software Assurance előnyeit, hogy helyszíni licencek Azure maximalizálása
services: virtual-machines-windows
documentationcenter: ''
author: kmouss
manager: jeconnoc
editor: ''
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/22/2017
ms.author: kmouss
ms.openlocfilehash: bfad3ff71be07026cf4a7dd6ad75df399349f844
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/28/2018
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Azure Hybrid Benefit Windows Serverhez
Software Assurance rendelkező ügyfelek Azure hibrid juttatás for Windows Server lehetővé teszi a helyszíni Windows Server licencek használja, és futtassa a Windows virtuális gépek Azure költséghatékony. Azure hibrid juttatás for Windows Server bármelyik új virtuális gépek telepítéséhez használható Azure platform Windows Server-lemezképet Windows egyéni támogatott. Ez a cikk halad keresztül lépéseket, a új virtuális gépek Azure hibrid juttatás for Windows Server telepítése, és hogyan frissíthet meglévő futtató virtuális gépek. További információ az Azure hibrid juttatás for Windows Server licencelési és pluszköltséggel kevésbé kell kihasználni, tekintse meg a [Azure hibrid juttatás for Windows Server licencelési oldal](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!IMPORTANT]
> Az örökölt "[HUB]" Windows Server-lemezképekben Azure piactéren nagyvállalati szerződéssel rendelkező ügyfelek esetén közzétett től 9/11/2017 eltávolították, a szabványos Windows Server használata a "Mentés pénz" lehetőséget, a portál az Azure hibrid juttatása Windows Server. További információkért tekintse meg a [cikk.](https://support.microsoft.com/en-us/help/4036360/retirement-azure-hybrid-use-benefit-images-for-ea-subscriptions)
>

> [!NOTE]
> Azure hibrid juttatás for Windows Server használata virtuális gépek, amelyek van szó, például az SQL Server vagy a külső piactéren elérhető rendszerkép bármelyikét további szoftvereket van megkezdődött. Ha egy 409 hibaüzenet jelenik meg, mint: tulajdonságának módosítása nem engedélyezett 'LicenseType'; majd kívánt alakítható, vagy egy új Windows Server virtuális gép üzembe helyezése, amely rendelkezik a költség, további szoftver, amely nem támogatja az adott régióban. Azonos, ha megpróbálja keresse meg a portál konfigurációjának beállítást ehhez a konvertálás, és nem látható ezt a virtuális gépet.
>


> [!NOTE]
> Klasszikus virtuális gépekhez csak az új virtuális gép helyszíni egyéni lemezképek telepítése esetén támogatott. Az ebben a cikkben támogatott képességek kihasználásához, először át kell klasszikus virtuális gépek erőforrás-kezelő modellhez.
>


## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Azure hibrid juttatás for Windows Server használatának módjai
Windows virtuális gépek használata az Azure hibrid juttatás néhány módja van:

1. Telepíthet egy, a megadott virtuális gépek [Windows Server lemezképeket az Azure piactéren](#https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)
2. Is [feltöltése egy egyéni VM](#upload-a-windows-vhd) és [telepítése a Resource Manager-sablon használatával](#deploy-a-vm-via-resource-manager) vagy [Azure PowerShell](#detailed-powershell-deployment-walkthrough)
3. Váltás és konvertálja a meglévő virtuális gép közötti Azure hibrid juttatás fut vagy igény szerinti költségek ellenében Windows Server
4. Ezenkívül telepítheti egy új virtuálisgép-méretezési Azure hibrid előnye a Windows Server beállítása

> [!NOTE]
> Egy meglévő virtuálisgép-méretezési Azure hibrid juttatás for Windows Server használandó konvertálása nem támogatott
>

## <a name="deploy-a-vm-from-a-windows-server-marketplace-image"></a>Telepítse a virtuális Gépet egy Windows Server Piactéri lemezképből
Az Azure piactérről rendelkezésre álló összes Windows Server-lemezképekben Azure hibrid előnye a Windows Server engedélyezve van. Például Windows Server 2016-os, a Windows Server 2012R2, a Windows Server 2012 és a Windows Server 2008SP1 és egyéb. Ezeket a lemezképeket segítségével közvetlenül az Azure-portálon, a Resource Manager sablonok, a Azure PowerShell vagy a más SDK a virtuális gépek telepítéséhez.

Telepítheti ezeket a lemezképeket, közvetlenül az Azure portálról. A Resource Manager-sablonok és az Azure PowerShell használatra listájának megtekintéséhez képek az alábbiak szerint:

### <a name="powershell"></a>PowerShell
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
A lépések segítségével [Windows virtuális gép létrehozása a PowerShell-lel](#https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) , és adja át a LicenseType = "Windows_Server". Ez a beállítás lehetővé teszi a meglévő Windows Server-licenc használatát az Azure-on.

### <a name="portal"></a>Portál
A lépések segítségével [Windows virtuális gép létrehozása az Azure portálon](#https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) és a beállítással használhatja a meglévő Windows Server-licencét.

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Azure hibrid juttatás for Windows Server egy meglévő virtuális gép átalakítása
Ha egy meglévő virtuális Gépet, amely szeretné átalakítása Azure hibrid juttatás for Windows Server előnyeit, frissítheti a virtuális gép licenc típusa az alábbiak szerint:

### <a name="convert-to-using-azure-hybrid-benefit-for-windows-server"></a>Azure hibrid előnye a Windows Server használatával átalakítása
```powershell
$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
$vm.LicenseType = "Windows_Server"
Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### <a name="convert-back-to-pay-as-you-go"></a>Vissza kell fizetnie menet átalakítása
```powershell
$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
$vm.LicenseType = "None"
Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### <a name="portal"></a>Portál
Portálról VM panelen frissítse a virtuális gép használata a Azure hibrid kiszolgálóterhelések használják ki a "Konfiguráció" lehetőség kiválasztásával és az "Azure hibrid előnyeit" beállítás bekapcsolására

> [!NOTE]
> A váltáshoz "Azure hibrid juttatás" a "Konfiguráció" beállítás nem jelenik meg, ha van, mert az átalakítás még nem támogatott a kiválasztott Virtuálisgép-típus (például a virtuális gépek beépített egyéni lemezképet, illetve egy olyanra, amely rendelkezik további fizetős szoftverek, például az SQL Server vagy Az Azure piactér harmadik felek szoftvereivel).
>

## <a name="upload-a-windows-server-vhd"></a>A Windows Server VHD feltöltése
Az Azure-ban a Windows Server virtuális gép üzembe helyezéséhez először az alap Windows-buildet tartalmazó virtuális merevlemez létrehozása. Ehhez a virtuális Merevlemezhez kell megfelelően készíteni a Sysprep segítségével az Azure-ba való feltöltés előtt. Is [további tudnivalókat a virtuális merevlemez követelményeiről és a Sysprep folyamat](upload-generalized-managed.md) és [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Készítsen biztonsági másolatot a virtuális Gépet a Sysprep futtatása előtt. 

Egyszer előkészítette a VHD-t, a VHD-fájlt feltölti az Azure Storage-fiókot az alábbiak szerint:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server és Dynamics is használhatja a frissítési garancia licencelési. Kell elő kell készíteni a Windows Server kép az alkalmazás-összetevők telepítése és ennek megfelelően biztosító licenckulcsot, akkor a lemezképet feltöltése az Azure-bA. Tekintse át a Sysprep futtatása az alkalmazással, például a megfelelő dokumentációja [telepítése az SQL Server Sysprep használatával szempontjai](https://msdn.microsoft.com/library/ee210754.aspx) vagy [a SharePoint Server 2016 referencia lemezkép (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).
>
>

Akkor is olvashat további tudnivalókat [a virtuális merevlemez feltöltése az Azure folyamat](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)

## <a name="deploy-a-vm-via-resource-manager-template"></a>A virtuális gép keresztül Resource Manager-sablon üzembe helyezése
A Resource Manager-sablonok, egy további paraméter belül `licenseType` meg kell adni. További tudnivalók [Azure Resource Manager sablonok készítése](../../resource-group-authoring-templates.md). Miután a virtuális merevlemez feltöltése az Azure-ba, szerkesztése, a licenc típusa a számítási szolgáltató részeként és a szokásos módon sablon üzembe helyezése a Resource Manager-sablon:

```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>Telepítse a virtuális Gépet keresztül PowerShell gyors üzembe helyezés
A Windows Server virtuális gép PowerShell használatával való telepítésekor, egy további paramétere lehet `-LicenseType`. Miután a virtuális merevlemez feltöltése az Azure-bA rendelkezik, hozzon létre egy virtuális gép használatával `New-AzureRmVM` és az alábbiak szerint adja meg a licencelési típusát:

A Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

El tudja olvasni a különböző lépéseket leíró útmutató [erőforrás-kezelő és a PowerShell használatával Windows virtuális gép létrehozása](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Ellenőrizze, hogy a virtuális Gépet a licencelési előnye van használata
A virtuális Gépet, vagy PowerShell, a Resource Manager-sablon vagy a portálon keresztül üzembe helyezett ellenőrizheti a licenc típusa a `Get-AzureRmVM` az alábbiak szerint:

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

A Windows Server az alábbi példához hasonló kimenete:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Ezzel szemben a hagyományosabb kimeneti a a következő virtuális Géphez nem Azure hibrid juttatás for Windows Server licencelési:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="list-all-azure-hybrid-benefit-for-windows-server-vms-in-a-subscription"></a>Minden Azure hibrid juttatás a Windows Server virtuális gép egy előfizetésben felsorolása

Telepített Azure hibrid előnye a Windows Server összes virtuális gépek száma és megtekintéséhez az előfizetésből futtathatja a következő parancsot:

```powershell
$vms = Get-AzureRMVM 
foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>A virtuálisgép-méretezési állítható be Azure hibrid juttatás for Windows Server telepítése
A virtuális gépen belüli virtuális gépek méretezési csoportjának Resource Manager-sablonok, egy további paraméter `licenseType` meg kell adni. További tudnivalók [Azure Resource Manager sablonok készítése](../../resource-group-authoring-templates.md). A Resource Manager sablon tartalmazza a licenseType tulajdonságot a méretezési virtualMachineProfile részeként és a szokásos módon sablon telepítése – lásd a következő példa 2016 Windows kiszolgálói lemezkép használatával történő szerkesztése:


```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```
Emellett [létrehozása és központi telepítése egy virtuálisgép-méretezési csoport](#https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-create) és a LicenseType tulajdonság

## <a name="next-steps"></a>További lépések
Tudjon meg többet az [Azure hibrid előnyökkel pénz mentése](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

További információ [Azure hibrid juttatás a Windows Server licencelési részletes útmutatást](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit)

További információ [használatával Resource Manager-sablonok](../../azure-resource-manager/resource-group-overview.md)

További információ [Azure hibrid juttatás for Windows Server és az Azure Site Recovery ellenőrizze áttelepítése alkalmazások az Azure-bA még több költséghatékony](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)

További információ [Azure több-Bérlős üzemeltető jogosultsággal rendelkező Windows 10-re](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)

Tudjon meg többet az [gyakran ismételt kérdések](#https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/)

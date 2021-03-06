---
title: Felügyelt Azure virtuális gép létrehozása a helyszíni általánosított virtuális merevlemezből |} A Microsoft Docs
description: Általános VHD feltöltése az Azure-ba, és ezzel hozzon létre új virtuális gépeket, a Resource Manager-alapú üzemi modellben.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/26/2018
ms.author: cynthn
ms.openlocfilehash: 8fd88a0e3c5b387ce3ea586f6f23b3643a03e58d
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618167"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Általános VHD feltöltése és ezzel hozzon létre új virtuális gépeket az Azure-ban

Ez a témakör végigvezeti egy általánosított virtuális gép VHD feltöltése az Azure-ba, a rendszerkép létrehozása a VHD-ből és a egy új virtuális gép létrehozása a rendszerképből PowerShell használatával. Feltölthet egy VHD-t egy helyszíni virtualizálási eszközből vagy egy másik felhőben exportált. Használatával [Managed Disks](managed-disks-overview.md) az új virtuális gép egyszerűbbé teszi a virtuális gép kezelése és jobb rendelkezésre állást biztosít, amikor a virtuális Gépet helyez egy rendelkezésre állási csoportban. 

Ha szeretne egy mintaszkriptet használja, lásd: [mintaparancsfájl VHD feltöltése az Azure-ba, és a egy új virtuális gép létrehozása](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Előkészületek

- Minden olyan VHD feltöltése az Azure-ba, mielőtt követendő [Windows VHD vagy VHDX feltöltése az Azure előkészítése](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Felülvizsgálat [tervezze meg a migrálás a Managed Disks szolgáltatásba](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) az áttelepítés megkezdése előtt [Managed Disks](managed-disks-overview.md).
- Ez a cikk az AzureRM modul 5.6-os vagy újabb verziójára van szükség. A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM.Compute`. Ha frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](/powershell/azure/install-azurerm-ps) ismertető cikket.


## <a name="generalize-the-source-vm-using-sysprep"></a>A forrásoldali virtuális gép általánosítása a Sysprep használatával

A Sysprep többek között minden személyes fiókadatot eltávolít, a gépet pedig előkészíti rendszerképként való használatra. További információ a Sysprepről: a [Sysprep áttekintése](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Ellenőrizze, hogy a Sysprep a gépen futó kiszolgálói szerepkörök támogatottak. További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Ha a virtuális merevlemez feltöltése az Azure-ban először előtt futtatja a Sysprep, ellenőrizze, hogy [készíteni a virtuális gép](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt. 
> 
> 

1. Jelentkezzen be a Windows virtuális gép.
2. Nyissa meg a parancsablakot rendszergazdaként. Módosítsa a könyvtárat a **%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.
3. A **Rendszer-előkészítő eszköz** párbeszédpanelen válassza **A kezdőélmény indítása** lehetőséget, és győződjön meg róla, hogy be van-e jelölve az **Általánosítás** jelölőnégyzet.
4. A **leállítási beállítások**válassza **leállítási**.
5. Kattintson az **OK** gombra.
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. A Sysprep a feladat befejezése után leállítja a virtuális gépet. Ne indítsa újra a virtuális Gépet.


## <a name="get-the-storage-account"></a>A storage-fiók létrehozása

Az Azure-ban tárolja a virtuális gép feltöltött kép egy tárfiókra van szükség. Használjon egy meglévő tárfiókot, vagy hozzon létre egy újat. 

Fog használni a virtuális merevlemez létrehozása egy felügyelt lemezt egy virtuális gép, ha a tárfiók helye kell egyeznie a hely, ahol létrehozni a virtuális Gépet.

A rendelkezésre álló tár fiókokat jeleníti meg, írja be:

```azurepowershell
Get-AzureRmStorageAccount | Format-Table
```

## <a name="upload-the-vhd-to-your-storage-account"></a>A virtuális lemezek feltöltése a storage-fiókba

Használja a [Add-AzureRmVhd](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd) parancsmaggal töltse fel a VHD-t a tárfiókban lévő tárolóba. Ez a példa feltölti a fájlt *myVHD.vhd* a *"C:\Users\Public\Documents\Virtual merevlemezek\"*  tárfiókhoz nevű *mystorageaccount* a a *myResourceGroup* erőforráscsoportot. A fájlt a tárolóba nevű kerülnek *mycontainer* és az új fájl neve lesz *myUploadedVHD.vhd*.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Sikeres művelet esetén olyan ehhez hasonló választ kap:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

A hálózati kapcsolat és a VHD-fájl méretétől függően ez a parancs is igénybe vehet igénybe

### <a name="other-options-for-uploading-a-vhd"></a>Más beállításokat a virtuális merevlemez feltöltése
 
Emellett feltölthet egy virtuális Merevlemezt a tárfiókhoz, használja az alábbi lehetőségek közül:

- [AzCopy](http://aka.ms/downloadazcopy)
- [Az Azure Storage Blob másolásához API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Az Azure Storage Explorer Blobok feltöltése](https://azurestorageexplorer.codeplex.com/)
- [Storage Import/Export szolgáltatás REST API-referencia](https://msdn.microsoft.com/library/dn529096.aspx)
-   Import/Export szolgáltatás használatát, ha a 7 napnál hosszabb ideje feltöltése, becsült javasoljuk. Használhat [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) megbecsülni az adatok mérete és átviteli egység időpontját. 
    Importálási/exportálási átmásolása a standard szintű tárfiók is használható. Szüksége lesz egy eszköz, például az AzCopy használata a premium storage-fiók átmásolása standard storage-ból.

> [!IMPORTANT]
> Ha a virtuális merevlemez feltöltése az Azure-bA az AzCopy használ, ellenőrizze, hogy meg van [/BlobType:page](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) -szkript feltöltése futtatása előtt. Ha a cél egy blobot, és ez a beállítás nincs megadva, alapértelmezés szerint az AzCopy egy blokkblobot hoz létre.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Létrehozhat egy felügyelt rendszerképet, a feltöltött virtuális merevlemezből 

Hozzon létre egy felügyelt rendszerképet, a operációs rendszer általánosított virtuális merevlemez használatával. Cserélje le az értékeket a saját adatait.


Először állítsa be az egyes paraméterek:

```powershell
$location = "East US" 
$imageName = "myImage"
```

A lemezkép a operációs rendszer általánosított virtuális merevlemez létrehozása.

```powershell
$imageConfig = New-AzureRmImageConfig `
   -Location $location
$imageConfig = Set-AzureRmImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzureRmImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```


## <a name="create-the-vm"></a>Virtuális gép létrehozása

Most, hogy már van egy rendszerképe, létrehozhat belőle egy vagy több új virtuális gépet. Ez a példa létrehoz egy virtuális gép nevű *myVM* származó a *myImage*, a a *myResourceGroup*.


```powershell
New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -ImageName $imageName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```


## <a name="next-steps"></a>További lépések

Jelentkezzen be az új virtuális gépet. További információkért lásd: [csatlakoztatása és bejelentkezés Windows rendszert futtató Azure virtuális gép](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


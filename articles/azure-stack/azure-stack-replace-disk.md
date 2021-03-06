---
title: Cserélje le egy fizikai lemezt az Azure Stackben |} A Microsoft Docs
description: Az Azure Stack fizikai lemez cseréje folyamatának ismertetése.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.openlocfilehash: 7ce501be5458282273e51a5b2bc18482592d2333
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44376952"
---
# <a name="replace-a-physical-disk-in-azure-stack"></a>Az Azure Stack fizikai lemez cseréje

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Ez a cikk ismerteti az Azure Stack fizikai lemez cseréje általános folyamata. Ha a fizikai lemez meghibásodik, cserélje le a lehető legrövidebb időn belül.

Ezt az eljárást követve integrált rendszerek, valamint development kit központi telepítésekhez, amelyek gyakran használt adatok rétegére – cserélhető lemezekkel rendelkeznek.

Tényleges lemezcsere lépései eltérőek lesznek a számítógépgyártó (OEM) hardver szállítójával alapján. Konkrétan a rendszer a részletes lépéseket a gyártója által biztosított mező telepen cserélhető egység (FRU) dokumentációjában talál. 

## <a name="review-disk-alert-information"></a>Lemez riasztási információk áttekintése
Ha egy lemez meghibásodik, kap egy riasztást, mely arra kéri, hogy kapcsolat megszakadt-e egy fizikai lemezt. 

 ![Riasztás megjelenítése megszakad a fizikai lemez](media/azure-stack-replace-disk/DiskAlert.png)

Megnyitja a riasztás, ha a riasztás leírása tartalmazza a skálázási egység csomópont és a lemezre, le kell cserélnie fizikai tárolóhely pontos helyét. Az Azure Stack további segít azonosítani a meghibásodott lemezt LED kijelző képességek segítségével.

 ## <a name="replace-the-disk"></a>Cserélje le a lemezt

Utasítások a OEM hardver szállítójával FRU tényleges lemezt kell cserélni.

> [!note]
> Cserélje le a lemezeket egy skálázási egység csomópont egyszerre. Várjon, amíg a virtuális lemez helyreállítási feladat befejeződését, mielőtt továbblép a következő skálázási egység csomópont

Letilthatja az egy nem támogatott lemez egy integrált rendszer, a rendszer blokkolja a lemezek, a szállító által nem támogatott. Meg nem támogatott lemez használata, ha egy új riasztás kiderül, hogy, hogy az egy lemezt egy nem támogatott modell vagy a belső vezérlőprogram miatt karanténba-e.

A lemezt cserél ki, az Azure Stack automatikusan felderíti az új lemezt, és elindítja a Virtuálislemez-javítási folyamatot.  
 
 ## <a name="check-the-status-of-virtual-disk-repair"></a>A Virtuálislemez-javítási állapot ellenőrzése
 
 Követően a lemezt cserél ki, a virtuális lemez állapot figyelése, és javítsa ki a feladat állapotát a rendszerjogosultságú végpont használatával. Kövesse az alábbi lépéseket bármely olyan számítógépről, amelyen a kiemelt végponthoz való hálózati kapcsolatot.

1. Nyisson meg egy Windows PowerShell-munkamenetet, és a kiemelt végponthoz kapcsolódjon.
    ````PowerShell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```` 
  
2. Futtassa a virtuális lemez állapotának megtekintéséhez a következő parancsot:
    ````PowerShell
        Get-VirtualDisk -CimSession s-cluster
    ````
   ![PowerShell Get-VirtualDisk parancs kimenete](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Futtassa a storage-feladat aktuális állapotának megtekintéséhez a következő parancsot:
    ```PowerShell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ````
      ![PowerShell Get-StorageJob parancs kimenete](media/azure-stack-replace-disk/GetStorageJobOutput.png)

## <a name="troubleshoot-virtual-disk-repair"></a>Virtuális lemezek javítási hibaelhárítása

Ha a virtuális lemezek javítása feladat jelenik meg elakadt, indítsa újra a feladatot, a következő parancsot:
  ````PowerShell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ```` 

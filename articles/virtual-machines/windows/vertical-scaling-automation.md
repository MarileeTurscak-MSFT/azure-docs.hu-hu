---
title: Azure Automation függőleges méretezése a Windows virtuális gépek használata |} Microsoft Docs
description: Függőleges méretezhető figyelési riasztásokhoz adható az Azure Automation szolgáltatásban válaszul egy Windows rendszerű virtuális gép
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abbbcce2ae72ac77ba6f6c21e626b34bcded7909
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30918421"
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Függőleges méretezése a Windows-alapú virtuális gépek az Azure Automation szolgáltatásban

Függőleges skálázás bővítése vagy csökkentése az erőforrásokat, a munkaterhelés válaszul gép során a rendszer. Az Azure-ban ehhez a virtuális gép méretének módosításával. Ez segítheti a következő esetekben

* Ha a virtuális gép nem gyakran használják, méretét, a havi költségek csökkentése érdekében kis méretben le
* Ha a virtuális gép egy csúcsterhelés lát, azt is átméretezhetők nagyobb méretűre a kapacitás növelése

Ehhez a lépéseket a Vázlat van, mint alatt

1. A virtuális gépek eléréséhez Azure Automation beállítása
2. Az Azure Automation függőleges méretezési runbookok importálnia kell az előfizetéshez
3. A webhook hozzáadása a runbookhoz
4. Adjon hozzá egy riasztást a virtuális géphez

> [!NOTE]
> Az első virtuális gépen, az azt is méretezhető, méretek mérete miatt előfordulhat, hogy korlátozva lesz, mert a rendelkezésre állási a fürt más méretű aktuális virtuális gép üzemel. A közzétett automatizálási runbookok a cikk ezt használja azt ebben az esetben mi gondoskodunk, és csak méretezése belül a virtuális gép mérete párok alatt. Ez azt jelenti, hogy, hogy egy Standard_D1v2 virtuális gép lesz hirtelen nem standard szintű, G5 akár méretezhető vagy Basic_A0 méretezhető.
> 
> | Virtuálisgép-méretek pár skálázás |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1v2 |Standard_D5v2 |
> | Standard_D11v2 |Standard_D14v2 |
> | Standard_G1 |Standard szintű, G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>A virtuális gépek eléréséhez Azure Automation beállítása
Először is végre kell hajtani, hozzon létre egy Azure Automation-fiók, amely a virtuális gépek méretezési használt runbookok tárolni fogja. Az Automation szolgáltatás új a "Futtatás mint fiók" szolgáltatás, amely lehetővé teszi a beállítás a szolgáltatás egyszerű fel automatikusan a runbookok futtatásáért a felhasználó nevében nagyon egyszerű. További információt az alábbi cikkben:

* [Runbookok hitelesítése Azure-beli futtató fiókkal](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Az Azure Automation függőleges méretezési runbookok importálnia kell az előfizetéshez
A runbookok, amelyek szükségesek a virtuális gép függőleges skálázás vannak már közzé van téve az Azure Automation-Runbook katalógus. Szüksége lesz az előfizetés importálja azokat. Megismerheti a runbookok importálása a következő cikk elolvasása.

* [Az Azure Automation forgatókönyv- és minták](../../automation/automation-runbook-gallery.md)

A runbookokat, importálandók kell az alábbi képen látható

![Runbookok importálása](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>A webhook hozzáadása a runbookhoz
Miután importált a runbookok kell hozzáadása egy webhook a runbookhoz úgy is elindítható a virtuális gép riasztások alapján. A webhook létrehozása a runbook részleteit itt olvasható

* [Azure Automation webhooks](../../automation/automation-webhooks.md)

Ellenőrizze, hogy a webhook másolja át a webhook párbeszédpanel bezárása, szüksége lesz a következő szakaszban előtt.

## <a name="add-an-alert-to-your-virtual-machine"></a>Adjon hozzá egy riasztást a virtuális géphez
1. Válassza ki a virtuális gép beállításait
2. Válassza ki a "Riasztási szabályok"
3. Válassza a "Riasztás hozzáadása"
4. Jelöljön ki egy metrikát az érvényesítést a riasztás a
5. Válassza ki azt a feltételt, amely teljesítése lesz miatt a riasztás az érvényesítést
6. Válassza ki a feltétel küszöbértéket az 5. lépés. teljesítendő
7. A feltétel és a küszöbérték felett, amelyek a figyelési szolgáltatás ellenőrzi időszak kiválasztása lépéseket 5 és 6
8. Illessze be a webhook előző szakaszából másolt.

![1 virtuális gép értesítés hozzáadása](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Riasztás hozzáadása a virtuális gép 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)


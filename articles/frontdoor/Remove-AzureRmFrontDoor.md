---
title: Azure bejárati ajtajának szolgáltatás – PowerShell-referencia |} A Microsoft Docs
description: Ez az útmutató részletesen a különböző Azure bejárati ajtajának szolgáltatás támogatott PowerShell-parancsmagok
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2018
ms.author: sharadag
Module Name: AzureRM.FrontDoor
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/remove-azurermfrontdoor
schema: 2.0.0
ms.openlocfilehash: ddc6a40521b951c8083ac185368621ea792b1c85
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048231"
---
# Remove-AzureRmFrontDoor
Bejárati ajtajának terheléselosztó eltávolítása

## SZINTAXIS

### ByFieldsParameterSet (alapértelmezett)
```
Remove-AzureRmFrontDoor -ResourceGroupName <String> -Name <String> [-PassThru]
 [-DefaultProfile <IAzureContextContainer>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByObjectParameterSet
```
Remove-AzureRmFrontDoor -InputObject <PSFrontDoor> [-PassThru] [-DefaultProfile <IAzureContextContainer>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ResourceIdParameterSet
```
Remove-AzureRmFrontDoor -ResourceId <String> [-PassThru] [-DefaultProfile <IAzureContextContainer>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## LEÍRÁS
A **Remove-AzureRmFrontDoor** parancsmag eltávolítja az aktuális előfizetésben bejárati ajtajának load balancer

## PÉLDÁK

### 1. példa: Távolítsa el a "frontdoor1" erőforrás csoport "rg1" az aktuális előfizetésben található.
```powershell
PS C:\> Remove-AzureRmFrontDoor -Name "frontdoor1" -ResourceGroupName "rg1"
```

Távolítsa el a "frontdoor1" erőforrás csoport "rg1" az aktuális előfizetésben található.

### 2. példa: Távolítsa el az erőforrás csoport "rg1" az aktuális előfizetéshez tartozó összes FrontDoors.
```powershell
PS C:\> Get-AzureRmFrontDoor -ResourceGroupName "rg1" | Remove-AzureRmFrontDoor
```

Távolítsa el az erőforrás csoport "rg1" az aktuális előfizetéshez tartozó összes FrontDoors.

### 3. példa: Távolítsa el az aktuális előfizetéshez tartozó összes FrontDoors.
```powershell
PS C:\> Get-AzureRmFrontDoor | Remove-AzureRmFrontDoor
```

Távolítsa el az aktuális előfizetéshez tartozó összes FrontDoors.

### 4. példa: Távolítsa el a neve "frontdoor1" az aktuális előfizetéshez tartozó összes FrontDoors.
```powershell
PS C:\> Remove-AzureRmFrontDoor -Name "frontdoor1" | Remove-AzureRmFrontDoor
```

Távolítsa el a neve "frontdoor1" az aktuális előfizetéshez tartozó összes FrontDoors.

## PARAMÉTEREK

### -DefaultProfile
A hitelesítő adatokat, fiók, bérlői és az Azure-ral való kommunikációhoz használt előfizetés.

```yaml
Type: Microsoft.Azure.Commands.Common.Authentication.Abstractions.IAzureContextContainer
Parameter Sets: (All)
Aliases: AzureRmContext, AzureCredential

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject
A törölni kívánt bejárati ajtajának objektum.

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSFrontDoor
Parameter Sets: ByObjectParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Név
Törli a bejárati ajtó neve.

```yaml
Type: System.String
Parameter Sets: ByFieldsParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru
(Ha az meg van adva) objektumot ad vissza.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResourceGroupName
Az erőforráscsoport, amelyek kezdettől fogva tartozik.

```yaml
Type: System.String
Parameter Sets: ByFieldsParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### Erőforrás-azonosítója
Erőforrás-azonosítóját a bejárati ajtó törlése

```yaml
Type: System.String
Parameter Sets: ResourceIdParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Megerősítése
A parancsmag futtatása előtt megerősítést fog kérni.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf
Megmutatja, hogy mi történne a parancsmag futtatásakor.
A parancsmag nem fut.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
Ez a parancsmag a következő általános paramétereket támogatja: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction és -WarningVariable. További információk: about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

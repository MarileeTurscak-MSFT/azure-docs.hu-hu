---
title: Az Azure InfoBox felhasználói felületi elemben |} A Microsoft Docs
description: Az Azure portal a Microsoft.Common.TextBlock felhasználói felületi elem ismerteti.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: abd1329f2ebac90bf846dfd5fc5b307ddb5e52bd
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095480"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Microsoft.Common.InfoBox felhasználói felületi elemben
Egy olyan vezérlőelem, amely hozzáad egy információs mező. A box fontos szöveget tartalmaz, vagy figyelmeztetések, amelyek segítségével a felhasználók a tartalmat értékek ismertetése. Azt is kapcsolat egy URI-t további információt.

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>Séma
```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="remarks"></a>Megjegyzések

* A `icon`, használjon **nincs**, **Info**, **figyelmeztetés**, vagy **hiba**.
* A `uri` tulajdonság nem kötelező.

## <a name="sample-output"></a>Példa kimenet

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="next-steps"></a>További lépések
* Felhasználóifelület-definíciók létrehozása bevezetésért lásd: [CreateUiDefinition használatának első lépései](create-uidefinition-overview.md).
* Egy felhasználói felületi elemeket általános tulajdonságok leírásáért lásd: [CreateUiDefinition elemek](create-uidefinition-elements.md).

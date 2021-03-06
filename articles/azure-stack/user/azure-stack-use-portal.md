---
title: Az Azure Stack portal használatával |} A Microsoft Docs
description: Ismerje meg, hogyan elérése és a felhasználói portál használata az Azure Stackben.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: mabrigg
ms.reviewer: efemmano
ms.openlocfilehash: 7ccf746cfd9645b63c2810bff83cfb0c4fa55d65
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713738"
---
# <a name="use-the-azure-stack-portal"></a>Az Azure Stack-portál használata

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Az Azure Stack-portál használatával előfizethetnek azokra nyilvános, és használhatja a szolgáltatásokat, amelyek ezeket az ajánlatokat nyújtanak. Ha korábban már használta a globális Azure-portálon, akkor már ismeri az a hely működését.

## <a name="access-the-portal"></a>A portál elérése

Az Azure Stack-operátorokról (a szolgáltató vagy a rendszergazda a szervezet), lehetővé teszi, hogy a helyes URL-portál eléréséhez.

- Egy integrált rendszer, az URL-címet az operátor régió és külső tartománynév függően változik, és a következő formátumban lesz https://portal.&lt; *régió*&gt;.&lt; *FQDN*&gt;.
- Ha az Azure Stack fejlesztői készletet használ, a portál címe https://portal.local.azurestack.external.

![Képernyőfelvétel az Azure Stack felhasználói portál](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Az irányítópult testreszabásával

Az irányítópult csempéi alapértelmezett készletét tartalmazza. Választhat **irányítópult szerkesztése** módosítsa az alapértelmezett irányítópult, vagy válasszon **új irányítópult** egyéni irányítópult létrehozása. Könnyen testre szabhat egy irányítópultot hozzáadásával vagy eltávolításával a csempéket. Jelölje be például a számítási csempe hozzáadása **+ erőforrás létrehozása**. Kattintson a jobb gombbal **számítási**, majd válassza ki **rögzítés az irányítópulton**.

## <a name="create-subscription-and-browse-available-resources"></a>Előfizetés létrehozása, és keresse meg a rendelkezésre álló erőforrások

Ha még nincs előfizetése, először is szüksége, előfizetés egy ajánlatra. Ezt követően keresse meg az elérhető erőforráskorlátokat. Keresse meg és -erőforrások létrehozása, az alábbi módszerek bármelyikét használhatja:

- Válassza ki a **Marketplace** csempét az irányítópulton.
- Az a **összes erőforrás** csempéről válassza **létre erőforrásokat**.
- A bal oldali navigációs ablaktáblán válassza ki a **+ erőforrás létrehozása**.

## <a name="learn-how-to-use-available-services"></a>Ismerje meg, hogyan használhatja az elérhető szolgáltatások

Ha az elérhető szolgáltatások használatára vonatkozó útmutatást, is előfordulhatnak más lehetőségek elérhető.

- A szervezet vagy a szolgáltató rendelkezhetnek a saját dokumentációját, amely általában a helyzet akkor, ha testre szabott szolgáltatások vagy alkalmazások kínálnak.
- Harmadik féltől származó alkalmazások rendelkeznek a saját dokumentációjában.
- Egységes Azure-szolgáltatásokhoz Határozottan javasoljuk, hogy először tekintse át az Azure Stack-dokumentáció. Az Azure Stack felhasználói dokumentáció eléréséhez, válassza ki a Súgó ikonra, és válassza **súgó + támogatás**.

    ![Súgó és támogatás a beállítás a felhasználói felületen](media/azure-stack-use-portal/HelpAndSupport.png)

    Különösen javasoljuk, hogy tekintse át a kezdéshez a következő cikkeket:

    - [Kulcs szempontok: services használatával, vagy alkalmazások készítéséhez az Azure Stackhez](azure-stack-considerations.md)
    - Az a **szolgáltatások** szakasz dokumentáció, van egy szempontok a cikk az egyes szolgáltatások. A szempontok lapra a szolgáltatás az Azure-ban érhető el, és ugyanazt a szolgáltatást az Azure Stackben kínált közötti különbségeket ismerteti. Egy vonatkozó példáért lásd: [virtuális gépekkel kapcsolatos szempontok](azure-stack-vm-considerations.md). Előfordulhat, hogy az egyéb információkat a **szolgáltatások** szakaszt, amely egyedi az Azure Stackhez.

      A szolgáltatás általános referenciaként az Azure-dokumentáció is használhatja, de ezek a különbségek tisztában kell lennie. Megismerheti, hogy a dokumentáció a hivatkozásokat tartalmaz a **gyorsútmutatók** Azure-dokumentáció, mutasson a csempére.

## <a name="get-support"></a>Támogatás kérése

Ha támogatásra van szüksége, forduljon segítségért a szervezet vagy szolgáltatás-szolgáltatónál.

Az Azure Stack fejlesztői készletének használata a [Azure Stack-fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) támogatási csak forrása.

## <a name="next-steps"></a>További lépések

[Kulcs szempontok: services használatával, vagy alkalmazások készítéséhez az Azure Stackhez](azure-stack-considerations.md)

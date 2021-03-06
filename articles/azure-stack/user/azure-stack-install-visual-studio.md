---
title: A Visual Studio telepítése és csatlakozás az Azure Stack |} A Microsoft Docs
description: Ismerje meg, a Visual Studio telepítése és csatlakozás az Azure Stack szükséges lépések
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: 2afbea68c017805e9bd7db43b03face0705608b7
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/22/2018
ms.locfileid: "42358747"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>A Visual Studio telepítése és csatlakozás az Azure Stack

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

A Visual Studio használatával és üzembe helyezését az Azure Resource Manager [sablonok](azure-stack-arm-templates.md) az Azure Stackhez. A jelen cikkben ismertetett lépések végigvezetik a Visual Studio telepítése a [Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), vagy egy külső számítógépen, ha azt tervezi, hogy az Azure Stack segítségével a [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>A Visual Studio telepítése

1. Töltse le és futtassa a [Webplatform-telepítő](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Nyissa meg a **Microsoft Webplatform-telepítő**.

3. Keresse meg **a Visual Studio Community 2015 Microsoft Azure SDK - 2.9.6**. Kattintson a **hozzáadása**, és **telepítése**.

4. Távolítsa el a **a Microsoft Azure PowerShell** részeként az Azure SDK telepítve van.

    ![Képernyőkép a WebPI-telepítés lépések](./media/azure-stack-install-visual-studio/image1.png) 

5. [A PowerShell telepítése az Azure Stack szolgáltatáshoz](azure-stack-powershell-install.md)

6. Indítsa újra az operációs rendszer, a telepítés befejezése után.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Csatlakozás az Azure Stack az Azure ad-vel

1. Indítsa el a Visual Studiót.

2. Az a **nézet** menüjében válassza **Cloud Explorer**.

3. Jelölje ki az új ablaktáblán **fiók hozzáadása** , és jelentkezzen be az Azure Active Directory (Azure AD) hitelesítő adataival.  

    ![Képernyőfelvétel a Cloud Explorer egyszer bejelentkezett, és csatlakozik az Azure Stackben](./media/azure-stack-install-visual-studio/image2.png)

Miután bejelentkezett, is [sablonok üzembe helyezése](azure-stack-deploy-template-visual-studio.md) vagy tallózással keresse meg az elérhető erőforrástípusok és -erőforráscsoportok saját sablonok létrehozásához.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>Csatlakozás az Azure Stack az AD FS-sel

1. Indítsa el a Visual Studiót.

2. A **eszközök**válassza **beállítások**.

3. Bontsa ki a **környezet** a a **navigációs** válassza **fiókok**.

4. Válassza ki **Hozzáadás**, és adja meg a felhasználó Azure Resource Manager-végpontot.  
  Az Azure Stack Development kit, az URL-je: `https://management.local.azurestack/external`.  
  Az Azure Stack integrált rendszerek az URL-cím van: `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. Válassza a **Hozzáadás** lehetőséget.  

    A Visual Studio az Azure Resource Manager-hívások, és felderíti a végpontok, beleértve a hitelesítési végpontot az Azure Directory összevont szolgáltatások (AD FS).

    ![Képernyőfelvétel a Cloud Explorer egyszer bejelentkezett, és csatlakozik az Azure Stackben](./media/azure-stack-install-visual-studio/image6.png)

6. Válassza ki **Cloud Explorer** származó a **nézet** menü.
7. Válassza ki **fiók hozzáadása** , és jelentkezzen be az AD FS hitelesítő adataival.  

    ![X](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer lekérdezi az elérhető előfizetésekkel. Választhat egyet az elérhető előfizetéssel kezelheti.

    ![X](./media/azure-stack-install-visual-studio/image8.png)

8. Böngészés a meglévő erőforrások, erőforráscsoportok vagy sablonok üzembe helyezése.

## <a name="next-steps"></a>További lépések

 - Tudjon meg többet [együttműködés](https://msdn.microsoft.com/library/ms246609.aspx) egyéb Visual Studio-verziókkal.
 - [Sablonok fejlesztése az Azure Stackhez](azure-stack-develop-templates.md)
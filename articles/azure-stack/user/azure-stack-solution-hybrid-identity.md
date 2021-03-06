---
title: A hibrid felhőbeli identitás konfigurálása az Azure és az Azure Stack-alkalmazások |} A Microsoft Docs
description: Ismerje meg, hogy hibrid felhőbeli identitás konfigurálása az Azure és az Azure Stack-alkalmazásokkal.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: bed67c0213ed5715b8b3d8fd393d8d856e0ea15b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964990"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Oktatóanyag: Hibrid felhő identitás az Azure és az Azure Stack-alkalmazások konfigurálása

*A következőkre vonatkozik: Azure Stackkel integrált rendszerek és az Azure Stack fejlesztői készlete*

Ismerje meg, hogy alkalmazásait az Azure és az Azure Stack egy hibrid felhőbeli identitás konfigurálása.

Az alkalmazások globális Azure-beli és az Azure Stack való hozzáférést két lehetősége van.

 * Amikor az Azure Stack egy folyamatos internetkapcsolattal rendelkezik, az Azure Active Directory (Azure AD) is használhatja.
 * Ha az Azure Stack nem kapcsolódik az internethez, használhatja az Azure Directory összevont szolgáltatások (AD FS).

A szolgáltatásnevek használatával hozzáférést biztosít az Azure Stack alkalmazások központi telepítése vagy konfigurálása az Azure Resource Manager használatával az Azure Stackben céljából.

Ebben az oktatóanyagban egy mintául szolgáló környezet fog létrehozni:

> [!div class="checklist"]
> - A globális Azure és az Azure Stack egy hibrid identitás létrehozása
> - Az Azure Stack API eléréséhez jogkivonat beszerzésére.

Ez az oktatóanyag lépéseit az Azure Stack operátori kell rendelkeznie.

> [!Tip]  
> ![hibrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> A Microsoft Azure Stack az Azure bővítménye. Az Azure Stack hatékonyságával és innovációjával a felhőalapú számítástechnika a helyszíni környezetben, és a egyetlen olyan hibrid felhő, amely lehetővé teszi, hogy létrehozása és üzembe helyezése bárhol a hibrid alkalmazások engedélyezésével biztosítható.  
> 
> A tanulmány [hibrid alkalmazások kapcsolatos kialakítási szempontok](https://aka.ms/hybrid-cloud-applications-pillars) tervezése, telepítése és a hibrid működő a szoftverminőség alappillérei (elhelyezését, a méretezhetőség, rugalmasság, kezelhetőségi és biztonsági) felülvizsgálatai az alkalmazások. A kialakítási szempontokat segítséget nyújt a hibrid alkalmazások kialakítását, minimálisra csökkentik az éles környezetben kihívások optimalizálása.


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>A portálon az Azure ad egyszerű szolgáltatás létrehozása

Ha már telepítette az Azure AD az ügyfélidentitás-tárolóval, mint az Azure Stack, létrehozhat egyszerű szolgáltatásokat, mint az Azure-ban végezhet el. A [szolgáltatásnevek létrehozása](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) a cikk bemutatja, hogyan végezheti el a lépéseket a portálon keresztül. Ellenőrizze, hogy rendelkezik a [szükséges Azure AD-engedélyekről](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) megkezdése előtt.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>Hozzon létre egy egyszerű szolgáltatást az AD FS Szolgáltatásokhoz a PowerShell használatával

Ha telepítette az AD FS az Azure Stack, PowerShell segítségével hozzon létre egy egyszerű szolgáltatást, hozzáférés szerepkör hozzárendelése és jelentkezzen be a PowerShell használatával az identitásukat. [Az AD FS egyszerű szolgáltatás létrehozása](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) bemutatja, hogyan végezheti el a PowerShell-lel szükséges lépéseket.

## <a name="using-the-azure-stack-api"></a>Az API-t az Azure Stack használatával

A [Azure Stack API](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-rest-api-use) az oktatóanyag végigvezeti egy tokent az Azure Stack API eléréséhez lekérését jelenti.

## <a name="connect-to-azure-stack-using-powershell"></a>Csatlakozhat az Azure Stack Powershell-lel

Ez a rövid útmutató [az első lépésekhez a PowerShell-lel az Azure Stackben](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart) végigvezeti Önt az Azure PowerShell telepítése és csatlakozás az Azure Stack-telepítés szükséges lépéseket.

### <a name="prerequisites"></a>Előfeltételek

Egy Azure Stack-telepítés csatlakozik az Azure Active Directory-előfizetéssel is elérheti. Ha egy Azure Stack-telepítés nem rendelkezik, ezek az utasítások beállításához használhatja egy [Azure Stack Development Kit](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy).

#### <a name="connect-to-azure-stack-using-code"></a>Azure Stack-kód használatával csatlakozhat.

Azure Stack-kód használatával csatlakozhat, használja az Azure Resource Manager-végpontok API, a hitelesítés és a graph-végpont lekérése az Azure Stack telepítése, és hitelesítse a REST-kérelmek használatával. Egy mintaalkalmazás ügyfél találhat [GitHub](https://github.com/shriramnat/HybridARMApplication).

>[!Note]
>Tetszőleges nyelven készült Azure SDK támogatja az Azure API-profilok, hacsak az SDK nem működnek az Azure Stack használatával. Az Azure API-profilokkal kapcsolatos további tudnivalókért tekintse meg a [API-verzióprofilok kezelése](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles) cikk.

## <a name="next-steps"></a>További lépések

 - Hogyan kezeli az Azure Stackben identitás kapcsolatos további információkért lásd: [identitásarchitektúra az Azure Stackhez készült](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).
 - Az Azure-minták Felhőkhöz kapcsolatos további információkért lásd: [tervezési minták Felhőkhöz](https://docs.microsoft.com/azure/architecture/patterns).

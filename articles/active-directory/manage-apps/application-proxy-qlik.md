---
title: Az Azure AD-alkalmazásproxy és Qlik Sense |} A Microsoft Docs
description: Kapcsolja be az alkalmazásproxyt, hogy az Azure Portalon, és az összekötők telepítése a fordított proxy.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 09/06/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5f103e9fe410374a551eb43d456d5993bdd36627
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057084"
---
# <a name="application-proxy-and-qlik-sense"></a>Az alkalmazásproxy és Qlik Sense 
Az Azure Active Directory alkalmazásproxy és Qlik Sense platformtechnológiát együtt, ellenőrizze, hogy könnyen tud alkalmazásproxy használatával a Qlik Sense-telepítés a távelérés biztosítása.  

## <a name="prerequisites"></a>Előfeltételek 
Ez a forgatókönyv további része feltételezi, a következőket:
 
- Konfigurált [Qlik Sense](https://community.qlik.com/docs/DOC-19822). 
- [Az Application Proxy connector telepítése](application-proxy-enable.md#install-and-register-a-connector) 
 
## <a name="publish-your-applications-in-azure"></a>Az alkalmazások közzététele az Azure-ban 
A közzétételhez QlikSense kell két alkalmazás közzététele az Azure-ban.  

### <a name="application-1"></a>#1. alkalmazás: 
Kövesse az alábbi lépéseket az alkalmazás közzétételéhez. Egy részletes útmutató lépéseit, 1 – 8., lásd: [alkalmazások közzététele az Azure AD-alkalmazásproxy](application-proxy-publish-azure-portal.md). 


1. Jelentkezzen be globális rendszergazdaként az Azure Portalon. 
2. Válassza ki **Azure Active Directory** > **vállalati alkalmazások**. 
3. Válassza ki **Hozzáadás** a panel tetején. 
4. Válassza ki **helyszíni alkalmazás**. 
5.       Töltse ki a kötelező mezőket az új alkalmazással kapcsolatos információkat. A következő útmutatást használhatja a beállításokat: 
    - **Belső URL-cím**: az alkalmazásnak tartalmaznia kell egy belső URL-CÍMÉT, amely önmagában QlikSense URL-címe. Ha például **https&#58;//demo.qlikemm.com:4244** 
    - **Az előhitelesítési módszer**: Azure Active Directory (de nem ajánlott) 
1.       Válassza ki **Hozzáadás** a panel alján. Az alkalmazás kerül, és megnyílik a gyors üzembe helyezési menü. 
2.       A gyors üzembe helyezési menüben válassza ki a **felhasználó hozzárendelése teszteléshez**, és legalább egy felhasználót az alkalmazáshoz. Ellenőrizze, hogy a teszt a helyszíni alkalmazás hozzáféréssel rendelkezik. 
3.       Válassza ki **hozzárendelése** a teszt felhasználó-hozzárendelés mentése. 
4.       (Nem kötelező) Az alkalmazás felügyeleti panelen válassza ki az egyszeri bejelentkezés. Válasszon **Kerberos által korlátozott delegálás** a legördülő menüből, majd töltse ki a kötelező mezőket, a Qlik konfiguráció alapján. Kattintson a **Mentés** gombra. 

### <a name="application-2"></a>#2. alkalmazás: 
Kövesse a lépéseket, mint az alkalmazás 1, a következő kivételekkel: 

**5. #**: A belső URL-címet kell azt a hitelesítési portot, amelyet az alkalmazás az QlikSense URL-CÍMÉT. Az alapértelmezett érték **4244** 4248 HTTP és HTTPS. Például: **https&#58;//demo.qlik.com:4244**</br></br> 
 **#10. lépés:** ne be egyszeri Bejelentkezést, és hagyja a **egyszeri bejelentkezés le van tiltva**
 
 
## <a name="testing"></a>Tesztelés 
Az alkalmazás teszteléséhez készen áll. A külső URL-cím, amellyel QlikSense közzététele az alkalmazás 1 és bejelentkezés egy felhasználó mindkét alkalmazáshoz rendelt eléréséhez.  

## <a name="next-steps"></a>További lépések

- [Az alkalmazásproxy-alkalmazások közzététele](application-proxy-publish-azure-portal.md)
- [Az alkalmazásproxy-összekötők](application-proxy-connector-groups.md).

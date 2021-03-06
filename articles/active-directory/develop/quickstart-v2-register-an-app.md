---
title: Alkalmazás regisztrálása az Azure AD 2.0-s verziójú végpontján | Microsoft Docs
description: Megtudhatja, hogyan regisztrálhat alkalmazásokat a Microsoftnál a bejelentkezés és a Microsoft-szolgáltatások elérésének az Azure AD 2.0-s verziójú végpontján keresztüli engedélyezéséhez.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.openlocfilehash: b2dea11b6573be8f574bd18fa69ee76658d6d698
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975810"
---
# <a name="quickstart-register-an-app-with-the-azure-active-directory-v20-endpoint"></a>Rövid útmutató: Alkalmazás regisztrálása az Azure Active Directory 2.0-s verziójú végpontján

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Olyan alkalmazás létrehozásához, amelybe személyes Microsoft-fiókkal (MSA), valamint munkahelyi vagy iskolai fiókkal (Azure AD) is be lehet jelentkezni, az alkalmazást az Azure Active Directory (Azure AD) 2.0-s verziójú végpontján kell regisztrálni. Jelenleg a meglévő alkalmazásokat nem lehet az MSA-val vagy az Azure AD-vel használni – ehhez teljesen új alkalmazást kell létrehozni.

> [!NOTE]
> A 2.0-s verziójú végpont nem minden Azure AD-forgatókönyvet és -funkciót támogat. Ha nem biztos benne, hogy érdemes-e a 2.0-s verziójú végpontot használnia, olvassa el a [2.0-s verzió korlátozásait](active-directory-v2-limitations.md).

## <a name="step-1-sign-in-to-the-microsoft-application-registration-portal"></a>1. lépés: Bejelentkezés a Microsoft alkalmazásregisztrációs portálra

1. Lépjen a Microsoft alkalmazásregisztrációs portáljára a következő helyen: [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).
1. Jelentkezzen be a személyes vagy a munkahelyi, illetve iskolai Microsoft-fiókjával. Ha egyikkel sem rendelkezik, regisztráljon egy új személyes fiókot.
1. Bejelentkezett? Most a Microsoft-alkalmazások listájának kell megjelennie a képernyőn – a lista valószínűleg üres. Ezt fogjuk most módosítani.

## <a name="step-2-register-an-app"></a>2. lépés: Alkalmazás regisztrálása

1. Válassza az **Alkalmazás hozzáadása** lehetőséget, és adjon meg egy nevet.
    A portál egy globálisan egyedi alkalmazásazonosítót oszt ki az alkalmazásnak, amelyet később a kódban fog használni. Ha az alkalmazás olyan kiszolgálóoldali összetevőt tartalmaz, amelynek hozzáférési jogkivonatokra van szüksége az API-k meghívásához (például az Office, az Azure vagy a saját webes API-ja), érdemes itt létrehoznia egy **Alkalmazáskulcsot** is.
1. Ezután adja hozzá a **Platformokat**, amelyeket az alkalmazás használni fog.
    * A webalapú alkalmazások esetében adja meg az **Átirányítási URI**-azonosítót, ahová az üzenetek küldve lesznek.
    * Mobilalkalmazások esetében másolja le az automatikusan létrehozott, alapértelmezett átirányítási URI-t.
    * A webes API-k esetében a rendszer automatikusan létrehoz egy alapértelmezett hatókört a webes API eléréséhez.
        További hatóköröket a **Hatókör hozzáadása** gombbal adhat hozzá. Az **Előre engedélyezett alkalmazások** űrlapon hozzáadhat olyan alkalmazásokat, amelyek számára előre engedélyezi a webes API használatát.
1. Ha szeretné, a **Profil** szakaszban testre szabhatja a bejelentkezési oldal megjelenését. 
1. **Mentse** a módosításokat, mielőtt továbblépne.

> [!NOTE]
> Ha a [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) használatával regisztrál egy alkalmazást, az alkalmazás a portálra való bejelentkezéshez használt fiók otthoni bérlőjén lesz regisztrálva. Ez azt jelenti, hogy az Azure AD-bérlőn nem regisztrálhat alkalmazásokat a személyes Microsoft-fiókja használatával. Ha egy alkalmazást kifejezetten egy adott bérlőn szeretne regisztrálni, jelentkezzen be egy eredetileg az adott bérlőn létrehozott fiókkal.

## <a name="next-steps"></a>További lépések

Most, hogy már rendelkezik Microsoft-alkalmazással, elvégezheti valamelyik 2.0-s verziójú rövid útmutatót. Az alábbiakban néhány javaslatot olvashat:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

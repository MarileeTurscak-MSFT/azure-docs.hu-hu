---
title: Az Azure IoT-megoldás gyorsítók és az Azure Active Directory |} Microsoft Docs
description: Ismerteti, hogyan Azure IoT-megoldás gyorsítók az Azure Active Directory használatával kezeli az engedélyeket.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: b7360ca4df63cac114b0eb1f93375367da6735cc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/07/2018
---
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Engedélyek az azureiotsuite.com webhelyen

## <a name="what-happens-when-you-sign-in"></a>Mi történik, amikor bejelentkezik

Az első bejelentkezéskor, [azureiotsuite.com][lnk-azureiotsuite], a hely határozza meg a jogosultsági szintek rendelkezik a kijelölt Azure Active Directory (AAD) bérlői és az Azure-előfizetés alapján.

1. Először töltse fel adatokkal a bérlők mellett a felhasználónévnek látható listáját, a hely szerzi meg az Azure-ból mely AAD-bérlő tartozik. Jelenleg a hely csak szerezhet be egy bérlői felhasználói jogkivonatokhoz egyszerre. Ezért amikor a bérlők a legördülő lista használatával jobb felső sarokban, a hely naplózza a arra a bérlőre, hogy a bérlő a tokenek beszerzése.

2. A hely a következő megkeresése, melyik előfizetések vannak társítva a kiválasztott bérlő az Azure-ból. Amikor létrehoz egy új megoldásgyorsító megjelenik az elérhető előfizetésekkel.

3. Végül a hely lekéri az összes lévő erőforrásokat a előfizetésekhez és erőforráscsoportokhoz megoldás gyorsítók megjelölhető tölti fel a csempék a kezdőlapon.

A következő szakaszok ismertetik a szerepköröket, amelyek hozzáférést a megoldás gyorsítók számára.

## <a name="aad-roles"></a>Az AAD-szerepkörök

Az AAD-szerepkörök a képességét kiépítési megoldás gyorsítók szabályozhatja, és egy megoldásgyorsító a felhasználók kezeléséhez.

Tudnivalók a rendszergazdai szerepkörökről további információkat talál az aad-ben a [rendszergazdai szerepkörök hozzárendelése az Azure AD][lnk-aad-admin]. A jelen cikkben összpontosít a **globális rendszergazda** és a **felhasználói** directory szerepkörök, a megoldás gyorsítók használják.

### <a name="global-administrator"></a>Globális rendszergazda

Az AAD bérlőnként globális rendszergazdák közül sokan lehet:

* Amikor létrehoz egy AAD-bérlőt, áll alapértelmezés szerint, hogy a bérlő globális rendszergazdája.
* A globális rendszergazda hozhat létre egy egyszerű és szabványos megoldás gyorsítókra.

### <a name="domain-user"></a>Tartományi felhasználó

AAD-bérlőt sok tartományi felhasználók lehet:

* A tartományi felhasználók építhető ki egy alapszintű megoldásgyorsító keresztül a [azureiotsuite.com] [ lnk-azureiotsuite] hely.
* Egy olyan tartományi felhasználó létrehozhat egy alapszintű megoldásgyorsító a parancssori felület használatával.

### <a name="guest-user"></a>Vendég felhasználó

Az AAD bérlőnként számos Vendég felhasználó lehet. Vendégfelhasználók rendelkeznek korlátozott jogok az AAD-bérlőben. Ennek eredményeképpen vendégfelhasználók nem használhatók egy megoldásgyorsító az AAD-bérlőben.

Felhasználók és szerepkörök az aad-ben kapcsolatos további információkért lásd a következőket:

* [Felhasználók létrehozása az Azure ad-ben][lnk-create-edit-users]
* [Felhasználók hozzárendelése alkalmazásokhoz][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure-előfizetéshez rendszergazdai szerepkörök

Az Azure rendszergazdai szerepkörök az Azure-előfizetés van leképezve az AD-bérlő általi szabályozása.

További információk a cikkben az Azure rendszergazdai szerepkörök [hozzáadása vagy módosítása az Azure Társadminisztrátoraként, a szolgáltatás-rendszergazda és a fiók rendszergazdájához][lnk-admin-roles].

## <a name="faq"></a>GYIK

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>A szolgáltatás-rendszergazdáknak vagyok, és szeretnék módosítani a könyvtár leképezése az előfizetésem és egy adott AAD-bérlőt között. Hogyan hajthatja végre ezt a feladatot?

Lásd: [hozzáadása egy meglévő előfizetéshez az Azure AD-címtár](../active-directory/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>A szolgáltatás rendszergazdai vagy társadminisztrátori Ha egy szervezeti fiókkal jelentkezik módosítása

Tekintse meg a támogatási cikk [módosítása szolgáltatás-rendszergazda és Társadminisztrátoraként, amikor egy szervezeti fiókkal jelentkezik be][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Miért jelenik meg ezt a hibát? "A fiók nem rendelkezik a megoldás létrehozása a megfelelő engedélyekkel. Ellenőrizze a fiók rendszergazdájához, vagy próbálkozzon egy másik fiókkal."

Tekintse meg az alábbi ábrában útmutatást:

![][img-flowchart]

> [!NOTE]
> Ha továbbra is megjelenik a hiba akkor ellenőrzése után egy globális rendszergazdája az AAD-bérlőt és egy az előfizetés társadminisztrátoraként, a fiók rendszergazdájához, távolítsa el a felhasználót, és újra hozzárendelnie az itt megadott sorrendben szükséges engedélyekkel rendelkezik. Először adja hozzá a felhasználót egy globális rendszergazdaként, és adja hozzá a felhasználó az Azure-előfizetés társadminisztrátoraként. Ha a probléma továbbra is fennáll, forduljon a [Súgó és támogatás][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Miért jelenik meg a hiba, ha az Azure-előfizetésre van? "Előkonfigurált megoldások létrehozásához Azure-előfizetés szükséges. Létrehozhat egy ingyenes próbafiók néhány percig."

Ha bizonyos Azure-előfizetéssel rendelkezik, a bérlő hozzárendelése az előfizetéshez, és ellenőrizheti a megfelelő bérlő a legördülő listában kiválasztott. Ha ellenőrizte, hogy a kívánt bérlő megfelelő, kövesse a fenti ábrán és az előfizetés és az AAD-bérlőt a leképezés érvényesítése.

## <a name="next-steps"></a>További lépések
És folytathatja az IoT-megoldás gyorsítók, tekintse meg, hogyan zajlik [testre szabhatja a megoldásgyorsító][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md

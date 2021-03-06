---
title: 'Az Azure AD Connect szinkronizálása: az Azure AD-szolgáltatásfiók kezelése |} A Microsoft Docs'
description: Ez a témakör az Azure AD-szolgáltatásfiók visszaállítása dokumentumok.
services: active-directory
keywords: AADSTS70002, AADSTS50054, az Azure AD Connect szinkronizálási szolgáltatás Connector-szolgáltatásfióknak a jelszó alaphelyzetbe állítása
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 93dcbe5ceac87d84db9638de6d5fe120de8c2d36
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46313648"
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Az Azure AD Connect szinkronizálása: az Azure AD-szolgáltatásfiók kezelése
Az Azure AD-összekötő által használt szolgáltatásfiók kellene lennie ingyenes szolgáltatás. Ha a hitelesítő adatok alaphelyzetbe kell, majd ez a témakör Önnek szól. Például egy globális rendszergazdai szerint hibásan van visszaállíthatja a jelszót a szolgáltatás fiók a PowerShell-lel.

## <a name="reset-the-credentials"></a>A hitelesítő adatok alaphelyzetbe állítása
A szolgáltatás fiók az Azure AD Connectoron meghatározott Azure AD hitelesítési problémái miatt nem tud kapcsolatba lépni, ha a jelszó állítható vissza.

1. Jelentkezzen be az Azure AD Connect szinkronizálási kiszolgáló, és indítsa el a Powershellt.
2. Futtassa az `Add-ADSyncAADServiceAccount` parancsot.  
   ![PowerShell-parancsmag addadsyncaadserviceaccount](./media/how-to-connect-azureadaccount/addadsyncaadserviceaccount.png)
3. Adja meg az Azure AD globális rendszergazdai hitelesítő adatait.

Ez a parancsmag alaphelyzetbe állítja a jelszót a fiókhoz, és frissítse az Azure ad-ben és a szinkronizálási motor is.

## <a name="known-issues-these-steps-can-solve"></a>Ismert problémákat oldhat meg ezeket a lépéseket
Ez a szakasz az ügyfelek által a hitelesítő adatok alaphelyzetbe állítása, az Azure AD-szolgáltatásfiók a javított által jelentett hibák listája.

- - -
Esemény 6900  
A kiszolgáló váratlan hibába ütközött a jelszóváltoztatási értesítés feldolgozása közben:  
AADSTS70002: Hiba történt a érvényesítésekor hitelesítő adatokat. AADSTS50054: Régi jelszót használt a hitelesítéshez.

- - -
Esemény 659  
Hiba történt a jelszó szinkronizálása szabályzatkonfiguráció beolvasása. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Hiba történt a érvényesítésekor hitelesítő adatokat. AADSTS50054: Régi jelszót használt a hitelesítéshez.

## <a name="next-steps"></a>További lépések
**Áttekintő témakör**

* [Az Azure AD Connect szinkronizálása: ismertetése, és testre szabhatja a szinkronizálás](how-to-connect-sync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](whatis-hybrid-identity.md)


---
title: 'Az Azure AD Connect szinkronizálása: az AD Lomtár engedélyezése |} A Microsoft Docs'
description: Ez a témakör az Azure AD Connect az AD Lomtár funkció használatát javasolja.
services: active-directory
keywords: AD lomtárának véletlen törlés, a forráshorgony
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 378e1d3aab992e9b4e6f2263c26ea4268a43d678
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46312143"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Az Azure AD Connect szinkronizálása: az AD Lomtár engedélyezése
Javasoljuk, hogy a helyszíni Active címtárak, amely szinkronizálva vannak az Azure AD az AD Lomtár engedélyezése. 

Ha véletlenül törölte egy helyszíni AD-felhasználói objektum és a funkció használatát, az Azure AD helyreállítja a megfelelő Azure AD felhasználói objektum visszaállítása.  Az AD Lomtár funkció kapcsolatos információkért tekintse meg a cikk [forgatókönyv áttekintése a visszaállítás törölt Active Directory-objektumok](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>Az AD Lomtár engedélyezése előnyei
Ez a szolgáltatás segít az Azure AD felhasználói objektumok visszaállításához a következő módon:

* Ha véletlenül törölte egy helyszíni AD-felhasználói objektumot, a megfelelő Azure AD felhasználói objektum törli a következő szinkronizálási ciklusban. Alapértelmezés szerint az Azure AD megtartja a törölt Azure AD felhasználói objektum 30 napig helyreállíthatóan törölt állapotban.

* Ha rendelkezik a helyszíni AD Recycle Bin szolgáltatás engedélyezve van, a törölt állíthatja vissza a helyszíni AD-felhasználói objektum a Forráshorgony értékét módosítása nélkül. Ha a helyreállított a helyszíni AD-felhasználói objektum szinkronizált Azure ad-hez, az Azure AD fog visszaállítást a megfelelő helyreállíthatóan törölt Azure AD felhasználói objektum. Forráshorgony-attribútumként kapcsolatos információkért tekintse meg a cikk [az Azure AD Connect: tervezési alapelvek](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Ha nem rendelkezik a helyszíni AD Recycle Bin szolgáltatás engedélyezve van, előfordulhat, hogy az AD-felhasználói objektum cserélje le a törölt objektum létrehozásához. Ha a rendszer által létrehozott AD-attribútum (például az ObjectGuid) használja a Forráshorgony-attribútumként az Azure AD Connect szinkronizálási szolgáltatás van konfigurálva, az újonnan létrehozott AD felhasználói objektum nem fog a törölt AD user objektum Forráshorgony értékét. Az újonnan létrehozott AD felhasználói objektum szinkronizálva van az Azure AD, ha az Azure ad-ben létrehoz egy új Azure AD felhasználói objektum helyett a helyreállíthatóan törölt visszaállítása az Azure AD felhasználói objektum.

> [!NOTE]
> Alapértelmezés szerint az Azure AD tartja az Azure AD felhasználói objektumok helyreállíthatóan törölt állapotban törlése 30 napig, mielőtt véglegesen törlődnek. A rendszergazdák azonban gyorsítható fel az ilyen objektumok a törlést. Az objektumok véglegesen törlődnek, ha azok már nem állíthatók helyre, még akkor is, ha a helyszíni AD Lomtár funkció engedélyezve van.



## <a name="next-steps"></a>További lépések
**Áttekintő témakör**

* [Az Azure AD Connect szinkronizálása: ismertetése, és testre szabhatja a szinkronizálás](how-to-connect-sync-whatis.md)

* [Helyszíni identitások integrálása az Azure Active Directoryval](whatis-hybrid-identity.md)

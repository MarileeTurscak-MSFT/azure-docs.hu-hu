---
title: 'Az Azure portal: SQL Database dinamikus adatmaszkolása |} A Microsoft Docs'
description: Ismerkedés az SQL Database dinamikus adatmaszkolás az Azure Portalon
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 1ec0634c89148cee59f399e437b92a7d2c6b283b
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165721"
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>Ismerkedés az SQL Database dinamikus adatmaszkolás az Azure portal használatával

Ez a cikk bemutatja, hogyan valósíthat meg [dinamikus adatmaszkolás](sql-database-dynamic-data-masking-get-started.md) az Azure portal használatával. Dinamikus adatok maszkolási használatával is alkalmazhat [Azure SQL Database-parancsmagok](https://docs.microsoft.com/powershell/module/azurerm.sql/) vagy a [REST API-val](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Állítsa be a dinamikus adatmaszkolás az adatbázis az Azure portal használatával
1. Indítsa el az Azure Portalra a [ https://portal.azure.com ](https://portal.azure.com).
2. Lépjen a beállítások lapon a kívánt maszkolandó bizalmas adatokat tartalmazó adatbázis.
3. Kattintson a **dinamikus Adatmaszkolás** csempét, amely elindítja a **dinamikus Adatmaszkolás** konfigurációs lapon.
   
   * Azt is megteheti, hogy is görgessen le a **műveletek** szakaszt, és kattintson a **dinamikus Adatmaszkolás**.
     
     ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. Az a **dinamikus Adatmaszkolás** konfigurációs lapján láthatja azokat a ajánlatok motor állóként maszkolási adatbázis az oszlopokat. Annak érdekében, hogy fogadja el a javaslatokat, egyszerűen kattintson **maszk hozzáadása** maszk és a egy vagy több oszlop jön létre az alapértelmezett típus az az oszlop alapján. A maszkolási függvényt a maszkolási szabályra kattintva, és a maszkolás szerkesztésével módosíthatja a mező formátuma tetszőleges más formátumba. Ügyeljen arra, hogy kattintson **mentése** a beállítások mentéséhez.
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. Az adatbázisban a felső részén egy maszk bármelyik oszlop hozzáadása a **dinamikus Adatmaszkolás** konfigurációs lapján kattintson a **maszk hozzáadása** megnyitásához a **maszkolási szabály hozzáadása** konfigurációs lapja .
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Válassza ki a **séma**, **tábla** és **oszlop** maszkolási a kijelölt mező meghatározásához.
7. Válasszon egy **maszkolási mezőformátum** bizalmas adatok maszkolása a kategóriák listájában.
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Kattintson a **mentése** adatmaszkolási szabály frissítéséhez maszkolási szabályok házirendet a dinamikus adatmaszkolás az oldalon található.
9. Írja be az SQL-felhasználók vagy az AAD-identitások maszkolási ki kell zárni, és rendelkezik a maszkolásukat megszüntetni bizalmas adatokhoz való hozzáférést. Ez legyen a felhasználók pontosvesszővel tagolt listája. Rendszergazdai jogosultságokkal rendelkező felhasználók mindig hozzáférhetnek az eredeti maszkolt adatokat.
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > Legyen, így az alkalmazásrétegre megjelenítheti az alkalmazás az emelt szintű felhasználók bizalmas adatait, adja hozzá az SQL-felhasználó vagy az adatbázis lekérdezéséhez használja az alkalmazás AAD-identitását. Azt javasoljuk, hogy ez a lista tartalmazza-e a minimalizálása érdekében a a bizalmas adatok megjelenítését a kiemelt jogosultságú felhasználók minimális száma.
   > 
   > 
10. Kattintson a **mentése** adatmaszkolási mentse az új vagy frissített (konfiguráció) lap a maszkolás házirend.


## <a name="next-steps"></a>További lépések

* Dinamikus adatmaszkolás áttekintését lásd: [dinamikus adatmaszkolás](sql-database-dynamic-data-masking-get-started.md).
* Dinamikus adatok maszkolási használatával is alkalmazhat [Azure SQL Database-parancsmagok](https://docs.microsoft.com/powershell/module/azurerm.sql/) vagy a [REST API-val](https://msdn.microsoft.com/library/dn505719.aspx).

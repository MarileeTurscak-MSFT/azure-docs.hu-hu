---
title: Adatmodell-kompatibilitási szintje az Azure Analysis Servicesben |} A Microsoft Docs
description: Táblázatos modell kompatibilitási szintnek ismertetése.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 17a75e86d5539427c8837b3077aacf85b4681ad6
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447523"
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Analysis Services rendszerbeli táblázatos modellek kompatibilitási szintje

*Kompatibilitási szint* kiadás-specifikus viselkedést az Analysis Services motor a hivatkozik. A kompatibilitási szint módosítása általában jelentős kiadásokat az SQL Server egybe. Ezek a változások is vannak megvalósítva, az Azure Analysis Servicesben mindkét platform paritása fenntartásához. Kompatibilitási szint érvénybe funkcióját a táblázatos modellek is változik. Például a DirectQuery és az objektum táblázatos metaadatok különböző megvalósításokkal rendelkeznek függően a kompatibilitási szintet. Kompatibilitási szint van megadva a táblázatosmodell-projektet a Visual Studio (SSDT). A Power BI Desktop-ról importált és létrehozott táblázatos modellek az 1400-as kompatibilitási szinten csak olyan.

Az Azure Analysis Services támogatja a táblázatos modelleket az 1200-as és 1400-as kompatibilitási szinteken. 

A legújabb kompatibilitási szint az 1400-as. Ez a szint az SQL Server 2017 Analysis Services időpontjával. Az 1400-as kompatibilitási szinten főbb funkciók a következők:

*  Új funkciók az Adatkapcsolat és importálása a TOM API-k és a TMSL-szkriptek támogatása. 
*  Adatátalakítás és -adatok adategyesítési képességek az adatok lekérése és az M kifejezésében.
*  Mértékek DAX-kifejezés részletsorok tulajdonságot támogatja. Ez a tulajdonság lehetővé teszi, hogy az ügyféleszközök elől hasonló lehatolhat egy összesített jelentés részletes adatait a Microsoft Excel. Például ha a felhasználó egy régió és hónaphoz tartozó összes értékesítés, akkor megtekintheti a társított rendelés részleteit. 
*  Az Objektumszintű biztonsági a bennük lévő adatok mellett a tábla- és oszlopneveket számára.
*  Kibővített támogatás a hézagos hierarchiák.
*  Teljesítmény- és figyelési fejlesztései.
  
## <a name="set-compatibility-level"></a>Set-kompatibilitási szint 
 Az SSDT-ben egy új táblázatos modellprojekt létrehozásakor megadhatja a kompatibilitási szint a **táblázatos modell tervezője** párbeszédpanel. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Ha a **ne jelenjen meg ez az üzenet** lehetőség, az összes későbbi projekt használata a megadott alapértelmezett kompatibilitási szintje. Módosíthatja az SSDT-ben az alapértelmezett kompatibilitási szintje **eszközök** > **beállítások**.  
  
 Frissíti egy meglévő, az SSDT táblázatosmodell-projektet, állítsa be a **kompatibilitási szint** tulajdonság a modellben **tulajdonságok** ablak. Ne feledje, a kompatibilitási szint frissítése nem vonható vissza.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Ellenőrizze az SQL Server Management Studio táblázatos modellű adatbázis kompatibilitási szintje 
 Az ssms-ben, kattintson a jobb gombbal az adatbázis neve > **tulajdonságok** > **kompatibilitási szint**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Ellenőrizze a támogatott kompatibilitási szint egy kiszolgálóhoz az ssms-ben  
 Az ssms-ben, kattintson a jobb gombbal a kiszolgáló neve > **tulajdonságok** > **támogatott kompatibilitási szint**.  
  
 Ez a tulajdonság egy adatbázist a kiszolgálón (kivéve az előzetes verzió) rendszerű a legmagasabb kompatibilitási szintjét határozza meg. A támogatott kompatibilitási szintje nem módosítható.  

## <a name="next-steps"></a>További lépések
  [Modell létrehozása az Azure Portalon](analysis-services-create-model-portal.md)   
  [Analysis Services kezelése](analysis-services-manage.md)  

---
title: Az SQL Database 1433-ason túli |} A Microsoft Docs
description: Ügyfélkapcsolatok ADO.NET-ből az Azure SQL Database is a proxyt, és közvetlenül az adatbázis 1433-astól különböző portokon keresztül kommunikál.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 560d96b188a02f8df0d41b040c90db9b813e3c0a
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063251"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Az ADO.NET 4.5 szoftverrel 1433-ason túli
Ez a témakör ismerteti az Azure SQL Database-kapcsolatok viselkedését az ADO.NET 4.5 vagy újabb verzióját használó ügyfelek számára. 

> [!IMPORTANT]
> Kapcsolati architektúra kapcsolatos információkért lásd: [Azure SQL Database kapcsolati architektúra](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Kívül és belül
Az Azure SQL Database-kapcsolatok esetén kell először megkérjük fut-e az ügyfélprogram *kívül* vagy *belül* az Azure-felhő határain. Az alszakaszok két gyakori forgatókönyv tárgyalják.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Külső:* ügyfél futtatja az asztali számítógépen
1433-as porton csak a portot kell megnyitni az asztali számítógépen, amelyen az SQL Database-ügyfélalkalmazást.

#### <a name="inside-client-runs-on-azure"></a>*Belső:* ügyfél futtatja az Azure-ban
Amikor az ügyfél az Azure-felhő határain belül fut, akkor használja, mi meghívhatjuk egy *közvetlen útvonal* kommunikál az SQL Database-kiszolgálóhoz. A kapcsolat létrejötte után további az ügyfél és az adatbázis közötti kapcsolat nem Azure SQL Database átjárója között.

A feladatütemezés a következőképpen történik:

1. Az ADO.NET 4.5 (vagy újabb verzió) kezdeményez az Azure-felhő rövid interakció, és dinamikusan azonosított portszám kap.
   
   * A dinamikusan azonosított portszám a 11000-11999 vagy 14000-14999 tartományán van.
2. ADO.NET majd csatlakozik az SQL Database-kiszolgálóhoz, közvetlenül nem közbenső a kettő között.
3. Adatbázissal küldi el, és közvetlenül az ügyfél az eredményt.

Győződjön meg arról, hogy a port 11000-11999 és az Azure-ügyfél gépen 14000-14999 tartományok ADO.NET 4.5 ügyfél interakciók az SQL Database elérhető marad.

* A tartomány portokat, ingyenes, bármely más kimenő blockers kell megadni.
* Az Azure virtuális gépen a **fokozott biztonságú Windows tűzfal** szabályozza a portbeállításokat.
  
  * Használhatja a [tűzfal a felhasználói felület](http://msdn.microsoft.com/library/cc646023.aspx) hozzáadása egy szabályt, amelynek meg a **TCP** protokoll egy porttartományt szintaxissal együtt, például **11000-11999**.

## <a name="version-clarifications"></a>Verzió magyarázata
Ez a szakasz a termékváltozatokra hivatkozó monikerek értelmezi. Emellett egyes párok termékek közötti verziójú sorolja fel.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0-s verzióját támogatja a TDS 7.3 protokoll, de nem 7.4.
* Az ADO.NET 4.5-ös és újabb verziók támogatja a TDS 7.4-protokollt.

#### <a name="odbc"></a>ODBC
* A Microsoft SQL Server ODBC 11 vagy újabb

#### <a name="jdbc"></a>JDBC
* A Microsoft SQL Server JDBC 4.2 vagy újabb (JDBC 4.0 ténylegesen támogatja a TDS 7.4, de nem valósít meg "átirányítás")


## <a name="related-links"></a>Kapcsolódó hivatkozások
* Az ADO.NET 4.6 fel lett oldva, 2015. július 20. Érhető el a bejelentést a blogon a .NET-csapattól [Itt](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* Az ADO.NET 4.5-ös verziója a 2012. augusztus 15 jelent meg. Érhető el a bejelentést a blogon a .NET-csapattól [Itt](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx). 
  * Érhető el egy ADO.NET 4.5.1 szóló blogbejegyzést [Itt](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).

* Microsoft® ODBC illesztőprogram 17 SQL Server® – Windows, Linux és macOS https://www.microsoft.com/en-us/download/details.aspx?id=56567

* Csatlakozás az Azure SQL Database V12 átirányítás https://blogs.msdn.microsoft.com/sqlcat/2016/09/08/connect-to-azure-sql-database-v12-via-redirection/

* [TDS protokollverziók listája](http://www.freetds.org/userguide/tdshistory.htm)
* [Az SQL Database fejlesztési áttekintése](sql-database-develop-overview.md)
* [Az Azure SQL Database-tűzfal](sql-database-firewall-configure.md)
* [Útmutató: az SQL Database tűzfalbeállításainak konfigurálása](sql-database-configure-firewall-settings.md)



---
title: Azure SQL Database-adatbázisok csatlakozási és lekérdezési útmutatói | Microsoft Docs
description: Az Azure SQL Database-adatbázisokhoz való csatlakozást és a rájuk vonatkozó lekérdezéseket ismertető rövid útmutatók.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc
ms.devlang: ''
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: carlrab
ms.openlocfilehash: ec39c5ad0771c2bc78655e52c58949db6e9b3353
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/28/2018
ms.locfileid: "32186011"
---
# <a name="azure-sql-database-connect-and-query-quickstarts"></a>Azure SQL Database-adatbázisok csatlakozási és lekérdezési útmutatói

A következő dokumentum az Azure SQL Database-adatbázisok csatlakozásával és lekérdezésével kapcsolatos Azure-példákra mutató hivatkozásokat tartalmaz. Emellett a TLS-re vonatkozó javaslatokat is tartalmaz.

## <a name="quickstarts"></a>Gyors útmutatók

| |  |
|---|---|
|[SQL Server Management Studio](sql-database-connect-query-ssms.md)|Ez a rövid útmutató azt ismerteti, hogyan használható az SSMS egy Azure SQL Database-adatbázishoz való csatlakozáshoz, és hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni, beszúrni, frissíteni és törölni az adatbázisban.|
|[SQL Operations Studio](https://docs.microsoft.com/sql/sql-operations-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)|Ez a rövid útmutató azt ismerteti, hogyan használható az SQL Operations Studio (előzetes verzió) egy Azure SQL Database-adatbázishoz való csatlakozáshoz, és hogyan lehet Transact-SQL- (T-SQL-) utasításokkal létrehozni az SQL Operations Studio (előzetes verzió) oktatóanyagaiban használt TutorialDB adatbázist.|
|[Azure Portal](sql-database-connect-query-portal.md)|Ez a rövid útmutató ismerteti, hogyan használható a Lekérdezésszerkesztő az SQL-adatbázisokhoz való csatlakozáshoz, majd hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni, beszúrni, frissíteni és törölni az adatbázisban.|
|[Visual Studio Code](sql-database-connect-query-vscode.md)|Ez a rövid útmutató azt ismerteti, hogyan használható a Visual Studio Code egy Azure SQL Database-adatbázishoz való csatlakozáshoz, és hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni, beszúrni, frissíteni és törölni az adatbázisban.|
|[.NET Visual Studióval](sql-database-connect-query-dotnet-visual-studio.md)|Ez a rövid útmutató azt ismerteti, hogyan használható a .NET-keretrendszer olyan C#-program a Visual Studióval való létrehozásához, amely egy Azure SQL Database-adatbázishoz csatlakozik, és hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni.|
|[.NET Core](sql-database-connect-query-dotnet-core.md)|Ez a rövid útmutató ismerteti, hogyan használható a .NET Core olyan C#-program létrehozásához Windows/Linux/macOS-gépeken, amely egy Azure SQL Database-adatbázishoz csatlakozik, és hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni.|
|[Go](sql-database-connect-query-go.md)|Ez a rövid útmutató azt ismerteti, hogyan használható a Go egy Azure SQL Database-adatbázishoz való csatlakozáshoz. Emellett az adatok lekérdezéséhez és módosításához használatos Transact-SQL-utasításokat is bemutatja.|
|[Java](sql-database-connect-query-java.md)|Ez a rövid útmutató azt ismerteti, hogyan használható a Java egy Azure SQL Database-adatbázishoz való csatlakozáshoz, és hogyan lehet Transact-SQL-utasítások használatával adatokat lekérdezni.|
|[Node.js](sql-database-connect-query-nodejs.md)|Ez a rövid útmutató azt ismerteti, hogyan használható a Node.js egy olyan program létrehozásához, amely egy Azure SQL Database-adatbázishoz csatlakozik, és hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni.|
|[PHP](sql-database-connect-query-php.md)|Ez a rövid útmutató azt ismerteti, hogyan használható a PHP egy olyan program létrehozásához, amely egy Azure SQL Database-adatbázishoz csatlakozik, és hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni.|
|[Python](sql-database-connect-query-python.md)|Ez a rövid útmutató azt ismerteti, hogyan lehet a Python használatával Azure SQL Database-adatbázishoz csatlakozni, valamint a Transact-SQL-utasítások használatával adatokat lekérdezni. |
|[Ruby](sql-database-connect-query-ruby.md)|Ez a rövid útmutató azt ismerteti, hogyan használható a Ruby egy olyan program létrehozásához, amely egy Azure SQL Database-adatbázishoz csatlakozik, és hogyan lehet Transact-SQL-utasításokkal adatokat lekérdezni.|
|||

## <a name="tls-considerations-for-sql-database-connectivity"></a>TLS-megfontolások az SQL Database-kapcsolatokhoz
A Transport Layer Security (TLS) protokollt az összes Microsoft által biztosított vagy támogatott illesztő használja az Azure SQL Database-hez való csatlakozáshoz. Nincs szükség különleges konfigurációra. Az SQL Server vagy az Azure SQL Database kapcsolataihoz javasoljuk, hogy az alkalmazásokhoz a következő vagy azokkal egyenértékű konfigurációkat állítson be:

 - **Encrypt = On**
 - **TrustServerCertificate = Off**

Egyes rendszerek eltérő, de egyenértékű kulcsszavakat használnak a fenti konfigurációs kulcsszavak helyett. Ezek a konfigurációk biztosítják, hogy az ügyfélillesztő ellenőrizze a kiszolgálótól kapott TLS-tanúsítvány identitását.

Javasoljuk továbbá, hogy tiltsa le az ügyfélen a TLS 1.1-et és 1.0-t, ha meg kell felelnie a Payment Card Industry – Data Security Standard (PCI-DSS) szabványnak.

Előfordulhat, hogy a nem a Microsofttól származó illesztők alapértelmezés szerint nem a TLS protokollt használják. Ez befolyásolhatja az Azure SQL Database-hez való csatlakozást. A beágyazott illesztőkkel rendelkező alkalmazások nem mindig teszik lehetővé a kapcsolatbeállítások szabályozását. Javasoljuk, hogy vizsgálja meg az ilyen illesztők és alkalmazások biztonságát, mielőtt bizalmas adatokat kezelő rendszereken használná őket.

## <a name="next-steps"></a>További lépések

A kapcsolati architektúrával kapcsolatos információkért tekintse meg [az Azure SQL Database kapcsolati architektúráját](sql-database-connectivity-architecture.md) ismertető cikket.
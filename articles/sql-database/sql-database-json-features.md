---
title: Az Azure SQL Database JSON-funkcióit |} A Microsoft Docs
description: Az Azure SQL Database lehetővé teszi elemzési, lekérdezéssel és JavaScript Object Notation (JSON) formátumban adatok formázása.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: dfbefd2ff4c2e99268c1e7e84154e93d58f542bb
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163630"
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Ismerkedés az Azure SQL Database-ben JSON-funkciókkal
Az Azure SQL Database lehetővé teszi a elemzése és kérdezhet le adatokat a JavaScript Object Notation jelölt [(JSON)](http://www.json.org/) formázhatja és exportálni a relációs adatok JSON-szövegben.

JSON-ja egy modern webes és mobilalkalmazások az adatcsere használt népszerű adatok formátuma. JSON félig strukturált adatok tárolására, naplófájlokban vagy hasonló NoSQL-adatbázisok esetében is alkalmazható [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). Számos REST webes szolgáltatás visszaadott eredmények formázott JSON-szöveget, vagy fogadja el az adatok JSON formátumú. A legtöbb Azure-szolgáltatások például [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), és [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) adja vissza, vagy JSON felhasználása REST-végpontokat.

Az Azure SQL Database lehetővé teszi a JSON-adatok egyszerűen dolgozhat, és az adatbázis integrálása a modern szolgáltatások.

## <a name="overview"></a>Áttekintés
Az Azure SQL Database a JSON-adatok használata a következő funkciókat biztosítja:

![JSON-függvények](./media/sql-database-json-features/image_1.png)

Ha JSON-szöveg, JSON-ból adatokat nyerhet ki, vagy győződjön meg arról, hogy a beépített funkciókkal megfelelően formázott JSON [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), és [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). A [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) funkció lehetővé teszi, hogy frissítse a JSON-szövegben lévő értéket. További speciális lekérdezés és elemzés céljából, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) függvény JSON-objektumok tömbje ismeretekké a sorkészletet. Bármely SQL-lekérdezés a visszaadott eredménykészlet hajtható végre. Végül pedig egy [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) záradék, amely lehetővé teszi a JSON-szövegként a relációs táblákban tárolt adatok formázása.

## <a name="formatting-relational-data-in-json-format"></a>Relációs adatok JSON formátumban formázása
Ha egy webszolgáltatás, hogy beolvassa az adatokat az adatbázisból. réteg és a JSON-választ biztosít formázhatja, vagy JSON-ként formázott ügyféloldali JavaScript-keretrendszert vagy -kódtárak együttese fogadja el az adatokat, egy SQL-lekérdezést közvetlenül a az adatbázis-tartalom formázhatók JSON-fájlként. Már van, amely formázza az eredményeket JSON-ként az Azure SQL Database-ből alkalmazáskódot írni, vagy néhány JSON szerializációs kódtárban átalakítása táblázatos lekérdezési eredményeket, és majd a JSON-formátumban való objektumok szerializálásának tartalmazza. Ehelyett használhatja a FOR JSON záradék SQL lekérdezési eredmények formázása az Azure SQL Database JSON-fájlként, és azt használni a saját alkalmazásában.

A következő példában a Sales.Customer tábla azon sorait formázott JSON-fájlként a FOR JSON záradék használatával:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

A FOR JSON PATH záradék a lekérdezés eredményeit, JSON-szövegként formázza. Oszlopnevek használják kulcsok, miközben az értékek JSON értékekként jönnek létre:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Az eredményhalmaz vannak formázva, egy JSON-tömb, ahol minden sor egy külön JSON-objektumként van formázva.

Elérési út azt jelzi, hogy testre szabható a JSON-eredményt kimeneti formátumba oszlopaliasnévként pontjelöléssel. A következő lekérdezés változik a kimenet JSON formátumban a "CustomerName" kulcs nevét, és a telefon-és faxszáma helyezi az "Ügyfél" alobjektumban:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Ez a lekérdezés kimenete a következőhöz hasonló:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

Ebben a példában azt helyett egy tömb egy egyetlen JSON-objektum által visszaadott megadása a [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) lehetőséget. Használhatja ezt a beállítást, ha tudja, hogy vannak-e visszatérő eredményeképpen query egyetlen objektumot.

A fő a FOR JSON záradék értéke, hogy lehetővé teszi, hogy összetett hierarchikus adatokat az adatbázisból, beágyazott JSON-objektum vagy tömb formájában adja vissza. Az alábbi példa bemutatja, hogyan foglalhat bele az ügyfél rendeléseinek beágyazott tömbként tartozó rendelések:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Ahogy az az alábbi kimeneti példa külön lekérdezéseket vásárlói adatokat, és majd beolvasni a kapcsolódó rendelések listáját, megtekintheti a szükséges adatok egyetlen lekérdezéssel, helyett:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>JSON-adatok használata
Ha nem rendelkezik szigorúan strukturált adatok, ha olyan alárendelt összetett objektumok, tömbök vagy hierarchikus adatokkal dolgozik, vagy ha időbeli alakulásával az adattárakon, a JSON-formátumban segítségével bármilyen összetett adatstruktúra jelölésére.

A szöveges formátum, mint minden más karakterláncot az Azure SQL Database-ben használható JSON-ja. Küldjön, vagy JSON-adatok tárolására, a standard szintű NVARCHAR:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

Ebben a példában használt JSON-adatokat az NVARCHAR(MAX) típust jelöli. JSON lehet beszúrni a táblába vagy megadva argumentumként, a tárolt eljárás standard Transact-SQL-szintaxis használatával az alábbi példában látható módon:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Bármely ügyféloldali nyelvi vagy tárat, a karakterlánc az Azure SQL Database működik is működik a JSON-adatok. JSON tárolható minden olyan tábla, amely támogatja az NVARCHAR típusú, például egy memóriaoptimalizált tábla vagy a rendszerverzióval ellátott tábla. JSON nem használ semmilyen megkötés vagy az ügyféloldali kódot, vagy az adatbázis-rétegben.

## <a name="querying-json-data"></a>JSON-adatok lekérdezése
Ha rendelkezik Azure SQL-táblákban tárolt JSON-ként formázott adatok, JSON-funkciók lehetővé teszik használja fel az adatokat bármely SQL-lekérdezésben.

Azure SQL database lehetővé teszik a JSON-függvények, mint bármely más SQL-adattípus JSON formátumú adatok kezelnie. Értékek kinyerése a JSON-szöveget, és JSON-adatokat használja a lekérdezés egyszerű:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

A JSON_VALUE függvény kigyűjti a oszlop tárolt JSON-szöveg értéket. Ez a függvény JavaScript-hez hasonló elérési utat használ való hivatkozáshoz egy JSON-szövegben kinyerni kívánt értéket. A kiolvasott értékkel bármely SQL-lekérdezés részeként is használható.

A JSON_QUERY függvény JSON_VALUE hasonlít. JSON_VALUE, ellentétben ez a függvény kinyeri a összetett alobjektumban például tömbök vagy objektumok kerülnek, JSON-szövegben.

A JSON_MODIFY funkció lehetővé teszi adja meg az elérési utat az érték a JSON-szöveget, hogy frissíteni kell, valamint egy új értéket, amely felülírja a régit. Ezzel a módszerrel JSON-szövegben anélkül, hogy a teljes struktúra reparsing egyszerűen frissíthetők.

Mivel a JSON-szövegként tárolt, garanciát nem jelentenek, hogy a szöveg oszlopaiban tárolt értékek megfelelően formázott. Ellenőrizheti, hogy tárolt JSON-oszlopban szereplő szöveg megfelelően van formázva standard szintű Azure SQL Database-ellenőrzési korlátozásokban, és a ISJSON függvény használatával:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Ha a bemeneti szöveg megfelelően formázott JSON-t, a ISJSON függvény 1 értéket adja vissza. Minden insert vagy update JSON oszlop ennél a határértéknél ellenőrzi, hogy új szöveg értéke nem JSON formátumú.

## <a name="transforming-json-into-tabular-format"></a>JSON átalakítása táblázatos formátumra
Az Azure SQL Database emellett lehetővé teszi átalakítása JSON-gyűjtemények táblázatos formátumban, és terhelés vagy lekérdezés JSON-adatokat nyerhet ki.

OPENJSON a táblázat értékű függvény, amely JSON-szöveget elemez, megkeresi a JSON-objektumok tömbje, a tömb elemei végighalad és egy sort adja vissza a kimeneti eredmények a tömb egyes elemei a rendszer.

![Táblázatos JSON](./media/sql-database-json-features/image_2.png)

A fenti példában adható meg helyének kijelölése a JSON-tömb, amely megnyitva kell lennie (a a $. Rendelések elérési út), mely oszlopok vissza kell adni az eredmény, valamint, hogy hol található cellákat visszaadott JSON-értékeit.

Hogy alakíthatja át a JSON-tömböt a @orders változó nagy mennyiségű sort és a egy csoportba az eredményhalmaz elemzése, vagy egy szabványos táblába beszúrandó sor:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
A Rendelések gyűjteményét formázott JSON-tömböt, és a tárolt eljárás paramétere elemzi, és a rendelések táblába beszúrt megadott.

## <a name="next-steps"></a>További lépések
Ismerje meg, hogyan JSON beágyazása az alkalmazásba, tekintse meg ezeket az erőforrásokat:

* [TechNet Blog](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [MSDN-dokumentáció](https://msdn.microsoft.com/library/dn921897.aspx)
* [Channel 9-videóban](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

A JSON integrálása az alkalmazásba különféle forgatókönyvekkel kapcsolatos tudnivalókért lásd: Ez a bemutatókat [Channel 9-videók](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) vagy nem található egy forgatókönyvet, amely megfelel a használati eset az [JSON-blogbejegyzések](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).


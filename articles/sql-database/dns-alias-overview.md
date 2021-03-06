---
title: DNS-alias az Azure SQL Database |} A Microsoft Docs
description: Az alkalmazások csatlakozhatnak az Azure SQL Database-kiszolgáló neve alias. Az alias mutat bármikor, és így tovább tesztelés elősegítése érdekében az SQL-adatbázis, módosíthatja.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: DhruvMsft
ms.author: dmalik
ms.reviewer: genemi,ayolubek
manager: craigg
ms.date: 02/05/2018
ms.openlocfilehash: 6c174871ff7bc61d11804e32aeac738bf6159c10
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47054115"
---
# <a name="dns-alias-for-azure-sql-database"></a>Az Azure SQL Database DNS-alias

Az Azure SQL Database tartozik egy tartománynévrendszer (DNS) kiszolgáló. PowerShell és REST API-k elfogadása [hívások hozhat létre és kezelhet a DNS-aliasokat](#anchor-powershell-code-62x) az SQL-adatbázis-kiszolgálójának nevét.

A *DNS-alias* az Azure SQL Database-kiszolgáló neve helyett használható. A kapcsolati karakterláncok az alias ügyfélprogramok használhatja. A DNS alias, amely a különböző kiszolgálókon átirányíthatja a ügyfélprogramok fordítási réteget biztosít. Ez a réteg megkíméli a nehézségeket, hogy a Keresés és az ügyfelek és a kapcsolati karakterláncok szerkesztése.

A DNS-alias gyakori használati módjai a következő esetekben:

- Hozzon létre egy könnyen megjegyezhető nevet egy Azure SQL Serverhez.
- Az alias kezdeti fejlesztése során egy SQL Database-kiszolgálók teszt utalhat. Amikor az alkalmazás élesíti, módosíthatja az aliast, tekintse meg az üzemi kiszolgáló. Teszt az éles környezetbe való áttérés nem igényel a konfigurációinak módosításairól számos, az adatbázis-kiszolgálóhoz csatlakozó ügyfelek.
- Tegyük fel, hogy a csak az alkalmazás-adatbázis áthelyezése egy másik SQL Database-kiszolgálóhoz. Itt módosíthatja a alias konfigurációit több ügyfél módosítása nélkül.

#### <a name="domain-name-system-dns-of-the-internet"></a>Tartománynévrendszer (DNS) az Internet

Az internetes DNS támaszkodik. A DNS-ben a rövid nevek fordítja le az Azure SQL Database-kiszolgáló nevét.

## <a name="scenarios-with-one-dns-alias"></a>Az egy DNS-alias forgatókönyvek

Tegyük fel, hogy át kell váltania, a rendszer egy új Azure SQL Database-kiszolgálóhoz. A múltban kellett keresse meg és minden ügyfél programban minden kapcsolati karakterlánc frissítése. De most, ha a kapcsolati karakterláncok DNS-aliast használni, csak egy aliast tulajdonság frissíteni kell.

A DNS-alias szolgáltatás az Azure SQL Database segítségével a következő esetekben:

#### <a name="test-to-production"></a>Ellenőrizze, hogy éles környezetben

Amikor az ügyfél-programok fejlesztése, rendelkezik a kapcsolati karakterláncokban lévő DNS-aliast használni őket. Az Azure SQL Database-kiszolgáló tesztelése verzióját választja ki az alias pont tulajdonságai között.

Később az új rendszer élesíti éles környezetben, ha az alias átirányítása az éles SQL Database-kiszolgáló tulajdonságainak frissítheti. Nem változik a ügyfélprogramok nem szükséges.

#### <a name="cross-region-support"></a>Régiók közötti támogatás

Vész-helyreállítási előfordulhat, hogy az SQL Database-kiszolgáló átvált egy másik földrajzi régióban. A rendszer DNS-alias használta a szolgáltatást, mint a kell megkeresnie és a kapcsolati karakterláncok frissítése az összes ügyfél elkerülhető. Ehelyett egy alias tekintse meg az új SQL Database-kiszolgáló, amely most az adatbázist tároló frissítheti.




## <a name="properties-of-a-dns-alias"></a>DNS-alias tulajdonságai

Minden DNS-alias az SQL Database-kiszolgálóhoz a következő tulajdonságok vonatkoznak:

- *Egyedi nevet:* egyes aliasnév hoz létre egyedi az összes Azure SQL Database-kiszolgálók között, ugyanúgy, mint a kiszolgáló nevében a rendszer.

- *Kiszolgálóra szükség:* A DNS alias nem hozható létre, ha pontosan egy kiszolgáló hivatkozik, és a kiszolgáló már léteznie kell. Frissített alias mindig kell hivatkoznia pontosan egy meglévő kiszolgáló.
    - Amikor egy SQL Database-kiszolgálóhoz, az Azure rendszer is csökken, tekintse meg a kiszolgáló összes DNS-aliasokat.

- *Bármelyik régióban nincs kötve:* DNS-aliasokat nem régióhoz kötött. Bármilyen DNS-aliasokat lehet frissíteni, tekintse meg az Azure SQL Database-kiszolgáló, amely minden olyan földrajzi régióban található.
    - Azonban amikor frissíti az alias egy másik kiszolgálóra utal, mindkét kiszolgáló léteznie kell az azonos Azure *előfizetés*.

- *Engedélyek:* kezelheti DNS-aliast, a felhasználónak rendelkeznie kell *Server Közreműködője* engedélyeket, vagy újabb verziója. További információkért lásd: [szerepköralapú hozzáférés-vezérlés az Azure Portalon – első lépések](../role-based-access-control/overview.md).





## <a name="manage-your-dns-aliases"></a>A DNS-aliasainak kezelése

Ahhoz, hogy programozott módon kezelheti a DNS-aliasokat REST API-k és a PowerShell-parancsmagok érhetők el.

#### <a name="rest-apis-for-managing-your-dns-aliases"></a>REST API-k a DNS-aliasok kezeléséhez

<!-- TODO
??2 "soon" in the following live sentence, is not the best situation.
TODO update this subsection very soon after REST API docu goes live.
Dev = Magda Bojarska
Comment as of:  2018-01-26
-->

A dokumentáció a REST API-k mellett a következő webes helyen érhető el:
- [Az Azure SQL Database REST API-val](https://docs.microsoft.com/rest/api/sql/)

Emellett a REST API-k a Githubon található láthatja:
- [Az Azure SQL Database-kiszolgálóhoz, a DNS alias REST API-k](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/sql/resource-manager/Microsoft.Sql/preview/2017-03-01-preview/serverDnsAliases.json)

<a name="anchor-powershell-code-62x"/>

#### <a name="powershell-for-managing-your-dns-aliases"></a>A DNS-aliasok kezeléséhez készült PowerShell-lel

PowerShell-parancsmagok érhetők el, amely a REST API-jainak hívására.

Kódrészlet például a DNS-aliasok kezeléséhez használt PowerShell-parancsmagokat dokumentált:
- [Az Azure SQL Database DNS-alias PowerShell](dns-alias-powershell.md)


A példakódban alkalmazott parancsmagok a következők:
- [Új-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/New-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): egy új DNS-alias létrehozása az Azure SQL Database szolgáltatás rendszerben. Az alias az Azure SQL Database-kiszolgáló 1 hivatkozik.
- [A GET-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): lekérés és listázás 1 SQL-adatbázis-kiszolgálóhoz rendelt összes DNS-aliasokat.
- [A Set-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Set-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): módosítja a kiszolgáló nevét, amely az alias konfigurálva van kiszolgálóról 1, 2. az SQL DB kiszolgáló hivatkoznak.
- [A Remove-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Remove-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): a DNS-alias eltávolítása az SQL DB kiszolgáló 2, az alias nevét.


A fenti parancsmagok lettek hozzáadva a **azurerm.SQL-hez** modul 5.1.1-es modul verziótól kezdve.




## <a name="limitations-during-preview"></a>Korlátozások az előzetes verzióban

Jelenleg a DNS alias a következő korlátozások vonatkoznak:

- *Legfeljebb 2 perces késleltetés:* frissíteni vagy eltávolítani egy DNS-alias akár 2 percig tart.
    - Minden olyan rövid késleltetés, függetlenül az alias azonnal leállítja az örökölt kiszolgálóra ügyfélkapcsolatok hivatkozó.

- *A DNS-címkeresés:* most a csak mérvadó módja végrehajtásával ellenőrizze, milyen kiszolgálói aliast hivatkozik a megadott DNS- [DNS-címkeresés](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup).

- *[Nem támogatott a táblanaplózás](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md):* DNS-alias nem használható az Azure SQL Database-kiszolgáló, amely rendelkezik *táblanaplózás* egy adatbázison engedélyezve van.
    - A táblanaplózás elavult.
    - Azt javasoljuk, hogy áthelyezi [Blobnaplózás](sql-database-auditing.md).




## <a name="related-resources"></a>Kapcsolódó források (lehet, hogy a cikkek angol nyelvűek)

- [Az Azure SQL Database üzletmenet-folytonossági funkcióinak áttekintése](sql-database-business-continuity.md), beleértve a vész-helyreállítási.



## <a name="next-steps"></a>További lépések

- [Az Azure SQL Database DNS-alias PowerShell](dns-alias-powershell.md)


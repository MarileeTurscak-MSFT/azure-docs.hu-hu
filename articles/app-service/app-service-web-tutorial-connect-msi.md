---
title: Az SQL Database-kapcsolatot biztonságossá tétele az App Service-ből felügyeltszolgáltatás-identitás segítségével | Microsoft Docs
description: Tegye biztonságosabbá adatbázis-kapcsolatát felügyeltszolgáltatás-identitás segítségével, és alkalmazza ezt egyéb Azure-szolgáltatásoknál is.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/17/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 173588c0200666c52f3ac0a5d2e70d667cfe3294
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445561"
---
# <a name="tutorial-secure-sql-database-connection-with-managed-service-identity"></a>Oktatóanyag: Az SQL Database-kapcsolat védelmének biztosítása felügyeltszolgáltatás-identitás segítségével

Az [App Service](app-service-web-overview.md) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás az Azure-ban. [Felügyeltszolgáltatás-identitást](app-service-managed-service-identity.md) biztosít az alkalmazásához, vagyis egy kulcsrakész megoldást, amely biztosítja az [Azure SQL Database-hez](/azure/sql-database/) és egyéb Azure-szolgáltatásokhoz való hozzáférés védelmét. Az App Service-ben található felügyeltszolgáltatás-identitások biztonságosabbá teszik alkalmazását a titkos kódok, pl. a kapcsolati sztringekban lévő hitelesítő adatok szükségességének megszüntetésével. Ebben az oktatóanyagban hozzáadja a felügyeltszolgáltatás-identitást ahhoz a minta ASP.NET-webalkalmazáshoz, amelyet az [Oktatóanyag: ASP.NET-alkalmazás létrehozása az Azure-ban SQL Database használatával](app-service-web-tutorial-dotnet-sqldatabase.md) című szakaszban hozott létre. Ha ezzel végzett, a mintaalkalmazása biztonságosan csatlakozhat az SQL Database-hez, felhasználónév és jelszó használata nélkül.

Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Felügyeltszolgáltatás-identitás engedélyezése
> * SQL Database-hozzáférés engedélyezése a szolgáltatásidentitáshoz
> * Alkalmazáskód konfigurálása SQL Database-hitelesítéshez, az Azure Active Directory-hitelesítés segítségével
> * Minimális jogosultságok engedélyezése a szolgáltatásidentitáshoz az SQL Database-ben

> [!NOTE]
> Az Azure Active Directory hitelesítése _eltér_ a helyszíni Active Directoryban (AD DS) lévő [integrált Windows-hitelesítéstől](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)). Az AD DS és az Azure Active Directory teljesen más hitelesítési protokollt használ. További információk [a Windows Server AD DS és az Azure AD közötti különbségekről](../active-directory/fundamentals/understand-azure-identity-solutions.md#the-difference-between-windows-server-ad-ds-and-azure-ad).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk az [Oktatóanyag: ASP.NET-alkalmazás létrehozása az Azure-ban SQL Database használatával](app-service-web-tutorial-dotnet-sqldatabase.md) szakasz folytatása. Ha még nem tette meg, kövesse az oktatóanyag utasításait. Alternatív megoldásként hozzáigazíthatja a lépéseket a saját ASP.NET-alkalmazása az SQL Database-zel való használatához.

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-service-identity"></a>Felügyeltszolgáltatás-identitás engedélyezése

Ha engedélyezni szeretné a szolgáltatásidentitást az Azure-alkalmazásához, használja az [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) parancsot a Cloud Shellben. Az alábbi parancsban cserélje le az *\<alkalmazásnév>* elemet.

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app name>
```

Egy példa az Azure Active Directory-ban létrehozott identitás kimenetére:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

A következő lépésben használni fogja a `principalId` értékét. Ha többet szeretne megtudni az Azure Active Directory új identitásáról, futtassa az alábbi választható parancsot a `principalId` értékével:

```azurecli-interactive
az ad sp show --id <principalid>
```

## <a name="grant-database-access-to-identity"></a>Adatbázis-hozzáférés engedélyezése az identitáshoz

A következő lépésben engedélyezi az adatbázis-hozzáférést az alkalmazása szolgáltatásidentitásához. Ehhez futtassa az [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin_create) parancsot a Cloud Shellben. Az alábbi parancsban cserélje le a *\<kiszolgáló_neve>* elemet és az <előző_lépés_principalid_értéke> elemet. Adjon meg egy rendszergazdanevet a *\<rendszergazdai_felhasználó >* elemnél.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

A felügyeltszolgáltatás-identitás ezentúl hozzáférhet az Azure SQL Database-kiszolgálójához.

## <a name="modify-connection-string"></a>Kapcsolati sztring módosítása

Módosítsa az alkalmazásához előzőleg beállított kapcsolatot. Ehhez futtassa az [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) parancsot a Cloud Shellben. Az alábbi parancsban cserélje le az *\<alkalmazásnév>* elemet a saját alkalmazásának nevére, és cserélje le a *\<kiszolgáló_neve>* és az *\<adatbázis_neve>* elemet az SQL Database értékeire.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>ASP.NET-kód módosítása

A Visual Studióban, a **DotNetAppSqlDb** projektjében nyissa meg a _packages.config_ elemet, és adja hozzá a következő sort a csomagok listájához.

```xml
<package id="Microsoft.Azure.Services.AppAuthentication" version="1.1.0-preview" targetFramework="net461" />
```

Nyissa meg a _Models\MyDatabaseContext.cs_ elemet, és adja hozzá a következő `using` utasításokat a fájl elejéhez:

```csharp
using System.Data.SqlClient;
using Microsoft.Azure.Services.AppAuthentication;
using System.Web.Configuration;
```

A `MyDatabaseContext` osztályban adja hozzá a következő konstruktort:

```csharp
public MyDatabaseContext(SqlConnection conn) : base(conn, true)
{
    conn.ConnectionString = WebConfigurationManager.ConnectionStrings["MyDbConnection"].ConnectionString;
    // DataSource != LocalDB means app is running in Azure with the SQLDB connection string you configured
    if(conn.DataSource != "(localdb)\\MSSQLLocalDB")
        conn.AccessToken = (new AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;

    Database.SetInitializer<MyDatabaseContext>(null);
}
```

Ez a konstruktor konfigurál egy egyéni SqlConnection objektumot annak érdekében, hogy az App Service-ből használhassa a hozzáférési jogkivonatot az Azure SQL Database-ben. A hozzáférési jogkivonattal az App Service-alkalmazása hitelesíti az Azure SQL Database-t a felügyeltszolgáltatás-identitás segítségével. További információt a [jogkivonatok Azure-erőforrásokhoz való beszerzéséről](app-service-managed-service-identity.md#obtaining-tokens-for-azure-resources) szóló témakörben talál. Az `if` utasítás lehetővé teszi, hogy a LocalDB segítségével továbbra is tesztelje alkalmazását helyileg.

> [!NOTE]
> Az `SqlConnection.AccessToken` jelenleg csak a .NET-keretrendszerben (4.6-os vagy újabb verzió) támogatott, a [.NET Core-ban](https://www.microsoft.com/net/learn/get-started/windows) nem.
>

Ha használni szeretné ezt az új konstruktort, nyissa meg a `Controllers\TodosController.cs` fájlt, és keresse meg a `private MyDatabaseContext db = new MyDatabaseContext();` sort. A meglévő kód az alapértelmezett `MyDatabaseContext` vezérlőt használja, hogy a standard kapcsolati sztringgel adatbázist hozzon létre, amely [a módosítás előtt](#modify-connection-string) tiszta szöveges felhasználónévvel és jelszóval rendelkezett.

Cserélje le a teljes sort az alábbi kódra:

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new SqlConnection());
```

### <a name="publish-your-changes"></a>A módosítások közzététele

Már csak közzé kell tennie a módosításait az Azure-ban.

A **Solution Explorer** (Megoldáskezelő) lapon kattintson a jobb gombbal a **DotNetAppSqlDb** projektre, és válassza a **Publish** (Közzététel) elemet.

![Közzététel a Megoldáskezelőből](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

A közzétételi oldalon kattintson a **Publish** (Közzététel) elemre. Amikor az új weblapon megjelenik a feladatlista, az alkalmazása kapcsolódik az adatbázishoz a felügyeltszolgáltatás-identitás segítségével.

![Az Azure webalkalmazás a Code First migrálás után](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Most már ugyanúgy szerkesztheti a feladatlistát, mint korábban.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>Minimális jogosultságok engedélyezése az identitáshoz

Az előző lépések során valószínűleg észrevette, hogy a felügyeltszolgáltatás-identitása Azure AD-rendszergazdaként kapcsolódik az SQL Serverhez. Ahhoz, hogy engedélyezze a minimális jogosultságokat a felügyeltszolgáltatás-identitásához, Azure AD-rendszergazdaként kell bejelentkeznie az Azure SQL Database-kiszolgálóba, majd hozzá kell adnia egy Azure Active Directory-csoportot, amely a szolgáltatásidentitást tartalmazza. 

### <a name="add-managed-service-identity-to-an-azure-active-directory-group"></a>A felügyeltszolgáltatás-identitás hozzáadása egy Azure Active Directory-csoporthoz

A Cloud Shellben adja hozzá az alkalmazásának felügyeltszolgáltatás-identitását egy új Azure Active Directory-csoporthoz, amelynek neve _myAzureSQLDBAccessGroup_, az alábbi szkriptben látható módon:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Ha minden egyes parancsnál meg szeretné tekinteni a JSON-kimenetet, hagyja el a `--query objectId --output tsv` paramétereket.

### <a name="reconfigure-azure-ad-administrator"></a>Az Azure AD-rendszergazda újrakonfigurálása

Előzőleg Azure AD-rendszergazdaként rendelte hozzá a felügyeltszolgáltatás-identitást az SQL Database-hez. Ezt az identitást nem használhatja interaktív bejelentkezéshez (adatbázis-felhasználók hozzáadásához), ezért az igazi Azure AD-felhasználóját kell használnia. Az Azure AD-felhasználója hozzáadásához kövesse az [Azure Active Directory-rendszergazda az Azure SQL Database-kiszolgálóhoz való regisztrálása ](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) témakörrel foglalkozó szakasz lépéseit. 

### <a name="grant-permissions-to-azure-active-directory-group"></a>Engedélyek kiosztása az Azure Active Directory-csoportnak

A Cloud Shellben az SQLCMD parancsot használva jelentkezzen be az SQL Database-be. Cserélje le a _\<kiszolgálónevet>_ az SQL Database kiszolgálónevére, és cserélje le az _\<AADfelhasználónév>_, valamint az _\<AADjelszó>_ elemeket az Azure AD-felhasználójának hitelesítő adataira.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

A SQL-parancssorban futtassa az alábbi parancsokat, amelyek hozzáadják az Ön által korábban felhasználóként létrehozott Azure Active Directory-csoportot.

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
GO
```

Az `EXIT` parancs begépelésével térjen vissza a Cloud Shell-parancssorba. Ezután ismét futtassa az SQLCMD parancsot, de pontosítsa az adatbázis nevét az _\<adatbázisnév>_ elem helyén.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

Az SQL-parancssorban az Ön által választott adatbázishoz futtassa az alábbi parancsokat olvasási és írási engedélyek az Azure Active Directory-csoporthoz való hozzárendeléséhez.

```sql
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

## <a name="next-steps"></a>További lépések

Az alábbiak elvégzését ismerte meg:

> [!div class="checklist"]
> * Felügyeltszolgáltatás-identitás engedélyezése
> * SQL Database-hozzáférés engedélyezése a szolgáltatásidentitáshoz
> * Alkalmazáskód konfigurálása SQL Database-hitelesítéshez, az Azure Active Directory-hitelesítés segítségével
> * Minimális jogosultságok engedélyezése a szolgáltatásidentitáshoz az SQL Database-ben

Lépjen a következő oktatóanyaghoz, amelyből megtudhatja, hogyan képezhet le egyedi DNS-nevet a webalkalmazáshoz.

> [!div class="nextstepaction"]
> [Meglévő egyéni DNS-név hozzákapcsolása az Azure-webalkalmazásokhoz](app-service-web-tutorial-custom-domain.md)

---
title: Az Azure Automation integrációs modul létrehozása
description: Ez az oktatóprogram végigvezeti az integrációs modulok létrehozásán, tesztelésén és példahasználatán az Azure Automationben.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 93c61f0b9b923f84b2c84d2db4456442e2f9fb27
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444504"
---
# <a name="azure-automation-integration-modules"></a>Azure Automation integrációs modulok
Az Azure Automation mögötti alapvető technológia a PowerShell. Minthogy az Azure Automation a PowerShellre épül, a PowerShell-modulok kulcsfontosságúak az Azure Automation bővíthetősége szempontjából. Ebben a cikkben azt végigvezeti Önt az Azure Automation "Integrációs modulok", és a saját PowerShell-modulok létrehozásának ajánlott eljárásai, győződjön meg arról, hogy azok biztosan működjenek integrációs modulként az Azure PowerShell-modulok használatát tulajdonságairól Automation. 

## <a name="what-is-a-powershell-module"></a>Mi az a PowerShell-modul?
Egy PowerShell-modul olyan PowerShell-parancsmagok csoportja, mint a **Get-Date** vagy a **Copy-Item**, amelyek használhatók a PowerShell konzolról, valamint olyan parancsfájlokból, munkafolyamatokból, forgatókönyvekből és PowerShell DSC-erőforrásokból, mint a WindowsFeature vagy File, amelyek használhatók PowerShell DSC konfigurációkból. A PowerShell összes funkciója parancsmagok és DSC-erőforrások révén jelenik meg, és minden parancsmag/DSC-erőforrás mögött egy PowerShell modul áll. Ezek jelentős része magához a PowerShellhez tartozik. A **Get-Date** parancsmag például része a Microsoft.PowerShell.Utility PowerShell-modulnak, a **Copy-Item** parancsmag része a Microsoft.PowerShell.Management PowerShell-modulnak, a Csomag DSC erőforrás pedig része a PSDesiredStateConfiguration PowerShell-modulnak. A PowerShellben mindkét modul megtalálható. Sok PowerShell-modul azonban nem része automatikusan a PowerShellnek, hanem olyan első vagy harmadik felek termékeivel érkeznek, mint a System Center 2012 Configuration Manager, illetve a hatalmas PowerShell-közösség biztosítja például a PowerShell-galériához hasonló helyeken. A modulok hasznosak, mert a beágyazott funkciók révén egyszerűbbé teszik a bonyolult feladatokat.  További információt talál a [PowerShell-modulokról az MSDN webhelyén](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>Mi az az Azure Automation integrációs modul?
Az integrációs modul nem különbözik egy PowerShell-modul. Egyszerűen egy olyan PowerShell-modul, amely opcionálisan tartalmaz egy további fájlt, egy Azure Automation kapcsolattípust megadó metaadatfájlt, amely a modul parancsmagjaival használható a forgatókönyvekben. Ezek a PowerShell-modulok (az opcionális fájllal vagy anélkül) importálhatók az Azure Automationbe, hogy a parancsmagjaik használhatók legyenek a forgatókönyvekben, és a DSC-erőforrásaik elérhetők legyenek használatra a DSC-konfigurációkon belülről. A színfalak mögött az Azure Automation tárolja ezeket a modulokat, és a forgatókönyv-feladat és a DSC-fordítási feladat a végrehajtásakor betölti őket az Azure Automation próbakörnyezetbe, ahol a rendszer végrehajtja a forgatókönyveket és a lefordítja a DSC-konfigurációkat. A modulok DSC-erőforrásai is automatikusan kikerülnek az Automation DSC lekéréses kiszolgálóra, így a DSC-konfigurációkat alkalmazni próbáló gépek lekérhetik őket.  

Az Azure Automation számos Azure PowerShell-modult tartalmaz használatra készen, hogy azonnal elkezdhesse az Azure-felügyelet automatizálását, továbbá importálhat PowerShell-modulokat is bármilyen integrálni kívánt rendszerhez, szolgáltatáshoz vagy eszközhöz. 

> [!NOTE]
> Bizonyos modulok „globális modulként” érhetők el az Automation szolgáltatásban. Ezek a globális modulok elérhetők, ha létrehoz egy automation-fiókot, és azokat időnként frissítjük, ami automatikusan leküldi az automation-fiókhoz. Ha nem szeretné, automatikus frissítését, bármikor importálhatja ugyanazon modulokat magát, és, amely élvez a globális modulverziókkal ezt a modult, amely a szolgáltatásban közzéteszünk. 

Az importálni kívánt integrációsmodul-csomag formátuma a modullal azonos nevet viselő, .zip kiterjesztésű tömörített fájl. A mappa tartalmazza a Windows PowerShell-modult és minden kiegészítő fájlt, beleértve a jegyzékfájlt (.psd1) is, ha a modul rendelkezik ilyennel.

Ha a modul esetleg tartalmaz egy Azure Automation kapcsolattípust, akkor tartalmaznia kell egy `<ModuleName>-Automation.json` nevű fájlt is, amely meghatározza a kapcsolattípus tulajdonságait. Ez a tömörített .zip fájl modulmappájában elhelyezett json-fájl, és tartalmazza a modul által képviselt rendszerhez vagy szolgáltatáshoz való csatlakozáshoz szükséges „kapcsolat” mezőit. Ez a kapcsolattípus létrehozása az Azure Automationben említi. Ezzel a fájllal megadhatja a modul kapcsolattípusához a mezőneveket, a típusait, valamint azt, hogy a mezőket titkosítani kell-e és/vagy hogy kötelezők-e. Az alábbi egy json fájlformátumú sablon:

```
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

Ha telepítette a Service Management Automation, és létrehozott Integrációsmodul-csomagokat az automatizálási forgatókönyvekhez, ez Bizonyára ismerős lesz Önnek. 

## <a name="authoring-best-practices"></a>Szerzői gyakorlati tanácsok
Bár az integrációs modulok lényegében PowerShell modulok, számos dolog van még, amit érdemes meggondolni egy PowerShell-modul létrehozásakor, hogy a lehető legtöbb hasznukat vegye az Azure Automationben. Ezeknek egy része kifejezetten az Azure Automationre vonatkozik, mások pedig ahhoz hasznosak, hogy a moduljai jól működjenek a PowerShell-munkafolyamatban, függetlenül attól, hogy használja-e az Automationt, vagy sem. 

1. A modul összes parancsmagjáról mellékeljen egy szinopszist, egy leírást és egy súgó URI-t. A PowerShellben meghatározhat bizonyos súgóinformációkat a parancsmagokhoz, hogy a felhasználó segítséget kapjon a **Get-Help** parancsmaggal való használatukkor. Az alábbi példát követve meghatározhatja a szinopszist és a súgó URI-t egy .psm1 fájlban megírt PowerShell-modulhoz.<br>  
   
    ```
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='http://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```
   <br> Ha ezt az információt megadja, nem csak a súgó jelenik meg a **Get-Help** parancsmag segítségével a PowerShell konzolon, de megjeleníti ezt a súgó funkciót az Azure Automation felületén is.  Például amikor tevékenységet szúr be a forgatókönyvek létrehozása alatt. Ha a „Részletes súgó megtekintése” lehetőségre kattint, megnyílik a súgó URI az Azure Automation eléréséhez használt webböngésző új lapján.<br>![Integrációs modul súgó](media/automation-integration-modules/automation-integration-module-activitydesc.png)
1. Ha a modul egy távoli rendszeren fut:

    a. Tartalmaznia kell az integrációs modulhoz tartozó metaadatfájlt, amely meghatározza az adott távoli rendszerhez való csatlakozáshoz szükséges információkat, azaz a kapcsolat típusát.  
    b. A modul minden egyes parancsmagjának képesnek kell lennie egy kapcsolat objektum (az adott kapcsolattípus egy példánya) befogadására paraméterként.  

    A modulban lévő parancsmagokat könnyebb használni az Azure Automationben, ha lehetővé teszi, hogy egy objektumot a kapcsolattípus mezőivel paraméterként továbbítson a parancsmagnak. Ily módon a felhasználóknak nem kell leképezniük a kapcsolat adategység-paramétereit a parancsmag megfelelő paramétereihez minden alkalommal, amikor meghívnak egy parancsmagot. A fenti forgatókönyv-példa alapján egy CorpTwilio nevű Twilio-kapcsolati adategység segítségével éri el a Twiliót, és visszaadja az összes telefonszámot a fiókból.  Figyelje meg, hogyan képezi le a kapcsolat mezőit a parancsmag paramétereire.<br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Egyszerűbb és jobb megoldás erre, ha közvetlenül továbbítja a kapcsolat objektumot a parancsmagnak:
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    Ilyen viselkedést engedélyezhet a parancsmagjaihoz, ha lehetővé teszi, hogy elfogadjon egy kapcsolat objektumot közvetlenül paraméterként ahelyett, hogy ha csak egy paraméter kapcsolatmezői lennének. Általában érdemes minden, egy paramétert, hogy a felhasználó nem használja az Azure Automation is meghívhatja a parancsmagjait anélkül hozhat létre, egy kapcsolat objektumként paraméterkészletre. Az alábbi **SpecifyConnectionFields** paraméterkészlettel továbbíthatja egyenként a kapcsolatmező-tulajdonságokat. A **UseConnectionObject** segítségével továbbíthatja egyenesen a kapcsolatot. Ahogy látja, a [Twilio PowerShell-modul](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) Send-TwilioSMS parancsmagja lehetővé teszi bármelyik továbbítást. 
   
    ```
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='http://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```
   <br>
1. Adja meg a modulban lévő összes parancsmag kimenettípusát. Egy parancsmag kimenettípusának megadása lehetővé teszi a tervezés közben az IntelliSense használatát, amely segít meghatározni egy parancsmag kimenetének tulajdonságait a megírásuk támogatására. Ez különösen hasznos az Automation-forgatókönyvek grafikus létrehozásakor, ahol a tervezés közbeni információk kulcsfontosságúak a könnyedfelhasználói élmény biztosításához a modulban.<br><br> ![Grafikus runbook kimenettípusa](media/automation-integration-modules/runbook-graphical-module-output-type.png)<br> Ez hasonlít egy parancsmag PowerShell ISE-ben kapott kimenetének „type ahead” funkciójára, csak nem kell futtatni.<br><br> ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)<br>
1. A modul parancsmagjai elméletben nem fogadnak komplex objektumtípusokat paraméterekként. A PowerShell-munkafolyamat eltér a PowerShelltől abban, hogy komplex típusokat tárol deszerializált formában. Az egyszerű típusok egyszerűek maradnak, de a komplex típusok deszerializált változatokká konvertálja, amelyek lényegében tulajdonságcsomagok. Ha például a **Get-Process** parancsmagot használta egy forgatókönyvben (vagy egy PowerShell-munkafolyamatban), az egy [Deserialized.System.Diagnostic.Process] típusú, nem pedig a várt [System.Diagnostic.Process] típusú objektumot ad vissza. Ez a típus ugyanazokkal a tulajdonságokkal rendelkezik, mint a nem deszerializált típus, de a metódusok nélkül. És ha megpróbálja továbbítja ezt az értéket paraméterként egy parancsmagnak, ahol a parancsmag a paraméter [System.Diagnostic.Process] értéket vár, a következő hibaüzenetet kapja: *argumentum-átalakítás a 'process' paraméter nem lehet feldolgozni. Hiba: „Nem lehetséges a „Deserialized.System.Diagnostics.Process” típusú „System.Diagnostics.Process (CcmExec)” érték konvertálása a következő típusra: „System.Diagnostics.Process”.*   Ennek az az oka, hogy típusbeli eltérés van a várt [System.Diagnostic.Process] típus és a megadott [Deserialized.System.Diagnostic.Process] típus között. A probléma megoldása az, ha biztosítjuk, hogy a modul parancsmagjai ne fogadjanak be komplex típusú paramétereket. Itt látható ennek a téves megközelítése.
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    Ez pedig a helyes mód: befogad egy primitívet, amely belső használatával a parancsmag befogadhatja és használhatja a komplex objektumot. Minthogy a parancsmagokat a rendszer a PowerShell környezetében hajtja végre, nem a PowerShell-munkafolyamatéban, a parancsmagon belül $process lesz a helyes [System.Diagnostic.Process] típus.  
   
    ```
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```
   <br>
   A kapcsolati adategységek a forgatókönyvekben kivonattáblák, amelyek komplex típusúak, de úgy tűnik, hogy ezek a kivonattáblák bekerülhetnek a parancsmagokba –Connection paraméterként, és a rendszer nem ad vissza kivételt. Technikailag bizonyos PowerShell-típusok képesek megfelelően átalakulni a szerializált formájukból a deszerializált formájukba, és így átadhatók a nem deszerializált típust fogadó paraméterként a parancsmagoknak. A kivonattábla egy ezek közül. Lehetséges implementálni egy modul szerzőjének definiált típusait oly módon, hogy a deszerializálásuk is helyesen történjen meg, de ehhez néhány dolgot feláldozását érdemes fontolóra venni. A típusnak rendelkeznie kell alapértelmezett konstruktorral, minden tulajdonságának nyilvánosnak kell kennie, és rendelkeznie kell egy PSTypeConverter elemmel. A modulkészítő által nem birtokolt, már definiált típusoknál nincs mód a „javításra”, így azt javasoljuk, hogy általában kerülje a paramétereknél az összetett típusokat. Forgatókönyv-készítési tipp: Ha valamilyen okból a parancsmagjainak komplex típusú paraméterre van szüksége, vagy valaki más komplex típusú paramétert igénylő moduljára van szüksége, PowerShell-alapú munkafolyamat-forgatókönyvekben és helyi PowerShellben lévő PowerShell-munkafolyamatokban a megoldás az, hogy ha becsomagolja a komplex típust létrehozó parancsmagot és a komplex típust használó parancsmagot ugyanabba az InlineScript-tevékenységbe. Minthogy az InlineScript a tartalmait inkább a PowerShellhez, mint a PowerShell-munkafolyamathoz hasonlóan hajtja végre, a komplex típust létrehozó parancsmag a helyes típust hozza létre, nem a deszerializált komplex típust.
1. A modul egyik parancsmagjának se legyen állapota. A PowerShell-munkafolyamat a munkafolyamatban meghívott parancsmagok mindegyikét külön munkamenetben hívja meg. Ez azt jelenti, hogy azok a parancsmagok, amelyek ugyanazon modul más parancsmagjai által létrehozott/módosított munkamenet-állapottól függenek, nem fognak működni a PowerShell-alapú munkafolyamat-forgatókönyvekben.  Az alábbi példa bemutatja, mit ne tegyen.
   
    ```
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```
   <br>
1. A modulnak teljes egészében egy Xcopy-kompatibilis csomagban kell lennie. Mivel az Azure Automation-modulokat a rendszer elosztja az Automation próbakörnyezetekbe, amikor a forgatókönyveket végre kell hajtani, függetlenül kell működniük a gazdagéptől, amelyen futnak. Ez azt jelenti, hogy képesnek kell lennie a modulcsomag tömörítésére és bármely más, azonos vagy újabb PowerShell-verziót futtató gazdagépre való áthelyezésére, valamint normális működtetésére, amikor a rendszer importálja az adott gazdagép PowerShell környezetébe. Annak érdekében, hogy ez megtörténjen, fontos, hogy a modul ne függjön egy fájltól sem a modulmappán kívül (ez a Azure Automationbe való importáláskor tömörített mappa), sem a gazdagép bármilyen egyedi beállításjegyzékétől, mint például a termék telepítésekor beállítottaktól. Ha nem tartja be az ajánlott gyakorlatot, a modul nem lesz használható az Azure Automationben.  

## <a name="next-steps"></a>További lépések

* A PowerShell-alapú munkafolyamat-forgatókönyvekkel való ismerkedéshez tekintse meg a következőt: [Az első PowerShell-alapú munkafolyamat-forgatókönyvem](automation-first-runbook-textual.md)
* További információk PowerShell-modulok létrehozásáról: [Windows PowerShell-modul írása](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx).


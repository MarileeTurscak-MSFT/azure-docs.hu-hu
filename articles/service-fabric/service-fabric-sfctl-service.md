---
title: Az Azure Service Fabric parancssori felület - sfctl-szolgáltatás |} A Microsoft Docs
description: Ismerteti a Service Fabric parancssori felület sfctl szolgáltatás parancsaihoz.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 84c2faaf137e19d78e7e17527feb50baebf8041b
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494574"
---
# <a name="sfctl-service"></a>sfctl service
Létrehozása, törlése és kezelése a service, a szolgáltatástípusok és a szolgáltatáscsomagok.

## <a name="commands"></a>Parancsok

|Parancs|Leírás|
| --- | --- |
| alkalmazás-neve | Lekérdezi egy szolgáltatás a Service Fabric-alkalmazás neve. |
| Code-package-list | A Service Fabric csomópont telepítve kódcsomagok listájának beolvasása. |
| létrehozás | Hozza létre a megadott Service Fabric-szolgáltatást. |
| delete | Törli a már meglévő Service Fabric-szolgáltatást. |
| üzembe helyezett típusa | Egy megadott szolgáltatás típusú Service Fabric-fürt egyik csomópontján üzembe helyezett alkalmazás adatainak beolvasása. |
| üzembe helyezett-type-list | A Service Fabric-fürt egyik csomópontján üzembe helyezett alkalmazások szolgáltatástípusok információt tartalmazó lista beolvasása. |
| leírás | Beolvassa egy meglévő Service Fabric-szolgáltatás leírását. |
| get-container-logs | Lekéri a tároló naplóit tároló üzembe helyezett egy Service Fabric-csomóponton. |
| állapot | A megadott Service Fabric-szolgáltatás állapotának beolvasása. |
| információ | A Service Fabric-alkalmazáshoz tartozó szolgáltatás adatainak beolvasása. |
| lista | Az alkalmazás által meghatározott az alkalmazáshoz tartozó összes szolgáltatás adatainak beolvasása |
| Manifest | A jegyzékfájl szolgáltatástípus leíró beolvasása. |
| csomag üzembe helyezése | A lemezkép-gyorsítótárból a megadott csomópont a megadott Szolgáltatásjegyzék társított csomagokat, letölti. |
| csomag-health | Lekérdezi az állapotfigyelő szolgáltatás információ üzembe helyezett egy Service Fabric-csomópont és az alkalmazás egy adott alkalmazáshoz. |
| csomag – információ | Beolvassa a megadott névnek megfelelő Service Fabric csomópont telepítve service-csomagok listáját. |
| Package-list | A Service Fabric csomópont telepítve szolgáltatáscsomagok listájának beolvasása. |
| helyreállítás | Azt jelzi, hogy a Service Fabric-fürthöz, hogy kell próbálnia helyreállítani a megadott szolgáltatás, amely jelenleg kvórumveszteségben elakadt. |
| report-health | A Service Fabric service health jelentést küld. |
| megoldás | Oldja meg a Service Fabric-partícióhoz. |
| típus-list | A Service Fabric-fürtben üzembe helyezett alkalmazás típus által támogatott szolgáltatástípusok információkat tartalmazó lista beolvasása. |
| update | Frissíti a megadott szolgáltatás használata az adott frissítés leírása. |

## <a name="sfctl-service-app-name"></a>sfctl-service-alkalmazás-neve
Lekérdezi egy szolgáltatás a Service Fabric-alkalmazás neve.

Lekérdezi a megadott szolgáltatás az alkalmazás nevére. 404-es FABRIC_E_SERVICE_DOES_NOT_EXIST hibaüzenetet ad vissza, ha a szolgáltatás, amely a megadott azonosító nem létezik.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. Ez az azonosító általában nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-code-package-list"></a>sfctl szolgáltatás code-package-list
A Service Fabric csomópont telepítve kódcsomagok listájának beolvasása.

Lekérdezi a listát a kód egy Service Fabric-csomópont az adott alkalmazás telepíthető.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| --csomópontnév [kötelező] | A csomópont neve. |
| --code-package-neve | A szolgáltatásjegyzék-alkalmazástípus a Service Fabric-fürt részeként regisztrált megadott kód csomag neve. |
| --szolgáltatás jegyzékfájl neve | A szolgáltatásjegyzék-alkalmazástípus a Service Fabric-fürt részeként regisztrált neve. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-create"></a>sfctl-szolgáltatás létrehozása
Hozza létre a megadott Service Fabric-szolgáltatást.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| [kötelező] – az app-id | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", lesz az identitása "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| – [kötelező] | A szolgáltatás neve. Ez legyen az alkalmazásazonosítót gyermeke. Ez az a teljes nevet, többek között a `fabric\:` URI-t. Például szolgáltatás `fabric\:/A/B` alkalmazás gyermeke `fabric\:/A`. |
| --service-type [kötelező] | A szolgáltatás típusának neve. |
| --aktiválási-mód | Az aktiválás módja a csomagban. |
| – korlátozások | Az elhelyezési korlátozások karakterláncként. Elhelyezési korlátozások a csomópont-tulajdonságok logikai kifejezésen, és lehetővé teszik a szolgáltatás adott csomópontok szolgáltatás követelményei alapján való korlátozása. Helyezhető el egy csomóponton, ahol NodeType csomóponttípus kék szolgáltatás adja meg például a következő\:"NodeColor kék ==". |
| --korrelált szolgáltatás | A célként megadott szolgáltatás korrelációját, ha a neve. |
| --korelace | Vesse össze az igazítási kapcsolatot használó meglévő service szolgáltatást. |
| --dns-name | A létrehozandó szolgáltatás DNS-nevét. A Service Fabric DNS rendszer szolgáltatás ezzel a beállítással kell engedélyezni. |
| --példányszám | A példányok száma. Ez csak állapotmentes szolgáltatások esetében érvényes. |
| – int-séma | Azt jelzi, hogy a szolgáltatás egységesen kell particionálni előjel nélküli egész számoknak számos. |
| – int-séma-száma | Az egész kulcstartományhoz szeretne létrehozni, ha egy egységes egész partícióséma használatával belül partíciók száma. |
| – int-séma – nagy | A kulcs egész szám tartományának, ha egy egységes egész partícióséma használatával végén. |
| – int-séma – alacsony | A kulcs egész szám tartományának, ha egy egységes egész partícióséma használatával kezdete. |
| --terhelés-metrikák | JSON kódolású használatos, ha a csomópont közötti terheléselosztás services metrikáinak listája látható. |
| --min-replica-set-size | A replikakészlet minimális méretének beállítása egy számot. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| --move-cost | Adja meg a szolgáltatás a move költsége. Lehetséges értékek a következők\: "Nulla", "Alacsony", "Közepes", "Maximum". |
| --nevű séma | Azt jelzi, hogy a szolgáltatás több elnevezett partíció kell rendelkeznie. |
| --nevű-séma-list | JSON-kódolású Névlista történő particionálása a szolgáltatás között, ha az elnevezett partícióséma használatával. |
| --no megőrzött állapot | Igaz értéke esetén jelzi a szolgáltatás nincs állandó állapot, a helyi lemezen tárolt rendelkezik, vagy csak állapot tárolja a memóriában. |
| --elhelyezési-házirend-list | JSON-kódolású elhelyezési házirendek a szolgáltatáshoz, és esetleg hozzá tartozó tartománynevek. Szabályzatok lehetnek egy vagy több\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --kvórum-adatveszteség-wait | A maximális időtartamot (másodpercben), a partíció, amelyhez a kvórum elvesztése a állapotban kell lennie engedélyezett. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| --replika-újraindítás-wait | Az időtartam másodpercben, ha egy replika leáll, és a egy új replika létrehozásakor között. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| – méretezés házirendek | JSON kódolású méretezés a szolgáltatásokra vonatkozó szabályzatok listája. |
| --singleton-séma | Azt jelzi, hogy a szolgáltatás kell egyetlen partícióval rendelkezik, vagy egy nem particionált szolgáltatás. |
| --bővíteni a replika megőrzése | A maximális időtartamot (másodpercben), mely készenlétben replikák maradnak eltávolításuk előtt. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| --állapotalapú | Azt jelzi, hogy a szolgáltatás az állapotalapú szolgáltatás. |
| – állapot | Azt jelzi, hogy a szolgáltatás az állapotmentes szolgáltatás. |
| --target-replica-set-size | A cél replika méretének beállítása egy számot. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-delete"></a>sfctl-szolgáltatás törlése
Törli a már meglévő Service Fabric-szolgáltatást.

A szolgáltatás azt törlése előtt kell létrehozni. Alapértelmezés szerint a Service Fabric megpróbálja zárja be a replikák szolgáltatás normális módon, és ezután törölje a szolgáltatást. Azonban a szolgáltatás nem tudta szabályosan bezárása a replikát, ha a törlési művelet hosszú időt vehet igénybe vagy elakad. Használja az opcionális ForceRemove jelzőt a normális Bezárás feladatütemezési kihagyásához és a kényszerített törölni a szolgáltatást.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. Ez az azonosító általában nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| --force-remove | Távolítsa el a Service Fabric-alkalmazás vagy szolgáltatás kellett zárnia a szabályos leállítást feladatütemezési áthaladás nélkül. Ez a paraméter használható kényszerítve törli egy alkalmazás vagy szolgáltatás, mely törlés van időtúllépése miatt problémák egy részét a szolgáltatás-kódot, amely megakadályozza a sikeres-e bezárása replikára. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-deployed-type"></a>sfctl-szolgáltatás üzembe helyezett típusa
Egy megadott szolgáltatás típusú Service Fabric-fürt egyik csomópontján üzembe helyezett alkalmazás adatainak beolvasása.

A Service Fabric-fürt egyik csomópontján üzembe helyezett alkalmazások egy adott szolgáltatás típusára vonatkozó információkat tartalmazó listájának beolvasása. A válasz tartalmazza a szolgáltatás típusa, a regisztrációs állapot, a regisztrált és az aktiválás kódcsomag nevére a csomag Csomagazonosítóját. Mindegyik bejegyzés jelöli egy aktiválási szolgáltatás típusú különbözteti meg a aktiválási azonosítója.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| --csomópontnév [kötelező] | A csomópont neve. |
| --service-típusnév [kötelező] | Adja meg a Service Fabric-szolgáltatás típusának neve. |
| --szolgáltatás jegyzékfájl neve | A szolgáltatásjegyzék üzembe helyezett szolgáltatás típusra vonatkozó adat a lista neve. Ha meg van adva, a válasz csak szolgáltatástípusok a szolgáltatásjegyzékben definiált információt tartalmaz. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-deployed-type-list"></a>sfctl szolgáltatás üzembe helyezett-type-list
A Service Fabric-fürt egyik csomópontján üzembe helyezett alkalmazások szolgáltatástípusok információt tartalmazó lista beolvasása.

A Service Fabric-fürt egyik csomópontján üzembe helyezett alkalmazások szolgáltatástípusok információt tartalmazó lista beolvasása. A válasz tartalmazza a szolgáltatás típusa, a regisztrációs állapot, a regisztrált és az aktiválás kódcsomag nevére a csomag Csomagazonosítóját.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| --csomópontnév [kötelező] | A csomópont neve. |
| --szolgáltatás jegyzékfájl neve | A szolgáltatásjegyzék üzembe helyezett szolgáltatás típusra vonatkozó adat a lista neve. Ha meg van adva, a válasz csak szolgáltatástípusok a szolgáltatásjegyzékben definiált információt tartalmaz. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-description"></a>sfctl-szolgáltatás leírása
Beolvassa egy meglévő Service Fabric-szolgáltatás leírását.

Beolvassa egy meglévő Service Fabric-szolgáltatás leírását. Egy szolgáltatás a leírását is beszerezhetők előtt kell létrehozni.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. Ez az azonosító általában nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-get-container-logs"></a>sfctl szolgáltatás get-tároló-naplók
Lekéri a tároló naplóit tároló üzembe helyezett egy Service Fabric-csomóponton.

A tároló üzembe helyezett egy Service Fabric-csomóponton, az adott kódcsomaghoz tároló naplóinak beolvasása.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| – [kötelező] code-package-neve | A szolgáltatásjegyzék-alkalmazástípus a Service Fabric-fürt részeként regisztrált megadott kód csomag neve. |
| --csomópontnév [kötelező] | A csomópont neve. |
| --service-jegyzékfájl-name [kötelező] | A szolgáltatásjegyzék-alkalmazástípus a Service Fabric-fürt részeként regisztrált neve. |
| – előző | Megadja, hogy a tároló naplóinak beolvasása a kód csomag példányának kilépett/kézbesítetlen tárolókból. |
| --tail | A naplók végéről sorok száma. Alapértelmezett érték 100. az "összes" a teljes körű naplók megjelenítése. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-health"></a>sfctl-szolgáltatás állapota
A megadott Service Fabric-szolgáltatás állapotának beolvasása.

Lekérdezi a megadott szolgáltatás üzemállapotával kapcsolatos adatokat. EventsHealthStateFilter gyűjteményét jelentett állapota alapján a service health-események szűréséhez használja. A gyűjtemény adott vissza partíciók szűrése PartitionsHealthStateFilter használja. Ha megad egy szolgáltatás, amely a a health Store adatbázisban nem létezik, a kérelem hibát ad vissza.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. Ez az azonosító általában nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| --események-állapot – állapot-szűrő | A gyűjtemény állapotesemény – a visszaadott objektumok állapota alapján szűrését teszi lehetővé. Ez a paraméter lehetséges értékei közé tartozik a következő állapotokat egy egész szám. Csak a szűrőnek megfelelő eseményeket adja vissza. Az összes esemény segítségével kiértékelése összesített állapotát. Ha nincs megadva, a rendszer minden bejegyzést adja vissza. Állapot értékei jelző-alapú számbavétel, így az érték lehet ezeket az értékeket, a bitenkénti "Vagy" operátor használatával beszerzett kombinációját. Például ha a megadott értéke 6 majd az OK (2), és figyelmeztetés (4) HealthState értékét az események vissza.  <br> -Alapértelmezett – alapértelmezett érték. Bármely HealthState illeszkedik. A tulajdonság értéke nulla.  <br> – Nincs – szűrő, amely nem felel meg a HealthState értéket. Annak érdekében, hogy ne adjon vissza eredményt egy adott gyűjteményen állapotok használja. Az érték az 1.  <br> -Ok - szűrheti, hogy egyezések bemeneti HealthState értékét az OK gombra. A 2 érték.  <br> -A figyelmeztetési - szűrő, hogy egyezések bemenet a HealthState értékét figyelmeztetés. Az érték a 4.  <br> -Hiba - szűrő, amely megfelel a bemeneti hiba HealthState értékkel. Értéke 8.  <br> -Az összes - szűrő, amely megfelel a bemeneti HealthState értéket. Az érték 65535. |
| --Állapotstatisztika kizárása | Azt jelzi-e az egészségügyi statisztikák a rendszer visszalépteti a lekérdezés eredménye részeként. Alapértelmezés szerint FALSE. A statisztika megjelenítése a gyermekek száma entitások állapota Ok, figyelmeztetés és hiba. |
| – partíciók-állapot – állapot-szűrő | Lehetővé teszi a partíciók egészségügyi állapot objektumok szűrése azok állapotát a service health lekérdezéshez eredményét adja vissza. Ez a paraméter lehetséges értékei közé tartozik a következő állapotokat egy egész szám. Csak a szűrőnek megfelelő partíciók adja vissza. Összesített állapota kiértékelheti, hogy az összes partíció szolgálnak. Ha nincs megadva, a rendszer minden bejegyzést adja vissza. Állapot értékei jelző-alapú számbavétel, így az érték lehet ezek értékére bitenkénti "Vagy" operátor használatával kombinációját. Például ha a megadott értéke 6 majd állapota OK (2), és figyelmeztetés (4) HealthState értékét a partíciók lesz visszaadva.  <br> -Alapértelmezett – alapértelmezett érték. Bármely HealthState illeszkedik. A tulajdonság értéke nulla.  <br> – Nincs – szűrő, amely nem felel meg a HealthState értéket. Annak érdekében, hogy ne adjon vissza eredményt egy adott gyűjteményen állapotok használja. Az érték az 1.  <br> -Ok - szűrheti, hogy egyezések bemeneti HealthState értékét az OK gombra. A 2 érték.  <br> -A figyelmeztetési - szűrő, hogy egyezések bemenet a HealthState értékét figyelmeztetés. Az érték a 4.  <br> -Hiba - szűrő, amely megfelel a bemeneti hiba HealthState értékkel. Értéke 8.  <br> -Az összes - szűrő, amely megfelel a bemeneti HealthState értéket. Az érték 65535. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-info"></a>sfctl-szolgáltatás adatai
A Service Fabric-alkalmazáshoz tartozó szolgáltatás adatainak beolvasása.

A megadott szolgáltatás a megadott Service Fabric-alkalmazáshoz tartozó információt ad vissza.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. Ez az azonosító általában nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-list"></a>sfctl-szolgáltatás listája
Az alkalmazás által meghatározott az alkalmazáshoz tartozó összes szolgáltatás adatainak beolvasása

Az alkalmazás által meghatározott az alkalmazáshoz tartozó összes szolgáltatás információt ad vissza

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| ---folytatási kód | A folytatási token paraméter eredmények következő készletét beszerzésére használatos. Az eredményeket a rendszer nem férnek el egyetlen válasz egy folytatási tokent egy nem üres értékkel szerepel az API-válasz. Ha ez az érték átadott, a következő API-hívás az API-t az eredmények tovább készletet ad vissza. Ha nincsenek további eredmények, a folytatási token neobsahuje értéket. Ez a paraméter értéke nem lehet URL-kódolású. |
| --service-type-name | A használatával szűrhetők a szolgáltatások lekérdezni a szolgáltatás neve. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-manifest"></a>sfctl-szolgáltatásjegyzékben
A jegyzékfájl szolgáltatástípus leíró beolvasása.

A jegyzékfájl szolgáltatástípus leíró beolvasása. A válasz tartalmazza a szolgáltatásjegyzék XML karakterláncként.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-típus-neve | Az alkalmazástípus neve. |
| – [kötelező] alkalmazás-típus-verziója | Az alkalmazástípus verziója. |
| --service-jegyzékfájl-name [kötelező] | A szolgáltatásjegyzék-alkalmazástípus a Service Fabric-fürt részeként regisztrált neve. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-package-deploy"></a>sfctl-szolgáltatás csomag – üzembe helyezés
A lemezkép-gyorsítótárból a megadott csomópont a megadott Szolgáltatásjegyzék társított csomagokat, letölti.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| [kötelező] – alkalmazás-típus-neve | A megfelelő kért Szolgáltatásjegyzék az alkalmazásjegyzékben neve. |
| [kötelező] – alkalmazás-típus-verzió | A megfelelő kért szolgáltatásjegyzékben az alkalmazásjegyzék verziója. |
| --csomópontnév [kötelező] | A csomópont neve. |
| --service-jegyzékfájl-name [kötelező] | A letöltött csomagok társított adatszolgáltatási jegyzékfájl neve. |
| – megosztás-policy | JSON kódolású megosztási házirendek listáját. Minden egyes megosztási házirend elemre a "name" és "hatókör" tevődik össze. A név felel meg a kódot, a konfiguráció vagy csomag, amely két felhőtípus neve. A hatókör is lehet: "None", "All", "Code", "Config" vagy "Adatok". |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-package-health"></a>sfctl szolgáltatási csomag-health
Lekérdezi az állapotfigyelő szolgáltatás információ üzembe helyezett egy Service Fabric-csomópont és az alkalmazás egy adott alkalmazáshoz.

Lekérdezi az állapotfigyelő szolgáltatás információ a Service Fabric csomópont telepítve egy adott alkalmazáshoz. EventsHealthStateFilter segítségével igény szerint szűrhet az üzembe helyezett service-csomag állapota alapján jelentett állapotesemény – objektumok gyűjteményét.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| --csomópontnév [kötelező] | A csomópont neve. |
| --service-csomag-name [kötelező] | A csomag neve. |
| --események-állapot – állapot-szűrő | A gyűjtemény állapotesemény – a visszaadott objektumok állapota alapján szűrését teszi lehetővé. Ez a paraméter lehetséges értékei közé tartozik a következő állapotokat egy egész szám. Csak a szűrőnek megfelelő eseményeket adja vissza. Az összes esemény segítségével kiértékelése összesített állapotát. Ha nincs megadva, a rendszer minden bejegyzést adja vissza. Állapot értékei jelző-alapú számbavétel, így az érték lehet ezeket az értékeket, a bitenkénti "Vagy" operátor használatával beszerzett kombinációját. Például ha a megadott értéke 6 majd az OK (2), és figyelmeztetés (4) HealthState értékét az események vissza.  <br> -Alapértelmezett – alapértelmezett érték. Bármely HealthState illeszkedik. A tulajdonság értéke nulla.  <br> – Nincs – szűrő, amely nem felel meg a HealthState értéket. Annak érdekében, hogy ne adjon vissza eredményt egy adott gyűjteményen állapotok használja. Az érték az 1.  <br> -Ok - szűrheti, hogy egyezések bemeneti HealthState értékét az OK gombra. A 2 érték.  <br> -A figyelmeztetési - szűrő, hogy egyezések bemenet a HealthState értékét figyelmeztetés. Az érték a 4.  <br> -Hiba - szűrő, amely megfelel a bemeneti hiba HealthState értékkel. Értéke 8.  <br> -Az összes - szűrő, amely megfelel a bemeneti HealthState értéket. Az érték 65535. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-package-info"></a>sfctl szolgáltatási csomag – információ
Beolvassa a megadott névnek megfelelő Service Fabric csomópont telepítve service-csomagok listáját.

A service-csomagok üzembe helyezett egy Service Fabric-csomóponton, az adott alkalmazás adatait adja vissza. Ezekkel az eredményekkel szolgáltatás csomagot, amelynek a neve egyezik pontosan a szolgáltatás nevét megadva paraméterként.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| --csomópontnév [kötelező] | A csomópont neve. |
| --service-csomag-name [kötelező] | A csomag neve. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-package-list"></a>sfctl-szolgáltatás-csomaglista
A Service Fabric csomópont telepítve szolgáltatáscsomagok listájának beolvasása.

A service-csomagok üzembe helyezett egy Service Fabric-csomóponton, az adott alkalmazás adatait adja vissza.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-azonosító | Az alkalmazás identitását. Ez általában a nélkül az alkalmazás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha az alkalmazás neve "fabric\:/myapp/app1", az identitása lenne "myapp\~app1" 6.0 + és "myapp/app1" korábbi verzióiban. |
| --csomópontnév [kötelező] | A csomópont neve. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-recover"></a>sfctl-szolgáltatás helyreállítása
Azt jelzi, hogy a Service Fabric-fürthöz, hogy kell próbálnia helyreállítani a megadott szolgáltatás, amely jelenleg kvórumveszteségben elakadt.

Azt jelzi, hogy a Service Fabric-fürthöz, hogy kell próbálnia helyreállítani a megadott szolgáltatás, amely jelenleg kvórumveszteségben elakadt. Ez a művelet csak akkor hajtható végre, ha ismert, hogy a replikákat, le nem állítható helyre. Ez az API nem megfelelő használata a lehetséges adatvesztést okozhat.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. Ez az azonosító általában nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-report-health"></a>sfctl szolgáltatás állapotjelentés
A Service Fabric service health jelentést küld.

A jelentés a megadott Service Fabric-szolgáltatás állapotát. A jelentésnek tartalmaznia kell a forrás az egészségügyi jelentés és, amelyen jelentett tulajdonság vonatkozó információk. A jelentést küld egy Service Fabric gateway szolgáltatás, amely továbbítja a health Store adatbázisban. A jelentés előfordulhat, hogy fogadja el az átjárót, azonban elutasította a health Store adatbázisban a további ellenőrzést. A health Store adatbázisban például elutasíthatják a jelentés érvénytelen paraméter, például egy elavult sorozatszám miatt. Látható-e a jelentés alkalmazta-e a a health Store adatbázisban, ellenőrizze, hogy a jelentés megjelenik a szolgáltatás hálózatállapot-események.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – rendszerállapot-tulajdonság [kötelező] | A tulajdonság az állapotadatokat. <br><br> Egy entitás különböző tulajdonságaihoz állapotjelentések rendelkezhet. Egy karakterláncot és a nem rögzített enumerálása, hogy az Eszközállapot-feltételt, amely elindítja a jelentés kategorizálása riporter rugalmasan tulajdonság. Például egy riporter a SourceId "LocalWatchdog" figyelheti az állapotot, a rendelkezésre álló lemez egy csomóponton, ezen a csomóponton, jelentést "AvailableDisk" tulajdonság. A ugyanolyan jelentéskészítői figyelheti a csomópont-kapcsolatban –, jelentést ugyanazon a csomóponton "Kapcsolat" tulajdonságot. A health Store adatbázisban ezek a jelentések az adott csomópont számára külön állapotesemények kell kezelni. A SourceId együtt a tulajdonság egyedileg azonosítja az egészségügyi adatokat. |
| --állapota [kötelező] | Lehetséges értékek a következők\: "Érvénytelen", "Ok", "Figyelmeztetés", "Error", "Ismeretlen". |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. <br><br> Ez általában a nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| – [kötelező] adatforrás-azonosítója | Az adatforrás neve azonosítja az ügyfél, a figyelő vagy a system összetevője, amely az egészségügyi információk jönnek létre. |
| – Leírás | Az egészségügyi információk leírását. <br><br> Azt jelöli, szabad szöveges adja hozzá a jelentés az emberi olvasható információk segítségével. A leírás karakterlánc maximális hossza 4096 karakternél. Ha a megadott karakterlánc hosszabb, akkor automatikusan csonkolva lesz. Csonkolva, amikor az utolsó karakter, a leírás tartalmaz egy "[Truncated]" jelölő, és teljes karaktersorozat 4096 karakternél. Jelenlétét, a jelölő azt jelzi, hogy a felhasználók számára, hogy a csonkolási történt. Vegye figyelembe, hogy csonkolva, a leírásnak legalább 4096 karakternél, az eredeti karakterláncot. |
| – azonnali | Azt a jelzőt, amely azt jelzi, hogy a jelentés azonnal kell küldeni. <br><br> Egy jelentés küld egy Service Fabric gateway alkalmazás, amely továbbítja a health Store adatbázisban. Ha az Immediate értékre van állítva. igaz, a jelentés azonnal címről érkezik a health Store adatbázisban, függetlenül a fabric-ügyfélbeállításokat a http-átjáró alkalmazások által használt HTTP-átjáró. Ez akkor hasznos, a kritikus fontosságú jelentések, amelyek a lehető leghamarabb kell küldeni. Attól függően, ütemezését és egyéb feltételek a jelentés elküldése továbbra is sikertelen lehet, például ha a HTTP-átjáró le van zárva, vagy az üzenet az átjáró nem érhető el. Ha Immediate hamis értékre van állítva, a jelentés alapján lesz elküldve az egészségügyi ügyfélbeállításokat a HTTP-átjáró. Ezért azt fogja kötegelni HealthReportSendInterval konfigurációjának megfelelően. Ez az az ajánlott beállítás, mivel így az egészségügyi ügyfél állapotfigyelő jelentési üzenetek a health Store adatbázisban, valamint az egészségügyi jelentés feldolgozása optimalizálása érdekében. Alapértelmezés szerint a rendszer a jelentések nem küldése azonnal. |
| --eltávolítása akkor, ha lejárt | Érték, amely azt jelzi, hogy a jelentés törlődik a health store adatbázisból, a lejárat után. <br><br> Ha igaz értékű, a jelentés távolítja el a health Store adatbázisban után jár le. Ha az értéke HAMIS, a jelentés egy hibát, ha lejárt számít. Ez a tulajdonság értéke alapértelmezés szerint False (hamis). Amikor az ügyfelek rendszeresen készít jelentést, akkor állítsa be RemoveWhenExpired false (alapértelmezett). Ezzel a módszerrel a riporter veti fel (pl. holtpont), és nem készíthető jelentés, az entitás értékeli ki a hiba, ha lejár az állapotjelentés. Az entitás hibaállapot állapottal megőrzendő tartalomként jelöli. |
| --sorszám | A numerikus karakterláncként egészségügyi jelentés sorszáma. <br><br> A jelentés sorszám használják a health Store adatbázisban elavult jelentések észleléséhez. Ha nincs megadva, egy megfelelő sorszám health-ügyfél által automatikusan létrehozott jelentések hozzáadásakor. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |
| – élettartam | Az időtartam, amelynek a jelentés érvénytelen. Ez a mező ISO8601 formátumot használ a időtartam megadása. <br><br> Amikor az ügyfelek rendszeresen készít jelentést, élettartam-nál nagyobb gyakorisággal kell küldenek jelentéseket. Az ügyfelek jelentés az átmenet, ha azok time to live végtelen, állíthatja be. Élettartam lejár, az egészségügyi adatokat tartalmazó állapotesemény esetén vagy eltávolítja a health Store adatbázisban, ha RemoveWhenExpired igaz értékre, vagy értékelhető a hiba, ha RemoveWhenExpired hamis. Ha nem a végtelen érték az alapértelmezett élettartam adott, ideje. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-resolve"></a>sfctl-szolgáltatás feloldása
Oldja meg a Service Fabric-partícióhoz.

Oldja meg a Service Fabric service partíció beolvasása a végpontok a szolgáltatás replikákról.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. Ez az azonosító általában nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| --key-partíciótípusnak | A partíciós kulcs típusa. A paraméter megadása kötelező, ha a szolgáltatás partícióséma Int64Range vagy elnevezett. A lehetséges értékek követi. – None (1) – azt jelzi, hogy a PartitionKeyValue paraméter nincs megadva. Ez a particionálási séma egypéldányosnak a partíciók esetén érvényes. Ez az alapértelmezett érték. Az érték az 1. -Int64Range (2) – azt jelzi, hogy a PartitionKeyValue paraméter egy int64 partíciókulcsot. Ez a particionálási séma Int64Range a partíciók esetén érvényes. A 2 érték. -Nevű (3) – azt jelzi, hogy a PartitionKeyValue paraméter a partíció nevét. Ez az a particionálási séma elnevezett partíciók esetén érvényes. Az érték 3. |
| --partíció-kulcs-érték | Partíciókulcs. Ez azért szükséges, ha a szolgáltatás partícióséma Int64Range vagy elnevezett. |
| --previous-rsp-version | A korábban kapott válasz a verzió mező értéke. Ez azért szükséges, ha a felhasználó tudja, hogy az eredmény, hogy közben lett korábban már elavult. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-type-list"></a>sfctl típusa – lista
A Service Fabric-fürtben üzembe helyezett alkalmazás típus által támogatott szolgáltatástípusok információkat tartalmazó lista beolvasása.

A Service Fabric-fürtben üzembe helyezett alkalmazás típus által támogatott szolgáltatástípusok információkat tartalmazó lista beolvasása. A megadott alkalmazástípus már léteznie kell. Ellenkező esetben a 404-es állapotot adott vissza.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] alkalmazás-típus-neve | Az alkalmazástípus neve. |
| – [kötelező] alkalmazás-típus-verziója | Az alkalmazástípus verziója. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |

## <a name="sfctl-service-update"></a>sfctl-szolgáltatás frissítése
Frissíti a megadott szolgáltatás használata az adott frissítés leírása.

### <a name="arguments"></a>Argumentumok

|Argumentum|Leírás|
| --- | --- |
| – [kötelező] szolgáltatás azonosítója | A szolgáltatás identitásának. Ez általában a nélkül a szolgáltatás teljes nevét a "fabric\:" URI-séma. 6.0-s verzió kezdve hierarchikus nevek vannak tagolva, az a "\~" karaktert. Például, ha a szolgáltatás neve "fabric\:/myapp/app1/svc1", a felügyeltszolgáltatás-identitás lenne "myapp\~app1\~svc1" 6.0 + és "myapp/app1/svc1" korábbi verzióiban. |
| – korlátozások | Az elhelyezési korlátozások karakterláncként. Elhelyezési korlátozások a csomópont-tulajdonságok logikai kifejezésen, és lehetővé teszik a szolgáltatás adott csomópontok szolgáltatás követelményei alapján való korlátozása. Helyezhető el egy csomóponton, ahol NodeType csomóponttípus kék szolgáltatás adja meg például a következő\: "NodeColor kék ==". |
| --korrelált szolgáltatás | A célként megadott szolgáltatás korrelációját, ha a neve. |
| --korelace | Vesse össze az igazítási kapcsolatot használó meglévő service szolgáltatást. |
| --példányszám | A példányok száma. Ez csak állapotmentes szolgáltatások esetében érvényes. |
| --terhelés-metrikák | JSON-kódolású mérőszámok listája használni terheléselosztás csomópontok között. |
| --min-replica-set-size | A replikakészlet minimális méretének beállítása egy számot. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| --move-cost | Adja meg a szolgáltatás a move költsége. Lehetséges értékek a következők\: "Nulla", "Alacsony", "Közepes", "Maximum". |
| --elhelyezési-házirend-list | JSON-kódolású elhelyezési házirendek a szolgáltatáshoz, és esetleg hozzá tartozó tartománynevek. Szabályzatok lehetnek egy vagy több\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| --kvórum-adatveszteség-wait | A maximális időtartamot (másodpercben), a partíció, amelyhez a kvórum elvesztése a állapotban kell lennie engedélyezett. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| --replika-újraindítás-wait | Az időtartam másodpercben, ha egy replika leáll, és a egy új replika létrehozásakor között. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| – méretezés házirendek | JSON kódolású méretezés a szolgáltatásokra vonatkozó szabályzatok listája. |
| --bővíteni a replika megőrzése | A maximális időtartamot (másodpercben), mely készenlétben replikák maradnak eltávolításuk előtt. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| --állapotalapú | Azt jelzi, hogy a célként megadott szolgáltatás egy olyan állapotalapú szolgáltatás. |
| – állapot | Azt jelzi, hogy a célként megadott szolgáltatás egy olyan állapotmentes szolgáltatás. |
| --target-replica-set-size | A cél replika méretének beállítása egy számot. Ez csak az állapotalapú szolgáltatások vonatkozik. |
| --időkorlát -t | Kiszolgálói időtúllépés másodpercben.  Alapértelmezett\: 60. |

### <a name="global-arguments"></a>Globális argumentumok

|Argumentum|Leírás|
| --- | --- |
| --debug | Növelése a naplózás az összes hibakeresési naplók megjelenítése. |
| --help -h | A súgóüzenetet és kilépési jelennek meg. |
| --kimeneti -o | Kimeneti formátum.  Megengedett értékek\: JSON-t, jsonc, tábla, tsv.  Alapértelmezett\: json. |
| – lekérdezés | JMESPath lekérdezési karakterláncot. Tekintse meg a http\://jmespath.org/ további információt és példákat. |
| – részletes | Növelése a naplózást. Használja a--debug teljes hibakeresési naplók. |


## <a name="next-steps"></a>További lépések
- [Állítsa be a](service-fabric-cli.md) a Service Fabric parancssori felület.
- Ismerje meg, hogyan használható a Service Fabric parancssori felület használatával a [-szkript minták](/azure/service-fabric/scripts/sfctl-upgrade-application).

---
title: Az Azure felügyeleti csoportok-erőforrások rendszerezéséhez |} Microsoft Docs
description: További tudnivalók a felügyeleti csoportok és a használatukat.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2018
ms.author: rithorn
ms.openlocfilehash: 31e71f153c7bbf76b0f06f8f17a74c43cc1b1c81
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/16/2018
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Az Azure felügyeleti csoportok-erőforrások rendszerezése 

Ha a szervezetben sok a lekérdezés, szükség lehet egy úgy, hogy hatékonyan kezelheti a hozzáférést, a házirendek és a megfelelőségi előfizetésekben. Az Azure felügyeleti csoportok fent előfizetések hatókör biztosít. A "felügyeleti csoport" nevű tárolók előfizetések rendszerezése és a cégirányítási feltételek vonatkoznak a felügyeleti csoportok. A felügyeleti csoporton belül minden előfizetés automatikusan öröklik a feltételeket, a felügyeleti csoportra alkalmazza. Felügyeleti csoportjai vállalati szintű felügyeleti függetlenül attól, milyen típusú előfizetések lehetséges, hogy nagy léptékben.

A felügyeleti csoport funkciót egy nyilvános előzetes verziójában érhető el. Indíthatja a felügyeleti csoportok, jelentkezzen be a [Azure-portálon](https://portal.azure.com) keresse meg a **felügyeleti csoportok** a a **minden szolgáltatás** szakasz. 

Tegyük fel a házirendeket is alkalmazhat a felügyeleti csoport, amely korlátozza a területek a virtuális gép (VM) létrehozásához. Ezzel a házirend csak a virtuális gépek az adott régióban létrehozni lehetővé minden felügyeleti csoportok, előfizetések és erőforrások adott felügyeleti csoportba tartozó volna alkalmazható.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>A felügyeleti csoportok és előfizetések hierarchia 

Felügyeleti csoport és az előfizetések a egyesített házirend- és hozzáférés-kezelés hierarchiába az erőforrások rendszerezéséhez rugalmas szerkezete hozhat létre. Az alábbi ábrán látható egy példa hierarchiában, amely a felügyeleti csoport és szervezeti egységek szerint vannak rendszerezve előfizetések áll.    

![fa](media/management-groups/MG_overview.png)

Hozzon létre egy hierarchiát, amely a szervezeti egységek szerint vannak csoportosítva, le is tudja hozzárendelése [átruházásához hozzáférés-vezérlés (RBAC)](../role-based-access-control/overview.md) szerepkörök, amelyek *öröklése* az adott felügyeleti csoportba tartozó szervezeti. Felügyeleti csoportok segítségével csökkentheti a terhelést, és csökkenti a hiba csak egyszer rendelhető hozzá a szerepkört ehhez. 

### <a name="important-facts-about-management-groups"></a>Fontos alapvető tudnivalók a felügyeleti csoportok
- 10 000 felügyeleti csoportok támogatja a egyetlen könyvtárban található. 
- A felügyeleti csoport fa mélysége legfeljebb hat szintet is támogat.
    - Ezt a határt nem tartalmazza a legfelső szintű vagy az előfizetés szintjén.
- Minden felügyeleti csoport csak egy szülőhöz támogat.
- Minden felügyeleti csoportban több gyermeke lehet. 

### <a name="preview-subscription-visibility-limitation"></a>Előzetes előfizetés látható korlátozása 
Jelenleg az előzetes kiadásban, ha még nem tudja megtekinteni az előfizetések, amelyek elérésére örökölt vannak belül korlátozása. A hozzáférés öröklik az előfizetéshez, de az Azure erőforrás-kezelő nem tudta tiszteletben még az öröklési hozzáférést.  

Az előfizetés információkat beolvasni a REST API használatával adatait adja vissza, rendelkezik hozzáféréssel, de az Azure portál és az Azure Powershell belül az előfizetések többé ne jelenjen meg. 

Ez az elem van végzett, és megszűnik, mielőtt a felügyeleti csoportok is megjelenik az összefoglalójuk mint "Általánosan rendelkezésre álló."  

### <a name="cloud-solution-providercsp-limitation-during-preview"></a>A felhőalapú megoldás Provider(CSP) korlátozás előzetes 
Nincs a jelenlegi korlátozás felhőalapú megoldás Provider(CSP) partnerek ahol nem hozhat létre, és az ügyfél címtáron belül az ügyfél felügyeleti csoportok kezelése.  
Ez az elem van végzett, és megszűnik, mielőtt a felügyeleti csoportok is megjelenik az összefoglalójuk mint "Általánosan rendelkezésre álló."


## <a name="root-management-group-for-each-directory"></a>Gyökérszintű felügyeleti csoport minden könyvtár

Minden könyvtár kap a "Gyökér" felügyeleti csoport egyetlen legfelső szintű felügyeleti csoportjában. A gyökérszintű felügyeleti csoport összeállítása a hierarchiába, hogy az összes felügyeleti csoportot, és előfizetések részekre bontani hozzá. A gyökérszintű felügyeleti csoport lehetővé teszi a globális házirendek és az RBAC-hozzárendelések a könyvtár szintjén kell alkalmazni. A [Directory rendszergazdának kell jogosultságszint-emelés maguk](../role-based-access-control/elevate-access-global-admin.md) kell kezdetben a legfelső szintű csoport tulajdonosa. Ha a rendszergazda a csoport tulajdonosa, azok rendelhet RBAC szerepköröket egyéb directory – felhasználók és csoportok kezelése a hierarchiában.  

### <a name="important-facts-about-the-root-management-group"></a>A legfelső szintű felügyeleti csoportra vonatkozó fontos tények
- A gyökérszintű felügyeleti csoport nevét és Azonosítóját alapértelmezés szerint az Azure Active Directory-Azonosítót kapnak. A megjelenített név az Azure-portálon belül különböző megjelenítendő bármikor lehet frissíteni. 
- Az előfizetések és a felügyeleti csoportok is részekre bontani a egy legfelső szintű felügyeleti csoporthoz a címtáron belül.  
    - Javasoljuk, hogy rendelkezik a directory modellrészek, akár globális Management a gyökérszintű felügyeleti csoport összes elemet.  
    - A nyilvános előzetes összes előfizetést a címtáron belül nem automatikusan történik a legfelső szintű gyermek.   
    - A nyilvános előzetes új előfizetések vannak nem automatikusan az alapértelmezett a legfelső szintű felügyeleti csoporthoz. 
- A gyökérszintű felügyeleti csoport helyezhető át vagy törölték, szemben a más felügyeleti csoportok. 
  
## <a name="management-group-access"></a>Felügyeleti csoport hozzáférése

Támogatja az Azure felügyeleti csoportok [átruházásához hozzáférés-vezérlés (RBAC)](../role-based-access-control/overview.md) az összes erőforrás hozzáférések és szerepkör-definíciók. Ezeket az engedélyeket a hierarchiában található gyermek erőforrásokhoz örökölt.   

Miközben bármely [beépített RBAC szerepkör](../role-based-access-control/overview.md#built-in-roles) lehet hozzárendelni felügyeleti csoporthoz, általánosan használt négy szerepkör is létezik: 
- **Tulajdonos** minden erőforrást, beleértve a jogot, hogy mások számára delegálása teljes hozzáféréssel rendelkezik. 
- **A közreműködői** is létrehozása és kezelése az Azure-erőforrások minden típusú, de nem tud hozzáférést biztosítani, mások számára.
- **Erőforrás házirend közreműködői** hozhat létre és kezelhet házirendeket az erőforrások könyvtárában.     
- **Olvasó** megtekintheti a meglévő Azure-erőforrások. 


## <a name="next-steps"></a>További lépések 
Felügyeleti csoportok kapcsolatos további információkért lásd: 
- [Azure-erőforrások rendszerezéséhez felügyeleti csoportok létrehozása](management-groups-create.md)
- [Módosítása, törlése és a felügyeleti csoportok kezelése](management-groups-manage.md)
- [Az Azure PowerShell modul telepítése](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Tekintse át a REST API-specifikáció](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Az Azure CLI-bővítményének telepítése](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)


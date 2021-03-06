---
title: Erőforrások rendszerezése az Azure Management Groups segítségével
description: Megismerheti a felügyeleti csoportokat és azok használatának módját.
author: rthorn17
manager: rithorn
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/18/2018
ms.author: rithorn
ms.openlocfilehash: d031059f9811cedb703fec4920e00fd1b2e3f877
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47045349"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Erőforrások rendszerezése az Azure Management Groups segítségével

Ha a vállalatnak sok előfizetése van, jól jöhet egy módszer, hogy hatékonyan kezelje az előfizetésekhez való hozzáférést, a szabályzatokat és a megfelelőséget. Az Azure Management Groups előfizetések fölötti hatókörszintet biztosít. Az előfizetéseket „felügyeleti csoportok” nevű tárolókba rendezheti, és az irányítási feltételeket alkalmazhatja a felügyeleti csoportokra. A felügyeleti csoporton belüli összes előfizetés automatikusan örökli a felügyeleti csoportra alkalmazott feltételeket. A felügyeleti csoportok nagy léptékű, nagyvállalati szintű felügyeletet tesznek lehetővé, függetlenül az előfizetése típusától.

Alkalmazhat például olyan szabályzatokat egy felügyeleti csoportra, amelyek korlátozzák a virtuális gépek (VM-ek) létrehozásához elérhető régiókat. A szabályzat minden felügyeleti csoportra, előfizetésre és erőforrásra érvényes lesz a felügyeleti csoporton belül, ha csak az adott régióban engedélyezi virtuális gépek létrehozását.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>A felügyeleti csoportok és előfizetések hierarchiája

A felügyeleti csoportok és előfizetések rugalmas szerkezetének létrehozásával hierarchiába rendezheti erőforrásait az egységes szabályzat- és hozzáféréskezeléshez. Az alábbi ábrán egy példa látható szabályozási hierarchia létrehozására felügyeleti csoportok használatával.

![fa](./media/MG_overview.png)

Ha ehhez a példához hasonló hierarchiát hoz létre, alkalmazhat egy szabályzatot, például a virtuális gépek helyét az USA nyugati régiójára korlátozó szabályzatot az „Infrastructure Team management group” csoportra a belső megfelelőségi és biztonsági szabályzatok elősegítése érdekében. Ezt a szabályzatot a felügyeleti csoport alá tartozó mindkét EA-előfizetés örökli, és az előfizetések alá tartozó összes virtuális gépre érvényes lesz. Mivel ez a biztonsági szabályzat a felügyeleti csoporttól öröklődik az előfizetésekre, az erőforrás vagy az előfizetés tulajdonosa nem módosíthatja, ami hatékonyabb kontrollt biztosít.

A felügyeleti csoportok használatának másik esete, amikor egyszerre több előfizetés számára szeretne felhasználói hozzáférést biztosítani. Ha több előfizetést helyez a felügyeleti csoport alá, mindössze egy [szerepköralapú hozzáférés-vezérlési](../../role-based-access-control/overview.md) (RBAC) hozzárendelést kell létrehoznia a felügyeleti csoporthoz, amelytől az összes előfizetés örökli a hozzáférést.
Ahelyett, hogy több előfizetésre szkriptelne RBAC-hozzárendeléseket, a felügyeleti csoporton egyetlen hozzárendeléssel biztosíthatja a szükséges hozzáférést a felhasználóknak.

### <a name="important-facts-about-management-groups"></a>A felügyeleti csoportok fontosabb jellemzői

- A címtárak legfeljebb 10 000 felügyeleti csoportot támogatnak.
- A felügyeleticsoport-fák legfeljebb hatszintűek lehetnek.
  - A korlátozásba nem tartozik bele a gyökérszint és az előfizetés szintje.
- Felügyeleti csoportonként vagy előfizetésenként egy szülő támogatott.
- Az egyes felügyeleti csoportok alá több gyermek tartozhat.
- Az egyes címtárakban minden előfizetés és felügyeleti csoport egyetlen hierarchiában található. Az előzetes verzióra vonatkozó kivételekről [A gyökérszintű felügyeleti csoport fontosabb jellemzői](#important-facts-about-the-root-management-group) című szakaszban olvashat.

## <a name="root-management-group-for-each-directory"></a>Az egyes címtárak gyökérszintű felügyeleti csoportja

Minden címtárhoz tartozik egy legfelső szintű, vagy más néven gyökérszintű felügyeleti csoport.
Ez a gyökérszintű felügyeleti csoport úgy épül be a hierarchiába, hogy minden felügyeleti csoport és előfizetés fölött legyen. A gyökérszintű felügyeleti csoport lehetővé teszi globális szabályzatok és RBAC-hozzárendelések címtárszintű alkalmazását. A [címtár rendszergazdájának először emelnie kell a jogosultsági szintjét](../../role-based-access-control/elevate-access-global-admin.md), hogy a gyökérszintű csoport tulajdonosa legyen. A csoport tulajdonosaként azután bármilyen RBAC-szerepkört hozzárendelhet a címtár felhasználóihoz vagy csoportjaihoz a hierarchia kezelése érdekében.

### <a name="important-facts-about-the-root-management-group"></a>A gyökérszintű felügyeleti csoport fontosabb jellemzői

- A gyökérszintű felügyeleti csoport neve és azonosítója alapértelmezés szerint meg van adva. A megjelenített név bármikor módosítható az Azure Portalon.
  - A név „Bérlői gyökércsoport” lesz.
  - Az azonosító az Azure Active Directory-azonosító lesz.
- A gyökérszintű felügyeleti csoportot a többi felügyeleti csoporttal szemben nem lehet törölni vagy áthelyezni.  
- A címtár összes előfizetése és felügyeleti csoportja a gyökérszintű felügyeleti csoport alá kerül.
  - A globális felügyelet érdekében a címtár erőforrásai is a gyökérszintű felügyeleti csoport alá kerülnek.
  - Az újonnan létrehozott előfizetések alapértelmezés szerint a gyökérszintű felügyeleti csoporthoz tartoznak.
- A gyökérszintű felügyeleti csoport az összes Azure-ügyfél számára látható, de nem mindegyikük rendelkezik hozzáféréssel a kezeléséhez.
  - Bárki, aki hozzáféréssel rendelkezik egy adott előfizetéshez, láthatja, hogy az hol helyezkedik el a hierarchiában.  
  - Senki nem kap alapértelmezés szerint hozzáférést a gyökérszintű felügyeleti csoporthoz. Kizárólag a globális címtárrendszergazdák emelhetik meg jogosultsági szintjüket, hogy hozzáférést kapjanak.  Ezt követően bármilyen RBAC-szerepkört hozzárendelhetnek a címtár felhasználóihoz.  

> [!NOTE]
> Ha Ön 2018. 06. 25. előtt kezdte meg címtárban a felügyeleti csoportok használatát, előfordulhat, hogy nem minden előfizetés lett beállítva a címtár hierarchiájában. Felügyeleti csoportokkal foglalkozó csapatunk 2018 júliusa és augusztusa között visszamenőlegesen frissíti azokat a címtárakat, amelyek a fenti időpontot megelőzően kezdtek felügyeleti csoportokat alkalmazni a nyilvános előzetes verzióban. Ennek keretében a címtárakban található összes előfizetés gyermekként lesz beállítva a korábbi gyökérszintű felügyeleti csoport alatt.
>
> Ha kérdése van a visszamenőleges folyamatot illetően, lépjen kapcsolatba velünk a következő e-mail-címen: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>A felügyeleti csoportok kezdeti beállítása

A felügyeleti csoportok használatának megkezdésekor először egy beállítási folyamat történik. A folyamat első lépéseként létrejön a gyökérszintű felügyeleti csoport a címtárban. A csoport létrehozása után a címtárban található összes meglévő előfizetés a gyökérszintű felügyeleti csoport gyermekeként lesz beállítva. A folyamat célja, hogy egy adott címtáron belül csak egy felügyeleticsoport-hierarchia legyen. Az egyetlen hierarchia beállítása lehetővé teszi a rendszergazdai ügyfelek számára globális hozzáférések és szabályzatok alkalmazását, amelyeket a címtárat használó többi ügyfél nem tud megkerülni. Minden gyökérszintű hozzárendelés érvényes lesz az összes felügyeleti csoportra, előfizetésre, erőforráscsoportra és erőforrásra a címtárban az egyetlen hierarchiának köszönhetően.

> [!IMPORTANT]
> A gyökérszintű felügyeleti csoporton végrehajtott felhasználóihozzáférés- és szabályzat-hozzárendelések **a címtárban lévő valamennyi erőforrásra érvényesek lesznek**.
> Ezért minden ügyfélnek fel kell mérnie, mire van szüksége ebben a hatókörben.
> A felhasználói hozzáférések és a szabályzatok hozzárendelései csak ebben a hatókörben lehetnek kötelezőek.  
  
## <a name="management-group-access"></a>Hozzáférés a felügyeleti csoportokhoz

Az Azure felügyeleti csoportjai támogatják az [Azure szerepkör-alapú hozzáférés-vezérlést (RBAC)](../../role-based-access-control/overview.md) minden erőforrás-hozzáféréshez és szerepkör-definícióhoz.
Ezeket az engedélyeket a hierarchiában található összes gyermek erőforrás örökli. Bármilyen beépített RBAC-szerepkör hozzárendelhető a felügyeleti csoporthoz, amely továbbörökíti azt a hierarchiában lejjebb található erőforrásoknak is.
A virtuálisgép-közreműködői RBAC-szerepkör például hozzárendelhető a felügyeleti csoporthoz. Ez a szerepkör nem végez műveletet a felügyeleti csoporton, de a csoport alá tartozó összes virtuális gép örökli.

Az alábbi ábrán a felügyeleti csoportokkal kapcsolatos szerepkörök és támogatott műveletek listája látható.

| RBAC-szerepkör neve             | Létrehozás | Átnevezés | Áthelyezés | Törlés | Hozzáférés hozzárendelése | Szabályzat hozzárendelése | Olvasás  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Tulajdonos                       | X      | X      | X    | X      | X             | X             | X     |
|Közreműködő                 | X      | X      | X    | X      |               |               | X     |
|Felügyeleti csoport közreműködője*             | X      | X      | X    | X      |               |               | X     |
|Olvasó                      |        |        |      |        |               |               | X     |
|Felügyeleti csoport olvasója*                  |        |        |      |        |               |               | X     |
|Erőforrás-szabályzat közreműködője |        |        |      |        |               | X             |       |
|Felhasználói hozzáférés rendszergazdája   |        |        |      |        | X             |               |       |

*: a Felügyeleti csoport közreműködője és a Felügyeleti csoport olvasója szerepkör kizárólag a felügyeleti csoport hatókörén belül engedélyezi az adott művelet végrehajtását a felhasználók számára.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Egyéni RBAC-szerepkördefiníció és -hozzárendelés

Az egyéni RBAC-szerepkörök jelenleg nem támogatottak a felügyeleti csoportok esetében. A funkció aktuális állapotáról a [felügyeleti csoportok visszajelzési fórumán](https://aka.ms/mgfeedback) tájékozódhat.

## <a name="next-steps"></a>További lépések

A felügyeleti csoportokkal kapcsolatos további tudnivalókért lásd:

- [Felügyeleti csoportok létrehozása az Azure-erőforrások rendszerezéséhez](create.md)
- [Felügyeleti csoportok módosítása, törlése és kezelése](manage.md)
- [Az Azure PowerShell-modul telepítése](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [A REST API-specifikáció áttekintése](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Az Azure CLI-bővítmény telepítése](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
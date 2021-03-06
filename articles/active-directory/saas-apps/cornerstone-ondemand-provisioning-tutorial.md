---
title: 'Oktatóanyag: Felhasználók automatikus átadása az Azure Active Directoryval alapkövei OnDemand konfigurálása |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az Azure Active Directoryban történő automatikus kiépítésének és megszüntetésének alapkövei OnDemand felhasználói fiókokat.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.openlocfilehash: 6a6cfb2cb1fd6b70be0437c8b6fa62f50e76e53b
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345413"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Oktatóanyag: Felhasználók automatikus átadása alapkövei OnDemand konfigurálása


Ez az oktatóanyag célja a lépéseket kell végrehajtania a alapkövei OnDemand és Azure Active Directory (Azure AD) konfigurálása az Azure AD automatikus kiépítésének és megszüntetésének felhasználók és csoportok a alapkövei OnDemand bemutatása.


> [!NOTE]
> Ez az oktatóanyag az Azure AD-felhasználó Provisioning Service-ra épülő összekötők ismerteti. Ez a szolgáltatás leírása, hogyan működik és gyakran ismételt kérdések a fontos tudnivalókat tartalmaz [automatizálhatja a felhasználókiépítés és -átadás megszüntetése SaaS-alkalmazásokban az Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Előfeltételek

Az ebben az oktatóanyagban ismertetett forgatókönyv feltételezi, hogy már rendelkezik a következő előfeltételek vonatkoznak:

*   Az Azure AD-bérlő
*   Egy alapkövei OnDemand-bérlő
*   Alapkövei OnDemand rendszergazdai engedélyekkel rendelkező felhasználói fiók


> [!NOTE]
> Az Azure AD létesítési integrációs támaszkodik a [alapkövei OnDemand webszolgáltatás](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), amely alapkövei OnDemand csapatok rendelkezésére áll.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Alapkövei OnDemand hozzáadása a katalógusból
Mielőtt konfigurálná a alapkövei OnDemand a felhasználók automatikus átadása az Azure ad-vel, szüksége az Azure AD alkalmazáskatalógusában alapkövei OnDemand hozzáadása a felügyelt SaaS-alkalmazások listája.

**Az Azure AD alkalmazáskatalógusában alapkövei OnDemand hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a **[az Azure portal](https://portal.azure.com)**, a bal oldali navigációs panelen, kattintson a a **Azure Active Directory** ikonra. 

    ![Az Azure Active Directory gomb][1]

2. Navigáljon a **vállalati alkalmazások** > **minden alkalmazás**.

    ![A vállalati alkalmazások szakasz][2]
    
3. Alapkövei OnDemand hozzáadásához kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Az új alkalmazás gomb][3]

4. A Keresés mezőbe írja be a **alapkövei OnDemand**.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/AppSearch.png)

5. Az eredmények panelen válassza ki a **alapkövei OnDemand**, majd kattintson a **Hozzáadás** gombra kattintva adhat hozzá OnDemand alapja az SaaS-alkalmazások listájához.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/AppSearchResults.png)

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Felhasználók hozzárendelése alapkövei OnDemand

Az Azure Active Directory "-hozzárendelések" nevű fogalma használatával határozza meg, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés. Felhasználók automatikus átadása kontextusában csak a felhasználók, illetve "rendelt" egy alkalmazás az Azure AD-csoportok szinkronizálódnak. 

Felhasználók automatikus kiépítés engedélyezése és konfigurálása, mielőtt, meg kell határoznia, melyik felhasználók, illetve a csoportok az Azure ad-ben alapkövei OnDemand hozzáférésre van szükségük. Ha úgy döntött, rendelhet a felhasználók és csoportok alapkövei OnDemand utasításokat követve:

*   [Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Felhasználók hozzárendelése alapkövei OnDemand fontos tippek

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve alapkövei OnDemand a felhasználók automatikus konfiguráció teszteléséhez. További felhasználók és csoportok később is rendelhető.

*   Amikor egy felhasználó hozzárendelése alapkövei OnDemand, a hozzárendelés párbeszédpanelen válassza ki bármely érvényes alkalmazás-specifikus szerepkört (ha elérhető). A felhasználók a **alapértelmezett hozzáférési** szerepkör nem tartoznak kiépítése.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Alapkövei OnDemand történő automatikus felhasználókiépítés konfigurálása

Ez a szakasz végigvezeti az Azure AD létesítési szolgáltatás létrehozása, frissítése és tiltsa le a felhasználók és csoportok az Azure AD-ben a felhasználó és/vagy a csoport-hozzárendelések alapján alapkövei OnDemand konfigurálásáról.


### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Konfigurálhatja a felhasználók automatikus átadása alapkövei OnDemand az Azure AD-ben:


1. Jelentkezzen be a [az Azure portal](https://portal.azure.com) és keresse meg a **Azure Active Directory > Vállalati alkalmazások > minden alkalmazás**.

2. Válassza ki a OnDemand alapja az SaaS-alkalmazások listájában.
 
    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/Successcenter2.png)

3. Válassza ki a **kiépítési** fülre.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Állítsa be a **Kiépítési mód** való **automatikus**.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Alatt a **rendszergazdai hitelesítő adataival** szakaszban adjon meg a **rendszergazdai felhasználónév**, **rendszergazdai jelszó**, és **tartomány** , a alapkövei OnDemand fiók.

    *   Az a **rendszergazdai felhasználónév** mezőbe a rendszergazdai fiók az alapkövei OnDemand-bérlőre az tartomány\felhasználónév feltöltéséhez. Példa: contoso\admin.

    *   Az a **rendszergazdai jelszó** mezőbe a jelszót a rendszergazdai felhasználónév megfelelő feltöltéséhez.

    *   Az a **tartomány** mezőbe a webszolgáltatás URL-címe a alapkövei OnDemand bérlő feltöltéséhez. Példa: A szolgáltatás nem található: `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`, a Contoso tartomány `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. A webszolgáltatás URL-címe lekérésével további információkért lásd: [Itt](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. 5. lépésben megjelenő mezők feltöltése, után kattintson a **kapcsolat tesztelése** annak biztosítása érdekében az Azure AD alapkövei OnDemand csatlakozhat. Ha a kapcsolat hibája esetén, győződjön meg arról, alapkövei OnDemand fiókja rendszergazdai engedélyekkel rendelkező, és próbálkozzon újra.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. Az a **értesítő e-mailt** mezőbe írja be az e-mail-címét egy személyt vagy csoportot, akik kell üzembe helyezési hiba értesítéseket fogadni, és jelölje be a jelölőnégyzetet - **e-mail-értesítés küldése, ha hiba történik**.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Kattintson a **Save** (Mentés) gombra.

9. Alatt a **leképezések** szakaszban jelölje be **szinkronizálása az Azure Active Directory-felhasználók a alapkövei OnDemand**.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Tekintse át a alapkövei OnDemand a az Azure AD-ből szinkronizált felhasználói attribútumok a **attribútumleképzés** szakaszban. A kiválasztott attribútumok **megfelelést kiváltó** tulajdonságok segítségével felel meg a frissítési műveletek alapkövei OnDemand levő felhasználói fiókokat. Válassza ki a **mentése** gombra kattintva véglegesítse a módosításokat.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Hatókörszűrő konfigurálásához tekintse meg a következő utasításokat a [Scoping szűrő oktatóanyag](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Az Azure AD létesítési szolgáltatás alapkövei OnDemand engedélyezéséhez módosítsa a **üzembe helyezési állapotra** való **a** a a **beállítások** szakaszban.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. A felhasználók és/vagy a kívánt csoportok definiálása alapkövei OnDemand való kiépítéséhez válassza ki a kívánt értékeket a **hatókör** a a **beállítások** szakaszban.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Ha készen áll rendelkezésre, kattintson a **mentése**.

    ![Alapkövei OnDemand kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)


Ez a művelet elindítja a kezdeti szinkronizálás, az összes olyan felhasználó és/vagy meghatározott csoportoknak **hatókör** a a **beállítások** szakaszban. A kezdeti szinkronizálás végrehajtásához, mint az ezt követő szinkronizálások, amely körülbelül 40 percenként történik, amennyiben az Azure AD létesítési szolgáltatás fut-e több időt vesz igénybe. Használhatja a **szinkronizálás részleteivel** szakasz előrehaladásának figyeléséhez, és kövesse a hivatkozásokat kiépítés tevékenységgel kapcsolatos jelentés, amely az Azure AD létesítési szolgáltatás a alapkövei OnDemand által végrehajtott összes műveletet ismerteti.

Az Azure AD létesítési naplók olvasása további információkért lásd: [-jelentések automatikus felhasználói fiók kiépítése](../manage-apps/check-status-user-account-provisioning.md).
## <a name="connector-limitations"></a>Összekötő-korlátozások

* A alapkövei OnDemand **pozíció** attribútum, amely megfelel a szerepköröket a alapkövei OnDemand portál értéket vár. Érvényes listájának **pozíció** értékek szerezhető be az **felhasználói rekord szerkesztése > szervezeti felépítés > pozíció** a alapkövei OnDemand-portálon.
    ![Felhasználó alapkövei OnDemand kiépítés szerkesztése](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png) ![pozíció kiépítés alapkövei OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png) ![alapkövei OnDemand pozíciók lista kiépítése](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)
    
## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése a vállalati alkalmazások kezelése](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)



## <a name="next-steps"></a>További lépések

* [Tekintse át a naplók és jelentések készítése a tevékenység kiépítése](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png

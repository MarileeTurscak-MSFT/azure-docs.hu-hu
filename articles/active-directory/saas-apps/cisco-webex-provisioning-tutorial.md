---
title: 'Oktatóanyag: A felhasználók automatikus átadása az Azure Active Directory konfigurálása a Cisco |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az Azure Active Directoryban történő automatikus kiépítésének és megszüntetésének Cisco Webex felhasználói fiókokat.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: v-wingf
ms.openlocfilehash: 8f5af3cba01e925591c9d90ea0e96ed78b2823e2
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348378"
---
# <a name="tutorial-configure-cisco-webex-for-automatic-user-provisioning"></a>Oktatóanyag: Felhasználók automatikus átadása Cisco Webex konfigurálása


Ez az oktatóanyag célja a lépéseket kell végrehajtania a Cisco Webex és Azure Active Directory (Azure AD) konfigurálása az Azure AD automatikus kiépítésének és megszüntetésének Cisco Webex felhasználók bemutatásához.


> [!NOTE]
> Ez az oktatóanyag az Azure AD-felhasználó Provisioning Service-ra épülő összekötők ismerteti. Ez a szolgáltatás leírása, hogyan működik és gyakran ismételt kérdések a fontos tudnivalókat tartalmaz [automatizálhatja a felhasználókiépítés és -átadás megszüntetése SaaS-alkalmazásokban az Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Előfeltételek

Az ebben az oktatóanyagban ismertetett forgatókönyv feltételezi, hogy már rendelkezik a következő előfeltételek vonatkoznak:

*   Az Azure AD-bérlő
*   Cisco Webex bérlő
*   A Cisco Webex rendszergazdai engedélyekkel rendelkező felhasználói fiókkal


> [!NOTE]
> Az Azure AD létesítési integrációs támaszkodik a [Cisco Webex webszolgáltatás](https://developer.webex.com/getting-started.html), amely Cisco Webex csapatok rendelkezésére áll.

## <a name="adding-cisco-webex-from-the-gallery"></a>Cisco Webex hozzáadása a katalógusból
Mielőtt konfigurálná a Cisco Webex a felhasználók automatikus átadása az Azure ad-vel, szüksége az Azure AD alkalmazáskatalógusában Cisco Webex hozzáadása a felügyelt SaaS-alkalmazások listája.

**Az Azure AD alkalmazáskatalógusában Cisco Webex hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a **[az Azure portal](https://portal.azure.com)**, a bal oldali navigációs panelen, kattintson a a **Azure Active Directory** ikonra.

    ![Az Azure Active Directory gomb][1]

2. Navigáljon a **vállalati alkalmazások** > **minden alkalmazás**.

    ![A vállalati alkalmazások szakasz][2]

3. Cisco Webex hozzáadásához kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Az új alkalmazás gomb][3]

4. A Keresés mezőbe írja be a **Cisco Webex**.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/AppSearch.png)

5. Az eredmények panelen válassza ki a **Cisco Webex**, majd kattintson a **Hozzáadás** gombra kattintva adja hozzá a Cisco Webex a SaaS-alkalmazások listájához.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/AppSearchResults.png)

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-webex"></a>Felhasználók hozzárendelése a Cisco Webex

Az Azure Active Directory "-hozzárendelések" nevű fogalma használatával határozza meg, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés. Felhasználók automatikus átadása kontextusában csak a felhasználók, illetve "rendelt" egy alkalmazás az Azure AD-csoportok szinkronizálódnak.

Konfigurálása, és engedélyezi a felhasználók automatikus átadása, mielőtt, meg kell határoznia, hogy mely felhasználók Azure AD-ben a Cisco Webex hozzáférésre van szükségük. Ha úgy döntött, az itt leírt utasításokat követve hozzárendelheti ezeket a felhasználókat Cisco Webex:

*   [Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-webex"></a>Felhasználók hozzárendelése a Cisco Webex fontos tippek

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve a Cisco Webex a felhasználók automatikus konfiguráció teszteléséhez. További felhasználók később is rendelhető.

*   Amikor egy felhasználó hozzárendelése Cisco Webex, jelöljön ki minden olyan érvényes alkalmazás-specifikus szerepkört (ha elérhető) a hozzárendelés párbeszédpanelen. A felhasználók a **alapértelmezett hozzáférési** szerepkör nem tartoznak kiépítése.

## <a name="configuring-automatic-user-provisioning-to-cisco-webex"></a>Cisco Webex történő automatikus felhasználókiépítés konfigurálása

Ez a szakasz végigvezeti az Azure AD létesítési szolgáltatás létrehozása, frissítése és letilthatja a felhasználókat az Azure AD-ben a felhasználó hozzárendelések alapján Cisco Webex konfigurálásáról.


### <a name="to-configure-automatic-user-provisioning-for-cisco-webex-in-azure-ad"></a>Konfigurálhatja a felhasználók automatikus átadása Cisco Webex az Azure AD-ben:


1. Jelentkezzen be a [az Azure portal](https://portal.azure.com) és keresse meg a **Azure Active Directory > Vállalati alkalmazások > minden alkalmazás**.

2. Válassza ki a Cisco Webex SaaS-alkalmazások listájából.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/Successcenter2.png)

3. Válassza ki a **kiépítési** fülre.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/ProvisioningTab.png)

4. Állítsa be a **Kiépítési mód** való **automatikus**.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/ProvisioningCredentials.png)

5. Alatt a **rendszergazdai hitelesítő adataival** szakaszban adjon meg a **bérlői URL-cím**, és **titkos jogkivonat** a Cisco Webex-fiók.

    *   Az a **bérlői URL-cím** mezőben töltse fel a Cisco Webex SCIM API URL-CÍMÉT a bérlő, amelyhez formájában történik `https://api.webex.com/v1/scim/[Tenant ID]/`, ahol `[Tenant ID]` alfanumerikus karakterből áll, 6. lépésben leírtak szerint.

    *   Az a **titkos jogkivonat** mezőben a titkos kulcs Token feltöltéséhez, 6. lépésben leírtak szerint.

1. A **Bérlőazonosító** és **titkos jogkivonat** a Cisco Webex a fiók található való bejelentkezéssel az [Cisco Webex fejlesztői webhely](https://developer.webex.com/) rendszergazdai fiókkal. Bejelentkezve egyszer -
    * Nyissa meg a [első lépések lap](https://developer.webex.com/getting-started.html)
    * Görgessen le a [hitelesítés szakaszban](https://developer.webex.com/getting-started.html#authentication)
    ![Cisco Webex hitelesítési Token](./media/cisco-webex-provisioning-tutorial/SecretToken.png)
    * A mezőbe a alfanumerikus karakterlánc a **titkos jogkivonat**. Ez a token másolása a vágólapra
    * Nyissa meg a [első saját saját részletei lap](https://developer.webex.com/endpoint-people-me-get.html)
        * Győződjön meg arról, hogy vizsgálati üzemmód be Kapcsolva
        * Írja be a "Tulajdonos" és egy szóközt szót, majd illessze be az engedélyezési mező a jogkivonat titkos kulcs ![Cisco Webex hitelesítési Token](./media/cisco-webex-provisioning-tutorial/GetMyDetails.png)
        * Kattintson a Futtatás
    * A jobb oldalon, a válasz szövegben a **Bérlőazonosító** "orgId" néven jelenik meg:

    ```json
    {
        "id": "(...)",
        "emails": [
            "admin.user@contoso.com"
        ],
        "displayName": "John Smith",
        "nickName": "John",
        "orgId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        (...)
    }
    ```

1. 5. lépésben megjelenő mezők feltöltése, után kattintson a **kapcsolat tesztelése** annak biztosítása érdekében az Azure AD Cisco Webex csatlakozhat. Ha a kapcsolat hibája esetén, győződjön meg arról, a Cisco Webex fiókja rendelkezik rendszergazdai engedélyekkel, és próbálkozzon újra.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/TestConnection.png)

8. Az a **értesítő e-mailt** mezőbe írja be az e-mail-címét egy személyt vagy csoportot, akik kell üzembe helyezési hiba értesítéseket fogadni, és jelölje be a jelölőnégyzetet - **e-mail-értesítés küldése, ha hiba történik**.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/EmailNotification.png)

9. Kattintson a **Save** (Mentés) gombra.

10. Alatt a **leképezések** szakaszban jelölje be **szinkronizálása az Azure Active Directory-felhasználók a Cisco Webex**.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/UserMapping.png)

11. Tekintse át a Cisco Webex a az Azure AD-ből szinkronizált felhasználói attribútumok a **attribútumleképzés** szakaszban. A kiválasztott attribútumok **megfelelést kiváltó** tulajdonságok segítségével felel meg a felhasználói fiókok, a Cisco Webex a frissítési műveleteket. Válassza ki a **mentése** gombra kattintva véglegesítse a módosításokat.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/UserMappingAttributes.png)

12. Hatókörszűrő konfigurálásához tekintse meg a következő utasításokat a [Scoping szűrő oktatóanyag](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Az Azure AD létesítési szolgáltatás a Cisco Webex engedélyezéséhez módosítsa a **üzembe helyezési állapotra** való **a** a a **beállítások** szakaszban.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/ProvisioningStatus.png)

14. Definiálása a felhasználók és/vagy a csoportok, adja meg a Cisco Webex kiépítéséhez válassza ki a kívánt értékeket a **hatókör** a a **beállítások** szakaszban.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/SyncScope.png)

15. Ha készen áll rendelkezésre, kattintson a **mentése**.

    ![Cisco Webex kiépítése](./media/cisco-webex-provisioning-tutorial/Save.png)


Ez a művelet elindítja a kezdeti szinkronizálás, az összes olyan felhasználó és/vagy meghatározott csoportoknak **hatókör** a a **beállítások** szakaszban. A kezdeti szinkronizálás végrehajtásához, mint az ezt követő szinkronizálások, amely körülbelül 40 percenként történik, amennyiben az Azure AD létesítési szolgáltatás fut-e több időt vesz igénybe. Használhatja a **szinkronizálás részleteivel** szakasz előrehaladásának figyeléséhez, és kövesse a hivatkozásokat kiépítés tevékenységgel kapcsolatos jelentés, amely az Azure AD létesítési szolgáltatás a Cisco Webex által végrehajtott összes műveletet ismerteti.

Az Azure AD létesítési naplók olvasása további információkért lásd: [-jelentések automatikus felhasználói fiók kiépítése](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Összekötő-korlátozások

* Cisco Webex jelenleg Cisco a korai mező tesztelése (elektronikus átutalás) fázisban van. További információkért lépjen kapcsolatba [Cisco a támogatási csapat](https://www.webex.co.in/support/support-overview.html). 

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése a vállalati alkalmazások kezelése](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>További lépések

* [Tekintse át a naplók és jelentések készítése a tevékenység kiépítése](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_03.png

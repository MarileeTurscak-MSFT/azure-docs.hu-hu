---
title: 'Oktatóanyag: Azure Active Directory-integráció az GoToMeeting |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és a GoToMeeting között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bcaf19f2-5809-4e1c-acbc-21a8d3498ccf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: jeedes
ms.openlocfilehash: b62b3b7f9f3bfd55237ed4d894954a0bde48e7fc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043720"
---
# <a name="tutorial-azure-active-directory-integration-with-gotomeeting"></a>Oktatóanyag: GoToMeeting-Azure Active Directory-integráció

Ebben az oktatóanyagban elsajátíthatja, hogyan GoToMeeting integrálása az Azure Active Directory (Azure AD).

GoToMeeting integrálása az Azure ad-ben nyújt a következő előnyökkel jár:

- Szabályozhatja, ki férhet hozzá GoToMeeting Azure AD-ben.
- Engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett GoToMeeting (egyszeri bejelentkezés), az Azure AD-fiókjukat.
- A fiókok egyetlen központi helyen – az Azure Portalon kezelheti.

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása a GoToMeeting, a következőkre van szükség:

- Az Azure AD-előfizetéshez
- Egy GoToMeeting egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. GoToMeeting hozzáadása a katalógusból
2. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-gotomeeting-from-the-gallery"></a>GoToMeeting hozzáadása a katalógusból
Konfigurálhatja az Azure AD integrálása a GoToMeeting, hozzá kell GoToMeeting a galériából a felügyelt SaaS-alkalmazások listájára.

**GoToMeeting hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a  **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Az Azure Active Directory gomb][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![A vállalati alkalmazások panelen][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Az új alkalmazás gomb][3]

4. A Keresés mezőbe írja be a **GoToMeeting**válassza **GoToMeeting** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az eredmények listájában GoToMeeting](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés GoToMeeting, egy "Britta Simon" nevű tesztelési felhasználó alapján.

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó GoToMeeting mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó GoToMeeting hivatkozás kapcsolata kell létrehozni.

GoToMeeting, rendelje hozzá az értékét a **felhasználónév** értékeként az Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezés GoToMeeting-tesztelése és konfigurálása, hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
2. **[Hozzon létre egy Azure ad-ben tesztfelhasználót](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
3. **[Hozzon létre egy GoToMeeting tesztfelhasználót](#create-a-gotomeeting-test-user)**  – egy megfelelője a Britta Simon GoToMeeting, amely a felhasználó Azure ad-ben ábrázolása van csatolva van.
4. **[Rendelje hozzá az Azure ad-ben tesztfelhasználó](#assign-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon és a GoToMeeting-alkalmazását az egyszeri bejelentkezés konfigurálása.

**Szeretné konfigurálni az Azure AD egyszeri bejelentkezés GoToMeeting, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **GoToMeeting** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezési hivatkozás konfigurálása][4]

2. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.
 
    ![Egyszeri bejelentkezési párbeszédpanel](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_samlbase.png)

3. Az a **GoToMeeting-tartomány és URL-címek** szakaszban, hajtsa végre az alábbi lépéseket:

    ![GoToMeeting-tartomány és URL-címeket egyetlen bejelentkezési adatait](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_url.png)

    Az a **azonosító** szövegmezőbe írja be az URL-cím: `https://authentication.logmeininc.com/saml/sp`

4. Kattintson a **speciális URL-cím megjelenítése konfigurációs** és konfigurálja az alábbi URL-címek

    **Bejelentkezési URL-cím** (üresen is hagyhatja ez)
    
    **Válasz URL-cím**: `https://authentication.logmeininc.com/saml/acs`
    
    **Paramétert**:
    
    - GoToMeeting-alkalmazás használata `https://global.gotomeeting.com`
    
    - Használja GoToTraining, `https://global.gototraining.com`
    
    - GoToWebinar használata `https://global.gotowebinar.com` 
    
    - GoToAssist használata `https://app.gotoassist.com`
    
5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gomb konfigurálása](./media/citrix-gotomeeting-tutorial/tutorial_general_400.png)

6. Egy másik böngészőablakban jelentkezzen be a [GoToMeeting szervezet Center](https://organization.logmeininc.com/). Meg kell adnia annak ellenőrzéséhez, hogy az identitásszolgáltató frissítve lett

7. Engedélyezze a "Saját identitásszolgáltató frissítve lett az új tartomány" jelölőnégyzetet. Kattintson a **kész** befejezésekor.


> [!TIP]
> Ezek az utasítások belül tömör verziója elolvashatja a [az Azure portal](https://portal.azure.com), míg a állítja be az alkalmazás!  Ez az alkalmazás hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentáció eléréséhez a  **Konfigurációs** alul található szakaszában. Tudjon meg többet a beágyazott dokumentáció szolgáltatásról ide: [Azure ad-ben embedded – dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure ad-ben tesztfelhasználó számára

Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

   ![Hozzon létre egy Azure ad-ben tesztfelhasználó számára][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon, a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.

    ![Az Azure Active Directory gomb](./media/citrix-gotomeeting-tutorial/create_aaduser_01.png)

2. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/citrix-gotomeeting-tutorial/create_aaduser_02.png)

3. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** felső részén a **minden felhasználó** párbeszédpanel bezárásához.

    ![A Hozzáadás gombra.](./media/citrix-gotomeeting-tutorial/create_aaduser_03.png)

4. Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![A felhasználó párbeszédpanel](./media/citrix-gotomeeting-tutorial/create_aaduser_04.png)

    a. Az a **neve** mezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** mezőbe írja be a felhasználó Britta Simon e-mail-címét.

    c. Válassza ki a **jelszó megjelenítése** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-gotomeeting-test-user"></a>GoToMeeting tesztfelhasználó létrehozása

Ebben a szakaszban egy Britta Simon nevű felhasználó GoToMeeting jön létre. GoToMeeting támogatja a just-in-time-kiépítés, amely alapértelmezés szerint engedélyezve van.

Nincs meg ebben a szakaszban a művelet elem. Ha a felhasználó még nem létezik a GoToMeeting, egy új jön létre, a hozzáférés GoToMeeting megkísérlésekor.

> [!NOTE]
> Ha manuálisan hozzon létre egy felhasználót, lépjen kapcsolatba kell [GoToMeeting támogatási csapatának](https://support.logmeininc.com/gotomeeting).

### <a name="assign-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés GoToMeeting Azure egyszeri bejelentkezés használatára.

![A felhasználói szerepkör hozzárendelése][200] 

**Britta Simon rendel GoToMeeting, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **GoToMeeting**.

    ![Az alkalmazások listáját a GoToMeeting hivatkozásra](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_app.png)  

3. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![A hozzárendelés hozzáadása panel][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Az egyszeri bejelentkezés vizsgálata

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha a hozzáférési panelen a GoToMeeting csempére kattint, meg kell beolvasása automatikusan bejelentkezett az GoToMeeting-alkalmazás.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)
* [Felhasználók átadásának konfigurálása](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrixgotomeeting-provisioning-tutorial)


<!--Image references-->

[1]: ./media/gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/gotomeeting-tutorial/tutorial_general_203.png


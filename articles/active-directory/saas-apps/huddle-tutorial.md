---
title: 'Oktatóanyag: Azure Active Directory-integráció az Huddle |} A Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés az Azure Active Directory és Huddle között.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: jeedes
ms.openlocfilehash: fc4ea2538ebe5876e8f3572ab8ad76c4b3b44b8c
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/15/2018
ms.locfileid: "45634336"
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Oktatóanyag: Azure Active Directory-integráció az Huddle

Ebben az oktatóanyagban elsajátíthatja, hogyan Huddle integrálása az Azure Active Directory (Azure AD).

Huddle integrálása az Azure ad-ben nyújt a következő előnyökkel jár:

- Szabályozhatja, hogy ki férhet hozzá Huddle Azure AD-ben
- Engedélyezheti a felhasználóknak, hogy automatikusan első bejelentkezett Huddle (egyszeri bejelentkezés) az Azure AD-fiókjukkal
- Kezelheti a fiókokat, egyetlen központi helyen – az Azure Portalon

Ha meg szeretné ismerni a SaaS-alkalmazás integráció az Azure ad-vel kapcsolatos további részletekért, lásd: [Mi az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Előfeltételek

Huddle az Azure AD-integráció konfigurálásához a következőkre van szükség:

- Azure AD-előfizetés
- Egy Huddle egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ebben az oktatóanyagban a lépéseket teszteléséhez nem ajánlott éles környezetben használja.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, csak szükség esetén.
- Ha nem rendelkezik egy Azure ad-ben a próbakörnyezet, beszerezheti a egy egy havi próbalehetőség [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása

Ebben az oktatóanyagban tesztelni az Azure AD egyszeri bejelentkezés egy tesztkörnyezetben. Az ebben az oktatóanyagban ismertetett forgatókönyvben két fő építőelemeket áll:

1. Huddle hozzáadása a katalógusból
2. Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

## <a name="adding-huddle-from-the-gallery"></a>Huddle hozzáadása a katalógusból

Az Azure ad-ben Huddle integráció konfigurálásához, hozzá kell Huddle a galériából a felügyelt SaaS-alkalmazások listájára.

**Huddle hozzáadása a katalógusból, hajtsa végre az alábbi lépéseket:**

1. Az a **[az Azure portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen a **minden alkalmazás**.

    ![Alkalmazások][2]

3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

4. A Keresés mezőbe írja be a **Huddle**.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/huddle-tutorial/tutorial_huddle_search.png)

5. Az eredmények panelen válassza ki a **Huddle**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/huddle-tutorial/tutorial_huddle_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurálás és tesztelés az Azure AD egyszeri bejelentkezés

Ebben a szakaszban konfigurálja, és a teszt "Britta Simon." nevű felhasználó Huddle az Azure AD egyszeri bejelentkezés tesztelése

Egyszeri bejelentkezés működjön, az Azure ad-ben tudnia kell, a partner felhasználó Huddle mi egy felhasználó számára az Azure ad-ben. Más szóval egy Azure AD-felhasználót és a kapcsolódó felhasználó Huddle hivatkozás kapcsolata kell létrehozni.

Az Azure AD egyszeri bejelentkezés az Huddle tesztelése és konfigurálása, hogy hajtsa végre a következő építőelemeit kell:

1. **[Az Azure AD egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – ahhoz, hogy ez a funkció használatát a felhasználók számára.
2. **[Az Azure ad-ben tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezés az Britta Simon teszteléséhez.
3. **[Huddle tesztfelhasználó létrehozása](#creating-a-huddle-test-user)**  – egy megfelelője a Britta Simon Huddle, amely a felhasználó Azure ad-ben ábrázolása van csatolva van.
4. **[Az Azure ad-ben tesztfelhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  – Britta Simon használata az Azure AD egyszeri bejelentkezés engedélyezéséhez.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri bejelentkezés az Azure Portalon, és Huddle alkalmazását az egyszeri bejelentkezés konfigurálása.

**Szeretné konfigurálni az Azure AD egyszeri bejelentkezés Huddle, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon az a **Huddle** alkalmazás integrációs oldalán kattintson a **egyszeri bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezési** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezéséhez.

    ![Egyszeri bejelentkezés konfigurálása](./media/huddle-tutorial/tutorial_huddle_samlbase.png)

3. Az a **Huddle tartomány és URL-címek** területén kövesse az alábbi lépéseket, ha az alkalmazás a konfigurálni kívánt **Identitásszolgáltató** kezdeményezett mód:

    > [!NOTE]
    > A huddle példány fogja automatikusan észlelni a tartományból, alatt adja meg.

    ![Huddle tartomány és URL-címeket egyetlen bejelentkezési adatait](./media/huddle-tutorial/tutorial_huddle_url.png)

    a. Az a **azonosító** szövegmezőbe írja be az URL-cím a következő minta használatával bármelyike:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.net`|
    | `https://my.huddle.net` |
    | |

    b. Az a **válasz URL-cím** szövegmezőbe írja be az URL-cím a következő minta használatával bármelyike:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.net/saml/idp-initiated-sso`|
    | `https://my.huddle.net/saml/idp-initiated-sso`|
    | |

4. Ellenőrizze **speciális URL-beállítások megjelenítése** , és hajtsa végre a következő lépést, ha az alkalmazás a konfigurálni kívánt **SP** kezdeményezett mód:

    ![Huddle tartomány és URL-címeket egyetlen bejelentkezési adatait](./media/huddle-tutorial/tutorial_huddle_url1.png)

    Az a **bejelentkezési URL-** szövegmezőbe írja be az URL-cím a következő minta használatával bármelyike:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.net`|
    | |

    > [!NOTE]
    > Ezek a értékei nem valódi. Frissítse a tényleges azonosítója, válasz URL-cím és a bejelentkezési URL-ezeket az értékeket. Kapcsolattartó [Huddle ügyfél-támogatási csapatának](https://huddle.zendesk.com) beolvasni ezeket az értékeket.

5. Az a **SAML-aláíró tanúsítvány** területén kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/huddle-tutorial/tutorial_huddle_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/huddle-tutorial/tutorial_general_400.png)

7. Az a **Huddle konfigurációs** területén kattintson **konfigurálása Huddle** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **SAML Entitásazonosító és SAML egyszeri bejelentkezési szolgáltatás URL-cím** származó a **gyors útmutató szakaszban.**

    ![Egyszeri bejelentkezés konfigurálása](./media/huddle-tutorial/tutorial_huddle_configure.png) 

8. Huddle oldalán konfigurálása egyszeri bejelentkezéshez, kell küldenie a letöltött **tanúsítvány**, **SAML egyszeri bejelentkezési szolgáltatás URL-cím**, és **SAML Entitásazonosító** való [ Ügyfél-támogatási csapatának huddle](https://huddle.zendesk.com). Akkor állítsa ezt a beállítást, hogy a SAML SSO-kapcsolat megfelelően állítsa be mindkét oldalon.  

    >[!NOTE]
    > Egyszeri bejelentkezés az Huddle ügyfélszolgálathoz engedélyeznie kell. A konfiguráció befejezése után értesítést kap.
    >

### <a name="creating-an-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó létrehozása

Ez a szakasz célja az Azure Portalon Britta Simon nevű hozzon létre egy tesztfelhasználót.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **az Azure portal**, a bal oldali navigációs panelén kattintson **Azure Active Directory** ikonra.

    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/huddle-tutorial/create_aaduser_01.png) 

2. A felhasználók listájának megjelenítéséhez, lépjen a **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/huddle-tutorial/create_aaduser_02.png) 

3. Megnyitásához a **felhasználói** párbeszédpanelen kattintson a **Hozzáadás** a párbeszédpanel tetején.
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/huddle-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure ad-ben tesztfelhasználó létrehozása](./media/huddle-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőbe írja be a **e-mail-cím** BrittaSimon az.

    c. Válassza ki **jelszó megjelenítése** és jegyezze fel az értékét a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-huddle-test-user"></a>Huddle tesztfelhasználó létrehozása

Ahhoz, hogy az Azure AD-felhasználók Huddle jelentkezzen be, akkor ki kell építeni Huddle be. Huddle, esetén kiépítése a manuális feladat.

**Felhasználók átadásának konfigurálása, hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be a **Huddle** rendszergazdaként a vállalati webhely.

2. Kattintson a **munkaterület**.

3. Kattintson a **személyek \> felkérése**.
   
    ![Személyek](./media/huddle-tutorial/IC787838.png "személyek")

4. Az a **hozzon létre egy új meghívó** szakaszban, hajtsa végre az alábbi lépéseket:
   
    ![Új meghívó](./media/huddle-tutorial/IC787839.png "új meghívó")
   
    a. Az a **válassza ki a csapatával egy felkérése csatlakozni** listáról válassza ki **csapat**.

    b. Típusa a **E-mail cím** egy érvényes Azure AD-fiókot kíván létrehozni a a **meghívni kívánt személyek e-mail címet adjon meg** szövegmezőbe.

    c. Kattintson a **meghívása**.   
   
    >[!NOTE]
    > Az Azure ad-ben fióktulajdonos kap egy e-mailt és a telepítőre mutató erősítse meg a fiókot, mielőtt aktívvá válik. 
    > 

>[!NOTE]
>Huddle felhasználói fiók létrehozása eszközöket és Huddle által biztosított API-k segítségével az Azure AD-felhasználói fiókok kiépítése. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure ad-ben tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Huddle Azure egyszeri bejelentkezés használatára.

![Felhasználó hozzárendelése][200] 

**Britta Simon rendel Huddle, hajtsa végre az alábbi lépéseket:**

1. Az Azure Portalon nyissa meg az alkalmazások megtekintése, és a könyvtár nézetben keresse meg és nyissa meg **vállalati alkalmazások** kattintson **minden alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listájában jelölje ki a **Huddle**.

    ![Egyszeri bejelentkezés konfigurálása](./media/huddle-tutorial/tutorial_huddle_app.png) 

3. A bal oldali menüben kattintson **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzárendelés hozzáadása** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **kiválasztása** gombot **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombot **hozzárendelés hozzáadása** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelni az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.

Ha a hozzáférési panelen a Huddle csempére kattint, szerezheti be automatikusan Huddle alkalmazás bejelentkezési oldalán.
A hozzáférési panelen kapcsolatos további információkért lásd: [Bevezetés a hozzáférési Panel használatába](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [SaaS-alkalmazások integrálása az Azure Active Directory foglalkozó oktatóanyagok listája](tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/huddle-tutorial/tutorial_general_01.png
[2]: ./media/huddle-tutorial/tutorial_general_02.png
[3]: ./media/huddle-tutorial/tutorial_general_03.png
[4]: ./media/huddle-tutorial/tutorial_general_04.png
[100]: ./media/huddle-tutorial/tutorial_general_100.png
[200]: ./media/huddle-tutorial/tutorial_general_200.png
[201]: ./media/huddle-tutorial/tutorial_general_201.png
[202]: ./media/huddle-tutorial/tutorial_general_202.png
[203]: ./media/huddle-tutorial/tutorial_general_203.png

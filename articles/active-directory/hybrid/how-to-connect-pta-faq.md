---
title: 'Az Azure AD Connect: Az átmenő hitelesítés – gyakori kérdések |} A Microsoft Docs'
description: Azure Active Directory átmenő hitelesítéssel kapcsolatos gyakori kérdésekre adott válaszok
services: active-directory
keywords: Az Azure AD Connect az átmenő hitelesítés, Active Directory telepítése szükséges összetevők SSO, Azure AD egyszeri bejelentkezés
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 611eabd377705af7758276a3d920f9cb4c38ac55
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46311128"
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Az Azure Active Directory átmenő hitelesítés: Gyakori kérdések

Ez a cikk foglalkozik az Azure Active Directory (Azure AD) átmenő hitelesítés kapcsolatos gyakori kérdésekre. Vissza a frissített tartalom megtartása ellenőrzése.

## <a name="which-of-the-methods-to-sign-in-to-azure-ad-pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs-should-i-choose"></a>Amely a módszerek bejelentkezni az Azure AD-ben az átmenő hitelesítés, a jelszó ujjlenyomat-szinkronizálás és az Active Directory összevonási szolgáltatások (AD FS) válasszam?

Felülvizsgálat [Ez az útmutató](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) összehasonlítása az Azure AD bejelentkezési módszerek és a szervezet bejelentkezési megfelelő módszer kiválasztásában.

## <a name="is-pass-through-authentication-a-free-feature"></a>Az átmenő hitelesítés egy ingyenes szolgáltatás?

Az átmenő hitelesítés egy olyan ingyenes szolgáltatás. Nem kell minden fizetős kiadásban az Azure AD használatát.

## <a name="is-pass-through-authentication-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Az átmenő hitelesítés érhető el a [Microsoft Azure Germany cloud](http://www.microsoft.de/cloud-deutschland) és a [Microsoft Azure Government felhőben](https://azure.microsoft.com/features/gov/)?

Nem. Az átmenő hitelesítés csak akkor használható az Azure AD világszerte elérhető példányával.

## <a name="does-conditional-accessactive-directory-conditional-access-azure-portalmd-work-with-pass-through-authentication"></a>Does [feltételes hozzáférési](../active-directory-conditional-access-azure-portal.md) működnek az átmenő hitelesítés?

Igen. Az összes feltételes hozzáférési képességeit, beleértve az Azure multi-factor Authentication az átmenő hitelesítés működnek.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Támogatja az átmenő hitelesítés "A másik azonosító", felhasználónév, "userPrincipalName" helyett?

Igen. Az átmenő hitelesítés támogatja `Alternate ID` a felhasználóneveként, ha az Azure AD Connectben konfigurálva. További információkért lásd: [az Azure AD Connect egyéni telepítési](how-to-connect-install-custom.md). Office 365-höz nem mindegyik alkalmazás támogatja `Alternate ID`. Tekintse meg az adott alkalmazás dokumentációja támogatási nyilatkozattal.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Jelszókivonat-szinkronizálást, átmenő hitelesítés egy tartalék szerepét?

Nem. Az átmenő hitelesítés _nem_ Jelszókivonat-szinkronizálás automatikusan feladatátvételt. Felhasználói bejelentkezési hibák elkerülése érdekében konfigurálnia kell az átmenő hitelesítés [magas rendelkezésre állású](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxymanage-appsapplication-proxymd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Telepíthetem egy [Azure AD-alkalmazásproxy](../manage-apps/application-proxy.md) -összekötő ugyanarra a kiszolgálóra, egy átmenő hitelesítési ügynök?

Igen. Az átmenő hitelesítési ügynök verziója 1.5.193.0 rebranded verzióiban vagy újabb támogatja ezt a konfigurációt.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Az Azure AD Connect és az átmenő hitelesítési ügynök mely verzióit van szüksége?

Ez a funkció működjön, meg kell 1.1.750.0 verzió vagy újabb Azure AD Connect és 1.5.193.0 vagy később a átmenő hitelesítési ügynök. Minden szoftver telepítése a kiszolgálókon Windows Server 2012 R2 vagy újabb.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication"></a>Mi történik, ha a felhasználó jelszava lejárt, és próbáljon meg bejelentkezni az átmenő hitelesítés használatával?

Ha konfigurálta a [jelszóvisszaíró](../authentication/concept-sspr-writeback.md) egy adott felhasználó, és ha a felhasználó bejelentkezik az átmenő hitelesítés használatával, módosíthatja vagy új jelszót kérjenek. A jelszavak rendszer visszaírja a helyszíni Active Directory elvárt módon.

Ha nem konfigurálta a jelszóvisszaírás egy adott felhasználó, vagy ha a felhasználó nem rendelkezik érvényes Azure AD-licenccel, a felhasználó nem tudja frissíteni a jelszavát, a felhőben. A jelszavát, azokat nem lehet frissíteni, akkor is, ha a jelszó lejárt. A felhasználó inkább ezt az üzenetet látja: "a szervezet nem engedélyezi, hogy frissítse a jelszavát, ezen a helyen. Frissítse a munkahelye által javasolt módszerrel, vagy kérje a rendszergazda, ha segítségre van szüksége." A felhasználó vagy a rendszergazda alaphelyzetbe kell állítania a jelszavát a helyszíni Active Directoryban.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Hogyan nem átmenő hitelesítés, találgatásos jelszó támadásokkal szembeni?

[Olvasható információk az intelligens zárolás](../authentication/howto-password-smart-lockout.md).

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Milyen tegye átmenő hitelesítési ügynökök kommunikálni 80-as és 443-as portokon keresztül?

- A hitelesítési ügynökök győződjön meg arról, HTTPS-kéréseket a művelet a szolgáltatás a 443-as porton keresztül.
- A hitelesítési ügynökök győződjön meg arról, HTTP-kéréseket az SSL tanúsítvány-visszavonási listákat (CRL) letöltése a 80-as porton keresztül.

     >[!NOTE]
     >Legutóbbi frissítések, amelyek a szolgáltatás használatához portok száma csökken. Ha az Azure AD Connect vagy a hitelesítési ügynök régebbi verzióját, megnyitva, ezeket a portokat is: 5671, 8080-as, 9090, 9091, 9350, 9352 és 10100-10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Az átmenő hitelesítés ügynökök kommunikálhatnak egy kimenő proxy webkiszolgálón keresztül?

Igen. Ha Proxy automatikus felderítési WPAD (Web) engedélyezve van a helyszíni környezetben, a hitelesítési ügynökök automatikusan megkísérli keresse meg és a egy proxy-webkiszolgálót használ a hálózaton.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Két vagy több átmenő hitelesítési ügynökök telepíthető ugyanarra a kiszolgálóra?

Egyetlen kiszolgáló nem, egy átmenő hitelesítési ügynök csak telepítheti. Ha az átmenő hitelesítés konfigurálása a magas rendelkezésre álláshoz szeretné [kövesse az itt](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability).

## <a name="how-do-i-remove-a-pass-through-authentication-agent"></a>Hogyan távolíthatom el egy átmenő hitelesítési ügynök?

Mindaddig, amíg egy átmenő hitelesítési ügynök fut, aktív marad, és folyamatosan a felhasználói bejelentkezési kérelmeket kezeli. Ha el szeretné távolítani a hitelesítési ügynök, lépjen a **Vezérlőpult -> Programok -> Programok és szolgáltatások** távolíthatja, és a **a Microsoft Azure AD Connect hitelesítési ügynökének** és a  **A Microsoft Azure AD Connect az ügynök frissítési** programok.

Ha az átmenő hitelesítés panelen ellenőrizheti a a [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) az előző lépés befejezése után megjelenik az azt jelzi, hogy a hitelesítési ügynök **inaktív**. Ez a _várt_. A hitelesítési ügynök automatikusan néhány nap elteltével törlődnek a listából.

## <a name="i-already-use-ad-fs-to-sign-in-to-azure-ad-how-do-i-switch-it-to-pass-through-authentication"></a>Már használom az AD FS bejelentkezni az Azure ad-hez. Hogyan lehet váltani, az átmenő hitelesítés?

Ha az Active Directory összevonási szolgáltatások (vagy más összevonási technológiákkal) átmenő hitelesítésre migrál, javasoljuk, hogy kövesse a részletes üzembe helyezési útmutató, közzétett [Itt](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx).

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-active-directory-environment"></a>Használható átmenő hitelesítés az Active Directory Többerdős környezetben?

Igen. Többerdős környezetben támogatottak, ha vannak az Active Directory-erdők között erdőszintű megbízhatóság, és ha névutótag megfelelően van konfigurálva.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Átmenő hitelesítés hány ügynök van szükségem telepítéséhez?

Több átmenő hitelesítési ügynökök telepítésével biztosítható [magas rendelkezésre állású](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability). Azonban nem biztosítják a hitelesítési ügynökök közötti terheléselosztás determinisztikus.

Fontolja meg a maximális és átlagos terhelés bejelentkezési kérések a bérlő látja a keresett. Alapként egy egyetlen hitelesítési ügynök képes kezelni másodpercenként egy standard 4 magos processzor, 16 GB RAM-MAL kiszolgáló 300, 400 hitelesítések.

Hálózati forgalom becslése, használja a következő olvasható méretezési Útmutató:
- Minden egyes kérés esetében a payload mappaméret (0,5 KB + 1 K * num_of_agents) bájt. azaz a adatok az Azure ad-ből a hitelesítési ügynök. Itt "num_of_agents" azt jelzi, hogy a bérlő regisztrált a hitelesítési ügynökök száma.
- Minden válasz van egy 1 KB; hasznos adat mérete azaz a származó adatok a hitelesítési ügynök az Azure ad-hez.

A legtöbb ügyfél számára két vagy három hitelesítési ügynökök összesen elegendőek a magas rendelkezésre állás és a kapacitás. Hitelesítési ügynökök közeli bejelentkezési késés javítása érdekében a tartományvezérlőket kell telepítenie.

>[!NOTE]
>12 hitelesítési ügynökök bérlőnként rendszer korlátozva van.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Telepíthetem az első átmenő hitelesítési ügynök egy másik Azure AD Connectet futtató kiszolgálóra?

Nem, ez a forgatókönyv még _nem_ támogatott.

## <a name="why-do-i-need-a-cloud-only-global-administrator-account-to-enable-pass-through-authentication"></a>Miért kell egy csak felhőalapú globális rendszergazdai fiókkal az átmenő hitelesítés engedélyezése?

Javasoljuk, hogy engedélyezi vagy letiltja az átmenő hitelesítés csak felhőalapú globális rendszergazdai fiók használatával. Ismerje meg [hozzáadása egy csak felhőalapú globális rendszergazdai fiókkal](../active-directory-users-create-azure-portal.md). Ennek során, így biztosítja, hogy ne zárja ki a bérlő.

## <a name="how-can-i-disable-pass-through-authentication"></a>Hogyan lehet átmenő hitelesítés letiltása?

Futtassa újra az Azure AD Connect varázsló, és módosítsa a felhasználói bejelentkezési metódus az átmenő hitelesítés más módszerrel. Ez a változás az átmenő hitelesítés letiltása, a bérlő, és a hitelesítési ügynök eltávolítása a kiszolgálóról. A többi kiszolgáló a hitelesítési ügynökök manuálisan el kell távolítania.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Mi történik, ha egy átmenő hitelesítési ügynökének eltávolítása?

Ha egy kiszolgálóról távolítja el egy átmenő hitelesítési ügynök, a kiszolgáló bejelentkezési kérelmek fogadását okoz. Ha el szeretné kerülni a felhasználói bejelentkezési funkció a bérlőn, gondoskodjon arról, hogy egy másik hitelesítési ügynök fut egy átmenő hitelesítési ügynökének eltávolítása előtt.

## <a name="next-steps"></a>További lépések
- [Aktuális korlátozások](how-to-connect-pta-current-limitations.md): ismerje meg, melyik forgatókönyvek is támogatottak, és melyek nem.
- [Gyors üzembe helyezési](how-to-connect-pta-quick-start.md): első lépésekhez az Azure AD átmenő hitelesítés.
- [Az AD FS át az átmenő hitelesítés](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) – egy részletes útmutató, amellyel áttelepíteni az átmenő hitelesítés az Active Directory összevonási szolgáltatások (vagy más összevonási technológiákkal).
- [Az intelligens zárolási](../authentication/howto-password-smart-lockout.md): ismerje meg, hogyan konfigurálhatja az intelligens zárolás funkciót a bérlő felhasználói fiókok védelmét.
- [Részletes technikai](how-to-connect-pta-how-it-works.md): az átmenő hitelesítési szolgáltatás működésének megismerése.
- [Hibaelhárítás](tshoot-connect-pass-through-authentication.md): ismerje meg az átmenő hitelesítés szolgáltatás szolgáltatással kapcsolatos gyakori problémák megoldásához.
- [A biztonság részletes bemutatása](how-to-connect-pta-security-deep-dive.md): ismerje meg az átmenő hitelesítés szolgáltatás technikai információit.
- [Az Azure AD közvetlen egyszeri bejelentkezés](how-to-connect-sso.md): További információ a kiegészítő funkció.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): az Azure Active Directory-fórumon használatával új funkcióra vonatkozó javaslata fájlt.


## <a name="playbook-steps"></a>Forgatókönyv lépései
1. [Bevezetés](../articles/active-directory/active-directory-playbook-intro.md)
2. [Összetevők](../articles/active-directory/active-directory-playbook-ingredients.md)
    * [Téma](../articles/active-directory/active-directory-playbook-ingredients.md)
    * [Környezet](../articles/active-directory/active-directory-playbook-ingredients.md#theme)
    * [Felhasználói célcsoport számára](../articles/active-directory/active-directory-playbook-ingredients.md#environment)
3. [Megvalósítása](../articles/active-directory/active-directory-playbook-implementation.md)
   * [Foundation - AD az Azure AD szinkronizálása](../articles/active-directory/active-directory-playbook-implementation.md#foundation---syncing-ad-to-azure-ad)
     * [Kiterjesztése a felhőre a helyszíni identitás](../articles/active-directory/active-directory-playbook-implementation.md#extending-your-on-premises-identity-to-the-cloud)  
     * [Csoportok használata az Azure AD-licencek felhasználókhoz rendelése](../articles/active-directory/active-directory-playbook-implementation.md#assigning-azure-ad-licenses-using-groups)
   * [Téma - alkalmazások számos, egyetlen identitás](../articles/active-directory/active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity)
     * [Integráció SaaS-alkalmazások – összevont egyszeri bejelentkezés](../articles/active-directory/active-directory-playbook-implementation.md#integrate-saas-applications---federated-sso)
     * [Egyszeri bejelentkezés és az identitás-Életciklusesemények](../articles/active-directory/active-directory-playbook-implementation.md#sso-and-identity-lifecycle-events)
     * [Integráció SaaS-alkalmazások – jelszavas egyszeri Bejelentkezést](../articles/active-directory/active-directory-playbook-implementation.md#integrate-saas-applications---password-sso)
     * [Biztonságos hozzáférés a megosztott fiókok](../articles/active-directory/active-directory-playbook-implementation.md#secure-access-to-shared-accounts)
     * [A helyszíni alkalmazások biztonságos távoli hozzáférést](../articles/active-directory/active-directory-playbook-implementation.md#secure-remote-access-to-on-premises-applications)
     * [LDAP-identitások az Azure AD szinkronizálása](../articles/active-directory/active-directory-playbook-implementation.md#synchronize-ldap-identities-to-azure-ad)
   * [Téma – a biztonság növelése](../articles/active-directory/active-directory-playbook-implementation.md#theme---increase-your-security)
     * [Biztonságos rendszergazdai fiók számára hozzáférést](../articles/active-directory/active-directory-playbook-implementation.md#secure-administrator-account-access)
     * [Alkalmazásokhoz való hozzáférés biztonsága](../articles/active-directory/active-directory-playbook-implementation.md#secure-access-to-applications)
     * [Engedélyezze az igény szerinti (JIT) felügyelet](../articles/active-directory/active-directory-playbook-implementation.md#enable-just-in-time-jit-administration)
     * [Kockázat alapján identitások védelme](../articles/active-directory/active-directory-playbook-implementation.md#protect-identities-based-on-risk)
     * [A jelszavak Tanúsítványalapú hitelesítés használata nélkül végezzen hitelesítést](../articles/active-directory/active-directory-playbook-implementation.md#authenticate-without-passwords-using-certificate-based-authentication)
   * [Téma - és önkiszolgáló](../articles/active-directory/active-directory-playbook-implementation.md#theme---scale-with-self-service)
     * [Önkiszolgáló jelszóváltoztatás](../articles/active-directory/active-directory-playbook-implementation.md#self-service-password-reset)
     * [Önkiszolgáló alkalmazás-hozzáférés](../articles/active-directory/active-directory-playbook-implementation.md#self-service-access-to-applications)
4. [Építőelemek](../articles/active-directory/active-directory-playbook-building-blocks.md)
   * [Az Aktorok katalógus](../articles/active-directory/active-directory-playbook-building-blocks.md)
   * [Az összes építőelemeket általános Előfeltételek](../articles/active-directory/active-directory-playbook-building-blocks.md#common-prerequisites-for-all-building-blocks)
   * [Címtár szinkronizálási - Jelszókivonat-szinkronizálás (nál) – új telepítés](../articles/active-directory/active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation)
   * [Védjegyzési](../articles/active-directory/active-directory-playbook-building-blocks.md#branding)
   * [Csoport alapú licencelés](../articles/active-directory/active-directory-playbook-building-blocks.md#group-based-licensing)
   * [SaaS-összevont egyszeri bejelentkezés konfigurálása](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-federated-sso-configuration)
   * [SaaS-jelszó egyszeri bejelentkezés konfigurálása](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-password-sso-configuration)
   * [SaaS a megosztott fiókok konfigurációja](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration)
   * [Alkalmazás-Proxy konfigurálása](../articles/active-directory/active-directory-playbook-building-blocks.md#app-proxy-configuration)
   * [Általános LDAP-összekötő konfigurálása](../articles/active-directory/active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration)
   * [Csoportok – delegált tulajdonjoga](../articles/active-directory/active-directory-playbook-building-blocks.md#groups---delegated-ownership)
   * [SaaS- és identitás-életciklus](../articles/active-directory/active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle)
   * [Önkiszolgáló alkalmazás-felügyelet elérésének](../articles/active-directory/active-directory-playbook-building-blocks.md#self-service-access-to-application-management)
   * [Önkiszolgáló jelszóváltoztatás](../articles/active-directory/active-directory-playbook-building-blocks.md#self-service-password-reset)
   * [Az Azure multi-factor Authentication-telefonhívások](../articles/active-directory/active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls)
   * [MFA a feltételes hozzáférés a SaaS-alkalmazások](../articles/active-directory/active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications)
   * [Privileged Identity Management (PIM)](../articles/active-directory/active-directory-playbook-building-blocks.md#privileged-identity-management-pim)
   * [Kockázati események felderítése](../articles/active-directory/active-directory-playbook-building-blocks.md#discovering-risk-events)
   * [Bejelentkezési kockázati házirend telepítése](../articles/active-directory/active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies)
   * [Tanúsítványalapú hitelesítés konfigurálása](../articles/active-directory/active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)

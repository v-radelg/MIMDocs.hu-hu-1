---
title: A Microsoft Identity Manager management Agent ügynököt a Microsoft Graph |} Microsoft Docs
author: fimguy
description: A Microsoft Graph (előzetes verzió) az külső felhasználó AD fiókkezelés életciklusát. Ebben a forgatókönyvben egy szervezet az Azure AD-címtárhoz történő meghívta a Vendégek, és kívánja ezeket a vendégek hozzáférést a helyi Windows-hitelesítés és Kerberos-alapú alkalmazásokhoz
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 77d322f447546897ad18f0981e5faad12efafef1
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/26/2018
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Azure AD-vállalatok (B2B) együttműködés a Microsoft Identity Manager(MIM) 2016 SP1 Azure Application Proxy (nyilvános előzetes verzió)
============================================================================================================================

A kezdeti helyzetet előzetes verzióját a külső felhasználó AD fiókkezelés életciklus.   Ebben a forgatókönyvben egy szervezet meghívta a vendégek be az Azure AD-címtár, és hozzáférést adott vendégek a helyi Windows-hitelesítés és Kerberos-alapú alkalmazások keresztül kívánja a [az Azure AD-alkalmazást](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish)proxy vagy más átjáró mechanizmusokat. Az Azure AD-alkalmazásproxy igényel minden felhasználó saját AD DS-fiókjához, azonosítása és a delegálás célokra

## <a name="scenario-specific-supported-guidance"></a>A forgatókönyv támogatott útmutatót

Ebben a forgatókönyvben a szervezet meghívta a vendégek be az Azure AD-címtár, és kívánja ezeket a vendégek hozzáférést a helyszíni Windows. Hitelesítés és Kerberos-alapú alkalmazások integrált keresztül a [az Azure AD-alkalmazást](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) proxy vagy más átjáró mechanizmusokat. Az Azure AD-alkalmazásproxy igényel minden felhasználó saját AD DS-fiókjához, azonosítása és a delegálás célokra

A MIM és Azure Application Proxy B2B konfigurációjában néhány ismertetése

-   Már telepítette a [Graph kezelőügynök](microsoft-identity-manager-2016-connector-graph.md).

-   Rendelkezik egy helyszíni AD és az Azure AD-kapcsolati set fel felhasználókat és csoportokat az Azure AD szinkronizálása.

    -   Alkalmazás szabályozása Office csoportok segítségével érhető [az Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Ön már beállított alkalmazásproxy összekötők és összekötő csoportok, ha nem, fel is keresheti [Itt](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) telepítése és konfigurálása

-   Egy vagy több alkalmazást, amely integrált Windows-hitelesítés és az egyes AD-fiókok Azure AD alkalmazás-proxyn keresztül történő közzététele

-   Meghívót, vagy egy vagy több Vendégek, az Azure ad-ben létrehozott meghívott <https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-self-service-portal>

-   A Microsoft Identity Manager telepítve, és alapvető konfigurációs szolgáltatás és portál és az Active Directory-kezelőügynök.
    <https://docs.microsoft.com/en-us/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>B2B teljes körű központi telepítés

Forgatókönyv

A Contoso Pharmaceuticals Trey Research Inc. az R & D részleghez részeként működik. A Trey Research alkalmazottak szükség van a Contoso Pharmaceuticals által biztosított alkalmazás reporting kutatás elérésére.

-   A Contoso Pharmaceuticals egy független bérlői, hogy konfigurálta-e az egyéni tartománynév van.

-   A külső felhasználó meghívott: az a Contoso Pharmaceuticals bérlői.
    Ez a felhasználó elfogadta a meghívót, és hozzáférhetnek a megosztott erőforrások.

-   Az alkalmazások App proxyn keresztül, és ebben az esetben a MIM szolgáltatás és a Vendég felhasználói portál segítségével részt venni a MIM folyamat, például a segélyszolgálati forgatókönyvekhez példaként közzé.

## <a name="create-the-graph-management-agent"></a>A Graph-kezelőügynök létrehozása

Megjegyzés: Összekötő létrehozása előtt győződjön meg arról, hogy tekintse át a [Graph kezelőügynök](microsoft-identity-manager-2016-connector-graph.md).

A Synchronization Service Manager felhasználói felületén válassza ki a **összekötők** és **létrehozása**.
Válassza ki **Graph (Microsoft)** , és adjon neki könnyen megjegyezhető nevet

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Kapcsolat

A kapcsolat lapon meg kell adnia a Graph API verziója éles készen **V 1.0**, nem éles van **béta**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Képességek

Globális paraméterek lap különbözeti Változásnapló és további LDAP funkciók DN konfigurálhatja. A lap az LDAP-kiszolgáló által előre megadott jelenti.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Globális paraméterek

Globális paraméterek lap különbözeti Változásnapló és további LDAP funkciók DN konfigurálhatja. A lap az LDAP-kiszolgáló által előre megadott jelenti.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Kiépítési hierarchia konfigurálása

Ezen a lapon a megkülönböztető név összetevő, például OU hozzárendelése az objektumtípus, amely kell megadva lennie, például organizationalUnit szolgál. Ezután hagyja az alapértelmezett kattintson

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása

A partíciók és hierarchiák lapon válassza ki az összes névtér objektumok importálni és exportálni szeretné.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Objektumtípusok kiválasztása

A partíciók és hierarchiák lapon válassza ki az összes névtér objektumok importálni és exportálni szeretné.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Attribútumok kiválasztása

Attribútumok kiválasztása képernyőn válassza ki a szükséges attribútumok B2B felhasználók kezeléséhez. Az "id" attribútum szükség

-   Azonosítója

-   displayName

-   mail

-   givenName

-   Vezetéknév

-   UserPrincipalName

-   UserType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Horgonyok konfigurálása

Konfigurálása kapcsolati alappal rendelkező konfigurálása a horgony lépése a szükséges. Alapértelmezés szerint használja az id attribútum a leképezés.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Összekötőszűrő konfigurálása

A beállítás Összekötőszűrő lap, lehetővé teszi objektumok attribútumszűrő alapján szűrik. Ebben a forgatókönyvben a B2B célja csak a userType, amely a Vendég, és a tag nem egyenlő a felhasználók állapotba kerüljön.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Csatlakozási és leképezési szabályok konfigurálása

Szinkronizálási szabály konfigurálása csatlakozási és leképezési szabályok kezel, nem szükséges a csatlakozási és leképezési maga az összekötő a azonosítania kell. Hagyja bejelölve az alapértelmezett, és kattintson az OK gombra.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Attribútumfolyam konfigurálása

A csatlakozás és leképezés, például nincs szükség a Attribútumfolyam meghatározásához, mert az leíró a szinkronizálási szabály, amely később kell létrehozni. Hagyja bejelölve az alapértelmezett, és kattintson az OK gombra.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Deprovision konfigurálása

Konfigurálása deprovision engedélyezi, hogy az objektum törlését, ha a metaverzum-objektum törlése. Ez a vizsgálat a biztosítjuk őket disconnectors, az a célja, hogy hagyja őket az Azure-ban is jelenleg nem exportál semmit az Azure-ba, mert az importálás csak.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Bővítmények konfigurálása

Bővítmények konfigurálása az ebben a felügyeleti ügynök lehetőség érhető el, de nem szükséges a szinkronizálási szabály használjuk. Ha egy korábban az Attribútumfolyam előzetes szabályának döntöttünk, majd lenne egy lehetőség, hogy adja meg.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>A MIM szolgáltatás szinkronizálása szabályok létrehozása

az alábbi lépéseket az első lépések a leképezési és a Attribútumfolyam B2B Vendég fiók. Néhány feltételezéseket itt történik, hogy már rendelkezik az Active Directory felügyeleti ügynök konfigurálva, és a FIM MA konfigurált felvegye a MIM szolgáltatás és -portál.

A szinkronizálási szabály létrehozása előtt hozzon létre egy attribútum a személy objektum MV-tervezővel kötve userPrincipalName neve kell.

A szinkronizálás ügyfélprogram válassza ki a Metaverzumtervezőben

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Válassza ki a személy objektum típusa

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Ezután a műveletek kattintson az attribútum hozzáadása

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Végül végezze el a következő részleteket

Az attribútumnév: **userPrincipalName**

Attribútumtípus: **karakterlánc (példánycímkének)**

Indexelt = **igaz**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

A következő lépéseket a FIM szolgáltatás felügyeleti ügynöke és az Active Directory tartományi szolgáltatások-kezelőügynök minimális konfiguráció szükséges.

További részletek itt találhatók a konfiguráció <https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx> -hogyan rendelkezés felhasználók Active Directory tartományi szolgáltatásokhoz

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Szinkronizálási szabály: Az Azure Active Directoryból a szinkronizálási szolgáltatás Metaverse MV Vendég felhasználó importálása<br>

Keresse meg a MIM szolgáltatás és portál és Sycronization szabályok válassza ki, majd kattintson új.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Válassza a "Erőforrás létrehozása az FIM" ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Attribútumfolyam szabályok:

| **Csak a kezdeti folyamata** | **Fenntartása teszt használata** | **Egymást követő (FIM érték ⇒ cél attribútum)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName] (javascript:void(0);)                        |
|                       |                           | [Balra (azonosító, 20) ⇒accountName] (javascript:void(0);)                        |
|                       |                           | [id⇒uid] (javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType] (javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [surname⇒sn] (javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
|                       |                           | [id⇒cn] (javascript:void(0);)                                          |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone] (javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Szinkronizálási szabály: Active Directory Vendég felhasználói fiók létrehozása 

A szinkronizálási szabály a felhasználó létrehozása az active Directoryban

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Attribútumfolyam szabályok:

| **Csak a kezdeti folyamata** | **Fenntartása teszt használata** | **Egymást követő (FIM érték ⇒ cél attribútum)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName] (javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [sn⇒sn] (javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", OU = B2BGuest, DC = scontoso, DC = com" ⇒dn] (javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd] (javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl] (javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Szinkronizálási szabály: Import B2B Vendég felhasználói objektumok SID való bejelentkezéshez mim-mel engedélyezése 

A szinkronizálási szabály a felhasználó létrehozása az active Directoryban

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Végül azt hívhat meg a felhasználó, és futtassa a felügyeleti a következő sorrendben:

-   Teljes importálás és a MIMMA felügyeleti ügynök a szinkronizálás

-   Teljes importálás és szinkronizálás a ADMA_SCONTOSO_B2B felügyeleti ügynök

-   Teljes importálás és szinkronizálás a B2B Graph felügyeleti ügynök

-   Exportálás, különbözeti importálás és szinkronizálás a ADMA_SCONTOSO_B2B Management Agent

-   Exportálás, különbözeti importálás és a MIMMA felügyeleti ügynök a szinkronizálás

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>A végső lépés: Alkalmazásproxy B2B Vendég, és jelentkezzen be a MIM

Most, hogy a szinkronizálási szabályok létrehoztunk a MIM-ben. Az alkalmazás proxykonfigurációt határozza meg a Kerberos által korlátozott Delegálás app proxy annak engedélyezése, hogy a felhő elv segítségével.
Is ezután a felhasználó manuálisan hozzáadni a a felhasználók és csoportok kezelése. A beállításokat nem mutatja, a felhasználó létrehozása a MIM és a Vendég egyszer kiépített office csoporthoz a jelen dokumentum nem ismerteti egy kicsit további konfigurációt igényel hozzáadása történt.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Csak a kezdeti folyamata** | **Fenntartása teszt használata** | **Egymást követő (FIM érték ⇒ cél attribútum)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName] (javascript:void(0);)                     |
|                       |                           | [A "CONTOSO" ⇒domain] (javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid] (javascript:void(0);)                                      |

Amennyiben az összes konfigurálása

Végül B2B felhasználói bejelentkezési adatokkal, és tekintse meg az alkalmazás

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>További lépések
----------

[Felhasználók kiépítése az Active Directory tartományi szolgáltatásokban](https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx)

[FIM 2010-funkciók dokumentációja](https://technet.microsoft.com/en-us/library/ff800820(v=ws.10).aspx)

[Útmutató a helyszíni alkalmazások biztonságos távoli hozzáférést biztosítanak](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

[Töltse le a Microsoft Identity Manager felügyeleti ügynök a Microsoft Graph (előzetes verzió)](http://go.microsoft.com/fwlink/?LinkId=717495)

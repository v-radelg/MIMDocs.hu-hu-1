---
title: A Microsoft Identity Manager-összekötő konfigurálása a Microsoft Graph B2B-hez |} A Microsoft Docs
author: billmath
description: A Microsoft Graph-összekötő az külső felhasználó AD fiókok életciklusának kezelése. Ebben a forgatókönyvben egy szervezet meghívta a vendégek be az Azure AD-címtárhoz, és meg kívánja szüntetni ezeket vendégek hozzáférési helyszíni Windows-Integrated hitelesítés és Kerberos-alapú alkalmazások
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 139c58510117ad422529a4ff0facd23040023713
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358771"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Az Azure AD vállalatközi (B2B) együttműködés a Microsoft Identity Manager(MIM) 2016 SP1-et az Azure-alkalmazásproxyval
============================================================================================================================

A kezdeti forgatókönyvet az külső felhasználó AD fiókok életciklusának kezelése.   Ebben a forgatókönyvben egy szervezet meghívta a vendégek be az Azure AD-címtárhoz, és meg kívánja szüntetni ezeket vendégek hozzáférési helyszíni Windows-Integrated hitelesítés és Kerberos-alapú alkalmazások keresztül a [Azure AD-alkalmazásproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) vagy más átjáró mechanizmusokat. Az Azure AD-alkalmazásproxy minden felhasználónak szüksége van a saját AD DS-fiókot, azonosítása és a delegálási célokra.

## <a name="scenario-specific-guidance"></a>A forgatókönyv jellemző útmutatást

Néhány kalkulált B2B és a MIM és az Azure AD-alkalmazásproxy konfigurációjában:

-   Már telepítette a helyszíni AD-ben és a Microsoft Identity Manager telepített és az alapszintű konfigurációs a MIM szolgáltatás, a MIM-portál, az Active Directory-kezelőügynök (AD MA) és a FIM-kezelőügynök (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Hogyan töltse le és telepítse a cikkben lévő utasítások már követte a [Graph összekötő](microsoft-identity-manager-2016-connector-graph.md).

-   Az Azure AD Connect szinkronizálási felhasználók és csoportok az Azure ad-hez konfigurálva van.

-   Az Azure AD Connect szinkronizálása az Office-csoportokat való az alkalmazás konfigurálva van [vissza a helyszíni Active Directory tartományi szolgáltatások](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Már állított be az alkalmazásproxy-összekötők és összekötőcsoportok, ha nem, keresse fel [Itt](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) telepítése és konfigurálása

-   Már egy vagy több alkalmazás, amely integrált Windows-hitelesítést vagy az egyes AD-fiókok Azure AD-alkalmazásproxyn keresztül közzétett

-   Meghívott vagy egy vagy több Vendégek, az Azure AD-ben létrehozott egy vagy több felhasználó eredményező meghívott <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>B2B teljes körű üzembe helyezési példa

Ez az útmutató az alábbi forgatókönyv épül:

Contoso Pharmaceuticals részeként az R & D részleg működik a Trey Research Inc. A Trey Research alkalmazottak a Contoso Pharmaceuticals által biztosított alkalmazás reporting kutatás eléréséhez szükséges.

-   A saját bérlő konfigurált egy egyéni tartományt a Contoso Pharmaceuticals találhatók.

-   Valaki meghívta egy külső felhasználót a Contoso Pharmaceuticals bérlőhöz.
    Ez a felhasználó elfogadta a meghívást, és hozzáférhetnek a megosztott erőforrások.

-   Contoso Pharmaceuticals egy alkalmazás alkalmazásproxyn keresztül közzétett. Ebben a forgatókönyvben a mintaalkalmazás a MIM-portálhoz. Ez lehetővé tenné a vendégfelhasználó részt venni a MIM folyamatokban, például a help desk forgatókönyvek, vagy kérjen hozzáférést a MIM-ben csoportokhoz.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Konfigurálja az AD és az Azure AD Connect Azure AD-ből felvett felhasználók kizárása

Alapértelmezés szerint az Azure AD Connect feltételezi, hogy a nem rendszergazda felhasználóknak az Active Directoryban kell szinkronizálni az Azure AD-be.  Ha az Azure AD Connect talál egy meglévő felhasználó az Azure ad-ben, amely megfelel a felhasználónak a helyszíni AD, az Azure AD Connect fogja a két fiók megfelelő és tegyük fel, hogy ez a felhasználó egy korábban szinkronizálási, győződjön meg arról, a helyszíni AD mérvadó.  Azonban ez az alapértelmezett viselkedés nem alkalmas a B2B-folyamatot, ha a felhasználói fiók származik-e az Azure ad-ben. 

Ezért a felhasználók beemelte Active Directory tartományi szolgáltatások a MIM által az Azure ad-ből kell úgy, hogy az Azure AD vissza az Azure AD-felhasználók szinkronizálása nem próbálja meg tárolni.
Ennek egyik módja, hogy az AD DS-ben egy új szervezeti egység létrehozása és konfigurálása az Azure AD Connect zárhat ki adott szervezeti egység.  

További információ található [Azure AD Connect szinkronizálása: a szűrés konfigurálása](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Az Azure AD-alkalmazás létrehozása 


Megjegyzés: A felügyeleti ügynök a graph-összekötő létrehozása előtt a MIM Sync előtt győződjön meg arról, tekintse át és üzembe helyezését az útmutató a [Graph összekötő](microsoft-identity-manager-2016-connector-graph.md), és létrehozott egy alkalmazást az ügyfél-azonosítója és kulcsa.
Győződjön meg arról, hogy az alkalmazás számára engedélyezett a(z) ezek az engedélyek közül legalább egyet: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` vagy `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Az új kezelőügynök létrehozása


A Synchronization Service Manager felhasználói felületén, válassza ki a **összekötők** és **létrehozás**.
Válassza ki **Graph (Microsoft)** , és adjon meg egy leíró nevet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Kapcsolat

A kapcsolatok lapon meg kell adnia a Graph API-verzió. Kész PAI üzemi van **V 1.0**, nem éles üzemi van **béta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Globális paraméterek

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Kiépítési hierarchia konfigurálása

Ezen a lapon a DN összetevő, például a szervezeti egység, leképezése az objektumtípus, amely kell létrehozni, például organizationalUnit szolgál. Ez nem szükséges ehhez a forgatókönyvhöz, ezért hagyja az alapértelmezett, és kattintson a Tovább gombra.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása

A partíciók és hierarchiák lapon válassza ki az objektumok importálása és exportálása tervezi az összes névtér.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Objektumtípusok kiválasztása

Az objektum típusú oldalon válassza ki az objektumtípusokat, történő importálását tervezi. Ki kell választania legalább "Felhasználó".

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Attribútumok kiválasztása

Az attribútumok kiválasztása képernyőn attribútumok kiválasztása az Azure AD B2B-felhasználók kezelése az ad-ben szükséges. Az "ID" attribútum szükség.  Az attribútumok `userPrincipalName` és `userType` később az ehhez a konfigurációhoz használni fogják.  Más jellemzők megadása nem kötelező, többek között

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Horgonyok konfigurálása

A Forráshorgony konfigurálása képernyő az konfigurálása a forráshorgony attribútumának lépésre szükség. Alapértelmezés szerint az azonosító attribútum használata a felhasználó-hozzárendelés.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Összekötőszűrő konfigurálása

Összekötőszűrő konfigurálása lapon MIM teszi lehetővé objektumok alapuló attribútumszűrő kiszűréséhez. A cél az ebben a forgatókönyvben B2B-hez, hogy csak a felhasználók a következő értékkel: a `userType` attribútum, amely egyenlő `Guest`, és nem a userType, amely megegyezik a felhasználók `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Csatlakozási és leképezési szabályok konfigurálása

Ez az útmutató feltételezi, hogy egy szinkronizálási szabályt létrehozni.  Szinkronizálási szabály konfigurálása csatlakozási és leképezési szabályok kezeli, ahogy azt nem szükséges egy csatlakozási és leképezési maga az összekötő a azonosítania kell. Hagyja bejelölve az alapértelmezett, és kattintson az OK gombra.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Attribútumfolyam konfigurálása

Ez az útmutató feltételezi, hogy egy szinkronizálási szabályt létrehozni.  Leképezése nem szükséges a MIM Sync, a Attribútumfolyam meghatározásához, mivel kezeli, a szinkronizálási szabály, amely később jön. Hagyja bejelölve az alapértelmezett, és kattintson az OK gombra.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Megszüntetési konfigurálása

A megszüntetési konfigurálása beállítással konfigurálhatja a MIM sync törölje az objektumot, és a metaverzum-objektum törlése. Ebben a forgatókönyvben VÁLLALUNK őket disconnectors, az a célja, hogy hagyja őket az Azure ad-ben. Ebben a forgatókönyvben azt nem exportál semmit az Azure ad-hez, és az összekötő úgy van beállítva az importáláshoz csak.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Bővítmények konfigurálása

Bővítmények konfigurálása a felügyeleti ügynök lehetőség, de nem szükséges, mert a szinkronizálási szabály használjuk. Ha úgy döntött korábban az Attribútumfolyam használni egy olyan speciális szabályt, majd lenne egy beállítást a szabályok bővítmény meghatározásához.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>A metaverzum-séma kiterjesztése


A szinkronizálási szabály létrehozása esetén előtt kell nevű MV-tervezővel személy objektum kötött userPrincipalName attribútum létrehozása.

Válassza ki a szinkronizációs ügyfél Metaverzumtervező

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Ezután válassza ki a személy

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Ezután a műveletek területen kattintson a attribútum hozzáadása

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Végezze el az alábbi részletek

Attribútumnév: **userPrincipalName**

Attribútum típusa: **karakterlánc (indexelhető)**

Indexelt = **igaz**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>A MIM szolgáltatás szinkronizálása szabályok létrehozása

az alábbi lépéseket a megkezdjük a B2B Vendég fiókkal és az Attribútumfolyam hozzárendelését. Bizonyos feltételezések történik: az Active Directory konfigurálva MA már rendelkezik, és a FIM MA konfigurálva ahhoz, hogy a felhasználók a MIM szolgáltatás és portál.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

A következő lépések hozzáadása a FIM MA és az AD MA minimális konfigurációs van szükség.

További részletekért itt található a konfiguráció <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> – hogyan lehet a felhasználók kiépítése az AD DS-ben

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Szinkronizálási szabály: Az Azure Active Directoryból a szinkronizálási szolgáltatás Metaverzum MV vendégfelhasználó importálása<br>

Keresse meg a MIM-portál, válassza ki a szinkronizálási szabályait, és kattintson az új.  A B2B folyamat a graph-összekötőn keresztül bejövő szinkronizálási szabály létrehozása.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

A kapcsolati feltételek lépésnél válassza az "Erőforrás létrehozása a FIM" lehet.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

A következő attribútum bejövő szabályok konfigurálása.  Ügyeljen arra, hogy feltölti a `accountName`, `userPrincipalName` és `uid` attribútumok szerint használni fogják őket később az ebben a forgatókönyvben:

| **Csak a kezdeti folyamat** | **Létezik-e teszt használata** | **Flow-(adatforrás-értéke ⇒ FIM attribútum)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Balra (azonosító, 20) ⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Szinkronizálási szabály: A Vendég felhasználói fiókot az Active Directory létrehozása 

A szinkronizálási szabály a felhasználó az Active Directoryban hoz létre.  Ügyeljen arra, hogy a flow for `dn` kell helyeznie a felhasználónak a szervezeti egység, amely az Azure AD Connect ki lett zárva.  Frissítse a flow for `unicodePwd` felel meg az AD-jelszóházirendet – a felhasználónak nem kell ismernie kell a jelszót.  Jegyezze fel az értékét a `262656` a `userAccountControl` kódolja a jelzők `SMARTCARD_REQUIRED` és `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Szabályok:

| **Csak a kezdeti folyamat** | **Létezik-e teszt használata** | **Flow (FIM érték ⇒ cél attribútum)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", OU = B2BGuest, DC = contoso, DC = com" ⇒dn] (javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Szinkronizálási szabály nem kötelező: Import B2B Vendég felhasználói objektumok SID, hogy a MIM bejelentkezési azonosító 

A bejövő szinkronizálási szabály elérhetővé teszi a felhasználó SID-attribútum az Active Directoryból vissza MIM-be, a felhasználó hozzáférhessen a MIM-portál.  A MIM-portál megköveteli, hogy a felhasználói attribútumok `samAccountName`, `domain` és `objectSid` töltse be a MIM szolgáltatás adatbázisában.

Az adatforrás külső rendszer konfigurálja a `ADMA`, mint a `objectSid` attribútumot a rendszer automatikusan beállítja AD, ha a MIM hoz létre a felhasználó által.
 
Vegye figyelembe, hogy a felhasználók a MIM szolgáltatásban létrehozandó konfigurálásakor győződjön meg arról, ezeket a rendszer nem minden alkalmazott SSPR felügyeletiházirend-szabályok szánt csoportok hatókörében.  Előfordulhat, hogy módosítani szeretné a szabályzatkészlet-definícióból kizárandó felhasználók, akik a B2B folyamat hozták létre. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Csak a kezdeti folyamat** | **Létezik-e teszt használata** | **Flow-(adatforrás-értéke ⇒ FIM attribútum)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | [A "CONTOSO" ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Futtassa a szinkronizálási szabályok

Ezután azt a felhasználó meghívása, és futtassa a felügyeleti ügynök szinkronizálási szabályok a következő sorrendben:

-   Teljes importálás és a szinkronizálás az a `MIMMA` felügyeleti ügynök.  Ez biztosítja a MIM Sync van konfigurálva a legújabb szinkronizálási szabályait.

-   Teljes importálás és a szinkronizálás az a `ADMA` felügyeleti ügynök.  Ez biztosítja, hogy a MIM és az Active Directory konzisztensek legyenek.  Ezen a ponton van még nem minden függőben lévő exportálásai vendégek.

-   Teljes importálás és szinkronizálás a B2B Graph kezelőügynökön.  Ekkor a vendégfelhasználók a metaverzumba.  Ezen a ponton egy vagy több fiókot függő exportálás-a lesz `ADMA`.  Ha nincs függőben lévő exportálások, majd ellenőrizze, hogy vendégfelhasználók összekötőterében importálta-e, és, hogy a szabályok konfigurált AD-fiókokat kell biztosítani.

-   Exportálási, különbözeti importálás és szinkronizálás a `ADMA` felügyeleti ügynök.  Ha a kivitel sikertelen volt, ellenőrizze a szabály konfigurálását, és határozza meg, hogy voltak-e hiányzó séma követelményeit. 

-   Exportálási, különbözeti importálás és szinkronizálás a `MIMMA` felügyeleti ügynök.  Amikor ezzel elkészült, a már nem kell minden olyan függőben lévő exportálások.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Választható lehetőség: Az alkalmazásproxy B2B-hez vendégek jelentkezik be a MIM-portál

Most, hogy a szinkronizálási szabályok a MIM-ben létrehozott azt. Az alkalmazás proxykonfigurációt határoz meg a felhő egyszerű használja az alkalmazásproxy engedélyezéséhez a kcd Szolgáltatáshoz.
Ezenkívül ezután a felhasználó manuálisan hozzáadni, a felhasználók és csoportok kezelése. A beállítások nem a felhasználó számára megjelenítendő mindaddig, amíg a MIM használatával adja hozzá a Vendég egyszer kiosztott office csoporthoz nem ebben a dokumentumban ismertetett egy kicsit több konfigurálást igényel létrehozása történt.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


B2B-felhasználói bejelentkezésre van konfigurálva, az összes, és a az alkalmazás megtekintéséhez.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>További lépések
----------

[Felhasználók kiépítése az Active Directory tartományi szolgáltatásokban](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[FIM 2010-funkciók dokumentációja](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Hogyan lehet a helyszíni alkalmazások biztonságos távoli elérést biztosíthat](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[A Microsoft Identity Manager-összekötő letöltése a Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)

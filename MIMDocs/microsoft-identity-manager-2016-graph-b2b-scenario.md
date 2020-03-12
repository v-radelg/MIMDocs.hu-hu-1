---
title: A Microsoft Graph for B2B Microsoft Identity Manager-összekötő konfigurálása | Microsoft Docs
author: billmath
description: Microsoft Graph-összekötő külső felhasználói AD-fiók életciklus-kezelése. Ebben a forgatókönyvben a szervezet felkérte a vendégeket az Azure AD-címtárba, és hozzáférést kíván biztosítani a vendégeknek a helyszíni Windows-hitelesítéshez vagy a Kerberos-alapú alkalmazásokhoz
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2f91a5c24df5475130755574c77b536f57e64d24
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044242"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Azure AD-beli vállalatközi (B2B) együttműködés a Microsoft Identity Manager (platform) 2016 SP1 és az Azure Application proxy használatával
============================================================================================================================

A kezdeti forgatókönyv a külső felhasználói AD-fiók életciklus-kezelése.   Ebben a forgatókönyvben a szervezet felkérte a vendégeket az Azure AD-címtárba, és hozzáférést kíván biztosítani a felhasználóknak a helyszíni Windows-hitelesítéshez vagy a Kerberos-alapú alkalmazásokhoz az [Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) -alkalmazásproxy vagy más átjáró-mechanizmusok segítségével. Az Azure AD-alkalmazásproxy megköveteli, hogy minden felhasználó saját AD DS fiókkal rendelkezzen az azonosítási és delegálási célból.

## <a name="scenario-specific-guidance"></a>Forgatókönyv-specifikus útmutató

Néhány feltételezés a B2B és az Azure AD Application Proxy:

-   Már telepített egy helyszíni AD-t, és Microsoft Identity Manager telepítve van, és alapszintű konfigurációt tartalmaz a Rendszerfelügyeleti webszolgáltatások, a webkiszolgálói portál, a Active Directory Management agent (AD MA) és a FIM felügyeleti ügynök (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Már követte a [Graph-összekötő](microsoft-identity-manager-2016-connector-graph.md)letöltésére és telepítésére vonatkozó cikk utasításait.

-   A felhasználók és csoportok Azure AD-be való szinkronizálásához Azure AD Connect konfigurálva.

-   Már beállított alkalmazásproxy-összekötőket és összekötő-csoportokat, ha nem, akkor látogasson el [ide](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) a telepítéshez és konfiguráláshoz

-   Már közzétett egy vagy több alkalmazást, amely a Windows integrált hitelesítésére vagy az egyes AD-fiókokra támaszkodik Azure AD alkalmazás proxyn keresztül

-   Meghívott vagy meghív egy vagy több vendéget, amelyek egy vagy több, az Azure AD-ban létrehozott felhasználót eredményeztek <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Példa a VÁLLALATKÖZI végpontok közötti üzembe helyezési forgatókönyvre

Ez az útmutató a következő forgatókönyvre épül:

A contoso Pharmaceuticals az R & D osztályának részeként együttműködik a Trey Research Inc.-vel. A Trey Research alkalmazottainak hozzá kell férniük a contoso Pharmaceuticals által biztosított Research Reporting alkalmazáshoz.

-   A contoso Pharmaceuticals a saját bérlőn vannak, hogy egyéni tartományt konfiguráljanak.

-   Valaki meghívott egy külső felhasználót a contoso gyógyszeripari bérlőnek.
    Ez a felhasználó elfogadta a meghívót, és hozzáférhet a megosztott erőforrásokhoz.

-   A contoso Pharmaceuticals az App proxyn keresztül közzétett egy alkalmazást. Ebben az esetben a példaként szolgáló alkalmazás a webszolgáltatási portál. Ez lehetővé tenné egy vendég felhasználó számára, hogy részt vegyen a következőben: webalkalmazási folyamatokban, például az ügyfélszolgálati forgatókönyvekben, vagy a csoportokhoz való hozzáférés kéréséhez.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Az AD és a Azure AD Connect konfigurálása az Azure AD-ből hozzáadott felhasználók kizárásához

Alapértelmezés szerint a Azure AD Connect azt feltételezi, hogy Active Directory nem rendszergazda felhasználókat kell szinkronizálni az Azure AD-be.  Ha Azure AD Connect megkeres egy meglévő felhasználót az Azure AD-ben, amely megfelel a helyszíni AD felhasználójának, Azure AD Connect a két fióknak fog megfelelni, és feltételezi, hogy ez a felhasználó korábbi szinkronizálása, és a helyszíni AD mérvadó.  Ez az alapértelmezett viselkedés azonban nem alkalmas a B2B-folyamathoz, ahol a felhasználói fiók az Azure AD-ből származik. 

Ezért az Azure AD-ből származó, a AD DS által bevitt felhasználókat úgy kell tárolni, hogy az Azure AD ne kísérelje meg a felhasználók Azure AD-beli szinkronizálását.
Ennek egyik módja egy új szervezeti egység létrehozása AD DSban, és a Azure AD Connect konfigurálása a szervezeti egység kizárásához.  

További információ a [Azure ad Connect Sync: szűrés konfigurálása](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering)című témakörben található. 
 

## <a name="create-the-azure-ad-application"></a>Az Azure AD-alkalmazás létrehozása 


Megjegyzés: a weblapon való létrehozása előtt szinkronizálja a Graph-összekötő felügyeleti ügynökét, győződjön meg arról, hogy áttekintette a [gráf-összekötő](microsoft-identity-manager-2016-connector-graph.md)üzembe helyezésére vonatkozó útmutatót, és létrehozott egy alkalmazást az ügyfél-azonosítóval és a titkos kulccsal.
Győződjön meg arról, hogy az alkalmazás jogosult a következő engedélyek legalább egyikére: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` vagy `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Az új felügyeleti ügynök létrehozása


A Synchronization Service Manager felhasználói felületen válassza az **összekötők** és **Létrehozás**lehetőséget.
Válassza a **Graph (Microsoft)**  lehetőséget, és adjon neki egy leíró nevet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Kapcsolat

A kapcsolat lapon meg kell adnia a Graph API verziót. A gyártásra kész posta a **V 1,0**, a nem éles üzemben lévő **béta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Globális paraméterek

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Kiépítési hierarchia konfigurálása

Ez a lap a DN összetevő, például a szervezeti egység (OU) leképezésére szolgál a kiépíteni kívánt objektumtípus számára, például organizationalUnit. Ez nem szükséges ehhez a forgatókönyvhöz, ezért hagyja meg az alapértelmezett beállítást, és kattintson a Tovább gombra.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciók és hierarchiák konfigurálása

A partíciók és hierarchiák lapon jelölje ki az importálni és exportálni kívánt objektumokat tartalmazó összes névteret.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Objektumtípusok kiválasztása

Az Objektumtípusok lapon válassza ki az importálni kívánt objektumtípusokat. Ki kell választania legalább "user" elemet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Attribútumok kiválasztása

Az attribútumok kiválasztása képernyőn válassza ki azokat az attribútumokat az Azure AD-ből, amelyekre szükség lesz a B2B-felhasználók felügyeletéhez az AD-ben. Az "ID" attribútum megadása kötelező.  A konfiguráció későbbi részében `userPrincipalName` és `userType` attribútumokat fogja használni a rendszer.  Egyéb attribútumok nem kötelezőek, beleértve a következőket is

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Horgonyok konfigurálása

A horgony konfigurálása képernyőn a horgony attribútum konfigurálása kötelező lépés. Alapértelmezés szerint a felhasználói leképezéshez használja az ID attribútumot.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Összekötő-szűrő konfigurálása

Az összekötő-szűrő beállítása lapon a rendszer lehetővé teszi az objektumok szűrését az attribútumok szűrője alapján. Ebben a példában a B2B esetében a cél az, hogy a felhasználók csak a `Guest`t egyenlő `userType` attribútum értékével legyenek felhasználva, nem pedig a userType, amely egyenlő `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Csatlakozási és leképezési szabályok konfigurálása

Ez az útmutató feltételezi, hogy egy szinkronizálási szabályt fog létrehozni.  Az illesztési és a leképezési szabályok konfigurálásakor a szinkronizálási szabály kezeli a csatlakozást és a leképezést az összekötőn. Hagyja meg az alapértelmezett értéket, majd kattintson az OK gombra.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Az attribútum folyamatának konfigurálása

Ez az útmutató feltételezi, hogy egy szinkronizálási szabályt fog létrehozni.  A kivetítés nem szükséges az attribútum folyamatának definiálásához a webalkalmazás-szinkronizálásban, mivel azt a később létrehozott szinkronizálási szabály kezeli. Hagyja meg az alapértelmezett értéket, majd kattintson az OK gombra.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Kiépítés konfigurálása

A kiépítés konfigurálására szolgáló beállítás lehetővé teszi, hogy a rendszer az objektum törlésére konfigurálja a webszolgáltatások szinkronizálását, ha a metaverse-objektumot törölték. Ebben a forgatókönyvben a leválasztók a cél az, hogy az Azure AD-ben hagyják el őket. Ebben az esetben nem exportálunk semmit az Azure AD-be, és az összekötő csak importálásra van konfigurálva.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Bővítmények konfigurálása

A bővítmények konfigurálása ezen a felügyeleti ügynökön egy lehetőség, de nem kötelező, mert szinkronizálási szabályt használunk. Ha úgy döntöttünk, hogy korábban egy speciális szabályt használunk az attribútum folyamatában, akkor a szabályok kiterjesztését is meg lehet határozni.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>A metaverse-séma kiterjesztése


A szinkronizálási szabály létrehozása előtt létre kell hoznia egy userPrincipalName nevű attribútumot, amely az MV Designer használatával van társítva a person objektumhoz.

A szinkronizálási ügyfélben válassza a metaverse Designer elemet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Ezután válassza ki a személy típusú objektumot

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Ezután a műveletek területen kattintson az attribútum hozzáadása elemre.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Ezután végezze el a következő adatokat

Attribútum neve: **userPrincipalName**

Attribútum típusa: **karakterlánc (indexelhető)**

Indexelt = **igaz**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>A fakiszolgálói szolgáltatás szinkronizációs szabályainak létrehozása

az alábbi lépésekben megkezdjük a B2B vendég fiók és az attribútum folyamatának hozzárendelését. Néhány feltételezést itt talál: már van konfigurálva a Active Directory MA, és a FIM MA úgy van konfigurálva, hogy a felhasználók számára elérhetővé tegye a rendszerállapot-szolgáltatást és a portált.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

A következő lépésekhez minimális konfiguráció hozzáadására van szükség a FIM MA és az AD MA szolgáltatáshoz.

További részletekért tekintse meg a konfigurációs <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> – hogyan lehet felhasználókat kiépíteni a AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Szinkronizálási szabály: a vendég felhasználó importálása a MV-be a Azure Active Directory-ből származó metaverse szolgáltatásba<br>

Navigáljon a webhelyes portálra, válassza a szinkronizálási szabályok lehetőséget, majd kattintson az új elemre.  Hozzon létre egy bejövő szinkronizálási szabályt a B2B-folyamathoz a Graph-összekötőn keresztül.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

A kapcsolati feltételek lépésben válassza az "erőforrás létrehozása a FIM-ben" lehetőséget.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Konfigurálja a következő bejövő attribútum-adatfolyamati szabályokat.  Ügyeljen arra, hogy feltöltse a `accountName`, `userPrincipalName` és `uid` attribútumokat, mivel ezek a forgatókönyv későbbi részében lesznek felhasználva:

| **Csak kezdeti folyamat** | **Használat létezési tesztként** | **Flow (forrásoldali érték ⇒ FIM-attribútum)**                          |
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

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Szinkronizálási szabály: Vendég felhasználói fiók létrehozása Active Directory 

Ez a szinkronizálási szabály a felhasználót Active Directory hozza létre.  Győződjön meg arról, hogy a `dn` folyamatának a szervezeti egységben kell megadnia a felhasználót, amely ki lett zárva Azure AD Connectból.  Frissítse a `unicodePwd` folyamatát is, hogy megfeleljen az AD-jelszóra vonatkozó házirendnek – a felhasználónak nem kell tudnia a jelszót.  Figyelje meg, hogy `userAccountControl` `262656` értéke a jelzők `SMARTCARD_REQUIRED` és `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Folyamat szabályai:

| **Csak kezdeti folyamat** | **Használat létezési tesztként** | **Flow (FIM Value ⇒ cél attribútum)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", OU = B2BGuest, DC = contoso, DC = com" ⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Választható szinkronizálási szabály: B2B vendég felhasználói objektumok biztonsági azonosítójának importálása, amely lehetővé teszi a bejelentkezést a rendszerbe 

Ez a bejövő szinkronizálási szabály a felhasználó SID-attribútumát Active Directory vissza a webhelyre, így a felhasználó hozzáférhet a webszolgáltatási portálhoz.  A webalkalmazás-portál használatához a felhasználónak rendelkeznie kell az attribútumokkal `samAccountName`, `domain` és `objectSid` a fakiszolgálói szolgáltatás adatbázisában.

Konfigurálja a forrás külső rendszerét `ADMA`ként, mivel a `objectSid` attribútum automatikusan be lesz állítva az AD által, amikor a felhasználó létrehozza a felhasználót.
 
Vegye figyelembe, hogy ha úgy konfigurálja a felhasználókat, hogy a fakiszolgálói szolgáltatásban jöjjenek létre, győződjön meg arról, hogy a felhasználók nem tartoznak az alkalmazottak SSPR-kezelési házirendjének szabályaihoz tartozó készletek hatókörébe.  Előfordulhat, hogy módosítania kell a set definíciókat, hogy kizárják a B2B-folyamat által létrehozott felhasználókat. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Csak kezdeti folyamat** | **Használat létezési tesztként** | **Flow (forrásoldali érték ⇒ FIM-attribútum)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | [A "CONTOSO" ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>A szinkronizálási szabályok futtatása

Ezután meghívja a felhasználót, majd futtassa a felügyeleti ügynök szinkronizálási szabályait a következő sorrendben:

-   Teljes Importálás és szinkronizálás a `MIMMA` felügyeleti ügynökön.  Ez biztosítja, hogy a webszolgáltatások szinkronizálása a legújabb szinkronizálási szabályokkal legyen konfigurálva.

-   Teljes Importálás és szinkronizálás a `ADMA` felügyeleti ügynökön.  Ezzel biztosítható, hogy a webActive Directory konzisztensek legyenek.  Ezen a ponton a vendégek még nem lesznek függőben lévő exportálások.

-   Teljes Importálás és szinkronizálás a B2B Graph felügyeleti ügynökön.  Így a vendég felhasználói a metaverse-ben jelennek meg.  Ezen a ponton egy vagy több fiók `ADMA`exportálásra vár.  Ha nincsenek függőben lévő exportálások, ellenőrizze, hogy a vendég felhasználókat importálták-e az összekötő területére, és hogy a szabályok az AD-fiókok számára lettek-e megadva.

-   Exportálás, különbözeti Importálás és szinkronizálás a `ADMA` felügyeleti ügynökön.  Ha az Exportálás sikertelen volt, ellenőrizze a szabály konfigurációját, és állapítsa meg, hogy van-e hiányzó séma-követelmény. 

-   Exportálás, különbözeti Importálás és szinkronizálás a `MIMMA` felügyeleti ügynökön.  Ha ez befejeződik, a továbbiakban nem lehet függőben lévő exportálás.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Nem kötelező: alkalmazásproxy a B2B-vendégek számára a webalkalmazás-portálra való bejelentkezéshez

Most, hogy létrehoztuk a modul szinkronizálási szabályait. Az alkalmazás-proxy konfigurációjában adja meg a KCD használatának engedélyezése a Felhőbeli rendszerbiztonsági tag használatával lehetőséget.
Emellett adja hozzá manuálisan a felhasználót a felhasználók és csoportok kezelése elemhez. A felhasználó nem jelenítheti meg a felhasználót, amíg a létrehozás nem történt meg a következőben: a vendég hozzáadása egy Office-csoporthoz az üzembe helyezést követően a jelen dokumentumban nem szereplő, valamivel több konfigurációra van szükség.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Az összes konfigurálása után a B2B felhasználói bejelentkezés és az alkalmazás megtekinthető.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>További lépések
----------

[Felhasználók kiépítése az Active Directory tartományi szolgáltatásokban](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[FIM 2010-funkciók dokumentációja](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Biztonságos távoli hozzáférés biztosítása a helyszíni alkalmazások számára](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Microsoft Graph Microsoft Identity Manager-összekötő letöltése](https://go.microsoft.com/fwlink/?LinkId=717495)

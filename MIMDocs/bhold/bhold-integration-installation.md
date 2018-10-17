---
title: FIM vagy MIM BHOLD integration telepítése |} A Microsoft Docs
description: A BHOLD integration modul hozzá önkiszolgáló szerepkörkezelés MIM és a FIM
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 317c9ae4c940a509b6ac328cd5bb7cd7baa4dde9
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358805"
---
# <a name="bhold-fimmim-integration-installation"></a>A BHOLD FIM vagy MIM-integráció telepítése

A BHOLD FIM integrációs modul ad önkiszolgáló szerepkörök kezeléséhez a Microsoft Identity Manager, teszi a felhasználók számára a további szerepkörök kérés és a kényszerítése, akik hajthatja végre ezeket a szerepköröket. A BHOLD FIM integrációs modul kiterjeszti a FIM-portál megkönnyíti a FIM általános Adminisztráció részeként felhasználói szerepkörök kezelése. Ez a témakör ismerteti, hogyan kell konfigurálnia a hálózati infrastruktúra telepítése és használata a BHOLD FIM integrációs modul teszi lehetővé. Azt is bemutatja, hogyan telepítheti a BHOLD FIM integrációs modul és a konfigurációt, amely a BHOLD FIM integrációs modul telepítése után szükség van.

## <a name="bhold-fim-integration-installation-requirements"></a>BHOLD FIM integrációs telepítési követelményei

A BHOLD FIM integrációs modul kiterjeszti a FIM-portál és a FIM szolgáltatás, hogy a felhasználók szerepkörökhöz a FIM-portálon kezelheti. Ezért elengedhetetlen, hogy a BHOLD Core modul és a szükséges FIM szolgáltatást telepíteni és konfigurálni a BHOLD FIM integrációs modul telepítése előtt.
A szoftver-összetevő, amely megtalálható a számítógépen kell lennie, a BHOLD FIM integrációs modul telepítése előtt a következők:

- A Microsoft Identity Manager 2016 portált és a szolgáltatás
- A Microsoft Silverlight 3 vagy újabb
- Az Internet information Services és az ASP.NET keretrendszerrel
- A Microsoft Silverlight-eszközök

Emellett a BHOLD Core és az Access Management-összekötő modulok már telepíteni kell a környezetben a kiszolgálón, és a FIM-egy vagy több BHOLD felügyeleti ügynököt kell konfigurálni. A BHOLD Core modul konfigurálásával kapcsolatos további információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Telepítésével és az Access Management-összekötő modullal kapcsolatos további információkért lásd: [Access Management Connector telepítése](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) és [tesztlabor-Útmutató: a BHOLD Access Management-összekötő](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> A FIM szolgáltatás adatbázis nevét a FIMService kell lennie. BHOLD FIM integrációs telepítése sikertelen lesz, ha a FIM nem lett telepítve a FIM szolgáltatás alapértelmezett adatbázisnév.

## <a name="before-you-begin"></a>Előkészületek

A BHOLD FIM integrációs modul telepítése előtt létre kell hoznia egy BHOLD könyvtárat a C: meghajtó (C:\BHOLD) gyökérkönyvtárában.

Emellett meg kell, amely a BHOLD FIM integrációs telepítővarázslója a telepítés befejezéséhez szükséges információk megadására. A következő munkalap segítségével fel ezt az információt, hogy készen áll a szükséges megadni.

### <a name="bholdfim-account-settings"></a>BHOLDFim fiókbeállításokat

| **Elem**                            | **Leírás**                                                                                                                                                                                                               | **Érték**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **A tartomány biztonsági szolgáltató használata** | Kiválasztott, itt adhatja meg, hogy az Active Directory Domain Services biztonsági fog férhet hozzá a BHOLD Core.                                                                                                                    | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                          | Megadja a tartomány tartalmazó a **szolgáltatásfiók** BHOLD Core telepítése során létrehozott. További információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló által automatikusan történik. Módosítsa a nevét, csak akkor, ha annak értéke helytelen. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-neve, nem a teljesen minősített tartománynevét (FQDN) használatával. Például ha a tartomány teljes Tartománynevét a fabrikam.com, adja meg a tartomány nevét, a FABRIKAM. |
| **Felhasználónév**                        | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                              | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                        | A szolgáltatás felhasználói fiók jelszava.                                                                                                                                                                           | Írás a jelszót kell megadnia: **fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM szolgáltatás beállításai

| **Elem**            | **Leírás**                                                                                                                                                                                                                               | **Érték**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Felhasználó**            | A bejelentkezési név annak a fióknak rendszergazdai jogosultságokkal rendelkező határozza FIM. A Microsoft határozottan javasolja, hogy nem használja a gyökér szintű felhasználó a BHOLD Core (alapértelmezés szerint a BHOLD Core telepítéséhez használt fiók) tartozó fiókot. | Írás a felhasználói fiók nevét itt:                                                                   |
| **Jelszó**        | A FIM rendszergazdai felhasználói fiók jelszava.                                                                                                                                                                                 | Írás a jelszót kell megadnia: **fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen. |
| **FIM-adatbázisba**    | A neve, a FIM szolgáltatás adatbázisához.                                                                                                                                                                                               | FIMService                                                                                          |
| **Webhely IP-Port** | Itt adhatja meg a nevét vagy IP-címe a FIM-portál kiszolgálóján és a webhelyének portja.                                                                                                                                                               | Írás a kiszolgáló nevét vagy a cím és port itt:                                                     |

### <a name="bhold-core-connection"></a>A BHOLD Core kapcsolat

| **Elem**               | **Leírás**                                                                                                                                                                                                                                                                                                                                                                               | **Érték**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Tartomány**             | A megadott fiók tartományának neve **felhasználói**, az alábbi. A tartomány NetBIOS (rövid) formátumban adja meg.                                                                                                                                                                                                                                                                   | A felhasználó domain Fióknév itt írni?                                                            |
| **Felhasználó**               | A fiókja bejelentkezési neve **egy felügyelő olyan BHOLD felhasználói** összes felhasználók és szerepkörök és a hivatkozásra, és felhasználói szerepkörök leválasztása engedéllyel rendelkezik. A Microsoft határozottan javasolja, hogy nem használja a gyökér szintű felhasználó a BHOLD Core (alapértelmezés szerint a BHOLD Core telepítéséhez használt fiók) tartozó fiókot. Ezt a fiókot kell ugyanazt a fiókot, amellyel csatlakozhat a FIM | Írás a felhasználói fiók nevét itt:                                                                   |
| **Jelszó**           | A megadott felhasználói fiók jelszavát adja **felhasználói**.                                                                                                                                                                                                                                                                                                                             | Írás a jelszót kell megadnia: **fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen. |
| **IP/gép címe** | Megadja a BHOLD Core-webhely kiszolgálójának IP-címét. Ne használja a kiszolgáló nevét.                                                                                                                                                                                                                                                                                                        | Írási itt az IP-cím:                                                                          |
| **Portszám**        | A BHOLD Core webhely-portszám meghatározására.                                                                                                                                                                                                                                                                                                                                          | Írja be ide a portszámot:                                                                         |

## <a name="bhold-fim-integration-setup"></a>BHOLD FIM-integráció beállítása

A BHOLD FIM integrációs modul telepítése, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD FIM integrációs modul telepítése:

- BholdFIMIntegration<em>\<verzió\></em>\_Release.msi

Cserélje le *\<verzió\>* a BHOLD FIM integrációs kiadás, amely telepíti a verziószámával.

A program rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

![msi fut](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Telepítés utáni feladatok

Miután telepítette a BHOLD FIM-integráció, konfigurálnia kell a Microsoft SharePoint a BHOLD szolgáltatási fiók a webhely-tulajdonosa engedélyt. Is ha a FIM-portál Secure Sockets Layer (SSL) biztonság használatára van konfigurálva, módosítania kell a címek szerepel a FIM-portálon BHOLD oldalak hivatkozásokat tartalmazó fájlok.

### <a name="configuring-sharepoint"></a>A SharePoint konfigurálása

A megfelelő működéshez BHOLD FIM-integráció a BHOLD service fiókot igényel a helyhez tartozó jogosultságokkal rendelkeznek a Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>A BHOLD szolgáltatásfiók helyhez tartozó engedélyek megadása

1.  Jelentkezzen be rendszergazdai jogosultságokkal a BHOLD FIM integrációs futtató kiszolgálón.

2.  Kattintson a **Start**, és kattintson a **Internet Exporer**.

3.  A címsorba írja be a <https://localhost> SharePoint SSL-biztonság használatára van konfigurálva, ellenkező esetben írja be <http://localhost>.

4.  A bal oldalon, a **csoportwebhely** kattintson **személyek és csoportok**.

5.  Alatt **csoportok** kattintson **hely csapattagok**, és a középső ablaktáblán eszköztáron kattintson **új**, és kattintson a **felhasználó hozzáadása**.

6.  Az a **felhasználó hozzáadása: csoportwebhely** lap **felhasználók/csoportok**, írja be a BHOLDApplicationGroup, majd kattintson a Névellenőrzés gomb alatt a **felhasználók/csoportok** mezőbe. A csoport nevét kell feloldhatónak lennie közé tartozik a tartomány nevét.

7.  Kattintson a **felhasználók engedélyt közvetlenül**válassza **teljes hozzáférés – teljes körű vezérléssel rendelkezik**, és kattintson a **OK**.

8.  Győződjön meg arról, hogy BHOLDApplicationGroup szerepel-e az **engedélyek: csoportwebhely**, majd zárja be az Internet Explorerben.

![msi fut](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>A BHOLD támogatása az SSL konfigurálása

Az FIM-portál SSL-biztonság használatára van konfigurálva, ha módosítania kell a FIM-kiszolgálón lévő fájlok, hogy a BHOLD hivatkozásokat nyílik meg. A fájlok találhatók, a következő mappában: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

Az alábbi táblázat a fájlok és a karakterláncokat kell szerkeszteni, eredeti és módosított változata.

| **Fájl**                  | **Eredeti karakterláncot**                                                                                                                   | **Módosított karakterlánc**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.HTML | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.HTML       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.HTML |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.HTML |
| Selfservice.aspx          | RoleExchangePoint = http: / /\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint.svc, Átviteli átviteli = | RoleExchangePoint = https: / /\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint / BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Ahol:

-   *\<BHOLD_Server\>*  a BHOLD-kiszolgáló nevét adja meg a fájl az eredeti verzió-beli formában

-   *\<MIM_Server\>*  a FIM-kiszolgáló nevét adja meg a fájl az eredeti verzió-beli formában

-   *\<BHOLD_Server_FQDN\>*  megadja a BHOLD-kiszolgáló teljesen minősített tartománynevét (FQDN)

-   *\<MIM_Port\>*  határozza meg a port számát, a FIM-kiszolgáló található az a fájl eredeti verzióját

-   *\<MIM_Server_FQDN\>*  a FIM-kiszolgáló teljes Tartománynevét adja meg

-   *\<MIM_SSL_Port\>*  adja meg egy másik portot használja az SSL-lel a FIM-kiszolgálón

### <a name="enable-approval-workflows-in-bhold-core"></a>A BHOLD Core jóváhagyási munkafolyamatokat engedélyezése

Amikor a FIM és a BHOLD integrálva van az önkiszolgáló működésre, a jóváhagyási munkafolyamatokat a FIM szolgáltatásban futnak. Ez hasonlít a FIM-portálon, például amikor egy felhasználó csatlakozhat egy terjesztési lista kérést küld a kérelmekkel munkafolyamat modelljére. BHOLD szerepkör munkafolyamatok és egyéb azonban a FIM szolgáltatásban futtatott munkafolyamatok közötti fő különbségek vannak. A BHOLD, esetén adja meg, hogy mely felhasználók jóvá kell hagynia szerepkör munkafolyamat-paraméterek származnak BHOLD ahelyett, hogy a munkafolyamat-meghatározások a FIM szolgáltatás adatbázisban tárolja. Ezek a paraméterek számára a FIM szolgáltatás által biztosított BHOLD az első kérést, és a egy munkafolyamat kommunikál az eredményeket a BHOLD Core.

A BHOLD önkiszolgáló kérelmek jóváhagyó három módszerrel lehetőséget választja:

-   **Vonalkezelő jóváhagyójaként: egy szervezeti egység (OrgUnit) szerepkör-alapú kijelölés** Ha egy szerepkör, amely a jóváhagyó vagy Mozgólépcső roletype nevű attribútummal rendelkezik, és az adott szerepkör keretén belül egy OrgUnit egy vagy több felhasználó van csatolva, kér adott OrgUnit felhasználóit jóvá kell hagynia a felhasználók, amely kapcsolódik a szerepkört a jóváhagyó vagy Mozgólépcső roletype egyikét.

-   **Vonalkezelő jóváhagyójaként: egy OrgUnit kijelölés Attribútumalapú** minden OrgUnit rendelkezhet egy vagy több attribútum által megadott felhasználók, akik a szerepkör-hozzárendeléseit a OrgUnit többi felhasználójával jóváhagyhatja az aliasokat. Ezek az attribútumok lesznek elnevezve approver1 approver2 és így tovább. Amikor egy felhasználó a OrgUnit a szerepkör-hozzárendelés, BHOLD a kérelmet (FIM) irányítja a felhasználókat a OrgUnit jóváhagyó attribútum által megadott. Ha egy OrgUnit rendelkezik ezen attribútumok beállítása, BHOLD ellenőrzi a legfelső szintű OrgUnit OrgUnits szülő.

-   **Správce rolí jóváhagyójaként: Attribútumalapú kiválasztása a szerepkörhöz** szerepkör rendelkezhet egy vagy több attribútum (is elnevezett approver1, és így tovább), amely meghatározza az aliasokat, felhasználók, akik hagyhatja jóvá a szerepkör-hozzárendelés. Amikor egy felhasználó ezen jóváhagyó attribútummal rendelkező szerepkör hozzárendelését, BHOLD irányítja a kérést a felhasználók által az attribútumok meghatározott.

Önkiszolgáló szerepkörrel kérelmek jóváhagyó nem adott meg az alábbi módszerek egyikét, ha alapértelmezés szerint a BHOLD automatikusan hozzárendeli a szerepkört jóváhagyás kérése nélkül. Ebből kifolyólag után azonnal telepíti a BHOLD FIM-integráció, konfigurálnia kell a legfelső szintű OrgUnit az aliasszal egy jóváhagyó, például a rendszergazdai fiók. Ez megakadályozza, hogy egy felhasználó véletlenül engedély megadása a szerepkör egy átfogóbb jóváhagyási szabályzata előtt.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>A legfelső szintű OrgUnit jóváhagyó konfigurálása

1.  Jelentkezzen be a BHOLD Core kiszolgálóra rendszergazdaként.

2.  Kattintson a **Start**, és kattintson a **az Internet Explorer**.

3.  Az Internet Explorer címsorába írja be a <http://localhost:5151/bhold/core>, és nyomja le az Enter billentyűt.

4.  A BHOLD Core kezdőlapja, alatt **attribútum def**, kattintson a **attribútum típusú**.

5.  Az a **attribútum típusa** kattintson **Hozzáadás**.

6.  Az a **hozzáadása attribútum típusa lap**, a **identitás**, approver1, írja be a **adattípus** listában, kattintson **alfanumerikus**, a **Maximális hossz**, 255, írja be **értékekből álló listát**, kattintson a **nem**, a **angol**, írja be a jóváhagyó 1, kattintson a **OK**, és kattintson a **kész**.

7.  A bal oldali panelen a **attribútum def** kattintson **attribútum típusú csoportok**.

8.  Az a **attribútum típusú csoportok** kattintson **Hozzáadás**.

9.  Az a **attribútumkészletet típus hozzáadása** lap **leírás**, írja be a OrgUnit attribútumok, és kattintson **OK**.

10. Az a **OrgUnit attribútumok** lapon, bontsa ki a **attribútum típusú**, és kattintson a **módosítás**.

11. Az a **attribútum típusa** listában, kattintson **approver1**, kattintson a **Hozzáadás**, és kattintson a **kész**.

12. A bal oldali ablaktáblán kattintson a **objektumtípusok**.

13. Az a **objektumtípusok** kattintson **OrgUnit**.

14. Az a **objektum típusa/OrgUnit** lapon, bontsa ki a **attribútum típusú csoportok**, és kattintson a **módosítás**.

15. Az a **attribútum típusának beállítása/OrgUnit hivatkozás** lap **sorrend**, 10, írja be a **attribútum típusa beállított** listában, kattintson **OrgUnit attribútumok**, Kattintson a **Hozzáadás**, és kattintson a **kész**.

16. A bal oldali panelen a **modell**, kattintson a **szervezeti egységek**.

17. Az a **szervezeti egységek** kattintson **legfelső szintű**.

18. Az a **szervezeti egység/root** kattintson **módosítás**.

19. Az a **módosítása szervezeti egység attribútumok/root** lap **jóváhagyó**, írja be a tartomány és a felhasználó nevét, a felhasználó, aki jóvá fogja hagyni a szerepkör-hozzárendelési kérelmek, a következő formátumban  *\<tartomány\>*\\*\<felhasználói\>*, ahol *\<tartomány\>* van a (Rövid) NetBIOS-tartománynév és *\<felhasználói\>* a felhasználó bejelentkezési neve.
20. Kattintson az **OK** gombra.

> [!IMPORTANT]
> A tartomány és a felhasználó nevének egyeznie kell az alapértelmezett alias a felhasználó a BHOLD Core adatbázisban.

Alternatív megoldásként adja meg a szervezeti egységek hagyhatja jóvá javasolt szerepkörök hagyhatja jóvá a BHOLD Core adatbázisban is megadhat. Ehhez a approver1 attribútum létrehozása, adja hozzá, hogy egy attribútum indexben az szerepkör objektumtípushoz társított és majd a adja meg a jóváhagyó, minden egyes javasolt szerepkör módosítása.

Munkafolyamat nagyobb biztonság, mellett a jóváhagyók, akkor ki kell jelölnie további jóváhagyást módok és a felhasználók létrehozása és feltöltése a következő attribútumok OrgUnits és szerepkörök:

- Mozgólépcső<em>\<n\></em>

- tulajdonos<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- értesítési<em>\<n\></em>

ahol *\<n\>* azt jelzi, hogy egy nem kötelező numerikus utótagból több, azonos típusú attribútumainak megadásához.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Ellenőrizze a jóváhagyási munkafolyamatokat a FIM szolgáltatás konfigurálása

BHOLD FIM Integration telepítése hoz létre, csoportok, a munkafolyamat-definíciókhoz és a felügyeleti házirendszabályok (MPR-EK) a FIM szolgáltatáshoz. Ha testreszabta volna a FIM környezet módosítása a Rendszergazdák csoportok vagy felhasználók, akik kéréseiket a készletek, győződjön meg róla, hogy az MPR-EK hivatkoznak-e a megfelelő felhasználói csoportokat.

> [!NOTE]
> A FIM-portál felhasználók használhatják az önkiszolgáló funkciók BHOLD által biztosított, mielőtt a felhasználói fiókok szinkronizálni kell a FIM szinkronizálási szolgáltatás a BHOLD-adatbázisba. Különösen kell lennie a BHOLD Core és az összes felhasználója számára önkiszolgáló kérheti, vagy egy jóváhagyó vagy önkiszolgáló kérelmek Mozgólépcső van megadva a FIM szolgáltatás adatbázisához egy felhasználórekordban tárolja.

## <a name="next-steps"></a>További lépések

- FIM-portál és más FIM szolgáltatások telepítésével kapcsolatos információkért lásd: [tervezés és architektúra](https://technet.microsoft.com/library/ee808044.aspx) a Microsoft Forefront technikai könyvtárban.
- [A BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

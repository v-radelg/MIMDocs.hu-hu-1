---
title: BHOLD FIM/a webszolgáltatások integrációjának telepítése | Microsoft Docs
description: BHOLD integrációs modul – önkiszolgáló szerepkör-kezelés hozzáadása a következőhöz és a FIM-hez
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3005e06606ec4b3b6854003213c712770376b35d
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042202"
---
# <a name="bhold-fimmim-integration-installation"></a>BHOLD FIM/a webalkalmazás-integráció telepítése

A BHOLD FIM integrációs modulja hozzáadja az önkiszolgáló szerepkör-kezelést a Microsoft Identity Managerhez, így a felhasználók további szerepköröket igényelhetnek, és kikényszerítik, hogy kik hajtják végre ezeket a szerepköröket. A BHOLD FIM integrációs modulja kibővíti a FIM-portált, így a teljes FIM-felügyelet részeként könnyedén kezelheti a felhasználói szerepköröket. Ez a témakör azt ismerteti, hogyan kell konfigurálnia a hálózati infrastruktúrát, hogy lehetővé tegye a BHOLD FIM integrációs modul telepítését és használatát. Azt is ismerteti, hogyan telepítheti a BHOLD FIM integrációs modulját és konfigurációját, amely a BHOLD FIM integrációs modul telepítése után szükséges.

## <a name="bhold-fim-integration-installation-requirements"></a>A BHOLD FIM-integráció telepítési követelményei

A BHOLD FIM integrációs modulja kiterjeszti a FIM-portált és a FIM szolgáltatást, így a felhasználók a FIM Portalon kezelhetik a szerepköreiket. Ezért fontos, hogy a BHOLD FIM integrációs modul telepítése előtt telepítse és konfigurálja a BHOLD Core modult és a szükséges FIM-funkciókat.
A BHOLD FIM integrációs moduljának telepítése előtt a következő szoftver-összetevőknek kell szerepelniük a számítógépen:

- Microsoft Identity Manager 2016-portál és-szolgáltatás
- Microsoft Silverlight 3 vagy újabb
- Internet Information Services és ASP.NET
- Microsoft Silverlight-eszközök

Emellett a BHOLD Core-és hozzáférés-kezelési összekötő moduljainak már telepítve kell lennie a környezetben található kiszolgálón, és a FIM-t egy vagy több BHOLD-felügyeleti ügynökkel kell konfigurálni. Az BHOLD Core modul telepítésével és konfigurálásával kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). További információ a hozzáférés-kezelési összekötő modul telepítéséről és használatáról: a [hozzáférés-vezérlési összekötő telepítése](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) és a [tesztelési labor útmutatója: BHOLD Access Management Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> A FIM szolgáltatás-adatbázis nevének FIMService kell lennie. A BHOLD FIM-integráció telepítése sikertelen lesz, ha a FIM nincs telepítve az alapértelmezett FIM szolgáltatás-adatbázis nevével.

## <a name="before-you-begin"></a>Előkészületek

Mielőtt megkezdené a BHOLD FIM integrációs modul telepítését, létre kell hoznia egy BHOLD könyvtárat a C: lemezmeghajtó (C:\BHOLD) gyökérkönyvtárában.

Emellett elő kell készítenie azokat az információkat, amelyeket a BHOLD FIM Integration telepítővarázsló a telepítés befejezéséhez szükséges. A következő munkalapon rögzítheti ezeket az adatokat, így készen áll arra, hogy szükség esetén megadja azt.

### <a name="bholdfim-account-settings"></a>BHOLDFim-fiók beállításai

| **Elem**                            | **Leírás**                                                                                                                                                                                                               | **Érték**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Biztonsági szolgáltató használata a tartományban** | Ha be van jelölve, akkor a Active Directory tartományi szolgáltatások biztonság szabályozza a BHOLD mag elérését.                                                                                                                    | Jelölje be a jelölőnégyzetet. **Fontos:** Ha ez a jelölőnégyzet nincs bejelölve, a telepítés sikertelen lesz.                                                                                                                                                                                                                   |
| **Tartományi**                          | A BHOLD Core telepítésekor létrehozott **szolgáltatásfiókot** tartalmazó tartományt adja meg. További információ: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló automatikusan megadja. A név csak akkor módosítható, ha helytelen. **Fontos:** A tartománynevet a NetBIOS (rövid) név használatával adja meg, ne a teljes tartománynevet (FQDN). Ha például a tartomány teljes tartományneve fabrikam.com, adja meg a tartománynevet FABRIKAM néven. |
| **Username**                        | Megadja a BHOLD Core szolgáltatás felhasználói fiókjának bejelentkezési nevét.                                                                                                                                                              | Írja be a felhasználói fiók nevét:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                        | Megadja a szolgáltatás felhasználói fiókjának jelszavát.                                                                                                                                                                           | Írja be a jelszót itt: **Fontos:** ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM szolgáltatás beállításai

| **Elem**            | **Leírás**                                                                                                                                                                                                                               | **Érték**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Felhasználó**            | Megadja egy olyan fiók bejelentkezési nevét, amely rendszergazdai jogosultságokkal rendelkezik a FIM-hez. A Microsoft nyomatékosan javasolja, hogy ne használja a root felhasználóhoz társított fiókot a BHOLD Core-ban (alapértelmezés szerint az BHOLD Core telepítéséhez használt fiók). | Írja be a felhasználói fiók nevét:                                                                   |
| **Jelszó**        | Megadja a FIM-rendszergazda felhasználói fiók jelszavát.                                                                                                                                                                                 | Írja be a jelszót itt: **Fontos:** ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja. |
| **FIM-adatbázis**    | Megadja a FIM szolgáltatás adatbázisának nevét.                                                                                                                                                                                               | FIMService                                                                                          |
| **Webhely IP-címe/port** | Megadja a FIM-portál kiszolgálójának nevét vagy IP-címét, valamint a webhely portját.                                                                                                                                                               | Itt írhatja be a kiszolgáló nevét vagy a portot:                                                     |

### <a name="bhold-core-connection"></a>BHOLD Core-kapcsolatok

| **Elem**               | **Leírás**                                                                                                                                                                                                                                                                                                                                                                               | **Érték**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Tartományi**             | Megadja a **felhasználó**által megadott fiók tartományának nevét. A tartományt a NetBIOS (rövid) formátumban kell megadni.                                                                                                                                                                                                                                                                   | Itt írhatja a felhasználói fiók tartománynevét?                                                            |
| **Felhasználó**               | Meghatározza **egy BHOLD-felhasználó** fiókjának bejelentkezési nevét, amely az összes felhasználó és szerepkör felettese, és jogosult a felhasználói szerepkörök összekapcsolására és leválasztására. A Microsoft nyomatékosan javasolja, hogy ne használja a root felhasználóhoz társított fiókot a BHOLD Core-ban (alapértelmezés szerint az BHOLD Core telepítéséhez használt fiók). Ez a fiók lehet ugyanaz a fiók, amelyet a FIM-hez való kapcsolódáshoz használ. | Írja be a felhasználói fiók nevét:                                                                   |
| **Jelszó**           | Megadja a **felhasználó**által megadott felhasználói fiók jelszavát.                                                                                                                                                                                                                                                                                                                             | Írja be a jelszót itt: **Fontos:** ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja. |
| **IP-cím/számítógép címe** | Megadja az BHOLD Core webhely kiszolgálójának IP-címét. A kiszolgáló nevét ne használja.                                                                                                                                                                                                                                                                                                        | Írja be az IP-címet itt:                                                                          |
| **Portszám**        | A BHOLD Core webhely portszámát adja meg.                                                                                                                                                                                                                                                                                                                                          | Írja be ide a portszámot:                                                                         |

## <a name="bhold-fim-integration-setup"></a>BHOLD FIM-integráció beállítása

A BHOLD FIM integrációs moduljának telepítéséhez jelentkezzen be a Tartománygazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként azon a kiszolgálón, amelyre telepíteni kívánja a BHOLD FIM integrációs modulját:

- BholdFIMIntegration<em>\<verzió\></em>\_. msi

Cserélje * \<le\> a verziót* a telepítendő BHOLD FIM integrációs kiadás verziószámára.

A programfájl rendszergazdaként való futtatásához kattintson a jobb gombbal a fájlra, majd kattintson a **Futtatás rendszergazdaként**parancsra.

![MSI futtatása](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Telepítés utáni feladatok

Miután telepítette a BHOLD FIM-integrációt, konfigurálnia kell a Microsoft SharePointot, hogy megadja a BHOLD-szolgáltatásfiók tulajdonosi engedélyeit. Továbbá, ha a FIM-portál SSL (SSL) biztonság használatára van konfigurálva, módosítania kell a FIM Portalhoz hozzáadott BHOLD-lapokra mutató hivatkozásokat tartalmazó fájlokat.

### <a name="configuring-sharepoint"></a>A SharePoint konfigurálása

Ahhoz, hogy megfelelően működjön, a BHOLD FIM-integráció megköveteli, hogy a BHOLD-szolgáltatásfiók a Microsoft SharePointban a hely-tag engedélyekkel rendelkezzen.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Hely – tag engedélyek megadása a BHOLD-szolgáltatásfiók számára

1.  Jelentkezzen be a BHOLD FIM-integrációt futtató kiszolgálóra rendszergazdai jogosultságokkal.

2.  Kattintson a **Start**gombra, majd az **internetes Exporer**elemre.

3.  A címsorba írja be <https://localhost> , hogy a SharePoint az SSL-biztonság használatára van-e <http://localhost>konfigurálva, máskülönben írja be a következőt:.

4.  A **csapat helye** lap bal oldalán kattintson a **személyek és csoportok**elemre.

5.  A **csoportok** területen kattintson a **csapattagok**elemre, majd a középső ablaktábla eszköztárán kattintson az **új**, majd a **felhasználók hozzáadása**elemre.

6.  A felhasználók **hozzáadása: csoportwebhely** oldal **felhasználók/csoportok**területén írja be a BHOLDApplicationGroup nevet, majd kattintson a Névellenőrzés gombra a **felhasználók/csoportok** mezőben. A csoport nevét fel kell oldani, hogy tartalmazza a tartománynevet.

7.  Kattintson a **felhasználók engedélyeinek közvetlen megadása**lehetőségre, válassza a **teljes hozzáférés lehetőséget – teljes hozzáférés**lehetőséggel, majd kattintson **az OK**gombra.

8.  Ellenőrizze, hogy a BHOLDApplicationGroup szerepel-e az **engedélyek: csoportwebhely**területen, majd kattintson az Internet Explorer elemre.

![MSI futtatása](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>A BHOLD konfigurálása az SSL támogatásához

Ha a FIM-portál SSL-biztonság használatára van konfigurálva, akkor módosítania kell a fájlokat a FIM-kiszolgálón, hogy a BHOLD-lapokra mutató hivatkozások meg legyenek nyitva. A fájlok a következő mappában találhatók: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

A következő táblázat a szerkeszteni kívánt karakterláncok fájljait és eredeti és módosított verzióit sorolja fel.

| **Fájl**                  | **Eredeti sztring**                                                                                                                   | **Módosított karakterlánc**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analitika. aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns. aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | 
| AttestationMain. aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx? hideMenu = 1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx? hideMenu = 1 |
| Jelentéskészítés. aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice. aspx          | RoleExchangePoint = http://\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. SVC, TransportMode = Transport | RoleExchangePoint = https://\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. SVC, TransportMode = Transport |

Az elemek magyarázata:

-   BHOLD_Server a fájl eredeti verziójában található BHOLD-kiszolgáló nevét adja meg. * \<\> *

-   MIM_Server a fájl eredeti verziójában található FIM-kiszolgáló nevét adja meg * \<\> *

-   BHOLD_Server_FQDN megadja a BHOLD-kiszolgáló teljes tartománynevét (FQDN). * \<\> *

-   MIM_Port a fájl eredeti verziójában található FIM-kiszolgáló portszámát adja meg. * \<\> *

-   MIM_Server_FQDN a FIM-kiszolgáló teljes tartománynevét adja meg. * \<\> *

-   MIM_SSL_Port egy másik portot ad meg a FIM-kiszolgálón található SSL használatával * \<\> *

### <a name="enable-approval-workflows-in-bhold-core"></a>Jóváhagyási munkafolyamatok engedélyezése a BHOLD Core-ban

Ha a FIM és a BHOLD integrálva van az önkiszolgáló szolgáltatásba, a jóváhagyásokhoz tartozó munkafolyamatok a FIM szolgáltatásban futnak. Ez hasonló a FIM-portálon alapuló kérések munkafolyamat-modelljéhez, például amikor egy felhasználó kérelmet küld egy terjesztési listához való csatlakozásra. A BHOLD szerepkör-munkafolyamatok és a FIM szolgáltatásban üzemeltetett egyéb munkafolyamatok között fontos különbségek vannak. A BHOLD esetében a munkafolyamat-paraméterek határozzák meg, hogy mely felhasználóknak kell jóváhagyni egy szerepkör-kérést a BHOLD-ből, nem pedig a FIM szolgáltatás adatbázisának munkafolyamat-definíciójában. Ezeket a paramétereket a FIM szolgáltatás BHOLD az első kérelem elvégzése után, a műveleti munkafolyamat pedig visszaküldi az eredményeket a BHOLD Core-nak.

A BHOLD a következő három módszer egyikével választ ki egy önkiszolgáló kérelem jóváhagyóját:

-   **Line Manager mint jóváhagyó: szervezeti egység (OrgUnit) szerepkörön alapuló kiválasztása** Ha egy szerepkör egy roletype nevű attribútummal rendelkezik, amely jóváhagyóként vagy Mozgólépcsőként van beállítva, és ha az adott szerepkör egy vagy több felhasználóhoz van társítva egy OrgUnit környezetben, a OrgUnit lévő felhasználóktól érkező kéréseket jóvá kell hagyni a jóváhagyó vagy a mozgólépcső roletype.

-   **Line Manager mint jóváhagyó: attribútum-alapú kijelölés egy OrgUnit** Minden OrgUnit rendelkezhet egy vagy több olyan attribútummal, amely meghatározza azokat a felhasználókat, akik a OrgUnit más felhasználói számára is jóváhagyják a szerepkör-hozzárendeléseket. Ezek az attribútumok neve approver1, approver2 stb. Ha a OrgUnit egyik felhasználója szerepkör-hozzárendelést kér, a BHOLD a kérést (FIM-n keresztül) átirányítja a OrgUnit-jóváhagyó attribútumai által megadott felhasználókra. Ha egy OrgUnit nem rendelkezik ezekkel az attribútumokkal, a BHOLD ellenőrzi a szülő OrgUnits a gyökér OrgUnit.

-   **Szerepkör-kezelő mint jóváhagyó: attribútum-alapú kijelölés egy szerepkörhöz** Egy szerepkörhöz tartozhat egy vagy több attribútum (más néven approver1 stb.), amelyek meghatározzák a szerepkör hozzárendelését jóváhagyó felhasználók aliasneveit. Amikor egy felhasználó hozzárendel egy olyan szerepkört, amely ezen jóváhagyó attribútumokat beállította, a BHOLD a kérést az attribútumok által megadott felhasználóknak irányítja.

Ha az önkiszolgáló szerepkör-kérelemhez tartozó jóváhagyót nem az egyik módszer adja meg, a BHOLD alapértelmezés szerint automatikusan hozzárendeli a szerepkört a jóváhagyás megkövetelése nélkül. Emiatt a BHOLD FIM-integráció telepítése után azonnal konfigurálnia kell a legfelső szintű OrgUnit egy jóváhagyó aliasával, például a root fiókkal. Ez megakadályozza, hogy a felhasználó akaratlanul is megkapja a szerepkört, mielőtt átfogóbb jóváhagyási házirendet lehetne megvalósítani.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>A legfelső szintű OrgUnit jóváhagyójának konfigurálása

1.  Jelentkezzen be a BHOLD Core kiszolgálóra rendszergazdaként.

2.  Kattintson a **Start**gombra, majd az **Internet Explorer**elemre.

3.  Az Internet Explorer címsorában írja be a <http://localhost:5151/bhold/core>következőt:, majd nyomja le az ENTER billentyűt.

4.  Az BHOLD alapoldalán, a **def attribútum**alatt kattintson az **attribútumok típusai**elemre.

5.  Az **attribútum típusa** lapon kattintson a **Hozzáadás**gombra.

6.  Az **attribútum típusának hozzáadása oldalon**az **identitás**mezőbe írja be a approver1, az **adattípus** listában kattintson az **alfanumerikus**elemre, a **maximális hossz**mezőbe írja be a 255 értéket, az **értékek listájában**kattintson a **nem**, **angol**nyelven, írja be a jóváhagyó 1, kattintson **az OK**gombra, majd kattintson a **kész**gombra.

7.  A bal oldali ablaktáblán, az **attribútum felbontása** területen kattintson az **attribútum típusa készletek**elemre.

8.  Az **attribútum típusú készletek** lapon kattintson a **Hozzáadás**gombra.

9.  Az **attribútum típusának hozzáadása** lapon a **Leírás**mezőbe írja be a OrgUnit attribútumokat, majd kattintson **az OK**gombra.

10. A **OrgUnit attribútumai** lapon bontsa ki az **attribútumok típusai**elemet, majd kattintson a **módosítás**gombra.

11. Az **attribútum típusa** listában kattintson a **approver1**elemre, kattintson a **Hozzáadás**, majd a **kész**gombra.

12. A bal oldali ablaktáblán kattintson az **Objektumtípusok**elemre.

13. Az **Objektumtípusok** lapon kattintson a **OrgUnit**elemre.

14. Az **objektumtípus/OrgUnit** lapon bontsa ki az **attribútum típusa készletek**elemet, majd kattintson a **módosítás**gombra.

15. Az **attribútum típusának beállítása/OrgUnit** lapon a **sorrend**mezőbe írja be a 10 értéket, az **attribútum típusa készlet** listában kattintson a **OrgUnit attribútumai**elemre, majd a **Hozzáadás**, végül pedig a **kész**gombra.

16. A bal oldali ablaktábla **modell**területén kattintson a **szervezeti egységek**elemre.

17. A **szervezeti egységek** lapon kattintson a **gyökér**elemre.

18. A **szervezeti egység/gyökér** lapon kattintson a **módosítás**lehetőségre.

19. A **szervezeti egység attribútumainak/gyökerének módosítása** lapon a **jóváhagyó**mezőben adja meg annak a felhasználónak a tartományát és felhasználónevét, aki a szerepkör-hozzárendelési kéréseket jóvá fogja hagyni a * \<tartományi\>*\\*\<felhasználó\>* formátumban, ahol * \<a\> tartomány* a NetBIOS (rövid) tartománynév, a * \<felhasználó\> * pedig a felhasználó bejelentkezési neve.
20. Kattintson az **OK** gombra.

> [!IMPORTANT]
> A tartománynak és a felhasználónévnek meg kell egyeznie egy felhasználó alapértelmezett aliasával a BHOLD Core adatbázisban.

A szervezeti egységekhez tartozó jóváhagyó megadása helyett megadhat egy jóváhagyót a javasolt szerepkörökhöz a BHOLD Core adatbázisban. Ehhez hozza létre a approver1 attribútumot, adja hozzá a szerepkör videótartalmakban társított attribútumhoz, majd módosítsa az egyes javasolt szerepköröket a jóváhagyó megadásához.

A jobb munkafolyamatok biztonságának biztosítása érdekében a jóváhagyók mellett további jóváhagyási módokat és felhasználókat kell kijelölnie a következő attribútumok létrehozásával és feltöltésével a OrgUnits és a szerepkörökhöz:

- n mozgólépcső<em>\<\></em>

- <em>\<n tulajdonos\></em>

- <em>\<n securityOfficer\></em>

- értesítés<em>\<n\></em>

ahol * \<n\> * egy opcionális numerikus utótagot jelöl, amely több azonos típusú attribútumot biztosít.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>A FIM szolgáltatásban konfigurált jóváhagyási munkafolyamatok ellenőrzése

A BHOLD FIM integrációs telepítése készletek, munkafolyamat-definíciók és felügyeleti házirend-szabályok (MPR) létrehozását hozza létre a FIM szolgáltatáshoz. Ha testreszabta a FIM üzemelő példányát, hogy megváltoztassa a rendszergazdák vagy a kérelmeket kérő felhasználók készleteit, győződjön meg arról, hogy a MPR a megfelelő felhasználói készletekre hivatkozik.

> [!NOTE]
> Mielőtt a FIM Portal felhasználói használhatják a BHOLD által biztosított önkiszolgáló funkciókat, a felhasználói fiókokat szinkronizálni kell a BHOLD-adatbázisba a FIM synchronization Service-ben. Különösen a BHOLD Core adatbázisban és a FIM szolgáltatás adatbázisában kell lennie egy felhasználói rekordnak minden olyan felhasználó számára, aki önkiszolgáló kérést készít, vagy jóváhagyóként vagy mozgólépcsőként van megadva önkiszolgáló kérésekhez.

## <a name="next-steps"></a>További lépések

- További információ a FIM-portál és az egyéb FIM-funkciók telepítéséről: [tervezés és architektúra](https://technet.microsoft.com/library/ee808044.aspx) a Microsoft Forefront Technical Library-ben.
- [BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

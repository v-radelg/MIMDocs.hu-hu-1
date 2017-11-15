---
title: "BHOLD FIM vagy MIM-integráció telepítése |} Microsoft Docs"
description: "BHOLD integrációs modul MIM és a FIM önkiszolgáló szerepkör hozzáadása"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-fimmim-integration-installation"></a>BHOLD FIM vagy MIM-integráció telepítése

A BHOLD FIM integrációs modul hozzáadása önkiszolgáló szerepkörkezelés való Microsof Identity Manager, a további szerepkörök igénylését lehetővé téve, és a számára ezekhez a szerepkörökhöz vehet igénybe. A BHOLD FIM integrációs modul bővíti a FIM-portál abba, hogy könnyen kezelhető a felhasználói szerepkörök átfogó FIM felügyeleti részeként. Ez a témakör ismerteti, hogyan kell konfigurálnia a hálózati infrastruktúra telepítése és használata a BHOLD FIM integrációs modul segítségével. Emellett bemutatja, hogyan telepítheti a BHOLD FIM integrációs modul és a BHOLD FIM integrációs modul telepítése után szükséges konfigurációt.

## <a name="bhold-fim-integration-installation-requirements"></a>BHOLD FIM-integráció telepítési követelmények

A BHOLD FIM integrációs modul bővíti a FIM-portál és a FIM szolgáltatás a felhasználók kezelése az FIM portálon szerepük. Ezért fontos, hogy a BHOLD Alap modulban és a szükséges FIM-szolgáltatások telepíthetők és konfigurálhatók a BHOLD FIM integrációs modul telepítése előtt.
Az alábbiakban található a számítógépen kell lennie, mielőtt telepíthetné a BHOLD FIM integrációs modul szoftverösszetevőket:

- A Microsoft Identity Manager 2016 portál és a szolgáltatás
- A Microsoft Silverlight 3-as vagy újabb
- Az Internet information Services és az ASP.NET
- A Microsoft Silverlight-eszközök

Továbbá a BHOLD Core és az Access Management-összekötő modulok már telepíthető a környezetben a kiszolgálón, és a FIM egy vagy több BHOLD felügyeleti ügynököt kell konfigurálni. A BHOLD Alap modulban konfigurálásával kapcsolatos további információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Az Access Management-összekötő modullal kapcsolatos információkért lásd: [Access Management-összekötő telepítés](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx) és [tesztlabor-Útmutató: BHOLD Access Management-összekötő](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx).

>[!IMPORTANT]
A FIM szolgáltatás adatbázisához neve FIMService kell lennie. BHOLD FIM-integráció telepítője sikertelen lesz, ha a FIM nem lett telepítve az alapértelmezett FIM szolgáltatás neve.

## <a name="before-you-begin"></a>Előkészületek

Mielőtt elkezdené a BHOLD FIM integrációs modul telepítése, a C: meghajtó (C:\BHOLD) gyökérkönyvtárában BHOLD könyvtár kell létrehoznia.

Ezenkívül kell előkészíteni a BHOLD FIM-integráció telepítője varázsló a telepítés befejezéséhez szükséges információk biztosítása érdekében. A következő munkalapra jegyezze fel ezt az információt, készen áll, hogy szükség esetén nyújt segítséget.

### <a name="bholdfim-account-settings"></a>BHOLDFim Fiókbeállítások

| **Elem**                            | **Leírás**                                                                                                                                                                                                               | **Érték**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **A biztonsági szolgáltató tartományra vonatkozó használatát** | Kiválasztásakor határozza meg, hogy az Active Directory tartományi szolgáltatások biztonsági szabályozza a BHOLD Core elérésére.                                                                                                                    | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                          | Megadja a tartomány tartalmazó a **szolgáltatásfiók** BHOLD központi telepítésekor létrehozott. További információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | A varázsló automatikusan rendelkezik a tartomány nevét. Módosítsa a nevét, ha az nem megfelelő. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-nevét, nem a teljes tartománynevét (FQDN) használatával. Például fabrikam.com esetén a a teljes Tartománynevét adja meg a tartománynév FABRIKAM. |
| **Felhasználónév**                        | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                              | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                        | A szolgáltatás felhasználói fiók jelszavát adja meg.                                                                                                                                                                           | A jelszó itt írási: **fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM-szolgáltatás beállításai

| **Elem**            | **Leírás**                                                                                                                                                                                                                               | **Érték**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Felhasználó**            | Meghatározza a bejelentkezési név annak a fióknak rendszergazdai jogosultságokkal rendelkező FIM. A Microsoft nyomatékosan javasolja, hogy nem használja a gyökér szintű felhasználó a BHOLD mag (alapértelmezés szerint a BHOLD központi telepítéséhez használt fiók) tartozó fiókot. | Írás a felhasználói fiók nevét itt:                                                                   |
| **Jelszó**        | A FIM rendszergazdai felhasználói fiók jelszavát adja meg.                                                                                                                                                                                 | A jelszó itt írási: **fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen. |
| **FIM-adatbázisba**    | Megadja a FIM szolgáltatás adatbázisához nevét.                                                                                                                                                                                               | FIMService                                                                                          |
| **IP/Webhelyportot** | Adja meg a nevét vagy IP-címe a FIM-portál kiszolgáló és a webhely portját.                                                                                                                                                               | Írás a kiszolgáló nevét vagy a cím és port itt:                                                     |

### <a name="bhold-core-connection"></a>BHOLD Core kapcsolat

| **Elem**               | **Leírás**                                                                                                                                                                                                                                                                                                                                                                               | **Érték**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Tartomány**             | A megadott fiók tartományának neve **felhasználói**, az alábbi. Adja meg a tartomány NetBIOS (rövid) formátumban.                                                                                                                                                                                                                                                                   | A felhasználói fiók tartomány nevét itt írási?                                                            |
| **Felhasználó**               | A fiók bejelentkezési neve **egy felügyelő olyan BHOLD felhasználói** összes felhasználók és szerepkörök és a hivatkozásra, és a felhasználói szerepkör szétválasztása engedéllyel rendelkezik. A Microsoft nyomatékosan javasolja, hogy nem használja a gyökér szintű felhasználó a BHOLD mag (alapértelmezés szerint a BHOLD központi telepítéséhez használt fiók) tartozó fiókot. Ez a fiók lehet ugyanazt a fiókot, amelyekkel csatlakozni a FIM | Írás a felhasználói fiók nevét itt:                                                                   |
| **Jelszó**           | A megadott felhasználói fiók jelszavát adja meg **felhasználói**.                                                                                                                                                                                                                                                                                                                             | A jelszó itt írási: **fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen. |
| **IP/gép címét** | A BHOLD Core-webhely kiszolgálójának IP-címet adja meg. Ne használja a kiszolgáló nevét.                                                                                                                                                                                                                                                                                                        | Írás az IP-cím:                                                                          |
| **Portszám**        | Adja meg a BHOLD Core webhely portszámát.                                                                                                                                                                                                                                                                                                                                          | Írja be ide a portszámot:                                                                         |

## <a name="bhold-fim-integration-setup"></a>BHOLD FIM-integráció telepítője

BHOLD FIM integrációs modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD FIM integrációs modul telepítése:

- BholdFIMIntegration*\<verzió\>*\_Release.msi

Cserélje le  *\<verzió\>*  rendelkező a telepíteni kívánt BHOLD FIM-integráció kiadás verziószáma.

A program fájlt rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

![msi fut](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Telepítés utáni feladatok

BHOLD FIM-integráció telepítése után konfigurálnia kell a Microsoft SharePoint, a BHOLD szolgáltatás fiók a webhely-tulajdonosa engedélyt adhat. Is ha a FIM-portál a Secure Sockets Layer (SSL) biztonság használatára van konfigurálva, módosítania kell a címek szerepel a FIM portálon BHOLD lapok mutató hivatkozásokat tartalmazó fájlokat.

### <a name="configuring-sharepoint"></a>A SharePoint konfigurálása

A megfelelő működéshez BHOLD FIM-integráció a helyhez tartozó jogosultságokkal rendelkezzen a Microsoft SharePoint BHOLD szolgáltatásfiók szükséges.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>A helyhez tartozó engedélyeket a BHOLD szolgáltatásfiók

1.  Jelentkezzen be rendszergazdai jogosultságokkal rendelkező BHOLD FIM-integráció futtató kiszolgálón.

2.  Kattintson a **Start**, és kattintson a **Internet Exporer**.

3.  Írja be a címsorba <https://localhost> ellenkező esetben adja meg a SharePoint biztonsági SSL használatára van beállítva, ha <http://localhost>.

4.  A bal oldalán a **csoportwebhely** kattintson **személyek és csoportok**.

5.  A **csoportok** kattintson **hely csapattagok**, és a központ-eszköztárában kattintson **új**, és kattintson a **felhasználó hozzáadása**.

6.  Az a **felhasználó hozzáadása: csoportwebhely** lap **felhasználók/csoportok**, írja be a BHOLDApplicationGroup, és kattintson a alatt Névellenőrzés gombjára a **felhasználók/csoportok** mezőbe. A csoportnév oldja fel a tartomány nevét tartalmazza.

7.  Kattintson a **felhasználók adjon engedélyeket közvetlenül**, jelölje be **teljes hozzáférés – teljes körű vezérléssel rendelkezik**, és kattintson a **OK**.

8.  Győződjön meg arról, hogy BHOLDApplicationGroup szerepel **engedélyek: csoportwebhely**, majd zárja be az Internet Explorerben.

![msi fut](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>BHOLD támogatása az SSL konfigurálása

Ha a FIM-portál SSL-biztonság használatára van konfigurálva, módosítania kell a FIM-kiszolgálón lévő fájlok, hogy BHOLD hivatkozásokat csak akkor nyílik meg. A fájlok találhatók a következő mappában: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

A következő táblázat a fájlok és az eredeti és módosított verziója a karakterláncok, szerkesztését.

| **Fájl**                  | **Eredeti karakterlánc**                                                                                                                   | **Módosított karakterlánc**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.HTML | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.HTML       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.HTML |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.HTML |
| Selfservice.aspx          | RoleExchangePoint = http: / /\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint.svc, Átviteli átviteli = | RoleExchangePoint = https: / /\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint / BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Ahol:

-   *\<BHOLD_Server\>*  határozza meg a fájl eredeti verziójában található a BHOLD-kiszolgáló nevét

-   *\<MIM_Server\>*  adja meg a fájl eredeti verziójában található a FIM-kiszolgáló nevét

-   *\<BHOLD_Server_FQDN\>*  határozza meg a BHOLD-kiszolgáló teljes tartománynevét (FQDN)

-   *\<MIM_Port\>*  adja meg a fájl eredeti verziójában található a FIM-kiszolgáló portjának száma

-   *\<MIM_Server_FQDN\>*  a FIM-kiszolgáló teljes Tartománynevét adja meg

-   *\<MIM_SSL_Port\>*  határozza meg egy másik portot használja az SSL-titkosítással a FIM-kiszolgálón

### <a name="enable-approval-workflows-in-bhold-core"></a>Jóváhagyási munkafolyamatok alapvető BHOLD engedélyezése

Amikor FIM és BHOLD integrálva vannak az Önkiszolgálás, a munkafolyamatok jóváhagyásra a FIM szolgáltatásban futnak. Ez hasonlít az FIM portálon, például amikor egy felhasználó kérést küld egy terjesztési listát csatlakozás kérelmekkel munkafolyamat mintáját. BHOLD szerepkör munkafolyamatok és az egyéb munkafolyamatok, azonban a FIM szolgáltatásban üzemeltetett közötti fő különbségek vannak. BHOLD, esetén adja meg, hogy mely felhasználók jóvá kell hagynia a szerepkör kérelem munkafolyamat-paraméterek származnak BHOLD ahelyett, hogy a munkafolyamat-definícióhoz a FIM szolgáltatás adatbázisban tárolja. Ezek a paraméterek számára a FIM szolgáltatás által biztosított BHOLD az első kérelem, és egy munkafolyamat kommunikál a eredmények BHOLD Core.

BHOLD egy önkiszolgáló kérelem jóváhagyó kiválasztja az alábbi három módszer egyikével:

-   **Vonal-kezelő jóváhagyó: egy szervezeti egységhez (OrgUnit) telepítendő szerepkör-alapú** érkező kérelmeket, jóváhagyó vagy Mozgólépcső roletype nevű attribútumot a szerepkör-e, és ez a szerepkör egy OrgUnit keretében egy vagy több felhasználó kapcsolódik, adott OrgUnit belüli felhasználók jóvá kell hagynia a felhasználók a szerepkört a jóváhagyó vagy Mozgólépcső roletype kapcsolódó egyikét.

-   **Vonal-kezelő jóváhagyó: egy OrgUnit telepítendő Attribútumalapú** minden OrgUnit lehet, hogy a felhasználók, akik jóváhagyhatja a OrgUnit más felhasználói szerepkör-hozzárendelések aliast adjon meg egy vagy több attribútum. Ezek az attribútumok neve approver1, approver2, és így tovább. Amikor egy felhasználó a OrgUnit a szerepkör-hozzárendelés kér, BHOLD továbbítja a kérelmet (FIM) a felhasználók a OrgUnit jóváhagyó attribútuma által megadott. Ha egy OrgUnit nem rendelkezik ilyennel ezen attribútumok készletét, BHOLD ellenőrzi a legfelső szintű OrgUnit OrgUnits szülő.

-   **Szerepkör-kezelő jóváhagyó: Attribútumalapú kiválasztása a szerepkörhöz** szerepkör tartalmazhat egy vagy több attribútumokat (elnevezett approver1 is, és így tovább), a felhasználók, akik a szerepkör-hozzárendelés jóváhagyhatja aliast adjon meg. Amikor egy felhasználó kéri ezek jóváhagyó attribútummal rendelkező szerepkört rendelni, BHOLD a kérést továbbítja az a attribútuma által megadott felhasználókat.

Ha a szerepkör önkiszolgáló kérelmek jóváhagyó ezen módszerek egyikével nincs megadva, alapértelmezés szerint BHOLD automatikusan hozzárendeli a szerepkör nélkül jóváhagyásra van szükség. Ezért azonnal BHOLD FIM-integráció telepítése után úgy kell konfigurálnia a legfelső szintű OrgUnit jóváhagyó, például a rendszergazdafiók a aliasú. Ez megakadályozza, hogy egy felhasználó véletlenül megkapják a szerepkör egy átfogóbb jóváhagyási házirend előtt.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>A legfelső szintű OrgUnit jóváhagyó konfigurálása

1.  Jelentkezzen be a BHOLD Core kiszolgálóra rendszergazdaként.

2.  Kattintson a **Start**, és kattintson a **Internet Explorer**.

3.  Az Internet Explorer címsorába írja be a <http://localhost:5151/bhold/core>, és nyomja le az Enter billentyűt.

4.  BHOLD legfontosabb kezdőlapját, a **attribútum def**, kattintson a **típusú attribútum**.

5.  Az a **attribútumtípus** kattintson **Hozzáadás**.

6.  Az a **hozzáadása attribútum típusa lap**, a **identitás**, approver1, írja be a **adattípus** listában, kattintson **alfanumerikus**, a **Maximális megengedett hossz**, 255, írja be **értékből álló lista**, kattintson a **nem**, a **angol**, írja be a jóváhagyó 1, kattintson a **OK**, és kattintson a **végzett**.

7.  A bal oldali ablaktáblán a **attribútum def** kattintson **attribútum típusú készletek**.

8.  A a **attribútum típusú készletek** kattintson **Hozzáadás**.

9.  Az a **attribútumkészletet típus hozzáadása** lap **leírás**, írja be a OrgUnit attribútumok, és kattintson **OK**.

10. Az a **OrgUnit attribútumok** ablaktáblában bontsa ki a **típusú attribútum**, és kattintson a **módosítás**.

11. Az a **attribútumtípus** listában, kattintson **approver1**, kattintson a **Hozzáadás**, és kattintson a **végzett**.

12. Kattintson a bal oldali ablaktáblában **objektumtípusok**.

13. Az a **objektumtípusok** kattintson **OrgUnit**.

14. A a **Object típusú/OrgUnit** ablaktáblában bontsa ki a **attribútum típusú készletek**, és kattintson a **módosítás**.

15. A a **attribútum típusának beállítása/OrgUnit hivatkozás** lap **rendelés**, 10, írja be a **típus attribútumkészletet** listában, kattintson **OrgUnit attribútumok**, Kattintson a **Hozzáadás**, és kattintson a **végzett**.

16. A bal oldali ablaktáblán a **modell**, kattintson a **szervezeti egységek**.

17. Az a **szervezeti egységek** kattintson **legfelső szintű**.

18. Az a **szervezeti egység/root** kattintson **módosítás**.

19. Az a **attribútumok/root/szervezeti egység módosítása** lap **jóváhagyó**, írja be a tartomány és a felhasználó nevét hagyja jóvá a szerepkör-hozzárendelési kérelmek formátumú felhasználó  *\<tartomány\>*\\*\<felhasználói\>*, ahol  *\<tartomány\>*  van a (Rövid) NetBIOS-tartománynév és  *\<felhasználói\>*  a felhasználó bejelentkezési neve.
20. Kattintson az **OK**gombra.

>[!IMPORTANT]
A tartomány és a felhasználó nevének egyeznie kell az alapértelmezett alias a felhasználó a BHOLD Core adatbázisban.

A szervezeti egységek jóváhagyó megadása helyett a BHOLD Core adatbázis javasolt szerepkörök jóváhagyó adhat meg. Ehhez a approver1 attribútum létrehozása, adja hozzá azt attribútuma indexben a szerepkör típusú társított, és módosítsa a javasolt szerepkörönként, a jóváhagyó megadásához.

Nagyobb biztonságot munkafolyamat, jóváhagyóknak, felül kell megadott további jóváhagyási mód és a felhasználók létrehozása és feltöltése az alábbi attribútumok OrgUnits és szerepkörök:

- Mozgólépcső*\<n\>*

- tulajdonos*\<n\>*

- securityOfficer*\<n\>*

- értesítés*\<n\>*

Ha  *\< n \>*  arra, hogy több, ugyanolyan típusú attribútum egy választható numerikus utótagból jelzi.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Ellenőrizze a FIM szolgáltatásban konfigurált jóváhagyási munkafolyamatok

BHOLD FIM-integráció telepítése hoz létre a beállítása, a munkafolyamat-definícióhoz és a felügyeleti házirendszabályok (házirendszabályok) a FIM szolgáltatáshoz. Ha a FIM telepítési rendszergazdák csoportját, vagy a különböző felhasználók számára is küld kérelmeket kellett testreszabott, biztosítania kell, hogy a házirendszabályok hivatkoznak-e a megfelelő felhasználói beállítása.

>[!NOTE]
A FIM-portál felhasználók használhatja BHOLD által nyújtott önkiszolgáló szolgáltatásokat, a felhasználói fiókok szinkronizálni kell a FIM szinkronizálási szolgáltatás BHOLD-adatbázisba. Különösen kell lennie a BHOLD Core-adatbázis és a FIM szolgáltatás adatbázisához összes felhasználója számára is önkiszolgáló kérés vagy jóváhagyó vagy Mozgólépcső önkiszolgáló kérelmeknél van megadva a felhasználói rekordban.

## <a name="next-steps"></a>További lépések

- FIM-portál és az egyéb FIM szolgáltatás telepítésével kapcsolatos információkért lásd: [tervezés és architektúra](https://technet.microsoft.com/library/ee808044.aspx) a Microsoft Forefront technikai könyvtárban.
- [BHOLD a telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

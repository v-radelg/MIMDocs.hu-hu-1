---
title: "BHOLD core telepítési |} Microsoft Docs"
description: "BHOLD suite telepítése core dokumentum"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 33fbe63528d5d7c543ae286f934654538782b4d5
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/14/2017
---
# <a name="bhold-core-installation"></a>BHOLD Core telepítés

A BHOLD Core a modul adja meg a kulcsfontosságú szolgáltatásokat BHOLD Suite a környezet. A BHOLD Alap modulban telepítve és más BHOLD Suite modulok telepítése előtt a helyi hálózaton egy kiszolgálón konfigurálni kell.

## <a name="bhold-core-installation-requirements"></a>BHOLD Core telepítési követelmények

A BHOLD Core modul a Microsoft BHOLD Suite alapját képezi. Más Microsoft BHOLD Suite modulok telepítése előtt telepítenie kell a BHOLD Alap modulban.

### <a name="bhold-core-hardware-requirements"></a>BHOLD Core hardverkövetelmények

A BHOLD Core modul a Microsoft BHOLD Suite alapját képezi. Más Microsoft BHOLD Suite modulok telepítése előtt telepítenie kell a BHOLD Alap modulban.

|          |        |          |
|----------|--------|----------|
|**Az összetevő** |**Minimális** | **Ajánlott** |
|Processzor | 64 bites processzor | Multicore 64 bites processzor |
| Memória |3 GB | Legalább 6 GB |
|Tárterület| 30 GB-ig érhető el |Központi telepítés méretétől függ |
|Hálózati adapter| Az SQL és a Forefront Identity Manager (FIM) kiszolgálóval létesített kapcsolat 100 MB/s | 1 GB/s kapcsolat SQL és a FIM-kiszolgálóhoz|

Ezek a javaslatok tipikus megvalósítások alapulnak, és nem veszi figyelembe a kiszolgálón futó egyéb alkalmazások. Előfordulhat, hogy szeretné használni, nagyobb teljesítményű összetevők attól függően, hogy az adott környezetben.

### <a name="bhold-core-software-requirements"></a>BHOLD Core szoftverkövetelmények

A BHOLD Alap modulban csak telepíthető olyan számítógépre, amely megfelel az alábbi követelményeknek:

- A kiszolgálón futnia kell a Windows Server 2012 (64 bites), Windows Server 2016 
- A kiszolgáló, egy Active Directory tartományi szolgáltatások (AD DS) tartomány tagjának kell lennie. Tesztkörülmények között a kiszolgáló lehet egy Active Directory tartományi szolgáltatások tartományvezérlő.
- BHOLD Core telepítéséhez egy fiókot, a kiszolgáló ugyanabban a tartományban, és amelyhez tartozik, a tartományban a Tartománygazdák csoportnak, és a rendszergazdák csoportba a kiszolgálón a bejelentkezett felhasználó.
- Microsoft Internet Information Services (IIS) ASP.NET telepítenie kell a kiszolgálón. Az IIS Windows-hitelesítést kell konfigurálni. BHOLD Core a Windows Server 2012 2016. december telepíti, ha az IIS 6 kompatibilitási üzemmód telepítenie kell. Ha BHOLD Core telepíti a Windows Server 2012, telepítenie kell az IIS 6 parancsfájlkezelő eszközök
- Telepíteni kell a .NET 3.5 keretrendszert.
  - BHOLD Core .NET 3.5-ös verzióját igényli, mert az nem telepíthető Server Core.
- A Silverlight 4 más BHOLD modulok által igényelt, és ezért ajánlott telepíteni a BHOLD központi telepítése előtt.
- Microsoft SQL Server 2014 vagy Microsoft SQL Server 2016 telepítenie kell a BHOLD Core kiszolgálón vagy a helyi hálózat egy másik kiszolgálón. 

Windows-ügyfélszámítógépek futnia kell a Microsoft Internet Explorer 6-os vagy újabb és Microsoft Silverlight verziója 4 vagy újabb verzió.

## <a name="before-you-begin"></a>Előkészületek

A BHOLD Alap modulban hitelesítését és engedélyezését a kiszolgáló és az egyéb hálózati entitások BHOLD alapszolgáltatás szolgáló felhasználói fiókot igényel. Ez a szakasz ismerteti, hogyan hozza létre és konfigurálja a felhasználói fiókhoz, és könnyebben BHOLD központi telepítés befejezéséhez szükséges információk összegyűjtése előtelepítési munkalap biztosít.

>{! FONTOS} BHOLD központi telepítésekor, BHOLD Core-adatbázisként is használhatja a meglévő SQL Server adatbázis, vagy engedélyezheti a BHOLD központi telepítése varázsló a BHOLD Core-adatbázis létrehozásához. Ha meglévő adatbázis használatát választja, győződjön meg róla, hogy más felhasználó hozzáférhessen az adatbázishoz, BHOLD központi telepítése. BHOLD központi telepítése előtt ellenőrizze a, hogy a hozzáférés-vezérlést az SQL Server-adatbázis nem teszik lehetővé csak a BHOLD Core telepítő felhasználó hozzáférését.

## <a name="required-user-and-group"></a>Szükséges felhasználók és csoportok

A BHOLD Alap modulban tud bejelentkezni a tartományba, külön erre a célra, és amely tagja a két meghatározott biztonsági csoportok, amelyek közül az egyik kifejezetten a BHOLD Alap modulban létrehozott felhasználói fiókkal kell lennie. A tartományi rendszergazdák csoport tagjának kell lennie a művelet végrehajtásához szükséges.
**Hozzon létre és BHOLD Core felhasználók és biztonsági csoport konfigurálása**

1.  A tartományvezérlőn kattintson **Start**, mutasson a **felügyeleti eszközök**, és kattintson a **Active Directory – felhasználók és számítógépek**.

2.  A konzolfán bontsa ki a tartományt, amelyben a fiók hozható létre, kattintson a jobb gombbal **felhasználók**, mutasson a **új**, és kattintson a **csoport**.

3.  Az a **új objektum – csoport** párbeszédpanel **csoportnév**, írja be a csoport nevét (BHOLD alapértelmezett: BHOLDApplicationGroup), és kattintson a **OK**.

4.  Kattintson a jobb gombbal **felhasználók**, mutasson a **új**, és kattintson a **felhasználói**.

5.  A **teljes nevét**, adjon meg egy nevet, amely segít azonosítani a fiókot, például BHOLD Core szolgáltatásfiók.

6.  A **bejelentkezési név**, írja be a felhasználónevet a BHOLD Core szolgáltatásfiók (BHOLD alapértelmezett: b1user), és kattintson a **következő**.

7.  A **jelszó** és **jelszó megerősítése**, írja be a fiók jelszavát.

8.  Törölje a jelet **kell változtatni a jelszót a következő bejelentkezéskor**, jelölje be **felhasználó nem módosíthatja a jelszót** és **jelszó sohasem jár le**, kattintson a **következő**, majd **Befejezés**.

9.  A konzol ablaktáblában kattintson a jobb gombbal a felhasználói fiókot, és kattintson **hozzáadása csoporthoz**.

10. Az a **csoportok kiválasztása** párbeszédpanelen írja be a korábban létrehozott csoport a megjelenítendő nevet, írja be egy pontosvesszővel (;), és írja be a IIS_IUSRS.

11. Kattintson a **Névellenőrzés**, és kattintson a **OK**.  

Az alábbi eljárás azon a számítógépen, ahol a BHOLD Alap modulban telepíteni kell elvégezni. A művelet végrehajtásához a Tartománygazdák csoport tagjaként kell bejelentkeznie.

## <a name="bhold-core-installation-worksheet"></a>BHOLD Core telepítési munkalap

BHOLD Core moduljának telepítése előtt kell készüljön fel a BHOLD Core telepítővarázslója a telepítés befejezéséhez szükséges információk. A következő munkalapra jegyezze fel ezt az információt, készen áll, hogy szükség esetén nyújt segítséget. Az egyes szakaszokon megfelel-e a BHOLD központi telepítése varázslóban lapra.

### <a name="account-settings"></a>Fiókbeállítások

| **Elem**                                    | **Leírás**                                                                                                                                                                                                                                                                                             | **Érték**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztásakor határozza meg, hogy az Active Directory tartományi szolgáltatások biztonsági szabályozza a BHOLD Core elérésére.                                                                                                                                                                                                  | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                 |
| **Tartomány**                                  | BHOLD server, a szolgáltatásfiók és az alkalmazáscsoport tartalmazó tartomány megadása **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-nevét, nem a teljes tartománynevét (FQDN) használatával. Például fabrikam.com esetén a a teljes Tartománynevét adja meg a tartomány neve CONTOSO. | A tartománynév írni:                                                                                                                                        |
| **Alkalmazáscsoport**                       | A korábban létrehozott biztonsági csoport neve [szükséges felhasználók és csoportok](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Írás a csoportnév:                                                                                                                                         |
| **Szolgáltatás felhasználói**                            | A szolgáltatás a korábban létrehozott felhasználói fiók bejelentkezési neve [szükséges felhasználók és csoportok](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Írás a felhasználói fiók nevét itt:                                                                                                                                  |
| **Jelszó**                                | Megadja a BHOLD Core felhasználói fiókjának jelszavát.                                                                                                                                                                                                                                              | A jelszó itt írási: **fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen.                                                                |
| **IP/Webhelyportot**                         | Adja meg az IP-cím és port számát a webhely az intraneten kiszolgálón kell létrehozni. Módosítsa az alapértelmezett értéket (\*) csak akkor, ha az alapértelmezett webhely, nem fogja használni az azonos IP-cím. Módosítsa a port számát egy tetszőleges szabad portot csak akkor, ha az alapértelmezett port (5151) már használatban van.             | Ha egy nem alapértelmezett IP-címet használja az alapértelmezett webhely, jegyezze itt: Ha az alapértelmezett portszám már használatban van, írja be a BHOLD webhely portszámot itt: |

### <a name="database-settings"></a>Adatbázis-beállítások

| **Elem**                                       | **Leírás**                                                                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beépített biztonság használatára**                    | Meghatározza, hogy az adatbázis eléréséhez használt-e a Windows-hitelesítést.                                                                                                                                                                                                     | Jelölje be a jelölőnégyzetet, ha Windows-hitelesítést használ az SQL-kiszolgálóhoz való csatlakozáshoz. Törölje a jelet a jelölőnégyzetből, ha az SQL Server-hitelesítés használata. Az adatbázist kell létrehozni futtató BHOLD Core telepítés Ha SQL Server-hitelesítés használata előtt. **Megjegyzés:** Windows-hitelesítés használata esetén meg kell bejelentkeznie egy olyan fiókkal, amely a sysadmin (rendszergazda) kiszolgálói szerepkörrel rendelkezik az adatbázis-kiszolgálón. |
| **Adatbázis-felhasználót** és **adatbázis-jelszó** | A felhasználónevet és egy felhasználó jelszavát adja meg a sysadmin (rendszergazda) kiszolgálói szerepkör az adatbázis-kiszolgálón. Ezek az értékek megadva, csak az SQL Server-hitelesítés használata esetén.                                                                                               | A SQL Server felhasználói nevet itt: írása az SQL Server felhasználói jelszó itt: **Megjegyzés:** mindenképp ezt a jelszót rejtett, biztonságos helyen.                                                                                                                                                                                                                                                  |
| **Adatbázis-kiszolgáló** és **adatbázis neve**   | Megadja az adatbázis-kiszolgáló NetBIOS-nevét és az adatbázis nevét (alapértelmezett: b1) BHOLD az alapvető telepítés hoz létre. Ha nem használ az alapértelmezett adatbázis-kiszolgálópéldányra, adja meg az adatbázis-kiszolgálópéldányra formájában * \<server\>*\\*\<példány\> *. | A kiszolgáló (vagy a kiszolgáló és példány) neve itt írási: írni az adatbázisnevet:                                                                                                                                                                                                                                                                                                                   |
| **Az adatbázis-felhasználó korlátozások**    | Elavult.                                                                                                                                                                                                                                                                 | Ne változtassa meg az alapértelmezett érték                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD az alapvető telepítés

BHOLD Core-modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD Core modul telepítése: 

- BholdCore * \<verzió\>*\_Release.msi

Cserélje le * \<verzió\> * rendelkező a telepíteni kívánt BHOLD Core kiadás verziószáma.

A program fájlt rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

### <a name="postinstallation-settings"></a>Teendőkkel beállításai

BHOLD központi telepítés befejezése után kell konfigurálnia a Windows tűzfal és az Internet Information Services BHOLD Core konfigurálása BHOLD Core alkalmazáskészlet speciális beállításainak módosítását. Ha szükséges, az igényeinek megfelelő BHOLD rendszerattribútumok is módosítania kell.

#### <a name="configuring-windows-firewall"></a>A Windows tűzfal konfigurálása

Ha a felhasználók BHOLD webböngésző segítségével távoli számítógépeken, konfigurálnia kell a Windows tűzfal engedélyezi a bejövő kapcsolatokat a webhely-BHOLD Core telepítés során megadott BHOLD Core-kiszolgálón.

A művelet végrehajtásához a helyi számítógépen a Rendszergazdák csoport tagjának kell lennie.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>A BHOLD webhelyre bejövő kapcsolatok engedélyezéséhez

1.  Kattintson a **Start**, mutasson a **felügyeleti eszközök**, kattintson a jobb gombbal **Windows tűzfal speciális beállításokkal**, és kattintson a **Futtatás rendszergazdaként**.

2.  A bal oldali ablaktáblán kattintson a **bejövő szabályok**, majd kattintson a jobb oldali ablaktáblán, és **új szabály**.

3.  Kattintson az új bejövő szabály varázsló **Port**, és kattintson a **következő**.

4.  Győződjön meg arról, hogy **TCP** van jelölve, a **adott helyi portok**, írja be a BHOLD Core portszám (5151), vagy ha BHOLD Core telepítve, és kattintson a megadott portszám ** Következő**.

5.  Győződjön meg arról, hogy **a kapcsolat engedélyezéséhez** van kiválasztva, és kattintson **következő**.

6.  Az a **profil** lapon, törölje a jelet a jelölőnégyzetből helyeket, ahonnan nem szeretné, hogy a BHOLD webhely eléréséhez, és kattintson a **következő**.

7.  Az a **neve** lapon adja meg (például BHOLD Core bejövő kapcsolatok engedélyezése) a szabály nevét, és kattintson a **Befejezés**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>BHOLD Core alkalmazáskészlet 32 bites alkalmazások engedélyezése

Ahhoz, hogy az IIS megfelelően működnek a BHOLD Alap modulban, konfigurálnia kell a BHOLD Core alkalmazáskészlet 32 bites alkalmazások támogatásához. A művelet végrehajtásához BHOLD Core-modul telepítéséhez használt fiók be kell bejelentkeznie.

**BHOLD Core alkalmazáskészlet 32 bites alkalmazást támogatásának engedélyezése**

1.  Internet Information Services kezelő megnyitásához kattintson **Start**, mutasson a **felügyeleti eszközei**, és kattintson a **Internet Information Services (IIS) Manager**.

2.  A konzolfán bontsa ki a kiszolgáló nevét, és kattintson **alkalmazáskészletek**.

3.  Az a **alkalmazáskészletek** listában kattintson a jobb gombbal **CoreAppPool**, és kattintson a **speciális beállítások**.

4.  Az a **speciális beállítások** párbeszédpanel a **engedélyezése 32 bites alkalmazások** listáról válassza ki **igaz**, és kattintson a **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Az egyszerű szolgáltatásnév a BHOLD-webhely létrehozása

Ha a hálózat neve, amellyel lépjen kapcsolatba a BHOLD webhelye nem ugyanaz, mint a kiszolgáló állomásneve, létre kell hoznia egy egyszerű szolgáltatásnév (SPN) a HTTP. Például ha egy olyan CNAME erőforrásrekordot a DNS-ben meg olyan aliasnevet, a kiszolgáló, vagy használhatja a hálózati terheléselosztás, regisztrálnia kell a következő további hálózati címek az Active Directoryban. Ha nem sikerül, az Internet Explorer nem használható a Kerberos protokoll, amikor kapcsolatba lép a BHOLD webhelyet.

>[!IMPORTANT]
Ha a BHOLD Alap modulban a FIM-portál ugyanazon a számítógépen telepítve van, létre kell hoznia DNS-erőforrásrekordokat (CNAME vagy A) állomásnevek különbözőek BHOLD Core futtató kiszolgálók és a FIM-portál futtató kiszolgálón. Egy adott szolgáltatás-típus/server-alias párhoz csak egy egyszerű Szolgáltatásnevet hozhatók létre, és BHOLD Core és a FIM-portál megkövetelik külön SPN-ek mivel a különböző fiókok általában futnak. A setspn parancs egy hibát jelez, ha egyszerű Szolgáltatásnevet már egy másik fiókkal lett létrehozva.

Tagság a **Tartománygazdák**, vagy egy ezzel egyenértékű, ez a művelet elvégzéséhez.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Az egyszerű Szolgáltatásnevet a BHOLD webhely létrehozásához

1.  Kattintson az Active Directory tartományi szolgáltatások tartományvezérlő **Start**, kattintson a **minden program**, kattintson a **Kellékek**, kattintson a jobb gombbal **parancssor **, és kattintson a **Futtatás rendszergazdaként**.

2.  A parancssorba írja be a következő parancsot, és nyomja le az ENTER BILLENTYŰT: setspn – S HTTP / * \<networkalias\> \<tartomány\> * \\ * \<accountname\> * ahol:

    -   *\<networkalias\> * használó ügyfelek csatlakozni a BHOLD webhely-címe

    -   *\<tartomány\>*\\*\<accountname\> * BHOLD központi telepítésekor létrehozott BHOLD Core szolgáltatásfiók tartomány és a felhasználó neve.

3.  Ismételje meg az előző lépést minden egyéb kiderül, hogy az ügyfelek használják a BHOLD webhely, például kapcsolódni, CNAME aliasok, egy teljesen minősített tartománynevet tartalmazó neveket vagy neve (rövid) NetBIOS-tartománynév tartalmaz.

#### <a name="setting-bhold-system-attributes"></a>BHOLD rendszer attribútumainak beállítása

Ahhoz, hogy ellenőrizze, hogy a BHOLD Core modul telepítése sikeres volt, a BHOLD Core portál megnyitásához és a rendszer tulajdonságainak megtekintése. Emellett ahhoz, hogy a BHOLD központi modul funkciót megfelelően a környezetben, módosíthatja a következő BHOLD rendszerattribútumokkal, szükség szerint:

| **Attribútum**                | **Leírás**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Állítsa be az Y, ha a BHOLD webhelye fut egy fürtözött webszolgáltatás annak ellenőrzéséhez, hogy nemrég megjelenített elemek megfelelően működik-e a. Állítsa be az N, ha a BHOLD webhelye fut-e a különálló IIS-kiszolgálón.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Y beállításával győződjön meg arról, hogy a szervezeti egységekből (orgunits) helyezheti át csak orgunits mint a szülő orgunit szervezeti azonos típusú. Például ez megakadályozza, hogy a projekt orgunit egy részleg orgunit történő áthelyezését. N állítani egy orgunit el kell helyezni egy eltérő típusú orgunit engedélyezése. |
| **ABA között eltelt napok futtatása**     | Ha értékre van állítva egy kétjegyű egész számot adja meg az időközt (napokban) között két attribútum-alapú engedélyezési (ABA) futtatása. Például, hogy ABA futtatása elválasztott két nap megadásához írja be a 02.                                                                                                                     |
| **ABA futtassa a kezdő óra**    | Egy kétjegyű egész számot adjon meg egy attribútum-alapú engedélyezési futása közben a óra beállítása történik. Például annak meghatározása, hogy a Futtatás ABA kerül sor. 11:00 órakor 23 (23:00), adja meg.                                                                                                             |
| **Rendszer kardinalitása**       | Állítsa be az N, ha nem szeretné, hogy a rendszer számossága négyzet BHOLD. Az alapértelmezett érték: Y.                                                                                                                                                                                                                             |
| **Naplózás**                  | Állítsa be az N, ha nem szeretné, hogy be kell jelentkezniük a módosításokat. Az alapértelmezett érték: Y.                                                                                                                                                                                                                                            |
| **SystemQueue feldolgozása**   | Állítsa be az N, ha nem szeretné, hogy rendszer várólista feldolgozása. Ne módosítsa ezt az értéket, kivéve, ha ehhez a terméktámogatási szolgálathoz által vezérelt.                                                                                                                                                                                           |

A művelet végrehajtásához a Tartománygazdák csoport tagjaként kell bejelentkeznie.

**BHOLD rendszerattribútum beállítása**

1.  Kattintson a **Start**, kattintson a **minden program**, és kattintson a **Internet Explorer**.

2.  A cím mezőbe írja be, ahol * \<server\> * a BHOLD-webhely kiszolgálójának neve és * \<port\> * a portszámot a webhelyhez kötött.

3.  Kattintson a **Home**, kattintson a **értékek**, és kattintson a **módosítás**.

4.  Keresse meg a nevét, írja be az új értéket az attribútum neve melletti négyzetet, és kattintson a kívánt **OK**.

## <a name="next-steps"></a>További lépések

BHOLD Core telepítve és, hogy a telepítés sikeres volt-e érvényesítve, után további modulok is telepítheti. Ezen a ponton a BHOLD-adatbázis lényegében üres, csak egy felhasználói fiókot, a legfelső szintű fiókot és egy szervezeti egységet (orgunit), a legfelső szintű orgunit tartalmazó lesz. Több felhasználó hozzáadása a BHOLD-adatbázis, vagy telepítheti a Access Management-összekötő modul vagy a BHOLD modellhez generátor modul igényeitől függően. Az Access Management-összekötő modul segítségével felhasználói adatokat importálhat a FIM szinkronizálási szolgáltatás, vagy használhatja a BHOLD modellhez generátor felhasználói adatok importálása a strukturált fájlokat. Az Access Management-összekötő modullal kapcsolatos további információkért lásd: [tesztlabor-Útmutató: BHOLD Access Management-összekötő](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx).

Az BHOLD modellhez generátor modullal kapcsolatos további információkért lásd:

- [Microsoft BHOLD Suite fogalmak útmutató](https://technet.microsoft.com/en-us/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/en-us/library/jj134935(v=ws.10).aspx).

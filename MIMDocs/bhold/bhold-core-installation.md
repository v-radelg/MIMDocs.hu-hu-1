---
title: A BHOLD core telepítése |} A Microsoft Docs
description: A BHOLD suite telepítése core dokumentum
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 4499c2846655ff75462794d684cae44f03134f1c
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333530"
---
# <a name="bhold-core-installation"></a>A BHOLD Core telepítése

A BHOLD Core modul BHOLD Suite a környezet legfontosabb funkcióit nyújtja. A BHOLD Core modul kell telepíteni és konfigurálni egy kiszolgálóra a helyi hálózaton, más BHOLD Suite modulok telepítése előtt.

## <a name="bhold-core-installation-requirements"></a>A BHOLD Core telepítési követelményei

A BHOLD Core modul Microsoft BHOLD Suite alapját képezi. Más Microsoft BHOLD Suite modulok telepítése előtt telepítenie kell a BHOLD Core modul.

### <a name="bhold-core-hardware-requirements"></a>A BHOLD Core hardverkövetelmények

A BHOLD Core modul Microsoft BHOLD Suite alapját képezi. Más Microsoft BHOLD Suite modulok telepítése előtt telepítenie kell a BHOLD Core modul.

|          |        |          |
|----------|--------|----------|
|**Összetevő** |**Minimum** | **Ajánlott** |
|Processzor | 64 bites processzor | Többmagos 64 bites processzor |
| Memória |3 GB | 6 GB vagy több |
|Tárterület| 30 GB-ig érhető el |Üzembe helyezés méretétől függ |
|Hálózati adapter| A Forefront Identity Manager (FIM) és az SQL server 100 MB/s kapcsolat | Az SQL és a FIM-kiszolgáló 1 GB/s kapcsolat|

Ezekkel az ajánlásokkal tipikus megvalósításokhoz alapulnak, és nem veszi figyelembe a kiszolgálón futó egyéb alkalmazások. Előfordulhat, hogy szeretné használni, nagyobb teljesítményű összetevőit az adott környezettől függően.

### <a name="bhold-core-software-requirements"></a>A BHOLD Core szoftverkövetelmények

A BHOLD Core modulnak telepítve kell lennie, amely megfelel a következő számítógépen:

- A kiszolgáló Windows Server 2012 (64 bites), a Windows Server 2016-ra kell futnia 
- A kiszolgáló, az Active Directory Domain Services (AD DS) tartomány tagjának kell lennie. Tesztelési a kiszolgáló egy Active Directory tartományi szolgáltatások tartományvezérlő lehet.
- A BHOLD Core telepíteni kell egy fiókot a kiszolgálóval azonos tartományban, és amely tartozik, a tartományi rendszergazdák csoport a tartomány és a rendszergazdák csoportba a kiszolgálón a bejelentkezett felhasználó által.
- A Microsoft Internet Information Services (IIS) az ASP.NET-tel a kiszolgálón telepítve kell lennie. Az IIS Windows-hitelesítés engedélyezve kell konfigurálni. Ha a BHOLD Core telepítése folyamatban van a Windows Server 2012/2016, az IIS 6 kompatibilitási telepíteni kell. Ha a BHOLD Core telepítése a Windows Server 2012, telepítenie kell az IIS 6-parancsfájlkezelő eszközök
- Telepítenie kell a .NET 3.5-keretrendszer.
  - A BHOLD Core szükség van a .NET 3.5-ös verzióját, mert azt nem telepíthető a Server Core.
- A Silverlight 4 más BHOLD modulok által igényelt, és ezért azt javasoljuk, telepítse a BHOLD Core telepítése előtt.
- A Microsoft SQL Server 2014 vagy Microsoft SQL Server 2016 telepítve kell lennie a BHOLD Core-kiszolgálón vagy a helyi hálózaton egy másik kiszolgálón. 

Windows-ügyfélszámítógépek futnia kell a Microsoft Internet Explorer 6 vagy újabb verziójára és a Microsoft Silverlight 4 vagy újabb verzióját.

## <a name="before-you-begin"></a>Előkészületek

A BHOLD Core modulnak szüksége van egy felhasználói fiók hitelesítéséhez és engedélyezéséhez a kiszolgáló és egyéb hálózati entitásokat, a BHOLD Core szolgáltatás használt. Ez a szakasz azt ismerteti, hogyan kell létrehozni és konfigurálni a felhasználói fiókhoz, és biztosít, amelyek segítenek a BHOLD Core telepítés befejezéséhez szükséges információkat gyűjthet előtelepítési munkalap.

>{! FONTOS} BHOLD Core telepítése, használhatja a meglévő SQL Server-adatbázis a BHOLD Core adatbázisként, vagy engedélyezheti a BHOLD Core telepítővarázsló az Ön számára a BHOLD Core adatbázis létrehozásához. Ha egy meglévő adatbázis használatát választja, győződjön meg róla, hogy más felhasználók nem férhetnek az adatbázist, akkor, amikor telepíti a BHOLD Core. A BHOLD Core telepítés előtt győződjön meg arról, hogy a hozzáférés-vezérlést, az SQL Server-adatbázis nem teszik lehetővé a BHOLD Core telepítése a felhasználó csak hozzáférés.

## <a name="required-user-and-group"></a>Felhasználó és csoport

A BHOLD Core modult kell tudni a tartományhoz, amely erre a célra van kijelölve, és amely tagja két adott biztonsági csoportok, köztük a BHOLD Core modul kifejezetten a létrehozott felhasználói fiókkal jelentkezzen be. Tagság a Tartománygazdák csoport a művelet végrehajtásához szükséges.
**Létrehozni és konfigurálni a BHOLD Core felhasználók és biztonsági csoport**

1.  A tartományvezérlőn kattintson **Start**, mutasson a **felügyeleti eszközök**, és kattintson a **Active Directory – felhasználók és számítógépek**.

2.  A konzolfán bontsa ki a tartományt, amelyben a fiókot létrehozni, kattintson a jobb gombbal **felhasználók**, mutasson a **új**, és kattintson a **csoport**.

3.  Az a **új objektum – csoport** párbeszédpanel **csoportnév**, írja be a csoport nevét (BHOLD alapértelmezett: BHOLDApplicationGroup), és kattintson a **OK**.

4.  Kattintson a jobb gombbal **felhasználók**, mutasson a **új**, és kattintson a **felhasználói**.

5.  A **teljes fájlvisszaállítási név**, adjon meg egy nevet, amely segít azonosítani a fiókot, például a BHOLD Core szolgáltatásfiók.

6.  A **felhasználói bejelentkezési név**, írja be a felhasználónevet a BHOLD Core szolgáltatásfiók (BHOLD alapértelmezett: b1user), és kattintson a **tovább**.

7.  A **jelszó** és **jelszó megerősítése**, írja be a fiók jelszavát.

8.  Egyértelmű **kell változtatni a jelszót a következő bejelentkezéskor**, jelölje be **nem lehet változtatni a jelszót** és **jelszó sohasem jár le**, kattintson a **tovább**, majd **Befejezés**.

9.  A konzol ablaktáblában kattintson a jobb gombbal a felhasználói fiókot, és kattintson **felvétele egy csoportba**.

10. Az a **csoportok kiválasztása** párbeszédpanelen írja be a korábban létrehozott csoport megjelenítendő nevét, írja be egy pontosvesszővel (;), és írja be a IIS_IUSRS.

11. Kattintson a **Névellenőrzés**, és kattintson a **OK**.  

Az alábbi eljárást a számítógépen, ahol a BHOLD Core modul telepíteni kell elvégezni. Ez az eljárás végrehajtásához a Tartománygazdák csoport tagjaként kell bejelentkeznie.

## <a name="bhold-core-installation-worksheet"></a>A BHOLD Core telepítése munkalap

A BHOLD Core-modul telepítése előtt kell a BHOLD Core telepítővarázslója a telepítés befejezéséhez szükséges információk megadására. A következő munkalap segítségével fel ezt az információt, hogy készen áll a szükséges megadni. A BHOLD Core telepítése varázsló egy lapot minden szakasz felel meg.

### <a name="account-settings"></a>Fiókbeállítások

| **Elem**                                    | **Leírás**                                                                                                                                                                                                                                                                                             | **Érték**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztott, itt adhatja meg, hogy az Active Directory Domain Services biztonsági fog férhet hozzá a BHOLD Core.                                                                                                                                                                                                  | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                 |
| **Tartomány**                                  | A BHOLD server, a szolgáltatásfiók és az alkalmazáscsoport tartalmazó tartomány megadása **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-neve, nem a teljesen minősített tartománynevét (FQDN) használatával. Például ha a tartomány teljes Tartománynevét a fabrikam.com, adja meg a tartomány neve CONTOSO. | Írás a tartománynév:                                                                                                                                        |
| **Alkalmazáscsoport**                       | A korábban létrehozott biztonsági csoport neve [szükséges felhasználók és csoportok](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Írás a csoport nevét itt:                                                                                                                                         |
| **Szolgáltatás felhasználói**                            | A szolgáltatás, amely a korábban létrehozott felhasználói fiók bejelentkezési neve [szükséges felhasználók és csoportok](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Írás a felhasználói fiók nevét itt:                                                                                                                                  |
| **Jelszó**                                | A BHOLD Core felhasználói fiókjának jelszavát határozza meg.                                                                                                                                                                                                                                              | Írás a jelszót kell megadnia: **fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen.                                                                |
| **Webhely IP-Port**                         | Megadja az IP-cím és port számát a webhely az intraneten kiszolgálón kell létrehozni. Módosítsa az alapértelmezett értéket (\*) csak akkor, ha Ön nem az alapértelmezett webhely fogja használni az azonos IP-címet. Módosítsa a portszámot, egy szabad portot csak akkor, ha az alapértelmezett portot (5151) már használatban van.             | Ha egy nem alapértelmezett IP-címet használják az alapértelmezett webhelyhez, írja itt: Ha az alapértelmezett portszám már használatban van, írja be a BHOLD webhely portszámot itt: |

### <a name="database-settings"></a>Adatbázis-beállítások

| **Elem**                                       | **Leírás**                                                                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beépített biztonság használata**                    | Megadja, hogy Windows-hitelesítés az adatbázis eléréséhez.                                                                                                                                                                                                     | Jelölje be a jelölőnégyzetet, ha a Windows-hitelesítés használata az SQL-kiszolgálóhoz való csatlakozáshoz. Törölje a jelölőnégyzet jelölését, az SQL Server-hitelesítés használata esetén. Az adatbázist kell létrehozni futtató BHOLD Core telepítés Ha SQL Server-hitelesítés van használata előtt. **Megjegyzés:** Ha Windows-hitelesítést használ, akkor jelentkezett be egy olyan fiókkal, amely a sysadmin (rendszergazda) kiszolgálói szerepkörrel rendelkezik az adatbázis-kiszolgálón. |
| **Adatbázis-felhasználót** és **adatbázis-jelszó** | Adja meg a felhasználónevet és a egy felhasználó jelszavát a sysadmin (rendszergazda) kiszolgálói szerepkörrel a kiszolgálón. Ezek az értékek csak az SQL Server-hitelesítés használata esetén vannak megadva.                                                                                               | Az SQL Server felhasználói nevét itt írási: Itt az SQL Server felhasználói jelszó írása: **Megjegyzés:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen.                                                                                                                                                                                                                                                  |
| **Adatbázis-kiszolgáló** és **adatbázis neve**   | Megadja az adatbázis-kiszolgáló NetBIOS-nevét és az adatbázis nevét (alapértelmezés: b1), amely a BHOLD Core telepítés hozza létre. Ha nem használ az alapértelmezett adatbázis-kiszolgálópéldányra, adja meg az adatbázis-kiszolgálópéldányra formájában  *\<kiszolgáló\>*\\*\<példány\>* . | A kiszolgáló (vagy a kiszolgálót és példányt) nevet itt írási: írási itt az adatbázis neve:                                                                                                                                                                                                                                                                                                                   |
| **Győződjön meg arról, az adatbázis-felhasználó korlátozások**    | Elavult.                                                                                                                                                                                                                                                                 | Ne módosítsa az alapértelmezett érték                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>A BHOLD Core telepítés

BHOLD Core-modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD Core-modul telepítése: 

- BholdCore  *\<verzió\>*\_Release.msi

Cserélje le *\<verzió\>* a BHOLD Core kiadás, amely telepíti a verziószámával.

A program rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

### <a name="postinstallation-settings"></a>Teendőkkel beállításai

A BHOLD Core telepítés után konfigurálja a Windows tűzfalat, és a BHOLD Core alkalmazáskészletet az Internet Information Services a BHOLD Core konfiguráció befejezéséhez speciális beállításainak módosítását. Ha szükséges, akkor kell megváltoztatnia BHOLD rendszerattribútumokkal az igényeknek is.

#### <a name="configuring-windows-firewall"></a>Windows tűzfal konfigurálása

Ha a felhasználó éri el a BHOLD webböngészők távoli számítógépeken, konfigurálnia kell a BHOLD Core-kiszolgáló a webhely portját, amely a BHOLD Core telepítése során megadott bejövő kapcsolatainak engedélyezése Windows tűzfal.

Ez az eljárás végrehajtásához a helyi számítógépen a Rendszergazdák csoport tagjának kell lennie.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>A BHOLD webhelyre bejövő kapcsolatok engedélyezéséhez

1.  Kattintson a **Start**, mutasson a **felügyeleti eszközök**, kattintson a jobb gombbal **speciális beállítások a Windows tűzfal**, és kattintson a **Futtatás rendszergazdaként**.

2.  A bal oldali ablaktáblán kattintson a **bejövő szabályok**, majd kattintson a jobb oldali ablaktáblán, és **új szabály**.

3.  Kattintson az új bejövő szabály varázsló **Port**, és kattintson a **tovább**.

4.  Ügyeljen arra, hogy **TCP** van kiválasztva, a **adott helyi portok**, írja be a BHOLD Core portszám (5151) vagy a port számát, amikor a BHOLD Core telepítése, és kattintson a megadott  **Tovább**.

5.  Ügyeljen arra, hogy **engedélyezze a kapcsolatot** van kiválasztva, és kattintson **tovább**.

6.  Az a **profil** lapon, törölje a jelet a jelölőnégyzetből, amelyről nem kívánja elvégezni a BHOLD-webhelyen való hozzáférés, majd kattintson a helyek **tovább**.

7.  Az a **neve** lapon adjon meg egy nevet a szabálynak (például a BHOLD Core bejövő kapcsolatokat engedélyező), és kattintson a **Befejezés**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>A BHOLD Core alkalmazáskészlet 32 bites alkalmazások engedélyezése

Ahhoz, hogy megfelelően működnek a BHOLD Core-modul az IIS, konfigurálnia kell a BHOLD Core alkalmazáskészlet 32 bites alkalmazások támogatásához. A művelet végrehajtásához a BHOLD Core-modul telepítéséhez használt fiókkal jelentkezik be kell jelentkeznie.

**A BHOLD Core alkalmazáskészlet 32 bites alkalmazást támogatásának engedélyezése**

1.  Az Internet Information Services kezelő megnyitásához kattintson a **Start**, mutasson a **felügyeleti eszközei**, és kattintson a **Internet Information Services (IIS) kezelője**.

2.  A konzolfán bontsa ki a kiszolgáló nevét, és kattintson **alkalmazáskészletek**.

3.  Az a **alkalmazáskészletek** listában kattintson a jobb gombbal **CoreAppPool**, és kattintson a **speciális beállítások**.

4.  Az a **speciális beállítások** párbeszédpanel a **engedélyezése 32 bites alkalmazások** listáról válassza ki **igaz**, és kattintson a **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>A BHOLD webhely egyszerű szolgáltatásnév létrehozása

Ha a hálózat nevét, amellyel lépjen kapcsolatba a BHOLD webhely nem ugyanaz, mint a kiszolgáló állomásneve, létre kell hoznia egy egyszerű szolgáltatásnevét (SPN) HTTP-hez. Például ha egy CNAME-erőforrásrekordot a DNS-ben megadjon egy aliast a kiszolgálón, vagy ha a hálózati terheléselosztás használ, az Active Directory regisztrálnia kell ezeket a további hálózati címüket. Ha ehhez nem, az Internet Explorer nem használható a Kerberos protokoll, amikor kapcsolatba lép a BHOLD webhely.

> [!IMPORTANT]
> A BHOLD Core-modul telepítve van a FIM-portál ugyanazon a számítógépen, ha az állomásnevek különbözőek a BHOLD Core futtató kiszolgálókhoz és a FIM-portál futtató kiszolgálón létre kell hoznia DNS-erőforrásrekordokat (CNAME vagy egy). Csak egy egyszerű Szolgáltatásnevet, létre lehet hozni egy adott szolgáltatás-típus vagy kiszolgálói aliast pár és így a BHOLD Core és a FIM-portál igényli külön SPN-EK, mert a különböző fiókok általában futnak. A setspn parancs hibát jelez, ha egyszerű Szolgáltatásnevet már létrejött egy másik fiókkal.

Tagság a **Tartománygazdák**, vagy ezekkel egyenértékű művelet végrehajtásához szükséges.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>A BHOLD webhely az egyszerű szolgáltatásnév létrehozásához

1.  Az Active Directory Domain Services tartományvezérlőn kattintson **Start**, kattintson a **minden program**, kattintson a **Kellékek**, kattintson a jobb gombbal **parancssor használatával** , és kattintson a **Futtatás rendszergazdaként**.

2.  A parancssorba írja be a következő parancsot, és nyomja le az ENTER BILLENTYŰT: setspn – S HTTP / *\<networkalias\>\<tartomány\>*\\*\<accountname\>* ahol:

    -   *\<networkalias\>* használó ügyfelek csatlakozni a BHOLD webhely-címe

    -   *\<tartomány\>*\\*\<accountname\>* BHOLD központi telepítésekor létrehozott BHOLD Core szolgáltatásfiók tartomány és a felhasználó neve.

3.  Ismételje az előző lépést minden egyéb ügyfelek által használt, lépjen kapcsolatba a BHOLD webhely, például neveket, CNAME aliasok, egy teljesen minősített tartománynevet tartalmazó neveket vagy neve (rövid) NetBIOS-tartománynév tartalmaz.

#### <a name="setting-bhold-system-attributes"></a>A BHOLD rendszer attribútumainak beállítása

Annak érdekében, hogy ellenőrizze, hogy a BHOLD Core-modul telepítése sikeres volt-e, nyissa meg a BHOLD Core portálját, és megtekintheti a rendszer attribútumait. Emellett ahhoz, hogy a BHOLD Core modul függvények megfelelően a környezetében, módosíthatja a következő BHOLD rendszerattribútumokkal, szükség szerint:

| **Attribútum**                | **Leírás**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Y a BHOLD-webhelyen fut, győződjön meg arról, hogy nemrég megjelenített elemek megfelelően működik-e egy fürtözött webszolgáltatás beállítása. Ha a BHOLD webhelye fut-e a különálló IIS-kiszolgálón állítsa be-N.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Y győződjön meg arról, hogy szervezeti egységek (orgunits) is áthelyezhető csak orgunits szervezeti azonos típusú, a szülő orgunit beállításával. Például ez megakadályozza, hogy a projekt orgunit folyamatban van egy szervezeti egység orgunit helyezve. N beállítás engedélyezi egy orgunit egy eltérő típusú orgunit kell elhelyezni. |
| **Nap közötti ABA futtatása**     | Értékre van állítva egy kétjegyű egész szám, adja meg az időköz (nap) két attribútum-alapú hitelesítést (ABA) futtatásai között. Írja be például, hogy ABA futtatások elválasztott két nap megadásához 02.                                                                                                                     |
| **ABA Futtatás kezdő órája**    | Egy kétjegyű egész szám, amely a nap, amikor egy attribútum-alapú engedélyezési futtassa az órát adja meg a készlet akkor áll elő. Ha például annak megadásához, hogy a Futtatás ABA kerül sor 11:00 órakor 23 (23:00), adja meg.                                                                                                             |
| **Rendszer számossága**       | Ha nem szeretné, hogy a rendszer Számosság ellenőrzése a BHOLD beállítása N. Az alapértelmezett érték: Y.                                                                                                                                                                                                                             |
| **Logging**                  | N értékre, ha nem szeretné, hogy be kell jelentkeznie a módosításokat. Az alapértelmezett érték: Y.                                                                                                                                                                                                                                            |
| **SystemQueue feldolgozása**   | N értékre, ha nem szeretné, hogy rendszer várólista feldolgozása. A terméktámogatási szolgálathoz. Ehhez utasításig ne módosítsa ezt az értéket.                                                                                                                                                                                           |

Ez az eljárás végrehajtásához a Tartománygazdák csoport tagjaként kell bejelentkeznie.

**A BHOLD rendszerattribútum beállítása**

1.  Kattintson a **Start**, kattintson a **minden program**, és kattintson a **az Internet Explorer**.

2.  A címsorba írja be, ahol *\<kiszolgáló\>* a BHOLD-webhely kiszolgálójának neve és *\<port\>* a portszámot a webhelyhez kötött.

3.  Kattintson a **kezdőlap**, kattintson a **értékek**, és kattintson a **módosítás**.

4.  Keresse meg a nevét, módosítsa, írja be az új értéket az attribútum neve melletti jelölőnégyzetbe, és kattintson a kívánt **OK**.

## <a name="next-steps"></a>További lépések

A BHOLD Core telepítése, és ellenőrzi, hogy a telepítés sikeres volt-e után telepíthet további modulok. Ezen a ponton a BHOLD-adatbázis lényegében üres, csak egy felhasználói fiókot, a root fiókjának és a egy szervezeti egységet (orgunit), a legfelső szintű orgunit tartalmazó lesz. Több felhasználó hozzáadása a BHOLD-adatbázis, vagy telepítheti az Access Management-összekötő modul vagy a BHOLD Model Generator modul igényeitől függően. Használhatja az Access Management-összekötő modul felhasználói adatok importálása a FIM szinkronizálási szolgáltatás, vagy használhatja a BHOLD Model Generator importálja a strukturált fájlokat a felhasználó adatait. Az Access Management-összekötő modullal kapcsolatos további információkért lásd: [tesztlabor-Útmutató: a BHOLD Access Management-összekötő](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

A BHOLD Model Generator modullal kapcsolatos további információkért lásd:

- [A Microsoft a BHOLD Suite fogalmak útmutató](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [A Microsoft a BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).

---
title: BHOLD Core-telepítés | Microsoft Docs
description: A BHOLD Suite telepítési alapdokumentuma
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: c4dfb4184292ba1b5da8c4e3e176d53e6a885ed8
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042270"
---
# <a name="bhold-core-installation"></a>BHOLD Core-telepítés

A BHOLD Core modul a BHOLD Suite főbb funkcióit biztosítja a környezetében. A BHOLD Core modult a helyi hálózatban lévő kiszolgálón kell telepíteni és konfigurálni, mielőtt más BHOLD Suite-modulokat is telepítené.

## <a name="bhold-core-installation-requirements"></a>A BHOLD Core telepítési követelményei

A BHOLD Core modul a Microsoft BHOLD Suite alapjait képezi. A Microsoft BHOLD Suite más moduljainak telepítése előtt telepítenie kell a BHOLD Core-modult.

### <a name="bhold-core-hardware-requirements"></a>A BHOLD alapvető hardverkövetelmények

A BHOLD Core modul a Microsoft BHOLD Suite alapjait képezi. A Microsoft BHOLD Suite más moduljainak telepítése előtt telepítenie kell a BHOLD Core-modult.

|          |        |          |
|----------|--------|----------|
|**Összetevő** |**Minimális** | **Ajánlott** |
|Processzor | 64 bites processzor | Többmagos 64 bites processzor |
| Memory (Memória) |3 GB | 6 GB vagy több |
|Storage| 30 GB elérhető |A központi telepítés méretétől függ |
|Hálózati adapter| 100Mbps az SQL és a Forefront Identity Manager (FIM) kiszolgálóval való kapcsolódáshoz | 1Gbps az SQL-és FIM-kiszolgálóhoz való kapcsolódáshoz|

Ezek a javaslatok a tipikus implementáción alapulnak, és nem veszik figyelembe a kiszolgálón futó egyéb alkalmazásokat. Előfordulhat, hogy az adott környezettől függően nagyobb teljesítményű összetevőket kell használnia.

### <a name="bhold-core-software-requirements"></a>A BHOLD Core szoftverre vonatkozó követelmények

A BHOLD Core modult olyan számítógépre kell telepíteni, amely megfelel az alábbi követelményeknek:

- A kiszolgálónak Windows Server 2012 (64 bites), Windows Server 2016 rendszernek kell futnia 
- A kiszolgálónak Active Directory tartományi szolgáltatások (AD DS) tartomány tagjának kell lennie. A tesztelési környezetekben előfordulhat, hogy a kiszolgáló egy AD DS tartományvezérlő.
- A BHOLD Core-t egy, a-kiszolgálóval megegyező tartományban lévő fiókkal bejelentkezett felhasználónak kell telepítenie, amely a tartomány Tartománygazdák csoportjába és a kiszolgálón található rendszergazdák csoportba tartozik.
- A kiszolgálón telepítve kell lennie a Microsoft Internet Information Services (IIS) ASP.NET. Az IIS-t engedélyezve kell lennie a Windows-hitelesítéshez. Ha a BHOLD Core-ot a Windows Server 2012/2016-es verzióra telepíti, telepíteni kell az IIS 6 felügyeleti kompatibilitását. Ha a BHOLD Core a Windows Server 2012 rendszerre van telepítve, telepíteni kell az IIS 6 parancsfájlkezelési eszközeit.
- Telepíteni kell a .NET 3,5 keretrendszert.
  - Mivel a BHOLD Core a .NET 3,5-es verzióra van szüksége, nem telepíthető a Server Core-ra.
- A Silverlight 4-et más BHOLD-modulok igénylik, ezért azt javasoljuk, hogy telepítse a BHOLD Core telepítése előtt.
- Microsoft SQL Server 2014 vagy Microsoft SQL Server 2016 kell telepíteni a BHOLD Core-kiszolgálón vagy egy másik kiszolgálón a helyi hálózaton. 

A Windows rendszerű ügyfélszámítógépeken a Microsoft Internet Explorer 6-os vagy újabb verzióját, valamint a Microsoft Silverlight 4-es vagy újabb verzióját kell futtatni.

## <a name="before-you-begin"></a>Előkészületek

Az BHOLD Core modulhoz olyan felhasználói fiók szükséges, amely a BHOLD Core szolgáltatás hitelesítésére és engedélyezésére szolgál a kiszolgálón és más hálózati entitásokban. Ez a szakasz ismerteti, hogyan hozhatja létre és konfigurálhatja a felhasználói fiókot, és olyan előtelepítési munkalapot biztosít, amely segít összegyűjteni az BHOLD Core telepítőjének befejezéséhez szükséges információkat.

>{! FONTOS} az BHOLD Core telepítésekor meglévő SQL Server adatbázist használhat BHOLD Core adatbázisként, vagy engedélyezheti az BHOLD Core telepítővarázsló számára, hogy létrehozza a BHOLD Core-adatbázist. Ha meglévő adatbázist használ, gondoskodnia kell arról, hogy más felhasználók ne férhessenek hozzá az adatbázishoz az BHOLD Core telepítésekor. A BHOLD Core telepítése előtt ellenőrizze, hogy a SQL Server adatbázis hozzáférés-vezérlése nem engedélyezi-e a hozzáférést az BHOLD Core-t telepítő felhasználótól eltérő felhasználók számára.

## <a name="required-user-and-group"></a>Szükséges felhasználó és csoport

A BHOLD Core modulnak képesnek kell lennie arra, hogy egy erre a célra kijelölt felhasználói fiókkal jelentkezzen be a tartományba, amely a két meghatározott biztonsági csoport tagja, beleértve a BHOLD Core modulhoz létrehozott egyet is. A művelet végrehajtásához a Tartománygazdák csoport tagjának kell lennie.
**A BHOLD Core-felhasználó és biztonsági csoport létrehozása és konfigurálása**

1.  A tartományvezérlőn kattintson a **Start**gombra, mutasson a **felügyeleti eszközök**pontra, majd kattintson a **Active Directory felhasználók és számítógépek**elemre.

2.  A konzolfán bontsa ki azt a tartományt, amelyben létre szeretné hozni a fiókot, kattintson a jobb gombbal a **felhasználók**elemre, mutasson az **új**elemre, majd kattintson a **csoport**elemre.

3.  Az **új objektum – csoport** párbeszédpanelen a **csoport neve**mezőbe írja be a csoport nevét (BHOLD default: BHOLDApplicationGroup), majd kattintson az **OK**gombra.

4.  Kattintson a jobb gombbal a **felhasználók**elemre, mutasson az **új**elemre, majd kattintson a **felhasználó**elemre.

5.  A **teljes név**mezőbe írjon be egy nevet, amely segítséget nyújt a fiók azonosításában, például BHOLD Core-szolgáltatásfiók.

6.  A **felhasználói bejelentkezési név**mezőbe írja be a BHOLD Core szolgáltatásfiók felhasználónevét (BHOLD default: b1user), majd kattintson a **tovább**gombra.

7.  A **jelszó és a** **Jelszó megerősítése**mezőbe írja be a szolgáltatásfiók jelszavát.

8.  **A következő bejelentkezéskor a felhasználónak meg kell változtatnia a jelszót**jelölőnégyzet bejelölése esetén a **felhasználó nem módosíthatja** a jelszót és a **jelszót soha nem jár le**, kattintson a **tovább**, majd a **Befejezés**gombra.

9.  A konzol eredmények ablaktábláján kattintson a jobb gombbal a felhasználói fiókra, majd kattintson a **Hozzáadás egy csoporthoz**lehetőségre.

10. A **csoportok kiválasztása** párbeszédpanelen írja be a korábban létrehozott csoport megjelenítendő nevét, írjon be egy pontosvesszőt (;), majd írja be a IIS_IUSRS.

11. Kattintson **a Névellenőrzés elemre,** majd **az OK**gombra.  

A következő eljárást kell végrehajtani azon a számítógépen, amelyen az BHOLD Core modult telepíteni kell. A művelet végrehajtásához a Tartománygazdák csoport tagjaként kell bejelentkeznie.

## <a name="bhold-core-installation-worksheet"></a>BHOLD Core telepítési munkalap

Mielőtt megkezdené a BHOLD Core modul telepítését, elő kell készítenie, hogy a BHOLD Core telepítővarázsló által a telepítés befejezéséhez szükséges információk meglegyenek. A következő munkalapon rögzítheti ezeket az adatokat, így készen áll arra, hogy szükség esetén megadja azt. Az egyes szakaszok az BHOLD Core telepítővarázsló egyik lapjára vonatkoznak.

### <a name="account-settings"></a>Fiókbeállítások

| **Elem**                                    | **Leírás**                                                                                                                                                                                                                                                                                             | **Érték**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Biztonsági szolgáltató használata tartományon vagy gépen** | Ha be van jelölve, akkor a Active Directory tartományi szolgáltatások biztonság szabályozza a BHOLD mag elérését.                                                                                                                                                                                                  | Jelölje be a jelölőnégyzetet. **Fontos:** Ha ez a jelölőnégyzet nincs bejelölve, a telepítés sikertelen lesz.                                                                 |
| **Tartományi**                                  | Megadja a BHOLD-kiszolgálót, a szolgáltatásfiókot és az erőforráscsoportot tartalmazó tartományt. **Fontos:** A tartománynevet a NetBIOS (rövid) név használatával adja meg, ne a teljes tartománynevet (FQDN). Ha például a tartomány teljes tartományneve fabrikam.com, a tartománynevet a CONTOSO nevet adja meg. | Írja be a tartománynevet itt:                                                                                                                                        |
| **Alkalmazáscsoport**                       | Megadja a korábban a [szükséges felhasználó és csoport](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug)által létrehozott biztonsági csoport nevét.                                                                                                                                  | Írja be a csoport nevét:                                                                                                                                         |
| **Szolgáltatás felhasználója**                            | Annak a szolgáltatás-felhasználói fióknak a bejelentkezési nevét adja meg, amelyet korábban a [szükséges felhasználóhoz és csoporthoz](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug)hozott létre.                                                                                                                      | Írja be a felhasználói fiók nevét:                                                                                                                                  |
| **Jelszó**                                | Megadja a BHOLD Core szolgáltatás felhasználói fiókjának jelszavát.                                                                                                                                                                                                                                              | Írja be a jelszót itt: **Fontos:** ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja.                                                                |
| **Webhely IP-címe/port**                         | Az intranetes kiszolgálón létrehozandó webhely IP-címét és portszámát adja meg. Csak akkor módosítsa az alapértelmezett\*értéket (), ha nem ugyanazt az IP-címet fogja használni, mint az alapértelmezett webhely. A portszámot csak akkor módosítsa egy elérhető portra, ha az alapértelmezett port (5151) már használatban van.             | Ha az alapértelmezett webhely nem alapértelmezett IP-címet használ, írja ide: Ha az alapértelmezett portszám már használatban van, írja be a BHOLD webhely portszámát itt: |

### <a name="database-settings"></a>Adatbázis-beállítások

| **Elem**                                       | **Leírás**                                                                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beépített biztonság használata**                    | Megadja, hogy a rendszer a Windows-hitelesítést használja az adatbázis eléréséhez.                                                                                                                                                                                                     | Jelölje be a jelölőnégyzetet, ha Windows-hitelesítést használ a SQL Serverhoz való kapcsolódáshoz. Törölje a jelölőnégyzet jelölését, ha SQL Server hitelesítést használ. Ha SQL Server hitelesítést használ, az adatbázist létre kell hozni az BHOLD Core telepítőjének futtatása előtt. **Megjegyzés:** Windows-hitelesítés használata esetén olyan fiókkal kell bejelentkeznie, amely a sysadmin kiszolgálói szerepkörrel rendelkezik az adatbázis-kiszolgálón. |
| **Adatbázis-felhasználó** és **adatbázis jelszava** | A sysadmin kiszolgálói szerepkörrel rendelkező felhasználó felhasználónevét és jelszavát adja meg az adatbázis-kiszolgálón. Ezeket az értékeket csak akkor kell megadni, ha SQL Server hitelesítés van használatban.                                                                                               | Írja be a SQL Server felhasználónevet: írja be ide a SQL Server felhasználói jelszavát: **Megjegyzés:** ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja.                                                                                                                                                                                                                                                  |
| **Adatbázis-kiszolgáló** és **adatbázis neve**   | Megadja az adatbázis-kiszolgáló NetBIOS-nevét és az adatbázis nevét (alapértelmezett: B1), amelyet a BHOLD Core telepítő fog létrehozni. Ha nem az alapértelmezett adatbázis-kiszolgáló példányt használja, akkor az adatbázis-kiszolgáló példányát kell megadnia a Form * \<\>Server*\\*\<-példányban\>*. | Írja be a kiszolgáló (vagy a kiszolgáló és a példány) nevét itt: írja be az adatbázis nevét:                                                                                                                                                                                                                                                                                                                   |
| **Az adatbázis-felhasználó korlátozásai**    | Elavult.                                                                                                                                                                                                                                                                 | Ne módosítsa az alapértelmezett értéket                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD Core-telepítő

Az BHOLD Core modul telepítéséhez jelentkezzen be a Tartománygazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként azon a kiszolgálón, amelyre telepíteni kívánja a BHOLD Core modult: 

- BholdCore * \<verzió\>*\_. msi

Cserélje * \<le\> a verziót* a telepítendő BHOLD Core verzió verziószámára.

A programfájl rendszergazdaként való futtatásához kattintson a jobb gombbal a fájlra, majd kattintson a **Futtatás rendszergazdaként**parancsra.

### <a name="postinstallation-settings"></a>Telepítés utáni teendőkkel-beállítások

A BHOLD Core telepítésének befejezése után konfigurálnia kell a Windows tűzfalat, és módosítania kell a speciális beállításokat a BHOLD Core Internet Information Services alkalmazáskészletben a BHOLD alapkonfigurációjának befejezéséhez. Ha szükséges, módosítsa a BHOLD rendszerattribútumait is, hogy megfeleljenek a követelményeinek.

#### <a name="configuring-windows-firewall"></a>A Windows tűzfal konfigurálása

Ha a felhasználók a távoli számítógépeken található webböngészők használatával érik el a BHOLD-t, a Windows tűzfalat úgy kell konfigurálnia a BHOLD Core kiszolgálón, hogy engedélyezze a bejövő kapcsolatokat a BHOLD Core telepítésekor megadott webhely-portra.

A művelet végrehajtásához a rendszergazdák csoport tagjának kell lennie a helyi számítógépen.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>A BHOLD webhelyre irányuló bejövő kapcsolatok engedélyezése

1.  Kattintson a **Start**gombra, mutasson a **felügyeleti eszközök**pontra, kattintson a jobb gombbal **a speciális beállításokkal rendelkező Windows tűzfal**elemre, majd kattintson a **Futtatás rendszergazdaként**parancsra.

2.  A bal oldali ablaktáblán kattintson a **Bejövő szabályok**elemre, majd a jobb oldali ablaktáblán kattintson az **új szabály**elemre.

3.  Az új bejövő szabály varázslóban kattintson a **port**elemre, majd a **tovább**gombra.

4.  Győződjön meg arról, hogy a **TCP** van kiválasztva, az **adott helyi portok**mezőben adja meg az alapértelmezett BHOLD (5151), vagy a BHOLD Core telepítésekor megadott portszámot, majd kattintson a **tovább**gombra.

5.  Győződjön meg arról, hogy **a kapcsolatok engedélyezése** lehetőség ki van választva, majd kattintson a **tovább**gombra.

6.  A **profil** lapon törölje a jelölőnégyzeteket azokról a helyekről, amelyekről nem kívánja elérni a BHOLD webhelyét, majd kattintson a **tovább**gombra.

7.  A **név** lapon írja be a szabály nevét (például engedélyezze a bejövő kapcsolatokat a BHOLD mag számára), majd kattintson a **Befejezés**gombra.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>32 bites alkalmazások engedélyezése a BHOLD Core alkalmazáskészlethez

Ahhoz, hogy az IIS megfelelően működjön az BHOLD Core modullal, konfigurálnia kell a BHOLD Core alkalmazáskészletet a 32 bites alkalmazások támogatásához. Az eljárás végrehajtásához a BHOLD Core modul telepítéséhez használt fiókkal kell bejelentkeznie.

**A 32 bites alkalmazások támogatásának engedélyezése a BHOLD Core alkalmazáskészlethez**

1.  A Internet Information Services Manager megnyitásához kattintson a **Start**gombra, mutasson a **felügyeleti eszközök**pontra, majd kattintson az **Internet Information Services (IIS) kezelője**elemre.

2.  A konzolfán bontsa ki a kiszolgáló nevét, majd kattintson az **alkalmazáskészletek**elemre.

3.  Az **alkalmazáskészletek** listában kattintson a jobb gombbal a **CoreAppPool**elemre, majd kattintson a **Speciális beállítások**elemre.

4.  A **Speciális beállítások** párbeszédpanel **32 bites alkalmazások engedélyezése** listájában válassza az **igaz**lehetőséget, majd kattintson **az OK**gombra.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Az egyszerű szolgáltatásnév létrehozása a BHOLD webhelyhez

Ha a BHOLD webhelyhez való kapcsolódáshoz használt hálózatnév nem ugyanaz, mint a kiszolgáló állomásneve, létre kell hoznia egy egyszerű szolgáltatásnév (SPN) HTTP-hez. Ha például CNAME erőforrásrekordot használ a DNS-ben a kiszolgáló aliasnevének megadásához, vagy ha hálózati terheléselosztást használ, a további hálózati címeket regisztrálnia kell Active Directoryban. Ha ezt nem teszi meg, az Internet Explorer nem használhatja a Kerberos protokollt a BHOLD webhelyhez való kapcsolódáskor.

> [!IMPORTANT]
> Ha a BHOLD Core modul ugyanazon a számítógépen van telepítve, mint a FIM-portál, akkor létre kell hoznia egy DNS-erőforrásrekordot (CNAME vagy A) a BHOLD Core-t futtató kiszolgálók és a FIM-portált futtató kiszolgáló különböző állomásneve alapján. Egy adott szolgáltatástípus/kiszolgáló-alias pár esetében csak egy egyszerű szolgáltatásnevet lehet létrehozni, így a BHOLD Core és a FIM Portal számára külön SPN-ket kell megadni, mivel általában különböző fiókokban futnak. A Setspn parancs hibát jelez, ha egy SPN már meg van határozva egy másik fiókban.

A művelet végrehajtásához legalább a **Tartománygazdák**vagy ezzel egyenértékű csoport tagjának kell lennie.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>A BHOLD webhely egyszerű szolgáltatásnév létrehozása

1.  A Active Directory tartományi szolgáltatások tartományvezérlőn kattintson a **Start**gombra, kattintson a **minden program**, majd a **kellékek**elemre, kattintson a jobb gombbal a **parancssor**elemre, majd kattintson a **Futtatás rendszergazdaként**parancsra.

2.  A parancssorba írja be a következő parancsot, majd nyomja le az ENTER billentyűt: Setspn – S http/ * \<\> \<networkalias tartomány\> * * \<\> * \\ accountname, ahol:

    -   *a networkalias\> az a címe, amelyet az ügyfelek a BHOLD webhelyhez való kapcsolódáshoz használnak \<*

    -   *\<a\>tartomány*\\accountname a BHOLD Core telepítésekor létrehozott BHOLD Core-szolgáltatásfiók tartománya és felhasználóneve.*\<\> *

3.  Ismételje meg az előző lépést mindazon neveknél, amelyeket az ügyfelek a BHOLD webhelyhez való kapcsolódáshoz használnak, például CNAME aliasok, a teljes tartománynevet tartalmazó nevek vagy a NetBIOS (rövid) tartománynevet tartalmazó nevek.

#### <a name="setting-bhold-system-attributes"></a>BHOLD-rendszerattribútumok beállítása

Annak ellenőrzéséhez, hogy a BHOLD Core modul telepítése sikeres volt-e, nyissa meg a BHOLD Core portált, és tekintse meg a rendszer attribútumait. Továbbá annak érdekében, hogy a BHOLD Core modul megfelelően működjön a környezetben, a következő BHOLD-attribútumokat módosíthatja a megfelelő módon:

| **Attribútum**                | **Leírás**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Előzmények**                | Állítsa be az Y értékre, ha a BHOLD webhelye fürtözött webszolgáltatáson fut, így biztosítva, hogy a legutóbb megjelenített elemek megfelelően működjenek. Állítsa N értékre, ha a BHOLD webhely önálló IIS-kiszolgálón fut.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Állítsa az Y értékre, hogy biztosítsa, hogy a szervezeti egységek (orgunits-EK) csak a szülő orgunit megegyező szervezeti típussal helyezhetők át a orgunits. Ez meggátolja például, hogy a projekt orgunit egy részleg orgunit. Állítsa N értékre, hogy egy orgunit egy másik típusú orgunit legyen elhelyezve. |
| **Az ABA futtatása közötti napok**     | Két számjegyből álló egész számra van állítva, hogy meghatározza a két attribútum-alapú hitelesítés (ABA) közötti intervallumot (napokban). Ha például azt szeretné megadni, hogy az ABA-futtatások két nappal elválasztva legyenek, írja be a 02 értéket.                                                                                                                     |
| **Az ABA futtatásának kezdő órája**    | Egy kétjegyű egész számra van állítva, amely megadja, hogy a nap melyik órájában történik az attribútum-alapú engedélyezési Futtatás. Például annak megadásához, hogy az ABA-Futtatás a 11:00 órakor történik. (23:00), írja be a következőt: 23.                                                                                                             |
| **Rendszerszintű**       | Állítsa N értékre, ha nem szeretné, hogy a rendszer-és BHOLD. Az alapértelmezett érték Y.                                                                                                                                                                                                                             |
| **Naplózás**                  | Állítsa N értékre, ha nem szeretné, hogy a módosítások naplózva legyenek. Az alapértelmezett érték Y.                                                                                                                                                                                                                                            |
| **SystemQueue feldolgozása**   | Állítsa N értékre, ha nem szeretné, hogy a rendszervárólista feldolgozása megtörténjen. Ne módosítsa ezt az értéket, kivéve, ha a termék támogatása erre utasította.                                                                                                                                                                                           |

A művelet végrehajtásához a Tartománygazdák csoport tagjaként kell bejelentkeznie.

**BHOLD rendszer attribútumának beállítása**

1.  Kattintson a **Start**gombra, kattintson a **minden program**elemre, majd az **Internet Explorer**elemre.

2.  A címe mezőbe írja be a következőt * \<:\> * , ahol a kiszolgáló a BHOLD neve, a * \<port\> * pedig a webhelyhez kötött portszám.

3.  Kattintson a **Kezdőlap**, majd az **értékek**, végül pedig a **módosítás**lehetőségre.

4.  Keresse meg a módosítani kívánt attribútum nevét, írja be az új értéket az attribútum neve melletti mezőbe, majd kattintson az **OK**gombra.

## <a name="next-steps"></a>További lépések

Miután telepítette a BHOLD Core-t, és ellenőrizte, hogy a telepítés sikeres volt-e, további modulokat is telepíthet. Ezen a ponton a BHOLD-adatbázis lényegében üres lesz, amely csak egy felhasználói fiókot, a legfelső szintű fiókot és egy szervezeti egységet (orgunit) tartalmaz, amely a legfelső szintű orgunit. Ha további felhasználókat szeretne felvenni a BHOLD-adatbázisba, az igény szerint telepítheti a hozzáférés-kezelési összekötő modult vagy a BHOLD Model Generator modult. A hozzáférés-vezérlési összekötő modullal importálhatja a felhasználói adatait a FIM synchronization Service-ből, vagy használhatja a BHOLD Model Generatort a felhasználói adatok a strukturált fájlokból való importálásához. További információ a hozzáférés-kezelési összekötő modul használatáról: a [tesztkörnyezet útmutatója: BHOLD Access Management Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

A BHOLD Model Generator modul használatával kapcsolatos további információkért lásd:

- [A Microsoft BHOLD Suite fogalmi útmutatója](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite-TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).

---
title: "Microsoft Identity Manager 2016 – Jelszókezelés| Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 156551f4083c71ee7059e817213751393db5833e
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Microsoft Identity Manager 2016 – Jelszókezelés

A több adatforrással rendelkező vállalati környezetek felügyeletének egyik összetett része a több felhasználói fiókhoz tartozó jelszavak kezelése. A Microsoft Identity Manager 2016 (MIM) két jelszókezelési megoldást kínál:

-   Jelszó-szinkronizálás – A jelszóváltozás-értesítési szolgáltatás használatával rögzíti az Active Directoryból származó jelszóváltozásokat, és átvezeti őket más csatlakoztatott adatforrásokba.

-   Felhasználóalapú jelszóváltoztatás-kezelés – A Windows Management Instrumentationt (WMI) használja a webalapú ügyfélszolgálaton alapuló és önkiszolgáló jelszóváltoztatási alkalmazásokon keresztül.

A jelszó-szinkronizálás és a felhasználóalapú jelszóváltoztatás-kezelés a következőket teszi lehetővé:

-   Csökkentheti azon jelszavak számát, amelyeket a felhasználóknak meg kell jegyezniük.

-   Egy felhasználó több fiókjához egyidejűleg állíthatja be vagy módosíthatja ugyanazt a jelszót.

-   Megengedheti a felhasználóknak a saját jelszavuk módosítását az Active Directoryban, és a módosítás más rendszerekbe való leküldését is lehetővé teheti.

-   Kiküszöbölheti a további jelszóadattár vagy hitelesítőadat-tár létesítésével járó kockázatokat.

-   Az Active Directory mint hiteles adatforrás segítségével több adatforrás között szinkronizálhat jelszavakat.

-   Valós időben, a MIM-műveletektől függetlenül hajthatja végre a jelszókezelési műveleteket.

## <a name="password-extensions"></a>Jelszóbővítmények

A címtárkiszolgálók kezelőügynökei alapértelmezés szerint támogatják a jelszómódosítási és jelszóbeállítási műveleteket. A jelszómódosítási és jelszóbeállítási műveleteket alapértelmezés szerint nem támogató fájlalapú, adatbázis- és bővíthetőkapcsolat-kezelőügynökök számára .NET-alapú jelszóbővítmény-DLL-t (dinamikus csatolású függvénytárat) hozhat létre.
Valahányszor ezek az ügynökök jelszómódosítási vagy jelszóbeállítási hívást kezdeményeznek, a hívást a .NET-alapú jelszóbővítmény-DLL fogadja. Az ügynökök jelszóbővítmény-beállításainak konfigurálása a Synchronization Service Manager eszközön keresztül történik. A jelszóbővítmények konfigurálásáról a FIM fejlesztői leírásában talál további információt.

| Az alábbi szolgáltatások kezelőügynökei alapértelmezés szerint támogatják a jelszókezelést: | Jelszóbővítmény használatával a következő szolgáltatások kezelőügynökei is támogatják a jelszókezelést: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | Attribútum-érték párokból álló szövegfájlok                                                                    |
| Active Directory Lightweight Directory-szolgáltatások (ADLDS)                   | Tagolt szövegfájlok                                                                               |
| IBM Directory Server                                                      | Directory Services Markup Language (DSML)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | Rögzített szélességű szövegfájlok                                                                             |
| Sun- és Netscape-címtárkiszolgálók                                        | IBM DB2 Universal Database                                                                         |
|                                                                           | LDAP Data Interchange formátum (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Oracle Database                                                                                    |

## <a name="password-synchronization"></a>Jelszó-szinkronizálás


A jelszó-szinkronizálás az Active Directory-tartományok jelszóváltozás-értesítési szolgáltatásával együttműködve lehetővé teszi, hogy az Active Directoryból származó jelszóváltozások automatikusan eljussanak a csatlakoztatott adatforrásokhoz. A MIM úgy hajtja ezt végre, hogy egy távoli eljáráshívási kiszolgálót futtat, amely az Active Directory-tartományvezérlőktől érkező jelszóváltoztatási értesítéseket figyeli. A jelszóváltoztatási kérelem beérkezése és hitelesítése után a MIM feldolgozza azt, és eljuttatja a megfelelő kezelőügynökökhöz.

>[!IMPORTANT]
A MIM nem támogatja a kétirányú jelszó-szinkronizálást. A kétirányú jelszó-szinkronizálás konfigurálása hurkot hozhat létre, amely fokozottan igénybe veszi a kiszolgáló erőforrásait, és negatívan befolyásolhatja az Active Directoryt és a MIM-et egyaránt.

A jelszóváltozás-értesítési szolgáltatás az Active Directory minden tartományvezérlőjén fut. A jelszóváltoztatási értesítést fogadó rendszert célnak nevezzük. A MIM-kiszolgálót a jelszóváltozás-értesítési szolgáltatás céljaként kell konfigurálni az Active Directoryban, még a jelszóváltoztatási értesítések kiküldése előtt. A jelszóváltoztatás-értesítési szolgáltatás konfigurációjában meg kell adni egy belefoglalási csoportot, és igény esetén egy kizárási csoportot. E csoportok használata korlátozza az érzékeny jelszavak kijutását a tartományból. Például ha minden felhasználónak jelszót kíván küldeni, a rendszergazdáknak azonban nem, akkor ajánlatos a tartományi felhasználókat belefoglalási csoportként, a tartománygazdákat pedig kizárási csoportként megadnia. A jelszómódosítás konfigurálásáról a [Using Password Synchronization](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx) (Jelszó-szinkronizálás használata) című cikkben talál további információt.

A jelszó-szinkronizálás folyamatában a következő összetevők kapnak szerepet:

-   **Jelszóváltoztatás-értesítési szolgáltatás (Pcnssvc.exe)** – A jelszóváltoztatás-értesítési szolgáltatás egy tartományvezérlőn fut, és az a feladata, hogy fogadja a helyi jelszószűrőtől érkező jelszóváltoztatási értesítéseket, várólistára tegye őket a MIM-et futtató célkiszolgáló számára, és távoli eljáráshívással kézbesítse őket. A szolgáltatás titkosítja a jelszót, és gondoskodik róla, hogy az biztonságban maradjon addig, amíg sikeresen kézbesítésre kerül a MIM-et futtató célkiszolgálón.

-   **Egyszerű szolgáltatásnév (SPN)** – Az SPN az Active Directory azon fiókobjektumának tulajdonsága, amelyet a Kerberos protokoll használ a jelszóváltoztatás-értesítési szolgáltatás és a cél kölcsönös hitelesítésére. Az SPN gondoskodik róla, hogy a jelszóváltoztatás-értesítési szolgáltatás a MIM-et futtató kiszolgálók közül csak a megfelelőt hitelesítse, és egyetlen más szolgáltatás se juthasson hozzá a jelszóváltoztatási értesítésekhez. Az SPN létrehozása és hozzárendelése a setspn.exe eszköz használatával történik. Az SPN konfigurálásáról a Jelszó-szinkronizálás használata című cikkben talál további információt.

-   **Jelszóváltoztatás-értesítési szűrő (Pcnsflt.dll)** – A jelszószűrő használatával egyszerű szöveges jelszavak kérhetők le az Active Directoryból. Ezt a szűrőt a helyi biztonsági szervezet (LSA) tölti be minden Windows Server-tartományvezérlőre, amely részt vesz a jelszavaknak a MIM-et futtató célkiszolgálókra való eljuttatásában. A szűrő telepítése és a tartományvezérlő újraindítása után a szűrő a tartományvezérlőről származó jelszóváltozásokról jelszóváltoztatási értesítéseket kap. A jelszóértesítési szűrő a tartományvezérlőn működő többi szűrővel párhuzamosan fut.

-   **A jelszóváltoztatás-értesítési szolgáltatás konfigurációs segédprogramja (Pcnscfg.exe)** – A pcnscfg.exe segédprogram használatával kezelhetők és karbantarthatók a jelszóváltoztatás-értesítési szolgáltatás Active Directoryban tárolt konfigurációs paraméterei. Ezekre a konfigurációs paraméterekre, köztük a célkiszolgálók meghatározására, a várólistás jelszavak újrapróbálkozási időközére, valamint a célkiszolgáló engedélyezésére, illetve letiltására akkor van szükség, amikor a rendszer jelszóértesítéseket küld a MIM-et futtató célkiszolgálóra.
    A szolgáltatás konfigurációját az Active Directory tárolja, ezért elég egyetlen tartományvezérlőn frissíteni. Az Active Directory az összes többi tartományvezérlőn replikálja a változást.

-   **Távoli eljáráshívási (RPC) kiszolgáló a MIM-et futtató kiszolgálón** – Ha engedélyezve van a jelszó-szinkronizálás, az RPC-kiszolgáló működésbe lép a MIM-et futtató kiszolgálón, és lehetővé teszi számára, hogy értesítéseket fogadjon a jelszóváltozás-értesítési szolgáltatástól. Az RPC dinamikusan választja ki a használt portok tartományát. Ha azt szeretné, hogy a MIM tűzfalon keresztül kommunikáljon az Active Directory-erdővel, meg kell nyitnia egy porttartományt.

-   **Jelszóbővítmény-DLL** – A jelszóbővítmény-DLL segítségével a jelszóbeállítási vagy jelszóváltoztatási műveletek szabálykiterjesztéssel hajthatók végre tetszőleges adatbázis-, bővíthető kapcsolati vagy fájlalapú kezelőügynökön.
    Ez egy „export_password” nevű, csak exportálási, titkosított attribútum létrehozásával történik, amely ténylegesen nem létezik a kapcsolt címtárban, de a szabálykiterjesztések kiépítése esetén hozzáférhető és beállítható, vagy az attribútumfolyam exportálása során használható fel. A jelszóbővítmények konfigurálásáról a [FIM fejlesztői leírásában](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx) talál további információt.

## <a name="preparing-for-password-synchronization"></a>A jelszó-szinkronizálás előkészítése

Mielőtt jelszó-szinkronizálást állítana be a MIM és az Active Directory-környezet számára, győződjön meg a következőkről:

-   A MIM a telepítési utasításoknak megfelelően telepítve van.

-   A jelszó-szinkronizáláshoz kezelni kívánt csatlakoztatott adatforrások kezelőügynökeit már létrehozták, továbbá folyamatban van az objektumok sikeres csatlakoztatása és szinkronizálása.

A jelszó-szinkronizálás beállításához:

-   Terjessze ki az Active Directory-sémát, hogy hozzáadhassa a jelszóváltoztatás-értesítési szolgáltatás telepítéséhez és futtatásához szükséges osztályokat és attribútumokat.

-   Telepítse a jelszóváltoztatás-értesítési szolgáltatást minden tartományvezérlőn.

-   Konfigurálja az Active Directoryban a MIM-szolgáltatásfiókhoz tartozó egyszerű szolgáltatásnevet (SPN).

-   Konfigurálja a jelszóváltoztatás-értesítési szolgáltatást a célként meghatározott MIM-szolgáltatással történő kommunikációra.

-   Konfigurálja a jelszó-szinkronizálás kapcsán felügyelendő csatlakoztatott adatforrások kezelőügynökeit.

-   Engedélyezze a jelszó-szinkronizálást a MIM-en.

A jelszó-szinkronizálás beállításáról a Using Password Synchronization (Jelszó-szinkronizálás használata) című cikkben talál további információt.

## <a name="password-synchronization-process"></a>A jelszó-szinkronizálás folyamata

Az alábbi ábrán egy Active Directory-beli tartományvezérlő által a csatlakoztatott adatforrásokhoz küldött jelszóváltoztatási kérelem szinkronizálási folyamata látható:

1.  A felhasználó a Ctrl+Alt+Del billentyűkombináció lenyomásával elindítja a jelszóváltoztatási kérelmet. A rendszer a jelszóváltoztatási kérelmet az új jelszóval együtt elküldi a legközelebbi tartományvezérlőre.

2.  A tartományvezérlő rögzíti a jelszóváltoztatási kérelmet, és értesíti a jelszóváltoztatás-értesítési szűrőt (Pcnsflt.dll).

3.  A jelszóváltoztatás-értesítési szűrő továbbítja a kérést a jelszóváltoztatás-értesítési szolgáltatáshoz.

4.  A jelszóváltoztatás-értesítési szolgáltatás ellenőrzi a jelszóváltoztatási kérelmet, majd a Kerberos használatával hitelesíti az egyszerű szolgáltatásnevet (SPN), és titkosított távoli eljáráshívásban továbbítja a jelszóváltoztatási kérelmet a MIM-célkiszolgálóhoz.

5.  A MIM érvényesíti a forrás-tartományvezérlőt, majd a tartománynév alapján megkeresi a tartományhoz tartozó kezelőügynököt, és a jelszóváltoztatási kérelemben található felhasználóifiók-információ segítségével megkeresi a megfelelő objektumot az összekötőtérben.

6.  Az illesztési tábla információinak felhasználásával a MIM meghatározza azokat a kezelőügynököket, amelyek megkapják a jelszóváltoztatást, majd le is küldi azt nekik.

## <a name="password-synchronization-security"></a>A jelszó-szinkronizálás biztonsága

A jelszó-szinkronizálás során a következő biztonsági intézkedések történnek:

-   Hitelesítés a jelszó forrásától – A jelszóváltoztatási értesítés átvétele után a MIM és a forrás-tartományvezérlő is elvégzi a Kerberos-hitelesítést a címzett és a feladó érvényességének együttes ellenőrzésére. A jelszóváltoztatási értesítés átvétele után a MIM biztosítja, hogy a hívónak fiókja legyen a hozzá tartozó tartomány tartományvezérlő-tárolójában.

-   Nem biztonságos kapcsolódás miatt sikertelen a célként szolgáló adatforráshellyel való jelszó-szinkronizálás – Ha a kezelőügynököt csak a biztonságos kapcsolódás elfogadására konfigurálták, és nem észlel ilyet, akkor a szinkronizálás sikertelen lesz.
    A szinkronizálás megtörténik, ha a kezelőügynököt a nem biztonságos kapcsolatok elfogadására is konfigurálták. A nem biztonságos kapcsolatokat csak a járulékos kockázatok megvizsgálása és megértése után szabad engedélyezni.

-   Biztonságos jelszótárolás – A MIM csak titkosított jelszavakat tárol, ideiglenesen. Minden jelszót, amely a jelszóváltoztatás-értesítési művelet során a MIM-hez kerül, a MIM-jelszókezelés folyamatába lépés pillanatában azonnal titkosít a rendszer.
    Abban a pillanatban, hogy a jelszó sikeresen eljutott a céloldali csatlakoztatott adatforráshoz, a titkosítás feloldódik, és a jelszót tároló memória azonnal törlődik. Ha a művelet során nem sikerül írni a céloldali csatlakoztatott adatforrásba, a memória addig tárolja a titkosított jelszót, amíg az összes újrapróbálkozás le nem fut, majd törlődik.

-   Biztonságos jelszóvárólisták – A jelszóváltoztatás-értesítési szolgáltatás várólistáin tárolt jelszavak a kézbesítésig titkosítva vannak.

## <a name="password-synchronization-error-recovery-scenarios"></a>A jelszó-szinkronizálási hibák utáni helyreállítás esetei

Ideális esetben valahányszor egy felhasználó jelszót módosít, a módosítás hibátlanul szinkronizálódik. A MIM az alábbi forgatókönyvek szerint végez helyreállítást a leggyakoribb szinkronizálási hibák után:

-   **Nem sikerült a jelszóértesítés eljuttatása az Active Directoryból a MIM-be** – Ez akkor következhet be, ha a hálózat nem működik, vagy a MIM-et futtató kiszolgáló nem érhető el. A jelszóváltoztatási értesítés helyi várólistán marad a jelszóváltoztatás-értesítési szolgáltatás tartományvezérlőjén. A jelszóváltoztatás-értesítési szolgáltatás a konfigurációjának megfelelő újrapróbálkozási időközzel ismét megkísérli az értesítést.

-   **Nem sikerült a jelszó szinkronizálása a céladatforrásba** – Ez szintén akkor következhet be, ha a hálózat nem működik, vagy a céladatforrás nem érhető el.
    A jelszóváltoztatási értesítés várólistára kerül, és a rendszer ismét megkísérli a műveletet a kezelőügynök újrapróbálkozásra és újrapróbálkozási időközökre vonatkozó konfigurációjának megfelelően. Az újrapróbálkozásig titkosítva tárolódnak a jelszavak, és a művelet sikeres végrehajtása vagy az újrapróbálkozási korlát elérése után törlődnek.

-   **MIM-et futtató üzemi tartalékkiszolgáló aktiválása a meghibásodás után** – Ha a MIM-et futtató elsődleges kiszolgáló meghibásodik, a jelszó-szinkronizáláshoz konfigurálhat és a jelszómódosítás elvesztése nélkül aktiválhat egy üzemi tartalékkiszolgálót. További információkért lásd a [MIISactivate: Server Activation Tool](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx) (MIISactivate: Kiszolgálóaktiválási eszköz) című cikket.

Egyes, súlyosabb meghibásodások esetén akárhány újrapróbálkozás esetén sem valószínű, hogy sikerül a művelet. Ilyenkor a rendszer naplózza a hibát, és leállítja a folyamatot. A következő eseményeknél nincs újrapróbálkozás:

| Esemény | Severity    | Leírás                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Adatok | A jelszó-szinkronizálás beállításának műveletét a rendszer nem hajtotta végre, mert az időbélyegző elavult.                                                                      |
| 6921  | Hiba       | A jelszó-szinkronizálás beállításának művelete nincs feldolgozva, mert a céloldali kezelőügynökön nincs engedélyezve a jelszókezelés.                                |
| 6922  | Hiba       | A jelszó-szinkronizálás beállításának művelete nincs feldolgozva, mert a céloldali kezelőügynökön nincs konfigurálva a jelszókezelés.                             |
| 6923  | Figyelmeztetés     | A jelszó-szinkronizálás beállításának művelete nincs feldolgozva, mert a céloldali összekötőtér objektuma nem található a csatlakoztatott címtárban.                  |
| 6927  | Hiba       | A jelszó-szinkronizálás beállításának művelete nem sikerült, mert a jelszó nem felel meg a célrendszer jelszószabályzatának.                                      |
| 6928  | Hiba       | A jelszó-szinkronizálás beállításának művelete nem sikerült, mert a céloldali kezelőügynök jelszókiterjesztése nincs konfigurálva a jelszóbeállítási műveletek támogatására. |

## <a name="user-based-password-change-management"></a>Felhasználóalapú jelszóváltoztatás-kezelés

A MIM két webalkalmazást kínál az új jelszavak Windows Management Instrumentation (WMI) használatával való kéréséhez. Ahogy a jelszó-szinkronizálásnál, itt is akkor aktiválhatja a jelszókezelést, amikor a kezelőügynök-tervezőben konfigurálja a kezelőügynököt. A jelszókezelésről és a WMI-ről a MIM fejlesztői leírásában talál további információt.

A telepítés során a MIM két biztonsági csoportot hoz létre kifejezetten a jelszókezelési műveletek támogatására:

-   FIMSyncBrowse – E csoport tagjai számára a WMI-lekérdezésekkel történő keresési műveletek során engedélyezett az információgyűjtés a felhasználó fiókjairól.

-   FIMSyncPasswordSet – E csoport tagjai számára engedélyezettek a fiókkeresési, jelszóbeállítási és jelszómódosítási műveletek, amikor a WMI-vel használják a jelszókezelési felületet.

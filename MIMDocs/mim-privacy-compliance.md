---
title: A Microsoft Identity Manager adatkezeléssel |} A Microsoft Docs
description: Megismerheti a Microsoft Identity Manager adatkezelési felismerést és a jelentés a környezeti adatok, a művelet végrehajtása a megadott system üzemeltetési funkciókat és követelmény alapján.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/02/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: f75eb69360852c9f629b60d4900638c8b51e068a
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825790"
---
# <a name="microsoft-identity-manager-data-handling"></a>A Microsoft Identity Manager-adatok kezelése 

Ez a cikk útmutatást nyújtanak a hogyan szervezetek döntéseket hozhat, amely több csatlakoztatott adatforrások között is alkalmazható.  Ez lehet elérni, a keresés, törlés, frissítés és a jelentés az operations.  Mielőtt eldönti, frissítése és törlése a megközelítést, a jelenlegi kialakítás és az identity manager rendszer (MIM) konfigurációjának megértését, kritikus fontosságú. 

Az alábbiakban néhány forgatókönyv-ügyfeleknek kell gondolja át, és a következő kérdések megválaszolásával: 

- Milyen adatokat kell, identity Management az üzleti folyamat segítségével?
- Hol aktuális adatokat fog tárolni a MIM-ben?
- Hogyan fogja használják ezeket az adatokat a rendszer?
- Osztanak-e bármilyen külső partnerekkel adatok sources(Exporting) ezeket az adatokat
- Mi az a mérvadó forrás, az adatok és a feldolgozásuk korlátozását?
- Mi lesz az adatok megőrzésére és az adatok törlésének terv?
- Azonosított kell feldolgozni és adatkezelés minden technológiát?

Segítségével megismerheti az aktuális MIM-környezet a MIM-környezet dokumentálása, vagy megvalósítási terv dokumentumaira késlelteti a következő eszközt használhat.
- [MIM dokumentáló – lehetővé teszi, hogy a jelenlegi konfiguráció exportálása](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Keresse meg, majd a személyes adatok azonosítása
A MIM adatok keresése a konfigurációs és beállítási függ lesz. A legtöbb környezetben vannak összekapcsolva, de az átláthatóság érdekében azt érvénytelenítése őket magas szintű összetevő.

### <a name="synchronization-service"></a>Synchronization Service

Minden adat, amely kapcsolódik a felhasználók a MIM az Active Directory (AD) és a HR-adatforrások származik. Személyes adatok keresése, érdemes lehet a Keresés az első hely AD vagy csatlakoztatott adatforrások. 

Ha nem biztos a mérvadó forrás lehet nyomon követheti ezt a felhasználót a MIM Synchronization Service Manager konzol, kattintson a Keresés a Metaverzumban sávon az azonosítható az adatbázisban tárolt személyes adatok megtekintéséhez. Felhasználók egy adott felhasználó vagy az attribútum kereshet.

- Tekintse át vagy a felhasználói objektumok adatok keresése
    - Nyissa meg a szinkronizálási szolgáltatás ügyfele
        - Használatával a metaverzumba designer lehetővé teszik, megtekintheti az attribútum a folyamat importálja a és a fontossági sorrend.
![a mim-adatvédelem-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - A Keresés a metaverzumban használata lehetővé teszi bármely olyan objektum, és az adatbázison belül attribútum keresésére ![mim-adatvédelem-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Miután megtalálta az objektum, az objektum kattint nyílik meg a felhasználói profil oldalához. Az objektum részletei, és a teljes körű részleteit, az attribútumokat, az utolsó módosítás és felügyeleti ügynök konfigurációja az alábbi példa származó mérvadó forrás és a kapcsolódó csatlakoztatott adatforráshoz adja meg.

![a mim-adatvédelem-megfelelőségi. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Szolgáltatás és portál / PAM
Ha egy példányát a szolgáltatás és portál vagy a PAM telepítése folyamatban lehet keresni a felhasználók fontos. 

Ha telepítette a portálon, a felhasználói felület segítségével bármely attribútum vagy a lekérdezés egy adott felhasználó keresése.

Ha csak akkor a szolgáltatás kiszolgáló (nélkül a portál felhasználói Felületét) telepítve van egy keresési szintaxist a [FIMAutomation PSSnapin] alapján is futtathatja, a példában található [Itt](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

A PAM használhatja a fenti ugyanazt a szintaxist, vagy használhatja a [MIMPAM modul](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) kifejezetten a get-PAM-felhasználó parancsmag és keresse meg a felhasználót a PAM-környezetben.

A szolgáltatás és -portál van más jelentéskészítési lehetőségek a rendelkezésre álló adatok kereséséhez.
- [A hibrid jelentéskészítés?](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [SCSM-jelentéskészítés](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
A Bhold Core szolgáltatás egy felhasználói felület, amely lehetővé teszi, hogy a keresés egy felhasználó vagy attribútumok rendelkezik. 

![a bhold-keresés](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Ha a BHOLD szinkronizál [elérésére a management-összekötő](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) szinkronizálási szolgáltatás a BHOLD core küld a csatlakoztatott felhasználói objektumok és attribútumok látni fogja.

A BHOLD Reporting modul is betöltheti.

- [A BHOLD Reporting](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Tanúsítványkezelés
Tanúsítvány-felügyeleti szolgáltatás keresés a felhasználói felület be van építve. A rendszergazda elindul, és válassza ki a "felhasználói és a nézet vagy azok adatainak kezelése"  

![cm-keresés](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Személyes adatok exportálása
Az MIM-ben entitásokkal kapcsolatos adatok több forrásból származnak, mert a szinkronizálási szolgáltatás adatbázisban tárolt legtöbb adatot. Ebből kifolyólag a MIM Sync objektum kapcsolatos adatokat kell exportálnia, vagy megadhatja, hogy ezek az adatok tulajdonosa.

### <a name="synchronization-service"></a>Synchronization Service
Szinkronizálási szolgáltatások esetén az adatok exportálása egyszerűen válassza ki az adatokat a keresési felhasználói felület és a példányt, és illessze be a fürt megosztott kötetei szolgáltatás vagy az előnyben részesített formátumban. Exportálja az adatokat egy másik módja, ha egy fájl alapú MA szükséges a lényeges megjelölt felhasználókkal kapcsolatos jelenlegi adatokat. Egy fájl alapú MA a példában található [Itt](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Szolgáltatás és portál / PAM
Szolgáltatás és portál exportálhatja az adatokat, futtassa a [FIMAutomation PSSnapin] alapján keresési szintaxist PAM együtt, a példában található [Itt](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) és átadhatja azt [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

A PAM használhatja a fenti ugyanazt a szintaxist, vagy használhatja a [MIMPAM modul](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) kifejezetten a get-PAM-felhasználó keresse meg a felhasználót a PAM-környezetben, és átadhatja azt egy CSV kötethez.

- [Példa a PowerShell használatával a MIM szolgáltatás lekérdezése](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
A Bhold-adatok használata a bhold reporting modul az előnyben részesített formátumban exportálhatók.

### <a name="certificate-management"></a>Tanúsítványkezelés
Személyes adatokkal kapcsolatos felügyeleti tanúsítványadatokat az active directory csatlakozik. A rendszergazda exportálhatja ezeket az adatokat az Active Directory powershell-lel.

## <a name="updating-personal-data"></a>Személyes adatok frissítése

Felhasználók és a MIM-megoldások objektumokról személyes adatokat általában származik a felhasználói objektum a szervezet csatlakoztatott adatforrások. Mivel a végrehajtott módosítások a felhasználói profilhoz a HR-forrás, vagy egy másik mérvadó rendszerben, például az AD majd a MIM Synchronization Service is megjelennek.

### <a name="synchronization-service"></a>Synchronization Service

Kezelési műveletek végrehajtásához a rendszergazdák a szinkronizálási műveletek vagy a rendszergazda meghatározott részét kell [Itt](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

Adatok frissítését végzi el a forrás-hitelesítésszolgáltató szabályok definiálása. Felügyeleti konzol segítségével azonosítsa a forrást, így a forrásnál frissíteni. Egy másik lehetőség, hozzon létre a szinkronizálási szabály vagy a szabály által szabályozhatja az adatok frissítése Ha forrásokhoz, például a HR-adatok továbbra is kell maradnia. Ezek a beállítások avialible támogatott.

Attribútum frissítése különböző módjairól további információkért lásd az alábbi. 

- [Szabályok bővítményekkel](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [A külső rendszerekkel való adatszinkronizálás ismertetése](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Szolgáltatás és portál / PAM

Szolgáltatás és -portál a PAM-adatait tartalmazzák a FIMAutomation vagy a PAM-parancsmagok használatával lehet frissíteni. Ha a portálon, közvetlenül is frissítheti a kereséssel és módosítani az objektumot. Egy dolog, vegye figyelembe, és egyszerűen frissítésével a portálról konfigurációjától függően nem jelenti azt, megmarad. Mivel a mérvadó forrás nagymértékben függ a teljes konfigurációs.

### <a name="bhold"></a>BHOLD

Felhasználók közvetlenül lehet frissíteni a BHOLD Core felhasználói felület vagy az access management-összekötő.

### <a name="certificate-management"></a>Tanúsítványkezelés

A felügyeleti szolgáltatás a felhasználók olyan tükre az active Directoryból. Használja az objektum adatainak módosítása az Active Directory frissítéséhez.

## <a name="deleting-personal-data"></a>Személyes adatok törléséről

>[!Note] 
> Ez a cikk útmutatást nyújt a személyes adatok törlése a Microsoft Identity Managerből módjai, és támogatja a GDPR szerint Ön nem használható. A GDPR-ral kapcsolatos általános információt a [Service Trust Portal GDPR szakaszában](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted) talál.

A MIM-ben adatok szinkronizálva és mindig a csatlakoztatott adatforrás frissítve lett. Ha egy objektumot törölnek a célhelyen, az objektum adatainak a MIM-ben lehessen őrizni az biztonsági vizsgálat céljából. Objektumtörlés egy csatlakoztatott data source szabályok vagy szabály extension(code) és/vagy objektumtörlési szabályok van konfigurálva.

### <a name="synchronization-service"></a>Synchronization Service
Szinkronizálási szolgáltatás az adatok kezeléséhez, vagy töröljön adatokat, attól függően, üzleti folyamatok, számos módon. Hogy jobban elképzelhesse, az alábbiakban néhány cikkeket, amelyek segítségével, törlése és frissítése az attribútumok lehetőségek ismertetése: 

- [A megszüntetés ismertetése](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Szabályok bővítményekkel](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [A MIM-ajánlott eljárások](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Szolgáltatás és portál / PAM

A szolgáltatás és portál, hogy őrizze meg az alapértelmezett 30 napig system resource megőrzésének konfigurálása ajánlott. Ez tájékoztatja a szolgáltatást, ha törli, nem csak a kérelem adatai, de az is, amelyet a rendszer törölni kell minden olyan objektum. A folyamat akkor fordul elő, ha ehhez az objektumhoz társított minden adatot az törlődni fog ez az összes SSPR regisztrációs adatait tartalmazza. Ez az objektum törlése a fenti konfigurációs be játszik le. Van egy olyan táblát is tároljuk az objektumok GUID azonosítóját. A build 4.4.1459 hozzáadtunk egy FIM_DeleteExpiredSystemObjectsJob részletek nevű ezt a folyamatot a folyamat tekintheti meg a tábla teljes méretének csökkentése érdekében [Itt](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![a mim-adatvédelem – megfelelőség – srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold, például a legtöbb rendszer csatlakozik a szinkronizálási szolgáltatás beállítható úgy, hogy egyszer az adatforrás-objektum törlése, például a HR törlődik. Ez a felügyeleti ügynök van konfigurálva. és a Objektumtörlés szabályok leírtaknak megfelelően a szinkronizálást szolgáltatásai által szabályozható.

Egy másik lehetőség, hogy távolítsa el a user objektum közvetlenül a BHOLD Core felhasználói felületen. Telepítő függően ez sikerült jól működnek, azonban vegye figyelembe a logikai kiépítés sikerült újra létrehozni a felhasználó, ha nem törölte a forráson.
![a mim-adatvédelem – megfelelőség – bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Tanúsítványkezelés
Felhasználó eltávolítása a CM, a felhasználó törlése az active directory szerepel.

A tanúsítványkezelés, mert csak a profil uid a tanúsítványszolgáltatás a tartomány sAMAccountName tárolja. Miután a felhasználó AD a felhasználói gyorsítótár nem csak a tanúsítványok jelen regisztrálnak váltás törlődik. Nem ajánlott törlése semmit az adatbázisban, mivel ez a művelet a környezet általános kárt okozhat.

## <a name="opt-out-of-telemetry"></a>Az elutasítás telemetria
FIM vagy MIM használt előző buildek minden egyes üzembe helyezéssel kapcsolatos anonimizált telemetriai adatokat gyűjt, és HTTPS-kapcsolaton keresztül továbbítja ezeket az adatokat a Microsoft-kiszolgálók. Ezeket az adatokat a Microsoft által használt segítheti a FIM vagy MIM későbbi verzióiban az elmúlt.

>[!Note] 
> A későbbi 4.5.x.x vagy nagyobb, adatok gyűjtése letiltásra kerül.

Tiltsa le az adatok korábbi verziójában, futtassa a gyűjtemény üzemmódjának módosítása, és kapcsolja ki a következő üzenet:

![a mim-adatvédelmi – megfelelőség – felhasználói élmény fokozása program. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

vagy szerkessze a beállításjegyzéket, és állítsa az értékét 0-ra: (összetevő) felhasználói élmény fokozása program HKLM\SOFTWARE\Microsoft\Forefront identitás Manager\2010

![a mim-adatvédelem – megfelelőség – ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>További lépések 
- [SQL kapcsolódó adatvédelmi útmutató](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [A szolgáltatás Szolgáltatásmegbízhatósági portálon GDPR szakasza](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 archív: Felkészülési – a Forefront Identity Manager 2010 megvalósítása](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)

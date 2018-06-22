---
title: A Microsoft Identity Manager adatkezeléssel |} Microsoft Docs
description: Megérteni a Microsoft Identity Manager adatkezelési felismerést környezeti adatok jelentést, és hajtsa végre a műveletet a megadott a rendszer működési funkciókat és követelmény alapján.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldiwn
ms.date: 05/22/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6bcf9ab26ba38f3c6eefbdb315d4975320a597b9
ms.sourcegitcommit: 66db63fe2813130764e52381f4f9c8e549d77d39
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/22/2018
ms.locfileid: "34449698"
---
# <a name="microsoft-identity-manager-data-handling"></a>A Microsoft Identity Manager kezelése 

Ez a cikk pedig útmutatást nyújtanak hogyan szervezet lehetséges, hogy a keresés, törlés, frissítése és jelentése műveletek a szervezet fog döntéseket kell meghoznia annak gyakorlat vagy megvalósítása között számos csatlakoztatott adatforrások keresztül. Mielőtt eldönti, törlése vagy megértéséhez frissítése a módszer az aktuális terv és az identity manager rendszer (MIM) konfigurációs fontos. Az alábbiakban néhány forgatókönyv ügyfél kell figyelembe venni, és a következő kérdések: 

- Milyen adatokat kell meg identitáskezeléshez üzleti folyamat számára?
- Ahol aktuális adatokat fog tárolni a MIM-ben?
- Hogyan fogja használhatók ezeket az adatokat a rendszer?
- Akkor oszt meg ezeket az adatokat bármely külső partnerekkel adatok sources(Exporting)
- Mi az az adatokat, majd azt a feldolgozása a mérvadó forrása?
- Mit fog az adatok megőrzése és az adatok törlése terv?
- Azonosított kell feldolgozni, és adatok kezelésében az összes technológia?

Megismerheti az aktuális MIM-környezet a következő eszközt, amely dokumentálja a MIM-környezet, vagy a megvalósítás tervezési dokumentumok késleltető használhatja.
- [MIM dokumentáló - lehetővé teszi a jelenlegi konfiguráció exportálása](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Keres, és a személyes adatok azonosítása
MIM adatainak keresése fognak függeni a konfiguráció és a telepítőt. A legtöbb környezetben vannak összekapcsolva, de az átláthatóság érdekében azt túllépte őket magas szintű összetevő.

### <a name="synchronization-service"></a>Synchronization Service

MIM felhasználók kapcsolódó összes adatot Active Directory (AD) és a HR-adatok forrásból származik. Személyes adatok keresése, amikor az első hely-érdemes rákeresve-e AD vagy csatlakoztatott adatforrások. 

Ha nem biztos a forrás hatóság nyomon követheti a felhasználó a MIM Synchronization Service Manager-konzolról, a Keresés a Metaverzumban elemre az adatbázisban tárolt azonosítható személyes adatok megtekintéséhez. Felhasználók kereshet egy adott felhasználó vagy az attribútum.

- Felülvizsgálati vagy a felhasználói objektumok adatok keresése
    - Nyissa meg a szinkronizálási szolgáltatás ügyfele
        - A metaverse használata designer lehetővé láthatja az Attribútumfolyam importálja és prioritását.
![a mim-adatvédelmi-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - A metaverzum-keresés használata lehetővé teszi, hogy az bármely objektum és az adatbázis attribútum ![mim-adatvédelmi-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Után az objektum keresése, az objektum kattintva megnyílik a felhasználói oldalon. Az objektum részletei adja meg, akkor az objektum, a hozzá tartozó attribútumok átfogó részleteivel utolsó módosítás és felügyeleti ügynök konfigurációja az alábbi példa származó hatóság forrását, és a kapcsolódó csatlakoztatott adatforrás.

![a mim-adatvédelmi-megfelelőség. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Szolgáltatás és -portál / PAM
Ha a szolgáltatás egy példánya, és telepítve alatt portál vagy a PAM felhasználók megkereshetik fontos. 

Ha telepítette a portált, a felhasználói felület segítségével bármely attribútum, vagy a lekérdezés egy adott felhasználó keresése.

Ha Ön csak a szolgáltatás-kiszolgáló (nélkül portál felhasználói felületének) telepítve van egy keresési szintaxisának [FIMAutomation PSSnapin] alapján is futtathatja, példa található [Itt](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

A PAM használhatja a fenti ugyanazt a szintaxist, vagy használhatja a [MIMPAM modul](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) kifejezetten a get-PAM-felhasználó parancsmaggal keresse meg a felhasználót a PAM-környezetben.

A szolgáltatás és -portál van más jelentéskészítési lehetőségek a rendelkezésre álló adatok kereséséhez.
- [A hibrid jelentéskészítés](https://docs.microsoft.com/en-us/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Jelentéskészítés a SCSM](https://docs.microsoft.com/en-us/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Bhold alapvető szolgáltatásához rendelkezik, amely lehetővé teszi, hogy az a felhasználó vagy attribútumok felhasználói Felületet. 

![bhold-keresés](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Ha a BHOLD szinkronizál [management-összekötő hozzáférési](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-access-management-connector-install) szinkronizálási szolgáltatás a BHOLD core küld a csatlakoztatott felhasználói objektumok és attribútumok látni fogja.

A BHOLD Jelentéskezelő modul is letölthető.

- [BHOLD-jelentés](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Tanúsítványkezelés
Tanúsítvány-management szolgáltatás keresés a felhasználói felület van beépítve. A rendszergazda elindul, és válassza ki a "felhasználói és a nézet vagy az adatok kezelése"  

![cm-keresés](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Személyes adatok exportálása
Az MIM-ben entitásokkal kapcsolatos adatok több forrásból származnak, mert a legtöbb adatok tárolódnak a szinkronizálási szolgáltatás adatbázis. Emiatt a MIM Sync kell exportálnia a objektum kapcsolatos adatokat, vagy azt is meghatározhatja, ezek az adatok tulajdonosa.

### <a name="synchronization-service"></a>Synchronization Service
Szinkronizálási szolgáltatások az adatexportálási egyszerűen válassza ki a felhasználói felület keresési adatait, és másolja és illessze be a fürt megosztott kötetei szolgáltatás vagy az előnyben részesített formátumban. Ezek az adatok exportálása egy másik módja, ha eldobása szükséges érdeklő megjelölt felhasználókkal kapcsolatos jelenlegi adatokat a fájl alapú MA létrehozása. A fájl alapú MA egy exmaple található [Itt](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Szolgáltatás és -portál / PAM
Szolgáltatás és -portál exportálhatja az adatokat egy keresési szintaxisának [FIMAutomation PSSnapin] alapján futtassa PAM együtt, például található [Itt](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) és átadhatja azt [csv](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

A PAM használhatja a fenti ugyanazt a szintaxist, vagy használhatja a [MIMPAM modul](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) kifejezetten a get-PAM-felhasználó keresse meg a felhasználót a PAM-környezetben, és átadhatja azt egy CSV kötethez.

- [Példa a PowerShell használatával a MIM szolgáltatás lekérdezése](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Bhold adatok exportálhatja a bhold modul jelentéskészítés a megfelelő formátumot használja.

### <a name="certificate-management"></a>Tanúsítványkezelés
Személyes adatokra vonatkozó felügyeleti Tanúsítványadatok csatlakozik-e az active directory. A rendszergazda exportálhatja ezeket az adatokat az Active Directory powershell-lel.

## <a name="updating-personal-data"></a>Személyes adatok frissítése

Felhasználók és a MIM megoldások objektumokról személyes adatokat általában a felhasználó objektumból származik a szervezete csatlakoztatott adatforrások. Mivel a HR forrás-, vagy egy másik mérvadó rendszert, például AD a felhasználói profil módosításai majd jelennek meg a MIM Synchronization Service.

### <a name="synchronization-service"></a>Synchronization Service

Kezelési műveletek végrehajtásához a rendszergazdák szinkronizálási műveletek vagy a rendszergazda meghatározott részét kell [Itt](https://docs.microsoft.com/en-us/previous-versions/mim/jj590183(v%3dws.10)).

Az adatok frissítése a forrás hatóság szabályok felállításával való történik. Felügyeleti konzol azonosítja, hogy frissíti a forrásnál hatóság forrását. Másik lehetőség is szinkronizálási szabály vagy a szabály kiterjesztés szabályozására, ha a forrás, például a HR-adatok továbbra is továbbra is hozzá kell frissítési adatok létrehozása. Ezek a beállítások avialible támogatott.

Többféleképpen is frissíthető az attribútum a további információkért lásd az alábbi. 

- [Szabályok bővítményekkel](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [A külső rendszerekkel való adatszinkronizálás ismertetése](https://docs.microsoft.com/en-us/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Szolgáltatás és -portál / PAM

Szolgáltatás és -portál PAM adatok FIMAutomation vagy a PAM-parancsmag segítségével lehet frissíteni. Ha a portált, közvetlenül is keresve frissíteni és módosítani az objektumot. Egy dolog, ne feledje, és egyszerűen frissítése a portálról konfigurációjától függően nem jelenti azt, marad. Mivel hatóság forrása nagymértékben függ a teljes konfiguráció.

### <a name="bhold"></a>BHOLD

Felhasználók BHOLD Core felhasználói felület vagy a access management-összekötő közvetlenül lehet frissíteni.

### <a name="certificate-management"></a>Tanúsítványkezelés

A tanúsítványkezelő szolgáltatás munkavégzésük tükre az active Directoryból. Használjon objektum adatainak módosítása az Active Directory frissítéséhez.

## <a name="deleting-personal-data"></a>Személyes adatok törlése

>[!Note] 
> Ez a cikk a személyes adatokat a Microsoft Identity Managerből törlés nyújt útmutatást, és a GDPR a kötelezettségeit támogatásához használható. Ha további általános információt GDPR van szüksége, tekintse meg a [GDPR szakasz a szolgáltatás megbízható portál](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

A MIM-ben adatok szinkronizálva és a csatlakoztatott adatforrásból mindig frissítve. Törlésekor objektum célként, a MIM-ben az objektum adatainak biztonsági vizsgálat céljából tarthatjuk fenn. Objektumtörlés csatlakoztatott adatok forrásszabályból vagy szabály extension(code) és/vagy objektumtörlési szabályok / van konfigurálva.

### <a name="synchronization-service"></a>Synchronization Service
Szinkronizálási szolgáltatás képes az adatok kezelésére, vagy töröljön adatokat, attól függően, hogy az üzleti folyamatok, számos módszer. Annak megértésében, az alábbiakban néhány cikkek annak megértésében, törlését és az attribútumok frissítése beállításokat: 

- [A megszüntetés ismertetése](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Szabályok bővítményekkel](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [A MIM gyakorlati tanácsok](https://docs.microsoft.com/en-us/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Szolgáltatás és -portál / PAM

A szolgáltatás & portálon, hogy őrizze meg az alapértelmezett 30 nap erőforrás megőrzési rendszerkonfiguráció ajánlott. Ez a szolgáltatás alapján, ha a művelet törli, nem csak a kérelem adatai, de az is, amelyet a távolítható el a rendszer minden objektum. Amennyiben az a folyamat megy végbe, ehhez az objektumhoz kapcsolódó összes adat törlődik a SSPR regisztrációs adatait tartalmazza. Ez játszik, a fenti objektum törlése konfigurációját. Egy tábla tudunk volt az objektum a guid tároljuk. A tábla a folyamattal FIM_DeleteExpiredSystemObjectsJob részletek nevezett folyamat található hozzáadott 4.4.1459 építés teljes méretének csökkentésére [Itt](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![a mim-adatvédelmi-megfelelőség-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Például a szinkronizálási szolgáltatáshoz való kapcsolódás rendszerek többsége Bhold beállítható úgy, hogy egyszer az adatforrás-objektum törlése, például HR törlődik. Ennek a konfigurációja a kezelőügynököt. és ellenőrzött leírtaknak megfelelően a szinkronizálások szolgáltatásainak Objektumtörlés szabályok.

Lehetősége a user objektum jogosultság eltávolítása a BHOLD központi felhasználói felületen. Attól függően, hogy a telepítő ez sikerült jól működnek, de vegye figyelembe a kiépítés logika újra hozza létre a a felhasználó, ha nem törli a forrás.
![a mim-adatvédelmi-megfelelőség-bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Tanúsítványkezelés
Felhasználó eltávolítása CM, a felhasználó törlése az active Directoryban van.

A tanúsítványkezelés, mert a profil uid tartomány sAMAccountName a tanúsítványszolgáltatások csak tárolja. A felhasználó az Active Directoryból törölt a felhasználó gyorsítótár nincs csak a tanúsítványok, áltás lesz a regisztrált. Nem ajánlott törlése semmit az adatbázisban, mivel ez a művelet a környezet általános kárt okozhat.

## <a name="opt-out-of-telemetry"></a>Lemondja a telemetriai adat
FIM vagy MIM használatával az régebbi buildjei anonimizált telemetriai adatainak egyes központi telepítések gyűjt, és ezeket az adatokat HTTPS-KAPCSOLATON keresztül továbbítja a Microsoft-kiszolgálókra. Ezeket az adatokat Microsoft használta korábban a FIM vagy MIM későbbi verzióiban javítása érdekében.

>[!Note] 
> A későbbi kiadásokban 4.5.x.x vagy nagyobb adatok gyűjtemény letiltásra kerül.

Tiltsa le az adatok futtatása korábbi verziójában gyűjtemény üzemmód módosítása, és kapcsolja ki a következő kérdés:

![a mim-adatvédelmi-megfelelőség-felhasználói élmény fokozása program. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

Állítsa be az értéket 0 és a beállításjegyzék szerkesztése: (összetevő) CEIP HKLM\SOFTWARE\Microsoft\Forefront identitás Manager\2010

![a mim-adatvédelmi-megfelelőség-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>További lépések 
- [Az SQL kapcsolódó adatvédelmi útmutató](https://docs.microsoft.com/en-us/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [A szolgáltatás megbízható portal GDPR szakasza](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 archív: Felkészülési - végrehajtási a Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
---
title: Az adatkezelés Microsoft Identity Manager | Microsoft Docs
description: Ismerje meg Microsoft Identity Manager az adatkezelést, hogy indentify és jelentsen a környezetre vonatkozó adatokról, az adott rendszeren az operatív funkciók és a követelmények alapján végezze el a műveleteket.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: e95cf26b62e582eaa3c07c40e551bc5930d3b1b0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044106"
---
# <a name="microsoft-identity-manager-data-handling"></a>Az adatkezelés Microsoft Identity Manager 

Ez a cikk útmutatást nyújt arról, hogy a szervezetek hogyan hozhatnak döntéseket, amelyek számos összekapcsolt adatforrásra alkalmazhatók.  Ez a keresési, törlési, frissítési és jelentési műveleteken keresztül érhető el.  Mielőtt döntene a törlésről vagy a frissítésről, fontos, hogy megértse az Identity Manager-rendszer aktuális kialakítását és konfigurációját. 

Az alábbiakban néhány forgatókönyv szerint az ügyfeleknek meg kell fontolniuk és meg kell válaszolniuk a következő kérdéseket: 

- Milyen adatokra van szüksége a személyazonosság kezeléséhez az üzleti folyamatok segítése érdekében?
- Hol lesznek a jelenleg tárolt adattárolók?
- Hogyan fogja használni ezeket az adatfájlokat a rendszeren?
- Ezeket az adatforrásokat a külső partnerek adatforrásaival (exportálással) osztja meg.
- Mi az az adatforrások mérvadó forrása és a feldolgozásuk?
- Mi lesz az adatmegőrzési és adattörlési terv?
- Azonosította az összes olyan technológiát, amelyre szüksége van az adatfeldolgozáshoz és az adatkezeléshez?

Az aktuális webszolgáltatási környezet megismerése érdekében a következő eszköz használatával dokumentálhatja a webalkalmazási környezetét, vagy elhalaszthatja a megvalósítási terv dokumentumait.
- [Webkiszolgáló-dokumentáló – lehetővé teszi az aktuális konfiguráció exportálását](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Személyes adatkeresés és-azonosítás
A munkaterületen belüli keresés a konfigurációtól és a beállítástól függ. A legtöbb környezet összekapcsolódik, de az érthetőség érdekében magas szintű összetevővel szakítottuk ki őket.

### <a name="synchronization-service"></a>Synchronization Service

A rendszer a felhasználókhoz kapcsolódó összes olyan adatforrást Active Directory (AD) és HR-adatforrásokból származtatja. Személyes adatkereséskor érdemes megfontolni az AD vagy a csatlakoztatott adatforrások keresését. 

Ha nem biztos abban, hogy a szolgáltatói forrás nyomon követheti ezt a felhasználót a Synchronization Service Manager-konzolon, kattintson a metaverse keresési sávjára az adatbázisban tárolt azonosítható személyes adatok megtekintéséhez. A felhasználók egy adott felhasználót vagy attribútumot kereshetnek.

- Felhasználói objektumok adatai felülvizsgálatának vagy keresésének elvégzése
    - A szinkronizálási szolgáltatás ügyfelének megnyitása
        - A metaverse Designer használatával megtekintheti az attribútumok folyamatának importálását és elsőbbségét.
![a-adatvédelemre compliance_1. PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - A metaverse Search segítségével bármely objektumra és attribútumra rákereshet az adatbázison belül ![a rendszer-adatvédelmi compliance_2. PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Az objektum megkeresése után az objektumra kattintva megnyílik a felhasználói profil oldal. Az objektum részletes adatai az objektumra, annak attribútumaira, utolsó módosítására és forrására, valamint a kapcsolódó csatlakoztatott adatforrásokra vonatkozó részletes információkat tartalmazzák az alábbi felügyeleti ügynök konfigurációs példából származtatva.

![webszolgáltatások – adatvédelem – megfelelőség. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Szolgáltatás és portál/PAM
Ha a szolgáltatás egy példánya, a portál vagy a PAM telepítve van, akkor fontos, hogy a felhasználók keressenek rá. 

Ha telepítette a portált, a felhasználói felületen keresheti meg az adott felhasználóhoz tartozó összes attribútumot vagy lekérdezést.

Ha csak a szolgáltatási kiszolgáló (a portál felhasználói felületének használata nélkül) van telepítve, futtathat egy keresési szintaxist a [FIMAutomation PSSnapin parancsmaggal] alapján, amely például [itt](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)található.

A PAM használhatja ugyanazt a szintaxist, vagy használhatja a [MIMPAM modult](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) , amely kifejezetten a Get-pamuser parancsmaggal keresi a felhasználót a PAM-környezeten belül.

Az elérhető adatkeresésre vonatkozó egyéb jelentési lehetőségek a szolgáltatásban és a portálon találhatók.
- [Hibrid jelentéskészítés](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Jelentéskészítés a SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
A bhold Core szolgáltatás olyan felhasználói FELÜLETtel rendelkezik, amely lehetővé teszi a felhasználók vagy attribútumok keresését. 

![bhold keresés](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Ha szinkronizálási szolgáltatáshoz a [hozzáférés-kezelési összekötővel](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) szinkronizálja a BHOLD, láthatja a csatlakoztatott felhasználói objektumokat és a BHOLD Core-ra küldött attribútumokat.

Emellett betöltheti a BHOLD jelentési modulját is.

- [BHOLD-jelentéskészítés](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Tanúsítványkezelés
A Tanúsítványkezelő szolgáltatás keresése be van építve a felhasználói felületen. A rendszergazda elindítja és kiválasztja a "felhasználó megkeresése és megtekintése vagy kezelése" lehetőséget.  

![cm keresés](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Személyes adatok exportálása
Mivel a következő helyen található entitásokhoz kapcsolódó adatok több forrásból származnak, a legtöbb adat tárolása a szinkronizációs szolgáltatás adatbázisában történik. Emiatt exportálnia kell az objektumokkal kapcsolatos adatait a felhasználói adatok szinkronizálása szolgáltatásból, vagy megadhatja az adatok tulajdonosát.

### <a name="synchronization-service"></a>Synchronization Service
Az adatok exportálására szolgáló szinkronizációs szolgáltatások egyszerűen kiválaszthatják a keresési felület adatait, és a másolást és beillesztést a CSV vagy az előnyben részesített formátumba. Ezen információk exportálásának másik módja, ha egy fájl-alapú MA-t hoz létre, hogy eldobja a megjelölt felhasználóhoz szükséges aktuális adatmennyiséget. [Itt](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/)található egy példában, amely a fájl alapú ma-t használja.


### <a name="service-and-portal--pam"></a>Szolgáltatás és portál/PAM
A szolgáltatás és a portál a PAM-val együtt exportálhatja ezeket az adatkeresési szintaxist a [FIMAutomation PSSnapin parancsmaggal] alapján, példaként [itt](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) található, és áthelyezheti a [CSV](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6)-be.

A PAM használhatja ugyanazt a szintaxist, vagy használhatja a [MIMPAM modult](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) , amely kifejezetten a Get-pamuser segítségével keresi a felhasználót a PAM-környezetben, és áthelyezi azt egy CSV-fájlba.

- [Példa a fakiszolgálói szolgáltatás a PowerShell használatával történő lekérdezésére](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
A bhold-adatait a bhold jelentéskészítő modul használatával exportálhatja az előnyben részesített formátumba.

### <a name="certificate-management"></a>Tanúsítványkezelés
A személyes adathoz kapcsolódó tanúsítványkezelői adat kapcsolódik az Active Directoryhoz. A rendszergazdák Active Directory PowerShell használatával is exportálhatunk ezeket az adatfájlokat.

## <a name="updating-personal-data"></a>Személyes adatok frissítése

A felhasználókra és az objektumokra vonatkozó személyes adatok általában a szervezet csatlakoztatott adatforrásaiban lévő felhasználó objektumból származnak. Mivel a HR-forrás felhasználói profiljában vagy más mérvadó rekordban, például az AD-ben végrehajtott módosítások megjelennek a webkiszolgáló-szinkronizálási szolgáltatásban.

### <a name="synchronization-service"></a>Synchronization Service

A felügyeleti műveletek végrehajtásához a rendszergazdáknak az [itt](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10))megadott szinkronizálási műveletek vagy adminisztrátorok részének kell lenniük.

Az adatok frissítése a szolgáltatótól származó szabályok meghatározásával történik. A felügyeleti konzol segít azonosítani a szolgáltató forrását, hogy frissítse azt a forrásnál. Egy másik lehetőség a szinkronizálási szabály vagy a szabály kiterjedésének szabályozása az Adatfrissítés vezérléséhez, ha a HR-adatforrást továbbra is meg kell őrizni. Ezek a avialible által támogatott beállítások.

További információ az attribútumok frissítésének különböző módjairól: alább. 

- [Szabályok bővítmények használata](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [A külső rendszerekkel való adatszinkronizálás ismertetése](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Szolgáltatás és portál/PAM

A PAM-adatbázisokat tartalmazó szolgáltatás és portál a FIMAutomation vagy a PAM-parancsmagok használatával frissíthető. Ha a portálon van, közvetlenül is frissítheti az objektum keresésével és módosításával. Az egyik lehetőség, hogy a konfigurációtól függően egyszerűen csak a portálról frissít, nem azt jelenti, hogy továbbra is megmarad. A szolgáltató forrása nagymértékben függ a teljes konfigurációtól.

### <a name="bhold"></a>BHOLD

A felhasználók közvetlenül frissíthetők a BHOLD Core felhasználói felülettel vagy a hozzáférés-kezelési összekötővel.

### <a name="certificate-management"></a>Tanúsítványkezelés

A Tanúsítványkezelő szolgáltatásban a felhasználók az Active Directoryból is tükröződnek. Ha frissíteni szeretné a használati Active Directory az objektum adatainak módosításához.

## <a name="deleting-personal-data"></a>Személyes adatok törléséről

>[!Note] 
> Ez a cikk útmutatást nyújt a személyes adatok Microsoft Identity Managerból való törlésének módjairól, és felhasználható a GDPR tartozó kötelezettségek támogatásához is. A GDPR-ral kapcsolatos általános információt a [Service Trust Portal GDPR szakaszában](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted) talál.

A többhelyes adatok szinkronizálva vannak, és mindig frissülnek a csatlakoztatott adatforrásból. Ha egy objektumot törölnek a célhelyen, a rendszer a biztonsági vizsgálat céljára megtarthatja az objektum adatait a munkaterületen. Az objektum törlése csatlakoztatott adatforrás-szabályok vagy szabály-kiterjesztési (kód) és/vagy objektum-törlési szabályok szerint van konfigurálva.

### <a name="synchronization-service"></a>Synchronization Service
A szinkronizációs szolgáltatás az adatkezelés számos módja, vagy az üzleti folyamatoktól függően az adattörlés. Az alábbiakban néhány cikk segítséget nyújt az attribútumok törlésével és frissítésével kapcsolatos lehetőségek megismeréséhez: 

- [A megszüntetés ismertetése](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Szabályok bővítmények használata](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Gyakorlati tanácsok](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Szolgáltatás és portál/PAM

Javasoljuk, hogy a Service &-portálon tartsa meg az alapértelmezett 30 nap rendszererőforrás-megőrzési konfigurációt. Ez azt jelzi, hogy a szolgáltatás Mikor törlődik, nem csak az adatok kérelmezését, hanem minden olyan objektumot, amelyet törölni kell a rendszerből. A folyamat elvégzése után a rendszer törli az objektumhoz csatolt összes adatkapcsolatot, amely tartalmazza az összes SSPR regisztrációs adattal. Ez a művelet az objektum törlési konfigurációját játssza le. Az objektumok GUID azonosítóját egyetlen táblázat tárolja. A Build 4.4.1459 lévő tábla teljes méretének csökkentése érdekében a folyamathoz egy FIM_DeleteExpiredSystemObjectsJob részleteket tartalmazó folyamatot vettünk fel, amely [itt](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden)található.

![srrc – adatvédelem – megfelelőség – adatvédelmi nyilatkozat. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

A szinkronizálási szolgáltatáshoz kapcsolódó legtöbb rendszer bhold úgy is konfigurálható, hogy a forrás objektum, például a HR eltávolítása után törölje a törlést. Ez a felügyeleti ügynökön van konfigurálva. és az objektum-törlési szabályok vezérlik a szinkronizálási szolgáltatás funkciói című témakörben leírtak szerint.

Egy másik lehetőség, hogy a felhasználói objektumot közvetlenül a BHOLD Core felhasználói felületről távolítsa el. A telepítéstől függően ez megfelelően működhet, de vegye figyelembe, hogy a kiépítési logika újból létrehozhatja ezt a felhasználót, ha nem törölte a forrást.
![bholdr – adatvédelem – megfelelőség – adatvédelmi nyilatkozat. PNG-](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Tanúsítványkezelés
Ha törölni szeretne egy felhasználót a CM-ből, törölje a felhasználót az Active Directoryban.

A Tanúsítványkezelő, mert csak a tanúsítványszolgáltatásokból származó profilt fogja tárolni a sAMAccountName. Miután a felhasználó törölve lett az AD-ből, a felhasználó gyorsítótára csak a tanúsítványokhoz van regisztrálva. Az adatbázisban nem ajánlott törölni semmit, mivel ez a környezet működésének általános károsodását okozhatja.

## <a name="opt-out-of-telemetry"></a>Telemetria
A korábbi buildek az egyes központi telepítésekhez tartozó névtelen telemetria gyűjtésére használt FIM/a rendszerkiszolgálói adatok, és HTTPS-kapcsolaton keresztül továbbítják a Microsoft-kiszolgálókra. Ezeket az adatmennyiségeket a Microsoft használta fel a FIM/a Windows későbbi verzióinak fejlesztéséhez a múltban.

>[!Note] 
> A 4.5. x. x vagy újabb verziókban a későbbi kiadásokban le lesz tiltva.

Ha le szeretné tiltani az adatgyűjtést a korábbi verzióban, futtassa a módosítási módot, és törölje a következő parancssort:

![alapszintű adatvédelem – megfelelőség – CEIP. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

vagy szerkessze a beállításjegyzéket, és állítsa az értéket 0: (összetevő) CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![ceip2 – adatvédelem – megfelelőség – adatvédelmi nyilatkozat. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>További lépések 
- [Az SQL-hez kapcsolódó adatvédelmi útmutató](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [A szolgáltatási megbízhatósági portál GDPR szakasza](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 archívum: üzembe helyezés a Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)

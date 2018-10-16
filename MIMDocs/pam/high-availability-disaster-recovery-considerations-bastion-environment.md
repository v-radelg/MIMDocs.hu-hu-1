---
title: PAM vészhelyreállítás | Microsoft Docs
description: Információk a Privileged Access Management konfigurálásáról magas rendelkezésre álláshoz és vészhelyreállításhoz.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5af801ae3b5077d39bbe3957b0288e24c33ce551
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333955"
---
# <a name="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment"></a>A magas rendelkezésre állással és a vészhelyreállítással kapcsolatos szempontok a megerősített környezet esetében

Ez a cikk a magas rendelkezésre állással és a vészhelyreállítással kapcsolatos szempontokat ismerteti az Active Directory tartományi szolgáltatásoknak (AD DS) és a Microsoft Identity Manager 2016 (MIM) verziónak a Privileged Access Management (PAM) megoldáshoz történő telepítése esetében.

A vállalatok a magas rendelkezésre állásra és a vészhelyreállításra összepontosítanak a Windows Server, az SQL Server és az Active Directory munkaterheléseivel kapcsolatban. Fontos azonban a Privileged Access Management megoldás megerősített környezetének megbízható rendelkezésre állása is. A megerősített környezet a szervezet informatikai infrastruktúrájának kritikus fontosságú része, mivel a felhasználók az ehhez tartozó összetevőket használják a rendszergazdai szerepkörükhöz kapcsolódó tevékenységek végrehajtásakor. Ha részletes tájékoztatásra van szüksége a magas rendelkezésre állás általános szempontjaival kapcsolatban, töltse le [Microsoft High Availability Overview](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc) (A Microsoft magas rendelkezésre állása – áttekintés) című tanulmányt.

## <a name="high-availability-and-disaster-recovery-scenarios"></a>Magas rendelkezésre állási és vészhelyreállítási forgatókönyvek

A magas rendelkezésre állás és a vészhelyreállítás tervezésekor gondolja át a következő kérdéseket:

- A kimaradások melyik funkciókat érinthetik?
- Melyek azok a funkciók, amelyek üzleti szempontból kritikus fontosságúak és/vagy kritikus fontosságúak az IT-üzemeltetés esetében?
- Melyek azok a kockázatok, amelyek kimaradáshoz vezethetnek ezekben a rendszerekben?

A felsorolt szempontok hatóköre befolyásolja a telepítési és az üzemeltetési költségeket, ezért előfordulhat, hogy a szervezetek egyes funkciókat a többinél magasabb prioritással kezelnek, és elfogadják az alacsonyabb prioritású funkciók átmeneti kimaradásainak kockázatát. A következő táblázat a szervezetek által használt egyik lehetséges prioritási besorolást foglalja össze:

| **Megerősített erdő funkciója** | **Relatív prioritás a helyreállítás során** | **Kockázatcsökkentés, ha a funkció nem érhető el** |
| --------------------------- | --------------------- | -------------- |
| Megbízhatósági kapcsolat kialakítása         | Alacsony | Várjon, amíg helyre nem áll a megerősített környezet. |
| Felhasználók és csoportok kockázatcsökkentése   | Alacsony | Várjon, amíg helyre nem áll a megerősített környezet. |
| MIM felügyelete          | Alacsony | Várjon, amíg helyre nem áll a megerősített környezet. |
| Rendszerjogosultságú szerepkör aktiválása  | Közepes | Használjon intelligens kártyára mentett dedikált fiókokat a felhasználóknak a felügyeleti csoportokhoz való hozzáadásához. |
| Erőforrás-kezelés         | Magas | Használjon intelligens kártyára mentett dedikált fiókokat a felhasználóknak a felügyeleti csoportokhoz való hozzáadásához. |
| A meglévő erdőben található felhasználók és csoportok figyelése | Alacsony | Várjon, amíg helyre nem áll a megerősített környezet. |

A következőkben áttekintjük a megerősített erdő fent felsorolt funkcióit.

### <a name="trust-establishment"></a>Megbízhatósági kapcsolat kialakítása

A megerősített környezet erdeje és a meglévő erdő tartományai között erdőszintű megbízhatóságnak kell lennie. Ez azért szükséges, hogy a megerősített környezetben hitelesített felhasználók felügyelhessék a meglévő erdőkben található erőforrásokat. További konfigurációra lehet szükség például a Windows Server korábbi verzióin működő tartományokban található felhasználók áttelepítésének engedélyezéséhez.

A megbízhatóság létrehozásához az szükséges, hogy a meglévő erdő tartományvezérlői, valamint a megerősített környezet MIM és AD összetevői online állapotúak legyenek.  Ha ezek bármelyikénél kimaradás lép fel a megbízhatóság létrehozása közben, a kimaradást okozó probléma megoldása után a rendszergazda ismét megpróbálhatja végrehajtani a műveletet.  Arra az esetre, ha a meglévő erdő tartományvezérlői vagy a megerősített környezet helyre lett állítva egy kimaradást követően, a MIM PowerShell-parancsmagokat – `Test-PAMTrust` és `Test-PAMDomainConfiguration` – is tartalmaz, amelyekkel ellenőrizhető, hogy a megbízhatóság továbbra is fennáll-e.

### <a name="user-and-group-migration"></a>Felhasználók és csoportok áttelepítése

A megbízhatóság létrehozása után árnyékcsoportok hozhatók létre a megerősített környezetben, valamint felhasználói fiókok ezen csoportok tagjainak és a jóváhagyóknak. Ez lehetővé teszi, hogy ezek a felhasználók aktiválják az emelt szintű szerepköröket, és visszaszerezzék az érvényes csoporttagságukat.

A felhasználók és a csoportok áttelepítéséhez az szükséges, hogy a meglévő erdő tartományvezérlői, valamint a megerősített környezet MIM és AD összetevői online állapotúak legyenek.   Ha a meglévő erdő tartományvezérlői nem érhetők el, akkor nem vehetők fel további felhasználók és csoportok a megerősített környezetbe, de a meglévő felhasználókra és csoportokra ez nincs hatással. Ha az összetevők bármelyikénél kimaradás lép fel az áttelepítés során, a kimaradást okozó probléma megoldása után a rendszergazda ismét megpróbálhatja végrehajtani a műveletet.

### <a name="mim-administration"></a>A MIM felügyelete

Miután megtörtént a felhasználók és a csoportok áttelepítése, a rendszergazdák a MIM-ben tovább konfigurálhatják a szerepkör-hozzárendeléseket a szerepkörökhöz aktiválandó felhasználók csatolásával.  Ezenkívül konfigurálhatják a jóváhagyásra és az Azure MFA használatára vonatkozó MIM-szabályzatokat.  

A MIM felügyelete megköveteli, hogy a megerősített környezet MIM és AD összetevői online állapotúak legyenek.

### <a name="privileged-role-activation"></a>Rendszerjogosultságú szerepkör aktiválása

Amikor egy felhasználó rendszerjogosultságú szerepkört kíván aktiválni, hitelesítenie kell magát a megerősített környezet tartományában, és kérelmet kell küldenie a MIM számára.  A MIM tartalmazza a SOAP és a REST API-t, valamint felhasználói felületeket a PowerShellben és egy weblapon.

Az emelt szintű szerepkörök aktiválása megköveteli, hogy a megerősített környezet MIM és AD összetevői online állapotúak legyenek.  Ha pedig a MIM az [Azure MFA](use-azure-mfa-for-activation.md) használatára van beállítva a kiválasztott szerepkör aktiválásához, akkor internet-hozzáférésre van szükség az Azure MFA szolgáltatáshoz való csatlakozáshoz.

### <a name="resource-management"></a>Erőforrás-kezelés

Miután a felhasználó aktiválása megtörtént a szerepkörhöz, a tartományvezérlő létrehozhat számára egy Kerberos-jegyet, amelyet a meglévő tartományokban található tartományvezérlők használhatnak, és így felismerhetők lesznek a felhasználó új ideiglenes csoporttagságai.

Az erőforrás-kezelés megköveteli, hogy az erőforrás tartományának tartományvezérlője, valamint a megerősített környezetben lévő tartományvezérlő online állapotú legyen.  Miután megtörtént a felhasználó aktiválása, a Kerberos-jegyének kiállításához nem szükséges, hogy a MIM vagy az SQL online állapotú legyen a megerősített környezetben.  (Vegye figyelembe, hogy a megerősített környezet Windows Server 2012 R2 működési szintje esetén az ideiglenes csoporttagság megszüntetéséhez az szükséges, hogy a MIM online állapotú legyen.)

### <a name="monitoring-of-users-and-groups-in-the-existing-forest"></a>A meglévő erdőben található felhasználók és csoportok figyelése

A MIM tartalmaz egy PAM-figyelőszolgáltatást is, amely rendszeresen ellenőrzi a felhasználókat és csoportokat a meglévő tartományokban, és megfelelően frissíti a MIM-adatbázist és az AD-t.  Ennek a szolgáltatásnak nem kell online állapotúnak lennie a szerepkörök aktiválásához vagy az erőforrások kezelése során.

A figyeléshez az szükséges, hogy a meglévő erdő tartományvezérlői, valamint a megerősített környezet MIM és AD összetevői online állapotúak legyenek.  

## <a name="deployment-options"></a>Telepítési lehetőségek

[A környezet áttekintése](environment-overview.md) című cikk egy alapszintű topológiát mutat be, amely a nem magas rendelkezésre állásra szánt technológia megismerésére alkalmas. Ez a szakasz leírja, hogyan bővíthető ez a topológia a magas rendelkezésre állás biztosítása érdekében az egy helyet és a több helyet használó szervezeteknél.

### <a name="networking"></a>Hálózat

A megerősített környezetben lévő számítógépek közötti hálózati forgalmat el kell különíteni a meglévő hálózatoktól, például más fizikai vagy virtuális hálózat használatával.  A megerősített környezetre vonatkozó kockázatoktól függően szükség lehet független fizikai összeköttetések használatára a számítógépek között.  A feladatátvevő fürtök bizonyos technológiái további követelményeket támasztanak a hálózati adapterekre vonatkozóan.

Az Active Directory tartományi szolgáltatásokat és a megerősített környezetben a MIM szolgáltatásokat üzemeltető számítógépek kétirányú kapcsolatot igényelnek a meglévő erdőkben található erőforrásokhoz, hogy végre tudják hajtani a következő műveleteket:

- A felhasználók hitelesítése a PRIV erdő tartományvezérlőivel
- A felhasználók aktiválási kérelmeinek megadása
- A felhasználók Kerberos-jegyeinek használhatóvá tétele a meglévő erdőben lévő erőforrások számára
- A meglévő erdőben lévő tartományok figyelése a MIM használatával
- E-mailek küldése a MIM használatával a meglévő erdőben található e-mail-kiszolgálón keresztül

### <a name="minimal-high-availability-topologies"></a>Minimális magas rendelkezésre állású topológiák

A szervezetek kiválaszthatják, hogy a megerősített környezetükben melyik funkciók igényelnek magas rendelkezésre állást. Néhány korlátozást azonban figyelembe kell venni:

- A megerősített környezet által biztosított minden funkció esetében legalább két tartományvezérlő szükséges a magas rendelkezésre álláshoz.  
- Az aktiválási kérelmek magas rendelkezésre állásához legalább két, a MIM szolgáltatást futtató számítógép szükséges, valamint az SQL Server magas rendelkezésre állása.
- Az SQL Server magas rendelkezésre állásához a feladatátvevő fürtök esetében legalább két olyan kiszolgálóra van szükség, amely az SQL Servert biztosítja, és ezek a kiszolgálók nem lehetnek azonosak a tartományvezérlőkkel.
- A MIM szolgáltatás nem telepíthető a tartományvezérlőre az egyes kiszolgálók támadási felületének minimálisra csökkentése érdekében.

A megerősített környezet összes funkciójának legkisebb magas rendelkezésre állású topológiáját legalább négy kiszolgáló és egy megosztott tároló alkotja. Két kiszolgálót tartományvezérlőnek kell konfigurálni az Active Directory tartományi szolgáltatások biztosításához. A másik két kiszolgáló beállítható az SQL Servert biztosító feladatátvevő fürtként, valamint a MIM szolgáltatás biztosítására.

Emellett a megerősített környezet szokásos telepítése is tartalmazhat egy emelt jogosultsági szintű felügyeleti munkaállomást a kiszolgálók felügyeletéhez, valamint egy figyelési összetevőt.

A következő ábra egy lehetséges architektúrát szemléltet:

![Megerősített környezet topológiája – ábra](media/bastion1.png)

Mindegyik funkcióhoz további kiszolgálók konfigurálhatók, hogy nagyobb teljesítményt biztosítsanak nagyobb munkaterhelés esetére, illetve a földrajzi redundancia céljából, amint azt alább ismertetjük.

### <a name="deployments-supporting-multiple-sites"></a>Több helyet támogató központi telepítések

A több helyre telepített erőforrások esetén megfelelő telepítési topológia kiválasztásánál három tényezőt kell figyelembe venni:

- A magas rendelkezésre állás és a vészhelyreállítás céljai és kockázatai
- A megerősített környezet üzemeltetéséhez szükséges hardver jellemzői
- A felügyeleti feladatok modellje minden hely esetében

Az egyik legegyszerűbb módszer a megerősített környezet meghatározott helyen történő futtatása.  Normál körülmények között a felhasználók az adott hely megerősített környezetében található MIM-telepítéshez csatlakoznak, itt kérnek aktiválást, és az aktiválások érvényesek lesznek mindegyik hely erőforrására vonatkozóan.  Ha megszakad a hálózati kapcsolat, vagy nem érhető el a megerősített környezetet üzemeltető hely, egy másik helyen található offline hitelesítő adatok elérésével folytathatók ideiglenesen a felügyeleti tevékenységek, amíg ismét létre nem jön a hálózati kapcsolat.  Ez a módszer olyan helyzetekben lehet megfelelő, amelyekben egy adott hely, például egy fiókiroda helyi felügyeletére várhatóan ritkán lesz szükség, és a felügylet valószínűleg az adott helynek a szervezeti hálózatához történő visszacsatlakoztatására korlátozódik majd.

![Egy megerősített környezet a többhelyes topológia számára – ábra](media/bastion2.png)

A helyek magas rendelkezésre állásának és vészhelyreállításának biztosításához a megerősített környezet összetevői telepíthetők minden helyen úgy, hogy közös PRIV címtárat és közös SQL-adatbázist használjanak.  Ebben a topológiában a hálózati kapcsolat megszakadása esetén az egyes helyeken lévő felhasználók egymástól függetlenül folytathatják tovább a tevékenységüket.

![A többhelyes topológia több megerősített környezete – ábra](media/bastion3.png)

Erre a telepítési módszerre az a megszorítás vonatkozik, hogy az SQL Server olyan fürtöt igényel, amely mindkét helyre kiterjed, és ezt nehéz lehet megvalósítani. Ilyen esetben érdemes másik megoldásként csak a megerősített környezet Active Directory tartományi szolgáltatásait (a PRIV erdőt) replikálni.  Abban az esetben, ha a helyek között megszakad a hálózat kapcsolat, a B hely azon felhasználói, akik korábban már aktiválták a rendszerjogosultságú szerepkörüket, továbbra is felügyelhetik a B hely erőforrásait.

![A többhelyes topológia replikált megerősített környezete – ábra](media/bastion4.png)

Ha mindegyik hely különálló felügyeleti határt jelent, akkor több független megerősített környezet is telepíthető.  Az egyes megerősített környezetekben ugyanazt a szoftvert kell használni, de a tartománynevük lehet különböző, és az egyes megerősített környezetek könyvtárainak és adatbázisainak nem lesznek közös tulajdonságaik. Ha egy felhasználó felügyelni kívánja egy adott hely erőforrásait, az adott hely megerősített környezetében kell aktiválnia egy felhasználói fiókot.

![A többhelyes topológia független megerősített környezetei – ábra](media/bastion5.png)

Összetettebb telepítések is lehetségesek, például több megerősített környezet konfigurálható egymástól függetlenül az adott tartományban lévő erőforrások felügyeletéhez.

![A többhelyes topológia összetett megerősített környezetei – ábra](media/bastion6.png)

### <a name="hosted-bastion-environment"></a>Más helyen üzemeltetett megerősített környezet

Egyes szervezetek olyan megoldást alkalmaznak, amelyben a megerősített környezetet a meglévő helyektől elkülönítve alakítják ki. A megerősített környezet szoftvere üzemeltethető virtuális platformon, a szervezet hálózatán belül vagy külső szolgáltatónál.  E megoldás kiértékelésekor a következőket kell figyelembe venni:

- A meglévő tartományokra irányuló támadások elleni védekezésképpen a megerősített környezet felügyeletét el kell különíteni a meglévő tartomány rendszergazdai fiókjaitól.
- A megerősített környezet TCP/IP-kapcsolatot igényel a meglévő tartományban lévő tartományvezérlőkhöz.  A portok listája a [Tűzfal beállítása tartományokhoz és bizalmi kapcsolatokhoz](https://support.microsoft.com/kb/179442) című cikkben található.
- Az Active Directory tartományi szolgáltatások virtualizált telepítése meghatározott funkciókat igényel a virtualizációs platformról [A virtualizált tartományvezérlő központi telepítése és konfigurálása](https://technet.microsoft.com/library/jj574223.aspx) című cikkben leírtak szerint.
- Az SQL Servernek a MIM szolgáltatáshoz történő magas rendelkezésre állású telepítése egy speciális tárolási konfigurációt igényel, amelynek leírása az [SQL Server database storage](#sql-server-database-storage) (Az SQL Server adatbázistára) című cikkben olvasható.  Jelenleg nem minden üzemeltetési szolgáltató kínálatában szerepel a Windows Servernek az SQL Server feladatátvevő fürtjeihez alkalmas lemezkonfigurációkkal valóüzemeltetése.

## <a name="deployment-preparation-and-recovery-procedures"></a>A telepítés és a helyreállítási eljárások elkészítése

A megerősített környezet magas rendelkezésre állású vagy vészhelyreállításra alkalmas telepítésének előkészítésekor meg kell fontolni, hogyan történjen a Windows Server Active Directorynak, az SQL Servernek és a megosztott tárolón található adatbázisának, valamint a MIM szolgáltatásnak és a PAM-összetevőinek a telepítése.

### <a name="windows-server"></a>Windows Server

A Windows Server tartalmaz egy beépített funkciót a magas rendelkezésre álláshoz, amely lehetővé teszi, hogy több számítógép működjön együtt feladatátvevő fürtként. A fürtözött kiszolgálókat fizikai kábelek és szoftverek kötik össze. Ha a fürtcsomópontok közül egy vagy több meghibásodik, más csomópontok veszik át a szolgáltatást (ez a feladatátvételi folyamat).   További részleteket a [Feladatátvételi fürtszolgáltatás – áttekintés](https://technet.microsoft.com/library/hh831579.aspx) című cikkben talál.

Győződjön meg arról, hogy a megerősített környezetben található operációs rendszer és alkalmazások fogadják a biztonsági frissítéseket. Egyes frissítések telepítése után szükség lehet a kiszolgáló újraindítására, ezért a hosszabb kimaradások elkerülése érdekében a frissítést olyan időpontra ütemezze, amikor a frissítések minden kiszolgálóra alkalmazhatók. Ennek egyik módszere a [fürttámogató frissítés](https://technet.microsoft.com/library/hh831694.aspx) használata a Windows Server rendszerű feladatátvevő fürtökben.

A megerősített környezetben lévő kiszolgálók tartományhoz lesznek csatlakoztatva, és a tartományi szolgáltatásoktól fognak függeni. Győződjön meg arról, hogy a kiszolgálók nincsenek véletlenül beállítva a szolgáltatások egy meghatározott tartományvezérlőjétől, például a DNS-től való függőségre.

### <a name="bastion-environment-active-directory"></a>Az Active Directory megerősített környezete

A Windows Server Active Directory tartományi szolgáltatások natív módon támogatják a magas rendelkezésre állást és a vészhelyreállítást.

#### <a name="preparation"></a>Előkészítés

Az emelt szintű hozzáférések felügyeletének tipikus éles környezetbeli telepítése legalább két tartományvezérlőt tartalmaz a megerősített környezetben. A megerősített környezetben található első tartományvezérlő beállításával kapcsolatos útmutatás a telepítéssel foglalkozó [A PRIV tartományvezérlő előkészítése](step-2-prepare-priv-domain-controller.md) című cikk 2. lépésben található.

A további tartományvezérlő hozzáadásának műveletét a [Replika Windows Server 2012 tartományvezérlő telepítése meglévő tartományban (200. szint)](https://technet.microsoft.com/library/jj574134.aspx) című cikk ismerteti.  

>[!NOTE]
> Ha a tartományvezérlőt virtualizációs platformon fogja üzemeltetni, például a Hyper-V platformon, akkor olvassa el [A virtualizált tartományvezérlő központi telepítése és konfigurálása](https://technet.microsoft.com/library/jj574223.aspx) című cikkben található figyelmeztetéseket.

#### <a name="recovery"></a>Helyreállítás

Ha kimaradás fordul elő, a megszűnése után és a kiszolgálók újraindítása előtt győződjön meg arról, hogy legalább egy tartományvezérlő elérhető a megerősített környezetben.

A tartományon belül az Active Directory elosztja a műveleti főkiszolgáló (FSMO) szerepköreit [How Operations Masters Work](https://technet.microsoft.com/library/cc780487.aspx) (A műveleti főkiszolgálók működése) című cikkben leírtaknak megfelelően.  Ha egy tartományvezérlő sikertelen volt, szükség lehet egy vagy több olyan [tartományvezérlői szerepkör](https://technet.microsoft.com/library/cc786438.aspx) átvitelére, amelyhez az adott tartományvezérlő hozzá volt rendelve.

Ha azt állapítja meg, hogy a tartományvezérlő nem állítható vissza az éles környezetbe, ellenőrizze, hogy voltak-e hozzárendelve valamilyen szerepkörök a tartományvezérlőhöz, és szükség végezze el azok újbóli hozzárendelését. Erre vonatkozó útmutatás [View the Current Operations Master Role Holders](https://technet.microsoft.com/library/cc816893.aspx) (Az aktuális műveleti főkiszolgáló szerepköreinek megtekintése) című cikkben és a kapcsolódó cikkekben található.

Célszerű ellenőrizni a megerősített környezethez csatlakoztatott számítógépek DNS-beállításait, valamint a CORP tartományokban található tartományvezérlőket, amelyek megbízhatósági kapcsolattal rendelkeznek az adott tartományvezérlőhöz annak biztosításához, hogy egyiknél se legyen az adott tartományvezérlő számítógépének IP-címétől való függőség.

### <a name="sql-server-database-storage"></a>Az SQL Server adatbázistára

A magas rendelkezésre állású telepítéshez SQL Server feladatátvevő fürtök szükségesek, és az SQL Server feladatátvevő fürt példányainak az összes csomópont között megosztott tárolót kell használniuk az adatbázis és a naplók tárolásához. A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában.  Fontos, hogy ezeket a megerősített környezetben kell kijelölnie; a tárhelynek a megerősített környezeten kívüli egyéb munkaterhelésekkel való megosztása nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet sértetlenségét.

### <a name="sql-server"></a>SQL-kiszolgáló

A MIM szolgáltatás az SQL Server telepítését igényli a megerősített környezetben.   A magas rendelkezésre álláshoz az SQL telepíthető feladatátvevőfürt-példány (FCI) használatával. A különálló példányoktól eltérően az FCI-kben az SQL Server magas rendelkezésre állását az FCI-ben jelen lévő redundáns csomópontok biztosítják. Hiba vagy tervezett frissítés esetén az erőforráscsoport tulajdonjogát átveszi a Windows Server feladatátvevő fürt egy másik csomópontja.

Ha csak a vészhelyreállításra vonatkozó támogatásra van szüksége (és a magas rendelkezésre állásra vonatkozóan nincs), akkor a feladatátvevő fürtök helyett használhat naplóküldést, tranzakcióreplikációt, pillanatkép-replikációt vagy adatbázis-tükrözést.   

#### <a name="preparation"></a>Előkészítés

Amikor az SQL Servert telepíti a megerősített környezetben, annak függetlennek kell lennie a CORP erdőkben már meglévő összes SQL Servertől.  Ezenkívül ajánlott az SQL Servert egy dedikált kiszolgálóra telepíteni, amely nem azonos a tartományvezérlővel.
További információ a SQL Server dokumentációjának [AlwaysOn Failover Cluster Instances](https://msdn.microsoft.com/library/ms189134.aspx) (AlwaysOn feladatátvevőfürt-példányok) című útmutatójában található.

#### <a name="recovery"></a>Helyreállítás

Ha az SQL Server a naplóküldést használó vészhelyreállításra volt konfigurálva, a helyreállítás során az SQL Servert frissíteni kell.  Ezenkívül újra kell indítani a MIM szolgáltatás minden példányát.

Ha hiba történt az SQL Serveren, vagy megszakadt a kapcsolat az SQL Server és a MIM szolgáltatás között, akkor az SQL Server helyreállítása után ajánlott újraindítani minden MIM szolgáltatást.  Ez biztosítja, hogy MIM szolgáltatás újból létrehozza a kapcsolatát az SQL Serverrel.

### <a name="mim-service"></a>MIM szolgáltatás
A MIM szolgáltatás az aktiválási kérelmek feldolgozásához szükséges.  Ahhoz, hogy a MIM szolgáltatást futtató számítógépet le lehessen állítani karbantartásra, miközben továbbra is érkeznek aktiválási kérelmek, a MIM szolgáltatás több számítógépre is telepíthető.  Vegye figyelembe, hogy a MIM szolgáltatás nem vesz részt a Kerberos műveleteiben, miután a felhasználó hozzá lett adva egy csoporthoz.  

#### <a name="preparation"></a>Előkészítés
A MIM szolgáltatást több, a PRIV tartományhoz csatlakozó kiszolgálóra célszerű telepíteni.
A magas rendelkezésre állással kapcsolatos tudnivalókért olvassa el a Windows Server dokumentációjában található következő cikkeket: [A Feladatátvételi fürtszolgáltatás hardverkövetelményei és tárolási beállításai](https://technet.microsoft.com/library/jj612869.aspx) és [Creating a Windows Server 2012 Failover Cluster](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx) (Windows Server 2012 feladatátvevő fürt létrehozása).

Éles környezetben, több kiszolgálóra végrehajtott telepítés esetén a hálózati terheléselosztás (NLB) segítségével osztható el a feldolgozási terhelés.  Érdemes csak egy aliast (például A vagy CNAME) használnia, hogy a felhasználó csak egy általános nevet lásson.

>[!IMPORTANT]
> Ha használ terheléselosztási technológiát, de az nem a Windows Server 2012 R2 által tartalmazott NLB szolgáltatás, akkor győződjön meg arról, hogy az Ön által használt megoldás az adott munkamenetet ugyanarra a kiszolgálóra irányítja át, és nem egy véletlenszerűen választott kiszolgálóra.

A MIM többkiszolgálós telepítése esetén mindegyik MIM szolgáltatáshoz tartozik egy külső állomásnév, egy szolgáltatásnév és egy szolgáltatáspartíció neve.  A szolgáltatásnév alapértelmezett értéke a számítógép neve, a külső állomásnév és a szolgáltatáspartíció nevének alapértelmezett értéke pedig a MIM szolgáltatás telepítésekor adható meg a MIM szolgáltatás kiszolgálójának címét kérő képernyőn. Ezeket a neveket a %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config fájl tárolja a `resourceManagementService` konfigurációs csomópont `externalHostName`, `serviceName` és `servicePartitionName` attribútumaként.  

Amikor a MIM szolgáltatás kérelmet kap, a szolgáltatáspartíció neve az adott kérelem attribútumaként lesz tárolva.   Ezt követően csak a MIM szolgáltatás azonos szolgáltatáspartíció-nevű más telepítései használhatják az adott kérelmet.  Ha a PAM forgatókönyve manuális jóváhagyásokat vagy más hosszú élettartamú kérelemfeldolgozást tartalmaz, akkor ügyeljen arra, hogy mindegyik MIM szolgáltatáshoz azonos `servicePartitionName` attribútum tartozzon a konfigurációs fájlban.

#### <a name="recovery"></a>Helyreállítás

Kimaradás után a MIM szolgáltatás újraindítása előtt győződjön meg arról, hogy legalább egy Active Directory tartományvezérlő és egy SQL Server elérhető a megerősített környezetben.  

A munkafolyamat-példányok csak a MIM szolgáltatás azon kiszolgálójával fejezhetők be, amelynek szolgáltatáspartíció-neve és szolgáltatásneve megegyezik a munkafolyamat-példányt elindító MIM szolgáltatás kiszolgálójának ilyen neveivel.  Ha egy adott számítógépen hiba történik a kérelmet feldolgozó MIM szolgáltatás futtatása közben, és az adott számítógép működése nem állítható vissza, akkor a MIM szolgáltatást egy új számítógépre kell telepíteni. A telepítés után az új MIM szolgáltatásban módosítsa a *resourcemanagementservice.exe.config* fájlt, és az új MIM telepítés `serviceName` és `servicePartitionName` attribútumát állítsa be úgy, hogy azonos legyen a meghibásodott számítógép állomásnevével és szolgáltatáspartíció-nevével.

### <a name="mim-pam-components"></a>A MIM PAM-összetevői

A MIM szolgáltatás és -portál telepítője is tartalmaz PAM-összetevőket, például a PowerShell-modulokat és két szolgáltatást.

#### <a name="preparation"></a>Előkészítés

Az emelt szintű hozzáférések felügyeletének összetevőit telepíteni kell a megerősített környezetben lévő minden olyan számítógépre, amelyre a MIM szolgáltatást telepíti.  Ezek nem adhatók hozzá a későbbiekben.

#### <a name="recovery"></a>Helyreállítás

A kimaradás utáni helyreállítást követően győződjön meg arról, hogy a MIM szolgáltatás fut legalább egy kiszolgálón.  Ezután a `net start "PAM Monitoring service"` parancs használatával ellenőrizze, hogy a MIM PAM-figyelési szolgáltatása is fut-e az adott kiszolgálón.

Ha a megerősített környezet erdőjének működési szintje Windows Server 2012 R2, a `net start "PAM Component service"` parancs használatával ellenőrizze, hogy a MIM PAM-összetevőjének szolgáltatása is fut-e az adott kiszolgálón.

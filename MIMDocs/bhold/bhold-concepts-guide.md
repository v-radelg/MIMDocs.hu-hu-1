---
title: A Microsoft a BHOLD Suite fogalmak útmutató |} A Microsoft Docs
description: Első lépések a MIM 2016 összetevői kapcsán – a Synchronization Service telepítése és konfigurálása
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/14/2017
ms.technology: security
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: bd468c30b64c512cea1f5b9c9e6d2dafab168a6d
ms.sourcegitcommit: f8cbdd6439d395971a4daa563a96240bbbbf4369
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49336579"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>A Microsoft a BHOLD Suite fogalmak útmutató

A Microsoft Identity Manager 2016 (MIM) segítségével a szervezetek teljes életciklusára vonatkozóan a felhasználói identitások és azok kapcsolódó hitelesítő adatok kezeléséhez. Az identitások szinkronizálásával, központilag kezelheti a tanúsítványokat és a jelszavakat és konfigurálta a felhasználókat a heterogén rendszerekből konfigurálható. A MIM-informatikai szervezetek határozza meg, és az identitások kezeléséhez a létrehozástól a használatból való kivonást egyaránt használt folyamatok automatizálásához.

BHOLD Suite a Microsoft ezeket a képességeket a MIM szerepköralapú hozzáférés-vezérlés hozzáadásával terjeszti ki. A BHOLD lehetővé teszi a szervezetek felhasználói szerepkörök definiálása, és a bizalmas adatokhoz és alkalmazásokhoz való hozzáférés szabályozásához. A hozzáférés-alapú megfelelően ezeket a szerepköröket. BHOLD Suite magában foglalja a szolgáltatások és eszközök, amelyek egyszerűbbé teszik a modellezést, a szerepkör-kapcsolatok a szervezeten belül. A BHOLD képezi le ezeket a szerepköröket, jogosultságokkal, és győződjön meg arról, hogy a szerepkör-definíciók és a társított jogosultságok megfelelően alkalmazzák a felhasználók számára. Ezek a képességek teljesen integrálva van a MIM-zökkenőmentes élményt biztosít a végfelhasználók és informatikai munkatársak egyaránt.

Ez az útmutató segítségével megismerheti a mim BHOLD Suite működését, és a következő témakörökkel foglalkozik:

- Szerepköralapú hozzáférés-vezérlés
- Állapotigazolás
- Elemzés
- Jelentéskészítés
- Access Management-összekötő
- A MIM-integráció

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés

A leggyakrabban használt módszer az adatokhoz és alkalmazásokhoz való felhasználói hozzáférés szabályozása a tulajdonosi hozzáférés-vezérlés (DAC) keresztül történik. A leggyakoribb megvalósításokban minden jelentős objektum van azonosított tulajdonosa. A tulajdonos képes engedélyezheti vagy megtagadhatja a hozzáférést az objektum, melyet más egyéni identitás vagy csoporttagság alapján. A gyakorlatban DAC általában eredményezi, biztonsági csoportokat néhány tükrözik, szervezeti felépítése, mások (például feladattípusok vagy projekt-hozzárendelések) működési csoportosításokat képviselő és egyéb olyan makeshift felhasználóból álló gyűjteményekbe áll, és eszközök, amelyek több ideiglenes célokra van csatolva. Szervezet növekedésével egyre nehezen kezelhető válik az ilyen csoportok tagságát. Például ha egy alkalmazott a másikra át a projektek, a csoportokat, amelyek a projektek eszközökhöz való hozzáférést szabályozzák, hogy manuálisan kell frissíteni. Ezekben az esetekben nem ritka, hogy megtörténjen, hibák, amelyek akadályozhatják a projekt biztonság és hatékonyság hibákkal.

A MIM szolgáltatásokat tartalmaz, amelyek azáltal, hogy csoport és a terjesztési lista tagsági automatizált szabályozhatja a probléma mérséklése érdekében. Azonban ez nem oldja meg, amely nem feltétlenül kapcsolódnak egymáshoz strukturált módon proliferáló csoportok belső bonyolultságát.

Jelentősen csökkentheti a elterjedése egyik módja, szerepköralapú hozzáférés-vezérlés (RBAC) üzembe helyezésével. Az RBAC nem helyettesítik DAC.  RBAC épülő DAC keretrendszer azáltal, hogy a felhasználók és informatikai erőforrások osztályozásához. Ez lehetővé teszi, hogy explicit kapcsolataikat, és megfelelően a besorolást megfelelő hozzáférési jogosultságokat. Például rendel egy felhasználói attribútum, amely meghatározza a felhasználók beosztás és projekt hozzárendelések, a felhasználó is hozzáférést kell biztosítani a felhasználói feladat és a felhasználó hozzájárul, hogy egy adott projekt szükséges adatokat a szükséges eszközöket. Amikor a felhasználó feltételezi, hogy egy másik feladat és -hozzárendelések más projekthez, módosítja az attribútumokat, amelyek adja meg a felhasználó beosztása és projektek automatikusan letiltja a hozzáférést a felhasználók előző pozíciónál csak szükséges erőforrásokat.

Mert a szerepkörök üzemelteti, amely hierarchikus struktúrában kapcsolódhatnak, szerepkörök (például az értékesítési igazgató és értékesítési képviselőjével szerepkörök is lehet a további általános szerepben található az értékesítés), egyes szerepkörök megfelelő jogosultságokat rendelhet hozzá, és továbbra is még meg egyszerűen Mindenki, aki a szélesebb körű szerepkör megfelelő jogosultságokkal. Például kórházban, orvosi munkatársak sikerült jogot kell adni a betegek rekordok megtekintéséhez, de csak orvosok (orvosi szerepkör alszerepkört) sikerült kell megadni a jogot arra, hogy adja meg a beteggel kapcsolatos előírásokat. Hasonlóképpen az irodai szerepkörhöz tartozó felhasználók sikerült megtagadja a hozzáférést a betegek kartonjai írnokok (egy alszerepkört irodai szerepkör), akik sikerült számára hozzáférést biztosítani a betegek rekordok, amelyek szükségesek a szolgáltatások beteg számlázási belső számlázási kivételével a kórházi által biztosított.

Az RBAC további előnye meghatározó és betartató feladatkörök (gyeptégla) lehetővé teszi. Ez lehetővé teszi a szervezetek szerepkörök, engedélyek, amelyek nem ugyanahhoz a felhasználóhoz, birtokában kell, hogy egy adott felhasználó nem rendelhető szerepköröket, amelyek lehetővé teszik a felhasználó egy fizetési kezdeményezéséhez és díjfizetést egy, például kombinációja határozza meg. RBAC automatikusan kikényszerítheti az ilyen szabályzatot, ahelyett, hogy a szabályzat, felhasználónkénti alapon hatékony végrehajtásának kiértékelheti, hogy itt.

### <a name="bhold-role-model-objects"></a>A BHOLD szerepkör adatmodell-objektumokat

A BHOLD Suite adja meg, és rendszerezheti a szerepkörök a szervezetben, térkép a felhasználók szerepkörökhöz, és a térkép megfelelő engedélyekkel szerepkörök. Ez a struktúra úgynevezett szerepkör modellt, és tartalmaz, és kapcsolódik az öt típusú objektumokat: 

- Szervezeti egységek
- Users
- Szerepkörök
- Engedélyek
- Alkalmazások

#### <a name="organizational-units"></a>Szervezeti egységek

Szervezeti egységek (OrgUnits) az egyszerű azt jelenti, hogy mely szerint a BHOLD szerepkör modellben felhasználók vannak rendszerezve. Minden felhasználónak legalább egy OrgUnit kell tartoznia. (Tulajdonképpen egy felhasználó eltávolításakor a BHOLD szervezeti egységből a felhasználói adatok rekord törlődik a BHOLD-adatbázis.)

> [!Important]
> A BHOLD szerepkör modellben szervezeti egység nem lehet a szervezeti egységek az Active Directory Domain Servicesben (AD DS). Általában a szervezeti egység struktúra, a BHOLD alapján a szervezet és az üzleti szabályzatok nem a hálózati infrastruktúra követelményeinek.

Bár ez nem szükséges, a legtöbb esetben szervezeti egységek felépítése a BHOLD, amely a tényleges, az alábbihoz hasonló szervezet hierarchikus struktúráját jelöli:

![](media/bhold-concepts-guide/org-chart.png)

Ebben a példában a szerepkör modell lenne organizationalganizatinal egység a vállalat egésze (a elnök, képviseli, mivel a elnöke nem egy mororganizationalganizatinal egységnek a része) vagy a BHOLD legfelső szintű szervezeti egység (amely minden esetben létezik) erre a célra használható. A vállalati részlegek a alelnökök élén jelölő OrgUnits kerül a vállalati szervezeti egységet. A marketing és értékesítés igazgatók tartozó következő, a szervezeti egységek kell hozzáadni a marketing és értékesítés szervezeti egységek, és a regionális értékesítési vezetők jelölő szervezeti egységek kerül a szervezeti egységet a a keleti régió értékesítési vezető. Nem kezelheti más felhasználók, akik értékesítési hozzárendeli a keleti régió értékesítési vezető a szervezeti egység tagjai kerül sor. Vegye figyelembe, hogy a felhasználók bármilyen szintű szervezeti egység tagjai lehetnek. Ha például egy asszisztens, aki nem a manager és a jelentéseket közvetlenül a alelnök, lenne a alelnök szervezeti egységben tag.

Mellett a szervezeti felépítés jelölő, szervezeti egységek is segítségével csoportosítsa a felhasználók és más szervezeti egységek működési feltételek szerint többek között a projektek vagy a specializáció. A következő ábra bemutatja, hogyan szervezeti egységek használni kívánt csoporthoz értékesítési hozzárendeli az ügyfél típusa alapján:

![](media/bhold-concepts-guide/org-chart-02.png)

Ebben a példában minden egyes értékesítési társítása két szervezeti egységre lenne tartozik: egy a szervezet felügyeleti struktúra a társítás helyet jelölő, és a egy jelölő a társítás rátába (kiskereskedelmi vagy vállalati). Egyes szervezeti egységek, amelyek ezután lehet engedélyeket különböző eléréséhez a szervezet különböző szerepkörök rendelhetők informatikai erőforrásra lenne szükség. Emellett szerepkörök öröklődnek a szülő szervezeti egységek, való propagálása a szerepköröket a felhasználókhoz. Másrészről egyes szerepkörök megakadályozható folyamatban örökölt, amely biztosítja, hogy egy adott szerepkörrel csak a megfelelő szervezeti egységekhez tartozó.

OrgUnits BHOLD Suite használatával a BHOLD Core webes portálon vagy a BHOLD Model Generator használatával létrehozhatók.

#### <a name="users"></a>Users

Fent felsorolt minden felhasználónak legalább egy szervezeti egységhez (OrgUnit) kell tartoznia. Mivel a felhasználó társítását ahhoz szerepkörök egyszerű mechanizmusa szervezeti egységek, a szervezetek többsége az egy adott felhasználó tartozik több OrgUnits, hogy egyszerűbb legyen a felhasználói szerepkörök társítása. Bizonyos esetekben azonban szükségessé válhat a szerepkör társítása egy felhasználót, hogy a felhasználó tartozik minden olyan OrgUnits szereplőkkel. Ennek következtében a felhasználó is közvetlen hozzárendelését egy szerepkör, valamint beszerzését szerepkörök a OrgUnits, amely a felhasználó tartozik a.

Amikor a felhasználó nem aktív, a szervezet (ideig azonnal orvosi hagyja, a példában), a felhasználó felfüggeszthető, amely visszavonja a felhasználói engedélyek anélkül, hogy eltávolítaná a felhasználó a szerepkör-modellből. Alapján ad vissza, a vámot, a felhasználó újraaktiválhatóak a, amely visszaállítja a felhasználói szerepkörök által nyújtott összes engedélyt.

Felhasználók objektumokat hozhat létre külön-külön BHOLD a BHOLD Core webes portálon keresztül, vagy importálni is lehet tömegesen BHOLD Model Generator, vagy a FIM szinkronizálási szolgáltatás lévő felhasználói adatok importálása az Access Management-összekötő használata ilyen adatforrások Active Directory Domain Services vagy az emberi erőforrások alkalmazásokként.

Felhasználók közvetlenül a BHOLD FIM szinkronizálási szolgáltatás használata nélkül is létrehozható. Ez akkor hasznos, ha a modellezési szerepkörök üzem előtti környezet vagy a tesztelési környezetben. Is engedélyezheti a külső felhasználók (például az alkalmazottak számára az alvállalkozónak) szerepkört, és így megadott anélkül, hogy az alkalmazottak adatbázis; hozzáadandó informatikai erőforrásokhoz való hozzáférés Ezek a felhasználók azonban nem lesz a BHOLD önkiszolgálói szolgáltatást is használhatja.

#### <a name="roles"></a>Szerepkörök

Fent említetteknek a szerepköralapú hozzáférés-vezérlés (RBAC) modellben engedélyek tartoznak az egyéni felhasználók helyett szerepköröket. Ez lehetővé teszi a felhasználói szerepkörök módosítása helyett külön megadásával vagy tagadja meg a beállított felhasználói engedélyeit a felhasználói feladatok elvégzéséhez szükséges engedélyek kiosztása az összes felhasználó. A hozzárendelés az engedélyek következtében már nem igényel az informatikai részleg részvételi, de helyette az üzleti kezelésének részeként végezhető. Egy szerepkör összesítheti közvetlenül vagy alszerepkörök, tovább csökkentve a informatikai kérelemfeldolgozásban való részvételének felhasználói engedélyek kezelése révén a különböző rendszerek való hozzáférésre vonatkozó engedélyeket.

Fontos megjegyezni, hogy szerepkörök érhetők el az RBAC-modellben szerepkörök általában nem lesznek kiépítve, célként megadott alkalmazásokat. Ez lehetővé teszi, hogy a meglévő alkalmazásokat, amely nem úgy tervezték, szerepkörök vagy a szerepkör-definíciók kell módosítani együtt lehet használni az RBAC igényeinek a változó üzleti modellek maguk az alkalmazások módosítása nélkül. Ha egy cél alkalmazás szerepkörök használatára lett kialakítva, majd is a BHOLD szerepkör modellben, megfelelő alkalmazás-szerepkörökhöz társított szerepköröket való az alkalmazás-specifikus szerepkörökhöz kezelésével engedélyekként.

A BHOLD rendelhet egy szerepkört a felhasználó elsődlegesen két mechanizmusokon keresztül:

- Egy szerepkört rendel egy szervezeti egység (szervezeti egység), amely a felhasználó tagja
- Szerepkör hozzárendelése közvetlenül a felhasználó által

A szülő szervezeti egység igény szerint rendelt szerepkör által a tag szervezeti egységeket is örökölhetők. Ha egy szerepkörhöz rendelt vagy öröklik a szervezeti egység, azt egy hatékony vagy javasolt szerepkörként lehet jelölni. Ha egy hatékony szerepkör, a szervezeti egységben lévő összes felhasználó számára a szerepkörhöz vannak hozzárendelve. Ha a javasolt szerepkör, aktiválnia kell minden egyes felhasználó- vagy tag szervezeti egység válik, hogy a felhasználó vagy szervezeti egység tagjai. Ez lehetővé teszi a felhasználók hozzárendelése egy részét a szerepköröket egy szervezeti egységhez társított, mint az automatikus összes szerepkör a szervezeti egység összes tagjára. Emellett szerepkörök kaphatnak a kezdő és záró dátuma, és korlátozások is helyezhető, amelyhez a szerepkör hatékony lehet egy szervezeti egységben lévő felhasználók aránya.

A következő ábra szemlélteti, hogy egy adott felhasználó rendelhető egy szerepkörhöz a BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Ez az ábra A szerepkör egy szervezeti egységhez, mint egy örökölt szerepkör hozzá van rendelve, és így öröklik a tag szervezeti egységeket és az adott szervezeti egységekhez tartozó összes felhasználó. B szerepkör hozzá van rendelve egy javasolt szerepkör egy szervezeti egység. Mielőtt a szervezeti egység egy felhasználó jogosult-e a szerepkör engedélyeivel kell aktiválni. Szerepkör C az hatékony szerepet, így a hozzá kapcsolódó jogosultságok azonnal érvénybe lépnek a szervezeti egységben lévő összes felhasználó számára. Szerepkör D közvetlenül a felhasználó kapcsolódik, és így engedélyeinek azonnal érvénybe lépnek, hogy a felhasználó.

Ezenkívül a szerepkör egy felhasználó a felhasználói attribútumok alapján aktiválható. További információkért lásd: attribútum-alapú hitelesítést.

#### <a name="permissions"></a>Engedélyek

A BHOLD szereplő egyik engedély egy cél-alkalmazás importálása engedélyezési felel meg. Azt jelenti amikor BHOLD konfigurálva van egy alkalmazással, kap az engedélyeket, BHOLD kapcsolat szerepkörök listáját. Például az Active Directory Domain Services (AD DS) való felvételekor BHOLD alkalmazást, kap, amelyek biztonsági csoportok, a BHOLD engedélyekként lehet kapcsolódni a BHOLD-szerepkörök listáját.

Alkalmazások engedélyek vonatkoznak. A BHOLD engedélyek egységes áttekintést nyújt a így engedélyeket is lehet társítva anélkül, hogy a szerepkör-kezelők az engedélyek a gyakorlati kivitelezés részleteinek megismeréséhez. A gyakorlatban a különböző rendszerek léptethet engedély eltérően. Az alkalmazás-specifikus-összekötő az alkalmazásnak a FIM szinkronizálási szolgáltatás határozza meg, hogyan biztosított, amelyek a felhasználó engedélyeinek módosítása. 

#### <a name="applications"></a>Alkalmazások

A BHOLD szerepköralapú hozzáférés-vezérlés (RBAC) alkalmazására külső alkalmazások metódus valósítja meg. Azt jelenti amikor BHOLD ki van építve a felhasználók és a egy alkalmazás engedélyeit, BHOLD társíthatja ezeket az engedélyeket a felhasználók szerepkörök hozzárendelése a felhasználókhoz és az engedélyek majd összekapcsolása a szerepkörök. Az alkalmazás háttérfolyamat leképezheti a megfelelő engedélyeket a felhasználóknak a szerepkör vagy engedély-leképezéssel a BHOLD alapján.

### <a name="developing-the-bhold-suite-role-model"></a>A BHOLD Suite szerepkör modell fejlesztése

Fejlesztés a szerepkör-modell segítségével BHOLD Suite Model Generator, egy eszköz, amely könnyen használható és átfogó biztosít.

Mielőtt Model Generator használ, létre kell hoznia, amelyek meghatározzák az objektumok Model Generator létrehozni a szerepkör modellt használó fájlok sorozatát. Ezek a fájlok létrehozásával kapcsolatos további információkért lásd: Microsoft BHOLD Suite – technikai útmutató.

Az első lépés a BHOLD Model Generator használatával a rendszer ezeket a fájlokat az alapvető alkotóelemeket-ba való betöltésének Model Generator importálja. Ha a fájlok sikeresen lett betöltve, Model Generator hozhat létre szerepköröket több osztályt használó feltétel majd adhatja meg:

- Tagsági szerepkörök rendelt felhasználó alapján, hogy a felhasználó tartozik OrgUnits (szervezeti egység)
- A BHOLD-adatbázis a felhasználói attribútumok alapján, a felhasználó hozzárendelt szerepkörök attribútum
- Javasolt olyan szerepköröket, amelyek egy szervezeti egységhez kapcsolódik, de a megadott felhasználók számára aktiválni kell
- Adja meg a szervezeti egységek és a szerepkörök, amelynek tulajdonosa nincs megadva az importált fájl a felhasználó szabályozhatja tulajdonosi szerepkörök

> [!Important]
> Fájlok feltöltésekor válassza ki a **megőrizheti a létező modell** jelölőnégyzet csak tesztelési környezeteket. Éles környezetben a kezdeti szerepkör modell létrehozásához Model Generator kell használnia. Nem használhatja a BHOLD-adatbázis egy meglévő szerepkör-modell módosítására.

Miután a Model Generator ezeket a szerepköröket a szerepkör-modellben, majd exportálhatja a szerepkör-modellt a BHOLD-adatbázis egy XML-fájl formájában.

### <a name="advanced-bhold-features"></a>A BHOLD speciális funkciók

Korábbi szakaszokban ismertetett szerepköralapú hozzáférés-vezérlés (RBAC) a BHOLD alapvető szolgáltatásait. Ez a szakasz ismerteti a BHOLD fokozott biztonságot és a szervezet megvalósítását RBAC rugalmasságot biztosító szolgáltatások. Ez a szakasz tartalmazza a következő BHOLD-funkciók áttekintésével:

- számossága
- A feladatkörök elkülönítése
- Testreszabható környezetet engedélyek
- Attribútum-alapú hitelesítést
- Rugalmas Attribútumtípusok


#### <a name="cardinality"></a>számossága

*Számosság* korlátozza, hogy hányszor két entitás kapcsolódhat egymással kialakított üzleti szabályok alkalmazása hivatkozik. A BHOLD, esetén Számosság szabályok szerepkörök, engedélyek és a felhasználók hozható létre.

Konfigurálhat egy szerepkör korlátozása a következő:

- A javasolt szerepkör aktiválhatja felhasználók maximális száma
- A szerepkör csatolható alszerepkörök maximális száma
- Az engedélyeket, amelyek a szerepkör lehet kapcsolódni a maximális számát

Korlátozhatja a következő engedély konfigurálhatja:

- A maximális számát, amely lehet kapcsolódni az engedély szerepkörök
- A maximális számát, akik engedéllyel is kell rendelkezni

A felhasználót, hogy korlátozza a következőket konfigurálhatja:

- A maximális számát, amely lehet kapcsolódni a felhasználói szerepkörök
- A felhasználó leállította a szerepkör-hozzárendelésekkel rendelhet engedélyeket maximális száma

#### <a name="separation-of-duties"></a>A feladatkörök elkülönítése

Feladatkörök (gyeptégla) egy üzleti alapelvet, amely arra törekszik, hogy megakadályozza, hogy a felhasználók eszközeikre, amely nem lehet egyetlen személy számára elérhető műveletek elvégzésére. Például egy alkalmazott kell egy fizetési igénylése és a fizetési engedélyezése nem sikerült. Gyeptégla elvének köszönhetően a szervezeteknek ellenőrzések és egyensúlyok megfelelő minimalizálása érdekében alkalmazott hiba vagy kötelezettségszegést való kockázati kitettség rendszert megvalósításához.

A BHOLD gyeptégla azáltal, hogy nem kompatibilis engedélyeket határozhat meg valósítja meg. Ha ezek az engedélyek vannak definiálva, BHOLD gyeptégla kényszerít megakadályozzák, hogy a kapcsolódó inkompatibilis engedélyek kapcsolódnak közvetlenül vagy öröklés útján, és megakadályozzák, hogy a felhasználók legyenek hozzárendelve több szerepkört, szerepkörök létrehozása során kombinált, akkor engedélyek nem kompatibilis, újra közvetlen hozzárendelés vagy öröklés útján. Szükség esetén az ütközések felülbírálható.

#### <a name="context-adaptable-permissions"></a>Testreszabható környezetet engedélyek

Az engedélyeket, amelyek automatikusan módosítható egy objektum attribútum alapján hoz létre, csökkentheti az engedélyek kezelése teljes száma. Környezet testreszabható engedélyek (nagybetűs) segítségével meghatározhatja a képletet, amely módosítja az engedélyt az alkalmazás az engedélyhez tartozó alkalmazásának módja engedély attribútumaként. Például létrehozhat egy képletet, amely a hozzáférési engedéllyel egy fájl mappába (keresztül egy biztonsági csoportot, a mappa hozzáférés-vezérlési lista társított) alapján, hogy a felhasználó módosítások tartozik egy szervezeti egység (szervezeti egység) tartalmazó teljes munkaidejű alkalmazottak vagy. Ha a felhasználó egy szervezeti egységből a másikba helyezik, az új engedély a rendszer automatikusan alkalmazza, és a régi engedély aktiválva. 

A Tengelysapka képlet lekérdezheti a attribútumának értéke, amely az alkalmazások, engedélyek, szervezeti egységek és felhasználók lettek alkalmazva.

#### <a name="attribute-based-authorization"></a>Attribútum-alapú hitelesítést

Egyirányú szabályozhatja, hogy egy szerepkör, amely egy szervezeti egységhez kapcsolódik (szervezeti egység) aktiválódik egy adott felhasználó a szervezeti egység attribútum-alapú hitelesítést (ABA) használatára van. ABA használatával automatikusan aktiválhatja a szerepkört csak bizonyos szabályokat a felhasználói attribútumok alapján teljesülése esetén. Például egy szerepkör kapcsolhat egy szervezeti egységhez, aktiválódik egy felhasználó, csak akkor, ha a felhasználó munkaköre megegyezik a beosztás a ABA szabályban. Ez így nem kell a manuálisan a felhasználó egy javasolt szerepkör aktiválásához. Ehelyett egy szerepkör a szervezeti egység összes olyan felhasználó számára, amely eleget tesz a szerepkör ABA szabály attribútum értéke aktiválható. Szabályok kombinálhatók, úgy, hogy a szerepkör aktiválásakor csak akkor, ha a felhasználói attribútumok felel meg a szerepkörhöz megadott ABA szabályok.

Fontos megjegyezni, hogy ABA szabály tesztek eredményét Számosság beállítások korlátozza. Például ha egy szabály a számossági beállítást határozza meg, hogy legfeljebb két felhasználót rendelhet hozzá egy szerepkörhöz, és a egy ABA szabály más módon kell aktiválnia egy szerepkört a négy felhasználó, a szerepkör az csak az első két olyan felhasználó, hogy a ABA teszt esetén aktiválódik.

#### <a name="flexible-attribute-types"></a>Rugalmas Attribútumtípusok

BHOLD attribútumokat a rendszer nagy mértékben bővíthetők. Megadhat új attribútumtípusokat, az ilyen objektumok felhasználóként, szervezeti egység (szervezeti egység) és -szerepkörök. Attribútumok megadhatók rendelkezik értékek, amelyek az egész szám, logikai (Igen/nem), csak alfanumerikus karakterek, dátum, idő és e-mail címek. Attribútumok egyetlen értékként vagy értéklista adható meg.

## <a name="attestation"></a>Állapotigazolás

A BHOLD Suite eszközöket biztosít, amelyek segítségével győződjön meg arról, hogy egyes felhasználók kapott megfelelő engedélyeket az azok üzleti műveletekhez. A rendszergazda-kezelés az igazolási folyamat tervezése a portálon a BHOLD igazolási modul által biztosított használhatja.

Az igazolási folyamat melyik kampány az megbízott lehetőséget kap a kampányok útján történik, és azt jelenti, hogy ellenőrizze, hogy a felhasználóknak, amelynek ők felelnek van-e a megfelelő hozzáférése legyen a BHOLD által kezelt alkalmazások és a megfelelő engedélyekkel Ezeket az alkalmazásokat. A kampány tulajdonosa ki van jelölve, a kampány felügyeletére, és győződjön meg arról, hogy a kampány végeznek megfelelően. A kampány egyszer vagy rendszeres hozható létre.

Általában a kampány steward lesz egy vezető, aki tanúsítja tartozó egy vagy több szervezeti egységet, amelynek a kezelő feladata felhasználókat a hozzáférési jogosultságokat. Megbízott automatikusan kijelölt folyamatban van a felhasználói attribútumok alapján kampány igazolt felhasználója számára, vagy a megbízott kampány egy fájlban, amely leképezi a minden felhasználó a kampányban résztvevő igazolt folyamatban van, a steward listázásával adható meg.

Kezdődik, amikor a BHOLD e-mailben értesítési üzenetet küld a kampány megbízott és a tulajdonos, és könnyebben karbantartása folyamatban van a kampányban résztvevő rendszeres időközönként emlékeztetőket küld. Megbízott irányítja a kampány portálra, ahol megjelenik, amelyhez felhasználókat és a felhasználókhoz rendelt szerepkörök listáját. A megbízott majd ellenőrizheti, hogy azok a listán szereplő felhasználók felelős és jóváhagyására a hozzáférési jogosultságokat az egyes a listán szereplő felhasználók.

Kampány tulajdonosok is a portál használatával nyomon követheti a kampány, és így kampány tulajdonosok elemezheti a kampány során végrehajtott műveletek naplózott tevékenységekről kampány.

## <a name="analytics"></a>Elemzés

A legfontosabb szempontok, ha végrehajtási egy átfogó rights-alapú hozzáférés-vezérlés (RBAC) rendszert a szigorú hozzáférés-vezérlés fenntartása, és kerülje a szükségtelen (vagy rosszabb, váratlan) közötti egyensúly egyik korlátok eléréséhez. Ez gyakran fenn részéről az erőfeszítés egy hozzáférés-vezérlési struktúra, hogy szinte elkerülhetetlen-e váratlan interakciók a házirendek között összetett eredményez.

Éppen ezért fontos lehet képes lesz elemezni a hozzáférés-vezérlési házirendek hatásait, mielőtt ténylegesen vezetnek be. A BHOLD Suite Analytics modul fejlesztése a szabályzatok képviselő szabályok, ami lehetővé teszi, és ezután engedélyeiket megjelenítése a felhasználók ehhez az elemzéshez képességével felelnek meg, vagy a szabály ütközik biztosít. Az elemzés alapján módosíthatja a szabályzatot, vagy módosíthatja azon szerepköröket és engedélyeket kiküszöbölése az ütközéseket a szabályzattal.

A BHOLD Analytics-portálon teszi lehetővé, amelyek egy vagy több szabályt annak hoz létre, tesztelheti egy adott szabályzat vagy szabályzatok állnak szabálykészletek létrehozására. A szabály a következő nagyobb részekből áll:

- Cím és leírás, amelyek lehetővé teszik azonosíthatja és a szabály leírása
- Egy állapot, amely azt jelzi, hogy tekintse át, készen áll-e a szabály alatt tekintse át, vagy jóváhagyott
- Egy elem (például a felhasználók vagy engedélyek) állítsa be, amely a szabály tesztelése
- Kifejezések, amelyek segítségével válassza ki a megfelelő alcsoport meg kell vizsgálni az elem nem kötelező részét szűrők
- Egy vagy több szabály szűrők, amelyek a kifejezések, amelyek a vizsgált házirendet.

A szabály a következő elem készletek közül bármelyik tesztelheti:

- Users
- Szervezeti egységek
- Szerepkörök
- Engedélyek
- Alkalmazások
- Fiókok

Az alábbi ábra egy egyszerű szabályt két részhalmazát szabályok és két Állapotszűrő szabályok:

![](media/bhold-concepts-guide/rules.png)

Vegye figyelembe a különbség a hatás, a sikertelen egy részhalmazát szűrőt, és a sikertelen szabályt szűrő: sikertelen volt egy részhalmazát szűrő távolít el elemobjektum tesztelése a soron következő szűrői során sikertelen szabály szűrő a legyen nem megfelelőként jelentett objektumot. Csak azokat az objektumokat, amelyek a részhalmazát-szűrők és a szabály az összes szűrő megfelelőként jelenti.

Minden szűrő áll egy, az operátornak (Ez a függő típusa), egy kulcsot (egy-egy elem) és egy értéket, amelyre vonatkozóan a kulcs az operátor lett tesztelve. Például a következő szűrő lenne megállapítására, hogy a felhasználók egy elem bejuthat száma meghaladja a 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Írja be:**   | Száma   |
| **Kulcs:**  | Users  |
| **Operátor**  | >  |
| **Érték:** | 10 |

A szabályok szűrők három típusa létezik, és adott a a típusuk, operátorok használata jelöli:

- Attribútum
  - < és >
  - = és! =
  - **tartalmaz**
  - **Nem tartalmazza**
- Száma
  - < és >
  - = és! =
- Korlátozó
  - **Bármely és kell rendelkeznie az összes**
  - **Nem rendelkezik ilyennel, és nem rendelkeznek**
  - **Csak rendelkezik, és csak lehet az összes**
  - **Kizárólag rendelkezik, és kizárólag rendelkeznek**

> [!Note]
> Korlátozó szűrők a jelzett operátorok használatával több érték összevetéssel kulcs teszteléséhez.

Például ha szeretne egy feladatkörök (gyeptégla) szabályzatot, amely arról tájékoztatja, hogy nincs felhasználó, aki jogosult a támogatási kérelem jóváhagyása fizetési engedélyre is nem elkülönítése végrehajtásának tesztelése, sikerült hozhat létre egy szabályt a következőhöz hasonló:

|   |  |
|---|--|
|Név:| Fizetési gyeptégla teszt|
|Elem:| Users|
|Részhalmazát szűrő:| Kellene bármilyen engedélye támogatási kérelem|
|A szabály szűrő: | Nem lehet bármilyen engedéllyel jóváhagyás fizetési|

Ez a szabály futtatásakor a BHOLD Analytics modul a kiválasztott részhalmazát (a támogatási kérelem engedéllyel rendelkező felhasználók száma) a felhasználók száma, amelyek megfelelnek a szabály a felhasználók száma és, amelyek nem felelnek meg a szabály felhasználók számát jeleníti meg. A nem megfelelő felhasználók ezután meg tudja jeleníteni, így kiküszöbölheti a inkonzisztenciát.

Az eredmények megjelenítése, mellett is mentheti a jelentés vesszővel tagolt (CSV) vagy XML-fájlt, hogy később elemezze az eredményeket. Az eredményül kapott jelentést, hogy további információkat, amelyek segítségével jobban megismerheti a hatását is testreszabhatja. Például ha felhasználók teszteli, Ön is megjelenítéséhez (vagy jelentés) a fiókokat a megfelelő vagy nem megfelelő felhasználók, így láthatja, hogy mely alkalmazásokat is érint.

Szabály több szűrőket is tartalmazhatnak, mert a kapcsolódás tesztelése, hogy hogy létezik-e egy adott feltételek kombinációja. Alapértelmezés szerint az eredmény a termék-és a logikai vizsgálat az összes szűrőt, de megadhatja, hogy a szűrő kombináció egy OR teszt hajtható végre.

Például ha a szabályzat van szüksége a kezelők vagy a fizetési módosítása engedéllyel, vagy a fizetési jóváhagyása engedéllyel rendelkezik, majd tesztelhet a házirendnek való megfelelőség hozhat létre egy szabályt a következőhöz hasonló:


|  |  |
|--|--|
|Név: | Fizetési gyeptégla teszt módosítása|
|Elem: | Users |
|Részhalmazát szűrő: | Bármely szerepkör Manager kellene|
| A szabály szűrők: |Bármilyen módosítás fizetési engedéllyel kell rendelkeznie. </br> Bármely jóváhagyás fizetési engedéllyel kell rendelkeznie|

Alapértelmezés szerint minden olyan felhasználó, aki egy vezető, aki módosítása fizetés és a fizetési kérése engedéllyel is rendelkezik kerülnek megfelelőnek. Azonban a szabályzat megköveteli, hogy a kezelő vagy engedély, nem feltétlenül is. Tényleges megfelelőségi szabályzat teszteléséhez kell használnia vagy logikai operátor a szabályt meghatározni, hogy vannak-e bármilyen vezetők, akik nem rendelkeznek a fizetési módosítása engedéllyel vagy a fizetési jóváhagyása engedéllyel.

Ellentétben más operátorokkal a **kizárólag rendelkezik** és a **kizárólag rendelkeznek** operátorok azt jelzik, ellenkező esetben lenne egy részhalmazát szűrő által kizárt objektumok megfelelőségét. Például, hogy minden vezetők (és csak menedzserek) engedélye jóváhagyása felülvizsgálatok házirend tesztelése sikerült hozhat létre egy szabályt a következő:

|  |  |
|--|--|
|Név: | Felülvizsgálati jóváhagyás teszt|
|Elem: | Users|
| Részhalmazát szűrő: | Bármely szerepkör Manager kellene
|A szabály szűrő: | Kizárólag a minden engedélyt jóvá felülvizsgálatra van|

Ez a szabály megfelelő felhasználók kezelők jelentés, és a jóváhagyása felülvizsgálatok engedély és felhasználók, akik nem kezelők, és akiknél nincs telepítve a jóváhagyása felülvizsgálatok engedéllyel rendelkezik. Ezzel szemben a vezetők, akik nem rendelkeznek engedéllyel, és a felhasználók, akik nem kezelők, de a engedélye jelentésen nem megfelelőként.

Ahogy korábban említettük, kombinálható szabályok egy szabálykészletben egyszerűbben kategorizálása és az üzleti szükségletek kielégítése céljából szabályok kezelése.

Azt is megadhatja, globális készlete, amely szűri, ha engedélyezve van, bármilyen tesztelni kívánt szabály vonatkozik. Milyen gyakran szeretne kizárni egy adott részét rekordok különböző szabálykészletek szabályok tesztelésekor, ha a globális szűrők, amelyek engedélyezik, vagy tiltsa le, igény szerint is megadhat.

## <a name="reporting"></a>Jelentéskészítés

A BHOLD Reporting modul teszi lehetővé a jelentések különböző szerepkör modellben információk megtekintéséhez. A BHOLD Reporting modul képletkategória olyan beépített jelentést biztosít, valamint tartalmaz egy varázslót, amely segítségével alapvető és speciális egyéni jelentéseket hozhat létre. A jelentés futtatásakor azonnal megjeleníti az eredményeket vagy a-eredményeket menteni egy Microsoft Excel (.xlsx) fájlban. Ez a fájl megtekintéséhez használja a Microsoft Excel 2000, a Microsoft Excel 2002-es vagy a Microsoft Excel 2003, töltse le és telepítse a Microsoft Office kompatibilitási csomagot a Word, Excel és PowerPoint-fájl formázza.


Használatával a BHOLD Reporting modul többnyire aktuális szerepköradatok alapuló jelentések előállítása érdekében. Azonosító adatok módosításait naplózási jelentések létrehozását, a Forefront Identity Manager 2010 R2 jelentéskészítő képességgel rendelkezik a FIM szolgáltatás, amely implementálva van a System Center Service Manager adatraktár. A FIM reporting kapcsolatos további információkért lásd: a Forefront Identity Manager 2010 és a Forefront Identity Manager 2010 R2 dokumentáció a Forefront Identity Management technikai könyvtárban.

A beépített jelentések által kezelt kategóriák a következők:

- Administration
- Állapotigazolás
- vezérlők
- Aktív hozzáférés-vezérlés
- Naplózás
- Modell
- statisztika
- Munkafolyamat

Jelentéseket hozhat létre, és hozzáadhatja őket a kategóriák, vagy megadhatja saját kategóriákkal, ahol egyéni és beépített jelentések helyezheti.

Jelentések létrehozása, a varázsló végigvezeti Önt a következő paraméterek megadása:

- Azonosító információkat, beleértve a nevét, leírását, kategória, használati, és a célközönség
- A jelentésben megjeleníteni kívánt mező
- Adja meg, hogy mely elemek szerepelnek a vizsgálandó lekérdezések
- Sorrendje sorok rendezni kívánt
- Bontsa a jelentést szakaszokra mezőket
- Annak érdekében, hogy finomíthatja a jelentésben visszaadott elemek

A varázsló minden fázisban megtekintheti a jelentést, amennyiben definiált, és mentheti, ha nem szeretne további paraméterek megadását. Is áthelyezheti vissza az előző lépést, amely korábban a varázslóban megadott paraméterek módosításához.

## <a name="access-management-connector"></a>Access Management-összekötő

A BHOLD Suite Access Management-összekötő modul BHOLD be az adatok kezdeti és a folyamatban lévő szinkronizálás támogatja. Az Access Management-összekötő működik a FIM szinkronizálási szolgáltatás többek között a BHOLD Core adatbázis, a MIM metaverzum, és a célként megadott alkalmazások és a identitástárolók adatok áthelyezéséhez.

A BHOLD korábbi verziói több MAs szabályozhatja a MIM és a köztes BHOLD-adatbázis tábláinak közötti adatáramlás szükséges. A BHOLD Suite SP1 az Access Management-összekötő lehetővé teszi, hogy felügyeleti ügynökök (MAs) meghatározhatja a mim szoftverben, amelyek közvetlenül a BHOLD és a MIM közötti adatátvitelt.

## <a name="mim-integration"></a>A MIM-integráció

Egy fontos és hatékony funkció, a Forefront Identity Manager 2010 és a Forefront Identity Manager 2010 R2-ről, amely lehetővé teszi a végfelhasználók számára, hogy megtekintése és kezelése az identitás- és csoporttagság adataikat az önkiszolgáló portál. A MIM-integráció a MIM portál felügyeleti önkiszolgáló szerepkörrel rendelkező terjeszti ki. Például a MIM-portál a BHOLD-funkciók révén a felhasználó kérheti a szerepkör-hozzárendelés, és megtekintheti az aktív szerepkörök és a függőben lévő kérelmek. További funkciókat, például a kérés szerepkör-hozzárendelések más felhasználók, delegált rendszergazdák is megadható.

Vegye figyelembe, hogy a MIM-portálhoz a BHOLD-bővítmények támogatják-e a önkiszolgáló szerepkört és a munkafolyamat felügyeleti és jelentéskészítési. Más BHOLD felügyeleti funkciókat, valamint a tanúsítvány, a webes portálok a BHOLD-modulok, amelyre a MIM-portál a külön-külön üzemeltetett által biztosított.

## <a name="next-steps"></a>További lépések

- [A BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

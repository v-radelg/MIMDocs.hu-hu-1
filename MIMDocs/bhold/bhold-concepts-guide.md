---
title: A Microsoft BHOLD Suite fogalmi útmutatója | Microsoft Docs
description: Első lépések a MIM 2016 összetevői kapcsán – a Synchronization Service telepítése és konfigurálása
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: f120709e517d82d4f94e72f4d0a44361f5552a1c
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042304"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>A Microsoft BHOLD Suite fogalmi útmutatója

Microsoft Identity Manager 2016 (a Rendszerfelügyeleti webszolgáltatások) lehetővé teszi a szervezetek számára a felhasználói identitások és a hozzájuk tartozó hitelesítő adatok teljes életciklusának kezelését. Beállítható úgy, hogy az identitásokat szinkronizálja, központilag kezelje a tanúsítványokat és a jelszavakat, és a felhasználókat a heterogén rendszerek között lehessen kiépíteni. A felügyeleti webszolgáltatások segítségével az IT-szervezetek meghatározhatják és automatizálják a létrehozástól a kivonulásig az identitások kezeléséhez használt folyamatokat.

A Microsoft BHOLD Suite szerepkör-alapú hozzáférés-vezérlés hozzáadásával kiterjeszti a felügyeleti csomag képességeit. A BHOLD lehetővé teszi a szervezetek számára a felhasználói szerepkörök definiálását, valamint a bizalmas adatokhoz és alkalmazásokhoz való hozzáférés szabályozását. A hozzáférés ezen szerepkörökhöz megfelelő. A BHOLD Suite olyan szolgáltatásokat és eszközöket tartalmaz, amelyek leegyszerűsítik a szerepkör-kapcsolatok modellezését a szervezeten belül. A BHOLD leképezi a szerepköröket, és ellenőrzi, hogy a szerepkör-definíciók és a hozzájuk tartozó jogosultságok megfelelően vannak-e alkalmazva a felhasználókra Ezek a képességek teljes mértékben integrálva vannak a felhasználói felülettel, zökkenőmentes élményt biztosítva a végfelhasználók és az informatikai szakemberek számára.

Ez az útmutató segít megérteni, hogyan működik együtt a BHOLD Suite, és hogyan fedi le a következő témákat:

- Szerepköralapú hozzáférés-vezérlés
- Igazolás
- Elemzés
- Jelentéskészítés
- Hozzáférés-kezelési összekötő
- Webalkalmazás-integráció

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés

Az adatkezeléshez és az alkalmazásokhoz való felhasználói hozzáférés szabályozásának leggyakoribb módja a tulajdonosi hozzáférés-vezérlés (DAC). A leggyakoribb implementációkban minden jelentős objektum rendelkezik egy azonosított tulajdonossal. A tulajdonos lehetősége van arra, hogy egyéni identitás vagy csoporttagság alapján megadjon vagy megtagadja az objektum elérését másoknak. A gyakorlatban a DAC általában számos biztonsági csoportot eredményez, amelyek a szervezeti struktúrát tükrözik, mások pedig a funkcionális csoportokat (például a feladattípusokat vagy a projekt-hozzárendeléseket), mások pedig a felhasználók és a a további ideiglenes célokra csatolt eszközök. A szervezetek növekedésével a csoportokba való csatlakozás egyre nehezebbé válik. Ha például egy alkalmazott átkerül egyik projektből a másikba, a projektek eszközeihez való hozzáférés szabályozásához használt csoportokat manuálisan kell frissíteni. Ilyen esetekben nem ritka, hogy hibák történnek, ami akadályozhatja a projekt biztonságát vagy a termelékenységet.

A felügyeleti csomag olyan szolgáltatásokat tartalmaz, amelyek segítenek a probléma megoldásában azáltal, hogy automatizált vezérlést biztosítanak a csoport-és terjesztési listák tagságához. Ez azonban nem teszi lehetővé a nem szükségszerűen egymással összekapcsolt csoportok strukturált módon való összeépítésének összetettségét.

A proliferáció jelentős csökkentésének egyik módja a szerepköralapú hozzáférés-vezérlés (RBAC) üzembe helyezése. A RBAC nem végzi el a DAC kitörését.  A RBAC a DAC-ra épül, és keretet biztosít a felhasználók és az IT-erőforrások besorolásához. Így explicit módon megadhatja a kapcsolatát és az adott besorolás szerint megfelelő hozzáférési jogosultságokat. Ha például egy olyan felhasználói attribútumhoz rendel hozzá felhasználókat, amely megadja a felhasználók feladatának címét és a projekt-hozzárendeléseket, akkor a felhasználó számára elérhetővé teheti azokat az eszközöket, amelyekre a felhasználónak szüksége van ahhoz, hogy egy adott projekthez hozzájáruljanak. Ha a felhasználó egy másik feladatot és a projekt különböző hozzárendeléseit feltételezi, akkor a felhasználó beosztását és projektjeit megadó attribútumok módosítása automatikusan letiltja az erőforrásokhoz való hozzáférést a korábbi felhasználók számára szükséges erőforrások eléréséhez.

Mivel a szerepkörök hierarchikus módon is szerepelhetnek a szerepkörökben (például az értékesítési vezető és az értékesítési képviselő szerepkörei az értékesítések általánosabb szerepkörében is szerepelhetnek), könnyen hozzárendelhetők a megfelelő jogosultságok az adott szerepkörökhöz, és még mindig biztosítanak megfelelő jogosultságok mindenki számára, akik a több befogadó szerepkört is megosztják. Például egy kórházban minden egészségügyi szakember jogosult lehet megtekinteni a betegeket, de csak az orvosok (az orvosi szerepkör alszerepköre) jogosultak a beteg előírásának megadására. Hasonlóképpen, a tulajdonosi szerepkörhöz tartozó felhasználókat nem lehet megtagadni a beteg-nyilvántartásokhoz való hozzáférést, kivéve a számlázási hivatalnokok (az elírásos szerepkör alszerepköre) számára, akik hozzáférést kaphatnak a betegeknek a szolgáltatásokhoz való számlázásához szükséges adatrekordokhoz. a kórház által biztosított.

A RBAC további előnyei a feladatok elkülönítésének (SoD) meghatározása és kikényszerített lehetősége. Ez lehetővé teszi, hogy a szervezet olyan szerepköröket határozzon meg, amelyek olyan engedélyeket biztosítanak, amelyek nem ugyanazon felhasználó birtokában vannak, így egy adott felhasználó nem rendelhet hozzá olyan szerepköröket, amelyek lehetővé teszik a felhasználó számára a fizetés elindítását és a fizetések engedélyezését, például:. A RBAC lehetővé teszi, hogy az ilyen szabályzatok automatikusan érvénybe lépjenek ahelyett, hogy a házirendet felhasználónként kiértékelje.

### <a name="bhold-role-model-objects"></a>BHOLD szerepkör-objektumok

A BHOLD Suite segítségével megadhatja és rendszerezheti a szervezeten belüli szerepköröket, leképezheti a felhasználókat a szerepkörökre, és leképezheti a megfelelő engedélyeket a szerepkörökhöz. Ezt a struktúrát szerepkör-modellnek nevezzük, amely öt típusú objektumot tartalmaz és kapcsol össze: 

- Szervezeti egységek
- Felhasználók
- Szerepkörök
- Engedélyek
- Alkalmazások

#### <a name="organizational-units"></a>Szervezeti egységek

A szervezeti egységek (OrgUnits) az a fő módszer, amellyel a felhasználók a BHOLD szerepkör-modellben vannak rendszerezve. Minden felhasználónak legalább egy OrgUnit kell tartoznia. (Valójában, ha egy felhasználó el lett távolítva a BHOLD-beli utolsó szervezeti egységből, a rendszer törli a felhasználó adatrekordját a BHOLD-adatbázisból.)

> [!Important]
> A BHOLD szerepkör-modell szervezeti egységei nem tévesztendő össze a szervezeti egységekkel Active Directory tartományi szolgáltatások (AD DS). A BHOLD szervezeti egységének szerkezete jellemzően a vállalat szervezetén és házirendjein alapul, nem pedig a hálózati infrastruktúra követelményeinek.

Bár ez nem kötelező, a legtöbb esetben a szervezeti egységek strukturálva vannak a BHOLD, hogy a tényleges szervezet hierarchikus struktúráját képviseljék, az alábbihoz hasonlóan:

![](media/bhold-concepts-guide/org-chart.png)

Ebben a példában a szerepkör-modell a vállalat teljes organizationalganizatinal egysége lesz (amelyet az elnök képvisel, mert az elnök nem része egy mororganizationalganizatinal egységnek) vagy a BHOLD gyökérszintű szervezeti egységnek (amely mindig létezik) erre a célra használható. Az alelnökök által vezetett vállalati részlegek OrgUnits a vállalati szervezeti egységbe kerül. Ezután a marketing-és értékesítési igazgatóknak megfelelő szervezeti egységek hozzáadódnak a marketing-és értékesítési szervezeti egységekhez, és a regionális értékesítési vezetőket képviselő szervezeti egységek a szervezeti egységbe kerülnek a Kelet-régió Sales Manager. A többi felhasználót nem kezelő Sales Associates a keleti régió Sales Manager szervezeti egységének tagjai lesznek. Vegye figyelembe, hogy a felhasználók bármilyen szinten tagjai lehetnek egy szervezeti egységnek. Például egy olyan felügyeleti segéd, amely nem felettes, és nem közvetlenül egy alelnök, a alelnöke szervezeti egységének tagja lenne.

A szervezeti struktúrán kívül a szervezeti egységek a felhasználók és más szervezeti egységek csoportosítására is használhatók a funkcionális feltételek szerint, például projektekhez vagy specializációhoz. Az alábbi ábra bemutatja, hogyan használhatók a szervezeti egységek az értékesítési munkatársak csoportosításához az ügyfél típusa szerint:

![](media/bhold-concepts-guide/org-chart-02.png)

Ebben a példában az egyes értékesítési hozzárendelések két szervezeti egységhez tartoznak: a szervezet felügyeleti struktúrájában a munkatárs helyét, a másik pedig a munkatárs ügyfélkörét (kereskedelmi vagy vállalati) jelképezi. Minden szervezeti egység különböző szerepkörökhöz rendelhető, amelyek viszont különböző engedélyeket rendelhetnek a szervezet informatikai erőforrásaihoz való hozzáféréshez. Emellett a szerepkörök örökölhető a szülő szervezeti egységektől, így egyszerűbbé válik a szerepkörök felhasználók számára történő propagálásának folyamata. Másfelől előfordulhat, hogy bizonyos szerepköröket nem lehet örökölni, így biztosítható, hogy egy adott szerepkör csak a megfelelő szervezeti egységekhez legyen társítva.

A OrgUnits a BHOLD Suite-ban a BHOLD Core webportál vagy a BHOLD Model Generator használatával hozhatók létre.

#### <a name="users"></a>Felhasználók

Ahogy fent említettük, minden felhasználónak legalább egy szervezeti egységhez (OrgUnit) kell tartoznia. Mivel a szervezeti egységek a felhasználók szerepkörrel való társításának fő mechanizmusa, az adott felhasználó többsége több OrgUnits tartozik, így egyszerűbbé válik a szerepkörök hozzárendelése a felhasználóhoz. Bizonyos esetekben azonban szükséges lehet egy szerepkört hozzárendelni egy felhasználóhoz a felhasználóhoz tartozó összes OrgUnits. Ennek következtében a felhasználók közvetlenül is hozzárendelhetők egy szerepkörhöz, valamint olyan szerepkörök beszerzéséhez a OrgUnits, amelyekhez a felhasználó tartozik.

Ha a felhasználó nem aktív a szervezeten belül (az orvosi elhagyás ideje alatt például), a felhasználó felfüggeszthető, amely visszavonja a felhasználó engedélyeit anélkül, hogy el kellene távolítania a felhasználót a szerepkör-modellből. A feladathoz való visszatérés után a felhasználó újraaktiválható, amely a felhasználó szerepkörei által biztosított összes engedélyt visszaállítja.

A felhasználók számára a BHOLD Core webportálon keresztül önállóan hozhatók létre objektumok, vagy a BHOLD Model Generator használatával tömegesen importálhatók, vagy a hozzáférés-kezelési összekötővel a FIM szinkronizációs szolgáltatással BHOLD importálhatók a következő helyről: ilyen források Active Directory tartományi szolgáltatások vagy emberi erőforrások alkalmazásaiban.

A felhasználók közvetlenül a BHOLD hozhatók létre a FIM szinkronizációs szolgáltatás használata nélkül. Ez akkor lehet hasznos, ha az előkészítési vagy tesztelési környezetben a szerepkörök modellezése folyamatban van. Azt is lehetővé teheti, hogy a külső felhasználók (például az alvállalkozó alkalmazottai) szerepköröket rendelnek hozzá, és így hozzáférést kapjanak az informatikai erőforrásokhoz anélkül, hogy az alkalmazott adatbázishoz hozzá kellene adni őket. Ezek a felhasználók azonban nem fogják tudni használni a BHOLD önkiszolgáló funkcióit.

#### <a name="roles"></a>Szerepkörök

Ahogy azt korábban említettük, a szerepköralapú hozzáférés-vezérlés (RBAC) modellben az engedélyek az egyéni felhasználók helyett szerepkörökhöz vannak társítva. Ez lehetővé teszi, hogy minden felhasználó számára engedélyezze a felhasználó feladatainak elvégzéséhez szükséges engedélyeket úgy, hogy a felhasználó szerepköreit nem külön adja meg vagy tagadja meg a felhasználói engedélyek megadását vagy elutasítását. Ennek következményeként az engedélyek hozzárendelése már nem igényli az IT-részleg részvételét, hanem a vállalat felügyeletének részeként is elvégezhető. Egy szerepkör összesítheti a különböző rendszerek elérésére vonatkozó engedélyeket közvetlenül vagy alszerepkörök használatával, így tovább csökkentve a felhasználói engedélyek kezelésének szükségességét.

Fontos megjegyezni, hogy a szerepkörök a RBAC-modell egyik funkciója; jellemzően a szerepkörök nincsenek kiépítve az alkalmazásokhoz. Ez lehetővé teszi, hogy a RBAC olyan meglévő alkalmazások mellett legyenek használhatók, amelyek nem szerepkörök használatára vagy a szerepkör-definíciók módosítására szolgálnak az üzleti modellek módosításának igénye nélkül, anélkül, hogy az alkalmazásokat módosítani kellene. Ha a célalkalmazás szerepkörök használatára van kialakítva, akkor a szerepköröket a BHOLD szerepkör-modellben társíthatja a megfelelő alkalmazás-szerepkörökkel, ha az alkalmazásspecifikus szerepköröket engedélyként kezeli.

A BHOLD-ben a szerepköröket elsődlegesen két mechanizmussal rendelheti hozzá a felhasználóhoz:

- Szerepkör kiosztása egy olyan szervezeti egységhez (szervezeti egységhez), amelynek a felhasználó tagja
- Szerepkör közvetlen kiosztása egy felhasználóhoz

A szülő szervezeti egységhez rendelt szerepkört a tag szervezeti egységei öröklik. Ha egy szervezeti egység hozzárendeli vagy örökli a szerepkört, akkor a rendszer érvényes vagy javasolt szerepkörként jelöli ki. Ha ez egy hatékony szerepkör, a szervezeti egységben lévő összes felhasználó hozzá lesz rendelve a szerepkörhöz. Ha ez egy javasolt szerepkör, akkor minden egyes felhasználó vagy tag szervezeti egység számára aktiválva kell lennie ahhoz, hogy az adott felhasználó vagy szervezeti egység tagjai számára érvénybe váljanak. Ez lehetővé teszi a felhasználók számára a szervezeti egységhez társított szerepkörök egy részhalmazának hozzárendelését ahelyett, hogy automatikusan hozzárendelje az összes szervezeti egység szerepkörét az összes taghoz. Emellett a szerepkörök is megadhatják a kezdő és a befejező dátumokat, a korlátok pedig egy olyan szervezeti egységben lévő felhasználók százalékos arányára helyezhetők el, amelyekhez a szerepkör érvényes lehet.

Az alábbi ábra azt szemlélteti, hogyan lehet egy adott felhasználóhoz hozzárendelni egy szerepkört a BHOLD-ben:

![](media/bhold-concepts-guide/org-chart-flow.png)

Ebben a diagramban az A szerepkör örökölhető szerepkörként van hozzárendelve egy szervezeti egységhez, ezért a tag szervezeti egységei és a szervezeti egységben lévő összes felhasználó örökli. A B szerepkör a szervezeti egységhez javasolt szerepkörként van hozzárendelve. Aktiválni kell, mielőtt a szervezeti egységben lévő felhasználó engedélyt kaphat a szerepkör engedélyeire. A C szerepkör érvényes szerepkör, így az engedélyei azonnal érvényesek a szervezeti egységben lévő összes felhasználóra. A D szerepkör közvetlenül a felhasználóhoz van csatolva, így az engedélyei azonnal érvényesek a felhasználóra.

Emellett a felhasználók a felhasználó attribútumai alapján is aktiválhatja a szerepköröket. További információ: attribútum-alapú hitelesítés.

#### <a name="permissions"></a>Engedélyek

A BHOLD engedély a célalkalmazás által importált engedélynek felel meg. Vagyis ha a BHOLD úgy van konfigurálva, hogy egy alkalmazással működjön, megkapja azon engedélyek listáját, amelyeket a BHOLD a szerepkörökhöz tud kapcsolni. Ha például Active Directory tartományi szolgáltatások (AD DS) van hozzáadva a BHOLD alkalmazáshoz, akkor megkapja azon biztonsági csoportok listáját, amelyeket a BHOLD engedélyeivel társíthat a BHOLD szerepköreihez.

Az engedélyek egyediek az alkalmazásokra. A BHOLD egyetlen, egységesített nézetet biztosít az engedélyek számára, hogy az engedélyek társítva legyenek a szerepkörökhöz anélkül, hogy a szerepkör-kezelők megértsék az engedélyek megvalósítási részleteit. A gyakorlatban a különböző rendszerek eltérő jogosultságokat igényelhetnek. Az alkalmazás-specifikus összekötő a FIM synchronization Service-ből az alkalmazásba meghatározza, hogy az adott alkalmazás milyen módosításokat biztosít a felhasználók számára. 

#### <a name="applications"></a>Alkalmazások

A BHOLD megvalósítja a szerepköralapú hozzáférés-vezérlés (RBAC) külső alkalmazásokba való alkalmazásának módszerét. Vagyis ha a BHOLD-t egy alkalmazás felhasználóinak és engedélyeinek megfelelően kiépítik, a BHOLD a felhasználókhoz szerepkörök hozzárendelésével, majd a szerepkörökhöz való csatolásával társíthatja ezeket az engedélyeket a felhasználóhoz. Az alkalmazás háttérbeli folyamata ezután leképezheti a megfelelő engedélyeket a felhasználók számára a BHOLD szerepkör/engedélyek leképezése alapján.

### <a name="developing-the-bhold-suite-role-model"></a>A BHOLD Suite szerepkör-modell fejlesztése

A BHOLD Suite lehetővé teszi a modell fejlesztését, amely könnyen használható és átfogó megoldás.

A Model Generator használata előtt létre kell hoznia egy olyan fájl-sorozatot, amely meghatározza, hogy a Model Generator milyen objektumokat állít össze a szerepkör-modell létrehozásához. További információ a fájlok létrehozásáról: a Microsoft BHOLD Suite technikai útmutatója.

A BHOLD Model Generator használatának első lépéseként importálnia kell ezeket a fájlokat, hogy betöltse az alapszintű építőelemeket a Model Generatorba. A fájlok sikeres betöltése után megadhatja azokat a feltételeket, amelyeket a Model Generator használ a szerepkörök több osztályának létrehozásához:

- A felhasználóhoz tartozó OrgUnits (szervezeti egységek) alapján a felhasználókhoz rendelt tagsági szerepkörök
- A felhasználóhoz a BHOLD-adatbázisban a felhasználó attribútumai alapján hozzárendelt attribútumok szerepkörei
- Szervezeti egységhez kapcsolódó javasolt szerepkörök, de bizonyos felhasználók számára aktiválva kell őket
- Olyan tulajdonosi szerepkörök, amelyek felhasználói ellenőrzést biztosítanak a szervezeti egységek és a szerepkörök számára, amelyekhez nincs megadva a tulajdonos az importált fájlokban

> [!Important]
> Fájlok feltöltésekor jelölje be a **meglévő modell megőrzése** jelölőnégyzetet csak tesztelési környezetekben. Éles környezetekben a modell generátort kell használnia a kezdeti szerepkör-modell létrehozásához. A BHOLD-adatbázisban nem használható meglévő szerepkör-modell módosítására.

Miután a Model Generator létrehozza ezeket a szerepköröket a szerepkör-modellben, XML-fájl formájában exportálhatja a szerepkör-modellt a BHOLD-adatbázisba.

### <a name="advanced-bhold-features"></a>Speciális BHOLD funkciók

Az előző szakaszokban a szerepköralapú hozzáférés-vezérlés (RBAC) alapszintű funkciói szerepelnek a BHOLD-ben. Ez a szakasz a BHOLD további szolgáltatásait ismerteti, amelyek fokozott biztonságot és rugalmasságot biztosítanak a szervezet RBAC megvalósításához. Ez a szakasz áttekintést nyújt a következő BHOLD-funkciókról:

- Számosság
- A feladatok elkülönítése
- Környezetfüggő engedélyek
- Attribútum-alapú hitelesítés
- Rugalmas attribútumok típusai


#### <a name="cardinality"></a>Számosság

A *kardinális* az üzleti szabályok megvalósítására szolgál, amelyek célja, hogy korlátozza a két entitás egymáshoz való kapcsolódásának időtartamát. BHOLD esetén a rendszer a szerepkörök, engedélyek és felhasználók számára is létrehozhatja a kardinális szabályokat.

Beállíthat egy szerepkört a következők korlátozására:

- Azoknak a felhasználóknak a maximális száma, amelyekhez javasolt szerepkört lehet aktiválni
- A szerepkörhöz kapcsolható alszerepkörek maximális száma
- A szerepkörhöz hozzárendelhető engedélyek maximális száma

Beállíthat egy engedélyt a következők korlátozására:

- Az engedélyhez társítható szerepkörök maximális száma
- Az engedélyt megadható felhasználók maximális száma

Beállíthatja, hogy a felhasználó korlátozza a következőket:

- A felhasználóhoz hozzárendelhető szerepkörök maximális száma
- A felhasználóhoz szerepkör-hozzárendeléseken keresztül hozzárendelhető engedélyek maximális száma

#### <a name="separation-of-duties"></a>A feladatok elkülönítése

A feladatok elkülönítése (SoD) olyan üzleti elv, amely arra törekszik, hogy megakadályozza, hogy a felhasználók olyan műveleteket hajtsanak végre, amelyeknek nem szabad egyetlen személy számára elérhetővé tenniük. Egy alkalmazott például nem igényelhet fizetést, és engedélyezheti a fizetést. A SoD elve lehetővé teszi a szervezetek számára az ellenőrzések és egyensúlyok rendszerének megvalósítását, hogy az alkalmazotti hiba vagy a kötelességszegés miatti kockázatokat csökkentsék.

A BHOLD nem kompatibilis engedélyek definiálásával valósítja meg a SoD-t. Ha ezek az engedélyek meg vannak adva, a BHOLD úgy kényszeríti a SoD-t, hogy megakadályozza a nem kompatibilis engedélyekhez kapcsolódó szerepkörök létrehozását, függetlenül attól, hogy közvetlenül vagy örökléssel vannak-e társítva, valamint hogy a felhasználók nem kapnak-e több olyan szerepkört, amely a kombinált szolgáltatás nem kompatibilis engedélyeket ad vissza közvetlen hozzárendeléssel vagy örökléssel. Az ütközések felülbírálása is lehetséges.

#### <a name="context-adaptable-permissions"></a>Környezetfüggő engedélyek

Az objektumok attribútuma alapján automatikusan módosítható engedélyek létrehozásával csökkentheti a felügyelni kívánt engedélyek teljes számát. A környezetfüggő engedélyek (CAPs) lehetővé teszik a képletek engedélyezési attribútumként való definiálását, amely módosítja, hogy a rendszer hogyan alkalmazza az engedélyt az engedélyhez társított alkalmazás. Létrehozhat például egy olyan képletet, amely módosítja a hozzáférési engedélyt egy fájl mappájára (a mappa hozzáférés-vezérlési listájához társított biztonsági csoporton keresztül) attól függően, hogy a felhasználó a következőt tartalmazó szervezeti egységhez (szervezeti egységhez) tartozik-e: teljes idejű vagy szerződéses alkalmazottak. Ha a felhasználót az egyik szervezeti egységről egy másikra helyezi át, a rendszer automatikusan alkalmazza az új engedélyt, és a régi engedély inaktiválva lesz. 

A CAP-képlet az alkalmazásokra, engedélyekre, szervezeti egységekre és felhasználókra alkalmazott attribútumok értékeit kérdezi le.

#### <a name="attribute-based-authorization"></a>Attribútum-alapú hitelesítés

Az egyik módszer annak szabályozására, hogy egy szervezeti egységhez (szervezeti egységhez) kapcsolódó szerepkör aktiválva van-e egy adott felhasználó számára a szervezeti egységben, az attribútum-alapú hitelesítés (ABA) használata. Az ABA-k használatával automatikusan aktiválhat egy szerepkört, ha a felhasználó attribútumain alapuló bizonyos szabályok teljesülnek. Például összekapcsolhat egy szerepkört egy olyan szervezeti egységhez, amely csak akkor válik aktívvá a felhasználó számára, ha a felhasználó beosztása megegyezik az ABA-szabályban szereplő feladat címével. Így nem kell manuálisan aktiválni egy javasolt szerepkört a felhasználó számára. Ehelyett egy szerepkör aktiválható egy olyan szervezeti egység összes felhasználója számára, aki attribútum értéke megfelel a szerepkör ABA-szabályának. A szabályok kombinálhatók, így egy szerepkör csak akkor aktiválódik, ha a felhasználó attribútumai megfelelnek a szerepkörhöz megadott összes ABA-szabálynak.

Fontos megjegyezni, hogy az ABA-szabályok tesztelésének eredményei a kardinális beállításokkal vannak korlátozva. Ha például egy szabály kardinális beállítása azt adja meg, hogy legfeljebb két felhasználó rendelhető hozzá egy szerepkörhöz, és ha egy ABA-szabály nem aktiválja a szerepkört négy felhasználó számára, akkor a szerepkör csak az ABA-tesztet áthaladó első két felhasználó számára aktiválódik.

#### <a name="flexible-attribute-types"></a>Rugalmas attribútumok típusai

A BHOLD-ben lévő attribútumok rendszere nagyon bővíthető. Az ilyen objektumokhoz a felhasználók, szervezeti egységek (szervezeti egységek) és szerepkörök használatával adhat meg új attribútum-típusokat. Az attribútumok meghatározhatók úgy, hogy egész számok, logikai (igen/nem), alfanumerikus, dátum, idő és e-mail-címek legyenek. Az attribútumok meghatározhatók egyetlen értékként vagy értéklistaként.

## <a name="attestation"></a>Igazolás

A BHOLD Suite olyan eszközöket biztosít, amelyekkel ellenőrizheti, hogy az egyes felhasználók megfelelő engedélyekkel rendelkeznek-e az üzleti feladatok elvégzéséhez. A rendszergazda a BHOLD igazolási modul által biztosított portál használatával megtervezheti az igazolási folyamat kezelését.

Az igazolási folyamat olyan kampányok keretében zajlik, amelyekben a reklámkampányok lehetőséget kapnak, és azt, hogy a felelős felhasználók megfelelő hozzáféréssel rendelkezzenek a BHOLD által felügyelt alkalmazásokhoz és a megfelelő engedélyekhez Ezek az alkalmazások. A kampány tulajdonosa a kampány felügyeletére van kijelölve, és gondoskodik arról, hogy a kampány megfelelően legyen végrehajtva. Létrehozhat egy kampányt, amely egyszer vagy ismétlődő módon történhet.

A kampányok kezelőjének jellemzően egy felettese lesz, aki tanúsítja az egy vagy több olyan szervezeti egységhez tartozó felhasználók hozzáférési jogosultságait, amelyekhez a vezető felelős. A rendszer automatikusan kiválaszthatja azokat a felhasználókat, akik a felhasználói attribútumok alapján tanúsítják a kampányban, vagy ha a kampányhoz tartozó gondnokok megadhatók olyan fájlban, amely a kampányban tanúsító összes felhasználót leképezi.

Amikor elindul egy kampány, a BHOLD e-mailben értesítést küld a kampány sáfárai és tulajdonosa számára, majd rendszeres emlékeztetőket küld, hogy segítsen nekik fenntartani a kampány előrehaladását. Az stewardok egy kampány portálra vannak irányítva, ahol megtekinthetik azoknak a felhasználóknak a listáját, akik felelősek, valamint az adott felhasználókhoz rendelt szerepköröket. A kezelők megtekinthetik, hogy a felsorolt felhasználók felelősek-e, és jóváhagyják vagy megtagadják a felsorolt felhasználók hozzáférési jogosultságait.

A kampány tulajdonosai a portál használatával is nyomon követik a kampány előrehaladását, és naplózzák a kampány tevékenységeit, így a kampányok tulajdonosai elemezni tudják a kampány során elvégzett műveleteket.

## <a name="analytics"></a>Elemzés

Az átfogó, jogosultságokon alapuló hozzáférés-vezérlési (RBAC) rendszerek megvalósításának egyik fontos szempontja a szigorú hozzáférés-vezérlés fenntartása és a szükségtelen (vagy, rosszabb, váratlan) korlátok közötti egyensúly elérése. Az egyensúly fenntartására irányuló erőfeszítés gyakran olyan hozzáférés-vezérlési struktúrát eredményez, amely annyira összetett, hogy a házirendek közötti váratlan interakciók szinte elkerülhetetlenek.

Ezért fontos, hogy a tényleges üzembe helyezésük előtt elemezze a hozzáférés-vezérlési házirendek hatásait. A BHOLD Suite elemzési modulja lehetővé teszi az elemzés végrehajtását, ha olyan szabályokat fejleszt, amelyek megfelelnek a szabályzatoknak, majd megjeleníti azokat a felhasználókat, akiknek az engedélyei megegyeznek vagy ütköznek a szabállyal. Ezen elemzés alapján módosíthatja a szabályzatot, vagy módosíthatja a szerepköröket és engedélyeket a szabályzattal való ütközések elkerülése érdekében.

A BHOLD Analytics-portál lehetővé teszi olyan szabályrendszerek összeállítását, amelyek egy vagy több olyan szabályból állnak, amelyet egy adott szabályzat vagy csoport tesztelésére hozott létre. A szabály a következő fő részekből áll:

- Cím és leírás, amely lehetővé teszi a szabály azonosítását és ismertetését
- Olyan állapot, amely jelzi, hogy a szabály készen áll-e véleményezésre, felülvizsgálatra vagy jóváhagyásra
- Egy elem (például felhasználók vagy engedélyek), amelyeket a szabály tesztelésre terveztek
- Választható részhalmazi szűrők, amelyek olyan kifejezések, amelyek segítségével kiválaszthatja a vizsgálandó elem megfelelő alcsoportját
- Egy vagy több olyan szabálykészlet, amely a tesztelt szabályzatot jelölő kifejezés.

Egy szabály a következő elemek egyikét tesztelheti:

- Felhasználók
- Szervezeti egységek
- Szerepkörök
- Engedélyek
- Alkalmazások
- Fiókok

Az alábbi ábra egy egyszerű szabályt ábrázol, amely két részhalmazi szabályból és két szűrési szabályból áll:

![](media/bhold-concepts-guide/rules.png)

Figyelje meg, hogy az alkészletek szűrésének és a szabályok szűrésének sikertelensége miatt nem sikerül az alkészletek szűrője, és a rendszer eltávolítja az elem objektumát a további szűrők alapján, míg a szabályok szűrése miatt az objektum nem megfelelőként fog jelenteni. Csak azok az objektumok felelnek meg, amelyek az összes részhalmaz szűrőt és az összes szabály szűrőjét továbbítják.

Mindegyik szűrő egy típusból, egy operátorból (amely típustól függ), egy kulcsból (az egyik elemből) és egy olyan értékből áll, amellyel a kezelő a kulcsot teszteli. Például a következő szűrő azt teszteli, hogy az elem részhalmazában lévő felhasználók száma meghaladja-e a 10 értéket:


|   |   |   |   |   |
|---|---|---|---|---|
|**Írja be:**   | Száma   |
| **Kulcs**  | Felhasználók  |
| **Üzemeltető**  | >  |
| **Érték:** | 10 |

A szabályok szűrői három típusból állnak, és a típusuk alapján meghatározott operátorokat is használhatnak:

- Attribútum
  - < és >
  - = és! =
  - **Tartalmaz**
  - **Nem tartalmazza**
- Száma
  - < és >
  - = és! =
- Korlátozó
  - **Rendelkeznie kell minden**
  - **Nem lehet az összes**
  - **Csak egyetlen, és csak az összesnek lehet**
  - **Kizárólag bármilyen és kizárólag**

> [!Note]
> A korlátozó szűrők használatával a jelzett operátorok több értékkel is ellenőrizhetik a kulcsokat.

Ha például azt szeretné, hogy a feladatok elkülönítésének (SoD) szabályzata egy olyan felhasználó számára, aki nem kér fizetési engedélyt, akkor is jóvá kell hagynia a fizetési engedélyt, a következőhöz hasonló szabályt hozhat létre:

|   |  |
|---|--|
|Név:| Fizetési SoD-teszt|
|Elem| Felhasználók|
|Részhalmaz szűrője:| Engedéllyel kapcsolatos kérelem fizetése|
|Szabály szűrője: | Nem lehet engedélyt jóváhagyni|

A szabály futtatásakor a BHOLD Analytics modul megjeleníti a kiválasztott részhalmazban lévő felhasználók számát (a kéréses fizetési engedéllyel rendelkező felhasználók számát), a szabálynak megfelelő felhasználók számát és azon felhasználók számát, akik nem felelnek meg a szabálynak. Ezután megjelenítheti a nem megfelelő felhasználókat, hogy kijavítsa az inkonzisztenciát.

Az eredmények megjelenítése mellett a jelentést vesszővel tagolt értékként (CSV) vagy XML-fájlként is mentheti, így később is elemezheti az eredményeket. Az eredményül kapott jelentést úgy is testreszabhatja, hogy további információkat jelenítsen meg, amelyek segíthetnek a hatás jobb megismerésében. Ha például a felhasználókat teszteli, megtekintheti (vagy bejelentheti) a megfelelő vagy nem megfelelő felhasználók fiókjait, így láthatja, hogy mely alkalmazások vesznek részt.

Mivel a szabályok több szűrőt is tartalmazhatnak, a szűrők összekapcsolásával ellenőrizheti, hogy léteznek-e bizonyos feltételek. Alapértelmezés szerint az eredmény az összes szűrő egy és logikai tesztének szorzata, de megadható, hogy a szűrő kombinációja vagy tesztelése elvégezhető-e.

Ha például az üzleti szabályzat megköveteli, hogy a vezetők vagy a fizetési engedély módosítása vagy a kifizetés jóváhagyása engedéllyel rendelkezzenek, akkor tesztelheti a szabályzat megfelelőségét egy olyan szabály létrehozásával, amely a következőhöz hasonló:


|  |  |
|--|--|
|Név: | Fizetési SoD-teszt módosítása|
|Elem | Felhasználók |
|Részhalmaz szűrője: | Szerepkör-kezelő|
| Szabály szűrői: |A kifizetés módosítására vonatkozó engedéllyel kell rendelkeznie </br> Jóvá kell hagynia a fizetési engedélyt|

Alapértelmezés szerint minden olyan felhasználó, aki felettes, aki rendelkezik a módosítási és a kéréses fizetési engedéllyel, megfelelőként fog jelenteni. Azonban a szabályzat megköveteli, hogy a kezelőnek legyen engedélye, nem feltétlenül mindkettőre. A szabályzat tényleges megfelelőségének teszteléséhez a (vagy a szabályhoz tartozó) logikai operátort kell használnia annak megállapításához, hogy vannak-e olyan vezetők, akik nem rendelkeznek a fizetési engedély módosításával vagy a fizetési engedély jóváhagyásával.

A többi operátortól eltérően a **kizárólag** a és a **kizárólag az összes** operátor jelzi az olyan objektumok megfelelőségét, amelyeket egy részhalmaz szűrője egyébként kizár. Ha például egy olyan szabályzatot szeretne tesztelni, amelyet az összes kezelő (és csak a kezelők) jóváhagy, a következőképpen hozhat létre egy szabályt:

|  |  |
|--|--|
|Név: | Jóváhagyási teszt áttekintése|
|Elem | Felhasználók|
| Részhalmaz szűrője: | Szerepkör-kezelő
|Szabály szűrője: | Kizárólag engedélyekkel rendelkező engedélyek jóváhagyása|

Ez a szabály olyan megfelelő felhasználókként fog jelentést készíteni, akik felettesek, és rendelkeznek a jóváhagyások jóváhagyása engedéllyel, valamint azokkal a felhasználókkal, akik nem a felettesek, és akik nem rendelkeznek jóváhagyás-felülvizsgálati engedéllyel Ezzel ellentétben azok a vezetők, akik nem rendelkeznek az engedéllyel és a nem kezelő felhasználókkal, de nem megfelelőnek minősülnek.

Amint azt korábban említettük, a szabályokat egy szabályrendszert egyesítheti, így könnyebben kategorizálhatja és kezelheti az üzleti igényeknek megfelelő szabályokat.

Megadhat olyan globális szűrőket is, amelyek engedélyezve vannak, és minden tesztelt szabályra érvényesek. Ha gyakran kell kizárnia a rekordok egy részhalmazát a különböző szabályrendszerek lévő szabályok tesztelésekor, megadhatja azokat a globális szűrőket, amelyeket igény szerint engedélyezhet vagy letilthat.

## <a name="reporting"></a>Jelentéskészítés

Az BHOLD jelentéskészítő modul lehetővé teszi, hogy különböző jelentéseken keresztül megtekinthesse a szerepkör-modell információit. Az BHOLD jelentéskészítő modul a beépített jelentések széles körét biztosítja, valamint egy varázslót is tartalmaz, amely az alapszintű és a speciális egyéni jelentések létrehozásához használható. A jelentések futtatásakor azonnal megjelenítheti az eredményeket, vagy mentheti az eredményeket egy Microsoft Excel (. xlsx) fájlban. Ha a fájlt a Microsoft Excel 2000, a Microsoft Excel 2002 vagy a Microsoft Excel 2003 használatával szeretné megtekinteni, letöltheti és telepítheti a Word-, Excel-és PowerPoint-fájlformátumokhoz készült Microsoft Office kompatibilitási csomagot.


A BHOLD jelentéskészítési modulja elsősorban olyan jelentések előállítására szolgál, amelyek az aktuális szerepkör adatain alapulnak. Az azonosító adatok változásainak naplózására szolgáló jelentések létrehozásához a Forefront Identity Manager 2010 R2 jelentéskészítési képességgel rendelkezik a System Center Service Manager adattárházban megvalósított FIM szolgáltatáshoz. A FIM jelentéskészítéssel kapcsolatos további információkért tekintse meg a Forefront Identity Manager 2010 és a Forefront Identity Manager 2010 R2 dokumentációját a Forefront Identity Management technikai könyvtárában.

A beépített jelentések a következő kategóriákba tartoznak:

- Administration
- Igazolás
- Vezérlők
- Befelé Access Control
- Naplózás
- Modell
- statisztikák
- Munkafolyamat

Jelentéseket hozhat létre, és felveheti ezeket a kategóriákat, vagy megadhatja saját kategóriáit is, amelyekben egyéni és beépített jelentéseket helyezhet el.

A jelentés készítése során a varázsló végigvezeti a következő paraméterek megadásán:

- Azonosítási adatok, beleértve a nevet, a leírást, a kategóriát, a használatot és a célközönséget
- A jelentésben megjelenítendő mezők
- Az elemezni kívánt elemeket megadó lekérdezések
- A sorok rendezésének sorrendje
- A jelentés csoportokba való tördeléséhez használt mezők
- A jelentésben visszaadott elemek pontosítására szolgáló szűrők

A varázsló minden egyes szakaszában előzetesen megtekintheti a jelentést, és mentheti, ha nem kell további paramétereket megadnia. Visszatérhet az előző lépésekhez is, hogy megváltoztassa a varázslóban korábban megadott paramétereket.

## <a name="access-management-connector"></a>Hozzáférés-kezelési összekötő

A BHOLD Suite hozzáférés-vezérlési összekötő modulja az adatok kezdeti és folyamatos szinkronizálását is támogatja a BHOLD-ben. A hozzáférés-kezelési összekötő a FIM synchronization Service segítségével helyezi át az adatátvitelt a BHOLD Core adatbázisba, a felügyeleti csomag metaverse-ba, valamint a cél alkalmazások és az identitások tárolói közé.

A BHOLD korábbi verziói több MAs-t igényeltek az adatáramlás vezérléséhez a BHOLD és a köztes adatbázis-táblák között. A BHOLD Suite SP1-ben a hozzáférés-kezelési összekötő lehetővé teszi, hogy olyan felügyeleti ügynököket (MAs) határozzon meg, amelyek az adatátvitelt közvetlenül a BHOLD és a felügyeleti csomag között biztosítják.

## <a name="mim-integration"></a>Webalkalmazás-integráció

A Forefront Identity Manager 2010 és a Forefront Identity Manager 2010 R2 fontos és hatékony funkciója az önkiszolgáló portál, amely lehetővé teszi a végfelhasználók számára az identitások és a tagsági információk megtekintését és kezelését. A Rendszerfelügyeleti webszolgáltatások integrációja kiterjeszti a webkiszolgálói szerepkör-kezelést. Például a BHOLD funkcióinak használatával a felhasználók igényelhetik a szerepkör-hozzárendelést, és megtekinthetik az aktív szerepköröket és a függőben lévő kéréseket. További funkciók is megadhatók a delegált rendszergazdák számára, például a szerepkör-hozzárendelések más felhasználók számára való kérelmezésének lehetősége.

Fontos megjegyezni, hogy a BHOLD-bővítmények támogatják az önkiszolgáló szerepkört és a munkafolyamatok kezelését, valamint a jelentéskészítést. Az egyéb BHOLD-felügyeleti funkciókat, valamint az igazolásokat a BHOLD-modulok webes portálai biztosítják, amelyek a webhelyeken külön futnak a webszolgáltatások portálján.

## <a name="next-steps"></a>További lépések

- [BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

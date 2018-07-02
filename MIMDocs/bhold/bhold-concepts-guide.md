---
title: Microsoft BHOLD Suite fogalmak útmutató |} Microsoft Docs
description: Első lépések a MIM 2016 összetevői kapcsán – a Synchronization Service telepítése és konfigurálása
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/14/2017
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 521025de3dc16a9bda02aed8287faeb3449192c1
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290067"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Microsoft BHOLD Suite fogalmak útmutató

A Microsoft Identity Manager 2016 (MIM) lehetővé teszi a szervezetek kezelheti a felhasználói identitások teljes életciklusát, és azok tartozó hitelesítő adatokat. Identitások szinkronizálása, központilag kezelheti a tanúsítványokat és a jelszavakat, és konfigurálta a felhasználókat a heterogén rendszerekből beállítható. A MIM informatikai szervezetek határozza meg, és az identitások kezeléséhez való létrehozása a használatból való kivonást használt folyamatok automatizálása.

Microsoft BHOLD Suite ezeket a képességeket a MIM kiterjeszti a szerepköralapú hozzáférés-vezérlés hozzáadásával. BHOLD lehetővé teszi a szervezetek felhasználói szerepkörök definiálása, és a bizalmas adatokhoz és alkalmazásokhoz való hozzáférés szabályozása. A hozzáférés alapul megfelelően ezekhez a szerepkörökhöz. BHOLD Suite magában foglalja a szolgáltatások és eszközök, amelyek egyszerűbbé teszik a modellezési a szerepkör-kapcsolatok a szervezeten belül. BHOLD továbbítja ezeket a szerepköröket a jogosultságokkal, és győződjön meg arról, hogy a szerepkör-definíciók és a társított jogosultságok megfelelően felhasználójára érvényesek. Ezeket a képességeket teljesen integrálva vannak a MIM, zökkenőmentes élményt biztosít a végfelhasználók és informatikai munkatársak egyaránt.

Ez az útmutató segítségével megismerheti, hogyan BHOLD Suite működik együtt a MIM és a következő témaköröket tartalmazza:

- Szerepköralapú hozzáférés-vezérlés
- Állapotigazolási
- Elemzés
- Jelentéskészítés
- Access Management-összekötő
- A MIM-integráció

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés

A leggyakrabban használt alkalmazások és adatok ellenőrző felhasználói hozzáférés módja tulajdonosi hozzáférés-vezérlés (DAC) keresztül. Leggyakoribb implementációk esetén minden jelentős objektumnak azonosított tulajdonost. A tulajdonos képes a engedélyezése vagy megtagadása az egyéni identitás vagy csoporttagság alapján másoknak objektumhoz való hozzáférést. A gyakorlatban DAC általában eredményezi, biztonsági csoportok, néhány tükröző szervezeti struktúra, mások számára, amelyek megfelelnek a működési csoportosításokat (például feladattípusok vagy projekt-hozzárendelések) és mások számára, hogy a felhasználók makeshift gyűjteményei áll formátum és További ideiglenes célokra kapcsolt eszközöket. Szervezet növekedésével ezen csoportok tagságára egyre nehezen kezelhető lesz. Például ha egy alkalmazott átkerül a projektek egy másik, a csoportokat, amelyek a projektek eszközökhöz való hozzáférést szabályozzák, hogy frissíteni kell a manuálisan. Ilyen esetekben nincs ritka, hogy az előforduló hibák, amelyek akadályozhatják a projekt biztonsági vagy termelékenység hibák.

A MIM szolgáltatásokat tartalmaz, amelyek segítenek a probléma automatizált szabályozhatják, és a terjesztési csoport tagságának megadásával. Azonban ez nem oldja meg a proliferáló csoportok, amelyek nem feltétlenül kapcsolódnak egymáshoz strukturált módon belső összetettségét.

Egy jelentősen csökkentheti a elterjedése módja a szerepköralapú hozzáférés-vezérlést (RBAC) telepítésével. Az RBAC nem helyettesítik DAC.  DAC keretet biztosít a felhasználók és informatikai erőforrások zárolásának RBAC épül. Ez lehetővé teszi, hogy explicit kapcsolataiknak és megfelelően e besorolást megfelelő hozzáférési jogosultságokat. Például rendel egy felhasználói attribútum, amely meghatározza a felhasználók beosztás és projekt hozzárendelések, a felhasználói eszközök a felhasználói feladat és a felhasználó hozzájárul, hogy egy adott projekt szükséges adatokat a szükséges hozzáférési engedélyt kapnak. Amikor a felhasználó azt feltételezi, hogy egy másik feladat és a hozzárendelés másik projektet, az attribútumokat, amelyek adja meg a felhasználó beosztás módosítása és projektek automatikusan engedélyezi, hogy a felhasználók előző pozíciónál csak szükséges erőforrásokat.

Mivel szerepkörök is szerepelhetnek hierarchikusan, szerepkörök (például a szerepkörök az értékesítési vezető és értékesítési képviselőink része lehet az értékesítési általánosabb szerepkör), könnyen egyes szerepkörök megfelelő jogosultságokat rendelhet hozzá, és még továbbra is biztosít megfelelő jogosultságokkal minden felhasználója számára osztja meg a szélesebb körű szerepkör. Például a kórháznak, az egészségügyi személyzetnek fordítanak a betegek rekordok megtekintéséhez, de csak orvosok (a orvosi szerepkör alszerepkört) sikerült jogot kell adni a türelmet vonatkozó előírásokat be. Hasonlóképpen a szellemi szerepkörbe tartozó felhasználók sikerült hozzáférése beteg rekordok kivételével számlázási írnokok (egy alszerepkört az irodai szerepkör), akik sikerült hozzáférési engedélyt ad egy, a szolgáltatások türelmet számlázási szükséges betegek rekordok részei: a kórháznak által biztosított.

Az RBAC további előnye, határozza meg, és kényszeríteni (Gyeptéglázás) feladataik elválasztását igénylik. Ez lehetővé teszi a szervezetek kombinációk engedélyek megadásához, amelyek azonos felhasználónak nem kell maradnia, hogy egy adott felhasználó nem rendelhető hozzá, amelyek lehetővé teszik a felhasználó egy fizetési kezdeményezése, és engedélyezik a fizetés, például szerepkörök szerepkörök meghatározásához. Szerepalapú képes automatikusan érvényesíteni házirendjét ahelyett, hogy a házirendet, felhasználónkénti alapon hatékony végrehajtásának kiértékeléséhez nyújt.

### <a name="bhold-role-model-objects"></a>BHOLD szerepkör modellobjektumokat

BHOLD Suite adja meg, és a szerepkörök rendezheti a szervezetben, térkép felhasználók szerepkörökhöz, és a térkép megfelelő engedélyekkel a szerepkörökhöz. Ez a struktúra elnevezése szerepkör-modell, és tartalmaz, és csatlakozik a öt típusú objektumokat: 

- Szervezeti egység
- Users
- Szerepkörök
- Engedélyek
- Alkalmazások

#### <a name="organizational-units"></a>Szervezeti egység

Szervezeti egység (OrgUnits) az egyszerű módszert, amellyel vannak rendszerezve a felhasználók BHOLD szerepkör modell. Minden felhasználó legalább egy OrgUnit kell tartoznia. (Valójában a felhasználó a BHOLD szervezeti egységből eltávolítása után a felhasználói adatok bejegyzés törlődik a BHOLD-adatbázis.)

> [!Important]
> A BHOLD szerepkör modell szervezeti egység nem lehet összetéveszthető szervezeti egységek az Active Directory tartományi szolgáltatások (AD DS). Általában a BHOLD szerkezet alapján a szervezet és a vállalati házirendek nem a hálózati infrastruktúra követelményeinek.

Bár nem kötelező, a legtöbb esetben szervezeti egységek felépítése a tényleges szervezet, hasonló alább hierarchikus képviselő BHOLD:

![](media/bhold-concepts-guide/org-chart.png)

Ez a példa a szerepkör modell volna organizationalganizatinal egységet a vállalat egésze (a elnök képviseli, mert a elnök nem része a mororganizationalganizatinal egység), vagy a BHOLD legfelső szintű szervezeti egységhez (amely mindig létezik) erre a célra használható. A vállalati szervezeti egységet a alelnökök élén vállalati osztályok képviselő OrgUnits kerül. Az értékesítési és marketing igazgatók megfelelő tovább, szervezeti egységek volna hozzá kell adni az értékesítés és marketing szervezeti egységek, és szervezeti egységek, a regionális értékesítési vezetőknek képviselő kerül a szervezeti egységet a a keleti régió értékesítési vezető. Értékesítési társult, akik más felhasználók nem kezelése, a keleti régió értékesítési vezető a szervezeti egység tagjai kerül sor. Vegye figyelembe, hogy a felhasználók bármilyen szintű szervezeti egység tagjai lehetnek. Például egy asszisztens, aki nem a manager és a jelentéseket közvetlenül egy alelnök, lenne a alelnök szervezeti egység tagja.

Mellett a szervezeti felépítés jelző, a felhasználók és más szervezeti egységek működési feltételek szerint csoportosíthatja, projektek vagy futtató és specializációt is szervezeti egységek is használt. A következő ábra bemutatja, hogyan szervezeti egységek ügyfél típusa alapján értékesítési társult csoportot szeretne használni:

![](media/bhold-concepts-guide/org-chart-02.png)

Ebben a példában minden egyes értékesítési társítható két szervezeti egységek volna tartozik: egy jelölő a társítható helyére a szervezeti struktúra, és a társítható ügyfél talál (kereskedelmi vagy vállalati) jelző egy. Szervezeti egységek rendelhetők hozzá másik szerepkörök, amelyek a viszont lehet engedélyeket különböző eléréséhez a szervezet informatikai erőforrásra lenne szükség. Ezenkívül szerepkörök is örökölhetők szülő szervezeti egységek, való propagálása a szerepkörök a felhasználók számára. Másrészről egyes szerepkörök megakadályozható alatt örökölt, amely biztosítja, hogy egy adott szerepkör csak a megfelelő szervezeti egységekhez tartozó.

OrgUnits BHOLD Suite a BHOLD fő webes portálon keresztül vagy a BHOLD modellhez generátor használatával létrehozhatók.

#### <a name="users"></a>Users

A fentiek szerint minden felhasználó legalább egy szervezeti egységhez (OrgUnit) kell tartoznia. Szervezeti egységek találhatók a felhasználói szerepkörök hozzárendeléséről a fő mechanizmusa, a legtöbb szervezet egy adott felhasználó tartozik több OrgUnits könnyebb, hogy a felhasználó szerepköröket hozzárendelni. Néhány esetben azonban elképzelhető kívül bármely OrgUnits, amely a felhasználó tagja a felhasználó egy szerepkörhöz hozzárendelni. Ezért a felhasználó rendelhet közvetlenül egy szerepkör, valamint a szerepkörök nem sikerült beolvasni a OrgUnits, amely a felhasználó tagja a.

Ha a felhasználó nem aktív, a szervezetben (míg számítógépnél orvosi hagyja, például), a felhasználó felfüggeszthető, amely a felhasználói engedélyek visszavonja a felhasználói szerepkör modellből eltávolítása nélkül. Visszatérés vám, akkor aktiválhatók, amely visszaállítja a felhasználói szerepkörök által biztosított minden engedély a felhasználó.

A felhasználók objektumok hozhatók létre külön-külön BHOLD a BHOLD fő webes portálon keresztül, vagy azok importálhatók tömegesen BHOLD modellhez generátor használatával, vagy az Access Management-összekötő segítségével a FIM szinkronizálási szolgáltatás felhasználói adatok importálása információforrásokat Active Directory tartományi szolgáltatások és a HR-alkalmazásként.

Felhasználók hozhatók létre közvetlenül BHOLD a FIM szinkronizálási szolgáltatás használata nélkül. Ez akkor lehet hasznos, ha a modellezési szerepkörök üzem előtti gyűjteményben vagy tesztelési környezetben. Is engedélyezheti a külső felhasználók számára (például az alkalmazottak a alvállalkozó) szerepkört, és így való hozzáférése nagy mennyiségű informatikai erőforrásra nélkül kíván hozzáadni az alkalmazott adatbázis; Ezek a felhasználók azonban nem lesz képes használni a BHOLD önkiszolgáló szolgáltatásokat.

#### <a name="roles"></a>Szerepkörök

Mint korábban feljegyzett, a szerepköralapú hozzáférés-vezérlést (RBAC) modellben engedélyek tartoznak szerepkörök helyett egyéni felhasználók számára. Ez lehetővé teszi minden felhasználó a felhasználói szerepkörök módosítása helyett külön-külön megadásával, vagy a felhasználói engedélyek letiltja a felhasználói feladatok elvégzéséhez szükséges engedélyeket biztosítják. Engedélyek hozzárendelése következtében többé nem kell az informatikai részleg részvételét, de ehelyett is lehet elvégezni, mivel az üzleti felügyeletének részeként. Egy szerepkör különböző rendszerek eléréséhez, vagy közvetlenül révén alszerepkörök további ami csökkenti az informatikai részvétel a felhasználói engedélyek felügyeletéhez engedélyekkel tudja-e összesíteni.

Fontos megjegyezni, hogy szerepkörök az RBAC-modellben; szolgáltatása szerepkörök általában célalkalmazások nem szolgálnak. Ez lehetővé teszi, hogy használható együtt a meglévő alkalmazások nem úgy tervezték, szerepkörök használatával vagy a szerepkör-definíciók kell módosítani az RBAC igényeihez üzleti modellek módosítása nélkül, maguk az alkalmazások módosítása. A célalkalmazás célja, hogy a szerepkörök használatával, majd is megfelelő alkalmazási szerepköröknek a BHOLD szerepkör modell társított szerepkör való az alkalmazás-specifikus szerepköröket, az engedélyek kezelésével.

BHOLD rendelhet egy szerepkört a felhasználó elsősorban két módon:

- Egy szerepkört rendel egy szervezeti egységet (szervezeti egység), amely tagja a felhasználó
- Egy szerepkör közvetlenül rendel egy felhasználói

A szülő szervezeti egység nem kötelezően rendelt szerepkör által a tag szervezeti egységek örökölhető. Ha egy szerepkör van hozzárendelve, vagy szervezeti egység örökölt, akkor jelölhetők hatékony vagy javasolt szerepet. Ha hatékony szerepet, a szervezeti egység összes felhasználó szerepkörrel a. Ha egy javasolt szerepkört, azt aktiválni kell a minden felhasználó vagy a tag szervezeti egység válik, hogy a felhasználó vagy a szervezeti egység tagjai. Ez lehetővé teszi felhasználók hozzárendelése egy részét a szerepköröket egy szervezeti egységhez társított, mint az automatikusan összes szerepkört a szervezeti egység összes tagjára. Ezenkívül szerepkörök kaphatnak a kezdő és záró dátumát, és korlátokat lehet tenni a felhasználók százalékos aránya belül egy szervezeti egységet, amelyhez szerepkör hatékonyak lehetnek.

A következő diagram azt ábrázolja, hogyan egy adott felhasználóhoz rendelhető BHOLD szerepet:

![](media/bhold-concepts-guide/org-chart-flow.png)

Ezen a diagramon A szerepkör örökölhető szerepkörként szervezeti egység hozzá van rendelve, és így öröklik a tag a szervezeti egységek és az adott szervezeti egység összes felhasználóját. B szerepkör hozzá van rendelve egy javasolt szerepkör megkereshet egy szervezeti egységet. Az aktiválni kell, mielőtt a felhasználó a szervezeti egység engedélyezni lehet a szerepkör engedélyekkel. Szerepkör C a rendszer egy hatékony, így az engedélyeket, közvetlenül a szervezeti egység összes felhasználóra vonatkozni. Szerepkör D közvetlenül a felhasználó kapcsolódik, és így az engedélyeket, közvetlenül vonatkozik, hogy a felhasználó.

Emellett a szerepkör akkor lehet aktiválni, egy felhasználó a felhasználói attribútumok alapján. További információkért tekintse meg az attribútum-alapú engedélyezési.

#### <a name="permissions"></a>Engedélyek

BHOLD az engedélyt a célalkalmazás importált engedélyezési felel meg. Ez azt jelenti, hogy BHOLD konfigurálásakor szeretne dolgozni egy alkalmazást kap engedélyt, amely BHOLD szerepkörök listáját. Például az Active Directory tartományi szolgáltatások (AD DS) BHOLD alkalmazásként való felvételekor kap a biztonsági csoportokat, amelyek, BHOLD engedélyekként kapcsolható BHOLD szerepkörök listáját.

Alkalmazások engedélyek vonatkoznak. BHOLD jeleníti meg egyetlen, egységes engedélyek, engedélyek társítható szerepkörök anélkül, hogy a szerepkör kezelők megértéséhez megvalósítási engedélyeit. A gyakorlatban különböző rendszerek lehet, hogy kényszerítése engedély másképp. Az alkalmazás-specifikus összekötő a FIM szinkronizálási szolgáltatás az alkalmazáshoz határozza meg, hogyan engedély módosításokat egy felhasználó számára biztosított alkalmazásra. 

#### <a name="applications"></a>Alkalmazások

BHOLD szerepköralapú hozzáférés-vezérlést (RBAC) alkalmazásának külső alkalmazások metódus valósítja meg. Ez azt jelenti, hogy ha BHOLD ki van építve felhasználókat és engedélyeket az adott alkalmazásból, BHOLD társíthatja ezeket az engedélyeket felhasználók szerepkörök hozzárendelése a felhasználókhoz, és az engedélyek majd összekapcsolása a szerepköröket. Az alkalmazás háttérfolyamatként leképezheti a megfelelő engedélyek a felhasználóknak a szerepkör vagy engedély leképezését BHOLD alapján.

### <a name="developing-the-bhold-suite-role-model"></a>A BHOLD Suite szerepkör modell fejlesztése

Segítségre van szüksége a szerepkör modellezése, BHOLD Suite modellhez generátor, olyan eszköz, amely könnyen használható és átfogó is kínál.

Modellhez generátor használata előtt létre kell hoznia a fájlokat, amelyek meghatározzák a szerepkör modell összeállításához használt modellhez generátor objektumok sorozata. Ezek a fájlok létrehozásával kapcsolatos további információkért lásd: Microsoft BHOLD Suite műszaki útmutatója.

Az első lépés a BHOLD modellhez generátor használatával, hogy importálja ezeket a fájlokat, az alapvető építőelemeket, amelyekből betölthető modellhez generátor. Ha a fájlok sikeresen lett betöltve, ezután megadhatja a modellhez generátor használó szerepkörök több osztályokat hozhatnak létre feltétel:

- A felhasználóhoz a felhasználóhoz tartozó OrgUnits (szervezeti egységek) alapján rendelt tagsági szerepkörök
- BHOLD-adatbázis a felhasználói attribútumok alapján felhasználóhoz rendelt attribútum szerepkörhöz
- Javasolt szerepköröket, amelyek egy szervezeti egységhez kapcsolódik, de bizonyos felhasználók részére aktiválni kell
- Adja meg egy szervezeti egység és a szerepkörök, amelynek tulajdonosa nincs megadva az importált fájlok a felhasználói vezérlése tulajdonjoga szerepkörök

> [!Important]
> Fájlok feltöltése, válassza ki a **meglévő modell megőrzése** jelölőnégyzetet csak tesztelési környezetben. Éles környezetben modellhez generátor kezdeti szerepkör modell létrehozásához kell használnia. Nem használható egy meglévő szerepkör modellt a BHOLD-adatbázis módosításához.

Miután modellhez generátor ezeket a szerepköröket a szerepkör modell hoz létre, majd exportálhatja a szerepkör modell XML-fájl formájában BHOLD-adatbázis.

### <a name="advanced-bhold-features"></a>A speciális BHOLD-funkciók

Korábbi szakaszokban BHOLD szerepköralapú hozzáférés-vezérlés (RBAC) az alapvető elemeket ismerteti. Ez a szakasz ismerteti, amely képes biztosítani a fokozott biztonságot és a szervezet végrehajtásának RBAC rugalmasan BHOLD szolgáltatások. Ez a szakasz áttekintést ad azokról a következő BHOLD-szolgáltatások:

- Kardinalitása
- Feladataik elválasztását igénylik
- A környezetben alkalmazkodó engedélyek
- Attribútum-alapú engedélyezési
- Rugalmas attribútum típusát


#### <a name="cardinality"></a>Kardinalitása

*Számossága* üzleti szabályok, amelyek korlátozzák a szám, ahányszor két olyan entitásra egymással kapcsolódhat végrehajtásának hivatkozik. BHOLD, esetén számossága szabályok a szerepkörök, az engedélyek és a felhasználók hozható létre.

Egy szerepkör korlátozása a következő konfigurálhatja:

- A javasolt szerepkör aktiválása, amelynek felhasználók maximális száma
- A szerepkörhöz rendelt alszerepkörök maximális száma
- A szerepkörhöz rendelt engedélyek maximális száma

Korlátozása a következő engedély is konfigurálhatja:

- Az engedély rendelt szerepkörök maximális száma
- Az engedélyeket a felhasználók maximális száma

Beállíthatja, hogy a felhasználók korlátozása a következő:

- Szerepkörök, amelyek a felhasználó kapcsolható maximális száma
- A felhasználó leállította a szerepkör-hozzárendelések rendelt engedélyek maximális száma

#### <a name="separation-of-duties"></a>Feladataik elválasztását igénylik

(Gyeptégla) feladataik elválasztását igénylik egy üzleti elvet, hogy kapjon a képesség, amely nem lehet egyetlen személy számára elérhető műveletek az egyéni felhasználók számára elkerülésére. Egy alkalmazott például nem lehet lekérni egy fizetési, és engedélyezik a fizetési kell. A Gyeptéglázás elvét lehetővé teszi a szervezetek az ellenőrzések és egyensúlyok alkalmazott hiba vagy kötelességszegés kockázatnak való kitettség csökkentése érdekében a rendszer.

BHOLD Gyeptéglázás azáltal, hogy nem kompatibilis engedélyek definiálhatja valósítja meg. Ha ezek az engedélyek vannak definiálva, BHOLD Gyeptéglázás érvényesíti, amely nem kompatibilis engedélyek van csatolva, kapcsolódnak közvetlenül vagy öröklés útján, és akadályozza meg, hogy a felhasználók nem szerepkört több, hogy a szerepkörök létrehozását, hogy ha kombinált, újra rendelését vagy öröklés útján nem kompatibilis engedélyeket biztosítanak. Másik lehetőségként ütközések felülbírálható lesz.

#### <a name="context-adaptable-permissions"></a>A környezetben alkalmazkodó engedélyek

Hozzon létre az engedélyeket, automatikusan módosítható objektum attribútum alapján, csökkentheti felügyelni kell jogosultságok teljes száma. Környezet alkalmazkodó engedélyek (CAP-ok) lehetővé teszik, hogy adja meg a képlet módosítja, az engedély alkalmazásának a módját az alkalmazás társítva van egy engedély attribútumaként. Például létrehozhat egy úgy, hogy a módosításokat (a biztonsági csoport, a mappa hozzáférés-vezérlési lista társított) fájlmappa hozzáférési engedélyek a alapján, hogy egy felhasználó tartozik egy szervezeti egységet (szervezeti egységhez) tartalmazó teljes munkaidejű alkalmazottak szerződés vagy. Ha a felhasználó egy másikra áthelyeznek egy szervezeti egységet, az új engedély automatikusan alkalmazza, és a régi engedély aktiválva. 

A Tengelysapka képlet lekérdezheti az alkalmazások, az engedélyek, a szervezeti egység és a felhasználók számára alkalmazott attribútum értéke.

#### <a name="attribute-based-authorization"></a>Attribútum-alapú engedélyezési

Egyirányú szabályozhatja, hogy a szerepkör, amely csatolva van egy szervezeti egységhez (szervezeti egység) aktiválva van egy adott felhasználó a szervezeti egység értéke attribútum-alapú engedélyezési (ABA) használatára. ABA használatával automatikusan aktiválhatja a szerepkör csak akkor, ha be van-e bizonyos szabályokat a felhasználói attribútumok alapján. Például egy szerepkör társíthatja egy szervezeti egységhez, amely válik aktívvá, egy felhasználó, csak akkor, ha a felhasználó beosztás megegyezik a beosztás a ABA szabályban. A szükségtelenné paranccsal manuálisan aktiválhatja a javasolt egy felhasználói szerepkört. Ehelyett a szerepkör akkor lehet aktiválni, egy szervezeti egység összes olyan felhasználó számára, amely eleget tesz a szerepkör ABA szabály attribútum értéke. Szabályok egyesíthetők, úgy, hogy a szerepkör csak akkor, ha a felhasználói attribútumok elégíti ki a szerepkörhöz megadott ABA szabályok aktiválva van.

Fontos megjegyezni, hogy a tesztek eredményét az ABA szabály számossága beállítások korlátozza. Például ha egy szabály a számossági beállítást határozza meg, hogy legfeljebb két felhasználókhoz rendelhető szerepkör, és egy ABA szabály egyébként aktiválnia egy szerepkört a négy felhasználó, a szerepkör az csak az első két felhasználó számára, a ABA teszt sikeres aktiválódik.

#### <a name="flexible-attribute-types"></a>Rugalmas attribútum típusát

A rendszer BHOLD attribútumok nem széles körben bővíthető. Meghatározhatja az ilyen objektumok új attribútumtípust felhasználóként, szervezeti egységek (szervezeti egységek) és a szerepkörök. Attribútumok definiálható értékek, amelyek az egész számok, logikai érték (Igen/nem), alfanumerikus, dátum, idő és e-mail címét. Egyetlen érték vagy egy értéklistát attribútumokat adhat meg.

## <a name="attestation"></a>Állapotigazolási

A BHOLD Suite eszközeivel segítségével győződjön meg arról, hogy egyes felhasználók kapott megfelelő engedélyeket az üzleti feladatok elvégzését. A rendszergazda a BHOLD igazolás modul által biztosított a portál segítségével egy kezelése az igazolás folyamat tervezhet.

Az igazolás folyamat mely kampány megbízott lehetőséget kap kampányok segítségével történik, és azt jelenti, amelynek felhasználókat rendelkezik-e megfelelő hozzáférési BHOLD által felügyelt alkalmazások és a megfelelő engedéllyel Ezeket az alkalmazásokat. A kampány tulajdonosa van kijelölve, a kampány felügyeletére, és győződjön meg arról, hogy a kampány végeznek megfelelően. A kampány egyszer vagy ismétlődő módon is létrehozható.

Általában a kampány részeként steward lesz egy funkcióját bizonyítja, hozzáférési jogosultságokat, amelynek a kezelő felelős egy vagy több szervezeti egységekhez tartozó felhasználókat. Megbízott automatikusan választható ki a felhasználók a felhasználói attribútumok alapján kampány igazolt folyamatban, vagy a kampány részeként megbízott definiálni egy fájlban, amely hozzárendeli minden felhasználót egy steward az a kampány igazolt alatt felsorolva.

Kezdődik, amikor a BHOLD e-mail értesítési üzenetet küld a kampány megbízott és a tulajdonos, és ezután elküldi a rendszeres időközönként emlékeztetőket felhasználóinál karbantartása folyamatban van a kampány. Megbízott irányítja a kampány portálra, ahol láthatják, amelynek felhasználókat és az azoknak a felhasználóknak rendelt szerepkörök listáját. A megbízott majd ellenőrizheti, hogy azok a felsorolt felhasználók felelős és jóváhagyására a felsorolt felhasználók mindegyikének hozzáférési jogosultságokat.

Kampány tulajdonosok portált is használhatja a kampány előrehaladásának figyeléséhez, és a kampány tevékenységeket naplózza, a kampány tulajdonosok elemezheti az a kampány során végrehajtott műveleteket.

## <a name="analytics"></a>Elemzés

Egyik fontos szempontja esetén egy átfogó jogok a szerepköralapú hozzáférés-vezérlést (RBAC) rendszer végrehajtási szigorú hozzáférés-vezérlés karbantartásáért, valamint a szükségtelen (vagy, rosszabb, váratlan) elkerülése közötti egyensúly korlátok eléréséhez. Ez gyakran fenn részéről az erőfeszítés hozzáférési adatstruktúrájuk összetett, hogy a házirendek közötti váratlan interakciókat majdnem elkerülhetetlen eredményez.

Ezért fontos lehet képes lesz elemezni a hozzáférési házirendek hatásait, mielőtt ténylegesen vezetnek be. BHOLD Suite Analytics modult ad meg hajthatnak végre az elemzés és így szabályok, amelyek megfelelnek a házirendek fejlesztésére és majd megjeleníti a felhasználók engedélyeiket felelnek meg, vagy ütközik a szabály a. Az elemzés alapján, módosítsa a házirendet, vagy módosítsa a szerepköröket és engedélyeket házirend ütközés elkerülése érdekében.

A BHOLD elemzési portál lehetővé teszi a szabálykészletek, amelyek egy vagy több, egy adott házirend vagy házirendek tesztelésére létrehozott szabályok összeállításához. A szabály a következő fő részből áll:

- Nevét és leírását, amelyek lehetővé teszik azonosításához, és a szabály leírása
- Egy állapotot jelzi, hogy a szabály készen áll a felülvizsgálatra, tekintse át, vagy engedélyezett
- Egy elem be (például a felhasználók vagy engedélyek) célja, hogy a szabály tesztelése
- Válassza ki kell vizsgálni a elem egy megfelelő alcsoport használó kifejezéseket választható részhalmaza szűrők
- Egy vagy több szabály szűrőket, amelyek megfelelnek a tesztelt házirend kifejezések.

Egy szabály tesztelheti a következő elem készletek egyikét sem:

- Users
- Szervezeti egység
- Szerepkörök
- Engedélyek
- Alkalmazások
- Fiókok

Az alábbi ábrán látható, két részhalmaza szabályok és a két Állapotszűrő szabályok álló egyszerű szabályt:

![](media/bhold-concepts-guide/rules.png)

Vegye figyelembe a különbség a hatás hiányában egy részhalmazát szűrő és sikertelen szabály szűrő: egy részhalmazát szűrő hiányában az elemobjektum eltávolítása tesztelése a későbbi szűrők, amíg egy szabály szűrő hiányában a nem megfelelőként jelentett objektumot. Csak azokat az objektumokat, amelyek megfelelnek a részhalmaza szűrőket és a szabály az összes szűrő megfelelőként jelenti.

Minden szűrő egy típust, (amely függő típusa) operátor, a kulcs (egyik eleme) és egy érték, amely alapján a kulcs lett tesztelve az operátor által áll. Például a következő szűrő volna a megállapítására, hogy egy elem részhalmazban felhasználók száma meghaladja a 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Írja be:**   | Száma   |
| **Kulcs:**  | Users  |
| **Operátor**  | >  |
| **Érték:** | 10 |

A szabályok szűrők három típusa létezik, és használja a típusra, adott operátorok jelöli:

- Attribútum
  - < és >
  - = és! =
  - **tartalmazza**
  - **Nem tartalmaz**
- Száma
  - < és >
  - = és! =
- Korlátozó
  - **Bármely és kell rendelkeznie az összes**
  - **Nem rendelkezik ilyennel, és nem rendelkeznek**
  - **Csak rendelkezik ilyennel és egyszerre csak az összes**
  - **Kizárólag rendelkezik ilyennel, és kizárólag rendelkeznek**

> [!Note]
> Korlátozó szűrők a jelzett operátorok segítségével tesztelheti egy kulcs összehasonlítja a több értékkel.

Például ha egy elkülönítése arról, hogy nincs támogatási kérelem jogosultsággal rendelkező felhasználó egyben jóváhagyása fizetési engedéllyel kell rendelkeznie azon (Gyeptéglázás) házirend végrehajtásának tesztelése, sikerült hozhat létre egy szabályt a következőhöz hasonló:

|   |  |
|---|--|
|Név:| Fizetési Gyeptéglázás tesztelése|
|Elem:| Users|
|Részhalmaza szűrő:| Ha bármilyen engedéllyel fizetési kérelem|
|A szabály szűrő: | Nem lehet bármilyen engedéllyel jóváhagyás fizetési|

Ez a szabály futtatásakor a BHOLD elemzési modul a kiválasztott részhalmazát (a támogatási kérelem engedéllyel rendelkező felhasználók száma) lévő felhasználók száma, amelyek megfelelnek a szabály a felhasználók számát és a felhasználók, amelyek nem felelnek meg a szabály számát jeleníti meg. Ezután megjelenítheti a nem megfelelő felhasználók úgy javíthatja ki a inkonzisztenciája miatt.

Mellett az eredményeket megjelenítő, is mentheti a jelentés egy vesszővel tagolt (CSV) vagy XML-fájl újabb elemezheti az eredményeket. További hibakeresési adatok megjelenítése, amelyek segítségével jobban megismerheti a hatását, hogy az eredményül kapott jelentés is testreszabhatja. Például ha a felhasználók teszteli, amelyeket is megjelenítése (vagy a jelentés) a fiókokat, hogy eldönthesse, melyik alkalmazások is érintett a megfelelő vagy nem megfelelő felhasználók.

Szabály tartalmazhat több szűrőt, mert szűrők tesztelése csatlakozhatnak-e hogy létezik-e egy adott kombinációja feltételek. Alapértelmezés szerint a termék egy szűrők és a logikai vizsgálat eredménye, de megadhatja, hogy a szűrő kombináció egy OR teszt hajtható végre.

Például ha a vállalati házirend kezelők kell a fizetési módosítása engedélyt vagy a fizetési jóváhagyása engedélyt igényel, majd sikerült tesztelni a házirendnek való megfelelőségének hozhat létre, például a következő szabályt:


|  |  |
|--|--|
|Név: | Fizetési Gyeptéglázás vizsgálat módosítása|
|Elem: | Users |
|Részhalmaza szűrő: | Minden olyan szerepkört Manager rendelkező|
| A szabály szűrők: |Bármilyen módosítás fizetési engedéllyel kell rendelkeznie. </br> A jóváhagyás fizetési engedéllyel kell rendelkeznie|

Alapértelmezés szerint bármely felhasználó, aki egy kezelője, akik a módosítása fizetési és a fizetési kérése engedéllyel is rendelkezik készül megfelelőnek. Azonban a házirend megköveteli, hogy a kezelő mindkét engedélyt, nem feltétlenül is. A házirend tényleges való teszteléséhez kell használnia a vagy logikai operátor a szabály vannak-e bármely vezetők szakértelmét, akik nem rendelkeznek a fizetési módosítása engedéllyel vagy a fizetési jóváhagyása engedéllyel.

Más operátorokkal eltérően a **kizárólag rendelkezik ilyennel** és a **kizárólagos hozzáféréssel rendelkeznek az összes** operátorok jelzi egy részhalmazát szűrő egyébként volna kizárt objektumok megfelelőségét. Például, hogy minden kezelők (és csak kezelők) engedélye jóváhagyása értékelést házirend tesztelése, sikerült hozhat létre egy szabályt az alábbiak szerint:

|  |  |
|--|--|
|Név: | Felülvizsgálat jóváhagyási tesztelése|
|Elem: | Users|
| Részhalmaza szűrő: | Minden olyan szerepkört Manager rendelkező
|A szabály szűrő: | Kizárólag rendelkezik minden engedély jóváhagyása felülvizsgálatra|

Ez a szabály megfelelő felhasználók, akik kezelője jelentés, és a jóváhagyása értékelést engedély és felhasználók, akik nem kezelők, és akiknél nincs telepítve a jóváhagyása értékelést engedéllyel rendelkezik. Ezzel ellentétben a vezetők szakértelmét, akik nem rendelkeznek engedéllyel, és a felhasználókat, akik nem kezelők, de a engedélye jelentésen nem megfelelőként.

Ahogy azt korábban említettük, kombinálható szabályok egy szabálykészletben megkönnyíti a kategorizálását, és kezelheti azokat a szabályokat az üzleti követelményeknek.

Azt is megadhatja, globális készlete szűrők, ha engedélyezve van, a tesztelni kívánt szabályok vonatkoznak. Ha gyakran kell egy adott részhalmazát rekordok kizárása különböző szabálykészletek szabályok tesztelésekor, globális szűrőket, amelyek engedélyezik, vagy tiltsa le a igény szerint is megadhat.

## <a name="reporting"></a>Jelentéskészítés

A BHOLD Jelentéskezelő modul lehetőséget ad a jelentések számos szerepkör modell információk megtekintéséhez. A BHOLD Jelentéskezelő modul biztosít olyan széles körű olyan beépített jelentést, valamint egy alapvető és speciális egyéni jelentések létrehozásához használható varázsló tartalmazza. Amikor jelentést futtat, azonnal az eredmények megjelenítéséhez vagy a-eredményeket menteni egy Microsoft Excel (.xlsx) fájlban. Microsoft Excel 2000, a Microsoft Excel 2002-es vagy a Microsoft Excel 2003 használatával a fájl megtekintéséhez töltse le és telepítse a Microsoft Office kompatibilitási csomagot a Word, Excel és PowerPoint fájlformátumok.


Használhatja a BHOLD Jelentéskezelő modul legtöbbször az aktuális szerepköri információkat alapuló jelentéseket készíthet. Azonosító adatok módosításainak naplózási jelentések jönnek létre, a Forefront Identity Manager 2010 R2 rendelkezik olyan jelentéskészítési képességet a FIM szolgáltatás, amelyet vezettek be a System Center Service Manager adatraktár. További információ a FIM reporting dokumentációjában a Forefront Identity Manager 2010 és a Forefront Identity Manager 2010 R2 a Forefront Identity Management technikai könyvtárban.

A beépített jelentések által kezelt kategóriák közé tartoznak a következők:

- Administration
- Állapotigazolási
- Vezérlők
- Aktív hozzáférés-vezérlés
- Naplózás
- Modell
- Statisztika
- Munkafolyamat

Jelentések létrehozása, és adja őket hozzá ezen kategóriák, vagy megadhatja a saját egyéni és beépített jelentések jelölje kategóriák.

Jelentés létrehozása a varázsló végigvezeti a következő paraméterek biztosítja:

- Azonosító adatok, ideértve a nevét, leírását, kategória, használatának és a célközönség
- A jelentésben szereplő megjelenítendő adatmezők
- Adja meg, mely elemek elemzése lekérdezések
- Sorrendje sorok rendezendő
- A jelentés felosztása szakaszok használandó mezők
- A jelentés a visszaadott elemek szűkítéséhez szűrőket

A varázsló minden fázisban megtekintheti a jelentés azt, amennyiben definiált és mentheti, ha nem kell további paraméterek megadását. Át is helyezheti vissza az előző lépéseket, amelyek korábban a varázslóban megadott paraméterek módosítása.

## <a name="access-management-connector"></a>Access Management-összekötő

A BHOLD Suite Access Management-összekötő modul BHOLD az adatok kezdőbetűje és a folyamatban lévő szinkronizálás támogatja. Az Access Management-összekötő a FIM szinkronizálási szolgáltatás áthelyezni az adatokat a BHOLD Core-adatbázis, a MIM metaverse, és a cél alkalmazások és a identitástárolók között működik.

BHOLD korábbi verzióiban szükség több MAs vezérlő MIM és a köztes BHOLD-adatbázis táblák között. BHOLD Suite SP1 az Access Management-összekötő lehetővé teszi kezelőügynökök (MAs) adja meg a mim szoftverben, amely közvetlenül a BHOLD és a MIM közötti adatátvitelhez.

## <a name="mim-integration"></a>A MIM-integráció

A Forefront Identity Manager 2010 és a Forefront Identity Manager 2010 R2 egyik fontos és hatékony szolgáltatása az önkiszolgáló portál, amely lehetővé teszi a végfelhasználók számára történő megtekintését és kezelését az identitás- és a tagsági információ. A MIM-integráció a MIM-portál önkiszolgáló szerepkör felügyeleti terjeszti ki. Például a MIM-portál a BHOLD szolgáltatások segítségével a felhasználók kérhetnek a szerepkör-hozzárendelés és tekintheti aktív szerepkör és a függőben lévő kérelmek. A meghatalmazott rendszergazdák, szerepkör-hozzárendelések kérelem más felhasználók például további képességeket engedélyezhetők.

Fontos megjegyezni, hogy a MIM-portálra BHOLD-bővítmények támogatják-e a önkiszolgáló szerepkört és a munkafolyamat felügyeleti és jelentéskészítési. Más BHOLD felügyeleti funkciók, valamint a tanúsítvány, a webes portálok a MIM portál külön üzemeltetett BHOLD modulok által biztosított.

## <a name="next-steps"></a>További lépések

- [BHOLD a telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

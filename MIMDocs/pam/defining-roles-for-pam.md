---
title: Rendszerjogosultságú szerepkörök definiálása a PAM számára | Microsoft Docs
description: Határozza meg, mely rendszerjogosultságú szerepköröket kell kezelni, és alakítsa ki mindegyik számára a kezelési házirendet.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 38a9fc174c037e5d7c3ea17b46dcf9f6ea924822
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/31/2018
ms.locfileid: "50380017"
---
# <a name="define-roles-for-privileged-access-management"></a>Szerepkörök definiálása a Privileged Access Management megoldáshoz

A Privileged Access Management megoldással felhasználókat rendelhet a rendszerjogosultságú szerepkörökhöz, amelyeket a felhasználók szükség szerint aktiválhatnak a csak a szükséges időben (just-in-time) működő hozzáféréshez. Ezeket a szerepköröket kézzel kell megadni és létrehozni a megerősített környezetben. Ez a cikk bemutatja, hogyan választhatja ki a PAM használatával felügyelendő szerepköröket, és hogyan adhat meg hozzájuk megfelelő engedélyeket és korlátozásokat.

A rendszerjogosultságú hozzáférés felügyeletének hatókörébe tartozó szerepkörök definiálásának egyszerű megközelítése szerint az összes információt összegyűjtheti egy táblázatban. Állítsa össze a szerepkörök listáját, és az oszlopokban tüntesse fel a cégirányítási követelményeket és az engedélyeket.

A cégirányítási követelmények a meglévő identitási és hozzáférési szabályzatoktól vagy megfelelőségi követelmények változhat. Az egyes szerepköröket azonosító paraméterek a következők lehetnek:

- A szerepkör tulajdonosa.
- A jelölt felhasználók, akik az adott szerepkör
- A hitelesítési, jóváhagyási vagy értesítési vezérlők, amelyek a szerepkör használatához társítva kell lennie.

A szerepkörengedélyek a felügyelt alkalmazásoktól függnek. Ebben a cikkben az Active Directory szerepel példaként, és az engedélyek két kategóriába vannak sorolva:

- Az Active Directory szolgáltatás kezeléséhez szükséges engedélyek (például a replikációs topológia konfigurálása).

- Az Active Directory-ban tárolt adatok (például felhasználók és csoportok) felügyeletéhez szükséges engedélyek.

## <a name="identify-roles"></a>A szerepkörök meghatározása

Kezdje a PAM használatával felügyelni kívánt szerepkörök meghatározásával. A táblázatban minden lehetséges szerepkör külön sorban szerepeljen.

A megfelelő szerepkörök meghatározásához vegye figyelembe a felügyelet hatókörébe eső egyes alkalmazásokat:

- Az alkalmazás a [tier 0, 1. rétegbeli vagy 2. szintű](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)?
- Melyik jogosultságok vannak hatással az alkalmazás titkosítására, integritására vagy rendelkezésre állására?
- Rendelkezik az alkalmazás függőségeit a rendszer más összetevőitől? Ha például rendelkezik az adatbázisok, hálózatkezelés, biztonsági infrastruktúra, virtualizálási vagy üzemeltetési platform függőségek?

Határozza meg, hogyan csoportosíthatók az alkalmazásra vonatkozó szempontok. Egyértelműen körülhatárolt szerepkörökre van szüksége, amelyek megfelelő engedélyeket biztosítanak az általános felügyeleti feladatok elvégzéséhez az alkalmazáson belül.

A szerepköröket mindig úgy tervezze meg, hogy csak a szükséges legkevesebb jogosultságot adja meg hozzájuk. Ez a felhasználók aktuális (vagy tervezett) szervezeti feladatkörein alapulhat, és azokra a jogosultságokra terjedhet ki, amelyek a felhasználó feladatainak végrehajtásához szükségesek. Figyelembe veheti azokat a jogosultságokat is, amelyek megkönnyítik a műveleteket anélkül, hogy kockázat alakulna ki.

A szerepkörök hatókörének meghatározásánál egyéb szempontok is felmerülnek:

- Hány személy dolgozik egy adott szerepkörben? Ha nincs legalább két személy, akkor elképzelhető, hogy túl szűken határozta meg a szerepkört ahhoz, hogy hasznos legyen, vagy egy meghatározott személy feladatait definiálta.

- Hány szerepkör tartozik egy személyhez? A felhasználók tudnak a feladatuknak megfelelő szerepkört választani?

- A felhasználók köre és az alkalmazások felhasználók általi használatának módja kompatibilis a rendszerjogosultságú hozzáférések felügyeletével?

- Elkülöníthető a felügyelet és a naplózás, hogy a felügyeleti szerepkörű felhasználók ne tudják törölni az általuk végzett műveletekről készült naplónyilvántartásokat?

## <a name="establish-role-governance-requirements"></a>A szerepkörökre vonatkozó cégirányítási követelmények meghatározása

A létrehozandó szerepkörök meghatározása után kezdje el kitölteni a táblázatot. Hozzon létre oszlopokat a szervezethez kapcsolódó követelmények számára. A követelmények például a következők lehetnek:

- Ki a szerepkör tulajdonosa, aki felelős lesz a szerepkör további definiálásért, az engedélyek kiválasztásáért, valamint a szerepkör irányítási beállításainak kezeléséért?

- Kik tartoznak a szerepkörhöz (melyik felhasználók), akik végrehajtják a szerepkörhöz kapcsolódó feladatokat vagy tevékenységeket?

- Milyen hozzáférési módszer (ezekkel a következő szakaszban foglalkozunk) lenne megfelelő a szerepkörhöz tartozó felhasználók számára?

- Szükség van a szerepkör tulajdonosa általi manuális jóváhagyásra, amikor egy felhasználó aktiválja a szerepkörét?

- Szükség van értesítésre, amikor egy felhasználó aktiválja a szerepkörét?

- A szerepkör használatának létre kell hoznia riasztást vagy értesítést nyomon követési célokból egy SIEM-rendszerben?

- Szükséges a szerepkört aktiváló felhasználók korlátozása úgy, hogy csak azokra a számítógépekre tudjanak bejelentkezni, amelyeken végre kell hajtaniuk a szerepkörköz tartozó feladatokat, valamint olyan esetben, amikor a gazdagép ellenőrzése elegendő ahhoz, hogy biztosítsa a jogosultságok és a hitelesítő adatok helytelen használatának megakadályozását?

- Szükség van dedikált rendszergazdai munkaállomás biztosítására a szerepkörhöz tartozó felhasználók számára?

- Milyen alkalmazásengedélyek (lásd például alább az Active Directory listáját) vannak társítva ehhez a szerepkörhöz?

## <a name="select-an-access-method"></a>Hozzáférési módszer választása

Több szerepkör lehet a rendszerjogosultságú hozzáférések felügyeleti rendszerében ugyanazokkal az engedélyekkel hozzájuk rendelve. Ez akkor fordulhat elő, ha a felhasználói különböző közösségekre különböző hozzáférés-irányítási követelmények vonatkoznak. Például egy szervezet különböző szabályzatokat alkalmazhat a teljes munkaidős dolgozóira és egy másik vállalat kiszervezett informatikai dolgozóira.

Bizonyos esetekben a felhasználó véglegesen hozzárendelhető egy szerepkörhöz. Ebben az esetben, nem kell kérelem vagy szerepkör-hozzárendelés aktiválása. Példák a tartós hozzárendelés helyzeteire:

- Felügyelt szolgáltatásfiók egy meglévő erdőben.

- A meglévő erdőben, a PAM-on kívül felügyelt hitelesítő adatot a felhasználói fiók. Ez lehet egy "vészhelyzeti" fiók. A vészhelyzeti fiók sikerült szerepkörre van szüksége például a "tartomány / tartományvezérlő karbantartása" például a megbízhatóság és a tartományvezérlő állapotával kapcsolatos problémák megoldásához. Vészhelyzeti fiók, akkor lesz a fizikai védelemmel ellátott jelszóval állandó jelleggel hozzárendelt szerepkör)

- Egy felhasználói fiók a felügyeleti erdőben, amely jelszóval hitelesíti magát. Ez lehet egy állandó, 24 x 7 rendszergazdai engedélyekre van szüksége, és a egy eszközről, amely nem támogatja az erős hitelesítés bejelentkezik felhasználó.

- Felhasználói fiók a felügyeleti erdőben, intelligens kártyával vagy virtuális intelligens kártyával (például egy fiók kapcsolat nélküli intelligens kártyával, amely ritkán végzett karbantartási feladatokhoz szükséges).

A hitelesítő adataik ellopása vagy illetéktelen használata miatt aggódó szervezetek [Az Azure MFA használata az aktiváláshoz](use-azure-mfa-for-activation.md) útmutató alapján végezhetik el a MIM konfigurálását, hogy egy sávon kívüli további ellenőrzést írhassanak elő a szerepkör aktiválása esetére.

## <a name="delegate-active-directory-permissions"></a>Az Active Directory engedélyeinek delegálása

A Windows Server az új tartományok létrehozásakor automatikusan létrehozza az alapértelmezett csoportokat, például a „Tartományi rendszergazdák” csoportot. Ezek a csoportok leegyszerűsítik a kezdeti lépéseket, és alkalmasak lehetnek a kisebb szervezetek számára. A nagyobb vállalatok, illetve a további elkülönítési rendszergazdai jogosultságokat kell üres ezeket a csoportokat és lecserélte azokat a csoportokat, amelyek részletes engedélyeket biztosítanak.

A Tartományi rendszergazdák csoport egyik korlátozása, hogy nem lehetnek külső tartományhoz tartozó tagjai. További korlátozása, hogy három külön funkció számára ad engedélyeket:

- Az Active Directory szolgáltatás kezelése
- Az Active Directoryban tárolt adatok kezelése
- A tartományhoz csatlakoztatott számítógépekre való távoli bejelentkezés engedélyezése

Alapértelmezett csoportok, például a tartományi rendszergazdák, helyett hozzon létre új biztonsági csoportokat, amelyek csak a szükséges engedélyeket biztosítanak. A MIM használatával dinamikusan adjon meg rendszergazdai fiókokat ezeket csoporttagsággal rendelkező majd kell.

### <a name="service-management-permissions"></a>A szolgáltatásfelügyelet engedélyei

A következő táblázat példákat mutat be az engedélyekre, amelyeket meg kell adni a szerepkörökben az AD felügyeletéhez.

| Szerepkör | Leírás |
| ---- | ---- |
| Tartomány/tartományvezérlő karbantartása | Tagság a TARTOMÁNY\Rendszergazdák csoportban, lehetővé teszi, hogy a tartományvezérlő operációs rendszerének hibaelhárítását és. Műveletek, például egy új tartományvezérlő előléptetése egy létező tartományba, abban az erdőben és az AD szerepköreinek delegálását.
|Virtuális tartományvezérlők kezelése | A tartományvezérlő (DC) virtuális gépek (VM) kezelése virtualizálási felügyeleti szoftver használatával. Ez a jogosultság az összes virtuális gép teljes vezérlésével adható meg a felügyeleti eszközben vagy a Szerepköralapú hozzáférés-vezérlés (RBAC) funkcióban. |
| Séma kiterjesztése | A séma kezelése, beleértve az új objektumdefiníciók hozzáadását, a sémaobjektumok engedélyeinek módosítását, valamint a séma objektumtípusokra vonatkozó alapértelmezett engedélyeinek módosítását. |
| Active Directory adatbázisának biztonsági mentése | Biztonsági másolat készítése az Active Directory teljes adatbázisáról, beleértve a tartományvezérlő és a tartomány összes titkos kulcsát. |
| Megbízhatóságok és működési szintek kezelése | Megbízható kapcsolatok létrehozása a külső tartományokhoz és erdőkhöz. |
| Helyek, alhálózatok és a replikáció kezelése | Az Active Directory replikációs topológiájához tartozó objektumok kezelése, beleértve a helyek, az alhálózatok és a helyhivatkozási objektumok módosítását, valamint a replikálási műveletek elindítását. |
| Csoportházirend-objektumok kezelése | Csoportházirend-objektumok létrehozása, törlése és módosítása a tartományban. |
| Zónák kezelése | DNS-zónák és objektumok létrehozása, törlése és módosítása az Active Directory-ban. |
| Nulladik rétegbeli szervezeti egységek módosítása | Nulladik rétegbe tartozó szervezeti egységek és a tárolt objektumok módosítása az Active Directoryban |

### <a name="data-management-permissions"></a>Adatkezelési engedélyek

Az alábbi táblázat példákat engedélyeket kell adni a szerepkörökben felügyelete vagy használata az AD-ben tárolt adatokat.

| Szerepkör | Leírás |
| ---- | ---- |
| Első rétegbeli rendszergazdai szervezeti egység módosítása                 | Első rétegbe tartozó rendszergazdai objektumokat tartalmazó szervezeti egységek módosítása az Active Directory-ban |
| Második rétegbeli rendszergazdai szervezeti objektum módosítása                 | Második rétegbe tartozó rendszergazdai objektumokat tartalmazó szervezeti egységek módosítása az Active Directory-ban |
| Fiókkezelés: létrehozás/törlés/áthelyezés | Általános jogú felhasználói fiókok módosítása.                                      |
| Fiókkezelés: alaphelyzetbe állítás és feloldás       | Jelszavak alaphelyzetbe állítása és fiókok zárolásának feloldása.                                  |
| Biztonsági csoport: létrehozás és módosítás          | Biztonsági csoportok létrehozása és módosítása az Active Directoryban              |
| Biztonsági csoport: törlés                 | Biztonsági csoportok törlése az Active Directory-ban                         |
| Csoportházirend-objektumok kezelése                         | A tartományban/erdőben lévő összes olyan csoportházirend-objektum kezelése, amelynek nincs hatása a nulladik rétegbeli kiszolgálókra             |
| Csatlakozás számítógéphez/helyi rendszergazda                    | Helyi rendszergazdai jogosultságok az összes munkaállomásnak.                               |
| Csatlakozás kiszolgálóhoz/helyi rendszergazda                   | Helyi rendszergazdai jogosultságok az összes kiszolgálónak.                                    |

## <a name="example-role-definitions"></a>A szerepkör-definíciókat bemutató példák

A kiválasztott szerepkör-definíciók a réteg a felügyelt kiszolgálók rétegétől függ. Ez a felügyelt alkalmazásoktól is függ. Alkalmazások, például az Exchange-hez vagy harmadik féltől származó vállalati termékek, mint például az SAP gyakran saját szerepkör-definíciókat tartalmaznak a delegált felügyelethez állapotba kerül.

A következő szakaszok példákat mutatnak be a jellemző vállalati forgatókönyvekre.

### <a name="tier-0---administrative-forest"></a>Nulladik réteg: Felügyeleti erdő

A megerősített környezetben lévő fiókokhoz megfelelő szerepkörök a következők lehetnek:

- Vészhelyzeti hozzáférés a felügyeleti erdőhöz
- „Piros kártyás” rendszergazdák: azok a felhasználók, akik a felügyeleti erdő rendszergazdái
- Azok a felhasználók, akik az éles környezetben működő erdő rendszergazdái
- Azok a felhasználók, akiknek korlátozott felügyeleti jogosultságokat delegáltak az éles környezetben működő erdőben található alkalmazásokhoz

### <a name="tier-0---enterprise-production-forest"></a>Nulladik réteg: Vállalati éles környezetben működő erdő

Nulladik rétegbeli éles környezetben működő erdő fiókjainak és erőforrásainak felügyeletére alkalmas szerepkörök következők lehetnek:

- Vészhelyzeti hozzáférés az éles környezetben működő erdőhöz
- Csoportházirendek rendszergazdái
- DNS-rendszergazdák
- PKI-rendszergazdák
- AD-topológia és -replikáció rendszergazdái
- Nulladik rétegbeli kiszolgálók virtualizációs rendszergazdái
- Tárhelyek rendszergazdái
- Nulladik rétegbeli kiszolgálók kártevők elleni védelmének rendszergazdái
- Nulladik rétegbeli SCCM SCCM-rendszergazdái
- Nulladik rétegbeli SCOM SCOM-rendszergazdái
- Nulladik réteg biztonsági mentési rendszergazdái
- Nulladik rétegbeli gazdagépekhez csatlakozó sávon kívüli és alaplapi felügyeleti vezérlők (KVM vagy Lights-Out felügyelethez) felhasználói

### <a name="tier-1"></a>Első réteg

Az első rétegben található kiszolgálók felügyeletéhez és biztonsági mentéséhez tartozó szerepkörök a következők lehetnek:

- Kiszolgáló karbantartása
- Első rétegbeli kiszolgálók virtualizációs rendszergazdái
- Biztonsági ellenőrzőeszköz fiókja
- Első rétegbeli kiszolgálók kártevők elleni védelmének rendszergazdái
- Első rétegbeli SCCM SCCM-rendszergazdái
- Első rétegbeli SCOM SCOM-rendszergazdái
- Első rétegbeli kiszolgálók biztonsági mentési rendszergazdái
- Első rétegbeli gazdagépekhez csatlakozó sávon kívüli és alaplapi felügyeleti vezérlők (KVM vagy Lights-Out felügyelethez) felhasználói

Az első rétegbeli vállalati alkalmazások kezelésére a következő szerepkörök is alkalmasak lehetnek:

- DHCP-rendszergazdák
- Exchange-rendszergazdák
- Skype Vállalati verzió rendszergazdái
- SharePoint-farmok rendszergazdái
- Felhőalapú szolgáltatások, például a vállalati webhelyek vagy a nyilvános DNS rendszergazdái
- HCM, pénzügyi vagy jogi rendszerek rendszergazdái

### <a name="tier-2"></a>Második réteg

A nem rendszergazda jogosultságú felhasználók és számítógépek felügyeletének szerepkörei a következők lehetnek:

- Fiókrendszergazdák
- Segélyszolgálat
- Biztonsági csoportok rendszergazdái
- Munkaállomások helyszíni támogatása

## <a name="next-steps"></a>További lépések

- [Referenciaanyag az emelt szintű hozzáférés biztonságossá tétele](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Aktiválás az Azure MFA használatával](use-azure-mfa-for-activation.md)

---
title: "Rendszerjogosultságú szerepkörök definiálása a PAM számára | Microsoft Docs"
description: "Határozza meg, mely rendszerjogosultságú szerepköröket kell kezelni, és alakítsa ki mindegyik számára a kezelési házirendet."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cfd7c5bee0038740db0ad526072ec248ed9f221d
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/01/2017
---
# <a name="define-roles-for-privileged-access-management"></a>Szerepkörök definiálása a Privileged Access Management megoldáshoz

A Privileged Access Management megoldással felhasználókat rendelhet a rendszerjogosultságú szerepkörökhöz, amelyeket a felhasználók szükség szerint aktiválhatnak a csak a szükséges időben (just-in-time) működő hozzáféréshez. Ezeket a szerepköröket kézzel kell megadni és létrehozni a megerősített környezetben. Ez a cikk bemutatja, hogyan választhatja ki a PAM használatával felügyelendő szerepköröket, és hogyan adhat meg hozzájuk megfelelő engedélyeket és korlátozásokat.

A rendszerjogosultságú hozzáférés felügyeletének hatókörébe tartozó szerepkörök definiálásának egyszerű megközelítése szerint az összes információt összegyűjtheti egy táblázatban. Állítsa össze a szerepkörök listáját, és az oszlopokban tüntesse fel a cégirányítási követelményeket és az engedélyeket.

A cégirányítási követelmények a meglévő identitás és hozzáférési szabályzatoktól vagy megfelelőségi követelménynek függenek. Az egyes szerepköröket azonosító paraméterek a következők lehetnek:

- A szerepkör tulajdonosa.
- Lehet, hogy a kijelölt felhasználók
- A szerepkör használatához társítandó hitelesítési, jóváhagyási vagy értesítési vezérlők.

A szerepkörengedélyek a felügyelt alkalmazásoktól függnek. Ebben a cikkben az Active Directory szerepel példaként, és az engedélyek két kategóriába vannak sorolva:

- Az Active Directory szolgáltatás kezeléséhez szükséges engedélyek (például a replikációs topológia konfigurálása).

- Az Active Directory-ban tárolt adatok (például felhasználók és csoportok) felügyeletéhez szükséges engedélyek.

## <a name="identify-roles"></a>A szerepkörök meghatározása

Kezdje a PAM használatával felügyelni kívánt szerepkörök meghatározásával. A táblázatban minden lehetséges szerepkör külön sorban szerepeljen.

A megfelelő szerepkörök meghatározásához vegye figyelembe a felügyelet hatókörébe eső egyes alkalmazásokat:

- Az alkalmazás a [réteg 0, 1. rétegbe vagy 2. szintű](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)?
- Melyik jogosultságok vannak hatással az alkalmazás titkosítására, integritására vagy rendelkezésre állására?
- Az alkalmazás rendelkezik függőségek a rendszer más összetevőitől? Például adatbázisok, hálózati, biztonsági infrastruktúra, virtualizálási vagy platform üzemeltető függőségekkel rendelkezik?

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

A rendszerjogosultságú hozzáférések felügyeleti rendszerében a hozzájuk rendelt ugyanazokkal az engedélyekkel több szerepkör lehet. Ez akkor fordulhat elő, ha a felhasználói különböző közösségekre különböző hozzáférés-irányítási követelmények vonatkoznak. Például egy szervezet különböző szabályzatokat alkalmazhat a teljes munkaidős dolgozóira és egy másik vállalat kiszervezett informatikai dolgozóira.

Néhány esetben a felhasználó véglegesen rendelt szerepkör. Ebben az esetben nincs szükségük vagy szerepkör-hozzárendelés aktiválása. Példák a tartós hozzárendelés helyzeteire:

- Felügyelt szolgáltatásfiók egy meglévő erdőben.

- Egy felhasználói fiókot a meglévő erdőben, a PAM-on kívül felügyelt hitelesítő adattal. Ennek oka lehet egy "vészhelyzeti" fiók. A vészhelyzeti fiók sikerült szerepkörre van szüksége, mint "tartomány / tartomány karbantartása" problémák, például a megbízhatóság és a tartományvezérlő állapotával kapcsolatos problémák megoldásához. Vészhelyzeti fiók, akkor lesz fizikai védelemmel ellátott jelszóval véglegesen rendelt szerepkör)

- Egy felhasználói fiókot az a felügyeleti erdőben, amely jelszóval hitelesíti magát. Ennek oka lehet, olyan felhasználó, aki 24 x 7 állandó rendszergazdai engedélyekkel kell, és egy eszközről, amely nem támogatja az erős hitelesítés bejelentkezik.

- Felhasználói fiók a felügyeleti erdőben, intelligens kártyával vagy virtuális intelligens kártyával (például egy fiók kapcsolat nélküli intelligens kártyával, amely ritkán végzett karbantartási feladatokhoz szükséges).

A hitelesítő adataik ellopása vagy illetéktelen használata miatt aggódó szervezetek [Az Azure MFA használata az aktiváláshoz](use-azure-mfa-for-activation.md) útmutató alapján végezhetik el a MIM konfigurálását, hogy egy sávon kívüli további ellenőrzést írhassanak elő a szerepkör aktiválása esetére.

## <a name="delegate-active-directory-permissions"></a>Az Active Directory engedélyeinek delegálása

A Windows Server az új tartományok létrehozásakor automatikusan létrehozza az alapértelmezett csoportokat, például a „Tartományi rendszergazdák” csoportot. Ezek a csoportok leegyszerűsítik a kezdeti lépéseket, és alkalmasak lehetnek a kisebb szervezetek számára. Nagyobb szervezeteknek, illetve a felügyeleti jogosultságokat jobban elkülöníteni kívánó kell ezeket a csoportokat üres, és lecserélheti azokat a csoportokat, amelyek részletesen meghatározott engedélyeket biztosítanak.

A Tartományi rendszergazdák csoport egyik korlátozása, hogy nem lehetnek külső tartományhoz tartozó tagjai. További korlátozása, hogy három külön funkció számára ad engedélyeket:

- Az Active Directory szolgáltatás kezelése
- Az Active Directoryban tárolt adatok kezelése
- A tartományhoz csatlakoztatott számítógépekre való távoli bejelentkezés engedélyezése

Alapértelmezett csoportok, például a Tartománygazdák, helyett hozzon létre új biztonsági csoportokat, amelyek csak a szükséges engedélyeket biztosítanak. A MIM használatával dinamikusan adjon meg rendszergazdai fiókokat adott csoporttagságok majd kell.

### <a name="service-management-permissions"></a>A szolgáltatásfelügyelet engedélyei

A következő táblázat példákat mutat be az engedélyekre, amelyeket meg kell adni a szerepkörökben az AD felügyeletéhez.

| Szerepkör | Leírás |
| ---- | ---- |
| Tartomány/tartományvezérlő karbantartása | Tagság a TARTOMÁNY\Rendszergazdák csoportban lehetővé teszi, hogy hibaelhárítását és megváltoztatását, a tartományvezérlő operációs rendszerének. Műveletek, például egy új tartományvezérlőt előléptetné egy létező tartományba, abban az erdőben és az AD szerepköreinek delegálását.
|Virtuális tartományvezérlők kezelése | A tartományvezérlő (DC) virtuális gépek (VM) kezelése virtualizálási felügyeleti szoftver használatával. Ez a jogosultság az összes virtuális gép teljes vezérlésével adható meg a felügyeleti eszközben vagy a Szerepköralapú hozzáférés-vezérlés (RBAC) funkcióban. |
| Séma kiterjesztése | A séma kezelése, beleértve az új objektumdefiníciók hozzáadását, a sémaobjektumok engedélyeinek módosítását, valamint a séma objektumtípusokra vonatkozó alapértelmezett engedélyeinek módosítását. |
| Active Directory adatbázisának biztonsági mentése | Biztonsági másolat készítése az Active Directory teljes adatbázisáról, beleértve a tartományvezérlő és a tartomány összes titkos kulcsát. |
| Megbízhatóságok és működési szintek kezelése | Megbízható kapcsolatok létrehozása a külső tartományokhoz és erdőkhöz. |
| Helyek, alhálózatok és a replikáció kezelése | Az Active Directory replikációs topológiájához tartozó objektumok kezelése, beleértve a helyek, az alhálózatok és a helyhivatkozási objektumok módosítását, valamint a replikálási műveletek elindítását. |
| Csoportházirend-objektumok kezelése | Csoportházirend-objektumok létrehozása, törlése és módosítása a tartományban. |
| Zónák kezelése | DNS-zónák és objektumok létrehozása, törlése és módosítása az Active Directory-ban. |
| Nulladik rétegbeli szervezeti egységek módosítása | Nulladik rétegbe tartozó szervezeti egységek és a tárolt objektumok módosítása az Active Directoryban |

### <a name="data-management-permissions"></a>Adatkezelési engedélyek

Az alábbi táblázat példákat engedélyeket kell adni a szerepkörökben az kezeléséhez vagy használatához az AD-ban tárolt adatokat.

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

Szerepkör-definíciók kiválasztása a réteg felügyelt kiszolgálók rétegétől függ. Ez a felügyelt alkalmazásoktól is függ. Alkalmazások, például Exchange vagy harmadik féltől származó vállalati termékek, mint például SAP gyakran megjelenik a saját delegált felügyeletre vonatkozó további szerepkör-definíciók.

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

- [Privileged Access Reference Material biztonságossá tétele](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Aktiválás az Azure MFA használatával](use-azure-mfa-for-activation.md)

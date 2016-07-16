---
title: "Szerepkörök definiálása a Privileged Access Management megoldáshoz | A Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: b8af77d2354428da19d91d5f02b490012835f544
ms.openlocfilehash: 7ba6f744f7fb7a1c5052b14669aa3de2cd10ddbb


---

# Szerepkörök definiálása a Privileged Access Management megoldáshoz

A Privileged Access Management megoldással felhasználókat rendelhet a rendszerjogosultságú szerepkörökhöz, amelyeket a felhasználók szükség szerint aktiválhatnak a csak a szükséges időben (just-in-time) működő hozzáféréshez. Ezeket a szerepköröket kézzel kell definiálni és létrehozni a megerősített környezetben. Ez a cikk bemutatja, hogyan választhatja ki a PAM használatával felügyelendő szerepköröket, és hogyan adhat meg hozzájuk megfelelő engedélyeket és korlátozásokat.

A rendszerjogosultságú hozzáférés felügyeletének hatókörébe tartozó szerepkörök definiálásának egyszerű megközelítése szerint az összes információt összegyűjtheti egy táblázatban. Állítsa össze a szerepkörök listáját, és az oszlopokban tüntesse fel a cégirányítási követelményeket és az engedélyeket.

A cégirányítási követelmények a meglévő identitási és hozzáférési szabályzatoktól vagy a megfelelőségi követelményektől függnek. Az egyes szerepköröket azonosító paraméterek közé tartozhat a szerepkör tulajdonosa, a szerepkörre kijelölt felhasználók, valamint a szerepkör használatához társítandó hitelesítési, jóváhagyási vagy értesítési vezérlők.

A szerepkörengedélyek a felügyelt alkalmazásoktól függnek. Ebben a cikkben az Active Directory szerepel példaként, és az engedélyek két kategóriába vannak sorolva:

- Az Active Directory szolgáltatás kezeléséhez szükséges engedélyek (például a replikációs topológia konfigurálása).

- Az Active Directory-ban tárolt adatok (például felhasználók és csoportok) felügyeletéhez szükséges engedélyek.

## A szerepkörök meghatározása

Kezdje a PAM használatával felügyelni kívánt szerepkörök meghatározásával. A táblázatban minden lehetséges szerepkör külön sorban szerepeljen.

A megfelelő szerepkörök meghatározásához vegye figyelembe a felügyelet hatókörébe eső egyes alkalmazásokat:

- Az alkalmazás a 0. rétegbe, az 1. rétegbe vagy 2. rétegbe tartozik?  
- Melyik jogosultságok vannak hatással az alkalmazás titkosítására, integritására vagy rendelkezésre állására?  
- Függ az alkalmazás a rendszer más összetevőitől, például az adatbázisoktól, a hálózati vagy a biztonsági infrastruktúrától, a virtualizálási vagy az üzemeltetési platformtól?

Határozza meg, hogyan csoportosíthatók az alkalmazásra vonatkozó szempontok. Egyértelműen körülhatárolt szerepkörökre van szüksége, amelyek megfelelő engedélyeket biztosítanak az általános felügyeleti feladatok elvégzéséhez az alkalmazáson belül.

A szerepköröket mindig úgy tervezze meg, hogy csak a szükséges legkevesebb jogosultságot adja meg hozzájuk. Ez a felhasználók aktuális (vagy tervezett) szervezeti feladatkörein alapulhat, és azokra a jogosultságokra terjedhet ki, amelyek a felhasználó feladatainak végrehajtásához szükségesek. Figyelembe veheti azokat a jogosultságokat is, amelyek megkönnyítik a műveleteket anélkül, hogy kockázat alakulna ki.

A szerepkörök hatókörének meghatározásánál egyéb szempontok is felmerülnek:

- Hány személy dolgozik egy adott szerepkörben? Ha nincs legalább két személy, akkor elképzelhető, hogy túl szűken határozta meg a szerepkört ahhoz, hogy hasznos legyen, vagy egy meghatározott személy feladatait definiálta.

- Hány szerepkör tartozik egy személyhez? A felhasználók tudnak a feladatuknak megfelelő szerepkört választani?

- A felhasználók köre és az alkalmazások felhasználók általi használatának módja kompatibilis a rendszerjogosultságú hozzáférések felügyeletével?

- Elkülöníthető a felügyelet és a naplózás, hogy a felügyeleti szerepkörű felhasználók ne tudják törölni az általuk végzett műveletekről készült naplónyilvántartásokat?

## A szerepkörökre vonatkozó cégirányítási követelmények meghatározása

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

## Hozzáférési módszer választása

A rendszerjogosultságú hozzáférések felügyeleti rendszerében lehet több olyan szerepkör is, amelyhez ugyanazok az engedélyek tartoznak, ha a különböző felhasználói közösségekre különböző hozzáférés-irányítási követelmények vonatkoznak. Például egy szervezet különböző szabályzatokat alkalmazhat a teljes munkaidős dolgozóira és egy másik vállalat kiszervezett informatikai dolgozóira.

Bizonyos esetekben előfordulhat, hogy egy felhasználóhoz tartósan hozzá lehet rendelni a szerepkört, így nem kell külön kérnie a szerepkör-hozzárendelést vagy a szerepkör aktiválását. Példák a tartós hozzárendelés helyzeteire:

- Felügyelt szolgáltatásfiók egy meglévő erdőben.

- Felhasználói fiók egy meglévő erdőben, a PAM-on kívül felügyelt hitelesítő adatokkal (például egy „vészhelyzeti” fiók, amelyben a megbízhatóság és a tartományvezérlő állapotával kapcsolatos problémák kijavításához tartósan van hozzárendelve a „Tartomány/Tartomány karbantartása” szerepkör a fiókhoz, amely fizikai védelemmel ellátott jelszóval rendelkezik).

- Felhasználói fiók a felügyeleti erdőben, amely jelszóval hitelesíti magát (például egy felhasználó, akinek tartós felügyeleti engedélyekre van szüksége a hét minden napján, éjjel nappal, és olyan eszközről jelentkezik be, amely nem támogatja az erős hitelesítést).

- Felhasználói fiók a felügyeleti erdőben, intelligens kártyával vagy virtuális intelligens kártyával (például egy fiók kapcsolat nélküli intelligens kártyával, amely ritkán végzett karbantartási feladatokhoz szükséges).

A hitelesítő adataik ellopása vagy illetéktelen használata miatt aggódó szervezetek [Az Azure MFA használata az aktiváláshoz](use-azure-mfa-for-activation.md) útmutató alapján végezhetik el a MIM konfigurálását, hogy egy sávon kívüli további ellenőrzést írhassanak elő a szerepkör aktiválása esetére.

## Az Active Directory engedélyeinek delegálása

A Windows Server az új tartományok létrehozásakor automatikusan létrehozza az alapértelmezett csoportokat, például a „Tartományi rendszergazdák” csoportot. Ezek a csoportok leegyszerűsítik a kezdeti lépéseket, és alkalmasak lehetnek a kisebb szervezetek számára. A nagyobb szervezeteknek, illetve a felügyeleti jogosultságokat jobban elkülöníteni kívánó szervezeteknek azonban ki kell üríteniük az olyan csoportokat, mint a Tartományi rendszergazdák, és olyan csoportokra kell lecserélniük azokat, amelyek részletesen meghatározott engedélyeket biztosítanak.

A Tartományi rendszergazdák csoport egyik korlátozása, hogy nem lehetnek külső tartományhoz tartozó tagjai. További korlátozása, hogy három külön funkció számára ad engedélyeket:  
- Az Active Directory szolgáltatás kezelése  
- Az Active Directoryban tárolt adatok kezelése  
- A tartományhoz csatlakoztatott számítógépekre való távoli bejelentkezés engedélyezése

Alapértelmezett csoportok, például a Tartományi rendszergazdák csoport helyett hozzon létre új biztonsági csoportokat, amelyek csak a szükséges engedélyeket biztosítják, és a MIM használatával dinamikusan adjon meg rendszergazdai fiókokat ezekkel a csoporttagságokkal.

### A szolgáltatásfelügyelet engedélyei

A következő táblázat példákat mutat be az engedélyekre, amelyeket meg kell adni a szerepkörökben az AD felügyeletéhez.

| Szerepkör | Leírás |
| ---- | ---- |
| Tartomány/tartományvezérlő karbantartása | Tagság a Tartomány\Rendszergazdák csoportban, amely lehetővé teszi a tartományvezérlő operációs rendszerének hibaelhárítását és megváltoztatását, az új tartományvezérlők előléptetését az erdőben található meglévő tartományba, valamint az AD szerepköreinek delegálását.
|Virtuális tartományvezérlők kezelése | A tartományvezérlő (DC) virtuális gépek (VM) kezelése virtualizálási felügyeleti szoftver használatával. Ez a jogosultság az összes virtuális gép teljes vezérlésével adható meg a felügyeleti eszközben vagy a Szerepköralapú hozzáférés-vezérlés (RBAC) funkcióban. |
| Séma kiterjesztése | A séma kezelése, beleértve az új objektumdefiníciók hozzáadását, a sémaobjektumok engedélyeinek módosítását, valamint a séma objektumtípusokra vonatkozó alapértelmezett engedélyeinek módosítását. |
| Active Directory adatbázisának biztonsági mentése | Biztonsági másolat készítése az Active Directory teljes adatbázisáról, beleértve a tartományvezérlő és a tartomány összes titkos kulcsát. |
| Megbízhatóságok és működési szintek kezelése | Megbízható kapcsolatok létrehozása a külső tartományokhoz és erdőkhöz. |
| Helyek, alhálózatok és a replikáció kezelése | Az Active Directory replikációs topológiájához tartozó objektumok kezelése, beleértve a helyek, az alhálózatok és a helyhivatkozási objektumok módosítását, valamint a replikálási műveletek elindítását. |
| Csoportházirend-objektumok kezelése | Csoportházirend-objektumok létrehozása, törlése és módosítása a tartományban. |
| Zónák kezelése | DNS-zónák és objektumok létrehozása, törlése és módosítása az Active Directory-ban. |
| 0. rétegbeli szervezeti egységek módosítása | A 0. rétegbe tartozó szervezeti egységek és a tárolt objektumok módosítása az Active Directoryban |

### Adatkezelési engedélyek

A következő táblázat példákat mutat be azokra engedélyekre, amelyeket meg kell adni a szerepkörökben az AD-ban tárolt adatok kezeléséhez vagy használatához.

| Szerepkör | Leírás |
| ---- | ---- |
| 1. rétegbeli rendszergazdai szervezeti egység módosítása                 | 1. rétegbe tartozó rendszergazdai objektumokat tartalmazó szervezeti egységek módosítása az Active Directory-ban. |
| 2. rétegbeli rendszergazdai szervezeti objektum módosítása                 | 2. rétegbe tartozó rendszergazdai objektumokat tartalmazó szervezeti egységek módosítása az Active Directory-ban. |
| Fiókkezelés: létrehozás/törlés/áthelyezés | Általános jogú felhasználói fiókok módosítása.                                      |
| Fiókkezelés: alaphelyzetbe állítás és feloldás       | Jelszavak alaphelyzetbe állítása és fiókok zárolásának feloldása.                                  |
| Biztonsági csoport: létrehozás és módosítás          | Biztonsági csoportok létrehozása és módosítása az Active Directoryban              |
| Biztonsági csoport: törlés                 | Biztonsági csoportok törlése az Active Directory-ban.                         |
| Csoportházirend-objektumok kezelése                         | A tartományban/erdőben lévő összes olyan csoportházirend-objektum kezelése, amelynek nincs hatása a 0. rétegbeli kiszolgálókra.             |
| Csatlakozás számítógéphez/helyi rendszergazda                    | Helyi rendszergazdai jogosultságok az összes munkaállomásnak.                               |
| Csatlakozás kiszolgálóhoz/helyi rendszergazda                   | Helyi rendszergazdai jogosultságok az összes kiszolgálónak.                                    |

## A szerepkör-definíciókat bemutató példák

A szerepkör-definíciók kiválasztása a rendszerjogosultságú fiókokkal felügyelt kiszolgálók rétegétől függ. Ezenkívül a felügyelt alkalmazásoktól is függ, mivel az alkalmazások, például az Exchange vagy a harmadik féltől származó vállalati termékek, mint az SAP, gyakran saját szerepkör-definíciókat tartalmaznak a delegált felügyelethez.

A következő szakaszok példákat mutatnak be a jellemző vállalati forgatókönyvekre.

### 0. réteg: Felügyeleti erdő

A megerősített környezetben lévő fiókokhoz megfelelő szerepkörök a következők lehetnek:

- Vészhelyzeti hozzáférés a felügyeleti erdőhöz
- „Piros kártyás” rendszergazdák: azok a felhasználók, akik a felügyeleti erdő rendszergazdái
- Azok a felhasználók, akik az éles környezetben működő erdő rendszergazdái
- Azok a felhasználók, akiknek korlátozott felügyeleti jogosultságokat delegáltak az éles környezetben működő erdőben található alkalmazásokhoz

### 0. réteg: Vállalati éles környezetben működő erdő

A 0. rétegbeli éles környezetben működő erdő fiókjainak és erőforrásainak felügyeletére alkalmas szerepkörök következők lehetnek:

- Vészhelyzeti hozzáférés az éles környezetben működő erdőhöz
- Csoportházirendek rendszergazdái
- DNS-rendszergazdák
- PKI-rendszergazdák
- AD-topológia és -replikáció rendszergazdái
- 0. rétegbeli kiszolgálók virtualizációs rendszergazdái
- Tárhelyek rendszergazdái
- 0. rétegbeli kiszolgálók kártevők elleni védelmének rendszergazdái
- 0. rétegbeli SCCM SCCM-rendszergazdái
- 0. rétegbeli SCOM SCOM-rendszergazdái
- 0. réteg biztonsági mentési rendszergazdái
- 0. rétegbeli gazdagépekhez csatlakozó sávon kívüli és alaplapi felügyeleti vezérlők (KVM vagy Lights-Out felügyelethez) felhasználói

### 1. réteg

Az 1. rétegben található kiszolgálók felügyeletéhez és biztonsági mentéséhez tartozó szerepkörök a következők lehetnek:

- Kiszolgáló karbantartása
- 1. rétegbeli kiszolgálók virtualizációs rendszergazdái
- Biztonsági ellenőrzőeszköz fiókja
- 1. rétegbeli kiszolgálók kártevők elleni védelmének rendszergazdái
- 1. rétegbeli SCCM SCCM-rendszergazdái
- 1. rétegbeli SCOM SCOM-rendszergazdái
- 1. rétegbeli kiszolgálók biztonsági mentési rendszergazdái
- 1. rétegbeli gazdagépekhez csatlakozó sávon kívüli és alaplapi felügyeleti vezérlők (KVM vagy Lights-Out felügyelethez) felhasználói

Az 1. rétegbeli vállalati alkalmazások kezelésére a következő szerepkörök is alkalmasak lehetnek:

- DHCP-rendszergazdák
- Exchange-rendszergazdák
- Skype Vállalati verzió rendszergazdái
- SharePoint-farmok rendszergazdái
- Felhőalapú szolgáltatások, például a vállalati webhelyek vagy a nyilvános DNS rendszergazdái
- HCM, pénzügyi vagy jogi rendszerek rendszergazdái

### 2. réteg

A nem rendszergazda jogosultságú felhasználók és számítógépek felügyeletének szerepkörei a következők lehetnek:

- Fiókrendszergazdák
- Segélyszolgálat
- Biztonsági csoportok rendszergazdái
- Munkaállomások helyszíni támogatása



<!--HONumber=Jun16_HO3-->



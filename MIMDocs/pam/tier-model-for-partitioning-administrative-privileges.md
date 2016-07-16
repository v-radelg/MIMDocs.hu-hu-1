---
title: "A rendszergazdai jogosultságok felosztásának rétegmodellje | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 509c05bbda5f0a0b936518fb023000771c45d4f7


---

# A rendszergazdai jogosultságok felosztásának rétegmodellje

Napjaink fenyegetésekkel teli világában nem az a kérdés, hogy egy támadó hozzáfér-e a rendszeréhez, hanem az, hogy mikor. Ez azt jelenti, hogy a belső biztonság éppen olyan fontos, mint az erős külső védelem. Ebben a cikkben egy olyan biztonsági modellel ismerkedhet meg, amely a jogok kiterjesztéséből adódó támadásokkal szembeni védelmet a magas szintű jogosultságokat igénylő tevékenységek és a nagy kockázatú zónák elkülönítésével biztosítja. Ez a modell úgy tartja fenn a jó felhasználói élményt, hogy közben alkalmazza az ajánlott eljárásokat és biztonsági alapelveket.

## A jogok kiterjesztése az Active Directory-erdőkben

A Windows Server Active Directory- (AD-) erdőkhöz állandó rendszergazdai jogosultságokkal rendelkező felhasználók, szolgáltatások és alkalmazások fiókjai komoly kockázatot jelentenek a szervezet céljaira és üzleti tevékenységeire nézve. Ezek a fiókok gyakran válnak a támadók célpontjaivá, mert ha sérül a biztonságuk, a támadó jogosultságot szerez a tartomány más kiszolgálóihoz vagy alkalmazásaihoz való kapcsolódáshoz.

A rétegmodell annak alapján választja szét egymástól a rendszergazdákat, hogy milyen erőforrásokat kezelnek. A felhasználói munkaállomások felett irányítással rendelkező rendszergazdák elkülönülnek azoktól, akik alkalmazásokat vagy vállalati identitásokat kezelnek. A modellel kapcsolatos további tudnivalók a [Securing privileged access reference material](http://aka.ms/tiermodel) (A rendszerjogosultságú hozzáférés biztonságossá tételének referenciaanyagai) című témakörben olvashatók.

## Hitelesítő adatok veszélyeztetettségének korlátozása bejelentkezési korlátozásokkal

A rendszergazdai fiókokhoz tartozó hitelesítő adatok ellopásának kockázata általában a rendszergazdák munkamódszereinek átalakításával csökkenthető, ezáltal korlátozható a támadóknak való kiszolgáltatottság. Első lépésként az alábbi intézkedéseket javasoljuk a szervezeteknek:

- Korlátozzák azoknak a gépeknek a számát, amelyeken elérhetők a rendszergazdai hitelesítő adatok.
- Korlátozzák a szerepkörök jogosultságait a lehető legkevesebbre.
- Semmiképpen ne végezzenek rendszergazdai feladatokat olyan gazdagépeken, amelyeket hagyományos felhasználói műveletekre (például levelezésre vagy böngészésre) használnak.

A következő lépés a bejelentkezési korlátozások megvalósítása, illetve a rétegmodell követelményeit kielégítő folyamatok és műveletek feltételeinek megteremtése. Ideális esetben a hitelesítő adatokhoz való hozzáférést is le kell csökkenteni az egyes rétegeken belül az adott szerepkörhöz szükséges legalacsonyabb jogosultsági szintre.

Bejelentkezési korlátozásokat kell érvényesíteni annak érdekében, hogy a kiemelt jogosultságokkal rendelkező fiókok ne férjenek hozzá a kevésbé biztonságos erőforrásokhoz. Példa:

- A tartományi rendszergazdák (0. szint) nem tudnak bejelentkezni a vállalati kiszolgálókra (1. szint) és a normál felhasználói munkaállomásokra (2. szint).
- A kiszolgálói rendszergazdák (1. szint) nem tudnak bejelentkezni a normál felhasználói munkaállomásokra (2. szint).

>[!NOTE] 
> A kiszolgálói rendszergazdák ne tartozzanak a tartományi rendszergazdák csoportjához. A tartományvezérlőkért és a vállalati kiszolgálókért felelő személyzetnek külön fiókokat kell biztosítani.

A bejelentkezési korlátozásokat a következőkkel lehet érvényesíteni:

- Csoportházirendek bejelentkezési jogmegadásának korlátozása, beleértve:  
    - A számítógép hálózati elérésének megtagadása  
    - Kötegelt feladatként való bejelentkezés megtagadása  
    - Szolgáltatásként való bejelentkezés megtagadása  
    - Helyi bejelentkezés megtagadása  
    - Távoli asztali beállításokkal való bejelentkezés megtagadása  
- Hitelesítési házirendek és silók alkalmazása Windows Server 2012 vagy újabb rendszer használata esetén
- Szelektív hitelesítés használata, ha a fiók dedikált rendszergazdai erdőben található

A következő, a [Megerősített környezet tervezése](planning-bastion-environment.md) című cikkben a Microsoft Identity Manager rendszergazdai fiókjainak létrehozásához szükséges dedikált felügyeleti erdőinek létrehozásáról olvashat.



<!--HONumber=Jun16_HO5-->



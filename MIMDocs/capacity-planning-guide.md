---
title: Kapacitástervezési útmutató | Microsoft Docs
description: Az útmutató azokat a tényezőket ismerteti, amelyeket célszerű figyelembe venni a MIM 2016 üzembe helyezése előtt – ilyenek például a terhelésszintek és a szabályozási döntések.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 32cdf03ffa0d0d282a6277af766f97e93e3a3f3a
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289013"
---
# <a name="capacity-planning-guide"></a>Kapacitástervezési útmutató

A Microsoft Identity Manager (MIM) segítségével létrehozhatja, frissítheti és eltávolíthatja a szervezetében lévő felhasználói fiókokat. Emellett a segítségével a végfelhasználók kezelhetik a saját fiókjuk önkiszolgáló szolgáltatásait. Ezek a műveletek még egy kis környezetben is gyorsan összeadódhatnak.

A MIM használatának megkezdése előtt a jelen útmutató és a tesztkörnyezet segítségével ismerje meg az üzembe helyezés megfelelő hatókörét. Ez a cikk számos olyan gyakori tényezővel foglalkozik, amelyet érdemes számításba venni. Ugyanakkor mivel minden üzembe helyezés egyedi, továbbra is az esetek laboratóriumi tesztelése a legjobb módszer az igényeinek megfelelő kiszolgálók, hardverek vagy topológiák meghatározására.

Ha még nem ismeri a MIM 2016 szoftvert és összetevőit, a folytatás előtt ismerje meg közelebbről a [Microsoft Identity Manager 2016-ot](microsoft-identity-manager-2016.md).

## <a name="overview"></a>Áttekintés

Számos általános kapacitását és a Microsoft Identity Manager-környezet teljesítményét befolyásoló tényezők:

- Az üzembe helyezésének módja, fizikailag a MIM-összetevők (topológia).
- A hardver, amelyen ezek az összetevők futni.
- A száma és összetettsége a MIM házirend-konfigurációs objektumai jelentős tényezőt jelent a kapacitástervezés során figyelembe kell venni.
- A központi telepítés és a tervezett terhelés várható mérete jellemzően viszonylag kézenfekvő tényezőt jelent a teljesítmény és kapacitás szempontjából is.

A következő táblázat a fő kapacitás és a MIM 2016-környezet teljesítményét befolyásoló tényezők ismertetnek:

| Tervezési tényező | Szempontok |
| ------------- | -------------- |
| Topológia | A MIM-szolgáltatások eloszlása a hálózaton található számítógépek között. |
| Hardver | A fizikai hardver (fizikai vagy virtuális) MIM egyes összetevőihez, beleértve a CPU, a memória, a hálózati adapter és a merevlemez konfigurációja. |
| A MIM házirend-konfigurációs objektumai | A MIM házirend-konfigurációs objektumainak száma és típusa – ide tartoznak a halmazok, a felügyeleti házirend-szabályok és a munkafolyamatok. |
| Méretezés | A felhasználók, csoportok, számított csoportok és egyéni objektumtípusok a MIM 2016 által kezelt. Vegye figyelembe a dinamikus csoportok összetettségét is, és vegye ezt számításba a csoportok beágyazásakor. |
| Terhelés | A használat gyakorisága, Műveletek, például új csoport vagy felhasználó létrehozása, a jelszó alaphelyzetbe állítása, vagy a portál meglátogat / perc vagy óra. Vegye figyelembe, hogy a terhelés egy adott óra, nap, hét vagy év alatt változhat. Az összetevőtől függően tervezhet a csúcsterheléssel vagy az átlagos terheléssel is. |

## <a name="hosting-microsoft-identity-manager-components"></a>A Microsoft Identity Manager összetevőinek üzemeltetése

A Microsoft Identity Manager összetevőinek nem kell ugyanazon a számítógépen lenniük. Ezen összetevők, valamint a futtatásukra szolgáló fizikai vagy virtuális gépek számbavétele a kapacitástervezés fontos mozzanata.

A hardveres tényezők befolyásolhatják a MIM-környezet teljesítményét. Például:

- Milyen fizikai lemezkonfigurációt használ a MIM 2016-szolgáltatás SQL-adatbázisát futtató számítógép? A lemezkonfigurációt alkotó forgórészek száma, illetve a napló- és adatfájlok elosztása jelentős mértékben befolyásolhatja a rendszer teljesítményét.

Mindezek mellett gondolja át a konfigurációt érintő külső tényezőket is. Például:

- Ha tárolóhálózatot használ a MIM 2016-szolgáltatás adatbázis-konfigurációjához: milyen egyéb alkalmazások használják még a tárolóhálózatot? Ha ezek az alkalmazások „versenyeznek” a tárolóhálózat megosztott lemezerőforrásaiért, az hatással lehet az adatbázis teljesítményére.

## <a name="users-and-groups"></a>Felhasználók és csoportok

A környezetben található felhasználók és csoportok száma jellemzően fontos szempont a rendszer méretezésének tervezésekor. A tervezéskor ugyanakkor számos egyéb kapcsolódó szempontot célszerű mérlegelnie.

- Létrehozhatnak-e a felhasználók csoportokat? Ha igen, érdemes felmérni, hogy a felhasználók által létrehozott új csoportok milyen hatással lesznek a csoportok növekedésére a környezetben.

- Várható-e dinamikus csoportok létrehozása? Mérje fel, hogy hány és milyen típusú dinamikus csoport létrehozása várható a környezetben.

## <a name="expected-load-levels"></a>Várható terhelési szintek

Vegye számításba a MIM-összetevők várható terhelésének típusát is. Szüksége lesz a terhelés becsléséhez a környezetben jelenleg futó alkalmazások felmérésével. Célszerű megfontolni többek között a következő kérdéseket:

- Milyen gyakran várhatók a csoporthoz való csatlakozásra és a csoport elhagyására irányuló kérések?

- Milyen gyakran várható, hogy valamelyik felhasználó statikus vagy dinamikus csoportot hoz létre?

- Hány nem felhasználói művelet várható? Ilyen lehet például a változások szinkronizálása külső rendszerekből. Fontos számításba venni az identitásadatok külső rendszerekkel való szinkronizálása által előidézett terhelést is.

- Milyen jellegű alkalmazási helyzetekben tervezi használni a rendszert? Különböző alkalmazási helyzetek eltérő terhelési mintázatokkal függ. A MIM 2016-ügyféllel rendszeresen rendelkező ügyfélszámítógépek például ellenőrzik, hogy e regisztráció a bejelentkezéshez szükséges.

- Várhatók-e nagy változások a terhelési szintben a normál és a csúcsterhelés között? Például hogy általában sok jelszó-átállításra szünnap időszakot követően. Ügyeljen arra, hogy a rendszer karbantartási és szinkronizálási ütemezése elkerülje a várható csúcsterheléseket. A kapacitástervezés során mindenképpen vegye figyelembe a csúcsterhelési időszakokat.

## <a name="policy-configuration-objects"></a>Házirend-konfigurációs objektumok

A MIM házirend-konfigurációs objektumai a házirendszabályok, a beállítása, a munkafolyamatok és a központi telepítés szinkronizálási szabályait tartalmazza. A MIM-példány minden ügyfélnél egyedi, hiszen a házirend-konfiguráció az adott környezet igényeinek megfelelően változik. Legfontosabb teljesítményi szempontok közé tartozik a következő MIM házirend-konfigurációs objektumai:

- **Halmazok** A rendszerben minden műveletet a meglévő halmaztagságok és az ezekben változást előidéző tényezők fényében kell értékelni. Például egy adott iroda épületszámának épület számának módosítása nem túl nagy hatással lehet. Más változásoknak azonban fokozatos közvetett hatásai lehetnek: egy vezetőváltás például több szinten több objektumra is hatással lehet.

- **Felügyeleti házirendszabályok** A felügyeleti házirendszabályok a hozzáférés-vezérlési szabályok kezelésére és munkafolyamatok indítására szolgálnak. A házirendszabályok létrehozása szükség van a halmazok számát növelésére, hogy a különböző átmeneti objektumállapotok rögzítése hozhasson létre. Az új halmazok újabb munkafolyamatokat indíthatnak, és minden munkafolyamat egyedi kérésekhez kapcsolódik a rendszerben. Mindez újabb megfontolandó tényezőt jelent a kapacitás tervezésekor.

A MIM házirend-konfigurációjában emellett a környezet kiépítési tevékenységeivel kapcsolatos döntések is érvényre jutnak. Gondolja át a következőket:

- Fog-e idegen biztonsági rendszerbiztonsági tagokat létesíteni, több Active Directory tartományi szolgáltatásokbeli (AD DS-) erdőben? Ha igen, azzal több munkafolyamatot és kérést idéz elő, ami növeli a rendszer terhelését.

- Fog-e kód nélküli kiépítést alkalmazni? Ha igen, az hatással lesz a várható szabálybejegyzések, illetve a kapcsolódó kérések és munkafolyamatok mennyiségére is a rendszerben.

## <a name="next-steps"></a>További lépések

- [Topológiai szempontok a MIM üzembe helyezésekor](topology-considerations.md)
- A letölthető [Forefront Identity Manager (FIM) 2010 kapacitásának tervezési útmutatója](http://go.microsoft.com/fwlink/?LinkId=200180) hiányzóra változik, részletes információkat olvashat egy tesztkörnyezetről és teljesítménytesztelési eredményekről.

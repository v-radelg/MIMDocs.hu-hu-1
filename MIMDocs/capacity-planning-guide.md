---
title: Kapacitástervezési útmutató | Microsoft Docs
description: Az útmutató azokat a tényezőket ismerteti, amelyeket célszerű figyelembe venni a MIM 2016 üzembe helyezése előtt – ilyenek például a terhelésszintek és a szabályozási döntések.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 15eb35d01ed5c5c6e125c45f238bb2f7a7c564d7
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042117"
---
# <a name="capacity-planning-guide"></a>Kapacitástervezési útmutató<!--test-->

A Microsoft Identity Manager (MIM) segítségével létrehozhatja, frissítheti és eltávolíthatja a szervezetében lévő felhasználói fiókokat. Emellett a segítségével a végfelhasználók kezelhetik a saját fiókjuk önkiszolgáló szolgáltatásait. Ezek a műveletek még egy kis környezetben is gyorsan összeadódhatnak.

A MIM használatának megkezdése előtt a jelen útmutató és a tesztkörnyezet segítségével ismerje meg az üzembe helyezés megfelelő hatókörét. Ez a cikk számos olyan gyakori tényezővel foglalkozik, amelyet érdemes számításba venni. Ugyanakkor mivel minden üzembe helyezés egyedi, továbbra is az esetek laboratóriumi tesztelése a legjobb módszer az igényeinek megfelelő kiszolgálók, hardverek vagy topológiák meghatározására.

Ha még nem ismeri a MIM 2016 szoftvert és összetevőit, a folytatás előtt ismerje meg közelebbről a [Microsoft Identity Manager 2016-ot](microsoft-identity-manager-2016.md).

## <a name="overview"></a>Áttekintés

Számos tényező befolyásolhatja a Microsoft Identity Manager üzemelő példányának általános kapacitását és teljesítményét:

- Az a módszer, amelyben fizikailag üzembe helyezi a központhoz tartozó összetevőket (topológia).
- A hardver, amelyen az összetevők futnak.
- A webszolgáltatási házirend konfigurációs objektumainak száma és összetettsége jelentős tényezőt jelent a kapacitás tervezésekor.
- A központi telepítés várható mérete és a várt terhelés általában a teljesítményt és a kapacitást befolyásoló nyilvánvaló tényezőket is felhasználja.

A következő táblázat ismerteti a 2016-es üzemelő példány kapacitását és teljesítményét befolyásoló fő tényezőket:

| Tervezési tényező | Megfontolandó szempontok |
| ------------- | -------------- |
| Topológia | A MIM-szolgáltatások eloszlása a hálózaton található számítógépek között. |
| Hardver | A fizikai hardver (fizikai vagy virtuális) minden egyes virtuális merevlemez-összetevőhöz, beleértve a CPU-t, a memóriát, a hálózati adaptert és a merevlemez-konfigurációt. |
| A MIM házirend-konfigurációs objektumai | A MIM házirend-konfigurációs objektumainak száma és típusa – ide tartoznak a halmazok, a felügyeleti házirend-szabályok és a munkafolyamatok. |
| Méretezés | A felhasználók, csoportok, számított csoportok és egyéni objektumtípusok, amelyeket a 2016-es felügyeleti webalkalmazás kezel. Vegye figyelembe a dinamikus csoportok összetettségét is, és vegye ezt számításba a csoportok beágyazásakor. |
| Betöltés | A használat gyakorisága, Olyan műveletek, mint az új csoport vagy felhasználó létrehozása, a jelszó alaphelyzetbe állítása vagy a portálon végzett látogatások percenként vagy órában. Vegye figyelembe, hogy a terhelés egy adott óra, nap, hét vagy év alatt változhat. Az összetevőtől függően tervezhet a csúcsterheléssel vagy az átlagos terheléssel is. |

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

Vegye számításba a MIM-összetevők várható terhelésének típusát is. A terhelést a környezet aktuális alkalmazásai alapján kell megbecsülni. Célszerű megfontolni többek között a következő kérdéseket:

- Milyen gyakran várhatók a csoporthoz való csatlakozásra és a csoport elhagyására irányuló kérések?

- Milyen gyakran várható, hogy valamelyik felhasználó statikus vagy dinamikus csoportot hoz létre?

- Hány nem felhasználói művelet várható? Ilyen lehet például a változások szinkronizálása külső rendszerekből. Fontos számításba venni az identitásadatok külső rendszerekkel való szinkronizálása által előidézett terhelést is.

- Milyen jellegű alkalmazási helyzetekben tervezi használni a rendszert? A különböző helyzetekben a különböző terhelési mintázatok is hozzájárulnak. Előfordulhat például, hogy az ügyfélszámítógépek, amelyeken a 2016-es adatbázis-kiszolgáló van telepítve, rendszeres időközönként ellenőrzik, hogy a bejelentkezéskor szükség van-e regisztrációra.

- Várhatók-e nagy változások a terhelési szintben a normál és a csúcsterhelés között? Például általában sok jelszó áll alaphelyzetbe az ünnepnapok után. Ügyeljen arra, hogy a rendszer karbantartási és szinkronizálási ütemezése elkerülje a várható csúcsterheléseket. A kapacitástervezés során mindenképpen vegye figyelembe a csúcsterhelési időszakokat.

## <a name="policy-configuration-objects"></a>Házirend-konfigurációs objektumok

A MPR, a készletek, a munkafolyamatok és a szinkronizálási szabályok egy központi telepítéshez tartoznak. A MIM-példány minden ügyfélnél egyedi, hiszen a házirend-konfiguráció az adott környezet igényeinek megfelelően változik. A fő teljesítményre vonatkozó szempontok a következő, a (z) webszolgáltatási házirend konfigurációs objektumai:

- **Halmazok** A rendszerben minden műveletet a meglévő halmaztagságok és az ezekben változást előidéző tényezők fényében kell értékelni. Előfordulhat például, hogy egy adott iroda felépítési számának változása nem jelentős hatással van. Más változásoknak azonban fokozatos közvetett hatásai lehetnek: egy vezetőváltás például több szinten több objektumra is hatással lehet.

- **Felügyeleti házirendszabályok** A felügyeleti házirendszabályok a hozzáférés-vezérlési szabályok kezelésére és munkafolyamatok indítására szolgálnak. A MPR létrehozása szükségessé teheti a készletek számának növelését, hogy a különböző objektumok átmeneti állapotának rögzítése is megtörténjen. Az új halmazok újabb munkafolyamatokat indíthatnak, és minden munkafolyamat egyedi kérésekhez kapcsolódik a rendszerben. Mindez újabb megfontolandó tényezőt jelent a kapacitás tervezésekor.

A MIM házirend-konfigurációjában emellett a környezet kiépítési tevékenységeivel kapcsolatos döntések is érvényre jutnak. Gondolja át a következőket:

- Fog-e idegen biztonsági rendszerbiztonsági tagokat létesíteni, több Active Directory Domain Services-beli (AD DS-) erdőben? Ha igen, azzal több munkafolyamatot és kérést idéz elő, ami növeli a rendszer terhelését.

- Fog-e kód nélküli kiépítést alkalmazni? Ha igen, az hatással lesz a várható szabálybejegyzések, illetve a kapcsolódó kérések és munkafolyamatok mennyiségére is a rendszerben.

## <a name="next-steps"></a>További lépések

- [Topológiai szempontok a MIM üzembe helyezésekor](topology-considerations.md)
- A [Forefront Identity Manager (FIM) 2010 kapacitás-tervezési útmutatója](https://www.microsoft.com/en-us/download/details.aspx?id=7437) részletesebben ismerteti a tesztelési buildek és a teljesítmény tesztelésének eredményét.

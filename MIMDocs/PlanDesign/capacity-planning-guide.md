---
title: "Kapacitástervezési útmutató | Microsoft Docs"
description: "Az útmutató azokat a tényezőket ismerteti, amelyeket célszerű figyelembe venni a MIM 2016 üzembe helyezése előtt – ilyenek például a terhelésszintek és a szabályozási döntések."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/21/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: e33c5bb284d15a5e94118e621b968fcad5b65e2d
ms.lasthandoff: 05/02/2017


---

# <a name="capacity-planning-guide"></a>Kapacitástervezési útmutató

A Microsoft Identity Manager (MIM) segítségével létrehozhatja, frissítheti és eltávolíthatja a szervezetében lévő felhasználói fiókokat. Emellett a segítségével a végfelhasználók kezelhetik a saját fiókjuk önkiszolgáló szolgáltatásait. Ezek a műveletek még egy kis környezetben is gyorsan összeadódhatnak.

A MIM használatának megkezdése előtt a jelen útmutató és a tesztkörnyezet segítségével ismerje meg az üzembe helyezés megfelelő hatókörét. Ez a cikk számos olyan gyakori tényezővel foglalkozik, amelyet érdemes számításba venni. Ugyanakkor mivel minden üzembe helyezés egyedi, továbbra is az esetek laboratóriumi tesztelése a legjobb módszer az igényeinek megfelelő kiszolgálók, hardverek vagy topológiák meghatározására.

Ha még nem ismeri a MIM 2016 szoftvert és összetevőit, a folytatás előtt ismerje meg közelebbről a [Microsoft Identity Manager 2016-ot](/microsoft-identity-manager/understand-explore/microsoft-identity-manager-2016).

## <a name="overview"></a>Áttekintés
A Microsoft Identity Manager-rendszer általános kapacitását és teljesítményét számos változó befolyásolhatja. A MIM-összetevők (topológia) fizikai üzembe helyezésének módja, valamint az összetevők futtatására szolgáló hardvereszközök fontos tényezőt jelentenek a MIM-rendszertől elvárható teljesítmény és kapacitás szempontjából. A MIM házirend-konfigurációs objektumainak száma és összetettsége kevésbé nyilvánvaló, ugyanakkor jelentős megfontolandó tényezőt jelent a kapacitástervezés során. Végül a környezet várható mérete és terhelése jellemzően viszonylag kézenfekvő tényezőt jelent a teljesítmény és kapacitás szempontjából.

A MIM 2016-környezettől elvárható kapacitást és teljesítményt érintő főbb tényezőket a következő táblázat ismerteti.

| Tervezési tényező | Szempontok |
| ------------- | -------------- |
| Topológia | A MIM-szolgáltatások eloszlása a hálózaton található számítógépek között. |
| Hardver | A MIM egyes összetevőihez futtatott fizikai és esetleges virtuális hardverek specifikációi. Ide tartozik a CPU, a memória, a hálózati adapter és a merevlemez konfigurációja. |
| A MIM házirend-konfigurációs objektumai | A MIM házirend-konfigurációs objektumainak száma és típusa – ide tartoznak a halmazok, a felügyeleti házirend-szabályok és a munkafolyamatok. |
| Méretezés | A MIM 2016 segítségével kezelni kívánt felhasználók, csoportok, számított csoportok és egyéni objektumtípusok, például számítógépek. Vegye figyelembe a dinamikus csoportok összetettségét is, és vegye ezt számításba a csoportok beágyazásakor. |
| Terhelés | A használat gyakorisága, például hogy várhatóan milyen gyakran hoznak létre új csoportokat vagy felhasználókat, állítanak vissza jelszavakat vagy látogatják meg a portált egy adott időszakban. Vegye figyelembe, hogy a terhelés egy adott óra, nap, hét vagy év alatt változhat. Az összetevőtől függően tervezhet a csúcsterheléssel vagy az átlagos terheléssel is. |


## <a name="hosting-microsoft-identity-manager-components"></a>A Microsoft Identity Manager összetevőinek üzemeltetése

A Microsoft Identity Manager összetevőinek nem kell ugyanazon a számítógépen lenniük. Ezen összetevők, valamint a futtatásukra szolgáló fizikai vagy virtuális gépek számbavétele a kapacitástervezés fontos mozzanata.

A hardveres tényezők befolyásolhatják a MIM-környezet teljesítményét. Példa:
- Milyen fizikai lemezkonfigurációt használ a MIM 2016-szolgáltatás SQL-adatbázisát futtató számítógép? A lemezkonfigurációt alkotó forgórészek száma, illetve a napló- és adatfájlok elosztása jelentős mértékben befolyásolhatja a rendszer teljesítményét.

Mindezek mellett gondolja át a konfigurációt érintő külső tényezőket is. Például:
- Ha tárolóhálózatot használ a MIM 2016-szolgáltatás adatbázis-konfigurációjához: milyen egyéb alkalmazások használják még a tárolóhálózatot? Ha ezek az alkalmazások „versenyeznek” a tárolóhálózat megosztott lemezerőforrásaiért, az hatással lehet az adatbázis teljesítményére.


## <a name="users-and-groups"></a>Felhasználók és csoportok
A környezetben található felhasználók és csoportok száma jellemzően fontos szempont a rendszer méretezésének tervezésekor. A tervezéskor ugyanakkor számos egyéb kapcsolódó szempontot célszerű mérlegelnie.

- Létrehozhatnak-e a felhasználók csoportokat? Ha igen, érdemes felmérni, hogy a felhasználók által létrehozott új csoportok milyen hatással lesznek a csoportok növekedésére a környezetben.

- Várható-e dinamikus csoportok létrehozása? Mérje fel, hogy hány és milyen típusú dinamikus csoport létrehozása várható a környezetben.


## <a name="expected-load-levels"></a>Várható terhelési szintek
Vegye számításba a MIM-összetevők várható terhelésének típusát is. Ez feltehetően megállapítható a környezetben jelenleg futó alkalmazások felmérésével. Célszerű megfontolni többek között a következő kérdéseket:

- Milyen gyakran várhatók a csoporthoz való csatlakozásra és a csoport elhagyására irányuló kérések?

- Milyen gyakran várható, hogy valamelyik felhasználó statikus vagy dinamikus csoportot hoz létre?

- Hány nem felhasználói művelet várható? Ilyen lehet például a változások szinkronizálása külső rendszerekből. Fontos számításba venni az identitásadatok külső rendszerekkel való szinkronizálása által előidézett terhelést is.

- Milyen jellegű alkalmazási helyzetekben tervezi használni a rendszert? A különböző alkalmazási helyzetek eltérő terhelési mintázatokkal járnak. A MIM 2016-ügyféllel rendelkező ügyfélszámítógépek például adott időközönként ellenőrzik, hogy szükséges-e regisztráció a bejelentkezéshez, ami növeli a rendszer terhelését.

- Várhatók-e nagy változások a terhelési szintben a normál és a csúcsterhelés között? Jellemző például, hogy a munkaszüneti időszakok után sok jelszóváltoztatás történik. Ügyeljen arra, hogy a rendszer karbantartási és szinkronizálási ütemezése elkerülje a várható csúcsterheléseket. A kapacitástervezés során mindenképpen vegye figyelembe a csúcsterhelési időszakokat.


## <a name="policy-configuration-objects"></a>Házirend-konfigurációs objektumok

A Microsoft Identity Manager házirend-konfigurációs objektumai az adott példány felügyeleti házirendszabályait, halmazait, munkafolyamatait és szinkronizálási szabályait foglalják magukban. A MIM-példány minden ügyfélnél egyedi, hiszen a házirend-konfiguráció az adott környezet igényeinek megfelelően változik. A MIM házirend-konfigurációs objektumaihoz kapcsolódó legfontosabb teljesítményi szempontok a következők:

- **Halmazok** A rendszerben minden műveletet a meglévő halmaztagságok és az ezekben változást előidéző tényezők fényében kell értékelni. Egy egyszerű változás, például egy adott iroda épületszámának változása valószínűleg nem jár túl nagy hatással. Más változásoknak azonban fokozatos közvetett hatásai lehetnek: egy vezetőváltás például több szinten több objektumra is hatással lehet.

- **Felügyeleti házirendszabályok** A felügyeleti házirendszabályok a hozzáférés-vezérlési szabályok kezelésére és munkafolyamatok indítására szolgálnak. A felügyeleti házirendszabályok létrehozása során azt tapasztalhatja, hogy a különböző átmeneti objektumállapotok rögzítése érdekében növelnie kell a halmazok számát. Az új halmazok újabb munkafolyamatokat indíthatnak, és minden munkafolyamat egyedi kérésekhez kapcsolódik a rendszerben. Mindez újabb megfontolandó tényezőt jelent a kapacitás tervezésekor.

A MIM házirend-konfigurációjában emellett a környezet kiépítési tevékenységeivel kapcsolatos döntések is érvényre jutnak. Gondolja át a következőket:

- Fog-e idegen biztonsági rendszerbiztonsági tagokat létesíteni, több Active Directory tartományi szolgáltatásokbeli (AD DS-) erdőben? Ha igen, azzal több munkafolyamatot és kérést idéz elő, ami növeli a rendszer terhelését.

- Fog-e kód nélküli kiépítést alkalmazni? Ha igen, az hatással lesz a várható szabálybejegyzések, illetve a kapcsolódó kérések és munkafolyamatok mennyiségére is a rendszerben.


## <a name="see-also"></a>További információ
- [Topológiai szempontok a MIM üzembe helyezésekor](topology-considerations.md)
- A letölthető [Forefront Identity Manager (FIM) 2010 kapacitástervezési útmutatóban](http://go.microsoft.com/fwlink/?LinkId=200180) részletes információkat olvashat egy tesztkörnyezetről és a kapcsolódó teljesítménytesztelési eredményekről.


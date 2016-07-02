---
title: "Topológiai szempontok a MIM üzembe helyezésekor | Microsoft Identity Manager"
description: "Ismerje meg a MIM 2016 összetevőit, és olvasson javaslatokat arról, hogyan telepheti őket a környezetben."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c023d147d0fcc1525fefbe866c952e217f7bee6b
ms.openlocfilehash: e33a08d77a0b5c422cdbc8c19516b55df980a2c6


---


# Topológiai szempontok
A Microsoft Identity Manager (MIM) összetevőit ugyanarra a kiszolgálóra, vagy több különböző konfigurációjú kiszolgálóra is telepítheti. Az üzembe helyezéshez választott topológia hatással van a MIM-mel elérhető teljesítményre. Ez a cikk több lehetséges üzembe helyezési topológiát mutat be.

## A MIM összetevői
Az üzembe helyezési topológia megtervezéséhez fontos tudni, hogy melyik összetevő mire alkalmas, és milyen kapcsolatban állnak egymással.

- **MIM-portál** – jelszó-átállításra, valamint csoportkezelési és felügyeleti feladatokra szolgáló felület.
    -
- **MIM szolgáltatás** – a MIM 2016 identitáskezelési funkcióit megvalósító webes szolgáltatás.
- **MIM Synchronization Service** – adatokat szinkronizál más identitáskezelő rendszerekkel.
- **Microsoft SQL Server** – a MIM szolgáltatás és a MIM Sync Service egyaránt SQL-adatbázisban tárolja az adatokat.

A következő táblázat ismerteti a lehetőségeket a MIM egyes összetevőinek üzemeltetésére. Az összetevők üzemeltethetők ugyanazon a számítógépen, vagy eloszthatók több kiszolgáló és fürt között.

| | MIM-portál | MIM szolgáltatás | MIM Sync Service | SQL Server |
| --- | --- | --- | --- | --- |
| Ugyanaz a számítógép | Igen | Igen | Igen | Igen |
| Külön kiszolgáló | Igen | Igen | Igen | Igen |
| Hálózati terheléselosztási fürt | Igen | Igen | | |
| Kiszolgálófürt | | | | Igen |


## Többrétegű topológia
A többrétegű a leggyakrabban használt topológia, amely a legnagyobb rugalmasságot kínálja. A MIM-portál, a MIM szolgáltatás és az adatbázisok külön rétegekben működnek, és különböző számítógépeken vannak telepítve. Ez a topológia rugalmasságot biztosít a különböző MIM-összetevők méretezéséhez. A MIM-portál például egy hálózati terheléselosztási fürt keretében további kiszolgálók hozzáadásával horizontálisan méretezhető. Ehhez hasonlóan a MIM szolgáltatás is skálázható hálózati terheléselosztási fürttel, illetve a fürtben működő számítógépek (csomópontok) számának igény szerinti növelésével.

Többrétegű topológiában minden SQL-adatbázist – egy a MIM szolgáltatáshoz, egy pedig a MIM Synchronization Service-hez – külön számítógép működtet. Az SQL-adatbázisokat üzemeltető számítógépek teljesítményének méretezhetősége a hardvereszközök bővítésével vagy fejlesztésével növelhető, például a processzorok fejlesztésével, további processzorok telepítésével, a RAM növelésével vagy fejlesztésével, illetve a merevlemez-konfigurációk fejlesztésével az olvasási és írási sebesség növelése és a késés csökkentése érdekében.

![A többrétegű MIM-topológia ábrája](media/MIM-topo-multitier.png)

Ebben a konfigurációban a MIM Synchronization Service és az adatbázisa ugyanazon a számítógépen fut. Hasonló teljesítmény érhető el ugyanakkor a MIM Synchronization Service és a kapcsolódó adatbázis külön számítógépen való üzemeltetése esetén is, ha egy gigabites dedikált hálózati kapcsolat van a számítógépek között.


## Többrétegű topológia több MIM szolgáltatással
Az adatok külső rendszerekkel való szinkronizálása hosszú időt vehet igénybe, és jelentős terhelést helyez a rendszerre az adott időszakban. Ha a szinkronizálási konfiguráció munkafolyamatos házirendek aktiválását eredményezi, akkor ezek a házirendek a végfelhasználói munkafolyamatokkal „versenyeznek” az erőforrásokért. Ezek a problémák hangsúlyozottan jelentkezhetnek a hitelesítési munkafolyamatok, például jelszó-átállítási feladatok esetén, amelyeknél a végfelhasználónak valós időben kell várnia, hogy a folyamat befejeződjön. Ha külön példányt biztosít a MIM szolgáltatásnak a végfelhasználói műveletekhez, és külön portált az adminisztratív adatszinkronizálási feladatokhoz, azzal fokozhatja a végfelhasználói műveletek válaszképességét.

![A többszörös többrétegű MIM-topológia ábrája](media/MIM-topo-multitier-multiservice.png)

A hagyományos többrétegű topológiához hasonlóan a MIM-portál teljesítménye is fokozható hálózati terheléselosztási fürt használatával, és a fürtben futó csomópontok számának igény szerinti növelésével.

A MIM Synchronization Service és a MIM szolgáltatás adatbázisainak üzemeltetésére szolgáló SQL Servert futtató számítógépek drámai módon befolyásolják a MIM-környezet általános teljesítményét. Ennek megfelelően az adatbázisok teljesítményének optimalizálásához kövesse az SQL Server dokumentációjában foglalt javaslatokat. További tudnivalókért lásd a következő dokumentumokat:

## Lásd még:
- A letölthető [Forefront Identity Manager (FIM) 2010 kapacitástervezési útmutatóban](http://go.microsoft.com/fwlink/?LinkId=200180) részletes információkat olvashat egy tesztkörnyezetről és a kapcsolódó teljesítménytesztelési eredményekről.



<!--HONumber=Jun16_HO4-->



---
title: "A PAM üzembe helyezése és konfigurálása | Microsoft Identity Manager"
description: "Ütemterv a MIM telepítéséhez és konfigurálásához a Privileged Access Management szolgáltatáshoz."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 4b4953089cb676baae97988f380debbfefcd1083


---

# A MIM-környezet konfigurálása a Privileged Access Management szolgáltatáshoz
Az erdők közötti hozzáférés beállítását, az Active Directory és a Microsoft Identity Manager telepítését és konfigurálását, illetve a szükséges időben való hozzáférésre irányuló kérelem bemutatását hét lépésben hajthatja végre.

Ezek a lépések úgy vannak felépítve, hogy segítségükkel a tesztkörnyezetet az alapoktól tudja elkezdeni és felépíteni. Ha a PAM szolgáltatást meglévő környezetben szeretné alkalmazni, használhatja saját tartományvezérlőit és felhasználói fiókjait is ahelyett, hogy újakat hozna létre a példáknak megfelelően.

1.  Készítse elő a *CORPDC* kiszolgálót tartományvezérlőként, a *CORPWKSTN* gépet pedig tag munkaállomásként történő használatra.

2.  Készítse elő a *PRIVDC* kiszolgálót tartományvezérlőként történő használatra.

3.  Készítse elő a *PAMSRV* kiszolgálót a *PRIV* erdőben.

4.  Telepítse a MIM-összetevőket a *PAMSRV* kiszolgálóra, illetve a parancsmagokat egy *CONTOSO* erdőbeli tag munkaállomásra, és készítse elő őket a Privileged Access Management szolgáltatáshoz.

5.  Hozzon létre megbízható kapcsolatot a *PRIV* és a *CONTOSO* erdő között.

6.  Védett erőforrásokhoz és tagfiókokhoz hozzáféréssel rendelkező privilegizált biztonsági csoportok előkészítése a szükséges időben történő (just-in-time) felügyelet elvére épülő Privileged Access Management szolgáltatásra.

7.  Védett erőforrásokhoz való privilegizált, emelt szintű hozzáférés kérelmezésének, fogadásának és felhasználásának bemutatása.

>[!div class="step-by-step"]
[Indítás »](step-1-prepare-corp-domain.md)



<!--HONumber=Jul16_HO3-->



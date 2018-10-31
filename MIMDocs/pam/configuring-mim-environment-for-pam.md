---
title: A MIM 2016 konfigurálása a Privileged Access Management szolgáltatás használatához | Microsoft Docs
description: Ütemterv a MIM telepítéséhez és konfigurálásához a Privileged Access Management szolgáltatáshoz.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fd444f9c67f1daeaf33883a988f54a97e12e685c
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379515"
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>A MIM-környezet konfigurálása a Privileged Access Management szolgáltatáshoz

Az erdők közötti hozzáférés beállítását, az Active Directory és a Microsoft Identity Manager telepítését és konfigurálását, illetve a szükséges időben való hozzáférésre irányuló kérelem bemutatását hét lépésben hajthatja végre.

Ezek a lépések úgy vannak felépítve, hogy segítségükkel a tesztkörnyezetet az alapoktól tudja elkezdeni és felépíteni. Ha a PAM szolgáltatást meglévő környezetben szeretné alkalmazni, használhatja saját tartományvezérlőit és felhasználói fiókjait is ahelyett, hogy újakat hozna létre a példáknak megfelelően.

1. Készítse elő a *CORPDC* kiszolgálót tartományvezérlőként, a *CORPWKSTN* gépet pedig tag munkaállomásként történő használatra.

2. Készítse elő a *PRIVDC* kiszolgálót tartományvezérlőként történő használatra.

3.  Készítse elő a *PAMSRV* kiszolgálót a *PRIV* erdőben.

4.  Telepítse a MIM-összetevőket a *PAMSRV* kiszolgálóra, illetve a parancsmagokat egy *CONTOSO* erdőbeli tag munkaállomásra, és készítse elő őket a Privileged Access Management szolgáltatáshoz.

5.  Hozzon létre megbízható kapcsolatot a *PRIV* és a *CONTOSO* erdő között.

6.  Védett erőforrásokhoz és tagfiókokhoz hozzáféréssel rendelkező privilegizált biztonsági csoportok előkészítése a szükséges időben történő (just-in-time) felügyelet elvére épülő Privileged Access Management szolgáltatásra.

7.  Védett erőforrásokhoz való privilegizált, emelt szintű hozzáférés kérelmezésének, fogadásának és felhasználásának bemutatása.

> [!div class="step-by-step"]
> [Indítás »](step-1-prepare-corp-domain.md)

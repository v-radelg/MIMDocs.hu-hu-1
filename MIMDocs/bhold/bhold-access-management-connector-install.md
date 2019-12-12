---
title: A BHOLD hozzáférés-kezelési összekötő telepítése | Microsoft Docs
description: Az BHOLD-összekötő modul támogatja az adatkezdeti és folyamatos szinkronizálást
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 60886a84c6105e94a2cd3d42f17b86b2d69c8c0a
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516064"
---
# <a name="access-management-connector-installation"></a>Hozzáférés-kezelési összekötő telepítése

A BHOLD Suite hozzáférés-vezérlési összekötő modulja az adatok kezdeti és folyamatos szinkronizálását is támogatja a BHOLD-ben. A hozzáférés-vezérlési összekötő a Microsoft Identity Manager (BHOLD Core adatbázis, a FIM 2010 metaverse és a TARGET Applications and Identity Stores) közötti adatáthelyezést is támogatja. A hozzáférés-kezelési összekötő modul telepítése után létrehozhat olyan FIM felügyeleti ügynököket, amelyek vezérlik az BHOLD és a felügyeleti csomag közötti adatforgalmat.

## <a name="access-management-connector-software-requirements"></a>Hozzáférés-kezelési összekötő szoftverre vonatkozó követelmények

A hozzáférés-kezelési összekötő modul telepítése előtt telepítenie kell a Microsoft .NET Framework 4 alkalmazást. A .NET-keretrendszer 4-es és a telepítési utasításokkal kapcsolatos további információkért tekintse meg a [Microsoft .net kezdőlapját](http://www.microsoft.com/net).
Telepítenie kell a hozzáférés-vezérlési összekötőt egy olyan számítógépre, amelyen a (z) rendszer a famodul FIM synchronization szolgáltatását futtatja

## <a name="access-management-connector-setup"></a>Hozzáférés-kezelési összekötő beállítása

A hozzáférés-vezérlési modul telepítéséhez jelentkezzen be a Tartománygazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként azon a kiszolgálón, amelyre telepíteni kívánja a BHOLD FIM integrációs modulját:

- AccessManagementConnector. msi

A programfájl rendszergazdaként való futtatásához kattintson a jobb gombbal a fájlra, majd kattintson a **Futtatás rendszergazdaként**parancsra.

## <a name="next-steps"></a>További lépések

- [BHOLD FIM-integráció telepítése](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) A szerepkörök végfelhasználói önkiszolgáló szolgáltatásának engedélyezéséhez telepítheti a BHOLD FIM integrációs modulját.
- [BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

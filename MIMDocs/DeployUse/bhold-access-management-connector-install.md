---
title: "BHOLD access management-összekötő telepítés |} Microsoft Docs"
description: "A BHOLD összekötő modul támogatja a kezdeti és folyamatos szinkronizálását."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/15/2017
---
# <a name="access-management-connector-installation"></a>Access Management Connector telepítése

A BHOLD Suite Access Management-összekötő modul BHOLD az adatok kezdőbetűje és a folyamatban lévő szinkronizálás támogatja. Az Access Management-összekötő a Microsoft Identity Manager (MIM) Synchronization Service áthelyezni az adatokat a BHOLD Core-adatbázis, a FIM 2010 metaverse, és a cél alkalmazások és a identitástárolók között működik. Az Access Management-összekötő-modul telepítése után lesz FIM Kezelőügynökök BHOLD és a MIM közötti adatáramlás szabályozó létrehozásához.

## <a name="access-management-connector-software-requirements"></a>Access Management-összekötő szoftverkövetelmények

Az Access Management-összekötő modul telepítése előtt telepítenie kell a Microsoft .NET-keretrendszer 4. .NET-keretrendszer 4 és a telepítési utasításokat kapcsolatos további információkért tekintse meg a [Microsoft .NET-kezdőlap](http://www.microsoft.com/net).
A FIM szinkronizálási szolgáltatás a MIM futtató számítógépen telepítenie kell az Access Management-összekötő.

## <a name="access-management-connector-setup"></a>Access Management-összekötő telepítése

A hozzáférés-vezérlés felügyeleti modul telepítése, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD FIM integrációs modul telepítése:

- AccessManagementConnector.msi

A program fájlt rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

## <a name="next-steps"></a>További lépések

- [BHOLD FIM-integráció telepítése](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) végfelhasználói önkiszolgáló szerepkörök engedélyezéséhez a BHOLD FIM integrációs modul telepítése
- [BHOLD a telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)
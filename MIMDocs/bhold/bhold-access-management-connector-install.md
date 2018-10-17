---
title: BHOLD access management connector telepítése |} A Microsoft Docs
description: A BHOLD összekötő modul támogatja az adatok kezdeti és folyamatos szinkronizálása
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 60886a84c6105e94a2cd3d42f17b86b2d69c8c0a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358601"
---
# <a name="access-management-connector-installation"></a>Access Management Connector telepítése

A BHOLD Suite Access Management-összekötő modul BHOLD be az adatok kezdeti és a folyamatban lévő szinkronizálás támogatja. Az Access Management-összekötő együttműködik a Microsoft Identity Manager (MIM) szinkronizálási szolgáltatás többek között a BHOLD Core adatbázis, a FIM 2010 metaverzum, és a cél alkalmazások és a identitástárolók adatok áthelyezéséhez. Az Access Management-összekötő modul telepítése után lesz FIM felügyeleti ügynökök, amelyek vezérlik a BHOLD és a MIM között az adatfolyam létrehozásához.

## <a name="access-management-connector-software-requirements"></a>Access Management-összekötő szoftverkövetelmények

Az Access Management-összekötő modul telepítése előtt telepítenie kell a Microsoft .NET-keretrendszer 4. .NET-keretrendszer 4 és telepítésével kapcsolatos további információkért lásd: a [a Microsoft .NET-kezdőlap](http://www.microsoft.com/net).
A FIM szinkronizálási szolgáltatás a mim-et futtató számítógépen telepítenie kell az Access Management-összekötő.

## <a name="access-management-connector-setup"></a>Access Management-összekötő beállítása

A hozzáférés-vezérlés felügyeleti modul telepítése, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD FIM integrációs modul telepítése:

- AccessManagementConnector.msi

A program rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

## <a name="next-steps"></a>További lépések

- [BHOLD FIM integration telepítése](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) ahhoz, hogy a végfelhasználók önkiszolgáló munkafolyamataihoz szerepkörök, telepítheti a BHOLD FIM integrációs modul
- [A BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

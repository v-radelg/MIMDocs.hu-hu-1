---
title: A BHOLD reporting telepítése |} A Microsoft Docs
description: A BHOLD reporting modul lehetővé teszi a szerepkörökről és engedélyezési házirendeket jelentéseket generálhat
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1f69f9f08cba24898509c771e4477b81c5ed272f
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358007"
---
# <a name="bhold-reporting-installation"></a>A BHOLD reporting telepítése

A BHOLD Reporting modul teszi lehetővé a BHOLD-szerepkörökről és egyéb engedélyezési szabályzatok jelentéskészítéshez. Ezek a jelentések gyakran hasznosak, a naplózás vagy a szabályozási követelményeknek való megfelelés bemutatásához. Ez a modul is, a hitelesítés a szervezeten belüli felhasználók szerepkörökhöz tagsága elemezheti az adatokat, így az képes terjeszti ki. A jelentések is korlátozott nézetek, győződjön meg arról, hogy a felhasználók, akik jelentéseket hozhat létre látható információk csak általuk megtekintéséhez.

## <a name="bhold-reporting-installation-requirements"></a>A BHOLD Reporting telepítési követelményei

A BHOLD Reporting modul a telepítés előtt telepítenie kell a BHOLD Core-modul a kiszolgálón, amelyen a BHOLD Reporting modul telepítését tervezi. A BHOLD Core modul telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> A BHOLD Reporting és BHOLD igazolási telepítésekor, telepítenie kell a BHOLD Reporting BHOLD igazolási telepítése előtt.

## <a name="before-you-begin"></a>Előkészületek

A BHOLD Reporting modul telepítése előtt kell a BHOLD Reporting telepítővarázsló a telepítés befejezéséhez szükséges információk megadására. A következő munkalap segítségével fel ezt az információt, hogy készen áll a szükséges megadni.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztott, itt adhatja meg, hogy az Active Directory Domain Services biztonsági fog férhet hozzá a BHOLD Core.                                                                                                                | Jelölje be a jelölőnégyzetet. </br>**Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                                  | Megadja azt a tartományt, amely tartalmazza a szolgáltatásfiókhoz, amelyet a BHOLD Core telepítése során létrehozott. További információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló által automatikusan történik. Módosítsa a nevét, csak akkor, ha annak értéke helytelen. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-neve, nem a teljesen minősített tartománynevét (FQDN) használatával. Például ha a tartomány teljes Tartománynevét a fabrikam.com, adja meg a tartomány nevét, a FABRIKAM. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                          | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | A szolgáltatás felhasználói fiók jelszava.                                                                                                                                                                       | Írás a jelszót kell megadnia: </br>**Fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>A BHOLD Reporting telepítése

BHOLD Reporting-modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájl, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD Reporting modul telepítése:

- BholdReporting<em>\<verzió\></em>\_Release.msi

Cserélje le *\<verzió\>* a BHOLD Reporting kiadás, amely telepíti a verziószámával.

A program rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

## <a name="next-steps"></a>További lépések

- [A BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

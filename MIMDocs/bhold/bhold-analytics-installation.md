---
title: A BHOLD Analytics telepítése |} A Microsoft Docs
description: A BHOLD Analytics modul adja meg a szabály alapú az adatokhoz való hozzáférés tesztelése
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ec7069156aa033b33a139ae83e26abcdea7b482a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358329"
---
# <a name="bhold-analytics-installation"></a>A BHOLD Analytics telepítése

A BHOLD Analytics modul adja meg az adatokhoz való hozzáférés biztosítására, hogy szervezete hatékonyan szabályozhatja a hozzáférést az adatokat, és megfelel-e belső és külső hozzáférési követelményekkel rendelkező szabályalapú tesztelése. A BHOLD Analytics modul által előállított automatizált hatáselemzés áttekintheti, hogy a felhasználók, akik a javasolt szabály végrehajtásának befolyásolhat egyaránt feleljenek meg kellene a szabály és a szabály sértene számát jeleníti meg. A BHOLD Analytics modulja is lehetővé teszi a felhasználók, akik lenne megfelelnek-e a szabály és azok számára, akik a szabály sértene részletes listáját.

## <a name="bhold-analytics-installation-requirements"></a>A BHOLD Analytics telepítési követelményei

A BHOLD Analytics modul a telepítés előtt telepítenie kell a BHOLD Core-modul a kiszolgálón, amelyen a BHOLD Analytics modul telepítését tervezi. A BHOLD Core modul telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Előkészületek

A BHOLD Analytics modul telepítése előtt kell a BHOLD Analytics telepítővarázslója a telepítés befejezéséhez szükséges információk megadására. A következő munkalap segítségével fel ezt az információt, hogy készen áll a szükséges megadni.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztott, itt adhatja meg, hogy az Active Directory Domain Services biztonsági fog férhet hozzá a BHOLD Core.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                                  | Megadja azt a tartományt, amely tartalmazza a szolgáltatásfiókhoz, amelyet a BHOLD Core telepítése során létrehozott. További információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló által automatikusan történik. Módosítsa a nevét, csak akkor, ha annak értéke helytelen. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-neve, nem a teljesen minősített tartománynevét (FQDN) használatával. Például ha a tartomány teljes Tartománynevét a fabrikam.com, adja meg a tartomány nevét, a FABRIKAM. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                          | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | A szolgáltatás felhasználói fiók jelszava.                                                                                                                                                                       | Írás a jelszót kell megadnia: **fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>A BHOLD Analytics telepítése

A BHOLD Analytics modul telepítése, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájl, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD Analytics modul telepítése:

- BholdAnalytics<em>\<verzió\></em>\_Release.msi

Cserélje le *\<verzió\>* a BHOLD Analytics kiadás, amely telepíti a verziószámával.

A program rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

# <a name="next-steps"></a>További lépések

- [A BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [A BHOLD telepítési útmutató](bhold-installation-guide.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

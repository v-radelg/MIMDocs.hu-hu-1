---
title: BHOLD jelentéskészítési telepítés | Microsoft Docs
description: A BHOLD jelentési modulja lehetővé teszi a szerepkörökkel és engedélyezési házirendekkel kapcsolatos jelentések létrehozását.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7bac40105aa53add914599c59e812587b6be9585
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042033"
---
# <a name="bhold-reporting-installation"></a>BHOLD jelentéskészítés telepítése

Az BHOLD jelentéskészítő modul lehetővé teszi jelentések készítését a szerepkörökkel és más engedélyezési házirendekkel kapcsolatban a BHOLD-ben. Ezek a jelentések gyakran hasznosak a naplózáshoz, illetve a szabályozási követelmények teljesítésének bemutatásához. Ez a modul kibővíti az engedélyezést a szervezeten belül azáltal, hogy megadja a felhasználóknak a szerepköreik tagságának elemzéséhez szükséges információkat. A jelentések korlátozott nézetekkel rendelkezhetnek, amelyek biztosítják, hogy a jelentéseket létrehozó felhasználók csak azokat az információkat jelenítsék meg, amelyek számára engedélyezett a megtekintés.

## <a name="bhold-reporting-installation-requirements"></a>BHOLD-jelentéskészítés telepítési követelményei

A BHOLD Reporting modul telepítése előtt telepítenie kell a BHOLD Core modult arra a kiszolgálóra, amelyre telepíteni kívánja a BHOLD jelentési modulját. Az BHOLD Core modul telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Ha a BHOLD-jelentéskészítési és a BHOLD-igazolást is telepíti, a BHOLD-igazolás telepítése előtt telepítenie kell a BHOLD-jelentéskészítést.

## <a name="before-you-begin"></a>Előkészületek

Mielőtt megkezdené a BHOLD-jelentéskészítő modul telepítését, elő kell készítenie, hogy a telepítés befejezéséhez szükséges információk megadásához a BHOLD Reporting telepítővarázsló számára szükség van. A következő munkalapon rögzítheti ezeket az adatokat, így készen áll arra, hogy szükség esetén megadja azt.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Biztonsági szolgáltató használata tartományon vagy gépen** | Ha be van jelölve, akkor a Active Directory tartományi szolgáltatások biztonság szabályozza a BHOLD mag elérését.                                                                                                                | Jelölje be a jelölőnégyzetet. </br>**Fontos:** Ha ez a jelölőnégyzet nincs bejelölve, a telepítés sikertelen lesz.                                                                                                                                                                                                                   |
| **Tartomány**                                  | A BHOLD Core telepítésekor létrehozott szolgáltatásfiókot tartalmazó tartományt adja meg. További információ: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló automatikusan megadja. A név csak akkor módosítható, ha helytelen. **Fontos:** A tartománynevet a NetBIOS (rövid) név használatával adja meg, ne a teljes tartománynevet (FQDN). Ha például a tartomány teljes tartományneve fabrikam.com, adja meg a tartománynevet FABRIKAM néven. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatás felhasználói fiókjának bejelentkezési nevét.                                                                                                                                                          | Írja be a felhasználói fiók nevét:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | Megadja a szolgáltatás felhasználói fiókjának jelszavát.                                                                                                                                                                       | Írja be a jelszót itt: </br>**Fontos:** Ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>BHOLD jelentéskészítés telepítése

Az BHOLD jelentéskészítő modul telepítéséhez jelentkezzen be a Tartománygazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként azon a kiszolgálón, amelyre telepíteni kívánja a BHOLD jelentéskészítő modult:

- A BholdReporting<em>\<verziója\></em>\_Release. msi

Cserélje le *\<version\>* a telepítendő BHOLD-jelentéskészítési kiadás verziószámára.

A programfájl rendszergazdaként való futtatásához kattintson a jobb gombbal a fájlra, majd kattintson a **Futtatás rendszergazdaként**parancsra.

## <a name="next-steps"></a>További lépések

- [BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

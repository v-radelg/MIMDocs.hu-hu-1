---
title: BHOLD Analytics-telepítés | Microsoft Docs
description: A BHOLD Analytics-modul az adathozzáférés szabályon alapuló tesztelését teszi lehetővé
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7d26abb58fa976d40638b9512d5684d86f483378
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79041930"
---
# <a name="bhold-analytics-installation"></a>BHOLD Analytics telepítése

A BHOLD Analytics modul az adathozzáférés szabály-alapú tesztelését biztosítja annak biztosításához, hogy a szervezete hatékonyan tudja szabályozni az adataihoz való hozzáférést, és megfelel a belső és külső hozzáférési követelményeknek. A BHOLD Analytics modul által generált automatikus Impact-elemzés áttekintést nyújt a javasolt szabályok betartatása által érintett felhasználók számáról, valamint azokról, akik a szabálynak eleget tesznek, és akik sértik a szabályt. A BHOLD Analytics modul részletes listát is tartalmaz azokról a felhasználókról, akik megfelelnek a szabálynak, és azok, akik sértik a szabályt.

## <a name="bhold-analytics-installation-requirements"></a>A BHOLD Analytics telepítési követelményei

A BHOLD Analytics modul telepítése előtt telepítenie kell a BHOLD Core modult arra a kiszolgálóra, amelyre telepíteni kívánja a BHOLD Analytics-modult. Az BHOLD Core modul telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Előkészületek

Mielőtt megkezdené a BHOLD Analytics modul telepítését, elő kell készítenie azokat az információkat, amelyeket a BHOLD Analytics telepítővarázsló a telepítés befejezéséhez szükséges. A következő munkalapon rögzítheti ezeket az adatokat, így készen áll arra, hogy szükség esetén megadja azt.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Biztonsági szolgáltató használata tartományon vagy gépen** | Ha be van jelölve, akkor a Active Directory tartományi szolgáltatások biztonság szabályozza a BHOLD mag elérését.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** Ha ez a jelölőnégyzet nincs bejelölve, a telepítés sikertelen lesz.                                                                                                                                                                                                                   |
| **Tartomány**                                  | A BHOLD Core telepítésekor létrehozott szolgáltatásfiókot tartalmazó tartományt adja meg. További információ: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló automatikusan megadja. A név csak akkor módosítható, ha helytelen. **Fontos:** A tartománynevet a NetBIOS (rövid) név használatával adja meg, ne a teljes tartománynevet (FQDN). Ha például a tartomány teljes tartományneve fabrikam.com, adja meg a tartománynevet FABRIKAM néven. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatás felhasználói fiókjának bejelentkezési nevét.                                                                                                                                                          | Írja be a felhasználói fiók nevét:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | Megadja a szolgáltatás felhasználói fiókjának jelszavát.                                                                                                                                                                       | Írja be a jelszót itt: **Fontos:** ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>BHOLD Analytics telepítése

A BHOLD Analytics modul telepítéséhez jelentkezzen be a Tartománygazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként azon a kiszolgálón, amelyre telepíteni kívánja a BHOLD Analytics-modult:

- A BholdAnalytics<em>\<verziója\></em>\_Release. msi

Cserélje le *\<version\>* a telepítendő BHOLD Analytics-kiadás verziószámára.

A programfájl rendszergazdaként való futtatásához kattintson a jobb gombbal a fájlra, majd kattintson a **Futtatás rendszergazdaként**parancsra.

## <a name="next-steps"></a>További lépések

- [BHOLD Core-telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [BHOLD telepítési útmutató](bhold-installation-guide.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

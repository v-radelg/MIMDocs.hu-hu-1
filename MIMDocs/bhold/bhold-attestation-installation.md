---
title: BHOLD-igazolás telepítése | Microsoft Docs
description: A BHOLD igazolási modulja lehetővé teszi a felülvizsgálók kijelölését és a felülvizsgálatok elvégzését
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d7e136ee86272429ff16a49b3c1319a4543648f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042321"
---
# <a name="bhold-attestation-installation"></a>BHOLD-igazolás telepítése

A BHOLD igazolási modulja lehetővé teszi a felülvizsgálók kijelölését és a felhasználók és az alkalmazáson belüli engedélyek és fiókok közötti kapcsolatok ismétlődő felülvizsgálatának elvégzését.

## <a name="bhold-attestation-installation-requirements"></a>BHOLD-igazolás telepítési követelményei

Az BHOLD igazolási modul telepítése előtt telepítenie kell a BHOLD Core modult arra a kiszolgálóra, amelyre telepíteni kívánja a BHOLD igazolási modulját. Az BHOLD Core modul telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Mivel a BHOLD-igazolási modul névjegyei e-mail-üzeneteket küldenek a felhasználóknak, a környezetnek rendelkeznie kell egy Simple Mail Transfer Protocol (SMTP) levelezési kiszolgálóval, például a Microsoft Exchange Serverrel.

> [!IMPORTANT]
> Ha a BHOLD-jelentéskészítési és a BHOLD-igazolást is telepíti, a BHOLD-igazolás telepítése előtt telepítenie kell a BHOLD-jelentéskészítést.

## <a name="before-you-begin"></a>Előkészületek

Mielőtt megkezdené a BHOLD igazolási modul telepítését, elő kell készítenie, hogy a BHOLD-igazolási telepítővarázsló által a telepítés befejezéséhez szükséges információk meglegyenek. A következő munkalapon rögzítheti ezeket az adatokat, így készen áll arra, hogy szükség esetén megadja azt.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Biztonsági szolgáltató használata tartományon vagy gépen** | Ha be van jelölve, akkor a Active Directory tartományi szolgáltatások biztonság szabályozza a BHOLD mag elérését.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** Ha ez a jelölőnégyzet nincs bejelölve, a telepítés sikertelen lesz.                                                                                                                                                                                                                   |
| **Tartomány**                                  | A BHOLD Core telepítésekor létrehozott szolgáltatásfiókot tartalmazó tartományt adja meg. További információ: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló automatikusan megadja. A név csak akkor módosítható, ha helytelen. **Fontos:** A tartománynevet a NetBIOS (rövid) név használatával adja meg, ne a teljes tartománynevet (FQDN). Ha például a tartomány teljes tartományneve fabrikam.com, adja meg a tartománynevet FABRIKAM néven. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatás felhasználói fiókjának bejelentkezési nevét.                                                                                                                                                          | Írja be a felhasználói fiók nevét:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | Megadja a szolgáltatás felhasználói fiókjának jelszavát.                                                                                                                                                                       | Írja be a jelszót itt: **Fontos:** ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>BHOLD-igazolás telepítése

A BHOLD-igazolási modul telepítéséhez jelentkezzen be a Tartománygazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként azon a kiszolgálón, amelyen telepíteni kívánja az BHOLD igazolási modulját:

- A BholdAttestation<em>\<verziója\></em>\_Release. msi

Cserélje le *\<version\>* a telepítendő BHOLD-igazolási kiadás verziószámára.

A programfájl rendszergazdaként való futtatásához kattintson a jobb gombbal a fájlra, majd kattintson a **Futtatás rendszergazdaként**parancsra.

## <a name="next-steps"></a>További lépések

- [BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

---
title: BHOLD igazolás telepítési |} Microsoft Docs
description: BHOLD igazolás modul lehetővé teszi, hogy kijelölje felülvizsgálók, és végezze el az értékelést
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 6838355c05f7c19436d8a83839044ea5f4e2533d
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290339"
---
# <a name="bhold-attestation-installation"></a>BHOLD-tanúsítvány telepítése

A BHOLD igazolás modul lehetővé teszi a felülvizsgálók kijelölni, és végezze el az ismétlődő értékelést, a felhasználók és alkalmazás engedélyek és fiókok közötti kapcsolatokat.

## <a name="bhold-attestation-installation-requirements"></a>BHOLD igazolás telepítési követelmények

Az igazolás BHOLD-modul telepítése előtt telepítenie kell a BHOLD Alap modulban a kiszolgáló, amelyen a BHOLD igazolás modul telepítését tervezi. A BHOLD Alap modulban telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). BHOLD igazolás modul névjegyek küld e-mail-üzenetek a felhasználók számára, mert a környezet egy Simple Mail Transfer Protocol (SMTP) e-mail kiszolgálóhoz, például a Microsoft Exchange Server kell rendelkeznie.

> [!IMPORTANT]
> BHOLD jelentéskészítési és BHOLD igazolás telepítésekor, telepítenie kell BHOLD Reporting BHOLD tanúsítvány telepítése előtt.

## <a name="before-you-begin"></a>Előkészületek

BHOLD igazolás moduljának telepítése előtt kell készüljön fel a BHOLD igazolás telepítővarázslója a telepítés befejezéséhez szükséges információk. A következő munkalapra jegyezze fel ezt az információt, készen áll, hogy szükség esetén nyújt segítséget.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztásakor határozza meg, hogy az Active Directory tartományi szolgáltatások biztonsági szabályozza a BHOLD Core elérésére.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                                  | Megadja azt a BHOLD központi telepítésekor létrehozott szolgáltatási fiókot tartalmazó tartományt. További információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A varázsló automatikusan rendelkezik a tartomány nevét. Módosítsa a nevét, ha az nem megfelelő. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-nevét, nem a teljes tartománynevét (FQDN) használatával. Például fabrikam.com esetén a a teljes Tartománynevét adja meg a tartománynév FABRIKAM. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                          | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | A szolgáltatás felhasználói fiók jelszavát adja meg.                                                                                                                                                                       | A jelszó itt írási: **fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>BHOLD-tanúsítvány telepítése

BHOLD igazolás-modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD igazolás modul telepítése:

- BholdAttestation<em>\<verzió\></em>\_Release.msi

Cserélje le *\<verzió\>* rendelkező a telepíteni kívánt BHOLD igazolás kiadás verziószáma.

A program fájlt rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

## <a name="next-steps"></a>További lépések

- [BHOLD a telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)
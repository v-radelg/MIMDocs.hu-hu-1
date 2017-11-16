---
title: "BHOLD elemzési telepítési |} Microsoft Docs"
description: "BHOLD elemzési modul adja meg az adat-hozzáférési szabály alapú tesztelése"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 631e08667e5d1535d8f63cc297aad360080f8b20
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-analytics-installation"></a>BHOLD elemzési telepítése

A BHOLD elemzési a modul adja meg annak érdekében, hogy a szervezet képes hatékonyan az adatok hozzáférésének vezérléséhez, és megfelel a belső és külső hozzáférési követelmények adat-hozzáférési szabály alapú tesztelését. Az automatizált hatáselemzés BHOLD elemzési modul által előállított áttekintést nyújt a felhasználók számát, akik a javasolt szabály végrehajtásának befolyásolhat mindkét rendelkező volna felel meg a szabály és azokra, akik a szabály sértené megjelenítő. A BHOLD elemzési modul a felhasználókat, akik volna megfelelnek-e a szabály és azok, akik a szabály sértené részletes listáját is megadhatja.

## <a name="bhold-analytics-installation-requirements"></a>BHOLD elemzési telepítési követelmények

A BHOLD elemzési-modul telepítése előtt telepítenie kell a BHOLD Alap modulban a kiszolgáló, amelyen a BHOLD elemzési modul telepítését tervezi. A BHOLD Alap modulban telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Előkészületek

BHOLD elemzési moduljának telepítése előtt kell készüljön fel a BHOLD elemzési telepítővarázslója a telepítés befejezéséhez szükséges információk. A következő munkalapra jegyezze fel ezt az információt, készen áll, hogy szükség esetén nyújt segítséget.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztásakor határozza meg, hogy az Active Directory tartományi szolgáltatások biztonsági szabályozza a BHOLD Core elérésére.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                                  | Megadja azt a BHOLD központi telepítésekor létrehozott szolgáltatási fiókot tartalmazó tartományt. További információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | A varázsló automatikusan rendelkezik a tartomány nevét. Módosítsa a nevét, ha az nem megfelelő. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-nevét, nem a teljes tartománynevét (FQDN) használatával. Például fabrikam.com esetén a a teljes Tartománynevét adja meg a tartománynév FABRIKAM. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                          | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | A szolgáltatás felhasználói fiók jelszavát adja meg.                                                                                                                                                                       | A jelszó itt írási: **fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>BHOLD elemzési telepítése

BHOLD elemzési-modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD elemzési modul telepítése:

- BholdAnalytics*\<verzió\>*\_Release.msi

Cserélje le  *\<verzió\>*  rendelkező a telepíteni kívánt BHOLD elemzési kiadás verziószáma.

A program fájlt rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

# <a name="next-steps"></a>További lépések

- [BHOLD Core telepítés](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)
- [BHOLD a telepítési útmutató](bhold-installation-guide.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)
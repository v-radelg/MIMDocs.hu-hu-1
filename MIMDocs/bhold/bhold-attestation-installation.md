---
title: A BHOLD attestation telepítése |} A Microsoft Docs
description: BHOLD igazolási modul lehetővé teszi a felülvizsgálók kijelölni, és hajtsa végre az értékelések
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 45b650c726ddf423bb6ebc5ec34d4a57e41bb82e
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334125"
---
# <a name="bhold-attestation-installation"></a>A BHOLD attestation telepítése

A BHOLD igazolási modul teszi lehetővé teszik a felülvizsgálók kijelölni, és végezze el a felhasználók és az alkalmazásonkénti engedélyeket és a fiókok közötti kapcsolatokat ismétlődő felülvizsgálatai.

## <a name="bhold-attestation-installation-requirements"></a>A BHOLD igazolási telepítési követelményei

A BHOLD igazolási modul a telepítés előtt telepítenie kell a BHOLD Core-modul a kiszolgálón, amelyen a BHOLD igazolási modul telepítését tervezi. A BHOLD Core modul telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). A BHOLD igazolási modul névjegyek küld e-mail-üzenetek a felhasználók számára, mert a környezet egy Simple Mail Transfer Protocol (SMTP) levelezési kiszolgáló, mint a Microsoft Exchange Server kell rendelkeznie.

> [!IMPORTANT]
> A BHOLD Reporting és BHOLD igazolási telepítésekor, telepítenie kell a BHOLD Reporting BHOLD igazolási telepítése előtt.

## <a name="before-you-begin"></a>Előkészületek

A BHOLD igazolási modul telepítése előtt kell a BHOLD igazolási telepítővarázslója a telepítés befejezéséhez szükséges információk megadására. A következő munkalap segítségével fel ezt az információt, hogy készen áll a szükséges megadni.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztott, itt adhatja meg, hogy az Active Directory Domain Services biztonsági fog férhet hozzá a BHOLD Core.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                                  | Megadja azt a tartományt, amely tartalmazza a szolgáltatásfiókhoz, amelyet a BHOLD Core telepítése során létrehozott. További információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló által automatikusan történik. Módosítsa a nevét, csak akkor, ha annak értéke helytelen. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-neve, nem a teljesen minősített tartománynevét (FQDN) használatával. Például ha a tartomány teljes Tartománynevét a fabrikam.com, adja meg a tartomány nevét, a FABRIKAM. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                          | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | A szolgáltatás felhasználói fiók jelszava.                                                                                                                                                                       | Írás a jelszót kell megadnia: **fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>A BHOLD Attestation telepítése

BHOLD igazolási-modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájl, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD igazolási modul telepítése:

- BholdAttestation<em>\<verzió\></em>\_Release.msi

Cserélje le *\<verzió\>* a BHOLD igazolási kiadás, amely telepíti a verziószámával.

A program rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

## <a name="next-steps"></a>További lépések

- [A BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)
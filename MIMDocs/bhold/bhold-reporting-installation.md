---
title: BHOLD reporting telepítésére |} Microsoft Docs
description: BHOLD Jelentéskezelő modul lehetővé teszi a szerepkörök és engedélyezési házirendeket vonatkozó jelentések létrehozásához
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 2b8e6935eda6c679b00b5b5b17752a675a257de9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/25/2018
---
# <a name="bhold-reporting-installation"></a>BHOLD reporting telepítésére

A BHOLD Jelentéskezelő modul lehetővé teszi a szerepkörök és a többi engedélyezési házirendek BHOLD létrehozni. Ezek a jelentések gyakran hasznosak, a naplózás vagy a megfelelőség szabályozási követelmények a bemutatásához. Ez a modul is, mivel szerepköreik tagsága elemzéséhez szükséges információt a felhasználók számára a szervezeten belüli fiókonkénti arra a képességére, terjeszti ki. A jelentések is korlátozott nézeteket, győződjön meg arról, hogy a felhasználókat, akik jelentések jelennek meg információk csak azok csatlakozhatnának a megtekintéséhez.

## <a name="bhold-reporting-installation-requirements"></a>BHOLD-jelentéskészítés telepítési követelmények

A BHOLD Jelentéskezelő modul telepítése, előtt telepítenie kell a BHOLD Alap modulban a kiszolgáló, amelyen a BHOLD Jelentéskezelő modul telepítésének megtervezése. A BHOLD Alap modulban telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

>[!IMPORTANT]
BHOLD jelentéskészítési és BHOLD igazolás telepítésekor, telepítenie kell BHOLD Reporting BHOLD tanúsítvány telepítése előtt.

## <a name="before-you-begin"></a>Előkészületek

A BHOLD Jelentéskezelő modul telepítésének megkezdése előtt kell előkészíteni az adatok a BHOLD Reporting telepítővarázsló számára szükséges a telepítés befejezéséhez. A következő munkalapra jegyezze fel ezt az információt, készen áll, hogy szükség esetén nyújt segítséget.

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztásakor határozza meg, hogy az Active Directory tartományi szolgáltatások biztonsági szabályozza a BHOLD Core elérésére.                                                                                                                | Jelölje be a jelölőnégyzetet. </br>**Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                                  | Megadja azt a BHOLD központi telepítésekor létrehozott szolgáltatási fiókot tartalmazó tartományt. További információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A varázsló automatikusan rendelkezik a tartomány nevét. Módosítsa a nevét, ha az nem megfelelő. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-nevét, nem a teljes tartománynevét (FQDN) használatával. Például fabrikam.com esetén a a teljes Tartománynevét adja meg a tartománynév FABRIKAM. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                          | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | A szolgáltatás felhasználói fiók jelszavát adja meg.                                                                                                                                                                       | Írási ide: </br>**Fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>BHOLD Reporting telepítése

A BHOLD Jelentéskezelő modul telepítésének, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD Jelentéskezelő modul telepítése:

- BholdReporting*\<verzió\>*\_Release.msi

Cserélje le *\<verzió\>* rendelkező a telepíteni kívánt BHOLD Reporting kiadás verziószáma.

A program fájlt rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

## <a name="next-steps"></a>További lépések

- [BHOLD a telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)
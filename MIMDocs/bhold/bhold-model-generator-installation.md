---
title: BHOLD modellhez generátor telepítési |} Microsoft Docs
description: BHOLD modell lehetővé teszi a struktúra adatokat különböző forrásokból
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 3d45d18042ccda83873aa929101222c15f36246a
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/25/2018
---
# <a name="bhold-model-generator-installation"></a>BHOLD modellhez generátor telepítése

BHOLD modellhez generátor modul használatával, amely tartalmazza a felhasználói és szervezeti adatait és hozzáférés-vezérlési listák (ACL) a modellbe a BHOLD felügyeletéhez használt mérvadó forrásból származó adatokat is struktúra.

## <a name="bhold-model-generator-installation-requirements"></a>BHOLD modellhez generátor telepítési követelmények 

A modellhez generátor BHOLD-modul telepítése előtt telepítenie kell a következő:

1. BHOLD Alap modulban a kiszolgálón, amelyen a BHOLD modellhez generátor modul telepítését tervezi. A BHOLD Alap modulban telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. A Microsoft OLE DB Provider for Microsoft Jet telepítve kell lennie. További információ: [Ez a cikk](http://support.microsoft.com/kb/271908).

>[!WARNING]
BHOLD modellhez generátor ne telepítse a termelési hálózat. BHOLD modellhez generátor offline tesztelési környezetben létrehozására szolgál, amelyet importálhat a vállalati szerepkör modellbe normalizált szerepkör modell célja. Az éles hálózattól BHOLD modellhez generátor fut a meglévő szerepkör-modell elvesztését eredményezheti.

## <a name="before-you-begin"></a>Előkészületek

A modellhez generátor BHOLD-modul telepítése előtt kell előkészíteni a BHOLD modellhez generátor telepítővarázslója a telepítés befejezéséhez szükséges információk biztosítása érdekében. A következő munkalapra jegyezze fel ezt az információt, készen áll, hogy szükség esetén nyújt segítséget. Győződjön meg arról is kell

Microsoft Access-adatbázis Engine 2010 újraterjeszthető csomag

 

*A \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Fiókbeállítások**

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztásakor határozza meg, hogy az Active Directory tartományi szolgáltatások biztonsági szabályozza a BHOLD Core elérésére.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                                  | Megadja azt a BHOLD központi telepítésekor létrehozott szolgáltatási fiókot tartalmazó tartományt. További információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A varázsló automatikusan rendelkezik a tartomány nevét. Módosítsa a nevét, ha az nem megfelelő. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-nevét, nem a teljes tartománynevét (FQDN) használatával. Például fabrikam.com esetén a a teljes Tartománynevét adja meg a tartománynév FABRIKAM. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                          | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | A szolgáltatás felhasználói fiók jelszavát adja meg.                                                                                                                                                                       | A jelszó itt írási: **fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

**A Backup database beállítások**

| Elem                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                  | Érték                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beépített biztonság használatára**                 | Meghatározza, hogy az adatbázis eléréséhez használt-e a Windows-hitelesítést.                                                                                                                                                                                                                                                                                                                                                        | Jelölje be a jelölőnégyzetet, ha Windows-hitelesítést használ az SQL-kiszolgálóhoz való csatlakozáshoz. Törölje a jelet a jelölőnégyzetből, ha az SQL Server-hitelesítés használata. Az adatbázist kell létrehozni futtató BHOLD Core telepítés Ha SQL Server-hitelesítés használata előtt. **Megjegyzés:** Windows-hitelesítés használata esetén meg kell bejelentkeznie egy olyan fiókkal, amely a sysadmin (rendszergazda) kiszolgálói szerepkörrel rendelkezik az adatbázis-kiszolgálón. **Fontos:** használható SQL Server-hitelesítés csak tesztelési környezetben. A Microsoft azt javasolja, a Windows-hitelesítés az éles környezetekben. |
| **Adatbázis-felhasználót** és **adatbázis-jelszó** | A felhasználónevet és egy felhasználó jelszavát adja meg a sysadmin (rendszergazda) kiszolgálói szerepkör az adatbázis-kiszolgálón. Ezek az értékek megadva, csak az SQL Server-hitelesítés használata esetén.                                                                                                                                                                                                                                                  | A SQL Server felhasználói nevet itt: írása az SQL Server felhasználói jelszó itt: </br></br> **Fontos:** mindenképp ezt a jelszót rejtett, biztonságos helyen.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Adatbázis-kiszolgáló** és **adatbázis neve**   | Az adatbázis-kiszolgáló NetBIOS-nevét és az adatbázis biztonsági másolatának BHOLD modellhez generátor a telepítő létrehozza a nevét adja meg. Ha nem használ az alapértelmezett adatbázis-kiszolgálópéldányra, adja meg az adatbázis-kiszolgálópéldányra formájában  *\<server\>*\\*\<példány\>* .  A Microsoft azt javasolja, hogy elnevezni az adatbázis biztonsági másolatának használata a BHOLD Core-adatbázis leendő nevét \_biztonsági másolat, például B1_BACKUP. | A kiszolgáló (vagy a kiszolgáló és példány) neve itt írási: </br> Írás az adatbázisnevet:

## <a name="bhold-model-generator-setup"></a>BHOLD modellhez generátor beállítása

BHOLD modellhez generátor-modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD Core modul telepítése:

- BholdModelGenerator  *\<verzió\>*\_Release.msi

Cserélje le *\<verzió\>* rendelkező a telepíteni kívánt BHOLD modellhez generátor kiadás verziószáma.

A program fájlt rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

## <a name="next-steps"></a>További lépések

- A bemeneti fájlok létrehozásával kapcsolatos információkat [Microsoft BHOLD Suite műszaki útmutatója](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [BHOLD a telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)
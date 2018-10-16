---
title: A BHOLD model generator telepítése |} A Microsoft Docs
description: A BHOLD díjszabási modellje lehetővé teszi struktúra adatok különböző forrásokból
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: ddb49219b5f68ff060f9b15a9ab64cb85a035d98
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333479"
---
# <a name="bhold-model-generator-installation"></a>A BHOLD Model Generator telepítése

A BHOLD Model Generator-modul segítségével hogyan strukturálhatja a mérvadó forrásokból származó adatokat, amely tartalmazza a felhasználói és szervezeti adatai hozzáférés-vezérlési listák (ACL) együtt használható a BHOLD felügyelete a modellbe.

## <a name="bhold-model-generator-installation-requirements"></a>A BHOLD Model Generator telepítési követelményei 

A BHOLD Model Generator modul a telepítés előtt telepítenie kell a következőket:

1. A BHOLD Core-modul a kiszolgálón, amelyen a BHOLD Model Generator modul telepítését tervezi. A BHOLD Core modul telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. A Microsoft OLE DB Provider for Microsoft Jet telepítve kell lennie. További információ: [Ez a cikk](http://support.microsoft.com/kb/271908).

> [!WARNING]
> A BHOLD Model Generator ne telepítse a termelési hálózat. A BHOLD Model Generator célja használandó kapcsolat nélküli átmeneti környezetben, amelyet importálhat a vállalati szerepkör modellbe normalizált szerepkör modell létrehozása. Az éles hálózattól BHOLD Model Generator futó a meglévő szerepkör-modell adatvesztést eredményezhet.

## <a name="before-you-begin"></a>Előkészületek

A BHOLD Model Generator modul a telepítés előtt kell a BHOLD Model Generator telepítő varázsló a telepítés befejezéséhez szükséges információk megadására. A következő munkalap segítségével fel ezt az információt, hogy készen áll a szükséges megadni. Győződjön meg arról is kell

Microsoft Access 2010 adatbázismotor terjeszthető adatbázis

 

*A \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Fiókbeállítások**

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tartomány/gépen biztonsági szolgáltató használata** | Kiválasztott, itt adhatja meg, hogy az Active Directory Domain Services biztonsági fog férhet hozzá a BHOLD Core.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** a telepítés sikertelen lesz, ha a jelölőnégyzet nincs bejelölve.                                                                                                                                                                                                                   |
| **Tartomány**                                  | Megadja azt a tartományt, amely tartalmazza a szolgáltatásfiókhoz, amelyet a BHOLD Core telepítése során létrehozott. További információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló által automatikusan történik. Módosítsa a nevét, csak akkor, ha annak értéke helytelen. **Fontos:** adja meg a tartomány nevét (rövid) NetBIOS-neve, nem a teljesen minősített tartománynevét (FQDN) használatával. Például ha a tartomány teljes Tartománynevét a fabrikam.com, adja meg a tartomány nevét, a FABRIKAM. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatásfiók-felhasználó bejelentkezési nevét.                                                                                                                                                          | Írás a felhasználói fiók nevét itt:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | A szolgáltatás felhasználói fiók jelszava.                                                                                                                                                                       | Írás a jelszót kell megadnia: **fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen.                                                                                                                                                                                                                  |

**Biztonsági mentési adatbázis-beállítások**

| Elem                                        | Leírás                                                                                                                                                                                                                                                                                                                                                                                                                  | Érték                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beépített biztonság használata**                 | Megadja, hogy Windows-hitelesítés az adatbázis eléréséhez.                                                                                                                                                                                                                                                                                                                                                        | Jelölje be a jelölőnégyzetet, ha a Windows-hitelesítés használata az SQL-kiszolgálóhoz való csatlakozáshoz. Törölje a jelölőnégyzet jelölését, az SQL Server-hitelesítés használata esetén. Az adatbázist kell létrehozni futtató BHOLD Core telepítés Ha SQL Server-hitelesítés van használata előtt. **Megjegyzés:** Ha Windows-hitelesítést használ, akkor jelentkezett be egy olyan fiókkal, amely a sysadmin (rendszergazda) kiszolgálói szerepkörrel rendelkezik az adatbázis-kiszolgálón. **Fontos:** használata SQL Server-hitelesítés csak tesztelési környezetben. A Microsoft azt javasolja, Windows-hitelesítés az éles környezetekben. |
| **Adatbázis-felhasználót** és **adatbázis-jelszó** | Adja meg a felhasználónevet és a egy felhasználó jelszavát a sysadmin (rendszergazda) kiszolgálói szerepkörrel a kiszolgálón. Ezek az értékek csak az SQL Server-hitelesítés használata esetén vannak megadva.                                                                                                                                                                                                                                                  | Az SQL Server felhasználói nevét itt írási: Itt az SQL Server felhasználói jelszót írni: </br></br> **Fontos:** mindenképp Ez a jelszó egy rejtett, biztonságos helyen.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Adatbázis-kiszolgáló** és **adatbázis neve**   | Az adatbázis-kiszolgáló NetBIOS-nevét és a biztonsági mentési adatbázis, amely a BHOLD Model Generator a telepítő létrehozza a nevét adja meg. Ha nem használ az alapértelmezett adatbázis-kiszolgálópéldányra, adja meg az adatbázis-kiszolgálópéldányra formájában  *\<kiszolgáló\>*\\*\<példány\>* .  A Microsoft javasolja, hogy nevezze el a használatával a BHOLD Core-adatbázis leendő nevét a backup database \_biztonsági MENTÉS, például B1_BACKUP. | A kiszolgáló (vagy a kiszolgálót és példányt) nevet itt írási: </br> Írási itt az adatbázis neve:

## <a name="bhold-model-generator-setup"></a>A BHOLD Model Generator beállítása

BHOLD Model Generator-modul telepítéséhez, jelentkezzen be a tartományi rendszergazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként a kiszolgálón, melyet a BHOLD Core-modul telepítése:

- BholdModelGenerator  *\<verzió\>*\_Release.msi

Cserélje le *\<verzió\>* a BHOLD Model Generator kiadás, amely telepíti a verziószámával.

A program rendszergazdaként futtatni, kattintson jobb gombbal a fájlra, és kattintson a **Futtatás rendszergazdaként**.

## <a name="next-steps"></a>További lépések

- Bemeneti fájlok létrehozásával kapcsolatos információkat [Microsoft BHOLD Suite – technikai útmutató](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [A BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)
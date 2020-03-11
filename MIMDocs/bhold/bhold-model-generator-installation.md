---
title: BHOLD Model Generator telepítése | Microsoft Docs
description: A BHOLD-modell lehetővé teszi különböző forrásokból származó adatok felépítését
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 6f7e0979246eb2124604f594c57b40ec11cc7140
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042016"
---
# <a name="bhold-model-generator-installation"></a>BHOLD Model Generator telepítése

A BHOLD Model Generator modul használatával az olyan mérvadó forrásokból származó adatokat, amelyek felhasználói és szervezeti adatokat tartalmaznak, valamint hozzáférés-vezérlési listákat (ACL-eket) is tartalmaz a BHOLD felügyeletéhez használható modellbe.

## <a name="bhold-model-generator-installation-requirements"></a>A BHOLD Model Generator telepítési követelményei 

A BHOLD Model Generator modul telepítése előtt telepítenie kell a következőket:

1. A BHOLD Core modul azon a kiszolgálón, amelyre telepíteni szeretné a BHOLD Model Generator-modult. Az BHOLD Core modul telepítésével kapcsolatos információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. A Microsoft Jet Microsoft OLE DB-szolgáltatóját telepíteni kell. További információkért tekintse meg [ezt a cikket](https://support.microsoft.com/kb/271908).

> [!WARNING]
> Ne telepítse a BHOLD Model Generatort az éles hálózatában. A BHOLD Model Generator a vállalati szerepkör-modellbe importálható normalizált szerepkör-modell létrehozásához az átmeneti környezetekben offline használatba vehető. A BHOLD Model Generator üzemi hálózatban való futtatása a meglévő szerepkör-modell elvesztését eredményezheti.

## <a name="before-you-begin"></a>Előkészületek

A BHOLD Model Generator modul telepítése előtt elő kell készítenie azokat az információkat, amelyeket a BHOLD-modell létrehozójának telepítővarázslója a telepítés befejezéséhez szükséges. A következő munkalapon rögzítheti ezeket az adatokat, így készen áll arra, hogy szükség esetén megadja azt. Emellett meg kell győződnie arról, hogy

Microsoft Access adatbázismotor 2010 Újraterjeszthető csomag

 

*\<* <http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>ról *\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Fiókbeállítások**

| **Elem**                                    | **Leírás**                                                                                                                                                                                                           | **Érték**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Biztonsági szolgáltató használata tartományon vagy gépen** | Ha be van jelölve, akkor a Active Directory tartományi szolgáltatások biztonság szabályozza a BHOLD mag elérését.                                                                                                                | Jelölje be a jelölőnégyzetet. **Fontos:** Ha ez a jelölőnégyzet nincs bejelölve, a telepítés sikertelen lesz.                                                                                                                                                                                                                   |
| **Tartomány**                                  | A BHOLD Core telepítésekor létrehozott szolgáltatásfiókot tartalmazó tartományt adja meg. További információ: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | A tartomány nevét a varázsló automatikusan megadja. A név csak akkor módosítható, ha helytelen. **Fontos:** A tartománynevet a NetBIOS (rövid) név használatával adja meg, ne a teljes tartománynevet (FQDN). Ha például a tartomány teljes tartományneve fabrikam.com, adja meg a tartománynevet FABRIKAM néven. |
| **Felhasználó**                                    | Megadja a BHOLD Core szolgáltatás felhasználói fiókjának bejelentkezési nevét.                                                                                                                                                          | Írja be a felhasználói fiók nevét:                                                                                                                                                                                                                                                                                    |
| **Jelszó**                                | Megadja a szolgáltatás felhasználói fiókjának jelszavát.                                                                                                                                                                       | Írja be a jelszót itt: **Fontos:** ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja.                                                                                                                                                                                                                  |

**Adatbázis biztonsági mentése – beállítások**

| Elem                                        | Leírás                                                                                                                                                                                                                                                                                                                                                                                                                  | Érték                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Beépített biztonság használata**                 | Megadja, hogy a rendszer a Windows-hitelesítést használja az adatbázis eléréséhez.                                                                                                                                                                                                                                                                                                                                                        | Jelölje be a jelölőnégyzetet, ha Windows-hitelesítést használ a SQL Serverhoz való kapcsolódáshoz. Törölje a jelölőnégyzet jelölését, ha SQL Server hitelesítést használ. Ha SQL Server hitelesítést használ, az adatbázist létre kell hozni az BHOLD Core telepítőjének futtatása előtt. **Megjegyzés:** Windows-hitelesítés használata esetén olyan fiókkal kell bejelentkeznie, amely a sysadmin kiszolgálói szerepkörrel rendelkezik az adatbázis-kiszolgálón. **Fontos:** Csak tesztelési környezetekben használjon SQL Server hitelesítést. A Microsoft nyomatékosan javasolja a Windows-hitelesítés használatát az éles környezetben. |
| **Adatbázis-felhasználó** és **adatbázis jelszava** | A sysadmin kiszolgálói szerepkörrel rendelkező felhasználó felhasználónevét és jelszavát adja meg az adatbázis-kiszolgálón. Ezeket az értékeket csak akkor kell megadni, ha SQL Server hitelesítés van használatban.                                                                                                                                                                                                                                                  | Írja be a SQL Server felhasználónevet: írja be ide a SQL Server felhasználói jelszavát: </br></br> **Fontos:** Ügyeljen arra, hogy a jelszót rejtett, biztonságos helyen tárolja.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Adatbázis-kiszolgáló** és **adatbázis neve**   | Megadja az adatbázis-kiszolgáló NetBIOS-nevét és annak a biztonsági mentési adatbázisnak a nevét, amelyet a BHOLD Model Generator telepítője fog létrehozni. Ha nem az alapértelmezett adatbázis-kiszolgálói példányt használja, akkor az adatbázis-kiszolgáló példányát az űrlapon *\<server\>* \\ *\<példány\>* .  A Microsoft azt javasolja, hogy a biztonsági mentési adatbázist a BHOLD Core-adatbázis nevével nevezze el, majd \_biztonsági mentést, például B1_BACKUP. | Írja be a kiszolgáló (vagy a kiszolgáló és a példány) nevét a következőre: </br> Írja be az adatbázis nevét:

## <a name="bhold-model-generator-setup"></a>BHOLD Model Generator telepítése

A BHOLD Model Generator modul telepítéséhez jelentkezzen be a Tartománygazdák csoport tagjaként, töltse le a következő fájlt, és futtassa rendszergazdaként azon a kiszolgálón, amelyre telepíteni kívánja a BHOLD Core modult:

- A BholdModelGenerator *\<verziója\>* \_Release. msi

Cserélje le a *\<version\>* -et a telepítendő BHOLD Model Generator-kiadás verziószámára.

A programfájl rendszergazdaként való futtatásához kattintson a jobb gombbal a fájlra, majd kattintson a **Futtatás rendszergazdaként**parancsra.

## <a name="next-steps"></a>További lépések

- A bemeneti fájlok létrehozásáról a [Microsoft BHOLD Suite technikai útmutatója](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx) című témakörben olvashat bővebben.
- [BHOLD telepítési útmutató](bhold-installation-guide.md)
- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

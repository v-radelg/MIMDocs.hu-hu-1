---
title: "Mi az a hibrid jelentéskészítés az Azure AD? | Microsoft Docs"
description: "Hibrid naplózási Tevékenységjelentések az Azure Active Directoryban lehetővé teszi a felhőben és a helyszíni naplózott események megtekintése."
keywords: 
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: eb9725df484fb5ac2ee44bd9a0423bdb4fbe7e86
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/20/2018
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Hibrid Identitáskezelés naplózási jelentéskészítés az Azure Active Directory-ban
Az Azure Active Directory (Azure AD) naplózási Tevékenységjelentés, figyelheti identitáskezelési tevékenységei helyi vagy a felhőben. A identitások és hozzáférések adatokat egyetlen kimutatásban kezelésével, időt takaríthat meg, és csökkenteni szeretné költségeit.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Mi az az Azure Active Directory hibrid jelentéskészítés?
Hibrid naplózási jelentéskészítési nyújt segítséget az informatikusok közös identitás-kezelési jelentési kihívásokkal, például cím:

* **Identitáskezelési tevékenységek gyűjtése a különböző rendszerekből**. A hibrid jelentéseken az Azure AD és az Identity Manager identitáskezelési tevékenységei egyaránt megjelennek.

* **Jelentés adatainak és egyéni jelentések készítése exportálása**. Jelentések nemcsak megtekinthetők az Azure-portálon, exportálhatja az adatokat saját egyéni nézetek létrehozása céljából.

* **A jelentéskészítő rendszer infrastrukturális költségeinek csökkentése**. A hibrid jelentéskészítés a felhő azt jelenti, hogy a költségek a helyszíni, az adatraktár infrastruktúra társított kiiktathatja.

## <a name="how-does-it-work"></a>Hogyan működik?

A helyszíni adatok gyűjtéséhez először telepítenie kell egy jelentéskészítő ügynököt az Identity Manager 2016-kiszolgálón. [A Microsoft Identity Manager hibrid jelentéskészítő ügynök letöltése](https://www.microsoft.com/download/details.aspx?id=55112).

A hibrid jelentéskészítés a következő folyamat megy keresztül:
1. A jelentéskészítő ügynök telepítését követően az Identity Manager-tevékenységek adatait a Windows eseménynaplóban történő küldött.
2. A jelentéskészítő ügynök feldolgozza a különbözeti események, 10 percenként vagy a Windows Eseménynapló szolgáltatás újraindításakor. Az ügynök majd feltölti az eseményeket az Azure-portálon.
3. Az Azure-portálon fogadni egy órán belül dolgozza fel a fogadott adatokhoz.
4. A tevékenységek adatai egy hónapig tárolódnak az Azure-ban.
5. Az Azure-portálon lekéri a jelentés adatainak naplózás, és megjeleníti azt az Azure naplózási Reporting ablakban.

## <a name="next-steps"></a>További lépések
További információk az alábbiakról:
- [Hibrid jelentéskészítés az Identity Managerben használata](working-with-identity-manager-hybrid-reporting.md)
- [Naplózási Tevékenységjelentések az Azure Active Directory portálon](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Jelentéskészítési adatmegőrzési](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [A Microsoft Azure napló integrációs (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Az Azure Active Directory reporting API-val](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)

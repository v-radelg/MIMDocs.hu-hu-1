---
title: "Mi az a hibrid jelentéskészítés az Azure AD? | Microsoft Docs"
description: "Hibrid naplózási Tevékenységjelentések az Azure Active Directoryban lehetővé teszi a felhőben és a helyszíni naplózott események megtekintése."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: e2391be3d05f61335c134c104673a31ad7fc3830
ms.sourcegitcommit: 3d8a2493eae1218bfdb75a399ffa4adc8c2a8fdf
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/20/2018
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory-public-preview-refresh"></a>Hibrid Identitáskezelés naplózni az Azure Active Directory Public Preview frissítési jelentés
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

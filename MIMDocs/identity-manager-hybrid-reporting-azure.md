---
title: Mi a hibrid jelentéskészítés az Azure AD-ben? | Microsoft Docs
description: A hibrid naplózási tevékenységek jelentései a Azure Active Directory lehetővé teszi a naplózott események megtekintését mind a felhőben, mind a helyszínen.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: 6f4f2aea998fc5682d1fb21d77e4d4f1c582d770
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042151"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Hibrid Identitáskezelés naplózási jelentéskészítése Azure Active Directory
Az Azure Active Directory (Azure AD) naplózási tevékenységgel kapcsolatos jelentéskészítéssel a helyszínen vagy a felhőben is figyelheti az Identitáskezelés tevékenységeit. Az összes identitásának és hozzáférésének egyetlen jelentésben való kezelésével időt takaríthat meg, és csökkentheti az általános költségeket.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Mi az az Azure Active Directory hibrid jelentéskészítés?
A hibrid naplózási jelentéskészítés segít az IT-szakembereknek az identitás-felügyeleti jelentéskészítési kihívásokkal foglalkozni, például:

* Az **Identitáskezelés különböző rendszereken keresztüli összegyűjtése**. A hibrid jelentéseken az Azure AD és az Identity Manager identitáskezelési tevékenységei egyaránt megjelennek.

* **Jelentéskészítési adatexportálás és egyéni jelentések létrehozása**. A jelentéseinek a Azure Portalban való megtekintése mellett az adatok exportálásával is létrehozhatja saját egyéni nézeteit.

* A **jelentési rendszer infrastrukturális költségeit**. A felhőben a hibrid jelentések segítségével elkerülheti a helyszíni, adattárház-infrastruktúrához kapcsolódó költségeket.

## <a name="how-does-it-work"></a>Hogyan működik?

A helyszíni adatok gyűjtéséhez először telepítenie kell egy jelentéskészítő ügynököt az Identity Manager 2016-kiszolgálón. [Töltse le a Microsoft Identity Manager Hybrid Reporting Agent ügynököt](https://www.microsoft.com/download/details.aspx?id=55112).

A hibrid jelentéskészítés a következő folyamattal rendelkezik:
1. A jelentéskészítő ügynök telepítése után a rendszer elküldi az Identity Manager tevékenységének adatait a Windows-eseménynaplóba.
2. A jelentéskészítő ügynök 10 percenként dolgozza fel a különbözeti eseményeket, vagy a Windows Eseménynapló szolgáltatás újraindításakor. Az ügynök ezután feltölti az eseményeket a Azure Portalba.
3. A Azure Portal egy órán belül feldolgozza a kapott adatmennyiséget.
4. A tevékenységek adatai egy hónapig tárolódnak az Azure-ban.
5. A Azure Portal lekéri a naplózási jelentési adatait, és megjeleníti azt az Azure audit jelentési ablakában.

## <a name="next-steps"></a>További lépések
További információk az alábbiakról:
- [A hibrid jelentéskészítés használata az Identity Managerben](working-with-identity-manager-hybrid-reporting.md)
- [Tevékenység-jelentések naplózása a Azure Active Directory portálon](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Jelentéskészítési adatmegőrzési szabályzatok](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure log Integration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory jelentési API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)

---
title: Mi a hibrid jelentéskészítés az Azure ad-ben? | Microsoft Docs
description: A hibrid naplózási tevékenységre vonatkozó jelentések az Azure Active Directory lehetővé teszi a felhőben és a helyszíni naplózott események megtekintését.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: dd87f00fb3faded60671a47a0ba1dab7e4c2a531
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358176"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Hibrid identitáskezelési naplózási jelentéskészítés az Azure Active Directoryban
Az Azure Active Directory (Azure AD) naplózási tevékenység jelentéskészítési, figyelheti végzett identitáskezelési tevékenységeket a helyszínen vagy a felhőben. Mivel kezeli az összes identitás- és hozzáférés adatát egyetlen jelentésben, időt, és csökkentheti a teljes költségeket.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Mi az az Azure Active Directory hibrid jelentéskészítés?
Hibrid naplózási jelentési révén informatikai szakemberek számára fontos kihívásaira általános identitás-kezelési jelentéskészítési, például:

* **Identitáskezelési tevékenységek gyűjtése a különböző rendszerekből**. A hibrid jelentéseken az Azure AD és az Identity Manager identitáskezelési tevékenységei egyaránt megjelennek.

* **Exportálás vonatkozó adatokról szóló jelentéseket és egyéni jelentések készítése**. A jelentések megtekintése az Azure Portalon, mellett exportálhatja az adatokat a saját egyéni nézeteket hozhat létre.

* **A jelentéskészítő rendszer infrastrukturális költségeinek csökkentése**. Hibrid jelentéskészítés a felhő az azt jelenti, hogy a költségek a helyszíni, adattárház-infrastruktúra társított kiiktathatja.

## <a name="how-does-it-work"></a>Hogyan működik?

A helyszíni adatok gyűjtéséhez először telepítenie kell egy jelentéskészítő ügynököt az Identity Manager 2016-kiszolgálón. [Töltse le a Microsoft Identity Manager hibrid jelentéskészítő ügynök](https://www.microsoft.com/download/details.aspx?id=55112).

A hibrid jelentéskészítés a következő folyamat megy keresztül:
1. Miután telepítette a jelentéskészítő ügynök, az Identity Manager-tevékenységek adatait a Windows Eseménynapló küldött.
2. A jelentéskészítő ügynök feldolgozza a változási eseményeket 10 percenként vagy a Windows Eseménynapló szolgáltatás újraindításakor. Az ügynök ezután feltölti az eseményeket az Azure Portalon.
3. Az Azure Portalon a fogadott adatokat fogadni egy órán belül dolgozza fel.
4. A tevékenységek adatai egy hónapig tárolódnak az Azure-ban.
5. Jelentési adatok az Azure Portalon, és megjeleníti azt az Azure naplózási jelentési ablakban.

## <a name="next-steps"></a>További lépések
További információk az alábbiakról:
- [Hibrid jelentéskészítés az Identity Managerben használata](working-with-identity-manager-hybrid-reporting.md)
- [Naplózási tevékenységre vonatkozó jelentések az Azure Active Directory portálon](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Jelentéskészítési adatmegőrzési házirendek](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [A Microsoft Azure log integration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Az Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)

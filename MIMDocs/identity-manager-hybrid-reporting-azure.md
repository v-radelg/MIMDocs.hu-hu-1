---
title: "Mi az a hibrid jelentéskészítés? | Microsoft Docs"
description: "Az Azure Active Directory hibrid naplózási jelentéseivel a felhőbeli és a helyszíni naplózott eseményeket egyaránt áttekintheti."
keywords: 
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 678626e7c32659570de88d8178c16821cceaf7ee
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh"></a>Hibrid identitáskezelési naplójelentések az Azure Active Directoryban – nyilvános előzetes verzió (frissítés)
Az Azure Active Directory (AD) naplójelentéseinek segítségével egyetlen jelentésben nyomon követheti a helyszínen vagy a felhőben végzett identitáskezelési tevékenységeket. Ez a funkció lehetővé teszi, hogy az összes identitását egy helyen kezelje, és egy helyen férjen hozzá adataihoz, így időt takaríthat meg és csökkentheti a teljes költségeket.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Mi az az Azure Active Directory hibrid jelentéskészítés?
A hibrid naplózási jelentéskészítés segít a rendszergazdáknak az identitáskezeléshez kapcsolódó általános jelentéskészítési feladatok elvégzésében.

1. **Identitáskezelési tevékenységek gyűjtése a különböző rendszerekből.** A hibrid jelentéseken az Azure AD és az Identity Manager identitáskezelési tevékenységei egyaránt megjelennek.

2. **Jelentési adatok exportálása és egyéni jelentések készítése.** Jelentéseit az Azure-portálon is megtekintheti, és exportálhatja is az adatokat saját egyéni nézetek létrehozása céljából.

3. **A jelentéskészítő rendszer infrastrukturális költségeinek csökkentése.** A felhőbeli hibrid jelentéskészítéssel felszámolhatja a helyszíni jelentéskészítési adattárház-infrastruktúrát.

## <a name="how-does-it-work"></a>Hogyan működik?

A helyszíni adatok gyűjtéséhez először telepítenie kell egy jelentéskészítő ügynököt az Identity Manager 2016-kiszolgálón. A jelentéskészítő ügynököt a Microsoft letöltési oldaláról [itt](https://www.microsoft.com/en-us/download/details.aspx?id=55112) töltheti le.

A hibrid jelentéskészítés folyamata a következő lépésekből áll:
1. A jelentéskészítő ügynök a telepítést követően az Identity Manager-tevékenységek adatait a Windows Eseménynaplóba exportálja.
2. A jelentéskészítő ügynök 10 percenként vagy a szolgáltatás újraindításakor feldolgozza a változási eseményeket a Windows Eseménynaplóban, és feltölti őket az Azure Portal webhelyre.
3. Az Azure Portal a beérkezéstől számított 1 órán belül feldolgozza az adatokat
4. A tevékenységek adatai egy hónapig tárolódnak az Azure-ban.
5. Az Azure Portal beolvassa, majd az Azure Audit Reporing (Azure-naplójelentések) panel naplójának formájában jeleníti meg a naplójelentési adatokat.

## <a name="see-also"></a>További információ
- További részletek: [A hibrid jelentéskészítés kezelése az Identity Managerben](working-with-identity-manager-hybrid-reporting.md)
- További információt a [Naplózási tevékenységre vonatkozó jelentések az Azure Active Directory-portálon](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs) című témakörben talál
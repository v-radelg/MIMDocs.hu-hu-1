---
title: "Mi az a hibrid jelentéskészítés? | Microsoft Docs"
description: "Az Azure Active Directory hibrid naplózási jelentéseivel a felhőbeli és a helyszíni naplózott eseményeket egyaránt áttekintheti."
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Hibrid identitáskezelési naplójelentések az Azure Active Directoryban
Az Azure Active Directory (AD) naplójelentéseinek segítségével egyetlen jelentésben nyomon követheti a helyszínen vagy a felhőben végzett identitáskezelési tevékenységeket. Ez a funkció lehetővé teszi, hogy az összes identitását egy helyen kezelje, és egy helyen férjen hozzá adataihoz, így időt takaríthat meg és csökkentheti a teljes költségeket.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Mi az az Azure Active Directory hibrid jelentéskészítés?
A hibrid naplózási jelentéskészítés segít a rendszergazdáknak az identitáskezeléshez kapcsolódó általános jelentéskészítési feladatok elvégzésében.

1. **Identitáskezelési tevékenységek gyűjtése a különböző rendszerekből.** A hibrid jelentéseken az Azure AD és az Identity Manager identitáskezelési tevékenységei egyaránt megjelennek.

2. **Jelentési adatok exportálása és egyéni jelentések készítése.** Jelentéseit az Azure-portálon is megtekintheti, és exportálhatja is az adatokat saját egyéni nézetek létrehozása céljából.

3. **A jelentéskészítő rendszer infrastrukturális költségeinek csökkentése.** A felhőben történő hibrid jelentéskészítéssel megszüntetheti a helyszíni jelentéskészítő adatraktározási infrastruktúrát.

## <a name="how-does-it-work"></a>Hogyan működik?

A helyszíni adatok gyűjtéséhez először telepítenie kell egy jelentéskészítő ügynököt az Identity Manager 2016-kiszolgálón. A jelentéskészítő ügynököt a Microsoft letöltési oldaláról [itt](https://www.microsoft.com/en-us/download/details.aspx?id=####/) töltheti le.

A hibrid jelentéskészítés folyamata a következő lépésekből áll:
1. A jelentéskészítő ügynök a telepítést követően az Identity Manager-tevékenységek adatait a Windows Eseménynaplóba exportálja.
2. A jelentéskészítő ügynök feldolgozza a Windows Eseménynapló eseményeit, majd feltölti azokat az Azure Portalba.
3. A tevékenységek adatai egy hónapig tárolódnak az Azure-ban.
4. Amikor jelentést kér le, az Azure elemzi és szűri az eseményeket a szükséges jelentésekhez.
5. Az Azure Portal lehívja és a tevékenységjelentési napló formájában megjeleníti a jelentési adatokat.

## <a name="see-also"></a>További információ
- További részletek: [A hibrid jelentéskészítés kezelése az Identity Managerben](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)
- További információt a [Naplózási tevékenységre vonatkozó jelentések az Azure Active Directory-portálon](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs) című témakörben talál


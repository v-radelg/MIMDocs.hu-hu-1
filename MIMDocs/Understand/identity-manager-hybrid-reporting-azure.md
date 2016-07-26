---
title: "Mi az a hibrid jelentéskészítés? | Microsoft Identity Manager"
description: "Az Azure Active Directory hibrid jelentéskészítési funkcióival felhőalapú és helyszíni eseményeket egyaránt tartalmazó egyéni jelentéseket készíthet."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: a074f3361e6d5be497b1a3c25d56aaa7008b128e


---

# Hibrid identitáskezelési jelentések az Azure-ban
Az Azure Active Directory (AD) segítségével egyetlen jelentés létrehozásával nyomon követheti a helyszínen vagy a felhőben végzett identitáskezelési tevékenységeket. Ez a funkció lehetővé teszi, hogy az összes identitását egy helyen kezelje, és egy helyen férjen hozzá adataihoz, így időt takaríthat meg és csökkentheti a teljes költségeket.

## Mi az Azure AD hibrid jelentéskészítési funkciója?
A hibrid jelentéskészítés segít a rendszergazdáknak az identitáskezeléshez kapcsolódó általános jelentéskészítési kihívások megoldásában.

1. **Identitáskezelési tevékenységek gyűjtése a különböző rendszerekből.** A hibrid jelentéseken az Azure AD és az Identity Manager identitáskezelési tevékenységei egyaránt megjelennek.

2. **Jelentési adatok exportálása és egyéni jelentések készítése.** Jelentéseit az Azure-portálon is megtekintheti, és exportálhatja is az adatokat saját egyéni nézetek létrehozása céljából.

3. **A jelentéskészítő rendszer infrastrukturális költségeinek csökkentése.** A felhőben történő hibrid jelentéskészítéssel megszüntetheti a helyszíni jelentéskészítő adatraktározási infrastruktúrát.

## Hogyan működik?

A helyszíni adatok gyűjtéséhez először telepítenie kell egy jelentéskészítő ügynököt az Identity Manager-kiszolgálón. A jelentéskészítő ügynök a [Klasszikus Azure portálon](https://manage.windowsazure.com/) a saját könyvtára Konfigurálás oldaláról tölthető le.

A hibrid jelentéskészítés folyamata a következő lépésekből áll:
1. A jelentéskészítő ügynök a telepítést követően az Identity Manager-tevékenységek adatait a Windows Eseménynaplóba exportálja.
2. A jelentéskészítő ügynök feldolgozza és feltölti a Windows Eseménynaplóba exportált eseményeket az Azure-ba.
3. A tevékenységek adatai egy hónapig tárolódnak az Azure-ban.
4. Amikor jelentést kér le, az Azure elemzi és szűri az eseményeket a szükséges jelentésekhez.
5. A klasszikus Azure-portál lehívja és a tevékenységjelentés formájában megjeleníti a jelentési adatokat.

## Lásd még:
- További részletek: [A hibrid jelentéskészítés kezelése az Identity Managerben](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Jul16_HO3-->



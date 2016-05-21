---
# required metadata

title: Hibrid Identity Manager-jelentések készítése az Azure-ban | Microsoft Identity Manager
description: Az Azure Active Directory hibrid jelentéskészítési funkcióival felhőalapú és helyszíni eseményeket egyaránt tartalmazó egyéni jelentéseket készíthet.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Hibrid Identity Manager-jelentések készítése az Azure-ban
Az Azure Active Directory (AD) segítségével összevont jelentést készíthet a helyszíni környezetben és a felhőben történt identitáskezelési tevékenységekről. Ez a jelentéskészítő funkció összevont helyet kínál az identitási és hozzáférési adatok kezeléséhez, emellett pedig csökkenti az általános költségeket.

## Mi az Azure AD hibrid jelentéskészítési funkciója?
A hibrid jelentéskészítés segít a rendszergazdáknak az identitáskezeléshez kapcsolódó általános jelentéskészítési kihívások megoldásában.

1. **Identitáskezelési tevékenységek gyűjtése a különböző rendszerekből.** A hibrid jelentéseken az Azure AD és az Identity Manager identitáskezelési tevékenységei egyaránt megjelennek.

2. **Jelentési adatok exportálása és egyéni jelentések készítése.** Jelentéseit az Azure-portálon is megtekintheti, és exportálhatja is az adatokat saját egyéni nézetek létrehozása céljából.

3. **A jelentéskészítő rendszer infrastrukturális költségeinek csökkentése.** A felhőben történő hibrid jelentéskészítéssel megszüntetheti a helyszíni jelentéskészítő adatraktározási infrastruktúrát.

## Hogyan működik?

A helyszíni adatok gyűjtéséhez először telepítenie kell egy jelentéskészítő ügynököt az Identity Manager-kiszolgálón. A jelentéskészítő ügynököt a [klasszikus Azure-portálon](https://manage.windowsazure.com/) a saját könyvtár konfigurálási oldaláról töltheti le..

A hibrid jelentéskészítés folyamata a következő lépésekből áll:
1. A jelentéskészítő ügynök a telepítést követően az Identity Manager-tevékenységek adatait a Windows Eseménynaplóba exportálja.
2. A jelentéskészítő ügynök feldolgozza és feltölti a Windows Eseménynaplóba exportált eseményeket az Azure-ba.
3. A tevékenységek adatai egy hónapig tárolódnak az Azure-ban.
4. Amikor jelentést kér le, az Azure elemzi és szűri az eseményeket a szükséges jelentésekhez.
5. A klasszikus Azure-portál lehívja és a tevékenységjelentés formájában megjeleníti a jelentési adatokat.

## Lásd még:
- További részletek: [A hibrid jelentéskészítés kezelése az Identity Managerben](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)


<!--HONumber=Apr16_HO4-->



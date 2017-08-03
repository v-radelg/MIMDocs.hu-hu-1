---
title: Privileged Access Management REST API-referencia |} Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Privileged Access Management REST API-referencia
A Microsoft Identity Manager (MIM) 2016 ad hozzá egy új forgatókönyv Privileged Access Management (PAM) néven. A PAM lehetővé teszi, hogy a szervezet további szabályozhassák bizalmas erőforrások magas szintű jogosultságokkal rendelkező felhasználói fiókok, például a rendszer vagy szolgáltatás-rendszergazdák, hozzáférési jogosultságokat. A PAM szabályozza a magas jogosultsági fiók hozzáférést, korlátozott ideig hozzáférési jogok, adja meg a JIT (JIT), ha a hozzáférési jogosultságok szükségesek.

A felhasználó kérje meg a MIM szolgáltatás a kiemelt hozzáférési jogosultságokkal (jogosultsági szint emeléséhez) az alábbi két módszer egyikével:

- A PAM REST API használatával
- A PAM PowerShell New-PAMRequest parancsmag segítségével:

Ez az útmutató témakörei ismertetik a PAM REST API-t. A PowerShell használatával kapcsolatos további információk a parancsmag: A tesztlabor-Útmutató: a tanúsítványkulcs Privileged Access Management szolgáltatáshoz használatával a Microsoft Identity Manager, a connect webhelyen érhető el.

##<a name="pam-rest-api-resources-and-operations"></a>PAM REST API források és műveletek
A következő erőforrások működik a PAM REST API
- **PAM-szerepkör**: A PAM-szerepkör felhasználók gyűjteményére társítja a hozzáférési jogosultságot tartalmazó gyűjtemény. A hozzáférési jogok biztonsági csoportok alapján határozzák meg.  Minden PAM-szerepkör felhasználói fiókok, amelyek jogosultak a PAM-szerepkör emelheti jelöltek nevű tartozik. A PAM-szerepkörök a következő műveleteket végezheti el:

    - [PAM-szerepkörök lekérése](privileged-access-management-get-roles.md)

- **PAM-kérelem**: a PAM-szerepkör hozzáférési jogok emelheti kívánó felhasználóknak küldje el a PAM-kérelmet, és a kérelem jóváhagyásáról ahhoz, hogy a jogosultságszint-emelés rendelkezik. A PAM-kérelem objektum követi nyomon, hogy ezt a kérelmet a MIM szolgáltatás teljes életciklusát. A PAM-kéréseket a következő műveleteket végezheti el:

    - [PAM-kérelem létrehozása](privileged-access-management-create-request.md)
    - [PAM-kérések lekérése](privileged-access-management-get-requests.md)
    - [PAM-kérelmek lezárása](privileged-access-management-close-request.md)

- **PAM-kérelem függőben lévő**: jóváhagyása vagy elutasítása a PAM-kéréseket, felhasználók által benyújtott használt. A következő műveleteket hajthat végre a PAM-kérelem függőben lévő:

    - [Függőben lévő PAM-kérések lekérése](privileged-access-management-get-pending-requests.md)
    - [Függőben lévő PAM-kérelmek jóváhagyása vagy elutasítása](privileged-access-management-approve-reject-pending-request.md)

- **A PAM-munkamenet**: a PAM REST API használatával, amikor az ügyfél (például egy webes böngésző) rendelkezik-e a munkamenetet a következővel a PAM REST API végpontra. Ebben a munkamenetben az ügyfél hitelesítése a REST API végpontra. A PAM-munkamenetek a következő műveleteket végezheti el:

     - [PAM-munkamenetadatok lekérése](privileged-access-management-get-session-info.md)

A szolgáltatásokkal kapcsolatos adatokat, lásd: [PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>A PAM-Mintaportálon a Githubon
Egy a PAM REST API használata módja a PAM-mintaportálon, az API-t használó webes mintaalkalmazás használatával. A PAM-mintaportálon a kódot is megtalálhatja a [PAM minta GitHub tárházából](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Megismerheti a PAM tesztlabor-útmutató a mintaportálról telepítéséhez.

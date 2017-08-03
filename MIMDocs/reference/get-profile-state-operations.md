---
title: "Profil állapotát Get műveletek |} Microsoft Docs"
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
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e8428984404b2aebda8f53e4f7841b699a5d23a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-state-operations"></a>Profil állapotát Get műveletek
Lehetséges az aktuális felhasználó a megadott profil által végrehajtható műveletek listájának lekérése. A kérelem a megadott műveletek majd kezdeményezhető.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/CertificateManagement/API/V1.0/Profiles/{ID}/Operations <br/>/CertificateManagement/API/V1.0/smartcards/{ID}/Operations

###<a name="url-parameters"></a>URL-cím Paraméterek
Paraméter | Leírás
---------|------------
Azonosítója | Az azonosító (GUID) a profil és az intelligens kártya.

###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
nincs

##<a name="response"></a>Válasz
###<a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
200     | OK
204 | Nincs tartalom
403 | Tiltott
500 | Belső hiba

###<a name="response-headers"></a>Válaszfejlécek
Közös válaszfejlécek, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="response-body"></a>Választörzs
Siker lehetséges, hogy a felhasználó az intelligens kártya által végrehajtható műveletek listáját adja vissza. Ez a lista tartalmazhat a következők tetszőleges számú: *OnlineUpdate*, *megújítási*, *helyreállítása*, *RecoverOnBehalf*, *kivonás*, *visszavonása*, és *feloldása*.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       

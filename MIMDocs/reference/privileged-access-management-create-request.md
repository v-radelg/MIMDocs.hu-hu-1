---
title: "PAM-kérelem létrehozása |} Microsoft Docs"
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>PAM-kérelem létrehozása
A PAM-szerepkörbe jogosultságszintjének emeléséhez használt rendszerjogosultságú fiókkal.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `http://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
POST     |/API/pamresources/pamrequests

###<a name="query-parameters"></a>Lekérdezés-paraméterek
Paraméter | Leírás
--------|-------------
Indoklás | Nem kötelező. A felhasználó által megadott okát a jogosultságszint-emelési kérelem.
RoleId| Kötelező. Egyedi azonosítóját (GUID) a PAM-szerepkör emelheti.
RequestedTTL| Kötelező. A kért lejárati idő, másodpercekben.
RequestedTime | Optoinal. Az az idő való jogosultságszint-emelés jogosultságokkal.  
v | Nem kötelező. Az API-verzió. Ha nem szereplő, az API-t (a legutóbb kiadott) aktuális verziója lesz. További információkért lásd: [Versioning a PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md#versioning)

**Megjegyzés:**: megadhatja a *indoklás*, *RoleId*, *RequestedTTL*, és *RequestedTime* paraméterek tulajdonságként a kérelem törzsében szereplő, nem lekérdezési paramétereket. A *v* parameteer csak egy lekérdezési paraméter adható meg.

###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) a *PAM REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
Nem kötelező. A fentieknek megfelelően, a *indoklás*, *RoleId*, *RequestedTTL*, és *RequestedTime* paraméterek, a kérelem törzsében helyett adja meg azokat az URL-cím tulajdonságainak lekérdezési karakterlánc adható meg.

##<a name="response"></a>Válasz
###<a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
200 | OK
401 | Jogosulatlan
403 | Tiltott
408 | Kérelem időtúllépése   
500 | Belső kiszolgálóhiba.
503 | A szolgáltatás nem érhető el

###<a name="response-headers"></a>Válaszfejlécek
Közös válaszfejlécek, lásd: [HTTP-kérelmek és válaszfejlécek](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) a *PAM REST API szolgáltatás részletei*.
###<a name="response-body"></a>Választörzs
A sikeres válasz tartalmaz egy PAM-kérelem objektum a következő tulajdonságokkal.

Tulajdonság | Leírás
--------|-------------
Kérelemazonosító | Az egyedi azonosítóját (GUID) a PAM-kérelem.
CreatorID | Az egyedi azonosítóját (GUID) a MIM szolgáltatás kérelmet létrehozó fiók.
Indoklás | Jogosultságszint-emelés okát.
CreationTime | A kérelem létrehozása idején.
CreationMethod | A kérelem létrehozásához használt módszer.
ExpirationTime | A lejárati idő, a kérelem.
RoleID| Egyedi azonosítóját (GUID) a PAM-szerepkör.
RequestedTTL | A kért lejárati időtúllépés másodpercben.
RequestedTime | A kért idő történik.
RequestStatus | A kérés állapotát. Lehetséges értékek a következők: "Feldolgozása", "Active", "Lezárva", "Záró", "lejárt", "Jóváhagyásra vár", "PendingMFA" és "Visszautasított".

##<a name="example"></a>Példa

###<a name="request-1"></a>Kérelem 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Válasz 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>Kérelem 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Válasz 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       

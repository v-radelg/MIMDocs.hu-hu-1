---
title: "A PAM függőben lévő kérelmek |} Microsoft Docs"
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>A PAM függőben lévő kérelmek
Függőben lévő kérelmek jóváhagyást igénylő listájának egy rendszerjogosultságú fiókot használják.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `http://api.contoso.com`.
##<a name="request"></a>Kérelem

Módszer  |Kérelem URL-címe  
---------|---------
GET     |/API/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Lekérdezés-paraméterek
Paraméter | Leírás
----------|--------------
$filter | Nem kötelező. Adja meg a Perding PAM-kérelem tulajdonságok valamelyikét egy kifejezést a válaszok szűrt listájához való visszatéréshez. További információ a támogatott operátorok: [szűrést a PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md#filtering)
v | Nem kötelező. Az API-verzió. Ha nem szereplő, az API-t (a legutóbb kiadott) aktuális verziója lesz. További információkért lásd: [Versioning a PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md#versioning)

###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) a *PAM REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
nincs

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
A sikeres válasz jóváhagyási kérelemobjektumok PAM a következő tulajdonságokkal listáját tartalmazza.

Tulajdonság | Leírás
---------|-------------
RoleName | A megjelenített neve a szerepkör, amely jóváhagyásra van szükség.
Kérelmező | Jóváhagyásra váró a kérelmező felhasználó neve.
Indoklás | Az indoklásnak a felhasználó által megadott.
RequestedTTL | A kért lejárati idő másodpercben.
RequestedTime | A kért idő történik.
CreationTime | A kérelem létrehozása idején.
FIMRequestID | Egyetlen elemet tartalmaz, "Érték" egyedi azonosítóval (GUID) a PAM-kérelem.
RequestorID | Egyetlen elemet tartalmaz, "Érték", az egyedi azonosítóra (GUID) az Active Directory-fiók, amely a PAM-kérelmet létrehozni.
ApprovalObjectID | Egyetlen elemet tartalmaz, a "érték", a jóváhagyási objektum egyedi azonosítóját (GUID) rendelkező.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       

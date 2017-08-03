---
title: "PAM-szerepkörök beszerzése |} Microsoft Docs"
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>PAM-szerepkörök beolvasása
Egy rendszerjogosultságú fiókot használják a listát, amelynek a fiók nem választható a PAM-szerepkörök.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `http://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/API/pamresources/pamroles

###<a name="query-parameters"></a>Lekérdezés-paraméterek
Paraméter | Leírás
----------|--------------
$filter | Nem kötelező. Adja meg a PAM-szerepkör tulajdonságok valamelyikét egy kifejezést a válaszok szűrt listájához való visszatéréshez. További információ a támogatott operátorok: [szűrést a PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md#filtering)
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
A sikeres válasz a következő tulajdonságokkal rendelkeznek, amelyek egy vagy több PAM-szerepkörök gyűjteményét tartalmazza.

Tulajdonság | Leírás
--------|-------------
RoleID | Egyedi azonosítóját (GUID) a PAM-szerepkör.
DisplayName | Megjelenített név a PAM-szerepkört a MIM szolgáltatás.
Leírás | A PAM-szerepkört a MIM szolgáltatás leírása
Élettartam | A szerepkör hozzáférési jogok a maximális lejárati időtúllépés másodpercben.
AvailableFrom | A legkorábbi időpontot, amely egy requst aktív lesz.
EszközAz | A legújabb időpontot, hogy a kérelem aktív lesz.
MFAEnabled | Egy logikai érték, amely azt jelzi hogy az aktiválási kéréseket a szerepkör szükséges az MFA-kérdést.
ApprovalEnabled | Logikai érték, amely jelzi, hogy a szerepkör aktiválási kéréseket a szerepkör tulajdonosa általi jóváhagyás megkövetelése.
AvailibitlyWindowEnabled | Logikai érték, amely jelzi, hogy a szerepkör csak akkor lehet aktiválni egy adott időtartam során.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       

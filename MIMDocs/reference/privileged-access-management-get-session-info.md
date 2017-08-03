---
title: "A PAM-munkamenet-adatok beolvasása |} Microsoft Docs"
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
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 150bc7e863fc679e785526935155611fb75cd69b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-session-info"></a>A PAM-munkamenet-adatok beolvasása
Egy rendszerjogosultságú fiókot használják a beolvasása, amely a munkamenet jelentkezett be a fiók felhasználónevét.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `http://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/API/Session/sessioninfo

###<a name="query-parameters"></a>Lekérdezés-paraméterek
Paraméter | Leírás
----------|--------------
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
A sikeres válasz a következő tulajdonságokkal PAM munkamenet-objektumként adja vissza.

Tulajdonság| Leírás
--------|-------------
Felhasználónév| Az ebben a munkamenetben bejelentkezett fiók felhasználónevét.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       

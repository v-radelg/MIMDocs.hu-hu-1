---
title: "Intelligens kártyás hitelesítés tarozik |} Microsoft Docs"
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
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1c7a6f5e7ebd1e6e4cfc0992607adb91743f343e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-authentication-response"></a>Intelligens kártyás hitelesítés-válasz lekérése
Lekérdezi egy alapszintű CSP hitelesítési kérdésre adott válasz.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/CertificateManagement/API/V1.0/Requests/{reqid}/smartcards/{scid}/authenticationresponse

###<a name="url-parameters"></a>URL-cím Paraméterek
Paraméter | Leírás
---------|------------
reqid | Kötelező. A kérelem azonosítója (MIM Tanúsítványkezelő specifikus).
scid | Kötelező. Az intelligens kártya azonosítója (MIM Tanúsítványkezelő specifikus). Kapott a [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektum.
###<a name="query-parameters"></a>Lekérdezés-paraméterek
Paraméter | Leírás
---------|------------
ATR | Nem kötelező. Az intelligens kártya válasz alaphelyzetbe állítása (ATR) karakterlánc.
cardid | Kötelező. A kártya azonosítója.
kérdés | Kötelező. A base-64 kódolású karakterlánc, amely a kihívás az intelligens kártya által kiadott.
biztosításával | Kötelező. Egy logikai jelző azt jelöli, az intelligens kártya adminisztrációs kulcsot időjárási lett biztosításával.


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
Siker adja vissza a BLOB, a kérdés-válasz jelölő bájtban.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##<a name="see-also"></a>Lásd még:

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse módszer](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)

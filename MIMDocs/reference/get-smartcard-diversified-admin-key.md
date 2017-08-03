---
title: "Intelligens kártya változatos adminisztrátori kulcs beszerzése |} Microsoft Docs"
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
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 57a8481b2a4f976717b07061e96ccb041164a5a6
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-diversified-admin-key"></a>Intelligens kártya változatos adminisztrátori kulcs beszerzése
Lekérdezi a változatos adminisztrátori kulcsot a a megadott intelligens kártya.

**Megjegyzés:**: az adminisztrációs kulcsot csak kell lennie biztosításával, ha a profil sablonházirendjének azt jelzi, hogy meg kell.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/CertificateManagement/API/V1.0/Requests/{reqid}/smartcards/{scid}/diversifiedkey

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
Siker adja vissza egy olyan bájtot jelölő a változatos adminisztrációs kulcsot BLOB.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       
##<a name="see-also"></a>Lásd még:

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard metódus (karakterlánc, karakterlánc, kérelem) módszer](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)

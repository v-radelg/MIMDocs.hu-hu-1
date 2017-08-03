---
title: "Intelligens kártya hozzárendelése egy kérelem |} Microsoft Docs"
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>Intelligens kártya hozzárendelése egy kérelem
A megadott kérelem van kötve a megadott intelligens kártya. Egyszer kötve, a kérelem csak hajtható végre ez a kártya.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
POST     |/CertificateManagement/API/V1.0/smartcards

###<a name="url-parameters"></a>URL-cím Paraméterek
nincs.
###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
A kérelem törzsében az alábbi tulajdonságait tartalmazza.

Tulajdonság | Leírás
---------|-----------
Kérelemazonosító | A kérelemhez, amely az intelligens kártyával kell kötni azonosítója.
cardid | Az intelligens kártya cardid.
ATR | Az intelligens kártya válasz alaphelyzetbe állítása (ATR) karakterlánc.


##<a name="response"></a>Válasz
###<a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
201     | Létrehozás dátuma
204 | Nincs tartalom
403 | Tiltott
500 | Belső hiba

###<a name="response-headers"></a>Válaszfejlécek
Közös válaszfejlécek, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="response-body"></a>Választörzs
Siker és az újonnan létrehozott intelligens kártya objektum ad vissza egy URI-t.
##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>Válasz
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>Lásd még:

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard metódus (karakterlánc, karakterlánc, kérelem)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)

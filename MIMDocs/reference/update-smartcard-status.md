---
title: "Intelligens kártya állapotának frissítése |} Microsoft Docs"
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
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cb7b04539b8568a8b2ae83172b2aa8cddbf6174d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="update-smartcard-status"></a>Intelligens kártya állapotának frissítése
Frissíti az állapotát, az intelligens kártya.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
## <a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/ CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>URL-cím Paraméterek
Paraméter | Leírás
---------|------------
reqid | Kötelező. A kérelem azonosítója (MIM Tanúsítványkezelő specifikus).
scid | Kötelező. Az intelligens kártya azonosítója (MIM Tanúsítványkezelő specifikus). Ez az a "uuid" tulajdonságot a [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektum.

### <a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
### <a name="request-body"></a>Kérés törzsében
A kérelem törzsében az alábbi tulajdonságait tartalmazza.

Tulajdonság | Leírás
---------|-----------
Állapot | Az állapot beállítása a kérelmet. Például "már nincs".


## <a name="response"></a>Válasz
### <a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
200     | OK
204 | Nincs tartalom
403 | Tiltott
500 | Belső hiba

### <a name="response-headers"></a>Válaszfejlécek
Közös válaszfejlécek, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
### <a name="response-body"></a>Választörzs
Siker, adja vissza a JSON-szerializált [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektum a következő tulajdonságokkal:

Név | Leírás
-----|-----------
AssignedUserUuid | A felhasználó, akinek az intelligens kártya azonosítóját.
ATR | Az intelligens kártya válasz alaphelyzetbe állítása (ATR) karakterlánc a kártya, amely jelenleg inicializálása.
Megjegyzés | A Megjegyzés, mely leírja az intelligens kártya.
Jelzők | A jelzőkkel, amelyek az intelligens kártya ismertetik.
Köztes | A köztes az intelligens kártyához.
ParentSmartcardUuid | A régi intelligens kártya, amely az intelligens kártya átvette azonosítóját.
PermanentSmartcardUuid | Az állandó intelligens kártyával, az intelligens kártyához kapcsolódó azonosítóját.
PrimarySmartcardUuid | Az elsődleges intelligens kártya azonosítóját.
ProfileTemplateUuid | A profilsablon, amely tartalmazza a házirendek és a beállításokat, amelyek az intelligens kártya azonosítóját.
ProfileTemplateVersion | Az intelligens kártya profil létrehozásának időpontjában a profilsablon verzióját.
Sorozatszám | Az intelligens kártya sorozatszáma.
Állapot | Az intelligens kártya állapota.
UUID | Az intelligens kártya profil azonosítója.

## <a name="example"></a>Példa

### <a name="request"></a>Kérelem
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### <a name="response"></a>Válasz
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
## <a name="see-also"></a>Lásd még:

- [Microsoft.Clm.Shared.Smartcards.Smartcard osztály](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))

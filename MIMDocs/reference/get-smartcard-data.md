---
title: "Intelligens kártya adatainak lekérése |} Microsoft Docs"
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
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a84982971d169b5c9e7bd758cc74d795416975b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-data"></a>Intelligens kártya adatainak lekérése
Lehetséges, az aktuális felhasználó által végrehajtható műveletek listája a felhasználó intelligens kártya profilok listájának lekérése. A kérelem a megadott műveletek majd kezdeményezhető.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/CertificateManagement/API/V1.0/smartcards <br/> / CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


###<a name="url-parameters"></a>URL-cím Paraméterek
Tulajdonság| Leírás
---------|--------
smartcarduuid | Nem kötelező. Az intelligens kártya UUID MIM CM jelölik. Ez az a "uuid" mezőt a [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektum.

###<a name="query-parameters"></a>Lekérdezés-paraméterek
Tulajdonság| Leírás
---------|--------
cardid | Nem kötelező. Az intelligens kártya UUID MIM CM jelölik. Ez az a "uuid" mezőt a [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objektum.


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

##<a name="example"></a>Példa

###<a name="request-1"></a>Kérelem 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###<a name="response-1"></a>Válasz 1
```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
###<a name="request-2"></a>Kérelem 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###<a name="response-2"></a>Válasz 2
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
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
##<a name="see-also"></a>Lásd még:

-[Microsoft.Clm.Shared.Smartcards.Smartcard osztály](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)

---
title: "Profil sablonok beszerzését |} Microsoft Docs"
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Profilsablonok beolvasása
Amely a megadott felhasználó regisztrálhat a profil sablonok listájának lekérése. Ez a módszer a profilsablon korlátozott nézete adja vissza. Kell, hogy elegendő a kérelmező felhasználó döntse el, hogy mely profilsablon, ha van ilyen, igényléséhez szükségük ahhoz, hogy a profil sablon adatokat adott vissza. Ha nincs munkafolyamat és engedélyt meg van adva, a felhasználó számára látható profilsablonok visszatér.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates? \[targetuser\] 

###<a name="url-parameters"></a>URL-cím Paraméterek
Paraméter| Leírás
--------|-------------
targetuser| Nem kötelező. Adja meg a felhasználói profil sablonok vissza. Ha nincs megadva, az aktuális felhasználó identitása lesz. <br/>**Megjegyzés:**: jelenleg csak az aktuális felhasználó esetén támogatott.

###<a name="request-headers"></a>Kérelem fejlécei
Az szolgáltatás témakörben közös kérelemfejléc
###<a name="request-body"></a>Kérés törzsében
nincs

##<a name="response"></a>Válasz
###<a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
200     | OK
204 | Nincs tartalom
500 | Belső hiba

###<a name="response-headers"></a>Válaszfejlécek
Olvassa el a szolgáltatás közös válaszfejlécek.
###<a name="response-body"></a>Választörzs
Sikeres művelet esetén a következő tulajdonságokkal ProfileTemplateLimitedView objektumok listáját adja vissza.

Tulajdonság| Típus| Leírás
--------|-----|--------
Név| karakterlánc| A profil sablon megjelenítendő neve.
Leírás| karakterlánc| A profilsablon leírása.
UUID| GUID| A profilsablon azonosítója.
IsSmartcardProfileTemplate| logikai érték| Azt jelzi, hogy a sablon egy intelligens kártya profilsablon.
IsVirtualSmartcardProfileTemplate| logikai érték| Azt jelzi, hogy a profilsablon virtuális intelligens kártya szükséges.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       

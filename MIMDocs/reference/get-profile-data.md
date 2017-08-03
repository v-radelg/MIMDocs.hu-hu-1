---
title: Profil adatok |} Microsoft Docs
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>Profil-adatok beolvasása
Lehetséges, az aktuális felhasználó által végrehajtható műveletek listája a felhasználó szoftver tanúsítványprofilok listájának lekérése. A kérelem a megadott műveletek majd kezdeményezhető.

**Megjegyzés:**: A kiszolgáló állítja be a PIN-kód csak akkor, ha a profil sablonházirendjének azt jelzi, hogy el kell végezni. Ellenkező esetben a felhasználónak kell adja meg azt.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/CertificateManagement/API/V1.0/Profiles<br/>/ CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/API/V1.0/Requests/{RequestId}/Profiles

###<a name="url-parameters"></a>URL-cím Paraméterek
Paraméter | Leírás
---------|------------
Azonosítója | Az azonosító (GUID) a profil való visszatéréshez.
Kérelemazonosító | A kérelem vissza a profilok azonosítóját.

###<a name="query-parameters"></a>Lekérdezés-paraméterek
Paraméter | Leírás
---------|------------
Állapot | Nem kötelező. Az adatok beolvasása a profilok állapotát jelzi. Lehetséges állapota típusok a következők: "Active", "Engedélyezett", "Megszakítva", "Kész", "Megtagadva", "Végrehajtás alatt álló", "Sikertelen", "None", és a "Függőben". <br/>Ha nincs állapot van megadva, minden profilban, függetlenül attól, állapot visszatér.
###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
nincs

##<a name="response"></a>Válasz
###<a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
200 | OK
204 | Nincs tartalom
403 | Tiltott
500 | Belső hiba

###<a name="response-headers"></a>Válaszfejlécek
Közös válaszfejlécek, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="response-body"></a>Választörzs
A sikeres, a JSON-szerializált listáját adja vissza [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) objektumok a következő tulajdonságokkal:

Tulajdonság | Leírás
---------|------------
AssignedUserUuid | A felhasználó, akinek a profil hozzá van rendelve azonosítóját.
Megjegyzés | A Megjegyzés, amely leírja a profilt.
Jelzők | A jelző, a profil leírása.
ParentProfileUuid | A régi profilt a profil felváltó azonosítóját.
PrimaryProfileUuid | Az elsődleges profil azonosítója.
ProfileOperations | A lehetséges a profilban az aktuális felhasználó által végrehajtható műveletek listája.
ProfileTemplateUuid | A profilsablon, amely tartalmazza a házirendek és a beállításokat, amelyek a profil azonosítója.
ProfileTemplateVersion | A profil létrejöttének időpontjában a profilsablon verzióját.
Állapot | A profil állapota.
UUID | A profil azonosítója.


##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##<a name="see-also"></a>Lásd még:

- [Microsoft.Clm.Shared.Profiles.Profile osztály](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)

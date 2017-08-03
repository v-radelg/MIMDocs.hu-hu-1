---
title: "Kérelem létrehozása |} Microsoft Docs"
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Kérelem létrehozása
A MIM Tanúsítványkezelő kérelem létrehozása.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
POST     |/CertificateManagement/API/V1.0/Requests

###<a name="url-parameters"></a>URL-cím Paraméterek
nincs

###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
A kérelem törzsében az alábbi tulajdonságait tartalmazza.

Tulajdonság | Leírás
---------|-----------
profiletemplateuuid | Kötelező. A felhasználó hoz létre a kérelem profil-sablon GUID azonosítója.
datacollection objektumot | Kötelező. Meg kell adni a jelentkező által tárolt adatok képviselő név-érték párok gyűjteménye. Az adatgyűjtés szükséges, hogy meg kell adni a profilsablon munkafolyamat házirend lekérhetők. Üres gyűjtemény adható meg.
cél | Nem kötelező. A GUID azonosítója, amelyben a kérés tartozó felhasználói. Ha nincs megadva, ez az alapértelmezett az aktuális felhasználónak.
típus | Kötelező. A létrehozandó kérelem típusa. Rendelkezésre álló kérések típusa: "Igénylési", "Duplikált", "OfflineUnblock", "OnlineUpdate", "Megújítási", "Helyreállítása", "RecoverOnBehalf", "Visszaállítása", "Eltávolítás", "Visszavonása", "TemporaryCards" és "Unblock".<br/>**Megjegyzés:**: kérelem nem minden alkalmazástípus összes profilsablonok támogatottak. Például egy szoftver profilsablon feloldása műveletet nem adható meg.
megjegyzés | Kötelező. Amely a felhasználó adja meg megjegyzéseit. A munkafolyamat házirend meghatározzák, hogy erre akkor szükség. Üres karakterlánc adható meg.
Prioritás | Nem kötelező. A kérés prioritását. Ha nincs megadva, az alapértelmezett szolgáltatáskérések prioritása alapján a profil sablonbeállítások használható.


##<a name="response"></a>Válasz
###<a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
201     | Létrehozás dátuma
403 | Tiltott
500 | Belső hiba

###<a name="response-headers"></a>Válaszfejlécek
Közös válaszfejlécek, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="response-body"></a>Választörzs
Siker az újonnan létrehozott kérelem URI-JÁNAK beolvasása.
##<a name="example"></a>Példa

###<a name="request-1"></a>Kérelem 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Válasz 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Kérelem 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Válasz 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Kérelem 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##<a name="see-also"></a>Lásd még:

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll módszer](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock módszer](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover módszer](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire módszer](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock módszer](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)

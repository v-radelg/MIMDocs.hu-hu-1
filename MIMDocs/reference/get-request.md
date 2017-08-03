---
title: "GET kérés |} Microsoft Docs"
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
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 56bb25f7282879943dc624f3c24b836c7ae4a58c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-request"></a>GET kérés
Egy vagy több megadott MIM CM kérést kap.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/ CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>URL-cím Paraméterek
Tulajdonság| Leírás
---------|--------
Azonosítója| Nem kötelező. A GUID egy kérelem beolvasása.
###<a name="query-parameters"></a>Lekérdezés-paraméterek
Tulajdonság| Leírás
---------|--------
targetuser| Nem kötelező. A kérelem célfelhasználó. Ha nem a cél van megadva, visszaadott összes kérelmet, függetlenül a célfelhasználó. <br/> **Megjegyzés:**: csak "aktuális" jelenleg támogatott.
Állapot| Nem kötelező. Lekérésére irányuló kérelem állapotát jelzi. Lehetséges állapota típusok a következők: *jóváhagyott*, *visszavonva*, *befejezve*, *megtagadva*, *Executing*, *sikertelen*, *nincs*, és *függőben lévő*. <br/>Ha nincs állapot van megadva, visszaadott összes kérelmet, függetlenül attól, állapota.

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
Siker, adja vissza egy vagy több [kérelem](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) objektumok a következő tulajdonságokkal:

Név | Leírás
-----|------------
Megjegyzés | A MIM CM kérelem társított megjegyzés.
Befejeződött | A MIM CM kérelem ideje.
Datacollection objektumot | A társított a MIM CM kérelem az adatgyűjtő elemeket.
DataCollectionFlags | A MIM CM kérelem gyűjtésének beállításai.
Jelzők | A MIM CM kérelem társított beállításokat.
IsDataCollectionComplete | Logikai érték, amely jelzi, ha az adatgyűjtés befejezése a MIM CM kérelem.
IsEnrollmentAgent | Logikai érték, amely jelzi, ha a tanúsítványigénylő megbízottat a MIM CM kérelem futtatásához szükséges.
IsSmartcard | Egy logikai érték, amely azt jelzi, hogy a MIM CM kérelem egy intelligens kártya kérelem vagy szoftver profil kérelem.
NewProfileUuid | Az új szoftverfrissítési profil a MIM CM kérelem által létrehozott identitását.
NewSmartcardUuid | Az új intelligens kártyával, a MIM CM kérelem által hozzárendelt identitását.
OldProfileUuid | A szoftver-profilt, amelynek a MIM CM iránti kérelem létrehozása identitását.
OldSmartcardUuid | Az identitás, az intelligens kártya, amelyre a MIM CM kérelmet létrehozták.
OriginatorUserUuid | A MIM CM kérést a felhasználó identitása
Prioritás | A MIM CM szolgáltatáskérések prioritása.
ProfileTemplateUuid | A profilsablon, amelyre a MIM CM iránti kérelem létrehozása identitását.
RequestType | A MIM CM kérelem típusa.
SecurityDescriptor | A MIM CM kérelem biztonsági leírója.
Állapot | A MIM CM kérés állapotát.
Elküldve | A MIM CM kérés el lett küldve időpontja.
TargetUserUuid | A MIM CM kérelem felhasználói identitását.
UUID | A MIM CM kérelem az azonosítója.

##<a name="example"></a>Példa

###<a name="request-1"></a>Kérelem 1
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1

```
###<a name="response-1"></a>Válasz 1
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}```       
###Request 2
```
/Certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1 beolvasása
```
###Response 2
```
A HTTP/1.1 200 OK

{"Uuid": "0c96d73f-967b-420e-854a-43ad2a1504bc", "RequestType": 5, "Status": 3, "jelzők": 2, "datacollection objektumot": [{"Name": "Sample Data Item", "Érték": null}], "DataCollectionFlags": 32, "OriginatorUserUuid": "8f1590dc-d932-4b66-8e68-2e91c5880780", "TargetUserUuid": "8f1590dc-d932-4b66-8e68-2e91c5880780", "benyújtott": "2015-07-07T23:40:33.133Z", "Kész": "0001-01-01T00:00:00", "NewProfileUuid": "b99ff38c-6653-471f-ae3c-325bb351a6bc", "OldProfileUuid": "b99ff38c-6653-471f-ae3c-325bb351a6bc", "NewSmartcardUuid": "17cf063d-e337-4aa9-a822-346554ddd3c9", "OldSmartcardUuid": "17cf063d-e337-4aa9-a822-346554ddd3c9", "Prioritás": 0, "Comment": "", "ProfileTemplateUuid": "a039b4d0-5ce8-4eff-8651-181c6576fda3", "SecurityDescriptor": "O:S-1-5-21-2127521184-1604012920-1887927527-14134865 D:(D;; LC;; S-1-5-21-2127521184-1604012920-1887927527-14995557) (A; SWRC;; S-1-5-21-2127521184-1604012920-1887927527-5094533) (A; RC;; WD) (A; CCDCLCSWSDRC;; S-1-5-21-2127521184-1604012920-1887927527-14134865) (A; D CSAK;; S-1-5-21-2127521184-1604012920-1887927527-14995557) (A; CCDCSW;; S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000 ","IsSmartcard": true,"IsEnrollmentAgent": false,"IsDataCollectionComplete": false}
```       

##See Also

- [Microsoft.Clm.Provision.FindOperations.FindRequest Method](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft.Clm.Shared.RequestPermission Enumeration](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft.Clm.Shared.Requests.Request Class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)

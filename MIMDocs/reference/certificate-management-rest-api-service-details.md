---
title: "Tanúsítványkezelő REST API szolgáltatás részletek |} Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>Tanúsítványkezelő REST API-szolgáltatás részletes adatai
Az alábbi szakaszok ismertetik a Microsoft Identity Manager (MIM) tanúsítványa (CM) felügyeleti REST API részleteit.

## <a name="architecture"></a>Architektúra 
MIM Tanúsítványkezelő REST API-hívások tartományvezérlők kezeli. A következő táblázat a tartományvezérlőket és a környezet, amelyben is használhatók minták teljes listáját jeleníti meg.

Tartományvezérlő| A minta útvonal
----------|-------------
CertificateDataController| /API/V1.0/Requests/{RequestId}/certificatedata /
CertificateRequestGenerationOptionsController| /API/V1.0/Requests/{RequestId}/certificaterequestgenerationoptions
CertificatesController| /API/V1.0/smartcards/{smartcardid}/Certificates <br/> /API/V1.0/Profiles/{profileid}/Certificates
OperationsController| /API/V1.0/smartcards/{smartcardid}/Operations <br/> /API/V1.0/Profiles/{profileid}/Operations
PoliciesController| / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| / api/v1.0/profiles/{id} <br/> /API/V1.0/Profiles <br/> / api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| / api/v1.0/profiletemplates/{id} <br/> /API/V1.0/profiletemplates <br/> / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| / api/v1.0/requests/{id} <br/> /API/V1.0/Requests
SmartcardsController| /API/V1.0/Requests/{RequestId}/smartcards/{ID}/diversifiedkey <br/> /API/V1.0/Requests/{RequestId}/smartcards/{ID}/serverproposedpin <br/> /API/V1.0/Requests/{RequestId}/smartcards/{ID}/authenticationresponse <br/> / api/v1.0/requests/{requestid}/smartcards/{id} <br/> / api/v1.0/smartcards/{id} <br/> /API/V1.0/smartcards
SmartcardsConfigurationController| /API/V1.0/profiletemplates/{profiletemplateid}/Configuration/smartcards
<br/>

## <a name="http-request-and-response-headers"></a>HTTP-kérelmek és válaszfejlécek

Az API-nak küldött HTTP-kérelmekre a következő fejlécek (ebben a listában nincs teljes körű) kell tartalmaznia:

Fejléc | Leírás
-------|------------
Engedélyezés | Kötelező. A tartalom függvényében adható meg a hitelesítési módszer, amely konfigurálható, és WIA (integrált Windows-hitelesítés) vagy az AD FS alapulhat.
Tartalomtípus | Szükséges, ha a kérés van törzse. "Application/json" kell lennie.
Content-Length | Szükséges, ha a kérés van törzse. 
Cookie-k | A munkameneti cookie-ból. Hitelesítési módszertől függően szükség lehet.
<br/>
HTTP-válaszokat tartalmazza a következő fejlécek (ebben a listában nincs teljes körű):

Fejléc | Leírás
-------|------------
Tartalomtípus | Az API-t mindig adja meg "az application/json".
Content-Length | A kérelem törzsében, ha van ilyen, bájtban hosszát.


## <a name="api-versioning"></a>API-Versioning 
A Tanúsítványkezelő REST API jelenlegi verziója 1.0. A verzió van megadva a közvetlenül a következő szegmens a `/api` szegmens URI azonosítójában: `/api/v1.0`. A jövőben változik a verziószám kell lényegesen módosul az API felületéhez.


## <a name="enabling-the-api"></a>Az API-t engedélyezése 
A `<ClmConfiguration>` szakasz a Web.config fájl megtörtént-e új kulccsal:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Ez a kulcs határozza meg, hogy Tanúsítványkezelő REST API ügyfelek van kitéve. Ha a kulcs értéke "false", az API a útvonal leképezés nem történik alkalmazás következő indításakor. Ez azt jelenti, hogy bármely ezt követő kérelmek API-végpontokon visszaállítja-e egy HTTP 404 hibakód jelzi. Alapértelmezés szerint a kulcs beállítva "letiltott".
Miután megváltoztatta ezt az értéket "true", ne felejtse el futtatása **iisreset** a kiszolgálón.

## <a name="enabling-tracing-and-logging"></a>Nyomkövetés és a naplózás engedélyezése 
A MIM Tanúsítványkezelő REST API-t az egyes neki küldött HTTP-kérelmek nyomkövetési adatok bocsát ki. A részletességi szint a nyomkövetési adatokat úgy, hogy a következő konfigurációs érték kibocsátott állíthatja be:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>Hibakezelés és hibaelhárítás 
Amikor egy kérelem feldolgozása során kivétel lép fel, a MIM Tanúsítványkezelő REST API-t egy HTTP-állapotkód: a webes ügyféllel ad vissza. Gyakori hibák az API-t megfelelő HTTP-állapotkódot és egy hibakódot ad vissza. 

Nem kezelt kivételek alakítja át a rendszer egy `HttpResponseException` HTTP-állapot kód 500 ("belső Error"), és az Eseménynapló és a MIM CM nyomkövetési fájl is nyomon követni. Nem kezelt kivételek írása az eseménynaplóba, a megfelelő korrelációs azonosítót. A fogyasztó az API a hibaüzenet a korrelációs azonosító is lett van küldve. A hiba elhárításához egy rendszergazda kereshet a megfelelő korrelációs azonosítója és a hiba részleteit az eseménynaplóban találhatók.

Biztonsági okok miatt hibák miatt a MIM Tanúsítványkezelő REST API fel keletkeztek megfelelő híváslánc megjelenik nem kap az ügyfélnek.

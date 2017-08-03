---
title: "Első generációs lehetőségek |} Microsoft Docs"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Első generációs lehetőségek

Lekérdezi a paraméterek az ügyféloldali tanúsítvány kérelem létrehozásához.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/CertificateManagement/API/V1.0/Requests/{RequestId}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>URL-cím Paraméterek
Paraméter | Leírás
--------|--------------
Kérelemazonosító| Kötelező. A MIM CM kérelem, amelynek a tanúsítvány kérelem generációs paraméterei kérhető GUID azonosítóját.

###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
nincs.


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
Siker CertificateRequestGenerationOptions objektumok listáját adja vissza. Az egyes CertificateRequestGenerationOptions objektumok felel meg egyetlen tanúsítványkérelem, hogy az ügyfél kell létrehoznia, és a következő tulajdonságokkal rendelkeznek:

Tulajdonság| Leírás
--------|-----------
Exportálható | Egy érték, amely meghatározza, hogy lehet exportálni a titkos kulcsnak a kérelem.
Rövid név | A regisztrált tanúsítványt megjelenítendő nevét.
HashAlgorithmName | A tanúsítvány a kérelem-aláírás létrehozásakor használt kivonatoló algoritmus.
KeyAlgorithmName | A nyilvánoskulcs-képzési algoritmus.
KeyProtectionLevel | A kulcs erős védelmét mértékét.
KeySize | A méret bit, a titkos kulcs generálása.
KeyStorageProviderNames | Elfogadható kulcstároló-szolgáltatók (KSP) a titkos kulcs létrehozásához használható listáját. Abban az esetben, hogy az első Kulcstároló nem használható a megadott KSP bármelyikét cert kérelmet létrehozni is használható, amíg valamelyik nem jár sikerrel.
KeyUsages | A művelet, amely a titkos kulcsnak a tanúsítványkérelem fájlnevét végzi el. Az alapértelmezett érték: az aláírás.
Tárgy | A tulajdonos nevét.

**Megjegyzés**: további információt a Tulajdonságok érhető el a [Windows.Security.Cryptography.Certificates.CertificateRequestProperties osztály](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) leírását, de vegye figyelembe, hogy nincs-e nem ez az osztály és CertificateRequestGenerationOptions objektumok egy az egyhez típusú megfeleltetése.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       

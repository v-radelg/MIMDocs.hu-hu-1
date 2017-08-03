---
title: "Intelligens kártya PIN-kód javasolt beszerzése |} Microsoft Docs"
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Intelligens kártya PIN-kód javasolt beolvasása
A kiszolgáló által létrehozott felhasználói PIN-kód beolvasása.

**Megjegyzés:**: A kiszolgáló állítja be a PIN-kód csak akkor, ha a profil sablonházirendjének azt jelzi, hogy el kell végezni. Ellenkező esetben a felhasználónak kell adja meg azt.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/CertificateManagement/API/V1.0/smartcards/{ID}/serverproposedpin

###<a name="url-parameters"></a>URL-cím Paraméterek
Paraméter | Leírás
---------|------------
Azonosítója | Az intelligens kártya azonosítója (MIM Tanúsítványkezelő specifikus). A Microsft.Clm.Shared.Smartcard objektumot kapott.
###<a name="query-parameters"></a>Lekérdezés-paraméterek
Paraméter | Leírás
---------|------------
ATR | A kártya atr.
cardid | A kártya azonosítója.
kérdés | A base-64 kódolású karakterlánc, amely a kihívás az intelligens kártya által kiadott.

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
A sikeres a PIN-kódot, a kiszolgáló által javasolt jelölő karakterláncot ad vissza.

##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

... body coming soon
```       

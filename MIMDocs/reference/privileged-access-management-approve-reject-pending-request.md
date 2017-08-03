---
title: "Jóváhagyhatja vagy elutasíthatja a függőben lévő PAM-kérelem |} Microsoft Docs"
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
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e5506f96a22d1918cff7d0c9b822babb0f6cfa9
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Jóváhagyása vagy elutasítása a PAM függőben lévő kérelem
Egy rendszerjogosultságú fiókot használják a jóváhagyása, zárja be, vagy elutasítása a PAM-szerepkörök emelheti kérést.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `http://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
POST     |/API/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/API/pamresources/pamrequeststoapprove({approvalId)/Reject

###<a name="url-parameters"></a>URL-cím Paraméterek
Paraméter | Leírás
----------|-----------
approvalId | A PAM a jóváhagyási objektum azonosítóját (GUID) az alábbiak szerint megadva`guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Lekérdezés-paraméterek
Paraméter | Leírás
----------|--------------
v | Nem kötelező. Az API-verzió. Ha nem szereplő, az API-t (a legutóbb kiadott) aktuális verziója lesz. További információkért lásd: [Versioning a PAM REST API szolgáltatás részletei](privileged-access-management-rest-api-service-details.md#versioning)


###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) a *PAM REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
nincs.

##<a name="response"></a>Válasz
###<a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
200 | OK
401 | Jogosulatlan
403 | Tiltott
408 | Kérelem időtúllépése   
500 | Belső kiszolgálóhiba.
503 | A szolgáltatás nem érhető el

###<a name="response-headers"></a>Válaszfejlécek
Közös válaszfejlécek, lásd: [HTTP-kérelmek és válaszfejlécek](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) a *PAM REST API szolgáltatás részletei*.
###<a name="response-body"></a>Választörzs
nincs
##<a name="example"></a>Példa

###<a name="request"></a>Kérelem
```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1


```
###<a name="response"></a>Válasz
```
HTTP/1.1 200 OK

```       

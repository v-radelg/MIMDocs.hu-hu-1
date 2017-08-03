---
title: "Intelligens kártya házirend beszerzése |} Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164795e81ff0ef2f93086144b721e760f8dd028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-policy"></a>Intelligens kártya házirend beolvasása

A profil sablonházirendjének lekérdezi a megadott munkafolyamat. Ezek az adatok kérelem létrehozása során használatos. A munkafolyamat házirend határozza meg, mely adatokat szeretne szükség van az ügyfél által kérelmet létrehozni. Ilyen adatok lehetnek: különböző az adatgyűjtő elemeket, a kérelem megjegyzések és egy időt jelszóházirend.

**Megjegyzés:**: Ebben a témakörben látható URL-címek vannak viszonyítva; API telepítésekor kiválasztott állomásnév például: `https://api.contoso.com`.
##<a name="request"></a>Kérelem


Módszer  |Kérelem URL-címe  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

###<a name="url-parameters"></a>URL-cím Paraméterek
Paraméter| Leírás
--------|-------------
Azonosítója| Kötelező. A profilsablon, amelyet a házirend kibontani a megfelelő GUID azonosító.
típus| Kötelező. A kért szabályzat típusát. Lehetséges értékek a következők: *beléptetés*, *ismétlődő*, *OfflineUnblock*, *OnlineUpdate*, *megújítási*, *helyreállítása*, *RecoverOnBehalf*, *visszaállítása*, *kivonás*, *visszavonása*, *TemporaryEnroll*, *Unblock*.

###<a name="request-headers"></a>Kérelem fejlécei
Közös kérelemfejléc, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="request-body"></a>Kérés törzsében
nincs

##<a name="response"></a>Válasz
###<a name="response-codes"></a>Válaszkódok
Kód  |Leírás  
---------|---------
200     | OK
403 | Tiltott
204 | Nincs tartalom
500 | Belső hiba

###<a name="response-headers"></a>Válaszfejlécek
Közös válaszfejlécek, lásd: [HTTP-kérelmek és válaszfejlécek](certificate-management-rest-api-service-details.md#http-request-and-response-headers) a *Tanúsítványkezelő REST API szolgáltatás részletei*.
###<a name="response-body"></a>Választörzs
Siker, adja vissza egy csoportházirend-objektum alapján egy [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) objektum. Legalább a csoportházirend-objektum tulajdonságait az alábbi táblázat tartalmazza, de további tulajdonságok attól függően, hogy a kért házirend tartalmazhat. Például az igénylési házirend kérelmet ad vissza egy [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) objektum. További információ a dokumentációban a {} típusparaméter a kérelemben szereplő társított csoportházirend-objektum. A csoportházirend-objektumok különböző típusú dokumentációjában találhatók a [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) dokumentációját.

Tulajdonság | Leírás
---------|------------
ApprovalsNeeded | A FIM CM-kérelmeket a szabályzat által igényelt jóváhagyások száma.
AuthorizedApprover | A biztonsági leírót a felhasználók, akik jogosultak a FIM CM kérelmek a házirend jóváhagyása.
AuthorizedEnrollmentAgent | A biztonsági leírót a felhasználók, akik tanúsítványigénylő megbízottak a házirend.
AuthorizedInitiator | A felhasználók számára is kezdeményezhető a FIM CM-kérelmeket a szabályzat biztonsági leíró.
CollectComments | Logikai érték, amely jelzi, hogy engedélyezve van-e megjegyzés gyűjtemény a FIM CM-kéréseket a házirend.
CollectRequestPriority | Logikai érték, amely jelzi, hogy engedélyezve van-e request gyűjtemény a prioritás a FIM CM-kéréseket a házirend.
DefaultRequestPriority | FIM CM-kérelmeket a házirend alapértelmezett prioritását.
Dokumentumok | A házirend-dokumentumok, a házirend vannak beállítva.
Engedélyezve | Logikai érték, amely jelzi, ha a házirend engedélyezve van-e.
EnrollAgentRequired | Logikai érték, amely jelzi, ha a tanúsítványigénylő megbízottak szükségesek a FIM CM-kéréseket a házirend.
OneTimePasswordPolicy | Lekérdezi a FIM CM-kéréseket hogyan egyszeri jelszavak, a jutnak el a házirend.
Személyre szabás | Az intelligens kártya testreszabási beállítások a szabályzat.
PolicyDataCollection | A rendszer a házirendhez társított az adatgyűjtő elemeket.
SelfServiceEnabled | Logikai érték, amely jelzi, hogy engedélyezve van-e FIM CM kérelmek önkiszolgáló kezdeményezését a házirend.

##<a name="example"></a>Példa

###<a name="request-1"></a>Kérelem 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Válasz 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Kérelem 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Válasz 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Lásd még:

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy osztály](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)

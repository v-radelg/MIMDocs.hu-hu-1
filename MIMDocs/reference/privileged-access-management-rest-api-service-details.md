---
title: "A PAM REST API-szolgáltatás részletes adatai |} Microsoft Docs"
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
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>A PAM REST API-szolgáltatás részletes adatai
Az alábbi szakaszok ismertetik a Microsoft Identity Manager (MIM) Privileged Access Management (PAM) REST API részleteit.

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

## <a name="versioning"></a>Versioning 
Az API jelenlegi verziója: 1. A kérelem URL-címét, az alábbi példában látható módon egy lekérdezési paraméter adható meg az API-verzió: `http://localhost:8086/api/pamresources/pamrequests?v=1` a kérelemben nincs megadva a verziója, ha a kérelem végrehajtása a API legutóbb kiadott verziójával. 

## <a name="security"></a>Biztonság 
Az API használatához integrált Windows-hitelesítéssel (IWA). Ezt manuálisan kell beállítania az IIS-ben a Microsoft Identity Manager (MIM) telepítése előtt.

HTTPS (TLS) támogatott, de az IIS-ben manuálisan kell beállítania. További információ:: **megvalósítása Secure Sockets Layer (SSL) a FIM-portál** a [9. lépés: FIM 2010 R2 telepítés utáni feladatok végrehajtása] (https://technet.microsoft.com/library/hh322875 (v=ws.10%29.aspx) a telepítése FIM 2010 R2 tesztlabor-útmutató. 

Létrehozhat egy új SSL-kiszolgálótanúsítványt a Visual Studio parancssorból a következő parancs futtatásával:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Ez a parancs létrehoz egy önaláírt tanúsítványt, a webes alkalmazás URL-cím test.cwap.com webkiszolgáló SSL-t használó teszteléséhez használható. Az Objektumazonosító, az - eku beállítás határozza meg a tanúsítvány azonosítja, egy SSL-tanúsítvány. A tanúsítványt a saját tárolóban és érhető el a számítógép szintjén, a tanúsítványok beépülő modulban a mmc.exe exportálása

## <a name="cross-domain-access-cors"></a>Kereszt-tartomány eléréséhez (CORS) 
A CORS támogatott, de az IIS-ben manuálisan kell beállítania. A következő elemek hozzáadása a telepített API web.config fájlban az API-t, hogy lehetővé teszi a tartományok közötti hívások konfigurálásához: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>Hibakezelés 
Az API HTTP hibaválaszok jelzi a hibát adja vissza. Hibák a megfelelő OData. A következő táblázat az ügyfél számára visszaadott hibakódok.

HTTP-állapotkód | Leírás
-----------------|------------
401 | Jogosulatlan 
403 | Tiltott 
408 | Kérelem időtúllépése   
500 | Belső kiszolgálóhiba. 
503 | A szolgáltatás nem érhető el 
<br/>

## <a name="filtering"></a>Szűrés 
A PAM REST API-kérés tartalmazhatnak úgy, hogy adja meg a tulajdonságokat, amelyeket fel kell venni a választ. Szűrőszintaxisának OData kifejezések alapul.

Szűrők adhatja meg a PAM-kéréseket, PAM-szerepkörök tulajdonságait. és a függőben lévő PAM-kéréseket. Például: *ExpirationTime*, *DisplayName*, vagy bármely más érvényes tulajdonság egy PAM-kérelem, a PAM-szerepkör vagy a függőben lévő kérelem.

Az API-t az alábbi műveleteket támogatja szűrőkifejezésekben: *és*, *egyenlő*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*, és *LessThanOrEqual*. 

A következő minta kérelmeket szűrőket tartalmazzák:

- Ehhez a kérelemhez megadott dátum közötti PAM-kéréseket adja vissza:`http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- A kérelem a Pam-szerepkör a megjelenített név: "SQL fájl hozzáférés" adja vissza:`http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `

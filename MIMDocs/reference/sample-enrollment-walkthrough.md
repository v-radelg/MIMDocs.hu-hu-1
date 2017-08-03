---
title: "Regisztrációs forgatókönyv minta |} Microsoft Docs"
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
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>A minta regisztrációs forgatókönyv
Ez a témakör bemutatja a virtuális intelligens kártya önkiszolgáló regisztráció végrehajtásához szükséges lépéseket. A felhasználó által beállított PIN-kód automatikusan jóváhagyta kérést láthatók.
1.  Az ügyfél akkor hitelesíti a felhasználót, majd, amely a hitelesített felhasználó regisztrálhat a profil sablonok listáját kéri:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  Az ügyfél a megjelenő listában megjeleníti a felhasználónak. A felhasználó kiválasztja a "Virtuális intelligens kártya VPN" nevű, és UUID vSC (virtuális intelligens kártya) profilsablon *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.  Az ügyfél lekéri a tanúsítványigénylési házirend a kijelölt profilsablon az előző lépés eredményeképpen visszakapott UUID használatával:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  Az ügyfél elemzi a "Datacollection objektumot" mező a visszaadott házirendben megjegyezni, hogy megjelenik-e a "Kérelem OK" elem jogosult egyetlen adatgyűjtés. Az ügyfél is jelzi, hogy a "collectComments" jelző értéke *hamis*, így nem, akkor a felhasználó megadja a kérni.

5.  Miután a felhasználó megadott a tanúsítvány igénylése az az oka, az ügyfél kérést hoz létre:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  A kiszolgáló sikeresen hozza létre a kérelmet, és visszaadja az ügyfélnek a kérelem URI: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  Az ügyfél lekéri a request objektumon a visszaadott URI meghívásával:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  Az ügyfél ellenőrzi, hogy a kérelem a "állapot" tulajdonság értéke "engedélyezett". Kérelem végrehajtásának megkezdése.

9.  Az ügyfél megvizsgálja a kérelem-e már társítva van a kérés a tartalma "newsmartcarduuid" paraméter elemzésével intelligens kártyát.

10. Ugyanis csak egy üres GUID-Azonosítót tartalmaz, az ügyfél kell használjon egy meglévő kártya már nem használja a MIM Tanúsítványkezelő, vagy hozzon létre egyet a virtuális intelligens kártyák konfigurált profilsablon esetén.

11. Az utóbbi meg van adva az ügyfélnek a kezdeti lekérdezése enrollable profilsablonok (1. lépés) keresztül, mert az ügyfél ezután létre kell hoznia egy virtuális intelligens kártyaeszközként.

12. Az ügyfél az intelligens kártya házirend átveszi a profilsablon (a 3. lépésben kiválasztott sablon UUID használatával):

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. Az intelligens kártya házirend az alapértelmezett adminisztrációs kulcsot a "DefaultAdminKeyHex" tulajdonságban a kártyához fog tartalmazni. Az intelligens kártya létrehozása, amikor az ügyfél a kezdő adminisztrációs kulcsot az intelligens kártya meg ezt a kulcsot.  

14. Az intelligens kártya eszköz hoz létre, akkor az ügyfél azt hozzá kell rendelnie a kérelem:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. A kiszolgáló válaszol, az ügyfél egy újonnan létrehozott intelligens kártya objektum URI-azonosítójú: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. Az ügyfél lekéri az intelligens kártya objektum:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. A "diversifyadminkey" jelző a 12. lépésben beolvasott intelligens kártya házirendben értékének ellenőrzésével az ügyfél tudja diversify az adminisztrációs kulcsot kell.

18. Az ügyfél lekéri a javasolt adminisztrációs kulcsot:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. Az ügyfél kell hitelesítenie a kártya rendszergazdaként az adminisztrációs kulcsot beállításához. Ehhez az szükséges, az ügyfél egy hitelesítési kérdés kér az intelligens kártyával, és elküldi a kiszolgálón:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    A kiszolgáló elküldi a kérdés-válasz a HTTP-válasz törzsében. Keresse meg a hexadecimális karakterlánc, base64 átalakítása, és adja át azt az URL-címben paraméterként. Az URL-címet ad vissza egy másik választ, vissza hexadecimális formátumban kell konvertálni.
<br/>
20. A kiszolgáló elküldi a kérdés-válasz a HTTP-válasz törzsében, és az ügyfél használja a adminisztrátori kulcs diverzifikálása.

21. Az ügyfél azt jelzi, hogy a felhasználó az intelligens kártya házirend "UserPinOption" mezője vizsgálatával biztosítania kell a kívánt PIN-kódjukban.

22. Miután a felhasználó PIN-kódjukban kívánt vállalt egy párbeszédpanel, az ügyfél hitelesítést hajt végre, kérdés-válasz foglaltaknak lépés 19, azzal a különbséggel, előfordulhat, hogy most lehet beállítani a változatos jelzőt "true"értékre:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. Az ügyfél a kiszolgálótól kapott válasz használja a kívánt felhasználói PIN kód megadását.

24. Az ügyfél készen áll a tanúsítvány kérelmek előállítására. Lekéri a profil tanúsítvány generációs Sablonparaméterek annak meghatározásához, hogyan hozható létre a kulcsok kérelmeket.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. A kiszolgáló válaszol, és részletesen leírja a, tanúsítvány rövid kulcsnév, kivonatoló algoritmust, a nyilvánoskulcs-képzési algoritmus, a kulcs mérete, a stb exportálhatóságának egyetlen CertificateRequestGenerationOptions JSON-szerializált objektum. Az ügyfél használja ezeket a paramétereket egy tanúsítványkérelmet létrehozásához.

26. A rendszer egy tanúsítványt kérelmet a kiszolgálóra. Az ügyfél Emellett megadhatja a PFX-jelszót, amellyel bármely PFX-BLOB abban az esetben, ha a tanúsítványsablon határozza meg, hogy a tanúsítványok a hitelesítésszolgáltatóhoz archiválás visszafejtéséhez azaz hitelesítésszolgáltató hoz létre a kulcspár, majd elküldi az ügyfélnek. Az ügyfél is dönthetnek úgy, hogy bizonyos megjegyzéseket.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Néhány másodpercen belül a kiszolgáló válaszol, egyetlen, a JSON-szerializált Microsoft.Clm.Shared.Certificate objektum. A "isPkcs7" jelző ellenőrzésével az ügyfél Tanulja meg, győződjön meg arról, hogy ez a válasz nem a PFX-blob. Az ügyfél kibontja a blob által base64 dekódolás a "pkcs7" karakterláncot, és telepíti azt.

28. Az ügyfél a kérelem befejezettként jelöli meg.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```

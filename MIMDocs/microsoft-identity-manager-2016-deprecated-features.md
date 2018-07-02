---
title: A MIM elavult funkciók és a jövőben megtervezése |} Microsoft Docs
description: Ez a cikk dokumentumok elavult funkciók a MIM Identity Manager 2016 SP1.
keywords: ''
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 2/28/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: a4239f1d69d8a43d70dd38af16e9ef8be62bd33c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288911"
---
# <a name="deprecated-features"></a>Elavult funkciók

Ez a cikk ismerteti a Microsoft Identity Manager 2016 SP1 elavult szolgáltatások. Ha a szolgáltatás továbbra is megtalálhatók a Microsoft Identity Manager, az továbbra is támogatott. Funkciók nem támogatottak az új központi telepítéseknél, a szolgáltatás kiadás eltávolíthatók.  A fejlesztők számára ajánlott nem használata elavult funkciók bármely új alkalmazások és a megoldások.

> [!NOTE]
> Szolgáltatások és funkciók a Microsoft Identity Manager SP1 távolítva azonosítják **. <br>
> További információ a támogatási [életciklusa a Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

A Microsoft nem javasolja az ügyfelek indítsa el a Microsoft BHOLD Suite összetevői a új központi telepítéséhez. BHOLD a meglévő telepítések továbbra is támogatja. Mostantól az Azure AD biztosít [értékelést hozzáférési](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) BHOLD igazolás kampány lehetőségei a helyébe lép.

## <a name="certificate-management"></a>Tanúsítványkezelés 

| **Kategória**                | **Elavult funkció**              | **Csere és leírását**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Kezelőügynökök | ** FIM tanúsítványkezelés | A MIM 2016 a FIM tanúsítvány kezelőügynök el lett távolítva.                                                             |

## <a name="service-and-portal"></a>Szolgáltatás és portál

| **Kategória**                | **Elavult funkció**              | **Csere és leírását**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programozható konfigurációja | Webes szolgáltatás konfigurációs felülete (ma-adatok és mv-adatok) | A következő verzió eltávolítják az ügyfélgépek konfigurálására a FIM szinkronizálási szolgáltatás FIM szolgáltatás webes szolgáltatáson keresztül.
|

## <a name="synchronization-service"></a>Synchronization Service 

| **Kategória**                | **Elavult funkció**              | **Csere és leírását**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programozható konfigurációja | Webes szolgáltatás konfigurációs felülete | A következő verzió eltávolítják az ügyfélgépek konfigurálására a FIM szinkronizálási szolgáltatás a FIM szolgáltatással.                                                          |
| Kezelőügynökök           | Beépített MAs                        | A MIM 2016 el lettek távolítva a következő MAs: </br> 1. ** MA a FIM tanúsítványkezelés </br>2. ** a Lotus Notes MA</br> 3. ** az SAP R/3 MA </br> Lotus Notes és SAP R/3 MAs új verziók helyett. További információkért lásd: [legújabb összekötő Verziókiadások & letöltése](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Kezelőügynökök           | ECMA1                               | A ECMA1/XMA bővítési keretrendszer az ECMA 2.0 lett lecserélve. Meglévő ECMA1 felügyeleti ügynökök frissítése ECMA2.0 összekötőkkel szükség.                                                                                                                                          |
| Kezelőügynökök           | Összekötők a folyamaton fut      | Ez a szolgáltatás nem fogja helyettesíteni. A szinkronizálási szolgáltatás mindig hívja az összekötő ugyanabban a folyamatban. A feladata az összekötő indíthatja el és kezelheti a másik folyamat. |
| Kezelőügynökök           | Konfigurálja a partíció megjelenített neve    | Ez a szolgáltatás nem fogja helyettesíteni. Ez a beállítás csak használatával egy alternatív név megadása a WMI felületeket a partíció.                                                                                                                                                                       |
| Futtatási profilok                | Kombinált profilok                   | A kombinált profilok különbözeti importálás vagy szinkronizálás, a teljes importálás/eltérés szinkronizálása és a teljes importálás vagy szinkronizálás törlődni fog. Akkor kell használni futtatási profilok két lépésben. 

> [!NOTE]
> Csak olyan környezetekben, ahol a teljesítmény csökkenhet volna meglévő disconnectors nagy számú kombinált futtatási profil legyen.


| **Kategória**                | **Elavult funkció**              | **Csere és leírását**           |
|--------|-------|---|    
| Attribútumsorrend | Multi-kezelésére/egyenlő sorrendje                       | Egyenlő sorrend törlődni fog. Ez a funkció nem váltja fel van. Manuális sorrend helyette úgy kell konfigurálni. Továbbra is használja ezt a szolgáltatást, ha a környezetben telepített FIM szolgáltatás felügyeleti ügynök. Ez a kezelőügynök nem export-nem-hivatkozási a deklaratív kiépítés elkerülése érdekében manuális precedenciát biztosítanak. |
| Csatlakozás a szabályok           | A "Bármely" objektumtípus csatlakozás                             | Ez a szolgáltatás nem fogja helyettesíteni. Csatlakozás a szabályok kell definiálhat explicit módon a csatlakoztatni kívánt metaverzum-objektum típusaként.       |
| Attribútumfolyamok      | Törölje a "engedélyezheti a null értékeket" exportált értékek            | Ez a szolgáltatás nem fogja helyettesíteni. "Engedélyezheti a null értékeket" mindig kijelöli. Meg kell győződnie arról, hogy rendelkezik-e "Üres megengedett" a kijelölt az aktuális környezetben.  |
| Attribútumfolyamok      | "Attribútumok visszahívása nem"                            | Ez a szolgáltatás nem fogja helyettesíteni. Attribútumok lesz mindig hívható, amelyek az ajánlott eljárás szerint.  |
| Szabályok bővítmény      | Bővítmény a folyamaton metaverse és ma futtató szabályok | Ez a szolgáltatás nem fogja helyettesíteni. A metaverse és attribútum folyamata szabályok a Szinkronizáló vezérlő meg fog futni.       |
| Szabályok bővítmény      | Tranzakció tulajdonságai                                | Ez a szolgáltatás nem fogja helyettesíteni. Ne adatok átadására a segédprogram osztály használatával bejövő, kiépítési és kimenő szinkronizálási között.  |
| Szabályok bővítmény      | ExchangeUtils: Create55\* módszerek                     | Exchange 5.5 kiszolgálók objektumok létrehozásának törlődni fog.        |
| Illesztő            | Mms_Metaverse                                        | ClmUtils osztály összes tag a következő verziójában törlődni fog.   |

## <a name="next-steps"></a>További lépések
További információk az alábbiakról:

- A Microsoft Identity Manager számos közös vonást mutat elődjével, a Forefront Identity Managerrel. Ha még FIM-et használ, vagy ha további dokumentációra van szüksége, tekintse meg a [FIM 2010 R2 dokumentáció - áttekintés](https://technet.microsoft.com/library/jj133885.aspx) című oldalt.
- [Topológiai szempontok a MIM üzembe helyezésekor](topology-considerations.md) Ez a cikk több üzembe helyezési topológiát mutat, előfordulhat, hogy végrehajtási be.
- [Kapacitástervezési útmutató](capacity-planning-guide.md) ebben az útmutatóban és a tesztkörnyezet használatával megérthetik, hogy az üzembe helyezéshez megfelelő hatókörét.

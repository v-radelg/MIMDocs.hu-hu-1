---
title: A rendszer elavult funkcióit és a jövőbeli tervezést is megtervezte | Microsoft Docs
description: 'Ez a cikk a következő dokumentumok elavult funkcióit dokumentálja: a 2016-es Identity Manager SP1 verziója.'
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fcf9ec8387761b6f154a95d6100ef54a12d4caf8
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518928"
---
# <a name="deprecated-features"></a>Elavult funkciók

Ez a cikk a Microsoft Identity Manager 2016 SP1 elavult funkcióit ismerteti. Ha a szolgáltatás továbbra is szerepel a Microsoft Identity Managerban, továbbra is támogatott. A funkciók nem ajánlottak új központi telepítések esetén, mivel azok a szolgáltatás kiadásában eltávolíthatók.  A fejlesztők számára azt javasoljuk, hogy az elavult funkciókat ne használja bármely új alkalmazásban vagy megoldásban.

> [!NOTE]
> A Microsoft Identity Manager SP1 rendszerből eltávolított szolgáltatások és funkciók a * *-vel vannak azonosítva. <br>
> A [Microsoft Identity Manager támogatási életciklusával](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010) kapcsolatos további információkért


## <a name="bhold"></a>BHOLD 

A Microsoft nem javasolja, hogy az ügyfelek új központi telepítéseket indítsanak a Microsoft BHOLD Suite-összetevőkből. A BHOLD meglévő telepítései továbbra is támogatottak lesznek. Az Azure AD olyan [hozzáférési felülvizsgálatokat](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) biztosít, amelyek lecserélik a BHOLD-igazolási kampány egyes funkcióit.

## <a name="certificate-management"></a>Tanúsítványkezelés 

| **Kategória**                | **Elavult funkció**              | **Helyettesítés és Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Felügyeleti ügynökök | \* * FIM-tanúsítványok kezelése | A FIM tanúsítványkezelő ügynök el lett távolítva a 2016-es webszolgáltatásban.                                                             |

## <a name="service-and-portal"></a>Szolgáltatás és portál

| **Kategória**                | **Elavult funkció**              | **Helyettesítés és Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programozott konfiguráció | Webszolgáltatás konfigurációs felülete (ma-az adathalmaz és az MV-adatforrások) | A FIM szinkronizálási szolgáltatás a FIM szolgáltatás webszolgáltatáson keresztül történő konfigurálásának lehetősége a következő verzióban lesz eltávolítva.
|

## <a name="synchronization-service"></a>Synchronization Service 

| **Kategória**                | **Elavult funkció**              | **Helyettesítés és Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programozott konfiguráció | Webszolgáltatás konfigurációs felülete | A FIM szinkronizálási szolgáltatás a FIM szolgáltatáson keresztül történő konfigurálásának lehetősége a következő verzióban lesz eltávolítva.                                                          |
| Felügyeleti ügynökök           | Beépített MAs                        | A következő MAs el lett távolítva a 2016-es webszolgáltatásban: </br> 1. * * MA a FIM-tanúsítványok kezeléséhez </br>2. * * MA Lotus-megjegyzésekhez</br> 3. * * MA az SAP R/3 esetében </br> A Lotus Notes és az SAP R/3 MAs új verziókkal lett lecserélve. További információ: a [legújabb Connector-verziók kiadási előzményei & Letöltés](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Felügyeleti ügynökök           | ECMA1                               | Az ECMA1/XMA bővíthetőségi keretrendszert a ECMA 2,0 váltotta fel. A meglévő ECMA1-felügyeleti ügynökök ECMA 2.0-összekötővel történő frissítése kötelező.                                                                                                                                          |
| Felügyeleti ügynökök           | A folyamaton kívüli összekötők futtatása      | Ez a szolgáltatás nem lesz lecserélve. A szinkronizációs szolgáltatás mindig ugyanabban a folyamatban hívja meg az összekötőt. Az összekötő feladata a másik folyamat elindítása és kezelése. |
| Felügyeleti ügynökök           | Partíció megjelenítendő nevének konfigurálása    | Ez a szolgáltatás nem lesz lecserélve. Ez a beállítás csak a partíciók alternatív nevének megadására szolgál a WMI-felületeken.                                                                                                                                                                       |
| Futtatási profilok                | Egyesített profilok                   | Az egyesített profilok különbözeti az importálás/szinkronizálás, a teljes importálási/különbözeti szinkronizálás, valamint a teljes importálás/szinkronizálás el lesz távolítva. Ehelyett a futtatási profilokat két lépéssel kell használnia. 

> [!NOTE]
> A kombinált futtatási profilokat csak olyan környezetekben érdemes megőrizni, ahol a teljesítményt nagy számú meglévő leválasztó érinti.


| **Kategória**                | **Elavult funkció**              | **Helyettesítés és Megjegyzés**           |
|--------|-------|---|    
| Attribútumsorrend | Többszörös fölény/egyenlő elsőbbség                       | Az egyenlő prioritás el lesz távolítva. Ehhez a szolgáltatáshoz nem tartozik helyettesítés. Ehelyett manuális prioritást kell beállítania. Továbbra is használhatja ezt a funkciót, ha a környezetében telepítve van egy FIM Service Management-ügynök. Ez a felügyeleti ügynök nem biztosít manuális elsőbbséget a deklaratív kiépítés nem precedens nélküli exportálásának elkerüléséhez. |
| Csatlakozási szabályok           | Csatlakozás "any" típusú objektumhoz                             | Ez a szolgáltatás nem lesz lecserélve. Minden csatlakozási szabálynak explicit módon meg kell határoznia azt a metaverse-objektumtípust, amelyhez csatlakozni próbálnak.       |
| Attribútumok folyamatai      | A "null értékek engedélyezése" jelölőnégyzet kihagyása az exportált értékeknél            | Ez a szolgáltatás nem lesz lecserélve. A "nullák engedélyezése" mindig ki lesz választva. Győződjön meg arról, hogy a jelenlegi környezetben a "nullák engedélyezése" lehetőség van kiválasztva.  |
| Attribútumok folyamatai      | "Attribútumok felidézése"                            | Ez a szolgáltatás nem lesz lecserélve. A rendszer mindig visszahívja az attribútumokat, ami az ajánlott eljárás.  |
| Szabályok bővítmény      | A metaverse és a ma Rules bővítmény futtatása a folyamaton kívül | Ez a szolgáltatás nem lesz lecserélve. A metaverse és az attribútum flow szabályai ugyanabban a folyamatban futnak, mint a szinkronizálási motor.       |
| Szabályok bővítmény      | Tranzakció tulajdonságai                                | Ez a szolgáltatás nem lesz lecserélve. Kerülje az adattovábbítást a beérkező, a kiépítés és a kimenő szinkronizálás között a segédprogram osztály használatával.  |
| Szabályok bővítmény      | ExchangeUtils: Create55\* metódusok                     | Az Exchange 5,5-kiszolgálókhoz létrehozott objektumok létrehozásának módszereit a rendszer eltávolítja.        |
| Interfész            | Mms_Metaverse                                        | A következő verzióban minden ClmUtils-osztály tagja el lesz távolítva.   |

## <a name="next-steps"></a>További lépések
További információk az alábbiakról:

- A Microsoft Identity Manager számos közös vonást mutat elődjével, a Forefront Identity Managerrel. Ha még FIM-et használ, vagy ha további dokumentációra van szüksége, tekintse meg a [FIM 2010 R2 dokumentáció - áttekintés](https://technet.microsoft.com/library/jj133885.aspx) című oldalt.
- A webszolgáltatások [üzembe helyezésének topológiái szempontjai](topology-considerations.md) Ez a cikk több üzembe helyezési topológiát mutat be.
- [Kapacitás-tervezési útmutató](capacity-planning-guide.md) Ez az útmutató a tesztelési környezetekkel együtt az üzemelő példány megfelelő hatókörének megismerésére használható.

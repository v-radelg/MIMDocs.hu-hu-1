---
title: A MIM elavult funkciók és a jövőben tervezése |} A Microsoft Docs
description: Ez a cikk dokumentumok elavult funkciói a MIM Identity Manager 2016 SP1-et.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fcf9ec8387761b6f154a95d6100ef54a12d4caf8
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358435"
---
# <a name="deprecated-features"></a>Elavult funkciók

Ez a cikk ismerteti az elavult funkciók a Microsoft Identity Manager 2016 SP1. Ha a szolgáltatás a Microsoft Identity Manager továbbra is megtalálható, továbbra is támogatott. Akkor lehet, hogy eltávolítja az új funkciók nem ajánlott új példányok üzembe helyezéséhez.  A fejlesztők javasoljuk, hogy nem minden olyan új alkalmazások vagy megoldások funkcióit használó elavult.

> [!NOTE]
> Szolgáltatások és funkciók a Microsoft Identity Manager SP1 eltávolítva az azonosított **. <br>
> További információ a támogatási [életciklus Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

A Microsoft nem javasolja a vásárló először a Microsoft a BHOLD Suite összetevők új központi telepítéséhez. A BHOLD a meglévő telepítések továbbra is támogatottak. Most már az Azure AD biztosítja [hozzáférési felülvizsgálatokkal](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) BHOLD igazolási kampány funkcióit cserélje le.

## <a name="certificate-management"></a>Tanúsítványkezelés 

| **Kategória**                | **Elavult funkció**              | **Csere és Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Felügyeleti ügynökök | ** FIM tanúsítványkezelés | FIM tanúsítvány felügyeleti ügynök el lett távolítva a MIM 2016-ban.                                                             |

## <a name="service-and-portal"></a>Szolgáltatás és portál

| **Kategória**                | **Elavult funkció**              | **Csere és Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programozható konfigurációja | Webalkalmazás-szolgáltatás konfigurációs felület (ma-adatok és mv-adatok) | A FIM szinkronizálási szolgáltatás FIM szolgáltatás webszolgáltatással beállításával egy következő verziójában törlődni fog.
|

## <a name="synchronization-service"></a>Synchronization Service 

| **Kategória**                | **Elavult funkció**              | **Csere és Megjegyzés**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programozható konfigurációja | Webalkalmazás-szolgáltatás konfigurációs felület | Lehetővé teszi a FIM szinkronizálási szolgáltatás a FIM szolgáltatás konfigurálása a következő verzió törlődni fog.                                                          |
| Felügyeleti ügynökök           | Beépített MAs                        | A következő MAs el lettek távolítva a MIM 2016-ban: </br> 1. ** a tanúsítványok kezeléséhez FIM MA </br>2. ** a Lotus Notes MA</br> 3. ** MA az SAP R/3 </br> Lotus Notes és SAP R/3 MAs az új verziók helyett. További információkért lásd: [legújabb Connector Verziókiadásai és letöltés](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Felügyeleti ügynökök           | ECMA1                               | A ECMA1/XMA bővíthetőségi keretrendszerét felváltotta a ECMA 2.0. Meglévő ECMA1 felügyeleti ügynökök frissítése a ECMA2.0 összekötőkkel szükség.                                                                                                                                          |
| Felügyeleti ügynökök           | Összekötők – folyamaton kívüli fut      | Ez a funkció nem pótolhatók. A szinkronizálási szolgáltatás mindig meghívja az összekötő ugyanabban a folyamatban. A feladata az összekötő elindításához és a többi folyamat kezeléséhez. |
| Felügyeleti ügynökök           | Konfigurálja a partíció megjelenített neve    | Ez a funkció nem pótolhatók. Ez a beállítás csak használt adjon meg egy alternatív nevet egy partíciónak a WMI felületeket.                                                                                                                                                                       |
| Futtatási profilok                | Kombinált profilok                   | A kombinált profilok különbözeti importálás vagy szinkronizálás, a teljes importálás/különbözeti szinkronizálás és a teljes importálás vagy szinkronizálás törlődni fog. Futtatási profilok inkább két lépést ajánlott használni. 

> [!NOTE]
> Csak olyan környezetekben, ahol a lenne lehet a teljesítményre meglévő disconnectors nagy számú kombinált futtatási profilokat kell tartania.


| **Kategória**                | **Elavult funkció**              | **Csere és Megjegyzés**           |
|--------|-------|---|    
| Attribútumsorrend | Multi-kapcsolatos szakismeretek/egyenlő sorrend                       | Azonos prioritású törlődni fog. Ez a funkció nincs helyettesítő van. Manuális elsőbbséget ehelyett kell konfigurálni. Továbbra is használják ezt a funkciót, ha a környezetben van üzembe helyezve a FIM szolgáltatás kezelőügynöke. Ez a kezelőügynök nem biztosít manuális elsőbbséget export-not-hivatkozási a deklaratív kiépítés elkerülése érdekében. |
| Csatlakozzon a szabályok           | Csatlakozás a "Bármely" Objektumtípus                             | Ez a funkció nem pótolhatók. Az összes illesztési szabályok explicit módon meg kell határoznia a csatlakoztatni kívánt metaverzum-objektum típusaként.       |
| Attribútumfolyamok      | Törölje a "engedélyezheti a null értékeket" exportált értékek            | Ez a funkció nem pótolhatók. "Engedélyezheti a null értékeket" mindig legyen kiválasztva. Győződjön meg arról, hogy a "NULL értékek engedélyezése" az aktuális környezet kiválasztott.  |
| Attribútumfolyamok      | "Attribútumok visszahívása nem"                            | Ez a funkció nem pótolhatók. Attribútumok fog mindig hívhatók vissza, amelynek ez az ajánlott eljárás.  |
| Szabályok bővítmény      | Metaverzum és ma-bővítmény – folyamaton kívüli szabályok | Ez a funkció nem pótolhatók. A metaverzum és attribútum szabályok ugyanabban a folyamatban, a Szinkronizáló vezérlő fog futni.       |
| Szabályok bővítmény      | Tranzakció tulajdonságai                                | Ez a funkció nem pótolhatók. Ez az osztály segédprogram használatával a bejövő, a kiépítési és a kimenő szinkronizálási közötti adatok átadására kerülendő.  |
| Szabályok bővítmény      | ExchangeUtils: Create55\* módszerek                     | 5.5-ös Exchange-kiszolgálók objektumok létrehozásának törlődni fog.        |
| Kapcsolat            | Mms_Metaverse                                        | A következő verzió összes ClmUtils osztálytagjaihoz törlődni fog.   |

## <a name="next-steps"></a>További lépések
További információk az alábbiakról:

- A Microsoft Identity Manager számos közös vonást mutat elődjével, a Forefront Identity Managerrel. Ha még FIM-et használ, vagy ha további dokumentációra van szüksége, tekintse meg a [FIM 2010 R2 dokumentáció - áttekintés](https://technet.microsoft.com/library/jj133885.aspx) című oldalt.
- [Topológiai szempontok a MIM üzembe helyezésekor](topology-considerations.md) Ez a cikk több lehetséges telepítési topológiákat be.
- [Kapacitástervezési útmutató](capacity-planning-guide.md) ebben az útmutatóban és a tesztkörnyezet segítségével ismerje meg az üzembe helyezés megfelelő hatókörét.

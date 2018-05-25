---
title: BHOLD SP1 telepítése |} Microsoft Docs
description: BHOLD SP1 dokumentáció
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: e0514530c9bceef18cc8eea7ec8b7060110811c2
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/25/2018
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Microsoft BHOLD Suite SP1 (6.0) a telepítési útmutató

Microsoft® BHOLD Suite Service Pack 1 (SP1) gyűjteménye, az alkalmazások, hogy a Microsoft Identity Manager 2016 SP1 (MIM) használatakor ad hozzá hatékony szerepkörkezelés, elemzés és igazolási MIM. Microsoft BHOLD Suite SP1 a következő modulok áll:

- BHOLD Core
- Access Management-összekötő
- BHOLD FIM vagy MIM-integráció
- BHOLD modellhez generátor
- BHOLD elemzési
- BHOLD-jelentés
- BHOLD-igazolás


>[!NOTE]
**Érvényes**: a Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Ez a dokumentum ismerteti

Ez a dokumentum azt ismerteti, hogyan az üzleti igényeknek, és minden BHOLD-modul telepítése a BHOLD központi telepítésének megtervezéséhez. Minden modul, a megfelelő hardver, a infrastruktúra- és szoftverkövetelményeit, előtelepítési hálózati konfiguráció a telepítés során szükséges információkat, és teendőkkel lépéseket, ha van ilyen, részletes leírást.

## <a name="pre-requisite-knowledge"></a>Az előfeltételként Tudásbázis

Jelen dokumentum céljából feltételezzük, hogy rendelkezik-e egy alapszinten megértse, hogyan szoftver telepítéséhez a számítógépén. Azt is feltételezi, hogy rendelkezik-e az Active Directory® tartományi szolgáltatások, a Microsoft Identity Manager SP1 (FIM) és a Microsoft SQL Server 2008 adatbázis-szoftver alapszintű ismeretét. Állítsa be, és függő technológiák, például Active Directory tartományi szolgáltatások és a FIM konfigurálása leírása nem része a jelen dokumentáció. A funkciók a Microsoft BHOLD modulok végző kapcsolatos információkért lásd: [a Microsoft BHOLD suite fogalmak útmutató](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Célközönség

Ez a dokumentum célja az IT-tervezők, rendszermérnökök, technológia döntéshozók, tanácsadókkal, infrastruktúra tervezők és informatikai részleg nehezebben, akik a Microsoft BHOLD Suite telepítése.

## <a name="bhold-infrastructure-considerations"></a>BHOLD infrastruktúra kapcsolatos szempontok

Leggyakrabban az BHOLD és FIM fogja használni a nagyméretű infrastruktúrával környezetekben. Az adott üzleti igényeknek megfelelő BHOLD és a FIM architektúrák igazíthatja. Az alábbiakban néhány lehetséges architekturális megoldás. Ez az Áttekintés nem minden lehetséges beállításainak átfogó listáját, de a hálózaton telepítheti a BHOLD módszereket javasol.
 
Ez a szakasz témakörei a következők:

- Egykiszolgálós architektúrája
- Kettős-kiszolgáló architektúrája
- kétrétegű architektúra
- Az SQL Serverre vonatkozó ajánlások

### <a name="single-server-architecture"></a>Egykiszolgálós architektúrája

Központi telepítést, kisebb szervezetek vagy fejlesztési célokra szolgál telepítheti BHOLD és a FIM SQL Server és Active Directory tartományi szolgáltatások, mint ugyanazon a kiszolgálón az alábbi ábrán látható módon.
 
![Egyetlen kiszolgáló architektúrája](media/bhold-installation-guide/single.png)

Telepítésekor BHOLD Suite SP1 és a FIM-portál egyszerre egy kiszolgálón kell létrehoznia másik gazdagépet aliasok (CNAME vagy A rekord) a DNS-ben BHOLD és a FIM. Ez lehetővé teszi, hogy külön álló egyszerű szolgáltatásnevek (SPN) a BHOLD és a FIM szolgáltatás kell létrehozni. További információkért lásd: [BHOLD Core telepítés](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
FIM egykiszolgálós konfiguráció telepítésével kapcsolatos útmutatásért lásd: [első lépések útmutatók általános beállításai](https://technet.microsoft.com/library/ff575965.aspx) a Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Kettős-kiszolgáló architektúrája

BHOLD Core és a FIM telepítése különálló kiszolgálókra biztosít nagyobb teljesítményt és rugalmasságot biztosít, amelyek nem igényelnek egy összetettebb központi telepítést végez, például többrétegű architektúrák által biztosított közepes méretű szervezeteknek. A következő ábrán látható BHOLD és a FIM telepített saját kiszolgálón; SQL Server adatbázis-szolgáltatások biztosításához BHOLD és a FIM is fut a FIM-kiszolgáló. A FIM szinkronizálási szolgáltatás a FIM-kiszolgálón futó szinkronizálja a módosításokat a FIM és BHOLD-adatbázis között. Vegye figyelembe, hogy végfelhasználói önkiszolgáló szükség, ha a BHOLD FIM integrációs modul telepíteni kell a FIM szolgáltatás és a FIM-portál ugyanarra a kiszolgálóra. A BHOLD FIM integrációs modul megköveteli, hogy a FIM szolgáltatás és a BHOLD FIM integrációs modul telepítve vannak ugyanarra a kiszolgálóra.

![Két kiszolgáló architektúrája](media/bhold-installation-guide/dual.png)

>[!IMPORTANT]
BHOLD FIM integrációs modul a jelentéskészítési szolgáltatás szükséges a BHOLD és a FIM adatbázisok ugyanazon SQL Server-példányra kell telepíteni, és a BHOLD szolgáltatásfióknak a FIM szolgáltatás adatbázisához hozzáférési jogosultsággal kell rendelkeznie.

### <a name="two-tier-architecture"></a>kétrétegű architektúra

A legtöbb környezetben, különösen ha teljesítmény fontos, futtatnia kell a BHOLD Suite SP1, a FIM és az SQL Server eltérő kiszolgálókra (kétrétegű architektúra). Egy kétrétegű architektúrával memória és CPU-erőforrást az egyes rétegekhez vannak fenntartva. A következő ábrán egy kétrétegű architektúra konfigurálása egyik lehetséges módja. A FIM szinkronizálási szolgáltatás a FIM-kiszolgálón futó szinkronizálja a módosításokat a FIM és BHOLD-adatbázis között. Vegye figyelembe, hogy végfelhasználói önkiszolgáló szükség, ha a BHOLD FIM integrációs modul telepíteni kell a FIM szolgáltatás és -portál ugyanarra a kiszolgálóra.

![kétrétegű architektúra](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Az SQL Serverre vonatkozó ajánlások

Ha egy nagy szervezet BHOLD telepíti, javasoljuk, hogy a Microsoft SQL Server adatbázisának beállítása az alábbi irányelvek követése:

- FIM vagy BHOLD szolgáltatások különálló kiszolgálóra telepítse az SQL Servert.
- Különítse el a naplófájl a fizikai lemez szinten az adatok fájlból.
- Amennyiben RAID adattároló redundanciája, amely arra használ, használnia RAID 10 (1 + 0). Ne használjon RAID 5. szint.
- Ne felejtse el legfeljebb 2 GB fizikai memória használata az SQL Server rendszert futtató kiszolgáló esetén adja meg a megfelelő beállításokat.
- BHOLD optimális teljesítmény érdekében használja a Microsoft SQL Server 2008 R2 vagy újabb.

SQL Server ajánlott eljárásaival kapcsolatos további információkért lásd: [tárolási felső 10 gyakorlati tanácsok](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) a Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Megbízható tanúsítványok listában frissítés

Windows beállítható úgy, hogy a szolgáltatás indítása előtt tanúsítványláncok ellenőrzése. Ilyen rendszerek esetében a szolgáltatás nem indítható el, ha a szolgáltatás végrehajtható kód egy tanúsítvánnyal, amely nem szerepel a kiszolgáló megbízható tanúsítványok listáját (TCL) alá volt írva. A Microsoft BHOLD Suite SP1 szoftvere kódaláíró tanúsítványláncra, amely a Microsoft legfelső szintű tanúsítvány hatóság 2010 tanúsítvánnyal származik használatával aláírt kód.
Windows beállítható úgy, hogy a legfelső szintű tanúsítványok beolvasása a Microsoft Internet-kapcsolaton keresztül. Leválasztott rendszeren azonban a Windows Server tartalmazza a csak azok a tanúsítványok jelen volt a a legfelső szintű program előtt kiadott Windows egyszerre. Windows Server előtt a Windows Server 2010 kiadásaiban ezeket a tanúsítványokat nem tartalmazza a legfelső szintű tanúsítvány szükséges a BHOLD Suite SP1 kódaláíró tanúsítványlánc érvényesítésekor. Egy vagy több Microsoft BHOLD Suite SP1 modulok, előfordulhat, hogy nincs egy naprakész TCL rendszeren telepíteni kívánja, esetén kell töltse le és a legfelső szintű-frissítés telepítéséhez, vagy a csoportházirend a gyökér-frissítés telepítéséhez, egy BHOLD Suite SP1 telepítése előtt a modul. További információkért lásd: [Windows gyökérkönyvtár certificate program tagjai](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>BHOLD Suite SP1 verziójának telepítése a Windows Server 2012/2016 szükséges lépés 

![IIS telepítése BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

A Windows Server 2012 vagy 2016 BHOLD Suite SP1 telepítése esetén a BHOLD weblapok nem lesz elérhető amíg található applicationHost.config fájlt módosítása ```C:\Windows\System32\inetsrv\config```. Az a ```<globalModules>``` területen írja be ```preCondition="bitness64``` kezdődő bejegyzést ```<add name="SPNativeRequestModule"``` , hogy olvassa be, hogy az alábbiak szerint:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Szerkesztése, és a fájl mentése után futtassa az iisreset parancsot alaphelyzetbe az IIS-kiszolgálón.


## <a name="upgrading-bhold-suite"></a>BHOLD Suite frissítése

Egy meglévő BHOLD Suite-telepítés nem frissíthető. Ehelyett távolítsa el egy meglévő BHOLD Suite telepítése BHOLD modulok frissítése előtt. Ha egy meglévő BHOLD szerepkör modell, a BHOLD-adatbázis frissítése, és használatra, ha a frissített BHOLD Core-modul telepítése. További információkért lásd: [BHOLD Suite cseréje BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>További lépések

- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

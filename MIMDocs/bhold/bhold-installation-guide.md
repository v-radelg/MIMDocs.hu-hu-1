---
title: A BHOLD SP1 telepítése |} A Microsoft Docs
description: A BHOLD SP1 dokumentáció
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: e73596ea1b07814a46d638ac705edf5fdada76a2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334346"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>A Microsoft a BHOLD Suite SP1 (6.0) telepítési útmutató

Microsoft® BHOLD Suite Service Pack 1 (SP1) alkalmazások gyűjteménye, amely a Microsoft Identity Manager 2016 SP1 (MIM) használatakor ad hozzá hatékony szerepkörkezelés, elemzési és állapotigazolási MIM. A Microsoft BHOLD Suite SP1 a következő modulok áll:

- A BHOLD Core
- Access Management-összekötő
- BHOLD FIM vagy MIM-integráció
- A BHOLD Model Generator
- A BHOLD Analytics
- A BHOLD Reporting
- A BHOLD-igazolás


> [!NOTE]
> **Érvényes**: a Microsoft Identity Manager 2016 SP1-et

## <a name="what-this-document-covers"></a>Ez a dokumentum ismerteti

Ez a dokumentum ismerteti, hogyan lehet az üzleti igényeknek, és minden BHOLD-modul telepítéséhez az BHOLD üzembe helyezésének megtervezése. Minden modul, a megfelelő hardver, az infrastruktúra és a szoftverkövetelményekről előtelepítési hálózat konfigurálására a telepítés során szükséges információkat, és teendőkkel lépéseket, ha van ilyen, részletes leírást talál.

## <a name="pre-requisite-knowledge"></a>Előfeltételként Tudásbázis

Jelen dokumentum céljából feltételezzük, hogy miként telepíthet szoftvereket a kiszolgáló-számítógépek alapvető ismeretekkel. Azt is feltételezi, hogy rendelkezik-e az Active Directory® tartományi szolgáltatások, a Microsoft Identity Manager SP1 (FIM) és a Microsoft SQL Server 2008 adatbázis-szoftver alapszintű ismerete. A leírás és függő technológiák, például Active Directory tartományi szolgáltatások és a FIM beállítása a ebben a dokumentációban hatókörén kívül esik. A Microsoft a BHOLD-modulok végző függvényeivel kapcsolatos további információkért lásd: [fogalmak útmutató a Microsoft a BHOLD suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Célközönség

Ez a dokumentum szól IT-tervezők, rendszermérnököknek, technológiai döntéshozók, tanácsadók, infrastruktúra-tervezők és informatikai munkatársak, akik a Microsoft a BHOLD Suite telepítése.

## <a name="bhold-infrastructure-considerations"></a>A BHOLD infrastruktúrával kapcsolatos szempontok

Leggyakrabban a BHOLD és a FIM szolgálnak a nagyméretű infrastruktúrával környezetben. A BHOLD és a FIM architektúra üzleti igényei szerint testre szabhatja. Az alábbiakban néhány lehetséges architekturális megoldásokat. Ez az Áttekintés nem minden lehetséges beállítást átfogó listáját, de a hálózaton telepítheti a BHOLD módszereket javasol.
 
Ez a szakasz témakörei a következők:

- Egyetlen kiszolgálóból álló architektúra
- Kettős-kiszolgáló architektúra
- Kétrétegű architektúrája
- Javaslatok az SQL Serverhez

### <a name="single-server-architecture"></a>Egyetlen kiszolgálóból álló architektúra

Központi telepítés kisebb szervezetek vagy fejlesztési célokra telepíthető BHOLD és a FIM ugyanarra a kiszolgálóra, az SQL Server és Active Directory tartományi szolgáltatások, az alábbi ábrán látható módon.
 
![Egyetlen kiszolgáló architektúrája](media/bhold-installation-guide/single.png)

BHOLD Suite SP1 együtt települnek a FIM-portálon, egyetlen kiszolgálón, amikor kell létrehoznia különböző gazdagép aliasok (CNAME és a egy rekord) a DNS-ben BHOLD és a FIM. Ez lehetővé teszi a külön szolgáltatás egyszerű szolgáltatásnevét (SPN) a BHOLD és a FIM szolgáltatás kell létrehozni. További információkért lásd: [BHOLD Core telepítése](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
FIM egykiszolgálós konfiguráció telepítését, tekintse át [gyakran alkalmazott konfiguráció az első lépéseket ismertető útmutatókban](https://technet.microsoft.com/library/ff575965.aspx) a Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Kettős-kiszolgáló architektúra

A BHOLD Core és a FIM telepítése különálló kiszolgálón biztosít a nagyobb teljesítmény- és közepes méretű szervezetek számára, amelyek nem igényelnek egy összetettebb központi telepítési, például a többrétegű architektúrák által biztosított rugalmasság. A következő ábrán látható BHOLD és a FIM-kiszolgálókra telepített saját; a FIM-kiszolgáló is fut az SQL Server és a BHOLD FIM adatbázis-szolgáltatások biztosításához. A FIM szinkronizálási szolgáltatás a FIM-kiszolgálón futó szinkronizálja a módosításokat a FIM és BHOLD-adatbázis között. Vegye figyelembe, hogy a végfelhasználói önkiszolgáló szükség, ha a BHOLD FIM integrációs modulnak telepítve kell lennie a FIM szolgáltatás és a FIM-portál ugyanazon a kiszolgálón. A BHOLD FIM integrációs modul megköveteli, hogy a FIM szolgáltatás és a BHOLD FIM integrációs modul telepítve ugyanarra a kiszolgálóra.

![Kettős kiszolgáló architektúrája](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> A BHOLD FIM integrációs modul a jelentési szolgáltatás szükséges a BHOLD és a FIM adatbázisok SQL Server-példányon kell telepíteni, és a BHOLD szolgáltatásfióknak kell rendelkeznie a FIM szolgáltatás adatbázisához való hozzáférési jogosultságok.

### <a name="two-tier-architecture"></a>Kétrétegű architektúrája

A legtöbb környezetben, különösen fontos a teljesítmény, ahol kell futtatásakor a BHOLD Suite SP1, a FIM és az SQL Server eltérő kiszolgálókra (kétrétegű architektúra). Kétrétegű architektúrával memória- és Processzor-erőforrások dedikált az egyes rétegekhez. Az alábbi ábrán egy kétrétegű architektúrát konfigurálásának egyik lehetséges módja. A FIM szinkronizálási szolgáltatás a FIM-kiszolgálón futó szinkronizálja a módosításokat a FIM és BHOLD-adatbázis között. Vegye figyelembe, hogy a végfelhasználói önkiszolgáló szükség, ha a BHOLD FIM integrációs modulnak telepítve kell lennie a FIM szolgáltatás és portál ugyanazon a kiszolgálón.

![kétrétegű architektúrája](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Javaslatok az SQL Serverhez

Ha egy nagy szervezet BHOLD helyez üzembe, azt javasoljuk, hogy ezeket az irányelveket a Microsoft SQL Server-adatbázis beállításához kövesse:

- Az SQL Server telepítése a FIM vagy BHOLD szolgáltatások külön kiszolgálón.
- Különítse el a naplófájl a fizikai lemez szintjén az adatfájlból.
- Ha a RAID használatával adja meg a tárhely-redundancia, használja a RAID-szinteket követelnek 10 (1 + 0). Ne használjon RAID 5. szint.
- Mindenképpen adja meg a megfelelő beállításokat, több mint 2 GB fizikai memória az SQL Servert futtató kiszolgáló használatakor.
- A BHOLD optimális teljesítményt, használja a Microsoft SQL Server 2008 R2 vagy újabb.

Az SQL Server ajánlott eljárásokkal kapcsolatos további információkért lásd: [tárolási Top 10 – gyakorlati tanácsok](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) a Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>A megbízható tanúsítványok lista frissítése

Windows beállítható úgy, hogy a szolgáltatás indítása előtt tanúsítványláncok ellenőrzése. Az ilyen rendszerek a szolgáltatás nem indítható el, ha a szolgáltatás a végrehajtható kód írták-e olyan tanúsítványt, amely nem szerepel a megbízható tanúsítványok listája (TCL) annak a kiszolgálónak. A Microsoft BHOLD Suite SP1 szoftver használatával kódaláíró tanúsítványláncra, amely származik, a Microsoft legfelső szintű tanúsítvány hatóság 2010 tanúsítvánnyal aláírt kód.
Windows legfelső szintű tanúsítványok lekérése a Microsoft internetkapcsolatra keresztül konfigurálhatók. Egy kapcsolat nélküli rendszerben azonban a Windows Server tartalmaz a csak azok a tanúsítványok jelen, a legfelső szintű programban egyszerre előtt kiadott Windows volt. A Windows Server a Windows Server 2010 előtti kiadásaiban ezek a tanúsítványok nem tartalmazza a legfelső szintű tanúsítvány szükséges a BHOLD Suite SP1 code aláíró tanúsítványlánc érvényesítésekor. Ha azt tervezi, a rendszer, amely egy naprakész TCL előfordulhat, hogy rendelkezik egy vagy több Microsoft BHOLD Suite SP1 moduljainak telepítése, meg kell letöltése és telepítése a főtanúsítvány-csomag, vagy egy BHOLD Suite SP1 telepítése előtt a gyökér-frissítési csomag telepítése a csoportházirenddel a modul. További információkért lásd: [Windows legfelső szintű tanúsítvány program tagjai](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>A BHOLD Suite SP1 telepítése a Windows Server 2012/2016 szükséges lépés 

![Az IIS telepítése a BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

A Windows Server 2012 vagy 2016 BHOLD Suite SP1 telepítése esetén a BHOLD weblapok nem lesz elérhető amíg nem módosítja az applicationHost.config fájlban található ```C:\Windows\System32\inetsrv\config```. Az a ```<globalModules>``` területen ```preCondition="bitness64``` -bejegyzést, amely a kezdődik ```<add name="SPNativeRequestModule"``` úgy, hogy azt a következőképpen:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Szerkesztése, és a fájl mentése után futtassa az iisreset parancsot alaphelyzetbe az IIS-kiszolgálón.


## <a name="upgrading-bhold-suite"></a>A BHOLD Suite frissítése

Egy meglévő BHOLD Suite telepítése nem frissíthető. Ehelyett el kell távolítania egy meglévő BHOLD Suite telepítése BHOLD-modulok frissítése előtt. Ha rendelkezik egy meglévő BHOLD szerepkör modell, a BHOLD-adatbázis frissítése, és használja azt a frissített BHOLD Core-modul telepítésekor. További információkért lásd: [BHOLD Suite cseréje BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>További lépések

- [BHOLD fejlesztői leírás](../reference/mim2016-bhold-developer-reference.md)
- [A BHOLD korábbi verziói](../reference/version-bhold-history.md)

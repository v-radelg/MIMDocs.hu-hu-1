---
title: A Microsoft Identity Manager licencelése és letöltések |} A Microsoft Docs
description: Ez a cikk ismerteti a megközelítések a szoftverek letöltési helye a Microsoft Identity Manager (MIM) 2016 a mutatók licenckezeléshez.
keywords: ''
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 9e74b7ccf332c1572ce395fad18b8629673c756a
ms.sourcegitcommit: 4f0b2883922bcb8fbef6b4284c35c6ca62c11565
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/27/2019
ms.locfileid: "56954548"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>A Microsoft Identity Manager 2016 licencelése és letöltések

Ez a cikk ismerteti a megközelítések a szoftverek letöltési helye a Microsoft Identity Manager (MIM) 2016 a mutatók licenckezeléshez.

## <a name="licensing-mim-for-your-organization"></a>A szervezet licencelési MIM

A Microsoft Identity Manager 2016 felhasználónkénti alapon licencelt.  A licencelési rendszer adatait, a termék használati feltételeiben szereplő, és a kapcsolódó dokumentumok, amely letölthető a [licencfeltételek](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx) lap.

### <a name="licensing-for-azure-ad-premium-customers"></a>Az Azure AD Premium-ügyfelek a licencelés

A Microsoft Identity Manager 2016 megtalálható az Azure Active Directory Premium (P1 és P2), amelynek része az Enterprise Mobility + Security.

Az Azure AD Premium keresztül érhető el egy [Microsoft nagyvállalati szerződés](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), a [Open mennyiségi licencprogramok](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), és a [Cloud Solution Providers](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) program. Az Azure és Office 365-előfizetők is is megvásárolhatják az Azure Active Directory Premium P1 és P2.  További információk: [Azure Active Directory díjszabását ismertető lapon](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### <a name="mim-cals"></a>A MIM ügyféllicencek

Ha nem rendelkezik Azure Active Directory Premium-előfizetések a felhasználók számára, és további MIM képességek mellett a szinkronizálás használ majd egy [ügyféllicenc (CAL)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) szükség minden olyan felhasználóhoz, akinek identitása felügyelt a mim szoftverben. Ha azt szeretné, hogy a külső felhasználók – például az üzleti partnerek, alvállalkozók külső vagy ügyfelek – tudják elérni a MIM, CAL-EK beszerezni a külső felhasználók számára, vagy külső csatlakozó (EC-) licenc beszerzése. A Microsoft Identity Manager 2016 CAL-EK nem szükségesek a felhasználók számára, akinek identitása csak a Microsoft Identity Manager synchronization service, és nem felügyelt egyéb MIM-összetevő.

### <a name="licenses-for-platform-components"></a>A webplatform-összetevők licencek

A Microsoft Identity Manager 2016 kiszolgáló szoftvereket használ, a Windows Server-bővítmény egy Windows Server-licenc szükséges. És a MIM üzembe helyezése megköveteli egy SQL Server-telepítést.  A Windows Server és SQL Server-licencek nem szerepelnek a MIM.

## <a name="obtaining-mim-software"></a>A MIM szoftver beszerzése

Egy korábbi MIM vagy frissítés egy új telepítés előtt győződjön meg arról, hogy a legújabb verziójú.

Új telepítést rendszertől, ha szüksége lesz a minden MIM-összetevő, amely a forgatókönyvéhez kapcsolódó telepítési fájlok letöltési. Ezután le ezeket a fájlokat a frissítéseket, és töltse le a letöltőközpontból önálló letöltés további összetevőket.


| Forgatókönyv | Összetevő | A forgatókönyvhöz szükséges? | DVD ISO-mappa neve | Megjegyzések |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Szinkronizálás| A szinkronizálási szolgáltatás (beleértve az AD-összekötő) | Igen | `Synchronization Service` | |
| Szinkronizálás | PCNS | Nem | `Password Change Notification Service` |  A tartományvezérlőkön lehet telepíteni |
| Szinkronizálás | Graph-LDAP-, SQL, Web Services, a PowerShell-lel, Lotus Domino-összekötő | Nem | – | Letöltőközpont keresztül |
| az emelt szintű hozzáférések felügyeletével | MIM szolgáltatás | Igen | `Service and Portal` | |
| Önkiszolgáló | A MIM szolgáltatás, a MIM-portál | Igen | `Service and Portal` | |
| Önkiszolgáló | Beépülő modulok és bővítmények | Nem | `Add-ins and extensions` | A végfelhasználói számítógépeken telepíteni |
| Önkiszolgáló | SCSM Reporting | Nem | `Data Warehouse Support Scripts` | |
| Önkiszolgáló | A hibrid jelentéskészítő ügynök | Nem | – | Letöltőközpont keresztül |
| Önkiszolgáló | Nyelvi csomagok | Nem | `LANGUAGE Packs` | |
| Tanúsítványkezelés | CM | Igen | `Certificate Management` | |
| Tanúsítványkezelés | CM csoportos ügyfél | Nem | `CM Bulk Client` | |
| Tanúsítványkezelés | CM-ügyfél | Nem | `CM Client`  | |
| Tanúsítványkezelés | A Windows a Tanúsítványkezelő alkalmazás | Nem | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Windows installer-csomag beszerzése

Új telepítés esetén a legtöbb szervezet a MIM telepítési csomagok letöltését a [mennyiségi Licencszolgáltatási központ](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


A DVD ISO-fájl tartalmaz egy mappát az egyes MIM-összetevők: Szinkronizálási szolgáltatás, szolgáltatás és -portál, stb. Ha azt tervezi, ahonnan letöltötte azt egy másik számítógépen a szoftver telepítéséhez, ügyeljen arra, hogy a teljes ISO-fájlt vagy a mappa az összetevő másolása: nem csupán másolja a csak egy MSI-fájl egy mappából a többi a fájlok és almappák nélkül.

Ha nem rendelkezik hozzáféréssel a mennyiségi Licencszolgáltatási központjából, megfelelő fejlesztői előfizetéssel rendelkező ügyfelek is letöltheti a MIM 2016 SP1-et, az ISO-fájl a [Visual Studio a saját előnyei letölti](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=).  Keresse meg "A Microsoft Identity Manager 2016 Service Pack 1".  

Ha nem rendelkezik hozzáféréssel a mennyiségi Licencszolgáltatási központjából, és csupán szeretné próbálja ki a MIM szoftvert csak korlátozott ideig, letöltheti egy [a MIM 2016 próbaverzióját](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Ezt a szoftvert nem éles használatra szánt első telepítés után 180 nappal a művelethez használandó megszűnik, és nem lehet frissíteni. A próbaverzió a Windows Server 2008 R2, Windows Server 2012 vagy Windows Server 2012 R2 telepítéséhez szükséges.  Ha most ismerkedik a MIM és a technológia megismerésére, vegye figyelembe, hogy a MIM-forgatókönyvekhez szükséges Active Directory-tartományban, a Windows Server és SQL Server jelen. Ha nem rendelkezik a Windows Server vagy SQL Server már jelen van, akkor lehet kipróbálnia [az SQL Server 2016 és Windows Server 2016 virtuális gép kiépítése](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Frissítések beszerzése

MSI a MIM telepítése után mellett a szükséges gyorsjavításokat kell telepítenie.

Ellenőrizze a [Identity Manager verziókiadások](./reference/version-history.md) található a hivatkozás a helyet, a installer javítókészletfájlokat legutóbbi frissítés kiadásban.

Hogy mely fájlokat szükség, ez a táblázat felsorolja az összetevők és a egy frissítés a megfelelő javítás (MSP) fájl neve.

| Forgatókönyv | Összetevő | DVD ISO-mappa neve | Megfelelő frissítési javítás fájl neve |
|----------|-----------|-   |-------------------|----------|--------------|
|Szinkronizálás| A szinkronizálási szolgáltatás | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Önkiszolgáló | A MIM szolgáltatás, a MIM-portál | `Service and Portal` | `FIMService_x64*msp` |
| Önkiszolgáló | Beépülő modulok és bővítmények | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Önkiszolgáló | Nyelvi csomagok | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Hozzáférés-kezelés (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Tanúsítványkezelés | CM |  `Certificate Management` | `FIMCM*.msp` |
| Tanúsítványkezelés | CM csoportos ügyfél |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Tanúsítványkezelés | CM-ügyfél | CM-ügyfél |`FIMCMClient*msp` |

Mindenképpen olvassa el a kibocsátási megjegyzéseket, társítva a frissítést az MSP-fájl telepítése előtt.

Frissítések [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) MSP-fájlok csak az MSI-telepítőket, mint nincsenek elosztva.

### <a name="additional-downloads"></a>További letöltések

Az alábbi letöltések aktiválásához szintén hasznosak lehetnek:

- [MIM hibrid jelentéskészítő ügynök](https://www.microsoft.com/download/details.aspx?id=55112)

- [Általános LDAP-összekötő, általános SQL-összekötő, Graph-összekötő, Lotus Domino-összekötő, PowerShell-összekötő, a Web Services-összekötő](http://go.microsoft.com/fwlink/?LinkId=717495)

- [A SharePoint felhasználói profil Store-összekötő](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Ha még nem rendelkezik Active Directory-tartományban, és állít be egy PAM-forgatókönyv kísérletezéssel, tekintse meg a [a MIM 2016 SP1 PAM üzembehelyezési szkriptek](sp1-deployment-scripts.md).

## <a name="next-steps"></a>További lépések

- Ismerje meg, további on forgatókönyvek verziókban elérhető [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Olvassa el a [kapacitástervezési útmutató](capacity-planning-guide.md).
- A MIM telepítése egy [szinkronizálási forgatókönyvekben](microsoft-identity-manager-deploy.md) vagy a [emelt szintű hozzáférés-kezelési forgatókönyvekben](./pam/privileged-identity-management-for-active-directory-domain-services.md).


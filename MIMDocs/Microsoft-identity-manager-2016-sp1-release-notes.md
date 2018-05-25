---
title: Microsoft Identity Manager 2016 Service Pack 1 | Microsoft Docs
description: Ismerje meg, hogy a MIM 2016 hogyan működik, és hogyan kínál biztonságosabb és kényelmesebb identitáskezelési környezetet a felhőben és helyszíni környezetben egyaránt.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 98d21076801bbe60b7a2d2d5b7e1c41d4bce1b4a
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/25/2018
---
# <a name="whats-new-for-microsoft-identity-manager-2016-service-pack-1"></a>A Microsoft Identity Manager 2016 Service Pack 1 újdonságai #

A Microsoft Identity Manager karbantartásához és frissítéséhez tartozó kiadási ciklus részeként örömmel jelentjük be a [Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212) megjelenését. Ez a dokumentum az ebben a kiadásban található frissítéseket, fejlesztéseket, funkciókat és módosításokat ismerteti.

Ha problémát észlel a MIM SP1 éles üzembe helyezése közben, lépjen kapcsolatba a Microsoft ügyfélszolgálatával.

Szívesen meghallgatnánk a véleményét is! Ha visszajelzése vagy megjegyzése van, esetleg problémát észlel, jelezze a termékcsoport felé a következő e-mail-címen: [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## <a name="updates-in-this-service-pack"></a>A szervizcsomag frissítései #

### <a name="mim"></a>MIM

- **A MIM-portál böngészőkompatibilitása végfelhasználók önkiszolgáló munkafolyamataihoz:** Ebben a szervizcsomagban támogatást biztosítunk a legtöbb ismert böngészőhöz. A felhasználók mostantól elérhetik és használhatják a MIM-portált önkiszolgáló csoportok és profilok kezelésére a Microsoft Edge, a Chrome és a Safari böngészőből.

- **MIM szolgáltatás támogatása Exchange Online-hoz:** A MIM szolgáltatás régóta támogatja e-mailek küldését és fogadását jóváhagyáskor és értesítéskor. Az SP1 előtt a MIM csak az Exchange Server vagy az SMTP szolgáltatást támogatta. Az 1. szervizcsomag megjelenésétől kezdve a MIM szolgáltatás küldhet és fogadhat kéréseket is, csakúgy, mint e-mailes értesítéseket egy Office365 Exchange online fiók használatával.

- **Kép fájlformátumának érvényesítése feltöltéskor:** A MIM-mel már lehetséges a képek fájlformátumának érvényesítése a portálra történő feltöltés közben.

### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **PAM „PRIV” (bástya) erdőtámogatás Windows Server 2016 működési szinthez:** A MIM PAM szolgáltatás konfigurálható olyan tartományvezérlőkkel rendelkező környezetben, amely a Windows Server 2016 Active Directory Domain Services-erdő működési szintjén fut. A konfigurálás után a felhasználók Kerberos-jegye időkorlátossá válik a szerepkör-aktiválásban megmaradt időre.

    >[!Note]
    Ha karban kívánja tartani a Windows Server 2012 R2-erdő működési szintjét a CORP-tartományban, a [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) és a [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) telepítése javasolt a CORP-tartományvezérlőn.

- **Rendszerjogosultságú fiók kiterjesztése a „PRIV” (bástya) erdő kizárólagos csoportjaiba:** Mostantól a rendszergazdák értesíthetik a „PRIV”-erdő kizárólagos MIM-szolgáltatáscsoportjait és -felhasználóit. Ez lehetővé teszi csoportok és felhasználók PAM-szerepkörbe helyezését.  Ezután aktiválhatók egy szerepkörhöz, és csoporttagság rendelhető hozzájuk a „PRIV” erdőben.

- **PAM-üzembehelyezési szkript:** A PAM-üzembehelyezési szkriptek lehetővé teszik a rendszergazdák számára a PAM-környezet telepítésének leegyszerűsítését.

- **PAM-parancsmagok hitelesítési házirendi siló konfigurálásához:** Az 1. szervizcsomagban új parancsmagok találhatók, amelyek megerősítik a bástyaerdő biztonságát. Ezek a parancsmagok automatikusan létrehoznak egy hitelesítési házirendi sablonhoz kötött hitelesítési házirendi silót.

    >[!Note]
    Ezek a parancsmagok automatikusan lefutnak az üzembehelyezési szkriptek részeként.


## <a name="platform-support"></a>Platformtámogatás
A frissített platformtámogatási információk megtalálhatók a [MIM 2016 által támogatott platformok](microsoft-identity-manager-2016-supported-platforms.md) című dokumentumban.  Az ebben a szervizcsomagban támogatott új platformok közé tartozik az SQL Server 2016 és a SharePoint 2016.

## <a name="issues-fixed-in-this-release-from-mim-2016-general-availability"></a>A MIM 2016 általános rendelkezésre állási hibáinak javításai ebben a kiadásban

### <a name="pam"></a>PAM
- Új – A PAMGroup nem hoz létre MIM-objektumokat tartományi helyi csoportokhoz a PRIV-erdőben.
- Új – A PAMDomainConfiguration sikertelen eredményt adna vissza „netdom” hibaüzenettel.
- A PAM figyelőszolgáltatása által naplózott figyelmeztetések csoportoknak a PRIV-erdőben.

## <a name="how-to-upgrade-to-service-pack-1"></a>Frissítés a Service Pack 1 verzióra

A Microsoft Identity Manager 2016 Service Pack 1-re áttérő ügyfeleknek ajánlott követni az alábbi útmutatót a környezetben használt összes szolgáltatásnál.

>[!Note]
>A Forefront Identity Manager 2010 R2 SP1 vagy korábbi verziót használó felhasználónak először frissíteniük kell a környezetüket a 2015 augusztusában megjelent Microsoft Identity Manager 2016-ra, majd követni az alábbi lépéseket.

Előkészületek

A MIM-szinkronizálási motor frissítése szükséges a MIM szolgáltatás és a portál frissítése előtt.
Biztonsági másolat készítése szükséges a MIMService és MIM Sync adatbázisról.

  1. Távolítsa el a frissíteni kívánt Microsoft Identity Manager-összetevőt.
  2. Az eltávolítás után nyissa meg a telepítési adathordozón található „FIMSplash.htm” kezdőlapot.
  3. Válassza ki a frissíteni kívánt MIM-összetevőt.
  4. Az utasításokat követve folytassa a telepítést.
    * A MIM szolgáltatás és portál telepítése: Az Exchange Online levelezési fiókként történő kiválasztásakor írja be az Exchange Online-fiókhoz tartozó e-mail-címet és hitelesítő adatokat a következő képernyőn.

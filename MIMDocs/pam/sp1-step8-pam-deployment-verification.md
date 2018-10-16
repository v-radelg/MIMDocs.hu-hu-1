---
title: '8. lépés: A PAM üzembe helyezésének ellenőrzése'
description: A PAM parancsfájlokkal történő üzembe helyezéséhez szükséges csomag ellenőrzési parancsfájlokat is tartalmaz, amelyekkel végrehajtható egy PAM-forgatókönyv, így ellenőrizhető, hogy a PAM-környezet a vártnak megfelelően működik-e.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 53ffcd4832ded5dce0878715cc716559c3883d6a
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332986"
---
# <a name="step-8-pam-deployment-verification"></a>Nyolcadik lépés: A PAM üzembe helyezésének ellenőrzése

> [!div class="step-by-step"]
> [« 7. lépés](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Kiegészítés »](sp1-pam-deployment-addendum.md)

Az üzembehelyezési csomag tartalmazza az ellenőrzési szkripteket is, amelyekkel végrehajtható egy PAM-forgatókönyv, így ellenőrizhető, hogy a PAM-környezet a vártnak megfelelően működik-e.
Az üzembe helyezés ellenőrzéséhez módosítsa a PAMDeploymentConfig.xml <PamValidation/> elnevezésű szakaszát.

>[!NOTE]
>Az érvényesítéshez szükséges egy, a CORP-tartományhoz csatlakoztatott ügyfélgép, amelyen a PAM-ügyféloldali összetevők is telepítve vannak. Az ügyfél telepítésével kapcsolatos szkriptekért lásd a kiegészítést.

Az ügyfélgép nevét frissíteni kell a PAMDeploymentConfig.xml <PAMValidationClient/> címkéjével. A(z) <PAMValidation/> csomópontban lévő többi adatot csak akkor kell szerkeszteni, ha ütköznek a meglévő felhasználókkal/csoportokkal, mivel ez az érvényesítés megkísérli létrehozni őket.
Alkalmazza a következő lépéseket az érvényesítés végrehajtásához:

Első lépés:

1. Bejelentkezés a CORPDC tartományvezérlőre CORP-tartományi rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Ezzel létrehozza az érvényesítéshez szükséges csoportokat és felhasználókat.

Második lépés:

1. Bejelentkezés a PAM-kiszolgálóra MIMAdmin jogosultsággal
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Ez a lépés áttelepíti a felhasználókat és csoportokat a PAM-környezetbe.

Harmadik lépés:

1. Bejelentkezés a CORP-ügyfélre helyi rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


Ebben a lépésben meg kell adnia a CORPAdmin hitelesítő adatait. Amint megadta, a rendszer felveszi a szükséges felhasználókat az „Asztal távoli felhasználók” és a „Rendszerfelügyeleti felhasználók” csoportba.
A CORP-ügyfélen a következő parancs használatával nyissa meg a PowerShellt az érvényesítés alatt álló PRIV-felhasználóként. </br></br>
**Runas/u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
A PowerShell ablakban írja be a következőt:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Ezzel megjeleníti a kérés állapotát.
  Kezdetben a felhasználónak nincs hozzáférése az erőforráshoz. A felhasználó a szerepkörhöz való igényalapú hozzáadását követően kapja meg a hozzáférést. Miután a kérés időtartama lejár, a felhasználó ismét elveszti a hozzáférését.
  A szkript az alapértelmezett értéket (11 perc) használja a kérések elévülésére.

> [!div class="step-by-step"]
> [« 7. lépés](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Kiegészítés »](sp1-pam-deployment-addendum.md)

---
title: "6. lépés: PAM bizalmi kapcsolat beállítása"
description: "A CORP-tartomány előkészítése a Privileged Identity Manager által szkriptek útján kezelt meglévő vagy új identitásokkal"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 5bcf4f4ef201236746ec1bf75c1c8900841a6c79


---

# <a name="step-6-set-up-the-pam-trust"></a>6. lépés: PAM bizalmi kapcsolat beállítása

>[!div class="step-by-step"]
[« 5. lépés](sp1-step5-configuring-pam.md)
[7. lépés »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Ennek végrehajtása a kizárólag PRIV-környezetekben nem szükséges** Jelentkezzen be a PAMServer kiszolgálón a MIMAdmin-fiókkal.

1. Bejelentkezés a PAMServer kiszolgálón a MIMAdmin-fiókkal
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. a 6. menüelem kiválasztása (PAM bizalmi kapcsolat beállítása)

  Ha a rendszer arra kéri, adja meg a CORP rendszergazdai fiók hitelesítő adatait. Miután megadta a hitelesítő adatokat, a bizalmi kapcsolat felépül, és a konfiguráció teljessé válik.

>[!div class="step-by-step"]
[« 5. lépés](sp1-step5-configuring-pam.md)
[7. lépés »](sp1-step7-setup-sidhistory-sidfiltering.md)



<!--HONumber=Nov16_HO2-->



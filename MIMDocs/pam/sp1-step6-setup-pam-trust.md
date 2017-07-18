---
title: "6. lépés: PAM bizalmi kapcsolat beállítása"
description: "A PAM parancsfájlokkal történő konfigurálásának 6. lépése. Ez a fejezet a CORP- és PRIV-tartományok között elvárt megbízhatóság beállítását ismerteti"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 3b232dfa515b42fd42a5606d1beff9d3fe50389c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# 6. lépés: PAM bizalmi kapcsolat beállítása
<a id="step-6-set-up-the-pam-trust" class="xliff"></a>

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

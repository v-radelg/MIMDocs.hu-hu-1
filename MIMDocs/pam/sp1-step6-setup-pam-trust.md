---
title: '6. lépés: PAM bizalmi kapcsolat beállítása'
description: A PAM parancsfájlokkal történő konfigurálásának 6. lépése. Ez a fejezet a CORP- és PRIV-tartományok között elvárt megbízhatóság beállítását ismerteti
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 5e3bed084b655535b0ac7b8b4252cdf541f6121f
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288775"
---
# <a name="step-6-set-up-the-pam-trust"></a>6. lépés: PAM bizalmi kapcsolat beállítása

> [!div class="step-by-step"]
> [« 5. lépés](sp1-step5-configuring-pam.md)
> [7. lépés »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Ennek végrehajtása a kizárólag PRIV-környezetekben nem szükséges** Jelentkezzen be a PAMServer kiszolgálón a MIMAdmin-fiókkal.

1. Bejelentkezés a PAMServer kiszolgálón a MIMAdmin-fiókkal
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. a 6. menüelem kiválasztása (PAM bizalmi kapcsolat beállítása)

   Ha a rendszer arra kéri, adja meg a CORP rendszergazdai fiók hitelesítő adatait. Miután megadta a hitelesítő adatokat, a bizalmi kapcsolat felépül, és a konfiguráció teljessé válik.

> [!div class="step-by-step"]
> [« 5. lépés](sp1-step5-configuring-pam.md)
> [7. lépés »](sp1-step7-setup-sidhistory-sidfiltering.md)

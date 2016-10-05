---
title: "6. lépés: PAM bizalmi kapcsolat beállítása"
description: "A CORP-tartomány előkészítése a Privileged Identity Manager által szkriptek útján kezelt meglévő vagy új identitásokkal"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 46afda513e849e457f5f3644a46f244161467e50


---

# PAM bizalmi kapcsolat beállítása

**Ennek végrehajtása a kizárólag PRIV-környezetekben nem szükséges** Jelentkezzen be a PAMServer kiszolgálón a MIMAdmin-fiókkal.

1. Bejelentkezés a PAMServer kiszolgálón a MIMAdmin-fiókkal
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. a 6. menüelem kiválasztása (PAM bizalmi kapcsolat beállítása)

  Ha a rendszer arra kéri, adja meg a CORP rendszergazdai fiók hitelesítő adatait. Miután megadta a hitelesítő adatokat, a bizalmi kapcsolat felépül, és a konfiguráció teljessé válik.



<!--HONumber=Sep16_HO4-->



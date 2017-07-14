---
title: "2. lépés: A CORP-tartomány konfigurálása"
description: "Ez a cikk a CORP-tartomány konfigurálásának második lépését ismerteti, amelyhez hozzátartozik egy parancsfájl futtatása is a SIDs.txt fájlnak a CORPDC-re másolása után"
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
ms.translationtype: MT
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# 2. lépés: A CORP-tartomány konfigurálása
<a id="step-2-configuring-the-corp-domain" class="xliff"></a>

>[!div class="step-by-step"]
[« 1. lépés](sp1-step1-configuring-priv-domain.md)
[3. lépés »](sp1-step3-installing-configuring-sql.md)

A SIDs.txt a CORPDC tartományvezérlőre másolása után **nem szükséges a PRIVOnly-környezetekhez**

1. Bejelentkezés a CORPDC tartományvezérlőbe rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. A 2. menüelem kiválasztása (CORP-erdő konfigurálása)

>[!div class="step-by-step"]
[« 1. lépés](sp1-step1-configuring-priv-domain.md)
[3. lépés »](sp1-step3-installing-configuring-sql.md)


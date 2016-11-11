---
title: "2. lépés: A CORP-tartomány konfigurálása"
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
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


---

# <a name="step-2-configuring-the-corp-domain"></a>2. lépés: A CORP-tartomány konfigurálása

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



<!--HONumber=Nov16_HO2-->



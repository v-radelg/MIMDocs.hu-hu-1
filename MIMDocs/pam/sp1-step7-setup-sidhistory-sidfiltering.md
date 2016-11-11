---
title: "7. lépés: SID-előzmények/SID-szűrés beállítása"
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
ms.openlocfilehash: a98d83a22c61ef534fcc02725e4cd500be10cc8a


---

# <a name="step-7-set-up-sid-historysid-filtering"></a>7. lépés: SID-előzmények/SID-szűrés beállítása

>[!div class="step-by-step"]
[« 6. lépés](sp1-step6-setup-pam-trust.md)
[8. lépés »](sp1-step8-pam-deployment-verification.md)

**Ennek végrehajtása a kizárólag PRIV-környezetekben nem szükséges** Jelentkezzen be a PAMServer kiszolgálón a MIMAdmin-fiókkal.

1. Bejelentkezés a CORP-tartományvezérlőre rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. A 8. menüelem kiválasztása (SID-előzmények/SID-szűrés beállítása)

A sikeres végrehajtást követően a következő üzenetek láthatóak:<br/></br>
SID-szűrés esetén: <br/></br>
„Megbízhatósági kapcsolat beállítása a biztonsági azonosítók szűrésének mellőzésére” vagy „A biztonsági azonosítók szűrése nem engedélyezett ebben a megbízhatósági kapcsolatban”. </br></br>
SID-előzményekhez: </br></br>
„A biztonsági azonosítók előzményeinek engedélyezése ebben a megbízhatósági kapcsolatban” vagy „A biztonsági azonosítók előzményei már engedélyezettek ebben a megbízhatósági kapcsolatban”.

>[!div class="step-by-step"]
[« 6. lépés](sp1-step6-setup-pam-trust.md)
[8. lépés »](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Nov16_HO2-->



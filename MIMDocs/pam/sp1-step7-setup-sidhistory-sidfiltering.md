---
title: "7. lépés: SID-előzmények/SID-szűrés beállítása"
description: "Ez a Privileged Identity Manager parancsfájlokkal történő konfigurálásának 7. lépése. Ehhez a lépéshez a SID-előzmények/SID-szűrés beállítása tartozik."
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
ms.openlocfilehash: e608593f40759e3bc995daa56c4575510a71e987
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# 7. lépés: SID-előzmények/SID-szűrés beállítása
<a id="step-7-set-up-sid-historysid-filtering" class="xliff"></a>

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


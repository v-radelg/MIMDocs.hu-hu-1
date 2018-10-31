---
title: '7. lépés: SID-előzmények/SID-szűrés beállítása'
description: Ez a Privileged Identity Manager parancsfájlokkal történő konfigurálásának 7. lépése. Ehhez a lépéshez a SID-előzmények/SID-szűrés beállítása tartozik.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: f85dd4eff32d5207948ec332bf2e9850b14a86fe
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379328"
---
# <a name="step-7-set-up-sid-historysid-filtering"></a>7. lépés: SID-előzmények/SID-szűrés beállítása

> [!div class="step-by-step"]
> [« 6. lépés](sp1-step6-setup-pam-trust.md)
> [8. lépés »](sp1-step8-pam-deployment-verification.md)

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

> [!div class="step-by-step"]
> [« 6. lépés](sp1-step6-setup-pam-trust.md)
> [8. lépés »](sp1-step8-pam-deployment-verification.md)

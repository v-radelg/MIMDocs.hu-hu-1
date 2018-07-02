---
title: '7. lépés: SID-előzmények/SID-szűrés beállítása'
description: Ez a Privileged Identity Manager parancsfájlokkal történő konfigurálásának 7. lépése. Ehhez a lépéshez a SID-előzmények/SID-szűrés beállítása tartozik.
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
ms.openlocfilehash: d2512690ce648767157a7417e5b41095c970b8eb
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289108"
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

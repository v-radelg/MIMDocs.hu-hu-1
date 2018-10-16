---
title: '5. lépés: PAM telepítése/konfigurálása'
description: Ez a Privileged Identity Manager parancsfájlokkal történő konfigurálásának 5. lépése, amely a PAM kiszolgáló üzembe helyezését részletezi.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e3e0d45f01f651400842b52e275ca4b6939f78c4
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333836"
---
# <a name="step-5-installingconfiguring-pam"></a>5. lépés: PAM telepítése/konfigurálása

> [!div class="step-by-step"]
> [« 4. lépés](sp1-step4-configuring-sharepoint.md)
> [6. lépés »](sp1-step6-setup-pam-trust.md)

Tartományhoz csatlakoztatott PAMServer esetén jelentkezzen be MIMAdminként, más esetben helyi rendszergazdaként.
1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Az 5. menüelem kiválasztása (MIM PAM beállítása)

>[!NOTE]
>Ha a gép még nincs PRIV-tartományhoz csatlakoztatva, kérni fogja a hitelesítő adatokat. A tartományhoz csatlakozás után a gép újraindul.

Miután a PAMServer újraindult, jelentkezzen be a gépre a MIMAdmin-fiókkal.

1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Az 5. menüelem kiválasztása (MIM PAM beállítása)

   Ha a rendszer kéri, írja be a MIM Monitor-fiókhoz, MIM-összetevőfiókhoz, MIM MA-fiókhoz, MIM-szolgáltatásfiókhoz, MIM-rendszergazdafiókhoz és a SharePoint-fiókhoz tartozó jelszót.
   A telepítés befejezése után a gép újraindul.

> [!div class="step-by-step"]
> [« 4. lépés](sp1-step4-configuring-sharepoint.md)
> [6. lépés »](sp1-step6-setup-pam-trust.md)

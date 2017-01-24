---
title: "5. lépés: PAM telepítése/konfigurálása"
description: "Ez a Privileged Identity Manager parancsfájlokkal történő konfigurálásának 5. lépése, amely a PAM kiszolgáló üzembe helyezését részletezi."
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
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 862f62ab9bac87bcee31c35e249db34740e9fb14


---
# <a name="step-5-installingconfiguring-pam"></a>5. lépés: PAM telepítése/konfigurálása

>[!div class="step-by-step"]
[« 4. lépés](sp1-step4-configuring-sharepoint.md)
[6. lépés »](sp1-step6-setup-pam-trust.md)

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

>[!div class="step-by-step"]
[« 4. lépés](sp1-step4-configuring-sharepoint.md)
[6. lépés »](sp1-step6-setup-pam-trust.md)



<!--HONumber=Jan17_HO2-->



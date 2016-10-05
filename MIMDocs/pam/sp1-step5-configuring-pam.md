---
title: "5. lépés: PAM telepítése/konfigurálása"
description: "A CORP-tartomány előkészítése a Privileged Identity Manager által szkriptek útján kezelt meglévő vagy új identitásokkal"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a5d86991f1579f292d7d303148422cef746d008a


---
# 5. lépés: PAM telepítése/konfigurálása

Tartományhoz csatlakoztatott PAMServer esetén jelentkezzen be MIMAdminként, más esetben helyi rendszergazdaként.
1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Az 5. menüelem kiválasztása (MIM PAM beállítása)

>[!NOTE] Ha a gép még nincs PRIV-tartományhoz csatlakoztatva, kérni fogja a hitelesítő adatokat. A tartományhoz csatlakozás után a gép újraindul.

Miután a PAMServer újraindult, jelentkezzen be a gépre a MIMAdmin-fiókkal.

1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Az 5. menüelem kiválasztása (MIM PAM beállítása)

  Ha a rendszer kéri, írja be a MIM Monitor-fiókhoz, MIM-összetevőfiókhoz, MIM MA-fiókhoz, MIM-szolgáltatásfiókhoz, MIM-rendszergazdafiókhoz és a SharePoint-fiókhoz tartozó jelszót.
  A telepítés befejezése után a gép újraindul.



<!--HONumber=Sep16_HO4-->



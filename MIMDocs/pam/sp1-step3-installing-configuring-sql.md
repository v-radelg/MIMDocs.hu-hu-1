---
title: "3. lépés: Az SQL konfigurálása"
description: "Ez a cikk a Privileged Identity Manager parancsfájlokkal történő konfigurálást ismertető sorozat 3. tagja, amely az SQL Server konfigurálásának lépéseit írja le."
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
ms.openlocfilehash: 93ae9f198d73d21ae966fe3c3b22e47435bd5608


---
# <a name="step-3-configuring-sql"></a>3. lépés: Az SQL konfigurálása

>[!div class="step-by-step"]
[« 2. lépés](sp1-step2-configuring-corp-domain.md)
[4. lépés »](sp1-step4-configuring-sharepoint.md)

Mielőtt folytatná a következő lépésekkel, győződjön meg róla, hogy az SQL Server 2012 SP1 vagy újabb verziót, illetve az SQL Server 2014 verziót használja. Tartományhoz csatlakoztatott kiszolgálók esetében jelentkezzen be a MIMAdmin-fiók használatával, más esetben jelentkezzen be helyi rendszergazdaként.
1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. A 3. menüelem kiválasztása (SQL Server beállítása)

  Ha a kiszolgáló még nem lett csatlakoztatva egy PRIV-tartományhoz, kérni fogja a hitelesítő adatokat és a kiszolgáló csatlakoztatását a tartományhoz.
  A tartományhoz csatlakozás után a gép újraindul. Sikeres újraindítás után jelentkezzen be a kiszolgálón a MIMAdmin-fiókkal.

1. A PowerShell futtatása rendszergazdaként
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. A 3. menüelem kiválasztása (SQL Server beállítása)

Ha szükséges, adja meg a MIMAdmin-szolgáltatásfiókhoz tartozó jelszót, és engedélyezze a telepítés folytatását. Ha elkészült, folytassa a 4. lépéssel.

>[!div class="step-by-step"]
[« 2. lépés](sp1-step2-configuring-corp-domain.md)
[4. lépés »](sp1-step4-configuring-sharepoint.md)



<!--HONumber=Jan17_HO2-->



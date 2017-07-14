---
title: "4. lépés: A SharePoint konfigurálása"
description: "Ez a PAM parancsfájlokkal történő konfigurálásának 4. lépése. Ebben a lépésben történik a SharePoint konfigurálása, hogy az használható legyen az üzembe helyezendő PAM részeként."
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
ms.openlocfilehash: 12eb9a00584f72b9c628e870562a743fb603d4a3
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# 4. lépés: A SharePoint konfigurálása
<a id="step-4-configuring-sharepoint" class="xliff"></a>

>[!div class="step-by-step"]
[« 3. lépés](sp1-step3-installing-configuring-sql.md)
[5. lépés »](sp1-step5-configuring-pam.md)

A SharePoint a SharePoint Foundation 2013-as verziója kell legyen az SP1 szervizcsomaggal.

Tartományhoz csatlakoztatott kiszolgálók esetében MIMAdmin jogosultsággal kell bejelentkeznie

1. A PowerShell futtatása rendszergazdaként
2.  .\PAMDeployment.ps1
3.  A 4. menüelem kiválasztása (SharePoint beállítása)


Munkacsoport-kiszolgálók esetén

1. A PowerShell futtatása rendszergazdaként
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. A 4. menüelem kiválasztása (SharePoint beállítása)

A SharePoint telepítése során a gép több alkalommal is újraindul. Minden alkalommal, amikor a SharePoint-telepítőt újra kell futtatni, győződjön meg róla, hogy a MIMAdmin-fiókkal jelentkezik be.
Ha a SharePoint telepítését végző gép nem rendelkezik internetkapcsolattal az előfeltételek letöltéséhez, azok külön is letölthetőek, és egy helyi mappába helyezhetőek. **A helyi mappa elérési útját frissíteni kell a PAMConfiguration.xml fájl <PrerequisitesBinaryLocation/> szakaszában.** A fájlok letöltésére használható hivatkozásokért lásd az 5. kiegészítést.
A telepítést követően megnyílik a SharePoint-konfiguráció grafikus felhasználói felülete, amely végigvezeti a SharePoint telepítésének befejezéséhez szükséges lépéseken. Válassza a Teljes kiszolgáló beállítást, és haladjon végig a felhasználói felület többi részén. A telepítést követően a rendszer megkéri a Konfiguráló varázsló futtatására. Végezze el az alábbiakban ismertetett lépéseket.

1. A **Kapcsolódás a kiszolgálófarmhoz** lapon váltson az **Új kiszolgálófarm létrehozása** elemre.
2. A konfigurációs adatbázis adatbázis-kiszolgálójaként adja meg az **SQLServer** kiszolgálót, a SharePoint által használható adatbázisfiókként pedig a **SharePoint ServiceAccount** fiókot.
3. Adjon meg egy jelszót a farm biztonsági hozzáférési kódjaként **(ez később nem lesz használatban)**.
4. Fogadja el a SharePoint konfiguráló varázslójának többi alapértelmezett beállítását, és hozzon létre egy egykiszolgálós farmot.

A részletek a [3. lépés: A PAM-kiszolgáló előkészítése](/microsoft-identity-manager/pam/step-3-prepare-pam-server) **A SharePoint konfigurálása** című szakaszában találhatóak. Ha végzett vele, futtassa ismét a „.\PAMDeployment.ps1” szkriptet, és válassza a 4-es elemet (SharePoint beállítása) a lépés befejezéséhez.

>[!div class="step-by-step"]
[« 3. lépés](sp1-step3-installing-configuring-sql.md)
[5. lépés »](sp1-step5-configuring-pam.md)


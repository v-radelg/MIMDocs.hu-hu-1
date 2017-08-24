---
title: "Kiegészítés"
description: "Ez a PAM parancsfájlokkal történő üzembe helyezését ismertető dokumentumok kiegészítése. Ismerteti a PRIV- és CORP-tartományok konfigurálását valamint egy, az érvényesítést végző ügyfélprogram beállítását. Információt nyújt a segítségkérés módjáról is."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 010fcac1d512775ed5c84031a42a4c5836cff34f
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/19/2017
---
# <a name="pam-deployment-scripts-addendum"></a>PAM üzembehelyezési szkriptek, kiegészítés:

## <a name="addendum-1-setting-up-the-priv-domain"></a>1. kiegészítés: A PRIV-tartomány beállítása

Miután kicsomagolta a tömörített fájlt az $env:SYSTEMDRIVE\PAM mappába, a PAMDeploymentConfig.xml fájl szerkesztésével adja meg a PRIV-erdő adatait. Frissítse a DNSName, a NetbiosName, a tartományvezérlő-név, az Adatbázis/napló elérési útja és a Sysvol elérési útja értéket. Frissítse továbbá a DomainMode és a ForestMode értéket. Amennyiben a Windows Server Technical Preview 5-ös verzióját teszteli, a DomainMode és a ForestMode értéket állítsa WinThreshold értékre.

1. Bejelentkezés a PRIV-tartományi tartományvezérlőre rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. a 9. menüelem kiválasztása (PRIV-erdő beállítása)


A befejezést követően a tartományvezérlő automatikusan újraindul. A címtárszolgáltatások helyreállító módjának (DSRM) rendszergazdai jelszava meg kell feleljen az alábbi követelményeknek:

  * A jelszó minimális hossza 15 karakter
  * A jelszó legalább egy kisbetűs karaktert tartalmaz
  * A jelszó legalább egy NAGYBETŰS karaktert tartalmaz
  * A jelszó legalább egy számjegyet vagy speciális karaktert tartalmaz

## <a name="addendum-2-setting-up-the-corp-domain"></a>2. kiegészítés: A CORP-tartomány beállítása

Ha induláskor PAM rendszer áll rendelkezésre, és szeretne telepíteni egy tesztkörnyezetet, a szkript lehetővé teszi egy CORP-tartomány konfigurálását is. Miután kicsomagolta a tömörített fájlt az $env:SYSTEMDRIVE\PAM mappába, a PAMDeploymentConfig.xml fájlt kiegészítve adja meg a CORP-erdő adatait. Frissítse a DNSName, a NetbiosName, a tartományvezérlő-név, az Adatbázis/napló elérési útja és a Sysvol elérési útja értéket. A működési szint legalább Windows Server 2012 R2 rendszerű kell legyen.

1. Bejelentkezés a CORP-tartományi tartományvezérlőre rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. a 10. menüelem kiválasztása (CORP-erdő beállítása)

A befejezést követően a tartományvezérlő automatikusan újraindul.

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>3. kiegészítés: CORP-ügyfél beállítása az érvényesítés végrehajtására

A konfigurációs fájlban a ClientBinaryLocation arra a helyre kell mutasson, ahol a setup.exe található.
Jelentkezzen be az ügyfélre helyi rendszergazdaként, és futtassa a következő parancsokat egy emelt szintű PowerShell-ablakban:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. A 7. menüelem kiválasztása (MIM PAM-ügyfél beállítása)


Ha a gép nincs csatlakoztatva a tartományhoz, a rendszer kéri a rendszergazdai hitelesítő adatokat a tartományhoz való csatlakozás végrehajtásához. A tartományhoz való csatlakozás után a gépet újra kell indítani. Jelentkezzen be ismét az ügyfélre helyi rendszergazdaként, és futtassa a következő parancsokat egy emelt szintű PowerShell-ablakban:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. A 7. menüelem kiválasztása (MIM PAM-ügyfél beállítása)

Folytassa a fentebb ismertetett 8. lépéssel.

## <a name="addendum-4-if-something-goes-wrong"></a>4. kiegészítés: Ha valami probléma merül fel

A szkriptek naplói mind az %AppData%\MIMPAMInstall helyen vannak tárolva. Tömörítse a mappát egy Zip-fájlba, és küldje el e-mailben a [mim2016@microsoft.com](mailto:mim2016@microsoft.com) címre a művelet és a hiba részleteivel együtt.

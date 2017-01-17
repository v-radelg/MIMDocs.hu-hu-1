---
title: "A PAM konfigurálása szkriptek használatával"
description: "Ez a cikk a PAM parancsfájlokkal történő konfigurálást ismertető sorozat tagja. Tartalma a PAM üzembe helyezési parancsfájlokhoz használandó XML fájl módosítását ismerteti."
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
ms.openlocfilehash: bd73f43a096d58e1f7250e28b59e33f4411e88a3


---

# <a name="configure-pam-using-scripts"></a>A PAM konfigurálása szkriptek használatával

Ha külön kiszolgálón telepíti az SQL- és SharePoint-szolgáltatásokat, az alábbi utasítások szerint kell konfigurálnia őket. Ha az SQL-, SharePoint- és a PAM-összetevők ugyanazon a gépen vannak telepítve, az alábbi lépéseket kell futtatni arról a gépről.

A következő lépések feltételezik, hogy egy PRIV-tartomány már be van állítva. A PRIV-tartomány konfigurálásnak utasításaiért tekintse meg a dokumentum végén található kiegészítést.

lépések:

1. Minden gépen csomagolja ki a „PAMDeploymentScripts.zip” tömörített fájlt a %SYSTEMDRIVE%\PAM mappába.
2. Az egyik gépen nyissa meg a **PAMDeploymentConfig.xml** fájlt, és frissítse a részleteket az alábbi ábra vagy az XML-fájlon belül található útmutatás alapján. Ha a CORP- és PRIV-erdők már be vannak állítva, csak a **DNSName** és a **NetbiosName** értékeket kell frissítenie.
3. A Szerepkörök szakaszban frissítse az SQL, SharePoint és MIM szerepkörökhöz tartozó **szolgáltatásfiók**, **gép adatai**, és a **telepítési bináris fájlok helye** értékét.
    1. A MIM bináris fájljai helyének a „Service and Portal” mappát tartalmazó könyvtárra kell mutatnia. Az ügyfél bináris fájljai helyének az „Add-ins and Extensions.msi” fájlt tartalmazó könyvtárra kell mutatnia.

4. Ha ez egy PRIVOnly-környezet, a PRIVOnly címkének igaz értékűnek kell lennie.
    1. PRIVOnly-környezetek esetében frissítse a PRIV-tartományhoz tartozó **DNSName** és **NetbiosName** értékét, hogy megegyezzenek a CORP-tartomány értékeivel. Győződjön meg róla, hogy a gép utótagjai helyesek azoknál a gépeknél, amelyekre SQL, SharePoint vagy MIM lesz telepítve, mivel az alapértelmezett sablon CORP- és PRIV-konfigurációt feltételez.
    2. A PRIVOnly környezetekkel kapcsolatos további információért kattintson ide.

5. Másolja ugyanazt a PAMDeploymentConfig.xml fájlt minden gép, CORPDC-, PRIVDC-, PAM Server, SQL Server és SharePoint-kiszolgáló %SYSTEMDRIVE%\PAM mappájába.


## <a name="deployment-worksheet"></a>Üzembehelyezési munkalap

Folytatás előtt frissítse a PAMDeploymentConfig.xml fájlt, és másolja a frissített fájlt minden gépre.

### <a name="setup"></a>Setup

|Machine   | Futtatás más nevében   |Parancsok   |
|---|---|---|
|  PRIVDC |PRIV tartományi rendszergazda   | .\PAMDeployment.ps1 Az 1. menüelem kiválasztása (PRIV-erdő konfigurálása)   |
|   |   |  A fenti lépés egy SIDs.txt fájlt hoz létre. A fájlt a CORPDC-ben található $envDrive:PAM mappába kell másolni a következő lépés végrehajtása előtt. |
| CORPDC  |CORP tartományi rendszergazda   | .\PAMDeployment.ps1 A 2. menüelem kiválasztása (CORP-erdő konfigurálása)   |
| PAMServer (vagy SQL Server)   |CORP tartományi rendszergazda   |  .\PAMDeployment.ps1 A 2. menüelem kiválasztása (CORP-erdő konfigurálása)  |
|  PAMServer |  Helyi rendszergazda (MIM-rendszergazda a tartomány csatlakoztatása után) |  .\PAMDeployment.ps1 A 4. menüelem kiválasztása (SharePoint beállítása)  |
| PAMServer  | Helyi rendszergazda (MIM-rendszergazda a tartomány csatlakoztatása után)  | .\PAMDeployment.ps1 Az 5. menüelem kiválasztása (MIM PAM beállítása)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 A 6. menüelem kiválasztása (PAM bizalmi kapcsolat beállítása) .\PAMDeployment.ps1 A 6. menüelem kiválasztása (PAM bizalmi kapcsolat beállítása) |

### <a name="validation"></a>Érvényesítés

|  Machine | Futtatás más nevében   | Parancsok   |
|---|---|---|
| CORPClient  | CORP-felhasználó (helyi rendszergazda)  |   .\PAMDeployment.ps1 A 7. menüelem kiválasztása (MIM PAM-ügyfél beállítása)  |
| CORPDC  | CORP tartományi rendszergazda   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | CORP-felhasználó (helyi rendszergazda)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>\PRIV.pamRequestor felhasználó, valamint PRIVOnly esetében: <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


>[!div class="step-by-step"]
[Indítás »](sp1-step1-configuring-priv-domain.md)



<!--HONumber=Jan17_HO2-->



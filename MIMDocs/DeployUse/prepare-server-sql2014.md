---
title: "Az SQL Server konfigurálása | Microsoft Docs"
description: "Az SQL Server 2014 telepítése a MIM 2016 üzembe helyezésének előkészületeként."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 4ecb80282591ae5e4e52124637ab0e2edf0809b3


---

# <a name="set-up-an-identity-management-server-sql-server-2014"></a>Identitáskezelési kiszolgáló beállítása: SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó – **Pass@word1**

## <a name="install-sql-server-2014-standard-edition"></a>Az **SQL Server 2014 Standard Edition** telepítése

1. Tartományi rendszergazdaként indítsa el a **PowerShellt**.

2. Nyissa meg azt a könyvtárat, amelyben az SQL Server telepítője található.

3. Írja be a következő parancsokat:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)



<!--HONumber=Nov16_HO2-->



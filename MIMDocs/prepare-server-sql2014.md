---
title: "Az SQL Server konfigurálása a Microsoft Identity Manager 2016 használatához | Microsoft Docs"
description: "Az SQL Server 2014 telepítése a MIM 2016 üzembe helyezésének előkészületeként."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 7f16c3a054f0a2c59f118ba33bf64fca10034690
ms.openlocfilehash: 105d2320ed5a0d610e8e6c5f459366680e3f8a77
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# Identitáskezelési kiszolgáló beállítása: SQL Server 2014
<a id="set-up-an-identity-management-server-sql-server-2014" class="xliff"></a>

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó – **Pass@word1**

## Az **SQL Server 2014 Standard Edition** telepítése
<a id="install-sql-server-2014-standard-edition" class="xliff"></a>

1. Tartományi rendszergazdaként indítsa el a **PowerShellt**.

2. Nyissa meg azt a könyvtárat, amelyben az SQL Server telepítője található.

3. Írja be a következő parancsokat:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)


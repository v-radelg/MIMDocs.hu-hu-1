---
title: SQL Server konfigurálása a Microsoft Identity Manager 2016 SP1 |} A Microsoft Docs
description: SQL Server 2016 telepítése a MIM 2016 üzembe előkészítésekor.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6fe251a3976167909aa55a687884585b1937ebf3
ms.sourcegitcommit: 28834821cbddd6384613d8ba45424c35f4c39ce6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538557"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>Identitáskezelési kiszolgáló beállítása: SQL Server 2016-ban

> [!div class="step-by-step"]
> [«Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
> 
> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **corpdc-re**
> - Tartománynév – **contoso**
> - MIM szolgáltatás kiszolgálójának neve – **corpservice**
> - MIM Sync-kiszolgáló neve – **corpsync**
> - Az SQL Server neve – **corpsql**
> - Jelszó – <strong>Pass@word1</strong>

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Telepítés **az SQL Server 2016 Standard vagy Enterprise Edition**

1. Tartományi rendszergazdaként indítsa el a **PowerShellt**.

2. Nyissa meg azt a könyvtárat, amelyben az SQL Server telepítője található.

3. Írja be a következő parancsokat:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"

More info SQL deployment accounts and services can be found [here](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS is no longer included in SQL 2016 downlaod details can be found [here](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)    ```
> 
> [!div class="step-by-step"]  
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)

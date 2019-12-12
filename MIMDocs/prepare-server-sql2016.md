---
title: SQL Server konfigurálása Microsoft Identity Manager 2016 SP2 rendszerhez | Microsoft Docs
description: 'Telepítse a SQL Server 2016-es vagy a 2017-es verzióját a következőre: előkészítés a rendszerbe 2016 a saját'
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4be699f123bf7d48b709ee8b8e91e2222cd492e2
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/05/2019
ms.locfileid: "73568024"
---
# <a name="set-up-an-identity-management-server-sql-server-2016-or-2017"></a>Identitáskezelés-felügyeleti kiszolgáló beállítása: SQL Server 2016 vagy 2017

> [!div class="step-by-step"]
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
 
> [!NOTE] 
> Az SQL Server 2017 telepítési eljárása nem tér el az SQL Server 2016 telepítési eljárásának.

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **corpdc**
> - Tartománynév – **contoso**
> - **Corpservice** -kiszolgáló neve
> - **Corpsync** -szinkronizálási kiszolgáló neve
> - SQL Server neve – **corpsql**
> - Jelszó – <strong>Pass@word1</strong>

> [!IMPORTANT]
> A AlwaysOn 2016 SP2 támogatja az SQL *AlRegisterAllProvidersIPon* rendelkezésre állási csoport (AoAG) figyelőit 0 értékre állítva, ami azt jelenti SQL Server, hogy az alhálózatok közötti feladatátvétel jelenleg nem támogatott.

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Telepítse a **SQL Server 2016 standard/Enterprise kiadást**

1. Tartományi rendszergazdaként indítsa el a **PowerShellt**.

2. Nyissa meg azt a könyvtárat, amelyben az SQL Server telepítője található.

3. Írja be a következő parancsokat:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
További információ az SQL-alapú központi telepítési fiókokról és szolgáltatásokról [itt](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017) található.

> [!NOTE]
> A SSMS már nem része az SQL 2016-nek. A letöltés részleteit [itt](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) találja

> [!div class="step-by-step"]  
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)

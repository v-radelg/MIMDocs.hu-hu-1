---
title: A SharePoint konfigurálása a Microsoft Identity Manager 2016-tal való használathoz | Microsoft Docs
description: A SharePoint Foundation telepítése és konfigurálása a MIM-portál oldalának üzemeltetéséhez.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 46080360dd0ad6c3554e2d9b3418ac518b75a5cd
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701384"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Identitáskezelés-felügyeleti kiszolgáló beállítása: SharePoint

> [!div class="step-by-step"]
> [«SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
> 
> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **corpdc**
> - Tartománynév – **contoso**
> - **Corpservice** -kiszolgáló neve
> - **Corpsync** -szinkronizálási kiszolgáló neve
> - SQL Server neve – **corpsql**
> - Jelszó – <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>A **SharePoint 2016** telepítése

> [!NOTE]
> A telepítő előfeltételeinek letöltéséhez internetkapcsolat szükséges. Ha a számítógép olyan virtuális hálózaton található, amely nem biztosít internetkapcsolatot, akkor bővítse a számítógépet internetkapcsolatot biztosító hálózati illesztővel. Ez a telepítés befejezése után letiltható.

A SharePoint 2016 telepítéséhez kövesse az alábbi lépéseket. A telepítés befejezése után a kiszolgáló újraindul.

1.  Indítsa el a PowerShellt tartományi fiókként helyi rendszergazdaként a **corpservice** és a **sysadmin** on SQL Database-kiszolgálón. a **contoso\miminstall**-t fogjuk használni.

    -   Váltson arra a könyvtárra, amelybe a SharePointot kicsomagolta.

    -   Írja be a következő parancsot.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  A **SharePoint** előfeltételeinek telepítését követően telepítse a **SharePoint 2016** alkalmazást a következő parancs beírásával:

    ```
    .\setup.exe
    ```

3.  Válassza a teljes kiszolgálótípust.

4.  A telepítés befejezése után futtassa a varázslót.

## <a name="run-the-wizard-to-configure-sharepoint"></a>A SharePoint konfigurálása a varázslóval

A SharePoint és a MIM együttműködésének konfigurálásához kövesse a **SharePoint termékek konfigurálása varázsló** lépéseit.

1. A **Csatlakozás kiszolgálófarmhoz** lapon adja meg, hogy új kiszolgálófarmot szeretne kialakítani.

2. Ezt a kiszolgálót állítsa be az adatbázis-kiszolgálóként, például a konfigurációs adatbázis **corpsql** , a *Contoso\SharePoint* pedig a sharepointhoz készült adatbázis-hozzáférési fiókként.
3. Hozza létre a farm biztonsági jelszavát.

4. A konfigurációs varázslóban javasoljuk, hogy válassza ki az **előtér-** [MinRole](/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server) típusát

5. Amikor a konfigurációs varázsló befejezi a 10-es konfigurációs feladatot, kattintson a Befejezés gombra, és megnyílik egy webböngésző.

6. Ha a rendszer felszólítja az Internet Explorer előugró ablakára, akkor a folytatáshoz hitelesítse magát *Contoso\miminstall* (vagy a megfelelő rendszergazdai fiókkal).

7. A web Wizard (a webalkalmazáson belül) kattintson a **Mégse/kihagyás**gombra.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>A SharePoint előkészítése a MIM-portál futtatására

> [!NOTE]
> Az SSL-t a rendszer nem konfigurálja előre. A portálhoz való hozzáférés engedélyezése előtt konfiguráljon SSL vagy azzal egyenértékű protokollt.

1. Indítsa el a **sharepoint 2016 felügyeleti rendszerhéját** , és futtassa a következő PowerShell-szkriptet egy **SharePoint 2016**-webalkalmazás létrehozásához.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Ekkor megjelenik egy figyelmeztető üzenet arról, hogy a rendszer klasszikus Windows-hitelesítést használ, és több percig is eltarthat, míg a záró parancs sikeresen lefut. Ha a parancs futása befejeződött, a kimenetben megjelenik az új portál URL-címe. A **SharePoint 2016 felügyeleti rendszerhéj** ablakának megnyitásához nyissa meg a hivatkozást később.

2. Indítsa el a SharePoint 2016 felügyeleti rendszerhéját, és futtassa a következő PowerShell-szkriptet egy, a webalkalmazáshoz társított **SharePoint-webhelycsoport** létrehozásához.

   ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
   ```

   > [!NOTE]
   > Ellenőrizze, hogy a *CompatibilityLevel* változó eredménye "15". Ha az eredmény eltér a "15" értéktől, akkor a webhelycsoport nem lett létrehozva a megfelelő verziójú felülettel; törölje a webhelycsoportot, és hozza létre újból.

3. Tiltsa le a **SharePoint kiszolgálóoldali Viewstate** és az "Health Analysis Task (óránként, Microsoft SharePoint Foundation időzítő, minden kiszolgáló)" SharePoint-feladatot a következő PowerShell-parancsok futtatásával a **SharePoint 2016 felügyeleti rendszerhéjban**:

   ```
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. Nyisson meg egy új webböngésző lapot http://mim.contoso.com/ az Identity Management-kiszolgálón, és jelentkezzen be *contoso\miminstall*néven.  Ekkor megjelenik egy üres SharePoint-webhely, *MIM Portal* néven.

    ![A webalkalmazási portál a http://mim.contoso.com/ rendszerképen](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Másolja az URL-t, majd az Internet Explorerben nyissa meg az **Internetbeállításokat**, lépjen a **Biztonság** lapra, válassza a **Helyi intranet** zónát, majd kattintson a **Helyek** gombra.

    ![Kép: Internetbeállítások](media/MIM-DeploySP2.png)

6. A **Helyi intranet** ablakban kattintson a **Speciális** gombra, majd illessze be a másolt URL-t **A webhely felvétele a zónába** szövegmezőbe. Kattintson a **Hozzáadás** gombra, és zárja be az ablakokat.

7. Indítsa el a **Felügyeleti eszközöket**, kattintson a **Szolgáltatások** elemre, keresse meg a SharePoint-felügyeleti szolgáltatást, és ha még nem fut, indítsa el.

> [!div class="step-by-step"]  
> [«SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)

---
title: A SharePoint konfigurálása a Microsoft Identity Manager 2016-tal való használathoz | Microsoft Docs
description: A SharePoint Foundation telepítése és konfigurálása a MIM-portál oldalának üzemeltetéséhez.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: eceb1ed31b0212970d5cf0eae0bc8d96aa087ff5
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Identitáskezelési kiszolgáló beállítása: SharePoint

>[!div class="step-by-step"]
[«Az SQL Server 2016](prepare-server-sql2016.md)
[Exchange-kiszolgáló»](prepare-server-exchange.md)

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **corpdc**
> - Tartománynév – **contoso**
> - MIM szolgáltatás kiszolgálójának neve – **corpservice**
> - MIM Sync-kiszolgáló neve – **corpsync**
> - SQL Server-neve - **corpsql**
> - Jelszó – **Pass@word1**


## <a name="install-sharepoint-2016"></a>Telepítés **SharePoint 2016-ot**

> [!NOTE]
> A telepítő előfeltételeinek letöltéséhez internetkapcsolat szükséges. Ha a számítógép olyan virtuális hálózaton található, amely nem biztosít internetkapcsolatot, akkor bővítse a számítógépet internetkapcsolatot biztosító hálózati illesztővel. Ez a telepítés befejezése után letiltható.

A lépések végrehajtásával telepítse a SharePoint 2016-ot. A telepítés befejezése után a kiszolgáló újraindul.

1.  Indítsa el **PowerShell** egy tartományi fiókot a helyi rendszergazda, a **corpservice** és **sysadmin** SQL adatbázis-kiszolgálót használjuk, a **contoso\ miminstall**.

    -   Váltson arra a könyvtárra, amelybe a SharePointot kicsomagolta.

    -   Írja be a következő parancsot:

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Miután **SharePoint** előfeltételeinek telepítését követően telepítse **SharePoint 2016** a következő parancs beírásával:

    ```
    .\setup.exe
    ```

3.  Válassza a teljes kiszolgálótípust.

4.  A telepítés befejezése után futtassa a varázslót.

## <a name="run-the-wizard-to-configure-sharepoint"></a>A SharePoint konfigurálása a varázslóval

A SharePoint és a MIM együttműködésének konfigurálásához kövesse a **SharePoint termékek konfigurálása varázsló** lépéseit.

1. A **Csatlakozás kiszolgálófarmhoz** lapon adja meg, hogy új kiszolgálófarmot szeretne kialakítani.

2. Adja meg ezen a kiszolgálón, például adatbázis-kiszolgálóként **corpsql** a konfigurációs adatbázishoz, és *Contoso\SharePoint* SharePoint által használandó adatbázis-hozzáférési fiókként.
    a. A konfiguráció varázsló ajánlott kiválasztásával [MinRole](https://docs.microsoft.com/en-us/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) típusú **előtér-**
3. Hozza létre a farm biztonsági jelszavát.

4. Miután a konfigurációs varázsló végrehajtotta mind a 10 konfigurációs lépést, kattintson a Befejezés gombra. Ekkor megnyílik egy böngészőablak.

5. Az Internet Explorer előugró ablakában hitelesítse *Contoso\miminstall* (vagy a megfelelő rendszergazdai fiókot) a folytatáshoz.

6. (A webalkalmazáson belül) varázslóban kattintson **Mégse/Skip**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>A SharePoint előkészítése a MIM-portál futtatására

> [!NOTE]
> Az SSL-t a rendszer nem konfigurálja előre. A portálhoz való hozzáférés engedélyezése előtt konfiguráljon SSL vagy azzal egyenértékű protokollt.

1. Indítsa el **SharePoint 2016-kezelőfelület** , és futtassa a következő PowerShell-parancsfájl létrehozásához egy **SharePoint 2016-webalkalmazás**.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Ekkor megjelenik egy figyelmeztető üzenet arról, hogy a rendszer klasszikus Windows-hitelesítést használ, és több percig is eltarthat, míg a záró parancs sikeresen lefut. Ha a parancs futása befejeződött, a kimenetben megjelenik az új portál URL-címe. Tartsa a **SharePoint 2016-kezelőfelület** ugyanis szükség lehet hivatkozás később.

2. Indítsa el a SharePoint 2013 felügyeleti rendszerhéjat, és a következő PowerShell-parancsprogrammal hozzon létre egy **SharePoint-webhelycsoportot** az imént készített webalkalmazáshoz:

  ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Ellenőrizze, hogy eredményét a *CompatibilityLevel* változó "15". Ha az eredmény nem a "15", majd a webhelycsoport nem történt a megfelelő élmény verzió; a webhely törlése, és hozza létre újra.

3. Tiltsa le a **SharePoint kiszolgálóoldali Viewstate** és a SharePoint "állapotelemzési feladat (óránként, a Microsoft SharePoint Foundation időzítő szolgáltatása, minden kiszolgáló)" futtassa a következő PowerShell-parancsok a a  **A SharePoint 2016-kezelőfelület**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Az identitáskezelési kiszolgálón, nyisson meg egy új böngészőlapot, lépjen http://mim.contoso.com/ majd jelentkezzen be *contoso\miminstall*.  Ekkor megjelenik egy üres SharePoint-webhely, *MIM Portal* néven.

    ![: A MIM-portál http://mim.contoso.com/ kép](media/MIM-DeploySP1.png)

5. Másolja az URL-t, majd az Internet Explorerben nyissa meg az **Internetbeállításokat**, lépjen a **Biztonság** lapra, válassza a **Helyi intranet** zónát, majd kattintson a **Helyek** gombra.

    ![Kép: Internetbeállítások](media/MIM-DeploySP2.png)

6. A **Helyi intranet** ablakban kattintson a **Speciális** gombra, majd illessze be a másolt URL-t **A webhely felvétele a zónába** szövegmezőbe. Kattintson a **Hozzáadás** gombra, és zárja be az ablakokat.

7. Indítsa el a **Felügyeleti eszközöket**, kattintson a **Szolgáltatások** elemre, keresse meg a SharePoint-felügyeleti szolgáltatást, és ha még nem fut, indítsa el.

>[!div class="step-by-step"]  
[«Az SQL Server 2016](prepare-server-sql2016.md)
[Exchange-kiszolgáló»](prepare-server-exchange.md)

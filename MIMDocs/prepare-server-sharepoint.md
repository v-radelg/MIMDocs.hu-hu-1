---
title: "A SharePoint konfigurálása a Microsoft Identity Manager 2016-tal való használathoz | Microsoft Docs"
description: "A SharePoint Foundation telepítése és konfigurálása a MIM-portál oldalának üzemeltetéséhez."
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8646620f8f0ea8e1bdea3705e3db8593aa5a464e
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/12/2017
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Identitáskezelési kiszolgáló beállítása: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó – **Pass@word1**


## <a name="install-sharepoint-foundation-2013-with-sp1"></a>Telepítse a **SharePoint Foundation 2013-at SP1 szervizcsomaggal**.

> [!NOTE]
> A telepítő előfeltételeinek letöltéséhez internetkapcsolat szükséges. Ha a számítógép olyan virtuális hálózaton található, amely nem biztosít internetkapcsolatot, akkor bővítse a számítógépet internetkapcsolatot biztosító hálózati illesztővel. Ez a telepítés befejezése után letiltható.

A SharePoint Foundation 2013 SP1 telepítéséhez kövesse a következő lépéseket. A telepítés befejezése után a kiszolgáló újraindul.

1.  Tartományi rendszergazdaként indítsa el a **PowerShellt**.

    -   Váltson arra a könyvtárra, amelybe a SharePointot kicsomagolta.

    -   Írja be a következő parancsot:

        ```
        .\prerequisiteinstaller.exe
        ```

2.  A **SharePoint** előfeltételeinek telepítését követően telepítse a következő paranccsal telepítse a **SharePoint Foundation 2013-at SP1 szervizcsomaggal** :

    ```
    .\setup.exe
    ```

3.  Válassza a teljes kiszolgálótípust.

4.  A telepítés befejezése után futtassa a varázslót.

## <a name="run-the-wizard-to-configure-sharepoint"></a>A SharePoint konfigurálása a varázslóval

A SharePoint és a MIM együttműködésének konfigurálásához kövesse a **SharePoint termékek konfigurálása varázsló** lépéseit.

1. A **Csatlakozás kiszolgálófarmhoz** lapon adja meg, hogy új kiszolgálófarmot szeretne kialakítani.

2. Ezt a kiszolgálót jelölje ki adatbázis-kiszolgálóként a konfigurációs adatbázishoz, a SharePoint által használandó adatbázis-hozzáférési fiókként pedig a *Contoso\SharePoint* fiókot állítsa be.

3. Hozza létre a farm biztonsági jelszavát.

4. Miután a konfigurációs varázsló végrehajtotta mind a 10 konfigurációs lépést, kattintson a Befejezés gombra. Ekkor megnyílik egy böngészőablak.

5. A folytatáshoz az előugró Internet Explorer-ablakban hitelesítse a *Contoso\Rendszergazda* fiókot (vagy a megfelelő tartományi rendszergazdai fiókot).

6. A SharePoint-farm konfigurálásához indítsa el a varázslót (a webalkalmazáson belül).

7. Jelölje be a meglévő felügyelt fiók (*Contoso\SharePoint*) használatára vonatkozó beállítást, majd kattintson a **Tovább** gombra.

8. A **Webhelycsoport létrehozása** ablakban kattintson a **Kihagyás** gombra.  Ezután kattintson a **Befejezés** gombra.

## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>A SharePoint előkészítése a MIM-portál futtatására

> [!NOTE]
> Az SSL-t a rendszer nem konfigurálja előre. A portálhoz való hozzáférés engedélyezése előtt konfiguráljon SSL vagy azzal egyenértékű protokollt.

1. Indítsa el a **SharePoint 2013 felügyeleti rendszerhéjat**, és a következő PowerShell-parancsprogrammal hozzon létre egy **SharePoint Foundation 2013-webalkalmazást**.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > Ekkor megjelenik egy figyelmeztető üzenet arról, hogy a rendszer klasszikus Windows-hitelesítést használ, és több percig is eltarthat, míg a záró parancs sikeresen lefut. Ha a parancs futása befejeződött, a kimenetben megjelenik az új portál URL-címe. Ne zárja be a **SharePoint 2013 felügyeleti rendszerhéjat**, később ugyanis szükség lehet az itt szereplő információkra.

2. Indítsa el a SharePoint 2013 felügyeleti rendszerhéjat, és a következő PowerShell-parancsprogrammal hozzon létre egy **SharePoint-webhelycsoportot** az imént készített webalkalmazáshoz:

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Győződjön meg arról, hogy a *CompatibilityLevel* (Kompatibilitási szint) változó eredménye „14”. Ha az eredmény 15, akkor a webhelycsoportot nem a 2010-es verzióhoz hozták létre. Ebben az esetben törölje, majd hozza létre újra a webhelycsoportot.

3. Futtassa a következő PowerShell-parancsokat a **SharePoint 2013 felügyeleti rendszerhéjban** a **SharePoint Server-oldali Viewstate** és az „Állapotelemzési feladat (Óránként, A Microsoft SharePoint Foundation időzítő szolgáltatása, Minden kiszolgáló)” letiltásához:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Az identitáskezelési kiszolgálón nyisson meg egy új böngészőlapot, lépjen a http://localhost:82/ címre, majd jelentkezzen be a *contoso\Rendszergazda* fiókkal.  Ekkor megjelenik egy üres SharePoint-webhely, *MIM Portal* néven.

    ![Kép: MIM-portál a http://localhost:82 címen](media/MIM-DeploySP1.png)

5. Másolja az URL-t, majd az Internet Explorerben nyissa meg az **Internetbeállításokat**, lépjen a **Biztonság** lapra, válassza a **Helyi intranet** zónát, majd kattintson a **Helyek** gombra.

    ![Kép: Internetbeállítások](media/MIM-DeploySP2.png)

6. A **Helyi intranet** ablakban kattintson a **Speciális** gombra, majd illessze be a másolt URL-t **A webhely felvétele a zónába** szövegmezőbe. Kattintson a **Hozzáadás** gombra, és zárja be az ablakokat.

7. Indítsa el a **Felügyeleti eszközöket**, kattintson a **Szolgáltatások** elemre, keresse meg a SharePoint-felügyeleti szolgáltatást, és ha még nem fut, indítsa el.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

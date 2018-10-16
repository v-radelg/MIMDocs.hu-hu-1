---
title: A PAM üzembe helyezése, 3. lépés – PAM-kiszolgáló | Microsoft Docs
description: Készítsen elő a Privileged Access Management számára egy PAM-kiszolgálót mind az SQL, mind a SharePoint üzemeltetéséhez.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 99c99d1808142ae60f90a44f43208a8a0a25e10f
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333428"
---
# <a name="step-3--prepare-a-pam-server"></a>3. lépés – PAM-kiszolgáló előkészítése

> [!div class="step-by-step"]
> [« 2. lépés](step-2-prepare-priv-domain-controller.md)
> [4. lépés »](step-4-install-mim-components-on-pam-server.md)

## <a name="install-windows-server-2012-r2"></a>A Windows Server 2012 R2 telepítése

Egy harmadik, virtuális gépen telepítse a Windows Server 2012 R2-t, konkrétabban a Windows Server 2012 R2 Standard (kiszolgáló grafikus felhasználói felülettel) x 64 kiadást a *PAMSRV* létrehozásához. Mivel az SQL Servert és a SharePoint 2013-at is erre a számítógépre telepíti, legalább 8 GB RAM-ra lesz szüksége.

1. Válassza a **Windows Server 2012 R2 Standard (kiszolgáló grafikus felhasználói felülettel) x64** kiadást.

    ![A grafikus felhasználói felületű Windows Server Standard kiválasztása – képernyőkép](media/PAM_GS_Select_WS2012.png)

2. Olvassa el és fogadja el a licencfeltételeket.

3.  Mivel a lemez üres, válassza az **Egyéni: A Windows telepítése a korábbi adatok megőrzése nélkül** beállítást, és használja a **nem inicializált lemezterületet**.

4.  Rendszergazdaként jelentkezzen be az új számítógépre.  A Vezérlőpulton adjon statikus IP-címet a számítógépnek a virtuális hálózatban, állítsa be, hogy a hálózati adapter a PRIVDC IP-címére küldje a DNS-lekérdezéseket, majd állítsa be a *PAMSRV* nevet a számítógépnek.  Ehhez a kiszolgáló újraindítása szükséges.

5.  Ha a virtuális hálózat nem biztosít internetkapcsolatot, akkor bővítse a számítógépet internetkapcsolatot biztosító hálózati adapterrel.  Erre a SharePoint telepítéséhez van szükség, és ennek a lépésnek befejezése után letilthatja.

6.  A kiszolgáló újraindítása után jelentkezzen be rendszergazdaként. A Vezérlőpulton állítsa be a számítógépet a frissítések keresésére, és telepítse a szükséges frissítéseket.  Ehhez a kiszolgáló újraindítására lehet szükség.

7.  A kiszolgáló újraindítása után jelentkezzen be rendszergazdaként, nyissa meg a Vezérlőpultot, és csatlakoztassa a PAMSRV kiszolgálót a PRIV tartományhoz (priv.contoso.local).  Ehhez meg kell adnia egy PRIV tartományi rendszergazda (PRIV\Rendszergazda) felhasználónevét és hitelesítő adatait. Miután az üdvözlő üzenet megjelenik, zárja be a párbeszédpanelt, és indítsa újra a kiszolgálót.


### <a name="add-the-web-server-iis-and-application-server-roles"></a>A webkiszolgálói (IIS) és az alkalmazáskiszolgálói szerepkörök beállítása

Vegye fel a Webkiszolgáló (IIS) és az Alkalmazáskiszolgáló szerepkört, a .NET-keretrendszer 3.5 funkcióit, a Windows PowerShellhez készült Microsoft Azure Active Directory-modult, illetve a SharePointhoz szükséges egyéb funkciókat

1.  Jelentkezzen be PRIV tartományi rendszergazdaként (PRIV\Rendszergazda), és indítsa el a PowerShellt.

2.  Írja be a következő parancsokat: A .NET-keretrendszer 3.5 forrásfájljainak helye eltérő lehet, ez esetben azt kell megadni. Ezek a funkciók általában nem érhetők el a Windows Server telepítésekor, csak a következő mappában az operációs rendszer telepítésére szolgáló lemezen, pl.: „d:\Sources\SxS\”.

    ```PowerShell
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### <a name="configure-the-server-security-policy"></a>A kiszolgálói biztonsági házirend konfigurálása

A kiszolgálói biztonsági házirendben engedélyezze az újonnan létrehozott fiókok szolgáltatásként történő futtatását.

1.  Indítsa el a **Helyi biztonsági házirend** programot.   
2.  Keresse meg a **Helyi házirend** > **Felhasználói jogok kiosztása** csomópontot.  
3.  A részletek ablaktábláján kattintson a jobb gombbal a **Bejelentkezés szolgáltatásként** lehetőségre, majd válassza a **Tulajdonságok** pontot.  
4.  Kattintson a **Felhasználó vagy csoport hozzáadása** elemre, és a Felhasználó- és csoportnevek területen írja be a *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer* karakterláncot. Kattintson a **Névellenőrzés**, majd az **OK** gombra.  

5.  A Tulajdonságok ablak bezárásához kattintson az **OK** gombra.
6.  A részletek ablaktábláján kattintson a jobb gombbal **A számítógép hálózati elérésének megtagadása** elemre, majd a **Tulajdonságok** pontra.  
7.  Kattintson a **Felhasználó vagy csoport hozzáadása** elemre, és a Felhasználó- és csoportnevek területen írja be a *priv\mimmonitor; priv\MIMService; priv\mimcomponent* karakterláncot, majd kattintson az **OK** gombra.  
8.  A Tulajdonságok ablak bezárásához kattintson az **OK** gombra.

9. A részletek ablaktábláján kattintson a jobb gombbal a **Helyi bejelentkezés megtagadása** elemre, majd a **Tulajdonságok** pontra.  
10. Kattintson a **Felhasználó vagy csoport hozzáadása** elemre, és a Felhasználó- és csoportnevek területen írja be a *priv\mimmonitor; priv\MIMService; priv\mimcomponent* karakterláncot, majd kattintson az **OK** gombra.  
11. A Tulajdonságok ablak bezárásához kattintson az **OK** gombra.  
12. Zárja be a Helyi biztonsági házirend ablakot.  

13. Nyissa meg a Vezérlőpultot, és váltson a **Felhasználói fiókok** területre.
14. Kattintson a **Hozzáférés megadása a számítógéphez más felhasználóknak** lehetőségre.
15. Kattintson a **Hozzáadás** elemre, adja meg a *PRIV* tartomány *MIMADMIN* felhasználóját, és a következő képernyőn, a varázslóban, kattintson az **Add this user as an Administrator** (A felhasználó hozzáadása rendszergazdaként) lehetőségre.  
16. Kattintson a **Hozzáadás** elemre, adja meg a *PRIV* tartomány *SharePoint* felhasználóját, és a következő képernyőn, a varázslóban, kattintson az **Add this user as an Administrator** (A felhasználó hozzáadása rendszergazdaként) lehetőségre.  
17. Zárja be a Vezérlőpultot.

### <a name="change-the-iis-configuration"></a>Az IIS konfigurációjának módosítása

Ahhoz, hogy az alkalmazások számára engedélyezhesse a Windows-hitelesítési mód használatát, az IIS-konfigurációt kétféleképpen módosíthatja. Győződjön meg róla, hogy MIMAdmin néven jelentkezett be, és kövesse az alábbi lehetőségek egyikét.

Ha a PowerShellt szeretné használni:

1. Kattintson jobb gombbal a PowerShellre, és válassza a **Futtatás rendszergazdaként** elemet.
2. Az alábbi parancsokkal állítsa le az IIS-t, és oldja fel az alkalmazásgazda beállításainak zárolását
   ```CMD
   iisreset /STOP
   C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
   iisreset /START
   ```

Ha szövegszerkesztőt, például a Jegyzettömböt szeretné használni:

1. Nyissa meg a **C:\Windows\System32\inetsrv\config\applicationHost.config** fájlt.
2. Görgessen le a fájl 82. soráig. Az **overrideModeDefault** címke értékének a következőnek kell lennie: **<section name="windowsAuthentication" overrideModeDefault="Deny" />**.  
3. Módosítsa az **overrideModeDefault** értékét az *Allow* értékre.  
4. Mentse a fájlt, és indítsa újra az IIS-t az `iisreset /START` PowerShell-paranccsal.

## <a name="install-sql-server"></a>Az SQL Server telepítése

Ha az SQL Server még nincs jelen a megerősített környezetben, telepítse az SQL Server 2012 (Service Pack 1 vagy újabb) vagy az SQL Server 2014 verziót. A következő lépések az SQL 2014 használatát feltételezik.

1. Győződjön meg róla, hogy MIMAdmin néven van bejelentkezve.
2. Kattintson jobb gombbal a PowerShellre, és válassza a **Futtatás rendszergazdaként** elemet.
3. Keresse meg azt a könyvtárat, amelyben az SQL Server telepítője található.  
4. Írja be a következő parancsot:  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1" /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## <a name="install-sharepoint-foundation-2013"></a>A SharePoint Foundation 2013 telepítése

Telepítse a SharePoint szoftver-előfeltételeit a PAMSRV kiszolgálóra a SharePoint Foundation 2013 SP1 szervizcsomagot is tartalmazó telepítőjével.

> [!NOTE]
> Az előfeltételek letöltéséhez a telepítőnek internetkapcsolatra van szüksége. A telepítés után a kiszolgáló újraindul.

1. Kattintson jobb gombbal a PowerShellre, és válassza a **Futtatás rendszergazdaként** elemet.  
2. Váltson arra a könyvtárra, amelybe a SharePointot kicsomagolta.  
3. Írja be a `.\prerequisiteinstaller.exe` parancsot.

A SharePoint előfeltételeinek telepítését követően telepítse a SharePoint Foundation 2013-at SP1 szervizcsomaggal.

1.  Kattintson jobb gombbal a PowerShellre, és válassza a **Futtatás rendszergazdaként** elemet.  
2.  Váltson arra a könyvtárra, amelybe a SharePointot kicsomagolta.  
3.  Írja be a `.\setup.exe` parancsot.  
4.  Válassza a **teljes kiszolgáló** típust.  
5.  A telepítés befejezése után futtassa a varázslót.  

### <a name="configure-sharepoint"></a>A SharePoint konfigurálása

A SharePoint konfigurálásához futtassa a SharePoint termékek konfigurálása varázslót.

1.  A Csatlakozás kiszolgálófarmhoz lapon adja meg, hogy **új kiszolgálófarmot szeretne kialakítani**.  
2.  A **PAMSRV** kiszolgálót jelölje ki adatbázis-kiszolgálóként a konfigurációs adatbázishoz, a SharePoint által használandó adatbázis-hozzáférési fiókként pedig a **PRIV\SharePoint** fiókot állítsa be.  
3.  Adja meg a farm biztonsági jelszavát (az útmutató során ezt a jelszót nem használjuk többet).  
4.  Egyelőre fogadja el a SharePoint konfigurációs varázslójának többi alapértelmezett beállítását egy egykiszolgálós farm létrehozásához.    
5.  Miután a konfigurációs varázsló végrehajtotta mind a 10 konfigurációs lépést, kattintson a **Befejezés** gombra. Ekkor megnyílik egy böngészőablak.  
6.  Az Internet Explorer előugró ablakában hitelesítse magát tartományi rendszergazdaként (PRIV\MIMAdmin) a folytatáshoz.  
7.  A SharePoint-farm konfigurálásához indítsa el a varázslót a webalkalmazáson belül.  
8.  Válassza ki a meglévő felügyelt fiók használatát (PRIV\SharePoint), törölje a jeleket a választható szolgáltatások letiltásához, majd kattintson a **Tovább** gombra.  
9. Ha a Webhelycsoport létrehozása ablak megjelenik, kattintson a **Kihagyás**, majd a **Befejezés** gombra.  

## <a name="create-a-sharepoint-foundation-2013-web-application"></a>SharePoint Foundation 2013-webalkalmazás létrehozása

A varázslók befejezése után a PowerShell segítségével hozza létre a SharePoint Foundation 2013-webalkalmazást a MIM portál futtatásához. Mivel ez az útmutató bemutatásra szolgál, nem engedélyezzük az SSL-t.

1.  Kattintson jobb gombbal a SharePoint 2013 – felügyeleti rendszerhéj elemre, válassza a **Futtatás rendszergazdaként** lehetőséget, és futtassa a következő PowerShell-parancsfájlt:

    ```PowerShell
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Ekkor megjelenik egy figyelmeztető üzenet arról, hogy a rendszer klasszikus Windows-hitelesítést használ, és több percig is eltarthat, míg a záró parancs sikeresen lefut.  Ha a parancs futása befejeződött, a kimenetben megjelenik az új portál URL-címe.

> [!NOTE]
> Ne zárja be a SharePoint 2013 felügyeleti rendszerhéj ablakát, mivel a következő lépésben szükség lesz rá.

## <a name="create-a-sharepoint-site-collection"></a>SharePoint-webhelycsoport létrehozása

Most hozzon létre egy SharePoint-webhelycsoportot az imént készített webalkalmazáshoz, mely a MIM portált üzemelteti.

1.  Nyissa meg a **SharePoint 2013 – felügyeleti rendszerhéj** ablakot, ha még nincs megnyitva, és futtassa a következő PowerShell-parancsfájlt

    ```PowerShell
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Győződjön meg arról, hogy a **CompatibilityLevel** (Kompatibilitási szint) változónál a *14* érték van megadva. Ha a visszaadott érték *15*, akkor a webhelycsoport nem a 2010-es verzióhoz jött létre. Ebben az esetben törölje, majd hozza létre újra a webhelycsoportot.

2.  Futtassa a következő PowerShell-parancsokat a **SharePoint 2013 felügyeleti rendszerhéjban**. Ez letiltja a SharePoint kiszolgálóoldali Viewstate funkciót és az **Állapotelemzési feladat (Óránként, A Microsoft SharePoint Foundation időzítő szolgáltatása, Minden kiszolgáló)** SharePoint-feladatot.

    ```PowerShell
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## <a name="change-update-settings"></a>Frissítési beállítások módosítása

1. Nyissa meg a Vezérlőpultot, lépjen a **Windows Update** pontra, és kattintson a **Beállítások módosítása** lehetőségre.  
2. Módosítsa úgy a beállításokat, hogy megkapja a frissítéseket a Windows Update szolgáltatástól, illetve a Microsoft Updates többi termékétől.  
3. Keressen új frissítéseket, és a folytatás előtt mindenképpen telepítsen minden fontos, függőben lévő frissítést.

## <a name="set-the-website-as-the-local-intranet"></a>A webhely beállítása helyi intranetként

1. Indítsa el az Internet Explorert, és nyisson meg egy új böngészőlapot
2. Navigáljon a http://pamsrv.priv.contoso.local:82/ , és jelentkezzen be PRIV\MIMAdmin felhasználóként.  Ekkor megjelenik egy üres SharePoint-webhely, „MIM Portal” néven.  
3. Az Internet Explorerben nyissa meg az **Internetbeállításokat**, lépjen a **Biztonság** lapra, válassza a **Helyi intranet** zónát, majd kattintson a `http://pamsrv.priv.contoso.local:82/` URL-címre.

Ha a bejelentkezés sikertelen, előfordulhat, hogy a [2. lépésben](step-2-prepare-priv-domain-controller.md) korábban létrehozott Kerberos SPN-eket frissíteni kell.

## <a name="start-the-sharepoint-administration-service"></a>A SharePoint felügyeleti szolgáltatás elindítása

A **Szolgáltatások** eszköz segítségével (mely a Felügyeleti eszközök között található), indítsa el a **SharePoint felügyeleti szolgáltatást**, ha még nem fut.

A 4. lépésben hozzákezdhet a MIM-összetevőknek a PAM-kiszolgálóra való telepítéséhez.

> [!div class="step-by-step"]
> [« 2. lépés](step-2-prepare-priv-domain-controller.md)
> [4. lépés »](step-4-install-mim-components-on-pam-server.md)

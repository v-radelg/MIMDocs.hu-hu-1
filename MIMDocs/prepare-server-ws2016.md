---
title: Konfigurálja a Windows Server 2016 a MIM 2016 SP1 |} Microsoft Docs
description: A lépései és minimumkövetelményei előkészítése a MIM 2016 SP1 működéséhez Windows Server 2016 beolvasása.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: bfc79d27f015ee3d57c33c26ecae0f5b8ff38370
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289489"
---
# <a name="set-up-an-identity-management-servers-windows-server-2016"></a>Az identitás-felügyeleti kiszolgáló beállítása: Windows Server 2016

> [!div class="step-by-step"]
> [«Tartomány előkészítése](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
> 
> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **corpdc**
> - Tartománynév – **contoso**
> - MIM szolgáltatás kiszolgálójának neve – **corpservice**
> - MIM Sync-kiszolgáló neve – **corpsync**
> - SQL Server-neve - **corpsql**
> - Jelszó – <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Windows Server 2016 csatlakoztatása a tartományhoz

Készítsen elő egy Windows Server 2016 számítógépet legalább 8 – 12GB RAM-mal. A telepítéskor válassza "A Windows Server 2016 Standard vagy Datacenter (kiszolgáló grafikus felhasználói Felülettel rendelkező) x 64" kiadást.

1. Rendszergazdaként jelentkezzen be az új számítógépre.

2. A Vezérlőpulton osszon ki a számítógépnek egy statikus IP-címet a hálózaton. Konfigurálja, hogy a hálózati illesztő DNS-lekérdezéseket küldjön az előző lépésben a tartományvezérlő IP-címét, és a számítógép neve **CORPSERVICE**.  Ehhez a kiszolgáló újraindítása szükséges.

3. Nyissa meg a Vezérlőpultot, és a számítógép csatlakoztatása a tartományhoz, a legutóbbi lépésben konfigurált *contoso.com*.  Ehhez meg kell adnia a tartományi rendszergazdai fiók – például *Contoso\Rendszergazda* felhasználónevét és hitelesítő adatait.  Miután az üdvözlő üzenet megjelenik, zárja be a párbeszédpanelt, és még egyszer indítsa újra a kiszolgálót.

4. Jelentkezzen be a számítógépre *CORPSERVICE* tartományi fiókként a helyi számítógép rendszergazdai például *Contoso\MIMINSTALL*.


5. Rendszergazdaként nyisson meg egy PowerShell-ablakot, és írja be a következő parancsot a számítógép frissítéséhez a csoportházirend-beállításokkal.

    ```
    gpupdate /force /target:computer
    ```

    A folyamat egy percen belül befejeződik, és a következő üzenet jelenik meg: „A számítógép-házirend frissítése sikeresen befejeződött.”

6. Vegye fel a **Webkiszolgáló (IIS)** és az **Alkalmazáskiszolgáló** szerepköröket, a **.NET-keretrendszer** 3.5, 4.0 és 4.5 funkcióit és az **Active Directory-modult a Windows PowerShell környezethez**.

    ![Kép: PowerShell-funkciók](media/MIM-DeployWS2.png)

7. A PowerShell-ablakba írja be következő parancsokat: Elképzelhető, hogy a **.NET-keretrendszer** 3.5 funkcióhoz tartozó forrásfájlokhoz más helyet kell megadni. Ezek a funkciók általában nem érhetők el a Windows Server telepítésekor, csak a következő mappában az operációs rendszer telepítésére szolgáló lemezen, pl.: „\*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>A kiszolgálói biztonsági házirend konfigurálása

A kiszolgálói biztonsági házirendben engedélyezze az újonnan létrehozott fiókok szolgáltatásként történő futtatását.
> [!NOTE] 
> Attól függően, hogy a konfigurációs egyetlen server(all-in-one) vagy csak kell hozzáadnia az elosztott kiszolgáló szerepköre alapján a tag gép például szinkronizálási kiszolgáló. 

1. A Helyi biztonsági házirend program elindítása

2. Keresse meg a **Helyi házirend > Felhasználói jogok kiosztása** csomópontot.

3. A Részletek panelen kattintson a jobb gombbal a **Bejelentkezés szolgáltatásként** lehetőségre, majd válassza a **Tulajdonságok** pontot.

    ![Kép: Helyi biztonsági házirend](media/MIM-DeployWS3.png)

4. Kattintson a **felhasználó vagy csoport hozzáadása**, és a szövegmezőben típus a következő szerepkör alapján `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, kattintson a **Névellenőrzés**, és kattintson a **OK**.

5. Kattintson az **OK** gombra a **Bejelentkezés szolgáltatásként – tulajdonságok** ablak bezárásához.

6.  A részletek panelen kattintson a jobb gombbal **megtagadja a hozzáférést a hálózati**, és válassza ki **tulajdonságok**. >

[!NOTE] Ha külön szerepkör kiszolgálók ebben a lépésben megszakítja néhány megfeleltetésével, mint SSPR-szolgáltatás.

7. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMService`, majd kattintson az **OK** gombra.

8. Az **OK** gombbal zárja be **A számítógép hálózati elérésének megtagadása – tulajdonságok** ablakot.

9. A Részletek panelen kattintson a jobb gombbal a **Helyi bejelentkezés megtagadása** elemre, majd a **Tulajdonságok** pontra.

10. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMService`, majd kattintson az **OK** gombra.

11. Kattintson az **OK** gombra a **Helyi bejelentkezés megtagadása – tulajdonságok** ablak bezárásához.

12. Zárja be a Helyi biztonsági házirend ablakot.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Az IIS Windows-hitelesítés üzemmód módosítása, szükség esetén

1.  Indítson el egy PowerShell-ablakot.

2.  Az *iisreset /STOP* paranccsal állítsa le az IIS szolgáltatást.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Tartomány előkészítése](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)

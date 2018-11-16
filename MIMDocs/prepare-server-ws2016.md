---
title: Konfigurálja a MIM 2016 SP1-et a Windows Server 2016 |} A Microsoft Docs
description: A lépéseket és a Windows Server 2016-ra való együttműködéshez a MIM 2016 SP1-et előkészítési beolvasása.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a0fa1e837fd73872043748ee73f19a29d1d1412f
ms.sourcegitcommit: 3b514aba69af203f176b40cdb7c2a51c477c944a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/16/2018
ms.locfileid: "51718336"
---
# <a name="set-up-an-identity-management-server-windows-server-2016"></a>Identitáskezelési kiszolgáló beállítása: Windows Server 2016-ban

> [!div class="step-by-step"]
> [«Tartomány előkészítése](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
> 
> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **corpdc-re**
> - Tartománynév – **contoso**
> - MIM szolgáltatás kiszolgálójának neve – **corpservice**
> - MIM Sync-kiszolgáló neve – **corpsync**
> - Az SQL Server neve – **corpsql**
> - Jelszó – <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>A Windows Server 2016 csatlakoztatása a tartományhoz

Indítsa el a Windows Server 2016 számítógépet legalább 8 – 12GB RAM. A telepítéskor válassza a "Windows Server 2016 Standard vagy Datacenter (kiszolgáló grafikus felhasználói felülettel) x 64" kiadást.

1. Rendszergazdaként jelentkezzen be az új számítógépre.

2. A Vezérlőpulton osszon ki a számítógépnek egy statikus IP-címet a hálózaton. Konfigurálja a hálózati adapter DNS-lekérdezéseket küldjön az előző lépésben a tartományvezérlő IP-címet, majd állítsa be a számítógép nevét **CORPSERVICE**.  Ez a művelet egy kiszolgáló újraindítást igényel.

3. Nyissa meg a Vezérlőpultot, és csatlakoztassa a számítógépet a tartományhoz, a legutóbbi lépésben konfigurált *contoso.com*.  Ez a művelet tartalmaz, a felhasználónév és a egy tartományi rendszergazda hitelesítő adatait például így *contoso\rendszergazda*.  Miután az üdvözlő üzenet megjelenik, zárja be a párbeszédpanelt, és még egyszer indítsa újra a kiszolgálót.

4. Jelentkezzen be a számítógép *CORPSERVICE* tartományi fiókként a helyi számítógép rendszergazdák például *Contoso\MIMINSTALL*.


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
> Attól függően, a konfiguráció egyetlen server(all-in-one) elosztott-kiszolgálók, vagy csak hozzá kell a tag gép, például a szinkronizálási kiszolgáló szerepköre alapján. 

1. A Helyi biztonsági házirend program elindítása

2. Keresse meg a **Helyi házirend > Felhasználói jogok kiosztása** csomópontot.

3. A részletek ablaktábláján kattintson a jobb gombbal a **szolgáltatásként jelentkezzen be**, és válassza ki **tulajdonságok**.

    ![Kép: Helyi biztonsági házirend](media/MIM-DeployWS3.png)

4. Kattintson a **felhasználó vagy csoport hozzáadása**, és a szövegmezőbe írja be a következő szerepkör alapján `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, kattintson a **Névellenőrzés**, és kattintson a **OK**.

5. Kattintson az **OK** gombra a **Bejelentkezés szolgáltatásként – tulajdonságok** ablak bezárásához.

6.  A részletek ablaktábláján kattintson a jobb gombbal a **megtagadja a hozzáférést ehhez a számítógéphez a hálózatról**, és válassza ki **tulajdonságok**. >

[!NOTE] Kiszolgálók szerepkör szétválasztása megszakítja a bizonyos funkciók, például az SSPR.

7. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMService`, majd kattintson az **OK** gombra.

8. Az **OK** gombbal zárja be **A számítógép hálózati elérésének megtagadása – tulajdonságok** ablakot.

9. A részletek ablaktábláján kattintson a jobb gombbal a **helyi bejelentkezés megtagadása**, és válassza ki **tulajdonságok**.

10. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMService`, majd kattintson az **OK** gombra.

11. Kattintson az **OK** gombra a **Helyi bejelentkezés megtagadása – tulajdonságok** ablak bezárásához.

12. Zárja be a Helyi biztonsági házirend ablakot.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Ha szükséges, módosítsa az IIS Windows-hitelesítési mód

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

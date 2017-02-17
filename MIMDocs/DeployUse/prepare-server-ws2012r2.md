---
title: "A Windows Server 2012 R2 konfigurálása a MIM 2016-tal való használathoz | Microsoft Docs"
description: "A Windows Server 2012 R2 és a MIM 2016 együttműködésének előkészítési lépései és minimumkövetelményei"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 1cb0d6cd310372ecaeff47c9cc4461ebc43b3390


---

# <a name="set-up-an-identity-management-server-windows-server-2012-r2"></a>Identitáskezelési kiszolgáló beállítása: Windows Server 2012 R2

>[!div class="step-by-step"]
[« Tartomány előkészítése](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó – **Pass@word1**

## <a name="join-windows-server-2012-r2-to-your-domain"></a>A Windows Server 2012 R2 csatlakoztatása a tartományhoz

Készítsen elő egy Windows Server 2012 R2 rendszerű számítógépet legalább 8 GB RAM-mal. A telepítéskor válassza a „Windows Server 2012 R2 Standard (kiszolgáló grafikus felhasználói felülettel) x64” kiadást.

1. Rendszergazdaként jelentkezzen be az új számítógépre.

2. A Vezérlőpulton osszon ki a számítógépnek egy statikus IP-címet a hálózaton. Állítsa be, hogy a hálózati illesztő DNS-lekérdezéseket küldjön az előző lépésben megadott tartományvezérlői IP-címre, majd állítsa be a **CORPIDM** nevet a számítógépnek.  Ehhez a kiszolgáló újraindítása szükséges.

3. Nyissa meg a Vezérlőpultot, és csatlakoztassa a számítógépet a legutóbbi lépésben konfigurált *contoso.local* tartományhoz.  Ehhez meg kell adnia a tartományi rendszergazdai fiók – például *Contoso\Rendszergazda* felhasználónevét és hitelesítő adatait.  Miután az üdvözlő üzenet megjelenik, zárja be a párbeszédpanelt, és még egyszer indítsa újra a kiszolgálót.

4. Jelentkezzen be a *CorpIDM* számítógépre tartományi rendszergazdaként, például a *Contoso\Rendszergazda* fiókkal.

5. Rendszergazdaként nyisson meg egy PowerShell-ablakot, és írja be a következő parancsot a számítógép frissítéséhez a csoportházirend-beállításokkal.

    ```
    gpupdate /force /target:computer
    ```

    A folyamat egy percen belül befejeződik, és a következő üzenet jelenik meg: „A számítógép-házirend frissítése sikeresen befejeződött.”

6. Vegye fel a **Webkiszolgáló (IIS)** és az **Alkalmazáskiszolgáló** szerepköröket, a **.NET-keretrendszer** 3.5, 4.0 és 4.5 funkcióit és az **Active Directory-modult a Windows PowerShell környezethez**.

    ![Kép: PowerShell-funkciók](media/MIM-DeployWS2.png)

7. A PowerShell-ablakba írja be következő parancsokat: Elképzelhető, hogy a **.NET-keretrendszer** 3.5 funkcióhoz tartozó forrásfájlokhoz más helyet kell megadni. Ezek a funkciók általában nem érhetők el a Windows Server telepítésekor, csak a következő mappában az operációs rendszer telepítésére szolgáló lemezen, pl.: „*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>A kiszolgálói biztonsági házirend konfigurálása

A kiszolgálói biztonsági házirendben engedélyezze az újonnan létrehozott fiókok szolgáltatásként történő futtatását.

1. A Helyi biztonsági házirend program elindítása

2. Keresse meg a **Helyi házirend > Felhasználói jogok kiosztása** csomópontot.

3. A Részletek panelen kattintson a jobb gombbal a **Bejelentkezés szolgáltatásként** lehetőségre, majd válassza a **Tulajdonságok** pontot.

    ![Kép: Helyi biztonsági házirend](media/MIM-DeployWS3.png)

4. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, kattintson a **Névellenőrzés** lehetőségre, majd az **OK** gombra.

5. Kattintson az **OK** gombra a **Bejelentkezés szolgáltatásként – tulajdonságok** ablak bezárásához.

6.  A részletező panelen jobb gombbal kattintson **A számítógép hálózati elérésének megtagadása** elemre, majd a **Tulajdonságok** pontra.

7. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMService`, majd kattintson az **OK** gombra.

8. Az **OK** gombbal zárja be **A számítógép hálózati elérésének megtagadása – tulajdonságok** ablakot.

9. A Részletek panelen kattintson a jobb gombbal a **Helyi bejelentkezés megtagadása** elemre, majd a **Tulajdonságok** pontra.

10. Kattintson a **Felhasználó vagy csoport hozzáadása** gombra, a szövegmezőbe írja be a következőt: `contoso\MIMSync; contoso\MIMService`, majd kattintson az **OK** gombra.

11. Kattintson az **OK** gombra a **Helyi bejelentkezés megtagadása – tulajdonságok** ablak bezárásához.

12. Zárja be a Helyi biztonsági házirend ablakot.


## <a name="change-the-iis-windows-authentication-mode"></a>Az IIS Windows-hitelesítés üzemmód módosítása

1.  Indítson el egy PowerShell-ablakot.

2.  Az *iisreset /STOP* paranccsal állítsa le az IIS szolgáltatást.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[« Tartomány előkészítése](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)



<!--HONumber=Jan17_HO4-->



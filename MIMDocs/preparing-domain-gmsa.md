---
title: Csoportosan felügyelt szolgáltatásfiókokat beállítása Microsoft Identity Manager 2016-hez | Microsoft Docs
description: Csoportosan felügyelt szolgáltatásfiókok beállítása tartományban Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 32b346dd9cf99b617edfaca953389cba30d6681c
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043511"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>Tartományon belüli felügyelt szolgáltatásfiókok (gMSA-) forgatókönyv konfigurálása

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)

> [!IMPORTANT]
> Ez a cikk csak a webalkalmazások 2016 SP2 verziójára vonatkozik.

A Microsoft Identity Manager (MIM) az Ön Active Directory- (AD-) tartományával együtt is használható. Az AD-nek már telepítve kell lennie, és győződjön meg arról is, hogy a környezetében rendelkezik egy tartományvezérlővel egy felügyelhető tartományhoz.  Ez a cikk azt ismerteti, hogyan állítható be csoportosan felügyelt szolgáltatásfiókok az adott tartományban a felügyeleti webszolgáltatások használatához.

## <a name="overview"></a>Házirend

A csoportosan felügyelt szolgáltatásfiókok szükségtelenné teszik a szolgáltatásfiók jelszavának rendszeres módosítását. A (z) 2016 SP2 kiadásával a következő, a telepítési folyamat során használandó gMSA-fiókokkal rendelkezhet:

-   Webkiszolgálói szinkronizációs szolgáltatás (FIMSynchronizationService)
-   FIMService-szolgáltatás
-   A szolgáltatás jelszavas regisztrációjának webhelye alkalmazáskészlete
-   Webhelyre vonatkozó jelszó-visszaállítási webhelyhez tartozó alkalmazáskészlet
-   PAM REST API webhely alkalmazáskészlete
-   PAM-figyelési szolgáltatás (PamMonitoringService)
-   PAM-összetevő szolgáltatás (PrivilegeManagementComponentService)

A következő rendszerelem-összetevők nem támogatják a futtató gMSA-fiókokat:

-   Webalkalmazás-portál. Ennek az az oka, hogy a webalkalmazás-portál a SharePoint-környezet része. Ehelyett telepítheti a SharePointot a farm üzemmódba, és [konfigurálhatja az automatikus jelszó-módosítást a SharePoint-kiszolgálón](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Minden felügyeleti ügynök
-   Microsoft Tanúsítványkezelő
-   BHOLD


A gMSA-ről további információt a következő cikkekben találhat:
-   [Csoportosan felügyelt szolgáltatásfiókok – áttekintés](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [Új – ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [A Key Distribution Services KDS létrehozása](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>Felhasználói fiókok és csoportok létrehozása

A MIM-telepítés minden összetevőjének saját identitással kell rendelkeznie a tartományban. Ez vonatkozik az olyan MIM-összetevőkre is, mint a Service, a Sync, a SharePoint és az SQL.


> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **DC**
> - Tartománynév – **contoso**
> - **Mimservice** -kiszolgáló neve
> - **Mimsync** -szinkronizálási kiszolgáló neve
> - SQL Server neve – **SQL**
> - Jelszó – <strong>Pass@word1</strong>

1. Jelentkezzen be a tartományvezérlőbe tartományi rendszergazdaként (*pl.: Contoso\Administrator*).

2. Hozza létre a következő felhasználói fiókokat a MIM-szolgáltatásokhoz. Indítsa el a PowerShellt, és írja be a következő PowerShell-szkriptet új AD-tartománybeli felhasználók létrehozásához (nem minden fiók kötelező, bár a parancsfájl csak tájékoztató jellegű, ajánlott eljárás egy dedikált *MIMAdmin* -fiók használata a következőhöz: és a SharePoint telepítési folyamata).

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Hozzon létre biztonsági csoportokat az összes csoporthoz.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  Állítson be egyszerű szolgáltatásneveket (SPN), amivel lehetővé válik a Kerberos hitelesítés használata a szolgáltatásfiókokhoz.

    ```PowerShell
    setspn -S http/mim.contoso.com contoso\svcMIMAppPool
    ```

5.  Ügyeljen arra, hogy regisztrálja a következő DNS "A" rekordokat a megfelelő névfeloldáshoz (feltéve, hogy a rendszer ugyanazon a gépen fogja üzemeltetni a webhelyeket, valamint a rendszerállapot-portált, a jelszó-visszaállítási és a jelszó-regisztrálási

    - mim.contoso.com – pont – a rendszer-és a portál-kiszolgáló fizikai IP-címe
    - passwordreset.contoso.com – pont – a rendszer-és a portál-kiszolgáló fizikai IP-címe
    - passwordregistration.contoso.com – pont – a rendszer-és a portál-kiszolgáló fizikai IP-címe

## <a name="create-key-distribution-service-root-key"></a>Kulcs terjesztési szolgáltatásának legfelső szintű kulcsának létrehozása

Győződjön meg arról, hogy rendszergazdaként jelentkezett be a tartományvezérlőbe a csoport kulcsának terjesztési szolgáltatásának előkészítéséhez.

Ha a tartományhoz már van gyökérszintű kulcs (a **Get-KdsRootKey** használatával ellenőrizze), majd folytassa a következő szakasszal.

6.  Szükség esetén hozza létre a Key Distribution Services (KDS) legfelső szintű kulcsát (csak egyszer egy tartományon). A KDS szolgáltatás a legfelső szintű kulcsot használja a tartományvezérlőkön (más információkkal együtt) a jelszavak létrehozásához. Tartományi rendszergazdaként írja be a következő PowerShell-parancsot:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *–* Előfordulhat, hogy a EffectiveImmediately legfeljebb \~10 óráig késlelteti a replikálást, mivel az összes tartományvezérlőre replikálni kell. Ez a késleltetés körülbelül 1 óra volt két tartományvezérlő esetében.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >A tesztkörnyezetben vagy a tesztkörnyezetben a következő parancs futtatásával elkerülhető a replikálás késleltetése 10 órán keresztül:
    ><br/>
    >Add-KDSRootKey-EffectiveTime ((Get-Date). AddHours (-10))

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>A rendszer-szinkronizálási szolgáltatás fiókjának, csoportjának és egyszerű szolgáltatásának létrehozása
-----------------------

Győződjön meg arról, hogy az összes olyan számítógép fiókja, amelyeken a rendszerfrissítési szoftver telepítve van, már csatlakoztatva van a tartományhoz.  Ezt követően hajtsa végre ezeket a lépéseket a PowerShellben tartományi rendszergazdaként.

7.  Hozzon létre egy csoportot *MIMSync_Servers* , és adja hozzá az összes összes rendszer-szinkronizálási kiszolgálót ebbe a csoportba.
    A következő beírásával hozzon létre új AD-csoportot a fakiszolgálói szinkronizációs kiszolgálókhoz. Ezt követően a következő csoportba tartozó felhasználói fiókok hozzáadása Active Directory számítógépfiókok, például *contoso\MIMSync $* .

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  A gMSA-szinkronizálási szolgáltatás létrehozása. Írja be a következő PowerShell-t.

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Tekintse át a *Get-ADServiceAccount PowerShell-* parancs végrehajtásával létrehozott GSMA részleteit:

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  Ha azt tervezi, hogy a jelszó-módosítási értesítési szolgáltatást futtatja, akkor a PowerShell-parancs végrehajtásával regisztrálnia kell a szolgáltatás egyszerű nevét:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. Ha a "MIMSync_Server" csoporttagság módosult, a rendszer újraindításával újraindíthatja a kiszolgálóval társított Kerberos-tokent.

## <a name="create-mim-service-management-agent-service-account"></a>Rendszerfelügyeleti webszolgáltatási ügynök szolgáltatás fiókjának létrehozása

11. A Rendszerfelügyeleti webszolgáltatásokat általában egy új fiók létrehozásakor kell létrehozni a Rendszerfelügyeleti webszolgáltatások ügynöke számára.  A gMSA két lehetőség közül választhat:

- A felhasználói fiókhoz tartozó szinkronizálási szolgáltatás csoportosan felügyelt szolgáltatásfiók használata, és ne hozzon létre külön fiókot

    Kihagyhatja a fakiszolgálói szolgáltatás felügyeleti ügynökének létrehozását. Ebben az esetben a fakiszolgálói szinkronizációs szolgáltatás gMSA nevét, például a *contoso\MIMSyncGMSAsvc $* -t kell használnia, nem pedig a következőt: a webszolgáltatási fiók telepítése. A Rendszerfelügyeleti webszolgáltatások kezelése ügynök konfigurációjának későbbi szakaszában engedélyezze a *"MIMSync-fiók használata"* lehetőséget.

    Ne engedélyezze a "bejelentkezés megtagadása a hálózatról" lehetőséget a gMSA-os szinkronizációs szolgáltatáshoz, mert a (z) "hálózati bejelentkezés engedélyezése" engedély szükséges a következőhöz:

- Normál szolgáltatásfiók használata a Rendszerfelügyeleti webszolgáltatások ügynökének szolgáltatási fiókjához

    Indítsa el a PowerShellt tartományi rendszergazdaként, és írja be a következőt új AD tartományi felhasználó létrehozásához:

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    Ne engedélyezze a "Bejelentkezés a hálózatról" beállítást a "hálózati bejelentkezés engedélyezése" engedélyhez szükséges "a hálózatról érkező hozzáférés tiltása" fiókhoz.

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>A rendszerbiztonsági fiókkezelő szolgáltatásfiókok, csoportok és szolgáltatásnév létrehozása

Folytassa a PowerShell használatát tartományi rendszergazdaként.
   
12. Hozzon létre egy csoportot *MIMService_Servers* és adja hozzá az összes címtárszolgáltatás-szolgáltatást ehhez a csoporthoz.  Írja be a következő PowerShell-t, hogy új AD-csoportot hozzon létre a contoso\MIMPortal-hez, és adja hozzá a fakiszolgálói szolgáltatáshoz Active Directory számítógépfiókot, például: *$* , ebbe a csoportba.

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  A gMSA létrehozása.

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  Regisztrálja a szolgáltatás egyszerű nevét, és engedélyezze a Kerberos-delegálást a következő PowerShell-parancs végrehajtásával:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  A SSPR-forgatókönyvek esetében a szükséges, hogy a rendszer képes legyen kommunikálni a famodul szinkronizációs szolgáltatásával, ezért a MIMSyncAdministrators vagy a felhasználói adatok szinkronizálása jelszó-visszaállításhoz vagy a csoportok tallózása:

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. Az "MIMService_Servers" csoporttagság megváltozása esetén indítsa újra a-kiszolgálóval társított Kerberos-tokent, és frissítse a rendszer-kiszolgálót.

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>Ha szükséges, hozzon létre más rendszerfiókokat és csoportokat

Ha a SSPR-t konfigurálja, majd a fentiekben leírtakat követve alkalmazza a következőt:

- Webhelyre vonatkozó jelszó-visszaállítási webhelyhez tartozó alkalmazáskészlet
- A szolgáltatás jelszavas regisztrációjának webhelye alkalmazáskészlete

Ha a gMSA-t konfigurálja, majd a fentiekben leírtakat követve alkalmazza a következőt:

- A REST API webhelye alkalmazáskészlete
- A webkiszolgáló PAM-összetevőjének szolgáltatása
- A felügyeleti rendszer PAM-figyelési szolgáltatása

## <a name="specifying-a-gmsa-when-installing-mim"></a>GMSA megadása a webszolgáltatások telepítésekor

Általános szabály, hogy a legtöbb esetben, amikor a gMSA-t használja, annak megadásához, hogy normál fiók helyett egy-egy gMSA szeretne használni, fűzze hozzá a Dollar-jel karakterét, például a **contoso\MIMSyncGMSAsvc $** nevet, és hagyja üresen a jelszó mezőt. Az egyik kivétel a *miisactivate. exe* eszköz, amely a gMSA nevét a dollár aláírása nélkül fogadja el.
<br/>

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)

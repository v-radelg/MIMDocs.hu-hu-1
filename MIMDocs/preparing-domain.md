---
title: Tartomány beállítása a Microsoft Identity Manager 2016 használatához | Microsoft Docs
description: Active Directory-tartományvezérlő létrehozása a MIM 2016 telepítése előtt
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 70ac40d38af06e6be98e00caf1dc73630b62334e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043477"
---
# <a name="set-up-a-domain"></a>Tartomány beállítása

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)

A Microsoft Identity Manager (MIM) az Ön Active Directory- (AD-) tartományával együtt is használható. Az AD-nek már telepítve kell lennie, és győződjön meg arról is, hogy a környezetében rendelkezik egy tartományvezérlővel egy felügyelhető tartományhoz.

Ez a cikk részletesen ismerteti a lépéseket, amelyekkel előkészítheti a tartományt a MIM-mel való működésre.

## <a name="create-user-accounts-and-groups"></a>Felhasználói fiókok és csoportok létrehozása

A MIM-telepítés minden összetevőjének saját identitással kell rendelkeznie a tartományban. Ez vonatkozik az olyan MIM-összetevőkre is, mint a Service, a Sync, a SharePoint és az SQL.

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **corpdc**
> - Tartománynév – **contoso**
> - **Corpservice** -kiszolgáló neve
> - **Corpsync** -szinkronizálási kiszolgáló neve
> - SQL Server neve – **corpsql**
> - Jelszó<strong>Pass@word1</strong>

1. Jelentkezzen be a tartományvezérlőbe tartományi rendszergazdaként (*pl.: Contoso\Administrator*).

2. Hozza létre a következő felhasználói fiókokat a MIM-szolgáltatásokhoz. Indítsa el a PowerShellt, és írja be a következő PowerShell-parancsprogramot a tartomány frissítéséhez.

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMINSTALL –name MIMINSTALL
    Set-ADAccountPassword –identity MIMINSTALL –NewPassword $sp
    Set-ADUser –identity MIMINSTALL –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMpool –name MIMpool
    Set-ADAccountPassword –identity MIMPool –NewPassword $sp
    Set-ADUser –identity MIMPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Hozzon létre biztonsági csoportokat az összes csoporthoz.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMInstall
    ```

4.  Állítson be egyszerű szolgáltatásneveket (SPN), amivel lehetővé válik a Kerberos hitelesítés használata a szolgáltatásfiókokhoz.

    ```CMD
    setspn -S http/mim.contoso.com Contoso\mimpool
    setspn -S http/mim Contoso\mimpool
    setspn -S http/passwordreset.contoso.com Contoso\mimsspr
    setspn -S http/passwordregistration.contoso.com Contoso\mimsspr
    setspn -S FIMService/mim.contoso.com Contoso\MIMService
    setspn -S FIMService/corpservice.contoso.com Contoso\MIMService
    ```
5.  A telepítés során hozzá kell adnia a következő DNS-rekordokat a megfelelő névfeloldáshoz:

- mim.contoso.com pont és corpservice fizikai IP-címe
- passwordreset.contoso.com pont és corpservice fizikai IP-címe
- passwordregistration.contoso.com pont és corpservice fizikai IP-címe

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)

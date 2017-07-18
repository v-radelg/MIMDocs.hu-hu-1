---
title: "Tartomány beállítása a Microsoft Identity Manager 2016 használatához | Microsoft Docs"
description: "Active Directory-tartományvezérlő létrehozása a MIM 2016 telepítése előtt"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: bd9c0da17c97cfc15023ad624a249e0f4a2d0825
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# Tartomány beállítása
<a id="set-up-a-domain" class="xliff"></a>

>[!div class="step-by-step"]
[Windows Server 2012 R2»](prepare-server-ws2012r2.md)

A Microsoft Identity Manager (MIM) az Ön Active Directory- (AD-) tartományával együtt is használható. Az AD-nek már telepítve kell lennie, és győződjön meg arról is, hogy a környezetében rendelkezik egy tartományvezérlővel egy felügyelhető tartományhoz.

Ez a cikk részletesen ismerteti a lépéseket, amelyekkel előkészítheti a tartományt a MIM-mel való működésre.

## Felhasználói fiókok és csoportok létrehozása
<a id="create-user-accounts-and-groups" class="xliff"></a>

A MIM-telepítés minden összetevőjének saját identitással kell rendelkeznie a tartományban. Ez vonatkozik az olyan MIM-összetevőkre is, mint a Service, a Sync, a SharePoint és az SQL.

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó – **Pass@word1**

1. Jelentkezzen be a tartományvezérlőbe tartományi rendszergazdaként (*pl.: Contoso\Administrator*).

2. Hozza létre a következő felhasználói fiókokat a MIM-szolgáltatásokhoz. Indítsa el a PowerShellt, és írja be a következő PowerShell-parancsprogramot a tartomány frissítéséhez.

    ```
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
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
    ```

3.  Hozzon létre biztonsági csoportokat az összes csoporthoz.

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

4.  Állítson be egyszerű szolgáltatásneveket (SPN), amivel lehetővé válik a Kerberos hitelesítés használata a szolgáltatásfiókokhoz.

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S FIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S FIMSynchronizationService/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]
[Windows Server 2012 R2»](prepare-server-ws2012r2.md)

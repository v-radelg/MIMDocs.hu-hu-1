---
title: "A PAM üzembe helyezése, 6. lépés – Csoport áthelyezése | Microsoft Identity Manager"
description: "Telepítse át a csoportot a PRIV erdőbe, hogy a Privilege Access Management által kezelhető legyen."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 603e5e28f0eee0f648ef7e00ef137f5a08b2ba34


---

# 6. lépés – Csoport áthelyezése a Privileged Access Management szolgáltatásba

>[!div class="step-by-step"]
[« 5. lépés ](step-5-establish-trust-between-priv-corp-forests.md)
[7. lépés »](step-7-elevate-user-access.md)

A rendszerjogosultságú fiókok a PRIV erdőben PowerShell-parancsmagokkal hozhatók létre. Ezek a parancsmagok a következő feladatokat végzik el:

- Új csoportot hoznak létre a PRIV erdőben a CORP erdőben található csoport biztonsági azonosítójával (SID) megegyező azonosítóval.  
- A MIM szolgáltatás adatbázisban létrehoznak egy objektumot a PRIV erdőben található csoporthoz kapcsolódóan.  
- Minden felhasználói fiókhoz létrehoznak két olyan objektumot a MIM szolgáltatás adatbázisban, amely kapcsolódik a CORP erdőben lévő felhasználóhoz, illetve a PRIV erdőben lévő új felhasználói fiókhoz.  
- Létrehoznak egy PAM-szerepkörobjektumot a MIM szolgáltatás adatbázisában.  

A parancsmagokat minden csoportban, illetve a csoport minden tagjánál egyszer kell lefuttatni. Az áttelepítési parancsmagok nem módosítanak semmilyen felhasználót vagy csoportot a CORP erdőben: a PAM-rendszergazda ezt később manuálisan hajtja végre.

1. Jelentkezzen be a PAMSRV-re közvetlenül vagy egy PRIV munkaállomásról *PRIV\MIMAdmin* felhasználóként.

2.  Indítsa el a PowerShellt, és írja be következő parancsokat.

    ```
    Import-Module MIMPAM
    Import-Module ActiveDirectory
    ```

3.  Hozzon létre egy meglévő erdőben megtalálható felhasználói fiókhoz kapcsolódó felhasználói fiókot a PRIV tartományban, bemutatási céllal.

    Írja be következő parancsokat a PowerShell-ablakba.  Ha nem az *Ilona* nevet használta korábban a contoso.local tartománybeli felhasználó létrehozásakor, akkor szükség szerint módosítsa a parancs paramétereit. A „Pass@word1” jelszó csak egy példa, egy egyedi jelszót kell ide behelyettesítenie.

    ```
    $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
    $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
    Set-ADUser –identity priv.Jen –Enabled 1
    ```

4. Másoljon egy csoportot és annak egy tagját,Ilonát, a CONTOSO tartományból a PRIV tartományba, bemutatási céllal.

    Futtassa az alábbi parancsokat, és adja meg a CORP tartományi rendszergazda (CONTOSO\Rendszergazda) jelszavát, ha a rendszer arra kéri:

        ```
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
        ```

    Összehasonlításul, a **New-PAMGroup** parancs paraméterei a következők:

        -   The CORP forest domain name in NetBIOS form  
        -   The name of the group to copy from that domain  
        -   The CORP forest Domain Controller NetBIOS name  
        -   The credentials of an domain admin user in the CORP forest  

5.  (Nem kötelező.) Törölje Ilona fiókját a **CONTOSO CorpAdmins** csoportból a CORPDC gépen, ha még szerepel benne.  Erre csak azért van szükség, mert ezen keresztül mutatjuk be, hogyan lehet engedélyeket társítani a PRIV erdőben létrehozott fiókokhoz.

    1.  Jelentkezzen be a CORPDC-re *CONTOSO\Rendszergazda* felhasználóként.

    2.  Indítsa el a PowerShellt, futtassa a következő parancsot, és erősítse meg a módosítást.

        ```
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


Ha azt szeretné bemutatni, hogy az erdők közötti hozzáférési jogok vannak érvényben a felhasználók rendszergazdai fiókjai esetében, folytassa a következő lépéssel.

>[!div class="step-by-step"]
[« 5. lépés ](step-5-establish-trust-between-priv-corp-forests.md)
[7. lépés »](step-7-elevate-user-access.md)



<!--HONumber=Jul16_HO3-->



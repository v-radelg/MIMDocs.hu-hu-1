---
title: "A PAM üzembe helyezése, 7. lépés – felhasználói hozzáférés | Microsoft Docs"
description: "Utolsó lépésként biztosítson ideiglenes rendszerjogosultságot egy felhasználónak, hogy tesztelhesse, sikeres volt-e a Privileged Access Management üzembe helyezése."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 89d9b38177b91f64e746fea583684abcecc9d7ff
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# 7. lépés – Felhasználó jogosultságszintjének emelése
<a id="step-7--elevate-a-users-access" class="xliff"></a>

>[!div class="step-by-step"]
[« 6. lépés](step-6-transition-group-to-pam.md)


Ebben a lépésben azt mutatjuk be, hogyan kérhet egy felhasználó hozzáférést egy szerepkörhöz a MIM-en keresztül.

## Győződjön meg róla, hogy Ilona nem tud hozzáférni a privilegizált erőforráshoz
<a id="verify-that-jen-cannot-access-the-privileged-resource" class="xliff"></a>
Emelt szintű jogosultságok nélkül Ilona nem férhet hozzá a CORP erdőben található privilegizált erőforrásokhoz.

1. Jelentkezzen ki a CORPWKSTN munkaállomásról, hogy megszüntessen minden gyorsítótárazott, nyitott kapcsolatot.
2. Jelentkezzen be a CORPWKSTN munkaállomásra a *CONTOSO\Ilona* felhasználónévvel, és váltson át **asztali** megjelenítésre.
3. Nyisson meg egy DOS parancssort.
4. Írja be a `dir \\corpwkstn\corpfs` parancsot. Meg kell jelennie **A hozzáférés megtagadva** hibaüzenetnek.
5. Hagyja nyitva a parancssor ablakát.

## Kérjen emelt szintű hozzáférést a MIM-ből.
<a id="request-privileged-access-from-mim" class="xliff"></a>
1. A CORPWKSTN munkaállomáson (még mindig CONTOSO\Ilona felhasználóként) írja be a következő parancsot.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Amikor a rendszer kéri, írja be a PRIV.Ilona fiók jelszavát. Megnyílik egy új parancssori ablak.
3. A PowerShell-ablak megjelenésekor írja be a következő parancsokat.

    > [!NOTE]
    > Miután futtatta ezeket a parancsokat, az alábbi lépések mindegyike időérzékennyé válik.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Ha ez befejeződött, zárja be a PowerShell ablakát.
5. A DOS parancsablakba írja be az alábbi parancsot.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Írja be a PRIV.Ilona fiók jelszavát. Megnyílik egy új parancssori ablak.

## Az emelt szintű hozzáférés ellenőrzése.
<a id="validate-the-elevated-access" class="xliff"></a>
Az újonnan megnyílt ablakba írja be az alábbi parancsokat.

```
whoami /groups
dir \\corpwkstn\corpfs
```

Ha a dir parancs végrehajtása nem sikerül, és megjelenik **A hozzáférés megtagadva** hibaüzenet, ellenőrizze újra a megbízhatósági kapcsolatot.

## Az emelt szintű szerepkör aktiválása
<a id="activate-the-privileged-role" class="xliff"></a>
Aktiválja a szerepkört a PAM-mintaportálon keresztüli emelt szintű hozzáférés-igényléssel.

1. A CORPWKSTN munkaállomáson győződjön meg arról, hogy a CORP\Ilona felhasználónéven van bejelentkezve.
2. Egy DOS parancsablakba írja be az alábbi parancsot.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Amikor a rendszer kéri, írja be a PRIV.Ilona fiók jelszavát. Megnyílik egy új böngészőablak.
4. Nyissa meg a http://pamsrv.priv.contoso.local:8090, és ellenőrizze, hogy látható-e a mintaportálról származó weblap.
5. Az Internet Explorerben válassza az **Eszközök** > **Internetbeállítások** elemet, és kattintson a **Biztonság** fülre.
6. Kattintson a **Helyi intranet zóna** > **Helyek** > **Speciális** elemre, majd adja hozzá a webhelyet a zónához.
7. Zárja be az **Internetbeállítások** párbeszédpanelt.
8. A bal oldali lapon kattintson az **Aktiválás** elemre. Válassza ki a **PAM-szerepkör** lehetőséget, majd kattintson az **Aktiválás** elemre.

> [!Note]
> Ebben a környezetben megismerkedhet a [Privileged Access Management REST API-referencia](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference) témakörben ismertetett PAM REST API-t használó alkalmazások fejlesztésével.

## Összefoglalás
<a id="summary" class="xliff"></a>
Az útmutató lépéseinek végrehajtását követően egy olyan Privileged Access Management-forgatókönyvet ismerhet meg, amelyben a felhasználók emelt szintű jogosultságai csak korlátozott ideig érvényesek, és a védett erőforrásokhoz egy elkülönített, emelt jogosultsági szintű fiókon keresztül férhetnek hozzá. Amint a jogosultságszint-emelés időtartama lejár, a rendszerjogosultságú fiók már nem tud hozzáférni a védett erőforrásokhoz. Annak eldöntése, hogy mely biztonsági csoportok kaphatnak kiemelt szerepköröket, a PAM-rendszergazda feladata. A hozzáférési jogoknak a Privileged Access Management-rendszerbe való áttelepítését követően a korábban az eredeti felhasználói fiókok számára biztosított hozzáférések csak akkor lesznek érvényesek, ha a felhasználó egy speciális, rendszerjogosultságú fiókkal bejelentkezik, és újra megkéri ezeket az engedélyeket. Ennek eredményeképpen a magas jogosultsági szintű csoportokhoz tartozó csoporttagságok csak korlátozott ideig használhatók eredményesen.

>[!div class="step-by-step"]
[« 6. lépés](step-6-transition-group-to-pam.md)


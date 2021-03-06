---
title: A PAM üzembe helyezése, 5. lépés – Erdőkapcsolat | Microsoft Docs
description: Megbízhatósági kapcsolat létrehozása a PRIV és CORP erdők között, hogy a PRIV rendszerjogosultságú felhasználói a CORP erőforrásaihoz is hozzáférjenek.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/29/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0cf952c93c0a7b95fd41939efc767e9e8c20be5e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043647"
---
# <a name="step-5--establish-trust-between-priv-and-corp-forests"></a>5. lépés – A CORP és a PRIV erdő közötti megbízhatósági kapcsolat létrehozása

> [!div class="step-by-step"]
> [«4](step-4-install-mim-components-on-pam-server.md)
> . lépés[6. lépés»](step-6-transition-group-to-pam.md)

Minden CORP tartománynál, így például a contoso.local tartomány esetében is, a PRIV és a CONTOSO tartományvezérlők között megbízhatósági kapcsolatnak kell fennállnia. Ez teszi lehetővé, hogy a PRIV tartomány felhasználói hozzáférhessenek a CORP tartományban lévő erőforrásokhoz.

## <a name="connect-each-domain-controller-to-its-counterpart"></a>Az egyes tartományvezérlők csatlakoztatása a párjukhoz

A megbízható kapcsolat kialakítása előtt minden tartományvezérlőn be kell állítani a párjuk DNS-nevének feloldását, a másik tartományvezérlő/DNS-kiszolgáló IP-címe alapján.

1.  Ha a tartományvezérlők vagy a MIM szoftvert tartalmazó kiszolgáló virtuális gépeken üzemelnek, ellenőrizze, hogy nem létezik-e más DNS-kiszolgáló, amely ezen számítógépek számára tartománynév-szolgáltatást biztosít.
    - Ha a virtuális gépek több hálózati adapterrel is rendelkeznek, a nyilvános hálózatokhoz csatlakozó hálózati adaptereket is beleértve, előfordulhat, hogy ideiglenesen le kell tiltania ezeket a kapcsolatokat, vagy felül kell írnia a Windows hálózatiadapter-beállításait. Fontos meggyőződnie arról, hogy egyik virtuális gép sem használ a DHCP által biztosított DNS-kiszolgálócímeket.

2.  Ellenőrizze, hogy minden meglévő CORP tartományvezérlő képes-e a neveket a PRIV erdőbe átirányítani. Minden PRIV erdőn kívüli tartományvezérlőn, például a CORPDC-n, indítsa el a PowerShellt, és írja be a következő parancsot:

    ```cmd
    nslookup -qt=ns priv.contoso.local.
    ```
    Győződjön meg róla, hogy a kimenet a PRIV tartományhoz tartozó, megfelelő IP-című névkiszolgáló-bejegyzést jelöli.

3.  Ha a tartományvezérlő nem tudja átirányítani a PRIV tartományt, a **Start** > **Alkalmazáseszközök** > **DNS** menüben található **DNS-kezelő** funkcióval konfigurálja, hogy a DNS átirányítsa a PRIV tartományt a PRIVDC IP-címére. Ha ez egy kiváló tartomány (például contoso. local), bontsa ki az ehhez a tartományvezérlőhöz és a tartományhoz tartozó csomópontokat, például a **CORPDC** > a**contoso. local****keresési zónákat** > , és győződjön meg arról, hogy a **priv** nevű kulcs neve kiszolgáló (NS) típusú.

    ![a priv kulcsot tartalmazó fájlstruktúra – képernyőkép](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>Megbízhatósági kapcsolat létrehozása a PAMSRV kiszolgálón

A PAMSRV kiszolgálón hozzon létre egy egyirányú megbízhatósági kapcsolatot minden tartományhoz, például a CORPDC-hez úgy, hogy a bizalom a CORP tartományvezérlők felől a PRIV erdő felé irányuljon.

1. Jelentkezzen be PRIV tartományi rendszergazdaként (PRIV\Rendszergazda) a PAMSRV kiszolgálóra.

2.  Indítsa el a PowerShellt.

3.  Írja be a következő PowerShell-parancsokat minden meglévő erdőben. Ha a rendszer kéri, adja meg a CORP tartományi rendszergazda (CONTOSO\Rendszergazda) hitelesítő adatait.

    ```PowerShell
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Írja be a következő PowerShell-parancsokat a meglévő erdő minden tartományában. Ha a rendszer kéri, adja meg a CORP tartományi rendszergazda (CONTOSO\Rendszergazda) hitelesítő adatait.

    ```PowerShell
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>Az erdők Active Directoryhoz való olvasási hozzáférésének megadása

A PRIV rendszergazdákon és a figyelőszolgáltatáson keresztül minden meglévő erdő számára olvasási hozzáférést biztosíthat az Active Directoryhoz.

1. Jelentkezzen be a meglévő CORP erdő tartományvezérlőjébe (CORPDC) az erdő legfelső szintű tartományának rendszergazdájaként (Contoso\Rendszergazda).  
2. Jelenítse meg az **Active Directory - felhasználók és számítógépek** ablakot.  
3. Kattintson a jobb gombbal a **contoso.local** tartományra, és válassza a **Vezérlés delegálása** parancsot.  
4. A Kijelölt felhasználók és csoportok lapon kattintson a **Hozzáadás** gombra.  
5. A Felhasználók, számítógépek vagy csoportok kiválasztása ablakban kattintson a **Helyek** elemre, és váltson át a *priv.contoso.local* helyre.  Az objektum nevéhez írja be a *Tartományi rendszergazdák* értéket, és kattintson a **Névellenőrzés** gombra. Amikor megjelenik egy előugró ablak, írja be a *priv\rendszergazda* felhasználónevet és a jelszavát.  
6. A Tartományi rendszergazdák név után írja be a „*; MIMMonitor*” értéket. Amikor megjelenik az aláhúzás a **Tartományi rendszergazdák** és a **MIMMonitor** név alatt, kattintson az **OK**, majd a **Tovább** gombra.  
7. A gyakori feladatok listáján jelölje ki **Az összes felhasználói információ olvasása** elemet, és kattintson a **Tovább**, majd a **Befejezés** gombra.  
8. Zárja be az Active Directory – felhasználók és számítógépek beépülő modult.

9. Indítson el egy PowerShell-ablakot.
10. Használja a `netdom` parancsot a SID-előzmények engedélyezéséhez, illetve a SID-szűrés letiltásához. Típus:
    ```cmd
    netdom trust contoso.local /quarantine:no /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    Vagy **A biztonsági azonosítók előzményeinek engedélyezése ebben a megbízhatósági kapcsolatban** vagy **A biztonsági azonosítók előzményei már engedélyezettek ebben a megbízhatósági kapcsolatban** kimenetet kell kapnia.

    A kimenetnek **A biztonsági azonosítók szűrése nem engedélyezett ebben a megbízhatósági kapcsolatban** szöveget is tartalmaznia kell. További információkért tekintse meg a [Disable SID filter quarantining](https://technet.microsoft.com/library/cc772816.aspx) (A SID-szűrők általi karanténba helyezés letiltása) című témakört.

## <a name="start-the-monitoring-and-component-services"></a>A figyelőszolgáltatás és a komponensszolgáltatás elindítása

1.  Jelentkezzen be PRIV tartományi rendszergazdaként (PRIV\Rendszergazda) a PAMSRV kiszolgálóra.

2.  Indítsa el a PowerShellt.

3.  Írja be az alábbi PowerShell-parancsokat.

    ```cmd
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

A következő lépésben a csoportoknak a PAM szolgáltatásba való áthelyezésével ismerkedhet meg.

> [!div class="step-by-step"]
> [«4](step-4-install-mim-components-on-pam-server.md)
> . lépés[6. lépés»](step-6-transition-group-to-pam.md)

---
title: A PAM üzembe helyezése, 5. lépés – Erdőkapcsolat | Microsoft Docs
description: Megbízhatósági kapcsolat létrehozása a PRIV és CORP erdők között, hogy a PRIV rendszerjogosultságú felhasználói a CORP erőforrásaihoz is hozzáférjenek.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 11/29/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: df4294ca6dbc98ec684e690d3ce66765d27cc359
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289091"
---
# <a name="step-5--establish-trust-between-priv-and-corp-forests"></a>5. lépés – A CORP és a PRIV erdő közötti megbízhatósági kapcsolat létrehozása

> [!div class="step-by-step"]
> [« 4. lépés](step-4-install-mim-components-on-pam-server.md)
> [6. lépés »](step-6-transition-group-to-pam.md)

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

3.  Ha a tartományvezérlő nem tudja átirányítani a PRIV tartományt, a **Start** > **Alkalmazáseszközök** > **DNS** menüben található **DNS-kezelő** funkcióval konfigurálja, hogy a DNS átirányítsa a PRIV tartományt a PRIVDC IP-címére. Ha ez egy felső szintű tartomány (mint a contoso.local), bontsa ki az ehhez a tartományvezérlőhöz és annak tartományához tartozó csomópontokat, például a **CORPDC** > **Címkeresési zónák** > **contoso.local** csomópontot, és ellenőrizze, hogy a **priv** nevű kulcs megtalálható-e a Névkiszolgáló (NS) típusok között.

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
2. Indítsa el az **Active Directory - felhasználók és számítógépek** beépülő modult.  
3. Kattintson a jobb gombbal a **contoso.local** tartományra, és válassza a **Vezérlés delegálása** parancsot.  
4. A Kijelölt felhasználók és csoportok lapon kattintson a **Hozzáadás** gombra.  
5. A Felhasználók, számítógépek vagy csoportok kiválasztása ablakban kattintson a **Helyek** elemre, és váltson át a *priv.contoso.local* helyre.  Az objektum nevéhez írja be a *Tartományi rendszergazdák* értéket, és kattintson a **Névellenőrzés** gombra. Amikor megjelenik egy előugró ablak, írja be a *priv\rendszergazda* felhasználónevet és a jelszavát.  
6. A Tartományi rendszergazdák név után írja be a „*; MIMMonitor*” értéket. Amikor megjelenik az aláhúzás a **Tartományi rendszergazdák** és a **MIMMonitor** név alatt, kattintson az **OK**, majd a **Tovább** gombra.  
7. A gyakori feladatok listáján jelölje ki **Az összes felhasználói információ olvasása** elemet, és kattintson a **Tovább**, majd a **Befejezés** gombra.  
8. Zárja be az Active Directory – felhasználók és számítógépek beépülő modult.

9. Indítson el egy PowerShell-ablakot.
10. Használja a `netdom` parancsot a SID-előzmények engedélyezéséhez, illetve a SID-szűrés letiltásához. Írja be ezt a parancsot:
    ```cmd
    netdom trust contoso.local /quarantine:no /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    Vagy **A biztonsági azonosítók előzményeinek engedélyezése ebben a megbízhatósági kapcsolatban** vagy **A biztonsági azonosítók előzményei már engedélyezettek ebben a megbízhatósági kapcsolatban** kimenetet kell kapnia.

    A kimenetnek **A biztonsági azonosítók szűrése nem engedélyezett ebben a megbízhatósági kapcsolatban** szöveget is tartalmaznia kell. További információkért tekintse meg a [Disable SID filter quarantining](http://technet.microsoft.com/library/cc772816.aspx) (A SID-szűrők általi karanténba helyezés letiltása) című témakört.

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
> [« 4. lépés](step-4-install-mim-components-on-pam-server.md)
> [6. lépés »](step-6-transition-group-to-pam.md)

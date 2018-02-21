---
title: "A hibrid jelentéskészítés az Azure-ban használva Identity Manager 2016 használata |} Microsoft Docs"
description: "Itt tájékozódhat arról, hogyan kombinálhatja a helyszíni és a felhőalapú adatokat hibrid jelentések formájában az Azure-ban, és hogyan kezelheti és jelenítheti meg ezeket a jelentéseket."
keywords: 
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: e135cc5066220765d97568b3a1e1b984a876b2a2
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/20/2018
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>A hibrid jelentéskészítés az Identity Manager használata

A cikkből megtudhatja, hogyan kombinálhatja a helyszíni és felhőalapú adatokat hibrid jelentések az Azure-ban formájában, és hogyan kezelheti és jelenítheti meg ezeket a jelentéseket.

## <a name="available-hybrid-reports"></a>Rendelkezésre álló hibrid jelentések
Az Azure Active Directory (Azure AD) elérhető első három Microsoft Identity Manager jelentéseket a következők:

- **Jelszó-visszaállítási tevékenység**: minden példány megjelenít, amikor a felhasználó önkiszolgáló jelszó-változtatási, önkiszolgáló jelszó-változtatási (SSPR) használatával, és a kapuk vagy a hitelesítéshez használt módszert biztosít.

- **Jelszó-átállítási regisztrációk**: minden alkalommal, amikor a felhasználó regisztrál az önkiszolgáló jelszó-Változtatási és a használt módszer jeleníti meg. Módszerek például a mobiltelefonszám vagy a kérdések és válaszok lehet.
   > [!NOTE]
   > A *jelszó-átállítási regisztrációk* jelenti a különbséget tenni az SMS és az MFA típusú átjárók között történik. Mindkét mobiltelefon módszerek minősülnek.

- **Önkiszolgáló csoportok tevékenység**: valaki hozzá vagy törölhet neki vagy saját magát egy csoportból, a csoport létrehozása minden kísérlet jeleníti meg.

    ![Kép: Azure hibrid jelentések – jelszó-átállítási tevékenységek](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * A jelentések jelenleg legfeljebb egy hónapra tevékenység megjeleníteni az adatokat.
> * Az előző hibrid jelentéskészítő ügynök el kell távolítani.
> * Hibrid jelentések eltávolításához távolítsa el a MIMreportingAgent.msi ügynököt.

## <a name="prerequisites"></a>Előfeltételek

* Identity Manager 2016 SP1 Identity Manager szolgáltatás, ajánlott build [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* A címtárban licencelt rendszergazda az Azure AD Premium bérlővel.

* Kimenő internetkapcsolattal az Identity Manager-kiszolgálón az Azure-bA.

## <a name="requirements"></a>Követelmények
Identity Manager hibrid jelentéskészítő használatának követelményei a következő táblázatban láthatók:

| Követelmény | Description |
| --- | --- |
| Prémium szintű Azure AD | A hibrid jelentéskészítés az Azure AD Premium szolgáltatás, és Azure AD Premium szükséges. </br>További információkért lásd: [Ismerkedés az Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Első egy [30 napos ingyenes próbaverzió az Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Az Azure ad globális rendszergazdának kell lennie |Alapértelmezés szerint csak a globális rendszergazdák is telepítse és konfigurálja az első lépések, a portál eléréséhez és műveletek hajtsanak végre Azure ügynökök. </br>**Fontos**: az ügynökök telepítésekor használt fióknak munkahelyi vagy iskolai fióknak kell lennie. Microsoft-fiók nem használható. További információkért lásd: [regisztráció az Azure-bA szervezetként](https://docs.microsoft.com/azure/active-directory/sign-up-organization). |
| Identity Manager hibrid ügynököt minden egyes megcélzott Identity Manager szolgáltatás kiszolgálóra van telepítve | Az adatok fogadására és figyelési és elemzési lehetőségeket nyújtson, a hibrid jelentéskészítés az ügynökök telepítésére és konfigurálására mindegyik célkiszolgálón van szükség.  </br>|
| Kimenő kapcsolat az Azure-szolgáltatásvégpontokhoz | A telepítés alatt és futásidőben az ügynöknek kapcsolódnia kell az Azure-szolgáltatásvégpontokhoz. Ha tűzfal blokkolja a kimenő kapcsolat, győződjön meg arról, hogy a következő végpontokat az engedélyezett bővítmények listájához kerülnek:<ul><li>\*.blob.core.windows.net </li><li>\*. servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li></ul> |
|Kimenő kapcsolódás IP-címek alapján | Az IP-címek alapján szűrést, ha a tűzfal, tekintse meg a [Azure IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).|
| SSL-ellenőrzést a kimenő forgalom szűrt vagy le van tiltva | Az ügynök regisztrációs lépés vagy adatok feltöltési művelet sikertelen lehet, ha SSL-ellenőrzést vagy a hálózati réteg a kimenő forgalom lezárását. |
| Az ügynököt futtató kiszolgáló tűzfalportjai | Kommunikálni az Azure szolgáltatásvégpontokra, az ügynök a következőt tűzfalportok megnyitását igényli:<ul><li>443-as TCP-port</li><li>5671-es TCP-port</li></ul> |
| Lehetővé teszi bizonyos webhelyek, ha az Internet Explorer fokozott biztonsági engedélyezve van |Ha az Internet Explorer fokozott biztonsági engedélyezve van, a következő webhelyeket engedélyezni kell, hogy az ügynök telepítve van a kiszolgálón:<ul><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Az összevonási kiszolgáló, a szervezet Azure Active Directoryban (például: https://sts.contoso.com) megbízhatónak.</li></ul> |
</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Az Azure AD Identity Manager jelentéskészítő ügynök telepítése
Jelentéskészítő ügynök a telepítést követően az Identity Manager-tevékenységek adatait exportálja az Identity Manager Windows eseménynaplóban történő. Identity Manager jelentéskészítő ügynök feldolgozza és eseményeket majd feltölti az Azure-bA. Az Azure elemzi, visszafejti és szűri az eseményeket a szükséges jelentésekhez.

1.  Telepítse az Identity Manager 2016.

2.  Töltse le az Identity Manager jelentéskészítő ügynököt, és tegye a következőket:

    a. Jelentkezzen be az Azure AD felügyeleti portálra, és válassza **Active Directory**.

    b. Kattintson duplán a könyvtár, amelynek globális rendszergazdája, és az Azure AD Premium előfizetéssel rendelkezik.

    c. Válassza ki **konfigurációs**, majd töltse le a jelentéskészítő ügynök.

3.  Telepítse a jelentéskészítő ügynök a következő módon:

    a.  Töltse le a [MIMHReportingAgentSetup.exe fájl](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) az Identity Manager szolgáltatás kiszolgáló.

    b.  Futtassa a `MIMHReportingAgentSetup.exe` parancsot. 

    c.  Futtassa az ügynök telepítőjét.

    d.  Győződjön meg arról, hogy az Identity Manager jelentéskészítő ügynök szolgáltatás fut-e.

    e.  Indítsa újra az Identity Manager szolgáltatást.

4.  Győződjön meg arról, hogy az Identity Manager jelentéskészítő ügynököt az Azure-ban működik.

    A jelentés adatokat az Identity Manager önkiszolgáló jelszó-visszaállítási portál a jelszó alaphelyzetbe állítása hozhat létre. Győződjön meg arról, hogy a jelszóátállítás sikeresen befejeződött-e, és ellenőrizze, hogy győződjön meg arról, hogy az adatokat az Azure AD felügyeleti portál megjeleníti.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Az Azure-portálon hibrid jelentések megtekintése

1.  Jelentkezzen be a [Azure-portálon](https://portal.azure.com/) a bérlő számára a globális rendszergazdai fiókkal.

2.  Válassza ki **az Azure Active Directory**.

3.  Válassza ki a bérlő címtárát az előfizetéssel elérhető címtárak listájában.

4.  Válassza ki **naplók**.

5.  Az a **kategória** legördülő listában, ellenőrizze, hogy **MIM szolgáltatás** van kiválasztva.

> [!IMPORTANT]
> Némi időbe telhet az Identity Manager naplózási adatai megjelennek az Azure-portálon.

## <a name="stop-creating-hybrid-reports"></a>A hibrid jelentések létrehozásának leállítása
Ha azt szeretné, hogy jelentéskészítési naplózási több adatot feltölteni a Identity Managerből az Azure AD, távolítsa el a hibrid jelentéskészítő ügynök. A Windows programok telepítése és törlése eszköz segítségével távolítsa el az Identity Manager hibrid jelentéskészítő.

## <a name="windows-events-used-for-hybrid-reporting"></a>A hibrid jelentéskészítéshez használt Windows-események
Identity Manager által generált eseményeket a Windows eseménynaplójában találhatók. Az események a **Eseménynapló** kiválasztásával **alkalmazás- és szolgáltatásnaplók** > **Identity Manager Request Log**. A rendszer minden Identity Manager kérelmet a JSON struktúrában a Windows Eseménynapló eseményként exportál. Exportálhatja az eredmény a biztonsági információk és az esemény (SIEM) felügyeleti rendszerbe.

|Eseménytípus|Azonosító|Esemény részletei|
|--------------|------|-----------------|
|Adatok|4121|Az Identity Manager esemény-adatait, amely tartalmazza az összes kérelem adatai.|
|Információ|4137|Az Identity Manager 4121-esemény bővítménye, ha egy adott eseményhez túl sok adat esetén. Ez az esemény fejlécének jelenik meg a következő formátumban: `"Request: <GUID> , message <xxx> out of <xxx>`.|

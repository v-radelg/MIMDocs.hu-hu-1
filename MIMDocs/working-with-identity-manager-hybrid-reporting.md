---
title: Hibrid jelentéskészítés az Azure Identity Manager 2016 használatával dolgozhat |} A Microsoft Docs
description: Itt tájékozódhat arról, hogyan kombinálhatja a helyszíni és a felhőalapú adatokat hibrid jelentések formájában az Azure-ban, és hogyan kezelheti és jelenítheti meg ezeket a jelentéseket.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 18e4127b1d854a53734142bb58442627619491ef
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358533"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Hibrid jelentéskészítés az Identity Manager használata

Ez a cikk ismerteti, hogyan kombinálhatja a helyszíni és felhőalapú adatokat hibrid jelentések az Azure-ban, és hogyan kezelheti, és ezek a jelentések megtekintéséhez.

## <a name="available-hybrid-reports"></a>Rendelkezésre álló hibrid jelentések
Az Azure Active Directoryban (Azure AD) az elérhető első három Microsoft Identity Manager jelentések a következők:

- **Jelszó-visszaállítási tevékenység**: Megjeleníti, amikor egy felhasználó hajtott végre a jelszó-visszaállítás használatával az önkiszolgáló jelszó-visszaállítás (SSPR) és a kapuk vagy a hitelesítéshez használt módszert biztosít.

- **Jelszó-átállítási regisztrációk**: minden alkalommal, amikor egy felhasználó regisztrál az SSPR, és a hitelesítéshez használt módszerek jeleníti meg. Példák módszerekkel lehet mobiltelefonszám vagy kérdéseket és válaszokat.
   > [!NOTE]
   > A *jelszó-átállítási regisztrációk* jelentések, sem a mobiltelefonos SMS és az MFA típusú átjárók között. Mobiltelefon módszereket is számítanak.

- **Önkiszolgáló csoportok tevékenysége**: valaki hozzáadása vagy törlése az őt vagy saját magát egy csoportból, csoport létrehozása az összes olyan kísérletet megjeleníti.

    ![Kép: Azure hibrid jelentések – jelszó-átállítási tevékenységek](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * A jelentések jelenleg tárolt adatok legfeljebb egy hónapig tevékenység.
> * A korábbi hibrid jelentéskészítő ügynököt el kell távolítani.
> * Hibrid jelentések eltávolításához távolítsa el a MIMreportingAgent.msi ügynököt.

## <a name="prerequisites"></a>Előfeltételek

* Identity Manager 2016 SP1 Identity Manager szolgáltatás, ajánlott build [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* A címtár licencelt rendszergazda az Azure AD Premium bérlővel.

* Kimenő internetkapcsolattal az Identity Manager server az Azure-bA.

## <a name="requirements"></a>Követelmények
Az alábbi táblázatban felsorolt Identity Manager hibrid jelentéskészítő használatának követelményei:


|                                         Követelmény                                         |                                                                                                                                                                                                                                                                                    Leírás                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Prémium szintű Azure AD                                       |                                                                                                        A hibrid jelentéskészítés egy Azure AD Premium-funkció, és a prémium szintű Azure AD szükséges. </br>További információkért lásd: [Ismerkedés az Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Get- [30 napos ingyenes próbaverzió az Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Az Azure AD globális rendszergazdájának kell lennie                     |                                                                   Alapértelmezés szerint csak a globális rendszergazdák telepítheti és az első lépések, hozzáférhet a portálhoz, és műveleteket hajtsanak végre az Azure-ügynökök konfigurálása. </br>**Fontos**: az ügynökök telepítésekor használt fióknak munkahelyi vagy iskolai fiókkal kell lennie. Microsoft-fiók nem használható. További információkért lásd: [Azure-előfizetésre regisztrál](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Identity Manager hibrid ügynökének összes célkiszolgálóra Identity Manager szolgáltatás telepítve van |                                                                                                                                                                                                       Az adatok fogadásához, és monitorozási és elemzési képességeket biztosítanak, a hibrid jelentéskészítéshez az ügynököket kell telepítenie és konfigurálnia a célkiszolgálókon.  </br>                                                                                                                                                                                                       |
|                    Kimenő kapcsolat az Azure-szolgáltatásvégpontokhoz                     | A telepítés alatt és futásidőben az ügynöknek kapcsolódnia kell az Azure-szolgáltatásvégpontokhoz. Ha tűzfalakkal blokkolta a kimenő kapcsolatot, győződjön meg arról, hogy az alábbi végpontok fel vannak véve az engedélyezett listára:<ul><li>\*.blob.core.windows.net </li><li>\*. servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         IP-címeken alapuló kimenő kapcsolatok                         |                                                                                                                                                                                                                      Az IP-címek alapú tűzfalas szűrésről, tekintse meg a [Azure IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 Kimenő forgalom SSL-vizsgálata le van tiltva vagy szűrve                 |                                                                                                                                                                                                               Az ügynök regisztrációs lépés vagy adatfeltöltési műveletei meghiúsulhat, ha nincs SSL-ellenőrzés vagy -lezárást biztosít a hálózati réteg a kimenő forgalom.                                                                                                                                                                                                                |
|                      Az ügynököt futtató kiszolgáló tűzfalportjai                       |                                                                                                                                                                                                          Az Azure-szolgáltatásvégpontokhoz kommunikálni, az ügynöknek kapcsolódnia kell a következőt tűzfalportok nyitva lenniük:<ul><li>443-as TCP-port</li><li>5671-es TCP-port</li></ul>                                                                                                                                                                                                          |
|          Lehetővé teszi bizonyos webhelyekhez, ha az Internet Explorer fokozott biztonsági engedélyezve van           |                                                                                Ha az Internet Explorer fokozott biztonsági beállításai engedélyezve vannak, az alábbi webhelyeket engedélyezni kell a kiszolgálón, amelyen az ügynök telepítve van:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>Az összevonási kiszolgáló, a szervezet Azure Active Directory által megbízhatónak tartott (például <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Az Azure AD Identity Manager jelentéskészítő ügynök telepítése
Jelentéskészítő ügynök a telepítést követően az Identity Manager tevékenységből exportálás Identity Manager Windows eseménynaplóba való. Identity Manager jelentéskészítő ügynök dolgozza fel a, és ezután feltölti azokat az Azure-bA. Az Azure elemzi, visszafejti és szűri az eseményeket a szükséges jelentésekhez.

1.  Telepítse az Identity Manager 2016-ra.

2.  Identity Manager jelentéskészítő ügynök letöltése, és tegye a következőket:

    a. Jelentkezzen be az Azure AD felügyeleti portálra, és válassza **Active Directory**.

    b. Kattintson duplán a könyvtár, amelynek globális rendszergazdája és az Azure AD Premium előfizetéssel rendelkezik.

    c. Válassza ki **konfigurációs**, majd töltse le a jelentéskészítő ügynök.

3.  Jelentéskészítő ügynök telepítése az alábbiak szerint:

    a.  Töltse le a [MIMHReportingAgentSetup.exe fájl](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) az Identity Manager szolgáltatás kiszolgáló.

    b.  Futtassa a `MIMHReportingAgentSetup.exe` parancsot. 

    c.  Futtassa az ügynök telepítőjét.

    d.  Győződjön meg arról, hogy az Identity Manager jelentéskészítő ügynök szolgáltatás fut-e.

    e.  Indítsa újra az Identity Manager szolgáltatást.

4.  Identity Manager jelentéskészítő ügynök működésének ellenőrzése az Azure-ban.

    Adatok alapján az Identity Manager önkiszolgáló jelszó-visszaállítási portál a jelszó alaphelyzetbe állítása jelentést hozhat létre. Győződjön meg arról, hogy a jelszó alaphelyzetbe állítása sikeresen befejeződött-e, és ellenőrizze, hogy győződjön meg arról, hogy az adatok megjelennek-e az Azure AD felügyeleti portálra.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Hibrid jelentések megtekintése az Azure Portalon

1.  Jelentkezzen be a [az Azure portal](https://portal.azure.com/) a bérlőhöz tartozó globális rendszergazdai fiókjával.

2.  Válassza ki **az Azure Active Directory**.

3.  Az előfizetéséhez elérhető címtárak listájában válassza ki a bérlő címtárát.

4.  Válassza ki **Auditnaplók**.

5.  Az a **kategória** legördülő listában, ellenőrizze, hogy **MIM szolgáltatás** van kiválasztva.

> [!IMPORTANT]
> Némi időbe telhet az Identity Manager naplóadatai megjelennek az Azure Portalon.

## <a name="stop-creating-hybrid-reports"></a>A hibrid jelentések létrehozásának leállítása
Ha azt szeretné, hogy jelentéskészítési naplózási adatot feltölteni a Identity Managerből az Azure ad-ben, távolítsa el a hibrid jelentéskészítő ügynököt. A Windows programok telepítése és törlése eszköz segítségével távolítsa el az Identity Manager hibrid jelentéskészítő.

## <a name="windows-events-used-for-hybrid-reporting"></a>A hibrid jelentéskészítéshez használt Windows-események
Identity Manager által előállított eseményeket a Windows eseménynaplójában találhatók. Az eseményeket is megtekintheti a **Eseménynapló** kiválasztásával **alkalmazás- és szolgáltatásnaplók** > **Identity Manager Request Log**. Egy esemény Windows eseménynaplóban, a JSON struktúrában Identity Manager kéréseknek exportálja. Az eredmény a biztonságiadat- és eseménykezelés (SIEM) Információbiztonsági rendszer exportálhatja.

|Eseménytípus|Azonosító|Esemény részletei|
|--------------|------|-----------------|
|Adatok|4121|Az Identity Manager eseményadatokat, amely minden kérés adatait tartalmazza.|
|Információ|4137|Az Identity Manager esemény 4121 bővítmény, ha egy adott eseményhez túl sok adatot. Ez az esemény fejlécének formátuma a következő jelenik meg: `"Request: <GUID> , message <xxx> out of <xxx>`.|

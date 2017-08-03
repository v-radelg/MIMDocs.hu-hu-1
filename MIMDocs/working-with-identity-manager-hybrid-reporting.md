---
title: "A hibrid jelentéskészítés az Azure-ban szolgáltatás használata a MIM 2016-tal | Microsoft Docs"
description: "Itt tájékozódhat arról, hogyan kombinálhatja a helyszíni és a felhőalapú adatokat hibrid jelentések formájában az Azure-ban, és hogyan kezelheti és jelenítheti meg ezeket a jelentéseket."
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: df842309034ad68151dd8cc4151507e7ece6626d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>A hibrid jelentéskészítés kezelése az Identity Managerben – nyilvános előzetes verzió (frissítés)

## <a name="available-hybrid-reports"></a>Rendelkezésre álló hibrid jelentések
Az Azure AD-ben elérhető első három Microsoft Identity Manager- (MIM-) jelentés a **Password reset activity** (Jelszó-átállítási tevékenység), a **Password reset registration** (Jelszó-átállítás regisztrációja) és a **Self-service groups activity** (Önkiszolgáló csoportok tevékenysége).

-   A jelszó-átállítási tevékenységről szóló jelentés az összes olyan esetet megjeleníti, amelyben egy felhasználó az önkiszolgáló jelszó-változtatási portállal (SSPR) megváltoztatta a jelszavát, és a hitelesítéshez használt átjárókat vagy **módszereket** is feltünteti.

-   A jelszó-átállítási regisztrációk jelentése az összes olyan esetet megjeleníti, amikor a felhasználó regisztrál az önkiszolgáló jelszó-változtatási portálra (SSPR), és feltünteti a hitelesítéshez használt **módszereket** is – ilyen lehet például a mobiltelefonszám vagy a kérdések-válaszok.
    Vegye figyelembe, hogy a jelszóátállítási regisztrációk jelentése nem tesz különbséget az SMS és az MFA típusú átjárók között – mindkettő **Mobile Phone** (Mobiltelefonos) típusú átjárónak minősül.

-   Az önkiszolgáló csoportok tevékenységéről szóló jelentés az összes olyan kísérletet megjeleníti, melynek során valaki megpróbálta felvenni magát egy csoportba, törölni magát egy csoportból, vagy csoportot próbált létrehozni.

    ![Kép: Azure hibrid jelentések – jelszó-átállítási tevékenységek](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> A jelentések jelenleg legfeljebb egy hónapra visszamenőleg képesek megjeleníteni az adatokat.</br>
> A korábbi hibrid ügynököt el kell távolítani</br>
> Ha el szeretné távolítani a hibrid jelentéseket, távolítsa el a MIMreportingAgent.msi ügynököt.

## <a name="prerequisites"></a>Előfeltételek

1.  Telepítse a Microsoft Identity Manager 2016 RTM-et vagy a MIM szolgáltatás SP1-et.

2.  Győződjön meg arról, hogy Azure AD Premium bérlővel rendelkezik, és a címtárban szerepel licencelt rendszergazda.

3.  Ellenőrizze, hogy a Microsoft Identity Manager rendelkezik-e kimenő internetkapcsolattal az Azure felé.

## <a name="requirements"></a>Követelmények
Az alábbi táblázatban látható lista a hibrid jelentéskészítés a Microsoft Identity Managerben való használatának követelményeit sorolja fel.

| Követelmény | Leírás |
| --- | --- |
| Prémium szintű Azure AD | A Hibrid jelentéskészítés a Prémium szintű Azure AD funkciója, amelyhez Prémium szintű Azure AD szükséges. </br></br>További információt a [Bevezetés a Prémium szintű Azure Active Directory használatába](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium) című témakörben találhat. </br>A 30 napos ingyenes próbaidőszakot a [Próbaverzió aktiválása](https://azure.microsoft.com/trial/get-started-active-directory/) című témakörben kezdheti meg. |
| A használat megkezdéséhez az Azure AD globális rendszergazdájának kell lennie |Alapértelmezés szerint csak a globális rendszergazdák telepíthetik és konfigurálhatják az ügynököket a használat megkezdéséhez, érhetik el a portált és hajthatnak végre műveleteket az Azure-ban. </br></br>**Fontos:** Az ügynökök telepítéséhez csak munkahelyi vagy iskolai fiók használható. Microsoft-fiók nem használható. További információt a [Sign up for Azure as an organization](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization) (Regisztráció az Azure-ra szervezetként) című témakörben találhat. |
| A Microsoft Identity Manager hibrid ügynökének telepítése a MIM szolgáltatás minden megcélzott kiszolgálóján | A hibrid jelentéskészítéshez telepíteni és konfigurálni kell az ügynököket a megcélzott kiszolgálókon, hogy fogadják az adatokat és biztosítsák a figyelési és elemzési képességeket </br>|
| Kimenő kapcsolat az Azure-szolgáltatásvégpontokhoz | A telepítés alatt és futásidőben az ügynöknek kapcsolódnia kell az Azure-szolgáltatásvégpontokhoz. Ha a kimenő kapcsolatot tűzfal tiltja le, győződjön meg arról, hogy a következő végpontok hozzá legyenek adva az engedélyezési listához: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Kimenő kapcsolat IP-címek alapján | A tűzfalak IP-címalapú szűréséhez lásd: [Azure IP Ranges](https://www.microsoft.com/en-us/download/details.aspx?id=41653) (Azure IP-címtartományok).|
| A kimenő forgalom SSL-ellenőrzésének szűrése vagy letiltása | Ha a hálózati rétegben a kimenő forgalomra SSL-ellenőrzés vagy -lezárás vonatkozik, sikertelen lehet az ügynökregisztráció vagy az adatfeltöltés. |
| Tűzfalportok az ügynököt futtató kiszolgálón. |A következő tűzfalportoknak kell nyitva lenniük ahhoz, hogy az ügynök kommunikálni tudjon az Azure-szolgáltatásvégpontokkal:</br></br><li>443-as TCP-port</li><li>5671-es TCP-port</li> |
| Az alábbi webhelyek engedélyezése, ha az Internet Explorer fokozott biztonsági beállításai engedélyezve vannak |Ha az Internet Explorer fokozott biztonsági beállításai engedélyezve vannak, az alábbi webhelyeket kell engedélyeznie azon a kiszolgálón, amelyen az ügynököt telepíteni fogja.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>A szervezetnek rendelkeznie kell egy olyan összevonási kiszolgálóval, melyet az Azure Active Directory megbízhatónak tekint. Példa: https://sts.contoso.com</li> |
</BR>

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>A Microsoft Identity Manager jelentéskészítő ügynök telepítése az Azure AD-ben
A jelentéskészítő ügynök a telepítést követően a Microsoft Identity Manager-tevékenységek adatait a MIM-ből a Windows Eseménynaplóba exportálja. A MIM jelentéskészítő ügynök feldolgozza és feltölti az eseményeket az Azure-ba. Az Azure elemzi, visszafejti és szűri az eseményeket a szükséges jelentésekhez.

1.  Telepítse a Microsoft Identity Manager 2016-ot.

2.  Töltse le a Microsoft Identity Manager jelentéskészítő ügynökeit:

    1.  Jelentkezzen be az Azure AD felügyeleti portálra, majd kattintson az Active Directory ikonra.

    2.  Kattintson duplán arra a címtárra, amelynek globális rendszergazdája, és amelyhez Azure AD Premium előfizetéssel rendelkezik.

    3.  Kattintson a **Konfiguráció** elemre, és töltse le a jelentéskészítő ügynököt.

3.  Telepítse a Microsoft Identity Manager jelentéskészítő ügynököt:

    1.  Töltse le a [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) fájlt a Microsoft Identity Manager Service-kiszolgálóra.
    2.  Futtassa a következőt: `MIMHReportingAgentSetup.exe` 
    3.  Futtassa az ügynök telepítőjét.

    4.  Ha a MIM jelentéskészítő ügynök szolgáltatása nem fut, indítsa el.

    5.  Indítsa újra a MIM szolgáltatást.

4.  Győződjön meg arról, hogy a Microsoft Identity Manager Reporting működik az Azure-ban.

    Jelentési adatok létrehozásához használja a Microsoft Identity Manager önkiszolgáló jelszó-változtatási portálját, és állítsa át egy felhasználó jelszavát. Győződjön meg arról, hogy a jelszóátállítás sikeresen befejeződött, majd ellenőrizze, hogy az Azure AD felügyeleti portál megjeleníti-e az adatokat.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Hibrid jelentések megtekintése az Azure Portal webhelyen

1.  Jelentkezzen be az [Azure Portalra](https://portal.azure.com/) a bérlőhöz tartozó globális rendszergazdai fiókkal.

2.  Kattintson az **Azure Active Directory** ikonra.

3.  Jelölje ki a bérlő címtárát az előfizetéssel elérhető címtárak listájából.

4.  Kattintson a **Naplók** elemre.

5.  A Kategória legördülő listában válassza a **MIM szolgáltatás** elemet.

> [!WARNING]
> Némi időbe telhet, amíg a Microsoft Identity Manager naplózási adatai megjelennek az Azure Portal webhelyen.

## <a name="stop-creating-hybrid-reports"></a>A hibrid jelentések létrehozásának leállítása
Ha nem szeretne több naplózási adatot feltölteni a Microsoft Identity Managerből az Azure Active Directoryba, távolítsa el a hibrid jelentéskészítő ügynököt. A Microsoft Identity Manager hibrid jelentéskészítő összetevőjét a Windows **Programok telepítése és törlése** eszközével távolíthatja el.

## <a name="windows-events-used-for-hybrid-reporting"></a>A hibrid jelentéskészítéshez használt Windows-események
A Microsoft Identity Manager által generált eseményeket a Windows Eseménynapló naplózza, és az Alkalmazás- és szolgáltatásnaplók – &gt; **Identity Manager Request Log** (Identity Manager-kérelemnapló) csomópont alatt érhetők el. A rendszer minden MIM-kérelmet eseményként exportál a Windows Eseménynaplóba, JSON struktúrában. Ezeket a saját SIEM-rendszerébe exportálhatja.

|Eseménytípus|Azonosító|Esemény részletei|
|--------------|------|-----------------|
|Adatok|4121|Az összes kérelemadatot tartalmazó MIM-eseményadatok.|
|Információ|4137|A 4121-es számú MIM-esemény bővítménye arra az esetre, ha egy adott eseményhez túl sok adat tartozik. Az esemény fejlécének formátuma: `"Request: <GUID> , message <xxx> out of <xxx>`|

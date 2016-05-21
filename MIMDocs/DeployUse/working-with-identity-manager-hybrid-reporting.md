---
# required metadata

title: A hibrid jelentéskészítés kezelése az Identity Managerben | Microsoft Identity Manager
description: Itt tájékozódhat arról, hogyan kombinálhatja a helyszíni és a felhőalapú adatokat hibrid jelentések formájában az Azure-ban, és hogyan kezelheti és jelenítheti meg ezeket a jelentéseket.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# A hibrid jelentéskészítés kezelése az Identity Managerben

## Rendelkezésre álló hibrid jelentések
Az Azure AD-ben elérhető első három Microsoft Identity Manager- (MIM-) jelentés a **Password reset activity** (Jelszó-átállítási tevékenység), a **Password reset registration** (Jelszó-átállítás regisztrációja) és a **Self-service groups activity** (Önkiszolgáló csoportok tevékenysége)..

-   A jelszó-átállítási tevékenységről szóló jelentés az összes olyan esetet megjeleníti, amelyben egy felhasználó az önkiszolgáló jelszó-változtatási portállal (SSPR) megváltoztatta a jelszavát, és a hitelesítéshez használt átjárókat vagy **módszereket** is feltünteti.

    ![Kép: Azure hibrid jelentések – jelszó-átállítási tevékenységek](media/MIM-Hybrid-passwordreset.jpg)

-   A jelszó-átállítási regisztrációk jelentése az összes olyan esetet megjeleníti, amikor a felhasználó regisztrál az önkiszolgáló jelszó-változtatási portálra (SSPR), és feltünteti a hitelesítéshez használt **módszereket** is – ilyen lehet például a mobiltelefonszám vagy a kérdések-válaszok.
    Vegye figyelembe, hogy a jelszó-átállítási regisztrációk jelentése nem tesz különbséget az SMS és az MFA típusú átjárók között – mindkettő **Mobile Phone** (Mobiltelefonos) típusú átjárónak minősül..

-   Az önkiszolgáló csoportok tevékenységéről szóló jelentés az összes olyan kísérletet megjeleníti, melynek során valaki megpróbálta felvenni magát egy csoportba, törölni magát egy csoportból, vagy csoportot próbált létrehozni.

> [!NOTE]
> A jelentések jelenleg legfeljebb egy hónapra visszamenőleg képesek megjeleníteni az adatokat.
>
> Ha el szeretné távolítani a hibrid jelentéseket, távolítsa el a MIMreportingAgent.msi ügynököt.

## Előfeltételek

1.  Telepítse a Microsoft Identity Manager 2016-ot a MIM szolgáltatással együtt.

2.  Győződjön meg arról, hogy Azure AD Premium bérlővel rendelkezik, és a címtárban szerepel licencelt rendszergazda.

3.  Ellenőrizze, hogy a Microsoft Identity Manager rendelkezik-e kimenő internetkapcsolattal az Azure felé.

## A Microsoft Identity Manager jelentéskészítő funkcióinak telepítése az Azure AD-ben
A jelentéskészítő ügynök a telepítést követően a Microsoft Identity Manager-tevékenységek adatait a MIM-ből a Windows Eseménynaplóba exportálja. A MIM jelentéskészítő ügynök feldolgozza és feltölti az eseményeket az Azure-ba. Az Azure elemzi, visszafejti és szűri az eseményeket a szükséges jelentésekhez.

1.  Telepítse a Microsoft Identity Manager 2016-ot.

2.  Töltse le a Microsoft Identity Manager jelentéskészítő ügynökeit:

    1.  Jelentkezzen be az Azure AD felügyeleti portálra, majd kattintson az Active Directory ikonra.

    2.  Kattintson duplán arra a címtárra, amelynek globális rendszergazdája, és amelyhez Azure AD Premium előfizetéssel rendelkezik.

    3.  Kattintson a **Konfiguráció** elemre, és töltse le a jelentéskészítő ügynököt.

3.  Telepítse a Microsoft Identity Manager jelentéskészítő ügynököt:

    1.  Hozzon létre egy könyvtárat a számítógépen.

    2.  Bontsa ki a `MIMHybridReportingAgent.msi` és a `tenant.cert` fájlokat a könyvtárba.

    3.  Futtassa az ügynök telepítőjét.

    4.  Ha a MIM jelentéskészítő ügynök szolgáltatása nem fut, indítsa el.

    5.  Indítsa újra a MIM szolgáltatást.

4.  Győződjön meg arról, hogy a Microsoft Identity Manager Reporting működik az Azure-ban.

    Jelentési adatok létrehozásához használja a Microsoft Identity Manager önkiszolgáló jelszó-változtatási portálját, és állítsa át egy felhasználó jelszavát. Győződjön meg arról, hogy a jelszóátállítás sikeresen befejeződött, majd ellenőrizze, hogy az Azure AD felügyeleti portál megjeleníti-e az adatokat.

## Hibrid jelentések megtekintése a klasszikus Azure-portálon

1.  Jelentkezzen be a [klasszikus Azure-portálra](https://manage.windowsazure.com/) a bérlőhöz tartozó globális rendszergazdai fiókkal.

2.  Kattintson az **Active Directory** ikonra.

3.  Jelölje ki a bérlő címtárát az előfizetéssel elérhető címtárak listájából.

4.  Kattintson a **Reports** (Jelentések), majd a **Password Reset Activity** (Jelszó-átállítási tevékenység) lehetőségre..

5.  A források legördülő menüjéből válassza az **Identity Manager** elemet.

> [!WARNING]
> Némi időbe telhet, amíg a Microsoft Identity Manager adatai megjelennek az Azure AD-ben.

## A hibrid jelentések létrehozásának leállítása
Ha nem szeretne több jelentési adatot feltölteni a Microsoft Identity Managerből az Azure Active Directoryba, távolítsa el a hibrid jelentéskészítő ügynököt. A Microsoft Identity Manager hibrid jelentéskészítő összetevőjét a Windows **Programok telepítése és törlése** eszközével távolíthatja el.

## A hibrid jelentéskészítéshez használt Windows-események
A Microsoft Identity Manager által generált eseményeket a Windows Eseménynapló naplózza, és az Alkalmazás- és szolgáltatásnaplók – &gt; **Identity Manager Request Log** (Identity Manager-kérelemnapló) csomópont alatt érhetők el. A rendszer minden MIM-kérelmet eseményként exportál a Windows Eseménynaplóba, JSON struktúrában. Ezeket a saját SIEM-rendszerébe exportálhatja.

|Eseménytípus|Azonosító|Esemény részletei|
|--------------|------|-----------------|
|Információ|4121|Az összes kérelemadatot tartalmazó MIM-eseményadatok.|
|Információ|4137|A 4121-es számú MIM-esemény bővítménye arra az esetre, ha egy adott eseményhez túl sok adat tartozik. Az esemény fejlécének formátuma: `"Request: <GUID> , message <xxx> out of <xxx>`|


<!--HONumber=Apr16_HO4-->



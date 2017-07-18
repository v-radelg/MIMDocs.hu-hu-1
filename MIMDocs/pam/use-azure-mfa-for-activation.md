---
title: "Az Azure MFA használata a PAM aktiválásához | Microsoft Docs"
description: "Állítsa be az Azure MFA-t második biztonsági szintként, ha a felhasználók szerepköröket aktiválnak a Privileged Access Managementben."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b937b30da2dff9bbfeabf7dceb43fcaca99a1b63
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# Aktiválás az Azure MFA használatával
<a id="using-azure-mfa-for-activation" class="xliff"></a>
A PAM-szerepkörök konfigurálásakor kiválaszthatja, hogyan szeretné engedélyekkel felruházni azokat a felhasználókat, akik a szerepkör aktiválását kérik. A PAM-engedélyezés a következő választási lehetőségeket nyújtja:

- Szerepkör tulajdonosának jóváhagyása
- Azure Multi-Factor Authentication (MFA)

Ha egyik ellenőrzési mód sincs engedélyezve, a jelölt felhasználók szerepköre automatikusan aktiválódik.

A Microsoft Azure Multi-Factor Authentication (MFA) olyan hitelesítési szolgáltatás, amely a bejelentkezési kísérletek mobilalkalmazással, telefonhívással vagy SMS-sel történő megerősítését kéri a felhasználóktól. A szolgáltatás a Microsoft Azure Active Directoryval, valamint felhőalapú és helyszíni nagyvállalati alkalmazásokkal is használható. PAM alkalmazása esetén az Azure MFA olyan további hitelesítési mechanizmust biztosít, amely attól függetlenül használható az engedélyezéskor, hogy korábban hitelesítve volt-e a jelölt felhasználó a Windows PRIV tartományban.

## Előfeltételek
<a id="prerequisites" class="xliff"></a>

Az Azure MFA-nak a MIM szolgáltatásban történő használatához a következők szükségesek:

- Internet-hozzáférés minden, a PAM megoldást használó MIM szolgáltatás esetén az Azure MFA szolgáltatás eléréséhez
- Azure-előfizetés
- Azure Active Directory Premium licenc vagy valamilyen alternatív, Azure MFA-licencet biztosító megoldás a jelölt felhasználóknál
- Telefonszám az összes jelölt felhasználó esetén

## Azure MFA-szolgáltató létrehozása
<a id="creating-an-azure-mfa-provider" class="xliff"></a>

Ez a szakasz az Azure MFA-szolgáltatónak a Microsoft Azure Active Directoryben történő beállításához nyújt útmutatást.  Ha már használja az Azure MFA-t – akár önállóan, akár az Azure Active Directory Premiummal konfigurálva – ugorjon a következő bekezdéshez.

1.  Nyisson meg egy webböngészőt, és jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com) Azure előfizetés-adminisztrátorként.

2.  A bal alsó sarokban kattintson az **Új** gombra.

3.  Válassza az **App Services > Active Directory > Multi-Factor Auth Provider > Quick Create** (Alkalmazásszolgáltatások > Active Directory > Többtényezős hitelesítési szolgáltató > Gyorslétrehozás) lehetőséget.

4.  A **Name** (Név) mezőbe írja be **PAM** szót, és a Usage Model (Használati modell) mezőben válassza a Per Enabled User (Engedélyezett felhasználónként) lehetőséget. Ha már rendelkezik Azure AD-címtárral, válassza azt a címtárat. Végül kattintson a **Create** (Létrehozás) gombra.

## Az Azure MFA szolgáltatás hitelesítő adatainak letöltése
<a id="downloading-the-azure-mfa-service-credentials" class="xliff"></a>

A következő lépésben létre fog hozni egy fájlt, amely tartalmazza a PAM-nek az Azure MFA-hoz való kapcsolódásához szükséges hitelesítési anyagokat.

1. Nyisson meg egy webböngészőt, és jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com) Azure előfizetés-adminisztrátorként.

2.  Az Azure portál menüjében kattintson az **Active Directory** elemre, majd lépjen a **Multi-Factor Auth Providers** (Többtényezős hitelesítési szolgáltatók) lapra.

3.  Kattintson a PAM-hoz használni kívánt Azure MFA-szolgáltatóra, majd kattintson a **Manage** (Kezelés) gombra.

4.  Az új ablakban a **Configure** (Konfigurálás) felirat alatti bal oldali panelen kattintson a **Settings** (Beállítások) lehetőségre.

5.  Az **Azure Multi-Factor Authentication** ablakban kattintson az **SDK** elemre a **Downloads** (Letöltések) területen.

6.  Kattintson a **Download** (Letöltés) hivatkozásra az **SDK for ASP.net 2.0 C\#** nevű fájl ZIP oszlopában.

![A Multi-Factor Authentication SDK letöltése – képernyőkép](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  A letöltött ZIP-fájlt másolja minden rendszerre, ahol a MIM szolgáltatás telepítve van. 

>[!NOTE]
> A ZIP-fájl az Azure MFA szolgáltatással való hitelesítésre szolgáló kulcsanyagokat tartalmazza.

## A MIM szolgáltatás konfigurálása az Azure MFA használatára
<a id="configuring-the-mim-service-for-azure-mfa" class="xliff"></a>

1.  Rendszergazdaként vagy a MIM-et telepítő felhasználói fiókkal jelentkezzen be arra a számítógépre, amelyre a MIM szolgáltatás telepítve van.

2.  Hozzon létre egy új mappát abban a könyvtárban, ahová a MIM szolgáltatás telepítve van, például: `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts`.

3.  A Windows Intézővel keresse meg a **pf\\certs** mappát az előző szakaszban letöltött ZIP-fájlban, és másolja a **cert\_key.p12** fájlt az új mappába.

4.  Lépjen a Windows Intézőben a ZIP-fájl **pf** mappájába, és nyissa meg a **pf\_auth.cs** fájlt egy egyszerű szövegszerkesztőben, például a WordPadben.

5.  Keresse meg a következő paramétereket: **LICENSE\_KEY**, **GROUP\_KEY**, **CERT\_PASSWORD**.

![Értékek másolása a pf\_auth.cs fájlból – képernyőkép](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  Nyissa meg a Jegyzettömbben az **MfaSettings.xml** fájlt, amely a `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service` mappában található.

7.  A pf\_auth.cs fájlból másolja a LICENSE\_KEY, GROUP\_KEY és a CERT\_PASSWORD paraméter értékét az MfaSettings.xml fájl megfelelő XML-elemeibe.

8.  A **<CertFilePath>** XML-elemben adja meg a korábban kibontott cert\_key.p12 fájl teljes elérési útját.

9.  A **<username>** elemben adjon meg egy tetszőleges felhasználónevet.

10.  A **<DefaultCountryCode>** elemben adja meg a felhasználók felhívásához szükséges országhívószámot, az Amerikai Egyesült Államok és Kanada esetében például az 1-et. Erre a számra abban az esetben van szükség, ha a felhasználók országkód nélkül megadott telefonszámmal vannak regisztrálva. Ha a felhasználó telefonszáma a vállalat számára beállított országhívószámtól különböző előhívószámmal hívható, az adott országkódnak szerepelnie kell a regisztrált telefonszámban.

11.  Mentse és írja felül az **MfaSettings.xml** fájlt a MIM szolgáltatás mappában (`C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`). 

> [!NOTE]
> A folyamat végén győződjön meg róla, hogy az **MfaSettings.xml** fájl, illetve annak minden példánya, valamint a ZIP-fájl nyilvánosan nem olvasható.

## PAM-felhasználók konfigurálása az Azure MFA számára
<a id="configure-pam-users-for-azure-mfa" class="xliff"></a>

Azure MFA-alapú hitelesítést igénylő szerepkört aktiváló felhasználó esetén a felhasználó telefonszámát tárolni kell a MIM szolgáltatásban. Ez az attribútum kétféleképpen állítható be.

Első lehetőség: a `New-PAMUser` parancs egy telefonszám-attribútumot másol a felhasználó CORP tartománybeli címtárbejegyzéséből a MIM szolgáltatás adatbázisába. Ezt a műveletet egyszer kell elvégezni.

Második lehetőség: a `Set-PAMUser` parancs frissíti a telefonszám-attribútumot a MIM szolgáltatás adatbázisában. Az alábbi parancs például egy létező PAM-felhasználó telefonszámát cseréli le a MIM szolgáltatásban. A szám címtárbeli bejegyzése nem változik.

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


## PAM-szerepkörök konfigurálása az Azure MFA számára
<a id="configure-pam-roles-for-azure-mfa" class="xliff"></a>

Ha már az adott PAM-szerepkörre jelölt összes felhasználó telefonszáma tárolva van a MIM szolgáltatás adatbázisában, a szerepkör konfigurálható az Azure MFA hitelesítés megkövetelésére. Ez a `New-PAMRole` vagy a `Set-PAMRole` paranccsal tehető meg. Például

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Egy adott szerepkör esetében az „-MFAEnabled 0” paraméternek a `Set-PAMRole` parancsban való megadásával tiltható le az Azure MFA.

## Hibaelhárítás
<a id="troubleshooting" class="xliff"></a>

A következő események a Privileged Access Management eseménynaplójában jelenhetnek meg:

| ID  | Severity | Létrehozója | Leírás |
|-----|----------|--------------|-------------|
| 101 | Hiba       | MIM szolgáltatás            | A felhasználó nem végezte el az Azure MFA hitelesítést (például nem vette fel a telefont) |
| 103 | Adatok | MIM szolgáltatás            | A felhasználó aktiválás közben hajtotta végre az Azure MFA hitelesítést                       |
| 825 | Figyelmeztetés     | A PAM figyelőszolgáltatása | A telefonszám megváltozott                                |

A sikertelen telefonhívások okára (101-es esemény) vonatkozó további információkért megtekinthet vagy letölthet egy Azure MFA-jelentést is.

1.  Nyisson meg egy webböngészőt, és jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com) globális Azure AD-rendszergazdaként.

2.  Az Azure-portál menüjében válassza az **Active Directory** lehetőséget, majd lépjen a **Multi-Factor Auth Providers** (Többtényezős hitelesítési szolgáltatók) lapra.

3.  Kattintson a PAM-hoz használni kívánt Azure MFA-szolgáltatóra, majd kattintson a **Manage** (Kezelés) gombra.

4.  Kattintson az új ablakban a **Usage** (Használat), majd a **User Details** (Felhasználó adatai) lehetőségre.

5.  Válassza ki az időtartományt, és jelölje be a jelölőnégyzetet a **Name** (Név) pont mellett, a további jelentések oszlopában. Kattintson az **Exportálás CSV-fájlba** elemre.

6.  A jelentést a létrehozását követően megtekintheti a portálon, illetve, ha az MFA-jelentés túl hosszú, letöltheti CSV-fájlként. Az **AUTH TYPE** oszlopban található **SDK**-értékek jelölik azokat a sorokat, amelyek a PAM-aktivációs kérésekhez kapcsolódnak: ezek a MIM-től vagy más helyszíni szoftvertől származó események. A **USERNAME** mező a MIM szolgáltatás adatbázisában található felhasználóobjektum GUID-azonosítója. Ha egy hívás sikertelen volt, az **AUTHD** oszlop értéke **No** lesz, a **CALL RESULT** oszlop pedig tartalmazza a hiba okát, és annak részleteit.

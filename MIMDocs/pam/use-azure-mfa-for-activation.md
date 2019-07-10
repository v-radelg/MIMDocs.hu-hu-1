---
title: Az Azure MFA használata a PAM aktiválásához | Microsoft Docs
description: Állítsa be az Azure MFA-t második biztonsági szintként, ha a felhasználók szerepköröket aktiválnak a Privileged Access Managementben.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: 72dd1d3cf34e28567fa672b747a04347b150797e
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690786"
---
# <a name="using-azure-mfa-for-activation"></a>Aktiválás az Azure MFA használatával
> [!IMPORTANT]
> Miatt elévülése az Azure multi-factor Authentication hitelesítés szoftverfejlesztői közleményt. Az Azure MFA SDK ügyfeleink 2018. November 14., a kivezetési dátum másnapi támogatott lesz. Új ügyfelek és a meglévő ügyfelek nem tudják SDK letöltéséhez már a klasszikus Azure portálon keresztül. Töltse le, hogy kell keresse fel a létrehozott csomagot biztosítunk MFA szolgáltatás hitelesítő adatai az Azure ügyfélszolgálatához. <br> A Microsoft fejlesztői csapat dolgozik MFA módosításai és az MFA-kiszolgáló SDK integrálásával.  Ez része lesz egy közelgő gyorsjavítás lásd: [korábbi verziók](../reference/version-history.md) hirdetmények. 


A PAM-szerepkörök konfigurálásakor kiválaszthatja, hogyan szeretné engedélyekkel felruházni azokat a felhasználókat, akik a szerepkör aktiválását kérik. A PAM-engedélyezés a következő választási lehetőségeket nyújtja:

- Szerepkör tulajdonosának jóváhagyása
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Ha egyik ellenőrzési mód sincs engedélyezve, a jelölt felhasználók szerepköre automatikusan aktiválódik.

A Microsoft Azure Multi-Factor Authentication (MFA) olyan hitelesítési szolgáltatás, amely a bejelentkezési kísérletek mobilalkalmazással, telefonhívással vagy SMS-sel történő megerősítését kéri a felhasználóktól. A szolgáltatás a Microsoft Azure Active Directoryval, valamint felhőalapú és helyszíni nagyvállalati alkalmazásokkal is használható. A PAM-forgatókönyv az Azure MFA olyan kiegészítő hitelesítési módszert biztosít. Az Azure MFA hitelesítést, függetlenül attól, hogyan hitelesíti a felhasználók a Windows PRIV tartományhoz használható.

## <a name="prerequisites"></a>Előfeltételek

A MIM az Azure MFA használatához szükséges:

- Internet-hozzáférés minden, a PAM megoldást használó MIM szolgáltatás esetén az Azure MFA szolgáltatás eléréséhez
- Azure-előfizetés
- Azure Active Directory Premium licenc vagy valamilyen alternatív, Azure MFA-licencet biztosító megoldás a jelölt felhasználóknál
- Telefonszám az összes jelölt felhasználó esetén

## <a name="creating-an-azure-mfa-provider"></a>Azure MFA-szolgáltató létrehozása

Ebben a szakaszban beállíthatja az Azure MFA-szolgáltató a Microsoft Azure Active Directoryban.  Ha már használja az Azure MFA-t – akár önállóan, akár az Azure Active Directory Premiummal konfigurálva – ugorjon a következő bekezdéshez.

1.  Nyisson meg egy webböngészőt, és jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com) Azure előfizetés-adminisztrátorként.

2.  A bal alsó sarokban kattintson az **Új** gombra.

3.  Válassza az **App Services > Active Directory > Multi-Factor Auth Provider > Quick Create** (Alkalmazásszolgáltatások > Active Directory > Többtényezős hitelesítési szolgáltató > Gyorslétrehozás) lehetőséget.

4.  A **Name** (Név) mezőbe írja be **PAM** szót, és a Usage Model (Használati modell) mezőben válassza a Per Enabled User (Engedélyezett felhasználónként) lehetőséget. Ha már rendelkezik Azure AD-címtárral, válassza azt a címtárat. Végül kattintson a **Create** (Létrehozás) gombra.

## <a name="downloading-the-azure-mfa-service-credentials"></a>Az Azure MFA szolgáltatás hitelesítő adatainak letöltése

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

## <a name="configuring-the-mim-service-for-azure-mfa"></a>A MIM szolgáltatás konfigurálása az Azure MFA használatára

1.  Rendszergazdaként vagy a MIM-et telepítő felhasználói fiókkal jelentkezzen be arra a számítógépre, amelyre a MIM szolgáltatás telepítve van.

2.  Hozzon létre egy új mappát abban a könyvtárban, ahová a MIM szolgáltatás telepítve van, például: ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  A Windows Intézőben keresse meg a ```pf\certs``` az előző szakaszban letöltött ZIP-fájlban mappát. Másolja a fájlt ```cert\_key.p12``` az új címtárra.

4.  A Windows Intézőben keresse meg a ```pf``` a ZIP, és nyissa meg a mappa ```pf\_auth.cs``` WordPadben egy szövegszerkesztőben.

5. Keresse meg a következő három paramétert: ```LICENSE\_KEY```, ```GROUP\_KEY```, ```CERT\_PASSWORD```.

![Értékek másolása a pf\_auth.cs fájlból – képernyőkép](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Nyissa meg a Jegyzettömbben az **MfaSettings.xml** fájlt, amely a ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service``` mappában található.

7. A pf\_auth.cs fájlból másolja a LICENSE\_KEY, GROUP\_KEY és a CERT\_PASSWORD paraméter értékét az MfaSettings.xml fájl megfelelő XML-elemeibe.

8. A **<CertFilePath>** XML-elemben adja meg a korábban kibontott cert\_key.p12 fájl teljes elérési útját.

9. A **<username>** elemben adjon meg egy tetszőleges felhasználónevet.

10. A **<DefaultCountryCode>** elemben adja meg a felhasználók felhívásához szükséges országhívószámot, az Amerikai Egyesült Államok és Kanada esetében például az 1-et. Erre a számra abban az esetben van szükség, ha a felhasználók országkód nélkül megadott telefonszámmal vannak regisztrálva. Ha a felhasználó telefonszáma a vállalat számára beállított országhívószámtól különböző előhívószámmal hívható, az adott országkódnak szerepelnie kell a regisztrált telefonszámban.

11. Mentse és írja felül az **MfaSettings.xml** fájlt a MIM szolgáltatás mappában (```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```).

> [!NOTE]
> A folyamat végén győződjön meg róla, hogy az **MfaSettings.xml** fájl, illetve annak minden példánya, valamint a ZIP-fájl nyilvánosan nem olvasható.

## <a name="configure-pam-users-for-azure-mfa"></a>PAM-felhasználók konfigurálása az Azure MFA számára

Azure MFA-alapú hitelesítést igénylő szerepkört aktiváló felhasználó esetén a felhasználó telefonszámát tárolni kell a MIM szolgáltatásban. Ez az attribútum kétféleképpen állítható be.

Első lehetőség: a `New-PAMUser` parancs egy telefonszám-attribútumot másol a felhasználó CORP tartománybeli címtárbejegyzéséből a MIM szolgáltatás adatbázisába. Ezt a műveletet egyszer kell elvégezni.

Második lehetőség: a `Set-PAMUser` parancs frissíti a telefonszám-attribútumot a MIM szolgáltatás adatbázisában. Az alábbi parancs például egy létező PAM-felhasználó telefonszámát cseréli le a MIM szolgáltatásban. A szám címtárbeli bejegyzése nem változik.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>PAM-szerepkörök konfigurálása az Azure MFA számára

Ha már az adott PAM-szerepkörre jelölt összes felhasználó telefonszáma tárolva van a MIM szolgáltatás adatbázisában, a szerepkör konfigurálható az Azure MFA hitelesítés megkövetelésére. Ez a `New-PAMRole` vagy a `Set-PAMRole` paranccsal tehető meg. Például

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Egy adott szerepkör esetében az „-MFAEnabled 0” paraméternek a `Set-PAMRole` parancsban való megadásával tiltható le az Azure MFA.

## <a name="troubleshooting"></a>Hibaelhárítás

A következő események a Privileged Access Management eseménynaplójában jelenhetnek meg:

| ID  | Severity | Létrehozója | Leírás |
|-----|----------|--------------|-------------|
| 101 | Hiba       | MIM szolgáltatás            | A felhasználó nem végezte el az Azure MFA hitelesítést (például nem vette fel a telefont) |
| 103 | Információ | MIM szolgáltatás            | A felhasználó aktiválás közben hajtotta végre az Azure MFA hitelesítést                       |
| 825 | Figyelmeztetés     | A PAM figyelőszolgáltatása | A telefonszám megváltozott                                |

A sikertelen telefonhívások okára (101-es esemény) vonatkozó további információkért megtekinthet vagy letölthet egy Azure MFA-jelentést is.

1.  Nyisson meg egy webböngészőt, és jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com) globális Azure AD-rendszergazdaként.

2.  Az Azure-portál menüjében válassza az **Active Directory** lehetőséget, majd lépjen a **Multi-Factor Auth Providers** (Többtényezős hitelesítési szolgáltatók) lapra.

3.  Kattintson a PAM-hoz használni kívánt Azure MFA-szolgáltatóra, majd kattintson a **Manage** (Kezelés) gombra.

4.  Kattintson az új ablakban a **Usage** (Használat), majd a **User Details** (Felhasználó adatai) lehetőségre.

5.  Válassza ki az időtartományt, és jelölje be a jelölőnégyzetet a **Name** (Név) pont mellett, a további jelentések oszlopában. Kattintson az **Exportálás CSV-fájlba** elemre.

6.  A jelentést a létrehozását követően megtekintheti a portálon, illetve, ha az MFA-jelentés túl hosszú, letöltheti CSV-fájlként. Az **AUTH TYPE** oszlopban található **SDK**-értékek jelölik azokat a sorokat, amelyek a PAM-aktivációs kérésekhez kapcsolódnak: ezek a MIM-től vagy más helyszíni szoftvertől származó események. A **USERNAME** mező a MIM szolgáltatás adatbázisában található felhasználóobjektum GUID-azonosítója. Ha egy hívás sikertelen volt, az **AUTHD** oszlop értéke **No** lesz, a **CALL RESULT** oszlop pedig tartalmazza a hiba okát, és annak részleteit.

## <a name="next-steps"></a>További lépések

- [Mi az Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Ingyenes Azure-fiók létrehozása még ma](https://azure.microsoft.com/free/)

---
title: "Az Önkiszolgáló jelszóváltoztatás portál kezelése | Microsoft Docs"
description: "Ismerje meg, hogy milyen újdonságokat kínál a MIM 2016 önkiszolgáló jelszó-változtatási összetevője, például hogy miként képes együttműködni a többtényezős hitelesítéssel."
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 90c773c30b0ab23ad29ca1a215745bf59b188764
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/14/2017
---
>[!IMPORTANT]
Miatt az Azure multi-factor Authentication Software Development Kit érvénytelenítése bejelentés. Az Azure MFA SDK használatból való kivonást időpontjáig 2018. október 01. a meglévő ügyfeleknek is támogatottak lesznek. Új ügyfelek és az ügyfelek aktuális fog tudni többé SDK letöltése a klasszikus Azure portálon keresztül. Töltse le, akkor kell érheti el a generált MFA szolgáltatás hitelesítő adatait a csomagot fogadó Azure ügyfélszolgálathoz. <br> A Microsoft fejlesztői csapat dolgozik a többtényezős hitelesítés módosításai tervezési integrálja az MFA kiszolgáló SDK-val. Ez szerepelni fog a jövőbeli gyorsjavítás korai 2018.

# <a name="working-with-self-service-password-reset"></a>Az önkiszolgáló jelszóváltoztatás kezelése
A Microsoft Identity Manager 2016 önkiszolgáló jelszó-változtatási funkciója új lehetőségeket kínál. A funkcionalitást több fontos elemmel bővítettük:

-   A felhasználók az Önkiszolgáló jelszóváltoztatás portálon és a Windows bejelentkezési képernyőjen már a jelszavuk módosítása és a támogatási rendszergazdák segítsége nélkül is feloldhatják fiókjukat. A felhasználók ártatlan tévedéssel is sokféleképpen kizárhatják magukat a fiókjukból, például ha régi jelszót adnak meg, ha egy kétnyelvű számítógépen a billentyűzet nem megfelelő nyelvre van beállítva, vagy ha olyan megosztott munkaállomáson próbálnak bejelentkezni, amelyen már meg van nyitva valaki más fiókja.

-   Az összetevő egy új hitelesítési kapuval, a Telefonos kapuval bővült. Ez lehetővé teszi a felhasználó telefonhíváson keresztüli hitelesítését.

-   Az összetevő már támogatja a Microsoft Azure Multi-Factor Authentication (MFA) többtényezős hitelesítési szolgáltatást. Ez a meglévő SMS-alapú egyszeri jelszó kapuhoz és az új telefonos kapuhoz egyaránt használható.

## <a name="azure-for-multi-factor-authentication"></a>Többtényezős hitelesítés az Azure-ral
A Microsoft Azure Multi-Factor Authentication egy olyan hitelesítési szolgáltatás, amely a bejelentkezési kísérletek mobilalkalmazással, telefonhívással vagy szöveges üzenettel történő megerősítését kéri a felhasználóktól. A szolgáltatás a Microsoft Azure Active Directoryval, valamint felhőalapú és helyszíni nagyvállalati alkalmazásokkal is használható.

Az Azure MFA olyan kiegészítő hitelesítési módszert kínál, amely megerősítheti a már meglévő hitelesítési folyamatokat, például a MIM által az önkiszolgáló bejelentkezési segéddel végzett hitelesítést.

Az Azure MFA használata esetén a felhasználók annak érdekében hitelesítik magukat a rendszerrel, hogy igazolják az identitásukat, amikor megpróbálnak újra hozzáférést nyerni fiókjukhoz és erőforrásaikhoz. A hitelesítés történhet SMS-ben vagy telefonhívással.   Minél erősebb a hitelesítés, annál biztosabb, hogy a hozzáférést igénylő felhasználó ténylegesen az adott identitás tulajdonosa. A hitelesítést követően a felhasználó új jelszót választhat a régi helyett.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Az MFA szolgáltatással végzett önkiszolgáló fiókzárfeloldás és jelszóváltoztatás beállításának előfeltételei
Jelen szakaszban feltételezzük, hogy már letöltötte és sikeresen üzembe helyezte a Microsoft Identity Manager 2016-ot, a következő összetevőkkel és szolgáltatásokkal együtt:

-   Egy Active Directory-kiszolgálóként beállított Windows Server 2008 R2-alapú kiszolgáló, AD tartományi szolgáltatások és tartományvezérlő szerepkörrel, kijelölt („vállalati”) tartománnyal.

-   A fiókzárolásra vonatkozóan definiált csoportházirend.

-   A MIM 2016 Synchronization Service (Sync) telepítve van, és az AD-tartományhoz csatlakoztatott kiszolgálón fut.

-   A MIM 2016 szolgáltatás és -portál (az önkiszolgáló jelszó-változtatási és -visszaállítási portállal együtt) telepítve van és fut egy kiszolgálón (ez lehet a Syncet futtató kiszolgáló is).

-   A MIM Sync be van állítva az identitások szinkronizálására az AD és a MIM között, beleértve a következőket:

    -   Az Active Directory-kezelőügynök (ADMA) be van állítva az AD DS-hez való csatlakozásra, és képes identitási adatokat importálni az Active Directoryba és exportálni onnan.

    -   A MIM-kezelőügynök (MIM MA) be van állítva a FIM szolgáltatás adatbázisához való kapcsolódásra, és képes identitási adatokat importálni a FIM-adatbázisba és exportálni onnan.

    -   A MIM-portál szinkronizálási szabályai úgy vannak beállítva, hogy lehetővé teszik a felhasználói adatok szinkronizálását, és támogatják a MIM szolgáltatás szinkronizálásra épülő tevékenységeit.

-   A MIM 2016 beépülő moduljai és bővítményei – többek között a Windows-bejelentkezésbe integrált önkiszolgáló jelszó-változtatási (SSPR-) ügyfél – telepítve vannak a kiszolgálón vagy egy különálló ügyfélszámítógépen.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>A MIM és a többtényezős hitelesítés együttműködésének előkészítése
Állítsa be a MIM Sync szolgáltatást a jelszó-átállítási és fiókfeloldási funkció támogatásához. További információért lásd: [A FIM beépülő moduljainak és bővítményeinek telepítése](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [A FIM SSPR telepítése](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Az SSPR hitelesítési kapui](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx), illetve [Tesztlabor-útmutató az SSPR-hez](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx).

A következő szakaszban az Azure MFA szolgáltató Microsoft Azure Active Directoryben történő beállításához nyújt útmutatást. Ennek keretében létre fog hozni egy fájlt, amely tartalmazza az MFA által az Azure MFA-hoz való kapcsolódáshoz szükséges hitelesítési anyagokat.  A folyamat végrehajtásához Azure-előfizetés szükséges.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>A többtényezős hitelesítési szolgáltató regisztrálása az Azure-ban

1.  Lépjen a [klasszikus Azure-portálra](http://manage.windowsazure.com), és jelentkezzen be Azure előfizetés-adminisztrátorként.

2.  A bal alsó sarokban kattintson a **New** (Új) gombra.

3.  Válassza az **App Services &gt; Active Directory &gt; Multi-Factor Auth Provider &gt; Quick Create** (Alkalmazásszolgáltatások > Active Directory > Többtényezős hitelesítési szolgáltató > Gyors létrehozás) lehetőséget.

![Az Azure portálon gyorsan MFA lemezkép létrehozása](media/MIM-SSPR-Azureportal.png)

4.  A **Name** (Név) mezőbe írja be, hogy **SSPRMFA**, majd kattintson a **Create** (Létrehozás) gombra.

![Kép: Többtényezős hitelesítési szolgáltató az Azure-portálon](media/MIM-SSPR-Azureportal-2.png)

5.  A klasszikus Azure-portál menüjében kattintson az **Active Directory** elemre, majd lépjen a **Multi-Factor Auth Providers** (Többtényezős hitelesítési szolgáltatók) lapra.

6.  Kattintson az **SSPRMFA** elemre, majd a **Manage** (Kezelés) gombra a képernyő alján.

    ![Ikon: A Manage gomb az Azure-portálon](media/MIM-SSPR-ManageButton.png)

7.  Az új ablakban a **Configure** (Konfigurálás) felirat alatti bal oldali panelen kattintson a **Settings** (Beállítások) lehetőségre.

8.  A **csalási riasztás**, törölje a jelet ** felhasználó blokkolása visszaélés jelentésekor. Ennek a célja megakadályozni adott esetben a teljes szolgáltatás letiltását.

9. A megjelenő **Azure Multi-Factor Authentication** (Azure többtényezős hitelesítés) ablakban a bal oldali menüben kattintson az **SDK** elemre a **Downloads** (Letöltések) területen.

10. Kattintson a **Download** (Letöltés) hivatkozásra az **SDK for ASP.net 2.0 C#** nyelvű fájl ZIP oszlopában.

    ![Kép: Azure MFA ZIP-fájl letöltése](media/MIM-SSPR-Azure-MFA.png)

11. A letöltött ZIP-fájlt másolja minden rendszerre, ahol a MIM szolgáltatás telepítve van.  Vegye figyelembe, hogy a ZIP-fájl az Azure MFA szolgáltatással való hitelesítésre szolgáló kulcskezelő anyagokat tartalmaz.

### <a name="update-the-configuration-file"></a>A konfigurációs fájl frissítése

1. A MIM-et telepítő felhasználói fiókkal jelentkezzen be arra a számítógépre, ahol a MIM szolgáltatás telepítve van.

2. Hozzon létre egy új könyvtárat a MIM szolgáltatás telepítési mappájában, például: **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. A Windows Intézővel keresse meg a **\pf\certs** mappát az előző szakaszban letöltött ZIP-fájlban, és másolja a **cert_key.p12** fájlt az új mappába.

4.  Az SDK ZIP-fájlban a **\pf** mappában nyissa meg a **pf_auth.cs** fájlt.

5.  Keresse meg a következő három paramétert: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Kép: A pf_auth.cs fájl kódja](media/MIM-SSPR-pFile.png)

6.  A **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** mappában nyissa meg az **MfaSettings.xml** fájlt.

7.  A pf_aut.cs fájlból a `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` paraméterek értékeit másolja a megfelelő XML-elemekbe az MfaSettings.xml fájlban.

8.  Az SDK ZIP-fájlban a \pf\certs mappából bontsa ki a **cert_key.p12** fájlt, majd az MfaSettings.xml fájl `<CertFilePath>` XML-elemében adja meg a kicsomagolt fájl teljes elérési útját.

9. A `<username>` elemben adjon meg egy tetszőleges felhasználónevet.

10. A `<DefaultCountryCode>` elemben adja meg az alapértelmezett országkódját. Országkód nélkül telefonszám bejegyzése esetén a rendszer ezt az országkódot fogja használni. Amennyiben a felhasználó nemzetközi országkóddal rendelkezik, azt fel kell tüntetni a regisztrált telefonszámban.

11. Mentse az MfaSettings.xml fájlt, ezen a néven, ugyanarra a helyre.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>A Telefonos kapu vagy az Egyszeri SMS-jelszó kapu beállítása

1.  Az Internet Explorerben lépjen a MIM-portálra, hitelesítse magát a MIM-rendszergazdai fiókkal, majd a bal oldali navigációs sávon kattintson a **Workflows** (Munkafolyamatok) elemre.

    ![Kép: Navigáció a MIM-portálon](media/MIM-SSPR-workflow.jpg)

2.  Jelölje be a **Password Reset AuthN Workflow** (Jelszó-változtatási hitelesítési munkafolyamat) négyzetet.

    ![Kép: Munkafolyamatok a MIM-portálon](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Kattintson az **Activities** (Tevékenységek) lapra, majd görgessen le az **Add Activity** (Tevékenység felvétele) elemhez.

4.  Válassza a **Phone Gate** (Telefonos kapu) vagy a **One-Time Password SMS Gate** (Egyszeri SMS-jelszavas kapu) elemet, majd kattintson a **Select** (Kiválasztás) lehetőségre, végül pedig az **OK** gombra.

A szervezeti felhasználók ezután már regisztrálhatnak a jelszóváltoztatásra.  A folyamat során meg kell adniuk munkahelyi vagy mobiltelefonszámukat, hogy a rendszer tudja, hol keresheti őket (vagy küldhet nekik SMS-t).

#### <a name="register-users-for-password-reset"></a>Felhasználók regisztrálása jelszóváltoztatásra

1.  A felhasználó fogja elindítani a webböngészőt, és keresse meg a MIM jelszó-változtatási regisztrációs portálra.  (A portál jellemzően Windows-hitelesítésre van konfigurálva.)  Identitása megerősítéséhez a portálon ismét meg kell adnia felhasználónevét és jelszavát.

    Be kell lépnie a jelszó-regisztrálási portálra, és hitelesítenie kell magát a felhasználónevével és jelszavával.

2.  A **Phone Number** (Telefonszám) vagy a **Mobile Phone** (Mobiltelefonszám) mezőben meg kell adnia az országkódot, majd egy szóközzel elválasztva a telefonszámot, majd a **Next** (Tovább) gombra kell kattintania.

    ![Kép: Telefonszám ellenőrzése a MIM-ben](media/MIM-SSPR-PhoneVerification.JPG)

    ![Kép: Mobiltelefonszám ellenőrzése a MIM-ben](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Hogyan működik mindez a felhasználóknál?
Most, hogy minden be van állítva és fut, bizonyára tudni szeretné, hogy mit kell tennie a felhasználónak, ha például közvetlenül a szabadsága előtt megváltoztatta a jelszavát, majd visszatérve rájön, hogy teljesen elfelejtette az új jelszót.

A felhasználó kétféleképpen használhatja a jelszó-változtatási és fiókfeloldási funkciót: a Windows bejelentkezési képernyőjéről vagy az önkiszolgáló portálról.

Ha a MIM beépülő moduljait és bővítményeit olyan tartományhoz csatlakoztatott számítógépen telepíti, amely a szervezeti hálózaton keresztül a MIM szolgáltatáshoz csatlakozik, a felhasználó az asztali bejelentkezési környezetből megváltoztathatja az elfelejtett jelszavát.  Ezen a folyamaton a következő lépésekkel haladhat végig.

#### <a name="windows-desktop-login-integrated-password-reset"></a>A Windows bejelentkezési felülettel integrált jelszó-átállítási funkció

1.  Ha a felhasználó többször is helytelen jelszót ad meg, a bejelentkezési képernyőn megjelenik a **Nem tud bejelentkezni?** hivatkozás. .

    ![Kép: Bejelentkezési képernyő](media/MIM-SSPR-problemsloggingin.JPG)

    Ha erre a hivatkozásra kattint, megjelenik a MIM-jelszó átállítása képernyő, amelyen megváltoztathatja a jelszavát vagy feloldhatja a fiók zárolását.

    ![Kép: MIM-jelszó átállítása](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  A rendszer hitelesítésre kéri a felhasználót. Ha a többtényezős hitelesítés be lett állítva, a felhasználó telefonhívást kap.

3.  A háttérben mi történik az adott Azure MFA a felhasználó telefonhívást megadott, amikor azok a szolgáltatásra regisztrált.

4.  Amikor a felhasználó felveszi a telefont, a rendszer arra kéri, hogy nyomja meg a kettőskereszt (#) gombot a készüléken. A felhasználónak ezután a **Next** (Tovább) gombra kell kattintania a portálon.

    Ha más kapuk is be lettek állítva, a rendszer a következő képernyőkön további információk megadására kéri a felhasználót.

    > [!NOTE]
    > Ha felhasználó türelmetlen, és még a # gomb megnyomása előtt a **Next** (Tovább) gombra kattint, a hitelesítés meghiúsul.

5.  Sikeres hitelesítést követően a felhasználó két lehetőség közül választhat: feloldhatja a fiók zárolását és megtarthatja a jelenlegi jelszavát, vagy új jelszót állíthat be.

6.  Ebben az esetben a felhasználónak kétszer meg kell adnia az új jelszót a jelszóváltoztatás érvényesítéséhez.

#### <a name="access-from-the-self-service-portal"></a>Elérés az önkiszolgáló portálról

1.  A felhasználó böngészőben a **jelszó-változtatási portálra** lép, megadja a felhasználónevét, majd a **Next** (Tovább) gombra kattint.

    Ha a többtényezős hitelesítés be lett állítva, a felhasználó telefonhívást kap. A háttérben mi történik az adott Azure MFA a felhasználó telefonhívást megadott, amikor azok a szolgáltatásra regisztrált.

    Amikor a felhasználó felveszi a telefont, a rendszer arra kéri, hogy nyomja meg a kettőskereszt (#) gombot a készüléken. A felhasználónak ezután a **Next** (Tovább) gombra kell kattintania a portálon.

2.  Ha más kapuk is be lettek állítva, a rendszer a következő képernyőkön további információk megadására kéri a felhasználót.

    > [!NOTE]
    > Ha felhasználó türelmetlen, és még a # gomb megnyomása előtt a **Next** (Tovább) gombra kattint, a hitelesítés meghiúsul.

3.  A felhasználónak választania kell, hogy a jelszavát szeretné megváltoztatni vagy a fiók zárolását kívánja feloldani. Ha a fiók zárolásának feloldását választja, a fiók zárolása megszűnik.

    ![Kép: MIM Bejelentkezési segéd – Fiókzárolás feloldása](media/MIM-SSPR-accountUnlock.JPG)

4.  Sikeres hitelesítést követően a felhasználó két lehetőség közül választhat: megtarthatja a jelenlegi jelszavát, vagy új jelszót állíthat be.

5.  ![Kép: Fiók zárolásának sikeres feloldása a MIM-ben](media/MIM-SSPR-account-unlock.JPG)

6.  Ha a felhasználó úgy dönt, hogy megváltoztatja a jelszavát, a módosításhoz kétszer be kell írnia az új jelszót, majd a **Next** (Tovább) gombra kell kattintania.

    ![Kép: Jelszóváltoztatás a MIM Bejelentkezési segéddel](media/MIM-SSPR-PR1.JPG)

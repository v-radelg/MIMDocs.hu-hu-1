---
title: Az önkiszolgáló jelszóváltoztatás kezelése |} A Microsoft Docs
description: Ismerje meg, hogy milyen újdonságokat kínál a MIM 2016 önkiszolgáló jelszó-változtatási összetevője, például hogy miként képes együttműködni a többtényezős hitelesítéssel.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/30/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 5f866df4652839f084770515ef69eccf28df4170
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334261"
---
# <a name="self-service-password-reset-deployment-options"></a>Az önkiszolgáló jelszó-visszaállítási üzembe helyezési beállítások

Az új ügyfelek, akik [jogosult az Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing), javasoljuk a [az Azure AD önkiszolgáló jelszó-visszaállítás](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks.md) a végfelhasználói élményt.  Az Azure AD önkiszolgáló jelszó alaphelyzetbe állítása mindkét webes és a Windows-alapú felületet biztosít a felhasználó számára a saját jelszó visszaállítása, és számos, a MIM, beleértve a másodlagos e-mail-cím és a Q & A kapuk megegyező funkciókat támogatja.  Amikor üzembe helyezése az Azure AD önkiszolgáló jelszó-visszaállítás, az Azure AD Connect támogatja [az új jelszavak írása Active Directory tartományi szolgáltatások](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback.md), és a MIM [jelszóváltozás-értesítési szolgáltatás](deploying-mim-password-change-notification-service-on-domain-controller.md) segítségével továbbíthatja a más rendszerekre, például más gyártó által címtárkiszolgálóra, valamint a jelszavakat.  A MIM üzembe helyezésekor [jelszókezelés](infrastructure/mim2016-password-management.md) nincs szükség a MIM szolgáltatás vagy a MIM önkiszolgáló jelszó-visszaállítási vagy regisztrációs portálok üzembe helyezni.  Ehelyett is kövesse az alábbi lépéseket:

- Első, ha kíván küldeni a jelszavak címtárakhoz eltérő Azure AD és az AD DS-összekötőkkel az Active Directory Domain Servicesben, és minden olyan további célrendszereket MIM Sync telepítése, konfigurálása a MIM [jelszókezelés](infrastructure/mim2016-password-management.md) és üzembe helyezése a [jelszóváltozás-értesítési szolgáltatás](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Ezt követően kell az Azure AD-től eltérő címtárak jelszavak küldése, ha konfigurálja az Azure AD Connect [az új jelszavak írása Active Directory tartományi szolgáltatások](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback.md).
- Másik lehetőségként [előzetes regisztráció felhasználók](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-authenticationdata.md).
- Végül [az Azure AD önkiszolgáló jelszóátállítás bevezetése a végfelhasználók számára](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment.md).

Meglévő ügyfeleink korábban telepítette a Forefront Identity Manager (FIM) az önkiszolgáló jelszó-visszaállítási, és az Azure Active Directory Premium-licencek, javasoljuk, hogy az Azure AD önkiszolgáló jelszó-re való áttérés megtervezése alaphelyzetbe állítása.  Átválthat a végfelhasználók számára, hogy az Azure AD önkiszolgáló jelszó-visszaállítás nélkül őket az újbóli regisztrációt, a [szinkronizálása, vagy a PowerShell beállítása a felhasználó másodlagos e-mail-cím vagy a mobiltelefon száma](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-authenticationdata.md). Miután a felhasználók regisztrálva vannak az Azure AD önkiszolgáló jelszó-visszaállítás, a FIM jelszó-visszaállítási portál leszerelhetők.

Azon ügyfeleink esetében, amelyek még nem telepítették az Azure AD önkiszolgáló jelszó-visszaállítást a felhasználók számára, a MIM emellett önkiszolgáló jelszó-visszaállítási portál.  FIM képest, a MIM 2016 tartalmazza a következő módosításokat:

- A MIM önkiszolgáló jelszóváltoztatás portálon és a Windows bejelentkezési képernyőjéről lehetővé teszik a felhasználók feloldhatják fiókjukat jelszavuk módosítása nélkül.
- Egy új hitelesítési kapuval telefonos kapu, a MIM lett hozzáadva. Ez lehetővé teszi a felhasználói hitelesítést telefonhíváson keresztül a Microsoft Azure multi-factor Authentication (MFA) szolgáltatás.

A MIM 2016 kibocsátási buildek 4.5.26.0 verzióra támaszkodnak az ügyfél letöltése az Azure multi-factor Authentication Software Development Kitet (az Azure MFA SDK).  Adott SDK elavult, és az Azure MFA SDK támogatja a meglévő ügyfeleknek csak a másnapi 2018. November 14., a kivezetési dátum. Eddig a dátumig ügyfelek kapcsolatba kell lépnie a létrehozott csomagot MFA szolgáltatás hitelesítő adatait az Azure ügyfélszolgálatához, azokat nem lehet az Azure MFA SDK letöltése. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>ÚJ! Az Azure multi-factor Authentication-kiszolgáló aktuális Azure MFA konfigurációjának frissítése

Ez [cikk](working-with-mfaserver-for-mim.md) ismerteti, hogyan frissítse a központi telepítési MIM önkiszolgáló jelszó-visszaállítási portál és a PAM konfigurációját, a többtényezős hitelesítés az Azure multi-factor Authentication-kiszolgáló használatával.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Központi telepítése a MIM az önkiszolgáló jelszó-változtatási portál a többtényezős hitelesítés az Azure MFA használatával

Az alábbi szakasz központi telepítése a MIM az önkiszolgáló jelszó-visszaállítási portál, a többtényezős hitelesítés az Azure MFA használatával.  Ezeket a lépéseket csak akkor szükség, ügyfelek esetében, akik az Azure AD önkiszolgáló jelszó-visszaállítás nem használ a felhasználók számára.

A Microsoft Azure Multi-Factor Authentication egy olyan hitelesítési szolgáltatás, amely a bejelentkezési kísérletek mobilalkalmazással, telefonhívással vagy szöveges üzenettel történő megerősítését kéri a felhasználóktól. A szolgáltatás a Microsoft Azure Active Directoryval, valamint felhőalapú és helyszíni nagyvállalati alkalmazásokkal is használható.

Az Azure MFA olyan kiegészítő hitelesítési módszert kínál, amely megerősítheti a már meglévő hitelesítési folyamatokat, például a MIM által az önkiszolgáló bejelentkezési segéddel végzett hitelesítést.

Az Azure MFA használata esetén a felhasználók annak érdekében hitelesítik magukat a rendszerrel, hogy igazolják az identitásukat, amikor megpróbálnak újra hozzáférést nyerni fiókjukhoz és erőforrásaikhoz. A hitelesítés történhet SMS-ben vagy telefonhívással.   Minél erősebb a hitelesítés, annál biztosabb, hogy a hozzáférést igénylő felhasználó ténylegesen az adott identitás tulajdonosa. A hitelesítést követően a felhasználó új jelszót választhat a régi helyett.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Az MFA szolgáltatással végzett önkiszolgáló fiókzárfeloldás és jelszóváltoztatás beállításának előfeltételei

Ez a szakasz azt feltételezi, hogy letöltötte és sikeresen a Microsoft Identity Manager 2016 üzembe [MIM Sync, a MIM szolgáltatás és a MIM-portál összetevők](microsoft-identity-manager-deploy.md), többek között a következő összetevők és szolgáltatások:

-   Egy Active Directory-kiszolgálóként beállított Windows Server 2008 R2-alapú kiszolgáló, AD tartományi szolgáltatások és tartományvezérlő szerepkörrel, kijelölt („vállalati”) tartománnyal.

-   A fiókzárolásra vonatkozóan definiált csoportházirend.

-   A MIM 2016 Synchronization Service (Sync) telepítve van, és az AD-tartományhoz csatlakoztatott kiszolgálón fut.

-   A MIM 2016 szolgáltatás és -portál (az önkiszolgáló jelszó-változtatási és -visszaállítási portállal együtt) telepítve van és fut egy kiszolgálón (ez lehet a Syncet futtató kiszolgáló is).

-   A MIM Sync be van állítva az identitások szinkronizálására az AD és a MIM között, beleértve a következőket:

    -   Az Active Directory-kezelőügynök (ADMA) be van állítva az AD DS-hez való csatlakozásra, és képes identitási adatokat importálni az Active Directoryba és exportálni onnan.

    -   A MIM-kezelőügynök (MIM MA) be van állítva a FIM szolgáltatás adatbázisához való kapcsolódásra, és képes identitási adatokat importálni a FIM-adatbázisba és exportálni onnan.

    -   A MIM-portál szinkronizálási szabályai úgy vannak beállítva, hogy lehetővé teszik a felhasználói adatok szinkronizálását, és támogatják a MIM szolgáltatás szinkronizálásra épülő tevékenységeit.

-   A MIM 2016 beépülő moduljai és bővítményei – többek között a Windows-bejelentkezésbe integrált önkiszolgáló jelszó-változtatási (SSPR-) ügyfél – telepítve vannak a kiszolgálón vagy egy különálló ügyfélszámítógépen.

Ebben a forgatókönyvben, amelyekhez a MIM CAL-eszközök a felhasználók, valamint az Azure MFA-előfizetés szükséges.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>A MIM és a többtényezős hitelesítés együttműködésének előkészítése
Állítsa be a MIM Sync szolgáltatást a jelszó-átállítási és fiókfeloldási funkció támogatásához. További információért lásd: [A FIM beépülő moduljainak és bővítményeinek telepítése](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [A FIM SSPR telepítése](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Az SSPR hitelesítési kapui](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx), illetve [Tesztlabor-útmutató az SSPR-hez](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx).

A következő szakaszban az Azure MFA szolgáltató Microsoft Azure Active Directoryben történő beállításához nyújt útmutatást. Ennek keretében létre fog hozni egy fájlt, amely tartalmazza az MFA által az Azure MFA-hoz való kapcsolódáshoz szükséges hitelesítési anyagokat.  A folyamat végrehajtásához Azure-előfizetés szükséges.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>A többtényezős hitelesítési szolgáltató regisztrálása az Azure-ban

1.  Hozzon létre egy [MFA szolgáltató](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider.md).

2. Nyisson meg egy támogatási esetet, és az közvetlen SDK-val igényelni az ASP.net 2.0 C#. Az SDK-t csak adható meg aktuális felhasználók a MIM az MFA, mert a közvetlen SDK elavult. Új ügyfeleknek el kell fogadnia, hogy az MFA-kiszolgálóval integrálni a MIM következő verziójára.

3. A letöltött ZIP-fájlt másolja minden rendszerre, ahol a MIM szolgáltatás telepítve van.  Vegye figyelembe, hogy a ZIP-fájl az Azure MFA szolgáltatással való hitelesítésre szolgáló kulcskezelő anyagokat tartalmaz.

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

1.  A felhasználó elindít egy böngészőt, és a MIM jelszó-változtatási regisztrációs portálra lép.  (A portál jellemzően Windows-hitelesítésre van konfigurálva.)  Identitása megerősítéséhez a portálon ismét meg kell adnia felhasználónevét és jelszavát.

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

3.  Ebben az esetben az Azure MFA szolgáltatás a háttérben felhívja azt a telefonszámot, amelyet a felhasználó megadott, amikor a szolgáltatásra regisztrált.

4.  Amikor a felhasználó felveszi a telefont, a rendszer arra kéri, hogy nyomja meg a kettőskereszt (#) gombot a készüléken. A felhasználónak ezután a **Next** (Tovább) gombra kell kattintania a portálon.

    Ha más kapuk is be lettek állítva, a rendszer a következő képernyőkön további információk megadására kéri a felhasználót.

    > [!NOTE]
    > Ha felhasználó türelmetlen, és még a # gomb megnyomása előtt a **Next** (Tovább) gombra kattint, a hitelesítés meghiúsul.

5.  Sikeres hitelesítést követően a felhasználó két lehetőség közül választhat: feloldhatja a fiók zárolását és megtarthatja a jelenlegi jelszavát, vagy új jelszót állíthat be.

6.  Ebben az esetben a felhasználónak kétszer meg kell adnia az új jelszót a jelszóváltoztatás érvényesítéséhez.

#### <a name="access-from-the-self-service-portal"></a>Elérés az önkiszolgáló portálról

1.  A felhasználó böngészőben a **jelszó-változtatási portálra** lép, megadja a felhasználónevét, majd a **Next** (Tovább) gombra kattint.

    Ha a többtényezős hitelesítés be lett állítva, a felhasználó telefonhívást kap. Ebben az esetben az Azure MFA szolgáltatás a háttérben felhívja azt a telefonszámot, amelyet a felhasználó megadott, amikor a szolgáltatásra regisztrált.

    Amikor a felhasználó felveszi a telefont, a rendszer arra kéri, hogy nyomja meg a kettőskereszt (#) gombot a készüléken. A felhasználónak ezután a **Next** (Tovább) gombra kell kattintania a portálon.

2.  Ha más kapuk is be lettek állítva, a rendszer a következő képernyőkön további információk megadására kéri a felhasználót.

    > [!NOTE]
    > Ha felhasználó türelmetlen, és még a # gomb megnyomása előtt a **Next** (Tovább) gombra kattint, a hitelesítés meghiúsul.

3.  A felhasználónak választania kell, hogy a jelszavát szeretné megváltoztatni vagy a fiók zárolását kívánja feloldani. Ha a fiók zárolásának feloldását választja, a fiók zárolása megszűnik.

    ![Kép: MIM Bejelentkezési segéd – Fiókzárolás feloldása](media/MIM-SSPR-accountUnlock.JPG)

4.  Sikeres hitelesítést követően a felhasználó két lehetőség közül választhat: megtarthatja a jelenlegi jelszavát, vagy új jelszót állíthat be.

5.  ! [A MIM ac
6.  zárolásának sikeres image](media/MIM-SSPR-account-unlock.JPG) száma

6.  Ha a felhasználó úgy dönt, hogy megváltoztatja a jelszavát, a módosításhoz kétszer be kell írnia az új jelszót, majd a **Next** (Tovább) gombra kell kattintania.
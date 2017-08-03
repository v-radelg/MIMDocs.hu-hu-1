---
title: "Frissítés a FIM 2010 R2-ről a Microsoft Identity Manager 2016-ra | Microsoft Docs"
description: "Megtudhatja, hogyan frissítheti a FIM 2010 R2 összetevőit és hogyan telepítheti a MIM 2016 új összetevőit."
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 02/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 20e733f17d6ed590844c526888b649eb6bf5f322
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="upgrade-from-forefront-identity-manager-2010-r2"></a>Frissítés a Forefront Identity Manager 2010 R2-ről

Ha Forefront Identity Manager (FIM) 2010 R2 környezettel rendelkezik, és ki szeretné próbálni a Microsoft Identity Manager (MIM) 2016-ot, akkor ezt a cikket használhatja útmutatóként. A frissítés három fázisból áll:

1.  A MIM Synchronization Service (Sync) telepítése egy olyan kiszolgálóra, amelynek a tartománya csatlakoztatva van az Ön Active Directory- (AD-) tartományához. Ez lecseréli a Sync FIM 2010 R2-es példányát.

2.  A MIM szolgáltatás és -portál telepítése. Ezen a ponton úgy is dönthet, hogy az önkiszolgáló jelszó-visszaállítási (SSPR) regisztrációs portált és a szolgáltatási portált is telepíti, a Privileged Access Management szolgáltatáskészlet telepítését pedig kihagyja.

3.  A MIM beépülő modulok és bővítmények telepítése egy különálló ügyfélszámítógépre. Ez magában foglalja az SSPR Windows bejelentkezési integrált ügyfelet is.


Ez az útmutató feltételezi, hogy az alábbiak már be vannak állítva:
- FIM 2010 R2, telepítve egy tesztkörnyezetben
- Windows Server 2012, Windows Server 2012 R2 vagy Windows Server 2008 R2 rendszert futtató kiszolgálók
- Helyi és környezeti előfeltételek (SQL Server, Exchange Server, SharePoint Services stb.), amelyek konfigurálva vannak a FIM 2010 R2-höz.


## <a name="preparation"></a>Előkészítés

1.  Készítsen biztonsági másolatot a FIM szolgáltatás adatbázisáról, a FIM Sync adatbázisáról, valamint a FIM Sync szolgáltatás konfigurációjáról és a szoftverről.

2.  A Contoso\Rendszergazda fiókkal jelentkezzen be azokon a számítógépeken, ahol telepítve vannak a FIM 2010 R2 összetevői – például a *CORPIDM*. Ezen telepítési példában rendszergazdai jogosultságok szükségesek a FIM 2010 R2 **MIM-re** való frissítéséhez.

3.  Töltse le vagy csomagolja ki a MIM szoftvert.

## <a name="upgrade-the-synchronization-service"></a>A Synchronization Service frissítése

1.  Jelentkezzen be rendszergazdaként egy kiszolgálóra, amelyen üzembe van helyezve a FIM 2010 R2 Synchronization Service („Sync”).

2.  Mielőtt elkezdené, készítsen biztonsági másolatot az adatbázisról.

3.  Nyissa meg a **Szolgáltatások** konzolt, keresse meg a **Forefront Identity Manager Synchronization Service** szolgáltatást, majd állítsa le.

    ![Kép: A Szolgáltatások konzol](media/MIM-UpgFIM1.PNG)

4.  Indítsa el a **MIM Synchronization Service telepítőjét**. A telepítő azonosítja a meglévő Sync szolgáltatás verzióját, és felajánlja a frissítést. A folytatáshoz kattintson az **Update** (Frissítés) gombra.

5.  Ha elfogadja a licencfeltételeket, kattintson a **Next** (Tovább) gombra a folytatáshoz.

6.  Adja meg a Sync által használt szolgáltatásfiók jelszavát, majd kattintson a **Tovább** gombra.

    ![Kép: A MIM Synchronization szolgáltatás fiókjának konfigurálása](media/MIM-UpgFIM3.png)

7.  Ellenőrizze a biztonsági csoportok nevét, majd kattintson a **Tovább** gombra.

    ![Kép: A MIM Synchronization szolgáltatás biztonsági csoportjainak konfigurálása](media/MIM-UpgFIM4.png)

8.  Ne módosítsa a bejövő RPC-kommunikáció tűzfalszabályaira vonatkozó négyzet jelölését.

9. A telepítő ekkor készen áll a Sync szolgáltatás frissítésére a FIM 2010 R2-ről a MIM-alapú verzióra. A frissítési folyamat elindításához kattintson az **Upgrade** (Frissítés) gombra.

10. A frissítés folyamatban van. Amíg tart, ne zárja be a telepítőt és ne indítsa újra a számítógépet.

    ![Kép: A MIM Sync telepítési állapota](media/MIM-UpgFIM7.png)

11. A frissítés közben megjelenik egy figyelmeztetés a Sync-adatbázis frissítéséről. Előzetesen érdemes biztonsági mentést készíteni az adatbázisról.

12. Amikor a telepítés sikeresen befejeződött, kattintson a **Befejezés** gombra.

    ![Kép: A MIM Sync telepítése sikeresen befejeződött](media/MIM-UpgSP1.png)

13. A **Synchronization Service** szolgáltatás újraindult.

## <a name="upgrade-the-service-and-portal"></a>A szolgáltatás és a portál frissítése

1.  Jelentkezzen be rendszergazdaként egy kiszolgálóra, amelyen üzembe van helyezve a FIM 2010 R2 szolgáltatás és -portál.

2.  Nyissa meg a **Szolgáltatások** konzolt, keresse meg a **Forefront Identity Manager Service** szolgáltatást, majd állítsa le.

    ![Kép: A Szolgáltatások konzol](media/MIM-UpgFIM9.PNG)

3.  Indítsa el a MIM szolgáltatás és -portál telepítőjét. A folytatáshoz kattintson az **Install** (Telepítés) gombra.

    ![Kép: A MIM szolgáltatás és -portál telepítése](media/MIM-UpgSP2.png)

4.  Ha elfogadja a licencfeltételeket, kattintson a **Next** (Tovább) gombra a folytatáshoz.

5.  A MIM Customer Experience Improvement Program (A MIM Felhasználói élmény fokozása programja) képernyőn kattintson a **Next** (Tovább) gombra a folytatáshoz.

6.  Jelölje ki a MIM telepíteni kívánt szolgáltatásait és összetevőit, majd ha elkészült, kattintson a **Next** (Tovább) gombra.

    ![Kép: Egyéni telepítés](media/MIM-UpgSP4.png)

    1.  **MIM Service (MIM szolgáltatás)**: kötelezően telepítendő legalább egy kiszolgálón, és SQL Server adatbázis-kiszolgálót is igényel ugyanazon vagy másik számítógépen.

    2.  **MIM Portal (MIM-portál):** kötelezően telepítendő legalább egy kiszolgálón; előfeltétele a SharePoint 2013 Foundation telepítése.

    3.  **MIM Password Registration Portal (MIM jelszó-regisztrálási portál):** az önkiszolgáló jelszóváltoztatáshoz szükséges.

    4.  **MIM Password Reset Portal (MIM jelszó-változtatási portál):** a jelszóváltoztatáshoz szükséges.

7.  Adja meg a FIM szolgáltatás adatbázisához használt SQL Server adatait. Jelölje be a Re-use the existing database (A meglévő adatbázis újrafelhasználása) beállítást az adatok megőrzéséhez. A folytatáshoz kattintson a **Tovább** gombra.

8. Ha bejelölte a meglévő adatbázis újrafelhasználására vonatkozó beállítást, megjelenik egy emlékeztető az adatbázis biztonsági mentéséről.

9. Adja meg a levelezőkiszolgáló adatait. Ha a levelezőkiszolgáló az aktuális kiszolgálón található, helyként írja be, hogy „localhost”. A folytatáshoz kattintson a **Next** (Tovább) gombra.

    ![Kép: A levelezőkiszolgálóval való kapcsolat beállítása](media/MIM-UpgSP6.png)

10. Válasszon egy tanúsítványt, amellyel a szolgáltatás ellenőrizni fogja az ügyfeleket. Használja azt a meglévő tanúsítványt a helyi tanúsítványtárból, amelyet korábban a FIM szolgáltatás használt.

    ![Kép: A szolgáltatás tanúsítványának konfigurálása](media/MIM-UpgSP7.png)

    1.  Ha a helyi tanúsítványtárra vonatkozó beállítást választotta, kattintson a **Select Cert** (Tanúsítvány választása) gombra, és az előugró ablakban válasszon egy tanúsítványt a listából. Kattintson az **OK**, majd a **Tovább** gombra.

        ![Kép: A Select a Certificate (Tanúsítvány kiválasztása) előugró ablak](media/MIM-UpgSP8.PNG)

11. Állítsa be a MIM szolgáltatás fiókjának hitelesítő adatait. Vegye figyelembe, hogy ez a szolgáltatásfiók nem lehet azonos a Synchronization szolgáltatás által használt fiókkal. Ennek a fióknak ugyanannak kell lennie, mint amit a FIM szolgáltatás használt.

    ![Kép: A MIM szolgáltatás fiókjának konfigurálása](media/MIM-UpgSP9.png)

12. Állítsa be a MIM Sync-kiszolgáló adatait az előző lépésben üzembe helyezett MIM szolgáltatás konfigurációjának megfelelően.

    ![Kép: A MIM szolgáltatás és -portál szinkronizálásának konfigurálása](media/MIM-UpgSP10.png)

13. A MIM-portál telepítésekor adja meg a MIM szolgáltatás kiszolgálójának címét. Kattintson a **Tovább** gombra.

14. A MIM-portál telepítésekor adja meg annak a SharePoint-webhelycsoportnak az URL-címét, amelyben a FIM-portál jelenleg üzemel. Kattintson a **Tovább**gombra.

## <a name="install-the-mim-password-registration-portal"></a>A MIM jelszó-regisztrálási portál telepítése

1. A MIM jelszó-regisztrálási portál telepítésekor adja meg a kért URL-t a jelszó-regisztrációs portálhoz. Kattintson a **Tovább**gombra.

2. Állítsa be, hogy az ügyfelek és a végfelhasználók használhassák a szolgáltatást és a portált.

    1.  Ellenőrizze, hogy **meg kívánja-e nyitni az 5725-ös és az 5726-os portot a tűzfalon**.

    2.  Adja meg, hogy **hozzáférést ad-e a hitelesített felhasználóknak a MIM-portál webhelyéhez**.

    3.  Kattintson a **Tovább**gombra.

3. Adja meg a hozzáféréshez szükséges adatokat és hitelesítő adatokat a MIM jelszó-regisztrálási portálhoz.

    ![Kép: A MIM jelszó-regisztrálási portál konfigurálása](media/MIM-UpgSP15.png)

    1.  Adja meg a MIM jelszó-regisztrálási portálhoz használni kívánt szolgáltatásfiók nevét (tartománnyal együtt) és jelszavát.

    2.  Adja meg a jelszó-regisztrálási portál gazdagépének adatait: nevét és portját (például 8080).

    3.  Jelölje be az **Open port in firewall** (Port nyitása a tűzfalon) négyzetet.

    4.  Kattintson a **Tovább**gombra.

4. A MIM jelszó-regisztrálási portál következő konfigurációs képernyőjén:

    1.  Adja meg a MIM jelszó-regisztrálási szolgáltatásnak a telepített MIM szolgáltatás helyét. Ez általában az adott rendszer.

    2.  Állítsa be, hogy a portál az extranetes és intranetes felhasználók számára, vagy csak az intranetes felhasználók számára legyen-e elérhető a FIM jelszó-regisztrálási szolgáltatás korábbi konfigurációjának megfelelően.

## <a name="install-the-mim-password-reset-portal"></a>A MIM jelszó-változtatási portál telepítése

1. A MIM jelszó-változtatási portál telepítésekor adja meg a hozzáféréshez szükséges adatokat és hitelesítő adatokat.

    ![Kép: A MIM jelszó-változtatási portál konfigurálása](media/MIM-UpgSP17.png)

    1.  Adja meg a MIM jelszó-változtatási portálhoz használni kívánt szolgáltatásfiók nevét (tartománnyal együtt) és jelszavát.

    2.  Adja meg a jelszó-változtatási portál gazdagépének adatait: nevét és portját (például 8088).

    3.  Jelölje be az **Open port in firewall** (Port nyitása a tűzfalon) négyzetet.

    4.  Kattintson a **Tovább**gombra.

2. A MIM jelszó-változtatási portál következő konfigurációs képernyőjén:

    1.  Adja meg a MIM jelszó-változtatási szolgáltatásnak a telepített MIM szolgáltatás helyét.

    2.  Állítsa be, hogy a portál az extranetes és intranetes felhasználók számára, vagy csak az intranetes felhasználók számára legyen-e elérhető.

## <a name="finish-installation-and-upgrade"></a>A telepítés és frissítés befejezése

1. Ha az összes konfigurációs beállítást sikeresen megadta, megjelenik a telepítési oldal. Kattintson az **Install** (Telepítés) gombra a MIM szolgáltatás és -portál telepítésének és frissítésének megkezdéséhez.

2. A MIM szolgáltatás és -portál telepítése és frissítése folyamatban van. A telepítés közben ne zárja be a telepítőt és ne indítsa újra a számítógépet.

3. Ha a MIM szolgáltatás és -portál telepítése (frissítése) sikeresen befejeződött, megjelenik egy visszaigazoló képernyő. A telepítés befejezéséhez és a telepítőből való kilépéshez kattintson a **Finish** (Befejezés) gombra.

4. A **Forefront Identity Manager Service** szolgáltatás újraindult.

Megjegyzés: Ha a FIM beépülő moduljai és bővítményei jelenleg telepítve vannak a felhasználók számítógépein az önkiszolgáló jelszó-változtatási (SSPR) szolgáltatáshoz, abban az esetben az új MFA telefonos jelszó-változtatási kapuk konfigurálásával várjon addig, amíg a FIM valamennyi beépülő modulját és bővítményét frissítette a MIM 2016-os verzióra.  A FIM 2010 és a FIM 2010 R2 beépülő moduljai és bővítményei nem ismerik fel az új kapukat, így hibaüzenetet fognak megjeleníteni, a felhasználók pedig nem fogják tudni megváltoztatni a jelszavukat.

A Microsoft Identity Manager 2016 SP1 frissítési útmutatása a [Microsoft Identity Manager 2016 Service Pack 1 frissítési csomagban található](https://blogs.technet.microsoft.com/iamsupport/2016/11/08/microsoft-identity-manager-2016-service-pack-1-update-package/)

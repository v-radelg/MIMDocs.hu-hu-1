---
title: A Microsoft Identity Manager szolgáltatás és -portál telepítése | Microsoft Docs
description: Itt olvashatók a Microsoft Identity Manager 2016 rendszerhez tartozó MIM szolgáltatás és -portál telepítési és konfigurálási lépései.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ca2a9a4a646387b044e3a504c19eae904b2a6be1
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701288"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>A (z) 2016-es telepítése: MIM szolgáltatás és -portál

> [!div class="step-by-step"]
> [« MIM Synchronization Service](install-mim-sync.md)
> [Adatbázisok szinkronizálása »](install-mim-sync-ad-service.md)
> 
> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó – <strong>Pass@word1</strong>
> - Szolgáltatásfiók neve – **MIMService**

Ha a legutóbbi lépésben nem telepítette a MIM telepítőcsomagját, akkor a folytatás előtt lépjen vissza és telepítse a Microsoft Identity Manager 2016 összetevőit.


## <a name="configure-mim-service-and-portal-for-installation"></a>A MIM szolgáltatás és -portál konfigurálása a telepítéshez

1. Futtassa a **MIM szolgáltatás és -portál telepítőjét** a kicsomagolt **Service and Portal** almappából.

2. Az üdvözlőképernyőn kattintson a **Next** (Tovább) gombra.

3. Olvassa el a végfelhasználói licencszerződést, és ha elfogadja, kattintson a **Next** (Tovább) gombra.

4. A **MIM Customer Experience Improvement Program** (A MIM Felhasználói élmény fokozása programja) képernyőn kattintson a **Next** (Tovább) gombra a folytatáshoz.

5. A telepíteni kívánt összetevők közül mindenképpen jelölje ki a MIM Service szolgáltatást (a MIM Reporting nélkül), valamint a MIM Portal (MIM-portál) komponenst. Igény szerint kiválaszthatja a MIM Password Reset Portal jelszó-változtatási portált és a MIM Password Change Notification Service jelszóváltozás-értesítési szolgáltatást is.

6. A **Configure the MIM database connection** (A MIM-adatbázis kapcsolatának konfigurálása) lapon válassza a **Create a new database** (Új adatbázis létrehozása) beállítást.

    ![Kép: A MIM-adatbázis kapcsolatának konfigurálása](media/install-mim-service-portal/MIM_Install10.png)

7. A **levelezési kiszolgáló kapcsolatainak konfigurálása**területen adja meg az Exchange-kiszolgáló nevét **levelezési kiszolgálóként** , vagy használhatja a **O365**-postaládát. Ha nincs levelezőkiszolgáló konfigurálva, akkor a **localhost** nevet adja meg, és törölje a felső két négyzet jelölését. Kattintson a **Tovább** gombra.

    ![Kép: A levelezőkiszolgálóval való kapcsolat beállítása](media/install-mim-service-portal/MIM_Install11.png)

8. Adja meg, hogy új önaláírt tanúsítványt szeretne-e generálni, vagy válassza ki a megfelelő tanúsítványt.

9. A Service Account Name mezőben adja meg a használni kívánt szolgáltatásfiók nevét – például *MIMService* –, a Service Account Password mezőben a szolgáltatásfiók jelszavát – például <em>Pass@word1</em>, a Service Account Domain mezőben a szolgáltatásfiók tartományát – például *contoso*, a Service Email Account mezőben pedig az e-mail fiókot, például *contoso*.

    ![Kép: A MIM szolgáltatás fiókjának konfigurálása](media/install-mim-service-portal/MIM_Install12.png)

10. Elképzelhető, hogy megjelenik egy üzenet, amely arra figyelmeztet, hogy a szolgáltatásfiók aktuális konfigurációja nem biztonságos.

11. Fogadja el az alapértelmezett értékeket a szinkronizációs kiszolgáló helyéhez, és a *contoso\MIMMA*.

    ![Kép: A MIM szolgáltatás és -portál konfigurálása](media/install-mim-service-portal/MIM_Install13.png)

12. A MIM portálhoz tartozó MIM-szolgáltatás kiszolgálójának címeként adja meg a *CORPIDM* nevet (az adott számítógép nevét).

13. A `* http://mim.contoso.com*` SharePoint-webhelycsoport URL-címének megadása.

14. A `* http://passwordregistration.contoso.com*` jelszó-regisztrálási URL-cím 80-as portját kell megadni, amely később a 443-es SSL-tanúsítvány frissítését javasolja.

15. A `* http://passwordreset.contoso.com*` jelszó-visszaállítási URL-cím 80-as portjának megadása, a frissítés későbbi frissítése a 443-es SSL-tanúsítvány használatával.

16. Jelölje be a tűzfalon az 5725-ös és 5726-os portok megnyitására szolgáló négyzetet, valamint azt, amelyik az összes hitelesített felhasználónak hozzáférést biztosít a MIM-portálhoz.

## <a name="configure-mim-password-registration-portal"></a>A MIM jelszó-regisztrálási portál konfigurálása

1. Az önkiszolgáló jelszó-regisztrálási szolgáltatáshoz (SSPR) állítsa be a *contoso\MIMSSPR* fióknevet és a <em>Pass@word1</em> jelszót.

2. Adja meg az *passwordregistration.contoso.com* -t a rendszerjelszó-regisztrációhoz használt állomásnévként, és állítsa a portot **80**-re. Jelölje be az **Open port in firewall** (Port nyitása a tűzfalon) négyzetet.

   ![Kép: Az IIS által használt konfigurációs információk megadása](media/install-mim-service-portal/MIM_Install14.png)

3. Megjelenik egy figyelmeztető üzenet – olvassa el, majd kattintson a **Next** (Tovább) gombra.

4. A következő rendszerjelszó-regisztrációs portál konfigurációs képernyőjén az *MIM.contoso.com* -t a jelszó-regisztrálási portálhoz tartozó fakiszolgálói szolgáltatási kiszolgáló címeként adhatja meg.

## <a name="configure-mim-password-reset-portal"></a>A MIM jelszó-változtatási portál konfigurálása

1. Állítsa be a szolgáltatásfiók nevét a SSPR-regisztrációhoz a *contoso\mimsspr fióknevet* és a <em>Pass@word1</em>jelszavához.

2. Adja meg az *PasswordReset.contoso.com* -t a rendszerállapot-jelszó-visszaállítási portál állomásneveként, és állítsa a portot **80**-re. Jelölje be az **Open port in firewall** (Port nyitása a tűzfalon) négyzetet.

   ![Kép: Az IIS által használt konfigurációs információk megadása](media/install-mim-service-portal/MIM_Install15.png)

3. Megjelenik egy figyelmeztető üzenet – olvassa el, majd kattintson a **Next** (Tovább) gombra.

4. A jelszó-változtatási portál következő konfigurációs képernyőjén a rendszerállapot-nyilvántartási portálon a *MIM.contoso.com* megadásával válassza ki a jelszó kérése szolgáltatás kiszolgálójának címe elemet.

## <a name="install-mim-service-and-portal"></a>A MIM szolgáltatás és -portál telepítése

Ha végzett a telepítés előtti teendőkkel, kattintson az **Install** (Telepítés) gombra a **szolgáltatás és portál** kijelölt összetevőinek telepítéséhez.

A telepítést követően győződjön meg arról, hogy a MIM-portál aktív.

1. Indítsa el az Internet Explorert, és kapcsolódjon a MIM-portál a *http://mim.contoso.com/identitymanagement* . Az oldal első látogatásakor némi késedelem lehet tapasztalható.

    - Ha szükséges, hitelesítse magát *contoso\miminstall* az Internet Explorerben.

2. Az Internet Explorerben nyissa meg az **Internetbeállításokat**, lépjen a **Biztonság** lapra, és ha még nem szerepel ott, vegye fel a webhelyet a **Helyi intranet** zónába.  Zárja be az **Internetbeállítások** párbeszédpanelt.

3. Engedélyezze a felhasználóknak saját bejegyzéseik megtekintését a MIM-ben.

    1.  Az Internet Explorerben a **MIM-portálon** kattintson a **Management Policy Rules** (Felügyeleti házirendszabályok) elemre.

    2.  A felügyeleti házirend szabályának keresése, **felhasználói kezelés: A felhasználók saját maguk is olvashatják**a saját attribútumaikat.

    3.  Jelölje ki ezt a felügyeleti házirendszabályt, majd törölje a **Policy is disabled** (A házirend le van tiltva) négyzet jelölését.

    4.  Kattintson az **OK**, majd a **Submit** (Küldés) gombra.

4.  Ellenőrizze, hogy a tűzfal engedélyezi-e a bejövő kapcsolatokat az 5725-ös és 5726-os TCP-portokon.

    1.  Nyissa meg a **Felügyeleti eszközöket**, majd válassza a **Fokozott biztonságú Windows tűzfal** lehetőséget.

    2.  Kattintson a **Bejövő szabályok** elemre.

    3.  Győződjön meg arról, hogy a következő két szabály szerepel a listában:

        -   Forefront Identity Manager Service (STS).

        -   Forefront Identity Manager Service (Webservice).

    4.  Végezze el a varázsló lépéseit, majd zárja be a **Windows tűzfalat**.

    5.  Válassza a **Vezérlőpult » Hálózat és internet » Hálózati állapot és hálózati feladatok megjelenítése** elemet.

    6.  Győződjön meg arról, hogy a listában szerepel egy contoso.local nevű aktív hálózat, tartományi hálózatként.

    7.  Zárja be a **Vezérlőpultot**.

> [!NOTE]
> Nem kötelező: Ezen a ponton telepítheti a fakiszolgáló beépülő modulokat és bővítményeket.
> 
> [!div class="step-by-step"]  
> [« MIM Synchronization Service](install-mim-sync.md)
> [Adatbázisok szinkronizálása »](install-mim-sync-ad-service.md)

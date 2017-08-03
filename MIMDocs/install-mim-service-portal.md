---
title: "A Microsoft Identity Manager szolgáltatás és -portál telepítése | Microsoft Docs"
description: "Itt olvashatók a Microsoft Identity Manager 2016 rendszerhez tartozó MIM szolgáltatás és -portál telepítési és konfigurálási lépései."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 974015bbba3a36e1107da33655eedf94e2938582
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="install-mim-2016-mim-service-and-portal"></a>A MIM 2016 telepítése: A MIM szolgáltatás és -portál

>[!div class="step-by-step"]
[« MIM Synchronization Service](install-mim-sync.md)
[Adatbázisok szinkronizálása »](install-mim-sync-ad-service.md)

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó – **Pass@word1**
> - Szolgáltatásfiók neve – **MIMService**

Ha a legutóbbi lépésben nem telepítette a MIM telepítőcsomagját, akkor a folytatás előtt lépjen vissza és telepítse a Microsoft Identity Manager 2016 összetevőit.


## <a name="configure-mim-service-and-portal-for-installation"></a>A MIM szolgáltatás és -portál konfigurálása a telepítéshez

1. Futtassa a **MIM szolgáltatás és -portál telepítőjét** a kicsomagolt **Service and Portal** almappából.

2. Az üdvözlőképernyőn kattintson a **Next** (Tovább) gombra.

3. Olvassa el a végfelhasználói licencszerződést, és ha elfogadja, kattintson a **Next** (Tovább) gombra.

4. A **MIM Customer Experience Improvement Program** (A MIM Felhasználói élmény fokozása programja) képernyőn kattintson a **Next** (Tovább) gombra a folytatáshoz.

5. A telepíteni kívánt összetevők közül mindenképpen jelölje ki a MIM Service szolgáltatást (a MIM Reporting nélkül), valamint a MIM Portal (MIM-portál) komponenst. Igény szerint kiválaszthatja a MIM Password Reset Portal jelszó-változtatási portált és a MIM Password Change Notification Service jelszóváltozás-értesítési szolgáltatást is.

6. A **Configure the MIM database connection** (A MIM-adatbázis kapcsolatának konfigurálása) lapon válassza a **Create a new database** (Új adatbázis létrehozása) beállítást.

    ![Kép: A MIM-adatbázis kapcsolatának konfigurálása](media/MIM-Install10.png)

7. A **Configure mail server connection** (Levelezőkiszolgáló-kapcsolat konfigurálása) párbeszédpanelen a **Mail Server** (Levelezőkiszolgáló) mezőben adja meg az Exchange Server-kiszolgáló nevét. Ha nincs levelezőkiszolgáló konfigurálva, akkor a **localhost** nevet adja meg, és törölje a felső két négyzet jelölését. Kattintson a **Tovább**gombra.

    ![Kép: A levelezőkiszolgálóval való kapcsolat beállítása](media/MIM-Install11.png)

8. Adja meg, hogy új önaláírt tanúsítványt szeretne-e generálni, vagy válassza ki a megfelelő tanúsítványt.

9. A Service Account Name mezőben adja meg a használni kívánt szolgáltatásfiók nevét – például *MIMService* –, a Service Account Password mezőben a szolgáltatásfiók jelszavát – például *Pass@word1*, a Service Account Domain mezőben a szolgáltatásfiók tartományát – például *contoso*, a Service Email Account mezőben pedig az e-mail fiókot, például *contoso*.

    ![Kép: A MIM szolgáltatás fiókjának konfigurálása](media/MIM-Install12.png)

10. Elképzelhető, hogy megjelenik egy üzenet, amely arra figyelmeztet, hogy a szolgáltatásfiók aktuális konfigurációja nem biztonságos.

11. A Synchronization Server (Szinkronizálási kiszolgáló) helyeként fogadja el az alapértelmezett értéket, a MIM Management Agent Account (MIM-kezelőügynök fiókja) mezőben pedig adja meg a *contoso\MIMsync* fiókot.

    ![Kép: A MIM szolgáltatás és -portál konfigurálása](media/MIM-Install13.png)

12. A MIM portálhoz tartozó MIM-szolgáltatás kiszolgálójának címeként adja meg a *CORPIDM* nevet (az adott számítógép nevét).

13. A SharePoint-webhelycsoport URL-címeként adja meg a következőt: *http://CorpIDM.contoso.local:82*.

14. A Password Registration jelszó-regisztrálási portál URL-címeként adja meg a következőt: *http://CorpIDM.contoso.local:8080*.

15. A Password Reset jelszó-változtatási szolgáltatás URL-címeként adja meg a következőt: *http://CorpIDM.contoso.local:8088*.

16. Jelölje be a tűzfalon az 5725-ös és 5726-os portok megnyitására szolgáló négyzetet, valamint azt, amelyik az összes hitelesített felhasználónak hozzáférést biztosít a MIM-portálhoz.

## <a name="configure-mim-password-registration-portal"></a>A MIM jelszó-regisztrálási portál konfigurálása

1.  Az önkiszolgáló jelszó-regisztrálási szolgáltatáshoz (SSPR) állítsa be a *contoso\MIMSSPR* fióknevet és a *Pass@word1* jelszót.

2.  A MIM jelszó-regisztráláshoz a Host Name mezőben adja meg a *CORPIDM* gazdagépnevet, és állítsa be a **8080**-as portot. Jelölje be az **Open port in firewall** (Port nyitása a tűzfalon) négyzetet.

    ![Kép: Az IIS által használt konfigurációs információk megadása](media/MIM-Install14.png)

3.  Megjelenik egy figyelmeztető üzenet – olvassa el, majd kattintson a **Next** (Tovább) gombra.

4. A MIM jelszó-regisztrálási portál következő konfigurációs képernyőjén a MIM szolgáltatás kiszolgálójának címeként adja meg a *http://CorpIDM.contoso.local* címet a jelszó-regisztrálási portálhoz.

## <a name="configure-mim-password-reset-portal"></a>A MIM jelszó-változtatási portál konfigurálása

1.  Az önkiszolgáló jelszó-regisztrálási szolgáltatáshoz (SSPR) állítsa be a *Contoso\MIMSSPRService* fióknevet és a *Pass@word1* jelszót.

2.  A MIM jelszó-változtatási portál Host Name értékeként adja meg a *CORPIDM* gazdagépnevet, és állítsa be a **8088**-as portot. Jelölje be az **Open port in firewall** (Port nyitása a tűzfalon) négyzetet.

    ![Kép: Az IIS által használt konfigurációs információk megadása](media/MIM-Install15.png)

3.  Megjelenik egy figyelmeztető üzenet – olvassa el, majd kattintson a **Next** (Tovább) gombra.

4. A MIM jelszó-regisztrálási portál következő konfigurációs képernyőjén a MIM szolgáltatás kiszolgálójának címeként adja meg a *http://CorpIDname.domain.local* címet a jelszó-változtatási portálhoz.

## <a name="install-mim-service-and-portal"></a>A MIM szolgáltatás és -portál telepítése

Ha végzett a telepítés előtti teendőkkel, kattintson az **Install** (Telepítés) gombra a **szolgáltatás és portál** kijelölt összetevőinek telepítéséhez.

A telepítést követően győződjön meg arról, hogy a MIM-portál aktív.

1. Indítsa el az Internet Explorert, és kapcsolódjon a MIM-portálhoz a következő címen: *http://corpidm.contoso.local:82/identitymanagement*. Az oldal első látogatásakor némi késedelem lehet tapasztalható.

    - Szükség esetén hitelesítse magát a *contoso\Rendszergazda* fiókkal az Internet Explorerben.

2. Az Internet Explorerben nyissa meg az **Internetbeállításokat**, lépjen a **Biztonság** lapra, és ha még nem szerepel ott, vegye fel a webhelyet a **Helyi intranet** zónába.  Zárja be az **Internetbeállítások** párbeszédpanelt.

3. Engedélyezze a felhasználóknak saját bejegyzéseik megtekintését a MIM-ben.

    1.  Az Internet Explorerben a **MIM-portálon** kattintson a **Management Policy Rules** (Felügyeleti házirendszabályok) elemre.

    2.  Keresse meg a **User management: Users can read attributes of their own** (Felhasználók felügyelete: A felhasználók olvashatják a saját attribútumaikat) felügyeleti házirendszabályt.

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
> Nem kötelező: Ezek után igény szerint telepítheti a MIM beépülő moduljait és bővítményeit.

>[!div class="step-by-step"]  
[« MIM Synchronization Service](install-mim-sync.md)
[Adatbázisok szinkronizálása »](install-mim-sync-ad-service.md)

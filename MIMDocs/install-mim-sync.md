---
title: A Microsoft Identity Manager Sync Service telepítése | Microsoft Docs
description: Első lépések a MIM 2016 összetevői kapcsán – a Synchronization Service telepítése és konfigurálása
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: b9bc580710a13b3a0868b0580d55469a753e7f33
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568086"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>A MIM 2016 telepítése: A MIM Synchronization Service

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [MIM szolgáltatás és -portál »](install-mim-service-portal.md)
 
> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **corpdc**
> - Tartománynév – **contoso**
> - **Corpservice** -kiszolgáló neve
> - **Corpsync** -szinkronizálási kiszolgáló neve
> - SQL Server neve – **corpsql**
> - Jelszó – <strong>Pass@word1</strong>

A Microsoft Identity Manager 2016 összetevőinek telepítéséhez először készítse elő a telepítési csomagot.

1. Jelentkezzen be *contoso\miminstall* -ként arra a kiszolgálóra, amelyet az Identity Management szinkronizálás Server **corpsync**használ.

2. Bontsa ki a MIM telepítési csomagját vagy csatlakoztassa a MIM DVD-lemezképét.  Ha nem rendelkezik ezzel a DVD-vel, tekintse meg a [Microsoft Identity Manager licencelés és letöltések](microsoft-identity-manager-licensing.md)című témakört.

## <a name="install-mim-2016-sp1-synchronization-service"></a>A 2016 SP1 szinkronizációs szolgáltatás telepítése

1. A kibontott MIM telepítési mappában nyissa meg a **Synchronization Service** mappát.

2. Indítsa el a **MIM Synchronization Service telepítőjét**. Az útmutatást követve végezze el a telepítést.

3. Az üdvözlőképernyőn kattintson a **Next** (Tovább) gombra.

    ![Kép: A MIM-telepítővarázsló üdvözlőképernyője](media/install-mim-sync/MIM_Install1.png)

4. Olvassa el és a **Next** (Tovább) gombbal fogadja el a licencfeltételeket.

5. A **Custom Setup** (Egyéni telepítés) képernyőn kattintson a **Next** (Tovább) gombra.

    ![Kép: Egyéni telepítés](media/install-mim-sync/MIM_Install2.png)

6. A Sync Service adatbázis-konfigurálási képernyőjén válassza a következő beállításokat:

   1.  A SQL Server a következő helyen található: **corpsql.contoso.com**nevű **távoli gép** .

   2.  The SQL Server instance is (SQL Server-példány): **The default instance** (Az alapértelmezett példány)

   ![Kép: Csatlakozás adatbázishoz](media/install-mim-sync/MIM_Install3.png)

    3. Webkiszolgáló *2016 SP2 és újabb verziók*: a rendszerkiszolgálói szinkronizációs szolgáltatás adatbázisának nevének konfigurálása

7. A korábban létrehozott fióknak megfelelően állítsa be a Sync szolgáltatásfiókját:

   1. Service account (Szolgáltatásfiók): *MIMSync*

   2. Jelszó: <em>Pass@word1</em>

   3. Service Account Domain or local computer name (Szolgáltatásfiók tartománya vagy helyi számítógép neve): *contoso*

    >[!NOTE]
    >Felügyeleti csomag 2016 SP2 és újabb verziók: csoportosan felügyelt szolgáltatásfiókok esetén győződjön meg arról, hogy a **$** karakter a szolgáltatásfiók neve végén található, például MIMSync $, és hagyja üresen a jelszó mezőt.

    ![Kép: Szolgáltatásfiók](media/install-mim-sync/MIM_Install4.png)

8. Adja meg a MIM Sync Service telepítőjében a megfelelő biztonsági csoportokat:

   1. Administrator (Rendszergazda) = *contoso\MIMSyncAdmins*

   2. Operator (Operátor) = *contoso\MIMSyncOperators*

   3. Joiner (Csatlakozó) = *contoso\MIMSyncJoiners*

   4. Connector Browse (Összekötő-tallózó) = *contoso\MIMSyncBrowse*

   5. WMI Password Management (WMI-jelszókezelés) = *contoso\MIMSyncPasswordReset*

   ![Kép: Biztonsági csoportok](media/install-mim-sync/MIM_Install5.png)

9. A biztonsági beállítások képernyőjén jelölje be az **Enable firewall rules for inbound RPC communications** (Tűzfalszabályok engedélyezése a bejövő RPC-kommunikációhoz) négyzetet, majd kattintson a **Next** (Tovább) gombra.

10. A MIM Sync Service telepítésének elindításához kattintson az **Install** (Telepítés) gombra.

    1. Elképzelhető, hogy megjelenik egy figyelmeztetés a MIM Sync szolgáltatásfiókjáról – ebben az esetben kattintson az **OK** gombra.

    2. A MIM Sync Service telepítése megkezdődik.

    3. Megjelenik egy üzenet arról, hogy készítsen biztonsági másolatot a titkosítási kulcsról. Kattintson az **OK** gombra, majd válasszon egy mappát a titkosítási kulcs biztonsági másolatának tárolásához.

    ![Kép: Üzenet a MIM Sync titkosítási kulcsának biztonsági mentéséről](media/MIM-Install7.png)

    4. Ha a telepítés sikeresen befejeződött, kattintson a **Finish** (Befejezés) gombra.

    5. A csoporttagsági változások életbelépéséhez ki kell jelentkeznie, majd újra be kell jelentkeznie. A kijelentkezéshez kattintson a **Yes** (Igen) gombra.

> [!div class="step-by-step"]  
> [« Exchange Server](prepare-server-exchange.md)
> [MIM szolgáltatás és -portál »](install-mim-service-portal.md)

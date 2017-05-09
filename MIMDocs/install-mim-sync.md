---
title: "A Microsoft Identity Manager Sync Service telepítése | Microsoft Docs"
description: "Első lépések a MIM 2016 összetevői kapcsán – a Synchronization Service telepítése és konfigurálása"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 54d03fbd03f6c44298139324ea2dc7d945f008bc
ms.openlocfilehash: c6cf0c93679319716c34904ea6239902010e0860
ms.lasthandoff: 01/24/2017


---

# <a name="install-mim-2016-mim-synchronization-service"></a>A MIM 2016 telepítése: A MIM Synchronization Service

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[MIM szolgáltatás és -portál »](install-mim-service-portal.md)

> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Példa:
> - Tartományvezérlő neve – **mimservername**
> - Tartománynév – **contoso**
> - Jelszó – **Pass@word1**

A Microsoft Identity Manager 2016 összetevőinek telepítéséhez először készítse elő a telepítési csomagot.

1. Jelentkezzen be az identitáskezeléshez használt kiszolgálóra a *contoso\Rendszergazda* fiókkal.

2. Bontsa ki a MIM telepítési csomagját vagy csatlakoztassa a MIM DVD-lemezképét.

## <a name="install-mim-2016-synchronization-service"></a>A MIM 2016 Synchronization Service telepítése

1. A kibontott MIM telepítési mappában nyissa meg a **Synchronization Service** mappát.

2. Indítsa el a **MIM Synchronization Service telepítőjét**. Az útmutatást követve végezze el a telepítést.

3. Az üdvözlőképernyőn kattintson a **Next** (Tovább) gombra.

    ![Kép: A MIM-telepítővarázsló üdvözlőképernyője](media/MIM-Install1.png)

4. Olvassa el és a **Next** (Tovább) gombbal fogadja el a licencfeltételeket.

5. A **Custom Setup** (Egyéni telepítés) képernyőn kattintson a **Next** (Tovább) gombra.

    ![Kép: Egyéni telepítés](media/MIM-Install2.png)

6.  A Sync Service adatbázis-konfigurálási képernyőjén válassza a következő beállításokat:

    1.  SQL Server is located on (SQL Server helye):**This computer** (Ez a számítógép).

    2.  The SQL Server instance is (SQL Server-példány): **The default instance** (Az alapértelmezett példány).

    ![Kép: Csatlakozás adatbázishoz](media/MIM-Install3.png)

7.  A korábban létrehozott fióknak megfelelően állítsa be a Sync szolgáltatásfiókját:

    1.  Service account (Szolgáltatásfiók): *MIMSync*

    2.  Jelszó: *Pass@word1*

    3.  Service Account Domain or local computer name (Szolgáltatásfiók tartománya vagy helyi számítógép neve): *contoso*

    ![Kép: Szolgáltatásfiók](media/MIM-Install4.png)

8.  Adja meg a MIM Sync Service telepítőjében a megfelelő biztonsági csoportokat:

    1. Administrator (Rendszergazda) = *contoso\MIMSyncAdmins*

    2. Operator (Operátor) = *contoso\MIMSyncOperators*

    3. Joiner (Csatlakozó) = *contoso\MIMSyncJoiners*

    4. Connector Browse (Összekötő-tallózó) = *contoso\MIMSyncBrowse*

    5. WMI Password Management (WMI-jelszókezelés) = *contoso\MIMSyncPasswordReset*

    ![Kép: Biztonsági csoportok](media/MIM-Install5.png)

9. A biztonsági beállítások képernyőjén jelölje be az **Enable firewall rules for inbound RPC communications** (Tűzfalszabályok engedélyezése a bejövő RPC-kommunikációhoz) négyzetet, majd kattintson a **Next** (Tovább) gombra.

10. A MIM Sync Service telepítésének elindításához kattintson az **Install** (Telepítés) gombra.

    1. Elképzelhető, hogy megjelenik egy figyelmeztetés a MIM Sync szolgáltatásfiókjáról – ebben az esetben kattintson az **OK** gombra.

    2. A MIM Sync Service telepítése megkezdődik.

    3. Megjelenik egy üzenet arról, hogy készítsen biztonsági másolatot a titkosítási kulcsról. Kattintson az **OK** gombra, majd válasszon egy mappát a titkosítási kulcs biztonsági másolatának tárolásához.

        ![Kép: Üzenet a MIM Sync titkosítási kulcsának biztonsági mentéséről](media/MIM-Install7.png)

    4. Ha a telepítés sikeresen befejeződött, kattintson a **Finish** (Befejezés) gombra.

    5. A csoporttagsági változások életbelépéséhez ki kell jelentkeznie, majd újra be kell jelentkeznie. A kijelentkezéshez kattintson a **Yes** (Igen) gombra.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[MIM szolgáltatás és -portál »](install-mim-service-portal.md)


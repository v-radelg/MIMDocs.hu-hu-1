---
title: A Microsoft Identity Manager Sync Service telepítése | Microsoft Docs
description: Első lépések a MIM 2016 összetevői kapcsán – a Synchronization Service telepítése és konfigurálása
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/01/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1bc313a66776778fe3617969645076a6d2d10418
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49357836"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>A MIM 2016 telepítése: A MIM Synchronization Service

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [MIM szolgáltatás és -portál »](install-mim-service-portal.md)
> 
> [!NOTE]
> Ez az útmutató egy Contoso nevű fiktív vállalat neveit és értékeit használja szemléltetésként. Ezeket helyettesítse a saját neveivel és értékeivel. Például:
> - Tartományvezérlő neve – **corpdc-re**
> - Tartománynév – **contoso**
> - MIM szolgáltatás kiszolgálójának neve – **corpservice**
> - MIM Sync-kiszolgáló neve – **corpsync**
> - Az SQL Server neve – **corpsql**
> - Jelszó – <strong>Pass@word1</strong>

A Microsoft Identity Manager 2016 összetevőinek telepítéséhez először készítse elő a telepítési csomagot.

1. Jelentkezzen be, *contoso\miminstall* identitáskezelési szinkronizálás kiszolgáló használ a kiszolgálóhoz **corpsync**.

2. Bontsa ki a MIM telepítési csomagját vagy csatlakoztassa a MIM DVD-lemezképét.

## <a name="install-mim-2016-sp1-synchronization-service"></a>A MIM 2016 SP1 Synchronization Service telepítése

1. A kibontott MIM telepítési mappában nyissa meg a **Synchronization Service** mappát.

2. Indítsa el a **MIM Synchronization Service telepítőjét**. Az útmutatást követve végezze el a telepítést.

3. Az üdvözlőképernyőn kattintson a **Next** (Tovább) gombra.

    ![Kép: A MIM-telepítővarázsló üdvözlőképernyője](media/install-mim-sync/MIM_Install1.png)

4. Olvassa el és a **Next** (Tovább) gombbal fogadja el a licencfeltételeket.

5. A **Custom Setup** (Egyéni telepítés) képernyőn kattintson a **Next** (Tovább) gombra.

    ![Kép: Egyéni telepítés](media/install-mim-sync/MIM_Install2.png)

6. A Sync Service adatbázis-konfigurálási képernyőjén válassza a következő beállításokat:

   1.  Az SQL Server található: **egy távoli gép** nevű **corpsql.contoso.com**.

   2.  Az SQL Server-példány: **az alapértelmezett példány**

   ![Kép: Csatlakozás adatbázishoz](media/install-mim-sync/MIM_Install3.png)

7. A korábban létrehozott fióknak megfelelően állítsa be a Sync szolgáltatásfiókját:

   1. Service account (Szolgáltatásfiók): *MIMSync*

   2. Jelszó: <em>Pass@word1</em>

   3. Service Account Domain or local computer name (Szolgáltatásfiók tartománya vagy helyi számítógép neve): *contoso*

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

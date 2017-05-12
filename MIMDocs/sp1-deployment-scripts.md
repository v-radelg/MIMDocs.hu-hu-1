---
title: "MIM2016 SP1 PAM üzembehelyezési szkriptek"
description: "Ez az oldal a Privileged Identity Manager parancsfájlokkal történő konfigurálást ismertető cikksorozat tagja. Tartalma a környezettel kapcsolatos előfeltételeket ismerteti."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 12c60e12dc5662ff0313e21bb9180b3709969af6
ms.contentlocale: hu-hu
ms.lasthandoff: 05/09/2017


---

# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM üzembehelyezési szkriptek

Ebben a szervizcsomagban elérhetővé tesszük az üzembehelyezési szkriptek egy csoportját, amelyek megkönnyítik a PAM üzembe helyezését. A szkriptek a letöltőközpontból érhetők el. Mielőtt elkezdené használni a szkripteket, fontos ellenőriznie, hogy az alábbi előfeltételek érvényesek-e a környezetére.

Lényegi előfeltételezések:
1. A minimális követelmény minden gépen a Windows Server 2012 R2 megléte. Amennyiben a Windows Server 2016 Technical Preview 5-ös verzióját teszteli, a PRIV-tartományvezérlőnek telepítve kell lennie a TP5 builden.
2. A DNS-t úgy kell konfigurálni, hogy a névfeloldás a tartományvezérlők és az összetevő-kiszolgálók közt automatikus legyen.
3. A telepítő bináris fájloknak helyben elérhetőnek kell lenniük a kijelölt kiszolgálókon az SQL, a SharePoint és a MIM telepítéséhez.
4. A környezet három dedikált (fizikai vagy virtuális) gépet tartalmaz, amelyeken külön fut a CORPDC, a PRIVDC és a PAMSERVER.
5. Az érvényesítési lehetőség esetében a rendszer feltételezi, hogy létezik egy dedikált ügyfélgép ennek a lépésnek a végrehajtására.

>[!NOTE]
>Amennyiben bármi probléma lépne a szkriptek végrehajtása során, érdemes áttekintenie a naplókat. A szkriptek naplói mind az %AppData%\MIMPAMInstall helyen vannak tárolva. Tömörítse a mappát egy Zip-fájlba, és küldje el e-mailben a mim2016@microsoft.com címre a művelet és a hiba részleteivel együtt.

Készen áll a PAM üzembehelyezési szkriptek használatára? Kezdje [A PAM konfigurálása szkriptek használatával](./pam/sp1-pam-configure-using-scripts.md) lépéssel.


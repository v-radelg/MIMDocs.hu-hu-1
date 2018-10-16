---
title: MIM2016 SP1 PAM üzembehelyezési szkriptek
description: Ez az oldal a Privileged Identity Manager parancsfájlokkal történő konfigurálást ismertető cikksorozat tagja. Tartalma a környezettel kapcsolatos előfeltételeket ismerteti.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: d9ed4414a3c4087b3ca7ec3709893ba1f8814d0a
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333190"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM üzembehelyezési szkriptek

Ebben a szervizcsomagban elérhetővé tesszük az üzembehelyezési szkriptek egy csoportját, amelyek megkönnyítik a PAM üzembe helyezését. A szkriptek a letöltőközpontból érhetők el. Mielőtt elkezdené használni a parancsprogramok fontos meggyőződni arról, hogy megfeleljen az alábbi követelményeknek:

1. Az operációs rendszer összes kiszolgálón legalább a Windows Server 2012 R2.
2. DNS engedélyezése a névfeloldás a tartományvezérlők és az összetevők kiszolgálóin között kell konfigurálni.
3. A telepítő bináris fájloknak helyben elérhetőnek kell lenniük a kijelölt kiszolgálókon az SQL, a SharePoint és a MIM telepítéséhez.
4. A környezet három dedikált (fizikai vagy virtuális) gépet tartalmaz, amelyeken külön fut a CORPDC, a PRIVDC és a PAMSERVER.
5. Az érvényesítési lehetőség esetében szüksége lesz egy dedikált munkaállomást.

>[!NOTE]
>Amennyiben bármi probléma lépne a szkriptek végrehajtása során, érdemes áttekintenie a naplókat. A szkriptek naplói mind az %AppData%\MIMPAMInstall helyen vannak tárolva. Tömörítse a mappát egy Zip-fájlba, és küldje el a művelet és a hiba részleteivel együtt a támogatási eset részeként.

Készen áll a PAM üzembehelyezési szkriptek használatára? Kezdje [A PAM konfigurálása szkriptek használatával](./pam/sp1-pam-configure-using-scripts.md) lépéssel.

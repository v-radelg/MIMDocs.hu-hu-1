---
title: MIM2016 SP1 PAM üzembehelyezési szkriptek
description: Ez az oldal a Privileged Identity Manager parancsfájlokkal történő konfigurálást ismertető cikksorozat tagja. Tartalma a környezettel kapcsolatos előfeltételeket ismerteti.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: eeee6473f7471d4c961a4f4d3113d1af73ddaffe
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044412"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM üzembehelyezési szkriptek

Ebben a szervizcsomagban elérhetővé tesszük az üzembehelyezési szkriptek egy csoportját, amelyek megkönnyítik a PAM üzembe helyezését. A szkriptek a letöltőközpontból érhetők el. A parancsfájlok használatának megkísérlése előtt fontos, hogy megfeleljen az alábbi követelményeknek:

1. Az operációs rendszer minden kiszolgálón legalább Windows Server 2012 R2.
2. A DNS-t úgy kell konfigurálni, hogy engedélyezze a névfeloldást a tartományvezérlők és az összetevő-kiszolgálók között.
3. A telepítő bináris fájloknak helyben elérhetőnek kell lenniük a kijelölt kiszolgálókon az SQL, a SharePoint és a MIM telepítéséhez.
4. A környezet három dedikált (fizikai vagy virtuális) gépet tartalmaz, amelyeken külön fut a CORPDC, a PRIVDC és a PAMSERVER.
5. Az érvényesítési beállításhoz dedikált munkaállomásra van szükség.

>[!NOTE]
>Amennyiben bármi probléma lépne a szkriptek végrehajtása során, érdemes áttekintenie a naplókat. A szkriptek naplói mind az %AppData%\MIMPAMInstall helyen vannak tárolva. Tömörítse a mappát egy Zip-fájlba, és küldje el a művelet és a hiba részleteivel együtt a támogatási eset részeként.

Készen áll a PAM üzembehelyezési szkriptek használatára? Kezdje [A PAM konfigurálása szkriptek használatával](./pam/sp1-pam-configure-using-scripts.md) lépéssel.

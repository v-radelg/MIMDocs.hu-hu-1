---
title: "MIM2016 SP1 PAM üzembehelyezési szkriptek"
description: "Ez az oldal a Privileged Identity Manager parancsfájlokkal történő konfigurálást ismertető cikksorozat tagja. Tartalma a környezettel kapcsolatos előfeltételeket ismerteti."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 77a222c0a36f4e244a5114eddfc0edadb168d1cd
ms.sourcegitcommit: 06add1a636720f74bc0c0f25b4100b19f1bd31da
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/17/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM üzembehelyezési szkriptek

Ebben a szervizcsomagban elérhetővé tesszük az üzembehelyezési szkriptek egy csoportját, amelyek megkönnyítik a PAM üzembe helyezését. A szkriptek a letöltőközpontból érhetők el. A parancsfájlok használata előtt fontos, győződjön meg arról, hogy teljesülnek az alábbi feltételek:

1. Az operációs rendszer minden olyan kiszolgálón van legalább Windows Server 2012 R2.
2. DNS be kell állítani ahhoz, hogy a névfeloldás a tartományvezérlők között az összetevők kiszolgálóin.
3. A telepítő bináris fájloknak helyben elérhetőnek kell lenniük a kijelölt kiszolgálókon az SQL, a SharePoint és a MIM telepítéséhez.
4. A környezet három dedikált (fizikai vagy virtuális) gépet tartalmaz, amelyeken külön fut a CORPDC, a PRIVDC és a PAMSERVER.
5. Az érvényesítési beállítás egy dedikált munkaállomás rendelkeznie kell.

>[!NOTE]
>Amennyiben bármi probléma lépne a szkriptek végrehajtása során, érdemes áttekintenie a naplókat. A szkriptek naplói mind az %AppData%\MIMPAMInstall helyen vannak tárolva. Tömörítse a mappát egy Zip-fájlba, és küldje el a művelet és a hiba részleteivel együtt a támogatási eset részeként.

Készen áll a PAM üzembehelyezési szkriptek használatára? Kezdje [A PAM konfigurálása szkriptek használatával](./pam/sp1-pam-configure-using-scripts.md) lépéssel.

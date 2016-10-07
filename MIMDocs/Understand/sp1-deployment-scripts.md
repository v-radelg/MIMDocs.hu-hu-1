---
title: "MIM2016 SP1 PAM üzembehelyezési szkriptek"
description: "A CORP-tartomány előkészítése a Privileged Identity Manager által szkriptek útján kezelt meglévő vagy új identitásokkal"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 43a176ed2f1375eb98851064c460515ec09de132


---

# MIM2016 SP1 PAM üzembehelyezési szkriptek

Ebben a szervizcsomagban elérhetővé tesszük az üzembehelyezési szkriptek egy csoportját, amelyek megkönnyítik a PAM üzembe helyezését. A szkriptek a letöltőközpontból érhetők el. Mielőtt elkezdené használni a szkripteket, fontos ellenőriznie, hogy az alábbi előfeltételek érvényesek-e a környezetére.

Lényegi előfeltételezések:
1. A minimális követelmény minden gépen a Windows Server 2012 R2 megléte. Amennyiben a Windows Server 2016 Technical Preview 5-ös verzióját teszteli, a PRIV-tartományvezérlőnek telepítve kell lennie a TP5 builden.
2. A DNS-t úgy kell konfigurálni, hogy a névfeloldás a tartományvezérlők és az összetevő-kiszolgálók közt automatikus legyen.
3. A telepítő bináris fájloknak helyben elérhetőnek kell lenniük a kijelölt kiszolgálókon az SQL, a SharePoint és a MIM telepítéséhez.
4. A környezet három dedikált (fizikai vagy virtuális) gépet tartalmaz, amelyeken külön fut a CORPDC, a PRIVDC és a PAMSERVER.
5. Az érvényesítési lehetőség esetében a rendszer feltételezi, hogy létezik egy dedikált ügyfélgép ennek a lépésnek a végrehajtására.

>[!NOTE] Amennyiben bármi probléma lépne a szkriptek végrehajtása során, érdemes áttekintenie a naplókat. A szkriptek naplói mind az %AppData%\MIMPAMInstall helyen vannak tárolva. Tömörítse a mappát egy Zip-fájlba, és küldje el e-mailben a mim2016@microsoft.com címre a művelet és a hiba részleteivel együtt.



<!--HONumber=Sep16_HO4-->



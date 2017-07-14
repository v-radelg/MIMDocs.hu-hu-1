---
title: "A PAM-környezet áttekintése | Microsoft Docs"
description: "A virtuális gépek száma és konfigurációs adatai, amelyek a Privileged Access Management sikeres üzembe helyezéséhez szükségesek"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 3e6c5a70c6b9ed140a56135676bbd14a84504317
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# A környezet áttekintése
<a id="environment-overview" class="xliff"></a>

A Privileged Access Management olyan virtuális gépekkel (VM) működik, amelyek megosztott hálózaton keresztül kapcsolódnak egymáshoz, és külön meghajtókkal rendelkeznek. Ezek a virtuális gépek Windows 8.1, Windows Server 2012 R2 vagy más operációsrendszer-platformokon is működhetnek.

![PAM-kiszolgálók: kapcsolatok és támogatott platformok – diagram](media/pam-test-lab-architecture.png)

Legalább három virtuális gépre lesz szüksége.  Ha még nem rendelkezik olyan AD-tartománnyal, amelyet a PAM kezelni tud, szüksége lesz egy további virtuális gépre is, amelyet CORP tartományvezérlőként működtethet.  Ha a PRIV szoftvert magas rendelkezésre álláshoz szeretné konfigurálni, további két virtuális gépre lesz szüksége.

A virtuális gépek lemezképeit tároló meghajtókon legalább 120 GB szabad lemezterületnek kell lennie az összes virtuális gép tárolásához.  Ha magas rendelkezésre állású üzemelő példányt tervez létrehozni, győződjön meg róla, hogy a lemezalrendszer megfelel-e az SQL megosztott tárhelyre vonatkozó követelményeknek.  A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában. Fontos, hogy ezeket a megerősített környezetben kell kijelölnie; a tárhely megosztása a megerősített környezeten kívüli egyéb munkaterhelésekkel nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet sértetlenségét.

> [!NOTE]
> Az aktuális MIM-ügyfél technikai előzetese (CTP) nem kompatibilis az előző CTP-ből származó adatbázis vagy a könyvtár tartalmával. Ha a MIM-et korábban már kipróbálta a PAM-re vagy más forgatókönyvekre vonatkozóan, készítsen biztonsági másolatot, illetve archiválja a teszteléshez használt virtuális gépeket, majd indítsa el a központi telepítést új, MIM-forgatókönyvekhez korábban még nem használt virtuális gépek lemezképeivel.


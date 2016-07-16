---
title: "A környezet áttekintése | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: b8af77d2354428da19d91d5f02b490012835f544
ms.openlocfilehash: a01cb2e1df52f3157b3d84a4eab837cececfbe1b


---

# A környezet áttekintése

A PAM olyan virtuális gépekkel (VM) működik, amelyek megosztott hálózaton keresztül kapcsolódnak egymáshoz, és külön meghajtókkal rendelkeznek. Ezek a virtuális gépek Windows 8.1, Windows Server 2012 R2 vagy más operációsrendszer-platformokon is működhetnek.

![PAM-kiszolgálók: kapcsolatok és támogatott platformok – diagram](media/pam-test-lab-architecture.png)

Legalább három virtuális gépre lesz szüksége.  Ha még nem rendelkezik olyan AD-tartománnyal, amelyet a PAM kezelni tud, szüksége lesz egy további virtuális gépre is, amelyet CORP tartományvezérlőként működtethet.  Ha a PRIV szoftvert magas rendelkezésre álláshoz szeretné konfigurálni, további két virtuális gépre lesz szüksége.

A virtuális gépek lemezképeit tároló meghajtókon legalább 120 GB szabad lemezterületnek kell lennie az összes virtuális gép tárolásához.  Ha magas rendelkezésre állású üzemelő példányt tervez létrehozni, győződjön meg róla, hogy a lemezalrendszer megfelel-e az SQL megosztott tárhelyre vonatkozó követelményeknek.  A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában. Fontos, hogy ezeket a megerősített környezetben kell kijelölnie; a tárhely megosztása a megerősített környezeten kívüli egyéb munkaterhelésekkel nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet sértetlenségét.

> [!NOTE]
> Az aktuális MIM-ügyfél technikai előzetese (CTP) nem kompatibilis az előző CTP-ből származó adatbázis vagy a könyvtár tartalmával. Ha a MIM-et korábban már kipróbálta a PAM-re vagy más forgatókönyvekre vonatkozóan, készítsen biztonsági másolatot, illetve archiválja a teszteléshez használt virtuális gépeket, majd indítsa el a központi telepítést új, MIM-forgatókönyvekhez korábban még nem használt virtuális gépek lemezképeivel.



<!--HONumber=Jun16_HO3-->



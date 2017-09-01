---
title: "A PAM-környezet áttekintése | Microsoft Docs"
description: "A virtuális gépek száma és konfigurációs adatai, amelyek a Privileged Access Management sikeres üzembe helyezéséhez szükségesek"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3be2e19673a863098739e830d9c83ce264abf412
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/01/2017
---
# <a name="environment-overview"></a>A környezet áttekintése

A Privileged Access Management olyan virtuális gépekkel (VM) működik, amelyek megosztott hálózaton keresztül kapcsolódnak egymáshoz, és külön meghajtókkal rendelkeznek. Ezek a virtuális gépek Windows 8.1, Windows Server 2012 R2 vagy más operációsrendszer-platformokon is működhetnek.

![PAM-kiszolgálók: kapcsolatok és támogatott platformok – diagram](media/pam-test-lab-architecture.png)

Legalább három virtuális gép van szüksége.  Ha még nem rendelkezik az AD-tartomány, a PAM kezelni tud, akkor egy további virtuális Gépet a CORP tartományvezérlőként.  Ha szeretne beállítani a PRIV szoftvert magas rendelkezésre állású, akkor két további virtuális gépeket.

A Virtuálisgép-lemezképeket tároló meghajtókat legalább 120 GB szabad lemezterület szükséges.  Ha magas rendelkezésre állású üzemelő példányt tervez létrehozni, győződjön meg róla, hogy a lemezalrendszer megfelel-e az SQL megosztott tárhelyre vonatkozó követelményeknek.  A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában.

>[!IMPORTANT]
Tárolási a megerősített környezetben kell kijelölnie. A megerősített környezeten kívüli egyéb munkaterhelésekkel való megosztása nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet sértetlenségét.

## <a name="next-steps"></a>További lépések

- [Privileged Access Management az Active Directory tartományi szolgáltatásokhoz](privileged-identity-management-for-active-directory-domain-services.md) a PAM és annak működéséről nyújt áttekintést.
- [A PAM-összetevők megismerése](principles-of-operation.md) különböző PAM-összetevők áttekintést.
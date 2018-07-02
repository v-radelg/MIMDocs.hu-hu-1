---
title: A PAM-környezet áttekintése | Microsoft Docs
description: A virtuális gépek száma és konfigurációs adatai, amelyek a Privileged Access Management sikeres üzembe helyezéséhez szükségesek
keywords: ''
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
ms.openlocfilehash: e83c326d32645ce80541d5c415cd9c0e9d1dae54
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288792"
---
# <a name="environment-overview"></a>A környezet áttekintése

A Privileged Access Management olyan virtuális gépekkel (VM) működik, amelyek megosztott hálózaton keresztül kapcsolódnak egymáshoz, és külön meghajtókkal rendelkeznek. Ezek a virtuális gépek Windows 8.1, Windows Server 2012 R2 vagy más operációsrendszer-platformokon is működhetnek.

![PAM-kiszolgálók: kapcsolatok és támogatott platformok – diagram](media/pam-test-lab-architecture.png)

Legalább három virtuális gép van szüksége.  Ha még nem rendelkezik az AD-tartomány, a PAM kezelni tud, akkor egy további virtuális Gépet a CORP tartományvezérlőként.  Ha szeretne beállítani a PRIV szoftvert magas rendelkezésre állású, akkor két további virtuális gépeket.

A Virtuálisgép-lemezképeket tároló meghajtókat legalább 120 GB szabad lemezterület szükséges.  Ha magas rendelkezésre állású üzemelő példányt tervez létrehozni, győződjön meg róla, hogy a lemezalrendszer megfelel-e az SQL megosztott tárhelyre vonatkozó követelményeknek.  A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában.

> [!IMPORTANT]
> Tárolási a megerősített környezetben kell kijelölnie. A megerősített környezeten kívüli egyéb munkaterhelésekkel való megosztása nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet sértetlenségét.

## <a name="next-steps"></a>További lépések

- [Privileged Access Management az Active Directory tartományi szolgáltatásokhoz](privileged-identity-management-for-active-directory-domain-services.md) a PAM és annak működéséről nyújt áttekintést.
- [A PAM-összetevők megismerése](principles-of-operation.md) különböző PAM-összetevők áttekintést.
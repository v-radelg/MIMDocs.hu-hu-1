---
title: A PAM-környezet áttekintése | Microsoft Docs
description: A virtuális gépek száma és konfigurációs adatai, amelyek a Privileged Access Management sikeres üzembe helyezéséhez szükségesek
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6f4b6e224b6b50bf2190688a994f35159d273713
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518534"
---
# <a name="environment-overview"></a>A környezet áttekintése

A Privileged Access Management olyan virtuális gépekkel (VM) működik, amelyek megosztott hálózaton keresztül kapcsolódnak egymáshoz, és külön meghajtókkal rendelkeznek. Ezek a virtuális gépek Windows 8.1, Windows Server 2012 R2 vagy más operációsrendszer-platformokon is működhetnek.

![PAM-kiszolgálók: kapcsolatok és támogatott platformok – diagram](media/pam-test-lab-architecture.png)

Legalább három virtuális gép szükséges.  Ha még nem rendelkezik a PAM által felügyelt AD-tartománnyal, egy további virtuális gépre van szüksége, amely CORP-tartományvezérlőként működik.  Ha magas rendelkezésre álláshoz szeretné konfigurálni a PRIV-szoftvert, két további virtuális gépre van szüksége.

A virtuális gépek lemezképeit tároló meghajtókon legalább 120 GB szabad lemezterület szükséges.  Ha magas rendelkezésre állású üzemelő példányt tervez létrehozni, győződjön meg róla, hogy a lemezalrendszer megfelel-e az SQL megosztott tárhelyre vonatkozó követelményeknek.  A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában.

> [!IMPORTANT]
> A tárterületet a megerősített környezetnek kell dedikáltnak lennie. A tárterület a megerősített környezeten kívüli egyéb munkaterhelésekkel való megosztása nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet integritását.

## <a name="next-steps"></a>További lépések

- A [Active Directory tartományi szolgáltatások Privileged Access Management](privileged-identity-management-for-active-directory-domain-services.md) a PAM áttekintése és működése.
- [A PAM összetevőinek megismerése](principles-of-operation.md) a PAM különböző összetevőinek áttekintése.

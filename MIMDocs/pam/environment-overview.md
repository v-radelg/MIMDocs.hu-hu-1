---
title: A PAM-környezet áttekintése | Microsoft Docs
description: A virtuális gépek száma és konfigurációs adatai, amelyek a Privileged Access Management sikeres üzembe helyezéséhez szükségesek
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4e3a3fb2b23deced2202994463645521beef81e8
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332918"
---
# <a name="environment-overview"></a>A környezet áttekintése

A Privileged Access Management olyan virtuális gépekkel (VM) működik, amelyek megosztott hálózaton keresztül kapcsolódnak egymáshoz, és külön meghajtókkal rendelkeznek. Ezek a virtuális gépek Windows 8.1, Windows Server 2012 R2 vagy más operációsrendszer-platformokon is működhetnek.

![PAM-kiszolgálók: kapcsolatok és támogatott platformok – diagram](media/pam-test-lab-architecture.png)

Szüksége lesz egy legalább három virtuális gép.  Ha még nem rendelkezik AD-tartomány, a PAM kezelni, egy további virtuális Gépre egy CORP tartományvezérlőként frissítenie.  Ha szeretné konfigurálni a PRIV szoftvert magas rendelkezésre állású, két további virtuális gépeket kell.

A a virtuális gépek lemezképeit tároló meghajtókon legalább 120 GB szabad lemezterület szükséges.  Ha magas rendelkezésre állású üzemelő példányt tervez létrehozni, győződjön meg róla, hogy a lemezalrendszer megfelel-e az SQL megosztott tárhelyre vonatkozó követelményeknek.  A megosztott tárolás történhet Windows Server feladatátvételi fürtszolgáltatási fürtlemezeken, tárolóhálózaton (SAN) lévő lemezeken vagy SMB-kiszolgálón található fájlmegosztások formájában.

> [!IMPORTANT]
> Storage a megerősített környezetben kell kijelölnie. A megerősített környezeten kívüli egyéb munkaterhelésekkel való megosztása a tároló nem ajánlott, mivel ez veszélyeztetheti a megerősített környezet sértetlenségét.

## <a name="next-steps"></a>További lépések

- [Privileged Access Management az Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) PAM és a működésének áttekintése.
- [A PAM-összetevők megismerése](principles-of-operation.md) PAM-összetevők különböző áttekintése.
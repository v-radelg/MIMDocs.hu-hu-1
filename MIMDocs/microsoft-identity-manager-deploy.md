---
title: Microsoft Identity Manager 2016 üzembe helyezéséhez szükséges lépések | Microsoft Docs
description: Itt olvashatók a Microsoft Identity Manager 2016 üzembe helyezését érintő lépések teljes listája, a környezet előkészítésétől kezdve a portálok konfigurálásáig.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 5a80718b038a10bb8d746a86856b87d016783fd6
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043188"
---
# <a name="deploy-microsoft-identity-manager-2016-sp2"></a>Microsoft Identity Manager 2016 SP2 telepítése
A jelen szakaszban olvasható cikkek részletes útmutatást nyújtanak az önkiszolgáló végfelhasználói esetek ellátására szolgáló Microsoft Identity Manager (MIM) 2016 üzembe helyezéséhez egy olyan új kiszolgálón, amelyen korábban még nem volt telepítve se a FIM, se a MIM rendszer.

> [!NOTE]
> A jelen szakaszban leírt üzembe helyezési topológia csak az első lépésekhez és a MIM-ről való tájékozódáshoz nyújt segítséget.  Az üzemi környezetek ellátására alkalmas topológiákról a [kapacitástervezési útmutató](capacity-planning-guide.md) nyújt részletesebb információt.  Javasoljuk, hogy a MIM éles környezetben való méretezésre vagy használatra történő üzembe helyezése előtt olvassa el a kapacitástervezési útmutatót.

Az emelt szintű hozzáférések kezelését másképpen kell üzembe helyezni, mint a MIM-forgatókönyveket, mivel az előbbi dedikált megerősített erdővel rendelkező környezetet igényel.  A MIM üzembe helyezéséről Privileged Identity Management esetén a [MIM-környezet konfigurálása a Privileged Access Management szolgáltatáshoz](./pam/configuring-mim-environment-for-pam.md) című témakörben olvashat.

A webszolgáltatások üzembe helyezésének folyamata nagyon hasonlít az elődje, a FIM 2010 R2 folyamatára. A FIM-dokumentációt a [Forefront Identity Manager 2010 R2 telepítési útmutatója](https://technet.microsoft.com/library/jj134310) című témakörben találja.

## <a name="first-prepare-a-domain"></a>Első lépés: Tartomány előkészítése
A MIM az Active Directoryval (AD) működik, ezért a következő lépések segítségével állítsa be az AD tartományvezérlőt.
- [Tartomány beállítása](preparing-domain.md) vagy
- [A csoportosan felügyelt szolgáltatásfiókok tartományának beállítása](preparing-domain-gmsa.md)


## <a name="next-prepare-an-identity-management-servers"></a>Következő: Identitáskezelés-felügyeleti kiszolgálók előkészítése
Miután telepítette és konfigurálta a tartományt, készítse elő a vállalati identitáskezelő kiszolgálót.

A támogatott platformokról további információt a következő témakörben talál: [a támogatott platformok a 2016-es vagy újabb verzióhoz](microsoft-identity-manager-2016-supported-platforms.md)

 Ehhez a következőket kell előkészíteni:
- [Windows Server](prepare-server-ws2016.md)
- [SQL Server](prepare-server-sql2016.md)
- [SharePoint Server](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (nem kötelező)

## <a name="finally-install-microsoft-identity-manager-2016-sp2-components"></a>Végül: telepítse a Microsoft Identity Manager 2016 SP2-összetevőket
Ha telepítette a tartományt és a kiszolgálót, akkor készen áll a MIM összetevőinek telepítésére és az összetevők AD-vel való szinkronizálásának konfigurálására.
- [MIM Synchronization Service](install-mim-sync.md)
- [MIM szolgáltatás és -portál](install-mim-service-portal.md)
- [Az Active Directory és a MIM szolgáltatás adatbázisainak szinkronizálása](install-mim-sync-ad-service.md)

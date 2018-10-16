---
title: Microsoft Identity Manager 2016 üzembe helyezéséhez szükséges lépések | Microsoft Docs
description: Itt olvashatók a Microsoft Identity Manager 2016 üzembe helyezését érintő lépések teljes listája, a környezet előkészítésétől kezdve a portálok konfigurálásáig.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d8cf13fc6a8b9b644d241679aa6095008fab21bc
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334329"
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>A Microsoft Identity Manager 2016 SP1 telepítése
A jelen szakaszban olvasható cikkek részletes útmutatást nyújtanak az önkiszolgáló végfelhasználói esetek ellátására szolgáló Microsoft Identity Manager (MIM) 2016 üzembe helyezéséhez egy olyan új kiszolgálón, amelyen korábban még nem volt telepítve se a FIM, se a MIM rendszer.

> [!NOTE]
> A jelen szakaszban leírt üzembe helyezési topológia csak az első lépésekhez és a MIM-ről való tájékozódáshoz nyújt segítséget.  Az üzemi környezetek ellátására alkalmas topológiákról a [kapacitástervezési útmutató](capacity-planning-guide.md) nyújt részletesebb információt.  Javasoljuk, hogy a MIM éles környezetben való méretezésre vagy használatra történő üzembe helyezése előtt olvassa el a kapacitástervezési útmutatót.

Az emelt szintű hozzáférések kezelését másképpen kell üzembe helyezni, mint a MIM-forgatókönyveket, mivel az előbbi dedikált megerősített erdővel rendelkező környezetet igényel.  A MIM üzembe helyezéséről Privileged Identity Management esetén a [MIM-környezet konfigurálása a Privileged Access Management szolgáltatáshoz](./pam/configuring-mim-environment-for-pam.md) című témakörben olvashat.

A MIM üzembe helyezésekor folyamat nagyon hasonlít a folyamat elődjének, a FIM 2010 R2-ről. A FIM-dokumentációt a [Forefront Identity Manager 2010 R2 telepítési útmutatója](https://technet.microsoft.com/library/jj134310) című témakörben találja.

## <a name="first-prepare-a-domain"></a>Első lépés: Tartomány előkészítése
A MIM az Active Directoryval (AD) működik, ezért a következő lépések segítségével állítsa be az AD tartományvezérlőt.
- [Tartomány beállítása](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>A következő: Az identitás felügyeleti kiszolgálók előkészítése
Miután telepítette és konfigurálta a tartományt, készítse elő a vállalati identitáskezelő kiszolgálót. Ehhez a következőket kell előkészíteni:
- [Windows Server 2016](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [A SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (nem kötelező)

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>A végső lépés: Telepítse a Microsoft Identity Manager 2016 SP1 összetevők
Ha telepítette a tartományt és a kiszolgálót, akkor készen áll a MIM összetevőinek telepítésére és az összetevők AD-vel való szinkronizálásának konfigurálására.
- [MIM Synchronization Service](install-mim-sync.md)
- [MIM szolgáltatás és -portál](install-mim-service-portal.md)
- [Az Active Directory és a MIM szolgáltatás adatbázisainak szinkronizálása](install-mim-sync-ad-service.md)
- [A MIM 2016 vagy újabb támogatott platformok](microsoft-identity-manager-2016-supported-platforms.md)

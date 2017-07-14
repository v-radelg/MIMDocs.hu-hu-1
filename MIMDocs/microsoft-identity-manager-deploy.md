---
title: "Microsoft Identity Manager 2016 üzembe helyezéséhez szükséges lépések | Microsoft Docs"
description: "Itt olvashatók a Microsoft Identity Manager 2016 üzembe helyezését érintő lépések teljes listája, a környezet előkészítésétől kezdve a portálok konfigurálásáig."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: fa200bb18871387420743af64ca196565397e5d5
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# A MIM 2016 üzembe helyezése
<a id="deploy-mim-2016" class="xliff"></a>
A jelen szakaszban olvasható cikkek részletes útmutatást nyújtanak az önkiszolgáló végfelhasználói esetek ellátására szolgáló Microsoft Identity Manager (MIM) 2016 üzembe helyezéséhez egy olyan új kiszolgálón, amelyen korábban még nem volt telepítve se a FIM, se a MIM rendszer.

> [!NOTE]
> A jelen szakaszban leírt üzembe helyezési topológia csak az első lépésekhez és a MIM-ről való tájékozódáshoz nyújt segítséget.  Az üzemi környezetek ellátására alkalmas topológiákról a [kapacitástervezési útmutató](capacity-planning-guide.md) nyújt részletesebb információt.  Javasoljuk, hogy a MIM éles környezetben való méretezésre vagy használatra történő üzembe helyezése előtt olvassa el a kapacitástervezési útmutatót.

Az emelt szintű hozzáférések kezelését másképpen kell üzembe helyezni, mint a MIM-forgatókönyveket, mivel az előbbi dedikált megerősített erdővel rendelkező környezetet igényel.  A MIM üzembe helyezéséről Privileged Identity Management esetén a [MIM-környezet konfigurálása a Privileged Access Management szolgáltatáshoz](./pam/configuring-mim-environment-for-pam.md) című témakörben olvashat.

A MIM 2016 telepítésének lépései hasonlóak elődjének, a FIM 2010 R2-nek a telepítési folyamatához. A FIM-dokumentációt a [Forefront Identity Manager 2010 R2 telepítési útmutatója](https://technet.microsoft.com/library/jj134310) című témakörben találja.

## Első lépés: Tartomány előkészítése
<a id="first-prepare-a-domain" class="xliff"></a>
A MIM az Active Directoryval (AD) működik, ezért a következő lépések segítségével állítsa be az AD tartományvezérlőt.
- [Tartomány beállítása](preparing-domain.md)

## A következő lépés: Identitáskezelési kiszolgáló előkészítése
<a id="next-prepare-an-identity-management-server" class="xliff"></a>
Miután telepítette és konfigurálta a tartományt, készítse elő a vállalati identitáskezelő kiszolgálót. Ehhez a következőket kell előkészíteni:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (nem kötelező)

## A végső lépés: A Microsoft Identity Manager 2016 összetevőinek telepítése
<a id="finally-install-microsoft-identity-manager-2016-components" class="xliff"></a>
Ha telepítette a tartományt és a kiszolgálót, akkor készen áll a MIM összetevőinek telepítésére és az összetevők AD-vel való szinkronizálásának konfigurálására.
- [MIM Synchronization Service](install-mim-sync.md)
- [MIM szolgáltatás és -portál](install-mim-service-portal.md)
- [Az Active Directory és a MIM szolgáltatás adatbázisainak szinkronizálása](install-mim-sync-ad-service.md)


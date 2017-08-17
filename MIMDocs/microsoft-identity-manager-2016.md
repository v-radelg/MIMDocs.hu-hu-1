---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: "A MIM magában foglalja a FIM 2010 hozzáférés-kezelési lehetőségeit és a felhasználók, hitelesítő adatok, szabályzatok és szervezeten belüli hozzáférési jogosultságok kezelésében nyújt segítséget."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b3cdc1a71b6e9eb14a132429ea66bb4ab33fe3c4
ms.sourcegitcommit: 0a78e39976cd03225a8e24a508e9ee23585e67cc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/11/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016
A Microsoft Identity Manager (MIM) 2016 a [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx) identitás- és hozzáférés-kezelési képességeire építkezik. Elődjéhez hasonlóan a MIM a szervezeti felhasználók, hitelesítő adatok, házirendek és hozzáférési jogosultságok kezelésében nyújt segítséget.  A MIM 2016 emellett hibrid funkciókat és az emelt szintű hozzáférések felügyeletére szolgáló képességeket biztosít, és új platformokat is támogat.

A Microsoft Identity Manager jelen verziója olyan új funkciókat biztosít, mint például a Privileged Identity Management vagy a REST API-alapú hozzáférés támogatása a Tanúsítványkezelőben. A Tanúsítványkezelő nincs mostantól támogatja a többerdős topológiákat, egy Windows-alkalmazás virtuális intelligens kártyák és a tanúsítvány-életciklus felügyelete, a frissített eseményeket és hibaelhárítási képességeket. Az önkiszolgáló funkciók között már elérhető a fiókok zárolásának feloldása, valamint az Azure MFA (Multi-Factor Authentication) típusú átjáró is a jelszóváltoztatáshoz.

## <a name="hybrid-experience"></a>Hibrid környezet
A Microsoft Identity Manager 2016 az Azure AD szolgáltatással együttműködve a teljes környezet felett biztosít ellenőrzést. Az Azure AD hibrid jelentéskészítési funkciói révén a felhőbeli és a helyszíni adatok egy helyen jeleníthetők meg. Emellett az önkiszolgáló jelszó-változtatási portál már támogatja az Azure Multi-Factor Authentication (MFA) többtényezős hitelesítési szolgáltatást is.

## <a name="privileged-identity-management"></a>Privileged Identity Management
A Privileged Identity Management a bizalmas erőforrások ideiglenes, feladatalapú elérhetőségének biztosításával segíti a rendszergazdai hozzáférés szabályozását és kezelését. Ez azt jelenti, hogy épp csak a szükséges mértékű engedélyeket adhat a felhasználóknak, ami csökkenti a veszélyét annak, hogy egy kibertámadó esetlegesen teljes rendszergazdai hozzáféréshez jusson. A Privileged Identity Management emellett képes kinyerni és elkülöníteni a rendszergazdai fiókokat a meglévő Active Directory-erdőkből.

A MIM támogat egy helyszíni Privileged Identity Management megoldást az Active Directory kezeléséhez. Bevezető útmutatóját [A Privileged Access Management használata](./pam/privileged-identity-management-for-active-directory-domain-services.md) című cikk tartalmazza.

## <a name="related-topics"></a>Kapcsolódó témakörök

- A Microsoft Identity Manager számos közös vonást mutat elődjével, a Forefront Identity Managerrel. Ha még FIM-et használ, vagy ha további dokumentációra van szüksége, tekintse meg a [FIM 2010 R2 dokumentáció - áttekintés](https://technet.microsoft.com/library/jj133885.aspx) című oldalt.
- [Topológiai szempontok a MIM üzembe helyezésekor](topology-considerations.md)
- [Kapacitástervezési útmutató](capacity-planning-guide.md)
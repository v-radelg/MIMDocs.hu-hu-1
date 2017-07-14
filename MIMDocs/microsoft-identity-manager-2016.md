---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: "A MIM magában foglalja a FIM 2010 hozzáférés-kezelési lehetőségeit és a felhasználók, hitelesítő adatok, szabályzatok és szervezeten belüli hozzáférési jogosultságok kezelésében nyújt segítséget."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 3f5024cf760834ba2181252325a13e40bda520cd
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# Microsoft Identity Manager 2016
<a id="microsoft-identity-manager-2016" class="xliff"></a>
A Microsoft Identity Manager (MIM) 2016 a [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx) identitás- és hozzáférés-kezelési képességeire építkezik. Elődjéhez hasonlóan a MIM a szervezeti felhasználók, hitelesítő adatok, házirendek és hozzáférési jogosultságok kezelésében nyújt segítséget.  A MIM 2016 emellett hibrid funkciókat és az emelt szintű hozzáférések felügyeletére szolgáló képességeket biztosít, és új platformokat is támogat.

A Microsoft Identity Manager jelen verziója olyan új funkciókat biztosít, mint például a Privileged Identity Manager vagy a REST API-alapú hozzáférés támogatása a Tanúsítványkezelőben. A Tanúsítványkezelő már támogatja a többerdős topológiákat, megújult eseményeket és hibaelhárítási képességeket kínál, a Windows Áruházból pedig letölthető egy alkalmazás a virtuális intelligens kártyák és a tanúsítványok életciklusának kezelésére. Az önkiszolgáló funkciók között már elérhető a fiókok zárolásának feloldása, valamint egy többtényezős hitelesítési kapu is a jelszóváltoztatáshoz.

## Hibrid környezet
<a id="hybrid-experience" class="xliff"></a>
A Microsoft Identity Manager 2016 az Azure-ral együttműködve a teljes környezet felett ellenőrzést biztosít. Az Azure hibrid jelentéskészítési funkciói révén a felhőbeli és a helyszíni adatok egy helyen jeleníthetők meg. Emellett az önkiszolgáló jelszó-változtatási portál már támogatja az Azure Multi-Factor Authentication (MFA) többtényezős hitelesítési szolgáltatást is.

## Privileged Identity Management
<a id="privileged-identity-management" class="xliff"></a>
A Privileged Identity Management a bizalmas erőforrások ideiglenes, feladatalapú elérhetőségének biztosításával segíti a rendszergazdai hozzáférés szabályozását és kezelését. Ez azt jelenti, hogy épp csak a szükséges mértékű engedélyeket adhat a felhasználóknak, ami csökkenti a veszélyét annak, hogy egy kibertámadó esetlegesen teljes rendszergazdai hozzáféréshez jusson. A Privileged Identity Management emellett képes kinyerni és elkülöníteni a rendszergazdai fiókokat a meglévő Active Directory-erdőkből.

A MIM támogat egy helyszíni Privileged Identity Management megoldást az Active Directory kezeléséhez. Bevezető útmutatóját [A Privileged Access Management használata](./pam/privileged-identity-management-for-active-directory-domain-services.md) című cikk tartalmazza.

## Kapcsolódó témakörök
<a id="related-topics" class="xliff"></a>
A Microsoft Identity Manager számos közös vonást mutat elődjével, a Forefront Identity Managerrel. Ha még FIM-et használ, vagy ha további dokumentációra van szüksége, tekintse meg a [FIM 2010 R2 dokumentáció - áttekintés](https://technet.microsoft.com/library/jj133885.aspx) című oldalt.


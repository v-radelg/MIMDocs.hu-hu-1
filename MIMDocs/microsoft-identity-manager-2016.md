---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: "A MIM magában foglalja a FIM 2010 hozzáférés-kezelési lehetőségeit és a felhasználók, hitelesítő adatok, szabályzatok és szervezeten belüli hozzáférési jogosultságok kezelésében nyújt segítséget."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 10/17/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cd8191e3fdf571f4140bcdd51c54aa25bd663215
ms.sourcegitcommit: 06add1a636720f74bc0c0f25b4100b19f1bd31da
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/17/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

A Microsoft Identity Manager (MIM) 2016 a [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx) identitás- és hozzáférés-kezelési képességeire építkezik. Elődjéhez hasonlóan a MIM a szervezeti felhasználók, hitelesítő adatok, házirendek és hozzáférési jogosultságok kezelésében nyújt segítséget.  A MIM 2016 emellett hibrid funkciókat és az emelt szintű hozzáférések felügyeletére szolgáló képességeket biztosít, és új platformokat is támogat.

Mellett szereplő meglévő identitáskezelési funkcióit [FIM](https://technet.microsoft.com/library/jj133868). A MIM 2016 például nyújt új és továbbfejlesztett szolgáltatásokat:

- Privileged Identity Management
- A Tanúsítványkezelő új funkciók
  - [Tanúsítványkezelő – REST API-referencia](./reference/certificate-management-rest-api-reference.md)
  - A többerdős topológiákat támogatása.
  - [Virtuális intelligens kártya egy Windows-alkalmazás](working-with-mim-certificate-manager.md)
  - Megújult eseményeket és hibaelhárítási képességeket. 
- [Az önkiszolgáló funkciók](working-with-self-service-password-reset.md) között már elérhető a fiókok zárolásának feloldása és az Azure többtényezős hitelesítés (többtényezős hitelesítési) kapu a jelszó-változtatási portálhoz.

## <a name="hybrid-experience"></a>Hibrid környezet

A Microsoft Identity Manager 2016 problémamentesen együttműködik velük [az Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) adhat meg a teljes környezet felett ellenőrzést. Az Azure AD hibrid jelentéskészítési funkciói révén a felhőbeli és a helyszíni adatok egy helyen jeleníthetők meg. Emellett a [önkiszolgáló jelszó-változtatási portál](working-with-self-service-password-reset.md) támogatja az Azure többtényezős hitelesítés (MFA).

## <a name="privileged-identity-management"></a>Privileged Identity Management

A Privileged Identity Management a bizalmas erőforrások ideiglenes, feladatalapú elérhetőségének biztosításával segíti a rendszergazdai hozzáférés szabályozását és kezelését. Ez azt jelenti, hogy épp csak a szükséges mértékű engedélyeket adhat a felhasználóknak, ami csökkenti a veszélyét annak, hogy egy kibertámadó esetlegesen teljes rendszergazdai hozzáféréshez jusson. A Privileged Identity Management emellett képes kinyerni és elkülöníteni a rendszergazdai fiókokat a meglévő Active Directory-erdőkből.

A MIM támogat egy helyszíni Privileged Identity Management megoldást az Active Directory kezeléséhez. Bevezető útmutatóját [A Privileged Access Management használata](./pam/privileged-identity-management-for-active-directory-domain-services.md) című cikk tartalmazza.

## <a name="related-topics"></a>Kapcsolódó témakörök

- A Microsoft Identity Manager számos közös vonást mutat elődjével, a Forefront Identity Managerrel. Ha még FIM-et használ, vagy ha további dokumentációra van szüksége, tekintse meg a [FIM 2010 R2 dokumentáció - áttekintés](https://technet.microsoft.com/library/jj133885.aspx) című oldalt.
- [Topológiai szempontok a MIM üzembe helyezésekor](topology-considerations.md) Ez a cikk több üzembe helyezési topológiát mutat, előfordulhat, hogy végrehajtási be.
- [Kapacitástervezési útmutató](capacity-planning-guide.md) ebben az útmutatóban és a tesztkörnyezet használatával megérthetik, hogy az üzembe helyezéshez megfelelő hatókörét.
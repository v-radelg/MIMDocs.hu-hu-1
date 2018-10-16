---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: A MIM magában foglalja a FIM 2010 hozzáférés-kezelési lehetőségeit és a felhasználók, hitelesítő adatok, szabályzatok és szervezeten belüli hozzáférési jogosultságok kezelésében nyújt segítséget.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de00cc6da8c431519c9bdef3a2e97a8333cbb4ac
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332289"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

A Microsoft Identity Manager (MIM) 2016 a [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx) identitás- és hozzáférés-kezelési képességeire építkezik. Elődjéhez hasonlóan a MIM a szervezeti felhasználók, hitelesítő adatok, házirendek és hozzáférési jogosultságok kezelésében nyújt segítséget.  A MIM 2016 emellett hibrid funkciókat és az emelt szintű hozzáférések felügyeletére szolgáló képességeket biztosít, és új platformokat is támogat.

Mellett szereplő meglévő identitáskezelési funkcióit [FIM](https://technet.microsoft.com/library/jj133868). A MIM 2016 például biztosít új funkciók és fejlesztések:

- Privileged Identity Management
- A Tanúsítványkezelő új funkciók
  - [Tanúsítványkezelő – REST API-referencia](./reference/certificate-management-rest-api-reference.md)
  - Támogatja a többerdős topológiákat.
  - [A virtuális intelligens kártya egy Windows-alkalmazás](working-with-mim-certificate-manager.md)
  - Megújult eseményeket és hibaelhárítási képességeket kínál. 
- [Az önkiszolgáló funkciók](working-with-self-service-password-reset.md) között már elérhető a fiókok zárolásának feloldása és az Azure MFA (multi-factor authentication) kapu új jelszó kéréséhez.

## <a name="hybrid-experience"></a>Hibrid környezet

A Microsoft Identity Manager 2016 problémamentesen együttműködik velük [Azure ad-ben](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) a teljes környezet felett ellenőrzést biztosítanak. Az Azure AD hibrid jelentéskészítési funkciói révén a felhőbeli és a helyszíni adatok egy helyen jeleníthetők meg. Emellett a [portált önkiszolgáló jelszó-visszaállítás](working-with-self-service-password-reset.md) támogatja az Azure multi-factor authentication (MFA).

## <a name="privileged-identity-management"></a>Privileged Identity Management

A Privileged Identity Management a bizalmas erőforrások ideiglenes, feladatalapú elérhetőségének biztosításával segíti a rendszergazdai hozzáférés szabályozását és kezelését. Ez azt jelenti, hogy épp csak a szükséges mértékű engedélyeket adhat a felhasználóknak, ami csökkenti a veszélyét annak, hogy egy kibertámadó esetlegesen teljes rendszergazdai hozzáféréshez jusson. A Privileged Identity Management emellett képes kinyerni és elkülöníteni a rendszergazdai fiókokat a meglévő Active Directory-erdőkből.

A MIM támogat egy helyszíni Privileged Identity Management megoldást az Active Directory kezeléséhez. Bevezető útmutatóját [A Privileged Access Management használata](./pam/privileged-identity-management-for-active-directory-domain-services.md) című cikk tartalmazza.

## <a name="related-topics"></a>Kapcsolódó témakörök

- A Microsoft Identity Manager számos közös vonást mutat elődjével, a Forefront Identity Managerrel. Ha még FIM-et használ, vagy ha további dokumentációra van szüksége, tekintse meg a [FIM 2010 R2 dokumentáció - áttekintés](https://technet.microsoft.com/library/jj133885.aspx) című oldalt.
- [Topológiai szempontok a MIM üzembe helyezésekor](topology-considerations.md) Ez a cikk több lehetséges telepítési topológiákat be.
- [Kapacitástervezési útmutató](capacity-planning-guide.md) ebben az útmutatóban és a tesztkörnyezet segítségével ismerje meg az üzembe helyezés megfelelő hatókörét.
---
title: '1. lépés: A PRIV-tartomány konfigurálása'
description: A CORP-tartomány előkészítése a Privileged Identity Manager által szkriptek útján kezelt meglévő vagy új identitásokkal
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: c1092df1c1fe43551dfde8bbe4f0e77cf46ee981
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379447"
---
# <a name="step-1-configuring-the-priv-domain"></a>1. lépés: A PRIV-tartomány konfigurálása

> [!div class="step-by-step"]
> [2. lépés »](sp1-step2-configuring-corp-domain.md)

1. Bejelentkezés a PRIVDC tartományvezérlőre rendszergazdaként
   * Amennyiben ez egy csak PRIV környezet, a CORPDC tartományvezérlőre kell bejelentkeznie
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Az 1. menüelem kiválasztása (PRIV-erdő konfigurálása)


A rendszer automatikusan létrehozza az SQL/SharePoint és a MIM felügyeletéhez szükséges szolgáltatásfiókokat, amennyiben azok még nem érhetők el a tartományon. A szkript futása során a szolgáltatásfiókok létrehozásához meg kell adnia a szükséges jelszavakat.
Amennyiben a PRIV-tartomány Windows Server 2016 rendszerű, és a működési szint a Windows Server 2016 Technical Preview 5 verzióra van állítva, a szkript kéri majd a PAM által igényelt választható Active Directory „Privileged Access Management szolgáltatás” engedélyezését. A folytatáshoz hagyja jóvá az „Igen” lehetőséget.
A Windows Server 2016 alatti működési szintek esetén zárja be a figyelmeztetést, amely arról tájékoztatja, hogy a további konfigurációk nem lesznek végrehajtva. Miután a rendszergazda Windows Server 2016 szintre emeli a működési szintet, újra kell futtatnia a PAMDeployment.ps1 szkriptet és a PAM-erdő konfigurálását.

>[!NOTE]
>Az alábbi lépések NEM szükségesek a PRIVOnly konfigurációk esetében

Másolja az $env:SYSTEMDRIVE\PAM helyen létrehozott SIDs.txt fájlt az ugyanilyen mappába a CORPDC tartományvezérlőn. Ez azért szükséges, hogy a CORPDC be tudja állítani a PRIV-felhasználók jogosultságát a CORP-felhasználói tulajdonságok olvasásához.
A szkript a futását befejezően arra kéri, hogy a módosítások hatályba léptetéséhez indítsa újra a gépet.

> [!div class="step-by-step"]
> [2. lépés »](sp1-step2-configuring-corp-domain.md)

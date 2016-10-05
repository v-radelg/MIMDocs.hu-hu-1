---
title: "1. lépés: A PRIV-tartomány konfigurálása"
description: "A CORP-tartomány előkészítése a Privileged Identity Manager által szkriptek útján kezelt meglévő vagy új identitásokkal"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 37ac600701ed9d90790e557d2f282be45eed43b4


---
# 1. lépés: A PRIV-tartomány konfigurálása

1. Bejelentkezés a PRIVDC tartományvezérlőre rendszergazdaként
  * Amennyiben ez egy csak PRIV környezet, a CORPDC tartományvezérlőre kell bejelentkeznie
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Az 1. menüelem kiválasztása (PRIV-erdő konfigurálása)


A rendszer automatikusan létrehozza az SQL/SharePoint és a MIM felügyeletéhez szükséges szolgáltatásfiókokat, amennyiben azok még nem érhetők el a tartományon. A szkript futása során a szolgáltatásfiókok létrehozásához meg kell adnia a szükséges jelszavakat.
Amennyiben a PRIV-tartomány Windows Server 2016 rendszerű, és a működési szint a Windows Server 2016 Technical Preview 5 verzióra van állítva, a szkript kéri majd a PAM által igényelt választható Active Directory „Privileged Access Management szolgáltatás” engedélyezését. A folytatáshoz hagyja jóvá az „Igen” lehetőséget.
A Windows Server 2016 alatti működési szintek esetén zárja be a figyelmeztetést, amely arról tájékoztatja, hogy a további konfigurációk nem lesznek végrehajtva. Miután a rendszergazda Windows Server 2016 szintre emeli a működési szintet, újra kell futtatnia a PAMDeployment.ps1 szkriptet és a PAM-erdő konfigurálását.

>[!Note] Az alábbi lépések NEM szükségesek a PRIVOnly konfigurációk esetében

Másolja az $env:SYSTEMDRIVE\PAM helyen létrehozott SIDs.txt fájlt az ugyanilyen mappába a CORPDC tartományvezérlőn. Ez azért szükséges, hogy a CORPDC be tudja állítani a PRIV-felhasználók jogosultságát a CORP-felhasználói tulajdonságok olvasásához.
A szkript a futását befejezően arra kéri, hogy a módosítások hatályba léptetéséhez indítsa újra a gépet.



<!--HONumber=Sep16_HO4-->



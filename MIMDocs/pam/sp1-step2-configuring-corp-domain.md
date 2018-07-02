---
title: '2. lépés: A CORP-tartomány konfigurálása'
description: Ez a cikk a CORP-tartomány konfigurálásának második lépését ismerteti, amelyhez hozzátartozik egy parancsfájl futtatása is a SIDs.txt fájlnak a CORPDC-re másolása után
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 775abc1546bc9eb93842b69edf64ac032518d66c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289778"
---
# <a name="step-2-configuring-the-corp-domain"></a>2. lépés: A CORP-tartomány konfigurálása

> [!div class="step-by-step"]
> [« 1. lépés](sp1-step1-configuring-priv-domain.md)
> [3. lépés »](sp1-step3-installing-configuring-sql.md)

A SIDs.txt a CORPDC tartományvezérlőre másolása után **nem szükséges a PRIVOnly-környezetekhez**

1. Bejelentkezés a CORPDC tartományvezérlőbe rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. A 2. menüelem kiválasztása (CORP-erdő konfigurálása)

> [!div class="step-by-step"]
> [« 1. lépés](sp1-step1-configuring-priv-domain.md)
> [3. lépés »](sp1-step3-installing-configuring-sql.md)

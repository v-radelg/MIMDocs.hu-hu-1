---
title: '2. lépés: A CORP-tartomány konfigurálása'
description: Ez a cikk a CORP-tartomány konfigurálásának második lépését ismerteti, amelyhez hozzátartozik egy parancsfájl futtatása is a SIDs.txt fájlnak a CORPDC-re másolása után
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 1215e0be4d978e879ebc09ecdd99223fb3667a85
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043834"
---
# <a name="step-2-configuring-the-corp-domain"></a>2. lépés: A CORP-tartomány konfigurálása

> [!div class="step-by-step"]
> [«1](sp1-step1-configuring-priv-domain.md)
> . lépés[3. lépés»](sp1-step3-installing-configuring-sql.md)

A SIDs.txt a CORPDC tartományvezérlőre másolása után **nem szükséges a PRIVOnly-környezetekhez**

1. Bejelentkezés a CORPDC tartományvezérlőbe rendszergazdaként
2. A PowerShell futtatása rendszergazdaként
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. A 2. menüelem kiválasztása (CORP-erdő konfigurálása)

> [!div class="step-by-step"]
> [«1](sp1-step1-configuring-priv-domain.md)
> . lépés[3. lépés»](sp1-step3-installing-configuring-sql.md)

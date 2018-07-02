---
title: Dinamikus naplózás a MIM szolgáltatásban | Microsoft Docs
description: A MIM szolgáltatás dinamikus naplózásának engedélyezése a felügyeleti szolgáltatás újraindítása nélkül
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: 35d210b06a1e58b3b8f4f08677c2a4151f540246
ms.sourcegitcommit: 88d4e41d8d57f44f4c6c4468fdbd37c2d7e91fd5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36957539"
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Dinamikus naplózás a MIM SP1-es (4.4.1436.0-s) verziójában
A 4.4.1436.0-s verzióban új naplózási funkció mutatkozott be, amellyel a rendszergazdák és a támogatási szakemberek a felügyeleti szolgáltatás újraindítása nélkül kapcsolhatják be a naplózást.

A telepítés után a  Microsoft.ResourceManagement.Service.exe.config fájlban a következő új sorok lesznek láthatóak:

*   Sor 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Sor 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Sor 266 ``</system.diagnostics> ``

![Az új, dinamikus naplózási bejegyzéseket mutató kiemelt szakaszok](media/mim-service-dynamic-logging/screen01.png)

A dinamikus naplózás szintjeit [itt](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3) soroljuk fel.

- Critical (Kritikus) = az alapértelmezett szintű szolgáltatás csak a kritikus eseményeket naplózza
- A 8. sorba (dynamicLogging mode="true" loggingLevel="Critical") írja be a kívánt naplózási szint értékét

Dinamikus naplózási konfiguráció található sor 266: Microsoft.ResourceManagement.Service.exe.config

![A különféle elérhető naplózási területeket mutató kiemelt szakaszok](media/mim-service-dynamic-logging/screen02.png)

Alapértelmezés szerint a naplózás helye lesz a ** C:\Program Files\Microsoft Forefront Identity Manager\2010\Service, a FIM szolgáltatás fiókkal kell írási engedélye ezen a helyen, a dinamikus napló létrehozásához.

![A naplókat tároló mappa](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  Nem várt hibák (például a Microsoft.ResourceManagement.Service.exe.config fájlbeli szintaxishibák) esetében a vonatkozó hibaüzenet a Microsoft.ResourceManagement.Service.exe_Emergency.log fájlba kerül, a %TMP%, a %TEMP% vagy a %USERPROFILE% mappába (a felsoroltak közül az első létezőbe).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

A nyomkövetés megtekintéséhez használja a [szolgáltatás nyomkövetési megjelenítőjének](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![A Service Trace Viewer képernyőképe](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Frissítések: Build 4.5.x.x vagy gyorsabb

A build 4.5.x.x kideríti, a naplózási szolgáltatás az alapértelmezett naplózási szint megadásához a **"Figyelmeztetés"**. A szolgáltatás írási üzenetek két fájlt (a "00" és "01" indexek kiterjesztése előtt kerülnek). A fájlok "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" könyvtárban találhatók. Ha a fájl nagyobb a megengedettnél egy másik fájlba írja be a szolgáltatás indításakor. Ha egy másik fájl létezik, felülírja. Alapértelmezett maximális a fájl mérete 1 GB. Módosítsa az alapértelmezett maximális méretét, nem kell hozzáadnia **"maxOutputFileSizeKB"** fájl maximális méretét kilobájtban figyelő be értékű paraméter (lásd a példát alább), és indítsa újra a MIM szolgáltatás. A szolgáltatás indításakor azt hozzáfűzi a legújabb fájl a naplók (ha meghaladja a korlátozást terület azt a legrégebbi fájl felülírása). 

> [!NOTE] Fájl méretének szolgáltatás ellenőrzése előtt az üzenet írása,-fájl mérete lehet nagyobb, mint egy üzenet mérete legfeljebb. Alapértelmezés szerint a naplók mérete körülbelül 6 GB lehet (három > figyelők két fájlt egy GB méretű esetében).

> [!NOTE] A szolgáltatásfióknak engedélye kell írni > "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" > címtár. Abban az esetben, ha a fiók nem rendelkezik olyan jogokat is, a > fájlok nem jönnek létre.

Példa svclog fájlok 200 MB (200 * 1024 KB) és 100 MB maximális méretének beállítása * (100 * 1024 KB) txt-fájlok

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`

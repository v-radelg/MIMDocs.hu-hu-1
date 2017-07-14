---
title: "Dinamikus naplózás a MIM szolgáltatásban | Microsoft Docs"
description: "A MIM szolgáltatás dinamikus naplózásának engedélyezése a felügyeleti szolgáltatás újraindítása nélkül"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.translationtype: MT
ms.sourcegitcommit: 1ff73d0bdfcbcb4ab79d0d81feca9abdc33f9213
ms.openlocfilehash: 1e2fb9a9ae508ab601ebad1dec7acc21dc44d13e
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017



---
# Dinamikus naplózás a MIM SP1-es (4.4.1436.0-s) verziójában
<a id="mim-sp1-4414360--service-dynamic-logging" class="xliff"></a>
A 4.4.1436.0-s verzióban új naplózási funkció mutatkozott be, amellyel a rendszergazdák és a támogatási szakemberek a felügyeleti szolgáltatás újraindítása nélkül kapcsolhatják be a naplózást.

A telepítés után a  Microsoft.ResourceManagement.Service.exe.config fájlban a következő új sorok lesznek láthatóak:

*   Sor 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Sor 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Sor 266 ``</system.diagnostics> ``

![Az új, dinamikus naplózási bejegyzéseket mutató kiemelt szakaszok](media/mim-service-dynamic-logging/screen01.png)

A dinamikus naplózás szintjeit [itt](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3) soroljuk fel.

- Critical (Kritikus) = az alapértelmezett szintű szolgáltatás csak a kritikus eseményeket naplózza
- A 8. sorba (dynamicLogging mode="true" loggingLevel="Critical") írja be a kívánt naplózási szint értékét

A dinamikus naplózás konfigurációja a 266. sorban: Microsoft.ResourceManagement.Service.exe.config

![A különféle elérhető naplózási területeket mutató kiemelt szakaszok](media/mim-service-dynamic-logging/screen02.png)

A naplózás alapértelmezett helye a **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** mappa. A dinamikus napló létrehozásához a FIM-szolgáltatásfióknak írási engedélyt kell adni erre a helyre.

![A naplókat tároló mappa](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 Nem várt hibák (például a Microsoft.ResourceManagement.Service.exe.config fájlbeli szintaxishibák) esetében a vonatkozó hibaüzenet a Microsoft.ResourceManagement.Service.exe_Emergency.log fájlba kerül, a %TMP%, a %TEMP% vagy a %USERPROFILE% mappába (a felsoroltak közül az első létezőbe).  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

A kivonatot a [Service Trace Viewer eszközzel](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx) nézheti meg

 ![A Service Trace Viewer képernyőképe](media/mim-service-dynamic-logging/screen04.png)


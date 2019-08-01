---
title: Dinamikus naplózás a MIM szolgáltatásban | Microsoft Docs
description: A MIM szolgáltatás dinamikus naplózásának engedélyezése a felügyeleti szolgáltatás újraindítása nélkül
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 90ef2ab63be3914d1d48c7319821177e7e62f9e0
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701298"
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

Dinamikus naplózási konfiguráció a 266. sorban: Microsoft. ResourceManagement. Service. exe. config

![A különféle elérhető naplózási területeket mutató kiemelt szakaszok](media/mim-service-dynamic-logging/screen02.png)

Alapértelmezés szerint a naplózási hely a * * C:\Program Files\Microsoft Forefront Identity Manager\2010\service mappában lesz, a FIM-szolgáltatásfiók írás engedélyre van szüksége erre a helyre a dinamikus napló létrehozásához.

![A naplókat tároló mappa](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  Nem várt hibák (például a Microsoft.ResourceManagement.Service.exe.config fájlbeli szintaxishibák) esetében a vonatkozó hibaüzenet a Microsoft.ResourceManagement.Service.exe_Emergency.log fájlba kerül, a %TMP%, a %TEMP% vagy a %USERPROFILE% mappába (a felsoroltak közül az első létezőbe).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

A nyomkövetés megtekintéséhez használhatja a [Service Trace Viewer eszközt](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![A Service Trace Viewer képernyőképe](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Frissítések Build 4.5. x. x vagy újabb

A Build 4.5. x. x verzióban módosítottuk a naplózási szolgáltatást az alapértelmezett naplózási szint megadásához: **"figyelmeztetés"** . A szolgáltatás két fájlban ír üzeneteket (a "00" és a "01" indexeket a bővítmény előtt adja hozzá). A fájlok a "C:\Program Files\Microsoft Forefront Identity Manager\2010\service mappában" könyvtárban találhatók. Ha a fájl túllépi a maximális méretet, a szolgáltatás elkezd írni egy másik fájlban. Ha egy másik fájl létezik, a rendszer felülírja. A fájl alapértelmezett maximális mérete 1 GB. Az alapértelmezett maximális méret módosításához hozzá kell adni a **"maxOutputFileSizeKB"** paramétert, amelynek értéke a maximális fájlméret a figyelőben (lásd az alábbi példát) és a újraindítása szolgáltatást. A szolgáltatás elindításakor a rendszer hozzáfűzi a naplókat a legutóbbi fájlban (ha túllépte a lemezterületet, felülírja a legrégebbi fájlt). 

> [!NOTE] 
> Mivel a szolgáltatás az üzenet megírása előtt megtekinti a fájl méretét, a fájl mérete nagyobb lehet, mint egy üzenet méretének maximális mérete. Alapértelmezés szerint a naplók mérete körülbelül 6 GB lehet (három > figyelő, amelynek két fájlja egy GB méretű).

> [!NOTE] 
> A szolgáltatásfiók engedélyekkel kell rendelkeznie a "C:\Program Files\Microsoft Forefront Identity Manager\2010\service mappában" > Directory > való íráshoz. Ha a szolgáltatásfiók nem rendelkezik ilyen jogosultságokkal, a rendszer nem hozza létre a > fájlokat.

Példa a maximális fájlméret 200 MB-ra (200 * 1024 KB) való beállítására a svclog-fájlokhoz és az 100 MB * (100 * 1024 KB) txt-fájlokhoz

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`

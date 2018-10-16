---
title: Dinamikus naplózás a MIM szolgáltatásban | Microsoft Docs
description: A MIM szolgáltatás dinamikus naplózásának engedélyezése a felügyeleti szolgáltatás újraindítása nélkül
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: ff82b2fce31abe417509347ce7b477dd1b4056f2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332337"
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

Dinamikus naplózás konfigurációja a 266. sorban található: Microsoft.ResourceManagement.Service.exe.config

![A különféle elérhető naplózási területeket mutató kiemelt szakaszok](media/mim-service-dynamic-logging/screen02.png)

Alapértelmezés szerint a naplózás helye lesz a ** C:\Program Files\Microsoft Forefront Identity Manager\2010\Service, a FIM szolgáltatás fiókkal kell írási engedélye a dinamikus napló létrehozásához ezen a helyen.

![A naplókat tároló mappa](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  Nem várt hibák (például a Microsoft.ResourceManagement.Service.exe.config fájlbeli szintaxishibák) esetében a vonatkozó hibaüzenet a Microsoft.ResourceManagement.Service.exe_Emergency.log fájlba kerül, a %TMP%, a %TEMP% vagy a %USERPROFILE% mappába (a felsoroltak közül az első létezőbe).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

A nyomkövetés megtekintéséhez használja a [Service Trace viewer eszközzel](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![A Service Trace Viewer képernyőképe](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Frissítések: Build 4.5.x.x vagy újabb

A build 4.5.x.x kideríti, hogy a naplózási szolgáltatás az alapértelmezett naplózási szint megadásához a **"Figyelmeztetés"**. A szolgáltatás két fájlt (a "00" és "01" indexek bővítmény előtt kerülnek) ír üzeneteket. A fájlokat a "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" könyvtárban találhatók. Ha a fájl maximális mérete meghaladja a szolgáltatás elindul egy másik fájlba írja. Ha egy másik fájl létezik, felülírja. A fájl alapértelmezett maximális mérete 1 GB-os. Alapértelmezett maximális méretének módosításához hozzáadásához szükség a **"maxOutputFileSizeKB"** paraméter értékével, a figyelő a KB-os maximális fájlméret (lásd az alábbi példát), majd indítsa újra a MIM szolgáltatás. A szolgáltatás indításakor azt fűzi hozzá újabb fájl-naplók (lemezterület-korlát túllépése azt felülírni a legrégebbi fájl). 

> [!NOTE] A szolgáltatás ellenőrzése fájl méretét, az üzenet íródik, mielőtt fájl mérete lehet nagyobb, mint a maximális méretét, egy üzenet mérete a. Alapértelmezés szerint a naplók mérete körülbelül 6 GB is lehet (három > figyelő, egy GB-os méret esetén két fájlt).

> [!NOTE] A szolgáltatásfiókot írási jogosultsággal kell rendelkeznie > "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" > könyvtár. Abban az esetben, ha a fiók nem rendelkezik ilyen jogosultságokkal a > fájlok nem lesz létrehozva.

Például hogyan állíthatja be a maximális mérete 200 MB-ot (200-as * 1024 KB) svclog fájlok és 100 MB-os * (100 * 1024 KB-os) a txt-fájlokat

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`

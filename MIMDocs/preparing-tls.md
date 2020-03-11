---
title: Microsoft Identity Manager 2016 megtervezése a TLS 1,2 környezetben | Microsoft Docs
description: Microsoft Identity Manager 2016 megtervezése TLS 1,2 környezetben
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f8d0be0cb9ffa0f32415f11b407954cb0c985024
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043443"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>A 2016 SP2 megtervezése a TLS 1,2 vagy a FIPS módú környezetekben


> [!IMPORTANT]
> Ez a cikk csak a webalkalmazás-2016 SP2-re vonatkozik

Ha a következő követelmények érvényesek az összes titkosítási protokollt tartalmazó, de a TLS 1,2-et letiltó lezárt környezetbe, a 2016 SP2 telepítése után:
- A biztonságos TLS 1,2-kapcsolatok létrehozásához a Windows Server és a .NET-keretrendszer legújabb frissítései szükségesek, amelyek lehetővé teszik a TLS 1,2-támogatás telepítését a .NET 3,5-keretrendszerben. A kiszolgáló konfigurációjától függően *Előfordulhat* , hogy engedélyeznie kell a [SystemDefaultTlsVersions a .net-keretrendszerhez](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) , és/vagy [le kell tiltania az összes Schannel protokollt, kivéve](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) az *ügyfél* -és a *kiszolgálói* alkulcsokban lévő beállításjegyzék 1,2-es verzióját.

## <a name="mim-synchronization-service-sql-ma"></a>Többkiszolgálós szinkronizációs szolgáltatás, SQL MA

- Az SQL Serverrel való biztonságos TLS 1,2-kapcsolat létrehozásához a 11.0.7001.0 és a beépített SQL felügyeleti ügynöknek [SQL Native Client](https://www.microsoft.com/download/details.aspx?id=50402) vagy újabb rendszerre van szüksége.

## <a name="mim-service"></a>MIM szolgáltatás
- Az önaláírt tanúsítványokat csak a TLS 1,2-környezetekben lehet használni a fakiszolgálói szolgáltatás. A Rendszerfelügyeleti webszolgáltatások telepítésekor válassza a megbízható hitelesítésszolgáltató által kibocsátott erős titkosítási kompatibilis tanúsítvány elemet.
- A következő SQL Server 18,2-es vagy újabb verziójú rendszerhez [OLE DB illesztőprogram szükséges:](https://www.microsoft.com/download/details.aspx?id=56730)

## <a name="fips-mode-considerations"></a>FIPS-mód szempontjai

Ha a rendszerállapot-érvényesítő szolgáltatást olyan kiszolgálóra telepíti, amelynek FIPS-üzemmódja engedélyezve van, le kell tiltania a FIPS-szabályzat érvényesítését, hogy lehetővé váljon a rendszerállapot-szolgáltatási munkafolyamatok végrehajtása Ehhez adja hozzá a *enforceFIPSPolicy enabled = false* elemet a *Microsoft. ResourceManagement. Service. exe. config* fájl *futtatókörnyezet* *szakaszához* az alábbi ábrán látható módon:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    

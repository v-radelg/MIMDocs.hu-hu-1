---
title: Támogatott összekötők | Microsoft Docs
description: Összekötők használatával a MIM és a csatlakoztatott adatforrások közötti adatátvitel kezeléséhez.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 1/24/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 023232b9ddb3cb0a299cbc14ab4c311b8c63fc47
ms.sourcegitcommit: fa30a8eb9c3a7f1ed6f8ce0f67362ca32751e00d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/22/2019
ms.locfileid: "56667199"
---
# <a name="connect-to-your-directories"></a>Csatlakozás a címtárakhoz <!--accepted-->

Összekötők a Microsoft Identity Manager SP1 (MIM) hivatkozás az adott csatlakoztatott adatforrásokat. A csatlakoztatott adatforrásból az összekötő adatokat helyez át az MIM alkalmazásba. Ha az MIM alkalmazásban az adatok módosulnak, az összekötő exportálja az adatokat a csatlakoztatott adatforráshoz, hogy szinkronizálja azt az MIM alkalmazással. Általában elmondható, hogy minden csatlakoztatott címtárhoz legalább egy összekötő tartozik.

A Forefront Identity Manager szoftver az összekötőket kezelőügynöknek nevezte. Ez a megnevezés továbbra is használatos néhány cikkben és a termék egyes részeiben, de mindkét kifejezés ugyanazt a fogalmat fedi.

Ez a cikk ismerteti az összekötő tartalmazza a & a MIM-ben támogatott, de az Extensible Connectivity 2.0-összekötő lehetővé teszi a további adatforrásokhoz is kapcsolódhat. Egyes partnerek saját összekötőket hoztak létre ezzel a módszerrel, és a egy teljes listája megtalálható a wikin [FIM 2010: Partnerek Kezelőügynökei](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>A MIM 2016 SP1 támogatott összekötők <!--rejected-->

| Name (Név) | A csatlakoztatott adatforrás & műszaki hivatkozások támogatott verziói |
| ---- | ----------------------------------------------- |
| Active Directory tartományi szolgáltatások | A Windows Server 2012 és Windows Server 2016 Active Directory |
| Active Directory Lightweight Directory-szolgáltatások (ADLDS) | Active Directory Lightweight Directory-szolgáltatások (ADLDS) |
| Active Directory globális címlista (GAL) | Az Active Directory globális címlista (GAL) – az Exchange 2013, az Exchange 2016 |
| Extensible Connectivity 2.0 | Bármely hívás- vagy fájlalapú adatforrás |
| FIM szolgáltatás | A MIM szolgáltatás. Vegye figyelembe, hogy a MIM Synchronization Service és a MIM szolgáltatás az azonos verziójúnak kell lennie. |
| IBM DB2 Universal Database | 9,5 vagy 9.7; IBM DB2-verzió IBM DB2 OLEDB v9.5 FP5 vagy v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory 8.7.3, 8.8.5 és 8.8.6 |
| Oracle Database | Oracle Database 10g vagy 11g; 64 bites ügyfél |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Oracle (korábban Sun és Netscape) Directory Server kiszolgálók | Sun Directory Server 6.x, 7.x és Oracle 11 |
| [Windows PowerShell-összekötő](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 vagy újabb |
| [A Microsoft Azure Active Directory-összekötő](https://msdn.microsoft.com/library/dn511001.aspx) | A Microsoft Azure Active Directory (az új központi telepítéseknél nem ajánlott) |
| [Általános LDAP-összekötő](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3-as kiszolgáló (RFC 4510 szabványnak megfelelő)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Általános SQL-összekötő](./reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Az összekötő támogatott minden 64 bites ODBC-illesztőprogram](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql.md) |
| [Lotus Domino-összekötő](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes kiadási v8.5.x kiadás |
| [SharePoint Services-összekötő UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013 vagy 2016 felhasználóiprofil-szolgáltatási alkalmazással (UPA) |
| [Web Services-összekötő](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 vagy 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 és más SOAP és REST API-k](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Attribútum-érték párokból álló szövegfájl](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Attribútum-érték párokból álló szövegfájlok |
| [Tagolt szövegfájl](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Tagolt szövegfájlok |
| [Címtárszolgáltatások felár nyelv (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Rögzített szélességű szövegfájl](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Rögzített szélességű szövegfájlok |
| [LDAP Data Interchange formátum (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange formátum (LDIF) |
| [A Microsoft Graph-összekötő](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Kapcsolódó témakörök

[A FIM 2010 R2 kezelőügynökei](https://technet.microsoft.com/library/jj133885.aspx)

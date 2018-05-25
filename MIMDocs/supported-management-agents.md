---
title: Támogatott összekötők | Microsoft Docs
description: Összekötők használatával a MIM és a csatlakoztatott adatforrások közti adatátvitel felügyelete.
keywords: ''
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 1/24/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 7b685ffb6f2a52bd2782395e4c1f26501ffe3101
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/25/2018
---
# <a name="connect-to-your-directories"></a>Csatlakozás a címtárakhoz

Összekötők a Microsoft Identity Manager SP1 (MIM) hivatkozásra az adott csatlakoztatott adatforrásokat. A csatlakoztatott adatforrásból az összekötő adatokat helyez át az MIM alkalmazásba. Ha az MIM alkalmazásban az adatok módosulnak, az összekötő exportálja az adatokat a csatlakoztatott adatforráshoz, hogy szinkronizálja azt az MIM alkalmazással. Általában elmondható, hogy minden csatlakoztatott címtárhoz legalább egy összekötő tartozik.

A Forefront Identity Manager szoftver az összekötőket kezelőügynöknek nevezte. Ez a megnevezés továbbra is használatos néhány cikkben és a termék egyes részeiben, de mindkét kifejezés ugyanazt a fogalmat fedi.

Ez a cikk ismerteti a tartalmazza a & a MIM-ben támogatott összekötők, de az Extensible Connectivity 2.0-összekötő lehetővé teszi a további adatforrásokhoz való kapcsolódáshoz. Egyes partnerek saját összekötőket hoztak létre, melyek teljes listája megtalálható a [FIM 2010: Partnerek kezelőügynökei](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) wikicikkben.

## <a name="supported-connectors-in-mim-2016-sp1"></a>A MIM 2016 SP1 által támogatott összekötők

| Név | A csatlakoztatott adatforrás & műszaki hivatkozások támogatott verziói |
| ---- | ----------------------------------------------- |
| Active Directory tartományi szolgáltatások | Active Directory 2012-ben 2016 |
| Active Directory Lightweight Directory-szolgáltatások (ADLDS) | Active Directory Lightweight Directory-szolgáltatások (ADLDS) |
| Active Directory globális címlista (GAL) | Az Active Directory globális címlista (GAL) – Exchange 2013 2016 |
| Extensible Connectivity 2.0 | Bármely hívás- vagy fájlalapú adatforrás |
| FIM szolgáltatás | FIM Service Management Agent (szinkronizálási szolgáltatás) el kell érnie a "Forefront Identity Manager szolgáltatás" telepített ugyanazon verzióját |
| IBM DB2 Universal Database | 9,5 vagy 9.7; IBM DB2-verzió IBM DB2 OLEDB v9.5 FP5 vagy v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory 8.7.3, 8.8.5 és 8.8.6 |
| Oracle Database | Oracle Database 10g vagy 11g; 64 bites ügyfél |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Oracle (korábban Sun és Netscape) Directory Server kiszolgálók | Sun Directory Server 6.x, 7.x és Oracle 11 |
| [Windows PowerShell-összekötő FIM 2010 R2 szoftverhez](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 vagy újabb |
| [Microsoft Azure Active Directory-összekötő FIM 2010 R2 szoftverhez](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Általános LDAP-összekötő FIM 2010 R2 szoftverhez](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3-as kiszolgáló (RFC 4510 szabványnak megfelelő)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Általános SQL-összekötő FIM 2010 R2 / MIM](https://msdn.microsoft.com/library/dn510997.aspx) | [Az összekötő támogatott 64 bites minden ODBC-illesztőprogram](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql) |
| [Lotus Domino-összekötő](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes kiadás v8.5.x kiadás |
| [SharePoint Services-összekötő UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013 vagy 2016 felhasználóiprofil-szolgáltatási alkalmazással (UPA) |
| [Web Services-összekötő](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 vagy 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Attribútum-érték párokból álló szövegfájl](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Attribútum-érték párokból álló szövegfájlok |
| [Tagolt szövegfájl](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Tagolt szövegfájlok |
| [Címtárszolgáltatások ugyanolyan nyelvi (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Rögzített szélességű szövegfájl](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Rögzített szélességű szövegfájlok |
| [LDAP Data Interchange formátum (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange formátum (LDIF) |

## <a name="related-topics"></a>Kapcsolódó témakörök

[A FIM 2010 R2 kezelőügynökei](https://technet.microsoft.com/library/jj133885.aspx)

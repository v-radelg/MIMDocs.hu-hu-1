---
title: "Támogatott összekötők | Microsoft Docs"
description: "Kezelje az MIM alkalmazás és a címtárai közötti adatátvitelt összekötők használatával."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: b26fe7bc56ab8229054afb1409c3652e81464a3d
ms.lasthandoff: 05/02/2017


---

# <a name="connect-to-your-directories"></a>Csatlakozás a címtárakhoz

Az összekötők adott csatlakoztatott adatforrásokat kapcsolnak össze a Microsoft Identity Manager (MIM) alkalmazással. A csatlakoztatott adatforrásból az összekötő adatokat helyez át az MIM alkalmazásba. Ha az MIM alkalmazásban az adatok módosulnak, az összekötő exportálja az adatokat a csatlakoztatott adatforráshoz, hogy szinkronizálja azt az MIM alkalmazással. Általában elmondható, hogy minden csatlakoztatott címtárhoz legalább egy összekötő tartozik.

A Forefront Identity Manager szoftver az összekötőket kezelőügynöknek nevezte. Ez a megnevezés továbbra is használatos néhány cikkben és a termék egyes részeiben, de mindkét kifejezés ugyanazt a fogalmat fedi.

Ez a cikk az MIM részét képző összekötőkre vonatkozik, de az Extensible Connectivity 2.0 összekötőivel további adatforrásokhoz is lehet csatlakozni. Egyes partnerek saját összekötőket hoztak létre, melyek teljes listája megtalálható a [FIM 2010: Partnerek kezelőügynökei](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) wikicikkben.

## <a name="supported-connectors-in-mim-2016"></a>Az MIM 2016 által támogatott összekötők

| Név | A csatlakoztatott adatforrás támogatott verziói |
| ---- | ----------------------------------------------- |
| Active Directory tartományi szolgáltatások | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Active Directory Lightweight Directory-szolgáltatások (ADLDS) | Active Directory Lightweight Directory-szolgáltatások (ADLDS) |
| Active Directory globális címlista (GAL) | Active Directory globális címlista (GAL) – Exchange 2000 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Bármely hívás- vagy fájlalapú adatforrás |
| MIM szolgáltatás | Microsoft Docs 2016 |
| IBM DB2 Universal Database | IBM DB2 9.1, 9,5 vagy 9.7; IBM DB2 OLEDB v9.5 FP5 vagy v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory 8.7.3, 8.8.5 és 8.8.6 |
| Oracle Database | Oracle Database 10g vagy 11g; 64 bites ügyfél |
| Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Oracle (korábban Sun és Netscape) Directory Server kiszolgálók | Sun Directory Server 6.x, 7.x és Oracle 11 |
| [Windows PowerShell-összekötő FIM 2010 R2 szoftverhez](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 vagy újabb |
| [Microsoft Azure Active Directory-összekötő FIM 2010 R2 szoftverhez](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Általános LDAP-összekötő FIM 2010 R2 szoftverhez](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3-as kiszolgáló (RFC 4510 szabványnak megfelelő) |
| [Lotus Domino-összekötő](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes v8.0.x vagy v8.5.x kiadás |
| [SharePoint Services-összekötő FIM 2010 R2 szoftverhez](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint Server 2013 vagy 2016 felhasználóiprofil-szolgáltatási alkalmazással (UPA) |
| [Web Services-összekötő](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 vagy 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| Attribútum-érték párokból álló szövegfájl | Attribútum-érték párokból álló szövegfájlok |
| Tagolt szövegfájl | Tagolt szövegfájlok |
| Directory Services Mark-up Language (DSML) | Directory Services Markup Language (DSML) 2.0 |
| Rögzített szélességű szövegfájl | Rögzített szélességű szövegfájlok |
| LDAP Data Interchange formátum (LDIF) | LDAP Data Interchange formátum (LDIF) |

## <a name="related-topics"></a>Kapcsolódó témakörök

[A FIM 2010 R2 kezelőügynökei](https://technet.microsoft.com/library/jj133885.aspx)


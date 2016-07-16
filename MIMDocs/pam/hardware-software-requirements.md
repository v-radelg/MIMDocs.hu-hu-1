---
title: "Hardver- és szoftverkövetelmények | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 77e7174e94ea8032c4e57155db489f493ce18177


---

A mögöttes szoftverplatformok rendszerkövetelményein, az elegendő memórián és lemezterületen, illetve a hálózati kapcsolaton kívül nincs további hardverkövetelmény. Ez a cikk az alapszintű telepítéshez szükséges minimális követelményeket ismerteti. Nem célja a teljesítmény, a skálázhatóság és a magas rendelkezésre állás bemutatása, és nem ismerteti a nagyvállalati vagy üzemi környezetben végzett telepítésekhez javasolt topológiát.

## Telepítés szoftvercsomagokból

A következő szoftver letölthető a TechNet Evaluation Center vagy az MSDN webhelyéről:  
- Microsoft Identity Manager 2016
  - Szolgáltatás és portál: tartalmazza a MIM szolgáltatáshoz és a MIM-portálhoz, illetve a PAM-telepítéshez szükséges telepítőfájlokat
  - Beépülő modulok és bővítmények: tartalmazzák a kérelmező PowerShell-parancsmagok telepítőjét

A következő szoftver letölthető a GitHub webhelyről:  
- PAMSamplePortal: tartalmazza a REST API minta-webalkalmazását

## Szükséges szoftverek

- Windows Server 2012 R2  
- Windows 8.1 Enterprise vagy Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 vagy SQL Server 2014  

## Próbaszoftver

Ha Ön nem rendelkezik Windows-, SQL Server- vagy Windows Server-licenccel, letöltheti ezek próbaverzióit.

### TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft letöltőközpont

- [SQL-kiszolgáló](https://www.microsoft.com/download/details.aspx?id=29066)  
- [A SharePoint Foundation 2013 SP1 és előfeltételei](https://www.microsoft.com/download/details.aspx?id=42039)

## Hardverkövetelmények

Minden PAM-összetevő esetében tekintse át a szoftverek rendszerkövetelményeit.

A CORPDC géphez:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) vagy újabb

A CORPWKSTN géphez:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

A PRIVDC géphez:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

A PAMSRV géphez:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) vagy [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)



<!--HONumber=Jun16_HO3-->



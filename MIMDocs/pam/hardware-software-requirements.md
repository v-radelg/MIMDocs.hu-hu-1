---
title: "A PAM szoftverkövetelményei | Microsoft Docs"
description: "A Privileged Access Management sikeres üzembe helyezéséhez szükséges hardver- és szoftverkövetelmények"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2985215821db843d2f90d8a34250a8ca6a84b592
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="hardware-and-software-requirements"></a>Hardver- és szoftverkövetelmények

A mögöttes szoftverplatformok rendszerkövetelményein kívül a Privileged Access Management nem rendelkezik további hardverkövetelményekkel. Ügyeljen rá, hogy rendelkezésre álljon elegendő memória vagy lemezterület, és hogy elérhető legyen a hálózati kapcsolat.

Ez a cikk az alapszintű telepítéshez szükséges minimális követelményeket ismerteti. Nem célja a teljesítmény, a skálázhatóság és a magas rendelkezésre állás bemutatása, és nem ismerteti a nagyvállalati vagy üzemi környezetben végzett telepítésekhez javasolt topológiát.

## <a name="installing-from-software-packages"></a>Telepítés szoftvercsomagokból

A következő szoftver letölthető a TechNet Evaluation Center vagy az MSDN webhelyéről:  
- Microsoft Identity Manager 2016
  - Szolgáltatás és portál: tartalmazza a MIM szolgáltatáshoz és a MIM-portálhoz, illetve a PAM-telepítéshez szükséges telepítőfájlokat
  - Beépülő modulok és bővítmények: tartalmazzák a kérelmező PowerShell-parancsmagok telepítőjét

A következő szoftver letölthető a GitHub webhelyről:  
- PAMSamplePortal: tartalmazza a REST API minta-webalkalmazását

## <a name="required-software"></a>Szükséges szoftverek

- Windows Server 2012 R2  
- Windows 8.1 Enterprise vagy Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 vagy SQL Server 2014  

## <a name="evaluation-software"></a>Próbaszoftver

Ha Ön nem rendelkezik Windows-, SQL Server- vagy Windows Server-licenccel, letöltheti ezek próbaverzióit.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### <a name="microsoft-download-center"></a>Microsoft letöltőközpont

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [A SharePoint Foundation 2013 SP1 és előfeltételei](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Hardverkövetelmények

Minden PAM-összetevő esetében tekintse át a szoftverek rendszerkövetelményeit.

A CORPDC géphez:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) vagy újabb

A CORPWKSTN géphez:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

A PRIVDC géphez:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

A PAMSRV géphez:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) vagy [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)

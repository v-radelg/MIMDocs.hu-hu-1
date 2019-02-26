---
title: Támogatott szoftverplatformok | Microsoft Docs
description: A MIM 2016 összetevői által támogatott termékek és termékverziók
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: c6da739349f8c4ba4016635e326ed30d5f5954c6
ms.sourcegitcommit: 67e2de99f86e762125979233f6ee80afcd78dc4d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/25/2019
ms.locfileid: "56795417"
---
# <a name="supported-platforms-for-mim-2016"></a>A MIM 2016 által támogatott platformok

Ez a tábla a Microsoft Identity Manager 2016 egyes összetevői által támogatott platformokat és verziókat ismerteti. A verziók jelölve egy * csak a MIM 2016 service pack 1 a legújabb gyorsjavítások támogatottak.  A kijelölt "NR", a verziók nem ajánlott, támogatottak, de használata nem ajánlott, ha a MIM platformra friss telepítését indítása.


| **MIM-összetevő** | **Platform** | **Verzió** |
|-------------------|--------------|--------------|
| **MIM Sync** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>A Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016* |
| | Az Active Directory működési szintjét a felhasználók átadásához, a jelszóváltoztatás-értesítési szolgáltatás és a globális Címlista szinkronizálásához | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016*
| | MIM Sync-adatbázis | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016* |
| | Az Active Directory-felhasználók létrehozásának, a jelszóváltoztatás-értesítési szolgáltatás és a globális Címlista szinkronizálásához (opcionális)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Exchange a postaládák létrehozásához és a globális címlista szinkronizálásához (opcionális)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 * |
| | Fejlesztői környezet (opcionális) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | További csatlakoztatott rendszer (opcionális) | Active Directory tartományi szolgáltatások<br/>Active Directory<br/>Lightweight Directory-szolgáltatások<br/>SQL Server 2008 vagy újabb<br/>SharePoint Server 2013<br/> SharePoint Server 2016* <br/> Egyéb külső termékek |
| **MIM szolgáltatás és -portál** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>A Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| |A PAM-forgatókönyv:  Windows Server | A Windows Server 2012 R2 (NR) <br/> Windows Server 2016* |
| |A PAM-forgatókönyv: Active Directory bástyakörnyezeti PAM-erdőhöz | A Windows Server 2012 R2 (NR) <br/> Windows Server 2016* |
| |A PAM-forgatókönyv: Az Active Directory PAM forgatókönyv meglévő (vállalat) erdőkhöz | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016* |
| | MIM szolgáltatás adatbázisa | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016* |
| | Levelezőkiszolgáló a MIM szolgáltatás jóváhagyási és csoportkezelési e-mailjeihez (opcionális) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Az Exchange Online * (értesítés csak előtt build [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490) |
| | Böngésző | Az összes fő támogatott böngészők * (mobil eszközök korlátozott)|
| **Jelentéskészítés a MIM szolgáltatással** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>A Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Adatraktár | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (4.4.1459-es verzióval)<br/> [A System Center 2016-tal kompatibilis SQL Server-verziók](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016) |
| **A MIM jelszó-változtatási és -regisztrálási portáljai** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>A Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Webböngésző | Az összes fő támogatott böngészők |
| **A MIM beépülő moduljai és bővítményei** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-integráció (opcionális) | Az Outlook 2010 (a Windows, kivéve a kattintásra)<br/>Az Outlook 2013 (a Windows, kivéve a kattintásra) <br/> Az Outlook 2016 (a Windows 10, kivéve a kattintásra) * |
| | A PAM PowerShell-kérelmező parancsmagjai (opcionális) | Windows 8.1<br/>Windows 10 |
| **MIM Tanúsítványkezelő** (kiszolgáló és hitelesítésszolgáltató integrációja) | Windows server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Hitelesítésszolgáltató | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | A MIM Tanúsítványkezelő adatbázisa | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016* |
| **MIM Tanúsítványkezelő** (alkalmazás) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Tanúsítványkezelő** (csoportos ügyfél) | Windows | Windows 7 |
| **MIM Tanúsítványkezelő** (ügyfél ActiveX-alapú intelligens kártyás) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | BHOLD-adatbázis | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2 <br/> SQL Server 2014* <br/> SQL Server 2016* |
| | Levelezőkiszolgáló (opcionális) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webböngésző | Az Internet Explorer böngésző Silverlight-támogatott |

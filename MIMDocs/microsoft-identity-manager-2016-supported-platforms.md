---
title: Támogatott szoftverplatformok | Microsoft Docs
description: A MIM 2016 összetevői által támogatott termékek és termékverziók
keywords: ''
author: fimguy
ms.author: davidste
manager: davidste
ms.date: 04/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 3ff2ca7b9a268d4723861fcdec7f2f11932a6026
ms.sourcegitcommit: 3502d636687e442f7d436ee56218b9b95f5056cf
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/16/2018
---
# <a name="supported-platforms-for-mim-2016"></a>A MIM 2016 által támogatott platformok

Ez a tábla a Microsoft Identity Manager 2016 egyes összetevői által támogatott platformokat és verziókat ismerteti. A verziók jelölésű egy * a MIM 2016 service pack 1 a legújabb gyorsjavítások esetén támogatottak.


| **MIM-összetevő** | **Platform** | **Verzió** |
|-------------------|--------------|--------------|
| **MIM Sync** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016* |
| | Active Directory működési szintje a felhasználók átadásához, a jelszóváltozás-értesítési Szolgáltatáshoz és a globális Címlista szinkronizálásához | Windows 2000 <br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016*
| | MIM Sync-adatbázis | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016* |
| | Az Active Directory a felhasználók átadása, a jelszóváltozás-értesítési Szolgáltatáshoz és a globális Címlista szinkronizálásához (opcionális)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Exchange a postaládák létrehozásához és a globális címlista szinkronizálásához (opcionális)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Fejlesztői környezet (opcionális) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | További csatlakoztatott rendszer (opcionális) | Active Directory tartományi szolgáltatások<br/>Active Directory<br/>Lightweight Directory-szolgáltatások<br/>SQL Server 2008 vagy újabb<br/>SharePoint Server 2013<br/> SharePoint Server 2016* <br/> Egyéb külső termékek |
| **MIM szolgáltatás és -portál** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| |PAM esetén: Windows Server | Windows Server 2012 R2 <br/> Windows Server 2016* |
| |PAM esetén: Az Active Directory bástyakörnyezeti PAM-erdőhöz | Windows Server 2012 R2 <br/> Windows Server 2016* |
| |PAM esetén: Az Active Directory PAM forgatókönyv meglévő (vállalati) erdőkhöz | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016* |
| | MIM szolgáltatás adatbázisa | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016* |
| | Levelezőkiszolgáló a MIM szolgáltatás jóváhagyási és csoportkezelési e-mailjeihez (opcionális) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (értesítés csak előtt build [4.4.1749.0](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/version-history#version-4417490) |
| | Böngésző | A fő támogatott böngészők * (mobil eszközök korlátozott)|
| **Jelentéskészítés a MIM szolgáltatással** | Windows Server |  Windows Server 2008 R2 SP1<br/>Windows Server 2012 <br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Adatraktár | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (4.4.1459-es verzióval)<br/> [A System Center 2016-tal kompatibilis SQL Server-verziók](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016)
 |
| **A MIM jelszó-változtatási és -regisztrálási portáljai** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Webböngésző | A fő támogatott böngészők |
| **A MIM beépülő moduljai és bővítményei** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-integráció (opcionális) | Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (Windows 10 rendszeren)* |
| | A PAM PowerShell-kérelmező parancsmagjai (opcionális) | Windows 8.1<br/>Windows 10 |
| **MIM Tanúsítványkezelő** (kiszolgáló és hitelesítésszolgáltató integrációja) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Hitelesítésszolgáltató | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | A MIM Tanúsítványkezelő adatbázisa | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016* |
| **MIM Tanúsítványkezelő** (alkalmazás) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Tanúsítványkezelő** (tömeges ügyfél) | Windows | Windows 7 |
| **MIM Tanúsítványkezelő** (ügyfél ActiveX alapú intelligens kártyás) | Windows | Windows 7 </br> Windows 8 </br> Windows 8.1 </br> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | BHOLD-adatbázis | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014* <br/> SQL Server 2016* |
| | Levelezőkiszolgáló (opcionális) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webböngésző | Az Internet Explorer támogatott böngészők silverlighttal |

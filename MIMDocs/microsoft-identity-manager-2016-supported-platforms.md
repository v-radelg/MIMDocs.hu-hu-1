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
ms.openlocfilehash: d4cb70ae60d23049251caa121a1834eeacb05677
ms.sourcegitcommit: 4c4bc7aa42cd5984c838abdd302490355ddcb4ea
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68238921"
---
# <a name="supported-platforms-for-mim-2016"></a>A MIM 2016 által támogatott platformok

Ez a tábla a Microsoft Identity Manager 2016 egyes összetevői által támogatott platformokat és verziókat ismerteti. A * -val jelölt verziók csak a webszolgáltatások 2016 Service Pack 1 verziójában támogatottak a legújabb gyorsjavítással.  Az "NR" jelölésű, nem javasolt verziók használata támogatott, de nem ajánlott, ha az adott platform új központi telepítését indítja.


| **MIM-összetevő** | **Platform** | **Verzió** |
|-------------------|--------------|--------------|
| **MIM Sync** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016* |
| | Active Directory működési szint a felhasználók üzembe helyezéséhez, a PCN-hez és a GAL-szinkronizáláshoz | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016*
| | MIM Sync-adatbázis | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| | Active Directory a felhasználók üzembe helyezéséhez, a PCN-hez és a GAL-szinkronizáláshoz (nem kötelező)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Exchange a postaládák létrehozásához és a globális címlista szinkronizálásához (opcionális)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 * |
| | Fejlesztői környezet (opcionális) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | További csatlakoztatott rendszer (opcionális) | Active Directory tartományi szolgáltatások<br/>Active Directory<br/>Lightweight Directory-szolgáltatások<br/>SQL Server 2008 vagy újabb<br/>SharePoint Server 2013<br/> SharePoint Server 2016* <br/> Egyéb harmadik féltől származó termékek |
| **MIM szolgáltatás és -portál** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| |PAM-forgatókönyv:  Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016* |
| |PAM-forgatókönyv: Active Directory bástyakörnyezeti PAM-erdőhöz | Windows Server 2012 R2 (NR) <br/> Windows Server 2016* |
| |PAM-forgatókönyv: Active Directory a PAM-forgatókönyv meglévő (CORP) erdőkhöz | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016* |
| | MIM szolgáltatás adatbázisa | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016* |
| | Levelezőkiszolgáló a MIM szolgáltatás jóváhagyási és csoportkezelési e-mailjeihez (opcionális) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (csak értesítés a [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)létrehozása előtt) |
| | Browser | Minden nagyobb támogatott böngésző * (mobileszközök korlátozott)|
| **Jelentéskészítés a MIM szolgáltatással** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Adatraktár | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (4.4.1459-es verzióval)<br/> [A System Center 2016-tal kompatibilis SQL Server-verziók](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016) |
| **A MIM jelszó-változtatási és -regisztrálási portáljai** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Webböngésző | Minden jelentős támogatott böngésző |
| **A MIM beépülő moduljai és bővítményei** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-integráció (opcionális) | Outlook 2010 (Windows rendszeren, a kattintások futtatását kivéve)<br/>Outlook 2013 (Windows rendszeren, a kattintások futtatását kivéve) <br/> Outlook 2016 (Windows 10 rendszeren, kattintson a futtatásra) * |
| | A PAM PowerShell-kérelmező parancsmagjai (opcionális) | Windows 8.1<br/>Windows 10 |
| **MIM Tanúsítványkezelő** (kiszolgáló és hitelesítésszolgáltató integrációja) | Windows server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | Hitelesítésszolgáltató | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | A MIM Tanúsítványkezelő adatbázisa | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| **MIM Tanúsítványkezelő** (alkalmazás) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| A felügyeleti webszolgáltatások **tanúsítványainak kezelése** (Tömeges ügyfél) | Windows | Windows 7 |
| A felügyeleti webszolgáltatások **tanúsítványainak kezelése** (Ügyfél-ActiveX-alapú intelligens kártya) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | BHOLD-adatbázis | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | Levelezőkiszolgáló (opcionális) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webböngésző | Az Internet Explorer által támogatott böngészők a Silverlight segítségével |

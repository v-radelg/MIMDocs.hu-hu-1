---
title: Támogatott szoftverplatformok | Microsoft Docs
description: A MIM 2016 összetevői által támogatott termékek és termékverziók
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/19/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 40a79d234e702ca8f98349b13687a7262ffc4a27
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044072"
---
# <a name="supported-platforms-for-mim-2016"></a>A MIM 2016 által támogatott platformok

Ez a tábla a Microsoft Identity Manager 2016 egyes összetevői által támogatott platformokat és verziókat ismerteti. A csillaggal (*) jelölt verziókat kizárólag a MIM 2016 1. szervizcsomagja támogatja. A * * * jelölésű verziók csak a webszolgáltatások 2016 Service Pack 2 vagy újabb gyorsjavításában támogatottak. Az "NR" jelölésű, nem javasolt verziók használata támogatott, de nem ajánlott, ha az adott platform új központi telepítését indítja.


| **MIM-összetevő** | **Platform** | **Verzió** |
|-------------------|--------------|--------------|
| **MIM Sync** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016*<br/>Windows Server 2019 * * |
| | Active Directory működési szint a felhasználók üzembe helyezéséhez, a PCN-hez és a GAL-szinkronizáláshoz | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016*
| | MIM Sync-adatbázis | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/>SQL Server 2016 SP2 *<br/>SQL Server 2017 * * |
| | Active Directory a felhasználók üzembe helyezéséhez, a PCN-hez és a GAL-szinkronizáláshoz (nem kötelező)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016*<br/> Windows Server 2019 * * |
| | Exchange a postaládák létrehozásához és a globális címlista szinkronizálásához (opcionális)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 *<br/>Exchange Server 2019 * * |
| | Fejlesztői környezet (opcionális) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | További csatlakoztatott rendszer (opcionális) | Active Directory tartományi szolgáltatások<br/>Active Directory<br/>Lightweight Directory-szolgáltatások<br/>SQL Server 2008 vagy újabb<br/>SharePoint Server 2013<br/> SharePoint Server 2016*<br/> SharePoint Server 2019 * * <br/> Egyéb harmadik féltől származó termékek |
| **MIM szolgáltatás és -portál** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016*<br/> Windows Server 2019 * * |
| |PAM-forgatókönyv: Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016* |
| |PAM-forgatókönyv: Active Directory a megerősített környezet PAM-erdője számára | Windows Server 2012 R2 (NR) <br/> Windows Server 2016* |
| |PAM-forgatókönyv: Active Directory a PAM-forgatókönyv meglévő (CORP) erdőkhöz | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016* |
| | MIM szolgáltatás adatbázisa | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 * * |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016*<br/> SharePoint 2019 * * |
| | Levelezőkiszolgáló a MIM szolgáltatás jóváhagyási és csoportkezelési e-mailjeihez (opcionális) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Exchange Server 2019 * * <br/> Exchange Online * (csak értesítés a [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)létrehozása előtt) |
| | Böngésző | Minden nagyobb támogatott böngésző * (mobileszközök korlátozott)|
| **Jelentéskészítés a MIM szolgáltatással** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016*<br/> Windows Server 2019 * * |
| | Adatraktár | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (4.4.1459-es verzióval)<br/> System Center 2019 Service Manager * * |
| **MIM jelszó-változtatási és -regisztrálási portálok** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016*<br/> Windows Server 2019 * * |
| | Webböngésző | Minden jelentős támogatott böngésző |
| **A MIM beépülő moduljai és bővítményei** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-integráció (opcionális) | Outlook 2010 (Windows rendszeren, a kattintások futtatását kivéve)<br/>Outlook 2013 (Windows rendszeren, a kattintások futtatását kivéve) <br/> Outlook 2016 (Windows 10 rendszeren, kattintson a futtatásra) *<br/>Office 365 Outlook (Windows 10 rendszeren, beleértve a kattintásra) * * |
| | A PAM PowerShell-kérelmező parancsmagjai (opcionális) | Windows 8.1<br/>Windows 10 |
| **MIM Tanúsítványkezelő** (kiszolgáló és hitelesítésszolgáltató integrációja) | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016*<br/> Windows Server 2019 * * |
| | Hitelesítésszolgáltató | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016*<br/> Windows Server 2019 * * |
| | A MIM Tanúsítványkezelő adatbázisa | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 * * |
| **MIM Tanúsítványkezelő** (alkalmazás) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Felügyeleti pont tanúsítványkezelő** (csoportos ügyfél) | Windows | Windows 7 |
| A **felügyeleti pont tanúsítványainak kezelése** (ügyfél-ActiveX-alapú intelligens kártya) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
| | BHOLD-adatbázis | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | Levelezőkiszolgáló (opcionális) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webböngésző | Az Internet Explorer által támogatott böngészők a Silverlight segítségével |

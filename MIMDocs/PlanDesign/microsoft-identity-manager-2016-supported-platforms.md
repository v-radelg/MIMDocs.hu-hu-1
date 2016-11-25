---
title: "Támogatott szoftverplatformok | Microsoft Docs"
description: "A MIM 2016 összetevői által támogatott termékek és termékverziók"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 09/29/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 55b7dc3c76c5e95153b5839ce1eb6bf4a7997889


---

# <a name="supported-platforms-for-mim-2016"></a>A MIM 2016 által támogatott platformok

Ez a tábla a Microsoft Identity Manager 2016 egyes összetevői által támogatott platformokat és verziókat ismerteti. A csillaggal (*) jelölt verziókat kizárólag a MIM 2016 1. szervizcsomagja támogatja.


| **MIM-összetevő** | **Platform** | **Verzió** |
|-------------------|--------------|-------------|
| **MIM Sync** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016* |
|| | MIM Sync-adatbázis | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016* |
|| | Active Directory a felhasználók átadásához, a jelszóváltozás-értesítési szolgáltatáshoz és a globális címlista szinkronizálásához (opcionális)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
|| | Exchange a postaládák létrehozásához és a globális címlista szinkronizálásához (opcionális)|Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| | Fejlesztői környezet (opcionális) | Visual Studio 2012<br/>Visual Studio 2013 |
|| | További csatlakoztatott rendszer (opcionális) | Active Directory tartományi szolgáltatások<br/>Active Directory<br/>Lightweight Directory-szolgáltatások<br/>SQL Server 2000 vagy újabb<br/>SharePoint Server 2013<br/> SharePoint Server 2016* <br/> Egyéb külső termékek |
| **MIM szolgáltatás** (kivéve PAM esetén) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
|| | MIM szolgáltatás adatbázisa | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016* |
|| | Exchange a MIM szolgáltatás jóváhagyási és csoportkezelési e-mailjeihez (opcionális) | Exchange Server 2007 SP3 (telepített Exchange felügyeleti konzollal)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
| **MIM szolgáltatás és portál** (csak PAM esetén)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
|| | Active Directory bástyakörnyezeti PAM-erdőhöz | Windows Server 2012 R2 <br/> Windows Server 2016* |
|| | Active Directory meglévő erdőkhöz | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
|| | MIM szolgáltatás adatbázisa | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016* |
|| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016* |
|| | Levelezőkiszolgáló a MIM szolgáltatás jóváhagyási és csoportkezelési e-mailjeihez (opcionális) | Exchange Server 2007 SP3 (telepített Exchange felügyeleti konzollal)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
|| | Böngésző | Minden főbb böngésző |
| **Jelentéskészítés a MIM szolgáltatással** | Windows Server | Windows Server 2012 <br/> Windows Server 2016* |
|| | Adatraktár | System Center 2012 Service Manager SP1 |
|| | Adatraktár-adatbázis | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 |
| **A MIM jelszó-változtatási és -regisztrálási portáljai** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016* |
|| | Webböngésző | Minden főbb böngésző |
| **A MIM beépülő moduljai és bővítményei** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
|| | Outlook-integráció (opcionális) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (Windows 10 rendszeren)* |
|| | A PAM PowerShell-kérelmező parancsmagjai (opcionális) | Windows 8.1<br/>Windows 10 |
| **MIM Tanúsítványkezelő** (kiszolgáló és hitelesítésszolgáltató integrációja) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
|| | Hitelesítésszolgáltató | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
|| | A MIM Tanúsítványkezelő adatbázisa | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
| **MIM Tanúsítványkezelő** (alkalmazás) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Tanúsítványkezelő** (ügyfél és csoportos ügyfél) | Windows | Windows 7 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
|| | BHOLD-adatbázis | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014* |
|| | Levelezőkiszolgáló (opcionális) | Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| | Webböngésző | Internet Explorer 7, 8, 9, 10 vagy 11 Silverlighttal |



<!--HONumber=Nov16_HO2-->



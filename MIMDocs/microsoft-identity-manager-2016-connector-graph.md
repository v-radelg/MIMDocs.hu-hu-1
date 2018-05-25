---
title: A Microsoft Identity Manager management Agent ügynököt a Microsoft Graph |} Microsoft Docs
author: fimguy
description: A Microsoft Graph (előzetes verzió) az külső felhasználó AD fiókkezelés életciklusát. Ebben a forgatókönyvben egy szervezet az Azure AD-címtárhoz történő meghívta a Vendégek, és kívánja ezeket a vendégek hozzáférést a helyi Windows-hitelesítés és Kerberos-alapú alkalmazásokhoz
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a66d424e8388005855ac8e64623f5a00f89682e9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/25/2018
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>A Microsoft Identity Manager management Agent ügynököt a Microsoft Graph (nyilvános előzetes verzió)
=======================================================================================

<a name="summary"></a>Összefoglalás 
=======

A [Microsoft Identity Manager felügyeleti ügynök a Microsoft Graph (előzetes verzió)](http://go.microsoft.com/fwlink/?LinkId=717495) lehetővé teszi, hogy további integrációs feladatokhoz a prémium szintű Azure AD-ügyfelek.

[Az Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) integrálható a helyszíni címtárakat az Azure ad-val, és biztosítja a felhasználók rendelkeznek egy közös identitás- és egységes hitelesítési különböző által felhasználók szinkronizálása az Azure AD-vel integrált Active Directory tartományi szolgáltatások, az Office 365, az Azure és a Szolgáltatottszoftver-alkalmazások és csoportok a helyszíni címtárakat az Azure ad Szolgáltatásba.   Ez a kezelőügynök speciális identitás- és felügyeleti műveletek felhasználói és az Azure AD-csoport szinkronizálási túl lehet központilag telepíteni.  Ez a kezelőügynök felfedi a MIM sync metaverse további objektumokban szerzett a [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 és a béta. 

<a name="scenarios-covered"></a>Ismertetett forgatókönyvek
=================

<a name="b2b-account-lifecycle-management"></a>B2B fiók életciklusának kezelésére
--------------------------------

A kezdeti forgatókönyv Preview a Microsoft Identity Manager felügyeleti ügynök a Microsoft Graph (előzetes verzió), külső felhasználói AD fiókkezelés életciklusát. Ebben a forgatókönyvben egy szervezet meghívta a vendégek be az Azure AD-címtár, és hozzáférést adott vendégek a helyi Windows-hitelesítés és Kerberos-alapú alkalmazások keresztül kívánja a [az Azure AD-alkalmazást](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)proxy vagy más átjáró mechanizmusokat. Az Azure AD-alkalmazásproxy igényel minden felhasználó saját AD DS-fiókjához, azonosítása és a delegálás célokra

További helyzeteket is lehetséges, hogy a jövőben hozzáadva és [itt dokumentált lehetőségektől](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>Az üzembe helyezési topológia meghatározása
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>A felügyeleti Agent(MA) használandó Microsoft Graph előkészítése
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>A MA kezelése az Azure AD-címtár engedélyezése
----------------------------------------------------

1.  Graph felügyeleti ügynöknek szüksége van a Web app / API-alkalmazás AzureAD létrehozni.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

1. kép. Új alkalmazás regisztrációja

2.  Nyissa meg a létrehozott alkalmazás, és ügyfél-azonosító például Alkalmazásazonosítót használja a MA kapcsolat oldalon:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

2. kép. Alkalmazásazonosító

2.  Új Ügyfélkulcs létrehozása minden beállítás megnyitásával-\> kulcsok. Állítsa be az egyes kulcs leírását, és válassza ki a needful időtartama. Mentse a módosításokat. A titkos érték nem lesz elérhető az oldal elhagyása után.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

3. kép. Új titkos Ügyfélkulcs

3.  "Microsoft Graph API" hozzáadni az alkalmazáshoz megnyitásával "Szükséges jogosultságokkal."

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

4. kép. Adja hozzá az új API

"Microsoft Graph API" hozzá kell adni a következő engedély:

| Az objektum művelet | Szükséges engedéllyel                                                                  | Engedély típusa |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importálási csoport          | Group.Read.All vagy Group.ReadWrite.All                                                | Alkalmazás     |
| Importálás felhasználó           | User.Read.All vagy User.ReadWrite.All vagy Directory.Read.All vagy Directory.ReadWrite.All | Alkalmazás     |

További információt a szükséges engedélyek található [Itt](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  -Összekötő létrehozása az alkalmazás azonosítójával, és generált ügyfél Secret.Each kezelőügynök kell rendelkezniük a saját alkalmazás AzureAD importálási ugyanahhoz az alkalmazáshoz a párhuzamosan futó elkerülése érdekében. Graph összekötő támogatja az objektum típusa a következők közül:

-   Felhasználói

    -   Teljes vagy különbözeti importálás

    -   Exportálás (hozzáadása, frissítése és törlése)

-   Csoport

    -   Teljes vagy különbözeti importálás

    -   Exportálás (hozzáadása, frissítése és törlése)


A támogatott attribútumtípust listáját:

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (a kapcsolódási térbe karakterlánc)

-   microsoft.graph.directoryObject (hivatkozás a kapcsolódási térbe bármely támogatott objektum)

-   Microsoft.Graph.Contact

Többértékű attribútumok (gyűjtemény) is támogatottak bármely típusú űrlap korábban a listában.

Graph-összekötő "id" attribútum a horgony és a megkülönböztető név az összes objektum használja.

Az átnevezés nem támogatott jelenleg, mert GraphAPI nem teszi lehetővé objektumok módosítása "id" attribútum már objektum.

<a name="access-token-lifetime"></a>Hozzáférés a jogkivonatok élettartama
=====================

Egy grafikonon alkalmazást egy jogkivonatot a GraphAPI eléréséhez szükséges. Összekötő fogja kérni egy új hozzáférési jogkivonat importálási törzsének (importálás iteráció az oldalon méretétől függ). Például:

-   AzureAD 10000 objektumokat tartalmaz.

-   Oldalméret Connector konfigurálva 5000

Ebben az esetben nem lesznek két ismétlési az importálás során, azok fogja szinkronizálni kívánt alatt 5000 objektumokat adjanak vissza. Igen egy új hozzáférési jogkivonat lesz kérelem kétszer.

Vegye figyelembe, hogy az exportálás során egy új jogkivonatot kérni fogja hozzáadni vagy frissíteni vagy törölni kell minden egyes objektumhoz.

<a name="installing-the-connector"></a>Az összekötő telepítése
========================

Az összekötő használatához győződjön meg arról, hogy a szinkronizálási kiszolgálón a következő: 4.5.2 a Microsoft .NET-keretrendszer vagy újabb Microsoft Identity Manager 2016 SP1 kell használni a gyorsjavítás 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) vagy újabb.

A Microsoft Identity Manager 2016 SP1 összekötők, a Graph rendelkezik egy letölthető a [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Összekötő-konfiguráció
=======================

Kapcsolat lapon:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

5. kép. Kapcsolat lap

A kapcsolat lap (1. kép) a Graph API verzióját használja, és a bérlői nevét tartalmazza. Az ügyfél-azonosító és a titkos ügyfélkódot határoz meg az Alkalmazásazonosítót és a kulcs-érték WebAPI alkalmazás AzureAD kell létrehozni.

Globális paraméterek lap:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

A kép 6. Globális paraméterek lap

Globális paraméterek lap tartalmaz a következő beállításokat:

Dátum és idő formátumban – bármely Edm.DateTimeOffset típusú attribútum használt formátum. Az importálás során, hogy a formátum használatával karakterlánc minden dátumra konvertálja. Set formátum bármely attribútum, amely dátum menti vonatkozik.

HTTP időkorlátja (másodperc) – időtúllépés másodpercben használandó WebAPI alkalmazás minden HTTP-hívása során.

Kényszerített létrehozott felhasználó következő bejelentkezésekor a jelszó módosítása – ezzel a beállítással az exportálás során létrehozott új felhasználó számára. Ha engedélyezve van, majd [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) úgy lesz beállítva, tulajdonság értéke igaz, ellenkező esetben lesz hamis.

<a name="troubleshooting"></a>Hibaelhárítás
===============

**Naplók engedélyezése**

Ha gondok vannak a Graph, majd naplók használható localize a problémát. A Graph-összekötő ugyanarra az adatforrásra, mint minden általános összekötőt használja. Igen, nyomkövetéseket sikerült engedélyezni a [azonos módon például általános összekötők című wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Vagy csak az miiserver.exe.config (belül system.diagnostics/sources szakaszban) ad hozzá a következőket:

\<adatforrás neve = "ConnectorsLog" switchValue "Részletes" =\>

\<figyelők\>

>   \<Adja hozzá az initializeData attribútumnak "ConnectorsLog" type="System.Diagnostics.EventLogTraceListener, a rendszer, verzió = 4.0.0.0, Culture = neutral, PublicKeyToken = = b77a5c561934e089" name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack,   Dátum és idő, Timestamp, hívási verem"/\>

\<Távolítsa el a name = "Alapértelmezett" /\>

\</listeners\>

\</ Source\>

Megjegyzés: Ha "A kezelőügynök futtatása külön folyamatban" engedélyezve van, akkor dllhost.exe.config használandó miiserver.exe.config helyett.

**Hozzáférési token lejárt hiba**

Összekötő HTTP 401-es jogosulatlan, hibaüzenetet "hozzáférési token lejárt." lehet, hogy vissza:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

7. kép. "Hozzáférési jogkivonat lejárt. Hiba

A probléma oka lehet, hogy hozzáférési jogkivonatok élettartama Azure oldalán konfigurációját. Alapértelmezés szerint a hozzáférési jogkivonat lejár, 1 óra. Lejárati idő növelése érdekében tekintse meg [Ez a cikk](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Ez használatának példája [az Azure AD PowerShell modul nyilvános előzetes verzió](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Új AzureADPolicy-definíció \@("{"TokenLifetimePolicy": {"Version": 1, **"AccessTokenLifetime":" 5: 00:00 "**}}") – DisplayName "OrganizationDefaultPolicyScenario" - IsOrganizationDefault \$true - típus "TokenLifetimePolicy"

<a name="next-steps"></a>További lépések
----------
- [Graph Explorer (nagy a HTTP-hívás problémák elhárítása)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versioning, támogatást és legfrissebb szabályzatok módosításakor a Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Töltse le a Microsoft Identity Manager felügyeleti ügynök a Microsoft Graph (előzetes verzió)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>A forgatókönyv-specifikus támogatott útmutatók
----------------------------------
[A MIM B2B teljes körű központi telepítés]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)

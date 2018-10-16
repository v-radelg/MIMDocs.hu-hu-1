---
title: A Microsoft Identity Manager-összekötő a Microsoft Graph |} A Microsoft Docs
author: fimguy
description: A Microsoft Identity Manager-összekötő a Microsoft Graph lehetővé teszi, hogy a külső felhasználót AD fiókok életciklusának kezelése. Ebben a forgatókönyvben egy szervezet meghívta a vendégek be az Azure AD-címtárhoz, és meg kívánja szüntetni ezeket vendégek hozzáférési helyszíni Windows-Integrated hitelesítés és Kerberos-alapú alkalmazások
keywords: ''
ms.author: billmath
manager: bhu
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 121b4f3759ef57a9b80d0c1f68c605542e72ae24
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333598"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>A Microsoft Graph-összekötő a Microsoft Identity Manager
=======================================================================================

<a name="summary"></a>Összefoglalás 
=======

A [Microsoft Identity Manager-összekötő a Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) további integrációs forgatókönyveket teszi lehetővé a prémium szintű Azure AD-ügyfelek számára.  Hibaelhárításra használható megjelenítése, az a MIM sync metaverzum további objektumokat származó a [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 és bétaverzióihoz.

<a name="scenarios-covered"></a>Az ismertetett forgatókönyvek
=================

<a name="b2b-account-lifecycle-management"></a>B2B-fiókok életciklusának kezelése
--------------------------------

A kezdeti forgatókönyvben a Microsoft Identity Manager-összekötő a Microsoft Graph van, mint egy összekötőt, melyekkel automatizálhatja a Tartományi fiók életciklus-felügyelet a külső felhasználók számára. Ebben a forgatókönyvben egy szervezet az alkalmazottak Azure AD szolgáltatással szinkronizál az Active Directory tartományi szolgáltatások, Azure AD Connect használatával, és a vendégek is meghívta be az Azure AD-címtár. Meghívja az adott szervezet Azure AD-címtár külső felhasználói objektum egy Vendég eredményez, amely nem szerepel a szervezet Active Directory tartományi Szolgáltatásokban. A szervezet kívánja ezeket a vendégek hozzáférést biztosíthat a helyszíni Windows beépített hitelesítés és Kerberos-alapú alkalmazások keresztül, majd a [Azure AD-alkalmazásproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) vagy más átjáró mechanizmusokat. Az Azure AD-alkalmazásproxy minden felhasználónak szüksége van a saját AD DS-fiókot, azonosítása és a delegálási célokra.  

Ismerje meg, hogyan konfigurálhatja a MIM sync szolgáltatást automatikus létrehozása és kezelése az AD DS-fiókok Vendégek, az utasításokat, ez a cikk elolvasása után olvassa tovább az a cikkben [a MIM 2016 SP1-et az Azure AD vállalatközi (B2B) együttműködés az Azure-alkalmazásproxyval](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  A cikk azt mutatja be, szükség az összekötő szinkronizálási szabályokat.

<a name="other-identity-management-scenarios"></a>Egyéb identitás-felügyeleti forgatókönyvek
---------------

Az összekötő más adott identitáskezeléshez érintő forgatókönyvek létrehozása, olvasása, frissítése és törlése felhasználó, csoport és az elérhetőségi objektumok Azure ad-felhasználók és csoportok szinkronizálását az Azure AD nem használható. A lehetséges forgatókönyvek kiértékelésekor ne feledje: Ez az összekötő forgatókönyvekben, amely eredményez olyan adatfolyam átfedésben lenne, tényleges vagy potenciális szinkronizálási ütközik egy Azure AD Connect üzemelő nem működik.  [Az Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) az ajánlott eljárás a helyszíni címtárak integrálása az Azure AD-felhasználók és csoportok a helyszíni címtárakat az Azure ad-val való szinkronizálásával.  Az Azure AD Connect számos további szinkronizálási funkcióval rendelkezik, és lehetővé teszi, hogy a forgatókönyvek, például a jelszó- és eszköz-visszaírási, amelyek nem lehetséges a MIM által létrehozott objektumok. Ha az adatok Active Directory tartományi szolgáltatásokba telepítik, például győződjön meg arról, hogy az ki lesz zárva az Azure AD Connect próbál meg azokat az objektumokat az Azure AD-címtár vissza a megfelelő.  És ez az összekötő segítségével módosíthatja az Azure AD-objektumok, amelyek az Azure AD Connect által létrehozott sem.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Az összekötő használatára a Microsoft Graph előkészítése
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Beolvasni vagy objektumokat az Azure AD-címtár kezelése az összekötő engedélyezése
----------------------------------------------------

1.  Az összekötő számára szükséges a webalkalmazás / API-alkalmazás az Azure AD-ben hozhatók létre, hogy működjön az Azure ad-ben megfelelő engedélyekkel rendelkező jogosult a Microsoft Graphon keresztül objektumokat.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

1. kép. Új alkalmazás regisztrálása

2.  Az Azure Portalon nyissa meg a létrehozott alkalmazást, és az Alkalmazásazonosító, Mentés másként később a MA kapcsolatok lap használandó ügyfél-Azonosítót:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

2. kép. Alkalmazásazonosító

3.  Új titkos Ügyfélkulcsot létrehozásához nyissa meg az összes beállítás-\> kulcsok. Állítsa be az egyes kulcs leírását, és válassza ki a needful időtartama. Mentse a módosításokat. A titkos érték nem lesz elérhető az oldal elhagyása után.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

3. kép. Új titkos Ügyfélkulcsot

4.  "A Microsoft Graph API" hozzáadni az alkalmazáshoz nyissa meg a "Szükséges engedélyekkel."

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

4. kép. Új API hozzáadása

A következő engedély szükséges hozzá kell adni az alkalmazást, hogy a "Microsoft Graph API-val", a forgatókönyvtől függően:

| A művelet az objektum | Engedély szükséges                                                                  | Engedély típusa |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Csoport importálása          | `Group.Read.All` vagy `Group.ReadWrite.All`                                                | Alkalmazás     |
| Felhasználó importálása           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` vagy `Directory.ReadWrite.All` | Alkalmazás     |

Szükséges engedélyekkel kapcsolatos további részletekért található [Itt](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Adja meg az alkalmazás a szükséges engedélyeket.


<a name="installing-the-connector"></a>Az összekötő telepítése
========================

6.  Az összekötő telepítése előtt győződjön meg arról, akkor a szinkronizálási kiszolgálón a következő: 

 - Microsoft keretrendszer 4.5.2-es vagy újabb
 - A Microsoft Identity Manager 2016 SP1-et, és meg kell felelnie a gyorsjavítás 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) vagy újabb.

7. Az összekötő mellett egyéb összekötőkhöz a Microsoft Identity Manager 2016 SP1-et, a Microsoft Graph, egy letölthető a [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Indítsa újra a MIM Synchronization Service.
 
<a name="connector-configuration"></a>Összekötő-konfiguráció
=======================


9.  A Synchronization Service Manager felhasználói felületén, válassza ki a **összekötők** és **létrehozás**.
Válassza ki **Graph (Microsoft)** , hozzon létre egy összekötőt, és adjon meg egy leíró nevet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. A MIM synchronization service felhasználói Felületet adja meg az Alkalmazásazonosítót, és a generált ügyfélkódot. Minden felügyeleti ügynök a MIM Sync konfigurálva kell rendelkeznie a saját alkalmazás importálása ugyanahhoz az alkalmazáshoz a párhuzamosan futó elkerülése érdekében az Azure ad-ben.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

5. kép. Kapcsolat lap

A kapcsolatok lap (kép 5) tartalmazza a Graph API verzióját használja, és a bérlő neve. Az ügyfél-Azonosítóját és Ügyfélkulcsát jelölik az Alkalmazásazonosító és a kulcs értékét a WebAPI-alkalmazás, amely az Azure ad-ben kell létrehozni.

11. Végezze el a szükséges módosításokat a globális paraméterek oldalon:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

6. kép. Globális paraméterek lap

Globális paraméterek lap tartalmaz a következő beállításokat:

- Dátum és idő formátumban – bármely attribútum Edm.DateTimeOffset típusú használt formátum. Az összes dátum karakterlánc konvertálja az importálás során, hogy a formátum használatával. Set-formátum bármely attribútuma, amely dátum menti a alkalmazni.

 - HTTP-időkorlát (másodperc) – időkorlát másodpercben, amely használható egyes WebAPI alkalmazás HTTP-hívás során.

 - Kényszerített a következő bejelentkezéskor a létrehozott felhasználói jelszó módosítása – ezzel a beállítással az exportálás során létrehozott új felhasználó számára. Ha a beállítás engedélyezve van, majd [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) állítja be a tulajdonság értéke igaz, ellenkező esetben lesz false (hamis).

<a name="configuring-the-connector-schema-and-operations"></a>Az összekötő séma és a műveletek konfigurálása
=========================

12.   A séma konfigurálásához.  Az összekötő a következő típusú objektumokat támogatja:

-   Felhasználói

    -   Teljes és különbözeti importálás

    -   Exportálás (hozzáadása, frissítése és törlése)

-   Csoport

    -   Teljes és különbözeti importálás

    -   Exportálás (hozzáadása, frissítése és törlése)


A támogatott típusú attribútumok listája:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (karakterlánc összekötőtérben)

-   `microsoft.graph.directoryObject` (bármely támogatott objektumot az összekötőtérben hivatkozás)

-   `microsoft.graph.contact`

Többértékű attribútumok (gyűjtemény) minden olyan típusú, a fenti listából is támogatottak.

Az összekötő használja a "`id`" minden objektum a forráshorgony és DN attribútum.  Ezért az átnevezés nem szükséges, mivel Graph API nem teszi lehetővé egy objektumot a hozzá tartozó "id" attribútum.


<a name="access-token-lifetime"></a>Hozzáférési jogkivonat élettartama
=====================

A Graph-alkalmazások eléréséhez a Graph API hozzáférési jogkivonat szükséges. Egy összekötő kérni fogja az importálás törzsének egy új hozzáférési jogkivonat (importálás az iteráció az oldalméret függ). Például:

-   Az Azure AD 10000 objektumokat tartalmazza.

-   Oldalméret-összekötő konfigurálva 5000

Ebben az esetben lesz két ismétlések az importálás során, azok adja vissza 5000 objektumok szinkronizálásának. Tehát egy új hozzáférési jogkivonatot fogja kérelem kétszer.

Az exportálás során egy új hozzáférési jogkivonatot fog kérni, amelyet be kell hozzá/frissített/törölt minden objektum esetén.

<a name="troubleshooting"></a>Hibaelhárítás
===============

**Naplók engedélyezése**

Gráf merül fel, ha majd naplók használható honosítani a problémát. Tehát nyomkövetések sikerült engedélyezni a [azonos módon, az általános összekötők például](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). A következő hozzáadásával vagy `miiserver.exe.config` (belül `system.diagnostics/sources` szakaszt):


\<adatforrás neve = "ConnectorsLog" switchValue = "Részletes"\>

\<figyelők\>

>   \<initializeData hozzáadása "ConnectorsLog" type="System.Diagnostics.EventLogTraceListener, a rendszer, verzió = 4.0.0.0, Culture = neutral, PublicKeyToken = = b77a5c561934e089" name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack,   Dátum és idő, Timestamp, hívási verem"/\>

\<Távolítsa el a name = "Alapértelmezett" /\>

\</listeners\>

\</ Source\>

>[!NOTE]
>"Ez a kezelőügynök futtatása külön folyamatban" engedélyezve van-e, majd `dllhost.exe.config` helyett használjon `miiserver.exe.config`.

**Hozzáférési token lejárt hibaüzenet**

Összekötő HTTP 401-es nem engedélyezett, hibaüzenet "hozzáférési token lejárt." vissza:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

7. kép. "Hozzáférési token lejárt." Hiba

Előfordulhat, hogy a probléma okának konfigurációját az Azure oldaláról a hozzáférési jogkivonat élettartama. Alapértelmezés szerint a hozzáférési jogkivonat lejár, 1 óra után. Lejárati idő növelésével kapcsolatban lásd: [Ez a cikk](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Ez a példa [az Azure AD PowerShell modul nyilvános előzetes kiadás](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Új AzureADPolicy-definíció \@("{"TokenLifetimePolicy": {"Verziójú": 1, **"AccessTokenLifetime":" 5: 00:00 "**}}') – DisplayName"OrganizationDefaultPolicyScenario"- IsOrganizationDefault \$true - típus "TokenLifetimePolicy"

<a name="next-steps"></a>További lépések
----------
- [Graph Explorer hibaelhárítási HTTP nagy hívja kapcsolatos problémák]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Verziókezelés, a támogatást és a használhatatlanná tévő változás szabályzatok a Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [A Microsoft Identity Manager-összekötő letöltése a Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>A forgatókönyv-specifikus útmutatók
----------------------------------
[A MIM B2B teljes körű üzembe helyezés]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)

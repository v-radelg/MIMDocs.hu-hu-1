---
title: Privileged Access Management az Active Directory Domain Serviceshez | Microsoft Docs
description: "További információk a Privileged Access Managementről, valamint az Active Directory-környezet kezelésében és védelmében elfoglalt szerepéről."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
experimental: true
experiment_id: kgremban_images
translationtype: Human Translation
ms.sourcegitcommit: f0947f186b5206d06a67140706ada33a5bc0e016
ms.openlocfilehash: cea5a2fc162870c1125b35b75376881eb15cd2e9
ms.lasthandoff: 01/11/2017

---

# <a name="privileged-access-management-for-active-directory-domain-services"></a>Privileged Access Management az Active Directory Domain Serviceshez
A Privileged Access Management (PAM) olyan megoldás, amely segít a szervezeteknek a meglévő Active Directory-környezetükben korlátozni a rendszerjogosultságú hozzáférést.

A Privileged Access Management két célt ér el:

- Visszaszerzi a felügyeletet a sérült biztonságú Active Directory-környezetek fölött: különálló megerősített környezetet működtet, amelyet biztosan nem érintettek a rosszindulatú támadások.  
- Elszigeteli a rendszerjogosultságú fiókok használatát, hogy mérsékelje az ilyen hitelesítő adatok ellopásának kockázatát.

> [!NOTE]
> A PAM a [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) egy példánya, amely a Microsoft Identity Manager (MIM) segítségével van megvalósítva.

## <a name="what-problems-does-pam-help-solve"></a>Milyen problémákat segít a PAM megoldani?
Napjaink vállalatai számára komoly aggályokat vet fel az Active Directory-környezeten belüli erőforrások elérése. Különösen aggasztóak azok a híradások, amelyek biztonsági résekről, a jogosultságok illetéktelenek általi megemeléséről és más típusú jogosulatlan hozzáférésekről (pass-the-hash (kivonatátadás), pass-the-ticket (jegyátadás), spear phishing (személy nevében történő adathalászat) és Kerberos-biztonsági problémák) szólnak.

Napjainkban a támadók nagyon könnyen megszerezhetik a tartományi rendszergazdák fiókjának hitelesítő adatait, az ilyen támadásokat azonban nagyon nehéz felderíteni, ha már bekövetkeztek. A PAM célja, hogy a rosszindulatú felhasználók kisebb eséllyel szerezhessenek hozzáférést, Ön pedig nagyobb mértékben kontrollálhassa és felügyelhesse környezetét.

A PAM megnehezíti a támadók számára, hogy behatoljanak a hálózatokra, és rendszerjogosultságú fiókot használjanak. A PAM további védelemmel óvja a rendszerjogosultságú csoportokat, amelyek szabályozzák a hozzáférést sok, a tartományhoz csatlakoztatott számítógéphez és rajtuk futó alkalmazáshoz. Bővíti ezenkívül a figyelési lehetőségek körét, javítja a láthatóságot, és részletesebb szabályozást tesz lehetővé, hogy a szervezetek ellenőrizni tudják, kik a rendszerjogosultságú rendszergazdáik, és mit csinálnak. A PAM révén a szervezetek jobban tájékozódhatnak arról, hogy milyen műveletekre kerül sor a környezetükben a rendszergazdai fiókok használatával.

## <a name="how-is-pam-set-up"></a>A PAM bevezetése
A PAM a szükséges időben (just-in-time) történő felügyelet elvére épül, amely összefügg az [éppen elég felügyelettel (just enough administration, JEA)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). A JEA egy Windows PowerShell-eszközkészlet, amely definiálja a magas jogosultságszintet igénylő tevékenységek elvégzésére szolgáló parancsok egy halmazát, valamint egy végpontot, ahol a rendszergazdák engedélyt szerezhetnek ezeknek a parancsoknak a futtatására. A JEA-ban egy rendszergazda dönti el, hogy milyen jogosultságra van szükségük a felhasználóknak ahhoz, hogy elvégezzenek egy feladatot. Minden alkalommal, amikor egy jogosult felhasználónak el kell végeznie ezt a feladatot, aktiválják ezt az engedélyt. Meghatározott idő elteltével az engedélyek lejárnak, hogy egy rosszindulatú felhasználó el ne lophassa a hozzáférési jogosultságot.

A PAM üzembe helyezése és működtetése négy lépésből áll.

![A PAM lépései: előkészítés, védelem, működtetés, figyelés – ábra](media/MIM_PIM_SetupProcess.png)

1.  **Előkészítés**: Azonosítsa azokat a csoportokat, amelyek magas jogosultságszinttel rendelkeznek a meglévő erdőn belül. Hozza létre ezeket a csoportokat tagok nélkül a megerősített erdőben.

2.  **Védelem**: Állítsa be az életciklust és a hitelesítéses védelmet (például többtényezős hitelesítést – Multi-Factor Authentication, MFA) arra az esetre, amikor a felhasználók a szükséges időben történő felügyeletre kérnek engedélyt. Az MFA megakadályozza kártevő szoftverek programszintű támadásait, valamint a hitelesítő adatok ellopását követő támadásokat.

3.  **Működtetés**: Ha egy felhasználói fiók megfelel a hitelesítési követelményeknek, és jóváhagyják a kérését, átmenetileg bekerül a megerősített erdő rendszerjogosultságú csoportjába. Ezt követően a rendszergazda az előre megadott időtartamra rendelkezik minden olyan jogosultsággal és engedéllyel, amely hozzá van rendelve ehhez a csoporthoz. Ennek az időnek az elteltével a fiók törlődik a csoportból.

4.  **Figyelés**: A PAM segítségével naplózhatók a magas jogosultságszint iránti kérések, riasztások köthetők hozzájuk, valamint jelentések készíthetők róluk. Megtekinthető, hogy mikor fért hozzá valaki emelt szintű jogosultságokkal a rendszerhez, és ki végzett egy adott tevékenységet. Megállapítható, hogy a tevékenység megfelel-e a szabályoknak, és könnyen felismerhetők a jogosulatlan tevékenységek (ha például valaki megpróbál közvetlenül hozzáadni egy felhasználót az eredeti erdő valamelyik rendszerjogosultságú csoportjához). Ez a lépés nemcsak a kártékony szoftverek azonosítást segíti, hanem a „belső” támadók figyelését is.

## <a name="how-does-pam-work"></a>Hogyan működik a PAM?
A PAM az AD DS új képességein alapul, különösen a tartományi fiókok hitelesítésének és engedélyezésének új lehetőségein, valamint a Microsoft Identity Manager új funkcióin. A PAM leválasztja a rendszerjogosultságú fiókokat a meglévő Active Directory-környezettől. Ha egy rendszerjogosultságú fiókot kell használni, akkor ezt először kérelmezni kell, majd jóvá kell hagyni. A jóváhagyást követően a rendszerjogosultságú fiók nem a felhasználó vagy az alkalmazás aktuális erdejében kap engedélyt, hanem egy új megerősített erdő külső rendszercsoportjának tagjaként. A megerősített erdő használata szélesebb körű szabályozást tesz lehetővé a szervezet számára, például arra vonatkozóan, hogy mikor lehet egy felhasználó tagja egy rendszerjogosultságú fióknak, és hogy a felhasználónak hogyan kell hitelesítenie magát.

Az Active Directory, a MIM szolgáltatás és ennek a megoldásnak az egyéb részei magas rendelkezésre állású konfigurációban is üzembe helyezhetők.

A következő példa részletesebben bemutatja, hogy miképpen működik a PIM.

![A PIM folyamata és résztvevői – ábra](media/MIM_PIM_howitworks.png)

A megerősített erdő korlátozott időre szóró csoporttagságokat oszt ki, amelyek pedig időben korlátozott jegymegadó jegyeket (TGT) állítanak elő. A Kerberos-alapú alkalmazások és szolgáltatások akkor fogadják el az ilyen TGT-ket, illetve tartatják be az engedélyeiket, ha az alkalmazások és szolgáltatások olyan erdőkben léteznek, amelyek megbíznak a megerősített erdőben.

A napi munka során használt felhasználói fiókoknak nem kell új erdőbe költözniük. Ugyanez igaz a számítógépekre, az alkalmazásokra és csoportjaikra. Ugyanott maradnak, ahol most is vannak, egy meglévő erdőben. Vegyünk például egy olyan szervezetet, amelyet aggasztanak napjaink említett számítógépes biztonsági problémái, de még nem tervezi, hogy rövidesen frissíti a kiszolgálói infrastruktúráját a Windows Server következő verziójára. Ez a szervezet ennek ellenére élhet ennek az egyesített megoldásnak az előnyeivel: ehhez az MIM-et és egy új megerősített erdőt kell használnia, aminek köszönhetően jobban tudja szabályozni a hozzáférést a meglévő erőforrásaihoz.

A PAM a következő előnyöket nyújtja:

-   **A jogosultságok izolálása/hatókörének kezelése**: A felhasználók nem rendelkeznek magas szintű jogosultságokkal olyan fiókokban, amelyeket hétköznapi feladatokra (például az e-mailjeik elolvasására vagy internetböngészésre) használnak. A felhasználóknak kérniük kell a magas szintű jogosultságokat. A kérések jóváhagyása és elutasítása a PAM-rendszergazda által definiált MIM-szabályzatok alapján történik. Mindaddig nem vehető igénybe rendszerjogosultságú hozzáférés, amíg nem engedélyezik a kérést.

-   **Jogosultságemelés (step-up) és hitelesítésiszint-emelés (proof-up)**: Ezek új hitelesítési és engedélyezési kérések, amelyek a külön rendszergazdai fiókok életciklusának kezelését segítik. A felhasználó kérheti egy rendszergazdai fiók jogosultságainak megemelését. A kérés a MIM munkafolyamatain halad végig.

-   **További naplózás**: A MIN beépített munkafolyamatain kívül a PAM további naplózási műveleteket végez. Ezek azonosítják a kérést, az engedélyezését, illetve a jóváhagyás után esetleg bekövetkező eseményeket.

-   **Testre szabható munkafolyamat**: A MIM munkafolyamatai különböző helyzetek kezelésére konfigurálhatók, és többféle munkafolyamat is használható, a kérelmező felhasználó vagy a kért szerepkörök alapján.

## <a name="how-do-users-request-privileged-access"></a>Hogyan kérik a felhasználók a rendszerjogosultságú hozzáférést?
A felhasználók a következő felületeken küldhetik be a kéréseket:  
- A MIM szolgáltatások webszolgáltatási API-ja  
- REST-végpont  
- Windows PowerShell (`New-PAMRequest`)

Megismerheti az [Emelt szintű hozzáférések felügyeletének parancsmagjai](https://technet.microsoft.com/library/mt604080.aspx) című témakör részleteit.

## <a name="what-workflows-and-monitoring-options-are-available"></a>Milyen munkafolyamatok és figyelési lehetőségek érhetők el?
Tegyük fel például, hogy egy felhasználó tagja volt egy rendszergazdai csoportnak még a PIM telepítése előtt. A PIM telepítésének részeként a felhasználó törlődik a rendszergazdai csoportból, és egy szabályzat jön létre az MIM-ben. A szabályzat előírja, hogy ha egy felhasználó rendszergazdai jogosultságokat kér, és egy MFA hitelesíti, akkor a kérés jóváhagyást kap, és a rendszer felvesz egy külön fiókot a felhasználónak a megerősített erdő rendszerjogosultságú fiókjába.

Ha a kérés jóváhagyást nyer, a munkafolyamat közvetlen kommunikációra lép a megerősített erdő Active Directoryjával, hogy az vegye fel a felhasználót egy csoportba. Ha például Ilona engedélyt kér a személyzeti adatbázis felügyeletére, Ilona rendszergazdai fiókja másodperceken belül bekerül a megerősített erdő rendszerjogosultságú csoportjába. Amikor eltelik a megadott időkorlát, rendszergazdai fiókjának tagsága megszűnik abban a csoportban. A Windows Server Technical Preview rendszerben az Active Directoryban párosul időkorlát ehhez a tagsághoz. A Windows Server 2012 R2 verzióban a MIM tartatja be ezt az időkorlátot a megerősített erdőben.

> [!NOTE]
> Ha új tagot vesz fel egy csoportba, ennek a változásnak replikálódnia kell a megerősített erdő többi tartományvezérlőjére. A replikáció késése megakadályozhatja a felhasználókat az erőforrások elérésében. A replikáció késéséről a [How Active Directory Replication Topology Works](https://technet.microsoft.com/library/cc755994.aspx) (Az Active Directory replikációs topológiájának működése) című cikkből tájékozódhat.
>
> Ezzel szemben a lejárt hivatkozást valós időben értékeli ki a biztonsági fiókkezelő (SAM). Még ha a csoporttag felvételét replikálnia is kell annak a tartományvezérlőnek, amely megkapja a kérést, a csoporttag eltávolítását azonnal kiértékeli minden tartományvezérlő.

Ez a munkafolyamat kifejezetten az ilyen rendszergazdai fiókok számára készült. A rendszerjogosultságú fiókokhoz csak alkalmi hozzáférést igénylő rendszergazdák (sőt akár szkriptek is) pontosan ilyen hozzáférést kérelmezhetnek. Az MIM naplózza a kérést és az Active Directoryban bekövetkező változásokat, Ön pedig megtekinthető őket az Eseménynaplóban, vagy elküldheti az adatokat vállalati figyelési megoldásoknak (például a System Center 2012 – Operations Manager naplózási szolgáltatásának (ACS) vagy más külső gyártású eszközöknek).


---
title: "Microsoft Identity Manager 2016 – Gyakorlati tanácsok | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.translationtype: MT
ms.sourcegitcommit: 3bb89e2c86724e6f6d32e4043fa37da74e2b7b24
ms.openlocfilehash: a0d00c7e5d99e43d3fb0b3011a3851f7194bfdf2
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---


# Microsoft Identity Manager 2016 – Gyakorlati tanácsok
<a id="microsoft-identity-manager-2016-best-practices" class="xliff"></a>

A következőkben a Microsoft Identity Manager 2016 (MIM) telepítésével és működtetésével kapcsolatos ajánlott eljárásokat ismertetjük

## SQL-telepítés
<a id="sql-setup" class="xliff"></a>
>[!NOTE]
A következő, egy SQL-t futtató kiszolgáló telepítésére vonatkozó javaslatok dedikált SQL-példányok meglétét feltételezik a FIMService-hez és a FIMSynchronizationService adatbázishoz. Ha a FIMService-t összevont környezetben futtatja, akkor el kell végeznie a konfigurációhoz szükséges módosításokat.

A Structured Query Language- (SQL-) kiszolgáló konfigurálása alapvető fontosságú a rendszer optimális teljesítményéhez. Az optimális MIM teljesítmény elérése nagy léptékű környezetekben az SQL-t futtató kiszolgálókra vonatkozó ajánlott eljárások alkalmazásától függ. További információk az SQL-lel kapcsolatos ajánlott eljárásokról következő témakörökben olvashatók:

-   [A tárolással kapcsolatos 10 legfontosabb gyakorlati tanács](http://go.microsoft.com/fwlink/?LinkID=183663) (angol nyelven)

-   [Optimizing tempdb Performance](http://go.microsoft.com/fwlink/?LinkID=188267) (A tempdb teljesítményének optimalizálása)

-   [SQL Server – Gyakorlati tanácsok cikk](http://go.microsoft.com/fwlink/?LinkID=188268) (angol nyelven)

-   [Reorganizing and Rebuilding Indexes](http://go.microsoft.com/fwlink/?LinkID=188269) (Indexek átszervezése és újraépítése)

### Adat- és naplófájlok előzetes méretezése
<a id="presize-data-and-log-files" class="xliff"></a>

Ne támaszkodjon az automatikus növekedés használatára. Ehelyett kezelje manuálisan ezen fájlok méretnövekedését. Biztonsági okokból bekapcsolva hagyhatja az automatikus növekedési funkciót, de proaktívan felügyelje az adatfájlok méretének növekedését. A MIM-adatbázis példaméreteit a [FIM kapacitástervezési útmutatóban](http://go.microsoft.com/fwlink/?LinkID=185246) találja (angol nyelven).

### Az adat- és naplófájlok előzetes méretezéséhez:
<a id="to-presize-sql-data-and-log-files" class="xliff"></a>

1.  Indítsa el az SQL Server Management Studio eszközt.

2.  Nyissa meg az adatbázishoz tartozó FIMService-t, kattintson jobb gombbal a FIMService-re, majd kattintson a Properties (Tulajdonságok) parancsra.

3.  A Files (Fájlok) lapon bővítse a szükséges méretre az adatbázisfájlokat.

### A napló elkülönítése az adatfájloktól
<a id="isolate-log-from-data-files" class="xliff"></a>

Az adatbázisok tranzakciós és az adatokra vonatkozó naplófájlok külön fizikai lemezekre való elkülönítéséhez kövesse az SQL Server ajánlott eljárásait.

További tempdb-fájlok létrehozása

Az optimális teljesítmény érdekében processzormagonként egy adatfájlt ajánlott létrehozni a tempdb-fájlban.

### További tempdb-fájlok létrehozásához:
<a id="to-create-additional-tempdb-files" class="xliff"></a>

1.  Indítsa el az SQL Server Management Studio eszközt.

2.  Keresse meg az adatbázishoz tartozó tempdb-fájlt a rendszeradatbázisok között, kattintson jobb gombbal a tempdb-fájlra, és válassza a Properties (Tulajdonságok) parancsot.

3.  A Files (Fájlok) lapon hozzon létre minden egyes processzormaghoz egy-egy adatfájlt. Ügyeljen arra, hogy a tempdb adat- és naplófájljai különböző meghajtókon és lemezeken legyenek elkülönítve.

### Elegendő szabad hely biztosítása a naplófájlok számára
<a id="ensure-adequate-space-for-log-files" class="xliff"></a>

Fontos a helyreállítási modell lemezkövetelményeinek ismerete. Az egyszerű helyreállítási mód a kezdeti rendszerbetöltés során megfelelő lehet a használt lemezterület korlátozására, de a legutóbbi biztonsági mentés után létrehozott adatok adatvesztésnek vannak kitéve. Teljes helyreállítási mód használatakor szükséges a lemezterület biztonsági másolatokra való használatának felügyelete, többek között a tranzakciónapló gyakori biztonsági mentései magas lemezterület-használatának megakadályozására. További információt a [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (A helyreállítási modell áttekintése) című részben talál.

### Az SQL Server memóriájának korlátozása
<a id="limit-sql-server-memory" class="xliff"></a>

Attól függően, hogy mennyi memóriát használ az SQL Server, és hogy megosztja-e az SQL Servert más szolgáltatásokkal (azaz a MIM 2016 szolgáltatással és a MIM 2016 Synchronization Service-szel), előfordulhat, hogy korlátozni kell az SQL memóriahasználatát. Ez az alábbi lépések segítségével tehető meg.

1.  Indítsa el az SQL Server Enterprise Managert.

2.  Válassza a New Query (Új lekérdezés) lehetőséget.

3.  Futtassa az alábbi lekérdezést:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  Ez a példa újrakonfigurálja az SQL Servert, hogy legfeljebb 12 gigabájt (GB) memóriát használjon.

4.  A beállítás ellenőrzésére használja a következő lekérdezést:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### Biztonsági mentés és helyreállítás konfigurálása
<a id="backup-and-recovery-configuration" class="xliff"></a>

Általánosságban a szervezet érvényes biztonsági mentési szabályzata szerint kell az adatbázisról biztonsági mentést készítenie. Ha a növekményes naplófájlok biztonsági mentései nem tervezettek, az egyszerű helyreállítási módot kell beállítani az adatbázishoz. Mielőtt hozzálátna a biztonsági mentési stratégia bevezetéséhez, meg kell bizonyosodnia afelől, hogy tisztában van a különböző helyreállítási modellek következményeivel, valamint ezen modellek lemezterület-szükségleteivel. A teljes helyreállítási modell a naplók gyakori biztonsági mentéseit igényli a magas lemezterület-használat elkerülése érdekében. További információt a [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (A helyreállítási modell áttekintése) és a [FIM 2010 Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (FIM 2010 biztonsági mentési és visszaállítási útmutató) című részben talál.

## Biztonsági mentésért felelős rendszergazdai fiók létrehozása a FIMService-hez a telepítés után
<a id="create-a-backup-administrator-account-for-the-fimservice-after-installation" class="xliff"></a>


>[!IMPORTANT]
A FIMService-rendszergazdák csoport tagjai a FIM környezet üzemeltetéséhez alapvető fontosságú egyedi engedélyekkel rendelkeznek. Ha Ön nem tud bejelentkezni a Rendszergazdák csoport tagjaként, egyetlen megoldás a rendszer visszaállítása egy korábbi biztonsági másolattal. Ezen helyzet elkerülése érdekében javasolt, hogy a telepítés utáni konfiguráció részeként adjon hozzá más felhasználókat a FIM rendszergazdai csoporthoz.

## FIM szolgáltatás
<a id="fim-service" class="xliff"></a>


### A FIM szolgáltatás Exchange-szolgáltatásfiókjának konfigurálása
<a id="configuring-fim-service-service-exchange-mailbox" class="xliff"></a>

Az alábbi javasolt megoldásokkal konfigurálhatja a Microsoft Exchange Servert a MIM 2016 szolgáltatás szolgáltatásfiókjához.

- A szolgáltatásfiókot úgy konfigurálja, hogy az csak belső e-mail-címekről fogadhasson leveleket. Konkrétan arról van szó, hogy a szolgáltatásfiók postafiókja sohasem fogadhat leveleket külső SMTP-kiszolgálókról.

#### A szolgáltatásfiók konfigurálása
<a id="to-configure-the-service-account" class="xliff"></a>

1.  Az Exchange felügyeleti konzolon válassza **A FIM szolgáltatás szolgáltatásfiókja** lehetőséget.

2.  Válassza a tulajdonságokat, az e-mail-forgalom beállításait, majd az **Üzenetkézbesítési korlátozások** lehetőséget.

3.  Jelölje be **Az összes feladó hitelesítése szükséges** jelölőnégyzetet.

További információkért lásd: [Configure Message Delivery Restrictions](http://go.microsoft.com/fwlink/?LinkID=183625) (Üzenetkézbesítési korlátozások konfigurálása).

-   Konfigurálja a szolgáltatásfiókot az 1 MB-nál nagyobb méretű levelek elutasítására. Kövesse az [üzenetek méretkorlátjának konfigurálása](http://go.microsoft.com/fwlink/?LinkID=183626) ajánlott eljárást (angol nyelvű cikk) a postafiókhoz vagy olyan nyilvános mappához, amelynél engedélyezve van a levelezés.

-   Konfigurálja a szolgáltatásfiókot 5 GB-os tárolási kvótára a postafiókhoz. Az optimális eredmények elérése érdekében kövesse a [Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929) (Postafiókok tárolási kvótáinak konfigurálása) című cikkben felsorolt ajánlott eljárásokat.

## MIM-portál
<a id="mim-portal" class="xliff"></a>


### SharePoint-indexelés letiltása
<a id="disable-sharepoint-indexing" class="xliff"></a>

Ajánlott letiltani a Microsoft Office SharePoint® indexelését. Nincsenek indexelendő dokumentumok és az indexelés számos hibanapló-bejegyzést és esetleges teljesítményproblémákat okozhat a FIM 2010 használatakor. A SharePoint-indexelés letiltásához:

1.  A MIM 2016 portált futtató kiszolgálón kattintson a Start gombra.

2.  Mutasson a Minden program pontra.

3.  A programok listájában kattintson a Felügyeleti eszközök pontra.

4.  A Felügyeleti eszközök alatt kattintson a SharePoint központi felügyelet elemre.

5.  A központi felügyelet lapján kattintson a Tevékenység elemre.

6.  A Tevékenység oldalon a Globális beállítások csoportban kattintson az Időzítőfeladat-definíciók elemre.

7.  Az időzítőfeladat-definíciók lapján kattintson a SharePoint Services – keresés frissítése elemre.

8.  Az Időzítőfeladat módosítása lapon kattintson a Letiltás elemre.

## A MIM 2016 kezdeti adatbetöltése
<a id="mim-2016-initial-data-load" class="xliff"></a>

Ez a szakasz felsorolja a teljesítmény növelése érdekében végrehajtandó lépéseket a külső rendszerekből származó kezdeti adatok a FIM 2010-be való betöltéséhez. Fontos tisztában lenni azzal, hogy ezen lépések némelyike ideiglenesen, a rendszer kezdeti feltöltése során alkalmazandó, és annak befejezését követően vissza kell állítani az alaphelyzetet. Ez egy egyszeri művelet, és nem folyamatos szinkronizálás.

>[!NOTE]
További információt a felhasználók a FIM 2010 és az Active Directory tartományi szolgáltatások (AD DS) közötti szinkronizálásáról a következő FIM-dokumentációban talál: [How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277) (A felhasználók az Active Directoryból a FIM-be való szinkronizálásának menete).

>[!IMPORTANT]
Győződjön meg arról, hogy alkalmazta a jelen útmutató SQL-telepítéssel foglalkozó szakaszában tárgyalt ajánlott eljárásokat.                                                                                                                                                      |

### 1. lépés: Az SQL Server konfigurálása a kezdeti adatbetöltéshez
<a id="step-1-configure-the-sql-server-for-initial-data-load" class="xliff"></a>
Ha sok adatot szeretne kezdetben betölteni, akkor lerövidítheti az adatbázis feltöltésének időtartamát a teljes szöveges keresés átmeneti kikapcsolásával és a MIM 2016 kezelőügynökbe (FIM MA) való exportálás befejezése utáni újbóli bekapcsolásával.

A teljes szöveges keresés átmeneti kikapcsolásához:

1.  Indítsa el az SQL Server Management Studio eszközt.

2.  Válassza a New Query (Új lekérdezés) lehetőséget.

3.  Futtassa a következő SQL-utasításokat:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING =
MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

Fontos tisztában lenni az SQL Server helyreállítási modell lemezkövetelményeivel. A biztonsági mentési ütemezéstől függően előfordulhat, hogy a kezdeti rendszerbetöltés során ajánlott az egyszerű helyreállítási módot használni a lemezterület használatának korlátozásához, de ismernie kell az adatvesztés kockázatát illető következményeket.
Teljes helyreállítási mód használatakor szükséges a lemezterület biztonsági másolatokra való használatának felügyelete, többek között a tranzakciónapló gyakori biztonsági mentései magas lemezterület-használatának megakadályozására.

>[!IMPORTANT]
Ezen eljárások elmulasztása magas lemezterület-használatot eredményezhet, és előfordulhat, hogy elfogy a szabad lemezterület. További információt ebben a témakörben a [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (A helyreállítási modell áttekintése) című részben találhat. A [FIM biztonsági mentési és visszaállítási útmutatója](http://go.microsoft.com/fwlink/?LinkID=165864) további információkat tartalmaz (angol nyelven).

### 2. lépés: A minimálisan szükséges MIM-konfiguráció alkalmazása a betöltési folyamat során
<a id="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process" class="xliff"></a>

A kezdeti betöltése során csak a FIM-konfigurációhoz a felügyeletiházirend-szabályokhoz (MPR-ek) és a készletdefiníciókhoz minimálisan szükséges konfigurációt kell alkalmazni. Az adatok betöltése után hozza létre az adott környezethez szükséges további készleteket. A szabályzatoknak a betöltött adatokra való visszamenőleges alkalmazásához használja a munkafolyamatok Run On Policy Update (Futtatás szabályzatfrissítéskor) beállítását.

### 3. lépés: A FIM szolgáltatás konfigurálása és feltöltése külső azonosító adatokkal
<a id="step-3-configure-and-populate-the-fim-service-with-external-identity-data" class="xliff"></a>


Ehhez kövesse a felhasználók az Active Directoryból a FIM-be való szinkronizálásának

menetét ismertető cikk lépéseit a rendszer konfigurálásához és szinkronizálásához az Active Directory-beli felhasználókkal. A csoportinformációk szinkronizálásának folyamatát a How Do I Synchronize Groups from Active Directory Domain Services to FIM (A csoportok az Active Directory tartományi szolgáltatásokból a FIM-be való szinkronizálásának menete) című útmutatóban találja.

#### A szinkronizálás és az exportálás sorrendje
<a id="synchronization-and-export-sequences" class="xliff"></a>

A teljesítmény optimalizálása érdekében az exportálást az összekötőtérben nagy számú függőben lévő exportálási műveletet eredményező szinkronizálás után kell futtatni.

Ezután futtasson megerősítő importálást az érintett összekötőtérrel társított kezelőügynökön. Például, ha a kezdeti adatbetöltés részeként több kezelőügynökön kell szinkronizálási futtatási profilokat futtatni, minden egyes szinkronizálásfuttatás után futtatnia kell egy exportálást, majd egy különbözeti importálást.

Minden az inicializálási ciklus részét képező forrás-kezelőügynök esetén hajtsa végre az alábbi lépéseket:

1.  Teljes importálás a forrás-kezelőügynökön.

2.  Teljes szinkronizálás a forrás-kezelőügynökön.

3.  Exportálás az összes érintett cél-kezelőügynökön szakaszos exportálási műveletekkel.

4.  Különbözeti importálás az összes érintett cél-kezelőügynökön szakaszos exportálási műveletekkel.

### 4. lépés: A teljes MIM-konfiguráció alkalmazása
<a id="step-4-apply-your-full-mim-configuration" class="xliff"></a>


A kezdeti adatbetöltés befejezése után alkalmaznia kell a teljes MIM-konfigurációt az adott környezethez.

Az alkalmazási helyzettől függően ebbe beletartozik a további készletek, MPR-ek és munkafolyamatok létrehozása. Az esetleges minden meglévő rendszerbeli objektumra visszamenőlegesen alkalmazandó szabályzatok esetén használja a Run On Policy Update (Futtatás szabályzatfrissítéskor) beállítást a munkafolyamatok esetén ezen szabályzatoknak a betöltött adatokra való visszamenőleges alkalmazásához.

### 5. lépés: Az SQL újrakonfigurálása az előző beállításokra
<a id="step-5-reconfigure-sql-to-previous-settings" class="xliff"></a>


Ne felejtse el módosítani az SQL-beállításokat a normál beállításokra. Ez a következő teendőket foglalja magában:

-   A teljes szöveges keresés bekapcsolása

-   A biztonsági mentési szabályzat frissítése a szervezeti házirend szerint

Miután befejezte a kezdeti adatbetöltést, kapcsolja be újra a teljes szöveges keresést. Futtassa a következő SQL-utasításokat a teljes szöveges keresés újbóli bekapcsolásához:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Ha egyszerű helyreállítási módra kell váltani, győződjön meg arról, hogy újrakonfigurálta a biztonsági mentési ütemezését a szervezet biztonsági mentési szabályzatának megfelelően. A FIM biztonsági mentési ütemezésről további részleteket a [FIM biztonsági mentési és visszaállítás útmutatójában](http://go.microsoft.com/fwlink/?LinkID=165864) talál (angol nyelven).

## Konfiguráció migrálása
<a id="configuration-migration" class="xliff"></a>


### Ne módosítsa a megjelenített neveket
<a id="avoid-changing-display-names" class="xliff"></a>

Számos objektumtípus, például MPR-ek esetén a syncproduction.ps1 szkript a megjelenítési nevet használja egyetlen horgonyattribútumként a két rendszer között. Ennél fogva egy meglévő MPR megjelenített nevének módosítása az új MPR létrehozása után a meglévő MPR törlését eredményezi. Ennek oka az, hogy a migrálási folyamat nem tudja sikeresen összekötni azon MPR-eket, amelyek illesztési feltételei megváltoztak. A probléma elkerülése érdekében minden konfigurációs objektumtípus esetén hozzáköthet egy egyéni attribútumot, és ezt az attribútumot használhatja illesztési feltételként. Ez lehetővé teszi a megjelenített nevek módosítását a migrálási folyamat befolyásolása nélkül.

### Ne változtassa meg a köztes fájlok tartalmát
<a id="avoid-changing-the-content-of-intermediate-files" class="xliff"></a>

Bár az alacsony szintű objektumok fájlformátuma és API-ja nyilvános, és a fejlesztők támogatják a módosításukat, nem ajánlott megváltoztatni a köztes formátumok tartalmát a migrálás során. Azonban szükség lehet teljes ImportObject-bejegyzések eltávolítására a changes.xml fájlból vagy keresési és csereműveletek végrehajtása a pilot.xml fájlon a verziószámok vagy a próbaverziós tartománynévrendszer (DNS) adatai a DNS éles verziójának adataira való lecseréléséhez.

### A verziók közötti migráláskor győződjön meg arról, hogy a verziószám megfelelő a pilot.xml fájlban
<a id="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions" class="xliff"></a>

Bár a verziószámok közötti migrálás nem javasolt és nem támogatott, gyakran elvégezheti ezt a próbaverziószámot az éles verziószámra cserélve a pilot.xml fájlban. Pontosabban a WorkflowDefinition és

ActivityInformationConfiguration objektumok igényelnek pontosan az éles környezetbeli munkafolyamat-tevékenységekre vonatkozó verziószámot. Ha a verziószámot nem cseréli le, a Compare-FIMConfig parancsmag azonosítja a WorkflowDefinitions XOML-attribútumai közötti eltéréseket, és migrálja a próbaverzió számát. Az éles környezetbeli FIM szolgáltatás indítása a munkafolyamat-tevékenységek a helytelen verziószámmal sikertelen lehet.

### Kerülje a körkörös hivatkozásokat
<a id="avoid-cyclic-references" class="xliff"></a>

Általában körkörös hivatkozások nem ajánlottak MIM-konfigurációkban.
Azonban néha előfordulhat körkörös hivatkozás, ha az A készlet a B készletre hivatkozik, és a B is hivatkozik az A-ra. A körkörös hivatkozások okozta problémák elkerülése érdekében módosítania kell az A vagy a B készlet definícióját, hogy mindkettő ne hivatkozzon egymásra. Ezután indítsa újra a migrálási folyamatot. Ha körkörös hivatkozások vannak, és a Compare-FIMConfig parancsmag hibát eredményez, manuálisan kell a körkörös hivatkozást megszüntetni. A Compare-FIMConfig parancsmag kimenete elsőbbségi sorrendben jeleníti meg a szükséges módosítások listáját, ezért a konfigurációs objektumok hivatkozásai között nem szerepelhet körkörös hivatkozás.

## Biztonság
<a id="security" class="xliff"></a>

### MIM MA-fiók
<a id="mim-ma-account" class="xliff"></a>

A MIM MA-fiók nem számít szolgáltatásfióknak, és egy szokásos felhasználói fióknak kell lennie. A fiókoknak be kell tudniuk jelentkezni helyileg ahhoz, hogy a FIM szinkronizálási szolgáltatás szolgáltatásfiókja megszemélyesíthesse.

A MIM MA-fiók helyi bejelentkezésének engedélyezéséhez:

1.  Kattintson a Start gombra, mutasson a Felügyeleti eszközök pontra, és kattintson a Helyi biztonsági házirend elemre.

2.  Nyissa meg a Helyi házirendek csomópontot, majd kattintson a Felhasználói jogok kiosztása elemre.

3.  A Helyi bejelentkezés engedélyezése házirendben győződjön meg arról, hogy a FIM MA-fiók explicit módon van-e megadva, vagy adja hozzá egy olyan csoporthoz, amely már rendelkezik hozzáféréssel.

### A FIM szinkronizálási szolgáltatás és a FIM Services-fiókok
<a id="fim-synchronization-service-and-fim-services-accounts" class="xliff"></a>

A MIM-kiszolgáló összetevőt futtató kiszolgálók biztonságos módon való konfigurálásához a szolgáltatásfiókokat korlátozni kell. Az előző eljárással engedélyezve a MIM MA-fiókot állítsa be a FIM szinkronizálási szolgáltatás és a FIM Services-fiókokat a következő korlátozásokkal:

-   Kötegelt feladatként való bejelentkezés megtagadása

-   Helyi bejelentkezés megtagadása

-   A számítógép hálózati elérésének megtagadása

A szolgáltatásfiókok nem lehetnek a helyi Rendszergazdák csoport tagjai.

A FIM szinkronizálási szolgáltatás szolgáltatásfiók nem lehet a FIM szinkronizálási szolgáltatáshoz való hozzáférés szabályozása használt biztonsági csoport (például a FIMSync kezdetű csoportok, mint a FIMSyncAdmins, és így tovább) tagja.

>[!IMPORTANT]
 Ha a készletekben ugyanazon fiók használatát választja mindkét szolgáltatási fiókhoz, és szétválasztja a FIM szolgáltatást és a FIM szinkronizációs szolgáltatást, akkor az mms Szinkronizációs szolgáltatás kiszolgálóján nem tilthatja le a hozzáférést ehhez a számítógéphez a hálózatról. Ha a hozzáférés meg lenne tagadva, ez megtiltaná a FIM szolgáltatás számára, hogy kapcsolatba lépjen a FIM szinkronizációs szolgáltatással a konfiguráció megváltoztatásához és a jelszavak kezeléséhez.

### A kioszkmódhoz hasonló módon üzemelő számítógépekre telepített jelszó-visszaállításnak helyi biztonsági beállítással kell rendelkeznie a virtuális memória lapozófájljának törléséhez
<a id="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile" class="xliff"></a>

A FIM jelszó-visszaállítás kioszkként használandó munkaállomáson történő telepítéskor javasoljuk, hogy kapcsolja be a helyi biztonsági házirend Leállítás: Virtuális memória lapozófájljának törlése beállítását annak biztosítására, hogy a folyamat memóriájából származó érzékeny információkhoz jogosulatlan felhasználók ne férhessenek hozzá.

### SSL implementálása a FIM-portálon
<a id="implementing-ssl-for-the-fim-portal" class="xliff"></a>

Határozottan javasoljuk, hogy használjon SSL-t a FIM-portál kiszolgálóján az ügyfelek és a kiszolgáló közötti adatforgalom biztonságossá tétele érdekében.

Az SSL implementálásához:

1.  A MIM-portál kiszolgálóján nyissa meg az IIS Manager alkalmazást.

2.  Kattintson a helyi számítógép nevére.

3.  Kattintson a Kiszolgálói tanúsítványok elemre.

4.  Kattintson a Tanúsítványkérelem létrehozása elemre.

5.  A Köznapi név mezőben adja meg a kiszolgáló nevét.

6.  Kattintson a Tovább, majd ismét a Tovább gombra.

7.  Mentse tetszőleges helyre a fájlt. A későbbi lépésekben szüksége lesz a hely elérésére.

8.  A Windows Internet Explorer® böngészőben nyissa meg a https://kiszolgálónév/certsrv oldalt. A „kiszolgálónév” részt cserélje le a tanúsítványokat kiállító kiszolgáló nevére.

9.  Kattintson az Új tanúsítvány kérése lehetőségre.

10. Kattintson a Speciális kérelem küldése lehetőségre.

11. Kattintson a Tanúsítványkérelem továbbítása base-64 kódolású fájl használatával lehetőségre.

12. Illessze be a fájlt, amelynek tartalmát az előző lépésben mentette.

13. A Tanúsítványsablon csoportban válassza a Webkiszolgáló elemet.

14. Kattintson a Küldés gombra.

15. Mentse a tanúsítványt asztali számítógépére.

16. Az IIS-kezelőben kattintson a Tanúsítványkérelem kitöltése lehetőségre.

17. Irányítsa az IIS-kezelőt az asztali számítógépére nemrég mentett tanúsítványra.

18. A Rövid név mezőbe írja a kiszolgáló nevét.

19. A Helyek gombra kattintva válassza a SharePoint – 80-as port lehetőséget.

20. Kattintson a Kötések, majd a Hozzáadás lehetőségre.

21. Jelölje be a https elemet.

22. A tanúsítványként jelölje ki azt, amelynek neve megegyezik a kiszolgálóéval (azaz az imént importált tanúsítványt).

23. Kattintson az OK gombra.

24. Távolítsa el a HTTP-kötést.

25. Kattintson az SSL-beállításokra, majd jelölje be az SSL megkövetelése négyzetet.

26. Mentse a beállításokat.

27. Kattintson a Start menü Felügyeleti eszközök pontjára, majd kattintson a Központi SharePoint-felügyelet 3.0-s verzió elemre.

28. Kattintson a Tevékenység, majd a Másodlagos címek leképezése elemre.

29. Kattintson a http://kiszolgálónév elemre.

30. A http://kiszolgálónév helyett adja meg a https://kiszolgálónév formát, és kattintson az OK gombra.

31. Kattintson a Start gombra, majd a Futtatás parancsra, írja be az iisreset parancsot, majd kattintson az OK gombra.

## Performance (Teljesítmény)
<a id="performance" class="xliff"></a>

Az optimális teljesítménykonfigurációhoz:

-   Alkalmazza a jelen a dokumentum SQL-telepítő szakaszában leírt ajánlott eljárásokat.

-   Kapcsolja ki a SharePoint-indexelést a FIM 2010 R2 Portal webhelyén. További tudnivalókat a jelen dokumentum SharePoint-indexelés letiltása című szakaszában találhat.

## Funkcióspecifikus ajánlott eljárások (El szeretném távolítani ezt, és összecsukni ezt a szakaszt, és csak adott funkciók legyenek a 2. fejléc szintjén a 3. helyett)
<a id="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3" class="xliff"></a>


### Kérelmek kezelése
<a id="request-management" class="xliff"></a>

Alapértelmezés szerint a MIM 2016 kiüríti a lejárt rendszerobjektumokat, amelyek 30 napos intervallumon belül tartalmazzák a befejezett kéréseket a hozzá kapcsolódó jóváhagyásokkal, a jóváhagyási válaszokkal és a munkafolyamat-példányokkal együtt. Ha a szervezetnek hosszabb kérelemelőzményekre van szüksége, exportálnia kell a MIM-től érkező kéréseket és tárolnia kell őket egy kiegészítő adatbázisban, hogy megőrizze őket a 30 napos időszakon túl. Míg a 30 napos kérelemtörlési időszak konfigurálható, az időszak kiterjesztése negatívan befolyásolhatja a teljesítményt a rendszer további objektumai miatt.

### Felügyeletiházirend-szabályok
<a id="management-policy-rules" class="xliff"></a>

#### Használja a megfelelő MPR-típust
<a id="use-the-appropriate-mpr-type" class="xliff"></a>

A MIM kétféle MPR-t használ, a kérelmet és a készletátmenetet:

-   Kérelem-MPR (RMPR)

 - Az erőforrásokon végzett műveletek létrehozási, olvasási, frissítési vagy törlési (CRUD) hozzáférés-vezérlési házirendjének (hitelesítési, engedélyezési és műveleti) meghatározásához használatos.
 - Akkor alkalmazza a rendszer, ha egy CRUD-művelet kiadására kerül sor egy cél-erőforrásra a FIM-ben.
   - Hatóköre a szabályban megadott megfelelési feltételek, azaz hogy mely CRUD-kérelmekre vonatkozik a szabály.


-   Készletátmenet-MPR (TMPR)
 - A szabályzat meghatározására használatos függetlenül attól, hogy az objektum milyen módon lépett a készletátmenet által jelzett aktuális állapotba. Használja a TMPR-t jogosultsági szabályzatok modellezésére.
 - Akkor alkalmazza a rendszer, amikor egy erőforrás belép egy kapcsolódó készletbe, vagy elhagyja azt.
 - Hatóköre a készlet tagjaira terjed ki.

>[MEGJEGYZÉS] További részletekért lásd: [Üzleti szabályok tervezése](http://go.microsoft.com/fwlink/?LinkID=183691).

#### Csak szükség esetén engedélyezze az MPR-eket
<a id="only-enable-mprs-as-necessary" class="xliff"></a>

A konfiguráció alkalmazásakor használja a legalacsonyabb jogosultsági szint elvét. Az MPR-ek a FIM környezet hozzáférési szabályzatát szabályozzák. Csak a felhasználók többsége által használt szolgáltatásokat engedélyezze. Nem minden felhasználó használhatja például FIM-et csoportfelügyeletre, így a társított csoportkezelési MPR-ek le lesznek tiltva. Alapértelmezés szerint a FIM-ben le van tiltva a legtöbb nem rendszergazdai jogosultság.

#### A beépített MPR-ek közvetlen módosítása helyett másolja azokat
<a id="duplicate-built-in-mprs-instead-of-directly-modifying" class="xliff"></a>

Ha módosítani szeretné a beépített MPR-eket, létre kell hoznia egy új MPR-t a szükséges konfigurációval és ki kell kapcsolnia a beépített MPR-t. Ez biztosítja, hogy a beépített MPR-ek a frissítési folyamaton keresztül bevezetett jövőbeli módosításai nem lesznek negatív hatással a rendszerkonfigurációra.

#### A végfelhasználói engedélyeknek a felhasználók üzleti igényeihez kötött kifejezett attribútumlistákat kell használnia
<a id="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs" class="xliff"></a>

A kifejezett attribútumlisták használatával megakadályozható az engedélyek véletlen megadása a rendszerjogosultsággal nem rendelkező felhasználók számára, amikor attribútumokat adnak hozzá objektumokhoz.
A rendszergazdáknak explicit módon kell hozzáférést adniuk az új attribútumokhoz a hozzáférés eltávolítása helyett.

Az adatok elérését a felhasználók üzleti igényeihez kell kötni. Például a csoportok tagjai nem férhetnek hozzá azon csoport szűrőattribútumához, amelyhez tartoznak. A szűrő véletlenül olyan szervezeti adatokat fedhet fel, amelyekhez a felhasználó normál esetben nem férhetne hozzá.

#### Az MPR-eknek a rendszerbeli vonatkozó engedélyeket kell tükrözniük
<a id="mprs-should-reflect-effective-permissions-in-the-system" class="xliff"></a>

Kerülje engedélyek megadását olyan attribútumokhoz, amelyeket a felhasználó soha nem használhat. Például nem adhat engedélyt az alapvető erőforrás-attribútumok, például az objectType módosítására. Az MPR ellenére a rendszer megtagadja az erőforrástípus létrehozását követő bármilyen módosítási kísérletet.

#### Az olvasási engedélyeknek el kell különülniük a módosítási és létrehozási engedélyektől, ha explicit attribútumokat használ az MPR-ekben
<a id="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs" class="xliff"></a>

Az MPR-kben szereplő attribútumok explicit listázásakor a létrehozáshoz és a módosításhoz szükséges attribútumok általában eltérnek az olvasáshoz elérhető attribútumoktól. Például az olvasási engedély megadható a rendszerattribútumok, például a létrehozó vagy az objektumazonosító felett, míg a rendszerattribútumoknál nem lehet megadni a Létrehozás vagy a Módosítás lehetőséget.

#### A létrehozási engedélyeknek el kell különülniük a módosítási engedélyektől, ha explicit attribútumokat használ a szabályokban
<a id="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules" class="xliff"></a>

A létrehozási művelet megköveteli, hogy a felhasználó kiválassza az objectType elemet a művelet során. Ez az alapvető rendszerattribútum nem módosítható a létrehozási művelet után.

#### Egy kérelem-MPR-t használjon az összes attribútumhoz ugyanazon hozzáférési követelményekkel
<a id="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements" class="xliff"></a>

Olyan azonos hozzáférési követelményekkel rendelkező attribútumok esetén, amelyek várhatóan nem változnak, a hatékonyság érdekében egyetlen kérelem- MPR-ben kombinálhatja azokat.

#### Kerülje a korlátlan hozzáférés megadását még a kiválasztott egyszerű rendszercsoportokhoz is
<a id="avoid-giving-unrestricted-access-even-to-selected-principal-groups" class="xliff"></a>

A FIM-ben az engedélyek definiálása pozitív állításként történik. Mivel a FIM nem támogatja a megtagadási engedélyeket, az erőforrásokhoz való korlátlan hozzáférés megadása bonyolítja bármely kizárás megadását az engedélyekben. Ajánlott eljárásként azt javasoljuk, hogy csak a szükséges engedélyeket adja meg.

>[!NOTE]
A Jogosultságok szakasz alább következik. Azon tűnődök, hogyan lehet egyesíteni őket, hogy elkerüljem az 5. szintű fejlécek létrehozását
#### Használjon TMPR-eket az egyéni jogosultságok megadásához
<a id="use-tmprs-to-define-custom-entitlements" class="xliff"></a>

Egyéni jogosultságok megadása helyett készletátmenet-MPR-eket (TMPR-eket) használjon.
A TMPR-ek állapotalapú modellt biztosítanak a jogosultságok hozzárendeléséhez vagy eltávolításához a definiált átmeneti készletek tagsága, illetve a szerepkörök és hozzájuk tartozó munkafolyamat-tevékenységek alapján. A TMPR-eket mindig párokban kell meghatározni, az egyiket a befelé áthelyezett erőforrásokhoz, egyet pedig a kifelé áthelyezettek számára. Emellett minden átmeneti MPR-nek külön munkafolyamatokat kell tartalmaznia a kiépítési és megszüntetési tevékenységekhez.

>[!NOTE]
Minden megszüntetési munkafolyamatnak biztosítania kell, hogy a Futtatás szabályzatfrissítéskor attribútum igaz értékű legyen.

#### Készletátmenet bejövő MPR-jének engedélyezése utolsóként
<a id="enable-the-set-transition-in-mpr-last" class="xliff"></a>

A TMPR-pár létrehozásakor a készletátmenet bejövő MPR-jének bekapcsolása utolsóként. Ez a sorrend biztosítja, hogy nem marad erőforrás a jogosultsággal, ha hozzáadják, illetve eltávolítják a készletből, miközben a bejövő MPR be van kapcsolva, de mielőtt a kimenő MPR ki van kapcsolva.

#### A TMPR-beli munkafolyamatok először a célerőforrás állapotát kell ellenőrizzék
<a id="workflows-in-tmpr-should-check-target-resource-state-first" class="xliff"></a>

A kiépítési munkafolyamatnak először azt ellenőrizniük, hogy a célként megadott erőforrás már ki lett-e építve a jogosultságnak megfelelően. Ha igen, nincs teendő.

A megszüntetési munkafolyamatoknak először azt ellenőrizniük, hogy a célként megadott erőforrás már ki lett-e építve. Ha igen, meg kell szüntetnie azt.
Ellenkező esetben nincs teendő.

#### Válassza a Run On Policy Update (Futtatás szabályzatfrissítéskor) lehetőséget a TMPR-ekhez
<a id="select-run-on-policy-update-for-tmprs" class="xliff"></a>

Ez biztosítja a megfelelő kiépítési működést a szabályzatfrissítések bevezetésekor és a Run On Policy Update (Futtatás szabályzatfrissítéskor) jelző használatakor a TMPR-ekhez társított munkafolyamatokon. Ez biztosítja, hogy a szabályzatdefiníciók módosításai az átmeneti készlet új tagjai számára is alkalmazzák a munkafolyamatokat.

#### Kerülje ugyanazon jogosultság társítását két különböző átmeneti készlethez
<a id="avoid-associating-the-same-entitlement-with-two-different-transition-sets" class="xliff"></a>

Ugyanazon jogosultság társítása két különböző átmeneti készlethez jogosultságok szükségtelen visszavonását és újbóli megadását okozhatja, ha az erőforrás egyik készletből a másikba helyeződik át. Győződjön meg róla, hogy egy készlet az összes olyan erőforrást tartalmazza, amelyhez a társított jogosultság szükséges. Ez biztosítja, hogy egy átmeneti készlethez csak egy, a jogosultságot megadó munkafolyamat tartozhat, és viszont.

#### A jogosultságok a rendszerből való eltávolításakor használja a megfelelő műveleti sorrendet
<a id="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system" class="xliff"></a>

A jogosultságok a rendszerből való eltávolításakor végrehajtott lépések sorrendje két különböző működési eredményt eredményezhet. Tisztában kell lennie azzal, hogy melyik sorrend eredményezi az elérni kívánt hatást.

A jogosultság eltávolításához a rendszerből (és visszavonásához a jelenleg azzal rendelkező összes tagtól):

1.  Tiltsa le a készletátmenet bejövő MPR-jét. Ezzel elkerülhetők az új jogosultság-megadások.

2.  Törölje az átmeneti készlet szűrőjét, vagy módosítsa oly módon, hogy a készlet üres legyen. Ez az összes meglévő tag kifelé történő áthelyezését váltja ki, és alkalmazza a kifelé történő áthelyezési szabályzatot, beleértve a jogosultsághoz társított beállított megszüntetési munkafolyamatokat is.

3.  Tiltsa le a készletátmenet kimenő MPR-jét.

A jogosultság eltávolításához, de a jelenlegi tagok békén hagyásához (például a FIM jogosultságkezelésre való használatának leállítása):

1.  Tiltsa le a készletátmenet bejövő MPR-jét. Ezzel elkerülhetők az új jogosultság-megadások.

2.  Tiltsa le a készletátmenet kimenő MPR-jét.

3.  Törölje az átmeneti készlet szűrőjét, vagy módosítsa oly módon, hogy a készlet üres legyen. Mivel a készlet már nem kötődik TMPR-hez, nem kerül sor megszüntetési munkafolyamat alkalmazására.

### Készletek
<a id="sets" class="xliff"></a>

A készletekre vonatkozó gyakorlati tanácsok alkalmazásakor vegye figyelembe a kezelhetőség optimalizálása és a jövőbeli egyszerű felügyelet szempontjait.
Az itt szereplő ajánlások alkalmazása előtt megfelelő tesztelést kell végezni a várható éles üzemelési körülmények között, hogy meg lehessen határozni a megfelelő egyensúlyt a teljesítmény és a kezelhetőség között.

>[!NOTE]
Az alábbi irányelvek a dinamikus készletekre és a dinamikus csoportokra vonatkoznak.


#### Minimalizálja a dinamikus beágyazás használatát
<a id="minimize-the-use-of-dynamic-nesting" class="xliff"></a>

Ez egy olyan készlet szűrőjére utal, amely egy másik készlet ComputedMember attribútumára hivatkozik. A készletek beágyazásának gyakori indoka, hogy elkerülhető legyen a tagsági feltételek többszörözése a készletekben. Bár ez a módszer a készlet jobb kezelhetőségét eredményezheti, a teljesítményre vonatkozóan kompromisszumot jelent. Úgy optimalizálhatja a teljesítményt, ha ahelyett, hogy magát a készletet ágyazná be, többszörözi a beágyazott készlet tagsági feltételeit.

Előfordulhat, hogy nem kerülheti el a készletek beágyazását a működési követelmények kielégítése érdekében. Az alábbiak a leggyakoribb olyan helyzetek, amelyekben készleteket kell beágyaznia. Például a teljes idejű alkalmazott tulajdonosok nélküli csoportok készletének definiálásához a készletek beágyazását kell használni az alábbiak szerint: `/Group[not(Owner =
/Set[ObjectID = ‘X’]/ComputedMember]`, ahol az ’X’ az összes teljes idejű alkalmazott készletének objektumazonosítója.

#### Minimalizálja a negatív feltételek használatát
<a id="minimize-the-use-of-negative-conditions" class="xliff"></a>

A negatív feltételek azon tagsági feltételek, amelyek a következő operátorokat vagy függvényeket használják: `!=`, `not()`, `\<` , `\<=`. Ha optimalizálni szeretné a teljesítményt, ahol lehetséges, fejezze ki a feltételeket több pozitív feltétellel, mintsem negatív feltételként.

#### Minimalizálja a többértékű referenciaattribútumokon alapuló tagsági feltételek használatát
<a id="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes" class="xliff"></a>

A többértékű referenciaattribútumokon alapuló feltételek használata kerülendő, mert ezen készletek nagy száma befolyásolhatja a tagsági feltételben használt attribútumon végzett műveletek teljesítményét.

### Jelszó-visszaállítás
<a id="password-reset" class="xliff"></a>

#### A jelszó-visszaállításhoz használt kioszkmódban üzemelő számítógépeknél a helyi biztonságot a virtuális memória lapozófájljának törlésére kell beállítani
<a id="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile" class="xliff"></a>

A FIM 2010 jelszó-visszaállítás kioszkként használandó munkaállomáson történő telepítéskor javasoljuk, hogy kapcsolja be a helyi biztonsági házirend Leállítás: Virtuális memória lapozófájljának törlése beállítását annak biztosítására, hogy a folyamat memóriájából származó érzékeny információkhoz jogosulatlan felhasználók ne férhessenek hozzá.

#### A felhasználóknak mindig azon a számítógépen kell regisztrálniuk a jelszó-visszaállításra, amelyen bejelentkeztek
<a id="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to" class="xliff"></a>

Amikor egy felhasználó megpróbál regisztrálni a jelszó-visszaállításra egy webportálon keresztül, a FIM 2010 mindig a bejelentkezett felhasználó nevében kezdeményezi a regisztrációt, függetlenül attól, hogy ki van bejelentkezve a webhelyen. A felhasználóknak mindig azon a számítógépen kell regisztrálniuk a jelszó-visszaállításra, amelyen bejelentkeztek.

#### Ne állítsa az AvoidPdcOnWan beállításkulcsot igaz értékre
<a id="do-not-set-the-avoidpdconwan-registry-key-to-true" class="xliff"></a>

A MIM 2016 jelszó-visszaállításra való használata esetén ne állítsa igaz értékűre az AvoidPdcOnWan beállításkulcsot.

Ha a beállításkulcs értéke igaz, a felhasználó nagy valószínűséggel végighalad a jelszókapukon, az elsődleges tartományvezérlőn (PDC) megtörténik a jelszó-visszaállítás, és megpróbál bejelentkezni. A beállításkulcs miatt a helyi tartományvezérlő nem végzi el a másodlagos érvényesítést az elsődleges tartományvezérlővel, ezért elutasítja a belépési kérelmet. Ha a rendszer elegendő alkalommal tagadta meg a hozzáférést a felhasználótól, sor kerülhet a tartományból való kizárására, és hívnia kell az ügyfélszolgálatot.

#### Ne kapcsolja be a tiszta szöveges jelszavak naplózását
<a id="do-not-turn-on-logging-of-clear-text-passwords" class="xliff"></a>

Lehetőség van a tiszta szöveges jelszavak naplózására a diagnosztikai szolgáltatásiszint-nyomkövetés bekapcsolásával a Windows

Communication Foundation (WCF) szolgáltatásban. Ez a beállítás nincs bekapcsolva alapértelmezés szerint, és az éles környezetben bekapcsolása nem ajánlott. Ezek a jelszavak tiszta szöveges elemekként jelennek meg a titkosított SOAP-üzenetekben, amikor a felhasználók a jelszó-visszállításra regisztrálnak. További információ: [Configuring Message Logging](http://go.microsoft.com/fwlink/?LinkID=168572) (Az üzenetnaplózás konfigurálása).

#### Az engedélyezési munkafolyamat nem feleltethető meg a jelszó-visszállítási folyamatnak
<a id="do-not-map-an-authorization-workflow-to-the-password-reset-process" class="xliff"></a>

Az engedélyezési munkafolyamatokat nem kapcsolhatja össze egy jelszó-visszaállítási művelettel.
A jelszó-visszaállításhoz szinkron válaszra van szükség, és jóváhagyási munkafolyamatok pedig aszinkron tevékenységeket tartalmaznak, például a jóváhagyást.

#### Több tevékenység nem feleltethető meg a jelszó-visszaállításnak
<a id="do-not-map-multiple-action-activities-to-password-reset" class="xliff"></a>

Nem lehet összekapcsolni egynél több művelet tevékenységet tartalmazó munkafolyamatot a jelszó-visszaállítási művelettel. Ilyen eset lenne például egy második AD DS jelszó-visszaállítási tevékenység hozzákapcsolása egy jelszó-visszaállítási MPR-hez. Ez a forgatókönyv nem támogatott.

#### A meglévő munkafolyamatok hozzáadása, eltávolítása vagy módosítása esetén követelje meg az újraregisztrálást
<a id="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow" class="xliff"></a>

A meglévő munkafolyamatban a hitelesítési tevékenységek hozzáadásakor, eltávolításakor vagy sorrendjének módosításakor mindig jelölje be az újraregisztrálás megkövetelését. Azok a felhasználók, akik megpróbálnak hitelesíteni a jelszó-visszaállításhoz, miután egy tevékenységet hozzáadtak vagy eltávolítottak egy munkafolyamatból, de mielőtt regisztrálták őket, nemkívánatos hatásokkal találkozhatnak.

### Portálkonfiguráció és erőforrásvezérlés-megjelenítési konfiguráció
<a id="portal-configuration-and-resource-control-display-configuration" class="xliff"></a>

#### Érdemes megfontolni egy adatvédelmi nyilatkozat hozzáadását a felhasználói profil oldalához
<a id="consider-adding-a-privacy-disclaimer-to-the-user-profile-page" class="xliff"></a>

A MIM-ben alapértelmezés szerint egyes felhasználói profilok adatai megjelenhetnek más felhasználók számára. A felhasználók segítése érdekében a rendszergazdáknak fontolóra kell venniük a cég szabályzataival összhangban lévő egyéni szöveg hozzáadását a Felhasználói profil oldalhoz. Egyéni szöveg MIM-portál oldalaihoz történő hozzáadásával kapcsolatos további információkért lásd: Introduction to [Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848) (Bevezetés a FIM-portál konfigurálásába és testreszabásába).

### Séma
<a id="schema" class="xliff"></a>

#### Ne törölje a személy vagy csoport típusú erőforrásokat
<a id="do-not-delete-person-or-group-resource-types" class="xliff"></a>

Bár a Személy és Csoport erőforrástípusok nem alapvető erőforrástípusok, magukat az erőforrásokat és a hozzájuk rendelt attribútumokat ne törölje. A MIM-portál felhasználói felülete megköveteli a Személy és Csoport erőforrástípusok és attribútumaik jelenlétét.

#### Ne módosítsa az alapvető attribútumokat
<a id="do-not-modify-the-core-attributes" class="xliff"></a>

13 alapvető, az összes erőforrástípushoz hozzárendelt attribútum van. Semmilyen módon ne módosítsa fennálló kapcsolataikat bármely erőforrástípussal. A 13 alapvető attribútum:

-   CreatedTime

-   Létrehozó

-   DeletedTime

-   Leírás

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Területi beállítás

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Ne töröljön a naplózási követelményektől függő sémaerőforrást

Ne töröljön sémaerőforrásokat, amíg fennálló naplózási követelmények találhatók ezekhez az erőforrásokhoz.

#### Kis-és nagybetűk megkülönböztetésének kikapcsolása a reguláris kifejezésekben
<a id="making-regular-expressions-case-insensitive" class="xliff"></a>

A FIM-ben hasznos lehet, ha bizonyos reguláris kifejezések nem különböztetik meg a kis-és nagybetűket. A ?!: operátor használatával figyelmen kívül hagyhatja a kis- és nagybetűket. Például az alkalmazott típusa esetén használja az alábbi kifejezést:

`\^(?!:contractor\|full time employee)%.`

#### A tag attribútum kiszámítása
<a id="calculation-of-the-member-attribute" class="xliff"></a>

A szinkronizáló vezérlő számára felfedett Tag attribútum ténylegesen a ComputedMembers attribútumnak van megfeleltetve. Ez a feltételek alapján és a manuálisan kijelölt tagok kombinációja. Még ha a mindhárom attribútumot (Filter, ExplicitMembers és ComputedMembers) is hozzáadja, a tag attribútum dinamikus számítása csak a csoport és a készlet erőforrástípusok esetében történik meg.

#### Kezdő és záró szóközök figyelmen kívül hagyása karakterláncokban
<a id="leading-and-trailing-spaces-in-strings-are-ignored" class="xliff"></a>

A FIM-ben megadhat kezdő és záró szóközökkel rendelkező karakterláncokat, de a FIM figyelmen kívül hagyja ezeket a szóközöket. Egy kezdő és záró szóközzel rendelkező karakterlánc elküldése esetén a szinkronizáló vezérlő és a webszolgáltatások figyelmen kívül hagyják ezeket a szóközöket.

#### Az üres karakterlánc nem egyenlő a null értékkel
<a id="empty-strings-do-not-equal-null" class="xliff"></a>

A FIM ezen kiadásában az üres karakterláncok értéke nem egyenlő a null értékkel. Az üres karakterláncot tartalmazó bemeneti érték érvényes értéknek minősül. Ha nem található bemeneti érték, az null értéknek minősül.

### Munkafolyamat- és a kérelemfeldolgozás
<a id="workflow-and-request-processing" class="xliff"></a>

#### Ne törölje a MIM 2016-tal szállított alapértelmezett munkafolyamatokat
<a id="do-not-delete-default-workflows-that-are-shipped-with-mim-2016" class="xliff"></a>

A következő, a FIM 2010-hez mellékelt munkafolyamatok nem törölhetők:

-   Lejárati munkafolyamat

-   Szűrőérvényesítési munkafolyamat a rendszergazdák számára

-   Szűrőérvényesítési munkafolyamat a nem rendszergazdák számára

-   Csoport lejárati értesítése munkafolyamat

-   Csoport érvényesítése munkafolyamat

-   Tulajdonos jóváhagyási munkafolyamata

-   Jelszó-visszaállítási művelet munkafolyamat

-   Jelszó-változtatási hitelesítési munkafolyamat

-   Kérelmező érvényesítése tulajdonoshitelesítéssel

-   Kérelmező érvényesítése tulajdonoshitelesítés nélkül

-   Regisztrációhoz szükséges rendszer-munkafolyamat

#### Ne futtasson párhuzamosan két vagy több ApprovalActivity-t
<a id="do-not-run-two-or-more-approvalactivities-in-parallel" class="xliff"></a>

Ne futtasson párhuzamosan két vagy több ApprovalActivity-t. Ha így tesz, előfordulhat, hogy a kérelem az engedélyezési fázisban elakad. A többszörös jóváhagyásokhoz vagy csatoljon nagyobb jóváhagyói listát, vagy adja meg a két tevékenység egymás utáni sorrendjét.

#### Az engedélyezési tevékenységek nem módosíthatják MIM-erőforrások adatait
<a id="authorization-activities-should-not-modify-mim-resources-data" class="xliff"></a>

Kerülje a MIM-erőforrásokat, például a függvénykiértékelői tevékenységet a munkafolyamatok részeként módosító tevékenységeket az engedélyezési munkafolyamatokban. Mivel a kérelem véglegesítését a feldolgozás engedélyezési pontjában még nem végezték el, a személyazonosító adatokon végrehajtott módosítások a kérelem esetleges visszautasítása ellenére alkalmazhatók.

### A FIM szolgáltatás partícióinak ismertetése
<a id="understanding-fim-service-partitions" class="xliff"></a>

A FIM célja az olyan FIM-ügyfelek, mint például a FIM Synchronization Service és az önkiszolgáló összetevők által kezdeményezhető kérelmek feldolgozása a konfigurált üzleti szabályzatok szerint. A kialakításból fakadóan mindegyik FIM-szolgáltatáspéldány egy logikai csoporthoz tartozik, amely egy vagy több FIM-szolgáltatáspéldányból, más néven FIM-szolgáltatáspartícióból áll. Ha csak egy FIM-szolgáltatáspéldány van telepítve az összes kérelem kezelésére, akkor előfordulhat, hogy feldolgozási késéseket tapasztal. Egyes műveletek akár az önkiszolgáló műveletekhez megfelelő alapértelmezett időtúllépési értékeket is meghaladhatják. A FIM-szolgáltatáspartíciók segíthetnek e probléma megoldásában. További információt a FIM-szolgáltatás partícióinak ismertetésében talál.


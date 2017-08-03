---
title: "Működik a Microsoft Identity Manager 2016 rendszerhez tartozó referencia |} Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>A Microsoft Identity Manager 2016 funkciók referenciája


A Microsoft Identity Manager (MIM) 2016 funkciók lehetővé teszik a módosítása attribútumértékek előtt áramló őket egy olyan cél számára függvény tevékenységet vagy deklaratív kiépítés. A jelen dokumentum célja a rendelkezésre álló funkciók áttekintését és hogyan használhatók a leírását.

Attribútumfolyam-megfeleltetéseket konfigurálása esetén egy elemi feladat szinkronizálási szabályai. A legegyszerűbb egy attribútum folyam leképezése egy közvetlen leképezés. Ahogy azt a nevet, közvetlen hozzárendelést a forrásattribútum értéket veszi fel, és alkalmazza azt a konfigurált cél attribútum. Előfordulhatnak olyan esetek, amikor vagy kell módosítani kívánt meglévő attribútumértékek vagy ki kell számítani, mielőtt a rendszer alkalmazza azokat a célkiszolgáló új attribútumértékek.

Funkciók a beépített van szüksége a Szinkronizáló vezérlő egy attribútum értékét a cél létrehozásakor alkalmazandó módosítás típusának azonosítására használt módszer.

A mim szoftverben a meglévő funkciók csoportosíthatja a következő kategóriákba sorolhatók:

-   **Az adatkezelési függvényeket**. A különböző karakterláncok adatkezelési műveletek elvégzésére funkciók.

-   **Adatok lekérése funkciók**. Ha adatokat szeretne kinyerni attribútumértékek funkciók.

-   **Adatok generálása funkciók**. Funkciók értékek generálásához.

-   **Logikai funkciók**. Funkciók feltételek alapján műveletek végrehajtásához.

A következő szakaszokban további információt az egyes kategóriák funkciók.

## <a name="data-manipulation-functions"></a>Adatkezelési függvények

Az adatkezelési függvényeket különböző karakterláncok adatkezelési műveletek végrehajtására használhatók.

| Összefűzésére        |   |
|--------------------|-------------------------|
| Leírás        | A ÖSSZEFŰZ függvény segítségével legalább két karakterlánc összefűzésére.                                                                                                       |
| Függvényaláíráshoz | karakterlánc1 + karakterlánc2...                                                                                                                                                     |
| Bemenetek             | Két vagy több karakterlánc                                                                                                                                                        |
| Üzemeltetés         | Az összes bemeneti karakterlánc paraméterek vannak összefűzéséhez egymással.                                                                                                              |
| Kimenet             | Egy karakterlánc        |


| Nagybetűk         |         |
|-------------------|---------|
| Leírás        | A nagybetűssé függvény egy karakterlánc karaktereinek összes nagybetűvel konvertálja.         |
| Függvényaláíráshoz | Karakterlánc UpperCase(string)                                                                                                                                                   |
| Bemenetek             | Egy karakterlánc                                                                                                                                                                 |
| Üzemeltetés         | Csupa kisbetűs karaktert tartalmaz a bemeneti paraméter nagybetűk alakulnak. Példa: UpperCase("test") "TEST" eredményez.                                     |
| Kimenet             | Egy karakterlánc                                                              |


| Kisbetűk          |                                 |
|--------------------|---------------------------------|
| Leírás        | A kisbetűssé függvény egy karakterlánc karaktereinek összes kisbetűsre konvertálja.                                                                                                  |
| Függvényaláíráshoz | Karakterlánc LowerCase(string)                                                                                                                                                   |
| Bemenetek             | Egy karakterlánc                                                                                                                                                                 |
| Üzemeltetés         | A bemeneti paraméter összes nagybetűk alakulnak kisbetűs karaktert tartalmaz. Példa: LowerCase("TeSt") "test" eredményez.                                     |
| Kimenet             | Egy karakterlánc               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Leírás        | A ProperCase funkciót coverts nagybetűvel karakterláncot szóközökkel elválasztott kötetnevek szavak az első karakter, és minden egyéb karakterek kisbetű alakulnak.           |
| Függvényaláíráshoz | Karakterlánc ProperCase(string)                                                                                                                                                  |
| Bemenetek             | Egy karakterlánc                                                                                                                                                                 |
| Üzemeltetés         | A bemeneti paraméter minden szóközökkel elválasztott kötetnevek szó első karaktere nagybetűsre alakítja át, és minden nagybetűk alakulnak kisbetűs karaktert tartalmaz. Ha egy szót a bemeneti paraméter nem ábécé karakterrel kezdődik, a szó első karaktere nem alakítja át nagybetűvel. <br/> Példák: <br/> -ProperCase("TEsT") "Test" eredményez. <br/> -ProperCase("britta simon") "Britta Simon" eredményez. <br/>-ProperCase("TEsT") "Test" eredményez. <br/> -ProperCase("\$TEsT") eredményezi "\$Test".|
| Kimenet             | Egy karakterlánc      |


| LTrim              |      |
|--------------------|------|
| Leírás        | LTrim függvény kezdő szóközöket eltávolít egy karakterláncból.                                                                                                             |
| Függvényaláíráshoz | Karakterlánc LTrim(string)                                                                                                                                                       |
| Bemenetek             | Egy karakterlánc                                                                                                                                                                 |
| Üzemeltetés         | A bemeneti paraméter lévő kezdő szóköz karakterek törlődnek. <br/><br/>Példa: LTrim ("Test") "Test" eredményez.                                              |
| Kimenet             | Egy karakterlánc      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Leírás        | RTrim függvény záró szóközt eltávolít egy karakterláncból.                                                                 |
| Függvényaláíráshoz | Karakterlánc RTrim(string)                                                                                                            |
| Bemenetek             | Egy karakterlánc                                                                                                                      |
| Üzemeltetés         | A rendszer eltávolítja a bemeneti paraméter található záró szóköz karaktereket. Példa: RTrim ("Test") "Test" eredményez.  |
| Kimenet             | Egy karakterlánc                                                                                                                      |


| Trim               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Leírás        | A vágás függvény eltávolítja a kezdő és záró szóközök karakterláncból.                                                      |
| Függvényaláíráshoz | Karakterlánc Trim(string)                                                                                                             |
| Bemenetek             | Egy karakterlánc                                                                                                                      |
| Üzemeltetés         | A karakterláncban található a kezdő és záró szóköztől eltérő karakterek törlődnek. Példa: Vágás ("Test") "Test" eredményez. |
| Kimenet             | Egy karakterlánc                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Leírás        | A RightPad függvény jobb-kézi a megadott kitöltési karakterek használatával megadott hosszúságú karakterláncot.                          |
| Függvényaláíráshoz | String (karakterlánc, hossz, padCharacter) RightPad                                                                                   |
| Üzemeltetés         | Ha a karakterlánc hossza nagyobb, mint, majd padCharacter ismételten fűz hozzá a karakterlánc végén amíg rá nem kényszerül hosszának egyenlőnek hosszát. <br/> Példák: <br/> -RightPad("User", 10, "0") "User000000" eredményezne. <br/> -RightPad(RandomNum(1,10), 5, "0") "9000" eredményezheti.   |
| Kimenet                                                                                                                                                          | Ha karakterlánc hossza nagyobb vagy egyenlő, hosszra, karakterlánc megegyezik egy karakterláncot ad vissza. Ha a karakterlánc hossza nagyobb, mint, majd a kívánt hosszúságú új karakterlánc ad vissza egy padCharacter feltöltve tartalmazó karakterlánc. Karakterlánc értéke null, ha a függvény egy üres karakterláncot ad vissza. |   |   |
>[!NOTE]
**padCharacter** szóköz karakter lehet, de nem lehet null értékű. Ha a hossza **karakterlánc** egyenlőnek vagy annál nagyobb **hossza**, **karakterlánc** változatlan adja vissza.


| LeftPad      |     |
|----|-------|
| Leírás  | A LeftPad függvény bal-kézi a megadott kitöltési karakterek használatával megadott hosszúságú karakterláncot.    |
| Függvényaláíráshoz      | String (karakterlánc, hossz, padCharacter) LeftPad     |
| Bemenetek |  - **Karakterlánc.** A karakterlánc szegélynél. <br/> - **hossza.** Egy karakterlánc kívánt hossza jelző egész számot. <br/> - **padCharacter.** Egy karakterlánc, amely kitöltő karakterként használható egyetlen karakter állhat. |
| Üzemeltetés  | Ha a karakterlánc hossza nagyobb, mint, majd padCharacter ismételten fűz hozzá a karakterlánc elején amíg rá nem kényszerül hosszának egyenlőnek hosszát. <br/> Példák: <br/> -LeftPad("User", 10, "0") "000000User" eredményezne. <br/> LeftPad(RandomNum(1,10), 5, "0") "0009" eredményezheti. |  
|Kimenet | Ha karakterlánc hossza nagyobb vagy egyenlő, hosszra, karakterlánc megegyezik egy karakterláncot ad vissza. <br/> Ha a karakterlánc hossza nagyobb, mint, majd a kívánt hosszúságú új karakterlánc ad vissza egy padCharacter feltöltve tartalmazó karakterlánc. <br/>  Ha **karakterlánc** értéke null, a függvény egy karakterláncot ad vissza üres.                                                   |

<[!NOTE]
**padCharacter** szóköz karakter lehet, de nem lehet null értékű. Ha a hossza **karakterlánc** egyenlőnek vagy annál nagyobb **hossza**, **karakterlánc** változatlan adja vissza.

| BitOr    |  |
|----- |------|
| Leírás  | A BitOr függvény a megadott bit beállítása 1 jelzőt.     |
| Függvényaláíráshoz  | Int BitOr(mask, flag)       |  
| Bemenetek     | 1. **maszk.** Egy hexadecimális érték, amely a bit beállítása a jelző határozza meg. <br/> 2. **jelzőt.** Egy hexadecimális érték, amely az, hogy egy adott bit módosítani.    |   
| Üzemeltetés         | Ez a funkció mindkét paraméter bináris megjelenítése alakítja át, és összehasonlítja azokat: <br/> -Állítása egy kicsit 1 esetén egyik vagy mindkét a megfelelő bit maszk és jelző 1 és 0 Ha mindkét megfelelő bit 0. <br/> -Az 1 értékkel tér vissza minden esetben, ahol a megfelelő mindkét paraméter bitje 0 kivételével. <br/> -Az eredményül kapott bitsorozaton pedig a "beállítása" (1 vagy true) bit bármely két operandust igényelnek. Ha több bits maszk értéke 1 több jelző bits állítható be.  |
| Kimenet             | Egy új verziója **jelző**, a megadott bitként **maszk** értéke 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Leírás        | A BitAnd függvény a megadott bit jelölő 0-ra állítja be.                           |
| Függvényaláíráshoz | Int BitOr(mask, flag)                                                              |
| Bemenetek             | 1. **maszk.** Meghatározza, hogy a módosítandó bit a jelző hexadecimális érték. <br/> 2. **jelzőt.** Az, hogy egy adott bit módosított hexadecimális érték   |
| Üzemeltetés         | Ez a funkció mindkét paraméter bináris megjelenítése alakítja át, és összehasonlítja azokat: <br/> -Állítása egy kicsit 0, ha az egyik vagy mindkét a megfelelő bit **maszk** és **jelző** 0 és 1, ha mind a megfelelő bits 1. <br/> – Kivéve, ha a megfelelő mindkét paraméter bitje 1 minden esetben 0 adja vissza. Ha több bits adható meg a 0 érték több jelző bits állítható be 0 **maszk.** |
| Kimenet             | Egy új verziója **jelző** a megadott bitként **maszk** értéke 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Leírás       | A DateTimeFormat függvény segítségével egy megadott formátumú karakterlánc formában egy dátum és idő formátumban.     |
| Függvényaláíráshoz   | Karakterlánc DateTimeFormat (dátum és idő, formátum)      |
| Bemenetek   | 1. a DateTime típusú érték. A dátum és idő formázása képviselő karakterláncot.  <br/> 2. **formátumban.** A formátum átalakítása képviselő karakterláncot.  |   
| Üzemeltetés           | A dátum és idő karakterláncban a dátum és idő formátumban megadott karakterlánc alkalmazható. <br/> A megadott formátumban karakterláncnak kell lennie egy érvényes dátum és idő formátumban. Ha nem, egy hibaüzenet arról, hogy a formátum nem egy érvényes dátum és idő formátumban. <br/> Példa: DateTime típusú érték ("12/25/2007", "éééé-hh-nn") "2007-12-25" eredményez.|   
| Kimenet     | Egy alkalmazott eredő karakterlánc **formátum** való **dátum és idő.**   |

>[!Note]                                                                                                                                                                             
A karakterek, felhasználó által definiált formátumokat létrehozni elfogadható, lásd: [felhasználói Dátum-/ időformátumok](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Leírás       | A ConvertSidToString egy biztonsági azonosító karakterláncot tartalmazó bájttömb alakítja át.         |
| Függvényaláíráshoz      | Karakterlánc ConvertSidToString(ObjectSID)    |
| Bemenetek  | **ObjectSID.** Egy biztonsági azonosítóval (SID) tartalmazó bájttömb.   |
| Üzemeltetés    | A megadott bináris SID karakterlánccá alakítja.    |
| Kimenet              | A SID-karakterlánc-ábrázolása.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Leírás         | **A ConvertStringToGuid** függvény konvertálja a bináris megjelenítése a GUID GUID karakterláncos ábrázolása.      |
| Függvény            | Byte [] ConvertStringToGuid(stringGuid)  |  
| Bemenetek              | **GUID.** Ebben a mintában formátumú karakterlánc: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, ahol a GUID azonosító értéke 8, 4, 4, 4 csoportokban hexadecimális számjegyeket és 12 számjegyből-ként és betű kötőjelekkel tagolva. Példa a visszatérési érték: "382c74c3-721d-4f34-80e557657b6cbc27".  |
| Üzemeltetés          | A karakterlánc **Guid** paraméterben megadott 1 alakítja át a bináris megjelenítése. <br/> Ha a karakterlánc formátuma nem érvényes ábrázolása **Guid**, a függvény elutasítja a argumentum a következő hiba miatt: <br/> **A paraméter a ConvertStringToGuid függvény egy karakterlánc, amely érvényes Guid-nak kell lennie.**  |
| Kimenet              | A bináris megjelenítése a Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Leírás         | A ReplaceString függvény egy karakterlánc másik karakterláncra összes előfordulását lecseréli.  |   
| Függvény            | Karakterlánc-ReplaceString (karakterlánc, OldValue, új érték)    |                                                                          
| Bemenetek              | 1. **Karakterlánc.** Cserélje le a értékek használandó karakterlánc. <br/> 2. **OldValue.** Keresse meg, és cserélje le a karakterlánc. <br/> 3. Új érték. A cserélendő karakterláncot. |
| Üzemeltetés          | A karakterláncban OldValue összes előfordulását az új érték helyett. Lehet, hogy a funkció tudja kezelni a következő speciális karaktereket: <br/> - **\n.** Új sor. <br/> - **\r.** Kocsivissza. <br/> - **\t.** A lapon. <br/> Példa: ReplaceString ("One\n\rMicrosoft\n\r\Way", "\n\r","") "One Microsoft Way" adja vissza. |   
| Kimenet              | Egy karakterlánc összes előfordulásának **OldValue** cseréli a karakterláncban **új érték.**      |

## <a name="data-retrieval-functions"></a>Adatok lekérése funkciók

Adatok lekérése funkciók a kívánt karakterek le egy karakterlánc-műveletek végrehajtásához használják.

| Word       |        |
|--------------------|---------------|
| Leírás        | A Word függvény egy karakterlánc, és a word számát adja vissza az elválasztó karaktert leíró paraméterek alapján belül található szót adja vissza.                                                                |
| Függvényaláíráshoz | (Karakterlánc, szám, határolójelek) karakterlánc Word                                                                                                                                                                        |
| Bemenetek             | 1. **karakterlánc.** A karakterláncot, amelyikből szót. <br/> 2. **számát.** Egy szám, mely word azonosítószámmal vissza kell adni. <br/> 3. **határoló karakterek.** A határoló karakterek szavak azonosítására használt képviselő karakterláncot. |
| Üzemeltetés         | Minden egyes karakterlánchoz karakterlánc, amely el egy a határoló karakter vagy egy word azonosítja. A word, 3 (szám) paraméterben megadott pozíciónál található ad vissza: <br/> -Ha < 1-es számú, a üres karakterlánc. <br/> -Ha karakterlánc null, üres karakterláncot adja vissza. <br/><br/> Példák: <br/> 1. Word ("tesztelése; % függvény;", 3,"; $& %") "függvény". <br/> 2. Word ("tesztelése; Függvény", 2,";") adja vissza "" (üres). 3. Word ("tesztelése; % függvény;", 0,"; $& %") adja vissza ""(üres karakterlánc).
| Kimenet             | A word helyén tartalmazó karakterláncot a kér a felhasználótól. Ha **karakterlánc** nagyobb számú szó, vagy **karakterlánc** nem tartalmaz minden szó által azonosított **határoló karakterek,** üres karakterláncot ad vissza. |  


| Bal oldali               |   |
|-------|-------|
| Leírás        | A bal oldali függvény egy karakterlánc bal oldali egy megadott számú karaktert adja vissza.       |
| Függvényaláíráshoz | Karakterlánc bal (karakterlánc, numChars)     |
| Bemenetek             | 1. **karakterlánc.** A karakterláncot, amelyikből karaktereket. 2. **numChars.** Egy azonosító szám karakterek vissza egy karakterlánc elejéről.         |
| Üzemeltetés         | **numChars** karaktereket a rendszer adja vissza a karakterlánc első pozícióját. <br/> Példa: Left ("Britta Simon", 3) "Bri" adja vissza.   |
| Kimenet             | A karakterlánc első numChars karaktereket tartalmazó karakterlánc.  <br/> -Ha numChars = 0, térjen vissza az üres karakterlánc. <br/> -Ha numChars < 0, térjen vissza a bemeneti karakterlánc. <br/> -Ha karakterlánc null, üres karakterláncot adja vissza. |




| Jobb oldali       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Leírás | A jobb oldali függvény a megadott számú karaktert ad vissza egy karakterlánc jobb (záró).                                 |
| Függvényaláíráshoz   | Karakterlánc jobb (karakterlánc, numChars)   |
| Bemenetek      | 1. **Karakterlánc.** A karakterláncot, amelyikből karaktereket. <br/> 2. **numChars.** Egy azonosító szám karakterek vissza a karakterlánc végén.  |
| Üzemeltetés  | **numChars.** Karaktereket a rendszer karakterláncnak végét adja vissza. <br/> Példa: Jobbra ("Britta Simon", 3) "f" adja vissza.                  |
| Kimenet      | Egy karakterláncon belül az utolsó numChars karaktereket tartalmazó karakterláncot. Ha numChars = 0, térjen vissza az üres karakterlánc. <br/> -Ha **numChars** < 0, térjen vissza a bemeneti karakterlánc. <br/> -Ha a karakterlánc null értékű, térjen vissza az üres karakterlánc. <br/> -Ha a karakterlánc a száma a megadott numChars-nál kevesebb karaktert tartalmaz, karakterlánc megegyezik egy karakterláncot ad vissza. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Leírás | A közép függvény egy karakterlánc megadott helyéről adja vissza a megadott számú karaktert.                              |
| Függvényaláíráshoz    | Karakterlánc Mid(string, pos, numChars)                                                                                             |
| Bemenetek      | 1. **karakterlánc.** A karakterláncot, amelyikből karaktereket.   <br/> 2. **pos.** Egy szám, a kezdő helyzete, a karakterek visszaküldését karakterláncnak azonosítására. <br/> 3. **numChars.** Egy azonosító szám a karakterek számát, a karakterlánc egy pozíciójára visszaadására.  |
| Üzemeltetés  | Térjen vissza **numChars** pozíció karakterek **pos** a karakterláncban. <br/>Példa: Mid ("Britta Simon", 3, 5) "itta" alakítanák vissza. |
| Kimenet      | Egy karakterláncot tartalmazó **numChars** karakter pozíciója az **pos** egy karakterláncon belül: <br/> -Ha **numChars** = 0, térjen vissza az üres karakterlánc. <br/> -Ha **numChars** < 0, térjen vissza az üres karakterlánc. <br/> -Ha **pos** > karakterlánc hosszát adja vissza egy bemeneti karakterlánc. <br/> -Ha **pos** kisebb mint 0, térjen vissza a bemeneti karakterlánc. <br/> -Ha **karakterlánc** érték null, üres karakterláncot adja vissza. <br/> Ha nincs **numChar** karaktert tartalmaz, a fennmaradó **karakterlánc** helyről **pos**, mert a sok karakterből áll, adhatók vissza a rendszer visszairányítja.

## <a name="data-generation-functions"></a>Adatok generálása funkciók

Adatok generálása funkciók segítségével bizonyos értékek generálásához.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Leírás        | A CRLF hoz létre egy kocsivissza visszatérési/soremelés. Ez a funkció használatával adja hozzá egy új sort. |
| Függvényaláíráshoz | Karakterlánc CRLF                                                                              |
| Bemenetek             | Nincs paraméter                                                                            |
| Üzemeltetés         | A CR LF Karakterpár adja vissza.                                                                      |
|                    | Példa: AddressLine1 + CRLF() + AddressLine2 eredményezi: <br/> -AddressLine1 <br/> -AddressLine2 |
| Kimenet             | A CR LF Karakterpár egy, a kimenet.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Leírás        | A RandomNum függvény belül egy megadott időszakban egy véletlenszerű számot ad vissza.                                       |   
| Függvényaláíráshoz | Int RandomNum(start, end)                                                                                         |   
| Bemenetek             | - **Indítsa el**. Egy szám alacsonyabb korlátot a véletlenszerű érték létrehozásához.   <br/> - **end**. Egy azonosító szám a felső határ véletlenszerű érték létrehozásához.  |
| Üzemeltetés         | Nagyobb vagy egyenlő véletlenszerűen **start** és kisebb vagy egyenlő, mint **end** jön létre. <br/>  Példa: Random(0,999) visszaadhatja 100.                      |
| Kimenet             | Egy véletlenszerűen választott által meghatározott tartományon belül **start** és **end**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Leírás        | A *EscapeDNComponent* metódus dolgozza fel a bemeneti karakterlánc az éppen használatban lévő kezelőügynök alapján. |
| Függvényaláíráshoz | Karakterlánc EscapeDNComponent(string)                                                                                  |
| Bemenetek     | **karakterlánc**. Egy karakterlánc, amely segítségével dolgozza fel a megkülönböztető név. A karakterlánc nem lehet escape-karakterrel megjelölt karakter. |
| Üzemeltetés | A művelet elvégzéséhez a EscapeDNComponent metódusnak MIISUtils szolgál. Ez a módszer a bemeneti karakterlánc az éppen használatban lévő kezelőügynök alapján dolgozza fel. <br/> Mivel a különböző felügyeleti ügynökök különböző megkülönböztető név formátumok igényel, ez a módszer a bemeneti karakterlánc kezelőügynök típusa alapján dolgozza fel. A típusok a következők Lightweight Directory Access Protocol LDAP) megkülönböztető nevét, az ilyen asActive Directory® tartományi szolgáltatások, a Sun Directory Server (korábbi nevén a címtárkiszolgáló iPlanet), a Microsoft Exchange Server-kiszolgáló hierarchikus nonLDAP, például a Microsoft Lotus Notes; és külső, például az adatbázis és az XML-LDAP nélkül megkülönböztető nevét. <br/> ** LDAP megkülönböztető név: ** <br/> -A rendszer egy adott részének érték részében lévő bármely érvénytelen XML-karakter hexadecimális kódolású. <br/>-Az összes nem megengedett karaktert (beleértve az érvénytelen XML-karakter) a megadott része részét hibát adott vissza. <br/> -A következő karakterek vannak escape-karaktersorozatot: <br/> &nbsp;&nbsp;&nbsp;-Vesszővel (",") <br/> &nbsp;&nbsp;&nbsp;-Egyenlőségjel ("=") <br/> &nbsp;&nbsp;&nbsp;-Plusz jelre ("+") <br/> &nbsp;&nbsp;&nbsp;-Kevesebb-jel ("<") <br/> &nbsp;&nbsp;&nbsp;-Nagyobb-jel (">") <br/> &nbsp;&nbsp;&nbsp;-Kereszttel (#) <br/> &nbsp;&nbsp;&nbsp;-Pontosvessző (";") <br/> &nbsp;&nbsp;&nbsp;-Fordított perjel ('\') <br/> &nbsp;&nbsp;&nbsp;-Idézőjel ("" ") <br/> -Ha a karakterlánc az utolsó karakter adható meg, majd, hogy a hely escape-karakterrel megjelölve. <br/> – A felesleges kezdő vagy záró szóköz rész nevét a rendszer eltávolítja. <br/> -A az XML-kezelőügynök Ha több részből állnak majd összetevőkre van betűrendben. <br/> – Ha több részből vannak megadva, az összetett megkülönböztető név karakterlánca a kapott az egyes karakterláncok plusz jel elválasztva. <br/> -Hiba történik, ha a bemeneti karakterlánc nem megfelelően formázott, LDAP-megkülönböztető név karakterláncát. <br/><br/> **Hierarchikus nem LDAP** <br/> -A felügyeleti ügynökök nem támogat több részből álló összetevőt. Ha több karakterláncok EscapeDNComponent átadott, egy ArgumentException fordul elő. <br/> -Ha a karaktereket a bemeneti karakterlánc érvénytelen XML-karakter, egy ArgumentException vált ki. <br/> – Az összes vesszővel válassza el egymástól, és a bemeneti karakterlánc fordított megjelölni. <br/> -Ha a karakterlánc az utolsó karakter adható meg, majd, hogy a hely escape-karakterrel megjelölve. <br/><br/> **Külső:** <br/> 1. Ha valamely része bináris vagy egy érvénytelen XML-karaktert tartalmaz, a "#" karakterrel előzi meg a karakterlánc eleje része tárolja a nyers adatok hexadecimális kódolású verziója. Például ha egy része (ahol x egy érvénytelen XML-karakter, például a "0x10" jelenti) AxC volt, az a része kódolása "#410010004300". <br/> 2. Ellenkező esetben a következő karakterek egyikét sem példányainak vannak escape-karaktersorozatot: <br/> &nbsp;&nbsp;&nbsp;-Fordított perjel ('\') <br/> &nbsp;&nbsp;&nbsp;-Vesszővel (",") <br/> &nbsp;&nbsp;&nbsp;-Plusz jelre ("+") <br/> &nbsp;&nbsp;&nbsp;-Kereszttel (#) <br/> 3. Ha egy adott részét karakterlánc utolsó karaktere szóközzel, hogy a hely escape-karakterrel megjelölve. <br/> 4. Ha több részből meg van adva, az összetett megkülönböztető név karakterlánca a plusz jel elválasztott minden egyes karakterlánc kapott.
| Kimenet      | Egy érvényes tartománynevet tartalmazó karakterlánc.                                                                                                                  |   

>[!NOTE]
Megkülönböztetett nevekhez érvényesítése kevésbé szigorú a szintaxist, az LDAP-paramétereknek definiálva. EscapeDNComponent(String[]) lehetővé teszi, hogy egy tetszőleges kombinációját közül egy vagy több karaktert tartalmazó rész neve "a" – "z", "A" – "Z", "0"-"9", "-", és ".". <br/>
Nincs lehetőség a adja meg, ez a metódus a bináris részhez. Azonban lehetséges, hogy bináris részhez **CommitNewConnector** Ha horgonyzási attribútumok értékekből összeállított megkülönböztető nevét, és a horgony attribútumok egyikét a bináris típusú.


| NULL értékű        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Leírás | A Null függvény adható meg, hogy a MA nincs közre attribútum és, hogy attribútumsorrend folytatni kell-e a következő MA együtt használatos. |   
| Függvényaláíráshoz    | Karakterlánc null értékű    |
| Bemenetek      | Nincs paraméter                                                                                                                                             |   
| Üzemeltetés  | A rendszer NULL értéket ad vissza. <br/> Példa: IIF(Eq(domain), "Ismeretlen", az Null())                                                                                           |   
| Kimenet      | A rendszer NULL értéket egy kimenet.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Logikai funkciók
Logikai funkciók segítségével végrehajtania egy műveletet a rendszer által értékelt feltételek alapján.

| AZ IIF        |  |
|-------------|---|
| Leírás | Az IIF függvény a megadott feltétel alapján lehetséges értékek egyikét adja vissza.    |
| Függvényaláíráshoz   | Objektum IIF (feltétel, valueIfTrue, valueIfFalse)   |                                                 |
| Bemenetek      | 1. **Az állapot**. Bármely érték vagy kifejezés, amelynek kiértékelése igaz vagy hamis. 2. **valueIfTrue** eredményül, ha a feltétel igaz értéket. <br/> 3. **valueIfFalse** eredményül, ha a feltétel hamis értéket. <br/><br/> Az alábbiakban használható funkciók az IIF függvény, mint a kifejezésként **feltétel:** <br/> **EQ.** Ez a függvény a két argumentumot egyenlőség hasonlítja össze. <br/> **NotEquals.** Ez a függvény a két argumentumot egyenlőtlen ad vissza IGAZ, ha azok nem egyenlő és hamis Ha azonos hasonlítja össze.<br/> Példa: NotEquals (EmployeeType, "Alvállalkozó")<br/> **LessThan.** Ez a függvény a két számot ad vissza IGAZ, ha az első kisebb, mint a második és a hamis egyébként hasonlítja össze.<br/>Példa: LessThan (fizetés, 100000) <br/>**GreaterThan.** Ez a függvény a két számot ad vissza IGAZ, ha az első egyéb nagyobb, mint a második és a hamis hasonlítja össze.<br/> Példa: GreaterThan (fizetés, 100000) <br/> **LessThanOrEquals.** Ez a függvény a két számot ad vissza IGAZ, ha az első kisebb vagy egyenlő, mint a második és a hamis egyébként hasonlítja össze.<br/>Példa: LessThanOrEquals (fizetés, 100000) <br/> **GreaterThanOrEquals.* Ez a funkció összehasonlítja két számot ad vissza, ha az első érték nagyobb, mint vagy egyenlő a második és a hamis más módon igaz. <br/>Példa: GreaterThanOrEquals (fizetés, 100000)<br/> IsPresent. A funkció veszi, adja meg a ILM sémában attribútum, és IGAZ, ha az attribútum nem null értékű, és hamis értéket, ha az attribútum értéke null értéket ad vissza.|
| Üzemeltetés  | Ha **feltétel** kiértékelésének eredménye true, a **valueIfTrue.** Ellenkező esetben a visszatérési **valueIfFalse.** <br/>Példa: IIF (Eq (EmployeeType, "Internalizálási"), "t-" + Alias, Alias) ad vissza annak a felhasználónak az aliasneve "t-" Ha a felhasználó egy internalizálási azt elejéhez adja hozzá. Ellenkező esetben adja vissza, mert a felhasználói alias. |
| Kimenet      | A kimenet **valueIfTrue** a feltétel teljesülése esetén vagy **valueIfFalse** Ha a feltétel hamis. |      

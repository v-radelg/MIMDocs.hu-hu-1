---
title: "Erőforrás vezérlő megjelenítési konfigurációs XML-hivatkozás |} Microsoft Docs"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>Erőforrás vezérlő megjelenítési konfigurációs XML-hivatkozása

Erőforrás vezérlő megjelenítése (RCDC) konfigurációs erőforrások felhasználói erőforrásokat, amelyek segítségével szabályozhatja, hogyan jelennek meg a Microsoft Identity Manager 2016 SP1 (MIM) adattár más erőforrások a felhasználói felület (UI) a végfelhasználók. Minden egyes RCDC erőforrás hozzáadása, módosítása és eltávolítása a felhasználói felületen megjelenő szövegben és felhasználói felületi vezérlők módosítható egy XML-konfigurációs fájlt tartalmazza. A MIM 2016 SP1 biztosít több alapértelmezett RCDC erőforrásokat, miközben egyéni RCDC erőforrások egyéni erőforrások is létrehozhat. Az FIM portálon a RCDC felhasználói felület használatával kapcsolatos további információkért lásd: [konfigurálásáról és testreszabásáról a FIM-portál bemutatása](http://go.microsoft.com/fwlink/?LinkID=165848) a FIM-dokumentáció.


## <a name="known-issues"></a>Ismert problémák

Alapértelmezett érték sok konfiguráció az erőforrás-vezérlő megjelenítése vezérlők nem támogatott

Ebben a kiadásban alapértelmezett beállításértékek vezérlők egy erőforrás-vezérlő nem támogatott a gomb vezérlőelem kivételével. A legördülő listában a probléma megkerüléséhez alapértelmezett érték, amely nincs társítva a felhasználót a kiválasztott beállítás bármely érték megadásával. A vezérlők más beállítása a probléma megoldása érdekében kell használnia egy engedélyezési munkafolyamat adja meg az alapértelmezett érték a kérelem elküldése során.

## <a name="basic-structure"></a>Alapvető szerkezet

Az XML-adatok RCDC erőforrás áll egyetlen XML-elem **ObjectControlConfiguration.**

>[!NOTE]
A teljes XSD-séma Lásd: a függelék: megfigyelendő alapértelmezett XSD-séma a dokumentum későbbi szakaszában.

A következő egy az XSD-séma ObjectControlConfiguration elemhez:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



A **ObjectControlConfiguration** elem tartalmazza-e a következő:

1.  **ObjectDataSource**: ennek az elemnek egy adatforrás osztály, amely a erőforrás vezérlő (RC) használ a TypeName határozza meg. A leírást és a sémadefiníciót lásd a következő adatok források ebben a dokumentumban. Egy **ObjectControlConfiguration** elem legfeljebb 32 csomópontot a tartalmazhat a **ObjectDataSource** elemet.

2.  **XmlDataSource**: Ez az összesítő oldal meghatározásához leggyakrabban használt egyszerű adatforrás. A leírást és a sémadefiníciót lásd a következő adatok források ebben a dokumentumban. Egy **ObjectControlConfiguration**: elem legfeljebb 32 csomópontot a tartalmazhat a **XmlDataSource** elemet.

3.  **Panel**: rendszergazda testre szabhatja a RCDC lap elrendezését, a Vezérlőpult elemei elemek módosításával. További információ az című Panel a dokumentum későbbi szakaszában. Egy **ObjectControlConfiguration** elemhez csak egy Panel elemet kell.

4.  **Események**: a rendszergazdák nem adhatók meg a testreszabott háttérkódot, ez a funkció korlátozva. Ez az esemény, amely egy panel, vagy egy vezérlő el tudná küldeni az állapotváltozás alapján. További információkért lásd: események a dokumentum későbbi szakaszában. Egy **ObjectControlConfiguration** elem tartalmazhat opcionálisan egy **esemény** elemet. Általánosságban az egyéni használatát **események** nem érhető el, kivéve, ha újabb fejlesztések kifejezetten létre.

## <a name="data-sources"></a>Adatforrások

A Microsoft Identity Manager adatforrások adatok kötődni felhasználói felületi összetevőket is használ. Ezzel a megoldással az adatok elkülönítését a bemutató rétegből megkönnyítése. A RCDC erőforrás konfigurációs adatokat az adatforrások két fő típusba sorolhatók: **ObjectDataSource** és **XmlDataSource**.

-   **ObjectDataSources** adja meg az adatokat az RC biztosít a Microsoft .NET-osztályt. Nincs elérhető naplófájltípusokat ObjectDataSources készletét feltéve, hogy a rendszergazda megadhatja RCDCs készítésekor felhasználását.

-   **XMLDataSources** egyszerűen struktúra XML-alapú adatokat, és a felhasználásuk rendszergazdái testreszabott adatok. Az XML-adatok közvetlenül a RCDC a meg kell adni, kivéve ha a beépített, előre definiált XML-struktúra. A beépített XML-struktúra generál összegző lapokon az RC szolgál.

A RCDC a köthető ezeknek az adatforrásoknak a felhasználói felületi vezérlők létrehozni a felhasználói felület RCDC megadott attribútumait.

### <a name="objectdatasource"></a>ObjectDataSource adatforrás

A Microsoft Identity manager nyújt a gyakori adatforrásokat a következő táblázatban (kivéve, ha az áttelepítés előtt feljegyzett) minden erőforrástípus esetén elérhetők.

| A TypeName értéke                        | Leírás     | A kétutas kötés támogatja | Támogatott kötés szintaxis        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | A FIM 2010 erőforrás létrehozott, módosított vagy megtekintett jelképez. A kötési karakterláncban elérési út az attribútum nevét. Vegye figyelembe, hogy az erőforrás típusa a RCDC nem a RCDC TargetObjectType attribútum van megadva. ConfigurationData attribútum. | Igen                     | [AttributeName] A név által megadott objektum attribútum értéke.    |
| PrimaryResourceDeltaDataSource  | Ez az adatforrás épít fel a különbözeti XML, amely összehasonlítja az eredeti állapotra és a FIM 2010 erőforrás aktuális állapotát. A létrehozott különbözeti XML fel a kérelmet átadó a felhasználó felhasználói felülete megjelenítéséhez RC összefoglaló vezérlő.                                    | Nem                      | DeltaXml: </br> Ez az összesítő vezérléssel megjelenítésére szolgál a különbözeti.                                                 |
| PrimaryResourceRightsDataSource | Ezt az adatforrást a beágyazott jogosultságokat biztosít összes attribútumot a FIM 2010 erőforrás. Ez lehetővé teszi az RC jogosultságokat az adott felhasználó rendelkezik-e ez az attribútum a elküldése előtt állapítsa meg, és ezután leképezési megfelelően ezt az attribútumot a felhasználói felülete.                     | Nem                      | [AttributeName]                                                                                         |
| SchemaDataSource                | Ez az adatforrás séma-kapcsolatos információkat, például a megjelenített név, leírás, szükség-e a attribútumra, valamint erőforrástípus információk eléréséhez használható.                                                                                             | Nem                      | [AttributeName]. **Szükséges** logikai érték, amely jelzi, hogy az attribútum érvényes értékkel kell rendelkeznie. <br/> [AttributeName]. **DisplayNameString** érték, amely a kötés megjelenített neve <br/>[AttributeName]. **DescriptionString** érték, amely a kötés leírása <br/>[AttributeName]. StringRegexString érték, amely a kötés karakterlánc Regex jelzi. <br/>[AttributeName]. **DisplayName** <br/> [AttributeName]. **Leírása** <br/> [AttributeName]. [AttributeName]. **IntergerValueMinimum** <br/>[AttributeName]. **IntergerValueMaximum** <br/>[AttributeName]. **LocalizedAllowedValues**|
| DomainDataSource                | Ez az adatforrás-tartománynak, a tartomány konfigurációs erőforrások alapján felsorolásoknak biztosít. Vegye figyelembe, hogy használható-e az adatforrás csak a csoport felhasználói erőforrásokat és a RCDCs.                                                                           | Igen                     | Domain           |

Az alábbiakban látható egy példa RCDC kódrészletet, amely három adatforrások kötve van a UocTextBox vezérlő egy csoport leírása attribútumának szerkesztése:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

Használatával egy **XMLDataSource**, megadhatja, hogy a RCDC felhasználhat egy adott erőforrás egyéni adatokat. Ebben az esetben az XML-adatok kell megadni a RCDC. Alternatív megoldásként ehhez az adatforráshoz való hivatkozáshoz egy beépített XML-adatok struktúra összefoglalás lapok a felhasználói felület megjelenítéséhez használható. Szabályozhatja, hogy milyen típusú **XMLDataSource** a RCDC a definiálásakor használatára.


| A TypeName értéke                 | Leírás   | | |
|--------------------------|------------|
| **XMLDataSource**            | Az adatforrás XML-adatok jelöli. Azok az XSL vagy beágyazott XSL formátumok: <br/>**XSL-formátum:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll < saját: XmlDataSource saját: Name = " <br/>summaryTransformXsl"my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl">< / a: XmlDataSource ><br/> **Beágyazott XSL-formátum:** <br/> < a: XmlDataSource saját: Name = "RequestStatusTransformXsl" > <br/> < xsl: stylesheet verzió = "1.0" xmlns:xsl = http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl = "urn: schemas-microsoft-com:xslt" ><br/>< / xsl: stylesheet >< / a: XmlDataSource >                       |Nem | ```Xpath[;namespaces]``` <br/> Hol: Xpath-hoz egy érvényes XML-xpath válassza ki a szükséges, gyakran "/" (gyökér) <br/>Névterek egy-egy választható lista előtag URI karakterláncok, ha az xpath elleni namespaced XML működéséhez szükséges a pontosvesszővel elválasztott =. |
| **ReferenceDeltaDataSource** | Az adatforrás attribútumok a többértékű hivatkozás eltérések jelöli. Csoport és a beállított csak a RCDC szolgál. <br/> Bár az adatforrás nincs korlátozva, csoportok vagy beállítása, a RCDC fogadó elküldeni az ilyen eltéréseit a kód módosítására van szükség. Csoport és a készlet jelenleg csak gazdagépből ismeri fel az adatforrást.  | Igen                      | [AttributeName]. Visszaadott adatok hozzáadása, ahol [AttributeName] jelenti-e a hivatkozási attribútum és a különbözeti kiegészítéseket. <br/> Példa: [ReferenceAttribute]. Hozzáadása <br/>Például: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName]. Távolítsa el, ahol [AttributeName] jelenti-e a hivatkozási attribútum, a visszaadott adatok pedig a különbözeti eltávolítására. <br/> DeltaXml |
|**RequestDetailsDataSource**| Az adatforrás kérelemobjektumok RequestParameter attribútumának jelöli. A paraméter állandóként állítja be, a megjelenített többértékű attribútum szerepel, csak RCDC kérelem attribútumértékek maximális számát. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| Nem | DeltaXml |
|**RequestStatusDataSource**| Az adatforrás-jelöli a **RequestStatusDetails** kérelemobjektumok attribútuma. <br/>A kérelem csak a RCDC szolgál.  | Nem | DeltaXml |

-   Egyéni XML-adatforrás meghatározása:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   A beépített összefoglaló vezérlő XSL használatához adja meg az adatforrás az alábbiak szerint:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Egy egyéni erőforrástípus RCDC hoz létre, ha ez a módszer segítségével jeleníti meg az adott egyéni erőforrás egy összegző lap automatikusan.

A következő egy példa bemutatja, hogyan a RCDC a PrimaryResourceDeltaDataSource használata a XMLDataSource a beépített XSL használatával az összefoglaló lapon létrehozásához:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Alternatív megoldásként a felhasználó cserélheti a korábban megadott, az alábbi formátumban megadhatók egy testreszabott elrendezéshez összefoglaló lap XmlDataSource elem. Referenciaként a FIM 2010 összegzés XSL alapértelmezett függelék b alapértelmezett összegzés XSL, szerepel a dokumentum későbbi szakaszában.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Adatforrások séma
A következő egy az XSD-séma, a két típusú adatforrások:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>Események
Egy esemény egy vezérlő változó állapotát határozza meg. A szolgáltatás a bővíthetőség korlátozva, mert egy testreszabott függvény (kezelő) nem írható a viselkedés meghatározása után az esemény akkor váltódik ki. A Panel elem esemény elem használható. További információ az című Panel a dokumentum későbbi szakaszában. A következő egy az XSD-séma, esemény elemhez:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Egy esemény üres elem, és nem rendelkezik a két attribútum.

**Attribútumok:**

1.  **Név**: Ez az esemény egyedi nevét. Az egyetlen támogatott esemény a **ObjectControlConfiguration** a betöltési esemény. Az esemény akkor váltódik ki, ha az oldal első be van töltve.

2.  **Kezelő**: leíró egyedi neve. Az esemény akkor váltódik ki, általában egy program metódus neve a vezérlő állapot módosításának kezelése. a következő esetekben nem támogatottak: egy meglévő kezelő eltávolítása a meglévő vezérlőt, egy új kezelő létrehozása és csatolása meglévő vagy új vezérlőre.

Példa:

Az események elemek például a következő:
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panel**


A Panel elem egy RCDC elrendezés core elemében. A következő az XSD-séma a Panel elem:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

Ez az elem egy ismétlődő elemet tartalmaz, csoportosítása. További információkért lásd: a jelen dokumentum a csoportosítás szakaszban.

A Panel elem négy attribútumokat tartalmaz:

1.  **Név**: a Panel neve. Ez az egy szükséges, karakterlánc típusú attribútum.

2.  **DisplayAsWizard**: Ez az attribútum jelenleg elavult. A megfelelő VerbContext attribútumot a RCDC irányító varázsló vagy a lapon üzemmódban erőforrás elrendezés esetén. Ha az értéke 0 (létrehozási módban), azt is varázsló módban van. Ellenkező esetben üzemmódban van fülre. További információkért lásd: konfigurálásáról és testreszabásáról a FIM-portál a dokumentáció bemutatása.

3.  **Felirat**: Ez az attribútum jelenleg elavult. A felhasználó megadhatja egy lap feliratok tartalmazó csoportok csak a fejléc-információ-ot. További információkért lásd: a jelen dokumentum a csoportosítás szakaszban.

4.  **AutoValidate**: Ez az opcionális logikai attribútum. Ha értéke igaz, érvényesítése a akkor váltódik ki, az aktuális lapon minden vezérlő ellen. Alapértelmezés szerint ha az attribútum nincs megadva, az értéke igaz. A reguláris kifejezés a tulajdonság együtt használható. További információkért lásd: "Reguláris kifejezés." a jelen dokumentum későbbi szakaszában.

## <a name="grouping"></a>Csoportosítás

A csoportosítás elem definiálja a Panel általános elrendezésének. Olyan tároló, amely különböző szakaszokra és lapok összevonja a egyéni vezérlők funkcionál. A következő egy az XSD-séma csoportosítási elemhez:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Három típusú **csoportosításokat**:

1.  **Fejléc csoportosítási**: A fejlécben csoportosítás nem kötelező megadni. Csak lehet egy fejléc csoportosítása a **Panel**. A fejléc csoportosításhoz felirat fölött egy panel jelenik meg.
    Csak egy UocCaptionControl használható ennél a csoportosításnál engedélyezett. Például egy fejléc csoportosítása a minta című rész.

2.  **Tartalom csoportosításokat**: legalább egy tartalom csoportosítását szükség. A Panel több tartalom csoportosításokat lehet. A tartalom csoportosításhoz fő tartalmát egy RCDC lap jelenik meg. Minden tartalom csoportosítását egy fülre az azonos panelen jelenik meg, és 1 256 vezérlőkhöz tárolására képes. Című rész a minta alábbi példa egy **tartalom csoportosítását**.

3.  **Összegző csoportosításokat**: A összefoglalás csoportosítás nem kötelező megadni. Csak lehet egy Összegzés csoportosítási panelen. Összefoglalás csoportosítása a panel utolsó lap jelenik meg. Csak egy **UocHtmlSummary** vezérlő használható összegzés csoportosítása a felhasználó rendelkezik a kérés elküldése előtt végrehajtott módosítások megjelenítéséhez. Összegzés csoportosítása példát alább minta szakaszban talál.

Minden egyes csoportosítás típusa a következő elemeket tartalmazza:

1.  **Súgó**: ennek az elemnek egy lapon súgó szövegét. A súgófájl a lapra mutató hivatkozás hozzáadása is használja.

2.  **Vezérlők**: Ezzel az elemmel kapcsolatos információkért tekintse meg az ellenőrző szakasz ebben a dokumentumban. Minden csoport rendelkeznie kell 1 és 256 vezérlők szélsőértékeket is beleértve, a csoportosítás típusától függően.

3.  **Események**: Ezzel az elemmel kapcsolatos információkért lásd: az események szakasz ebben a dokumentumban. Minden csoport lehetőségként rendelkezhet egy eseményt. Az események csoportosítása elemben támogatott a következők:

    - **BeforeLeave**: az esemény akkor váltódik ki, amikor a felhasználó egy lapon hagyja a tartalom csoportosítását készen áll.
    - **AfterEnter**: az esemény akkor váltódik ki, amikor a felhasználó a lapon írja be a tartalom csoportosítását készen áll.

Attribútumok:

1.  **Név**: a csoportosítás kötelező neve. A **neve** belül egyedieknek kell lenniük a **Panel**.

2.  **Felirat**: A **felirat** fejléc csoportosítása a fejléc felirat jelenik meg. Úgy tűnik, a tartalom vagy az összefoglalás lapon feliratának csoportosítási.

3.  **Leírás**: egy nem kötelező karakterlánc attribútum **leírás** működőképességét csak használat esetén a tartalom csoportosítását. Ez az elem segítségével biztosíthat a végfelhasználó néhány részletet a adatokat ugyanazon a lapon belül.

  >[!NOTE]
  Ha ez az attribútum összegzés csoportosítása használja, az XML-fájl érvénytelen tekinthető. Ezt az attribútumot használja a fejléc csoportosítása, ha az XML-fájl érvényes, de figyelmen kívül hagyott tekinthető meg.

4.  **Engedélyezve**: egy nem kötelező logikai attribútum, állítsa be true értékre, ha a hiányzó engedélyezve. Engedélyezve értéke HAMIS, ha a végfelhasználó látja a le van tiltva a lapon. Ez az attribútum csak a tartalom csoportosítását működőképességét.

  >[!NOTE]
  Ha ez az attribútum összegzés csoportosítása használja, az XML-fájl érvénytelen tekinthető. Ezt az attribútumot használja a fejléc csoportosítása, ha az XML-fájl érvényes, de figyelmen kívül hagyott tekinthető meg.

5.  **Látható**: elrejtheti a egy RCDC lap lap vagy a fejlécet, ha az attribútum értéke false. Alapértelmezés szerint ez az opcionális, logikai érték típusú attribútumnak az értéke igaz. Ez az attribútum csak a tartalom csoportosítását működőképességét.

  >[!NOTE]
  Ha a Panel csak egy tartalom csoportosítását, ez a szolgáltatás nem működik. Ha a Panel egynél több tartalom csoportosítását, úgy viselkedik a fent leírt módon.

6.  **IsHeader**: ennek az attribútumnak egy nem kötelező, logikai attribútum, amely meghatározza, hogy-e a csoportosítási fejléc csoportosítása. Ha ez az attribútum nincs megadva, az hamis értékre van beállítva.

7.  **IsSummary**: Ez az opcionális, logikai attribútum, amely meghatározza, hogy-e a csoportosítás összefoglaló csoportosítása. Ha ez az attribútum nincs megadva, az hamis értékre van beállítva.

![Számlázott konfigurációs XML-kód](media/rcd-configuration-xml-reference/image005.jpg)

A következő XML-minta kódot állít elő, a korábbi fejléc csoportosítását. A fejléc csoportosítás akkor szöveget a minta fejléc csoportosítási területen.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![Számlázott konfigurációs XML-kód](media\rcd-configuration-xml-reference/image007.jpg)

A következő XML-minta kódot állít elő, a korábbi tartalom csoportosítását. A tartalom csoportosítását használható a bal szélső lap szöveget **minta tartalom csoportosítási**.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![Számlázott konfigurációs XML-kód](media/rcd-configuration-xml-reference/image010.jpg)

A következő XML-minta kódot állít elő, az előző összegzés csoportosítása. Az összefoglalás a csoportosítás akkor szöveget a jobb oldali lapon **összegzés**.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>Súgó

A Súgó elem lehetőségként tartalmazhat egyesülés vagy egy vezérlő elemet. Csoportosítása használatban van, ha a használt első elemének kell lennie. A végfelhasználók számára, hogy segítséget nyújthatnak a pontos információkkal szöveges segítségével biztosít. A következő egy az XSD-séma súgó elemhez:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
A következő egy példa a Súgó elem:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Szabályozás

A csoportosítás elem egy vagy több vezérlő elemet tartalmaz. Vezérlők, amelyek egy RCDC fő elemei. Testre szabhatja a csoportosítási elem meghatározása a különböző vezérlő elemeket tartalmaz. A következő egy Control elemhez az XSD-séma:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

A vezérlő a következő elemeket tartalmazza:

1.  **Súgó**: ennek az elemnek a rendszer figyelmen kívül hagyja. Csak a csoportosító működik.

2.  **Egyéni tulajdonságok**: Ez az elem nem támogatott.

3.  **Beállítások**: Ez az elem csak együtt használják a **UocDropDownList** vagy **UocRadioButtonList** szabályozza. Nincs más vezérlőkkel működési. Ez a dokumentum ezen elemének struktúra beállítások szakaszában talál. Tekintse meg a vezérlőt egy vezérlő környezetében használatának módját.

4.  **Gombok**: Ez az elem csak együtt használják a **UocListView** vezérlő. Nincs más vezérlők a működési. További információkért lásd: a UocListView szakasz ebben a dokumentumban.

5.  Tulajdonságok: Ez az elem az összes vezérlő megadására szolgál a vezérlő további eszközöket. Ezzel az elemmel kapcsolatos információkért lásd: a Tulajdonságok szakaszának ebben a dokumentumban.

6.  **Események**: Ez az elem szerkezete című rész az események a jelen dokumentum korábbi. Tekintse meg az egyéni vezérlő definíciós annak meghatározásához, hogy melyik esemény szolgál.

A vezérlő tartalmaz a következő attribútumokat:

1.  **Név**: a vezérlő neve. Vezérlő neve minden panel belül egyedinek kell lennie. Ez az egy szükséges, karakterlánc típusú attribútum.

2.  **A TypeName értéke**: Ez az attribútum azt határozza meg, milyen típusú vezérlő. Ez az egy szükséges, karakterlánc típusú attribútum. Ez a dokumentum minden vezérlő név egyes vezérlők szakaszában talál.

3.  **Felirat**: Ez az attribútum segítségével a vezérlő feliratát tartalmazza.
    A felirat az általában az adatokat, hogy a vezérlő megjelenítése vagy beírása megjelenítendő nevét. Explicit módon adja meg a fejléc értékét, vagy séma attribútum megjelenítendő neve információkkal a kötést. A felirat jelenik meg a normál méretű vezérlőelem bal oldalán. Ha egy vezérlő átfedés van a teljes képernyős a felirat jelenik meg a vezérlő fölött. Ez az opcionális, karakterlánc típusú attribútum. Attribútum- vagy tulajdonságérték-azonosítójú adatforrás kötési kapcsolatos információkért lásd: a Tulajdonságok szakaszának.

   A következő példa bemutatja, hogyan felirat explicit módon használható:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     Az alábbi példa megjelenítése felirat hogyan használható az adatforrás. Ha a sablon az a jelen dokumentum korábbi látható adatforrás használt, az adatokat az adatforrásból séma. Azt javasoljuk, hogy a felirat attribútummal a attribútum DisplayName kötni.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Engedélyezve van: Ez az opcionális, logikai érték-type attribútum. Az attribútum értéke false értékre állításával a felhasználó letilthatja a vezérlő. Az alapértelmezett beállítás értéke igaz.

5.  Látható: Ez az opcionális, logikai érték-type attribútum. Ez az attribútum segítségével a teljes vezérlés elrejtése. Az alapértelmezett beállítás értéke igaz.

6.  Leírás: A nem kötelező, karakterlánc típusú attribútum segítségével a leírása, a végfelhasználó megértése, mi ezek kell helyezni a vezérlőben, vagy a vezérlő funkciója segítségével. Adjon meg egy értéket, a leírás explicit módon, vagy a séma attribútum leírása információval a kötést. <br/>A szöveg egy normál méretű vezérlőelemet a felirat alatti bal oldalán megjelenik. Ha egy vezérlő átfedés van a teljes képernyőt, a leírás megjelenik a felirat alatt a vezérlő tetején. Attribútum, vagy egy tulajdonságérték-azonosítójú adatforrás kötési kapcsolatos információkért lásd: a Tulajdonságok szakaszának ebben a dokumentumban.

7.  A következő példa bemutatja, hogyan leírását explicit módon használható:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  Ez a példa bemutatja, hogyan adatforrás leírását használható. Ha a sablon az a jelen dokumentum korábbi látható adatforrás használt, az adatforrás van-e **séma**. Azt javasoljuk, hogy köthető-e az attribútum **leírás** leírás attribútummal.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: Ez az attribútum jelzi, hogy a vezérlő is a teljes képernyőt. Ez az opcionális, logikai érték-type attribútum. Az alapértelmezett beállítás hamis értékre.

    >[!NOTE]
    A felirat és leírás attribútumok le vannak tiltva, ha ez az attribútum értéke igaz. A UocLabel vezérlő felirat bővített vezérlő így kell használnia.
9. **Mutató**: Ez az opcionális, karakterlánc típusú attribútum. A szöveget a Hierarchiamutató attribútum segítségével a végfelhasználó, döntse el, mi az, hogy a vezérlő érvényes adatot. A mutatót a vezérlő alatt jelenik meg.

10.  **Teheti lehetővé**: Ez az opcionális, logikai érték-type attribútum. Az alapértelmezett értéke hamis. Ha értéke HAMIS, a lap frissítésével nem frissülnek a vezérlő. Információ teheti lehetővé keresse meg a Microsoft ASP.NET UI vezérlő tulajdonságának azonos nevű.

11. **RightsLevel**: Ez az opcionális, karakterlánc típusú attribútum. Ez az attribútum csak beágyazott jogosultságokkal rendelkező adatforrás köthető. A vezérlő dinamikusan engedélyezve van vagy le van tiltva, a felhasználói jogok alapján. Köthető adatforrások attribútum vagy tulajdonság értéke kapcsolatos információkért lásd: a Tulajdonságok szakaszának ebben a dokumentumban.

    A példa bemutatja, hogyan egy **RightsLevel** adatforrás attribútum használható. Ha a sablon az a jelen dokumentum korábbi látható adatforrás használt, az adatforrás van-e **jogok**. Az elérési utat használja a attribútum nevét.

### <a name="properties"></a>Tulajdonságok

A tulajdonság segítségével még jobban testre szabhatja az egyes vezérlő viselkedését. A tulajdonság üres elemben. A következő egy az XSD-séma a tulajdonság elemhez:
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



Minden tulajdonsághoz két szükséges attribútumokkal rendelkezik:

1.  **Név**: A karakterlánc típusú attribútum a tulajdonság egyedi nevét.
    Eltérő vezérlők más tulajdonságokkal rendelkezik. Nincsenek minden vezérlő által használható bizonyos általános tulajdonságokat. Milyen neveket érhetők el egy adott vezérlő kapcsolatos további információkért tekintse meg az általános tulajdonságok és egyéni vezérlők szakasz.

2.  **Érték**: Ez a tulajdonság értéke. Az érték mutatása tulajdonsággal megállapíthatja, melyik hozzá van rendelve adattípusát. A következő részben a tulajdonságokat a megengedett érték formátuma.

Néhány tulajdonság adatforrásból származó információkat is köthető. Ehhez kell használnia a következő karakterlánc-formátum. Tekintse meg az egyéni vezérlők szakaszban megtudhatja, hogyan lehet kötést létrehozni őket egy adatforrással egyes tulajdonságokat.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**PÉLDA:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Általános tulajdonságai
-----------------

Ebben a dokumentumban megadott RCDC vezérlők is a következő közös jellemzőkkel rendelkezik. Ezeket a tulajdonságokat, más eltérő vezérlők jellemző tulajdonságok együtt használható.

1.  Kötelező: Ez a tulajdonság azt jelzi, hogy a mező kötelező vagy választható mező. Kötelező megadni értéket kötelező megadni. Üres értéket nem támogatja a bemeneti karakterlánc esetében. Választható mező is üresen kell hagyni. Ha ezt a mezőt kötelező kitölteni érték nélkül, a hibaüzenet jelenik meg a bemeneti vezérlő fölött. Ön kifejezetten megad, hogy egy mező kötelező vagy választható-e. A mező a sémaadatok, az olyan attribútumnál, illetve erőforrástípus adott köt is köthető. Alapértelmezés szerint ez a tulajdonság hiányzik, ha azt jelenti, hogy a vezérlő egy nem kötelező bemeneti vezérlő.

   A következő egy példa, amely egy konkrét értéket használja ehhez a tulajdonsághoz:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Itt látható egy példa, amely egy dinamikus adatforrást használ ehhez a tulajdonsághoz. Ha a sablon az a jelen dokumentum az előző részben található adatforrás használt, az adatokat az adatforrásból séma. Használjon \<attribútumnév\>. Az elérési út szükséges.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **Csak olvasható**: ezt a tulajdonságot true értékre állításával a végfelhasználói élmény a vezérlő csak olvasható módban. Ez az opcionális, logikai érték-type attribútum.
    Az alapértelmezett beállítás hamis értékre. Azonban néha viselkedését ez a tulajdonság felülírja az adott személy rendelkezik köti az adatokat a vezérlőhöz jogok típusát. Például ha a felhasználó nem jogosult a mezőt, és a mező kötve van soron belüli jogosultságokkal, a felhasználó láthatja az adatokat egy csak olvasható módban még akkor is, ez a tulajdonság hamis értékre van beállítva.

3.  **Reguláris kifejezés**: Ez a tulajdonság határozza meg a megadott korlátozások, a vezérlő értékét. A formátumok e tulajdonság értékének a szabványos .NET StringRegex által támogatott formátumok. További információkért lásd: [.NET-keretrendszer reguláris kifejezések](http://go.microsoft.com/fwlink/?LinkId=165361). A vezérlő segítségével adjon meg egy értéket, ha az értéket veti az ebben a tulajdonságban megadott korlátozás amikor a felhasználó atempts, hogy az aktuális oldalon.
    A hibaüzenet jelenik meg, amely rendelkezik a megadott érvénytelen vezérlő fölött. A felhasználónak explicit módon adhatja meg a karakterlánc reguláris kifejezést. A felhasználó is kódkifejezés egy adott attribútum séma adatokkal. Alapértelmezés szerint ez a tulajdonság hiányzik, ha az azt jelenti, hogy a vezérlő nem ellenőrzi a bemeneti karakterlánc korlátozásait.
    A következő egy példa, amely egy konkrét értéket használja ehhez a tulajdonsághoz:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Itt látható egy példa, amely egy dinamikus adatforrást használ ehhez a tulajdonsághoz. Ha a sablon az ebben a dokumentumban leírt korábban adatforrás használt, az adatokat az adatforrásból séma. Használja a <attribute name>. A elérési útjaként StringRegex.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Látható: Ez az opcionális, logikai érték-type attribútum. Ez az attribútum segítségével a teljes vezérlés elrejtése. Az alapértelmezett beállítás értéke igaz.

### <a name="options"></a>Lehetőségek

A beállítások elem egy vagy több lehetőséget alsóbb szintű tartalmazza. A beállítások elem csak a UocRadioButtonList és UocDropDownList vezérlők használatos. Részletes információt is használhatja őket az egyéni vezérlő című rész. A következő beállítások elemhez az XSD-séma:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


Attribútumok:

1.  Érték: Karakterlánc típusú kötelező attribútum értéke. A value attribútumként ugyanarra a vezérlőre belül egyedinek kell lennie. Csak A-Z, azonban nem karakterek is használhatók.

2.  Leírás: Ez szükséges attribútuma minden egyes beállítás megjelenített neve.

3.  Emlékeztető: Ez a nem kötelező attribútum. Ez az attribútum használatával további információk és javaslatok biztosíthat a végfelhasználók.

### <a name="environment-variables"></a>Környezeti változók

A következő táblázatban levő környezeti változók használható RCDC beállításra.

| változó | Leírás |
|--------|--------|
| `<LoginID>`       | Megjeleníti a jelenleg bejelentkezett felhasználó Azonosítóját.           |
| `<LoginDomain>`   | Megjeleníti a tartományt, a felhasználó jelenleg be van jelentkezve.       |
| `<Today>  `       | Megjeleníti az aktuális dátum és idő                                |
| `<FromToday_nnn>` | Az aktuális dátum plusz nnn és időpontját jeleníti meg. nnn egy egész számot.  |
| `<ObjectID> `     | A RCDC elsődleges erőforrás-azonosító.                                     |
| `<Attribute_xxx>` | Egy adott attribútummal, xxx a RCDC elsődleges erőforrás adja vissza. |

### <a name="debugging-xml-configuration-files"></a>XML-konfigurációs fájlok hibakeresés


Ha fejleszt, vagy egy RCDC az XML konfigurációs fájl módosítása, csökkentheti az hibák ellen XSD-fájlok, például Microsoft Visual Studio® egy szerkesztővel XML érvényesítésével. További információkért lásd: [Bevezetés az XML-eszközök a Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512).

### <a name="customizing-a-help-file"></a>A súgófájl testreszabása

Ha létrehozta az új erőforrásokat és attribútumokat, előfordulhat, hogy frissíteni kívánt a meglévő súgófájlokat az FIM portálon testreszabott erőforrások tartalmú. Az FIM portálon súgófájlok formátumú, és azokat manuálisan kell módosítani.

>[!IMPORTANT]
Az egyéni attribútumok létrehozásával kapcsolatos további információkért tekintse meg a FIM 2010 dokumentációjának megismerkedhet az egyéni erőforrás és kezelése.

>[!IMPORTANT]
Ez a szakasz nem közli a alapokkal kapcsolatos formázás vagy HTML szerkesztése. Módosíthatja a Súgó fájlokat már ismernie kell a HTML szerkesztése


**A súgófájlok helyének**: a Microsoft Identity Management 2016 SP1 portálon minden súgófájl a MIM szolgáltatás kiszolgálójának következő mappájában található:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Hogyan keresse meg a megfelelő súgófájl**: az FIM portál minden súgófájl megnevezett globálisan egyedi azonosítóval (GUID). Keresse meg a megfelelő fájlt az egyéni erőforrás:

1.  Az FIM portálon a súgó megnyitásához, amely a testreszabni kívánt Portal oldalon.

2.  Kattintson a jobb gombbal a súgófájl, és kattintson **tulajdonságok**.

3.  Jelölje ki és másolja a `<GUID\>.htm` fájlt a "URL-címet" mezőben.

4.  Nyissa meg a Súgó fájlokat tároló mappa, és keresse meg a fájlt.

**Egy új attribútum tartalom-Hozzáadás**: leíró tartalom olyan meglévő csoportosítási elemben (lap) egy új attribútum hozzáadásához:

1.  Határozza meg, és keresse meg a megfelelő súgófájlt.

2.  HTML-szerkesztőt, nyissa meg a fájlt.

3.  Keresse meg, amelyen a tartalmat hozzáadni kívánt. Ez általában egy további bekezdésben szereplő esetekben, például:

`<p xmlns="">A new paragraph with customized information.</p>`

Egy elem egy meglévő listájához, például beszúrt is lehet:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Tartalom csoportosítási új elem hozzáadása**: A FIM-portál lapok többsége rendelkezik több csoportosítási elemet (vagy lapok) és a hozzá tartozó Súgó fájlok könyvjelzővel rendelkezik részek, amely minden csoportosítási elem vonatkoznak. A szakaszok a könyvjelzők a HTML-formátumban vannak megadva. Például ez az a felhasználó létrehozása oldal az FIM portálon a súgófájl a a munkahelyi adatai lap HTML:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

A csoportosítás elem által hivatkozott **WorkInfo** a konfigurációs adatok XML-fájljának a **felhasználó létrehozásához szükséges konfigurációs** RCDC. Vegye figyelembe, hogy a `\<GUID\>.htm` fájl neve és a könyvjelző meg van adva a saját: hivatkozás paraméter:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Egyszerű ellenőrző minták**

Az alábbi ábra megmutatja, különböző egyszerű szöveg-vezérlők különböző módokban.

Például:

![](media/rcd-configuration-xml-reference/image016.gif)

A következő kódszegmens explicit használja az összes attribútumok és tulajdonságok első szövegdoboz vezérlő hoz létre:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


A következő kódszegmens hoz létre a második szövegdoboz vezérlő, dinamikus kötés technika használja egy másik adatforráshoz vezérlő csatolni:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


A következő kódszegmens hoz létre, a harmadik bővített label, mind a szövegdoboz vezérlő:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

A következő kódszegmens hoz létre a negyedik letiltott szövegdoboz-vezérlő.
Bár ez a vezérlő látható különbségének a letiltott állapotú és engedélyezett állapota nem jelennek meg, a felhasználó már nem adatokat írhat a szövegmezőben.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>Egyéni vezérlők

### <a name="uocbutton"></a>UocButton

**Név**: UocButton

**Leírás**: Ez az egyszerű gomb vezérlőelem, amelyek segítségével bizonyos műveleteket indít. Azonban mivel a saját eseménykezelő nem adható meg a vezérlő használatát korlátozva.

**Tulajdonságok**:

1.  **Az összes közös tulajdonság**: ezt a tulajdonságot kapcsolatos információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  **Szöveg**: Ez a tulajdonság meghatározza a szöveg, amely akkor jelenik meg a gombra. Ez az opcionális, karakterlánc típusú attribútum. A szöveg egy explicit karakterláncértéket vesz igénybe.

Esemény:

   • **OnButtonClicked**: az esemény is ki lesz adva a gombra kattintáskor következik.

Például:

![](media/rcd-configuration-xml-reference/image017.png)


A következő XML-szegmens hoz létre a egyszerű gombra:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Név**: UocCaptionControl

**Leírás**: Ez a vezérlő egy RCDC lap feliratának megjelenítésére használ. Ez a vezérlő célja, hogy csak a fejléc csoportosítása egyetlen vezérlő használható.
Bármely más környezetben használják megjelenítési problémák vagy a portál hibákat okozhat.

**Mód**: csak olvasható (OneWay)

**Tulajdonságok:**

1.  **Az összes általános tulajdonságok:** további információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  **MaxHeight:** Ez a tulajdonság határozza meg az ikon maximális magasságát a szakaszban. Ez a tulajdonság opcionális. Ez a tulajdonság egy egész számot képpontban vesz igénybe. Az alapértelmezett érték: 32 képpont.

Például:

![](media/rcd-configuration-xml-reference/image020.jpg)

A következő kódszegmens hoz létre a **fejléc felirat**:
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


A kódszegmens hoz létre a **Explicit tartalom felirat:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

A következő kódszegmens hoz létre a **megjelenített név** dinamikus felirat:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Név**: UocCheckBox

Leírás: Ez az egyszerű jelölőnégyzet-vezérlőelem. Azt javasoljuk, hogy a felhasználó kötést létrehozni a logikai érték típusú adatot tartalmazó vezérlő. Ez a vezérlő használható egy csak olvasható vagy egy frissíthető vezérlőelemet, is kötve van az adatok alapján.

>[!NOTE]
Ebben a kiadásban, ha a jelölőnégyzet segítségével szabályozhatja egy logikai attribútum megjelenítendő szerkesztési módban az attribútum nincs hozzárendelve korábban, az erőforrás-vezérlő hozzáadja érték **hamis** attribútumához amikor **OK** szerkesztési módban a felhasználó kattint. A feladata körülbelül, mindig hozzon létre egy logikai attribútum, amely azt feltételezi, hogy nem fenntartása azonos **hamis**, vagy használjon más vezérlők, például a megfelelő választógombra kattintva logikai attribútumokhoz.

**Tulajdonságok**:

1.  **Az összes közös tulajdonság**: további információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  **DefaultValue**: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Az alapértelmezett beállítás hamis értékre. Ez a mező határozza meg a jelölőnégyzet alapértelmezés.
    Ezzel adható meg explicit módon.

3.  **Be van jelölve**: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Az alapértelmezett beállítás hamis értékre. Ez az érték a DefaultValue tulajdonság felülírja, amikor együtt DefaultValue telepítve. Ez a mező határozza meg a jelölőnégyzet viselkedését. Például a DefaultValue érték ez is explicit módon megadott vagy kötött adatokkal a kiszolgálóról.

4.  **Szöveg**: Ez az opcionális, karakterlánc típusú attribútum. A szöveg jelenik meg a jelölőnégyzettől jobbra. Ez a tulajdonság segítségével adja meg a szöveg, amelyet a végfelhasználók nyújt részletesebb információt.

**Események**:

   • CheckedChanged: megváltozásakor jelölőnégyzetet az állapotát, ez az esemény is ki lesz adva.

A következő példa egy egyéni kötésben az egyéni erőforrás típusa és az attribútum között létrejövő **IsConfigurationType**. Az XML-fájl használatban van egy egyéni erőforrástípus RCDC.

Például:

![](media/rcd-configuration-xml-reference/image022.png)

A következő szegmens előállított kódot egy **dinamikus jelölőnégyzetet**, mint a dinamikus jelölőnégyzetet az előző ábrán látható módon. Ez a kötési típus általában több sokoldalú és hasznos, mint egy explicit jelölőnégyzetet. Az attribútum az aktuális erőforrástípus kell tartoznia.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Leírás**: Ez a különleges karakterlánc formátumának támogató többsoros szövegmező-vezérlőt. Minden egyes érték a többértékű tételek között van egymástól pontosvesszővel elválasztva; vagy a szövegmezőben sortörés. Azt javasoljuk, hogy a vezérlő többértékű, rövid karakterlánc és az egész szám típusú adatokkal köthető. Ez a vezérlő csak olvasható módban és a frissíthető módot is támogatja.

**Tulajdonságok**:

1.  **Az összes közös tulajdonság**: további információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  **DataType**: Ez az egy szükséges, karakterlánc típusú attribútum. Adhat meg ezt a **karakterlánc, egész**, vagy **DateTime** írja be explicit módon. A séma attribútummal rendelkező attribútum is köthető **adattípus** tulajdonság. Többértékű referencia típus kezelje **UOCListView** vagy **UOCIdentityPicker**. Többértékű logikai változó értéke nem támogatott adattípust.

3.  **Sorok**: Ez az opcionális, egész szám típusú attribútum. A lista magassága karakterek számát is meghatározhatja. Az alapértelmezett értéke 1.

4.  **Oszlopok**: Ez az opcionális, egész szám típusú attribútum. Meghatározhatja, hány kiterjedő a be van-e számú karakterből. Az alapértelmezett értéke
    20.

5.  **Érték**: Ez az opcionális, karakterlánc típusú attribútum. Ezt az attribútumot csak az adatforrás köthető.

Események:

   • **ValueListChanged**: Ez az esemény akkor váltódik ki, a vezérlő aktuális megváltozásakor.

A következő példában egy többértékű karakterlánc attribútum nevű **AMultiValueString** hozzák létre, és az egyéni erőforrás típusa kötve. Ebben a példában csak a kötés létrehozása után is működik.

**Példa:**

![](media/rcd-configuration-xml-reference/image024.jpg)

A következő kódszegmens hoz létre az előző ábra:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**Név**: UocDateTimeControl

**Leírás**: a szövegdoboz vezérlő hasonló, de **leírás** csak bizonyos formátuma nem fogad el. Csak olvasható módban akkor jelenik meg a címke. A bemeneti karakterlánc támogatott formátuma, tekintse meg a **DateTimeFormat** tulajdonság ebben a szakaszban.

**Tulajdonságok**:

1.  **Az összes közös tulajdonság**: további információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  **DateTimeFormat**: Ez az opcionális, karakterlánc típusú attribútum. A támogatott formátumok a következők: DateTime vagy DateOnly. Az alapértelmezett értéke a dátum és idő formátumban.
    a. Dátum és idő formátumban: Ez az attribútum van formázva a hh/nn/éééé óó: pp: mm AM.

      <[!NOTE]
      Mindkét **DateTime** és **DateOnly** formátum támogatott, függetlenül a felhasználót, hogy a különbség az határozza meg.
3.  **Érték**: Ez az opcionális, karakterlánc típusú attribútum. Ezt az attribútumot az erőforrás adatforrás kötési meg. Ez az attribútum értéke felel meg a helyes dátum és idő formátumban.

Események:

   • **DateTimeChanged**: Ha a datetime érték módosításokat, az esemény akkor következik be.

Például:

![](media/rcd-configuration-xml-reference/image027.jpg)

A következő kódszegmens hozza létre az első **DateTime** vezérlő.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


A következő kódszegmens hozza létre a második **DateTime** vezérlő. Ha a mintakódot használt adatok források szakaszában, a **ExpirationTime** attribútum az összes erőforrástípus van kötve. Ezért használhatja az alábbi kódra:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**Név**: UocDropDownList

Leírás: Egy egyszerű legördülő lista vezérlő. Ez a vezérlő általában akkor használják, ha azt szeretné kiválasztani a beállítások a választási lehetőségek egy adott csoportján. Karakterlánc, egész szám, dátum és idő és logikai érték típusú adatok esetén a vezérlő használható jól.

**Tulajdonságok**:

1.  **Az összes közös tulajdonság**: további információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  **ValuePath**: A tulajdonság a Value attribútumként lekérése ItemSource. Ha egyéni ItemSource van megadva, az érték elérési értékre van beállítva. Akkor van kötve az érték mezőben az a dokumentum későbbi szakaszában meghatározott beállítás elemből.

3.  **CaptionPath**: A tulajdonság a Value attribútumként lekérése ItemSource. Az ItemSource egyéni megadása esetén érték elérési értéke felirat. Akkor van kötve a felirat mezőben az a dokumentum későbbi szakaszában meghatározott beállítás elemből.

4.  **HintPath**: A tulajdonság a Value attribútumként lekérése ItemSource. Az ItemSource egyéni megadása esetén érték elérési mutató van beállítva. Azt van kötve a mutatót a mezőben az a dokumentum későbbi szakaszában meghatározott beállítás elemből.

5.  **Az ItemSource**: ListControlItems, amely a listában határozza meg a választási lehetőségek gyűjteménye. A felhasználó explicit módon válassza az egyéni, és a beállítás elem segítségével az érték megadásához.

6.  **SelectedValue**: A jelenleg kiválasztott érték. Ez egy kötelezően megadandó, karakterlánc típusú tulajdonság. Ez a tulajdonság az adatforrásból kötött karakterlánc adatokkal.

Események:

  • SelectedIndexChanged: az esemény a legördülő lista a kijelölés megváltozásakor következik be.

Paraméterek:

Egy beállítás elem szerkezete Ez a dokumentum "Beállítás" szakaszában talál.

1.  **Érték**: az érték egy beállítás elemnek bármilyen karakterlánc, amely az adatforrás, amelyhez a vezérlőelem van kötve egy érvényes bemeneti állítható be.

2.  **Felirat**: felirat tetszőleges karakterlánc-érték lehet.

3.  **Mutató**: mutató tetszőleges karakterlánc-érték lehet.

Például:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Ahhoz, hogy a minta működik, kötni kell egy meglévő, karakterlánc típusú attribútum **hatókör** az egyéni erőforrás típusú, amely a RCDC vonatkozik.


A kódszegmens állít elő, a legördülő listában:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

Name: UocFileDownload

Leírás: A vezérlő hivatkozást tartalmaz. Amikor a hivatkozásra kattint, megjelenik a Windows a fájl mentése lap. A felhasználó a helyi meghajtóra mentheti a fájlt.
A megnyitási beállítást is támogatott, ha az Internet Explorer meg tudja jeleníteni a fájl formátumát. Az ezzel a vezérlővel, a javasolt adattípusokat formázott karakterlánc (XML) és a bináris típusú.

>[!NOTE]
Ebben a kiadásban a Microsoft Identity Manager 2016 SP1 a felhasználó zárja be az Internet Explorer-ablakban, amely tőle megnyitni a fájlt, és ezután frissítse a lapot. Után frissíteni az Internet Explorer-ablakban, a felhasználó is indíthatja a letöltés mentéséhez, vagy nyissa meg újra ugyanazt a fájlt az eredeti ablak.


Tulajdonságok:

1.  **Az összes közös tulajdonság**: további információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  **Szöveg**: Ez az opcionális, karakterlánc típusú attribútum, amely meghatározza a hivatkozás szövegét. A felhasználó megadhatja egy explicit karakterlánc ehhez a tulajdonsághoz.

3.  **Érték**: Ez az egyik kötelező attribútuma. Meghatározza a adatkockaattribútum-kötése a kiszolgálón, amelynek tartalma le kell tölteni.

4.  **PromptedFileName**: Ez az opcionális, karakterlánc típusú attribútum. Ez az, hogy a fájl nevét, amely a felhasználó számára a letöltött fájl mentésekor.

5.  **A ContentType**: Ez az egy szükséges, karakterlánc típusú attribútum. Ez a, amely az adatok mentése a fájl típusa. Szöveges vagy bináris a két módon használható karakterláncot. Ha a szöveg, a visszatérési érték hosszú karakterláncnak kell tekinteni.
    Ellenkező esetben a bináris, a visszatérési érték tekintendő byte []. Szöveges bejelölt, ha a felhasználó, lehetőségként hozzáadhat egy utótagot adja meg a szöveg formátumban van. Például text/xml érvénytelen.


>[!NOTE]
Ehhez a vezérlőhöz kötött értéke üres, ha a vezérlő hiányzik a hivatkozás letöltési művelet indításához használható. Ennek oka az, semmit nem kell letölteni.


**Események**:

Nincsenek események a vezérlőelemnél.

**Példa**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Feltölteni ezt a mintafájlt, mielőtt a felhasználó létre kell hoznia egy egyéni erőforrás típusa és a meglévő ConfigurationData attribútum közötti kötés.


A következő kódszegmenst a letöltési vezérlőt az előző ábrán állít elő:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Név**: UocFileUpload

**Leírás**: Ez a vezérlő tartalmaz, amely megjeleníti a feltölteni kívánt helyi fájl helyét, a fájl gomb és egy feltöltési gomb szövegmező. Amikor a végfelhasználó a Tallózás gombra kattint, megjelenik egy Windows-fájl megnyitása ablak. A végfelhasználó megadhatja egy fájl feltöltése a helyi meghajtón. Ha a fájl be van jelölve, a fájl helye látható a szövegmezőben. Amikor a Feltöltés gombra kattint, a rendszer a fájl feltöltése, az ügyféloldali helyi adatforráshoz. A fájl tartalma még nem továbbíthatók a kiszolgálóhoz. Az ajánlott adattípusokat ezzel a vezérlővel, a a következők: formátumú karakterlánc (XML) vagy a bináris típusú.

>[!NOTE]
A rendszer a feltöltési folyamatáról és az állapot nem jelzi. Ha a fájlt tölt fel az a helyi adatforrást, a szövegmező nincs bejelölve.


Tulajdonságok:

1.  Az összes általános tulajdonságok: Információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  Érték: Ez az egyik kötelező attribútuma. Azt adja meg a séma adatkockaattribútum-kötése, amelyre az adatok feltöltése a kiszolgálón.

3.  A ContentType: Ez a nem kötelező, karakterlánc típusú attribútum. Ez a fájl a kiszolgálón mentett adatok típusa. Ez a szöveges vagy bináris állítható. Ha a tulajdonság hiányzik, az alapérték bináris.

4.  MaxFileSize: Ez az opcionális, karakterlánc típusú attribútum. MaxFileSize meghatározza mekkora a feltöltött fájl méretét is. Alapértelmezés szerint ha a tulajdonság hiányzik, a maximális mérete 1 megabájt (MB).

5.  PromptedForNoValue: Ez az opcionális, karakterlánc típusú attribútum. A megjelenő szöveg meghatározza a felhasználó számára, amikor nem feltölteni a fájlt.

Események:

   • FileUploaded: Ez az esemény kibocsátott, ha a fájl feltöltése sikerült.

Például:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
Ahhoz, hogy az alábbi példakód működik, kell létrehozni a ABinaryAttribute nevű új bináris típusú attribútum, és hozzon létre egy új kötést egy egyéni erőforrás típusa és az attribútum között.


A következő kódszegmenst a fájlfeltöltés-vezérlő az előző ábrán állít elő:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

Name: UocFilterBuilder

Leírás: Egy összetett vezérlő, amely lehetővé teszi a felhasználó a MIM 2016 XPath kifejezés megjelenítéséhez. Néhány XPath kifejezések használata nem támogatott. A szűrő-szerkesztő használatával kapcsolatos információkért lásd: a szűrő builder súgójában.

Tulajdonságok:

1.  Összes közös tulajdonság: További információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  PermittedObjectTypes: Ez határozza meg a jelennek meg a utasítást egy szűrőkészítője típusú erőforrások listáját. A szűrő-szerkesztő használatával kapcsolatos információkért lásd: a szűrő builder súgójában. A karakterlánc ResourceTypeA, ResourceTypeB, ahol minden erőforrástípus vesszővel (,) elválasztva formátumban van.

3.  Érték: Ez az az érték, amellyel a szűrőkészítője megjelenítése.
    Csak egy kötés egy XPath-kifejezést tartalmazó karakterlánc típusú adatok esetén támogatott. A szűrő attribútum egy ajánlott attribútumot a vezérlő kötési.

4.  PreviewButtonVisible: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ez a tulajdonság értéke HAMIS, a felhasználó nem jelennek meg a Előnézet gombra. Az alapértelmezett beállítás értéke igaz. Ez a gomb segítségével egy listanézet vezérlő együtt XPath-kifejezés az eredmények.

5.  ExcludeGroupMembership: Ez a logikai tulajdonsággal. Ha ez a tulajdonság értéke igaz, nem hozható létre egy szűrőt használó \<hivatkozási attribútum\> (például ResourceID) tagja \<csoportobjektum\>. Ez azt jelenti, ha ez a tulajdonság értéke igaz, nem hozható létre egy szűrőt, amely a csoport tagságát könyvtárat használja.

6.  PreviewButtonCaption: Ez a választható karakterláncra. Ha PreviewButtonVisible értéke igaz, használhatja ezt a tulajdonságot a gombra egy egyéni szöveget adhat. A szöveg jelenik meg az Előnézet gombra.

Események:

   • OnFilterChanged: Ez az esemény akkor váltódik ki, amikor szűrő jelentéskészítő tartalma változik.

Például:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
A mintakód használatához hozzon létre egy új kötést egyik létező attribútumához szűrőt és az egyéni erőforrástípus között.


A mintakód, amely tartalmazza az UOCLabel vezérlő, a PermittedObjectTypes egyszerű szűrőkészítője és az Előnézet lista a következő: A lista nézet ListFilter tulajdonság és a szűrőkészítője Value tulajdonságához a ugyanazon adatforrás-attribútum a két csatolni kell mutatnia.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Name: UocHtmlSummary

Leírás: Segítségével a vezérlő határozza meg az Összegzés lapon RCDC-lapon.
Az összefoglalás lapon jelenik meg, miután a felhasználó kérést. Ez a vezérlő csak akkor használható az összegzés csoportosítása, és csak vezérlő kell lennie. Erősen ajánlott, hogy a megadott minta kódot használja.

>[!NOTE]
Ez a vezérlő sem tesztelték alaposan.


Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát.

2.  ModificationsXml: Ez a tulajdonság kell formázni {kötés forrás különbözeti, elérési út = = DeltaXml}, ahol a konfigurációs fejléc ObjectDataSource különbözeti van definiálva.

3.  TransformXsl: Ezt a tulajdonságot általában formátuma {kötés forrás = summaryTransformXsl, elérési út = /}, ahol a konfigurációs fejléc XmlDataSource summaryTransformXsl van definiálva.

A vezérlő egy meglévő mintát tekintse meg a csoportosítási szakaszt a jelen dokumentum korábbi "Összegzés csoportosításának".

### <a name="uochyperlink"></a>UocHyperLink

Name: UocHyperLink

Leírás: Egy egyszerű hyperlink vezérlő. Használhatja a vezérlő hivatkozásként információk megjelenítéséhez.

Tulajdonságok:

1.  Az összes általános tulajdonságok: Információkért lásd: a dokumentum az általános tulajdonságok szakaszát.

2.  ObjectReference: Ez az egy nem kötelező, referencia típusú tulajdonság. Egy érvényes erőforrásra hivatkozik az ebben a tulajdonságban megadott GUID, ha a hivatkozás segítségével a végfelhasználó az erőforrás elérésére. Ez a kölcsönösen kizárják egymást az NavigateUrl tulajdonságához (alább).

3.  Szöveg: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság segítségével határozza meg a szöveg, amelyet a hivatkozásként jelenik meg.

4.  NavigateUrl: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság segítségével határozza meg a teljes elérési útja mutató URL-címet a hivatkozás. Ez a kölcsönösen kizárják egymást az ObjectReference tulajdonság (fent).

Például:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
Érvényes GUID-azonosító egy erőforrást a összekapcsolás van szüksége. Ebben az esetben a második hivatkozás jön létre egy érvényes GUID. Az első egy lehet olyan webhelyet.


A következő kódszegmens átirányítása hivatkozást állít elő:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

A következő kódszegmens olyan erőforrásra hivatkozott hivatkozást állít elő. A kifejezés a explicit hivatkozás behelyettesíthető {kötés forrás objektum elérési útja = létrehozó =} kötni, ez egy adatforrás. Ez lehet érvényes, csak ha az erőforrás-kezelő létezik, és a referencia-típus értéke.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

Name: UocIdentityPicker

Leírás: A vezérlő áll egy választható Resolve mezőbe, és a böngészőablakban. A választható Resolve áll egy választható szolgáló szövegmező. Adja meg az identitás, a Feloldás gombra a identitás megoldásához és a Tallózás gombra a előugró böngészőablakban kéréséhez. A tallózási ablak lehetővé teszi a felhasználó számára egy listanézet vezérlő keresztül identitásokat. A kijelölt identitás a Tallózás ablakban a feloldás mezőben is megjelenik.

Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát.

2.  UsageKeywords: Ez az egy nem kötelező karakterlánc típusú tulajdonság. Megadhatja, hogy a keresési tartományok SearchScopeConfiguration struktúra által támogatott használati kulcsszavakat listájának megadásával az erőforrás-választó használhatók ahol minden kulcsszó (') elválasztott listáját.

3.  Szűrő: Ez az egy nem kötelező karakterlánc típusú tulajdonság. A felhasználó megadja a hatókör csak a megadott hatókörön belüli illeszkedő elemek megjelenítése céljából az erőforrás-választó XPath kifejezés. Ez a tulajdonság (fent) UsageKeywords tulajdonság kölcsönösen kizárják egymást. A keresés hatókörét alkalmazása esetén ez a tulajdonság nincs hatása.

4.  ResultObjectType: Ez az egy nem kötelező karakterlánc típusú tulajdonság. Az erőforrástípus az előugró párbeszédpanelen listában erőforrások megjelenítéséhez használatos. Ez használatos a szűrő segítségével az identitás-választó azonosíthatja az erőforrás típusát adja vissza, a szűrő, és ennek megfelelően megjeleníteni az adatokat. Ez a tulajdonság (lásd fent) UsageKeywords tulajdonság kölcsönösen kizárják egymást. Ha a keresés hatókörét alkalmazza, ennek nincs hatása. Az ehhez a tulajdonsághoz elfogadott karakterlánca bármely egyetlen, érvényes, erőforrástípus-név, például személy.
    A szűrő több erőforrástípusok vissza várakozásával erőforrást használja.

5.  PreviewTitle: Ez a lista nézetben használt preview címe. Ez a tulajdonság kapcsolatos információkért UocListView című.

6.  ListViewTitle: Ez az egy nem kötelező karakterlánc típusú tulajdonság. Ez a tulajdonság segítségével határozza meg a listanézet címként felett megjelenő szöveg.

7.  Érték: Ez az egy nem kötelező karakterlánc típusú tulajdonság. Javasoljuk, hogy Ön ez elemmel történő kötés egy séma attribútumának értéke adatforráshoz kapcsolódni.

8.  Mód: Ez az egy nem kötelező karakterlánc típusú tulajdonság. Ez a tulajdonság segítségével határozza meg, hogy egy érték kiválasztható az identitás-választó, vagy több identitás választható. SingleResult és MultipleResult értékei engedélyezett. Alapértelmezés szerint azt értéke SingleResult.

9.  ObjectTypes: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Megadhatja, hogy a felhasználók fel tudják oldani a bejegyzések szemben a identitás objektumválasztó megoldásához mezőbe típusú erőforrások listáját. Az a lista az erőforrástípus nevei vesszővel (,) elválasztva.

10. AttributesToSearch: Egy nem kötelező, karakterlánc-typeproperty. Megadhatja, hogy-elem az identitás-választó, ahol a lista séma attribútumlistát, választja el egymástól vesszővel (,) feloldásához használt attribútumok listáját. Például ha AttributesToSearch Alias DisplayName, értéke, ez azt jelenti, hogy a felhasználó a szolgáltatáskéréshez elemek megkereshetik = \<érték keresése\> vagy Alias =\<érték keresése\>. Azt javasoljuk, hogy az itt megadott attribútumnevek kell-e érvényes attribútumok használatát a céltípusok erőforrás értékének elhelyezni az adatforrás. A cél erőforrástípusok esetében a ObjectTypes mezőben található. Összes attribútumát a bármely adott erőforrástípusok esetében, amelyek a ObjectTypes mezőben idézik érvényesnek kell lennie.

11. ColumnsToDisplay: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. A felhasználó a séma attribútumnevek, vesszővel (,) elválasztva listáját tartalmazza. Az itt megadott attribútumokat a listanézet az identitás-választó az oszlop alkotják.

12. Sorok: Ez az egy nem kötelező, egész szám típusú tulajdonság. Csak akkor, ha a mód beállítása MultipleResult működik. Ez a tulajdonság használatával a feloldás szövegmező magasságának beállítása a megadott méretre karakter egységekben.

13. MainSearchScreenText: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez az egyéni szöveg, a Keresés a Tallózás ablakban futása közben.

Események:

 • SelectedObjectChanged: Ez az esemény kibocsátott, amikor a felhasználó módosítja a kiválasztott erőforrásokat.

Például:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Ez a minta működéséhez létre kell hoznia egy új kötést a kezelő attribútumnál, illetve az XML-kód vonatkozik egyéni erőforrás típussal.


A következő kódszegmens állít elő, és a ResultObjectType tulajdonságainak használatával a RCDC részeként egy identitás objektumválasztó egyetlen módban:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



Például:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Ahhoz, hogy a mintakód működik, a ExplicitMember attribútum (egy többértékű hivatkozási attribútum) kötni kell az egyéni erőforrás típusát. A UsageKeyword tulajdonsága személy és csoport keresési tartományokat is létre kell hoznia.


A következő kódszegmens létrehozza a vezérlőelemet az előző ábrán:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

Name: UocLabel

Leírás: Ez az egyszerű, csak olvasható, szöveges felirat. Azt javasoljuk, hogy a vezérlő csak olvasható adatok megjelenítéséhez használható.

Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát.

2.  Szöveg: Esetén ez egy karakterlánc-type attribútum. Ez a tulajdonság határozza meg, egy explicit karakterlánc beírásával vagy egy adatforrással kötés. Ennek a tulajdonságnak walue rendelő minta-kötés {kötés forrás objektum elérési útja = =\<érvényes név attribútum\>.

A UocLabel vezérlő mintát lásd: egyszerű vezérlőelem egyszerű vezérlő minták.

### <a name="uoclistview"></a>UocListView

Name: UocListView

Leírás: Egy speciális Listanézet-vezérlő. Egy egyszerű lista nézet, egy nem kötelező egyszerű keresés, egy nem kötelező speciális keresési vezérlőelemet, egy nem kötelező kijelölési minta mezőben és egy műveletsávon gomb áll. A választható egyszerű keresés a keresés hatókörét és egy egyszerű keresőmezőben áll. A speciális keresési vezérlőelemet a szűrőkészítője. A listán erőforrás prerendered listáját jeleníti meg. Ez akkor is megjelenhet a keresési eredményeket a keresési vezérlőkkel a vezérlő származik. Az akciógombra kattinthat sáv határozza meg, mi a teendő átvihető alapján a kijelölés a listanézetben. A kijelölési minta mezőben jeleníti meg, milyen elemet listájából.

>[!IMPORTANT]
UocListView egyértékű hivatkozás attribútumok nem működik. Csak többértékű hivatkozás attribútumokkal rendelkező használható. Hivatkozás egyértékű attribútumok lásd: UocIdentityPicker ebben a dokumentumban.


Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát.

2.  SelectedValue: Egy nem kötelező, karakterlánc típusú tulajdonság, amely általában egy többértékű hivatkozási attribútum egy GUID-formátumú karakterlánc-listával elfogadása van kötve.

3.  A pageSize értékének: Ez az egy nem kötelező egész szám típusú tulajdonság. A felhasználó megadhatja, hogy hány bejegyzés elfér a listanézet-vezérlő egy oldallal. Az alapértelmezett érték: 10 bejegyzéseket. Érvénytelen a bármilyen pozitív egész szám lehet.

4.  UsageKeyword: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. A felhasználó megadhatja, amelyek meghatározzák, milyen keresési hatókör szolgál a Listanézet keresési vezérlőelemet a kulcsszavak listáját. FIM 2010 kiszolgáló keresési hatókör erőforrások találhatók. Az attribútumot a SearchScopeConfiguration struktúra, UsageKeyword, nevű csoport keresési tartományok készlete szolgál. A lista nézet a kulcsszavak listáját, hogy használ fel. Minden kulcsszó vesszővel (,) választja el egymástól.
    Ezt a usagekeyword használja a megfelelő keresési hatókör, amelyet a lista nézet megjelenítése. Ez csak akkor használható, ha ShowSearchControl tulajdonság értéke igaz.

5.  SearchControlAutoPostback: Ez a logikai tulajdonságot nem kötelező megadni. Ez a tulajdonság értékét állítsa igaz értékre végre teheti lehetővé, ha akkor váltódik ki, a keresés. Alapértelmezés szerint SearchControlAutoPostback hamis értékre van beállítva.

6.  EmptyResultText: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Alapértelmezés szerint nincs elem értékűre van állítva, de karakterlánc értéke beállítható. Ez a szöveg jelenik meg a Keresés eredménye üres.

7.  ButtonHeight: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság értékének beállítása bármilyen pozitív egész értékre. Ez a tulajdonság megadja a gombok magassága képpontban műveletsávon. Az alapértelmezett érték: 32 képpont.

8.  ButtonWidth: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság értékének beállítása bármilyen pozitív egész értékre. Ez a tulajdonság megadja a gombok szélessége képpontban műveletsávon. Az alapértelmezett érték: 32 képpont.

9.  CaptionImageMaxHeight: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság értékének értékre bármilyen pozitív egész szám lehet. Ez a tulajdonság megadja egy választhatóan ikon maximális magasságát. Az alapértelmezett érték: 32 képpont.

10. CaptionImageMaxWidth: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság értékének értékre bármilyen pozitív egész szám lehet. Ez a tulajdonság megadja egy választhatóan ikon maximális szélességét. Az alapértelmezett érték: 32 képpont.

11. CaptionImageUrl: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság megadja egy olyan URL-címet a felirat képként megjelenő lemezképre.

12. PreviewTitle: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság segítségével határozza meg a kijelölési minta mezőben felett megjelenő szöveg.

13. EnableSelection: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ez a tulajdonság segítségével határozza meg, hogy egy listanézeti kijelölés módban van. Ha egy listanézeti kijelölési módban, jelölőnégyzetek oszlop jelenik meg a listanézet bal szélső oszlopában, és kijelölési minta megjelenik egy a listanézet alján. Az alapértelmezett érték a tulajdonság értéke igaz.

14. SingleSelection: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ha a kijelölési mód a listanézet be van kapcsolva, az érték true korlátozza a végfelhasználó csak egy elem kijelölése a listából. Alapértelmezés szerint ez a tulajdonság értéke hamis. Ez azt jelenti, hogy alapértelmezés szerint a végfelhasználó is több elemet jelöl ki a listából.

15. RedirectUrl: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság használatával a hivatkozott elem a listában kattintáskor lap megadása. Az URL-cím, amely a futtatás során a tényleges érték helyére helyőrzők tartalmazhat. A helyőrzőket a következők:

     ◦ {0} - objectType

     ◦ {1} - objectID

     ◦ {2} - displayName

16.  ShowTitleBar: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ez a tulajdonság használatával adja meg, hogy a címsorában látható legyen-e. Az alapértelmezett érték a tulajdonság értéke "false".

17.  ShowActionBar: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ez a tulajdonság használatával adja meg, hogy a művelet sáv területen látható legyen-e. Ez a tulajdonság alapértelmezett értéke igaz.

18.  ShowPreview: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ez a tulajdonság használatával adja meg, hogy az előnézeti területen látható legyen-e. Ez a tulajdonság alapértelmezett értéke igaz.

19.  ShowSearchControl: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ez a tulajdonság használatával adja meg, hogy a keresési vezérlőelemet látható legyen-e. Ez a tulajdonság alapértelmezett értéke igaz.

20.  ResultObjectType: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság használatával adhatja meg a keresési eredmények között várt adat: objektum. Ez a tulajdonság alapértelmezett értéke erőforrás. Ha a Keresés eredménye több erőforrás-típust tartalmaz, ezt az értéket kell megadni: erőforrás.

21.  ColumnsToDisplay: Ez a tulajdonságot nem kötelező megadni. Ez a tulajdonság használatával adja meg, melyik attribútumok oszlopok megjeleníteni a listanézetben. Ez a tulajdonság alapértelmezett értéke DisplayName, ResourceType. Minden oszlop egy attribútum a rendszer nevét jelöli. Egyes oszlopok vesszővel (,) választja el egymástól. Nem kell megadnia egy értéket a tulajdonság a kijelölési mód a listanézet használata esetén. Kijelölési módban az oszlop beállítás a keresés hatókörét a kiválasztott SearchScopeColumn attribútumának származik.

22.  ListFilter: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez az a XPath elérési utat a listanézetben leképezéséhez használt, és csak akkor használható, ha ShowSearchControl tulajdonsága hamis értékre van beállítva. Ez az érték megadása esetén a lista nézet használ a tulajdonság értéke a lekérdezések, és a listanézet nem kijelölési módban. A szűrő vagy köthető egy karakterlánc attribútumot az erőforráshoz az alábbiak szerint:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     vagy karakterlánc, amely tartalmaz néhány előre meghatározott környezeti változó a következők: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: Ez a rendszer elavult tulajdonságot. Az érték legyen. a rendszer a többértékű hivatkozott attribútum nevét. Azt javasoljuk, hogy ez a tulajdonság nem használható kapcsolatban. Például a csoportok kezelése helyett a következőt:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  Meg kell:`<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság használatával adja meg, hogy az elem megtekintése a listanézet eleme server visszaküldés elindítása vagy a részletek megjelenítéséhez kattintson a. Két lehetőség érték támogatott: ModelessDialog és a kiszolgáló. Az alapértelmezett érték: ModelessDialog.

25.  SearchOnLoad: Ez az egy nem kötelező, a logikai érték típusú tulajdonság, amely meghatározza, hogy a listanézet vezérlő betöltéskor kell lekérdezni. Ez a tulajdonság csak a lista nézet kijelölés módban van esetén alkalmazható. Ez a tulajdonság alapértelmezett értéke true. Kikapcsolható, ha a felhasználó általában be a keresési eredmény jelentéssel bíró szöveget várt. Ebben az esetben a listán kezdetben jeleníti meg egy üzenetet, amely a felhasználó értesítése, hogyan hajthat végre egy keresést. A szöveg testre szabható a következő tulajdonságokkal:

26.  MainSearchScreenText: Ez nem kötelező, karakterlánc típusú tulajdonság alkalmazható csak ha SearchOnload értéke TRUE. Ez a tulajdonság segítségével testre szabhatja a szöveg, amelyet a listanézet közepén akkor jelenik meg, amikor a listanézet automatikusan nem keres. Ez a tulajdonság alapértelmezett értéke az erőforrások azt szeretné, a Keresés a fenti keresés. Ahhoz, hogy a szöveg a forgatókönyvhöz kapcsolódó több értéket is megadhat.

27.  SubSearchScreenText: Ez nem kötelező, karakterlánc típusú tulajdonság segítségével testre szabhatja a MainSearchScreenText alatt megjelenő szöveget. Általában nem kell adjon meg egy értéket ehhez a tulajdonsághoz, kivéve, ha hozzá szeretne adni a listanézet használatával kapcsolatos néhány további utasítás.

Együtt a UocFilterBuilder vezérlő listanézet használata preview listaként példákért lásd a jelen dokumentum korábbi UocFilterBuilder mintát. A UocListView a szűrőkészítője nélkül is használható.

### <a name="uocnumericbox"></a>UocNumericBox

Name: UocNumericBox

Leírás: Ez az egyszerű szövegmező, amely csak egész szám lehet. Ez a vezérlő csak olvasható módban és a frissíthető módot is támogatja.

Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát.

2.  MaxValue: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság segítségével határozza meg a vezérlő egy ügyféloldali ellenőrzés. A végfelhasználó által, kérésre érték nem haladhatja meg ezt az értéket. Explicit egész számot adjon meg, vagy eszközben ez egész adatokat az adatforrásból származó {kötés forrás = schema elérési út = IntegerMaximum}.

3.  A MinValue: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság segítségével határozza meg a vezérlő egy ügyféloldali ellenőrzés. Az érték, amely a végfelhasználó által, kérésre nem lehet alacsonyabb, mint ez az érték. Explicit egész számot adjon meg, vagy ez elemmel történő kötés egész adatok adatforrásból származó forrás kötés használatával = schema, elérési út = IntegerMinimum}.

4.  DefaultValue: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság segítségével határozza meg a vezérlő alapértelmezett értéket, ha a vezérlő segítségével hozzon létre új adatokat. Ez az érték csak explicit módon beállítva egy statikus egész számra.

5.  Érték: Ez az egy nem kötelező, egész szám típusú tulajdonság. Amikor létrehozta kötését Ez az egész szám típusú adatot tartalmazó adatforrásból származó, adott attribútum értéke akkor jelenik meg, amikor a lapon be van töltve, és majd a rendszer menti az adatforráshoz való elküldése után.

Kezelő:

   • TextChanged: Ez az esemény kibocsátott megváltozásakor aktuális a vezérlőn belül.

Például:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
Az alábbi példakód állít elő, az első numerikus mezőben. A numerikus mezőben nem adatforrás vagy bármely sémaadatok csatlakozik.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

Az alábbi példakód állít elő, a második numerikus mezőben.

>[!NOTE]
Ez a minta használatához létre kell hoznia egy új, egész szám típusú hívott AnIntegerAttribute attribútumot, majd kösse a egyéni erőforrás-típusú.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

Name: UocPictureBox

Leírás: A vezérlő használt kép, a bináris típusú adatok megjelenítéséhez. Azt javasoljuk, hogy a vezérlő bináris típusú adatok használható. A kép megjeleníthetők, akár egy megadott kép URL-címe, a bináris típusú adatok vagy a kép típusú adatokat tartalmazó attribútum forrása.

Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát.

2.  ImageUrl: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Adja meg a cél kép URL-CÍMÉT.

3.  MaxHeight: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Meghatározza a kép képpontban maximális magasságát.

4.  MaxWidth: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Meghatározza a kép képpontban maximális szélességét.

5.  Képadatok: Ez a bináris típusú tulajdonság. Ez a tulajdonság használatával a megjelenített kép adatforrás kötése. A kötött adatforrás nem bináris lehet.
    Ez a mező használatával explicit módon beállítva egy képet, adja meg a byte [] formátumban.

6.  ImageResource: Ez az egy nem kötelező, a bináris típusú tulajdonság.

7.  AlternativeText: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a tulajdonság alternatív szövegként jelenik meg, amikor a képet nem lehet megjeleníteni.

Példa:

>[!NOTE]
Ez a minta használatához rendelkeznie kell egy meglévő képadatok kötni a vezérlőhöz.


A következő kódszegmens mezőben vezérlőt a vezérlőhöz adatforráshoz van kötve, állít elő:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

A következő kódszegmens mezőben vezérlőt, ha van kötve a vezérlő URL-cím rendszerképének állít elő:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

Name: UocRadioButtonList

Leírás: A rendszer egy egyszerű,-választógombot. Az alábbiak közül választhat a listában szereplő kölcsönösen kizárják egymást. Ez a vezérlő ajánlott, ha a felhasználók öt vagy kevesebb lehetőségek közül választhat. Ellenkező esetben UOCListView ajánlott.

Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát.

2.  ValuePath: Az érték elérési értékre van beállítva. Az ebben a dokumentumban megadott beállítás elemből az érték mezőbe a kötés.

3.  CaptionPath: Az érték elérési út értéke felirat. Az ebben a dokumentumban megadott beállítás elemből és a felirat mező kötés.

4.  HintPath: Mutató elérési érték van beállítva. Az ebben a dokumentumban megadott beállítás elemből a mutatót mezőhöz kötés.

5.  SelectedValue: A jelenleg kiválasztott értéknek. Ez egy kötelezően megadandó, karakterlánc típusú tulajdonság. Ez a tulajdonság karakterlánc adatokat az adatforrásból van kötve.

Események:

1.  SelectedIndexChanged

2.  CheckedChanged

Paraméterek:

Csak lehet két beállítás elemek beállításai ennél a vezérlőnél.

1.  : Lehetőség az elemet a érték mező értéke IGAZ vagy hamis értékűre kell beállítani.

2.  Leírás: Ez lehet a bármilyen karakterlánc típusú értéket.

3.  Emlékeztető: Ez lehet a bármilyen karakterlánc típusú értéket.

Például:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Ez a példa működéséhez a létre kell hoznia egy új logikai attribútum, ABooleanAttribute, és az egyéni erőforrás típusú a kötést.

A következő kódszegmens az előző ábrán hoz létre a beállítások gomb listában:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


Name: UocSimpleRadioButton

Leírás: Ez az egyszerű, választógomb vezérlőelem. Ez a vezérlő használata hasonló egy egyszerű jelölőnégyzetet. Nincsenek két választógombok, és a címkézés szöveg megjelenítése. A vezérlő kötve logikai érték típusú adatok használata ajánlott.

Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát

2.  Igaz szöveg: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez az a választógomb megjelenítését kiválasztásakor megjelenő szöveget.

3.  Hamis szöveg: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez az szöveg, ha nincs bejelölve a választógomb megjelenítését.

4.  SelectedItem: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ez az érték azt jelzi, hogy van-e jelölve a választógomb megjelenítését. Ez köthető logikai érték típusú adatot tartalmazó adatforrásból. Az alapértelmezett beállítás hamis értékre.

Események:

   • CheckedChanged: Ha a beállítás gombra módosítások állapot, a kiválasztott nincs bekapcsolva, vagy ellenkező esetben ez a jel is ki lesz adva.

Például:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Ahhoz, hogy a minta működik, kell létrehozni új logikai attribútum ABooleanAttribute, majd kösse az egyéni erőforrás típusát. A RCDC adatok vonatkozik, az egyéni erőforrás ugyanolyan.

A következő kódszegmenst a választógomb megjelenítését az előző ábrán állít elő:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

Name: UocTextBox

Leírás: Ez az egyszerű szövegmező, amely támogatja a bemeneti karakterlánc típusú. Azt javasoljuk, hogy használja-e a vezérlő kötni, karakterlánc típusú adatokat.

Tulajdonságok:

1.  Az összes általános tulajdonságok: Ez a tulajdonság információ: a dokumentum az általános tulajdonságok szakaszát.

2.  MaxLength: Ez az opcionális, egész szám típusú attribútum. Ezt a tulajdonságot a bemeneti karakterlánc maximális hosszát határozza meg. Ez a tulajdonság alapértelmezett értéke 128 karakter hosszú lehet.

3.  Szöveg: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez az a szöveg, megjelenik a szövegmezőben. Megadhatja egy explicit karakterlánc, amely megjelenik a szövegmezőben a vezérlő a kezdeti betöltés során, és kösse a séma attribútum egy string típusú.

4.  Sorok: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság megadja a szövegmező magassága karakter egységekben. Az alapértelmezett értéke 1 karakter.

5.  Oszlopok: Ez az egy nem kötelező, egész szám típusú tulajdonság. Ez a tulajdonság megadja a szövegmező szélessége karakter egységekben. Az alapértelmezett érték 20 karakter.

6.  Naplócsonkulási: Ez az egy nem kötelező, a logikai érték típusú tulajdonság. Ez a tulajdonság true értékre állításával a felhasználó engedélyezi a szövegmezőben tördelése szolgáltatás. Az alapértelmezett érték a tulajdonság értéke igaz.

7.  UniquenessValidationXPath: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Egy érvényes FIM XPath-kifejezést fogad, és biztosítja, hogy a felhasználó által bemeneti értéke az erőforrásokat, amelyek a szűrő hatókörében belül egyedi.
    Például győződjön meg arról, hogy a felhasználó azt kérte a megjelenítendő név egyedi összes engedélyezése levelezési biztonsági csoportja szerepel a FIM szolgáltatás adatbázisa belül, akkor használja az XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. Az ellenőrzési művelet történik, amikor a felhasználó elhagyja a lapot. Ez a tulajdonság csak egy erőforrást a RCDC támogatott.

8.  UniquenessErrorMessage: Ez az egy nem kötelező, karakterlánc típusú tulajdonság. Ez a karakterlánc szolgál egy hibaüzenetet jeleníti meg, ha a UniquenessValidationXPath érvényesítés meghiúsul, és explicit szöveg vagy egy karakterlánc erőforrás változó lehet. Ha ez a tulajdonság nincs megadva, az ellenőrzése sikertelen volt az alapértelmezett hibaüzenet-e a "érték % már létezik. Adjon meg egy másik."

Események:

   • TextChanged: Ez az esemény is ki lesz adva a szövegmezőben lévő szöveg megváltozásakor.

Az egyszerű vezérlő minták című szakaszában talál egy teljes mintát a vezérlő.

## <a name="appendix-a-default-xsd-schema"></a>A függelék: alapértelmezett XSD-séma

Az alábbiakban áttekintjük a teljes XSD-séma összes alapértelmezett RCDCs által biztosított Microsoft Identity Manager 2016 SP1:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>B függelék: alapértelmezett XSL összegzése

Az alábbiakban áttekintjük a teljes összefoglaló XSL biztosított Microsoft Identity Manager 2016 SP1:
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```

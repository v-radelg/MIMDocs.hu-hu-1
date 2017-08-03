---
title: "A Microsoft Identity Manager 2016 portál testreszabása |} Microsoft Docs"
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
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>A Microsoft Identity Manager 2016 portál testreszabása


>[!Warning]
Győződjön meg arról, törölje a gyorsítótárban, ha bármely CSS testreszabást.

A Microsoft Identity Manager 2016 (MIM), beleértve a szalagcím emblémájának, erőforrásait, és egymásra épülő stíluslapok személyre is szabhatja a jelszó-csoporttal, a kiválasztott elemek.

Ehhez az szükséges, néhány dolog szükség, a Testreszabás szintjétől függően. A MIM 2016 jelszó-regisztrálási portál testreszabásával kapcsolatban részt, és állítsa alaphelyzetbe a portálok elemek listája a következő:

-   Testreszabások mappa - azt a mappát, amely az alapértelmezett érték használata előtt ellenőrzi a MIM 2016. Minden egyes portál, amely a testreszabása szükséges testreszabások mappa. Testreszabások csak kell végre ebben a mappában, mert a telepítő nem fog felülírja a frissítések, módosítása módban telepíti vagy javítása módban telepíti.

-   Strings.resources – Ez az egy XML-alapú fájl, amely lehetővé teszi, hogy a karakterláncokkal, amely megjelenik a portálon módosíthatja. Ez a fájl kell a testreszabások mappában találhatók.

-   Style.css – a portál által használt testreszabási az egymásra épülő stíluslap. A stíluslap kell létrehozni és módosítani az embléma módosítani kell, vagy lehet teljes egészében cserélni a saját egyéni beállításokat.

Részletes a testreszabás, a jelszó-regisztráció és a jelszó-változtatási kapuk témakörben tesztlabor-Útmutató:, amely tartalmazza a MIM 2016 jelszó-regisztráció és a portál testreszabás alaphelyzetbe állítása.

>[! WARGNING] ahhoz, hogy felismerje a testreszabott módosításokat MIM, újra kell indítania az IIS iisreset futtatásával.


## <a name="creating-the-customizations-folder"></a>A testreszabások mappa létrehozása

Indításkor a MIM a Strings.resources fájlt a testreszabások mappában keresi az alapértelmezett érték használata előtt. A könyvtárban, a portál (pl. a jelszó-regisztrálási portál vagy a jelszó-változtatási portál) testreszabni kívánt egyéni beállításokat mappát kell létrehoznia. Ha szeretné testre szabni mindkét portálok akkor szüksége lesz a következő tartozó testreszabások mappa létrehozása:

-   C:\\programfájlok\\Microsoft Forefront Identity Manager\\2010\\jelszó-regisztrálási portál

-   C:\\programfájlok\\Microsoft Forefront Identity Manager\\2010\\jelszó-visszaállítási portál

### <a name="to-create-the-customization-folder"></a>A testreszabási mappa létrehozása

1.  Lépjen a C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\jelszó-regisztrálási portál mappa.

2.  Hozza létre a testreszabásokat.

3.  Keresse meg a jelszó-változtatási portál mappát egy biztonsági szintet, és hozza létre a testreszabások a.

## <a name="customizing-strings"></a>Karakterlánc testreszabása

A portál felhasználói felületének karakterláncok számos testreszabható Strings.resources fájlt hoz létre, és vegye fel ezt a fájlt a testreszabások mappába. Szüksége lesz az egyes portál, amely a testreszabni kívánt Strings.resources fájl létrehozásához.

### <a name="to-customize-strings"></a>Karakterlánc testreszabása

1.  A Jegyzettömb segítségével a következő kódot az alábbi másolja és mentse Strings.resources testreszabások mappába

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  Az a `<!-- Customizations begin here -->` szakaszban módosítsa az nevét, amelyről testre szabhatja, és írja be az érték között karakterláncokat szeretne a karakterláncok kereséséhez a <value> </value> címkék. Tekintse meg az alábbi listáról a karakterláncokat, amely testre szabható, és az alapértelmezett értékekre.

>[!NOTE]
A **Strings.resources** fájl nyelvsemleges. Nyelvi adott testreszabott karakterláncok létrehozásához meg kell adott nyelvi csomag telepítve van, és mentse a fájlt a formátumú karakterlánc. `<language>-<culture>.resources`, például Strings.en us.resources.

A következő látható, amely testre szabható portál karakterláncok listáját.

<a name="portal-strings"></a>Portál karakterláncok
--------------

| Név                                                     | Alapértelmezett érték                                                                                                                                                                                                                                                                                                                                         | Megjegyzés                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Információ         |       |
| ButtonCancel                                             | Mégse                                                                                 |     |
| ButtonFinish                                             | Befejezés    |    |
| ButtonNext                                               | Tovább    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | A munkamenet már nem aktív. Bezárhatja az ablakot, vagy az alábbi hivatkozásra kattintva újraindíthatja.         |      |
| CancelFinishedTitle                                      | Munkamenet véget ért                                                              |      |
| ErrorDescription_3000                                    | Hiba történt. Próbálja meg újból, és ha a probléma továbbra is fennáll, forduljon a Súgó vagy a rendszergazdához. (Hiba: 3000)       |          |
| ErrorDescription_3001                                    | Gondoskodjon arról, hogy helyesen adja meg a felhasználó nevét. Ha továbbra sem sikerül átállítania a jelszavát, lépjen kapcsolatba a      |           |
|                                                          | a segélyszolgálat segítségét. (Hiba: 3001)   |                   |
| ErrorDescription_3002                                    | A munkamenet véget ért. Térjen vissza a kezdőlapra, indítsa újra. (Hiba: 3002)                                     |                |
| ErrorDescription_3003                                    | Az aktuális felhasználói fiók nem ismeri fel a Forefront Identity Manager.Please lépjen kapcsolatba a Súgó vagy a rendszergazdához. (Hiba: 3003)           |               |
| ErrorDescription_3004                                    | Nincs joga regisztrálhatnak a jelszóváltoztatásra. Lépjen kapcsolatba a Súgó vagy a rendszergazdához. (Hiba: 3004)     |              |
| ErrorDescription_3005                                    | Legalább egy beírt válasz nem egyeznek meg a jelszó Registration.In sorrendje a jelszó alaphelyzetbe állítása során megadott válaszokkal, most már a válaszokat meg kell egyeznie a választ regisztrálásakor. Indítsa újra a kezdőlapról, vagy lépjen kapcsolatba a Súgó vagy a rendszergazdához. (Hiba: 3005) |           |
| ErrorDescription_3006                                    | A megadott jelszó érvénytelen. Regisztrálhat a jelszó alaphelyzetbe állítása a helyes jelszót kell megadnia. (Hiba: 3006)            |              |
| ErrorDescription_3007                                    | Ön jelenleg nem engedélyezett a jelszó alaphelyzetbe állításával. Próbálkozzon újra később, vagy kérjen segítséget a Súgó vagy a rendszergazdához. (Hiba: 3007)     |         |
| ErrorDescription_3008                                    | Hiba történt. Próbálja meg újból, és ha a probléma továbbra is fennáll, forduljon a Súgó vagy a rendszergazdához. (Hiba: 3008)        |          |
| ErrorDescription_3009                                    | A bemenet nem engedett formátumú szöveget tartalmaz. Próbálja meg újból más bemeneti adatokkal, vagy forduljon a Súgó vagy a rendszergazdához. (Hiba: 3009)     |             |
| ErrorDescription_3010_Registration                       | Parancsfájl-kezelési nincs engedélyezve a böngészőben. Engedélyezze a parancsfájlkezelést, és vissza a jelszó-regisztráció kezdőlapjára, vagy forduljon a segélyszolgálathoz segítségért.      |               |
| ErrorDescription_3010_Reset                              | Parancsfájl-kezelési nincs engedélyezve a böngészőben. Engedélyezze a parancsfájlkezelést, és térjen vissza a Jelszóátállítás kezdőlapjára, vagy forduljon a segélyszolgálathoz segítségért.     |            |
| ErrorDescription_3011                                    | A webhely cookie-kat használ. A böngészőben a cookie-k elfogadására, és próbálkozzon újra, vagy forduljon a segélyszolgálathoz segítségért.          |                                           |
| ErrorDescription_3012                                    | A megadott adatok nem felelt meg az Önnek küldött biztonsági kóddal. Próbálkozzon újra átállítani a jelszavát, vagy forduljon a segélyszolgálathoz segítségért.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Nem lehet biztonsági kódot küldeni. Lépjen kapcsolatba a támogatási szolgálat segítségét.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Adja meg a felhasználónév formátuma helytelen.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Adjon meg egy felhasználónevet, a folytatáshoz.                                              |                         |
| ErrorMessagePasswordRequired                             | Adjon meg egy jelszót.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Győződjön meg arról, mind a jelszó egyezik-e.                                |           |
| ErrorPageDefaultHeading                                  | Alkalmazáshiba                                            |               |
| ErrorPageServerTime                                      | Kiszolgáló idő: {0:T}                     | {0} az az idő, ahol a rendszer a kivétel lépett fel. A t "hatására az átadott időt kell formázni"hosszú idő." Az kat, az óra, perc, és második és esetleg a du. megjelölést (attól függően, hogy a jelenlegi kulturális környezethez). |
| ErrorPageTitle                                           | A Forefront Identity Management - jelszó hiba                     |   |
| ErrorTitle_3000                                          | Hiba                                  |  |
| ErrorTitle_3001                                          | A hozzáférés megtagadva                          |  |
| ErrorTitle_3002                                          | Munkamenet véget ért                          |  |
| ErrorTitle_3003                                          | Ismeretlen felhasználó                      |  |
| ErrorTitle_3004                                          | Jogosulatlan felhasználók                      |  |
| ErrorTitle_3005                                          | Válaszok nem egyeznek.                    |  |
| ErrorTitle_3006                                          | Helytelen jelszó                     |  |  
| ErrorTitle_3007                                          | A hozzáférés megtagadva ideiglenesen              |  |
| ErrorTitle_3008                                          | Kommunikációs hiba                    |  |
| ErrorTitle_3009                                          | Tiltott bemeneti                       |  |
| ErrorTitle_3010                                          | Hiba történt a böngésző konfiguráció            |  |
| ErrorTitle_3011                                          | Hiba történt a böngésző konfiguráció            |  |
| ErrorTitle_3012                                          | Ellenőrzése sikertelen.                    |  |
| ErrorTitle_3013                                          | Nem sikerült elküldeni a biztonsági kódot   |  |
| FinalizeRegistrationHeading1                             | Ha meg a jelszó átállítására lenne szükség:        |   |
| FinalizeRegistrationSubHeading1                          | Ugrás a jelszó-változtatási portált   |   |
| FinalizeRegistrationSubHeading2                          | Személyazonosságát   |   |
| FinalizeRegistrationSubHeading3                          | Új jelszó    |     |
| FinishingDescription                                     | Új jelszó        |    |
| FinishingTitle                                           | Jelszó alaphelyzetbe állítása:      |     |
| GotoPortalPrefix                                         | odamegy     |        |
| GotoPortalSuffix                                         | Kezdőlap     |      |
| LabelTroubleshootingLinkText                             | Részleteinek megtekintése           |    |
| LoadingText                                              | Betöltése...     |  |
| NoScriptTagErrorMessage                                  | Parancsfájl-kezelési nincs engedélyezve a böngészőben. Engedélyezze a parancsfájlkezelést, és térjen vissza a kezdőlapra, vagy forduljon a segélyszolgálathoz segítségért.      |   |
| PasswordResetOperationGeneralErrorMessage                | Hiba történt a jelszó átállítására tett kísérlet során.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | A jelszó nem felel meg a szervezet jelszóházirendjének.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Hiba történt a jelszó alaphelyzetbe állítása, a felhasználó nem módosíthatja a jelszót.    |   |
| PrivacyStatement                                         | Adatvédelem                                                      |    |
| RegistrationDescription                                  | Az önkiszolgáló jelszó-regisztráció     |    |
| RegistrationMission                                      | Ha bármikor elfelejtené jelszavát, visszaállíthatja rajta saját kezűleg a segélyvonal hívása nélkül.   |      |
| RegistrationPageTitle                                    | A Forefront Identity Management – jelszó-regisztráció                                          |   |
| RegistrationSteps                                        | A regisztrációs folyamat megkezdéséhez kattintson a "Tovább gombra".   |      |
| RegistrationSuccessDescription                           | Most már regisztrálva van        |   |
| RegistrationSuccessTitle                                 | Befejeződött:   |    |
| RegistrationWelcomeTitle                                 | Jelszó-regisztrálási portál:    | |
| ResetDescription                                         | Önkiszolgáló jelszóváltoztatás  |    |
| ResetEnterNamePrompt                                     | Adja meg a felhasználó az alábbi     |     |
| ResetEnterPassword                                       | Adjon meg egy új jelszót:                                                  |   |
| ResetExample1                                            | Contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Példák:  |    |
| ResetPageTitle                                           | A Forefront Identity Management - jelszó alaphelyzetbe állítása       |     |
| ResetReenterPassword                                     | A jelszó újbóli megadására:    | |
| ResetSuccessDescription                                  | A jelszó alaphelyzetbe lett állítva.    |    |
| ResetSuccessTitle                                        | Sikerült:                                |    |
| ResetUseNewPassword                                      | Most már használhatja az új jelszót a bejelentkezéshez.     |      |
| ResetUsernameTextFormat                                  | (A következőhöz: {0} jelszó alaphelyzetbe állításával)       | {0} a felhasználói bejelentkezés             |
| ResetWelcomeTitle                                        | Jelszó alaphelyzetbe állítása:     |      |
| TroubleshootingEmailSubject                              | FIM-kérés feldolgozási hibájának részletei     |       |
| TroubleshootingLabelAttributes                           | Attribútumok:    |    |
| TroubleshootingLabelCloseButton                          | Bezárás       |    |
| TroubleshootingLabelCopyToClipboard                      | Másolja a vágólapra     |     |
| TroubleshootingLabelCorrelationId                        | Korrelációs azonosító:      |          |
| TroubleshootingLabelDetails                              | Részletek:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | Az adatokat a vágólapra másolta.           |    |
| TroubleshootingLabelRequestId                            | Kérelemazonosító:                  |    |
| TroubleshootingLabelSendEmail                            | Küldje el e-mailben                            |    |
| TroubleshootingLabelSource                               | OK:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Szolgáltatáskérés részleteinek megtekintése        |              |                                                    
| TroubleshootingLinkText                                  | Hibaelhárítási információk          |                  | |



A következő látható, amely testre szabható hitelesítési kapu karakterláncok listáját.

<a name="authentication-gate-strings"></a>Hitelesítési kapu karakterláncok
---------------------------

| Név                                          | Alapértelmezett érték                                                                                                                                                                                                                | Commnent |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | E-mail cím:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | Az e-mail cím mező nem lehet üres.     |          |
| OTPEmailRegistrationFooterReadOnly            | Az e-mail cím frissítéséhez kövesse a szervezet által meghatározott eljárást, vagy hívja a segélyszolgálatot.                   |          |
| OTPEmailRegistrationFooterReadWrite           | Az e-mail cím a szervezet a Forefront Identity Manager alkalmazásban tárolja.                       |          |
| OTPEmailRegistrationGateTitle                 | E-mail cím ellenőrzése   |          |
| OTPEmailRegistrationHeaderReadOnly            | Ha Ön a jelszó átállítására lenne szükség, ellenőrző biztonsági kódot kapnak az e-maileket. Ha az alább megjelenő e-mail cím nem megfelelő, akkor frissítheti az önkiszolgáló jelszóátállítás használatához. |          |
| OTPEmailRegistrationHeaderReadWrite           | Adja meg az alábbi e-mail címet. Ön a jelszó átállítására lenne szükség, ha egy megerősítési kódot kapnak az e-maileket.                 |          |
| OTPEmailResetGateTitle                        | Identitás ellenőrzése: E-mail         |          |
| OTPEmailResetHeader                           | Adja meg a biztonsági kódot alább. Egy biztonsági kódot a rendszer a szervezethez regisztrált e-mail címre küldte.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | A megadott érték nem egyezik a várt formátum.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | A biztonsági kód mező nem lehet üres.        |          |
| OTPResetVerificationLabel                     | Biztonsági kódot:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | A mobiltelefonszám frissítéséhez kövesse a szervezet által meghatározott eljárást, vagy hívja a segélyszolgálatot.   ||
| OTPSmsRegistrationFooterReadWrite                     | A mobiltelefonszámot a szervezet a Forefront Identity Manager alkalmazásban tárolja.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Mobiltelefonszám ellenőrzése                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | Ha Ön a jelszó átállítására lenne szükség, egy ellenőrző biztonsági kódot a mobiltelefonjára kapnak. Ha a lent látható módon mobiltelefonszám nem megfelelő, akkor frissítheti az önkiszolgáló jelszóátállítás használatához. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Adja meg az alábbi mobiltelefonjának számát. Ön a jelszó átállítására lenne szükség, ha egy megerősítési kódot a mobiltelefonjára kapnak.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | A mobiltelefonszám mező nem lehet üres.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Mobiltelefon:                    |   |
| OTPSmsResetGateTitle                                  | Az identitás ellenőrzése: mobiltelefon         |   |
| OTPSmsResetHeader                                     | Adja meg a biztonsági kódot alább. Egy biztonsági kódot a rendszer a szervezethez regisztrált mobiltelefonra küldte.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Adja meg az alábbi jelenlegi jelszavát, majd kattintson a "Tovább" gombra.        |   |
| PasswordGateErrorMessagePasswordRequired              | Adja meg jelenlegi jelszavát.                  |   |
| PasswordGateGateTitle                                 | A jelenlegi jelszavát               |   |
| PasswordGatePasswordLabelText                         | Jelszó:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(bejelentkezett, mint:`<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | Legalább válaszolnia kell {0} kérdéseket.        |   |
| QAGateIncorrectAnswer                                 | A válaszok helytelenek.       |   |
| QAGatePrivacyNotice                                   | A megadott válaszokat a szervezet a Forefront Identity Manager alkalmazásban tárolja.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Legalább válaszolnia kell regisztrálni {0} kérdéseket.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Egy vagy több válasz nem felel meg házirend.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | Ez a válasz nem felel meg házirend.      |   |
| QAGateRegistrationTitle                               | A válaszok regisztrálása         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | {0} a következő {1} kérdésre válaszolnia kell.   |   |
| QAGateResetTitle                                      | Az identitás ellenőrzése: A válaszok elküldése                                  |   |



## <a name="customizing-the-logo-banner"></a>Az embléma szalagcím testreszabása

Az alapértelmezett szalagcím a portál oldalain testre szabható a szervezet számára.
Az embléma szalagcím testreszabása:

1.  Az egyéni szalagok létrehozása, és mentheti azokat .png fájlként. A fájlok meg kell felelnie az alábbi javaslatokat:

 - Méret: 490 X 50 pixelt.

 - Bit mélysége: 32

2.  A fájlok másolása a \\minden portál, amely a testreszabni kívánt egyéni beállításokat mappájában.

3.  Hozzon létre egy Style.css fájlt minden mappában. Rendelkezik, mutasson a testreszabások mappa és az új embléma... Módosíthatja az embléma neve, ha van ilyen (azaz /Customizations/contosologo.png). A kód a következő hasonlóan kell kinéznie:

   **1. példa:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Ha az Internet Explorer 6.0 használ, akkor adjon meg egy másik nem átlátszó embléma, majd adja hozzá a következő kódot Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **2. példa:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Ha az Internet Explorer 6.0 használ, akkor adjon meg egy másik nem átlátszó embléma, majd adja hozzá a következő kódot Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Az okostelefonok lemezkép testreszabása

Testre szabhatja a kép okostelefonok használatával a következő. A SmartPhone lemezkép testreszabása:

1.  A lemezkép létrehozásához, és mentse őket .png fájlként. A fájlok meg kell felelnie az alábbi javaslatokat:

    - Méret: 190 X 50 pixelt.
    - Bit mélysége: 32

2.  A fájlok másolása a \\minden portál, amely a testreszabni kívánt egyéni beállításokat mappájában.

2.  Hozzon létre egy Style.css fájlt minden mappában. Rendelkezik, mutasson a testreszabások mappa és az új embléma... Módosíthatja az embléma neve, ha van ilyen (azaz /Customizations/contosologo.png). A kód a következő hasonlóan kell kinéznie:

  **1. példa:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>Stíluslap testreszabása

Testreszabott stíluslapok (CSS) használatával módosíthatja a elrendezés és a jelszó portálok stílusát. A testre szabott CSS használata:

1.  A testre szabott CSS-fájlok létrehozása, és mentheti azokat Style.css.

2.  A fájlok másolása a \\minden portál, amely a testreszabni kívánt egyéni beállításokat mappájában.

A következő egy Style.css fájlt egy egyszerű példa látható:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
Ahhoz, hogy felismerje a testreszabott módosításokat MIM újra kell indítani az IIS iisreset futtatásával.                                                                                                                                                                                       

A következő példája egy speciális egy Style.css fájlt. Ez a fájl smartphone és ipad adott kapcsolatos információkat biztosít a portálok megjelenítése az eszközön.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Testreszabási kapcsolatos gyakori hibák

Az alábbi táblázatban olvashat egy listát frissítéséhez. a FIM szolgáltatás és -portál jelentkező gyakori problémákat. Ezt a táblázatot is tartalmaz az a probléma megoldásához

|Probléma |Megoldás                                                                    |
|------|------------------------------|
| Egy karakterlánc testreszabása végzett, de az nem volt megtalálható a felhasználói felületen         | Strings.resources karakterlánc testreszabását minden esetben szükséges iisreset         |
| A strings.resources módosítások elvégzése után nem látható a karakterlánc módosításokat többé     | Strings.resources formátum valószínűleg helytelen formátumú, és ezért figyelmen kívül hagyja a portálon. Ellenőrizze az eseménynaplót a Windows-naplók – alkalmazás- és szolgáltatásnaplók – Forefront Identity Manager                        |
| A hozzáadott Style.css, először I nem talál a változtatásokat a portálon            | A Style.css fájlt alkalmazná első alkalommal végre kell hajtani az iisreset     |
| Új stílusok fel vagy módosíthatók Style.css, de módosítások nem látják a böngészőben      | Törölje a gyorsítótárban, és frissítse a lapot <br/> Ellenőrizze a CSS-szintaxis    |
| A path_to_sspr_portal CSS mappa tartalmát közvetlenül megváltozott\\css\\\*.css vagy a szalagcím emblémájának a path_to_sspr_portal\\képek\\fimlogo.png, ezek a változások, a frissítés megszakadt | Ezek a fájlok először soha ne módosítsa. Csak a testreszabási mappa eszközeként biztosításához használja a szalagcím emblémájának és CSS stílus testreszabások Style.css. Testreszabások mappa szándékosan nem írják fontosabb frissítés <br/><br/>Ne próbálja meg a portál szerelvényekben karakterláncok módosítása ILSpy és megfelelési segítségével. Használja a strings.resources alapértelmezett karakterláncok felülbírálására. A frissítés helyett a szerelvények  |
| Szalagcím emblémájának csak akkor jelenik meg a portálok / továbbra is látható a FIM-embléma     | A lemezkép nevét és az elérési utat a Style.css érvénytelen, vagy a gyorsítótárban nem lett törölve.          |
| Szalagcím emblémájának ugly IE6 a következőhöz       | Adjon meg nem átlátszó IE6 és egy különleges kísérő stílus style.css kell        |

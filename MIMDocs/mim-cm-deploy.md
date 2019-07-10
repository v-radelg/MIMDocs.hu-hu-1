---
title: A Microsoft Identity Manager Tanúsítványkezelő üzembe helyezése |} A Microsoft Docs
description: Telepítse a Microsoft Identity Manager 2016 tanúsítványkezelőben
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 9a9e00f7dca118627a5140967a104d13273cbc26
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690803"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>A Microsoft Identity Manager Tanúsítványkezelő 2016 (MIM CM) üzembe helyezése

A Microsoft Identity Manager Tanúsítványkezelő 2016 (MIM CM) a telepítés magában foglalja a több lépésből áll. Arra, hogy a folyamat egyszerűsítése érdekében azt vannak bontásához dolog. Előzetes lépések előtt tényleges MIM CM lépéseket kell tenni. Az előzetes munka nélkül a telepítés nagy eséllyel lesz sikertelen.

Az alábbi ábrán egy példa a használni kívánt környezet típusát mutatja. A rendszerek számokkal szerepelnek a következő lista az ábrán, és fejezze be a cikkben szereplő lépések szükségesek. Végül a rendszer Windows 2016 Datacenter kiszolgálókat használja:

![Környezet diagramja](media/mim-cm-deploy/image001.png)

1. A CORPDC-tartományvezérlő
2. CORPCM – a MIM CM-kiszolgáló
3. CORPCA – hitelesítésszolgáltató
4. CORPCMR – a MIM CM Rest API webes – Tanúsítványkezelő portálra a Rest API – használatos később
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10-tartományhoz

## <a name="deployment-overview"></a>Üzembe helyezés – áttekintés

- Alap operációs rendszer telepítése

    A tesztkörnyezet windows 2016 Datacenter kiszolgálók áll.

    >[!NOTE]
    >A további részleteket a MIM 2016 által támogatott platformok vessen egy pillantást című cikkben [a MIM 2016 által támogatott platformok](microsoft-identity-manager-2016-supported-platforms.md).

1. Központi telepítés előtti lépések

    - [A séma kiterjesztése](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Szolgáltatásfiókok létrehozása

    - [Tanúsítvány-sablonok létrehozása](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Configuring Kerberos

    - Adatbázissal kapcsolatos lépések

        - Az SQL konfigurációs követelményei

        - Adatbázis-engedélyek

2. Környezet

## <a name="pre-deployment-steps"></a>Központi telepítés előtti lépések

A MIM CM konfigurációs varázsló szükséges információkat biztosítanak ahhoz, hogy sikeresen befejeződik, meg kell adni.

![Diagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>A séma kiterjesztése

A séma kiterjesztésének folyamata nagyon egyszerű, de annak végleges jellege miatt körültekintően kell megközelíthető.

>[!NOTE]
>Ez a lépés szükséges, hogy a használt fiók rendelkezik-e a séma-rendszergazdai jogosultságokkal.

1. Keresse meg a MIM-adathordozó helyét, és navigáljon a \\tanúsítványkezelés\\x64 mappát.

2. A séma mappába másolja a corpdc-re, majd keresse meg a fájlt.

    ![Diagram](media/mim-cm-deploy/image005.png)

3. Futtassa a parancsfájl resourceForestModifySchema.vbs egyetlen erdőben forgatókönyv. Az erőforrás-erdő forgatókönyvhöz a szkriptek futtatására:
   - TartomanyA – a felhasználók található (userForestModifySchema.vbs)
   - ResourceForestB – (resourceForestModifySchema.vbs) CM telepítési helye.

     >[!NOTE]
     >Sémaváltozások egy egyirányú művelet, és egy erdőt igényel vissza helyreállítási ezért ellenőrizze, hogy a szükséges biztonsági másolatot. A séma szerint ez a művelet végrehajtása végzett módosításokat részleteiért tekintse át a cikk a [Forefront Identity Manager 2010 tanúsítvány felügyeleti sémaváltozások](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![Diagram](media/mim-cm-deploy/image007.png)

4. Futtassa a szkriptet, és meg kell kapnia a siker egyszer jelenik meg, hogy a parancsfájl futása befejeződött.

    ![Sikeres műveletről tájékoztató üzenet](media/mim-cm-deploy/image009.png)

Az Active Directory-séma már ki van bővítve a MIM CM támogatásához.

### <a name="creating-service-accounts-and-groups"></a>Szolgáltatás-fiókok és csoportok létrehozása

Az alábbi táblázat foglalja össze, a fiókok és a MIM CM szükséges engedélyek. Engedélyezheti, hogy a MIM CM automatikus létrehozása a következő fiókokat, vagy létrehozhatja őket a telepítés előtt. A tényleges fiókneveket módosítható. Ha fiókokat hozhat létre a saját magának, fontolja meg, a felhasználói fiókok olyan nyelvet a lenti listában, hogy könnyebbé vált a felel meg a felhasználói fiók nevét, a függvény elnevezési.

Felhasználók:

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Szerepkör**                   | **Felhasználói bejelentkezési név** |
|----------------------------|---------------------|
| A MIM CM-ügynök               | MIMCMAgent          |
| A MIM CM kulcs-helyreállítási megbízott  | MIMCMKRAgent        |
| A MIM CM hitelesítési ügynök | MIMCMAuthAgent      |
| A MIM CM hitelesítésszolgáltató Manager-ügynök    | MIMCMManagerAgent   |
| A MIM CM webes készlet ügynök      | MIMCMWebAgent       |
| A MIM CM tanúsítványigénylő ügynök    | MIMCMEnrollAgent    |
| A MIM CM frissítési szolgáltatás      | MIMCMService        |
| A MIM telepítési fiók        | MIMINSTALL          |
| Súgó az ügyfélszolgálati ügynök            | CMHelpdesk1-2       |
| CM-kezelő                 | CMManager1-2        |
| Előfizető felhasználó            | CMUser1-2           |

Csoportok:

| **Szerepkör**               | **Csoport**         |
|------------------------|-------------------|
| CM segélyszolgálat tagjai    | mimcm-ügyfélszolgálat    |
| CM Manager tagok     | MIMCM-Managers    |
| CM előfizető tagjai | MIMCM-előfizetők |

PowerShell: Az ügynök fiókok:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Frissítés **CORPCM** helyi házirend kiszolgálói ügynök-fiókok 

| **Felhasználói bejelentkezési név** | **Leírás és engedélyek**   |
|------|---------------------|
| MIMCMAgent          | A következő szolgáltatásokat nyújtja: </br>– Lekéri a hitelesítésszolgáltató titkos kulcsok titkosítva. </br>-Védi az intelligens kártya PIN-kód adatainak az FIM CM-adatbázisban. </br>– A FIM CM és a hitelesítésszolgáltató közötti kommunikáció védi. </br></br> A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal:</br>-   **Helyi bejelentkezés engedélyezése** felhasználói jogosultsággal.</br>-   **Tanúsítványok kiállítása és kezelése** felhasználói jogosultsággal. </br>-Olvasási és írási engedéllyel a rendszer Temp mappa a következő helyen: % WINDIR %\\Temp.</br>-A digitális aláírás és titkosítás tanúsítványt ki, és telepítve van a felhasználói tárolóba.
|MIMCMKRAgent        | Helyreáll a hitelesítésszolgáltató titkos kulcsok archivált. A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal:</br> -   **Helyi bejelentkezés engedélyezése** felhasználói jogosultsággal.</br>-A helyi csoporttagság **rendszergazdák** csoport. </br>-Regisztrálási az engedéllyel a **KeyRecoveryAgent** tanúsítványsablont. </br>-Kulcs helyreállítási ügynök tanúsítvány kiállítva és telepítve van a felhasználói tárolóba. A tanúsítvány hozzá kell adni a listához, a kulcs-helyreállítási ügynökök a hitelesítésszolgáltatón. </br>-Engedély írási és olvasási engedéllyel a rendszer Temp mappa a következő helyen található: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Meghatározza, hogy felhasználói jogok és engedélyek a felhasználókat és csoportokat. A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal: </br>-A csoporttagságot a Windows 2000 előtti rendszerrel kompatibilis hozzáférés tartományhoz. </br> -Nyújtott a **biztonsági naplózás létrehozása** felhasználói jogosultsággal.             |
| MIMCMManagerAgent   | CA felügyeleti tevékenységeket hajt végre. </br> Ez a felhasználó hozzá kell rendelni a hitelesítésszolgáltató kezelése engedély.        |
| MIMCMWebAgent       | Az identitást biztosít az IIS alkalmazáskészlethez. FIM CM fut Microsoft Win32® alkalmazás alkalmazásprogramozási felület folyamat, amely a felhasználói hitelesítő adatokat használ. </br> A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal:</br> -A helyi csoporttagság **IIS_WPG, a windows 2016 = IIS_IUSRS** csoport. </br>-A helyi csoporttagság **rendszergazdák** csoport.</br>-Nyújtott a **biztonsági naplózás létrehozása** felhasználói jogosultsággal. </br>-Nyújtott a **az operációs rendszer részeként való működés** felhasználói jogosultsággal. </br>-Nyújtott a **Folyamatszintű token lecserélése** felhasználói jogosultsággal.</br>-Az IIS-alkalmazáskészlet identitásának hozzárendelt **CLMAppPool**. </br>-Nyújtott olvasási jogosultságot a **HKEY_LOCAL_MACHINE\\szoftver\\Microsoft\\CLM\\1.0-s verziójú\\kiszolgáló\\WebUser** beállításkulcsot. </br>– Ezt a fiókot is delegálás kell lennie.|
| MIMCMEnrollAgent    | Regisztráció felhasználó nevében hajt végre. A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal:</br>-A tanúsítványigénylő ügynök tanúsítvány kibocsátott, és a felhasználói tárolóba telepítve.</br>-   **Helyi bejelentkezés engedélyezése** felhasználói jogosultsággal. </br>-Regisztrálási az engedéllyel a **tanúsítványigénylő megbízott** tanúsítványsablont (vagy az egyéni sablont, ha van).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Tanúsítványsablonok MIM CM szolgáltatásfiókok létrehozása

A MIM CM használt szolgáltatásfiókokat a hármat szükséges tanúsítvány, és a konfigurációs varázsló megköveteli, hogy Ön adja meg a tanúsítványok sablonokat kell tanúsítványt igényelni a számukra az nevét.

A szolgáltatásfiókok, amely tanúsítványt igényel a következők:

- MIMCMAgent: A fióknak rendelkeznie kell a felhasználói tanúsítvány

- MIMCMEnrollAgent: A fióknak rendelkeznie kell a tanúsítványigénylő ügynök tanúsítványával

- MIMCMKRAgent: A fióknak rendelkeznie kell egy **kulcs-helyreállítási megbízott** tanúsítvány

Vannak olyan sablonok, az ad-ben már jelen van, de a MIM Tanúsítványkezelő használata saját verziók létre kell hoznunk. Ahogy azt el kell végezni az eredeti sablonokból alapkonfiguráció módosítása.

A fenti fiókok mindhárom azzal emelt szintű jogosultságokkal a szervezeten belül, és kezelni.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>A MIM CM aláíró tanúsítvány sablonjának létrehozása

1. A **felügyeleti eszközök**, nyissa meg **hitelesítésszolgáltató**.

2. Az a **hitelesítésszolgáltató** konzolján, a konzolfán bontsa ki a **Contoso-CorpCA**, és kattintson a **tanúsítványsablonok**.

3. Kattintson a jobb gombbal **tanúsítványsablonok**, és kattintson a **kezelés**.

4. Az a **Tanúsítványsablonok konzolt**, a a **részletek** ablaktáblán válassza ki, kattintson a jobb gombbal **felhasználói**, és kattintson a **sablon megkettőzése** .

5. Az a **sablon megkettőzése** párbeszédpanelen jelölje ki **Windows Server 2003 Enterprise**, és kattintson a **OK**.

    ![Eredményül kapott módosítások megjelenítése](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >A MIM CM 3 tanúsítványsablonok alapuló tanúsítványok nem működik. Létre kell hoznia egy Windows Server® 2003 Enterprise (2. verzió) tanúsítványsablont. Lásd: [V3 részletek](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) további információt.

6. Az a **új sablon tulajdonságai** párbeszédpanel a **általános** lap a **sablon megjelenítendő neve** mezőbe írja be **MIM CM aláírási**. Módosítása a **érvényességi** való **2 évig**, és törölje a jelet a **tanúsítvány közzététele az Active Directory** jelölőnégyzetet.

7. Az a **kérelmek kezelése** lapra, győződjön meg arról, hogy a **titkos kulcs exportálható** jelölőnégyzet be van jelölve, és kattintson a **titkosítás lap**.

8. Az a **titkosítás kiválasztása** párbeszédpanelen letiltása **Microsoft Enhanced titkosításszolgáltató v1.0**, engedélyezze **Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató**, és kattintson a **OK**.

9. Az a **tulajdonosnévvel** lapon törölje a **e-mail név belefoglalása a tulajdonosnévbe** és **E-mail név** jelölőnégyzeteket.

10. Az a **bővítmények** lap a **a sablonban található bővítmények** listában, ellenőrizze, hogy **alkalmazás-házirendek** van kiválasztva, és kattintson **szerkesztése** .

11. Az a **használati szabályzatok bővítmény szerkesztése** párbeszédpanelen jelölje ki mindkét a **titkosított fájlrendszer** és a **biztonságos E-mail** alkalmazás-házirendek. Kattintson a **eltávolítása**, és kattintson a **OK**.

12. Az a **biztonsági** lapon tegye a következőket:

    - Távolítsa el **rendszergazda**.

    - Távolítsa el **Tartománygazdák**.

    - Távolítsa el **Tartományfelhasználók**.

    - Csak hozzárendelése **olvasási** és **írási** engedélyekkel **vállalati rendszergazdák**.

    - Adjon hozzá **MIMCMAgent.**

    - Rendelje hozzá **olvasási** és **beléptetés** engedélyekkel **MIMCMAgent**.

13. Az a **új sablon tulajdonságai** párbeszédpanelen kattintson a **OK**.

14. Hagyja a **Tanúsítványsablonok konzolt** megnyitásához.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>A MIM CM tanúsítványigénylő ügynök-tanúsítvány sablonjának létrehozása

1. Az a **Tanúsítványsablonok konzolt**, a a **részletek** ablaktáblán válassza ki, kattintson a jobb gombbal **tanúsítványigénylő megbízott**, és kattintson a **sablonmegkettőzése**.

2. Az a **sablon megkettőzése** párbeszédpanelen jelölje ki **Windows Server 2003 Enterprise**, és kattintson a **OK**.

3. Az a **új sablon tulajdonságai** párbeszédpanel a **általános** lap a **sablon megjelenítendő neve** mezőbe írja be **MIM CM tanúsítványigénylő megbízott**. Ügyeljen arra, hogy a **érvényességi** van **2 évig**.

4. Az a **kérelmek kezelése** fülre, engedélyezze **titkos kulcs exportálható**, és kattintson a **CSP-k vagy a titkosítás lap.**

5. Az a **CSP kiválasztása** párbeszédpanel letiltása **Microsoft Base titkosításszolgáltató v1.0**, tiltsa le **Microsoft Enhanced titkosításszolgáltató v1.0**, engedélyezése **A Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató**, és kattintson a **OK**.

6. Az a **biztonsági** lapon hajtsa végre a következőket:

    - Távolítsa el **rendszergazda**.

    - Távolítsa el **Tartománygazdák**.

    - Csak hozzárendelése **olvasási** és **írási** engedélyekkel **vállalati rendszergazdák**.

    - Adjon hozzá **MIMCMEnrollAgent**.

    - Rendelje hozzá **olvasási** és **beléptetés** engedélyekkel **MIMCMEnrollAgent**.

7. Az a **új sablon tulajdonságai** párbeszédpanelen kattintson a **OK**.

8. Hagyja a **Tanúsítványsablonok konzolt** megnyitásához.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>A MIM CM kulcs-helyreállítási megbízott-tanúsítvány sablonjának létrehozása

1. Az a **tanúsítványsablonok** konzoljának a **részletek** ablaktáblán válassza ki, kattintson a jobb gombbal **kulcs-helyreállítási megbízott**, és kattintson a **sablonmegkettőzése**.

2. Az a **sablon megkettőzése** párbeszédpanelen jelölje ki **Windows Server 2003 Enterprise**, és kattintson a **OK**.

3. Az a **új sablon tulajdonságai** párbeszédpanel a **általános** lap a **sablon megjelenítendő neve** mezőbe írja be **MIM CM kulcs-helyreállítási megbízott**. Ügyeljen arra, hogy a **érvényességi** van **2 évig** a a **titkosítás lap.**

4. Az a **szolgáltatók kiválasztása** párbeszédpanelen letiltása **Microsoft Enhanced titkosításszolgáltató v1.0**, engedélyezze **Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató**, majd **OK**.

5. Az a **feltételeitől** lapra, győződjön meg arról, hogy **hitelesítésszolgáltató tanúsítványkezelői jóváhagyást** van **le van tiltva**.

6. Az a **biztonsági** lapon hajtsa végre a következőket:

    - Távolítsa el **rendszergazda**.

    - Távolítsa el **Tartománygazdák**.

    - Csak hozzárendelése **olvasási** és **írási** engedélyekkel **vállalati rendszergazdák**.

    - Adjon hozzá **MIMCMKRAgent**.

    - Rendelje hozzá **olvasási** és **beléptetés** engedélyekkel **KRAgent**.

7. Az a **új sablon tulajdonságai** párbeszédpanelen kattintson a **OK**.

8. Zárja be a **Tanúsítvány-sablonok konzolt**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>A szükséges tanúsítvány-sablonok, a hitelesítésszolgáltató közzététele

1. Állítsa vissza a **hitelesítésszolgáltató** konzolon.

2. Az a **hitelesítésszolgáltató** konzol konzolfáján kattintson a jobb gombbal **tanúsítványsablonok**, mutasson a **új**, és kattintson a **tanúsítvány Tanúsítványsablon**.

3. Az a **tanúsítványsablonok engedélyezése** párbeszédpanelen jelölje ki **MIM CM tanúsítványigénylő megbízott**, **MIM CM kulcs-helyreállítási megbízott**, és **MIM CM aláírási**. Kattintson az **OK** gombra.

4. A konzolfán kattintson **tanúsítványsablonok**.

5. Ellenőrizze, hogy a három új sablonok megjelennek a **részletek** ablaktáblán, majd zárja be **hitelesítésszolgáltató**.

    ![A MIM CM aláírása](media/mim-cm-deploy/image016.png)

6. Zárjon be minden megnyitott ablakot, és jelentkezzen ki.

### <a name="iis-configuration"></a>IIS-konfiguráció

Annak érdekében, hogy a webhely üzemeltetése a CM, telepítse és konfigurálja az IIS.

#### <a name="install-and-configure-iis"></a>IIS telepítése és konfigurálása

1. Jelentkezzen be **a CORLog** , **MIMINSTALL** fiók

    >[!IMPORTANT]
    >A MIM-telepítési fiók helyi rendszergazdának kell lennie.

2. Nyissa meg a Powershellt, és futtassa a következő parancsot

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Alapértelmezett webhely nevű webhelyhez az IIS 7 alapértelmezés szerint telepítve van. Ha a hely átnevezte vagy nevű hely eltávolítása Default Web Site kell érhető el a MIM Tanúsítványkezelő telepítése előtt.

#### <a name="configuring-kerberos"></a>Configuring Kerberos

A MIMCMWebAgent fiókot fog futni a MIM Tanúsítványkezelő portálra. Az IIS-ben és az akár kernel alapértelmezés szerint módú hitelesítés alapértelmezés szerint használatos IIS-ben. Tiltsa le a Kerberos rendszermag módú hitelesítést, és SPN-ek konfigurálása a MIMCMWebAgent fiók helyett. Néhány parancsot emelt szintű engedélyekkel az active Directoryban és CORPCM kiszolgáló szükséges.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**IIS a CORPCM frissítése**

![Diagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Adja hozzá a "cm.contoso.com" A DNS-rekordját, és mutasson CORPCM IP kell

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>A MIM CM-portálon SSL megkövetelése

Azt javasoljuk, hogy szükséges-e az SSL a MIM CM-portálon. Ha még nem a varázsló fogja figyelmeztetni azt.

1. A webalkalmazás tanúsítvány regisztrálása **cm.contoso.com** alapértelmezett hely hozzárendelése

2. Nyissa meg **IIS-kezelő** , és keresse meg **tanúsítványkezelés**

3. Szolgáltatások nézetben kattintson duplán az SSL-beállítások.

4. Az SSL-beállítások oldalán válassza **SSL megkövetelése**.

5. Kattintson a Műveletek ablaktáblában **alkalmaz.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Adatbázis-konfigurációs **CORPSQL** a MIM Tanúsítványkezelő

1. Győződjön meg arról, hogy a CORPSQL01 kiszolgálóhoz csatlakozott.

2. Győződjön meg arról, mint az SQL-adatbázis van bejelentkezve.

3. Futtassa a következő T-SQL-parancsfájlt, hogy a CONTOSO\\MIMINSTALL fiókot létrehozni az adatbázist, akkor a konfigurációs lépés

    >[!NOTE]
    >Térjen vissza az SQL-Ha a kilépési & házirend modul készen állunk kell

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![A MIM CM konfigurációs varázsló hibaüzenet](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>A Microsoft Identity Manager 2016 Tanúsítványkezelő telepítése

1. Győződjön meg arról, hogy csatlakozott a CORPCM kiszolgálón, és hogy a **MIMINSTALL** fiók tagja a **a helyi rendszergazdák** csoport.

2. Győződjön meg arról, be vannak jelentkezve Contoso\\MIMINSTALL.

3. Csatlakoztassa a Microsoft Identity Manager SP1 ISO.

4. **Nyissa meg** a **tanúsítványkezelés\\x64** könyvtár.

5. Az a **x64** ablakban kattintson a jobb gombbal **telepítő**, és kattintson a **Futtatás rendszergazdaként**.

6. Üdvözli a Microsoft Identity Manager tanúsítvány felügyeleti telepítővarázsló lapra, kattintson a **tovább.**

7. A végfelhasználói licencszerződés oldalon olvassa el a szerződést, engedélyezze a elfogadom a licencszerződés feltételeit **jelölőnégyzet**, majd kattintson a Tovább gombra.

8. Egyéni beállítása lapon győződjön meg arról, a **MIM Tanúsítványkezelő portálra** és **MIM CM frissítési szolgáltatás-összetevők** , telepítve vannak beállítva, majd **kattintson a Tovább gombra**.

9. A virtuális webhely mappa lapon győződjön meg arról, hogy a virtuális mappa neve **CertificateManagement**, majd **kattintson a Tovább gombra**.

10. Telepítse a Microsoft Identity Manager Tanúsítványkezelő lapon **kattintson a telepítés**.

11. Az a **befejezve** a Microsoft Identity Manager tanúsítvány felügyeleti telepítővarázsló lap **kattintson a Befejezés**.

![A MIM CM varázsló befejeződött](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>A Microsoft Identity Manager 2016 tanúsítványkezelés konfigurációs varázsló

Vegyen fel MIMINSTALL, mielőtt bejelentkezne abba CORPCM **tartományi rendszergazdák, a Sémagazdák és a helyi rendszergazdák** csoport konfigurációs varázsló. Konfigurálás elvégzését követően ez később lehet eltávolítani.

![Hibaüzenet](media/mim-cm-deploy/image028.png)

1. Az a **Start** menüben kattintson a **tanúsítvány konfigurációs varázsló**. A futtató és **rendszergazda**

2. Az a **üdvözli a konfigurációs varázsló** kattintson **tovább**.

3. Az a **hitelesítésszolgáltató konfigurációjának** lapon, győződjön meg arról, hogy a kiválasztott hitelesítésszolgáltató **Contoso-CORPCA-CA**, győződjön meg arról, hogy a kijelölt kiszolgáló **CORPCA. A CONTOSO.COM**, és kattintson a **tovább**.

4. Az a **beállítása a Microsoft® SQL Server® adatbázisban** lap a **nevét az SQL Server** mezőbe írja be **CORPSQL1** , engedélyezése a **hozhat létre a hitelesítő adatok használata a adatbázis** jelölőnégyzetet, majd kattintson a **tovább**.

5. A a **adatbázis-beállítások** lap, fogadja el az alapértelmezett adatbázis **FIMCertificateManagement**, ügyeljen arra, hogy **integrált SQL-hitelesítés** van kiválasztva, majd Kattintson a **tovább**.

6. Az a **Active Directory beállítása** lapon fogadja el az alapértelmezett nevet, a szolgáltatáskapcsolódási pont foglalt, és kattintson a **tovább**.

7. Az a **hitelesítési módszer** lapon erősítse meg, **windows beépített hitelesítés** van kiválasztva, majd kattintson a **tovább**.

8. Az a **ügynökök – FIM CM** lapon törölje a jelet a **az FIM CM alapértelmezett beállítások használata** jelölőnégyzetet, majd kattintson a **egyéni fiókok**.

9. Az a **ügynökök – FIM CM** többlapos párbeszédpanelen, az egyes lapokon írja be a következő információkat:

   - Felhasználónév: **Frissítés**

   - Jelszó: **Adja át\@word1**

   - Jelszó megerősítése: **Adja át\@word1**

   - Használjon egy meglévő felhasználó: **Engedélyezve van**

     >[!NOTE]
     >Korábban létrehozott ezeket a fiókokat. Győződjön meg arról, hogy az eljárások a 8. lépés az összes hat ügynök fiók lap megismétlődnek.

     ![A MIM CM-fiókok](media/mim-cm-deploy/image030.png)

10. Az összes ügynök fiókadatok befejeződése után kattintson a **OK**.

11. Az a **ügynökök – a MIM CM** kattintson **tovább**.

12. Az a **állítsa be a kiszolgálói tanúsítványok** lapon, a következő tanúsítványsablonok engedélyezése:

    - A helyreállítási kulcs-helyreállítási megbízott ügynöktanúsítványa használandó tanúsítványsablon: **MIMCMKeyRecoveryAgent**.

    - Tanúsítványsablon a FIM CM-ügynök tanúsítvány használható: **MIMCMSigning**.

    - Tanúsítványsablon a tanúsítványigénylő ügynök tanúsítványával használható: **FIMCMEnrollmentAgent**.

13. Az a **beállítás kiszolgálói tanúsítványok** kattintson **tovább**.

14. Az a **telepítő E-mail kiszolgáló, a dokumentum nyomtatása** lap a **adja meg az e-mail értesítéseket a regisztrációs használni kívánt SMTP-kiszolgáló nevét** mezőbe, majd kattintson a **tovább.**

15. Az a **konfigurálásra kész** kattintson **konfigurálása**.

16. Az a **konfigurációs varázsló – Microsoft Forefront Identity Manager 2010 R2** figyelmeztető párbeszédpanel, kattintson a **OK** gombra annak megerősítéséhez, hogy az SSL nincs engedélyezve az IIS virtuális könyvtár.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >A konfigurációs varázsló végrehajtása befejezéséig ne kattintson a Befejezés gombra. Naplózás a varázslóban itt találja: **% programfiles %\\Microsoft Forefront Identity Management\\2010\\tanúsítványkezelés\\config.log**

17. Kattintson a **Befejezés**gombra.

    ![A MIM CM varázsló befejeződött](media/mim-cm-deploy/image033.png)

18. Zárjon be minden megnyitott windows.

19. Adjon hozzá `https://cm.contoso.com/certificatemanagement` a böngésző helyi intranet zónához.

20. Keresse fel a webhelyet CORPCM kiszolgálóról `https://cm.contoso.com/certificatemanagement`  

    ![Diagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Ellenőrizze a CNG-kulcs elkülönítése szolgáltatás

1. A **felügyeleti eszközök**, nyissa meg **szolgáltatások**.

2. Az a **részletek** ablaktáblán kattintson duplán a **CNG-kulcs elkülönítése**.

3. Az a **általános** fülre, módosítsa a **indítási típus** való **automatikus**.

4. Az a **általános** fülre, indítsa el a szolgáltatást, ha nem egy elindított állapotú.

5. Az a **általános** lapra, majd **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Telepítése, és a CA-modulok beállítása:

Ebben a lépésben azt telepíti és a FIM CM hitelesítésszolgáltató modulok konfigurálása a hitelesítésszolgáltatónál.

1. FIM CM csak vizsgálhatja meg a felügyeleti műveletek vonatkozó felhasználói engedélyek konfigurálása

2. Az a **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\tanúsítványkezelés\\webes** ablakban másolata legyen  **Web.config** a másolat elnevezési **web.1.config**.

3. Az a **webes** ablakban kattintson a jobb gombbal **Web.config**, és kattintson a **nyílt**.

    >[!Note]
    >A Web.config fájl megnyitása a Jegyzettömbben

4. Ha a fájl megnyílik, nyomja le a CTRL + F.

5. Az a **keresése és cseréje** párbeszédpanel a **keresett** mezőbe írja be **UseUser**, és kattintson a **a következő** három alkalommal.

6. Zárja be a **keresés és csere** párbeszédpanel bezárásához.

7. A sorban kell  **\<key="Clm.RequestSecurity.Flags hozzáadása" érték "UseUser UseGroups" = /\>** . Módosítsa a sort beolvasni  **\<key="Clm.RequestSecurity.Flags hozzáadása" érték "UseUser" = /\>** .

8. Zárja be a fájlt, az összes módosításainak mentése folyamatban van.

9. Hozzon létre egy fiókot a hitelesítésszolgáltató számítógép az SQL server \<parancsfájl nélkül\>

10. Győződjön meg arról, hogy csatlakozik az **CORPSQL01** kiszolgáló.

11. Győződjön meg arról, be vannak jelentkezve **DBA**

12. Az a **Start** indítsa el a menü **SQL Server Management Studio**.

13. Az a **kapcsolódás a kiszolgálóhoz** párbeszédpanel a **kiszolgálónév** mezőbe írja be **CORPSQL01,** majd **Connect**.

14. A konzolfán bontsa ki **biztonsági**, és kattintson a **bejelentkezések**.

15. Kattintson a jobb gombbal **bejelentkezések**, és kattintson a **új bejelentkezés**.

16. Az a **általános** lap a **bejelentkezési név** mezőbe írja be **contoso\\CORPCA\$** . Válassza ki **Windows-hitelesítés**. Alapértelmezett adatbázis **FIMCertificateManagement**.

17. A bal oldali panelen válassza ki a **Felhasználóleképezés**. Kattintson a jobb oldali ablaktáblában található jelölőnégyzetek bejelölésével a **térkép** oszlop melletti **FIMCertificateManagement**. Az a **adatbázis-szerepköri tagság a következőhöz: FIMCertificateManagement** listázása, engedélyezze a **clmApp** szerepkör.

18. Kattintson az **OK** gombra.

19. Bezárás **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>A hitelesítésszolgáltató FIM CM-modulok telepítéséhez a hitelesítésszolgáltatónál

1. Győződjön meg arról, hogy csatlakozik az **CORPCA** kiszolgáló.

2. Az a **X64** windows, kattintson a jobb gombbal **Setup.exe**, és kattintson a **Futtatás rendszergazdaként**.

3. Az a **üdvözli a Microsoft Identity Manager tanúsítvány felügyeleti telepítővarázsló** kattintson **tovább**.

4. Az a **végfelhasználói licencszerződés** oldalon olvassa el a szerződést. Válassza ki a **elfogadom a licencszerződés feltételeit** jelölőnégyzetet, majd kattintson a **tovább**.

5. Az a **egyéni beállítása** lapon jelölje be **MIM Tanúsítványkezelő portálra**, és kattintson a **Ez a funkció nem lesz elérhető**.

6. Az a **egyéni telepítés** lapon jelölje be **MIM CM frissítési szolgáltatás**, és kattintson a **Ez a funkció nem lesz elérhető**.

    >[!Note]
    >Ez a MIM CM hitelesítésszolgáltató fájlok hagyja az egyetlen szolgáltatás, a telepítés engedélyezve van.

7. Az a **egyéni telepítés** kattintson **tovább**.

8. Az a **telepítse a Microsoft Identity Manager Tanúsítványkezelő** kattintson **telepítése**.

9. Az a **a Microsoft Identity Manager tanúsítvány felügyeleti telepítővarázslójának befejezése** kattintson **Befejezés**.

10. Zárjon be minden megnyitott windows.

### <a name="configure-the-mim-cm-exit-module"></a>A MIM CM kilépési modul konfigurálása

1. A **felügyeleti eszközök**, nyissa meg **hitelesítésszolgáltató**.

2. A konzolfán kattintson a jobb gombbal **contoso-CORPCA-CA**, és kattintson a **tulajdonságok**.

3. Az a **kilépési modul** lapon jelölje be **FIM CM kilépési modul**, és kattintson a **tulajdonságok**.

4. Az a **adja meg a CM adatbázis-kapcsolati karakterlánc** mezőbe írja be **Connect Timeout = 15. Biztonsági információ megőrzése = True; Integrated Security = sspi; Initial Catalog = FIMCertificateManagement; adatforrás = CORPSQL01**. Hagyja a **a kapcsolati karakterlánc titkosítása** engedélyezve jelölőnégyzetet, és kattintson a **OK**.
5. Az a **Microsoft FIM tanúsítványkezelés** üzenet mezőbe, kattintson **OK**.

6. Az a **contoso-CORPCA-hitelesítésszolgáltató-tulajdonságok** párbeszédpanelen kattintson a **OK**.

7. Kattintson a jobb gombbal **contoso-CORPCA-CA** *,* mutasson **feladatok**, és kattintson a **szolgáltatás leállítása**. Várjon, amíg az Active Directory tanúsítványszolgáltatás leáll.

8. Kattintson a jobb gombbal **contoso-CORPCA-CA** *,* mutasson **feladatok**, és kattintson a **szolgáltatás indítása**.

9. Minimalizálja a **hitelesítésszolgáltató** konzolon.

10. A **felügyeleti eszközök**, nyissa meg **Eseménynapló**.

11. A konzolfán bontsa ki **alkalmazás- és szolgáltatásnaplók**, és kattintson a **FIM tanúsítványkezelés**.

12. Események listájában, győződjön meg arról, hogy nincs-e a legújabb események *nem* tartalmazza **figyelmeztetés** vagy **hiba** események a tanúsítványszolgáltatások az utolsó újraindítás óta.

    >[!NOTE] 
    >Az utolsó esemény tartalmaznia kell, hogy a kilépési modul betöltése a beállítások használatával: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimalizálja a **Eseménynapló**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>A MIMCMAgent tanúsítvány ujjlenyomata Windows® vágólapra másolása

1. Állítsa vissza a **hitelesítésszolgáltató** konzolon.

2. A konzolfán bontsa ki **contoso-CORPCA-CA**, és kattintson a **kiállított tanúsítványok**.

3. Az a **részletek** ablaktáblán kattintson duplán a tanúsítványra a **CONTOSO\\MIMCMAgent** a a **kérelmező neve** oszlop és az **FIM CM Aláírási** a a **tanúsítványsablon** oszlop.

4. Az a **részletek** lapon jelölje be a **ujjlenyomat** mező.

5. Válassza ki az ujjlenyomatot, majd nyomja le a CTRL + C billentyűkombinációt.

    >[!NOTE]
    >Tegye **nem** tartalmazza a vezető területet ujjlenyomat-karakterek listájában.

6. Az a **tanúsítvány** párbeszédpanelen kattintson a **OK**.

7. A a **Start** menü, a a **Keresés programokban és fájlokban** mezőbe írja be **Jegyzettömb**, és nyomja le az ENTER BILLENTYŰT.

8. A **Jegyzettömb**, az a **szerkesztése** menüben kattintson a **beillesztési**.

9. Az a **szerkesztése** menüben kattintson a **cseréje**.

10. Az a **keresett** mezőbe, írja be egy szóközt, és kattintson **cserélje le az összes**.

    >[!Note]
    >Ez eltávolítja az összes a tárolóhelyek az ujjlenyomatot a karakter közötti lehet.

11. Az a **cserélje le** párbeszédpanelen kattintson a **Mégse**.

12. Válassza ki a konvertált *thumbprintstring*, majd nyomja le a CTRL + C billentyűt.

13. Bezárás **Jegyzettömb** módosítások mentése nélkül.

### <a name="configure-the-fim-cm-policy-module"></a>A FIM CM szabályzatmodul konfigurálása

1. Állítsa vissza a **hitelesítésszolgáltató** konzolon.

2. Kattintson a jobb gombbal **contoso-CORPCA-CA**, és kattintson a **tulajdonságok**.

3. Az a **contoso-CORPCA-hitelesítésszolgáltató-tulajdonságok** párbeszédpanel a **házirendmodul** lapra, majd **tulajdonságok**.

    - Az a **általános** lapra, győződjön meg arról, hogy **FIM CM kéréseket továbbíthat az alapértelmezett házirend modulját a feldolgozáshoz** van kiválasztva.

    - Az a **aláíró tanúsítványok** lapra, majd **Hozzáadás**.

    - A tanúsítvány párbeszédpanelen kattintson a jobb gombbal a **adja meg hexadecimális kódolású tanúsítvány kivonata** mezőbe, majd kattintson a **beillesztési**.

    - Az a **tanúsítvány** párbeszédpanelen kattintson a **OK**.

        >[!Note]
        >Ha a **OK** gomb nincs engedélyezve, akkor véletlenül szerepel egy rejtett karakter az ujjlenyomat karakterláncát clmAgent tanúsítvány ujjlenyomata másolása során. Ismételje meg a kezdő lépéseket **4. feladat: Másolja a MIMCMAgent tanúsítvány ujjlenyomata Windows vágólapra** ebben a gyakorlatban.

4. Az a **konfigurációs tulajdonságok** párbeszédpanelen ellenőrizze, hogy, hogy az ujjlenyomat megjelenik a **érvényes aláíró tanúsítványok** listában, és kattintson **OK**.

5. Az a **FIM tanúsítványkezelés** üzenet mezőbe, kattintson **OK**.

6. Az a **contoso-CORPCA-hitelesítésszolgáltató-tulajdonságok** párbeszédpanelen kattintson a **OK**.

7. Kattintson a jobb gombbal **contoso-CORPCA-CA** *,* mutasson **feladatok**, és kattintson a **szolgáltatás leállítása**.

8. Várjon, amíg az Active Directory tanúsítványszolgáltatás leáll.

9. Kattintson a jobb gombbal **contoso-CORPCA-CA** *,* mutasson **feladatok**, és kattintson a **szolgáltatás indítása**.

10. Zárja be a **hitelesítésszolgáltató** konzolon.

11. Zárjon be minden megnyitott ablakot, és kilép.

**A központi telepítésben az utolsó lépés** , szeretnénk biztosítani, hogy a CONTOSO\\MIMCM-kezelők telepítheti és sablonok létrehozása és konfigurálása a rendszer anélkül, hogy a séma- és tartományi rendszergazdák. A következő szkriptet fogja ACL a dsacls használt tanúsítványsablonok engedélyeket. Futtassa a fiókkal, amely biztonsági módosításához szükséges teljes körű jogosultsággal rendelkezik olvasási és írási engedélyek az erdő minden meglévő tanúsítványsablont.

Első lépések: **Szolgáltatáskapcsolódási pont és a cél biztonságicsoport-engedélyeit konfigurálásával & sablon Tanúsítványprofil-kezelésének delegálása**

1. Engedélyek konfigurálása a szolgáltatáskapcsolódási pont (SCP).

2. Delegált profilkezelés sablon konfigurálása.

3. Engedélyek konfigurálása a szolgáltatáskapcsolódási pont (SCP). **\<parancsfájl nélkül\>**

4.   Győződjön meg arról, hogy csatlakozik az **CORPDC** virtuális kiszolgáló.

5. Jelentkezzen be **contoso\\corpadmin**

6. A **felügyeleti eszközök**, nyissa meg **Active Directory – felhasználók és számítógépek**.

7. A **Active Directory – felhasználók és számítógépek**, az a **nézet** menü, ügyeljen arra, hogy **speciális funkciók** engedélyezve van.

8. A konzolfán bontsa ki **Contoso.com** \| **rendszer** \| **Microsoft** \| **tanúsítvány-életciklus Kezelő**, és kattintson a **CORPCM**.

9. Kattintson a jobb gombbal **CORPCM**, és kattintson a **tulajdonságok**.

10. Az a **CORPCM tulajdonságok** párbeszédpanel a **biztonsági** lapon vegye fel a következő csoportok a megfelelő engedélyekkel:

    | Csoport          | Engedélyek      |
    |----------------|------------------|
    | mimcm-Managers | Olvasás </br> FIM CM naplózása</br> FIM CM tanúsítványigénylő ügynök</br> FIM CM kérelem regisztrálása</br> FIM CM-kérelem helyreállítása</br> FIM CM kérelem megújítása</br> FIM CM-kérelem visszavonása </br> FIM CM kérelem intelligens kártya letiltásának feloldása |
    | mimcm-HelpDesk | Olvasás</br> FIM CM tanúsítványigénylő ügynök</br> FIM CM-kérelem visszavonása</br> FIM CM kérelem intelligens kártya letiltásának feloldása |

11. Az a **CORPDC tulajdonságok** párbeszédpanelen kattintson a **OK**.

12. Hagyja **Active Directory – felhasználók és számítógépek** megnyitásához.

**Engedélyek konfigurálása a následnické a felhasználói objektumok**

1. Ellenőrizze, hogy továbbra is a **Active Directory – felhasználók és számítógépek** konzolon.

2. A konzolfán kattintson a jobb gombbal **Contoso.com**, és kattintson a **tulajdonságok**.

3. Az a **biztonsági** lapra, majd **speciális**.

4. Az a **Contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás**.

5. Az a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **írja be a kijelölendő objektum nevét** mezőbe írja be **mimcm-kezelők**, és kattintson a **OK**.

6. Az a **engedélybejegyzés a Contoso** párbeszédpanel a **a alkalmazni** listáról válassza ki **leszármazott felhasználó objektumai** , majd engedélyezze a **engedélyezése**jelölje be a következő **engedélyek**:

    - **Az összes tulajdonság olvasása**

    - **Olvasási engedélyek**

    - **FIM CM naplózása**

    - **FIM CM tanúsítványigénylő ügynök**

    - **FIM CM kérelem regisztrálása**

    - **FIM CM-kérelem helyreállítása**

    - **FIM CM kérelem megújítása**

    - **FIM CM-kérelem visszavonása**

    - **FIM CM kérelem intelligens kártya letiltásának feloldása**

7. Az a **engedélybejegyzés a Contoso** párbeszédpanelen kattintson a **OK**.

8. Az a **Contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás**.

9. A a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **írja be a kijelölendő objektum nevét** mezőbe írja be **mimcm-segélyszolgálat**, és kattintson a **OK**.

10. Az a **engedélybejegyzés a Contoso** párbeszédpanel a **a alkalmazni** listáról válassza ki **leszármazott felhasználó objektumai** majd válassza ki a **engedélyezése**jelölje be a következő **engedélyek**:

    - **Az összes tulajdonság olvasása**

    - **Olvasási engedélyek**

    - **FIM CM tanúsítványigénylő ügynök**

    - **FIM CM-kérelem visszavonása**

    - **FIM CM kérelem intelligens kártya letiltásának feloldása**

11. Az a **engedélybejegyzés a Contoso** párbeszédpanelen kattintson a **OK**.

12. Az a **Contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **OK**.

13. Az a **contoso.com tulajdonságok** párbeszédpanelen kattintson a **OK**.

14. Hagyja **Active Directory – felhasználók és számítógépek** megnyitásához.

**Engedélyek konfigurálása a následnické a felhasználói objektumok \<parancsfájl nélkül\>**

1. Ellenőrizze, hogy továbbra is a **Active Directory – felhasználók és számítógépek** konzolon.

2. A konzolfán kattintson a jobb gombbal **Contoso.com**, és kattintson a **tulajdonságok**.

3. Az a **biztonsági** lapra, majd **speciális**.

4. Az a **Contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás**.

5. Az a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **írja be a kijelölendő objektum nevét** mezőbe írja be **mimcm-kezelők**, és kattintson a **OK**.

6. Az a **engedélybejegyzés a CONTOSO** párbeszédpanel a **a alkalmazni** listáról válassza ki **leszármazott felhasználó objektumai** , majd engedélyezze a **engedélyezése**jelölje be a következő **engedélyek**:

    - **Az összes tulajdonság olvasása**

    - **Olvasási engedélyek**

    - **FIM CM naplózása**

    - **FIM CM tanúsítványigénylő ügynök**

    - **FIM CM kérelem regisztrálása**

    - **FIM CM-kérelem helyreállítása**

    - **FIM CM kérelem megújítása**

    - **FIM CM-kérelem visszavonása**

    - **FIM CM kérelem intelligens kártya letiltásának feloldása**

7. Az a **engedélybejegyzés a CONTOSO** párbeszédpanelen kattintson a **OK**.

8. Az a **CONTOSO speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás**.

9. A a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **írja be a kijelölendő objektum nevét** mezőbe írja be **mimcm-segélyszolgálat**, és kattintson a **OK**.

10. Az a **engedélybejegyzés a CONTOSO** párbeszédpanel a **a alkalmazni** listáról válassza ki **leszármazott felhasználó objektumai** majd válassza ki a **engedélyezése**jelölje be a következő **engedélyek**:

    - **Az összes tulajdonság olvasása**

    - **Olvasási engedélyek**

    - **FIM CM tanúsítványigénylő ügynök**

    - **FIM CM-kérelem visszavonása**

    - **FIM CM kérelem intelligens kártya letiltásának feloldása**

11. Az a **engedélybejegyzés a contoso** párbeszédpanelen kattintson a **OK**.

12. Az a **Contoso speciális biztonsági beállításai** párbeszédpanelen kattintson a **OK**.

13. Az a **contoso.com tulajdonságok** párbeszédpanelen kattintson a **OK**.

14. Hagyja **Active Directory – felhasználók és számítógépek** megnyitásához.

Második lépéseket: **Tanúsítványsablonok felügyeleti engedélyeinek delegálása \<parancsfájl\>**

- A tanúsítványsablonok tároló-engedélyek delegálása.

- Az OID-tárolóra vonatkozó engedélyek delegálása.

- A meglévő tanúsítványsablonok-engedélyek delegálása.

A tanúsítványsablonok tároló vonatkozó engedélyek meghatározásához:

1. Állítsa vissza a **Active Directory – helyek és szolgáltatások** konzolon.

2. A konzolfán bontsa ki **szolgáltatások**, bontsa ki a **nyilvánoskulcs-szolgáltatások**, és kattintson a **tanúsítványsablonok**.

3. A konzolfán kattintson a jobb gombbal **tanúsítványsablonok**, és kattintson a **Vezérlés delegálása**.

4. Az a **Vezérlés delegálása** varázslóban kattintson **tovább**.

5. Az a **felhasználók vagy csoportok** kattintson **Hozzáadás**.

6. Az a **válassza ki a felhasználók, számítógépek vagy csoportok** párbeszédpanel a **írja be a kijelölendő objektumok nevét** mezőbe írja be **mimcm-kezelők**, és kattintson a **OK**.

7. Az a **felhasználók vagy csoportok** kattintson **tovább**.

8. Az a **Delegálandó feladatok** kattintson **hozzon létre egy egyéni feladat**, és kattintson a **tovább**.

9.  Az a **Active Directory objektumtípus** lapon, győződjön meg arról, hogy **ebben a mappában, ez a mappa, és ez a mappa az új objektumok létrehozásának a meglévő objektumok** van kiválasztva, és kattintson **következő**.

10. Az a **engedélyek** lap a **engedélyek** listáról válassza ki a **teljes hozzáférés** jelölőnégyzetet, majd kattintson a **tovább**.

11. Az a **a Vezérlés delegálása varázsló befejezése** kattintson **Befejezés**.

Engedélyek megadása az OID tárolón:

1. A konzolfán kattintson a jobb gombbal **OID**, és kattintson a **tulajdonságok**.

2. Az a **OID tulajdonságok** párbeszédpanel a **biztonsági** lapra, majd **speciális**.

3. Az a **OID speciális biztonsági beállításai** párbeszédpanelen kattintson a **Hozzáadás**.

4. Az a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **írja be a kijelölendő objektum nevét** mezőbe írja be **mimcm-kezelők**, és kattintson a **OK**.

5. Az a **engedélybejegyzés az OID** párbeszédpanelen ellenőrizze, hogy, hogy az engedélyek vonatkoznak **Ez az objektum és a gyermekobjektumok**, kattintson a **teljes hozzáférés**, majd  **OK**.

6. Az a **OID speciális biztonsági beállításai** párbeszédpanelen kattintson a **OK**.

7. Az a **OID tulajdonságok** párbeszédpanelen kattintson a **OK**.

8. Bezárás **az Active Directory – helyek és szolgáltatások**.

**Parancsfájlok: Az Objektumazonosító, profilsablon & tanúsítványsablonok tároló engedélyeit**

![Diagram](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Parancsfájlok: A meglévő tanúsítványsablonok-engedélyek delegálása.**  

![Diagram](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```

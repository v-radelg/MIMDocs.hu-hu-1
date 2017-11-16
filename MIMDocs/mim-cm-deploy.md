---
title: "Központi telepítése a Microsoft Identity Manager Tanúsítványkezelő |} Microsoft Docs"
description: "Telepítse a Microsoft Identity Manager 2016 tanúsítványkezelőben"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/19/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 2473ef1c3d6fc5350d60d81bd508296a33343f01
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/14/2017
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>A Microsoft Identity Manager Tanúsítványkezelő 2016 (MIM CM) üzembe helyezése

A Microsoft Identity Manager Tanúsítványkezelő 2016 (MIM CM) telepítése magában foglalja a több lépésből áll. Így a folyamat leegyszerűsítése érdekében azt bontja dolgot. Előzetes lépések előtt minden tényleges MIM CM lépést kell tenni. Az előzetes munka nélkül a telepítés valószínű, hogy sikertelen lesz. 

1. Telepítésének áttekintése
2. Központi telepítés előtti lépések
3. Milyen hiba?

Az alábbi ábra azt szemlélteti, amelyeket felhasználhatunk környezet típusú. A rendszer a számokból szerepelnek a diagram az alábbi listából, és fejezze be a cikkben szereplő lépések szükségesek. Végezetül Windows 2016 Datacenter kiszolgálókat használnak:

![Környezet ábrája](media/mim-cm-deploy/image001.png)

1. CORPDC – tartományvezérlő
2. CORPCM – a MIM CM-kiszolgáló
3. CORPCA – hitelesítésszolgáltató
4. CORPCMR – a MIM Tanúsítványkezelő Rest API webes – CM Portal a Rest API – használható későbbi használatra
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10-tartományhoz

## <a name="deployment-overview"></a>Telepítésének áttekintése

- Alap operációs rendszer telepítése
  - A tesztlabor a windows 2016 Datacenter kiszolgálókból áll.
       >[!NOTE]
Az a MIM 2016 által támogatott platformok vonatkozó részletes információért tekintse meg a című cikk [a MIM 2016 által támogatott platformok](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)
- Központi telepítés előtti lépések
  - [A séma kiterjesztése](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)
  - Szolgáltatásfiókok létrehozása
  - [Tanúsítványsablonok létrehozása](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)
  - IIS
  - A Kerberos konfigurálása
  - Adatbázissal kapcsolatos lépéseket
    - SQL-konfigurációs követelményei
    - Adatbázis-engedélyek
- Környezet

## <a name="pre-deployment-steps"></a>Központi telepítés előtti lépések

A MIM CM konfigurációs varázsló meg kell adni ahhoz, hogy az sikeres befejezéséhez menet információra van szüksége. 
![](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>A séma kiterjesztése

A folyamat a séma kiterjesztésének egyszerű, de visszafordíthatatlan jellegéből körültekintően kell során.

>[!NOTE]
Ez a lépés megköveteli, hogy a használt fiók rendelkezik-e a séma-rendszergazdai jogosultságokkal.

- Keresse meg a MIM adathordozó helyét, és keresse meg \\Tanúsítványkezelő\\x64 mappa.

- A séma mappa másolása CORPDC, majd keresse meg a fájlt.

    ![](media/mim-cm-deploy/image005.png)

- A parancsfájl resourceForestModifySchema.vbs egyetlen erdő forgatókönyv futtatása

- Az erőforrás-erdő forgatókönyvhöz a parancsfájlok futtatása:
  - TartomanyA – a felhasználók található (userForestModifySchema.vbs)
  - ResourceForestB – (resourceForestModifySchema.vbs) CM telepítési helye

>[!NOTE]
Sémaváltozások egyirányú művelettel és egy erdőt igényel a visszaállítás helyreállítási ezért győződjön meg arról, hogy a szükséges biztonsági másolatot. A módosításait a séma szerint ez a művelet végrehajtása a részleteket nézze át a [Forefront Identity Manager 2010 tanúsítvány felügyeleti sémaváltozások](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

![](media/mim-cm-deploy/image007.png)

Futtassa a parancsfájlt, és sikeres kell kapnia egyszer jelenik meg, hogy a parancsfájl befejeződik.

![Sikeres üzenet](media/mim-cm-deploy/image009.png)

Az Active Directory-séma már ki van bővítve a MIM Tanúsítványkezelő támogatásához.

### <a name="creating-service-accounts-and-groups"></a>Szolgáltatási fiókok és csoportok létrehozása

A következő táblázat összefoglalja a fiókok és a MIM CM szükséges engedélyek.
Engedélyezheti, hogy a MIM CM hozza létre a következő fiókokat automatikusan, vagy a telepítés előtti létrehozhat. A tényleges számítógépfiók-nevét módosíthatja. Ha fiókokat hozhat létre a saját magának, fontolja meg, úgy, hogy nem felel meg a felhasználói fiók nevét, a függvény a felhasználói fiókok elnevezési.

Felhasználók:

![](media/mim-cm-deploy/image010.png)

![](media/mim-cm-deploy/image012.png)

| **Szerepkör**                   | **Felhasználói bejelentkezési név** |
|----------------------------|---------------------|
| A MIM CM-ügynök               | MIMCMAgent          |
| A MIM CM kulcs-helyreállítási megbízott  | MIMCMKRAgent        |
| A MIM CM engedélyezési ügynök | MIMCMAuthAgent      |
| A MIM CM hitelesítésszolgáltató Manager-ügynök    | MIMCMManagerAgent   |
| A MIM CM webes készlet ügynök      | MIMCMWebAgent       |
| A MIM CM tanúsítványigénylő ügynök    | MIMCMEnrollAgent    |
| A MIM CM frissítési szolgáltatás      | MIMCMService        |
| A MIM telepítési fiók        | MIMINSTALL          |
| Súgó ügyfélszolgálati ügynök            | CMHelpdesk1-2       |
| CM-kezelő                 | CMManager1-2        |
| Előfizető felhasználó            | CMUser1-2           |

Csoportok:

| **Szerepkör**               | **Csoport**         |
|------------------------|-------------------|
| CM támogató tagok    | MIMCM-segélyszolgálat    |
| CM Manager tagok     | MIMCM-kezelők    |
| CM-előfizető tagjai | MIMCM-előfizetők |

PowerShell: Ügynök fiókok

```
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

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Frissítés **CORPCM** kiszolgáló helyi házirend ügynök fiókok 

| **Felhasználói bejelentkezési név** | **Leírás és engedélyek**   |
|------|---------------------|
| MIMCMAgent          | Az alábbi szolgáltatásokat nyújtja: </br>-Lekéri a hitelesítésszolgáltató titkos kulcsok titkosítva. </br>-Védi az intelligens kártya PIN-kód adatait az FIM CM-adatbázisban. </br>-A FIM CM és a hitelesítésszolgáltató közötti kommunikáció védelme. </br></br> A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal:</br>-   **Helyi bejelentkezés engedélyezése** felhasználói jogosultsággal.</br>-   **Tanúsítványok kiállítása és kezelése** felhasználói jogosultsággal. </br>-Olvasási és írási engedéllyel a rendszer ideiglenes mappa a következő helyen: % WINDIR %\\Temp.</br>-A digitális aláírás és titkosítás tanúsítványt ki, és a felhasználó tárolóra telepíti.
|MIMCMKRAgent        | A helyreállást archiválja a hitelesítésszolgáltató titkos kulcsok. A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal:</br> -   **Helyi bejelentkezés engedélyezése** felhasználói jogosultsággal.</br>-A helyi csoporttagság **rendszergazdák** csoport. </br>-Regisztrálni engedéllyel a **KeyRecoveryAgent** tanúsítványsablont. </br>-Kulcs-helyreállítási megbízott tanúsítvány kiállítva, és a felhasználó tárolóra telepíti. A tanúsítvány hozzá kell adni a listához, a kulcs-helyreállítási ügynökök a hitelesítésszolgáltatón. </br>-Olvasási engedélyt, és írási engedéllyel a rendszer ideiglenes mappa a következő helyen:```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Meghatározza, hogy felhasználói jogok és engedélyek egyes felhasználókhoz és csoportokhoz. A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal: </br>-A csoporttagságot a Windows 2000 előtti rendszerrel kompatibilis hozzáférés tartomány. </br> -Rendelkeznek a **biztonsági naplózás létrehozása** felhasználói jogosultsággal.             |
| MIMCMManagerAgent   | CA felügyeleti tevékenységeket hajt végre. </br> A felhasználó a hitelesítésszolgáltató kezelése engedéllyel kell hozzárendelni.        |
| MIMCMWebAgent       | Az IIS-alkalmazáskészlet identitásának biztosít. FIM CM fut Microsoft Win32® alkalmazás alkalmazásprogramozási felület folyamat, amely a felhasználói hitelesítő adatokat használ. </br> A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal:</br> -A helyi csoporttagság **IIS_WPG, windows 2016 = IIS_IUSRS** csoport. </br>-A helyi csoporttagság **rendszergazdák** csoport.</br>-Rendelkeznek a **biztonsági naplózás létrehozása** felhasználói jogosultsággal. </br>-Rendelkeznek a **az operációs rendszer részeként** felhasználói jogosultsággal. </br>-Rendelkeznek a **csere Folyamatszintű token** felhasználói jogosultsággal.</br>-Az IIS-alkalmazáskészlet identitásának hozzárendelt **CLMAppPool**. </br>-Nyújtott olvasási jogosultságot a a **HKEY_LOCAL_MACHINE\\szoftver\\Microsoft\\CLM\\1.0-s verziójú\\Server\\WebUser** beállításkulcsot. </br>– Ezt a fiókot is delegálásra kell lennie.|
| MIMCMEnrollAgent    | Regisztráció egy felhasználó nevében végez. A felhasználói fióknak a következő hozzáférés-vezérlési beállításokkal:</br>-Egy tanúsítványigénylő megbízott tanúsítvány, amely ki, és a felhasználó tárolóra telepíti.</br>-   **Helyi bejelentkezés engedélyezése** felhasználói jogosultsággal. </br>-Regisztrálni engedéllyel a **tanúsítványigénylő megbízott** tanúsítványsablont (vagy az egyéni sablont, ha van).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>A MIM CM szolgáltatásfiókok tanúsítványsablonok létrehozása

Három a MIM CM általuk használt szolgáltatásfiókokat a szükséges tanúsítvány, és a konfiguráció varázsló kérni fogja a tanúsítványok sablonok, amelyet a számukra tanúsítványok lekérésére használnia kell a neve.

A szolgáltatásfiókok, amely tanúsítványt igényel a következők:

- MIMCMAgent: Ez a fiók szükséges felhasználói tanúsítvány

- MIMCMEnrollAgent: Ezt a fiókot kell tanúsítványigénylő ügynök tanúsítványával

- MIMCMKRAgent: Ennek a fióknak szüksége van egy **kulcs-helyreállítási megbízott** tanúsítvány

Vannak olyan sablonok már jelen van, az ad-ben, de létre kell hozni és a MIM Tanúsítványkezelő használata saját verziójú. Mivel az eredeti alapkonfiguráció sablonokból módosítás végrehajtásához igazolnia kell.

A fenti fiókok három fog rendelkezik emelt szintű jogosultságokkal a szervezeten belül, és gondosan kell kezelni.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>A MIM CM aláíró tanúsítvány sablonjának létrehozása

1. A **felügyeleti eszközök**, nyissa meg **hitelesítésszolgáltató**.
2. Az a **hitelesítésszolgáltató** konzol, a konzolfán bontsa ki a **Contoso-CorpCA**, és kattintson a **tanúsítványsablonok**.
3. Kattintson a jobb gombbal **tanúsítványsablonok**, és kattintson a **kezelése**.
4. Az a **Tanúsítványsablonok konzolt**, a a **részletek** ablaktábla, válassza ki, és kattintson a jobb gombbal **felhasználói**, és kattintson a **Sablon duplikálása** .
5. Az a **Sablon duplikálása** párbeszédpanelen jelölje ki **Windows Server 2003 Enterprise**, és kattintson a **OK**.

![Eredményül kapott változásainak](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    MIM CM does not work with certificates based on version 3 certificate templates. You must create a Windows Server® 2003 Enterprise (version 2)certificate template. See the following link for V3 details https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade

6. Az a **új sablon tulajdonságai** párbeszédpanel a **általános** lap a **sablon megjelenítendő neve** mezőbe írja be **MIM CM aláírási**. Módosítsa a **érvényességi** való **2 év**, és törölje a **a tanúsítvány közzététele az Active Directory** jelölőnégyzetet.

7. Az a **kérelmek kezelése** lapra, győződjön meg arról, hogy a **a titkos kulcs exportálható** jelölőnégyzet be van jelölve, és kattintson a **titkosítás lap**.

8. Az a **titkosítás kijelölés** párbeszédpanel, tiltsa le a **Microsoft Enhanced titkosításszolgáltató v1.0**, engedélyezése **Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató**, és kattintson a **OK**.

Az a **tulajdonosnévvel** lapon törölje a **e-mail név belefoglalása a tulajdonosnévbe** és **E-mail név** jelölőnégyzeteket.

Az a **bővítmények** lap a **a sablonban található bővítmények** listában, ügyeljen arra, hogy **alkalmazás-házirendek** van kiválasztva, és kattintson **szerkesztése** .

Az a **használati szabályzatok bővítmény szerkesztése** párbeszédpanelen jelölje ki mindkét a **titkosított fájlrendszer** és a **biztonságos e-mailek** alkalmazás-házirendek. Kattintson a **eltávolítása**, és kattintson a **OK**.

Az a **biztonsági** lapon hajtsa végre a következő lépéseket:

- Távolítsa el **rendszergazda**.

- Távolítsa el **Tartománygazdák**.

- Távolítsa el **tartományi felhasználók**.

- Rendeljen csak **olvasási** és **írási** engedélyekkel **vállalati rendszergazdák**.

- Adja hozzá **MIMCMAgent.**

- Rendelje hozzá **olvasási** és **beléptetés** engedélyekkel **MIMCMAgent**.

Az a **új sablon tulajdonságai** párbeszédpanel, kattintson a **OK**.

Hagyja a **Tanúsítványsablonok konzolt** megnyitásához.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>A MIM CM tanúsítványigénylő megbízott tanúsítványsablon létrehozása

-   Az a **Tanúsítványsablonok konzolt**, a a **részletek** ablaktábla, válassza ki, és kattintson a jobb gombbal **tanúsítványigénylő megbízott**, és kattintson a **sablonmásolása**.

Az a **Sablon duplikálása** párbeszédpanelen jelölje ki **Windows Server 2003 Enterprise**, és kattintson a **OK**.

Az a **új sablon tulajdonságai** párbeszédpanel a **általános** lap a **sablon megjelenítendő neve** mezőbe írja be **MIM CM tanúsítványigénylő megbízott**. Győződjön meg arról, hogy a **érvényességi** van **2 év**.

Az a **kérelmek kezelése** lapján engedélyezése **a titkos kulcs exportálható**, és kattintson a **kriptográfiai szolgáltatók vagy titkosítás lap.**

Az a **CSP kiválasztása** párbeszédpanel, tiltsa le a **Microsoft Base titkosításszolgáltató v1.0**, tiltsa le a **Microsoft Enhanced titkosításszolgáltató v1.0**, engedélyezése **A Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató**, és kattintson a **OK**.

Az a **biztonsági** lapon tegye a következőket:

- Távolítsa el **rendszergazda**.

- Távolítsa el **Tartománygazdák**.

- Rendeljen csak **olvasási** és **írási** engedélyekkel **vállalati rendszergazdák**.

- Adja hozzá **MIMCMEnrollAgent**.

- Rendelje hozzá **olvasási** és **beléptetés** engedélyekkel **MIMCMEnrollAgent**.

Az a **új sablon tulajdonságai** párbeszédpanel, kattintson a **OK**.

Hagyja a **Tanúsítványsablonok konzolt** megnyitásához.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>A MIM CM kulcs-helyreállítási megbízott tanúsítványsablon létrehozása

1. Az a **tanúsítványsablonok** konzol a **részletek** ablaktábla, válassza ki, és kattintson a jobb gombbal **kulcs-helyreállítási megbízott**, és kattintson a **Sablonduplikálása**.

2. Az a **Sablon duplikálása** párbeszédpanelen jelölje ki **Windows Server 2003 Enterprise**, és kattintson a **OK**.

3. Az a **új sablon tulajdonságai** párbeszédpanel a **általános** lap a **sablon megjelenítendő neve** mezőbe írja be **MIM CM kulcs-helyreállítási megbízott**. Győződjön meg arról, hogy a **érvényességi** van **2 év** a a **titkosítás lap.**

4. Az a **szolgáltatók kiválasztása** párbeszédpanel, tiltsa le a **Microsoft Enhanced titkosításszolgáltató v1.0**, engedélyezése **Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató**, majd **OK**.

5. Az a **kiállítási feltételek** lapra, győződjön meg arról, hogy **hitelesítésszolgáltató tanúsítványfelelősének jóváhagyása** van **le van tiltva**.

6. Az a **biztonsági** lapon tegye a következőket:

    - Távolítsa el **rendszergazda**.

    - Távolítsa el **Tartománygazdák**.

    - Rendeljen csak **olvasási** és **írási** engedélyekkel **vállalati rendszergazdák**.

    - Adja hozzá **MIMCMKRAgent**.

    - Rendelje hozzá **olvasási** és **beléptetés** engedélyekkel **KRAgent**.

7. Az a **új sablon tulajdonságai** párbeszédpanel, kattintson a **OK**.

8. Zárja be a **tanúsítvány-sablonok konzolt**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>A hitelesítésszolgáltató, a szükséges tanúsítványsablonok közzététele

1. Állítsa vissza a **hitelesítésszolgáltató** konzol.

2. Az a **hitelesítésszolgáltató** konzol, a konzolfán kattintson a jobb gombbal **tanúsítványsablonok**, mutasson a **új**, és kattintson a **tanúsítvány Tanúsítványsablon**.
3. Az a **tanúsítványsablonok engedélyezése** párbeszédpanelen jelölje ki **MIM CM tanúsítványigénylő megbízott**, **MIM CM kulcs-helyreállítási megbízott**, és **MIM CM aláíró**. Kattintson az **OK**gombra.
4. A konzolfán kattintson **tanúsítványsablonok**.
5. Ellenőrizze, hogy a három új sablonok megjelennek a **részletek** ablaktáblán, majd zárja be **hitelesítésszolgáltató**.
    ![A MIM CM aláírása](media/mim-cm-deploy/image016.png)
6. Zárjon be minden nyitott, és jelentkezzen ki.

### <a name="iis-configuration"></a>IIS-konfiguráció 

A webhely üzemeltetésére a CM hoIn rendezéshez sorrendben megrekedésének kezelése, és konfigurálja az IIS

#### <a name="install-and-configure-iis"></a>IIS telepítése és konfigurálása

1. Jelentkezzen be **, a CORLog **MIMINSTALL** fiók

>[!IMPORTANT]
A MIM telepítési fiókot a helyi rendszergazdának kell lennie.

2. Nyissa meg a powershellt, és futtassa a következő parancsot

   - ```Install-WindowsFeature –ConfigurationFilePath```

>[!NOTE]
 Az IIS 7 alapértelmezés szerint telepítve van a alapértelmezett webhely nevű webhelyhez. Ha, hogy a hely át lett nevezve vagy eltávolítani a nevű hely alapértelmezett webhely kell érhető el a MIM Tanúsítványkezelő telepítése előtt.

#### <a name="configuring-kerberos"></a>A Kerberos konfigurálása

A MIMCMWebAgent fiókkal fog futni a MIM Tanúsítványkezelő portálra. Alapértelmezés az IIS-ben, és fel rendszermag módú hitelesítés alapértelmezés szerint szolgál az IIS-ben. Tiltsa le a Kerberos a rendszermag módú hitelesítés lesz, és SPN-ek inkább a MIMCMWebAgent fiók konfigurálása. Néhány parancs emelt szintű engedélyekkel az active directory és a CORPCM kiszolgáló szükséges.

![](media/mim-cm-deploy/image020.png)

```
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}

```

** IIS frissítése a **CORPCM**


![](media/mim-cm-deploy/image022.png)

```
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true

```


>[!NOTE]
Adja hozzá a "cm.contoso.com" A DNS-rekordját és CORPCM IP mutasson kell

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>SSL megkövetelése a MIM CM-portálon

Erősen ajánlott, hogy a MIM CM-portálon SSL szükséges. Ha még nem a varázsló fogja figyelmeztetni azt.

1. A webkiszolgáló-tanúsítvány regisztrálása **cm.contoso.com** alapértelmezett hely hozzárendelése

2. Nyissa meg **IIS-kezelő** , és keresse meg **tanúsítványkezelés**

3. A szolgáltatások nézetben kattintson duplán az SSL-beállítások.

4. Az SSL-beállítások lapon válassza ki **SSL megkövetelése**.

5. Kattintson a műveletek ablaktábla **alkalmaz.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Adatbázis-konfigurációs **CORPSQL** a MIM Tanúsítványkezelő

1. Győződjön meg arról, hogy a CORPSQL01 kiszolgálóhoz csatlakozott.

2. Győződjön meg arról, SQL DBA vannak bejelentkezve

3. Futtassa a következő T-SQL parancsfájlt a CONTOSO engedélyezéséhez\\MIMINSTALL fiókot létrehozni az adatbázist, amikor azt nyissa meg a konfigurációs lépés

>[!NOTE]
Fel kell térjen vissza az SQL amikor azt készen áll a kilépési & házirend modul

```
create login [CONTOSO\\MIMINSTALL] from windows;
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
```

![A MIM Tanúsítványkezelő konfigurációs varázsló hibaüzenet](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>A Microsoft Identity Manager 2016 Tanúsítványkezelő központi telepítése

1. Győződjön meg arról, hogy a CORPCM kiszolgáló, és csatlakozott a **MIMINSTALL** fiók tagja a **helyi rendszergazdák** csoport.

2. Ellenőrizze, hogy be vannak jelentkezve a Contoso\\MIMINSTALL.

3. A Microsoft Identity Manager SP1 ISO csatlakoztatni.

4. **Nyissa meg** a **Tanúsítványkezelő\\x64** könyvtár.

5. Az a **x64** ablak, kattintson a jobb gombbal **telepítő**, és kattintson a **Futtatás rendszergazdaként**.

6. Üdvözli a Microsoft Identity Manager tanúsítvány felügyeleti telepítővarázsló lapra, kattintson a **tovább.**

7. A végfelhasználói licencszerződés lapon olvassa el a szerződést, engedélyezze a elfogadom a licencszerződés **jelölőnégyzetet**, majd kattintson a Tovább gombra.

8. Egyéni beállítása lapon győződjön meg arról, hogy a **MIM Tanúsítványkezelő portálra** és **MIM CM frissítési szolgáltatás-összetevők** , telepítve van beállítva, majd **kattintson a Tovább gombra**.

9. A virtuális webes mappa lapon győződjön meg arról, hogy a virtuális mappa neve ** CertificateManagement, majd **kattintson a Tovább gombra**.

10. A telepítése a Microsoft Identity Manager Tanúsítványkezelő lapon **kattintson a telepítés**.

11. Az a **befejezve** a Microsoft Identity Manager tanúsítvány felügyeleti telepítővarázsló lap **kattintson a Befejezés**.

![A MIM Tanúsítványkezelő varázsló befejeződött](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>A Microsoft Identity Manager 2016 tanúsítványkezelés konfigurációs varázsló

CORPCM való bejelentkezés előtt vegye fel a MIMINSTALL **tartományi rendszergazdák, a Sémagazdák és a helyi rendszergazdák** csoportot ehhez a konfigurációs varázsló. Ez lehet eltávolítani, később konfigurálásának befejezését követően.      
    
![Hibaüzenet](media/mim-cm-deploy/image028.png)

1. Az a **Start** menüben kattintson a **tanúsítvány konfigurációs varázsló**. És futtató **rendszergazda**
2. Az a **üdvözli a konfiguráció varázsló** kattintson **következő**.
3. Az a **hitelesítésszolgáltató konfigurációjának** lapon, győződjön meg arról, hogy a kijelölt hitelesítésszolgáltató **Contoso-CORPCA-hitelesítésszolgáltató**, győződjön meg arról, hogy a kijelölt kiszolgáló **CORPCA. A CONTOSO.COM**, és kattintson a **következő**.
4. A a **beállítása a Microsoft® SQL Server®-adatbázis** lap a **nevet az SQL Server** mezőbe írja be **CORPSQL1** , engedélyezze a **létrehozásához használja a hitelesítő adataimat a adatbázis** jelölőnégyzetet, majd kattintson a **következő**.
5. Az a **adatbázis beállításainak** lap, fogadja el az alapértelmezett adatbázis **FIMCertificateManagement**, ügyeljen arra, hogy **SQL integrált hitelesítés** van kijelölve, majd Kattintson a **következő**.

6. Az a **beállítása az Active Directory** lapon fogadja el az alapértelmezett nevet, a szolgáltatáskapcsolódási pont előírt, és kattintson a **következő**.

7. Az a **hitelesítési módszer** lapon erősítse meg **windows beépített hitelesítés** van kiválasztva, majd kattintson az **következő**.

8. Az a **ügynökök – FIM CM** lapon törölje a jelet a **az FIM CM alapértelmezett beállításokat használja** jelölőnégyzetet, majd kattintson a **egyéni fiókok**.

9. Az a **ügynökök – FIM CM** többlapos párbeszédpanel, az egyes lapokon írja be a következőt:
   - Felhasználónév: **frissítés** 
   - Jelszó: **átadni\@word1**
   - Jelszó megerősítése: **átadni\@word1**
   - Használjon egy meglévő felhasználó: **engedélyezve**
>[!NOTE]
Korábban létrehozott ezeket a fiókokat. Győződjön meg arról, hogy az eljárások a 8. lépés ismétlődjenek-e az összes hat ügynök fiók lap.

![A MIM CM-fiókok](media/mim-cm-deploy/image030.png)

10. Amikor befejeződött az összes ügynök fiók adatait, kattintson **OK**.

11. Az a **ügynökök – a MIM Tanúsítványkezelő** kattintson **következő**.

12. Az a **beállítása a kiszolgálói tanúsítványok** lapon, a következő tanúsítványsablonok engedélyezése:
    - A helyreállítási ügynök kulcs-helyreállítási megbízott tanúsítványt használt tanúsítványsablont: **MIMCMKeyRecoveryAgent**.
    - Az FIM CM-ügynök tanúsítvány használt tanúsítványsablont: **MIMCMSigning**.
    - A tanúsítványigénylő ügynök tanúsítványával használt tanúsítványsablont: **FIMCMEnrollmentAgent**.
13. Az a **beállításról kiszolgálótanúsítványok** kattintson **következő**.
14. Az a **telepítő E-mail kiszolgáló, a dokumentum nyomtatása** lap a **adja meg a regisztrációs értesítések küldéséhez használni kívánt SMTP-kiszolgáló nevét** mezőbe, majd kattintson **tovább.**
15. Az a **beállíthatja az** kattintson **konfigurálása**.
16. Az a **konfigurációs varázsló – Microsoft Forefront Identity Manager 2010 R2** figyelmeztető párbeszédpanel, kattintson a **OK** megerősíti, hogy az SSL nincs engedélyezve az IIS virtuális könyvtár számára.

    ![Media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    Ne kattintson a Befejezés gombra, amíg a konfiguráció varázsló végrehajtása befejeződik. Naplózás, a varázsló itt találhatók:**% programfiles %\\Microsoft Forefront Identity Management\\2010\\Tanúsítványkezelő\\config.log**
17. Kattintson a **Befejezés**gombra.

![A MIM Tanúsítványkezelő varázsló befejeződött](media/mim-cm-deploy/image033.png)

18. Zárjon be minden nyitott.

19. Adja hozzá a https://cm.contoso.com/certificatemanagement a böngésző helyi intranet zónához.

20. A kiszolgáló CORPCM https://cm.contoso.com/certificatemanagement keresse fel a webhelyet  

    ![](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Ellenőrizze a CNG-kulcs elkülönítése szolgáltatás

1. A **felügyeleti eszközök**, nyissa meg **szolgáltatások**.

2. Az a **részletek** ablaktáblán kattintson duplán a **CNG-kulcs elkülönítése**.

3. Az a **általános** lapján módosíthatja a **indítási típus** való **automatikus**.

4. Az a **általános** lapra, indítsa el a szolgáltatást, ha az nem indult el állapotban van.

5. Az a **általános** lapra, majd **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Telepítés és a hitelesítésszolgáltató modulok konfigurálása:

Ebben a lépésben az telepítjük és a FIM CM hitelesítésszolgáltató modulok konfigurálása a hitelesítésszolgáltatónál.

1. FIM CM a műveleteket a felhasználói engedélyek csak vizsgálata konfigurálása

2. Az a **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Tanúsítványkezelő\\webes** ablakban másolata legyen  **Web.config** a másolat elnevezési **web.1.config**.

3. Az a **webes** ablak, kattintson a jobb gombbal **Web.config**, és kattintson a **nyitott**.

    >[!Note]
    A Web.config fájlt a Jegyzettömb alkalmazásban van megnyitva.

4. A fájl megnyitása után nyomja le a CTRL + F.

5. Az a **keresése és cseréje** párbeszédpanel a **mit** mezőbe írja be **UseUser**, és kattintson a **következő** három alkalommal.

6. Zárja be a **keresése és cseréje** párbeszédpanel megnyitásához.

7. A sorban kell  **\<key="Clm.RequestSecurity.Flags hozzáadása" érték "UseUser, UseGroups" = /\>**. Módosítsa a sor olvasására  **\<key="Clm.RequestSecurity.Flags hozzáadása" érték "UseUser" = /\>**.

8. Zárja be a fájlt, az összes módosításainak mentése folyamatban van.

9. A hitelesítésszolgáltató számítógép-fiók létrehozása az SQL-kiszolgálón \<nem parancsprogram\>

10. Győződjön meg arról, hogy csatlakozik a **CORPSQL01** kiszolgáló.

11. Ellenőrizze, hogy be vannak jelentkezve **DBA**

12. Az a **Start** menü, az indítási **SQL Server Management Studio**.

13. A a **kapcsolódás a kiszolgálóhoz** párbeszédpanel a **kiszolgálónév** mezőbe írja be **CORPSQL01,** majd **Connect**.

14. A konzolfán bontsa ki a **biztonsági**, és kattintson a **bejelentkezések**.

15. Kattintson a jobb gombbal **bejelentkezések**, és kattintson a **új bejelentkezés**.

16. Az a **általános** lap a **bejelentkezési név** mezőbe írja be **contoso\\CORPCA\$**. Válassza ki **Windows-hitelesítés**. Alapértelmezett adatbázis **FIMCertificateManagement**.

17. A bal oldali panelen válassza ki a **Felhasználóleképezés**. Kattintson a jobb oldali ablaktáblában található jelölőnégyzetek bejelölésével a **térkép** oszlop mellett **FIMCertificateManagement**. Az a **adatbázis-szerepköri tagság a következőhöz: FIMCertificateManagement** listában, engedélyezze a **clmApp** szerepkör.

18. Kattintson az **OK**gombra.

19. Bezárás **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>A hitelesítésszolgáltatón a hitelesítésszolgáltató FIM CM-modulok telepítése

1. Győződjön meg arról, hogy csatlakozik a **CORPCA** kiszolgáló.

2. Az a **X64** windows, kattintson a jobb gombbal **Setup.exe**, és kattintson a **Futtatás rendszergazdaként**.

3. Az a **üdvözli a Microsoft Identity Manager felügyeleti telepítő varázsló** kattintson **következő**.

4. Az a **végfelhasználói licencszerződés** oldalon olvassa el a szerződést. Válassza ki a **elfogadom a licencszerződés feltételeit** jelölőnégyzetet, majd kattintson a **következő**.

5. Az a **egyéni telepítés** lapon jelölje be **MIM Tanúsítványkezelő portálra**, és kattintson a **Ez a funkció nem lesz elérhető**.

6. Az a **egyéni telepítés** lapon jelölje be **MIM CM frissítési szolgáltatás**, és kattintson a **a szolgáltatás nem érhető el**.

    >[!Note]
    Ez a MIM CM hitelesítésszolgáltató fájlok hagyja a egyetlen szolgáltatás, a telepítés engedélyezve van.

7. Az a **egyéni telepítés** kattintson **következő**.

8. Az a **telepítése a Microsoft Identity Manager Tanúsítványkezelő** kattintson **telepítése**.

9. Az a **befejeződött a Microsoft Identity Manager tanúsítvány felügyeleti telepítővarázslója** kattintson **Befejezés**.

10. Zárjon be minden nyitott.

### <a name="configure-the-mim-cm-exit-module"></a>A MIM CM kilépési modul konfigurálása

1. A **felügyeleti eszközök**, nyissa meg **hitelesítésszolgáltató**.

2. A konzolfán kattintson a jobb gombbal **contoso-CORPCA-hitelesítésszolgáltató**, és kattintson a **tulajdonságok**.

3. Az a **kilépési modul** lapon jelölje be **FIM CM kilépési modul**, és kattintson a **tulajdonságok**.

4. Az a **a CM adatbázis kapcsolati karakterláncának meghatározása** mezőbe írja be **Connect Timeout = 15. Biztonsági információ megőrzése = True; Integrated Security = sspi; Initial Catalog = FIMCertificateManagement; adatforrás = CORPSQL01**. Hagyja a **a kapcsolati karakterlánc titkosítása** engedélyezve jelölőnégyzetet, majd kattintson a **OK**.
5. Az a **Microsoft FIM Tanúsítványkezelő** üzenetpanelen, kattintson a **OK**.

6. Az a **contoso-CORPCA-hitelesítésszolgáltató tulajdonságainak** párbeszédpanel, kattintson a **OK**.

7. Kattintson a jobb gombbal **contoso-CORPCA-hitelesítésszolgáltató***,* mutasson **feladataival**, és kattintson a **szolgáltatás leállítása**. Várjon, amíg az Active Directory tanúsítványszolgáltatások leáll.

8. Kattintson a jobb gombbal **contoso-CORPCA-hitelesítésszolgáltató***,* mutasson **feladataival**, és kattintson a **szolgáltatás indítása**.

9. Minimalizálja a **hitelesítésszolgáltató** konzol.

10. A **felügyeleti eszközök**, nyissa meg **Eseménynapló**.

11. A konzolfán bontsa ki a **alkalmazás- és szolgáltatásnaplók**, és kattintson a **FIM Tanúsítványkezelő**.

12. Az események listájában ellenőrizze, hogy a legújabb események hajthatja végre *nem* foglalja magában a **figyelmeztetés** vagy **hiba** események a tanúsítványszolgáltatások az utolsó újraindítás óta.

    >[!NOTE] 
    Az utolsó esemény tartalmaznia kell, hogy a kilépési modul betöltése a beállítások használatával```SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit```

13. Minimalizálja a **Eseménynapló**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>A MIMCMAgent tanúsítvány ujjlenyomata Windows® vágólapra másolása

1. Állítsa vissza a **hitelesítésszolgáltató** konzol.

2. A konzolfán bontsa ki a **contoso-CORPCA-hitelesítésszolgáltató**, és kattintson a **kiállított tanúsítványok**.

3. A a **részletek** ablaktáblán kattintson duplán a tanúsítványt, amelynek **CONTOSO\\MIMCMAgent** a a **igénylő neve** oszlop és **FIM CM Aláírási** a a **tanúsítványsablon** oszlop.

4. Az a **részletek** lapon jelölje be a **ujjlenyomat** mező.

5. Válassza ki az ujjlenyomatot, és nyomja le az CTRL + C billentyűket.

    >[!NOTE]
    Tegye **nem** közé tartozik a szóközök ujjlenyomat karakterek közül.

6. Az a **tanúsítvány** párbeszédpanel, kattintson a **OK**.

7. Az a **Start** menüben, a a **Keresés programokban és fájlokban** mezőbe írja be **Jegyzettömb**, és nyomja le az ENTER BILLENTYŰT.

8. A **Jegyzettömb**, az a **szerkesztése** menüben kattintson a **Beillesztés**.

9. Az a **szerkesztése** menüben kattintson a **cserélje le**.

10. Az a **mit** mezőbe, írja be a szóköz karaktert, majd kattintson **Mindet**.

    >[!Note]
    Ez eltávolítja az összes az ujjlenyomatot a karakter a szóközt.
    
11. Az a **cserélje le** párbeszédpanel, kattintson a **Mégse**.

12. Válassza ki a konvertált *thumbprintstring*, majd nyomja le a CTRL + C az.

13. Bezárás **Jegyzettömb** módosítások mentése nélkül.

### <a name="configure-the-fim-cm-policy-module"></a>A FIM CM-házirend modul konfigurálása

1. Állítsa vissza a **hitelesítésszolgáltató** konzol.

2. Kattintson a jobb gombbal **contoso-CORPCA-hitelesítésszolgáltató**, és kattintson a **tulajdonságok**.

3.  Az a **contoso-CORPCA-hitelesítésszolgáltató tulajdonságainak** párbeszédpanel a **irányelvmodul** lapra, majd **tulajdonságok**.

- Az a **általános** lapra, győződjön meg arról, hogy **FIM CM kérelmek át az alapértelmezett házirend modul a feldolgozási** van kiválasztva.
- Az a **aláíró tanúsítványok** lapra, majd **Hozzáadás**.
- A tanúsítvány párbeszédpanelen kattintson a jobb gombbal a **hexadecimális kódolású tanúsítvány kivonatát adja meg** gombra, majd **Beillesztés**.
- Az a **tanúsítvány** párbeszédpanel, kattintson a **OK**.
    >[!Note]
    Ha a **OK** gomb nincs engedélyezve, akkor véletlenül egy rejtett karakter a ujjlenyomat karakterláncban található a clmAgent tanúsítvány ujjlenyomata másolása során. Ismételje meg a-től kezdődő lépéseket **4. feladat: a Windows vágólapra másolása a MIMCMAgent tanúsítvány ujjlenyomata** ebben a gyakorlatban.

- Az a **konfigurációs tulajdonságok** párbeszédpanelen győződjön meg arról, hogy az ujjlenyomat megjelenik a **érvényes aláíró tanúsítványok** listára, majd **OK**.

- Az a **FIM Tanúsítványkezelő** üzenetpanelen, kattintson a **OK**.

- Az a **contoso-CORPCA-hitelesítésszolgáltató tulajdonságainak** párbeszédpanel, kattintson a **OK**.

- Kattintson a jobb gombbal **contoso-CORPCA-hitelesítésszolgáltató***,* mutasson **feladataival**, és kattintson a **szolgáltatás leállítása**.

- Várjon, amíg az Active Directory tanúsítványszolgáltatások leáll.

- Kattintson a jobb gombbal **contoso-CORPCA-hitelesítésszolgáltató***,* mutasson **feladataival**, és kattintson a **szolgáltatás indítása**.

- Zárja be a **hitelesítésszolgáltató** konzol.

- Zárjon be minden nyitott, és jelentkezzen.

- **Utolsó lépés a központi telepítésben lévő** , győződjön meg arról, hogy a CONTOSO szeretnénk\\MIMCM-kezelők telepítheti és sablonok létrehozása és konfigurálása a rendszer anélkül, hogy a séma- és tartományi rendszergazdák. A következő parancsfájl fog ACL az engedélyeket, hogy a dsacls használt tanúsítványsablonok. Futtassa a teljes jogosultságot kapnak, módosítsa a biztonsági fiók olvasási és írási engedéllyel az erdő minden meglévő tanúsítványsablonhoz.

- Első lépések: **szolgáltatáskapcsolódási pont és a cél csoportengedélyek konfigurálásával & profil sablon kezelésének delegálása**
  - Engedélyek konfigurálása a szolgáltatáskapcsolati pontot (SCP).

  - Delegált profilkezelés sablon konfigurálása.

  - Engedélyek konfigurálása a szolgáltatáskapcsolati pontot (SCP). **\<parancsfájl\>**

        -   Győződjön meg arról, hogy csatlakozik a **CORPDC** virtuális kiszolgáló.

        -   Jelentkezzen be **contoso\\corpadmin**

        -   A **felügyeleti eszközök**, nyissa meg **Active Directory – felhasználók és számítógépek**.

        -   A **Active Directory – felhasználók és számítógépek**, a **nézet** menü, ügyeljen arra, hogy **speciális funkciók** engedélyezve van.

        -   A konzolfán bontsa ki a **Contoso.com** \| **rendszer** \| **Microsoft** \| **tanúsítványának élettartama Manager**, és kattintson a **CORPCM**.

        -   Kattintson a jobb gombbal **CORPCM**, és kattintson a **tulajdonságok**.

        -   Az a **CORPCM tulajdonságok** párbeszédpanel a **biztonsági** lapon vegye fel a következő csoportok a megfelelő engedélyekkel:

    | Csoport          | Engedélyek                                                                                                                                                         |
    |----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | mimcm-kezelők | Olvasás </br> FIM CM naplózási</br> FIM CM tanúsítványigénylő ügynök</br> FIM CM kérelem regisztrálása</br> FIM CM kérelem helyreállítása</br> FIM CM kérelem megújítása</br> FIM CM kérelem visszavonása </br> FIM CM kérelem intelligens kártya letiltásának feloldása |
    | MIMCM-segélyszolgálat | Olvasás</br> FIM CM tanúsítványigénylő ügynök</br> FIM CM kérelem visszavonása</br> FIM CM kérelem intelligens kártya letiltásának feloldása                                                                                |
- Az a **CORPDC tulajdonságok** párbeszédpanel, kattintson a **OK**.

- Hagyja **Active Directory – felhasználók és számítógépek** megnyitásához.

- **Engedélyek konfigurálása a leszármazott felhasználó objektumok**
    - Ellenőrizze, hogy még mindig a **Active Directory – felhasználók és számítógépek** konzol.
    - A konzolfán kattintson a jobb gombbal **Contoso.com**, és kattintson a **tulajdonságok**.
    - Az a **biztonsági** lapra, majd **speciális**.
    - Az a **speciális biztonsági beállítások a Contoso** párbeszédpanel, kattintson a **Hozzáadás**.
    - A a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **adja meg a kiválasztandó objektum nevét** mezőbe írja be **mimcm-kezelők**, majd kattintson az **OK**.
    - Az a **Contoso engedélybejegyzés** párbeszédpanel a **alkalmazása** listáról válassza ki **leszármazott felhasználó objektumai** , majd engedélyezze a **engedélyezése**jelölőnégyzetét az alábbi **engedélyek**:
        - **Az összes tulajdonság olvasása**
        - **Olvasási engedélyek**
        - **FIM CM naplózási**
        - **FIM CM tanúsítványigénylő ügynök**
        - **FIM CM kérelem regisztrálása**
        - **FIM CM kérelem helyreállítása**
        - **FIM CM kérelem megújítása**
        - **FIM CM kérelem visszavonása**
        - **FIM CM kérelem intelligens kártya letiltásának feloldása**
    - Az a **Contoso engedélybejegyzés** párbeszédpanel, kattintson a **OK**.
    - Az a **speciális biztonsági beállítások a Contoso** párbeszédpanel, kattintson a **Hozzáadás**.
    - Az a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **adja meg a kiválasztandó objektum nevét** mezőbe írja be **mimcm-segélyszolgálat**, és kattintson a **OK**.
    - Az a **Contoso engedélybejegyzés** párbeszédpanel a **alkalmazása** listáról válassza ki **leszármazott felhasználó objektumai** , és válassza a **engedélyezése**jelölőnégyzetét az alábbi **engedélyek**: - **az összes tulajdonság olvasása** - **olvasási engedéllyel** - **FIM CM tanúsítványigénylő megbízott** - **FIM CM kérelem Revoke** - **FIM CM kérelem intelligens kártya letiltásának feloldása**
    - Az a **Contoso engedélybejegyzés** párbeszédpanel, kattintson a **OK**.
    -A a **speciális biztonsági beállítások a Contoso** párbeszédpanel, kattintson a **OK**.
    - Az a **contoso.com tulajdonságok** párbeszédpanel, kattintson a **OK**.
    - Hagyja **Active Directory – felhasználók és számítógépek** megnyitásához.

    - **Engedélyek konfigurálása a leszármazott felhasználó objektumokon \<nem parancsprogram\>**
        - Ellenőrizze, hogy még mindig a **Active Directory – felhasználók és számítógépek** konzol.
        - A konzolfán kattintson a jobb gombbal **Contoso.com**, és kattintson a **tulajdonságok**.
        - Az a **biztonsági** lapra, majd **speciális**.
        - Az a **speciális biztonsági beállítások a Contoso** párbeszédpanel, kattintson a **Hozzáadás**.
        - A a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **adja meg a kiválasztandó objektum nevét** mezőbe írja be **mimcm-kezelők**, majd kattintson az **OK**.
        - Az a **CONTOSO engedélybejegyzés** párbeszédpanel a **alkalmazása** listáról válassza ki **leszármazott felhasználó objektumai** , majd engedélyezze a **engedélyezése**jelölőnégyzetét az alábbi **engedélyek**:
            - **Az összes tulajdonság olvasása**
            - **Olvasási engedélyek**
            - **FIM CM naplózási**
            - **FIM CM tanúsítványigénylő ügynök**
            - **FIM CM kérelem regisztrálása**
            - **FIM CM kérelem helyreállítása**
            - **FIM CM kérelem megújítása**
            - **FIM CM kérelem visszavonása**
            - **FIM CM kérelem intelligens kártya letiltásának feloldása**
    - Az a **CONTOSO engedélybejegyzés** párbeszédpanel, kattintson a **OK**.
    - Az a **speciális biztonsági beállítások a CONTOSO** párbeszédpanel, kattintson a **Hozzáadás**.
    - Az a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **adja meg a kiválasztandó objektum nevét** mezőbe írja be **mimcm-segélyszolgálat**, és kattintson a **OK**.
    - Az a **CONTOSO engedélybejegyzés** párbeszédpanel a **alkalmazása** listáról válassza ki **leszármazott felhasználó objektumai** , és válassza a **engedélyezése**jelölőnégyzetét az alábbi **engedélyek**: - **az összes tulajdonság olvasása** - **olvasási engedéllyel** - **FIM CM tanúsítványigénylő megbízott** - **FIM CM kérelem Revoke** - **FIM CM kérelem intelligens kártya letiltásának feloldása**
    - Az a **contoso engedélybejegyzés** párbeszédpanel, kattintson a **OK**.
    - Az a **speciális biztonsági beállítások a Contoso** párbeszédpanel, kattintson a **OK**.
    - Az a **contoso.com tulajdonságok** párbeszédpanel, kattintson a **OK**.
    - Hagyja **Active Directory – felhasználók és számítógépek** megnyitásához.
- Második lépéseket: **tanúsítványsablonok felügyeleti engedélyeinek delegálása \<parancsfájl\>**
    - A tanúsítványsablonok tároló engedélyeinek delegálása.
    - Az OID-tárolóra vonatkozó engedélyek delegálása.
    - A meglévő tanúsítványsablonok engedélyeinek delegálása.
- Adja meg a tanúsítványsablonok tároló az engedélyek
     1. Állítsa vissza a **Active Directory – helyek és szolgáltatások** konzol.
     2. A konzolfán bontsa ki a **szolgáltatások**, bontsa ki a **nyilvánoskulcs-szolgáltatások**, és kattintson a **tanúsítványsablonok**.
     3. A konzolfán kattintson a jobb gombbal **tanúsítványsablonok**, és kattintson a **Vezérlés delegálása**.
     4. Az a **Vezérlés delegálása** varázsló, kattintson a **következő**.
     5. Az a **felhasználók vagy csoportok** kattintson **Hozzáadás**.
     6. Az a **felhasználók, számítógépek vagy csoportok** párbeszédpanel a **írja be a kijelölendő** mezőbe írja be **mimcm-kezelők**, és kattintson a **OK**.
     7. Az a **felhasználók vagy csoportok** kattintson **következő**.
     8. Az a **Delegálandó feladatok** kattintson **hozzon létre egy egyéni feladat**, és kattintson a **következő**.
     9.  Az a **Active Directory objektumtípus** lapon **ebben a mappában, ez a mappa, és ebben a mappában új objektumok létrehozása a meglévő objektumok** van kiválasztva, és kattintson **tovább**.
     10. Az a **engedélyek** lap a **engedélyek** listáról válassza ki a **teljes hozzáférés** jelölőnégyzetet, majd kattintson a **következő**.
     11. Az a **a Vezérlés delegálása varázsló befejezése** kattintson **Befejezés**.

- Adja meg a OID tároló az engedélyek
     1. A konzolfán kattintson a jobb gombbal **OID**, és kattintson a **tulajdonságok**.
     2. Az a **OID tulajdonságok** párbeszédpanel a **biztonsági** lapra, majd **speciális**.
     3. Az a **OID speciális biztonsági beállítások** párbeszédpanel, kattintson a **Hozzáadás**.
     4. A a **válassza ki a felhasználó, számítógép, szolgáltatásfiók vagy csoport** párbeszédpanel a **adja meg a kiválasztandó objektum nevét** mezőbe írja be **mimcm-kezelők**, majd kattintson az **OK**.
     5. A a **engedélybejegyzés OID azonosítója** párbeszédpanelen győződjön meg arról, hogy az engedélyek alkalmazása **Ez az objektum és a gyermekobjektumok**, kattintson a **teljes hozzáférés**, és kattintson a  **OK**.
     6. Az a **OID speciális biztonsági beállítások** párbeszédpanel, kattintson a **OK**.
     7. Az a **OID tulajdonságok** párbeszédpanel, kattintson a **OK**.
     8. Bezárás **Active Directory – helyek és szolgáltatások**.

**Parancsfájlok: Az Objektumazonosító, profilsablon & tanúsítványsablonok tároló engedélyeinek**

![](media/mim-cm-deploy/image021.png)

a(z) "import-module activedirectory $adace = @{"OID"=" AD:\\CN OID, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = = com "; "Ki" = "AD:\\CN tanúsítványsablonok, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = = com"; "PT" = "AD:\\CN Profilsablonok, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = = com"} $adace. GetEnumerator() |} **Foreach-Object** {$acl = **Get-hozzáférés-vezérlési lista** *-elérési út* $_. $Sid érték = (**Get-ADGroup** "MIMCM-vezetők"). SID $p = **új objektum** System.Security.Principal.SecurityIdentifier($sid)
##<a name="httpsmsdnmicrosoftcomen-uslibrarysystemdirectoryservicesactivedirectorysecurityinheritancevvs110aspx"></a>https://msdn.microsoft.com/en-us/library/System.DirectoryServices.activedirectorysecurityinheritance (v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule ($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow, [ DirectoryServices.ActiveDirectorySecurityInheritance]::All) $acl. AddAccessRule($ace) **Set-hozzáférés-vezérlési lista** *-elérési út* $_. Érték *- AclObject* $acl}
```

**Scripts: Delegating permissions on the existing certificate templates.**

![](media/mim-cm-deploy/image039.png)

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

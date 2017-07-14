---
title: "A PAM üzembe helyezése, 2. lépés – PRIV DC | Microsoft Docs"
description: "A PRIV tartományvezérlő előkészítése, amely olyan megerősített környezetet biztosít, amelyben a Privileged Access Management elkülönítve szerepel."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: edc15b41d4248887f4a93217f68d8125f6500585
ms.contentlocale: hu-hu
ms.lasthandoff: 07/10/2017


---

# 2. lépés: A PRIV tartományvezérlő előkészítése
<a id="step-2---prepare-the-first-priv-domain-controller" class="xliff"></a>

>[!div class="step-by-step"]
[« 1. lépés](step-1-prepare-corp-domain.md)
[3. lépés »](step-3-prepare-pam-server.md)

Ebben a lépésben egy új tartományt fog létrehozni, amely megerősített környezetet biztosít a rendszergazdai hitelesítéshez.  Ebben az erdőben szükség lesz legalább egy tartományvezérlőre és legalább egy tagkiszolgálóra. A tagkiszolgálót a következő lépésben fogja konfigurálni.

## Új Privileged Access Management-tartományvezérlő létrehozása
<a id="create-a-new-privileged-access-management-domain-controller" class="xliff"></a>

Ebben a szakaszban egy virtuális gépet fog beállítani egy új erdő tartományvezérlőjeként.

### A Windows Server 2012 R2 telepítése
<a id="install-windows-server-2012-r2" class="xliff"></a>
Egy „PRIVDC” számítógép létrehozásához telepítse a Windows Server 2012 R2 rendszert egy másik új virtuális gépre, amelyen nincs telepített szoftver.

1. Válassza a Windows Server egyéni (nem frissítő) telepítését. A telepítéskor válassza a **Windows Server 2012 R2 Standard (kiszolgáló grafikus felhasználói felülettel) x64** kiadást. _Ne válassza az_ **Adatközpont vagy Server Core** lehetőséget.

2. Olvassa el és fogadja el a licencfeltételeket.

3. Mivel a lemez üres, válassza az **Egyéni: A Windows telepítése a korábbi adatok megőrzése nélkül** beállítást, és használja a nem inicializált lemezterületet.

4. A megfelelő verziójú operációs rendszer telepítése után jelentkezzen be az új rendszergazdaként az új számítógépre. A Vezérlőpulton adja a *PRIVDC* nevet a számítógépnek, adjon neki statikus IP-címet a virtuális hálózatban, és konfigurálja a DNS-kiszolgálót úgy, hogy az legyen előző lépésben telepített tartományvezérlő. Ehhez a kiszolgáló újraindítása szükséges.

5. A kiszolgáló újraindítása után jelentkezzen be rendszergazdaként. A Vezérlőpulton állítsa be a számítógépet a frissítések keresésére, és telepítse a szükséges frissítéseket. Ehhez a kiszolgáló újraindítására lehet szükség.

### Szerepkörök hozzáadása
<a id="add-roles" class="xliff"></a>
Vegye fel az Active Directory tartományi szolgáltatásokat (AD DS) és a DNS-kiszolgálói szerepkört.

1. Indítsa el a PowerShellt rendszergazdaként.

2. A Windows Server Active Directory telepítésének előkészítéséhez írja be a következő parancsokat.

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### A beállításjegyzék beállításainak megadása a SID-előzmények áttelepítéséhez
<a id="configure-registry-settings-for-sid-history-migration" class="xliff"></a>

Indítsa el a PowerShellt, és írja be a következő parancsot, amellyel beállíthatja, hogy a forrástartomány engedélyezze a távoli eljáráshívás (RPC) hozzáférését a biztonsági fiókkezelő (SAM) adatbázisához.

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## Új Privileged Access Management erdő létrehozása
<a id="create-a-new-privileged-access-management-forest" class="xliff"></a>

Ezután léptesse elő a kiszolgálót egy új erdő tartományvezérlőjévé.

A jelen dokumentumban a priv.contoso.local név az új erdő tartományneve.  Az erdő nevének nincs nagy jelentősége, és nem szükséges, hogy a szervezet meglévő erdőnevének alárendeltje legyen. Az új erdő tartománynevének és NetBIOS-nevének azonban egyedinek kell lennie, és különböznie kell a szervezetben található más tartományok nevétől.  

### Tartomány és erdő létrehozása
<a id="create-a-domain-and-forest" class="xliff"></a>

1. Az új tartomány létrehozásához írja be a következő parancsokat a PowerShell ablakban.  Ez egy DNS-delegálást is létrehoz az előző lépésben létrehozott felső szintű tartományban (contoso.local).  Ha azt tervezi, hogy később konfigurálja a DNS-t, akkor ne adja meg a következő paramétereket: `CreateDNSDelegation -DNSDelegationCredential $ca`.

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. A megjelenő helyi menüben adja meg a CORP erdő rendszergazdájának hitelesítő adatait (például a CONTOSO\\Rendszergazda felhasználónevet és az ahhoz tartozó jelszót az 1. lépésből).

3. A PowerShell ablakában megjelenik egy figyelmeztetés a csökkentett mód rendszergazdai jelszavának használatára vonatkozóan. Adjon meg egy új jelszót kétszer. A DNS-delegálással és a titkosítási beállításokkal kapcsolatos figyelmeztető üzenet jelenik meg. Ez nem rendellenes.

Az erdő létrehozásának befejezése után a kiszolgáló automatikusan újraindul.

### Felhasználói és szolgáltatásfiókok létrehozása
<a id="create-user-and-service-accounts" class="xliff"></a>
Hozzon létre felhasználói és szolgáltatásfiókokat a MIM szolgáltatás és a portál beállításához. Ezek a fiókok a priv.contoso.local tartomány Felhasználók tárolójába kerülnek.

1. A kiszolgáló újraindítása után jelentkezzen be a PRIVDC számítógépre tartományi rendszergazdaként (PRIV\\Rendszergazda).

2. Indítsa el a PowerShellt, és írja be következő parancsokat. A 'Pass@word1' jelszó csak példaként szolgál, a fiókokhoz használjon más jelszót.

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### Naplózási és a bejelentkezési jogok konfigurálása
<a id="configure-auditing-and-logon-rights" class="xliff"></a>

Be kell állítania a naplózást ahhoz, hogy létre lehessen hozni a PAM konfigurációját az erdőkre vonatkozóan.  

1. Győződjön meg arról, hogy tartományi rendszergazdaként van bejelentkezve (PRIV\\Rendszergazda).

2. Válassza a **Start** > **Felügyeleti eszközök** > **Csoportházirend-kezelés** lehetőséget.

3. Lépjen az **Erdő: priv.contoso.local** > **Tartományok** > **priv.contoso.local** > **Tartományvezérlők** > **Alapértelmezett tartományvezérlői házirend** elemhez. Megjelenik egy figyelmeztető üzenet.

4. Kattintson a jobb gombbal az **Alapértelmezett tartományvezérlői házirend** elemre, majd válassza a **Szerkesztés** lehetőséget.

5. A Csoportházirendkezelés-szerkesztő konzolfáján jelölje ki a **Számítógép konfigurációja** > **Házirendek** > **Windows-beállítások** > **Biztonsági beállítások** > **Helyi házirend** > **Naplózási házirend** elemet.

6. A Részletek ablaktáblán kattintson a jobb gombbal a **Fiókkezelés naplózása** elemre, és válassza a **Tulajdonságok** parancsot. Kattintson **A következő házirend-beállítások megadása** elemre, jelölje be a **Siker** jelölőnégyzetet, jelölje be a **Hiba** jelölőnégyzetet, és kattintson az **Alkalmaz**, majd az **OK** gombra.

7. A Részletek ablaktáblán kattintson a jobb gombbal a **Címtárszolgáltatás-hozzáférés naplózása** elemre, és válassza a **Tulajdonságok** parancsot. Kattintson **A következő házirend-beállítások megadása** elemre, jelölje be a **Siker** jelölőnégyzetet, jelölje be a **Hiba** jelölőnégyzetet, és kattintson az **Alkalmaz**, majd az **OK** gombra.

8. Lépjen a **Számítógép konfigurációja** > **Házirendek** > **Windows-beállítások** > **Biztonsági beállítások** > **Fiókházirendek** > **Kerberos-irányelv** elemhez.

9. A Részletek ablaktáblában kattintson a jobb gombbal a **Felhasználói jegy maximális élettartama** elemre, és válassza a **Tulajdonságok** parancsot. Kattintson **A következő házirend-beállítások megadása** elemre, az órák számát állítsa *1*-re, és kattintson az **Alkalmaz**, majd az **OK** gombra. Vegye figyelembe, hogy az ablakban található egyéb beállítások is megváltoznak.

10. A Csoportházirend kezelése ablakban válassza az **Alapértelmezett tartományházirend** elemet, kattintson rá a jobb gombbal, és válassza a **Szerkesztés** parancsot.

11. Bontsa ki a **Számítógép konfigurációja** > **Házirendek** > **Windows-beállítások** > **Biztonsági beállítások** > **Helyi házirendek** elemet, és válassza a **felhasználói jogok kiosztása** lehetőséget.

12. A Részletek ablaktáblában kattintson a jobb gombbal a **Kötegelt munka bejelentkezésének megtagadása** elemre, és válassza a **Tulajdonságok** parancsot.

13. Jelölje be **A következő házirend-beállítások megadása** jelölőnégyzetet, kattintson a **Felhasználó vagy csoport hozzáadása** elemre, írja be a felhasználó és a csoport nevének mezőjébe a *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* értéket, és kattintson az **OK** gombra.

14. Kattintson az **OK** gombra az ablak bezárásához.

15. A Részletek ablaktáblában kattintson a jobb gombbal a **Távoli asztali szolgáltatások használatával történő bejelentkezés tiltása** elemre, és válassza a **Tulajdonságok** parancsot.

16. Jelölje be **A következő házirend-beállítások megadása** jelölőnégyzetet, kattintson a **Felhasználó vagy csoport hozzáadása** elemre, írja be a felhasználó és a csoport nevének mezőjébe a *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* értéket, és kattintson az **OK** gombra.

17. Kattintson az **OK** gombra az ablak bezárásához.

18. Zárja be a Csoportházirendkezelés-szerkesztő ablakát és a Csoportházirend kezelése ablakot.

19. Rendszergazdaként nyisson meg egy PowerShell-ablakot, és írja be a következő parancsot a tartományvezérlőnek a csoportházirend-beállításokkal történő frissítéséhez.

  ```
  gpupdate /force /target:computer
  ```

  Egy perc elteltével a következő üzenet jelenik meg: „A számítógép-házirend frissítése sikeresen befejeződött.”


### A DNS-névátirányítás konfigurálása a PRIVDC számítógépen
<a id="configure-dns-name-forwarding-on-privdc" class="xliff"></a>

A PRIVDC számítógépen a PowerShell használatával konfigurálja a DNS-névátirányítást, hogy a PRIVDC tartomány felismerje a többi meglévő erdőt.

1. Indítsa el a PowerShellt.

2. Mindegyik tartománynál a meglévő erdők tetején írja be a következő parancsot, amely megadja a meglévő DNS-tartományt (például contoso.local), valamint az adott tartomány főkiszolgálójának IP-címét.  

  Ha az előző lépésben az egyetlen contoso.local tartományt hozta létre, akkor adja meg a *10.1.1.31* értéket a CORPDC számítógép virtuális hálózati IP-címeként.

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> A többi erdőnek is képesnek kell lennie arra, hogy a PRIV erdő DNS-kéréseit átirányítsa erre a tartományvezérlőre.  Több meglévő Active Directory-erdő esetén mindegyik erdőbe fel kell vennie egy feltételes DNS-továbbítót is.

### A Kerberos konfigurálása
<a id="configure-kerberos" class="xliff"></a>

1. A PowerShell használatával vegyen fel egyszerű szolgáltatásneveket (SPN), hogy a SharePoint, a PAM REST API és a MIM szolgáltatás használni tudja a Kerberos-hitelesítést.

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> A dokumentumban található következő lépések bemutatják, hogyan telepítheti a MIM 2016 kiszolgálói összetevőit egyetlen számítógépre. Ha a magas rendelkezésre állás érdekében további kiszolgáló hozzáadását tervezi, akkor a Kerberos további konfigurálására lesz szükség a [FIM 2010: A Kerberos-hitelesítés beállítása](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx) című témakörben leírtak szerint.

### Delegálás konfigurálása a MIM szolgáltatásfiókok hozzáférésének megadásához
<a id="configure-delegation-to-give-mim-service-accounts-access" class="xliff"></a>

Végezze el a következő lépéseket a PRIVDC számítógépen tartományi rendszergazdaként.

1. Jelenítse meg az **Active Directory - felhasználók és számítógépek** ablakot.  
2. Kattintson a jobb gombbal a **priv.contoso.local** tartományra, és válassza a **Vezérlés delegálása** parancsot.  
3. A Kijelölt felhasználók és csoportok lapon kattintson a **Hozzáadás** gombra.  
4. A Felhasználók, számítógépek vagy csoportok kiválasztása ablakban írja be a *mimcomponent; mimmonitor; mimservice* nevet, és kattintson a **Névellenőrzés** gombra. Miután a nevek alatt megjelent az aláhúzás, kattintson az **OK**, majd a **Tovább** gombra.  
5. A gyakori feladatok listáján jelölje ki a **Felhasználói fiókok létrehozása, törlése és kezelése** és az **Egy csoport tagságának módosítása** elemet, és kattintson a **Tovább**, majd a **Befejezés** gombra.

6. Kattintson ismét a jobb gombbal a **priv.contoso.local** tartományra, és válassza a **Vezérlés delegálása** parancsot.  
7. A Kijelölt felhasználók és csoportok lapon kattintson a **Hozzáadás** gombra.  
8. A Felhasználók, számítógépek vagy csoportok kiválasztása ablakban adja meg a *MIMAdmin* nevet, és kattintson a **Névellenőrzés** gombra. Miután a nevek alatt megjelent az aláhúzás, kattintson az **OK**, majd a **Tovább** gombra.  
9. Válassza az **Egyéni feladat** lehetőséget, és alkalmazza az **Ez a mappa** elemre az **Általános engedélyek** beállítás megadásával.    
10. Az engedélyek listájában válassza a következőket:  
  - **Olvasás**  
  - **Írás**  
  - **Az összes gyermekobjektum létrehozása**  
  - **Az összes gyermekobjektum törlése**  
  - **Az összes tulajdonság olvasása**  
  - **Az összes tulajdonság írása**  
  - **SID-előzmények áttelepítése**  
  Kattintson a **Tovább**, majd a **Befejezés** gombra.

11. Kattintson ismét a jobb gombbal a **priv.contoso.local** tartományra, és válassza a **Vezérlés delegálása** parancsot.  
12. A Kijelölt felhasználók és csoportok lapon kattintson a **Hozzáadás** gombra.  
13. A Felhasználók, számítógépek vagy csoportok kiválasztása ablakban adja meg a *MIMAdmin* nevet, majd kattintson a **Névellenőrzés** gombra. Miután a nevek alatt megjelent az aláhúzás, kattintson az **OK**, majd a **Tovább** gombra.  
14. Válassza az **Egyéni feladat** lehetőséget, alkalmazza az **Ez a mappa** elemre, majd kattintson a **Csak felhasználói objektumok** elemre.    
15. Az engedélyek listájában válassza a **Jelszó módosítása** és a **Jelszó alaphelyzetbe állítása** elemet. Kattintson a **Tovább**, majd a **Befejezés** gombra.  
16. Zárja be az Active Directory – felhasználók és számítógépek beépülő modult.

17. Nyisson meg egy parancssort.  
18. Ellenőrizze az Admin SD Holder objektum hozzáférés-vezérlési listáját a PRIV tartományokban. Ha a tartomány például „priv.contoso.local” volt, írja be a következő parancsot:  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
  ```
19. Ha szükséges, módosítsa a hozzáférés-vezérlési listát úgy, hogy a MIM szolgáltatás és a MIM összetevő frissíteni tudja az adott hozzáférés-vezérlési lista által védett csoportok tagságát.  Írja be a következő parancsot:  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
  ```
20. Indítsa újra a PRIVDC kiszolgálót, hogy a változtatások érvénybe lépjenek.

## PRIV munkaállomás előkészítése
<a id="prepare-a-priv-workstation" class="xliff"></a>

Ha még nem rendelkezik munkaállomással, amely a PRIV tartományhoz fog tartozni a PRIV erőforrások (például a MIM) karbantartásának végrehajtásához, akkor kövesse az alábbi utasításokat a munkaállomás előkészítéséhez.  

### A Windows 8.1 vagy a Windows 10 Enterprise telepítése
<a id="install-windows-81-or-windows-10-enterprise" class="xliff"></a>

Egy új virtuális gépen, amelyen még nincs telepített szoftver, telepítse a Windows 8.1 Enterprise vagy a Windows 10 Enterprise verziót. Ez lesz a *„PRIVWKSTN”* számítógép.

1. A telepítéshez használja az expressz beállításokat.

2. Vegye figyelembe, hogy előfordulhat, hogy a telepítés nem fog tudni csatlakozni az internethez. Kattintson a **Helyi fiók létrehozása** elemre. Adjon meg más felhasználónevet, ne használja a „Rendszergazda” vagy az „Ilona” nevet.

3. A Vezérlőpulton adjon statikus IP-címet a számítógépnek a virtuális hálózatban, és az adapter elsődleges DNS-kiszolgálójának állítsa be a PRIVDC kiszolgáló DNS-kiszolgálóját.

4. A Vezérlőpulton csatlakoztassa a PRIVWKSTN számítógépet a priv.contoso.local tartományhoz. Ehhez meg kell adnia a PRIV tartomány rendszergazdai hitelesítő adatait. Amikor ezzel elkészült, indítsa újra a PRIVWKSTN számítógépet.

A további részleteket [az emelt szintű hozzáféréssel rendelkező munkaállomások biztonságossá tételét](https://technet.microsoft.com/en-us/library/mt634654.aspx) ismertető cikk tartalmazza.

A következő lépésben egy PAM-kiszolgáló előkészítésével foglalkozunk.

>[!div class="step-by-step"]
[« 1. lépés](step-1-prepare-corp-domain.md)
[3. lépés »](step-3-prepare-pam-server.md)


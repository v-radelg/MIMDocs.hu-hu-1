---
title: Csoportosan felügyelt szolgáltatásfiók MIM szolgáltatások átalakítás |} Microsoft Docs
description: Csoportosan felügyelt szolgáltatásfiók konfigurálásához szükséges alapvető lépéseket ismertető témakörben.
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.openlocfilehash: 090e82cac6c734beb9767e2d2e6230320e44c26f
ms.sourcegitcommit: c6cb2556bb9f2256b959a3c95db7ca5bbfc2b437
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/28/2018
ms.locfileid: "37065142"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>A MIM egyes szolgáltatások átalakítása csoportosan felügyelt szolgáltatásfiók

Ez az útmutató az alapvető lépéseken támogatott szolgáltatások csoportosan felügyelt szolgáltatásfiók konfigurálásához lesz lépéseit. A csoportosan felügyelt szolgáltatásfiók átalakítása érték könnyen után előre konfigurálhatja a környezetben.

A gyorsjavítás szükséges: \<legújabb KB mutató hivatkozás\>

Támogatott:

-   MIM Synchronization service(FIMSynchronizationService)
-   A MIM Service(FIMService)
-   A MIM jelszó-regisztráció
-   A MIM jelszó-visszaállítás
-   A PAM-figyelési Service(PamMonitoringService)
-   A PAM összetevő szolgáltatása (PrivilegeManagementComponentService)

Nem támogatott:

-   MIM-portál nem támogatott a sharepoint-környezet része, és a farm üzemmódban történő telepítéséhez kellene és [SharePointServer konfigurálja automatikus jelszó módosítása](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Minden felügyeleti ügynök
-   Microsoft tanúsítványkezelés
-   BHOLD

<a name="general-information"></a>Általános információk 
--------------------

Olvasási beállításának befejezése és megértése szükséges

-   [Csoportosan felügyelt szolgáltatásfiókok – áttekintés](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/en-us/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/en-us/library/jj128430(v=ws.11).aspx>

A windows-alapú tartományvezérlőn fist lépés

1.  Szükség esetén hozzon létre a fő terjesztési Services(KDS) legfelső szintű kulcs (tartomány csak egyszer). Legfelső szintű kulcs használatával a KDS (mellett más információk) a tartományvezérlők jelszavak előállításához.

    -   -KDSRootKey – effectiveimmediately parancs

    -   "– Effectiveimmediately parancs" azt jelenti, hogy Várjon, amíg legfeljebb \~10 óra / kell replikálni a minden tartományvezérlő. Ez volt a két tartományvezérlő körülbelül 1 óra.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synchronization Service
-----------------------

1.  Fist lépésben létrehoz egy csoportot hívás "MIMSync_Servers", és minden szinkronizálási kiszolgálók hozzáadása ehhez a csoporthoz.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  A windows PowerShell, majd végrehajtása alatt parancs tartományi rendszergazdaként számítógépfiók már csatlakozott a tartományhoz

    -   Új ADServiceAccount-MIMSyncGMSAsvc - DNSHostName MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers" nevet

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Szinkronizálási érhető el a GSMA részleteit:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Ha a jelszóváltozás-értesítési szolgáltatás fut, akkor a delegálás frissítése

    -   Set-ADServiceAccount-identitás MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Ezután a szinkronizálási szolgáltatások ügyeljen arra, hogy a titkosítási kulcs biztonsági mentése, módosítási üzemmód telepítése után fog kért

    -   Keresse meg a kiszolgálót, amely a szinkronizálási szolgáltatás telepítve van a szinkronizálási szolgáltatás kulcs felügyeleti eszköz

    -   Alapértelmezés szerint a ** exportálási kulcs set ** már be van jelölve

    -   Kattintson a **következő**

    -   Most kéri adja meg a meglévő szinkronizálási fiók adatait

    -   Adja meg, és ellenőrizze a FIM szinkronizálási fiók adatait

        -   Szolgáltatásfiók neve – a Synchronization Service fiók a kezdeti telepítés során használt fiók neve

        -   Jelszó - szinkronizálás szolgáltatás fiókjának jelszava

        -   Tartomány -, amely a szinkronizálási szolgáltatásfiók egymástól a tartományban

    -   Kattintson a **következő**

    -   Ha valami nem megfelelően, akkor a következő hibaüzenet jelenik

    -   Miután sikeresen megadta a fiókadatokat, választhat, hogy a biztonsági mentési titkosítási kulcs (Exportálás fájl helye) céljának módosítása

        -   Alapértelmezés szerint az Exportálás elérési útja **C:\\Windows\\system32**\\miiskeys-1.bin.

4. Telepítse a Microsoft Identity Manager SP1 szinkronizálási szolgáltatás build 4.4.1302.0. a mennyiségi licenc letöltőközpontból vagy MSDN letöltési helyen találhatók. Telepítés befejeződése után győződjön meg arról, kulcskészlet miiskeys.bin menti.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Telepítse a legújabb [gyorsjavítás 4.5.x.x](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/version-history) vagy újabb.

- Többször lett leállítása FIM szinkronizálási szolgáltatás.
- Vezérlőpult Programok és szolgáltatások a Microsoft Identity Manager
- Változás - szinkronizálási szolgáltatás\> tovább -\> konfigurálása -\> tovább

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Törölje a fiók neve
-  Típus szolgáltatásfiók neve **MIMSyncGMSA** rendelkező \$ ennek a szimbólum a
- képernyőkép. Jelszót hagyja üresen.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Tovább -\> tovább -\> telepítése
- A mentett .bin fájl kulcskészlet visszaállíthatók.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> SQL engedély hozzá-fiók létrehozása bejelentkezési azonosítóhoz ezért engedélyeznie kell a felhasználó módosítási mód engedély fiók és a dbo hozzáadásához a szinkronizálási szolgáltatás adatbázis alkalmazása

## <a name="mim-service"></a>MIM szolgáltatás
-----------

>[!IMPORTANT]
>Az alábbi eljárást kell használni, először alakítsa át a MIM szolgáltatás kapcsolatos fiókoknak csoportosan felügyelt szolgáltatásfiók fiókok. A PowerShell-parancsmagok a függelékben feljegyzett csak segítségével módosítható a fiók adatait, ha a kezdeti konfiguráció lett done.*

1.  Hozzon létre csoportosan felügyelt fiókokat a MIM szolgáltatás, a PAM Rest API, PAM-figyelőszolgáltatást, PAM összetevő szolgáltatása, önkiszolgáló jelszó-változtatási, SSPR visszaállítási portál.

    -   Ellenőrizze, hogy a csoportosan felügyelt szolgáltatásfiók delegálása és SPN frissítése
        -   Set-ADServiceAccount-identitás \<fiók\> - ServicePrincipalNames \@{Hozzáadás = "\<SPN\>"}
        -   Delegálás
            -   Set-ADServiceAccount-identitás \<gsmaaccount\> - TrustedForDelegation \$igaz
        -   A korlátozott delegálás
            -   \$delspns = "http/mim", "http/mim.contoso.com"
            -   Új ADServiceAccount-név \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<csoport\> - ServicePrincipalNames \$SPN - OtherAttributes \@{"msDS-AllowedToDelegateTo" =\$delspns}

2.  Adja hozzá a MIM szolgáltatás szinkronizálása csoportokban. Sspr szükség.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **MEGJEGYZÉS:**.  Ismert hiba, amely felügyelt fiók használó szolgáltatások lefagy-kiszolgálóhoz, mert a Microsoft kulcsszolgáltató szolgáltatás újraindítása után nem indult el a Windows újraindítása után. Szolgáltatás nem indítható el, és a Windows nem indítható újra túl. A hiba csak reprodukálható legalább Windows Server 2012 R2 rendszeren. A probléma megoldása parancs futtatása 

-   **sc triggerinfo kdssvc start/networkon**

    a Microsoft kulcsszolgáltató szolgáltatás elindítására, ha a hálózat (általában rendszerindító ciklusban korai).

    Hasonló problémáról lásd: <https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Emelt szintű MSI a MIM szolgáltatás futtatásához, és válassza ki a módosítása.

5.  "Konfigurálása fő kiszolgáló oldalán kapcsolat" jelölje be "Használhat más fiók Exchange-hez (a felügyelt fiókokhoz)" jelölőnégyzetet. Itt egy lehetőség, hogy a régi fiókot használjon, amely-postaládával rendelkező vagy felhő postaláda használni fog.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  A "Konfigurálása a MIM szolgáltatás-fiók" lap típus szolgáltatásfiók \$ szimbólum végén. E-mail fiók jelszavának beírásával. Le kell tiltani a szolgáltatásfiók jelszavát.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  LogonUser függvény felügyelt fiókok nem működik, mint következő lapon fog kell figyelmeztetés "Ellenőrizze, ha a szolgáltatásfiók aktuális konfigurációja biztonságos".

![CID:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  A "Konfigurálása Privileged Access Management REST API" lapon adja meg Application Pool Account Name rendelkező \$ szimbólumának végén, és a jelszó mezőt hagyja üresen.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  "A PAM összetevő szolgáltatás beállítása" lapon írja be a szolgáltatás fiók nevét \$ szimbólumának végén, és a jelszó mezőt hagyja üresen.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.png\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  A "Privileged Access felügyeleti figyelési szolgáltatás konfigurálása" lapon írja be a szolgáltatás fiók nevét \$ szimbólumának végén, és a jelszó mezőt hagyja üresen.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  "Konfigurálása a MIM jelszó-regisztrálási portál" lapon írja be azt a fióknevet \$ szimbólumának végén, és a jelszó mezőt hagyja üresen.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  "Konfigurálása a MIM jelszó-változtatási portál" lapon írja be azt a fióknevet \$ szimbólumának végén, és a jelszó mezőt hagyja üresen.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  A telepítés befejezéséhez.

Megjegyezés:

-  A telepítés után két új kulcsot a beállításjegyzékben által létrehozott elérési útja
    - "HKEY_LOCAL_MACHINE\\szoftver\\Microsoft\\Forefront Identity
    - Manager\\2010\\szolgáltatás "titkosított Exchange jelszó tárolására. Egy a
    - Exchange online-hoz, és egy másik (az egyik legyen kell helyszíni Exchange-hez
    - üres).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Frissítse a jelszavát, hogy azt a megadott parancsfájl [itt letöltése](microsoft-identity-manager-2016-gmsascript.md) , az ügyfél nem lesz módváltás futtatásához

- Exchange titkosításához jelszó a telepítő további szolgáltatás létrehozása és
    - a felügyelt fiókkal futtatja azt. A következő üzenet megjelenik
    - Alkalmazások telepítése során eseménynaplójában.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)

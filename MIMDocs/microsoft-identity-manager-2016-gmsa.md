---
title: A MIM egyes szolgáltatások átalakítása csoportosan felügyelt szolgáltatásfiók |} A Microsoft Docs
description: Csoportosan felügyelt szolgáltatásfiók konfigurálása alapvető lépéseit ismertető témakör.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 96d375d82a71a21f0be444d628f387c4e1ffdd09
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825773"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>A MIM egyes szolgáltatások átalakítása csoportosan felügyelt szolgáltatásfiók

Ez az útmutató fog végighaladhat az alapvető lépéseken, a támogatott szolgáltatások csoportosan felügyelt szolgáltatásfiók konfigurálásához. A csoportosan felügyelt szolgáltatásfiók átalakítása folyamat is egyszerűen, miután előre konfigurálta a környezetben.

A gyorsjavítás szükséges: \<legújabb KB mutató hivatkozás\>

Támogatott:

-   A MIM szinkronizálási service(FIMSynchronizationService)
-   A MIM Service(FIMService)
-   A MIM jelszó-regisztráció
-   A MIM jelszó-visszaállítás
-   A PAM-figyelési Service(PamMonitoringService)
-   A PAM összetevő szolgáltatása (PrivilegeManagementComponentService)

Nem támogatott:

-   MIM-portál nem támogatott a sharepoint-környezet része és a farm üzemmódban történő telepítése kell és [SharePointServer jelszavának automatikus módosítását konfigurálása](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Az összes felügyeleti ügynökök
-   A Microsoft tanúsítványkezelés
-   BHOLD

<a name="general-information"></a>Általános információk 
--------------------

Olvasási szükséges a telepítő majd ismertetése

-   [Csoport felügyelt szolgáltatásfiókok – áttekintés](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Első lépésként a windows tartományvezérlő

1.  A kulcs terjesztési Services(KDS) legfelső szintű kulcs (csak egyszer tartományonként) létrehozásához, ha szükséges. Legfelső szintű kulcs a KDS (együtt az egyéb információk) tartományvezérlők a jelszavak használt.

    -   -KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately" azt jelenti, hogy várjon legfeljebb \~10 óra / kell replikálnia a az összes Tartományvezérlőre. Ez a két tartományvezérlőt körülbelül egy óra volt.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synchronization Service
-----------------------

1.  Hozzon létre egy "MIMSync_Servers" nevű csoportot, és az összes szinkronizálási kiszolgálók hozzáadása ehhez a csoporthoz.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  A windows PowerShell, majd hajtsa végre alábbi a parancs tartományi rendszergazdaként a számítógépfiók már csatlakozott a tartományhoz

    -   Új ADServiceAccount-MIMSyncGMSAsvc - DNSHostName MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers" nevet.

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Szinkronizálás a GSMA részleteinek lekérése:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Ha a jelszóváltoztatás-értesítési szolgáltatás szolgáltatás fut, szüksége lesz a delegálás frissítése

    -   Set-ADServiceAccount-identitás MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Ezután a Synchronization services ügyeljen arra, hogy a titkosítási kulcs biztonsági mentése, módosítás üzemmód telepítése után fog kérni

    -   Keresse meg a kiszolgálót, amelyet a szinkronizálási szolgáltatás telepítve van a szinkronizálási szolgáltatás kulcskezelés eszköz

    -   Alapértelmezés szerint a **kulcskészletben exportálása** már be van jelölve

    -   Kattintson a **tovább**

    -   Most kéri, adja meg a meglévő szinkronizálási fiókadatok

    -   Adja meg, és ellenőrizze a FIM szinkronizálási fiókadatok

        -   Szolgáltatásfiók neve – a szinkronizálási szolgáltatás fiók a kezdeti telepítés során használt fiók neve

        -   Jelszó - szinkronizálási szolgáltatás fiókjának jelszava

        -   Tartomány - tartományt, amely a szinkronizálási szolgáltatásfiókot az egymástól

    -   Kattintson a **tovább**

    -   Ha valami nem megfelelően, akkor a következő hibaüzenetet kap

    -   Sikeresen megadta a fiók adatait, most megjelenik, hogy a titkosítási kulcs biztonsági mentése (Exportálás elérési útja) céljának módosítása

        -   Alapértelmezés szerint az exportálási fájl helye a **C:\\Windows\\system32**\\miiskeys-1.bin.

4. Telepítse a Microsoft Identity Manager SP1 szinkronizálási szolgáltatás build 4.4.1302.0. a mennyiségi licenc letöltőközpontból vagy MSDN letöltése helyen találhatók. Telepítés befejezése után ellenőrizze, hogy, kulcskészlet miiskeys.bin menti.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Telepítse a legújabb [gyorsjavítás 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) vagy újabb.

- Egyszer javítani leállítása FIM szinkronizálási szolgáltatás.
- Vezérlőpult Programok és szolgáltatások a Microsoft Identity Manager
- Változás - szinkronizálási szolgáltatás\> tovább -\> konfigurálása –\> tovább

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Törölje a fiók nevét
-  Szolgáltatásfiók neve típusa **MIMSyncGMSA** a \$ szimbólum, ahogyan a
- képernyőkép. Jelszó hagyja üresen.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Tovább -\> tovább -\> telepítése
- Állítsa vissza a .bin fájl mentése kulcskészletet.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> Hozzáadott SQL engedély fiók jön létre a bejelentkezéshez ezért engedélyeznie kell a felhasználó módosítási mód engedéllyel a szinkronizálási szolgáltatás adatbázis-fiók és a dbo hozzáadandó alkalmazása

## <a name="mim-service"></a>MIM szolgáltatás
-----------

>[!IMPORTANT]
>A következő folyamat kell használni, amikor először konvertálása a MIM szolgáltatás kapcsolatos fiókoknak gMSA-fiók. A PowerShell-parancsmagok a függelékben feljegyzett csak akkor használható a fiókadatok módosítása után az eredeti konfigurációval lett done.*

1.  Hozzon létre csoportosan felügyelt fiókokat a MIM szolgáltatás, a PAM Rest API-t, PAM-figyelőszolgáltatást, PAM összetevő szolgáltatása, önkiszolgáló jelszó-változtatási, SSPR visszaállítási portál.

    -   Ellenőrizze, hogy a csoportosan felügyelt szolgáltatásfiókok delegálási és az egyszerű szolgáltatásnév frissítése
        -   Set-ADServiceAccount-identitás \<fiók\> - ServicePrincipalNames \@{Add = "\<SPN\>"}
        -   Delegálás
            -   Set-ADServiceAccount-identitás \<gsmaaccount\> - TrustedForDelegation \$igaz
        -   A korlátozott delegálás
            -   \$delspns = ' http/mim', "http/mim.contoso.com"
            -   Új ADServiceAccount-név \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<csoport\> - ServicePrincipalNames \$SPN - OtherAttributes \@{"msDS-AllowedToDelegateTo" =\$delspns}

2.  Fiók hozzáadása a szinkronizálási csoportok MIM szolgáltatáshoz. Az SSPR szükség.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **MEGJEGYZÉS:**.  Ismert hiba, amely miatt a Microsoft kulcsszolgáltató szolgáltatás újraindítása után lefagy felügyelt fiókot használó szolgáltatások a Windows újraindítása után nem indult el. Nem sikerült elindítani a szolgáltatást, és a Windows nem indítható túl. A hiba csak reprodukálható legalább Windows Server 2012 R2 rendszeren. A probléma megoldása futtatása paranccsal 

-   **sc start/triggerinfo kdssvc networkon**

    a Microsoft kulcsszolgáltató szolgáltatás indítása, ha a hálózat (általában a rendszerindító ciklus korai).

    Hozzászólás hasonló problémával kapcsolatban lásd: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Emelt szintű MSI a MIM szolgáltatás futtatásához, és válassza ki a módosítás.

5.  A "Konfigurálás fő kiszolgáló kapcsolati lap" Ellenőrizze a "Használjon másik fiókot az Exchange-hez (a felügyelt fiókkal)" jelölőnégyzetet. Itt kell a régi fiókot használjon, amely rendelkezik egy postaládát vagy felhőbeli postaláda használatára.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  A "Konfigurálása a MIM szolgáltatás fiók" oldal típus szolgáltatásfiókhoz \$ szimbólum végén. Írja be a szolgáltatás E-mail fiókja jelszavát is. Szolgáltatásfiók jelszavának le kell tiltani.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  LogonUser függvény felügyelt fiókok esetében nem működik, mert tovább lap lesz kell figyelmeztetés "Ellenőrizze, hogy e szolgáltatásfiók aktuális konfigurációja a biztonságos".

![CID:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  "Konfigurálása Privileged Access Management REST API" lapon írja be az alkalmazáskészlet-fiók neve \$ végén szimbólum megadásával, valamint jelszó mezőt hagyja üresen.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  "A PAM összetevő szolgáltatás beállítása" lapon írja be a szolgáltatás fiók nevét a \$ végén szimbólum megadásával, valamint jelszó mezőt hagyja üresen.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.png\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  A "Privileged Access Management figyelési szolgáltatás konfigurálása" lapon írja be a szolgáltatás fiók nevét \$ végén szimbólum megadásával, valamint jelszó mezőt hagyja üresen.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  A "Konfigurálása a MIM jelszó-regisztrálási portál" lapon írja be a fiók nevét \$ végén szimbólum megadásával, valamint jelszó mezőt hagyja üresen.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  A "Konfigurálása a MIM jelszó-változtatási portál" lapon írja be a fiók nevét \$ végén szimbólum megadásával, valamint jelszó mezőt hagyja üresen.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  A telepítés befejezéséhez.

Megjegyezés:

-  A telepítés után két új kulcsok jönnek létre beállításjegyzékbeli elérési út alapján
    - "HKEY_LOCAL_MACHINE\\szoftver\\Microsoft\\Forefront Identity
    - Kezelő\\2010\\szolgáltatás "Exchange titkosított jelszót tárolásához. Egy a
    - Exchange online-hoz és a egy másik (az egyiket kell lennie a helyszíni Exchange-hez
    - üres).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- A jelszó frissítése, hogy megadott parancsfájl [innen tölthet le](microsoft-identity-manager-2016-gmsascript.md) így az ügyfél nem kell futtatnia a módváltás

- Exchange titkosításához jelszó a telepítő további szolgáltatást hoz létre, és
    - a felügyelt fiókkal futtatja azt. A következő üzeneteket fogja tartalmazni
    - Alkalmazás eseménynaplóját a telepítés során.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)

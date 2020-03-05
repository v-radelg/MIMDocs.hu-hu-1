---
title: A gMSA-specifikus szolgáltatások átalakítása a következőre | Microsoft Docs
description: A gMSA konfigurálásának alapvető lépéseit ismertető témakör.
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 49216a2d2077dd1be83f17719e996a20abb61cf8
ms.sourcegitcommit: d98a76d933d4d7ecb02c72c30d57abe3e7f5d015
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/05/2020
ms.locfileid: "78289515"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>GMSA-specifikus szolgáltatások átalakítása

Ez az útmutató végigvezeti a támogatott szolgáltatások gMSA konfigurálásához szükséges alapvető lépéseken. A gMSA konvertálásának folyamata egyszerű, ha előre konfigurálja a környezetet.

Gyorsjavítás szükséges: [4.5.26.0 vagy újabb](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history)

Támogatott:

-   Webkiszolgálói szinkronizációs szolgáltatás (FIMSynchronizationService)
-   FIMService-szolgáltatás
-   A regisztrációs adatbázis jelszava regisztrációja
-   A rendszerállapot-visszaállítás jelszava
-   PAM-figyelési szolgáltatás (PamMonitoringService)
-   PAM-összetevő szolgáltatás (PrivilegeManagementComponentService)

Nem támogatott:

-   A webszolgáltatási portál nem támogatott, mert a SharePoint-környezet része, és Farm módban kell telepítenie, és az [automatikus jelszó-módosítást a SharePoint Server-ben kell konfigurálnia](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change) .
-   Minden felügyeleti ügynök
-   Microsoft Tanúsítványkezelő
-   BHOLD

<a name="general-information"></a>Általános információ 
--------------------

A telepítés és a megértés befejezéséhez szükséges olvasás

-   [Csoportosan felügyelt szolgáltatásfiókok – áttekintés](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Első lépés a Windows-tartományvezérlőn

1.  Szükség esetén hozza létre a Key Distribution Services (KDS) legfelső szintű kulcsát (csak egyszer egy tartományon). A legfelső szintű kulcsot a KDS szolgáltatás használja a DCs-ben (más információkkal együtt) a jelszavak létrehozásához.

    -   Add-KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately": várjon akár \~10 órát, vagy az összes TARTOMÁNYVEZÉRLŐre replikálni kell. Két tartományvezérlő esetében körülbelül 1 óra volt.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synchronization Service
-----------------------

1.  Hozzon létre egy "MIMSync_Servers" nevű csoportot, és vegye fel az összes szinkronizációs kiszolgálót ebbe a csoportba.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  A Windows PowerShellben futtassa az alábbi parancsot tartományi rendszergazdaként a tartományhoz már csatlakoztatott számítógépfiók használatával.

    -   New-ADServiceAccount-Name MIMSyncGMSAsvc-DNSHostName MIMSyncGMSAsvc.contoso.com-PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   A szinkronizálási GSMA részleteinek beolvasása:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Ha a PCN szolgáltatást futtatja, akkor frissítenie kell a delegálást.

    -   Set-ADServiceAccount-Identity MIMSyncGMSAsvc-ServicePrincipalNames \@{add = "PCNSCLNT/mimsync. contoso. com"}

3. A szinkronizálási szolgáltatásokban a következő lépésekkel készítsen biztonsági másolatot a titkosítási kulcsról, mivel azt a rendszer a módosítási mód telepítésekor kéri

    -   Azon a kiszolgálón, amelyen a szinkronizálási szolgáltatás telepítve van, keresse meg a szinkronizálási szolgáltatás kulcskezelő eszközét.

    -   Alapértelmezés szerint az **exportálási kulcs készlete** már ki van választva.

    -   Kattintson a **Next (tovább** ) gombra

    -   Ekkor a rendszer felszólítja, hogy adja meg a meglévő szinkronizálási fiók adatait

    -   A FIM Sync-fiók adatainak megadása és ellenőrzése

        -   Fióknév: a kezdeti telepítés során használt szinkronizálási szolgáltatásfiók fiókneve

        -   A szinkronizálási szolgáltatás fiókjának jelszava – jelszó

        -   Az a tartomány, amelyhez a szinkronizációs szolgáltatás fiókja kívül esik

    -   Kattintson a **Next (tovább** ) gombra

    -   Ha helytelenül adta meg a hibát, a következő hibaüzenet jelenik meg:

    -   Most, hogy sikeresen megadta a fiók adatait, megjelenik egy lehetőség, amellyel megváltoztathatja a biztonsági másolat titkosítási kulcsának célját (exportfájl helye).

        -   Alapértelmezés szerint az exportálási fájl helye a **C:\\Windows\\system32**\\miiskeys-1. bin.

4. Telepítse a Microsoft Identity Manager SP1 szinkronizációs szolgáltatást Build 4.4.1302.0. a mennyiségi licencelési letöltőközpontban vagy az MSDN letöltési webhelyén található. A telepítés befejezését követően mentse a kulcskészlet miiskeys. bin fájl mentését.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Telepítse a legújabb [gyorsjavítást 4.5. x. x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) vagy újabb verzióra.

- Miután a javította a FIM szinkronizációs szolgáltatást.
- Vezérlőpult Programok és szolgáltatások Microsoft Identity Manager
- Szinkronizálási szolgáltatás módosítása –\> Next-\> konfigurálás –\> következő

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Fiók nevének törlése
-  Írja be a következőt a **MIMSyncGMSA** neve \$ szimbólummal
- képernyőkép. Hagyja üresen a jelszót.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Következő –\> következő\> telepítése
- Állítsa vissza a kulcskészlet elemet a mentett. bin fájlból.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> A rendszer a fiókhoz tartozó SQL-engedélyekkel hozza létre a bejelentkezést, ezért engedélyeznie kell a felhasználó számára a módosítási mód engedélyezését a fiók-és dbo hozzáadásához a szinkronizációs szolgáltatás adatbázisában.

## <a name="mim-service"></a>MIM szolgáltatás
-----------

>[!IMPORTANT]
>A következő folyamatot kell használni, amikor először konvertálja a gMSA-fiókokkal kapcsolatos, a fakiszolgálói szolgáltatáshoz kapcsolódó fiókokat. A függelékben szereplő PowerShell-parancsmagok csak a kezdeti konfiguráció befejezése után módosíthatók a fiókadatok módosításához. *

1.  Csoportosan felügyelt fiókok létrehozása a SSPR-hez, a PAM REST API-hoz, a PAM-figyelési szolgáltatáshoz, a PAM összetevő-szolgáltatáshoz, a SSPR-visszaállítási portálhoz.

    -   Győződjön meg róla, hogy frissíti a gMSA delegálást és az SPN-t
        -   Set-ADServiceAccount-Identity \<fiók\>-ServicePrincipalNames \@{add = "\<SPN\>"}
        -   Delegálás
            -   Set-ADServiceAccount-Identity \<gsmaaccount\>-TrustedForDelegation \$igaz
        -   Korlátozott delegálás
            -   \$delspns = ' http/', ' http/. contoso. com '
            -   New-ADServiceAccount-Name \<gsmaaccount\>-DNSHostName \<gsmaaccount\>. contoso.com-PrincipalsAllowedToRetrieveManagedPassword \<csoport\>-ServicePrincipalNames \$SPN-OtherAttributes \@{"msDS-AllowedToDelegateTo" =\$delspns}

2.  Fiók hozzáadása a modulhoz szinkronizálási csoportokban. A SSPR szükséges.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **Megjegyzés:**  Ismert probléma, hogy a felügyelt fiókot használó szolgáltatások a kiszolgáló újraindítása után a Microsoft Key Distribution Service miatt nem indulnak el a Windows újraindítása után. A szolgáltatás nem indítható el, és a Windows nem indítható újra. A probléma legalább a Windows Server 2012 R2 rendszeren reprodukálható. A probléma megoldásához futtassa a parancsot. 

-   **SC triggerinfo kdssvc Start/Networkon**

    a Microsoft Key Distribution Service elindítása a hálózat bekapcsolásakor (jellemzően a rendszerindítási ciklus korai szakaszában).

    Lásd a hasonló problémákkal kapcsolatos vitát: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Futtassa a fakiszolgáló emelt szintű MSI-szolgáltatását, és válassza a módosítás lehetőséget.

5.  A "fő kiszolgáló kapcsolatainak konfigurálása" lapon jelölje be a "másik fiók használata az Exchange-hez (felügyelt fiókok esetében)" jelölőnégyzetet. Itt lehetősége lesz használni a régi fiókot, amely rendelkezik postaládával vagy Felhőbeli postaláda használatával.
    >[!NOTE]
    >Ha az **Exchange Online használata** lehetőség van kiválasztva, hogy a fakiszolgálói szolgáltatás engedélyezze a HKLM\SYSTEM\CurrentControlSet\Services\FIMService-hez tartozó engedélyezési válaszokat, a telepítés után a PollExchangeEnabled 1. számú beállításkulcs értékét 1-re kell állítania.
    
![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  A "címtárszolgáltatás-szolgáltatásfiók konfigurálása" lapon írja be a következőt: \$ szimbólum a végén. Írja be a szolgáltatás E-mail fiókjának jelszavát is. A szolgáltatásfiók jelszavát le kell tiltani.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Ahogy a LogonUser függvény nem működik a felügyelt fiókoknál, a következő oldal figyelmeztetést jelenít meg: "Ellenőrizze, hogy a szolgáltatásfiók biztonságos-e a jelenlegi konfigurációjában".

![CID: image007. png\@01D36EB 7.562 E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  A "Privileged Access Management REST API konfigurálása" lapon írja be az alkalmazáskészlet-fiók nevét \$ szimbólummal a végén, és hagyja üresen a jelszó mezőt.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  A "PAM-összetevő szolgáltatás konfigurálása" lapon írja be a következőt: szolgáltatásfiók neve \$ szimbólummal a végén, és hagyja üresen a jelszó mezőt.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID: image010. png\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  A "Privileged Access Management figyelési szolgáltatás konfigurálása" lapon írja be a következőt: szolgáltatásfiók neve \$ szimbólummal a végén, és hagyja üresen a jelszó mezőt.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  A "helyi jelszó-regisztrációs portál konfigurálása" lapon írja be a fiók nevét \$ szimbólummal a végén, és hagyja üresen a jelszó mezőt.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  A "jelszó-visszaállítási portál konfigurálása" lapon írja be a fiók nevét \$ szimbólummal a végén, és hagyja üresen a jelszó mezőt.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Fejezze be a telepítést.

Megjegyezés:

-  A telepítés után két új kulcs jön létre a beállításjegyzékben útvonal alapján
    - "HKEY_LOCAL_MACHINE\\szoftver\\Microsoft\\Forefront Identity
    - Manager\\2010\\szolgáltatás "titkosított Exchange-jelszó tárolásához. Egyet a következőhöz:
    - Exchange Online és egy másik a helyszíni Exchange-hez (az egyiknek a következőnek kell lennie:
    - üres).

![CID: image014. jpg\@01D36F 53.303 D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- A jelszó frissítéséhez egy parancsfájl [letöltését](microsoft-identity-manager-2016-gmsascript.md) adta meg, így az ügyfélnek nem kell módosítania a módosítási módot

- Az Exchange-jelszó titkosításához a telepítő további szolgáltatást hoz létre, és
    - a felügyelt fiók alatt futtatja. A következő üzenetek lesznek hozzáadva a következőhöz:
    - Alkalmazás-eseménynapló a telepítés során.

![CID: image016. jpg\@01D36F 53.303 D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)

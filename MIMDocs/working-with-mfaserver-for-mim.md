---
title: Az Azure multi-factor Authentication kiszolgáló SDK használata a PAM-re vagy az SSPR forgatókönyvek aktiválásához |} A Microsoft Docs
description: Állítsa be az Azure multi-factor Authentication kiszolgáló SDK egy második biztonsági réteggel, amikor a felhasználók szerepköröket aktiválnak a Privileged Access Management és az önkiszolgáló jelszó-visszaállítás.
keywords: ''
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 868e5c543fb55257f7de903eade69529d90ec895
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334363"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Azure multi-factor Authentication-kiszolgáló használata a PAM-re vagy az SSPR aktiválásához
A következő dokumentum ismerteti, hogyan állíthatja be az Azure MFA-kiszolgáló egy második biztonsági réteggel, amikor a felhasználók szerepköröket aktiválnak a jogosultságra hozzáférés-kezelés és önkiszolgáló jelszó-változtatási.

> [!IMPORTANT]
> Miatt elévülése az Azure multi-factor Authentication hitelesítés szoftverfejlesztői közleményt. Az Azure MFA SDK ügyfeleink 2018. November 14., a kivezetési dátum másnapi támogatott lesz. Új ügyfelek és a meglévő ügyfelek nem tudják SDK letöltéséhez már a klasszikus Azure portálon keresztül. Töltse le, hogy kell keresse fel a létrehozott csomagot biztosítunk MFA szolgáltatás hitelesítő adatai az Azure ügyfélszolgálatához. <br> A Microsoft fejlesztői csapat dolgozik MFA módosításai és az Azure multi-factor Authentication kiszolgáló SDK integrálásával.

Az alábbi cikkre szerkezeti lesz, a konfigurációjának frissítése és a egy egyszerű kapcsoló engedélyezésének lépései az Azure MFA SDK-t az Azure multi-factor Authentication kiszolgáló SDK kiadásakor, mert ez egy soron következő gyorsjavítás fognak szerepelni lásd [korábbi verziók ](/reference/version-history.md) hirdetmények. 

## <a name="prerequisites"></a>Előfeltételek

A MIM az Azure multi-factor Authentication-kiszolgáló használatához szükséges:

- Internet-hozzáférés minden MIM szolgáltatás és a PAM és az Azure MFA szolgáltatáshoz való SSPR, MFA-kiszolgáló
- Azure-előfizetés
- Telepítés már használja az Azure MFA SDK-val
- Azure Active Directory Premium licenc vagy valamilyen alternatív, Azure MFA-licencet biztosító megoldás a jelölt felhasználóknál
- Telefonszám az összes jelölt felhasználó esetén
- A MIM gyorsjavítás 4.5-ös verzióját. vagy nagyobb lásd [korábbi verziók](/reference/version-history.md) hirdetmények

## <a name="azure-multi-factor-authentication-server-configuration"></a>Az Azure multi-factor Authentication kiszolgáló konfigurációja 
> [!NOTE] 
> Egy érvényes SSL-tanúsítványt, az SDK-t telepíteni kell a konfigurációban. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>1. lépés: Az Azure multi-factor Authentication kiszolgáló letöltése az azure Portalról 
Jelentkezzen be a [az Azure portal](https://portal.azure.com/) , és töltse le az Azure MFA-kiszolgáló.
![működő-az-mfaserver-az-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>2. lépés: Az aktiváló hitelesítő adatok előállítása
Használja a **aktiváló hitelesítő adatok a használat megkezdéséhez hozzon létre** aktiváló hitelesítő adatok előállítása mutató hivatkozást. Létrehozott mentse későbbi használatra.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>3. lépés: Az Azure multi-factor Authentication-kiszolgáló telepítése
Miután letöltötte a kiszolgálót [telepítése](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) azt.  Az aktiváló hitelesítő adatok lesz szükség. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>4. lépés: Az IIS-webalkalmazás, az SDK-t futtató létrehozása
1. Nyissa meg az IIS-kezelőt ![működő-az-mfaserver-az-mim_iis. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Hozzon létre új webhely hívás "MIM MFASDK", a hivatkozás egy üres könyvtárra ![működő-az-mfaserver-az-mim_sdkweb. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Nyissa meg a multi-factor Authentication hitelesítés konzolt, majd kattintson a Web Service SDK ![működő-az-mfaserver-az-mim_sdkinstall. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Egyszer varázslók, kattintson a konfiguráció, a Select "MIM MFASDK" és az alkalmazáskészlet

> [!NOTE] 
> Varázsló kell létrehozni egy felügyeleti csoportot. További információ az Azure-ban található >> többtényezős hitelesítés az Azure multi-factor Authentication kiszolgáló dokumentációját.

5. Ezután a MIM szolgáltatás fiók nyissa meg a multi-factor Authentication konzolon válassza a "Felhasználók" importálni kell egy. Kattintson a "Importálás az Active Directory" b. Keresse meg a szolgáltatásfiókot, más néven "contoso\mimservice" c. Kattintson a "Importálása" és "Bezárás" ![működő-az-mfaserver-az-mim_importmimserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. A MIM szolgáltatás fiók engedélyezése a multi-factor Authentication konzolon szerkesztésével ![működő-az-mfaserver-az-mim_enableserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Frissítse az IIS-hitelesítés, a "MIM MFASDK" webhelyen. Először letiltjuk a "Névtelen", majd a Windows-hitelesítés engedélyezése" ![működő-az-mfaserver-az-mim_iisconfig. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Utolsó lépés: A MIM-szolgáltatásfiókhoz hozzáadása a PhoneFactor-Adminisztrátorok" ![működő-az-mfaserver-az-mim_addservicetomfaadmin. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>A MIM szolgáltatás az Azure multi-factor Authentication-kiszolgáló konfigurálása 

### <a name="step-1-patch-server-to-452020"></a>1. lépés: Patch kiszolgáló 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>2. lépés: Biztonsági mentés, és nyissa meg az MfaSettings.xml található a "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>3. lépés: A következő sorokat frissítése
1. A következő konfigurációs bejegyzéseket sorok eltávolítása/törlése <br>
&LT; LICENCKULCS &GT;&LT; / LICENCKULCS &GT;<br>
&LT; GROUP_KEY &GT;&LT; / GROUP_KEY &GT;<br>
&LT; CERT_PASSWORD &GT;&LT; / CERT_PASSWORD &GT;<br>
<CertFilePath></CertFilePath><br>

2. Frissítés vagy a következő sorokat ad hozzá a következő MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Indítsa újra a MIM szolgáltatás és funkció elérhetővé válik az Azure multi-factor Authentication-kiszolgáló tesztelése.

> [!NOTE] 
> MfaSettings.xml cserélje le a beállítás a 2. lépésben a biztonsági mentési fájlt tartalmazó visszaállítása


## <a name="next-steps"></a>További lépések

-    [Ismerkedés az Azure multi-factor Authentication-kiszolgálón](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Mi az Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Egyéni többtényezős hitelesítési API használata a PAM-re vagy az SSPR aktiválásához](Working-with-custommfaserver-for-mim.md)
- [A MIM verziókiadásai](./reference/version-history.md)
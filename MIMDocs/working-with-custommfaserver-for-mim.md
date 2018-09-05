---
title: Használjon egy másik többtényezős hitelesítési szolgáltató PAM aktiválásához egy API-n keresztül vagy az SSPR forgatókönyvben |} A Microsoft Docs
description: Állíthatja be egyéni többtényezős hitelesítés API-t egy második biztonsági szintként, ha a felhasználók szerepköröket aktiválnak a Privileged Access Management és az önkiszolgáló jelszó-visszaállítás használatához.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.openlocfilehash: 85ab84231c2ebfede125ffaf5fb39964e8a8450c
ms.sourcegitcommit: acb2c61831cb634278acc439d6d9496ff51a6a54
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/04/2018
ms.locfileid: "43694915"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Egy API-val egyéni multi-factor Authentication-szolgáltatót használ, a PAM-szerepkör aktiválása során, vagy az SSPR-ben

Ügyfeleink az Azure AD prémium vagy az Azure MFA is integrálhatja az Azure MFA két MIM forgatókönyv – a Privileged Access Management (PAM) szerepkör-aktiválás és az önkiszolgáló jelszó alaphelyzetbe állítása (SSPR).

A MIM-ügyfelek két további lehetősége van:

 - Használjon egy egyéni egy egyszeri jelszó kézbesítési szolgáltató, amely csak a MIM SSPR esetben értelmezhető és útmutató dokumentált [konfigurálása az önkiszolgáló jelszó-visszaállítási az OTP SMS-kapu](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10))
 - A telefonos egyéni többtényezős hitelesítési szolgáltatót használ. Ez egyaránt vonatkozik az ebben a cikkben leírt a MIM SSPR és a PAM forgatókönyveket

Ez a cikk ismerteti a MIM használata egy egyéni többtényezős hitelesítési szolgáltató API-k és SDK-t az ügyfél által fejlesztett integrációs keresztül.  

## <a name="prerequisites"></a>Előfeltételek

A MIM egy egyéni többtényezős hitelesítési szolgáltató API használatához szükséges:

- Telefonszám az összes jelölt felhasználó esetén
- A MIM-gyorsjavítás [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) vagy újabb – tekintse meg a [korábbi verziók](/reference/version-history.md) hirdetmények
- Konfigurált SSPR vagy a PAM MIM szolgáltatás

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Egyéni többtényezős hitelesítési kód segítségével módszert

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>1. lépés: Győződjön meg, hogy a MIM szolgáltatás nem 4.5.202.0 verzió vagy újabb

Töltse le és telepítse a MIM-gyorsjavítás [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) vagy újabb verziója.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>2. lépés: Hozzon létre egy IPhoneServiceProvider illesztő implementálását végző DLL

A dll-fájlt egy osztály, amely három módszert kell tartalmaznia:

- `InitiateCall`: A MIM szolgáltatás meghívja ezt a módszert. A szolgáltatás továbbítja a telefon száma és a kérés azonosítója paraméterekként.  A módszert kell visszaadnia egy `PhoneCallStatus` értékét `Pending`, `Success` vagy `Failed`.
- `GetCallStatus`: Ha egy korábbi hívása `initiateCall` visszaadott `Pending`, a MIM szolgáltatás meghívja ezt a módszert. Ez a módszer emellett adja vissza `PhoneCallStatus` értékét `Pending`, `Success` vagy `Failed`.
- `GetFailureMessage`: Ha egy előző meghívását `InitiateCall` vagy `GetCallStatus` visszaadott `Failed`, a MIM szolgáltatás meghívja ezt a módszert. Ez a módszer egy diagnosztikai üzenetet adja vissza.

Ezek a metódusok implementációiban szálbiztos, kell lennie, és emellett megvalósítása a `GetCallStatus` és `GetFailureMessage` kell nem feltételezi, hogy azok fogja meghívni, egy korábbi hívása elemmel azonos szálban `InitiateCall`.

A dll-Store a `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` könyvtár.

A mintakód, amely lehet lefordított használatával a Visual Studio 2010 vagy újabb.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>3. lépés: Biztonsági mentés az MfaSettings.xml található a "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>4. lépés: Az MfaSettings.xml fájl szerkesztése

Frissítés, vagy törölje a következő sorokat:

- Az összes konfigurációs bejegyzéseket sor eltávolítása/törlése 

- Frissítés vagy a következő sorokat ad hozzá a következő MfaSettings.xml a egyéni telefonszám-szolgáltatónál <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>5. lépés: A MIM szolgáltatás újraindítása

A szolgáltatás újraindulása után használja az SSPR és/vagy a PAM funkciók az egyéni identitásszolgáltató ellenőrzése.

> [!NOTE] 
> Vissza a beállítás MfaSettings.xml cserélje a 3. lépésben a biztonságimásolat-fájl


## <a name="next-steps"></a>További lépések

- [Ismerkedés az Azure multi-factor Authentication-kiszolgálón](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Mi az Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [A MIM verziókiadásai](./reference/version-history.md)

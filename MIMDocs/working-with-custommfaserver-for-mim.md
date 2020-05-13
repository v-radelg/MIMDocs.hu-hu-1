---
title: Alternatív Multi-Factor Authentication szolgáltató használata egy API-n keresztül a PAM vagy a SSPR forgatókönyv aktiválásához | Microsoft Docs
description: Állítsa be az egyéni MFA API-t második biztonsági rétegként, amikor a felhasználók a szerepköröket aktiválja Privileged Access Management és az önkiszolgáló jelszó-visszaállítást használják.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 284345d79cda8d60d055a642d047e28e63ea20cb
ms.sourcegitcommit: 80507a128d2bc28ff3f1b96377c61fa97a4e7529
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/13/2020
ms.locfileid: "83279946"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Egyéni Multi-Factor Authentication-szolgáltató használata egy API-n keresztül a PAM szerepkör aktiválása vagy a SSPR

Az prémium szintű Azure AD vagy az Azure MFA ügyfelei az Azure MFA-t két, a Privileged Access Management (PAM) szerepkör-aktiválással és az önkiszolgáló jelszó-visszaállítással (SSPR) integrálják.

A felhasználók két további lehetőség közül választhatnak:

 - Egyéni egyszeri jelszavas kézbesítési szolgáltató használata, amely csak a SSPR-forgatókönyvben alkalmazható, és az [önkiszolgáló jelszó-visszaállítás az OTP SMS Gate](https://docs.microsoft.com/previous-versions/mim/hh824692(v=ws.10)) szolgáltatással való konfigurálásának útmutatójában szerepel.
 - Használjon egyéni multi-Factor Authentication telefonos szolgáltatót. Ez a jelen cikkben ismertetett SSPR és PAM-forgatókönyvekben is alkalmazható.

Ez a cikk azt ismerteti, hogyan használható a többtényezős hitelesítés egy egyéni multi-Factor Authentication-szolgáltatóval egy API-n keresztül, valamint egy, az ügyfél által fejlesztett integrációs SDK használatával.  

## <a name="prerequisites"></a>Előfeltételek

Ha egyéni Multi-Factor Authentication szolgáltatói API-t kíván használni a felhasználói felülettel, a következőkre lesz szüksége:

- Telefonszám az összes jelölt felhasználó esetén
- 4.5.202.0-gyorsjavítás vagy újabb – a bejelentések [előzményeinek](reference/version-history.md) megtekintése
- SSPR vagy PAM-hoz konfigurált webkiszolgáló-szolgáltatás

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Megközelítés egyéni multi-Factor Authentication-kód használatával

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>1. lépés: Győződjön meg arról, hogy a 4.5.202.0 vagy újabb verzióban van

Töltse le és telepítse a [4.5.202.0](https://support.microsoft.com/help/4346632/hotfix-rollup-package-build-4-5-202-0-is-available-for-microsoft) vagy újabb verziót.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>2. lépés: hozzon létre egy DLL-t, amely megvalósítja a IPhoneServiceProvider felületet

A DLL-nek tartalmaznia kell egy osztályt, amely három módszert valósít meg:

- `InitiateCall`: A rendszerkiszolgálói szolgáltatás ezt a metódust fogja meghívni. A szolgáltatás paraméterként továbbítja a telefonszámot és a kérelem AZONOSÍTÓját.  A metódusnak a vagy a értékét kell visszaadnia `PhoneCallStatus` `Pending` `Success` `Failed` .
- `GetCallStatus`: Ha egy korábbi visszahívást ad `initiateCall` vissza `Pending` , a rendszer meghívja ezt a metódust. Ez a metódus a `PhoneCallStatus` vagy a értékét is megadja `Pending` `Success` `Failed` .
- `GetFailureMessage`: Ha az előző hívás vagy a `InitiateCall` `GetCallStatus` visszaadott érték `Failed` , a rendszer meghívja ezt a metódust. Ez a metódus egy diagnosztikai üzenetet ad vissza.

Ezeknek a módszereknek a megvalósításának a szál biztonságosnek kell lennie, továbbá a és a megvalósításának nem szabad azt `GetCallStatus` `GetFailureMessage` feltételezni, hogy ugyanazt a szálat fogja meghívni, mint a korábbi hívása `InitiateCall` .

Tárolja a DLL-fájlt a `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` könyvtárban.

A Visual Studio 2010-es vagy újabb verziójával összeállítható mintakód.

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
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>3. lépés: a "C:\Program Files\Microsoft Forefront Identity Manager\2010\service mappában" mappában található MfaSettings. xml fájl biztonsági mentése

### <a name="step-4-edit-the-mfasettingsxml-file"></a>4. lépés: a MfaSettings. xml fájl szerkesztése

Frissítse vagy törölje a következő sorokat:

- Az összes konfigurációs bejegyzés sorainak eltávolítása/törlése 

- Frissítse vagy adja hozzá a következő sorokat a MfaSettings. xml fájlhoz az egyéni telefonos szolgáltatóval <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>5. lépés: a rendszerindítási szolgáltatás újraindítása

A szolgáltatás újraindítása után a SSPR és/vagy a PAM használatával érvényesítheti az egyéni identitás-szolgáltató funkcióit.

> [!NOTE] 
> Ha vissza szeretné állítani a beállítást, cserélje le a MfaSettings. xml fájlt a biztonságimásolat-fájlra a 3. lépésben


## <a name="next-steps"></a>Következő lépések

- [Azure Multi-Factor Authentication-kiszolgáló – első lépések](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Mi az Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [A rendszerfrissítési csomag verziószáma](./reference/version-history.md)

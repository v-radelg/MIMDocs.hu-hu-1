---
title: A jelszóváltozás-értesítési szolgáltatás üzembe helyezése | Microsoft Docs
description: Ezekkel a lépésekkel telepítheti és konfigurálhatja a MIM jelszóváltozás-értesítési szolgáltatást a tartományvezérlőn.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f195a6db259bf0fefabcd05c8890ca65c9624314
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041964"
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>A MIM jelszóváltozás-értesítési szolgáltatás üzembe helyezése tartományvezérlőn

## <a name="install-the-password-change-notification-service"></a>A jelszóváltozás-értesítési szolgáltatás telepítése
A tartományvezérlőkre telepíthető jelszóváltozás-értesítési szolgáltatás lehetővé teszi a jelszavak szinkronizálását a MIM által más rendszerekre, például más gyártó által készített címtárkiszolgálóra. A jelszó-szinkronizáláshoz telepítse a jelszóváltozás-értesítési szolgáltatást minden egyes tartományvezérlő kiszolgálón.

1.  Tartományi rendszergazdaként jelentkezzen be egy Active Directory Domain Services szerepkörben működő Windows Server-alapú kiszolgálóra.

2.  Másolja a jelszóváltozás-értesítési szolgáltatás telepítési mappáját a számítógépre.

3.  Keresse meg a *Password Change Notification Service.msi* fájlt, kattintson rá jobb gombbal, majd hozzon létre egy parancsikont.

4.  Keresse meg a parancsikont, kattintson rá jobb gombbal, majd válassza a **Tulajdonságok** elemet.

5.  A Cél mezőben az MSI-fájl elérési útja elé helyezze el az *msiexec.exe /i* karakterláncot, utána pedig a *SCHEMAONLY=TRUE* utótagot. Ha például a telepítési mappa a *C:\PCNS*, akkor a következő parancsot kell futtatni (egy sorban):

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Mentse a parancsikonfájl módosításait.

7.  Futtassa a parancsikonfájlt a jelszóváltozás-értesítési szolgáltatás sémabővítési módban történő telepítéséhez. A következő képernyő megjelenésekor kattintson a **Tovább** gombra.

8.  A rendszer közli, hogy a telepítő frissíteni fogja az Active Directory-sémát a jelszóváltozás-értesítési szolgáltatáshoz. Kattintson az **OK** gombra a sémafrissítés végrehajtásához.

9. Amikor a sémabővítési folyamat befejeződik, és megjelenik a következő képernyő, kattintson a **Befejezés** gombra.

10. Futtassa újra a *Password Change Notification Service.msi* fájlt – ezúttal közvetlenül, futtatási sztring nélkül.  A következő képernyő megjelenésekor kattintson a **Tovább** gombra.

11. Fogadja el a licencszerződést, majd kattintson a **Tovább** gombra.

12. Ezzel megkezdődik a telepítés.

13. Amikor megjelenik a telepítés sikeres befejezéséről szóló képernyő, kattintson a **Befejezés** gombra.

14. A MIM jelszóváltozás-értesítési szolgáltatást érintő konfigurációs módosítások érvénybe lépéséhez indítsa újra a számítógépet. Ezt megteheti a megjelenő ablakban a **Yes** (Igen) gombra kattintva, de később is újraindíthatja a rendszert.

## <a name="configuring-the-password-change-notification-service"></a>A jelszóváltozás-értesítési szolgáltatás konfigurálása
Miután tartományi rendszergazdaként újra csatlakozott a tartományvezérlő kiszolgálóhoz, nyissa meg a *C:\Program Files\Microsoft Password Change Notification* mappát. Futtassa a *pcnscfg.exe* fájlt.

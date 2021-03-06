---
title: Tanúsítványok kérése a Tanúsítványkezelőben sablonokkal | Microsoft Docs
description: Megtudhatja, hogyan hozhat létre és újíthat meg szoftvertanúsítványokat a Tanúsítványkezelőben profilsablonok segítségével.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b8a1b55ed836f46c184941ccf6ec25ef63ad4c30
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042100"
---
# <a name="create-software-certificates-with-certificate-manager"></a>Szoftvertanúsítványok létrehozása a Tanúsítványkezelővel
A szoftvertanúsítványok regisztrálásához és megújításához nem kell rendszergazdának lennie, és virtuális intelligens kártyára sincs szükség. A rendszer valamikor egy tanúsítványművelet engedélyezésére fogja kérni. Ez normális.

## <a name="create-a-software-certificate-profile-template-in-mim-2016-certificate-manager"></a>Szoftvertanúsítvány-profilsablon létrehozása a MIM 2016 Tanúsítványkezelőben

1.  Hozzon létre egy sablont ahhoz a tanúsítványhoz, amelyet a virtuális intelligens kártyához fog igényelni. Indítsa el az MMC-t.

2.  Kattintson a **Fájl**, majd a **Beépülő modul hozzáadása/eltávolítása** parancsra.

3.  Az elérhető beépülő modulok listában kattintson a **Tanúsítványsablonok**elemre, majd a **Hozzáadás**gombra.

4.  Ekkor az MMC-ben a Konzolgyökér területen megjelenik a **Tanúsítványsablonok** lehetőség. A rendelkezésre álló tanúsítványsablonok megtekintéséhez kattintson rá duplán.

5.  Kattintson a jobb gombbal a **Felhasználói sablon** elemre, majd kattintson a **Sablon duplikálása** parancsra.

6.  A **Kompatibilitás** lapon a Hitelesítésszolgáltató területen válassza a Windows Server 2008 lehetőséget, majd a Tanúsítvány kedvezményezettje területen válassza a Windows 8.1 / Windows Server 2012 R2 beállítást.

    1.  Az **Általános** lapon a Megjelenített név mezőbe írja be a következőt: **Archivált tanúsítványsablon**.

    2.  b.  A **Kérelmek kezelése** lapon

        1.  A **Felhasználási cél** értékét állítsa Aláírás és titkosítás értékre.

        2.  Jelölje be **A tulajdonos által engedélyezett szimmetrikus algoritmusok belefoglalása** négyzetet.

        3.  Ha archiválni szeretné a kulcsot, jelölje be **A tulajdonos titkos kulcsának archiválása** beállítást.

        4.  „A következő történjen” beállításnál... válassza a **Felhasználó értesítése az igénylés alatt** lehetőséget.

    3.  A **Titkosítás** lapon

        1.  A Szolgáltató besorolása területen válassza a **Kulcstároló-szolgáltató** elemet.

        2.  Válassza **A kérelmekhez a tulajdonos számítógépén rendelkezésre álló bármely szolgáltató használható** beállítást.

    4.  A **Biztonság** lapon vegye fel azt a biztonsági csoportot, amelyhez **Igénylés** típusú hozzáférést kíván beállítani. Ha például az összes felhasználónak szeretne hozzáférést biztosítani, válassza a **Hitelesített** felhasználók csoportot, majd válasszon **igénylési** engedélyt számukra.

    5.  A **Tulajdonos neve** lapon

        1.  Törölje a jelet **Az e-mail név belefoglalása a tulajdonosnévbe** jelölőnégyzetből.

        2.  **A következő információk belefoglalása az alternatív tulajdonosnévbe** beállításnál törölje az **E-mail név** beállítás jelölését.

    6.  A módosítások véglegesítéséhez és az új sablon létrehozásához kattintson az **OK** gombra. Az új sablonnak ekkor meg kell jelennie a tanúsítványsablonok listájában.

    7.  A Hitelesítésszolgáltató beépülő modul MMC-konzolra való felvételéhez válassza a **Fájl** menü **Beépülő modul hozzáadása/eltávolítása** elemét. Ha a rendszer megkérdezi, hogy melyik számítógépet szeretné felügyelni, válassza a **helyi számítógép**lehetőséget.

    8.  Az MMC bal oldali panelén bontsa ki a **Hitelesítésszolgáltató (helyi)** csomópontot, majd a hitelesítésszolgáltatók listájában bontsa ki a saját hitelesítésszolgáltatóját.

    9. Kattintson jobb gombbal a **Tanúsítványsablonok** elemre, majd az **Új**, végül pedig a **Kiállítandó tanúsítványsablon** elemre.

    10. A listáról válassza ki az újonnan létrehozott sablont (**Archivált tanúsítványsablon**), majd kattintson az **OK** gombra.

## <a name="create-the-profile-template"></a>A profilsablon létrehozása

1.  Rendszergazdai jogosultsággal jelentkezzen be a Tanúsítványkezelő portálra.

2.  Nyissa meg az **Adminisztráció &gt; kezelése profil sablonokat** , és győződjön meg arról, hogy a jelölőnégyzet be van jelölve **MIM cm minta intelligens kártya bejelentkezési profil sablon** , majd kattintson a **Másolás a kiválasztott profil sablonra**elemre.

3.  Írja be a profilsablon nevét, majd kattintson az **OK** gombra.

4.  A következő képernyőn kattintson az **Add new certificate template** (Új tanúsítványsablon hozzáadása) elemre, és jelölje be a hitelesítésszolgáltató neve melletti négyzetet.

5.  Jelölje be az Archivált szoftvertanúsítvány neve melletti négyzetet, majd kattintson az **Add** (Hozzáadás) gombra.

6.  A User template (Felhasználói sablon) eltávolításához jelölje be a mellette található négyzetet, kattintson a **Delete selected certificate templates** (Kijelölt tanúsítványsablonok törlése) parancsra, majd az **OK** gombra.

7.  Kattintson a **Change general settings** (Általános beállítások módosítása) elemre.

8.  Jelölje be a **Generate encryption keys on the server** (Titkosítási kulcsok generálása a kiszolgálón) elemtől balra lévő négyzeteket, majd kattintson az **OK** gombra. A bal oldali panelen kattintson a **Recover Policy** (Helyreállítási szabályzat) elemre.

9. Kattintson a **Change general settings** (Általános beállítások módosítása) elemre.

10. Ha szeretné újra kiadni az archivált tanúsítványokat, jelölje be a **Reissue archived certificates** (Archivált tanúsítványok újrakiadása) elemtől balra lévő négyzeteket, majd kattintson az **OK** gombra.

11. A Virtual Smart Card Tanúsítványkezelő használata esetén le kell tiltania az adatgyűjtő elemeket, az összetevő ugyanis aktív adatgyűjtés mellett nem működik. Tiltsa le az adatgyűjtő elemeket minden házirendnél. Ehhez kattintson a házirendre a bal oldali panelen, törölje a **Sample data item** (Mintaadatelem) melletti négyzet jelölését, majd kattintson a **Delete data collection items** (Adatgyűjtési elemek törlése) parancsra. Ezt követően kattintson az **OK** gombra.

---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55889033"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>So installieren Sie Wartungsmodus-Hotfixes über Windows PowerShell für StorSimple
> [!IMPORTANT]
> Im Wartungsmodus müssen Sie den Hotfix zunächst auf einem und dann auf dem anderen Controller anwenden.
> 
> 

1. Versetzen Sie das Gerät in den Wartungsmodus. Weitere Informationen zum Wechseln in den Wartungsmodus finden Sie im Abschnitt [Schritt 2: Wechseln in den Wartungsmodus](../articles/storsimple/storsimple-update-device.md#step2).
2. Geben Sie zum Anwenden des Hotfixes Folgendes ein:
   
     `Start-HcsHotfix` 
3. Geben Sie bei entsprechender Aufforderung den Pfad zur Netzwerkfreigabe mit den Hotfix-Dateien ein.
4. Sie werden aufgefordert, diesen Schritt zu bestätigen. Geben Sie **J** ein, um mit der Installation des Hotfixes fortzufahren.
5. Nachdem Sie den Hotfix auf einem Controller installiert haben, melden Sie sich bei dem anderen Controller an. Wenden Sie den Hotfix wie beim vorherigen Controller an.
6. Nachdem alle Hotfixes angewendet wurden, beenden Sie den Wartungsmodus. Anweisungen dazu finden Sie unter [Schritt 4: Beenden des Wartungsmodus](../articles/storsimple/storsimple-update-device.md#step4).


---
title: Konfigurieren der Exportrichtlinie für ein Azure NetApp Files-Volume | Microsoft-Dokumentation
description: Beschreibt, wie Sie die Exportrichtlinie zum Steuern des Zugriffs auf ein Azure NetApp Files-Volume konfigurieren.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 1098c5c2f42b242b3ba2f68ec7480de90dd7c22e
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57766647"
---
# <a name="configure-export-policy-for-a-volume"></a>Konfigurieren der Exportrichtlinie für ein Volume

Sie können optional die Exportrichtlinie zum Steuern des Zugriffs auf ein Azure NetApp Files-Volume konfigurieren. 

## <a name="steps"></a>Schritte 

1.  Klicken Sie auf dem Blatt für die Volumeverwaltung auf das Blatt **Create Export Policy** (Exportrichtlinie erstellen). 

2.  Geben Sie Informationen für die folgenden Felder an, um eine Exportrichtlinienregel zu erstellen:   
    *  **Index**   
        Geben Sie die Indexnummer für die Regel an.  
        Eine Exportrichtlinie kann bis zu fünf Regeln umfassen. Regeln werden gemäß ihrer Reihenfolge in der Indexnummernliste ausgewertet. Regeln mit einer niedrigeren Indexnummer werden zuerst ausgewertet. So wird beispielsweise die Regel mit der Indexnummer 1 vor der Regel mit der Indexnummer 2 ausgewertet. 

    * **Zulässige Clients**   
        Geben Sie den Wert in einem der folgenden Formate an:  
        * IPv4-Adresse (Beispiel: `10.1.12.24`) 
        * IPv4-Adresse mit einer Subnetzmaske, ausgedrückt als Anzahl von Bits (Beispiel: `10.1.12.10/4`)

    * **zugreifen**  
        Wählen Sie einen der folgenden Berechtigungstypen aus:  
        * Kein Zugriff 
        * Lesen/Schreiben
        * Nur Leseberechtigung

    * **Protokolle**   
        Geben Sie das gewünschte Protokoll für die Exportrichtlinie an.   
        Momentan wird für Azure NetApp Files nur NFSv3 unterstützt.

    ![Exportrichtlinie](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>Nächste Schritte 
* [Verwalten von Volumes](azure-netapp-files-manage-volumes.md)
* [Bereitstellen oder Aufheben der Bereitstellung eines Volumes für virtuelle Computer](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Verwalten von Momentaufnahmen](azure-netapp-files-manage-snapshots.md)

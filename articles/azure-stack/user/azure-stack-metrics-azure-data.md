---
title: Azure Monitor in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie mehr zu Azure Monitor in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.lastreviewed: 12/01/2018
ms.openlocfilehash: 469213ae67f1f2bcece6afa5d9b7873bfdfe8d6c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58095737"
---
# <a name="azure-monitor-on-azure-stack"></a>Azure Monitor in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme*

Dieser Artikel bietet eine Übersicht über den Azure Monitor-Dienst in Azure Stack. Er beschreibt die Vorgänge beim Betrieb von Azure Monitor und enthält zusätzliche Informationen zur Verwendung von Azure Monitor in Azure Stack. 

Eine Einführung, eine Übersicht und erste Schritte mit Azure Monitor finden Sie im allgemeinen Azure-Artikel [Azure Monitor – Übersicht](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

![Blatt „Monitor“ in Azure Stack](./media/azure-stack-metrics-azure-data/azs-monitor.png)

Azure Monitor ist der Plattformdienst, mit dem Sie Ihre Azure-Ressourcen an einem zentralen Ort verwalten können. Mit Azure Monitor können Sie Metriken und Protokolle aus den Ressourcen in Azure visualisieren, abfragen, weiterleiten und archivieren und andere Maßnahmen ergreifen. Sie können im Administratorportal von Azure Stack, mit Monitor PowerShell-Cmdlets, in einer plattformübergreifenden Befehlszeilenschnittstelle oder in Azure Monitor-REST-APIs mit diesen Daten arbeiten. Weitere Informationen zu spezifischen Verbindungen, die von Azure Stack unterstützt werden, finden Sie unter [Nutzen der Überwachungsdaten von Azure Stack](azure-stack-metrics-monitor.md).

> [!Note]
> Metrik- und Diagnoseprotokolle stehen für das Azure Stack Development Kit nicht zur Verfügung.

## <a name="prerequisites-for-azure-monitor-on-azure-stack"></a>Voraussetzungen für Azure Monitor in Azure Stack

Registrieren Sie den Ressourcenanbieter **Microsoft.insights** in den Einstellungen für die Angebotsressource Ihres Abonnements. Sie können überprüfen, ob der Ressourcenanbieter in dem mit Ihrem Abonnement verknüpften Angebot verfügbar ist:

1. Öffnen Sie das Azure Stack-Administratorportal.
2. Wählen Sie **Angebote** aus.
3. Wählen Sie das Angebot aus, das dem Abonnement zugeordnet ist.
4. Wählen Sie unter **Einstellungen** die Option **Ressourcenanbieter** aus. 
5. Suchen Sie in der Liste nach **Microsoft.Insights**, und stellen Sie sicher, dass der Status **Registriert** lautet.

## <a name="overview-of-azure-monitor-on-azure-stack"></a>Übersicht über Azure Monitor in Azure Stack

Wie in Azure stellt Azure Monitor in Azure Stack grundlegende Infrastrukturmetriken und Protokolle für die meisten Dienste bereit.

## <a name="azure-monitor-sources-compute-subset"></a>Azure Monitor-Quellen: Computeteilmenge

![Quellen für Azure Monitor in Azure Stack: Computeteilmenge](media//azure-stack-metrics-azure-data/azs-monitor-computersubset.png)

Der Ressourcenanbieter **Microsoft.Compute** in Azure Stack umfasst Folgendes:
 - Virtual Machines 
 - VM-Skalierungsgruppen

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Anwendung: Diagnoseprotokolle, Anwendungsprotokolle und Metriken

Anwendungen können im Betriebssystem eines virtuellen Computers mit dem Ressourcenanbieter **Microsoft.Compute** ausgeführt werden. Diese Anwendungen und VMs geben einen eigenen Satz von Protokollen und Metriken aus. Azure Monitor greift auf die Azure-Diagnoseerweiterung (Windows oder Linux) zurück, um die meisten Metriken und Protokolle auf Anwendungsebene zu sammeln. 

Folgende Measuretypen sind u.a. verfügbar:
 - Leistungsindikatoren
 - Anwendungsprotokolle
 - Windows-Ereignisprotokolle
 - .NET-Ereignisquelle
 - IIS-Protokolle
 - Manifestbasierte ETW
 - Absturzabbilder
 - Benutzerdefinierte Fehlerprotokolle

> [!Note]  
> Die Linux-Diagnoseerweiterung in Azure Stack wird nicht unterstützt.

### <a name="host-and-guest-vm-metrics"></a>Metriken von Host- und Gast-VMs

Die zuvor aufgelisteten Computeressourcen sind einem dedizierten virtuellen Hostcomputer und Gastbetriebssystem zugeordnet. Die Host-VM und das Gastbetriebssystem entsprechen dem virtuellen Stammcomputer und dem virtuellen Gastcomputer im Hyper-V-Hypervisor. Sie können Metriken sowohl für die Host-VM als auch das Gastbetriebssystem erfassen. Darüber hinaus können Sie Diagnoseprotokolle zum Gastbetriebssystem sammeln. Eine Liste der Metriken, die für Host und Gast-VM in Azure Stack gesammelt werden können, finden Sie unter [Unterstützte Metriken von Azure Monitor in Azure Stack](azure-stack-metrics-supported.md). 

### <a name="activity-log"></a>Aktivitätsprotokoll

Sie können die Aktivitätsprotokolle nach Informationen zu Ihren Computeressourcen aus Sicht der Azure Stack-Infrastruktur durchsuchen. Dieses Protokoll enthält Informationen wie etwa den Erstellungs- oder Löschzeitpunkt von Ressourcen. Die Aktivitätsprotokolle in Azure Stack sind mit denen in Azure konsistent. Weitere Informationen finden Sie in der Beschreibung [Überwachen der Abonnementaktivität per Azure-Aktivitätsprotokoll](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). 


## <a name="azure-monitor-sources-everything-else"></a>Azure Monitor-Quellen: Sonstiges

![Quellen für Azure Monitor in Azure Stack: Sonstiges](media//azure-stack-metrics-azure-data/azs-monitor-othersubset.png)

### <a name="resources---metrics-and-diagnostics-logs"></a>Ressourcen: Metriken und Diagnoseprotokolle

Die erfassbaren Metriken und Diagnoseprotokolle variieren je nach Ressourcentyp. Eine Liste der erfassbaren Metriken für jede Ressource in Azure Stack finden Sie unter den unterstützten Metriken. Weitere Informationen finden Sie unter [Unterstützte Metriken von Azure Monitor in Azure Stack](azure-stack-metrics-supported.md).

### <a name="activity-log"></a>Aktivitätsprotokoll

Das Aktivitätsprotokoll entspricht dem für Computeressourcen. 

### <a name="uses-for-monitoring-data"></a>Verwendungszwecke für Überwachungsdaten

**Speicherung und Archivierung**  

Einige Überwachungsdaten sind bereits gespeichert und für einen festgelegten Zeitraum in Azure Monitor verfügbar. 
 - Metriken werden 90 Tage lang gespeichert. 
 - Aktivitätsprotokolleinträge werden 90 Tage lang gespeichert. 
 - Diagnoseprotokolle werden nicht gespeichert.
 - Archivieren Sie die Daten in einem Speicherkonto, wenn Sie sie länger aufbewahren möchten.

**Abfrage**  

Sie können die Azure Monitor-REST-API, Befehle der plattformübergreifenden Befehlszeilenschnittstelle, PowerShell-Cmdlets oder das .NET SDK verwenden, um auf die Daten im System oder in Azure Storage zuzugreifen. 

**Visualisierung**

Indem Sie Ihre Überwachungsdaten in Grafiken und Diagrammen visualisieren, können Sie Trends schneller ausmachen, als wenn Sie die Daten selbst ansehen. 

Einige Beispiele für Visualisierungsmethoden:
 - Verwenden von Benutzer- und Administratorportal für Azure Stack
 - Weiterleiten von Daten an Microsoft Power BI
 - Weiterleiten von Daten an ein Visualisierungstool eines Drittanbieters mittels Livestreaming oder durch Lesen eines Archivs in Azure Storage

## <a name="methods-of-accessing-azure-monitor-on-azure-stack"></a>Methoden für den Zugriff auf Azure Monitor in Azure Stack

Nachverfolgung, Routing und Abfrage von Daten können ganz allgemein mit einer der folgenden Methoden angepasst werden. Manche Methoden sind unter Umständen für bestimmte Aktionen oder Datentypen nicht verfügbar.

 - [Azure Stack-Portal](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-use-portal)
 - [PowerShell](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples)
 - [Plattformübergreifende Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples)
 - [REST-API](https://docs.microsoft.com/rest/api/monitor)
 - [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Möglichkeiten zur Nutzung der Überwachungsdaten in Azure Stack finden Sie im Artikel [Nutzen der Überwachungsdaten von Azure Stack](azure-stack-metrics-monitor.md).

---
title: Überwachen der Leistung von Azure App Services | Microsoft-Dokumentation
description: Überwachung der Anwendungsleistung für Azure App Services. Ladezeit für Diagramme und Antwortzeit, Informationen zu den Abhängigkeiten und Festlegen von Benachrichtigungen zur Leistung.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: application-insights
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: mbullwin
ms.openlocfilehash: 19e0e5797e05589baa1e104f3e9ab8b4d9cc2d6c
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267290"
---
# <a name="monitor-azure-app-service-performance"></a>Überwachen der Leistung von Azure App Service
Im [Azure-Portal](https://portal.azure.com) können Sie eine Anwendungsleistungsüberwachung für Ihre Web-Apps, mobilen Back-Ends und API-Apps in [Azure App Service](../../app-service/overview.md) einrichten. [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) instrumentiert Ihre App, um Telemetriedaten über ihre Aktivitäten an den Application Insights-Dienst zu senden, wo sie gespeichert und analysiert werden. Hier können Sie dann mithilfe von Metrikdiagrammen und Suchtools Probleme diagnostizieren, die Leistung verbessern und die Nutzung untersuchen.

## <a name="run-time-or-build-time"></a>Laufzeit oder Buildzeit
Zum Konfigurieren der Überwachung kann die App auf zwei Arten instrumentiert werden:

* **Laufzeit:** Sie können eine Erweiterung für die Leistungsüberwachung auswählen, wenn Ihre App Service-Instanz bereits live geschaltet wurde. Die App muss dazu nicht neu erstellt oder erneut installiert werden. Sie erhalten einen Standardsatz von Paketen zur Überwachung von Antwortzeiten, Erfolgsraten, Ausnahmen, Abhängigkeiten und Ähnlichem. 
* **Buildzeit:** Sie können ein Paket in der App installieren, die Sie gerade entwickeln. Diese Option ist wesentlich flexibler. Neben den gleichen Standardpaketen können Sie auch Code schreiben, um die Telemetrie anzupassen oder Ihre eigenen Telemetriedaten zu senden. Sie können bestimmte Aktivitäten protokollieren oder Ereignisse gemäß der Semantik Ihrer App-Domäne erfassen. 

## <a name="run-time-instrumentation-with-application-insights"></a>Laufzeitinstrumentierung mit Application Insights
Wenn Sie bereits eine App Service-Instanz in Azure ausführen, verfügen Sie schon über eine Art der Überwachung, und zwar von Anforderungs- und Fehlerraten. Fügen Sie Application Insights hinzu, um die Überwachung zu erweitern, z.B. auf Reaktionszeiten, Überwachung beim Aufrufen von Abhängigkeiten, intelligente Erkennung und die leistungsstarke Abfragesprache Kusto. 

1. In der Azure-Systemsteuerung für Ihre App Service-Instanz können Sie **Application Insights auswählen**.

    ![Auswählen von „Application Insights“ unter „Einstellungen“](./media/azure-web-apps/settings-app-insights.png)

   * Wählen Sie die Option zum Erstellen einer neuen Ressource, sofern Sie nicht bereits eine Application Insights-Ressource für diese Anwendung eingerichtet haben. 

    > [!NOTE]
    > Beim Klicken auf **OK** zum Erstellen der neuen Ressource wird die Aufforderung **Überwachungseinstellungen anwenden** angezeigt. Wenn Sie **Weiter** wählen, wird Ihre neue Application Insights-Ressource mit Ihrer App Service-Instanz verknüpft, und außerdem wird **ein Neustart Ihrer App Service-Instanz ausgelöst**. 

    ![Instrumentieren Ihrer Web-App](./media/azure-web-apps/create-resource.png)

2. Nach Angabe der zu verwendenden Ressource können Sie plattformspezifisch auswählen, wie in Application Insights Daten für Ihre Anwendung erfasst werden sollen. (Die Überwachung von ASP.NET-Apps ist standardmäßig mit zwei verschiedenen Erfassungsebenen aktiviert.)

    ![Auswählen plattformspezifischer Optionen](./media/azure-web-apps/choose-options-new.png)

    * Die .NET-Ebene **Basissammlung** bietet wesentliche Einzelinstanz-APM-Funktionen.
    
    * Die .NET-Ebene **Empfohlen Sammlung**:
        * Fügt CPU-, Speicher- und E/A-Nutzungstrends hinzu.
        * Korreliert von Microservices über Anforderungs-/Abhängigkeitsgrenzen hinweg.
        * Sammelt Nutzungstrends und ermöglicht die Korrelation von Verfügbarkeitsergebnissen und Transaktionen.
        * Erfasst Ausnahmen, die vom Hostprozess nicht behandelt werden.
        * Verbessert die Genauigkeit der APM-Metriken unter Last, wenn die Stichprobe verwendet wird.
    
    .NET Core bietet **Empfohlen Sammlung** oder „Deaktiviert“ für .NET Core 2.0 und 2.1.

3. **Instrumentieren Sie Ihre App Service-Instanz**, nachdem Application Insights installiert wurde.

   **Clientseitige Überwachung aktivieren** für Seitenansicht und Benutzertelemetrie.

    (Dies ist für .NET Core-Apps standardmäßig mit **Empfohlene Sammlung** aktiviert, unabhängig davon, ob die App-Einstellung „APPINSIGHTS_JAVASCRIPT_ENABLED“ vorhanden ist. Präzise UI-basierte Unterstützung zum Deaktivieren der clientseitigen Überwachung ist derzeit für .NET Core nicht verfügbar.)
    
   * Wählen Sie „Einstellungen“ > „Anwendungseinstellungen“ aus.
   * Fügen Sie unter „App-Einstellungen“ ein neues Schlüssel-Wert-Paar hinzu:

    Schlüssel: `APPINSIGHTS_JAVASCRIPT_ENABLED`

    Wert: `true`
   * **Speichern** Sie die Einstellungen, und **starten Sie die App neu**.

4. Erkunden Sie die Überwachungsdaten Ihrer App, indem Sie **Einstellungen** > **Application Insights** > **Weitere Informationen in Application Insights anzeigen** wählen.

Später können Sie die App bei Bedarf mit Application Insights erstellen.

*Wie entferne ich Application Insights oder stelle auf das Senden an eine andere Ressource um?*

* Öffnen Sie in Azure das Steuerungsblatt der Web-App und anschließend **Application Insights** (unter „Einstellungen“). Sie können Application Insights deaktivieren, indem Sie im oberen Bereich auf **Deaktivieren** klicken. Alternativ können Sie im Abschnitt **Change your resource** (Ressource ändern) eine neue Ressource auswählen.

## <a name="build-the-app-with-application-insights"></a>Erstellen der App mit Application Insights
Application Insights kann durch Installieren eines SDK in Ihrer App eine detailliertere Telemetrie bereitstellen. Beispielsweise können Sie Ablaufverfolgungsprotokolle erfassen, [benutzerdefinierte Telemetriedaten schreiben](../../azure-monitor/app/api-custom-events-metrics.md) und ausführlichere Berichte zu Ausnahmen erhalten.

1. Konfigurieren Sie in **Visual Studio** (2013 Update 2 oder höher) Application Insights für Ihr Projekt.

    Klicken Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie **Hinzufügen > Application Insights** oder **Projekt** > **Application Insights** > **Application Insights konfigurieren**.

    ![Klicken Sie mit der rechten Maustaste auf das Webprojekt, und wählen Sie „"Application Insights hinzufügen“ oder „Application Insights konfigurieren“.](./media/azure-web-apps/03-add.png)

    Wenn Sie zur Anmeldung aufgefordert werden, verwenden Sie die Anmeldeinformationen für Ihr Azure-Konto.

    Der Vorgang hat zwei Auswirkungen:

   1. Es wird eine Application Insights-Ressource in Azure erstellt, unter der die Telemetriedaten gespeichert, analysiert und angezeigt werden.
   2. Das Application Insights-NuGet-Paket wird dem Code hinzugefügt (sofern noch nicht vorhanden) und so konfiguriert, dass Telemetriedaten an die Azure-Ressource gesendet werden.
2. **Testen Sie die Telemetrie**, indem Sie die App auf Ihrem Entwicklungscomputer (F5) ausführen.
3. **Veröffentlichen Sie die App** wie gewohnt in Azure. 

*Wie führe ich die Umstellung durch, damit Daten an eine andere Application Insights-Ressource gesendet werden?*

* Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie **Application Insights konfigurieren** und dann die gewünschte Ressource aus. Sie erhalten die Möglichkeit, eine neue Ressource zu erstellen. Führen Sie die Neuerstellung und dann die erneute Bereitstellung durch.

## <a name="more-telemetry"></a>Mehr Telemetrie

* [Ladedaten für Webseiten](../../azure-monitor/app/javascript.md)
* [Benutzerdefinierte Telemetrie](../../azure-monitor/app/api-custom-events-metrics.md)

## <a name="troubleshooting"></a>Problembehandlung

### <a name="appinsightsjavascriptenabled-causes-incomplete-html-response-in-net-core-web-applications"></a>APPINSIGHTS_JAVASCRIPT_ENABLED bewirkt unvollständige HTML-Antworten in .NET CORE-Webanwendungen.

Durch das Aktivieren von Javascript über App Services können HTML-Antworten abgeschnitten werden.

* Problemumgehung 1: Legen Sie die Anwendungseinstellung APPINSIGHTS_JAVASCRIPT_ENABLED auf „false“ fest, oder entfernen Sie sie vollständig, und führen Sie einen Neustart durch.
* Problemumgehung 2: Fügen Sie das SDK über den Code hinzu, und entfernen Sie die Erweiterung (Profiler und Momentaufnahmedebugger können mit dieser Konfiguration nicht verwendet werden).

Wir verfolgen dieses Problem [hier](https://github.com/Microsoft/ApplicationInsights-Home/issues/277).

Für .NET Core werden derzeit **nicht unterstützt**:

* Eigenständige Bereitstellung.
* Apps für das .NET Framework.
* .NET Core 2.2-Anwendungen.

> [!NOTE]
> .NET Core 2.0 und .NET Core 2.1 werden unterstützt. Wird .NET Core 2.2-Unterstützung hinzugefügt, wird dieser Artikel aktualisiert.

## <a name="next-steps"></a>Nächste Schritte
* [Ausführen des Profilers in Ihrer Live-App](../../azure-monitor/app/profiler.md)
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample): Überwachen Sie Azure Functions mit Application Insights
* [Ermöglichen des Sendens von Azure-Diagnosedaten an Application Insights](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Überwachen von Dienstintegritätsmetriken](../../azure-monitor/platform/data-collection.md), um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
* [Empfangen von Warnbenachrichtigungen](../../azure-monitor/platform/alerts-overview.md) , wenn ein Vorgangsereignis auftritt oder Metriken einen Schwellenwert überschreiten.
* Verwenden von [Application Insights für JavaScript-Apps und Webseiten](../../azure-monitor/app/javascript.md), um Clienttelemetriedaten von den Browsern zu erhalten, mit denen auf eine Webseite zugegriffen wird
* [Einrichten von Verfügbarkeitswebtests](../../azure-monitor/app/monitor-web-app-availability.md), um benachrichtigt zu werden, wenn Ihre Website nicht verfügbar ist


---
title: Verwenden des Azure Stack-Überprüfungstools | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Protokolldateien für die Diagnose in Azure Stack sammeln.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: adshar
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: eca66bc2e737b0f9a9954cad21a446e82d753f84
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203810"
---
# <a name="validate-azure-stack-system-state"></a>Überprüfen des Azure Stack-Systemstatus

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Als Azure Stack-Betreiber in der Lage zu sein, die Integrität und den Status Ihres Systems bei Bedarf zu ermitteln, ist sehr wichtig. Das Azure Stack-Überprüfungstool (**Test-AzureStack**) ist ein PowerShell-Cmdlet, mit dem Sie eine Reihe von Tests auf Ihrem System ausführen können, um vorliegende Fehler zu identifizieren. In der Regel werden Sie aufgefordert, dieses Tool über den [privilegierten Endpunkt (PEP)](azure-stack-privileged-endpoint.md) auszuführen, wenn Sie sich mit der Meldung eines Problems an Microsoft Customer Services Support (CSS) wenden. Wenn die systemweiten Integritäts- und Statusinformationen vorliegen, kann CSS detaillierte Protokolle sammeln und analysieren, sich auf den Bereich konzentrieren, in dem der Fehler aufgetreten ist, und mit Ihnen zusammenarbeiten, um das Problem zu beheben.

## <a name="running-the-validation-tool-and-accessing-results"></a>Ausführen des Überprüfungstools und Zugreifen auf die Ergebnisse

Wie bereits erwähnt, wird das Überprüfungstool über den PEP ausgeführt. Jeder Test gibt im PowerShell-Fenster einen **PASS/FAIL**-Status zurück. Darüber hinaus wird ein detaillierter HTML-Bericht erstellt, auf den zu einem späteren Zeitpunkt während der [Protokollsammlung](azure-stack-diagnostics.md) zugegriffen werden kann. Hier ist eine Gliederung des Prozesses für End-to-End-Validierungstests: 

1. Greifen Sie auf den privilegierten Endpunkt (PEP) zu. Führen Sie die folgenden Befehle aus, um eine PEP-Sitzung einzurichten:

   ```powershell
   Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
   ```

   > [!TIP]
   > Wenn Sie auf einem ASDK-Hostcomputer auf den PEP zugreifen möchten, geben Sie für „-ComputerName“ den Wert „AzS-ERCS01“ an.

2. Sobald Sie im PEP sind, führen Sie Folgendes aus: 

   ```powershell
   Test-AzureStack
   ```

   Weitere Informationen finden Sie in den Abschnitten [Überlegungen zu Parametern](azure-stack-diagnostic-test.md#parameter-considerations) und [Beispiele für Anwendungsfälle](azure-stack-diagnostic-test.md#use-case-examples).

3. Wenn Tests **FAIL** melden, führen Sie Folgendes aus:

   ```powershell
   Get-AzureStackLog -FilterByRole SeedRing -OutputSharePath "<path>" -OutputShareCredential $cred
   ```

   Das Cmdlet sammelt Protokolle, die von Test-AzureStack generiert wurden. Weitere Informationen zu Diagnoseprotokollen finden Sie unter [Azure Stack-Diagnosetools](azure-stack-diagnostics.md). Sie sollten keine Protokolle sammeln und sich nicht an CSS wenden, wenn bei Tests **WARN** gemeldet wird.

4. Wenn Sie durch den CSS angewiesen wurden, das Überprüfungstool auszuführen, wird der CSS-Vertreter die von Ihnen gesammelten Protokolle anfordern, um die Behebung Ihres Problems fortsetzen zu können.

## <a name="tests-available"></a>Verfügbare Tests

Mithilfe des Überprüfungstools können Sie eine Reihe von Tests auf Systemebene sowie grundlegende Cloudszenarien ausführen, die Ihnen einen Einblick in den aktuellen Status bieten und Probleme in Ihrem System ermitteln.

### <a name="cloud-infrastructure-tests"></a>Tests der Cloudinfrastruktur

Diese Tests mit geringer Auswirkung arbeiten auf einer Infrastrukturebene und liefern Ihnen Informationen zu verschiedenen Systemkomponenten und -funktionen. Tests sind derzeit in die folgenden Kategorien gruppiert:

| Testkategorie                                        | Argument für „-Include“ und „-Ignore“ |
| :--------------------------------------------------- | :-------------------------------- |
| Zusammenfassung der Azure Stack-Warnung                            | AzsAlertSummary                   |
| Zusammenfassung der Verfügbarkeit von Azure Stack-Sicherungsfreigaben       | AzsBackupShareAccessibility       |
| Zusammenfassung der Azure Stack-Steuerungsebene                    | AzsControlPlane                   |
| Zusammenfassung von Azure Stack Defender                         | AzsDefenderSummary                |
| Zusammenfassung Hostinginfrastruktur-Firmware für Azure Stack  | AzsHostingInfraFWSummary          |
| Zusammenfassung der Cloudhostinginfrastruktur für Azure Stack     | AzsHostingInfraSummary            |
| Nutzung der Cloudhostinginfrastruktur für Azure Stack | AzsHostingInfraUtilization        |
| Kapazität der Azure Stack-Infrastruktur                  | AzsInfraCapacity                  |
| Leistung der Azure Stack-Infrastruktur               | AzsInfraPerformance               |
| Zusammenfassung der Infrastrukturrolle von Azure Stack              | AzsInfraRoleSummary               |
| Zusammenfassung des Azure Stack-Updates                           | AzsInfraUpdateSummary             |
| Zusammenfassung von Azure Stack-Portal und API                   | AzsPortalAPISummary               |
| VM-Ereignisse der Azure Stack-Skalierungseinheit                     | AzsScaleUnitEvents                |
| VM-Ressourcen der Azure Stack-Skalierungseinheit                  | AzsScaleUnitResources             |
| Zusammenfassung der Azure Stack SDN-Überprüfung                   | AzsSDNValidation                  |
| Zusammenfassung der Azure Stack Service Fabric-Rolle              | AzsSFRoleSummary                  |
| Zusammenfassung des Azure Stack BMC                              | AzsStampBMCSummary                |
| Zusammenfassung der Azure Stack-Speicherdienste                 | AzsStorageSvcsSummary             |
| Zusammenfassung des Azure Stack SQL-Stores                        | AzsStoreSummary                   |
| Zusammenfassung der Azure Stack VM-Platzierung                     | AzsVmPlacement                    |

### <a name="cloud-scenario-tests"></a>Cloudszenariotests

Zusätzlich zu den vorstehenden Infrastrukturtests haben Sie die Möglichkeit zur Ausführung von Cloudszenariotests, um die Funktionalität für Infrastrukturkomponenten zu überprüfen. Weil in diesen Tests die Bereitstellung von Ressourcen enthalten ist, sind zu deren Ausführung Anmeldeinformationen für Cloudadministratoren erforderlich. 
    > [!NOTE]
    > Currently you cannot run cloud scenario tests using Active Directory Federated Services (AD FS) credentials. 

Die folgenden Cloudszenarien werden durch das Überprüfungstool getestet:
- Ressourcengruppenerstellung   
- Planerstellung              
- Angebotserstellung            
- Speicherkontoerstellung   
- Erstellung des virtuellen Computers 
- Speichervorgang für Blobs   
- Speichervorgang für Warteschlangen  
- Speichervorgang für Tabellen  

## <a name="parameter-considerations"></a>Überlegungen zu Parametern

- Der Parameter **List** kann verwendet werden, um alle verfügbaren Testkategorien anzuzeigen.

- Die Parameter **Include** und **Ignore** können verwendet werden, um Testkategorien einzubeziehen bzw. auszuschließen. Weitere Informationen zu der Kurznotation, die bei diesen Argumenten verwendet werden kann, finden Sie im Abschnitt [Verfügbare Tests](azure-stack-diagnostic-test.md#tests-available).

  ```powershell
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
  ```

  ```powershell
  Test-AzureStack -Ignore AzsInfraPerformance
  ```

- Ein Mandanten-VM wird im Rahmen eines der Cloudszenariotests bereitgestellt. Dies können Sie mit **DoNotDeployTenantVm** deaktivieren. 

- Sie müssen den Parameter **ServiceAdminCredential** angeben, um die Cloudszenariotests entsprechend der Beschreibung im Abschnitt [Beispiele für Anwendungsfälle](azure-stack-diagnostic-test.md#use-case-examples) auszuführen.

- **BackupSharePath** und **BackupShareCredential** werden beim Testen von Einstellungen für die Infrastruktursicherung verwendet, wie im Abschnitt [Beispiele für Anwendungsfälle](azure-stack-diagnostic-test.md#use-case-examples) gezeigt wird.

- Das Überprüfungstool unterstützt auch gängige PowerShell-Parameter: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable und OutVariable. Weitere Informationen finden Sie unter [about_Commonparameters](https://go.microsoft.com/fwlink/?LinkID=113216).  

## <a name="use-case-examples"></a>Beispiele für Anwendungsfälle

### <a name="run-validation-without-cloud-scenarios"></a>Ausführen der Überprüfung ohne Cloudszenarien

Führen Sie das Überprüfungstool ohne den Parameter **ServiceAdminCredential** aus, um die Ausführung von Cloudszenariotests zu überspringen: 

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred
Test-AzureStack
```

### <a name="run-validation-with-cloud-scenarios"></a>Ausführen der Überprüfung mit Cloudszenarien

Wenn für das Überprüfungstool der Parameter **ServiceAdminCredentials** angegeben wird, werden standardmäßig die Cloudszenariotests ausgeführt: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" 
```

Wenn Sie NUR Cloudszenarien ausführen möchten, ohne die übrigen Tests auszuführen, können Sie dazu den Parameter **Include** verwenden: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" -Include AzsScenarios   
```

Der Benutzername des Cloudadministrators muss im UPN-Format eingegeben werden: serviceadmin@contoso.onmicrosoft.com (Azure AD). Geben Sie bei Aufforderung das Kennwort für das Cloudadministratorkonto ein.

### <a name="run-validation-tool-to-test-system-readiness-before-installing-update-or-hotfix"></a>Ausführen des Überprüfungstools zum Testen der Systembereitschaft vor dem Installieren des Updates oder Hotfixes

Bevor Sie mit der Installation eines Updates oder Hotfixes beginnen, sollten Sie das Überprüfungstool ausführen, um den Status Ihres Azure Stack zu überprüfen:

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
```

### <a name="run-validation-tool-to-test-infrastructure-backup-settings"></a>Ausführen des Überprüfungstools zum Testen der Einstellungen für die Infrastruktursicherung

*Vor* der Konfiguration der Infrastruktursicherung können Sie mit dem Test **AzsBackupShareAccessibility** den Sicherungsfreigabepfad und die Sicherungsanmeldeinformationen testen: 

  ```powershell
  New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
  ```

*Nach* der Konfiguration der Sicherung können Sie **AzsBackupShareAccessibility** ausführen, um zu überprüfen, ob über das ERCS auf die Freigabe zugegriffen werden kann:

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility
  ```

Um neue Anmeldeinformationen mit der konfigurierten Sicherungsfreigabe zu testen, führen Sie Folgendes aus: 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential "<PSCredential for backup share>"
  ```



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Stack-Diagnosetools und zur Protokollierung von Problemen finden Sie unter [Azure Stack-Diagnosetools](azure-stack-diagnostics.md).

Weitere Informationen zur Problembehandlung finden Sie unter [Problembehandlung von Microsoft Azure Stack](azure-stack-troubleshooting.md).
<properties
   pageTitle="Problembehandlung bei Azure Security Center | Microsoft Azure"
   description="Dieses Dokument hilft bei Problemen in Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Azure Security Center-Fehlerbehebungshandbuch
Dieses Handbuch ist für IT-Experten (IT), Informationen Sicherheitsexperten und Cloud-Administratoren, deren Organisationen Azure Security Center und Problembehandlung benötigen, Sicherheitscenter Fragen.

## <a name="troubleshooting-guide"></a>Fehlerbehebung
Dieses Handbuch erläutert die Problembehandlung Sicherheitscenter Fragen. Die meisten der Fehlerbehebung im Sicherheitscenter ausgeführt erfolgt durch die Datensätze im [Prüfprotokoll](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) für die fehlerhafte Komponente zuerst untersucht. Durch Audit-Protokolle können Sie Folgendes ermitteln:

- Welche Vorgänge durchgeführt wurden
- Der den Vorgang initiiert hat
- Wenn der Vorgang aufgetreten
- Der Status des Vorgangs
- Die Werte der anderen Eigenschaften, mit denen Sie möglicherweise untersuchen den Vorgang

Das Prüfprotokoll enthält alle Schreibvorgänge (PUT, POST, DELETE) für die Ressourcen jedoch nicht Lesevorgänge (GET) enthalten ist.

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Problembehandlung bei monitoring Agentinstallation unter Windows

Security Center monitoring Agent dient zur Datensammlung durchführen. Nach Sammlung aktiviert ist und der Agent auf dem Zielcomputer korrekt installiert ist, sollte diese Prozesse in Ausführung:

- ASMAgentLauncher.exe - Agent überwachen Azure 
- ASMMonitoringAgent.exe - Erweiterung Azure Security-Überwachung
- ASMSoftwareScanner.exe – Azure Scan Manager

Die Erweiterung Azure Security Überwachung sucht nach verschiedenen relevanten Sicherheitskonfiguration und Sicherheitsprotokolle vom virtuellen Computer erfasst. Scan Manager wird als Patch Scanner verwendet werden.

Wenn die Installation erfolgreich durchgeführt sollte einen Eintrag ähnlich dem folgenden in den Überwachungsprotokollen für die Ziel-VM angezeigt werden:

![Audit-Protokolle](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Sie erhalten auch Informationen zum Installationsvorgang durch Lesen der Agentenprotokolle am *%systemdrive%\windowsazure\logs* (Beispiel: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Wenn Azure Security Center Agent fehlerhafte müssen Sie das Ziel VM starten, da kein Befehl zum Beenden und starten den Agent.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Problembehandlung bei monitoring Agentinstallation unter Linux
Bei der Behandlung von VM-Agent-Installation in einem Linux-System sollten die Erweiterung/Var/Lib/Waagent / heruntergeladen wurde. Führen Sie folgenden Befehl überprüfen, ob es installiert wurde:

`cat /var/log/waagent.log` 

Die anderen Protokolldateien, die Sie überprüfen können zur Problembehandlung Zweck sind: 

- /var/log/mdsd.Err
- / Var/Log/Azure /

In einem sollte eine Verbindung mit der Mdsd-Prozess auf TCP-29130 angezeigt werden. Dies ist mit der Mdsd-Prozess Syslog. Sie können dieses Verhalten mit dem folgenden Befehl überprüfen:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Microsoft Support kontaktieren

Einige Probleme können mit den Richtlinien in diesem Artikel finden Sie auch andere am Security Center öffentlichen [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter)dokumentiert identifiziert werden. Aber wenn weitere Problembehandlung können Sie eine neue Supportanfrage mithilfe von Azure-Portal wie folgt öffnen: 

![Microsoft-Support](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie Sicherheitsrichtlinien in Azure Security Center konfigurieren. Erfahren Sie mehr über Azure Security Center finden Sie hier:

- [Planen von Azure Security Center und Operations Guide](security-center-planning-and-operations-guide.md) – Informationen zu Vorüberlegungen Azure Security Center zu verstehen.
- [Sicherheit von Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md) – Informationen zum Überwachen der Azure-Ressourcen
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Sicherheitshinweisen auf
- [Überwachung Partner mit Azure Security Center](security-center-partner-solutions.md) – So überwachen Sie den Status Ihrer Lösungen Partner.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Suchen über Azure Sicherheits- und Compliance-Blogbeiträge

<properties
   pageTitle="Integration von Azure Security Center Alerts Azure Protokoll Integration (Vorschau) | Microsoft Azure"
   description="Dieser Artikel hilft Ihnen den Einstieg in Azure Protokoll Integration Sicherheitshinweise des Sicherheitscenters integrieren."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Integration von Sicherheitshinweise des Sicherheitscenters in Azure Protokoll Integration (Vorschau)

Viele Sicherheitsmaßnahmen und Incident Response Teams nutzen eine Sicherheitsinformations- und Ereignismanagement (SIEM) Lösung als Ausgangspunkt für selektieren und Sicherheitshinweise untersuchen. Integration der Azure-Protokoll können Kunden Azure Security Center Alerts mit virtuellen Sicherheitsereignisse Azure-Diagnose und Azure Überwachungsprotokollen der Protokollanalyse oder SIEM-Lösung in Echtzeit erfassten synchronisieren.

Integration von Azure Protokoll arbeitet mit HP ArcSight, Splunk und IBM QRadar.

## <a name="what-logs-can-i-integrate"></a>Welche Protokolle können werden integriert?

Azure führt ausführliche Protokollierung für jeden Dienst. Diese Protokolle sind wie folgt kategorisiert:

- **Control-Management-Protokolle** an Sichtbarkeit in Azure Ressourcen-Manager erstellen, UPDATE und DELETE-Operationen.
- **Datenebene Protokolle** Einblick in die Ereignisse ausgelöst, wenn ein Azure Ressource zu. Ein Beispiel ist das Windows-Ereignisprotokoll - Sicherheit und Anwendungsprotokolle auf einem virtuellen Computer.

Integration von Azure Protokoll unterstützt derzeit die Integration von:

- Azure VM-Protokolle
- Azure Überwachungsprotokolle
- Azure Security Center-Alarme

## <a name="install-azure-log-integration"></a>Integration von Azure Protokoll installieren

[Azure anmelden Integration](https://www.microsoft.com/download/details.aspx?id=53324)herunterladen

Azure anmelden Integrationsdienst sammelt Daten von dem Computer, auf dem es installiert ist.  Telemetriedaten ist:

- Ausnahmen, die während der Ausführung von Azure anmelden integration
- Metriken zur Anzahl der Abfragen und verarbeiteten Ereignisse
- Statistiken über die Azlog.exe Befehlszeilenoptionen verwendet werden

> [AZURE.NOTE] Sie können die Sammlung von Daten deaktivieren diese Option deaktivieren.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integration von Azure Prüfprotokolle und Security Center alerts

1. Öffnen Sie die Befehlszeile und **cd** in **c:\Program Files\Microsoft Azure Protokoll Integration**.

2. Führen Sie den Befehl **Azlog Createazureid** ein [Azure Active Directory Service Principal](../active-directory/active-directory-application-objects.md) in Azure Active Directory (AD) Mieter erstellen, die den Azure-Abonnements enthalten.

    Sie werden aufgefordert, Ihre Azure anmelden.

    > [AZURE.NOTE] Sie müssen das Abonnement Besitzer oder Co-Administrator des Abonnements sein.

    Authentifizierung in Azure erfolgt über Azure AD.  Dienstprinzipalnamen für Azure Protokoll Integration wird Azure AD-Identität erstellen, die Lesezugriff auf Azure-Abonnements erhalten.

3. Ausführen der **Autorisieren Azlog <SubscriptionID> ** Befehl in Schritt 2 erstellten Dienstprinzipalnamen Lesezugriff auf das Abonnement zugewiesen. Wenn Sie **SubscriptionID**angeben, wird anschließend die Dienstprinzipalnamen Rolle alle Abonnements zugewiesen, Zugriff haben.

    > [AZURE.NOTE] Warnung möglicherweise angezeigt, wenn Sie den Befehl **Autorisieren** unmittelbar nach dem **Createazureid** -Befehl ausgeführt, da eine gewisse Latenzzeit zwischen der Erstellung des Azure AD-Kontos und das Konto zur Verfügung. Wenn Sie ungefähr 10 Sekunden nach dem Ausführen des Befehls **Createazureid** zum Ausführen des Befehls **Autorisieren** warten, sollten diese Warnung nicht angezeigt.

4. Überprüfen Sie die folgenden Ordner zu bestätigen, dass die JSON Protokolldateien:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Überprüfen Sie die folgenden Ordner um sicherzustellen, dass Sie Sicherheitshinweise des Sicherheitscenters vorhanden:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Zeigen Sie standard SIEM Datei Weiterleitung Connector in den entsprechenden Ordner auf die Daten der Instanz SIEM leiten. Siehe [SIEM-Konfigurationen](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) auf die SIEM-Konfiguration.

Zur Integration von Azure Protokoll Fragen senden Sie eine e-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Nächste Schritte

Azure Audit-Protokolle und Definitionen finden Sie unter:

- [Überwachen von Vorgängen mit Ressourcen-Manager](../resource-group-audit.md)
- [Liste der Ereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx) - Audit Log Ereignisse abzurufen.

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Sicherheitshinweise auf.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure-Neuigkeiten und Informationen.

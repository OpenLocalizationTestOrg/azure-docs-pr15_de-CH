<properties
   pageTitle="Erste Schritte mit Azure Protokoll Integration | Microsoft Azure"
   description="Informationen Sie zu Azure anmelden Integrationsdienst Protokolle von Azure-Speicher, Azure Prüfprotokolle und Azure Security Center Alerts zu integrieren."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Erste Schritte mit Azure Protokoll Integration (Vorschau)

Azure Protokoll-Integration können Sie unformatierte Protokollen von Azure Ressourcen in der lokalen Sicherheitsinformations- und Ereignismanagement (SIEM) integriert. Diese Integration ermöglicht ein einheitliches Dashboard für alle Ihre Daten lokal oder in der Cloud, damit Sie sammeln, Korrelieren, analysieren und Warnung für Sicherheitsereignisse Anwendung zugeordnet.

Dieses Lernprogramm führt Sie durch Azure Protokoll Integration installieren und Protokolle von Azure-Speicher, Azure Prüfprotokolle und Azure Security Center Alerts zu integrieren. Veranschlagte Zeit für diese praktische Einführung ist eine Stunde.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes:

- Ein Computer (lokal oder in der Cloud) Azure anmelden Integration-Dienst installieren. Dieser Computer muss eine 64-Bit-Windows-Betriebssystem mit .net installiert 4.5.1 ausgeführt werden. Dieser Computer heißt **Azlog Integrator**.
- Azure-Abonnement. Wenn Sie eine nicht verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/)anmelden.
- Azure-Diagnose für Ihren Azure virtuelle Maschinen (VMs) aktiviert. Diagnose für Cloud-Services finden Sie [Azure-Diagnose in Azure Cloud Services aktivieren](../cloud-services/cloud-services-dotnet-diagnostics.md). Diagnose für eine unter Windows Azure VM finden Sie [Mit PowerShell in einer virtuellen Maschine unter Windows Azure-Diagnose aktivieren](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Verbindung von Azlog Integrator Azure-Speicher zu authentifizieren und Autorisieren von Azure-Abonnement.
- Azure VM Protokolle installiert SIEM-Agent (universelle Splunk-Weiterleitung, HP ArcSight Windows-Ereignissammlungsdienst Agent oder IBM QRadar WinCollect) in Azlog Integrator.

## <a name="deployment-considerations"></a>Aspekte der Bereitstellung

Wenn Ereignis hoch ist, können Sie mehrere Instanzen von Azlog Integrator ausführen. Lastenausgleich Azure Diagnostics Speicher für Windows *(WAD)* und die Anzahl der Subskriptionen zu den Instanzen der Kapazität beruhen.

Auf einem Computer mit 8 Prozessoren (Core) kann eine einzelne Instanz von Azlog Integrator ca. 24 Millionen Ereignisse pro Tag (~1M/hour) verarbeiten.

Auf einem Computer 4-Prozessor (Core) kann eine einzelne Instanz von Azlog Integrator ungefähr 1,5 Millionen Ereignisse pro Tag (~62.5K/hour) verarbeiten.

## <a name="install-azure-log-integration"></a>Integration von Azure Protokoll installieren

[Azure anmelden Integration](https://www.microsoft.com/download/details.aspx?id=53324)herunterladen

Azure anmelden Integrationsdienst sammelt Daten von dem Computer, auf dem es installiert ist.  Telemetriedaten ist:

- Ausnahmen, die während der Ausführung von Azure anmelden integration
- Metriken zur Anzahl der Abfragen und verarbeiteten Ereignisse
- Statistiken über die Azlog.exe Befehlszeilenoptionen verwendet werden

> [AZURE.NOTE] Sie können die Sammlung von Daten deaktivieren diese Option deaktivieren.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Integration von Azure VM Protokolle Speicherkonten Azure-Diagnose

1. Überprüfen Sie um sicherzustellen, dass das Speicherkonto Bündel Protokolle sammeln, zunächst die Integration Azure Protokoll aufgeführten Komponenten. Führen die folgenden Schritte aus, wenn das Bündel Speicherkonto Protokolle nicht erfasst.

2. Öffnen Sie die Befehlszeile und **cd** in **c:\Program Files\Microsoft Azure Protokoll Integration**.

3. Führen Sie den Befehl

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      Ersetzen Sie StorageAccountName durch den Namen des Kontos Azure-Speicher konfiguriert die VM Diagnose Ereignisse empfangen.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Möchten Sie die Abonnement-Id im XML angezeigt werden, fügen Sie die Abonnement-ID angezeigter Name:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. 30-60 Minuten (einer Stunde dauern) und die Ereignisse, die das Speicherkonto entnommen werden. Öffnen **Ereignisanzeige > Windows-Protokolle > weitergeleitete Ereignisse** auf Azlog Integrator.

5. Stellen Sie sicher, dass der standard SIEM-Connector auf dem Computer installiert Ereignisse aus dem Ordner **Weitergeleitete Ereignisse** und Ihrer Instanz SIEM pipe konfiguriert ist. Überprüfen Sie Konfiguration der SIEM zu konfigurieren finden Sie unter Integration von Protokollen.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Wenn Daten nicht im Ordner "weitergeleitete Ereignisse" angezeigt werden?

Wird nach einer Stunde Daten nicht im Ordner " **Weitergeleitete Ereignisse** " klicken:

1. Überprüfen Sie den Computer und bestätigen sie Azure zugreifen können. Versuchen Sie zum Testen der Konnektivität, [Azure-Portal](http://portal.azure.com) im Browser öffnen.

2. Stellen Sie sicher, dass Benutzer Konto **Azlog** Schreibzugriff auf den Ordner **Users\azlog**.

3. Stellen Sie sicher das Speicherkonto Befehl **Azlog Quelle** hinzugefügt beim Ausführen den Befehl **Azlog Quellliste**aufgeführt ist.
4. Gehen Sie zu **Ereignisanzeige > Windows-Protokolle > Anwendung** gibt Fehler gemeldet von Azure Protokoll-Integration.

Wenn Sie weiterhin die Ereignisse dann sehen:

1. [Microsoft Azure Storage Explorer](http://storageexplorer.com/)herunterladen.

2. Verbinden Sie mit Speicherkonto Befehl **Azlog Quelle**hinzugefügt.

3. Microsoft Azure Storage Explorer suchen Sie Tabelle **WADWindowsEventLogsTable** auf Daten. Wenn nicht, dann Diagnose auf dem virtuellen Computer nicht richtig konfiguriert.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integration von Azure Überwachungsprotokolle und Sicherheitshinweise des Sicherheitscenters

1. Öffnen Sie die Befehlszeile und **cd** in **c:\Program Files\Microsoft Azure Protokoll Integration**.

2. Führen Sie den Befehl

        azlog createazureid

      Dieser Befehl fordert Sie Ihre Azure anmelden. Der Befehl erstellt dann ein [Azure Active Directory Service Principal](../active-directory/active-directory-application-objects.md) in Azure AD-Mandanten, die Azure-Abonnements enthalten, in denen der Benutzer ein Administrator, Co-Administrator oder Besitzer ist. Der Befehl schlägt fehl, wenn der Benutzer nur ein Gastbenutzer in Azure AD-Mandanten ist. Authentifizierung in Azure erfolgt durch Azure Active Directory (AD).  Erstellen einen Dienstprinzipal Azlog Integration erstellt Azure AD-Identität, die Lesezugriff auf Azure-Abonnements erhalten.

3. Führen Sie den Befehl

        azlog authorize <SubscriptionID>

      Dies weist Lesezugriff auf das Abonnement principal in Schritt 2 erstellt haben. Wenn Sie eine SubscriptionID angeben, versucht es principal Service Rolle zuweisen auf alle, die Abonnements Zugriff haben.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Warnungen möglicherweise angezeigt werden, wenn der Befehl **Autorisieren** unmittelbar nach dem **Createazureid** -Befehl ausgeführt. Es gibt eine gewisse Latenzzeit zwischen der Erstellung des Azure AD-Kontos und das Konto zur Verfügung. Wenn Sie ungefähr 10 Sekunden nach dem Ausführen des Befehls **Createazureid** zum Ausführen des Befehls **Autorisieren** warten, sollten diese Warnung nicht angezeigt.

4. Überprüfen Sie die folgenden Ordner zu bestätigen, dass die JSON Protokolldateien:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Überprüfen Sie die folgenden Ordner um sicherzustellen, dass Sie Sicherheitshinweise des Sicherheitscenters vorhanden:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Zeigen Sie standard SIEM Datei Weiterleitung Connector in den entsprechenden Ordner auf die Daten der Instanz SIEM leiten. Möglicherweise einige Feld Zuordnung basierend auf dem verwendeten SIEM.

Zur Integration von Azure Protokoll Fragen senden Sie eine e-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt zu Azure Protokoll Integration Protokolle von Azure-Speicher integriert. Weitere Informationen finden Sie hier:

- [Microsoft Azure Protokoll Integration für Azure-Protokolle (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center für Details, das System und Installationshinweise Azure Protokoll Integration.
- [Einführung in Azure Protokoll Integration](security-azure-log-integration-overview.md) – dieses Dokument führt Sie in Azure Protokoll Integration, seine wichtigsten Funktionen und deren Funktionsweise.
- [Konfigurationsschritte für Partner](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dieser Blogbeitrag zeigt Azure Protokoll Integration mit Splunk ArcSight HP und IBM QRadar zu konfigurieren.
- [Azure Protokoll Integration häufig gestellte Fragen (FAQ)](security-azure-log-integration-faq.md) - dieser FAQ beantwortet Fragen zur Azure Protokoll Integration.
- [Sicherheitshinweise des Sicherheitscenters Integration mit Azure anmelden Integration](../security-center/security-center-integrating-alerts-with-log-integration.md) – dieses Dokument zeigt, wie Synchronisierung Sicherheitshinweise des Sicherheitscenters mit virtuellen Sicherheitsereignisse werden von Azure Diagnostics und Azure Überwachungsprotokolle der Protokollanalyse oder SIEM-Lösung.
- [Neue Funktionen für Azure Diagnostics und Azure Überwachungsprotokolle](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) in diesem Blogbeitrag lernen Sie Überwachungsprotokolle Azure und andere Funktionen Einblicke in die Vorgänge der Azure-Ressourcen.

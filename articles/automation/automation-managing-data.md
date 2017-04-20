<properties 
   pageTitle="Verwalten von Azure Automation Daten | Microsoft Azure"
   description="Dieser Artikel enthält mehrere Themen zur Verwaltung von Azure Automation-Umgebung.  Derzeit enthält Data Retention und Azure Automatisierung Disaster Recovery in Azure Automation sichern."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Verwalten von Azure Automation-Daten

Dieser Artikel enthält mehrere Themen zur Verwaltung von Azure Automation-Umgebung.

## <a name="data-retention"></a>Datenaufbewahrung

Beim Löschen einer Ressource in Azure Automation bleibt es 90 Tage für Überwachungszwecke dauerhaft entfernt.  Sie nicht sehen oder verwenden Sie die Ressource in dieser Zeit.  Diese Richtlinie gilt auch für Ressourcen, die zur Automation-Konto gehören, die gelöscht wird.

Azure Automatisierung automatisch gelöscht und Aufträge, die älter als 90 Tage entfernt.

In der folgenden Tabelle werden die Aufbewahrungsrichtlinie für verschiedene Ressourcen zusammengefasst.

|Daten|Richtlinie|
|:---|:---|
|Konten|90 Tage nach dem Löschen des Kontos von einem Benutzer gelöscht.|
|Anlagen|90 Tage nach die Anlage von einem Benutzer gelöscht oder 90 Tage nach dem Konto, das hält die Anlage von einem Benutzer gelöscht wird dauerhaft entfernt.|
|Module|90 Tage nach das Modul von einem Benutzer gelöscht oder 90 Tage nach dem Konto, das hält das Modul von einem Benutzer gelöscht wird dauerhaft entfernt.|
|Runbooks|90 Tage nach die Ressource von einem Benutzer gelöscht oder 90 Tage nach dem Konto, das enthält, dass die Ressource von einem Benutzer gelöscht werden dauerhaft entfernt.|
|Aufträge|Gelöschte und dauerhaft entfernt 90 Tage nach dem letzten geändert wird. Dies könnte der Auftrag abgeschlossen ist, wird beendet oder angehalten wird.|
|Knoten Konfigurationen/MOF-Dateien| Alte Knotenkonfiguration wird 90 Tage nach eine neuen Knotenkonfiguration generiert wird dauerhaft entfernt.|
|DSC-Knoten| 90 Tage nach der Knoten von Automatisierungskonto Azure-Portal oder [Registrierung-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) -Cmdlet in Windows PowerShell ist entfernt dauerhaft. Knoten sind auch 90 Tage nach dem Konto gelöscht, die der Knoten von einem Benutzer gelöscht hat. |
|Knoten Berichte| 90 Tage nach dem Generieren eines neuen Berichts für diesen Knoten gelöscht|

Die Aufbewahrungsrichtlinie gilt für alle Benutzer und derzeit nicht angepasst werden.

## <a name="backing-up-azure-automation"></a>Sichern von Azure Automation

Wenn Sie in Microsoft Azure Automation-Konto löschen, werden alle Objekte im Konto gelöscht einschließlich Runbooks, Module, Konfigurationen, Einstellungen, Projekte und Ressourcen. Objekte können nicht wiederhergestellt werden, nachdem das Konto gelöscht wurde.  Die folgende Informationen können Sie um den Inhalt Ihres Kontos Automatisierung vor dem Löschen zu sichern. 

### <a name="runbooks"></a>Runbooks

Sie können Skriptdateien mit Azure-Verwaltungsportal oder das Cmdlet " [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) " in Windows PowerShell die Runbooks exportieren.  Diese Skriptdateien können in eine andere Automation-Konto importiert werden, wie in [Erstellen oder importieren ein Runbook](https://msdn.microsoft.com/library/dn643637.aspx).


### <a name="integration-modules"></a>Integrationsmodule

Sie können keine Integrationsmodule Azure Automation exportieren.  Sie müssen sicherstellen, dass sie außerhalb der Automation-Konto.

### <a name="assets"></a>Anlagen

[Ressourcen](https://msdn.microsoft.com/library/dn939988.aspx) können nicht von Azure Automation exportiert werden.  Azure-Verwaltungsportal verwenden, beachten Sie die Details der Variablen, Anmeldeinformationen, Zertifikate, Anschlüsse und Zeitpläne.  Sie müssen manuell alle Elemente erstellen, die von Runbooks verwendet werden, die in einem anderen Automatisierung importieren.

[Azure-Cmdlets](https://msdn.microsoft.com/library/dn690262.aspx) können Sie Details der unverschlüsselten Anlagen speichern für spätere abrufen oder entsprechende Anlagen in einem anderen Automation-Konto erstellen.

Den Wert für verschlüsselte Variablen oder das Kennwortfeld Anmeldeinformationen mithilfe von Cmdlets kann nicht abgerufen werden.  Wenn Sie diese Werte nicht kennen, können Sie sie in ein Runbook mit [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) und [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) Aktivitäten abrufen.

Zertifikate können nicht von Azure Automation exportiert werden.  Sie müssen sicherstellen, dass alle Zertifikate von Azure verfügbar sind.

### <a name="dsc-configurations"></a>DSC-Konfigurationen

Sie können die Konfigurationen auf Skriptdateien mit Azure-Verwaltungsportal oder [Export-AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) -Cmdlet in Windows PowerShell exportieren. Diese Konfigurationen können importiert und in ein anderes automatisierungskonto verwendet werden.


##<a name="geo-replication-in-azure-automation"></a>Geo-Replikation in Azure Automatisierung

Geo-Replikation Standard in Azure Automation-Konten sichert Daten einer anderen geographischen Region Redundanz. Primäre Region können beim Einrichten des Kontos und dann eine sekundäre Region zugewiesen automatisch. Die sekundären aus der primären Region kopierten Daten bei Datenverlust fortlaufend aktualisiert.  

Die folgende Tabelle zeigt die primären und sekundären Region verfügbaren Kombinationen.

|Primäre            |Sekundäre
| ---------------   |----------------
|Südlichen zentralen USA   |Norden der USA – zentral
|Osten der USA 2          |USA
|Westeuropa        |Nordeuropa
|Südostasien    |Ostasien
|Japan OST         |Japan West

In dem unwahrscheinlichen Fall, dass eine primäre Region wird versucht Microsoft wiederherzustellen. Wenn primären Daten wiederhergestellt werden können, dann Geo-Failover durchgeführt und die betroffenen Kunden darüber durch ihr Abonnement benachrichtigt.


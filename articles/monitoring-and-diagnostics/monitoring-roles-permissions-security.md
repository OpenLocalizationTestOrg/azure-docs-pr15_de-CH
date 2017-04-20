<properties
    pageTitle="Erste Schritte mit Rollen, Berechtigungen und Sicherheit mit Azure | Microsoft Azure"
    description="Informationen Sie zum Azure Monitor integrierte Rollen und Berechtigungen verwenden, um Zugriff auf Ressourcen überwachen."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Erste Schritte mit Rollen, Berechtigungen und Sicherheit mit Azure

Viele Teams müssen Zugang zu Daten und Einstellungen streng regulieren. Beispielsweise wenn Sie Teammitglieder arbeiten ausschließlich auf Überwachung (Support-Mitarbeiter Devops Engineers) oder verwenden Sie einen Dienstleister sie Zugriff nur Überwachungsdaten beschränkt die Fähigkeit zum erstellen möchten, ändern oder Löschen von Ressourcen. Dieser Artikel beschreibt, wie schnell eine integrierte Überwachung RBAC-Rolle zu einem Benutzer in Azure oder erstellen eigene benutzerdefinierte Rollen für einen Benutzer, der begrenzte Überwachung Berechtigungen benötigt. Es erläutert anschließend Sicherheitsaspekte für Ihr Monitor Azure Ressourcen und wie Sie Zugriff auf Daten beschränken, die sie enthalten.

## <a name="built-in-monitoring-roles"></a>Integrierte Überwachung Rollen

Azure Monitor integrierten Rollen werden sollen Zugriff auf Ressourcen in einem Abonnement dennoch verantwortlich für die Überwachung der Infrastruktur und Daten konfigurieren. Azure Monitor bietet zwei der vordefinierten Rollen: A Überwachung Reader und Mitwirkender überwachen.

### <a name="monitoring-reader"></a>Überwachen von Reader

Überwachen der Rolle zugewiesene Personen können Einstellungen für die Überwachung von Ressourcen anzeigen alle Daten in einem Abonnement aber kann keiner Ressource ändern oder bearbeiten. Diese Rolle ist für Benutzer in einer Organisation, z. B. Unterstützung oder Operationen Ingenieure können möchten:

- Zeigen Sie Überwachung Dashboards im Portal an und erstellen Sie eigene private Dashboards überwachen.
- Abfrage für Metriken mit [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-Cmdlets](insights-powershell-samples.md)oder [plattformübergreifende CLI](insights-cli-samples.md).
- Fragen Sie mit Portal, Azure Monitor REST API, PowerShell-Cmdlets oder plattformübergreifende CLI Aktivitätsprotokoll ab.
- Die [Diagnose Einstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) für eine Ressource anzeigen
- [Protokoll-Profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) für ein Abonnement anzeigen.
- Ansichtsoptionen skalieren.
- Warnung Aktivität anzeigen und Einstellungen.
- Application Insights Daten und zeigen Sie Daten im AI Analytics an.
- Suche Arbeitsbereichsdaten Protokoll Analytics (OMS) einschließlich der Daten für den Arbeitsbereich.
- Protokoll-Analyse (OMS) Gruppen anzeigen
- Suchschema Protokoll Analytics (OMS) abrufen.
- Liste Protokoll Analytics (OMS) Intelligence Packs.
- Abrufen und Log Analytics (OMS) gespeicherte Suchvorgänge ausführen.
- Die Protokoll-Analyse (OMS) Konfiguration abrufen.

> [AZURE.NOTE] Diese Funktion gibt nicht Lesezugriff auf Daten, die an einen Hub Ereignis übertragen oder in ein Speicherkonto gespeichert wurde. [Unten finden Sie](#security-considerations-for-monitoring-data) Informationen zum Konfigurieren des Zugriffs auf diese Ressourcen.

### <a name="monitoring-contributor"></a>Überwachen der Teilnehmer

Überwachen der Teilnehmerrolle zugewiesene Personen können alle Daten in einem Abonnement anzeigen und oder Überwachung Einstellungen ändern, aber keine Ressourcen ändern. Diese Rolle stellt eine Obermenge der Rolle überwachen und eignet sich für einer Organisation überwachen Team oder managed Service Provider, die zusätzlich zu den oben genannten Berechtigungen auch können müssen:

- Veröffentlichen Sie Überwachung Dashboards als freigegebene Dashboard.
- Sie [Diagnose-Einstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) für einen resource.*
- Das [Protokoll Profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) für einen subscription.*
- Warnung Aktivität und Einstellungen festgelegt.
- Erstellen von Webtests Anwendung Einblicke und Komponenten.
- Liste Protokoll Analytics (OMS) Workspace gemeinsamer Schlüssel.
- Aktivieren Sie oder deaktivieren Sie Log Analytics (OMS) Intelligence Packs.
- Erstellen und löschen und Log Analytics (OMS) gespeicherte Suchvorgänge ausführen.
- Erstellen Sie und löschen Sie die Speicherkonfiguration Protokoll Analytics (OMS).

* Benutzer muss auch ListKeys-für die Zielressource (Storage Konto oder Ereignis Hub Namespace) eines Protokolls Profil oder Diagnose Einstellung Berechtigung.

> [AZURE.NOTE] Diese Funktion gibt nicht Lesezugriff auf Daten, die an einen Hub Ereignis übertragen oder in ein Speicherkonto gespeichert wurde. [Unten finden Sie](#security-considerations-for-monitoring-data) Informationen zum Konfigurieren des Zugriffs auf diese Ressourcen.

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Überwachen von Berechtigungen und benutzerdefinierte RBAC-Rollen
Wenn die oben genannten integrierten Rollen Bedürfnisse Ihres Teams nicht erfüllen, können Sie mit spezifischere Berechtigungen [erstellt eine benutzerdefinierte RBAC-Rolle](../active-directory/role-based-access-control-custom-roles.md) . Unten sind allgemeinen Azure Monitor RBAC-Operationen mit Beschreibung.

| Vorgang                                                   | Beschreibung                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read, schreiben, löschen]         | Lesen/Schreiben/Löschen von Warnregeln.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Liste von Warnregeln Vorfälle (Geschichte der Warnregel ausgelöst). Dies gilt nur für das Portal.                                              |
| Microsoft.Insights/AutoscaleSettings/[Read, schreiben, löschen]  | Lesen/Schreiben/Löschen Autoscale Settings.                                                                                                                     |
| Microsoft.Insights/DiagnosticSettings/[Read, schreiben, löschen] | Lesen/Schreiben/Löschen Diagnose Settings.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Diese Berechtigung ist für Benutzer, die Zugriff auf Aktivitäten über das Portal erforderlich.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Liste Aktivitätsprotokoll Ereignisse (Verwaltung) in einem Abonnement. Diese Berechtigung gilt für programmgesteuerte und Portal Zugriff auf das Aktivitätsprotokoll. |
| Microsoft.Insights/LogDefinitions/Read                      | Diese Berechtigung ist für Benutzer, die Zugriff auf Aktivitäten über das Portal erforderlich.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Lesen Sie metrische Definitionen (Liste der verfügbaren metrischen Typen für eine Ressource).                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Lesen Sie Metriken für eine Ressource.                                                                                                                              |

> [AZURE.NOTE] Zugriff auf Alerts, der diagnostische und Metriken für eine Ressource erfordert, dass der Benutzer Lesezugriff auf Typ und Bereich der Ressource. Eine Diagnose ein Speicherkonto oder Datenströme Ereignis Hubs Archive Einstellung oder Protokoll-Profil muss erstellen ("Write") der Benutzer auch ListKeys-Berechtigung für die Zielressource.

Beispielsweise verwenden die obige Tabelle eine benutzerdefinierte RBAC-Rolle kann für eine "Aktivität Protokollleser" wie folgt erstellen:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Sicherheitsaspekte von Überwachungsdaten
Überwachungsdaten – besonders Protokolldateien, können vertrauliche Informationen wie IP-Adressen oder Benutzernamen enthalten. Überwachungsdaten von Azure liegt in drei Formen vor:
1. Das Aktivitätsprotokoll beschreibt alle Steuerungsebene Aktionen Ihres Azure-Abonnements.
2. Diagnoseprotokolle, die Protokolle, die von einer Ressource ausgegeben.
3. Metriken von Ressourcen ausgegeben werden.

Alle drei dieser Datentypen kann ein Speicherkonto gespeichert oder an Event Hub allgemeine Azure Ressourcen sind übertragen. Da sind allgemeine Ressourcen ist erstellen, löschen und den Zugriff ein privilegierter Vorgang normalerweise Administrator. Wir empfehlen, dass Sie die folgenden Methoden zum Überwachen von Ressourcen verwenden, um Missbrauch vorzubeugen:

- Verwenden Sie eine einzelne, spezielle Speicherkonto zur Überwachung von Daten. Benötigen Sie Überwachungsdaten in mehrere Storage-Konten aufteilen, niemals ein Speicherkonto zwischen Verbrauch und nicht überwachen Daten Dies kann versehentlich die nur Zugriff auf Überwachungsdaten (z. b. ein Drittanbieter-SIEM) nicht überwachen auf Daten.
- Verwenden eines einzelnen dedizierten Service Bus oder Ereignis Hub Namespaces über alle Diagnose Einstellungen aus demselben Grund wie oben.
- Beschränken des Zugriffs auf Überwachung Speicherkonten oder Ereignis Hubs in einer eigenen Ressourcengruppe und Überwachung Rollen [Bereich](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) auf nur Gruppe halten.
- Wenn ein Benutzer nur Zugriff auf Überwachungsdaten erteilen Sie Berechtigungen ListKeys-Speicherkonten oder Ereignis Hubs zur Abonnementebene. Stattdessen geben diese Berechtigungen für dem Benutzer auf eine Ressource oder Ressourcengruppe (Wenn Sie eine dedizierte Überwachung Ressourcengruppe) Bereich.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Einschränken des Zugriffs auf Überwachung Speicher
Wenn ein Benutzer oder eine Anwendung Zugriff auf Überwachungsdaten in ein Speicherkonto benötigt, sollten Sie das Speicherkonto, das Überwachungsdaten Servicelevel nur-Lese-Zugriff auf BLOB-Speicher enthält [eine Konto-SAS erzeugen](https://msdn.microsoft.com/library/azure/mt584140.aspx) . In PowerShell könnte dies aussehen:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Sie können dem Token zur Person geben, muss dieser Speicher gelesen Konto und können Liste alle Blobs in diesem Storage-Konto lesen.

Auch benötigen Sie diese Berechtigung mit RBAC steuern, können Sie diese Entität Microsoft.Storage/storageAccounts/listkeys/action Berechtigungen für dieses Konto Speicher gewähren. Dies ist erforderlich für Benutzer, die möglicherweise eine Diagnose Einstellung oder ein Speicherkonto archivieren-Profil anmelden. Beispielsweise können Sie folgende benutzerdefinierte RBAC-Rolle für einen Benutzer oder eine Anwendung, die nur ein Speicherkonto gelesen erstellen:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] Die ListKeys-Berechtigung kann der Benutzer die primären und sekundären Speicher kontoschlüssel aufgelistet. Diese Schlüssel gewähren Sie dem Benutzer alle Berechtigungen angemeldet (Lesen, schreiben, Erstellen von Blobs, Löschen von Blobs usw.) für alle Dienste (BLOB-Warteschlange Tabelle, Datei) in diesem Storage-Konto angemeldet. Wir empfehlen eine Konto-SAS beschriebenen, wenn möglich.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Einschränken des Zugriffs auf monitoring-Veranstaltung hubs
Ein ähnliches Muster folgen mit Event-Hubs, jedoch müssen Sie zunächst eine dedizierte hören Autorisierungsregel erstellen. Wenn Sie Zugriff auf eine Anwendung, die nur Überwachung bezogene Ereignis Hubs anhören möchten, folgendermaßen Sie vor:

1. Erstellen einer Richtlinie gemeinsamen Zugriff auf das Ereignis ausgelieferte(n), die für das streaming von Daten mit nur Listen erstellt wurden. Dies kann im Portal erfolgen. Beispielsweise können Sie sie "MonitoringReadOnly" aufrufen Wenn möglich, sollten Sie diesen Schlüssel direkt an dem Verbraucher und überspringen Sie den nächsten Schritt.
2. Wenn der Verbraucher muss zu den wichtigsten Ad-hoc-, erteilen Sie Benutzer die ListKeys-Aktion für dieses Ereignis-Hub. Dies ist auch für Benutzer, die möglicherweise eine Diagnose Einstellung oder Stream Ereignis Hubs Profil anmelden. Beispielsweise können Sie eine RBAC-Regel erstellen:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Nächste Schritte
- [Informationen Sie zu RBAC und Berechtigungen im Ressourcen-Manager](../active-directory/role-based-access-control-what-is.md)
- [Lesen Sie die Übersicht der Überwachung in Azure](monitoring-overview.md)

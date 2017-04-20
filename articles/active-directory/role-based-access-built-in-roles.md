<properties
    pageTitle="RBAC: Integrierte Rollen | Microsoft Azure"
    description="Dieses Thema beschreibt die integrierte Rollen rollenbasierte Zugriffskontrolle (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Integrierte Rollen

(Azure Role-Based Access Control, RBAC) enthält die folgenden integrierten Rollen, die Benutzer, Gruppen und Dienste zugewiesen werden können. Sie können die Definitionen von Standardrollen nicht ändern. Allerdings können Sie [benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md) den Bedürfnissen Ihrer Organisation erstellen.

## <a name="roles-in-azure"></a>Rollen in Azure

Die folgende Tabelle enthält kurze Beschreibung vordefinierter Rollen. Klicken Sie auf den Rollennamen, um die detaillierte Liste der **Aktionen** und **Notactions** für die Rolle. **Actions** -Eigenschaft gibt die zulässigen Aktionen auf Azure-Ressourcen. Aktion-Zeichenfolgen können Platzhalterzeichen verwenden. Die **Notactions** -Eigenschaft gibt die Aktionen, die die zulässigen Aktionen ausgeschlossen werden.

>[AZURE.NOTE] Azure Funktionsdefinitionen werden immer raffinierter. Dieser Artikel ist so aktuell wie möglich gehalten, aber finden Sie immer die aktuellen Rollen Definitionen in Azure PowerShell. Verwenden Sie die Cmdlets `(get-azurermroledefinition "<role name>").actions` oder `(get-azurermroledefinition "<role name>").notactions` nach Bedarf.

| Rollenname | Beschreibung |
| --------- | ----------- |
| [API-Management Service Teilnehmer](#api-management-service-contributor) | API-Management-Services verwalten können |
| [Anwendung Einblicke Komponente Teilnehmer](#application-insights-component-contributor) | Kann Anwendung Einblicke Komponenten verwalten |
| [Automatisierung Operator](#automation-operator) | Damit starten, stoppen, unterbrechen und Fortsetzen von Aufträgen |
| [BizTalk-Teilnehmer](#biztalk-contributor) | BizTalk-Dienste verwalten können |
| [ClearDB MySQL DB Contributor](#cleardb-mysql-db-contributor) | Kann ClearDB MySQL-Datenbanken verwalten |
| [Teilnehmer](#contributor) | Dies kann alles Zugriff verwalten. |
| [Factory Mitwirkenden für Daten](#data-factory-contributor) | Kann erstellen und verwalten Daten Factorys und untergeordneten Ressourcen darin. |
| [DevTest Labs Benutzer](#devtest-labs-user) | Alles und verbinden, Start, Neustart und Herunterfahren virtueller Computer |
| [DNS-Zone Teilnehmer](#dns-zone-contributor) | Kann DNS-Zonen und Einträge verwalten |
| [DocumentDB-Konto Teilnehmer](#documentdb-account-contributor) | DocumentDB-Konten verwalten kann |
| [Intelligente Systeme Konto Teilnehmer](#intelligent-systems-account-contributor) | Intelligente Systeme Konten verwalten kann |
| [Netzwerk-Teilnehmer](#network-contributor) | Können alle Netzwerk-Ressourcen verwalten |
| [Neue Relikt APM Konto Teilnehmer](#new-relic-apm-account-contributor) | Können neue Relikt Application Performance Management Konten und Applikationen verwalten |
| [Besitzer](#owner) | Kann alles verwalten, einschließlich Zugriff |
| [Reader](#reader) | Alles anzeigen, jedoch keine Änderungen |
| [Redis Cache-Teilnehmer](#redis-cache-contributor) | Redis Cache verwalten können |
| [Scheduler Job Sammlungen Teilnehmer](#scheduler-job-collections-contributor) | Können Planer Auftrag verwalten |
| [Suche Service-Mitarbeiter](#search-service-contributor) | Suchdienste verwalten können |
| [Sicherheits-Manager](#security-manager) | Können Sicherheitskomponenten, Sicherheitsrichtlinien und virtuelle Computer verwalten |
| [SQL DB Contributor](#sql-db-contributor) | Können SQL-Datenbanken, jedoch nicht ihre Sicherheits-Richtlinien verwalten |
| [SQL Security Manager](#sql-security-manager) | Können Richtlinien Sicherheit von SQL Server und Datenbanken verwalten |
| [SQL Server-Teilnehmer](#sql-server-contributor) | Kann SQL Server und Datenbanken, jedoch nicht deren sicherheitsbezogener Richtlinien verwalten |
| [Klassische Storage Konto Teilnehmer](#classic-storage-account-contributor) | Klassische Speicherkonten verwalten kann |
| [Storage-Konto Teilnehmer](#storage-account-contributor) | Storage-Konten verwalten kann |
| [Benutzeradministrator-Zugriff](#user-access-administrator) | Verwalten des Benutzerzugriffs auf Azure Ressourcen können |
| [Klassische virtuellen Teilnehmer](#classic-virtual-machine-contributor) | Verwalten können klassische Virtual Machines, aber nicht die virtuellen Netzwerk- oder Speicher-Konto mit dem sie verbunden sind |
| [Virtual Machine-Teilnehmer](#virtual-machine-contributor) | Verwalten können virtuelle Computer, jedoch nicht die virtuellen Netzwerk- oder Speicher-Konto mit dem sie verbunden sind |
| [Klassische Netzwerk Teilnehmer](#classic-network-contributor) | Können klassische virtuelle Netzwerke und reservierte IP-Adressen verwalten |
| [Web Plan Contributor](#web-plan-contributor) | Kann Web-Pläne verwalten |
| [Website-Teilnehmer](#website-contributor) | Verwalten können Websites, aber nicht Web Pläne mit denen sie verbunden sind |

## <a name="role-permissions"></a>Berechtigungen
Die folgenden Tabellen beschreiben die spezifischen Berechtigungen jeder Rolle. Dazu gehören **Aktionen**, die Berechtigungen zu erteilen und **NotActions**einschränken.

### <a name="api-management-service-contributor"></a>API-Management Service Teilnehmer
API-Management-Services verwalten können

| **Aktionen** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Erstellen und Verwalten von API Management Services |
| Microsoft.Authorization/*/read | Leseberechtigung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen Sie Rollen und Aufgaben der Rolle |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="application-insights-component-contributor"></a>Anwendung Einblicke Komponente Teilnehmer
Kann Anwendung Einblicke Komponenten verwalten

| **Aktionen** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben der Rolle |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Komponenten Einblicke |
| Microsoft.Insights/webtests/* | Erstellen und Verwalten von Webtests |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="automation-operator"></a>Automatisierung Operator
Damit starten, stoppen, unterbrechen und Fortsetzen von Aufträgen

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben der Rolle |
| Microsoft.Automation/automationAccounts/jobs/read | Automatisierung Konto Aufträge lesen |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Einen Automatisierung Konto Auftrag fortsetzen |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Einen Automatisierung Konto Auftrag beenden |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Automatisierung Konto Auftrag Streams lesen |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Einen Automatisierung Konto Auftrag anhalten |
| Microsoft.Automation/automationAccounts/jobs/write | Schreiben der Automatisierung Konto Aufträge |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Ein Kontenschema Auftrag Automatisierung lesen |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Ein Kontenschema Auftrag Automatisierung lesen |
| Microsoft.Automation/automationAccounts/read | Lesen Sie Automatisierung |
| Microsoft.Automation/automationAccounts/runbooks/read | Automatisierung Runbooks lesen |
| Microsoft.Automation/automationAccounts/schedules/read | Automatisierung Kontenschemata lesen |
| Microsoft.Automation/automationAccounts/schedules/write | Automatisierung Kontenschemata schreiben |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Komponenten Einblicke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="biztalk-contributor"></a>BizTalk-Teilnehmer
BizTalk-Dienste verwalten können

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben der Rolle |
| Microsoft.BizTalkServices/BizTalk/* | Erstellen und Verwalten von BizTalk-Diensten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Contributor
Kann ClearDB MySQL-Datenbanken verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben der Rolle |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |
| successbricks.cleardb/Databases/* | Erstellen und Verwalten von ClearDB MySQL-Datenbanken |

### <a name="contributor"></a>Teilnehmer
Alles außer Zugriff verwalten können

| **Aktionen** ||
| ------- | ------ |
| * | Erstellen Sie und verwalten Sie aller Ressourcen |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Rollen und Aufgaben der Rolle kann nicht gelöscht werden. |  
| Microsoft.Authorization/*/Write | Rollen und Aufgaben der Rolle kann nicht erstellt werden. |

### <a name="data-factory-contributor"></a>Factory Mitwirkenden für Daten
Erstellen Sie und verwalten Sie Daten Factorys und untergeordneten Ressourcen darin.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.DataFactory/dataFactories/* | Erstellen Sie und verwalten Sie Daten Factorys und untergeordneten Ressourcen darin. |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="devtest-labs-user"></a>DevTest Labs Benutzer
Alles und verbinden, Start, Neustart und Herunterfahren virtueller Computer

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.Compute/availabilitySets/read | Eigenschaften der Verfügbarkeit legt |
| Microsoft.Compute/virtualMachines/*/read | Eigenschaften einer virtuellen Maschine (VM Größen Runtime Status, VM-Erweiterungen usw.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Virtuelle Computer freigeben |
| Microsoft.Compute/virtualMachines/read | Die Eigenschaften eines virtuellen Computers |
| Microsoft.Compute/virtualMachines/restart/action | Virtuelle Maschinen starten |
| Microsoft.Compute/virtualMachines/start/action | Virtuelle Maschinen starten |
| Microsoft.DevTestLab/*/read | Eigenschaften eines Labors |
| Microsoft.DevTestLab/labs/createEnvironment/action | Erstellen einer Lab-Umgebung |
| Microsoft.DevTestLab/labs/formulas/delete | Löschen von Formeln |
| Microsoft.DevTestLab/labs/formulas/read | Lesen von Formeln |
| Microsoft.DevTestLab/labs/formulas/write | Hinzufügen oder Ändern von Formeln |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Auswerten von labrichtlinien |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Beitreten eines Load Balancer Backend-Adresspools |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Ein Lastenausgleich Join eingehende NAT-Regel |
| Microsoft.Network/networkInterfaces/*/read | Eigenschaften einer Netzwerkschnittstelle (z. B. alle den Lastenausgleich, die Netzwerkschnittstelle gehört) |
| Microsoft.Network/networkInterfaces/join/action | Fügen Sie einen virtuellen Computer eine Netzwerkschnittstelle |
| Microsoft.Network/networkInterfaces/read | Netzwerkschnittstellen lesen |
| Microsoft.Network/networkInterfaces/write | Schreiben von Netzwerkschnittstellen |
| Microsoft.Network/publicIPAddresses/*/read | Eigenschaften einer öffentlichen IP-Adresse |
| Microsoft.Network/publicIPAddresses/join/action | Eine öffentliche IP-Adresse beitreten |
| Microsoft.Network/publicIPAddresses/read | Öffentliche IP-Netzwerkadressen lesen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Ein virtuelles Netzwerk |
| Microsoft.Resources/deployments/operations/read | Bereitstellung Lesevorgänge |
| Microsoft.Resources/deployments/read | Bereitstellung lesen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/listKeys/action | Speicherkontoschlüssel Liste |

### <a name="dns-zone-contributor"></a>DNS-Zone Teilnehmer
DNS-Zonen und Einträge managen.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/\*lesen | Lesen Sie Rollen und Aufgaben der Rolle |
| Microsoft.Insights/alertRules/\* | Erstellen und Verwalten von Warnregeln |
| Microsoft.Network/dnsZones/\* | Erstellen und Verwalten von DNS-Zonen und Einträge |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie den Zustand Ihrer Ressourcen |
| Microsoft.Resources/deployments/\* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/\* | Erstellen und Verwalten von Supportanfragen |


### <a name="documentdb-account-contributor"></a>DocumentDB-Konto Teilnehmer
DocumentDB-Konten verwalten kann

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.DocumentDb/databaseAccounts/* | Erstellen und Verwalten von Konten DocumentDB |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="intelligent-systems-account-contributor"></a>Intelligente Systeme Konto Teilnehmer
Intelligente Systeme Konten verwalten kann

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.IntelligentSystems/accounts/* | Erstellen und Verwalten von intelligenten Systemen Konten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="network-contributor"></a>Netzwerk-Teilnehmer
Können alle Netzwerk-Ressourcen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.Network/* | Erstellen und verwalten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="new-relic-apm-account-contributor"></a>Neue Relikt APM Konto Teilnehmer
Können neue Relikt Application Performance Management Konten und Applikationen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |
| NewRelic.APM/accounts/* | Erstellen Sie und verwalten Sie neuer Relikt Application Performance management |

### <a name="owner"></a>Besitzer
Kann alles verwalten, einschließlich Zugriff

| **Aktionen** ||
| ------- | ------ |
| * | Erstellen Sie und verwalten Sie aller Ressourcen |

### <a name="reader"></a>Reader
Alles anzeigen, jedoch keine Änderungen

| **Aktionen** ||
| ------- | ------ |
| * / Lesen | Ressourcen aller Typen außer Schlüssel zu lesen. |

### <a name="redis-cache-contributor"></a>Redis Cache-Teilnehmer
Redis Cache verwalten können

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.Cache/redis/* | Erstellen und Verwalten von Redis-caches |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="scheduler-job-collections-contributor"></a>Scheduler Job Sammlungen Teilnehmer
Können Planer Auftrag verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Scheduler/jobcollections/* | Erstellen und Verwalten von Job-Sammlungen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen  |

### <a name="search-service-contributor"></a>Suche Service-Mitarbeiter
Suchdienste verwalten können

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Search/searchServices/* | Erstellen und Verwalten von Suchdiensten |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen  |

### <a name="security-manager"></a>Sicherheits-Manager
Können Sicherheitskomponenten, Sicherheitsrichtlinien und virtuelle Computer verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.ClassicCompute/*/read | Lesen Sie Classic berechnen virtuelle Computer Informationen |
| Microsoft.ClassicCompute/virtualMachines/*/write | Konfiguration für virtuelle Computer schreiben |
| Microsoft.ClassicNetwork/*/read | Informationen über klassische Netzwerk gelesen  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Security/* | Erstellen und Verwalten von Sicherheitskomponenten und Richtlinien |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen  |

### <a name="sql-db-contributor"></a>SQL DB Contributor
Können SQL-Datenbanken jedoch nicht ihre Sicherheits-Richtlinien verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Rollen und Aufgaben-Rolle |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Sql/servers/databases/* | Erstellen und Verwalten von SQL-Datenbanken |
| Microsoft.Sql/servers/read | SQL Server lesen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Überwachungsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditingSettings/* | Einstellungen kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Datensätze können nicht gelesen werden. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Verbindungsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Daten, die Maskierung Richtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Warnung Sicherheitsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/securityMetrics/* | Metriken können nicht bearbeitet werden. |

### <a name="sql-security-manager"></a>SQL Security Manager
Können Richtlinien Sicherheit von SQL Server und Datenbanken verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie Microsoft-Autorisierung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Sql/servers/auditingPolicies/* | Erstellen und Verwalten von Überwachungsrichtlinien für SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Erstellen und Verwalten von SQL Server überwachen festlegen |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Erstellen und Verwalten von Überwachungsrichtlinien für SQL Server-Datenbank |
| Microsoft.Sql/servers/databases/auditingSettings/* | Erstellen und Verwalten von Überwachungsrichtlinien für SQL Server-Datenbank |
| Microsoft.Sql/servers/databases/auditRecords/read | Datensätze gelesen |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Erstellen und Verwalten von SQL Server-Datenbank-Verbindungsrichtlinien |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Erstellen und Verwalten von SQL Server-Datenbankdaten masking-Richtlinien |
| Microsoft.Sql/servers/databases/read | Lesen Sie SQL-Datenbanken |
| Microsoft.Sql/servers/databases/schemas/read | Lesen Sie SQL Server-Datenbankschemas |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Lesen Sie SQL Server-Datenbankspalten |
| Microsoft.Sql/servers/databases/schemas/tables/read | Lesen Sie SQL Server-Datenbanktabellen |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Erstellen und Verwalten von SQL Server-Datenbank-Warnung Sicherheitsrichtlinien |
| Microsoft.Sql/servers/databases/securityMetrics/* | Erstellen und Verwalten von SQL Server-Datenbank Metriken |
| Microsoft.Sql/servers/read | SQL Server lesen |
| Microsoft.Sql/servers/securityAlertPolicies/* | Erstellen und Verwalten von SQL Server-Warnung Sicherheitsrichtlinien |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="sql-server-contributor"></a>SQL Server-Teilnehmer
Kann SQL Server und Datenbanken jedoch nicht ihre Sicherheits-Richtlinien verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Leseberechtigung|
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Sql/servers/* | Erstellen und Verwalten von SQL Server |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Überwachungsrichtlinien für SQL Server kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/auditingSettings/* | Überwachungsrichtlinien für SQL Server kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Überwachungsrichtlinien für SQL Server-Datenbank kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungsrichtlinien für SQL Server-Datenbank kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Datensätze können nicht gelesen werden. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Richtlinien für SQL Server-Datenbank-Verbindung kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL Server-Datenbankdaten masking Richtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL Server-Datenbank-Warnung Sicherheitsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/securityMetrics/* | SQL Server-Datenbank Metriken können nicht bearbeitet werden. |
| Microsoft.Sql/servers/securityAlertPolicies/* | SQL Server-Warnung Sicherheitsrichtlinien kann nicht bearbeitet werden. |

### <a name="classic-storage-account-contributor"></a>Klassische Storage Konto Teilnehmer
Klassische Speicherkonten verwalten kann

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Leseberechtigung |
| Microsoft.ClassicStorage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="storage-account-contributor"></a>Storage-Konto Teilnehmer
Storage-Konten können nicht darauf zugreifen.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Alle Leseberechtigung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.Insights/diagnosticSettings/* | Diagnostische verwalten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Storage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="user-access-administrator"></a>Benutzeradministrator-Zugriff
Verwalten des Benutzerzugriffs auf Azure Ressourcen können

| **Aktionen** ||
| ------- | ------ |
| * / Lesen | Ressourcen aller Typen außer Schlüssel zu lesen. |
| Microsoft.Authorization/* | Verwalten der Autorisierung |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="classic-virtual-machine-contributor"></a>Klassische virtuellen Teilnehmer
Verwalten können klassische virtuelle Computer aber nicht die virtuellen Netzwerk- oder Speicher-Konto mit dem sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Leseberechtigung |
| Microsoft.ClassicCompute/domainNames/* | Erstellen und Verwalten von klassischen Compute-Domänennamen |
| Microsoft.ClassicCompute/virtualMachines/* | Erstellen und Verwalten von virtuellen Maschinen |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Netzwerksicherheitsgruppen beitreten |
| Microsoft.ClassicNetwork/reservedIps/link/action | Reservierte IP-Adressen verknüpfen |
| Microsoft.ClassicNetwork/reservedIps/read | Lesen reservierte IP-Adressen |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Virtuelle Netzwerken beitreten |
| Microsoft.ClassicNetwork/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Konto-Datenträger lesen |
| Microsoft.ClassicStorage/storageAccounts/images/read | Storage-Konto Bilder |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Speicherkontoschlüssel Liste |
| Microsoft.ClassicStorage/storageAccounts/read | Klassische Speicherkonten lesen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="virtual-machine-contributor"></a>Virtual Machine-Teilnehmer
Verwalten können virtuelle Computer aber nicht die virtuellen Netzwerk- oder Speicher-Konto mit dem sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Leseberechtigung |
| Microsoft.Compute/availabilitySets/* | Erstellen und Verwalten von Compute Verfügbarkeit legt |
| Microsoft.Compute/locations/* | Erstellen und Verwalten von Compute-Speicherorte |
| Microsoft.Compute/virtualMachines/* | Erstellen und Verwalten von virtuellen Maschinen |
| Microsoft.Compute/virtualMachineScaleSets/* | Erstellen und Verwalten von virtuellen Maßstab legt |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Netzwerk-Gateway Backend-Adresse Anwendungspools beitreten |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Load Balancer Backend-Adresspools beitreten |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Beitreten zum Lastenausgleich eingehende NAT-Pools |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Beitreten zum Lastenausgleich eingehende NAT-Regeln |
| Microsoft.Network/loadBalancers/read | Lastenausgleich lesen |
| Microsoft.Network/locations/* | Erstellen und Verwalten von Netzwerkadressen |
| Microsoft.Network/networkInterfaces/* | Erstellen und Verwalten von Netzwerkschnittstellen |
| Microsoft.Network/networkSecurityGroups/join/action | Netzwerksicherheitsgruppen beitreten |
| Microsoft.Network/networkSecurityGroups/read | Lesen von Netzwerk-Sicherheitsgruppen |
| Microsoft.Network/publicIPAddresses/join/action | Öffentliche IP-Netzwerkadressen beitreten |
| Microsoft.Network/publicIPAddresses/read | Öffentliche IP-Netzwerkadressen lesen |
| Microsoft.Network/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.Network/virtualNetworks/subnets/join/action | Virtuelle Netzwerk-Subnetze beitreten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Storage/storageAccounts/listKeys/action | Speicherkontoschlüssel Liste |
| Microsoft.Storage/storageAccounts/read | Speicherkonten lesen |
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="classic-network-contributor"></a>Klassische Netzwerk Teilnehmer
Können klassische virtuelle Netzwerke und reservierte IP-Adressen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Leseberechtigung |
| Microsoft.ClassicNetwork/* | Erstellen und Verwalten von klassischen Netzwerke |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |

### <a name="web-plan-contributor"></a>Web Plan Contributor
Kann Web-Pläne verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Leseberechtigung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |
| Microsoft.Web/serverFarms/* | Erstellen und Verwalten von Serverfarmen |

### <a name="website-contributor"></a>Website-Teilnehmer
Verwalten können Websites jedoch nicht Web Pläne mit denen sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Leseberechtigung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnregeln Einblicke |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Komponenten Einblicke |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status der Ressourcen lesen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellung |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |  
| Microsoft.Support/* | Erstellen und Verwalten von Supportanfragen |
| Microsoft.Web/certificates/* | Erstellen und Verwalten von Website-Zertifikate |
| Microsoft.Web/listSitesAssignedToHostName/read | Lesen von Websites ein Hostname zugewiesen |
| Microsoft.Web/serverFarms/join/action | Join-Serverfarmen |
| Microsoft.Web/serverFarms/read | Serverfarmen lesen |
| Microsoft.Web/sites/* | Erstellen und Verwalten von websites |

## <a name="see-also"></a>Siehe auch
- [Rollenbasierte Zugriffskontrolle](role-based-access-control-configure.md): Erste Schritte mit RBAC in Azure-Portal.
- [Benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md): benutzerdefinierte Rollen entsprechend Ihrer Bedürfnisse Zugriff erstellen.
- [Erstellen einer Access Änderungshistorie](role-based-access-control-access-change-history-report.md): Überblick ändern rollenzuweisungen RBAC.
- [Rollenbasierte Zugriffskontrolle Problembehandlung](role-based-access-control-troubleshooting.md): Vorschläge für die Behebung von Problemen zu erhalten.

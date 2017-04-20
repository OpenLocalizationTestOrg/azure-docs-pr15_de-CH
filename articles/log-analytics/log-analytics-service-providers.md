<properties
    pageTitle="Analysefunktionen für Dienstanbieter anmelden | Microsoft Azure"
    description="Protokollanalyse können Managed Service Providers (MSPs) große Unternehmen Independent Software Vendors (ISVs) und Hosting-Dienstanbieter verwalten und Überwachen von Servern im Standort des Kunden oder Cloud-Infrastruktur."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Analysefunktionen für Dienstanbieter anmelden

Protokollanalyse können managed Service Providers (MSPs), große Unternehmen, unabhängige Softwareanbieter (ISVs) und Hosting-Dienstanbieter verwalten und Überwachen von Servern im Standort des Kunden oder Cloud-Infrastruktur. 

Großunternehmen Dienstleister viele Ähnlichkeit freigeben, gibt es eine zentrale IT-Team, das für die Verwaltung zuständig ist IT für verschiedene Geschäftseinheiten. Zur Vereinfachung dieses Dokument verwendet den Begriff *Dienstanbieter* jedoch die gleiche Funktionalität auch für Unternehmen und andere Kunden.

## <a name="cloud-solution-provider"></a>Cloud-Lösungsanbieter

Für Partner und Dienstanbieter sind Teil der [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) Anwendung gehört Protokollanalyse der Azure-Dienste auf einem CSP-Abonnement. 

Folgenden Funktionen sind für Protokollanalyse *Cloud-Lösungsanbieter* Abonnements aktiviert.

Als *Cloud-Lösungsanbieter* können Sie:

+ Erstellen Sie Protokollanalyse Arbeitsbereiche Mieter (Kunde) Abonnement.
+ Zugriff von Mieter erstellte Arbeitsbereiche. 
+ Hinzufügen und Entfernen von Benutzerzugriff auf Arbeitsbereich Azure Verwaltung. Ein Mieter Arbeitsbereich im Portal OMS Seite die Verwaltung unter Einstellung ist nicht verfügbar
  - Protokollanalyse unterstützt rollenbasierten Zugriff noch -Benutzer geben `reader` Berechtigung im Azure-Portal ermöglicht diese Konfiguration OMS-Portal ändern

Ein Mieter Abonnement anmelden müssen Sie Mandanten-ID angeben. Der Mieter Bezeichner ist häufig das letzte Teil der e-Mail-Adresse anmelden.

+ Im Portal OMS hinzufügen `?tenant=contoso.com` in der URL für das Portal. Zum Beispiel`mms.microsoft.com/?tenant=contoso.com`
+ In PowerShell verwenden die `-Tenant contoso.com` Parameter mit `Add-AzureRmAccount` Cmdlets
+ Mandanten-ID wird automatisch hinzugefügt, wenn Sie verwenden die `OMS portal` Verknüpfung von Azure-Portal zu öffnen, und melden Sie sich beim OMS-Portal für den ausgewählten Arbeitsbereich

Als *Kunde* Sie können Cloud-Lösungsanbieter:

+ Protokoll CSP Abonnement Analytics Arbeitsbereiche erstellen
+ Der CSP erstellt Access-Arbeitsbereiche
  -  Verwenden der `OMS portal` Verknüpfung von Azure-Portal zu öffnen, und melden Sie sich beim OMS-Portal für den ausgewählten Arbeitsbereich
+ Anzeigen und Benutzer Verwaltung Seite unter Einstellungen im OMS-portal

>[AZURE.NOTE] Backup und Site Recovery Lösungen für Protokollanalyse können nicht in ein Depot Recovery Services verbinden und können in einem Abonnement CSP konfiguriert.

## <a name="managing-multiple-customers-using-log-analytics"></a>Verwalten von mehreren Kunden Protokollanalyse 

Es wird empfohlen, einen Arbeitsbereich Protokollanalyse für jeden Debitor erstellen, die Sie verwalten. Protokollanalyse Arbeitsbereich bietet:

+ Ein Standort für Daten gespeichert werden. 
+ Granularität für die Abrechnung 
+ Datenisolation 
+ Eindeutige Konfiguration

Erstellen Sie einen Arbeitsbereich pro Kunde, können Sie Daten des Kunden separate sowie die Verwendung jedes Kunden verfolgen.

Weitere Informationen darüber, wann und warum Sie mehrere Arbeitsbereiche erstellen gemäß [verwalten Zugriff protokollieren Analytics] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Erstellung und Konfiguration der Kunde Arbeitsbereiche können automatisiert werden, [PowerShell](log-analytics-powershell-workspace-configuration.md) [Ressourcenmanager Vorlagen](log-analytics-template-workspace-configuration.md)oder die [REST-API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Die Verwendung der Ressourcen-Manager Vorlagen für Workspace-Konfiguration können Sie eine master-Konfiguration, die zum Erstellen und Konfigurieren von Arbeitsbereichen. Sie können sicher sein, dass Arbeitsbereiche für Kunden erstellt sie automatisch an Ihre Bedürfnisse konfiguriert werden. Beim Aktualisieren der Vorschriften die Vorlage wird aktualisiert und erneut vorhandener Arbeitsbereiche. Dadurch wird sichergestellt, dass vorhandene Arbeitsbereiche neue Standards entsprechen.    

Mehrere Protokollanalyse Arbeitsbereiche verwalten, empfehlen wir die vorhandene Ticketsystem jeder Arbeitsbereich integrieren / Operatorkonsole mithilfe [der Warnungsfunktion](log-analytics-alerts.md) . Integration in Ihre vorhandenen Systeme können Mitarbeiter weiterhin ihre vertraute Prozesse befolgen. Protokollanalyse regelmäßig überprüft jeden Arbeitsbereich mit festgelegten Warnungskriterien und generiert eine Warnung, wenn Maßnahmen erforderlich.

Für executive Berichte, die Daten zusammenfassen Arbeitsbereiche hinweg können Sie die Integration Protokollanalyse und [PowerBI](log-analytics-powerbi.md). Benötigen Sie ein anderes reporting System integrieren, können die Such-API (über PowerShell oder [REST](log-analytics-log-search-api.md)) Sie Abfragen ausführen und Ergebnisse exportieren.

## <a name="next-steps"></a>Nächste Schritte

+ Automatisieren der Erstellung und Konfiguration von Arbeitsbereichen [Ressourcenmanager](log-analytics-template-workspace-configuration.md) Vorlagen
+ Automatische Erstellung von Arbeitsbereichen mit [PowerShell](log-analytics-powershell-workspace-configuration.md) 
+ Verwenden Sie [Alerts](log-analytics-alerts.md) Integration mit vorhandenen Systemen
+ Zusammenfassende Berichte über [PowerBI](log-analytics-powerbi.md)

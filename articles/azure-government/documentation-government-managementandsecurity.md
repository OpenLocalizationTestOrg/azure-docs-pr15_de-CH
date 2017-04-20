<properties
    pageTitle="Azure Regierung Dokumentation | Microsoft Azure"
    description="Dies bietet einen Vergleich der Features und Hinweise auf die Anwendungsentwicklung für Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Azure Government Management und Sicherheit

## <a name="automation"></a>Automatisierung

Automatisierung ist erhältlich in Azure Government.

### <a name="variations"></a>Variationen

Die folgenden Automatisierungsfeatures sind nicht derzeit in Azure Regierung.

+ Erstellung von Service-Prinzip Anmeldeinformationen für die Authentifizierung

Weitere Informationen finden Sie unter [Automatisierung öffentliche Dokumentation](../automation/automation-intro.md).


##  <a name="key-vault"></a>Key Vault
Einzelheiten zu diesen Dienst und dessen Verwendung finden Sie die <a href="https://azure.microsoft.com/documentation/services/key-vault">Öffentliche Dokumentation Azure Key Vault.</a>
### <a name="data-considerations"></a>Aspekte der Daten
Die folgenden Informationen identifizieren Azure Government Grenze Azure Key Vault:

| Reguliert/gesteuert Daten erlaubt | Reguliert/gesteuert Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle mit einer Azure Key Vault-Schlüssel verschlüsselte Daten enthalten Daten reguliert/gesteuert. | Azure Key Vault Metadaten darf nicht Exportkontrolle Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten beim Erstellen und Verwalten von Ihrem Tresor Schlüssel eingegeben.  Regulierte/gesteuert Daten nicht in die folgenden Felder eingeben: Ressourcengruppennamen Key Vault Namen, Namen |

Key Vault ist erhältlich in Azure Government. In der Öffentlichkeit ist es keine Erweiterung Key Vault über PowerShell und CLI nur.
## <a name="log-analytics"></a>Protokollanalyse
Protokollanalyse ist erhältlich in Azure Government. 

### <a name="variations"></a>Variationen

Protokollanalyse Funktionen und Solutions stehen nicht derzeit in Azure Regierung. Diese Aktualisierung bei den Status des Features / Solutions ändert.

+ In der Vorschau in öffentlichen Azure Solutions einschließlich:
  - Monitoring-Lösung
  - Überwachen der Anwendung Abhängigkeit
  - Office 365-Lösung
  - Windows 10 Upgrade Analytics-Lösung
  - Anwendung Einblicke
  - Azure Networking Analytics-Lösung
  - Azure Automation Analytics
  - Schlüssel Depot Analytik
+ Projektmappen und Features, Updates von lokalen Software benötigen, einschließlich
  - Integration mit System Center Operations Manager 2016 (frühere Versionen von Microsoft Operations Manager werden unterstützt)
  - Computer Gruppen von System Center Configuration Manager
  - Hub Lösung
+ Funktionen, die in der Vorschau in öffentlichen Azure einschließlich
  - Exportieren von Daten PowerBI
+ Azure Metriken und Azure-Diagnose
+ OMS Mobile applications

URLs für Protokollanalyse unterscheiden sich in Azure Government:

| Azure Public | Azure Government | Notizen |
|--------------|------------------|-------|
| MMS.Microsoft.com | OMS.Microsoft.US | Analytics-Portal anmelden |
| *WorkspaceId*. ods.opinsights.azure.com | *WorkspaceId*. ods.opinsights.azure.us | [Datensammler API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent-Kommunikation - [Firewall konfigurieren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent-Kommunikation - [Firewall konfigurieren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent-Kommunikation - [Firewall konfigurieren](../log-analytics/log-analytics-proxy-firewall.md) |


Analysefunktionen von Protokoll weisen unterschiedliche Azure Government:

+ Der Windows-Agent muss [Protokollanalyse Portal](https://oms.microsoft.us) für Azure heruntergeladen werden.
+ Zum Protokollanalyse System Center Operations Manager-Verwaltungsserver herstellen, müssen Sie downloaden und aktualisierte Management Packs importieren.
  1. Laden Sie und speichern Sie die [managementpacks aktualisiert](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Extrahieren Sie die heruntergeladenen Datei
  3. Operations Manager importieren Sie managementpacks. Informationen zum Importieren ein Management Packs von einer Diskette finden [eine Operations Manager Management Pack importieren](http://technet.microsoft.com/library/hh212691.aspx) auf der Microsoft TechNet-Website.
  4. Operations Manager Protokollanalyse Verbindung, folgen Sie den Schritten [Protokollanalyse Operations Manager verbinden](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

+ Kann ich Daten von Protokollanalyse öffentlichen Azure Azure Regierung migrieren?
  - Nein. Es ist nicht möglich, öffentliche Azure Azure Regierung Daten oder den Arbeitsbereich verschieben.
+ Kann ich zwischen öffentlichen Azure und Azure Government Arbeitsbereiche von OMS-Protokollanalyse-Portal wechseln?
  - Nein. Portale für Öffentliche Azure und Azure unterscheiden und Informationen nicht freigeben. 

Weitere Informationen finden Sie unter [Öffentliche Protokollanalyse-Dokumentation](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Informationen und Updates Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Regierung Blog.</a>
 

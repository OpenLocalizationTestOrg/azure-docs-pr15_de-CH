<properties
    pageTitle="Azure Regierung Dokumentation | Microsoft Azure"
    description="Dies bietet einen Vergleich der Features und Hinweise auf die Anwendungsentwicklung für Azure."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure Regierung Überwachung und Verwaltung

Dieser Artikel beschreibt die Überwachung und Management services-Varianten und für die Regierung Azure-Umgebung.

## <a name="automation"></a>Automatisierung

Automatisierung ist erhältlich in Azure Government.

### <a name="variations"></a>Variationen

Die folgenden Automatisierungsfeatures sind nicht derzeit in Azure Regierung.

+ Erstellung von Service-Prinzip Anmeldeinformationen für die Authentifizierung

Weitere Informationen finden Sie unter [Automatisierung öffentliche Dokumentation](../automation/automation-intro.md).

## <a name="log-analytics"></a>Protokollanalyse

Protokollanalyse ist erhältlich in Azure Government.

### <a name="variations"></a>Variationen

Protokollanalyse Funktionen und Solutions stehen nicht derzeit in Azure Regierung.

+ In der Vorschau in Microsoft Azure Solutions einschließlich:
  - Monitoring-Lösung
  - Anwendungslösung Abhängigkeit überwachen
  - Office 365-Lösung
  - Windows 10 Upgrade Analytics-Lösung
  - Einblicke Lösung
  - Azure Networking Analytics-Lösung
  - Azure Automatisierung Analytics-Lösung
  - Schlüssel Depot Analytik Lösung
+ Projektmappen und Features, Updates von lokalen Software benötigen, einschließlich:
  - Integration mit System Center Operations Manager 2016 (frühere Versionen von Microsoft Operations Manager werden unterstützt)
  - Computer Gruppen von System Center Configuration Manager
  - Hub Lösung
+ Funktionen, die in der Vorschau in öffentlichen Azure einschließlich:
  - Export der Daten in Power BI
+ Azure Metriken und Azure-Diagnose
+ Mobile Operations Management Suite-Anwendung

URLs für Protokollanalyse unterscheiden sich in Azure Government:

| Azure Public | Azure Government | Notizen |
|--------------|------------------|-------|
| MMS.Microsoft.com | OMS.Microsoft.US | Analytics-Portal anmelden |
| *WorkspaceId*. ods.opinsights.azure.com | *WorkspaceId*. ods.opinsights.azure.us | [Datensammler API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent-Kommunikation - [Firewall konfigurieren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent-Kommunikation - [Firewall konfigurieren](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent-Kommunikation - [Firewall konfigurieren](../log-analytics/log-analytics-proxy-firewall.md) |


Analysefunktionen von Protokolldateien Verhalten in Azure Government unterschiedlich:

+ Der Windows-Agent muss [Protokollanalyse Portal](https://oms.microsoft.us) für Azure heruntergeladen werden.
+ Zum Protokollanalyse System Center Operations Manager-Verwaltungsserver herstellen, müssen Sie downloaden und aktualisierte managementpacks importieren.
  1. Laden Sie und speichern Sie der [managementpacks aktualisiert](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Entpacken Sie die heruntergeladene Datei.
  3. Operations Manager importieren Sie managementpacks. Informationen zum Importieren eines Management Packs von einer Diskette finden Sie auf der Microsoft TechNet-Website [eine Operations Manager Management Pack importieren](http://technet.microsoft.com/library/hh212691.aspx) .
  4. Operations Manager Protokollanalyse Verbindung, folgen Sie den Schritten [Protokollanalyse Operations Manager herstellen](../log-analytics/log-analytics-om-agents.md).


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

+ Kann ich Daten von Protokollanalyse in Microsoft Azure Azure Regierung migrieren?
  - Nein. Es ist nicht möglich, Daten oder den Arbeitsbereich von Microsoft Azure Azure Regierung verschieben.
+ Kann ich zwischen Microsoft Azure und Azure Regierung Arbeitsbereiche Operations Management Suite Protokollanalyse-Portal wechseln?
  - Nein. Portale für Microsoft Azure und Azure unterscheiden und Informationen nicht freigeben.

Weitere Informationen finden Sie unter [Öffentliche Protokollanalyse-Dokumentation](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Informationen und Updates Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Regierung Blog.</a>

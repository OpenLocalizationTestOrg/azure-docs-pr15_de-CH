<properties
    pageTitle="Neuigkeiten in Azure Stapel | Microsoft Azure"
    description="Neuigkeiten in Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Neuigkeiten in Azure Stapel Technical Preview 2
Diese Version bietet neue Funktionen für Mieter und Administratoren.

## <a name="network"></a>Netzwerk   
   - [iDNS](azure-stack-understanding-dns-in-tp2.md) bietet interne netzwerkregistrierung und Auflösung ohne zusätzliche DNS-Infrastruktur (DNS = Domain Name System).
   - [Virtuelle Netzwerk-Gateways](azure-stack-create-vpn-connection-one-node-tp2.md) bieten VPN-Konnektivitätsoptionen für Azure oder lokale Ressourcen.
   - Benutzer definierten Routen können Netzwerk Datenverkehr über Firewalls, Sicherheit, andere Geräte und andere Dienste.
   - Sie können Ressourcen aus dem Markt erstellen.   

## <a name="storage"></a>Speicher
 - [Azure-Warteschlangen](https://msdn.microsoft.com/library/dd179353.aspx) aktivieren zuverlässig und permanente messaging.
 - [Speicheranalyse](https://msdn.microsoft.com/library/azure/hh343270.aspx) Storage Performance Daten. Diese Daten können Sie Anfragen verfolgen, Verwendungstrends analysieren und diagnostizieren Probleme mit Ihrem Speicherkonto.
 - BLOB-Speicher unterstützt [Block angehängt](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Premium Storage API-Kundensupport.
 - Mieter Storage Service-Support für gängige Programme und SDKs Azure CLI PowerShell, .NET, Python und Java SDK. 
 - [Storage Konto SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx) aktivieren Unterteilung des Zugriffs auf Speicher-Services ohne Ihre Rechnung Schlüssel.  
 - Speicher-Services verwenden [Gruppe verwaltete Dienstkonten](https://technet.microsoft.com/library/hh831477.aspx) für hohe Sicherheit mit geringen Verwaltungsaufwand.
 - Sie können nicht verwendete Mieter Ressourcen bei Bedarf freigeben.
 - Gelöschte Konto Aufbewahrung Speicherlänge ist konfigurierbar.
 - Sie können gelöschte Mieter Speicherkonten wiederherstellen.

## <a name="compute"></a>Berechnen
- Sie können virtuelle Computer freigeben.
- Sie können virtuelle Computer Extensions für Problembehandlung und Configuration Management erneut bereitstellen.
- Sie können Festplatten mit virtuellen Maschinen ändern.
- Virtuelle Computer können mehrere Netzwerkschnittstellen.

### <a name="portal-experience"></a>Portalfunktionalität
 - Azure Stapel Bereiche sind eine logische Einheit und Management in Azure Stapel. In dieser Vorschau können Sie Informationen auf Dienste wie Computing-, Netzwerk- und Speicher nach Region anzeigen.
 - Sie können jetzt Schnittstelle [Azure Stapel updates.md] (Updates) anzeigen.

## <a name="key-vault"></a>Key Vault
- [Key Vault Azure Stack](azure-stack-kv-intro.md) bietet sicheren Schlüssel und Kennwörter für Cloud-apps.
- Sie können überwachen die Verwendung von apps und VMs und überwachen.

## <a name="billing-and-usage"></a>Abrechnung und Verwendung
 - [Abrechnung und Verbrauch APIs](azure-stack-billing-and-chargeback.md) stellen Daten wie Ihre Dienstleistungen verbraucht werden.  
 - Delegierte Anbietern ermöglichen Händler Ihre Azure Stapel Dienstleistungen für ihre Kunden.

## <a name="monitoring-and-health"></a>Überwachung und Status
 - Können Sie neue Überwachungsfunktionen im Portal und APIs proaktiv anzeigen und Verwalten von Warnungen in Ihrer Umgebung.  

## <a name="next-steps"></a>Nächste Schritte
- [Kennen Sie Azure Stack POC-Architektur](azure-stack-architecture.md)      
- [Bereitstellung erforderlicher Komponenten verstehen](azure-stack-deploy.md)
- [Bereitstellen von Azure Stapel](azure-stack-run-powershell-script.md)

  

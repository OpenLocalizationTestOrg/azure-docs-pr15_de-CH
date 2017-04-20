<properties
    pageTitle="Azure Regierung Dokumentation | Microsoft Azure"
    description="Dies bietet einen Vergleich der Features und Hinweise auf die Anwendungsentwicklung für Azure"
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
    ms.date="08/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-documentation-overview"></a>Azure Government-Übersicht

##  <a name="introduction-to-azure-government-documentation"></a>Einführung in Azure Government Dokumentation

Diese Seite beschreibt die Funktionen des [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) Services und bietet allgemeine Leitlinien für alle Kunden. Vor speziell regulierte Daten in Ihrem Abonnement Azure Regierung, sollten mit Azure Government Funktionen vertraut und Fragen Ihr Kundenbetreuer-Team wenden.

Lesen Sie die [Microsoft Azure Trust Center Compliance Seite](http://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx) für aktuelle Azure Government Dienstleistungen gemäß bestimmten Akkreditierungen. Weitere Microsoft-Dienste auch zur Verfügung, aber sind nicht Rahmen behandelt Azure Government Services und nicht Gegenstand dieses Dokuments. Azure Government Services können auch zulassen, verwenden viele zusätzliche Ressourcen, Programme oder Dienste von Drittanbietern – oder Microsoft separaten Rahmen und Datenschutzrichtlinien – die nicht in diesem Dokument enthalten sind. Sie sind verantwortlich für die Überprüfung von Zahlungsbedingungen alle solche "Add-on"-Angebote, wie Markt angeboten, um sicherzustellen, dass sie auf Ihre Bedürfnisse.

Azure Government steht für Entitäten, die Daten, die bestimmte Vorschriften und Auflagen (wie NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 und CJIS verarbeiten), Verwendung von Azure Government muss Vorschriften. Azure Regierungskunden unterliegen Überprüfung der Berechtigung.

Personen mit Berechtigung für Azure Fragen wenden ihr Kundenteam.

##  <a name="principles-for-securing-customer-data-in-azure-government"></a>Grundsätze für das Sichern von Kundendaten in Azure Government

Azure Government bietet eine Reihe von Features und Diensten, mit denen Sie Cloud Lösungen zu Ihren Bedürfnissen geregelt/gesteuert. Eine kompatible Lösung ist nichts anderes als die Umsetzung der vordefinierten Azure Government Funktionen gekoppelt mit solide Sicherheit.
Microsoft behandelt diese Anforderungen auf der Ebene der Cloud-Infrastruktur beim Hosten einer Lösung in Azure Regierung.

Das folgende Diagramm zeigt das Azure Verteidigung Modell. Microsoft bietet beispielsweise grundlegende Cloudinfrastruktur DDOS, mit Kunden Funktionen wie Sicherheits-Appliances für kundenspezifische Anwendung benötigten DDOS.

![ALT-text](./media/azure-government-Defenseindepth.png)

Diese Seite beschreibt die Grundprinzipien für die Services und Applikationen, die Leitfäden und bewährte Methoden, wie diese Grundsätze gelten; in anderen Worten, wie Kunden nutzen sollten smart Azure Regierung die Pflichten und Aufgaben, die für eine Lösung, die ITAR enthält.

Die übergeordneten Prinzipien zum Sichern von Daten sind:
* Datenschutz mit Verschlüsselung
* Kennwörter verwalten
* Isolation Datenzugriff einschränken

##  <a name="protecting-customer-data-using-encryption"></a>Schutz von Kundendaten Verschlüsselung

Risikominderung und Einhaltung von gesetzlichen Vorschriften fahren die zunehmenden Fokus und Verschlüsselung. Eine effektive verschlüsselungsimplementierung aktuellen Netzwerk- und Sicherheitsmaßnahmen zu verwenden – und das Gesamtrisiko der Cloudumgebung.

### <a name="Overview"></a>Verschlüsselung ruhender
Die Verschlüsselung von Daten gilt für den Schutz der kundeninhalte im Speicher. Es gibt hierfür mehrere Arten:

### <a name="Overview"></a>Storage Service-Verschlüsselung

Azure Storage Service-Verschlüsselung ist auf Speicherebene Konto aktiviert Block-Blobs sowie Seitenblobs automatisch verschlüsselt werden, wenn in den Azure-Speicher geschrieben. Beim Lesen der Daten aus dem Azure-Speicher wird es vom Speicherdienst entschlüsselt vor. Hiermit sichern Sie Ihre Daten ohne ändern oder jede Anwendung Code hinzufügen.

### <a name="Overview"></a>Verschlüsselung Azure Festplatten
Verwenden Sie Azure Datenträgerverschlüsselung zum Verschlüsseln von Betriebssystem-Datenträger und Datenträger verwendet eine Azure Virtual Machine. Integration mit Azure Schlüssel gibt Ihnen Kontrolle und Datenträger Verschlüsselungsschlüssel verwalten können.

### <a name="Overview"></a>Clientseitige Verschlüsselung
Clientseitige Verschlüsselung integriert die Java und .NET Speicher Clientbibliotheken, die damit einfach implementieren Azure Key Vault APIs nutzen können. Verwenden Sie Azure Key Vault den Zugriff auf vertrauliche Informationen in Azure Key Vault für Einzelpersonen mit Azure Active Directory.

### <a name="Overview"></a>Verschlüsselung in transit

Die einfache Verschlüsselung Konnektivität Azure Regierung unterstützt Transport Level Security (TLS) 1.2 Protokoll und x. 509-Zertifikate. Bundesrepublik Informationen Processing Standard (FIPS) 140-2 Level 1-Verschlüsselungsalgorithmen auch für Infrastruktur Netzwerkanschlüsse zwischen Azure Government Rechenzentren verwendet werden.  Windows Server 2012 R2 und Windows 8-plus VMs und Azure Dateifreigaben können SMB 3.0 für die Verschlüsselung zwischen der VM und Dateifreigabe. Verwenden Sie clientseitige Verschlüsselung zum Verschlüsseln der Daten erfolgt in Speicher in einer Clientanwendung und zum Entschlüsseln der Daten nach dem aus übertragen.

### <a name="Overview"></a>Best Practices für Verschlüsselung

* IaaS VMs: Verwenden Sie Azure Datenträgerverschlüsselung. Storage Service Verschlüsselung Verschlüsseln VHD-Dateien, mit denen die Festplatten in Azure-Speicher sichern einschalten, aber dies nur neu geschriebene Daten verschlüsselt. Dies bedeutet, dass Sie einen virtuellen Computer erstellen, aktivieren Sie Storage Service Verschlüsselung auf das Speicherkonto, das VHD-Datei enthält die Änderungen verschlüsselt werden, nicht die ursprüngliche VHD-Datei.
* Clientseitige Verschlüsselung: Dies ist die sicherste Methode zum Verschlüsseln von Daten vor der Übertragung verschlüsselt und verschlüsselt die Daten. Es erfordert, dass Sie Ihre Anwendung mit Speicher nicht möchten möglicherweise Code hinzufügen. In diesen Fällen können HTTPs für Ihre Daten bei der Übertragung und Verschlüsselung Storage Service Sie die Daten verschlüsseln. Clientseitige Verschlüsselung erfordert mehr Auslastung auf dem Client, müssen Sie diese in Ihre Pläne Skalierbarkeit berücksichtigt insbesondere dann, wenn Sie verschlüsseln und große Datenmengen übertragen.

Weitere Informationen über die Verschlüsselung finden Sie unter Optionen in Azure [Storage Security Guide](/storage-security-guide).

##  <a name="protecting-customer-data-by-managing-secrets"></a>Schutz von Kundendaten durch Kennwörter verwalten

Sichere Schlüsselmanagement ist unverzichtbar für Datenschutz in der Cloud. Kunden sollten versuchen, Key Management vereinfachen und Kontrolle von Cloudanwendungen und Dienste zum Verschlüsseln von Daten verwendet.

### <a name="Overview"></a>Best Practices für die Verwaltung von vertraulichen Daten

* Verwenden Sie Schlüssel Vault, um Risiken ausgesetzt durch hartcodierte Konfigurationsdateien, Skripts oder im Quellcode Geheimnisse. Azure Key Vault verschlüsselt (wie die Verschlüsselungsschlüssel für Azure Disk Encryption) und Schlüssel (z. B. Kennwörter), speichern sie im FIPS 140-2 Level 2 Hardwaresicherheitsmodule (HSMs) überprüft. Für zusätzliche Sicherheit importieren oder in diese HSMs Schlüssel generieren.
* Anwendungscode und Vorlagen sollten nur URI-Verweise auf Daten enthalten (d. h., der tatsächliche Schlüssel nicht im Code, Konfiguration oder Quellcode Repositories). Dadurch werden wichtige Phishing-Angriffe auf internen oder externen Repogeschäfte wie Ernte-Bots im GitHub.
* Verwenden Sie starke RBAC-Steuerelemente in Schlüssel Depot. Wenn vertrauenswürdige Mitarbeiter das Unternehmen oder die Übertragung zu einer neuen Gruppe innerhalb der Firma verlässt, sollte verhindert werden auf Daten zugreifen.  

Weitere Informationen finden Sie unter [Schlüssel Depot für Azure](/azure-government/azure-government-tech-keyvault)

##  <a name="isolation-to-restrict-data-access"></a>Isolation Datenzugriff einschränken

Isolation ist mit Grenzen, Segmentierung und Containern Datenzugriff nur autorisierte Benutzer, Dienste und Programme beschränken. Beispielsweise ist die Trennung zwischen einen wesentlichen Mechanismus für mandantenfähigen Cloudplattformen wie Microsoft Azure. Logischer Isolierung verhindert, dass ein Mandant beeinträchtigt den Betrieb der anderen Mandanten.

### <a name="Overview"></a>Isolierung der Umgebung
Azure Government ist eine physische Instanz getrennt vom Rest des Microsoft Netzwerk. Dies wird durch eine Reihe von physischen und logischen Steuerelemente, die folgende: Sicherung Hindernisse mit biometrischen Geräten und Kameras.  Verwendung von bestimmten Anmeldeinformationen und die kombinierte Authentifizierung von Microsoft Personal logischen Zugriff auf Produktions-Umgebung.  Alle Infrastruktur für Azure befindet sich in den USA.

#### <a name="Overview"></a>Pro Kunde Isolation
Netzwerkzugriffsteuerung Azure implementiert und Trennung durch VLAN-Isolierung ACLs laden Balancers und IP-Filter

Kunden können ihre Ressourcen über Abonnements, Ressourcengruppen, virtuelle Netzwerke und Subnetze weiter eingrenzen.

Weitere Informationen zu Microsoft Azure Isolation finden Sie unter [Isolation Abschnitt Azure Security](/azure-security-getting-started/#isolation).

Für zusätzliche Informationen zum Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Regierung Blog.</a>

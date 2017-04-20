<properties
    pageTitle="Multi-Tenant-Web Application Muster | Microsoft Azure"
    description="Suchen Sie strukturierte Übersichten und Entwurfsmuster, die beschreiben, wie eine Anwendung Multi-Tenant Azure implementiert."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Mandantenfähigen Applikationen in Azure

Eine mehrinstanzenfähige Anwendung ist eine freigegebene Ressource, die separate Benutzer oder "Mieter" die Anwendung anzeigen, als sei es eigene ermöglicht. Eine Situation, die zu einer mehrinstanzenfähigen Anwendung gehört, in dem alle Benutzer möchten die Benutzeroberfläche anpassen aber ansonsten die gleichen Vorschriften für Unternehmen. Beispiele für große mandantenfähigen Applikationen sind Office 365, Outlook.com und visualstudio.com.

Aus Sicht einer Anwendungsanbieter beziehen sich die Vorteile des Tenancy meist betriebliche Effizienz Kosten. Eine Version der Anwendung kann den Bedürfnissen der viele Mandanten/Kunden, Konsolidierung des Systems Verwaltungsaufgaben wie monitoring, Performance-tuning, Software-Wartung und Daten.

Die folgenden enthält eine Liste der wichtigsten Ziele und Vorgaben hinsichtlich des Anbieters.

- **Bereitstellung**: Sie dürfen neue Mieter für die Anwendung bereitstellen.  Mandantenfähigen Applikationen mit einer großen Anzahl der Mandanten ist es normalerweise notwendig, diesen Prozess automatisieren, indem Sie aktivieren Self-service-Bereitstellung.
- **Wartungsfreundlichkeit**: Sie dürfen die Anwendung und anderen Wartungsaufgaben ausführen, während mehrere Mandanten verwendet werden.
- **Überwachung**: Sie müssen möglicherweise die Anwendung jederzeit Probleme zu identifizieren und zu beheben. Dazu gehören wie jeder der Anwendung überwachen.

Eine ordnungsgemäß implementierte mandantenfähige Anwendung bietet die folgenden Vorteile für Benutzer.

- **Isolation**: die Aktivitäten der einzelnen Mandanten wirken sich nicht auf die Verwendung der Anwendung durch andere Mandanten. Mieter können nicht auf die Daten zugreifen. Offenbar, Pächter haben exklusive Verwendung der Anwendung.
- **Verfügbarkeit**: einzelne Mieter möchten vielleicht mit einer SLA definiert ständig verfügbar sein. Aktivitäten anderer Mandanten sollten wieder keinen Einfluss auf die Verfügbarkeit der Anwendung.
- **Skalierung**: die Anwendung der einzelnen Mandanten Bedarf skaliert. Das Vorhandensein und die Aktionen anderer Mandanten sollten nicht die Leistung der Anwendung beeinträchtigen.
- **Kosten**: Kosten sind geringer als das Ausführen einer Anwendung und einem Mandanten, da mehrere Mandanten ermöglicht die gemeinsame Nutzung von Ressourcen.
- **Anpassungsfähigkeit**. Die Möglichkeit zum Anpassen der Anwendung für eine einzelne Mandanten auf verschiedene Weise wie hinzufügen oder Entfernen von Features, Farben und Logos ändern oder sogar ihren eigenen Code oder Skripts hinzufügen.

Kurz gesagt, sind viele Faktoren, die Sie berücksichtigen müssen hochgradig skalierbare Dienste bereitstellen, werden auch eine Reihe von Zielen und Anforderungen in vielen mandantenfähigen Einige möglicherweise nicht in bestimmten Szenarien, und die Bedeutung der einzelnen Ziele und Vorgaben werden in jedem Szenario unterscheiden. Als Anbieter von mandantenfähigen Anwendung haben Sie auch Anforderungen, Einhaltung der Mieter Ziele und Anforderungen, Rentabilität, Abrechnung, mehrere Service-Level, Bereitstellung, Verwaltung Überwachung und Automatisierung.

Weitere Informationen über zusätzliche Aspekte einer mehrinstanzenfähigen Anwendung finden Sie in [einer mehrinstanzenfähigen Anwendung in Azure][]. Finden Sie Informationen auf gemeinsame Daten Architektur Multi-Tenant-Software als Service (SaaS) ergeben [Entwurfsmuster für Multi-Tenant SaaS mit Azure SQL-Datenbank](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure bietet viele Funktionen, die wichtigsten Probleme beim Entwerfen eines mandantenfähigen Systems an.

**Isolierung**

- Segment Website Mieter von Hostheadern mit oder ohne SSL-Kommunikation
- Segment Website Mieter von Abfrageparametern
- Webdienste in Workerthreads
    - Worker-Rollen. in der Regel verarbeiten, die Daten im Back-End einer Anwendung.
    - Web-Rollen, die in der Regel als Frontend für Applikationen verwendet.

**Speicher**

Datenmanagement Azure SQL-Datenbank oder Azure Storage Services wie die Tabelle bereit zum Speichern großer Mengen von unstrukturierten Daten und Blob-Dienst bietet Dienste zum Speichern von großer Datenmengen unstrukturierten Text oder Binärdaten wie Video, Audio und Bilder.

- Sicherung mehrere Daten in entsprechende SQL-Datenbank pro-Tenant-SQL Server-Benutzernamen.
- Azure-Tabellen für Anwendung Ressourcen durch Angabe einer Ebene Container-Richtlinie verwenden, können Sie Berechtigungen anpassen, ohne neue URL für die Ressourcen mit SAS geschützt.
- Azure-Warteschlangen für Anwendung Ressourcen Azure-Warteschlangen häufig zum Laufwerk Verarbeitung Mieter, aber können auch zur Arbeit für die Bereitstellung oder Verwaltung erforderlich.
- Service Bus Warteschlangen für Anwendungsressourcen legt Arbeit einen gemeinsamen Dienst, können Sie eine einzelne Warteschlange hat jeder Mieter Absender nur Berechtigungen (wie ACS ausgestellte Ansprüche) um die Warteschlange zu verschieben, während nur die Empfänger aus dem Dienst aus der Warteschlange abrufen die Daten aus mehreren Mandanten berechtigt.


**Verbindung und Sicherheitsdienste**

- Azure Service Bus eine Infrastruktur, die zwischen Applikationen damit zum Austausch von Nachrichten in einer lose verknüpften Weise verbesserte Skalierung und Stabilität.

**Netzwerkdienste**

Azure bietet mehrere Netzwerkdienste, die Authentifizierung unterstützen und die Verwaltung der gehostete Anwendung verbessert. Diese Services umfassen die folgenden:

- Azure Virtual Network können Sie bereitstellen und Verwalten von virtuellen privaten Netzwerken (VPNs) in Azure sowie Verknüpfen sicher mit lokalen IT-Infrastruktur.
- Traffic Manager für virtuelle Netzwerke können Sie eingehenden Datenverkehr Saldo in mehreren gehosteten Azure Services laden, ob sie im gleichen Datencenter oder in verschiedenen Rechenzentren auf der ganzen Welt arbeiten.
- Azure Active Directory (Azure AD) ist eine moderne, REST-basierten Dienst Identität Management und Access Control-Funktionen für Ihre Cloudanwendungen. Mithilfe von Azure AD für Anwendungsressourcen bietet Azure Anzeige leicht zu authentifizieren und Autorisieren von Benutzern auf Ihre ASP.NET-Webanwendungen und-Dienste zugreifen und die Funktionen der Authentifizierung und Autorisierung aus Code berücksichtigt werden.
- Azure Service Bus bietet ein secure messaging und verteilten Daten Nachrichtenfluss für Hybrid Applications wie Kommunikation zwischen Azure gehostet Applikationen und lokalen Applikationen und ohne komplexe Infrastrukturen Firewall und Sicherheit. Als Endpunkte verfügbar gemachten Dienste Service Bus Relay für Anwendungsressourcen mit Mieter (z. B. außerhalb des Systems wie lokal gehostet) gehören möglicherweise oder sie möglicherweise Services speziell für den Mandanten (da Sie vertrauliche Daten mandantenspezifische Reisen) bereitgestellt.



**Bereitstellung von Ressourcen**

Azure bietet verschiedene Arten Bereitstellen neuer Mandanten für die Anwendung. Mandantenfähigen Applikationen mit einer großen Anzahl der Mandanten ist es normalerweise notwendig, diesen Prozess automatisieren, indem Sie aktivieren Self-service-Bereitstellung.

- Worker-Rollen ermöglichen und Deduplizierung pro Mandant Ressourcen (z. B. wenn ein neuer Mandant anmeldet oder abgebrochen), Metriken sammeln, verwenden und Verwalten von Skalierung nach einem bestimmten Zeitplan oder auf das Überschreiten der Schwellenwerte für KPIs. Diese dieselbe Rolle verwendet werden müssen, Updates und Upgrades der Lösung.
- Azure-Blobs Compute bereitgestellt oder vorinitialisiert Speicherressourcen für neue Mandanten gleichzeitig auf Container-Richtlinien zu den Compute Servicepakete, Abbildern und andere Ressourcen.
- Optionen für die Bereitstellung von Datenbankressourcen SQL ein:

    -   DDL in Skripts oder als Ressourcen in Assemblys
    -   SQL Server 2008 R2 DAC verteilten Pakete programmgesteuert.
    -   Kopieren von master Referenzdatenbank
    -   Datenbank importieren und Bereitstellen neuer Datenbanken aus einer Datei exportieren.



<!--links-->

[Ein Multi-Tenant Anwendung in Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716

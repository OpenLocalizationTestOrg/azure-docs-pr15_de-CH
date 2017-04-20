<properties
   pageTitle="Azure SQL Azure Fallstudie - Daxko-CSI Datenbank | Microsoft Azure"
   description="Lernen Sie, wie Daxko-CSI SQL-Datenbank verwendet der Entwicklungszyklus beschleunigen und der Kunden-Service und Leistung"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/08/2016"
   ms.author="carlrab"/>
   
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko-CSI verwendet Azure der Entwicklungszyklus beschleunigen und der Kunden-Service und Leistung

![Daxko-CSI-Logo](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko-CSI Software Herausforderung: Kundenbasis Eignung und Freizeit wuchs, Dank der umfassenden Enterprise-Software-Lösung, aber halten mit der IT-Infrastruktur, Kunden Basis testete des Unternehmens IT-Mitarbeiter. Das Unternehmen wurde durch steigende Operationen Aufwand, besonders für wachsenden Datenbanken verwalten zunehmend eingeschränkt. Schlimmer noch, Schnitt Operationen Aufwand in Ressourcen für neue Initiativen wie neue Mobilitätsfunktionen für die Software.

Azure bereitgestellt nach David Molina, Director of Product Development Daxko/CSI CSI Software Platform-as-a-Service (PaaS) Modell, die Datenbank zu vereinfachen, skalierbar und Ressourcen konzentrieren statt Ops Software benötigt. "Azure SQL-Datenbank wurde eine hervorragende Option für uns. Nicht war zum Verwalten von SQL Server einen Failovercluster und die Infrastruktur muss sich für uns."

Seit Migrieren in Azure ist CSI Software ein Mitarbeiter zwei 600 Kundendatenbanken verwalten. Das Unternehmen verwendet elastische Pools Azure SQL-Datenbank Kundendatenbanken basierend auf Größe verschieben und muss.

Molina weiterhin "Kunden die Änderung sofort spürbar. Elastische Pools mussten sie gelegentlich Timeouts und andere Probleme Burst Zeiten. Azure elastische Pools können als benötigt und die Software ohne Probleme verwenden."

Verbessert die Leistung für Kunden, Datenbank Azure elastischen Pools CSI Software Ressourcen neue Dienste und Features, anstatt mit Operationen freigegeben. Die IT-Ressourcen geholfen CSI-Software die Enterprise-Software bietet SpectrumNG, um zu engagieren Fitnesscenter Mitglieder, Verbesserung der Effizienz, verbessern und Personal und mobilen Zugriff gewähren für interaktive Aufgaben und Echtzeit-Benachrichtigungen.

Azure hat auch CSI Software beschleunigen und optimieren die Entwicklung und Qualitätssicherungstests (QA) Zyklus durch Automatisierungsoptionen aktivieren. Das Unternehmen Azure Implementierung können Build-Manager Komponenten auf eine Schaltfläche packen. Molina beschreibt "als Teil des Releasezyklus kann QA jetzt Bereitstellung einer Umgebung in Azure eng unsere Produktion Stapel nachahmt. Wir können unsere Entwicklungsumgebung ändert Tierarzt Builds sofort bereitstellen. Das ist ein großer Gewinn für uns, weil wir Parität für Tests vor, haben."

## <a name="offloading-to-the-cloud"></a>Verschiebung in der cloud

Zuerst in die Cloud errichtete CSI Software erfolgreich eigene mandantenfähigen Infrastruktur in einem lokalen Rechenzentrum in Houston. Erweitert das Unternehmen konfrontiert zunehmende Anfangsschwierigkeiten Kauf und Bereitstellung aller Hard- und Software für ihre Kunden verwalten. IT Personal Operationen zu anderen Engpass zu einer Verlangsamung der Bereitstellung neuer Ressourcen und Rollout neuer Services für Kunden geführt wurde.

CSI Software haben Optionen zur Beseitigung dieser Aufwand Cloud, damit es seine anstatt Vorgänge konzentrieren kann. Das Unternehmen festgestellt, dass viele der oberen Cloud-Anbieter nur Infrastruktur als Dienst (IaaS), die weiterhin große IT-Personal Lösungen zu IaaS-Stapel erforderlich. Schließlich festgestellt CSI-Software, dass Azure PaaS-Lösung die beste Lösung für ihre Bedürfnisse. Erklärt Molina "Azure aus dem Weg, die Hardware und Software wird damit wir auf unsere Software-Angebote gleichzeitig unsere IT-Aufwand konzentrieren können."

## <a name="making-the-transition-to-azure"></a>Übergang in Azure

Nach der Auswahl Azure die PaaS-Lösung, begannen CSI Software Back-End-Infrastruktur und Datenbanken in der Cloud. SpectrumNG Kunden mussten vor Azure installieren eine Clientanwendung kommuniziert mit einem Dienst Windows Communication Foundation (WCF) auf dem Back-End. Nach Molina haben"zwar einige Kunden alles in eigenen Rechenzentren gehostet wir das Produkt mandantenfähigen sein. Wir haben alles in einem Rechenzentrum in Houston, mit SQL Server als Datenspeicher.

"Unser Produkt auch enthalten einen Member mit Web-Portal (geschrieben in ASP.net) weiß der Kunde Webpräsenz und SOAP-API zur Unterstützung der Onlineseiten und Integration von Dritten gekennzeichnet werden soll."

Migration zur Cloud dauerte nicht lange für die Architektur. Nach Molina "Der Großteil der Aufwand behandelt, wie wir Informationen zu Dateien, eine Änderung zentrale Verbindungszeichenfolge und Automatisierung der Verpackung hochladen und Bereitstellung unserer Versionen lesen ändern."

Entwickeln Sie die Buildautomatisierung zum CSI Softwareentwickler Azure PowerShell und REST-APIs Pakete erstellen und Hochladen in einer Stagingumgebung Version jede Nacht.
Allgemeine Übergang mit einer Azure Cloud-basierte Bereitstellung ging schnell und reibungslos CSI Software IT-Teams. Molina erklärt, ", wir eine Beta-Umgebung in der Cloud innerhalb von drei bis vier Wochen auf das Projekt. Das war überraschend Gewinn für uns."

Konfigurieren und Testen der Umgebung CSI Software begannen Kunden. Für Kunden mit bereits CSI Software hosten wurde der Übergang nahezu nahtlose. Für Kunden von einer Ausfallzeit Migration zur Cloud einige zusätzliche gedauert, aber noch in erster Linie für Kunden und CSI Software einfach.

Für neuer Kunden CSI-Software die IT-Mitarbeiter mithilfe des folgenden Verfahrens zum integrierten sie in Azure:

1.  Azure PowerShell-Skripts wird eine neue Datenbank für den Kunden dreht. alle Kunden beginnen auf einer Ebene Premium genügend anfänglichen Durchsatz für den Übergang sicherzustellen.
2.  Wenn möglich, verwendet CSI Software Azure SQL-Migrationsassistenten, einer Azure SQL-Datenbankinstanz vorhandene Daten verschieben.
3.  Schließlich werden Microsoft SQL Server Integration Services (SSIS) zu Diskrepanzen in den Daten oder alle Daten als erforderliche Aufräumarbeiten verwendet.

99 Prozent CSI Kunden werden heute über vier regionale Rechenzentren (North Central South Central, Ost und West) in Azure gehostet. Latenz ist mit Rechenzentren in geografischen Region des Kunden auf ein Minimum beschränkt.

## <a name="azure-elastic-database-pools-free-up-it-resources"></a>Azure elastischen datenbankpools IT-Ressourcen

Einige Features von Azure haben CSI Software UMSCHALT-Infrastruktur und Betrieb konzentriert, Funktion und Entwicklung konzentriert. Vielleicht wurde der größte Vorteil aus elastischen Datenbank.

CSI-Software bietet derzeit ca. 550 Datenbanken für Kunden. Bevor elastische Pools war es schwierig, viele Datenbanken innerhalb einer Ebene verwalten. OPS Manager musste Performance-Stufen Burst Bedarf des Kunden erheblichen IT-Ressourcenaufwand erforderlich zuweisen. Elastische Datenbank Pools können Manager Mieter Premium oder standard-Pool gegebenenfalls zuordnen Kunden basierend auf Größe und benötigen. Kunden spüren Pools elastische Datenbank sofort; vor elastische Pools Kunden hatten Timeouts und anderen Problemen bei Burst-Perioden, aber elastische Pools können auftreten Aktivität Bursts Bedarf und können sie weiterhin SpectrumNG ohne Probleme verwenden.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure aktive Geo-Replikation beschleunigt reporting

Mehrere CSI Software-Kunden werden auch von Azure aktive Geo-Replikation nutzen. Mit aktiven Geo-Replikation kann bis zu vier lesbare sekundäre Datenbanken in demselben oder einem anderen Rechenzentrum Regionen konfiguriert werden. CSI-Software können aktive Geo-Replikation auf zwei Arten verwenden: zuerst werden sekundären Datenbanken bei Datacenter-Ausfall oder eine Verbindung mit der primären Datenbank nicht möglich und zweitens sekundären Datenbanken werden können schreibgeschützte Arbeitslasten wie Aufträge abnimmt. Einige CSI Software nutzen diesen Vorteil reporting Workflows beschleunigen.

## <a name="csi-software-application-logic-and-architecture"></a>CSI Software Anwendungslogik und Architektur

SpectrumNG verwendet Webrollen. Ist die Anwendung Multi-Tenant wird ein WCF-Dienst zum Verbindungsaufbau Anfrage von Kunden. Wie Molina identifiziert"die Anforderung jedem Kunden ermöglicht uns das Erstellen einer Verbindungszeichenfolge, um zu tun was wir tun müssen."

Für die Webebene Dienst nutzt CSI Software Azure automatische Skalierung auf Tag und die Uhrzeit. Verfügbare Ressourcen sind automatisch angepasst höhere Auslastung während der üblichen Geschäftszeiten nach Zeitzone jede regionale Datenzentrum erhöht. Ressourcen sollen auch an Wochenenden, verkleinern, beim Kunden liegen.

     
![Daxko-CSI-Architektur](./media/sql-database-implementation-daxko/figure1.png)

Abbildung 1. Eine Cloud Services Worker-Rolle zeichnet strukturierte Daten aus Azure SQL-Datenbank und semistrukturierten Daten Tabellenspeicher. SpectrumNG Benutzer interagieren mit Daten über eine Webrolle Dienste.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Mit Web-apps und Web-Plan Tier für mobile apps

Neue Initiativen eine vollständige Plattform aktivieren mithilfe von Azure SQL-Datenbank freigegeben Ressourcen CSI Software basierend auf einer benutzerdefinierten API in Azure webapps gehostet. Die Plattform ermöglicht Fitnesscenter Mitglieder und Mitarbeiter mit mobilen Geräten Zeitpläne überprüfen und buchen Klassen empfangen.

Die Plattform verwendet die Service-orientierte Architektur (SOA) zu einer Komponente – wie ein POS-System (POS) oder einem Vertriebssystem – dynamisch in einen anderen Web-Plan verschieben und drehen Sie Service Komponente, wobei alles auf das ursprüngliche Web unterstützt. Diese ermöglicht Flexibilität CSI Software und hilft Kosten.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure ermöglicht CSI Software Entwickler konzentrieren apps und services

Azure SQL-Datenbank ist nicht nur ein Segen SpectrumNG Kunden, den schnellen und zuverlässigen Service, sondern auch einen großen Gewinn für CSI Software IT-Mitarbeiter und Entwickler. Ops Azure in der Cloud-Abladung CSI Software verringert die Overhead und Ressourcen, erheblich beschleunigt die Entwicklungszyklen und mehr muss mikromanagement Datenbanken Mieter Leistung optimieren.

## <a name="more-information"></a>Weitere Informationen

- Erfahren Sie mehr über Azure elastischen datenbankpools anzeigen Sie [elastische datenbankpools](sql-database-elastic-pool.md)

- Datenbanktools und flexible Skalierung finden Sie unter [elastische Datenbanktools und flexible Skalierung](sql-database-elastic-scale-get-started.md).

- Weitere Informationen zum Migrieren einer SQL Server-Datenbank finden Sie unter [Azure SQL-Migrationsassistenten](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md).

- Erfahren Sie mehr über aktive Geo-Replikation finden Sie unter [Aktive Geo-Replikation](sql-database-geo-replication-overview.md).

- Erfahren Sie mehr über Web-Rollen und Arbeitskraft anzeigen Sie [Worker-Rollen](../fundamentals-introduction-to-azure.md#compute) 

- Weitere Informationen zu Azure Service Bus finden Sie unter [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- Erfahren Sie mehr über automatische Skalierung Siehe [Skalieren von Cloud-Diensten](../cloud-services/cloud-services-how-to-scale.md).

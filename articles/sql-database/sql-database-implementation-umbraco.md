<properties
   pageTitle="Azure SQL Azure Fallstudie - Umbraco Datenbank | Microsoft Azure"
   description="Erfahren Sie mehr über wie Umbraco SQL-Datenbank verwendet, um schnell bereitzustellen und Skalierung Services für Tausende von Mandanten in der cloud"
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
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco verwendet Azure SQL-Datenbank zu schnell bereitstellen und Skalierung für Tausende von Mandanten in der cloud

![Umbraco Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco ist ein beliebtes Open Source Content Management System (CMS), das nichts von kleinen Kampagne oder Broschüre Sites komplexe Applikationen für Fortune 500-Unternehmen und globale Websites ausführen kann. 

> "Wir haben ganz eine große Community von Entwicklern, die das System mit mehr als 100.000 Entwickler in unseren Foren und mehr als 350.000 Websites live Umbraco ausgeführt."

> Morten Christensen, Technische Leiter Umbraco

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Zur Vereinfachung der Kunden Umbraco Umbraco-as-a-Service (UaaS) hinzugefügt: ein Software-as-a-Service (SaaS) anbieten, die lokale Bereitstellung überflüssig bietet integrierte Skalierung und Management overhead Entwickler Produktmanagement Innovation als Lösung konzentrieren entfernt. Umbraco kann diese Vorteile bieten auf flexible Plattform als Service (PaaS) Modells von Microsoft Azure bereitgestellt.

UaaS ermöglicht SaaS Umbraco CMS-Funktionen verwenden, die bereits außerhalb ihrer Reichweite. Diese Kunden sind eine CMS Umgebung bereitgestellt, die eine Produktionsdatenbank enthält. Kunden können bis zu zwei zusätzliche Datenbanken für Entwicklung und Stagingumgebungen je nach Bedarf hinzufügen. Bei der Anforderung einer neuen Umgebung weist automatisiert Kunden eine Datenbank automatisch. Die Datenbank ist in Sekunden, da die Datenbank bereits Umbraco aus einer Azure elastische Datenbanken bereits bereitgestellt wurde (siehe Abbildung 1).


![Umbraco Bereitstellung Lebenszyklus](./media/sql-database-implementation-umbraco/figure1.png)

Abbildung 1. Bereitstellung Lebenszyklus Umbraco als Dienst (UaaS)
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure elastische Pools und Automatisierung vereinfacht Bereitstellung

Mit Azure SQL-Datenbank und anderen Azure Umbraco Kunden können selbst ihre Umgebung bereitstellen und Umbraco leicht überwachen und Verwalten von Datenbanken im Rahmen einer intuitiven Workflows:

1.  Bereitstellung

    Umbraco verwaltet eine Kapazität von 200 verfügbaren vorab bereitgestellte Datenbanken aus elastischen. Wenn ein neuer Kunde für UaaS, bietet Umbraco durch Zuweisen einer Datenbank aus der Verfügbarkeit der Kunde mit neuen CMS-Umgebung in Echtzeit.

    Wenn ein Pool Verfügbarkeit den Schwellenwert erreicht, ein neue elastischer Pool erstellt und neue Datenbanken sind bereits bereitgestellte Kunden zugewiesen werden.

    Implementierung mit C# Management Bibliotheken und Azure Service Bus-Warteschlangen vollständig automatisiert.

2.  Nutzen

    Kunden verwenden ein bis drei Umgebungen (für Produktion, Staging und Entwicklung), mit einer eigenen Datenbank. Kundendatenbanken sind elastische datenbankpools ermöglicht Umbraco effiziente Skalierung ohne bereitstellen.

    ![Umbraco Projektübersicht](./media/sql-database-implementation-umbraco/figure2.png)

    ![Projektdetails Umbraco](./media/sql-database-implementation-umbraco/figure3.png)

    Abbildung 2. Umbraco-as-a-Service (UaaS) Kundenwebsite Projektübersicht und Details anzeigen

    Azure SQL-Datenbank verwendet Datenbank Buchungseinheiten (DTUs) zur Darstellung des relativen Leistungsbedarf für reale Datenbanktransaktionen. Für UaaS Kunden Datenbanken arbeiten normalerweise 10 DTUs jeweils die Elastizität bei Bedarf zu skalieren. Das bedeutet, dass UaaS sicherstellen können, dass Kunden immer erforderlichen Ressourcen auch in Spitzenzeiten. Beispielsweise verbindet eine UaaS Erfahrung Kundendatenbank bis 100 DTUs im letzten Sonntag Sport-Ereignis für die Dauer des Spiels. Azure elastische Pools ermöglicht Umbraco, Nachfrage ohne Performance-Einbußen unterstützt.

3.  Monitor

    Umbraco Monitore mit Dashboards innerhalb von Azure-Portal mit benutzerdefinierten e-Mail-Datenbank.

4.  Disaster recovery

    Azure bietet zwei Optionen für Disaster Recovery (DR): aktive Geo-Replikation und Geo-Wiederherstellung. DR-Option, die ein Unternehmen sollten hängt von [Business-Continuity-Zielsetzungen](sql-database-business-continuity.md).

    Aktive Geo-Replikation bietet die schnellste Antwort bei einem Ausfall. Aktive Geo-Replikation verwenden, können bis zu vier lesbare sekundäre auf Servern in unterschiedlichen Regionen und Sie können dann Failover zu einem sekundären Instanzen im Fehlerfall initiieren.

    Umbraco Geo-Replikation erfordert nicht, aber es nutzen von Azure Geo-Wiederherstellung Ausfallzeiten bei einem Ausfall sicherstellen. Geo-Wiederherstellung hängt Datenbanksicherungskopien Geo redundante Azure-Speicher. Dadurch wird ein Ausfall der primären Region aus einer Sicherungskopie wiederherstellen.

5.  De-Bereitstellung

    Wenn projektumgebung gelöscht wird, werden alle zugehörigen Datenbanken (Entwicklung, Bereitstellung oder live) während Warteschlangencleanup Azure Service Bus entfernt. Diesem automatisierte Prozess setzt die nicht verwendeten Datenbanken an der Umbraco elastische Datenbank-Verfügbarkeit für künftige Bereitstellung während der maximalen Auslastung verfügbar zu machen.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Elastische Pools können UaaS problemlos skalieren

Unter Ausnutzung der Azure elastische datenbankpools optimieren Umbraco für seine Kunden ohne über oder unter bereitstellen. Umbraco hat derzeit fast 3.000 Datenbanken 19 elastische datenbankpools mit einfach skalieren wie erforderlich ihre 325.000 Bestandskunden oder Neukunden ein CMS in der Cloud bereitstellen.

Tatsächlich ist "nach Morten Christensen, Technical Lead bei Umbraco, UaaS jetzt über 30 neue Kunden pro Tag Wachstum. Unsere Kunden sind sehr zufrieden mit der Möglichkeit, neue Projekte in Sekunden Bereitstellung sofort veröffentlichen von aktualisierten Websites live aus einer Umgebung mit "One-Click-Bereitstellung und Änderungen schnell, wenn sie Fehler finden."

Wenn ein Kunde eine zweite oder dritte Umgebung nicht mehr benötigen, können sie die Umgebung einfach entfernen. Die frei Ressourcen als Teil des Umbraco elastische Datenbank-Verfügbarkeit für andere Kunden verwendet werden können.

![Umbraco Bereitstellungsarchitektur](./media/sql-database-implementation-umbraco/figure4.png)

Abbildung 3. UaaS Bereitstellungsarchitektur auf Microsoft Azure

##<a name="the-path-from-datacenter-to-cloud"></a>Der Pfad vom Rechenzentrum in die cloud

Wenn Entwickler Umbraco zunächst zu einem SaaS-Modell entschieden, Wussten sie kostengünstigen und skalierbaren können den Service erstellen müssten.

> "Elastische datenbankpools sind perfekt für unsere SaaS anbieten da wir Kapazität nach oben und unten Bedarf wählen können. Bereitstellung ist einfach und Einrichtung, halten wir Nutzung maximal."

> Morten Christensen, Technische Leiter Umbraco

"Wir wollten unsere Zeit unserer Kunden Probleme zu lösen nicht Infrastruktur verwalten. Wir sagt erleichtern unseren Kunden den nutzen, "Niels Hartvig, Gründer von Umbraco. "Wir als Server selbst hosten, aber Planung wäre ein Albtraum." Zufällig beschäftigt Umbraco keine Administratoren unterstreicht die Hauptargumente für die Verwendung von UaaS.

Ein wichtiges Ziel für Entwickler Umbraco war UaaS Kunden schnell und ohne Kapazität Umgebung bereitstellen. Aber einen dedizierten gehosteten Dienst in Umbraco Rechenzentren würde viele überschüssige Kapazitäten Bursts Verarbeitung erforderlich ist. Das hieße regelmäßig ausgelasteten würde beträchtlichen Compute-Infrastruktur hinzufügen.

Darüber hinaus sollte das Entwicklungsteam Umbraco Lösung, sie so viel wie möglich ihren vorhandenen Code wiederverwenden. Als Entwickler Umbraco gibt Mikkel Madsen "waren wir zufrieden mit den Entwicklungstools von Microsoft, die wir bereits kennen, wie Microsoft SQL Server, Microsoft Azure SQL-Datenbank, ASP.net und Internet Information Services (IIS). Investitionen in eine IaaS oder eine PaaS cloud-Lösung, wollten wir sicherstellen, dass Unterstützung unserer Microsoft Tools und Plattformen, sodass wir nicht massiv Code machen. "

Um die Kriterien erfüllen, sah Umbraco für einen cloudpartner mit den folgenden Merkmalen:

-   Ausreichend Kapazität und Zuverlässigkeit
-   Unterstützung für Microsoft-Entwicklungstools, sodass Umbraco Ingenieure nicht gezwungen wäre, ihre Entwicklungsumgebung vollständig neu
-   Präsenz aller geografischen Märkten UaaS (Unternehmen müssen sicherstellen Wettbewerb, dass sie schnell auf ihre Daten zugreifen können und ihre Daten an einem Speicherort gespeichert ist, die ihre regionalen Vorschriften erfüllt)

##<a name="why-umbraco-chose-azure-for-uaas"></a>Warum Umbraco Azure für UaaS

Laut Morten Christensen "nach Prüfung unseren Optionen, wir haben Azure da alle Kriterien von Verwaltung und Skalierbarkeit Vertrautheit und Kosteneffizienz erreicht. Wir die Umgebung auf Azure VMs, und jede Umgebung hat eine eigene Instanz Azure SQL-Datenbank mit allen Instanzen elastische datenbankpools. Durch die Trennung von Datenbanken zwischen Entwicklungs-, Staging- und live-Umgebung bieten wir unseren Kunden stabile Leistung Isolation Skalierung zugeordnet – ein großer Gewinn. "

Morten weiterhin "Bevor wir Server Webdatenbanken manuell bereitstellen hatten. Nun müssen wir denken. Alles ist automatisiert – von der Bereitstellung zu bereinigen. "

Morten freut sich auch mit der Skalierung von Azure bereitgestellt. "Elastische datenbankpools sind perfekt für unsere SaaS anbieten da wir Kapazität nach oben und unten Bedarf wählen können. Bereitstellung ist einfach und Einrichtung, halten wir Nutzung maximal." Morten Staaten "Einfachheit elastische Pools sowie die Sicherstellung der Service-Tier-basierte DTUs kann wir neue Resource Pools bei Bedarf bereitgestellt. Vor kurzem erreichte eines unserer größeren Kunden zu 100 DTUs Umgebung live. Mit Azure bereitgestellt elastische Pools Kundendatenbanken Ressourcen in Echtzeit Bedarf ohne DTU Vorschriften vorherzusagen. Einfach gesagt, unsere Kunden erhalten die Bearbeitungszeit, die sie erwarten und wir unsere Performance-SLAs erfüllen."

Mikkel Madsen fasst: "Wir nutzen leistungsstarken Azure Algorithmus, der SaaS häufig (Onboarding neue Kunden Ebene) verbindet unsere Anwendung Muster (Datenbanken, sowohl die Vorabbereitstellung und live) auf die zugrunde liegende Technologie (mit Azure Servicebus-Warteschlangen zusammen mit Azure SQL-Datenbank)."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Mit Azure überschreitet UaaS Kunden erwarten

Auswahl Azure als cloudpartner seit Umbraco können UaaS mit optimierter Content Management, ohne die IT-Ressourcen Investitionen selbst gehostete Lösung Kunden. So Morten "gerne Developer Komfort und Erweiterbarkeit Azure uns ermöglicht und unsere Kunden sind mit Funktionen und Zuverlässigkeit. Insgesamt ist ein großer Gewinn für uns schon!"
 
## <a name="more-information"></a>Weitere Informationen

- Erfahren Sie mehr über Azure elastischen datenbankpools anzeigen Sie [elastische datenbankpools](sql-database-elastic-pool.md)

- Weitere Informationen zu Azure Service Bus finden Sie unter [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- Erfahren Sie mehr über Web-Rollen und Arbeitskraft anzeigen Sie [Worker-Rollen](../fundamentals-introduction-to-azure.md#compute) 

- Weitere Informationen über virtuelle Netzwerke finden Sie unter [virtuelle Netzwerke](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Weitere Informationen zu Sicherung und Wiederherstellung finden Sie unter [Business Continuity](sql-database-business-continuity.md).  

- Um weitere Informationen zum Überwachen von Ppols finden Sie unter [monitoring Pools](sql-database-elastic-pool-manage-portal.md). 

- Erfahren Sie mehr über Umbraco als Dienst anzeigen Sie [Umbraco](https://umbraco.com/cloud)


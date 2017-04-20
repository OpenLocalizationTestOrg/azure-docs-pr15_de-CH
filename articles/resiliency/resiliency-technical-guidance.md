<properties
   pageTitle="Resiliency technische Anleitung Index | Microsoft Azure"
   description="Index der technischen Artikel zu verstehen und Entwerfen leistungsstarke, hochverfügbare, fehlertolerante Applikationen sowie für Disaster Recovery und Business Continuity Planung"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance"></a>Technische Anleitung Azure Stabilität

##<a name="introduction"></a>Einführung

Hohe Verfügbarkeit und Disaster Recovery erfüllen benötigt zwei Arten wissen:

- Detaillierte technische Kenntnisse eine Cloud-Funktionen
- Einen verteilten Dienst ordnungsgemäß Architekt wissen

Dieser Artikelreihe behandelt die ehemaligen: Stärken und Schwächen der Azure-Plattform im Hinblick auf Stabilität (Business Continuity bezeichnet). Wenn Sie diese interessiert sind, finden Sie unter Artikelserie konzentriert, [Disaster Recovery und hohe Verfügbarkeit für Azure Applications](https://aka.ms/drtechguide). Obwohl dieser Artikelserie Architektur und Design Patterns betrifft, ist nicht der Schwerpunkt der Serie. Design-Anleitung finden Sie in Abschnitt [zusätzliche Ressourcen](#additional-resources) .

Die Informationen sind in den folgenden Artikeln:

- [Recovery bei lokalen](resiliency-technical-guidance-recovery-local-failures.md).
Physikalische Hardware (z. B. Laufwerke, Server und Netzwerkgeräte) kann fehlschlagen. Laden, Spitzen können Ressourcen ausgeschöpft werden. Dieser Artikel beschreibt die Funktionen, mit denen Azure hoher Verfügbarkeit unter diesen Umständen.

- [Wiederherstellung einer Azure flächendeckende Service-Unterbrechung](resiliency-technical-guidance-recovery-loss-azure-region.md).
Weit verbreitet sind selten, aber theoretisch möglich. Gesamte Regionen können aufgrund von Netzwerkfehlern isoliert oder sie können beschädigt Naturkatastrophen. Erläutert, wie Azure Applications erstellen geographisch Regionen verwenden.

- [Recovery von lokalen in Azure](resiliency-technical-guidance-recovery-on-premises-azure.md).
Die Cloud ändert erheblich die Wirtschaftlichkeit der Wiederherstellung Unternehmen Azure verwenden, um einen zweiten Standort für die Wiederherstellung. Dies ist zu einem Bruchteil der Kosten und sekundären Datencenter möglich. Dieser Artikel beschreibt die Azure für die Erweiterung von einem lokalen Rechenzentrum in die Cloud bietet Funktionen.

- [Wiederherstellung nach Datenkorruption oder versehentliches Löschen](resiliency-technical-guidance-recovery-data-corruption.md).
Clientanwendungen können Fehler, beschädigten Daten. Operatoren können falsch Daten löschen. Dieser Artikel beschreibt, was Azure bereit Daten sichern und Wiederherstellen auf eine frühere Zeit.

##<a name="additional-resources"></a>Zusätzliche Ressourcen

- [Disaster Recovery und hohe Verfügbarkeit bei Microsoft Azure](resiliency-disaster-recovery-high-availability-azure-applications.md).
Dieser Artikel ist eine ausführliche Übersicht über die Verfügbarkeit und Disaster Recovery. Er behandelt die Herausforderung, manuelle Replikation Referenz und Transaktionsdaten. Letzten Abschnitten Zusammenfassung verschiedener Disaster Recovery-Topologien, die Azure Bereiche für höchste Verfügbarkeit umfassen.

- [Hohe Verfügbarkeit: Checkliste](resiliency-high-availability-checklist.md).
Dieser Artikel ist eine Liste der Features, Dienste und Designs können zum Resiliency und Verfügbarkeit der Anwendung erhöhen.

- [Microsoft Azure Service Resiliency Anleitung](resiliency-service-guidance-index.md).
Dieser Artikel ist ein Index der Azure Services und Links zu Disaster Recovery Leitfäden und Designrichtlinien.

- [Übersicht: Cloud Business Continuity und Datenbank-Wiederherstellung mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md).
Dieser Artikel bietet Azure SQL-Datenbank-Techniken für Verfügbarkeit. Hauptsächlich zentriert für Sicherung und Wiederherstellung Strategien. Wenn Sie Azure SQL-Datenbank in Ihrem Cloud-Dienst verwenden, sollten Sie dieses Dokument und seine verknüpften Ressourcen überprüfen.

- [Hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
Dieser Artikel beschreibt Verfügbarkeitsoptionen Verwendung Infrastruktur als Service (IaaS) zum Hosten Ihrer Datenbankdienste durchsuchen können. Es beschreibt AlwaysOn Availability Gruppen, später Protokollversand und Backup und Wiederherstellung. Verschiedene Lernprogramme zeigen, wie diese Techniken.

- [Best Practices für umfangreiche Dienstleistungen Azure Cloud Services entwerfen](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
Dieser Artikel konzentriert sich auf die Entwicklung hochgradig skalierbare Cloud-Architekturen. Viele der Techniken, mit denen Sie zu Skalierbarkeit verbessern Verfügbarkeit. Auch wenn Ihre Anwendung erhöhte Belastung skalieren nicht möglich, ist skalierbar Verfügbarkeit.

- [Backup und Wiederherstellung für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Dieser Artikel enthält technische Anleitung zum Sichern und Wiederherstellen von Microsoft SQL Server auf Azure virtuellen Computer ausgeführt.

- [Failsafe: Leitfaden für robustes Cloudarchitekturen](https://channel9.msdn.com/Series/FailSafe).
Dieser Artikel enthält Hinweise zur erstellen sowie Cloudarchitekturen Leitlinien zur Umsetzung dieser Architekturen Microsoft Technologies und Rezepte für die Umsetzung dieser Architekturen für bestimmte Szenarien.

- [Technische Fallstudie: mit Technologie zur Verbesserung der Disaster Recovery](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
Diese Fallstudie veranschaulicht, wie Microsoft IT Azure zur Verbesserung der Disaster Recovery.

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe technische Leitlinien für Azure Stabilität. Wenn Sie andere Artikel dieser Reihe interessiert sind, können Sie [bei lokalen](resiliency-technical-guidance-recovery-local-failures.md)Wiederherstellung starten.

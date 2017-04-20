<properties
    pageTitle="Neuigkeiten in SQL Datenbank V12 | Microsoft Azure"
    description="Business-Systeme, Azure SQL-Datenbank in der Cloud profitieren werden durch eine Aktualisierung auf Version V12 jetzt beschreibt."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>Neuigkeiten in SQL Datenbank V12


Dieses Thema beschreibt die zahlreichen Vorteile Version V11 neue V12 Version von Azure SQL-Datenbank hat.


Wir weiterhin V12 Funktionen hinzu. Daher empfehlen wir Ihnen die Webseite Service-Updates für Azure besuchen und Filter verwenden


- In der [SQL-Datenbankdienst](https://azure.microsoft.com/updates/?service=sql-database)gefiltert.
- Allgemeine Verfügbarkeit [(GA) Ankündigungen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) für SQL-Datenbankfunktionen gefiltert.


Die neueste Informationen über Ressourcengrenzen für SQL-Datenbank wird dokumentiert unter:<br/>[SQL Azure-Datenbank Ressourcengrenzen](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>Höhere Kompatibilität mit SQL Server


Ein Hauptziel für SQL Datenbank V12 wurde zur Verbesserung der Kompatibilität mit Microsoft SQL Server 2014 und Kompatibilität als neuer Versionen von SQL Server. Unter anderem erreicht V12 mit SQL Server im Bereich Programmierung wichtig. Zum Beispiel:

- [JSON-Unterstützung](https://msdn.microsoft.com/library/dn921897.aspx)

- [Funktionen](http://msdn.microsoft.com/library/ms189798.aspx)mit [über](http://msdn.microsoft.com/library/ms189461.aspx)

- [XML-Indizes](http://msdn.microsoft.com/library/bb934097.aspx) und [selektive XML-Indizes](http://msdn.microsoft.com/library/jj670104.aspx)

- [Versionsvergleich](http://msdn.microsoft.com/library/bb933875.aspx)

- [WÄHLEN SIE... IN](http://msdn.microsoft.com/library/ms188029.aspx)

- [Volltextsuche](http://msdn.microsoft.com/library/ms142571.aspx)

- [ALTER ausgelegte DATENBANKKONFIGURATION (Transact-SQL)](http://msdn.microsoft.com/library/mt629158.aspx)

Finden Sie [hier](sql-database-transact-sql-information.md) für eine kleine Gruppe von Features in SQL-Datenbank noch nicht unterstützt.


### <a name="compatibility-level-130"></a>Kompatibilitätsgrad 130


> [AZURE.IMPORTANT] Ab **Juni 2016**haben *neu* erstellten Datenbanken auf Azure SQL-Datenbank V12 ihre Kompatibilität bei 130, starten Sie Microsoft SQL Server 2016 GA entspricht
> 
> Sie können `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` Sie bevorzugen.
> 
> Vor Juni 2016 erstellte Datenbanken müssen nicht deren Kompatibilitätsgrad von dieser Änderung Standard geändert. Auch ist die Datenbank durch Aktualisieren von V11 auf V12 geändert.



Eine Erläuterung der Vergleich der wichtigsten Abfragen zwischen den neuesten gegenüber früheren Kompatibilität finden Sie unter:

- [Verbesserte Abfrageperformance mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>Weitere Premium-Performance und neue Leistungsmerkmale


V12 haben wir alle Premium-Performance-Level zugeordnet, um 25 % ohne Aufpreis Datenbank-Buchungseinheiten (DTUs). Noch größere Leistungsvorteile erreicht durch neue Features:


- Unterstützung für im Speicher [Columnstore Indizes](http://msdn.microsoft.com/library/gg492153.aspx).
- [Tabellenpartitionierung nach Zeilen](http://msdn.microsoft.com/library/ms187802.aspx) mit verwandten Tabelle [ABSCHNEIDEN](http://msdn.microsoft.com/library/ms177570.aspx).
- Die Verfügbarkeit der dynamische Ansichten [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) überwachen und Optimieren der Leistung.


### <a name="reliable-performance"></a>Zuverlässige Leistung


Wenn Ihr Clientprogramm SQL Datenbank V12 verbindet, während der Client auf eine Azure Virtual Machine (VM) ausgeführt wird, müssen Sie folgende Portbereiche auf dem virtuellen Computer öffnen:

- 11000 11999
- 14000 14999


Klicken Sie [hier](sql-database-develop-direct-route-ports-adonet-v12.md) für Informationen über die Ports für die SQL-Datenbank V12. Die Ports werden von Leistungssteigerungen in SQL Datenbank V12 benötigt.


## <a name="better-support-for-cloud-saas-vendors"></a>Bessere Unterstützung für Cloud SaaS-Anbieter


Nur veröffentlichte V12 wir neue Standard Leistungsstufe S3 und die public Preview [elastische Datenbank](sql-database-elastic-pool.md)Pools. Elastische datenbankpools ist eine Lösung für Cloud SaaS Kreditoren.  Elastische Datenbank Pools können Sie:


- DTUs Datenbanken zur Kostensenkung für viele Datenbanken freigeben
- Führen Sie [Aufträge elastische Datenbank](sql-database-elastic-jobs-overview.md) Datenbanken Ebene verwalten.


## <a name="security-enhancements"></a>Sicherheitsfunktionen


Sicherheit ist ein Hauptanliegen für alle, die ihr Unternehmen in der Cloud ausgeführt wird. Die neuesten V12 veröffentlicht Sicherheitsfunktionen:


- [Zeilenbasierter Sicherheit](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [Dynamic Data-Maskierung](sql-database-dynamic-data-masking-get-started.md)
- [Eigenständige Datenbanken](http://msdn.microsoft.com/library/ff929188.aspx)
- [Anwendungsrollen](http://msdn.microsoft.com/library/ms190998.aspx) verwaltet erteilen, verweigern, widerrufen
- [Transparente Verschlüsselung](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (DSA)
- [Herstellen einer Verbindung mit Datenbank SQL Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md)
 - SQL-Datenbank unterstützt jetzt Azure Active Directory-Authentifizierung, einen Mechanismus mit SQL-Datenbank mithilfe von Identitäten in Azure Active Directory (Azure AD). Mit Azure Active Directory-Authentifizierung können Sie zentral die Identitäten der Benutzer und anderen Microsoft-Diensten an einem zentralen Ort verwalten.
- [Verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx) (in der Vorschau) ist Verschlüsselung Anwendung transparent und ermöglicht Clients beim Entschlüsseln sensibler Daten in Clientanwendungen ohne die Verschlüsselungsschlüssel mit SQL-Datenbank.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Business Continuity erhöht, wenn eine Wiederherstellung erforderlich ist


V12 bietet verbesserte Recovery Point Objectives (RPO) und geschätzten Recovery (ERTs):


| Business Continuity-Funktion | Frühere version | V12 |
| :-- | :-- | :-- |
| Geo-Wiederherstellung | • RPO < 24 Stunden.<br/>• ERT < 12 Stunden. | • RPO < 1 Stunde.<br/>• ERT < 12 Stunden. |
| Aktive Geo-Replikation | • RPO < 5 Minuten.<br/>• ERT < 1 Stunde. | • RPO < 5 Sekunden.<br/>• ERT < 30 Sekunden. |


Weitere Informationen finden Sie in der [SQL-Datenbank Business Continuity](sql-database-business-continuity.md) .


## <a name="more-reasons-to-upgrade-now"></a>Weitere Gründe für ein upgrade


Es gibt viele Gründe, warum Kunden nun auf Azure SQL-Datenbank V12 aus V11 aktualisieren sollten:


- SQL Datenbank V12 hat eine lange Liste von Features Features V11.
- Wir weiterhin V12 neue Features hinzugefügt, aber V11 keine neuen Features hinzugefügt.
- Die meisten neuen Features auf SQL Datenbank V12 veröffentlicht, bevor sie für Microsoft SQL Server freigegeben werden.


## <a name="are-you-using-v12-already"></a>Verwenden V12 bereits Sie?


Eine einfache Möglichkeit, festzustellen, ob eine Datenbank oder einen logischen Server auf eine frühere Version der SQL-Datenbankdienst ist Folgendes:


1. Zum [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf **Durchsuchen**.
3. Klicken Sie auf **SQL Server**.
4. Neben Ihrer Server oder Datenbank erzählt:
 - ![Symbol für einen v12-Server](./media/sql-database-v12-whats-new/v12_icon.png) **V12 logischer Server**
 - ![Symbol für frühere Version Server](./media/sql-database-v12-whats-new/earlier_icon.png) **früheren Version logische Server**


Ein anderes Verfahren zum Ermitteln der Version ist zum Ausführen der `SELECT @@version;` Anweisung in der Datenbank und ähnliche Ergebnisse:


- **12**.0.2000.10 &nbsp; *(Version V12)*
- **11**.0.9228.18 &nbsp; *(Version V11)*


V12-Datenbank kann nur auf einem V12 logischen Server gehostet werden. Und ein Server V12 V12-Datenbanken.


Wenn Sie noch nicht auf V12 ausgeführt werden, können Sie die Schritte im [Upgrade auf SQL Datenbank V12 an](sql-database-v12-plan-prepare-upgrade.md)logischen Server aktualisieren.


## <a name="V12AzureSqlDbPreviewGaTable"></a>Allgemeine Verfügbarkeit Regionen


- Bis zum 31. Juli 2015 haben alle Regionen, allgemeine Verfügbarkeit heraufgestuft.
- V12 wurde im Dezember 2014 jedoch nur den Status der Vorschau veröffentlicht.

[Zusätzliche Geschäftsbedingungen von Microsoft Azure-Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

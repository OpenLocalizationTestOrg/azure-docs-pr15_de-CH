<properties 
   pageTitle="Häufig gestellte Fragen zur Azure SQL-Datenbank" 
   description="Antworten auf häufige Fragen von Kunden Fragen Clouddatenbanken und Azure SQL-Datenbank, Microsoft Datenbank-Managementsystem (RDBMS) und Datenbank als Dienst in der Cloud." 
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
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>SQL Datenbank häufig gestellte Fragen

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Wie wird die Verwendung der SQL-Datenbank auf meiner Rechnung angezeigt? 
SQL Datenbank Rechnungen vorhersagbare Stundensatz basierend auf der Dienstebene + Leistungsstufe für einzelne Datenbanken oder eDTUs pro Pool elastische Datenbanken. Tatsächlichen Verbrauch berechnet und anteilig stündlich, damit Ihre Rechnung Brüche Stunde angezeigt. Beispielsweise wird 12 Stunden im Monat eine Datenbank vorhanden ist, Ihre Rechnung Verwendung von 0,5 Tage. Darüber hinaus werden Dienstebenen + Leistung und eDTUs pro Pool aufgeschlüsselt in der Stückliste leichter an die Anzahl der für jede in einem Monat verwendet.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Wenn eine einzelne ist aktiv für weniger als eine Stunde oder weniger als einer Stunde verwendet eine höhere Service-Stufe?
Werden berechnet für jede Stunde eine Datenbank vorhanden ist, mit der höchsten Service-Stufe + Leistung bei dieser Stunde unabhängig Verwendung, ob die Datenbank weniger als eine Stunde aktiv war. Wenn Sie eine Datenbank erstellen und fünf Minuten löschen reflektiert beispielsweise Ihre Rechnung eine Gebühr für eine Datenbank Stunde. 

Beispiele
    
- Wenn Sie eine Datenbank erstellen und dann sofort Standard s1 aktualisieren, werden Sie Geschwindigkeit Standard S1 Stunde berechnet.

- Wenn Sie eine Datenbank von Basic Premium 10:00 Uhr aktualisieren und Aktualisierung 1:35 Uhr am folgenden Tag werden Sie Premium Rate ab 1:00 Uhr berechnet 

- Wenn Sie eine Datenbank aus einfachen um 11:00 Uhr downgraden 15 Uhr abgeschlossen ist und die Datenbank erhoben Rate Premium bis 3:00 Uhr nach dem wird der Grundpreis berechnet.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Wie erscheint elastische Datenbank Auslastung auf meiner Zahlung und was passiert, wenn ich ändern eDTUs pro Pool?
Elastische Datenbank Pool Gebühren angezeigt auf Ihrer Abrechnung elastischen DTUs (eDTUs) in den Schritten unter eDTUs pro Pool auf [Preise](https://azure.microsoft.com/pricing/details/sql-database/)angezeigt. Es ist kostenlos pro Datenbank elastische datenbankpools. Sie werden für jede Stunde berechnet ein Pool existiert am höchsten eDTU unabhängig Verwendung, ob der Pool weniger als eine Stunde aktiv war. 

Beispiele

- Beim Erstellen von elastische Standarddatenbank Pool mit 200 eDTUs um 18 Uhr werden fünf Datenbanken zum Pool hinzufügen 200 eDTUs ganze Stunde berechnet ab 11 Uhr durch den Rest des Tages.
- Tag2, 5:05 Uhr Datenbank 1 beginnt 50 eDTUs und stetig bis zum Tag enthält. Datenbanken 2-5 schwanken zwischen 0 und 80 eDTUs. Während des Tages fügen Sie fünf Datenbanken, die unterschiedliche eDTUs ganzen Tag beanspruchen. Tag2 ist eine vollständige 200 eDTU abgerechnet. 
- An Tag 3, 5 Uhr ein anderes 15 Datenbanken hinzufügen. Verwendung der Datenbank erhöht zu beschließen zu eDTUs für den Pool von 200 bis 400 05 Uhr zeigen Zuschläge auf 200 eDTU galten bis 20: 00 Uhr und um 400 eDTUs für die verbleibenden vier Stunden. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Wie wird die Verwendung der aktive Geo-Replikation in einem Datenbankpool elastische auf meiner Zahlung angezeigt?
Im Gegensatz zu einzelnen Datenbanken elastische Datenbanken [Aktive Geo-Replikation](sql-database-geo-replication-overview.md) mit Abrechnung unmittelbar keinen.  Sie Zahlen nur für die Bereitstellung aller Pools (Pool primären und sekundären Pool) eDTUs

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Wie wirkt sich die Verwendung der Überwachungsfunktion meiner Rechnung? 
Überwachung basiert in der SQL-Datenbankdienst ohne zusätzliche Kosten und Basic, Standard und Premium-Datenbanken zur Verfügung. Zum Speichern der Überwachungsprotokolle basierend Überwachung Funktion verwendet ein Azure Storage-Konto und für Tabellen und Warteschlangen in Azure Storage gelten auf die Protokollgröße überwachen.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Wie finde ich das richtige Service-Ebene und Performance Level für einzelne Datenbanken und elastische Datenbank? 
Es gibt einige Tools verfügbar. 

- Lokalen Datenbanken verwenden Sie [DTU Sizing Berater](http://dtucalculator.azurewebsites.net/) zu den Datenbanken und DTUs erforderlich und auswerten Sie Datenbanken für elastische datenbankpools.
- Eine einzelne Datenbank in einem Pool profitieren würden, empfiehlt Azure intelligente Modul elastischen Datenbankpool sieht es historisch Verwendungsmuster, die garantiert. Siehe [Überwachen und Verwalten einer elastischen Datenbank mit Azure-Portal](sql-database-elastic-pool-manage-portal.md). Informationen selbst dafür sehen Sie [und Hinweise für einen Datenbankpool elastische](sql-database-elastic-pool-guidance.md)
- Um festzustellen, ob Sie eine einzelne Datenbank nach oben oder unten wählen müssen, finden Sie unter [Performance-Leitfaden für einzelne Datenbanken](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Wie oft kann der Dienstebene Tier oder Leistung einer einzelnen Datenbank ändern? 
V12-Datenbanken können Sie die Dienstebene (zwischen Basic, Standard und Premium) oder die Leistung innerhalb einer Dienstebene (z.B. S1, S2) ändern beliebig oft. Für Datenbanken aus früheren Versionen können Sie die Dienstebene Tier oder Leistung insgesamt vier Mal innerhalb von 24 Stunden ändern.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Wie oft kann ich die eDTUs pro Pool anpassen? 
So oft Sie möchten.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Wie lange dauert der Dienstebene Tier oder Leistung einer einzelnen Datenbank ändern oder Verschieben einer Datenbank in einem Datenbankpool elastische? 
Ändern der Dienstebene einer Datenbank und verschieben in einem Pool muss die Datenbank auf der Plattform im Hintergrund kopiert werden. Ändern der Dienstebene dauert einige Minuten je nach der Größe der Datenbanken mehrere Stunden. In beiden Fällen bleiben die Datenbanken während des Verschiebens online und verfügbar. Informationen zum Ändern der einzelner Datenbanken finden Sie unter [Ändern der Dienstebene einer Datenbank](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Wann sollte eine einzelne Datenbank und elastische Datenbanken verwenden? 
Im Allgemeinen elastische datenbankpools dienen normalerweise [Software als Service (SaaS) Anwendungsmuster](sql-database-design-patterns-multi-tenancy-saas-applications.md)besteht eine Datenbank pro Kunde oder Mieter. Einzelne Datenbanken Kauf und Bereitstellung überproportional vieler Variablen und peak bei Bedarf für jede Datenbank ist nicht kosteneffizient. Pools verwalten die kollektive Leistung des Pools und Datenbanken nach oben und unten Automatisches Skalieren. 

Azure intelligente Modul empfiehlt einen Pool für Datenbanken, wenn ein Verwendungsmuster garantiert. Details finden Sie in der [SQL-Datenbank Preise Tier Recommendations](sql-database-service-tier-advisor.md). Detaillierte Informationen über die Wahl zwischen Einzel- und elastische Datenbanken anzeigen Sie [und Aspekte für elastische datenbankpools](sql-database-elastic-pool-guidance.md)

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Was bedeutet es bis zu 200 % der maximalen bereitgestellte Datenbank Speicher für backup-Speicher? 
Backup-Speicher ist der Speicher zugeordnete Backups automatisierter für [Point-In-Time-Wiederherstellung verwendet werden](sql-database-recovery-using-backups.md#-point-in-time-restore) und [Geografischen Wiederherstellung](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL-Datenbank bietet bis zu 200 % der maximalen bereitgestellte Datenbankspeicher backup-Speicher ohne zusätzliche Kosten. Beispielsweise haben Sie einen Standard Datenbankinstanz bereitgestellte DB-Größe von 250 GB mit 500 GB backup-Speicher kostenlos erhalten Sie. Wenn Ihre Datenbank bereitgestellten backup-Speicher überschreitet, können Sie reduzieren die Aufbewahrungsdauer von Azure Kundendienst oder zusätzliche backup-Speicher Standardsatz Lesezugriff geografisch redundanten Speicher (RA-GRS) abgerechnet. Weitere RA-GRS Abrechnung Preise Speicherdetails anzeigen

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Muss ich ziehe aus Online-Business zu neuen Dienstebenen was ich wissen?
Azure SQL Web Datenbanken werden jetzt deaktiviert. Die Stufen Basic, Standard, Premium und elastisch ersetzen die Abschaltung Web Datenbanken. Wir haben weitere häufig gestellte Fragen, mit denen Sie in dieser Übergangszeit sollte. [Web und Business Edition Sonnenuntergang FAQ](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Was ist eine erwartete Replikation Lag bei Geo Datenbank zwischen zwei gleichen Azure Geographie?  
Wir unterstützen derzeit einen RPO von Sekunden und die Replikation Verzögerung wurde kleiner ist als der gepaarten Region bei Geo-sekundäre in Azure gehostet wird empfohlen, und die gleichen Service-Tier.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Was ist eine erwartete Replikations Verzögerung beim Geo sekundäre im selben Bereich wie die primäre Datenbank?  
Empirische Daten wird nicht viel Unterschied zwischen Region innerhalb und zwischen Bereich Replikation Lag die Azure sollten paarweise Region verwendet wird. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Liegt ein Netzwerkfehler zwischen zwei Bereiche, wie funktioniert die Wiederholungslogik bei Geo-Replikation?  
Ist eine Trennung, wiederholen wir alle 10 Sekunden, um Verbindungen.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Was kann ich tun, um sicherzustellen, dass eine wesentliche Veränderung in der primären Datenbank repliziert wird?
Geo-sekundär ist ein Async und wir versuchen nicht vollständig synchronisiert mit dem primären beibehalten. Sondern eine Methode zur Synchronisierung die Replikation von kritischen ändert (z. B. Kennwort Updates) erzwingen. Synchronisierung beeinträchtigt die Leistung, da den aufrufenden Thread blockiert wird, bis alle festgeschriebenen Transaktionen repliziert werden. Weitere Informationen finden Sie unter [Sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Welche Tools zur Überwachung der Replikation Verzögerung zwischen der primären Datenbank und Geo sekundäre sind?
Wir setzen Echtzeitreplikation Verzögerung zwischen der primären Datenbank und Geo-sekundär durch eine DMV. Weitere Informationen finden Sie unter [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

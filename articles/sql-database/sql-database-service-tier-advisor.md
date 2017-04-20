<properties 
   pageTitle="Preise Tier Vorschläge für Azure SQL-Datenbank" 
   description="Beim Ändern der Preise sind Ebenen in Azure-Portal Preise Tier Vorschläge sollten die Ebene, der am besten geeignet für eine vorhandene Azure SQL-Datenbank Arbeitslast ausgeführt. Tarifen Beschreiben der Service-Tier und Leistung einer SQL-Datenbank." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>SQL Datenbank Preise Tier recommendations

 Preise Tier Recommendations vorschlagen Tier und Leistung Dienstebene, die für eine vorhandene SQL Azure-Datenbank-Arbeitslast ausgeführt.

> [AZURE.NOTE] Preise Tier Vorschläge sind nur verfügbar für Web Datenbanken und elastische Datenbank – und nur in [Azure-Portal](https://portal.azure.com/).


Preise Tier Recommendations bei folgenden Aufgaben:

- [Ändern der Service Tier und Leistung (Tarif) eine SQL-Datenbank](sql-database-scale-up.md)
- [SQL Azure-Server auf V12 aktualisieren](sql-database-upgrade-server-portal.md)
- Wechseln Sie zu dem V12-Server. [SQL Datenbank Preise Tier Vorschläge](sql-database-service-tier-advisor.md)anzeigen
- [Elastische Datenbankpool erstellen](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>Übersicht

Der SQL-Datenbankdienst analysiert aktuelle Leistung und Anforderungen mit der Bewertung der Verlaufsdaten Ressourcenverwendung für eine SQL-Datenbank. Darüber hinaus wird die minimale akzeptablen Dienstebene bestimmt anhand der Größe der Datenbank und der aktivierten [Business Continuity](sql-database-business-continuity.md) -Funktionen. 

Diese Informationen analysiert und der Dienstebene Tier und Leistung, der am besten geeignet für die Datenbank normalerweise Arbeitslast ausgeführt und es aktuelle Funktionsumfang ist wird empfohlen.

- Der Dienst überprüft die vorherigen 15 bis 30 Tage Verlaufsdaten (Ressourcenverwendung Datenbankgröße und Datenbankaktivität) und führt einen Vergleich zwischen Ressourcen und die tatsächlichen Grenzen der derzeit Dienstebenen und Performance.
- Daten in Intervallen von 15 Sekunden analysiert wird und jedes Intervall Resultset in vorhandenen Dienstebene und Leistungsstufe am besten geeignet für die Behandlung dieses Resultset Arbeitslast.
- Dieser 15 Sekunden sind Beispiele dann zu größeren 15-30 Tage Analyse und Tier und Leistung Dienstebene, die optimal 95 % der Verlaufsdaten Arbeitslast verarbeiten kann, wird empfohlen.

### <a name="recommendations"></a>Empfehlung

Anhand der Datenbankverwendung zurzeit 2 Kategorien empfohlenen auftreten können:


| Empfehlung | Beschreibung |
| :--- | :--- |
| Aktualisieren | Upgrade auf eine neue Ebene. |
| Nicht verfügbar | Eine Datenbank erfordert eine minimale Auslastung oder etwa 35 Tage Aktivität. Es ist nicht genügend Daten, um eine gültige Empfehlung. |

## <a name="getting-pricing-tier-recommendations"></a>Erste Ebene empfohlenen Preisen

Preise Tier Empfehlung durch Auswählen einer vorhandenen Datenbank Web oder Business, **Alle**klicken **Preisstufe (Skalieren DTUs)**. (Preise Tier Vorschläge sind auch verfügbar, wenn Sie [Upgrade Azure SQL Server V12](sql-database-upgrade-server-portal.md).)

1. Mit der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **Durchsuchen** > **SQL-Datenbanken**.
4. Klicken Sie auf der Datenbank, eine Empfehlung für Blatt **SQL-Datenbanken** :

    ![Datenbank auswählen][1]

5. Auf die Datenbank wählen Sie **Alle** dann **Preisstufe (Skalieren DTUs)**.


7. **Empfohlene Preisgestaltung Ebenen** öffnen, klicken Sie auf die empfohlene Stufe und klicken **Wählen Sie** auf dieser Ebene ändern.

    ![Melden Sie sich für die Vorschau][4]

8. Klicken Sie optional auf **Ansicht Verwendungsdetails** **Tier Empfehlung Preisdetails** Blade öffnen Sie empfohlene Ebene für die Datenbank und einem Featurevergleich zwischen aktuellen und empfohlene Ebenen und ein Diagramm der Verwendungsanalyse historisch Ressource.

    ![Melden Sie sich für die Vorschau][5]



## <a name="summary"></a>Zusammenfassung

Preise Tier Recommendations bieten eine automatisierte Lösung Telemetriedaten für jede SQL-Datenbank und optimale Service Tier-Performance Level Kombination auf der Grundlage einer Datenbank muss die tatsächliche Leistung und Anforderungen empfehlen. Klicken Sie auf die Einstellungen **Preisstufe (Skalieren DTUs)** Preise Tier Vorschläge für Web- und Datenbanken.



## <a name="next-steps"></a>Nächste Schritte

Abhängig von den Details einer speziellen Datenbank ausführen ein Upgrade oder Downgrade normalerweise nicht sofort geschieht. Das Portal angeben Benachrichtigungen Übergängen Datenbank auf die neue Ebene oder überwachen den Aktualisierungsstatus von [sys.dm_operation_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270022.aspx) auf master Datenbank SQL Server-Datenbank Abfragen.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 

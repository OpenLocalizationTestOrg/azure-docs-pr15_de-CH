<properties 
   pageTitle="SQL Azure-Datenbank Query Performance Insight" 
   description="Abfrage Performance monitoring bezeichnet die meisten CPU-Nutzung Abfragen einer Azure SQL-Datenbank." 
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
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>SQL Azure-Datenbank Query Performance Insight


Verwalten und Optimieren der Leistung von relationalen Datenbanken ist eine Herausforderung, die erhebliche und Zeit erforderlich ist. Leistung Einblick Abfrage können Sie weniger Zeit Datenbankperformance folgende Problembehandlung:

- Einblick in Ihre Datenbanken Ressourcenverbrauch (DTU). 
- Erste Abfragen nach CPU/Dauer/Ausführung Anzahl potenziell für verbesserte Leistung optimiert werden können.
- Die Möglichkeit, einen Drilldown in die Details einer Abfrage hier Text und Verlauf der Ressourcenverwendung. 
- Performance-tuning Anmerkungen, die Aktionen von [SQL Azure-Datenbank Ratgeber](sql-database-advisor.md) anzeigen  



## <a name="prerequisites"></a>Erforderliche Komponenten

- Leistung Einblick Abfrage ist nur bei Azure SQL-Datenbank V12 verfügbar.
- Leistung Einblick Abfrage muss [Abfrage Speicher](https://msdn.microsoft.com/library/dn817826.aspx) in der Datenbank aktiv. Wenn Query Store nicht ausgeführt wird, fordert das Portal aktivieren.

 
## <a name="permissions"></a>Berechtigungen

Zugriffsberechtigungen von [rollenbasierten Zugriff](../active-directory/role-based-access-control-configure.md) müssen Leistung Einblick Abfrage verwenden: 

- Die beliebteste Quelle verwendeten Abfragen und Diagramme sind **Reader**, **Besitzer**, **Teilnehmer**, **SQL DB Teilnehmer-**oder **SQL Server Contributor** Berechtigungen erforderlich. 
- **Besitzer**, **Teilnehmer**, **SQL DB Teilnehmer-**oder **SQL Server Contributor** Berechtigungen müssen Abfragetext anzuzeigen.



## <a name="using-query-performance-insight"></a>Verwenden einen Einblick Abfrage-Leistung

Einen Einblick Abfrage-Leistung ist einfach zu verwenden:

- [Azure-Portal](https://portal.azure.com/) öffnen und Datenbank zu suchen. 
  - Wählen Sie Menü links unter Support und Problembehandlung "Query Performance Insight".
- Überprüfen Sie die Liste der Top ressourcenintensive Abfragen auf der ersten Registerkarte.
- Wählen Sie eine einzelne Abfrage Details dazu anzuzeigen.
- Öffnen Sie [SQL Azure-Datenbank Advisor](sql-database-advisor.md) und überprüfen Sie, ob alle empfohlenen verfügbar sind.
- Schieberegler verwenden oder zoom Symbole beobachteten Intervall ändern.

    ![Leistungsdashboard](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Ein paar Stunden von Abfrage laden SQL Datenbank Abfrage Leistung Einblicke erfasst werden muss. Wenn die Datenbank keine Aktivität oder Abfrage Speicher während eines bestimmten Zeitraums nicht aktiv war, werden die Diagramme leer sein Periode anzeigen. Wenn nicht ausgeführt wird, können Sie Query Store jederzeit aktivieren.   



## <a name="review-top-cpu-consuming-queries"></a>Überprüfen Sie obere CPU verbraucht Abfragen

Das [Portal](http://portal.azure.com) folgendermaßen vor:

1. SQL Datenbank durchsuchen und auf **Alle** > **Support + Problembehandlung** > **Leistung Einblick Abfrage**. 

    ![Einen Einblick Abfrage-Leistung][1]

    Erste Abfragen anzeigen Öffnet und oben verwendeten CPU-Abfragen aufgelistet.

1. Klicken Sie auf, um das Diagramm für Details.<br>Die oberste Zeile zeigt insgesamt DTU % für die Datenbank die Balken CPU verbraucht durch die ausgewählten Abfragen im ausgewählten Intervall angezeigt (z. B., wenn der ausgewählte **Woche** jeder Balken steht für einen Tag).

    ![erste Abfragen][2]

    Untersten Raster stellt aggregierte Informationen für Abfragen angezeigt.

  - Abfrage-ID-Bezeichner der Abfrage in der Datenbank.
  - CPU pro Abfrage erkennbare Intervall (Aggregationsfunktion abhängig).
  - Dauer pro Abfrage (Aggregationsfunktion abhängig).
  - Gesamtanzahl der Ausführung für eine bestimmte Abfrage.

    Aktivieren Sie oder deaktivieren Sie einzelne Abfragen oder das Diagramm mithilfe von Kontrollkästchen ausschließen.

1. Wenn die Daten veralten, klicken Sie auf **Aktualisieren** .
1. Verwenden Sie Schieberegler und zoom-Tasten Beobachtung Intervall ändern und Spitzen untersuchen:  ![Settings](./media/sql-database-query-performance/zoom.png)
1. Optional können Sie ggf. eine andere Ansicht **benutzerdefinierte** Registerkarte und festlegen:
  
  - Metrik (CPU, Dauer, Ausführungsanzahl)
  - Zeitintervall (letzte 24 Stunden, letzte Woche, letzten Monat). 
  - Anzahl der Abfragen.
  - Aggregatfunktion.

    ![Einstellungen](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Abfrage-Details anzeigen

Abfragedetails anzeigen:

1. Klicken Sie auf eine Abfrage in der Liste der Top-Abfragen.

    ![Details](./media/sql-database-query-performance/details.png)

1. Die Detailansicht wird geöffnet und Abfragen CPU-Verbrauch, Dauer/Ausführung Anzahl mit der Zeit aufgeschlüsselt.
1. Klicken Sie auf, um das Diagramm für Details.
  - Obere Diagramm mit insgesamt Datenbank DTU % und Balken CPU von der ausgewählten Abfrage verwendet werden.
  - Im zweiten Diagramm zeigt Gesamtdauer von der ausgewählten Abfrage.
  - Unteren Diagramm insgesamt wie die ausgewählte Abfrage.
    
    ![Abfragedetails][3]

1. Optional, Schieberegler, vergrößern Sie Schaltflächen oder auf **Einstellungen** anpassen, wie Daten angezeigt oder einen anderen Zeitraum auswählen.

## <a name="review-top-queries-per-duration"></a>Erste Abfragen pro Dauer überprüfen

In der letzten Aktualisierung der Abfrage Performance Insight wir zwei neue Metriken, die Engpässe erleichtern eingeführt: Anzahl Dauer und Ausführung.<br>

Abfragen haben das größte Potenzial für mehr Ressourcen Sperren andere Benutzer blockieren und Skalierbarkeit einschränken. Sie sind auch die besten Kandidaten für die Optimierung.<br>

Lang ausgeführte Abfragen identifizieren:

1. Öffnen Sie Registerkarte **Benutzerdefiniert** in Query Performance Insight für ausgewählte Datenbank
1. Ändern Sie Metriken **Dauer**
1. Anzahl der Abfragen und Beobachtung Intervall
1. Wählen Sie die Aggregatfunktion
  - **Summe** addiert alle Ausführung der Abfrage während der gesamten Beobachtung Intervall.
  - **Max** findet Abfragen die Ausführungszeit in ganzen Beobachtung Intervall maximale wurde.
  - **AVG** durchschnittliche Ausführungszeit aller Abfrage wie sucht und anzeigen die Top diese Durchschnittswerte. 

    ![Abfragedauer][4]

## <a name="review-top-queries-per-execution-count"></a>Erste Abfragen pro Ausführungsanzahl überprüfen

Hohe Anzahl der Ausführung möglicherweise nicht Datenbank beeinflussen und Ressourcenverbrauch kann niedrig sein, aber allgemeine Anwendung möglicherweise langsam.

In einigen Fällen führt zu sehr hohen Ausführungsanzahl Netzwerkroundtrips. Schleifen erheblich beeinträchtigen. Sie sind Netzwerklatenz und Downstreamserver Latenz. 

Beispielsweise Zugriff auf viele datengesteuerten Websites stark der Datenbank für jede Benutzeranfrage. Während Connection pooling hilft, den gestiegenen Netzwerkdatenverkehr und Verarbeitungslast auf dem Datenbankserver die Leistung beeinträchtigen kann.  Allgemeine Beratung ist Schleifen auf ein Minimum.

Um häufig identifizieren ausgeführt Abfragen Abfragen ("weitschweifender"):

1. Öffnen Sie Registerkarte **Benutzerdefiniert** in Query Performance Insight für ausgewählte Datenbank
1. Metriken **Ausführungsanzahl** zu ändern
1. Anzahl der Abfragen und Beobachtung Intervall

    ![Abfrage Ausführungsanzahl][5]

## <a name="understanding-performance-tuning-annotations"></a>Understanding Performance tuning Kommentare 

Und Ihre Arbeitslast Query Performance Insight, bemerken Sie Symbole durch vertikale Linie auf das Diagramm.<br>

Diese Symbole sind Kommentare. Sie stellen die Aktionen von [SQL Azure-Datenbank Advisor](sql-database-advisor.md)Leistung. Durch hovering Anmerkung erhalten Sie grundlegende Informationen zur Aktion:

![Abfrage-Anmerkung][6]

Wenn Sie mehr wissen oder Berater Empfehlung anwenden möchten, klicken Sie auf das Symbol. Details der Aktion wird geöffnet. Empfehlung für aktive ist können Sie sofort mit Befehl anwenden.

![Anmerkung Abfragedetails][7]

### <a name="multiple-annotations"></a>Mehrere Kommentare. ###

Es ist möglich, dass wegen Zoomfaktor Strukturänderungen, die nebeneinander in eine reduziert erhalten. Dies wird durch spezielle Symbol, klicken Öffnen neuer Blade, Liste der Strukturänderungen gruppiert, wird angezeigt.
Korrelation von Abfragen und Aktionen für die Optimierung können um Ihre Arbeit besser zu verstehen. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Optimieren der Abfrage Speicherkonfiguration einen Einblick Abfrage-Leistung

Während der Nutzung der Leistung Einblick Abfrage können folgenden Abfrage Shop-Nachrichten auftreten:

- "Query Store ist nicht ordnungsgemäß für diese Datenbank konfiguriert. Klicken Sie hier, um mehr zu erfahren."
- "Query Store ist nicht ordnungsgemäß für diese Datenbank konfiguriert. Klicken Sie hier ändern." 

Diese Nachrichten werden normalerweise beim Query Store nicht neue Daten sammeln kann. 

Ersten Fall Abfrage Informationsspeicher wird schreibgeschützt und Parameter optimal eingestellt. Sie können dies beheben, durch Vergrößern von Abfrage laden oder Abfrage Speicher deaktivieren.

![Qds-Schaltfläche][8]

Zweiten Fall Abfrage Speicher ist deaktiviert oder nicht optimal Parameter festgelegt. <br>Ändern Sie die Richtlinie Archivierung und erfassen und aktivieren Sie Abfrage Speicher durch Ausführen der folgenden Befehle auch direkt im Portal:

![Qds-Schaltfläche][9]

### <a name="recommended-retention-and-capture-policy"></a>Empfohlene Richtlinie Aufbewahrung und Aufnahme

Es gibt zwei Arten von Richtlinien:

- Größe – auf AUTO festgelegt Daten automatisch bei max. erreicht gesäubert werden.
- Zeitbasierte - standardmäßig wir es auf 30 Tage festgelegt wird also wenn Query Store Speicherplatz ausführen, löschen sie Abfrageinformationen, die älter als 30 Tage

Aufzeichnen auf Richtlinien festgelegt werden:

- **Alle** – alle Abfragen aufgezeichnet.
- **Auto** -seltene Abfragen und Abfragen mit nicht signifikanter Kompilierung und Ausführung werden ignoriert. Intern werden die Schwellenwerte für die Dauer der Ausführung Count, kompilieren und Laufzeit bestimmt. Dies ist die Standardoption.
- **None** – Abfrage Laden beendet die Aufzeichnung neuer Abfragen Runtime Statistiken für bereits erfasste Abfragen noch erfasst sind.
    
Wir empfehlen, alle Richtlinien automatisch und saubere 30 Tage:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Vergrößern von Abfrage laden. Dies kann mit einer Datenbank verbinden und folgende Abfrage ausgeführt werden:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Diese Einstellung anwenden machen schließlich Query Store sammeln neue Abfragen, aber wenn Sie nicht warten möchten Abfrage Informationsspeicher gelöscht werden kann. 
> [AZURE.NOTE] Ausführen der folgenden Abfrage werden alle aktuellen Informationen im Informationsspeicher Abfrage gelöscht. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Zusammenfassung

Leistung Einblick Abfrage können die Auswirkung der Abfrage-Systemlast und Beziehung zu Datenbank Ressourcenverbrauch. Mit diesem Feature wird verbraucht Abfragen oben erfahren und leichter zu beheben, bevor sie zum Problem werden.




## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Informationen zum Verbessern der Leistung der SQL-Datenbank klicken Sie auf [Empfohlene](sql-database-advisor.md) **Leistung Einblick Abfrage** -Blade.

![Performance Advisor](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png


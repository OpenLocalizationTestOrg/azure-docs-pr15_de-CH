<properties
    pageTitle="Überwachen und Verwalten einer elastischen Datenbank mit Azure-Portal | Microsoft Azure"
    description="Erfahren Sie, wie Azure-Portal und integrierte Intelligenz SQL-Datenbank zu verwalten und passenden einen Pool skalierbare elastische Datenbank Datenbank optimieren und Verwalten der Kosten."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Überwachen Sie und verwalten Sie einer elastischen Datenbank mit Azure-portal

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Azure-Portal können Sie Überwachung und Verwaltung ein Pool für elastische Datenbanken und Datenbanken im Pool. Über das Portal können Sie die Auslastung des elastischen Pool und Datenbanken in diesem Pool überwachen. Sie ändert die elastische Pool machen und alle gleichzeitig senden. Hierzu gehören auch hinzufügen oder Entfernen von Datenbanken, Einstellungen elastischen Pool ändern oder Ihre Datenbank Einstellungen.

Die folgende Grafik zeigt einen Beispiel elastischen Pool. Die Ansicht enthält:

*  Diagramme für die Überwachung der Ressourcenverwendung elastischen Pool und Datenbanken im Pool enthalten.
*  **Der Pool Konfigurationsschaltfläche elastischen Pool ändern.**
*  Schaltfläche **Datenbank erstellen** erstellt eine neue Datenbank und die aktuellen elastischen Pool hinzugefügt.
*  Elastische Aufträge welche Management großer Datenbanken durch Ausführen von Transact-SQL-Skripts für alle Datenbanken in einer Liste.

![Pool-Ansicht][2]

Um die Schritte in diesem Artikel zu arbeiten, benötigen Sie einen SQL-Server in Azure mindestens eine Datenbank mit einer elastischen Pool. Wenn Sie keinen elastischen Pool finden Sie unter [Erstellen eines Pools](sql-database-elastic-pool-create-portal.md); Wenn Sie eine Datenbank haben, finden Sie im [Lernprogramm für SQL](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Elastische Pool überwachen

Sie können einen bestimmten Pool die Ressourcenverwendung zu wechseln. Standardmäßig ist Pool konfiguriert Speicher- und eDTU Nutzung der letzten Stunde. Das Diagramm kann konfiguriert werden andere Metrik über verschiedene Zeitfenster.

1. Wählen Sie einen Pool mit arbeiten.
2. Überwachter **elastische Pool** ist ein Diagramm **Ressourcenverwendung**gekennzeichnet. Klicken Sie auf das Diagramm.

    ![Elastische Pool überwachen][3]

    **Metrik** -Blade wird geöffnet, eine detaillierte Ansicht der angegebenen Kriterien über die angegebenen Zeitfenster.   

    ![Blatt Metrik][9]

### <a name="to-customize-the-chart-display"></a>Die Darstellung des Diagramms anpassen

Bearbeiten Sie das Diagramm und Blatt Metrik anderen Metriken wie CPU-Prozentsatz Daten e/a-Prozentsatz und Log IO Prozentsatz angezeigt.

2. Klicken Sie auf Blatt Metrik auf **Bearbeiten**.

    ![Klicken Sie auf Bearbeiten][6]

- Blatt **Diagramm bearbeiten** wählen Sie einen neuen Zeitraum (Stunden, heute oder letzte Woche) oder klicken Sie auf **benutzerdefinierten** Datumsbereich in den letzten zwei Wochen aktivieren. Wählen Sie den Diagrammtyp (Balken- oder) und dann die Ressourcen überwachen.

> Hinweis: Nur Metriken mit derselben Maßeinheit können im Diagramm gleichzeitig angezeigt werden. Beispielsweise "eDTU Prozentsatz" wählt werden dann nur mit Metriken wie die Maßeinheit auswählen Sie.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Klicken Sie auf **OK**.


## <a name="elastic-database-monitoring"></a>Elastische Datenbankzustands

Einzelne Datenbanken können auch potenzielle Probleme überwacht werden.

1. **Elastische Datenbank überwachen**ist ein Diagramm, Metriken für fünf Datenbanken angezeigt. Standardmäßig zeigt das Diagramm die oberen 5 Datenbanken im Pool durchschnittliche eDTU Verwendung in der letzten Stunde. Klicken Sie auf das Diagramm.

    ![Elastische Pool überwachen][4]

2. **Ressourcenverwendung der Datenbank** Blade wird angezeigt. Dies bietet eine detaillierte Ansicht der Datenbankverwendung im Pool. Das Raster im unteren Teil des Blades können Sie alle Datenbanken im Pool seine Verwendung im Diagramm (bis zu 5 Datenbanken) anzeigen auswählen. Im Diagramm angezeigt werden, indem Sie auf **Diagramm bearbeiten**Fenster Metriken und Uhrzeit können angepasst werden.

    ![Datenbank Ressource Auslastung blade][8]

### <a name="to-customize-the-view"></a>Zum Anpassen der Ansicht

1. Das Blade **Ressourcenverwendung der Datenbank** klicken Sie auf **Diagramm bearbeiten**.

    ![Klicken Sie auf Diagramm bearbeiten](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. Blatt Diagramm **Bearbeiten** wählen Sie einen neuen Zeitraum (Stunden oder letzte 24 Stunden), oder klicken Sie auf **benutzerdefinierte** wählen Sie einen anderen Tag in den letzten 2 Wochen angezeigt.

    ![Klicken Sie auf Benutzerdefiniert](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klicken Sie auf Dropdown **Datenbanken vergleichen** verschiedene Metrik Vergleich zu Datenbanken zu aktivieren.

    ![Bearbeiten Sie das Diagramm](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Datenbanken zur Überwachung auswählen

In der Datenbankliste die **Ressourcenverwendung der Datenbank** Blatt finden bestimmte Datenbanken durch die Seiten in der Liste oder geben den Namen einer Datenbank Sie. Verwenden Sie das Kontrollkästchen, um die Datenbank auszuwählen.

![Suchen nach Datenbanken überwachen][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Hinzufügen einer Benachrichtigung für eine Ressource pool

Sie können Ressourcen Regeln hinzufügen, die e-Mail oder Warnmeldungszeichenfolgen URL Endpunkte trifft die Ressource einen Schwellenwert Auslastung, den Sie haben eingerichtet.

> [AZURE.IMPORTANT]Ressourcenverwendung für elastische Pools wurde eine Verzögerung von mindestens 20 Minuten. Definieren von Alarmen von weniger als 30 Minuten für elastische wird derzeit nicht unterstützt. Alarme für elastische mit festgelegt (Parameter namens "-WindowSize" PowerShell-API) von weniger als 30 Minuten kann nicht ausgelöst werden. Stellen Sie sicher, dass Warnungen für elastische definieren einen Zeitraum (WindowSize) von mindestens 30 Minuten.

**Eine Warnung eine Ressource hinzu**

1. Klicken Sie auf Diagramm **Ressourcenverwendung** Blade **Metrik** zu öffnen, klicken Sie auf **quot**und füllen Sie **eine Warnungsregel hinzufügen** Blatt (**Ressource** wird automatisch eingerichtet Pool sein, mit dem Sie arbeiten).
2. Geben Sie einen **Namen** und eine **Beschreibung** , die Warnung, die Sie und die Empfänger bezeichnet.
3. Wählen Sie eine **Metrik** , die Warnung aus der Liste.

    Das Diagramm zeigt dynamisch Ressourcenverwendung für diese Metrik einen Schwellenwert auszuwählen.

4. Wählen Sie eine **Bedingung** (größer als, kleiner als usw.) und einen **Schwellenwert**.
5. Klicken Sie auf **OK**.



## <a name="move-a-database-into-an-elastic-pool"></a>Eine Datenbank in einer elastischen Pool verschieben

Sie können hinzufügen oder Entfernen von Datenbanken von einem vorhandenen Pool. Die Datenbanken können anderen Pools. Sie können jedoch nur Datenbanken hinzufügen, die auf dem logischen Server.

1. Blatt für den Pool klicken Sie **elastische Datenbanken** **Pool konfigurieren**.

    ![Klicken Sie auf Configure pool][1]

2. Klicken Sie im Blade **Konfigurieren Pool** **zu Pool hinzufügen**.

    ![Klicken Sie auf Pool hinzufügen](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Wählen Sie Blatt **Hinzufügen Datenbanken** Datenbank(en) Pool hinzugefügt. Klicken Sie auf **auswählen**.

    ![Auswählen von Datenbanken hinzufügen](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    **Konfigurieren Pool** Blade listet jetzt die Datenbank gewählte mit der **ausstehenden**hinzugefügt werden.

    ![Ausstehende Pool hinzufügen](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. **Pool-Blade konfigurieren**klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Verschieben einer Datenbank aus einer elastischen

1. Wählen Sie Blatt **Konfigurieren Pool** Datenbank(en) entfernen.

    ![Datenbanken auflisten](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klicken Sie auf **aus**.

    ![Datenbanken auflisten](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    **Konfigurieren Pool** Blade führt jetzt die Datenbank, deren Status auf **Pending**entfernt werden ausgewählt.

    ![Vorschau-Datenbank hinzufügen und entfernen](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. **Pool-Blade konfigurieren**klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Ändern der Leistung eines Pools

Überwachen die Ressourcenverwendung eines Pools können Sie feststellen, dass einige Korrekturen erforderlich sind. Vielleicht braucht das Pool in das Leistung oder Speicher. Möglicherweise möchten die Datenbank im Pool ändern. Sie können die Einrichtung des Pools jederzeit das beste Gleichgewicht zwischen Leistung und zu ändern. Siehe [Wenn sollte ein Datenbankpool elastische verwendet werden?](sql-database-elastic-pool-guidance.md) Weitere Informationen.

**So ändern Sie die eDTUs oder Storage Limits pro Pool und eDTUs pro Datenbank**

1. Öffnen Sie das Blatt **Pool konfigurieren** .

    Verwenden Sie unter **elastische Datenbank Pool Settings**eine Schiebereglers Pool ändern.

    ![Elastische Pool Ressourcenverwendung](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Ändert die Einstellung Display im der monatlichen Kosten der Änderung.

    ![Ein Pool und neue monatliche Kosten aktualisieren](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Elastische stellen erstellen und verwalten

Elastische Aufträge können Sie die Transact-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool ausgeführt werden. Sie können neue Aufträge erstellen oder Arbeitsplätze über das Portal verwalten.

![Elastische stellen erstellen und verwalten][5]


Vor der Verwendung von Aufträgen elastische Aufträge Komponenten installieren und Ihre Anmeldeinformationen. Weitere Informationen finden Sie unter [elastische Datenbank Aufträge (Übersicht)](sql-database-elastic-jobs-overview.md).

[Dezentrales Skalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md)finden Sie unter: elastische Datenbanktools Dezentrales Skalieren, Verschieben von Daten verwenden, oder erstellen Sie Transaktionen.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Datenbank elastischen pool](sql-database-elastic-pool.md)
- [Einen Pool für elastische Datenbanken mit dem Portal erstellen](sql-database-elastic-pool-create-csharp.md)
- [Einen Pool für elastische Datenbanken mit PowerShell erstellen](sql-database-elastic-pool-create-powershell.md)
- [Erstellen Sie einen Datenbankpool elastische mit C#](sql-database-elastic-pool-create-csharp.md)
- [Preis und Performance Considerations für elastische datenbankpools](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png

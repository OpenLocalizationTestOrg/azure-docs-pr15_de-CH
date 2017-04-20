<properties
    pageTitle="Eine einzelne Tabelle von Azure SQL-Datenbank-Sicherung wiederherstellen | Microsoft Azure"
    description="Erfahren Sie, wie eine einzelne Tabelle von Azure SQL-Datenbank-Sicherung wiederherstellen."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Eine einzelne Tabelle aus einer Azure SQL-Datenbank-Sicherung wiederherstellen

Möglicherweise eine Situation in der versehentlich Daten in einer SQL-Datenbank geändert, und Sie möchten nun die betroffene Tabelle wiederherzustellen. Dieser Artikel beschreibt eine einzelne Tabelle in einer Datenbank aus einer SQL-Datenbank [automatisch Sicherungskopien](sql-database-automated-backups.md)wiederherstellen.

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Vorbereitende Schritte: Benennen Sie die Tabelle und eine Kopie der Datenbank wiederherstellen
1. Kennzeichnen Sie die Tabelle in der SQL Azure-Datenbank, die durch die wiederhergestellte Kopie ersetzt werden soll. Verwenden Sie Microsoft SQL Management Studio, die Tabelle umzubenennen. Benennen Sie beispielsweise die Tabelle als &lt;Namen&gt;_old.

    **Hinweis** Um zu vermeiden, blockiert werden, stellen Sie sicher, dass keine Aktivität auf die Tabelle, die Sie umbenennen möchten. Wenn Probleme auftreten, stellen Sie sicher, die dieser in einem Zeitraum durchführen.

2. Wiederherstellen einer Sicherung der Datenbank an einem Punkt rechtzeitig in [Punkt In_Time wiederherstellen](sql-database-recovery-using-backups.md#point-in-time-restore) wie wiederherstellen möchten.

    **Notizen**:
    - Der Name der wiederhergestellten Datenbank werden im Format DBName + Zeitstempel. beispielsweise **Adventureworks2012_2016-01-01T22-12z).**. Dieser Schritt wird nicht auf dem Server vorhandenen Datenbanknamen überschrieben. Dies ist eine Sicherheitsmaßnahme und können Sie die wiederhergestellte Datenbank überprüfen, bevor sie ihre aktuelle Datenbank und benennen Sie die wiederhergestellte Datenbank für die Produktion soll.
    - Alle Performance-Stufen von grundlegenden Premium werden automatisch vom Dienst mit backup Aufbewahrung Metriken je nach der Ebene gesichert:

| DB-Wiederherstellung | Grundlegende Ebene | Standard-tiers | Premium-Ebenen |
| :-- | :-- | :-- | :-- |
|  Zeitpunkt wiederherstellen |  Einen Wiederherstellungspunkt innerhalb von 7 Tagen|Einen Wiederherstellungspunkt 35 Tagen| Einen Wiederherstellungspunkt 35 Tagen|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Kopieren der Tabelle aus der wiederhergestellten Datenbank mit SQL-Datenbank-Migrationstool
1. Downloaden Sie und installieren Sie des [Assistenten für die Migration von SQL-Datenbank](https://sqlazuremw.codeplex.com).

2. Öffnen Sie auf **Prozess auswählen** des Migrationsassistenten SQL Datenbank, wählen Sie **Datenbank analysieren-Migration**und klicken Sie auf **Weiter**.
![SQL Datenbank Migrationsassistenten - Prozess auswählen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. Klicken Sie im Dialogfeld **Verbindung mit Server** Einstellungen:
 - **Servername**: der SQL Azure-Instanz
 - **Authentifizierung**: **SQL Server-Authentifizierung**. Geben Sie Ihre Anmeldeinformationen.
 - **Datenbank**: **Master DB (Liste aller Datenbanken)**.
 - **Hinweis** Standardmäßig speichert der Assistent Ihre Anmeldeinformationen. Wenn Sie es **Vergessen Anmeldeinformationen**auswählen möchten.
![SQL Datenbank-Assistent - Quelle auswählen - Schritt 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. Wählen Sie im Dialogfeld **Datenquelle auswählen** wiederhergestellte Datenbank vom Abschnitt **Vorbereitungsschritte** als Quelle und klicken Sie auf **Weiter**.

    ![Datenbankmigration SQL - Quelle auswählen - Schritt 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. Klicken Sie im Dialogfeld **Objekte auswählen** die Option **bestimmte Datenbankobjekte auswählen** und wählen Sie die Table(or tables), die auf den Zielserver migrieren möchten.
![SQL Datenbank Migrationsassistenten - Objekte auswählen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. Klicken Sie auf der Seite **Zusammenfassung des Skript-Assistenten** auf **Ja** , wenn Sie aufgefordert werden, zu, ob Sie ein SQL-Skript generieren möchten. Sie können auch das TSQL-Skript für die spätere Verwendung speichern.
![SQL Datenbank Migrationsassistenten - Skript-Zusammenfassung](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Klicken Sie auf der Seite **Zusammenfassung** auf **Weiter**.
![SQL Datenbank Migrations-Assistent – Zusammenfassung](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. Auf der Seite **Setup Zielverbindung** **mit Server**verbinden Sie, und geben Sie die Details wie folgt:
    - **Servername**: Server-Zielinstanz
    - **Authentifizierung**: **SQL Server-Authentifizierung**. Geben Sie Ihre Anmeldeinformationen.
    - **Datenbank**: **Master DB (Liste aller Datenbanken)**. Diese Option Listet alle Datenbanken auf dem Zielserver.

    ![SQL Datenbank Migrationsassistenten - Ziel-Server-Verbindung einrichten](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Klicken Sie auf **Verbinden**wählen Sie die Zieldatenbank, der die Tabelle verschieben möchten, und klicken Sie auf **Weiter**. Dies sollte zuvor generierten Skript abgeschlossen und müsste neu verschobene Tabelle in die Zieldatenbank kopiert.

## <a name="verification-step"></a>Schritt
1. Fragen Sie ab, und Testen Sie die neu kopierte Tabelle um sicherzustellen, dass die Daten intakt sind. Nach der Bestätigung können Sie in Tabellenform **Vorbereitungsschritte** Abschnitt löschen. (z. B. &lt;Namen&gt;_old).

## <a name="next-steps"></a>Nächste Schritte

[Automatische Backups von SQL-Datenbank](sql-database-automated-backups.md)

<properties
    pageTitle="Erste Schritte mit SQL-Datenbanken Daten synchronisieren"
    description="Dieses Lernprogramm hilft Ihnen Einstieg in Azure SQL Data Sync (Vorschau)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Erste Schritte mit SQL Azure Data Sync (Vorschau)
In diesem Lernprogramm lernen Sie die Grundlagen von Azure SQL Daten synchronisieren mithilfe der Azure-Verwaltungsportal.

Minimale Erfahrung Azure SQL-Datenbank mit SQL Server vorausgesetzt. In diesem Lernprogramm erstellen Sie eine Synchronisierungsgruppe Hybrid (SQL Server und SQL-Datenbankinstanzen) vollständig konfiguriert und auf dem von Ihnen festgelegten Zeitplans synchronisiert.

> [AZURE.NOTE] Die vollständige technische Dokumentation für Azure SQL Data Sync früher auf MSDN befindet ist als PDF-Dateien zur Verfügung. Downloaden sie [hier](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Schritt 1: Verbinden Sie mit der SQL Azure-Datenbank

1. Melden Sie sich im [Verwaltungsportal](http://manage.windowsazure.com).

2. Klicken Sie im linken Bereich auf **SQL-Datenbanken** .

3. Klicken Sie auf **SYNC** am unteren Rand der Seite. Wenn SYNCHRONISIEREN klicken, wird eine Liste mit Dinge, die Sie hinzufügen können: **Neue Synchronisierungsgruppe** und **Neue Sync-Agent**.

4. Klicken Sie auf **Neue Sync-Agent**, um den neuen Agenten SQL Sync-Assistenten zu starten.

5. Wenn einen Agent vor **Downloaden sie hier**hinzugefügt haben.

    ![Bild1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Schritt 2: Hinzufügen eines Client-Agents
Dieser Schritt ist nur eine lokale SQL Server-Datenbank in der Synchronisierungsgruppe haben soll. Fahren Sie mit Schritt 4 hat der Synchronisierungsgruppe SQL Datenbank-Instanzen.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Schritt 2a: installieren die erforderliche Software
Sie sicher, dass Folgendes installiert auf dem Computer, auf dem der Client-Agent installiert, sein.

- **.NET Framework 4.0**

 Installieren Sie.NET Framework 4.0 [hier](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 SP1 System CLR-Typen (x86)**

 Installieren Sie Microsoft SQL Server 2008 R2 SP1 System CLR Types (x86) [hier](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Microsoft SQL Server 2008 R2 SP1 freigegebene Verwaltungsobjekte (x86)**

 Installieren Sie Microsoft SQL Server 2008 R2 SP1 freigegebene Management Objects (x86) [hier](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Schritt 2 b: einen neuen Client Agent installieren

Führen Sie die Schritte [Installieren eines Client-Agents (SQL Data Sync)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) zum Installieren des Agenten.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Schritt 2c: Beenden Sie den Assistenten neue SQL Data Sync-Agent

1.  Zurück zum Assistenten neue SQL Data Sync-Agent.
2.  Geben Sie dem Agent einen aussagekräftigen Namen.
3.  Wählen Sie in der Dropdownliste die **REGION** (Rechenzentrum) dieser Agent hosten.
4.  Wählen Sie aus der Dropdownliste aus **Abonnement** für diesen Agent hosten.
5.  Klicken Sie auf den Pfeil nach rechts.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Schritt 3: Registrieren einer SQL Server-Datenbank mit der Client-Agent

Nach der Installation der Client-Agent registriert alle lokalen SQL Server-Datenbank, die in einer Synchronisierungsgruppe mit dem Agent einschließen möchten.
Um eine Datenbank mit dem Agent registrieren, gehen Sie wie bei [Einer SQL Server-Datenbank mit einem Client](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Schritt 4: Erstellen einer Synchronisierungsgruppe


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Schritt 4a: Starten Sie den Assistenten neue Synchronisierungsgruppe

1.  Zurück zum [Verwaltungsportal](http://manage.windowsazure.com).
2.  Klicken Sie auf **SQL-Datenbanken**.
3.  Auf **SYNC hinzufügen** am unteren Rand der Seite und neue Synchronisierungsgruppe aus der Schublade.

    ![Bild2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Schritt 4 b: Geben Sie die grundlegenden


1.  Geben Sie einen aussagekräftigen Namen für die Synchronisierungsgruppe.
2.  Wählen Sie in der Dropdownliste die **REGION** (Rechenzentrum) zum Hosten dieser Synchronisierungsgruppe.
3. Klicken Sie auf den Pfeil nach rechts.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Schritt 4c: definieren den Sync-Hub

1. Wählen Sie aus der Dropdownliste aus SQL-Datenbankinstanz als Hub Gruppe synchronisieren dienen.
2. Geben Sie die Anmeldeinformationen für diese Instanz SQL Datenbank - **HUB Benutzername** und **Kennwort HUB**.
3. Warten Sie SQL Data Sync, den Benutzernamen und das Kennwort zu bestätigen. Sie sehen ein grünes Häkchen neben Kennwort angezeigt, wenn die Anmeldeinformationen bestätigt sind.
4. Wählen Sie aus der Dropdownliste die **KONFLIKTBEHEBUNG** Richtlinie.

 **Hub Wins** - Hub Datenbank schreiben Verweis Datenbankverkehrs geschrieben ändern, überschreiben Änderungen in der gleichen Datensatz verweisen. Funktionell bedeutet dies, dass die erste Änderung an den Hub geschrieben in andere Datenbanken übermittelt.


 **WINS-Client** - Änderungen an den Hub werden Änderung Referenzdatenbanken überschrieben. Funktionell bedeutet dies, dass die letzte Änderung an den Hub geschrieben aufbewahrt und in andere Datenbanken übertragen.

5.  Klicken Sie auf den Pfeil nach rechts.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Schritt 4d: hinzufügen eine Datenbank


Wiederholen Sie diesen Schritt für jede weitere Datenbank Synchronisierungsgruppe hinzufügen möchten.

1. Wählen Sie aus der Dropdownliste der Datenbank hinzufügen.

    Datenbanken in der Dropdownliste enthalten beide SQL Server-Datenbanken, die mit der SQL-Datenbankinstanzen und registriert wurden.
2.  Geben Sie Anmeldeinformationen für diese Datenbank - **Benutzernamen** und das **Kennwort**ein.
3.  Wählen Sie aus der Dropdownliste **SYNC-Richtung** für diese Datenbank.

    **Bidirektionale** - Änderungen in der Referenzdatenbank sind Hub-Datenbank und ändert die Hub-Datenbank auf die Datenbank geschrieben.

    **Synchronisierung vom Hub** - Datenbank empfängt Updates vom Hub. Er sendet keine Änderung an den Hub.

    **Synchronisierung mit dem Hub** - Datenbank Updates an den Hub gesendet. Ändert im Hub werden nicht in diese Datenbank geschrieben.

4.  Klicken Sie Erstellen der Synchronisierungsgruppe das Häkchen unten rechts des Assistenten. Warten Sie SQL Data Sync, die Anmeldeinformationen zu bestätigen. Ein grünes Häkchen zeigt an, dass die Anmeldeinformationen bestätigt werden.

5.  Klicken Sie ein zweites Mal auf das Häkchen. Jetzt wird wieder der **SYNC** -Seite unter SQL-Datenbanken. Diese Synchronisierungsgruppe wird jetzt mit den anderen Synchronisierungsgruppen und die Agents aufgeführt.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Schritt 5: Definieren Sie die Daten synchronisieren

Azure SQL Data Sync können Sie Tabellen und Spalten zum Synchronisieren auswählen. Möchten Sie auch eine Spalte so, dass nur bestimmte Werte Zeilen filtern (z. B. Age > = 65) synchronisieren, bei Azure SQL Data Sync-Portal verwenden und die Dokumentation wählen Sie Tabellen, Spalten und Zeilen zu synchronisierende definieren die Daten synchronisieren.

1.  Zurück zum [Verwaltungsportal](http://manage.windowsazure.com).
2.  Klicken Sie auf **SQL-Datenbanken**.
3.  Klicken Sie auf die Registerkarte **SYNCHRONISIEREN** .
4.  Klicken Sie auf den Namen dieser Gruppe synchronisieren.
5.  Klicken Sie auf die Registerkarte **SYNCHRONISIERUNG** .
6.  Wählen Sie die Datenbank synchronisieren Gruppe Schema bereitstellen möchten.
7.  Klicken Sie auf den Pfeil nach rechts.
8.  Klicken Sie auf **SCHEMA aktualisieren**.
9.  Wählen Sie die Spalten für die Synchronisationen für jede Tabelle in der Datenbank.
    - Spalten mit nicht unterstützten Datentypen können nicht ausgewählt werden.
    - Wenn keine Spalten in einer Tabelle ausgewählt werden, ist die Tabelle nicht in der Synchronisierungsgruppe enthalten.
    - Zum Aktivieren/deaktivieren die Tabellen klicken Sie am unteren Bildschirmrand.
10. Klicken Sie auf **Speichern**und Warten der Synchronisierungsgruppe Bereitstellung abgeschlossen.
11. Zur Startseite Daten synchronisieren klicken Sie zurück-Pfeil im linken oberen Bildschirmrand (über die Synchronisierungsgruppe Name).

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Schritt 6: Konfigurieren der Synchronisierungsgruppe

Sie können eine Synchronisierungsgruppe immer auf SYNC unten Data Sync-Zielseite synchronisieren.
Um nach einem Zeitplan synchronisieren, konfigurieren Sie die Synchronisierungsgruppe.

1.  Zurück zum [Verwaltungsportal](http://manage.windowsazure.com).
2.  Klicken Sie auf **SQL-Datenbanken**.
3.  Klicken Sie auf die Registerkarte **SYNCHRONISIEREN** .
4.  Klicken Sie auf den Namen dieser Gruppe synchronisieren.
5.  Klicken Sie auf die Registerkarte **Konfigurieren** .
6.  **AUTOMATISCHE SYNCHRONISIERUNG**
    - Klicken Sie zum Konfigurieren der Synchronisierungsgruppe in regelmäßigen Abständen synchronisieren **auf**. Sie können weiterhin bei Bedarf synchronisieren auf SYNCHRONISIEREN.
    - Klicken Sie auf **OFF** konfigurieren die Synchronisierungsgruppe Synchronisierung nur, wenn Sie SYNCHRONISIEREN klicken.
7.  **SYNCHRONISIERUNGSHÄUFIGKEIT**
    - Wenn automatische SYNCHRONISIERUNG auf ist, legen Sie die Synchronisierungsfrequenz. Die Frequenz muss zwischen 5 Minuten und 1 Monat.
8.  Klicken Sie auf **Speichern**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Herzlichen Glückwunsch! Sie haben eine Synchronisierungsgruppe erstellt eine Instanz der SQL-Datenbank und einer SQL Server-Datenbank.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu SQL-Datenbank und SQL Data Sync finden Sie unter:

* [Die vollständige SQL Data Sync technische Dokumentation](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [SQL-Datenbank (Übersicht)](sql-database-technical-overview.md)
* [Datenbank-Lifecycle-Management](https://msdn.microsoft.com/library/jj907294.aspx)
 

 

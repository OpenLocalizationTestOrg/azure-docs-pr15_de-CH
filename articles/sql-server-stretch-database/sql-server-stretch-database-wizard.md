<properties
    pageTitle="Erste Schritte mit der Datenbank aktivieren Stretch-Assistenten | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren einer Datenbank für Strecken Datenbank Stretch-Assistent mit der Datenbank aktivieren."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Erste Schritte mit der Datenbank aktivieren Stretch-Assistenten

Führen Sie zum Konfigurieren einer Datenbank für Strecken Datenbank Datenbank aktivieren Stretch-Assistenten.  Dieses Thema beschreibt die Informationen, die Sie eingeben und die Optionen im Assistenten müssen.

Erfahren Sie mehr über Stretch-Datenbank finden Sie unter [Stretch-Datenbank](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Später deaktivieren Stretch Datenbank daran Stretch-Datenbank für eine Tabelle oder eine Datenbank deaktiviert das remote-Objekt nicht löschen. Wenn Sie die entfernte Tabelle oder der entfernten Datenbank löschen möchten, müssen Sie mithilfe das Azure-Verwaltungsportal ablegen. Die entfernten Objekte weiterhin Azure Kosten, bis Sie sie manuell löschen. 

## <a name="launch-the-wizard"></a>Starten Sie den Assistenten

1.  Wählen Sie die Datenbank auf der Strecken soll, in SQL Server Management Studio im Objekt-Explorer.

2.  Rechts\-auf Wählen Sie **Aufgaben**, und wählen Sie **Strecken**und wählen Sie **Aktivieren** , um den Assistenten zu starten.

## <a name="Intro"></a>Einführung
Erläutern des Zwecks des Assistenten und der erforderlichen Komponenten.

Die folgenden: wichtigen Komponenten

-   Sie müssen ein Administrator Datenbank ändern.
-   Sie müssen ein Microsoft Azure-Abonnement verfügen.
-   Die SQL Server muss mit dem Remoteserver Azure kommunizieren.

![Einleitungsseite des Assistenten verlängern][StretchWizardImage1]

## <a name="Tables"></a>Tabellen auswählen
Wählen Sie Tabellen für gestreckt werden soll.

Tabellen mit vielen Zeilen am Anfang der sortierten Liste angezeigt. Bevor der Assistent die Liste der Tabellen angezeigt, analysiert diese Datentypen, die derzeit nicht von Stretch-Datenbank unterstützt werden.

![Seite des Assistenten Stretch Tabellen auswählen][StretchWizardImage2]

|Spalte|Beschreibung|
|----------|---------------|
|(kein Titel)|Aktivieren Sie das Kontrollkästchen in dieser Spalte die ausgewählte Tabelle für Strecken aktivieren.|
|**Name**|Gibt den Namen der Spalte in der Tabelle.|
|(kein Titel)|Ein Symbol in dieser Spalte kann eine Warnung darstellen,\'t verhindern das Aktivieren der ausgewählten Tabelle für Strecken. Darstellen auch ein blockierendes Problem, das Aktivieren der ausgewählten Tabelle für Strecken verhindert \- angenommen, da die Tabelle einen nicht unterstützten Datentyp verwendet. Zeigen Sie auf das Symbol mehr in einer QuickInfo angezeigt. Weitere Informationen finden Sie unter [Einschränkungen Stretch-Datenbank](sql-server-stretch-database-limitations.md).|
|**Gestreckt**|Gibt an, ob die Tabelle bereits für Strecken aktiviert ist.|
|**Migrieren**|Migrieren einer gesamten Tabelle (**Gesamte Tabelle**) oder Angeben eines Filters auf eine vorhandene Spalte in der Tabelle. Möchten Sie anderen Filter-Funktion zum Auswählen von Zeilen zu migrieren, führen Sie die Anweisung ALTER TABLE die Filterfunktion angeben, nachdem Sie den Assistenten beenden. Weitere Informationen über die Filterfunktion finden Sie unter [Migrieren mithilfe einer Filterfunktion wählen](sql-server-stretch-database-predicate-function.md). Weitere Informationen zu der Funktion finden Sie unter [Aktivieren Stretch Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md) oder [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Zeilen**|Gibt die Anzahl der Zeilen in der Tabelle.|
|**Größe (KB)**|Gibt die Größe der Tabelle in KB.|

## <a name="Filter"></a>Optional bieten Sie einen Zeilenfilter

Wenn Sie Filter Funktion Zeilen migrieren möchten, gehen Sie folgendermaßen vor auf **Tabellen auswählen** .

1.  Klicken Sie auf **Ganze Tabelle** in der Zeile für die Tabelle, in der Liste **Wählen Sie die Tabellen, die Sie vergrößern möchten** . Das Dialogfeld **Strecken wählen** wird geöffnet.

    ![Definieren Sie eine Filterfunktion][StretchWizardImage2a]

2.  Wählen Sie im Dialogfeld **Wählen Sie Strecken** **Zeilen auswählen**.

3.  Geben Sie im **Feld Name**einen Namen für die Filterfunktion.

4.  Die **Where** -Klausel wählen Sie eine Spalte aus der Tabelle einen Wert und einen Operator auswählen.

5. Klicken Sie auf **Überprüfen** , um die Funktion zu testen. Kehrt die Funktion Ergebnisse aus der Tabelle - Wenn Zeilen zu migrieren, die die Bedingung - meldet, also der Test **erfolgreich**.

    >   [AZURE.NOTE] Das Textfeld, in dem die Filterabfrage angezeigt ist. Die Abfrage im Textfeld kann nicht bearbeitet werden.

6.  Klicken Sie auf Fertig, um zur Seite **Tabellen auswählen** zurückzukehren.

Die Filterfunktion ist in SQL Server nur erstellt, wenn Sie fertig. Bis dahin können Sie zurück zur Seite **Tabellen auswählen** , ändern oder Umbenennen der Filterfunktion

![Wählen Sie Seite Tabellen nach dem Definieren einer Filterfunktion][StretchWizardImage2b]

Soll eine andere Art von Filterfunktion zu migrieren Zeilen auswählen, haben Sie die folgenden Aktionen.  

-   Beenden Sie den Assistenten, und führen Sie die Anweisung ALTER TABLE Strecken für die Tabelle aktiviert und eine Filterfunktion. Weitere Informationen finden Sie unter [Stretch Datenbank für eine Tabelle aktivieren](sql-server-stretch-database-enable-table.md).  

-   Führen Sie die Anweisung ALTER TABLE, um eine Filterfunktion festzulegen, nachdem Sie den Assistenten beenden. Die erforderlichen Schritte finden Sie unter [Hinzufügen eine Filterfunktion nach Ausführung des Assistenten](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="Configure"></a>Azure-Bereitstellung konfigurieren

1.  Melden Sie sich bei Microsoft Azure mit einem microsoftkonto an.

    ![Melden Sie sich bei Azure - Stretch-Datenbank-Assistenten][StretchWizardImage3]

2.  Auswählen des Azure-Abonnements für Stretch-Datenbank verwendet.

3.  Eine Azure-Region auswählen
    -   Erstellen eines neuen Servers wird der Server in diesem Bereich erstellt.  
    -   Haben Sie vorhandene Server in den ausgewählten Bereich, führt der Assistent sie, wenn Sie **vorhandene Server**auswählen.

    Wählen Sie Wartezeit minimieren, Azure Region in der der SQL Server befindet. Weitere Informationen über Bereiche finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

4.  Geben Sie an, ob vorhandenen Server verwenden oder einen neuen Azure Server erstellen.

    Wenn Active Directory auf Ihrem SQL Server mit Azure Active Directory verbunden ist, können optional verbundenen Dienstkonto für SQL Server Sie mit dem Remoteserver Azure kommunizieren. Weitere Informationen zu dieser Option finden Sie unter [ALTER Datenbankoptionen festlegen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Neuen Server erstellen**

        1.  Erstellen Sie einen Benutzernamen und ein Kennwort für den Administrator.

        2.  Optional verwenden Sie verbundenen Dienstkonto für SQL Server für die Kommunikation mit dem Remoteserver Azure.

        ![Neuen Azure Server - Stretch-Datenbank-Assistenten erstellen][StretchWizardImage4]

    -   **Vorhandenen server**

        1.  Wählen Sie den vorhandenen Azure Server.

        2.  Wählen Sie die Authentifizierungsmethode aus.

            -   Wenn Sie **SQL Server-Authentifizierung**auswählen, bieten Sie dem Administratorbenutzernamen und Kennwort.

            -   Wählen Sie **Active Directory-integrierte Authentifizierung** verbundenen Dienstkonto für SQL Server für die Kommunikation mit dem Remoteserver Azure. Der ausgewählte Server nicht in Azure Active Directory integriert ist, wird diese Option angezeigt.

        ![Wählen Sie vorhandene Azure Server - Stretch-Datenbank-Assistent][StretchWizardImage5]

## <a name="Credentials"></a>Sichere Anmeldeinformationen
Sie müssen einen Datenbank-Hauptschlüssel zum Schützen der Anmeldeinformationen, die Stretch-Datenbank mit der entfernten Datenbank herstellt.  

Wenn bereits ein Datenbank-Hauptschlüssel Geben Sie das Kennwort für sie.  

![Seite des Assistenten Stretch sichere Anmeldeinformationen][StretchWizardImage6b]

Die Datenbank keinen vorhandenen Master-Schlüssels, geben Sie ein sicheres Kennwort einen Datenbank-Hauptschlüssel erstellen.  

![Seite des Assistenten Stretch sichere Anmeldeinformationen][StretchWizardImage6]

Weitere Informationen zu den Datenbank-Hauptschlüssel finden Sie unter [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) und [erstellen einen Datenbank-Hauptschlüssel](https://msdn.microsoft.com/library/aa337551.aspx). Weitere Informationen über die Anmeldeinformationen, die der Assistent erstellt finden Sie unter [Erstellen Datenbank begrenzt Anmeldeinformationen (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>IP-Adresse
Verwenden Sie Subnetz IP-Adressbereich (empfohlen) oder die öffentliche IP-Adresse der SQL Server, eine Firewallregel auf Azure erstellen SQL Server die Kommunikation mit dem Remoteserver Azure.

Die IP-Adresse, die Sie auf dieser Seite sagen Azure Server eingehenden Daten, Abfragen und Verwaltungsvorgänge initiierte SQL Server Azure Firewall passieren können. Der Assistent Änderung nicht in der Firewall für den SQL Server-.

![Wählen Sie IP-Adressseite des Assistenten verlängern][StretchWizardImage7]

## <a name="Summary"></a>Zusammenfassung
Überprüfen Sie die eingegebenen Werte und Optionen im Assistenten und die geschätzten Kosten in Azure ausgewählt. Wählen Sie **Fertig stellen** Strecken aktivieren.

![Zusammenfassungsseite des Assistenten verlängern][StretchWizardImage8]

## <a name="Results"></a>Ergebnisse
Überprüfen Sie die Ergebnisse.

Zum Überwachen des Status der Datenmigration finden Sie unter [Überwachung und Problembehandlung bei der Datenmigration (Stretch Datenbank)](sql-server-stretch-database-monitor.md).

![Seite des Assistenten verlängern][StretchWizardImage9]

## <a name="KnownIssues"></a>Den Assistenten zur Fehlerbehebung
**Stretch-Datenbank-Assistent ist fehlgeschlagen.**
Wenn Sie den Assistenten ohne Administratorrechte führen zu Strecken Datenbank noch nicht auf Serverebene aktiviert, schlägt der Assistent fehl. Systemadministrator Stretch-Datenbank auf dem lokalen Server zu aktivieren, und führen Sie den Assistenten erneut aus. Weitere Informationen finden Sie unter [erforderliche: Berechtigung aktivieren Stretch-Datenbank auf dem Server](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Nächste Schritte
Aktivieren Sie zusätzliche Tabellen für Stretch-Datenbank. Datenmigration überwachen und Verwalten von Stretch\-Datenbanken und Tabellen aktiviert.

-   [Aktivieren Stretch Datenbank eine Tabelle](sql-server-stretch-database-enable-table.md) zusätzliche Tabellen aktivieren.

-   [Überwachung und Problembehandlung bei der Datenmigration](sql-server-stretch-database-monitor.md) den Status der Datenmigration anzuzeigen.

-   [Anhalten und Fortsetzen von Stretch-Datenbank](sql-server-stretch-database-pause.md)

-   [Verwaltung und Problembehandlung von Stretch-Datenbank](sql-server-stretch-database-manage.md)

-   [Sicherungsdatenbanken Strecken aktiviert](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Siehe auch

[Stretch-Datenbank für eine Datenbank aktivieren](sql-server-stretch-database-enable-database.md)

[Stretch-Datenbank für eine Tabelle aktivieren](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png

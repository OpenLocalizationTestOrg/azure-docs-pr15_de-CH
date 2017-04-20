<properties
    pageTitle="Verbinden Sie Excel mit SQL Datenbank | Microsoft Azure"
    description="Erfahren Sie, wie Microsoft Excel mit SQL Azure-Datenbank in die Cloud zu verbinden. Importieren Sie Daten für Berichte und Daten in Excel."
    services="sql-database"
    keywords="Verbindung auf excel, excel importieren"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Lernprogramm für SQL: eine SQL Azure-Datenbank mit Excel und Erstellen eines Berichts

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Erfahren Sie, wie Excel mit einer SQL-Datenbank in der Cloud verbinden, so dass Daten und Tabellen und Diagramme auf Basis von Werten in der Datenbank erstellen. In diesem Lernprogramm Verbindung zwischen Excel und einer festgelegten, speichern Sie die Datei, die Daten und die Verbindungsinformationen für Excel speichert, und erstellen Sie eine PivotChart aus den Datenbankwerten.

Sie benötigen eine Datenbank SQL Azure, bevor Sie beginnen. Wenn Sie haben, finden Sie unter [erstellen Ihre erste SQL-Datenbank](sql-database-get-started.md) zu einer Datenbank mit Beispieldaten von wenigen Minuten. In diesem Artikel Sie importieren Daten in Excel aus diesem Artikel jedoch Schritten wie Ihre eigenen Daten.

Sie benötigen außerdem eine Kopie der Excel. In diesem Artikel verwendet [Microsoft Excel 2016](https://products.office.com/en-US/).

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Eine SQL-Datenbank mit Excel und erstellen Sie eine Odc-Datei

1.  Zum Verbinden von Excel mit SQL-Datenbank öffnen Sie Excel eine neue Arbeitsmappe erstellen und Öffnen einer Excel-Arbeitsmappe.

2.  Klicken Sie in der Menüleiste am oberen Rand der Seite auf **Daten** **Aus anderen Quellen**klicken und **Aus SQL Server**klicken.

    ![Auswählen einer Datenquelle: Excel mit SQL-Datenbank verbinden.](./media/sql-database-connect-excel/excel_data_source.png)

    Der Datenverbindungs-Assistent wird geöffnet.

3.  Geben Sie im Dialogfeld **zum Datenbankserver verbinden** Sie möchten aus SQL-Datenbank- **Servername** <*Servername*>**. database.windows.net**. Beispielsweise **adworkserver.database.windows.net**.

4.  **Anmeldeinformationen**klicken Sie auf **Benutzername und Kennwort**unter Geben Sie **Benutzernamen** und **Kennwort** bei seiner Erstellung für die SQL-Datenbank einrichten und dann auf **Weiter**.

    ![Geben Sie die Server und der Anmeldeinformationen](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] Je nach Ihrem Netzwerk möglicherweise nicht herstellen oder Sie verlieren die Verbindung, wenn der SQL Server Verkehr von der IP-Adresse des Clients nicht. Zum [Azure-Portal](https://portal.azure.com/)auf SQL Server, klicken Sie auf den Server, klicken Sie unter Einstellung auf Firewall und die Client-IP-Adresse hinzufügen. Einzelheiten finden Sie unter [Konfigurieren der Firewall Settings](sql-database-configure-firewall-settings.md) .

5. Wählen Sie im Dialogfeld **Datenbank und Tabelle wählen** Sie aus der Liste möchten Datenbank und klicken Sie dann auf Tabellen oder Ansichten zu arbeiten (Wir haben **vGetAllCategories**), und klicken Sie dann auf **Weiter**.

    ![Wählen Sie eine Datenbank und Tabelle.](./media/sql-database-connect-excel/select-database-and-table.png)

    Die **Datenverbindungsdatei speichern und Fertig stellen** Dialogfeld, wo Sie Informationen über die Office-Datenbank Verbindungsdatei (Datenbankabfragedateien) bereitstellen, Excel verwendet. Sie können Standardwerte oder Ihre Auswahl anpassen.

6. Standardwerte können insbesondere Notieren Sie den **Dateinamen** . Eine **Beschreibung**, **Anzeigenamen**und **Schlüsselwörter** können Sie und andere Benutzer merken, was Sie zu verbinden und die Verbindung. **Immer versuchen, diese Datei zum Aktualisieren der Daten** klicken Sie auf Verbindungsinformationen in der Odc-Datei aktualisieren verbinden, und klicken Sie dann auf **Fertig stellen**.

    ![Speichern eine Odc-Datei](./media/sql-database-connect-excel/save-odc-file.png)

    Das Dialogfeld **Daten importieren** wird angezeigt.

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Die Daten in Excel importieren und eine PivotChart erstellen
Sie haben die Verbindung und erstellt die Datei Daten und Informationen, lesen Sie zum Importieren der Daten.

1. Im Dialogfeld **Daten importieren** auf das gewünschte zum Darstellen der Daten im Arbeitsblatt, und klicken Sie auf **OK**. **PivotChart**ausgewählt. Sie können auch ein **Neues Arbeitsblatt** zu erstellen oder **diese Daten an ein Datenmodell hinzufügen**. Weitere Informationen zu Datenmodellen finden Sie unter [erstellen ein Datenmodell in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Klicken Sie auf **Eigenschaften** , Informationen über die Odc-Datei, die Sie im vorherigen Schritt erstellt und Optionen zum Aktualisieren von Daten.

    ![Wählen das Format für die Daten in Excel](./media/sql-database-connect-excel/import-data.png)

    Das Arbeitsblatt hat nun eine leere PivotTables und PivotCharts.

8. Wählen Sie unter **PivotTable-Felder**alle die Kontrollkästchen für die Felder, die Sie anzeigen möchten.

    ![Konfigurieren der Datenbank.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Wenn Sie andere Excel-Arbeitsmappen und Arbeitsblätter mit der Datenbank verbinden möchten, **Klicken Sie auf**, klicken Sie auf **Verbindungen**klicken Sie auf **Hinzufügen**, wählen Sie aus der Liste erstellte Verbindung und klicken Sie auf **Öffnen**.
> ![Öffnen Sie eine Verbindung aus einer anderen Arbeitsmappe](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [mit SQL-Datenbank mit SQL Server Management Studio](sql-database-connect-query-ssms.md) für erweiterte Abfragen und Analysen.
- Informationen Sie zu den Vorteilen des [elastischen Pools](sql-database-elastic-pool.md).
- Informationen zum [Erstellen einer Web-Anwendung, die SQL-Datenbank auf dem Back-End verbunden](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).

<properties
    pageTitle="DB2-Connector in Ihren Apps Logik hinzufügen | Microsoft Azure"
    description="Übersicht über DB2 Connector mit REST-API"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Erste Schritte mit der DB2-connector
Microsoft-Connector für DB2 verbunden Logik Apps mit Ressourcen in IBM DB2-Datenbank. Dieser Connector enthält einen Microsoft Client remote DB2-Server-Computer über ein Netzwerk kommunizieren. Dazu gehören Clouddatenbanken IBM Bluemix DashDB oder IBM DB2 für Windows Azure Virtualisierung ausgeführt und lokalen Datenbanken mit lokalen Daten-Gateway. Finden Sie in der [Liste unterstützt](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2-Plattformen und Versionen (in diesem Thema).

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik Apps allgemein verfügbar (GA). 

DB2-Connector unterstützt die folgenden Datenbankvorgänge:

- Liste Tabellen
- Lesen Sie wählen Sie eine Zeile
- Lesen Sie wählen Sie alle Zeilen
- Fügen Sie eine Zeile mit INSERT
- Ändern Sie eine Zeile mit UPDATE
- Entfernen Sie eine Zeile mit löschen

In diesem Thema veranschaulicht die Verwendung des Connectors in einer app Logik Datenbankoperationen Prozess.

Erfahren Sie mehr über Logik Apps finden Sie unter [Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="available-actions"></a>Verfügbare Aktionen
DB2-Connector unterstützt Logik app Aktionen:

- GetTables
- [GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>Tabellen
Erstellen einer app Logik für jeden Vorgang besteht aus vielen Schritten durch Microsoft Azure-Portal.

Innerhalb der app Logik können Sie Tabellen in einer DB2-Datenbank eine Aktion hinzufügen. Die Aktion weist den Konnektor einer DB2-Schema-Anweisung beispielsweise zu `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik
1.  **Azure starten Board**, wählen **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2.  Geben Sie den **Namen**wie `Db2getTables`, **Abonnements**, **Ressourcengruppe**, **Speicherort**und **App Service-Plan**. Wählen Sie **Dashboard**, und wählen Sie **Erstellen**.

### <a name="add-a-trigger-and-action"></a>Trigger und Aktion hinzufügen
1.  Wählen Sie **Leere LogicApp** **Logik Apps Designer**in der Liste **Vorlagen** .
2.  Wählen Sie in der Liste **Trigger** **Wiederholung**. 
3.  Im Trigger **Serie** **Bearbeiten**wählen Sie aus wählen Sie **Häufigkeit** Dropdown- **Tag**auswählen und legen Sie das **Intervall** **7**eingeben.  
4.  Aktivieren Sie das **+ Neuer Schritt** , und wählen Sie **eine Aktion hinzufügen**.
5.  Geben Sie in der Liste **Aktionen** `db2` **Suche weitere Aktionen** Bearbeitungsfeld und dann **DB2 - Tabellen (Vorschau) abgerufen werden**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  Wählen Sie im Konfigurationsbereich **DB2 - Tabellen abrufen** **Kontrollkästchen** **Verbindung für lokale datengateway**aktivieren. Beachten Sie, dass die Cloud lokalen einstellen.
    - Geben Sie Wert für **Server**in Form von Adresse oder Alias Doppelpunkt Portnummer. Beispielsweise `ibmserver01:50000`.
    - Wert für **Datenbank**eingeben. Beispielsweise `nwind`.
    - Wert für **Authentifizierung**auswählen. **Beispielsweise auswählen.**
    - Geben Sie Wert für **Benutzername**. Beispielsweise `db2admin`.
    - Geben Sie Wert **ein**. Beispielsweise `Password1`.
    - Wert für **Gateway**auswählen. Beispiel: **datagateway01**.
7. Wählen Sie **Erstellen**und dann **Speichern**. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  Blatt **Db2getTables** in der Liste **Alle** unter **Zusammenfassung**wählen Sie zuerst aufgeführt (die letzte Ausführung).
9.  Wählen Sie im Blatt **Logik app ausführen** **Testlaufdetails**. Wählen Sie in der Liste **Aktion** **Get_tables**. Siehe den Wert **Status** **erfolgreich**sein sollten. Wählen Sie die **Eingaben Link** Eingaben anzeigen. Wählen Sie **Verknüpfung gibt**und zeigen Sie die Ausgaben an; die Liste der Tabellen enthalten soll.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Erstellen der Verbindung
Dieser Connector unterstützt Verbindungen zu Datenbanken lokal und in der Cloud Die folgenden Verbindungseigenschaften. 

Eigenschaft | Beschreibung
--- | ---
Server | Erforderlich. Akzeptiert einen Zeichenfolgenwert, der eine TCP-Portnummer ein TCP/IP-Adresse oder Alias, IPv4 oder IPv6-Format folgen (Doppelpunkt getrennt) darstellt. 
Datenbank | Erforderlich. Akzeptiert einen Zeichenfolgenwert, der eine DRDA relationalen Datenbank Name (RDBNAM) darstellt. DB2 für Z/OS akzeptiert eine 16-Byte-Zeichenfolge (Datenbank ist als ein IBM DB2 Z/OS-Speicherort bezeichnet). DB2 für i5/OS akzeptiert eine Zeichenfolge 18 Byte (Datenbank nennt eine IBM DB2 für i relationalen Datenbank). DB2 LUW für akzeptiert eine 8-Byte-Zeichenfolge.
Authentifizierung | Optional. Akzeptiert einen Liste Elementwert Basic oder Windows (Kerberos). 
Benutzername | Erforderlich. Nimmt einen String-Wert. DB2 für Z/OS akzeptiert eine 8-Byte-Zeichenfolge. DB2 für i 10-Byte-Zeichenfolge akzeptiert. DB2 für Linux oder UNIX akzeptiert eine 8-Byte-Zeichenfolge. DB2 für Windows akzeptiert eine Zeichenfolge 30 Byte.
Kennwort | Erforderlich. Nimmt einen String-Wert.
Gateway | Erforderlich. Nimmt einen Element Listenwert für lokale datengateway definierten Logik-Apps in der Speichergruppe darstellt.  

## <a name="create-the-on-premises-gateway-connection"></a>Das lokale Gateway-Verbindung erstellen
Dieser Connector kann eine lokale DB2-Datenbank mit dem lokalen Gateway zugreifen. Siehe Gateway finden Sie weitere Informationen. 

1. Wählen Sie im Konfigurationsbereich **Gateways** **Kontrollkästchen** **Verbindung Gateway**aktivieren. Beachten Sie, dass die Cloud lokalen einstellen.
2. Geben Sie Wert für **Server**in Form von Adresse oder Alias Doppelpunkt Portnummer. Beispielsweise `ibmserver01:50000`.
3. Wert für **Datenbank**eingeben. Beispielsweise `nwind`.
4. Wert für **Authentifizierung**auswählen. **Beispielsweise auswählen.**
5. Geben Sie Wert für **Benutzername**. Beispielsweise `db2admin`.
6. Geben Sie Wert **ein**. Beispielsweise `Password1`.
7. Wert für **Gateway**auswählen. Beispiel: **datagateway01**.
8. Wählen Sie **Erstellen** möchten. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Erstellen Sie die cloudverbindung
Dieser Connector kann eine Cloud DB2-Datenbank zugreifen. 

1. Im Konfigurationsbereich **Gateways** deaktiviert lassen Sie das **Kontrollkästchen** **Verbindung Gateway**(auf). 
2. Geben Sie Wert für **Name der Verbindung**. Beispielsweise `hisdemo2`.
3. Geben Sie Wert für **DB2-Servernamen**aus Adresse oder Alias Doppelpunkt Portnummer. Beispielsweise `hisdemo2.cloudapp.net:50000`.
3. Geben Sie Wert für **Name der DB2-Datenbank**. Beispielsweise `nwind`.
4. Geben Sie Wert für **Benutzername**. Beispielsweise `db2admin`.
5. Geben Sie Wert **ein**. Beispielsweise `Password1`.
6. Wählen Sie **Erstellen** möchten. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Zeilen Sie alle mit SELECT
Sie können eine logische app-Aktion zum Abrufen aller Zeilen in einer DB2-Tabelle definieren. Dies weist den Konnektor DB2 SELECT-Anweisung wie zu `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik
1.  **Azure starten Board**, wählen **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2.  Geben Sie den **Namen**wie `Db2getRows`, **Abonnements**, **Ressourcengruppe**, **Speicherort**und **App Service-Plan**. Wählen Sie **Dashboard**, und wählen Sie **Erstellen**.

### <a name="add-a-trigger-and-action"></a>Trigger und Aktion hinzufügen
1.  Wählen Sie **Leere LogicApp** **Logik Apps Designer**in der Liste **Vorlagen** .
2.  Wählen Sie in der Liste **Trigger** **Wiederholung**. 
3.  Im Trigger **Serie** **Bearbeiten**wählen Sie aus wählen Sie **Häufigkeit** Dropdown- **Tag**auswählen und wählen Sie **Intervall** **7**eingeben. 
4.  Aktivieren Sie das **+ Neuer Schritt** , und wählen Sie **eine Aktion hinzufügen**.
5.  Geben Sie in der Liste **Aktionen** `db2` **Suche weitere Aktionen** Bearbeitungsfeld und **DB2 - Zeilen (Vorschau)**klicken.
6. Wählen Sie in der Aktion **werden Zeilen (Vorschau)** **Änderung Verbindung**.
7. Wählen Sie im Konfigurationsbereich **Verbindung** **neu erstellen**. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. Im Konfigurationsbereich **Gateways** deaktiviert lassen Sie das **Kontrollkästchen** **Verbindung Gateway**(auf).
    - Geben Sie Wert für **Name der Verbindung**. Beispielsweise `HISDEMO2`.
    - Geben Sie Wert für **DB2-Servernamen**aus Adresse oder Alias Doppelpunkt Portnummer. Beispielsweise `HISDEMO2.cloudapp.net:50000`.
    - Geben Sie Wert für **Name der DB2-Datenbank**. Beispielsweise `NWIND`.
    - Geben Sie Wert für **Benutzername**. Beispielsweise `db2admin`.
    - Geben Sie Wert **ein**. Beispielsweise `Password1`.
9. Wählen Sie **Erstellen** möchten.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten**, und wählen Sie **Bereich**.
11. Optional Wählen Sie **Erweiterte Optionen anzeigen** Abfrageoptionen festlegen.
12. **Wählen Sie aus.** 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. Blatt **Db2getRows** in der Liste **Alle** unter **Zusammenfassung**wählen Sie zuerst aufgeführt (die letzte Ausführung).
14. Wählen Sie im Blatt **Logik app ausführen** **Testlaufdetails**. Wählen Sie in der Liste **Aktion** **Get_rows**. Siehe den Wert **Status** **erfolgreich**sein sollten. Wählen Sie die **Eingaben Link** Eingaben anzeigen. Wählen Sie **Verknüpfung gibt**und zeigen Sie die Ausgaben an; die Liste der Zeilen enthalten soll.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Fügen Sie eine Zeile mit INSERT
Sie können eine logische app Aktion um eine DB2-Tabelle eine Zeile hinzuzufügen. Diese Aktion weist den Konnektor DB2-INSERT-Anweisung z. B. zu `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik
1.  **Azure starten Board**, wählen **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2.  Geben Sie den **Namen**wie `Db2insertRow`, **Abonnements**, **Ressourcengruppe**, **Speicherort**und **App Service-Plan**. Wählen Sie **Dashboard**, und wählen Sie **Erstellen**.

### <a name="add-a-trigger-and-action"></a>Trigger und Aktion hinzufügen
1.  Wählen Sie **Leere LogicApp** **Logik Apps Designer**in der Liste **Vorlagen** .
2.  Wählen Sie in der Liste **Trigger** **Wiederholung**. 
3.  Im Trigger **Serie** **Bearbeiten**wählen Sie aus wählen Sie **Häufigkeit** Dropdown- **Tag**auswählen und wählen Sie **Intervall** **7**eingeben. 
4.  Aktivieren Sie das **+ Neuer Schritt** , und wählen Sie **eine Aktion hinzufügen**.
5.  Geben Sie in der Liste **Aktionen** **db2** im Bearbeitungsfeld **Suchen Weitere Aktionen** , und wählen Sie **DB2 - Zeile einfügen (Vorschau)**.
6. Wählen Sie in der Aktion **werden Zeilen (Vorschau)** **Änderung Verbindung**. 
7. Wählen Sie im Konfigurationsbereich **Verbindungen** eine Verbindung. Beispiel: **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten**, und wählen Sie **Bereich**.
9. Geben Sie Werte für alle erforderlichen Spalten (Siehe Sternchen). Beispielsweise `99999` **AREAID**geben `Area 99999`, und `102` für **REGIONID**. 
10. **Wählen Sie aus.**

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. Blatt **Db2insertRow** in der Liste **Alle** unter **Zusammenfassung**wählen Sie zuerst aufgeführt (die letzte Ausführung).
12. Wählen Sie im Blatt **Logik app ausführen** **Testlaufdetails**. Wählen Sie in der Liste **Aktion** **Get_rows**. Siehe den Wert **Status** **erfolgreich**sein sollten. Wählen Sie die **Eingaben Link** Eingaben anzeigen. Wählen Sie **Verknüpfung gibt**und zeigen Sie die Ausgaben an; die neue Zeile aufzunehmen.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Rufen Sie wählen Sie eine Zeile ab
Sie können eine logische app-Aktion zum Abrufen einer Zeile in einer DB2-Tabelle definieren. Diese Aktion weist den Konnektor zur Verarbeitung einer Anweisung DB2 auswählen, z. B. `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik
1.  **Azure starten Board**, wählen **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2.  Geben Sie den **Namen** (z.B. "**Db2getRow**"), **Abonnement** **Ressourcengruppe**, **Speicherort**und **App Service-Plan**. Wählen Sie **Dashboard**, und wählen Sie **Erstellen**.

### <a name="add-a-trigger-and-action"></a>Trigger und Aktion hinzufügen
1.  Wählen Sie **Leere LogicApp** **Logik Apps Designer**in der Liste **Vorlagen** . 
2.  Wählen Sie in der Liste **Trigger** **Wiederholung**. 
3.  Im Trigger **Serie** **Bearbeiten**wählen Sie aus wählen Sie **Häufigkeit** Dropdown- **Tag**auswählen und wählen Sie **Intervall** **7**eingeben. 
4.  Aktivieren Sie das **+ Neuer Schritt** , und wählen Sie **eine Aktion hinzufügen**.
5.  Geben Sie in der Liste **Aktionen** **db2** im Bearbeitungsfeld **Suchen Weitere Aktionen** , und wählen Sie **DB2 - Zeilen (Vorschau)**.
6. Wählen Sie in der Aktion **werden Zeilen (Vorschau)** **Änderung Verbindung**. 
7. Wählen Sie im Bereich Konfigurationen **Verbindungen** eine Verbindung. Beispiel: **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten**, und wählen Sie **Bereich**.
9. Geben Sie Werte für alle erforderlichen Spalten (Siehe Sternchen). Beispielsweise `99999` für **AREAID**. 
10. Optional Wählen Sie **Erweiterte Optionen anzeigen** Abfrageoptionen festlegen.
11. **Wählen Sie aus.** 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. Blatt **Db2getRow** in der Liste **Alle** unter **Zusammenfassung**wählen Sie zuerst aufgeführt (die letzte Ausführung).
13. Wählen Sie im Blatt **Logik app ausführen** **Testlaufdetails**. Wählen Sie in der Liste **Aktion** **Get_rows**. Siehe den Wert **Status** **erfolgreich**sein sollten. Wählen Sie die **Eingaben Link** Eingaben anzeigen. Wählen Sie **Verknüpfung gibt**und zeigen Sie die Ausgaben an; die Zeile aufzunehmen.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Ändern Sie eine Zeile mit UPDATE
Sie können eine logische app Aktion ändern eine Zeile in einer DB2-Tabelle definieren. Diese Aktion weist den Konnektor DB2 UPDATE-Anweisung wie zu `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik
1.  **Azure starten Board**, wählen **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2.  Geben Sie den **Namen**wie `Db2updateRow`, **Abonnements**, **Ressourcengruppe**, **Speicherort**und **App Service-Plan**. Wählen Sie **Dashboard**, und wählen Sie **Erstellen**.

### <a name="add-a-trigger-and-action"></a>Trigger und Aktion hinzufügen
1.  Wählen Sie **Leere LogicApp** **Logik Apps Designer**in der Liste **Vorlagen** .
2.  Wählen Sie in der Liste **Trigger** **Wiederholung**. 
3.  Im Trigger **Serie** **Bearbeiten**wählen Sie aus wählen Sie **Häufigkeit** Dropdown- **Tag**auswählen und wählen Sie **Intervall** **7**eingeben. 
4.  Aktivieren Sie das **+ Neuer Schritt** , und wählen Sie **eine Aktion hinzufügen**.
5.  Geben Sie in der Liste **Aktionen** **db2** im Bearbeitungsfeld **Suchen Weitere Aktionen** , und wählen Sie **DB2 - Update-Zeile (Vorschau)**.
6. Wählen Sie in der Aktion **werden Zeilen (Vorschau)** **Änderung Verbindung**. 
7. Wählen Sie im Bereich Konfigurationen **Verbindungen** eine Verbindung auswählen. Beispiel: **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten**, und wählen Sie **Bereich**.
9. Geben Sie Werte für alle erforderlichen Spalten (Siehe Sternchen). Beispielsweise `99999` **AREAID**geben `Updated 99999`, und `102` für **REGIONID**. 
10. **Wählen Sie aus.** 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. Blatt **Db2updateRow** in der Liste **Alle** unter **Zusammenfassung**wählen Sie zuerst aufgeführt (die letzte Ausführung).
12. Wählen Sie im Blatt **Logik app ausführen** **Testlaufdetails**. Wählen Sie in der Liste **Aktion** **Get_rows**. Siehe den Wert **Status** **erfolgreich**sein sollten. Wählen Sie die **Eingaben Link** Eingaben anzeigen. Wählen Sie **Verknüpfung gibt**und zeigen Sie die Ausgaben an; die neue Zeile aufzunehmen.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Entfernen Sie eine Zeile mit löschen
Sie können eine logische app Aktion um eine Zeile in einer DB2-Tabelle entfernen. Diese Aktion weist den Konnektor an DB2 löschen Verarbeitung einer Anweisung, wie `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Erstellen einer Logik
1.  **Azure starten Board**, wählen **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2.  Geben Sie den **Namen**wie `Db2deleteRow`, **Abonnements**, **Ressourcengruppe**, **Speicherort**und **App Service-Plan**. Wählen Sie **Dashboard**, und wählen Sie **Erstellen**.

### <a name="add-a-trigger-and-action"></a>Trigger und Aktion hinzufügen
1.  Wählen Sie **Leere LogicApp** **Logik Apps Designer**in der Liste **Vorlagen** . 
2.  Wählen Sie in der Liste **Trigger** **Wiederholung**. 
3.  Im Trigger **Serie** **Bearbeiten**wählen Sie aus wählen Sie **Häufigkeit** Dropdown- **Tag**auswählen und wählen Sie **Intervall** **7**eingeben. 
4.  Aktivieren Sie das **+ Neuer Schritt** , und wählen Sie **eine Aktion hinzufügen**.
5.  Wählen Sie in der Liste **Aktionen** **db2** im Bearbeitungsfeld **Suchen Weitere Aktionen** , und wählen Sie die **DB2 - Zeile (Vorschau) löschen**.
6. Wählen Sie in der Aktion **werden Zeilen (Vorschau)** **Änderung Verbindung**. 
7. Wählen Sie im Bereich Konfigurationen **Verbindungen** eine Verbindung. Beispiel: **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Wählen Sie in der Liste **Tabellenname** den **Pfeil nach unten**, und wählen Sie **Bereich**.
9. Geben Sie Werte für alle erforderlichen Spalten (Siehe Sternchen). Beispielsweise `99999` für **AREAID**. 
10. **Wählen Sie aus.** 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. Blatt **Db2deleteRow** in der Liste **Alle** unter **Zusammenfassung**wählen Sie zuerst aufgeführt (die letzte Ausführung).
12. Wählen Sie im Blatt **Logik app ausführen** **Testlaufdetails**. Wählen Sie in der Liste **Aktion** **Get_rows**. Siehe den Wert **Status** **erfolgreich**sein sollten. Wählen Sie die **Eingaben Link** Eingaben anzeigen. Wählen Sie **Verknüpfung gibt**und zeigen Sie die Ausgaben an; die gelöschte Zeile aufzunehmen.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Technische Details

## <a name="actions"></a>Aktionen
Eine Aktion ist eine Operation in eine Logik-app definierten Workflow durchgeführt. DB2-Datenbank-Verbindung umfasst die folgenden Aktionen. 

|Aktion|Beschreibung|
|--- | ---|
|[[GetRow](connectors-create-api-db2.md#get-row)|Ruft eine einzelne Zeile aus einer DB2-Tabelle|
|[GetRows](connectors-create-api-db2.md#get-rows)|Ruft Zeilen aus einer DB2-Tabelle|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Fügt eine neue Zeile in eine DB2-Tabelle|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|Löscht eine Zeile aus einer DB2-Tabelle|
|[GetTables](connectors-create-api-db2.md#get-tables)|Tabellen aus einer DB2-Datenbank abgerufen|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Aktualisiert eine vorhandene Zeile in einer DB2-Tabelle|

### <a name="action-details"></a>Aktionsdetails

Finden Sie in diesem Abschnitt Einzelheiten zu jeder Aktion sowie alle erforderlichen oder optionalen Eingabeeigenschaften entsprechende Ausgabe der Connector zugeordnet.

#### <a name="get-row"></a>Zeile erhalten 
Ruft eine einzelne Zeile aus einer DB2-Tabelle ab.  

| Eigenschaftenname| Angezeigter Name |Beschreibung|
| ---|---|---|
|Tabelle * | Tabellenname |Name der DB2-Tabelle|
|ID * | Zeilen-id |Eindeutiger Bezeichner des abzurufenden Zeile|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Artikel

| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|


#### <a name="get-rows"></a>Abrufen von Zeilen 
Ruft Zeilen aus einer DB2-Tabelle ab.  

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Tabellenname|Name der DB2-Tabelle|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|
|$filter|Filterabfrage|Eine Filterabfrage ODATA beschränkt die Anzahl der Einträge|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|


#### <a name="insert-row"></a>Zeile einfügen 
Eine DB2-Tabelle eine neue Zeile eingefügt.  

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Tabellenname|Name der DB2-Tabelle|
|Artikel *|Zeile|Zeile in der angegebenen Tabelle in DB2 einfügen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Artikel

| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|


#### <a name="delete-row"></a>Zeile löschen 
Löscht eine Zeile aus einer DB2-Tabelle.  

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Tabellenname|Name der DB2-Tabelle|
|ID *|Zeilen-id|Eindeutiger Bezeichner der zu löschenden Zeile|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.

#### <a name="get-tables"></a>Abrufen von Tabellen 
Tabellen aus einer DB2-Datenbank abgerufen.  

Es sind keine Parameter für diesen Aufruf. 

##### <a name="output-details"></a>Ausgabedetails 
TablesList

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|

#### <a name="update-row"></a>Zeile aktualisieren 
Aktualisiert eine vorhandene Zeile in einer DB2-Tabelle.  

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Tabellenname|Name der DB2-Tabelle|
|ID *|Zeilen-id|Eindeutiger Bezeichner der zu aktualisierenden Zeile|
|Artikel *|Zeile|Zeile mit aktualisierten Werte|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails  
Artikel

| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|


### <a name="http-responses"></a>HTTP-Antworten

Verschiedenen Aktionen aufrufen, können Sie bestimmte Antworten erhalten. Die folgende Tabelle enthält die Antworten und deren Beschreibung:  

|Name|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="supported-db2-platforms-and-versions"></a>DB2-Plattformen und Versionen
Dieser Connector unterstützt die folgenden IBM DB2-Plattformen und Versionen sowie IBM DB2 kompatible Produkte (z. B. IBM Bluemix DashDB), verteilte relationale Datenbank-Architektur (DRDA) SQL Access Manager (SQLAM) Version 10 und 11:

- IBM DB2 für Z/OS 11.1
- IBM DB2 für Z/OS 10.1
- IBM DB2 für i 7.3
- IBM DB2 für i 7.2
- IBM DB2 für i 7.1
- IBM DB2 LUW 11
- IBM DB2 LUW 10.5


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Durchsuchen der verfügbaren Connectors Logik Apps [APIs Liste](apis-list.md).


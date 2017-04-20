<properties
   pageTitle="Mit DB2-Connector in Microsoft Azure App Service | Microsoft Azure"
   description="Verwendung den DB2-Connector mit Logik app Auslöser und Aktionen"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="db2-connector"></a>DB2-connector
>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion.

Microsoft-Connector für DB2 ist ein API-app für eine IBM DB2-Datenbank gespeicherten Ressourcen Applikationen über Azure App mit. Connector enthält einen Microsoft Client in einem TCP/IP-Netzwerk eine Verbindung zu Remotecomputern DB2-Server, einschließlich Azure hybridverbindungen an lokalen DB2 Server Azure Service Bus Relay-Connector unterstützt die folgenden Datenbankoperationen:

- Lesen Sie wählen Sie Zeilen
- Umfrage zum Lesen von Zeilen mit SELECT COUNT gefolgt von auswählen
- Fügen Sie eine Zeile oder mehrere (Bulk) Zeilen mit INSERT
- Ändern Sie eine Zeile oder mehrere (Bulk) Zeilen mit UPDATE
- Entfernen Sie eine Zeile oder mehrere (Bulk) Zeilen mit
- Lesen mit CURSOR wählen gefolgt von UPDATE WHERE CURRENT OF CURSOR ändern
- Lesen mit CURSOR wählen Sie UPDATE WHERE CURRENT OF CURSOR folgt Zeilen entfernen
- Führen Sie die Prozedur mit Eingabe-und Ausgabeparameter, Rückgabewert Resultset mit
- Benutzerdefinierte Befehle und zusammengesetzte Operationen mit SELECT, INSERT, UPDATE, DELETE

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Connector unterstützt folgende Logik app Auslöser und Aktionen:

Trigger | Aktionen
--- | ---
<ul><li>Daten abrufen</li></ul> | <ul><li>Bulk Insert</li><li>Einfügen</li><li>Bulk Update</li><li>Update</li><li>Aufrufen</li><li>BULK löschen</li><li>Löschen</li><li>Wählen Sie</li><li>Bedingte Aktualisierung</li><li>An EntitySet</li><li>Bedingte löschen</li><li>Wählen Sie Einheit</li><li>Löschen</li><li>Upsert zum EntitySet</li><li>Benutzerdefinierte Befehle</li><li>Zusammengesetzte Operationen</li></ul>


## <a name="create-the-db2-connector"></a>DB2-Connector erstellen
Sie können einen Connector Logik App oder Azure Marketplace wie im folgenden Beispiel definieren:  

1. Wählen Sie in Azure Startmenü **Marketplace**.
2. Blatt **Alles** Geben Sie **db2** im Feld **Suche alles** , und klicken Sie dann auf die EINGABETASTE.
3. Die Suche ergibt alles Bereich, **DB2 Verbinder**auswählen.
4. Wählen Sie Blatt Beschreibung DB2 Connector **Erstellen**.
5. Blatt Paket DB2 Connector Geben Sie den Namen (z.B. "Db2ConnectorNewOrders"), App Service-Plan und andere Eigenschaften.
6. Bearbeiten Sie **Paket**, und geben Sie die folgenden Paket:  

    Name | Erforderlich |  Beschreibung
--- | --- | ---
ConnectionString | Ja | DB2 Clientverbindungszeichenfolge (z. B. "Network Address = Servername; Netzwerk-Port = 50000; Benutzer-ID = Benutzername; Password = Kennwort; Initial Catalog = Probe; Paket Auflistung = NWIND; Standard Schema NWIND = ").
Tabellen | Ja | Durch Kommas getrennte Liste der Tabelle, Ansicht und alias-Namen für OData-Operationen und generieren swagger Dokumentation mit Beispielen (z. B. "*NEWORDERS*").
Verfahren | Ja | Durch Kommas getrennte Liste von Prozedur- und Namen (z.B. "SPORDERID").
Lokale Edition | Nein | Bereitstellen von lokalen Azure Service Bus Relay verwenden.
ServiceBusConnectionString | Nein | Azure Service Bus Relay-Verbindungszeichenfolge.
PollToCheckData | Nein | SELECT COUNT-Anweisung mit Logik app Trigger (z. B. "SELECT COUNT (\*) von NEWORDERS WHERE Lieferdatum IS NULL").
PollToReadData | Nein | SELECT-Anweisung mit einer Logik app Trigger (z. B. "Wählen Sie \* aus NEWORDERS, Lieferdatum NULL für Aktualisierung ist").
PollToAlterData | Nein | Update- oder DELETE-Anweisung mit einer Logik app Trigger (z. B. "UPDATE NEWORDERS legen Lieferdatum = aktuelle Datum WHERE CURRENT OF &lt;CURSOR&gt;").

7. Wählen Sie **OK**und dann **Erstellen**.
8. Nach Abschluss des Vorgangs ähneln dem Paket Folgendes:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Logik-app mit DB2 Connector Daten hinzufügen ##
Sie können eine logische app Aktion eine Entität OData Vorgang API einfügen oder Post mit DB2-Tabelle Daten hinzuzufügen. Beispielsweise können Sie einen neuer Auftrag Kundendatensatz durch Einfügen verarbeitet eine SQL INSERT-Anweisung für eine Tabelle mit einer Identitätsspalte definiert den Identitätswert oder Logik App (ORDID vom letzten Tabelle auswählen (Einfügen in NWIND betroffenen Zeilen zurückgibt. NEWORDERS (CUSTID LIEFERNAME AUFGENOMMENE PERSON, LIEFERORT, SHIPREG, SHIPZIP) WERTE (?,?,?,?,?,?))).

> [AZURE.TIP] DB2-Verbindung "*an EntitySet*" Wert für die Identitätsspalte und zurückgibt "*API einfügen*" Zeilen betroffen

1. Wählen Sie in Azure Startmenü **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2. Geben Sie den Namen (z.B. "NewOrdersDb2"), App Service-Plan, andere Eigenschaften, und wählen Sie **Erstellen**.
3. Wählen Sie in Azure Startmenü die Logik app erstellten, **Standardeinstellungen**und **Trigger und Aktionen**.
4. Wählen Sie Blatt Auslöser und Aktionen Logik App Vorlagen **Erstellen** .
5. Wählen Sie im Bereich API-Apps **Wiederholung**, eine Frequenz und Intervall und **Markierung**festgelegt.
6. Wählen Sie im Bereich API-Apps **DB2 Verbinder aus**, erweitern Sie die Liste Vorgänge **NEWORDER einfügen**wählen.
7. Erweitern Sie die Parameterliste folgende Werte eingeben:  

    Name | Wert
--- | --- 
CUSTID | 10042
LIEFERNAME | Verzögerte K Kountry Speicher 
AUFGENOMMENE PERSON | 12 Orchester Terrasse
LIEFERORT | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Wählen Sie das **Häkchen** zu der Einstellung, und **Speichern**.
9. Die Einstellung sollte wie folgt aussehen:  
![][3]

10. Wählen Sie in der Liste **Alle** unter **Vorgänge**zuerst aufgeführt (die letzte Ausführung). 
11. Wählen Sie im Blatt **Logik app ausgeführt** **Aktion** Element **db2connectorneworders**.
12. Wählen Sie Blatt **logische app Aktion** **EINGABEN LINK**. DB2-Connector verwendet die Eingaben eine parametrisierte INSERT-Anweisung verarbeitet.
13. Wählen Sie Blatt **logische app Aktion** **Ausgaben verknüpfen**. Eingaben sollten wie folgt aussehen:  
![][4]

#### <a name="what-you-need-to-know"></a>Was Sie wissen müssen

- Connector kürzt DB2 Tabellennamen bei Logik app Aktionsnamen. **Einfügen NEWORDERS** Vorgang wird z. B. in **NEWORDER**abgeschnitten.
- Nach dem Speichern der Anwendung Logik **Auslöser und Aktionen**verarbeitet Logik app den Vorgang. Möglicherweise eine Verzögerung eine Anzahl von Sekunden (z. B. 3 bis 5 Sekunden) Logik app verarbeitet den Vorgang. Optional können Sie **Jetzt ausführen,** um den Vorgang zu verarbeiten klicken.
- DB2 Connector definiert EntitySet Member Attribute, z. B. ob der Member einer DB2-Spalte mit einem Standardwert entspricht oder Spalten (z. B. Identity) generiert. Logik-app zeigt ein rotes Sternchen neben dem EntitySet ID Membernamen, DB2 Spalten anzugeben, die Werte erfordern. Sie sollten keinen Wert eingeben, die für den Member ORDID DB2 Identitätsspalte entspricht. Sie können Werte für andere optionale Elemente (Elemente, ORDDATE, REQDATE, SHIPID, Fracht, SHIPCTRY) eingeben die DB2-Spalten mit Standardwerten entsprechen. 
- DB2 Connector gibt Logik App die Antwort auf auf EntitySet, die enthält die Werte für Identitätsspalten DRDA SQLDARD (Antwort auf SQL Daten Bereich) abgeleitet wird auf die vorbereitete SQL INSERT-Anweisung. DB2-Server kehrt die Werte für Spalten mit Standardwerten.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Logik-app mit DB2 Connector Daten hinzufügen ##
Sie definieren eine logische app Aktion eine API Bulk Insert-Operation eine DB2-Tabelle Daten hinzuzufügen. Beispielsweise können Sie zwei neue Reihenfolge Kundendatensätze, durch Einfügen verarbeitet eine SQL INSERT-Anweisung mit einem Array der Werte für eine Tabelle mit einer Identitätsspalte Logik App (ORDID vom letzten Tabelle auswählen (Einfügen in NWIND betroffenen Zeilen zurückgeben. NEWORDERS (CUSTID LIEFERNAME AUFGENOMMENE PERSON, LIEFERORT, SHIPREG, SHIPZIP) WERTE (?,?,?,?,?,?))).

1. Wählen Sie in Azure Startmenü **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2. Geben Sie den Namen (z.B. "NewOrdersBulkDb2"), App Service-Plan, andere Eigenschaften, und wählen Sie **Erstellen**.
3. Wählen Sie in Azure Startmenü die Logik app erstellten, **Standardeinstellungen**und **Trigger und Aktionen**.
4. Wählen Sie Blatt Auslöser und Aktionen Logik App Vorlagen **Erstellen** .
5. Wählen Sie im Bereich API-Apps **Wiederholung**, eine Frequenz und Intervall und **Markierung**festgelegt.
6. Wählen Sie im Bereich API-Apps **DB2 Verbinder aus**, erweitern Sie die Liste Vorgänge **Bulk Insert in neu**aktivieren.
7. Geben Sie **Zeilen** als Array. Zum Beispiel kopieren Sie und fügen Sie die folgenden ein:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
    ```

8. Wählen Sie das **Häkchen** zu der Einstellung, und **Speichern**. Die Einstellung sollte wie folgt aussehen:  
![][6]

9. Klicken Sie in der Liste **Alle** unter **Vorgänge**zuerst aufgeführt (die letzte Ausführung).
10. Klicken Sie auf **den Aktionspunkt** Blatt **Logik app ausgeführt** .
11. Das Blade **logische app Aktion** klicken Sie auf **EINGABEN LINK**. Die Ergebnisse sollten wie folgt aussehen:  
[][7]
12. Das Blade **logische app Aktion** klicken Sie auf **Ausgaben verknüpfen**. Die Ergebnisse sollten wie folgt aussehen:  
![][8]

#### <a name="what-you-need-to-know"></a>Was Sie wissen müssen

- Connector kürzt DB2 Tabellennamen bei Logik app Aktionsnamen. Beispielsweise ist **Bulk Insert in NEWORDERS** Vorgang **Bulk Insert in neue**abgeschnitten.
- DB2-Datenbank durch Auslassen Identitätsspalten (z.B. ORDID), Spalten NULL-Werte zulässt (z. B. Lieferdatum) und Spalten mit Standardwerten (ORDDATE, REQDATE, SHIPID, Fracht, SHIPCTRY), Werte generiert.
- Indem "heute" und "morgen" DB2 Connector generiert "Datum" und "Aktuelle Datum + 1 Tag" Funktionen (z.B. REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Logik-app mit DB2 Connector Trigger lesen, ändern oder Löschen von Daten ##
Sie können Trigger Logik app abrufen und Lesen von Daten aus einer DB2-Tabelle mit einer API-Umfrage zusammengesetzten Datenoperation definieren. Angenommen, Sie erhalten eine oder mehrere neue Reihenfolge Datensätze, Datensätze nach Anwendung Logik. Die Paket-app DB2 Verbindungszeichenfolge sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) von NEWORDERS Lieferdatum NULL ist
PollToReadData | Wählen Sie \* aus NEWORDERS Lieferdatum NULL für Aktualisierung ist
PollToAlterData | <no value specified>


Außerdem können Sie Logik app Trigger abzurufen, lesen und ändern Sie Daten in einer DB2-Tabelle mit API-Umfragedaten zusammengesetzte Operationen definieren. Angenommen, Sie erhalten eine oder mehrere neue Reihenfolge Datensätze, Zeilenwerte Logik app (vor Aktualisierung) ausgewählte Datensätze nach aktualisieren. Die Paket-app DB2 Verbindungszeichenfolge sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) von NEWORDERS Lieferdatum NULL ist
PollToReadData | Wählen Sie \* aus NEWORDERS Lieferdatum NULL für Aktualisierung ist
PollToAlterData | UPDATE NEWORDERS SET SHIPDATE = aktuelle Datum, aktuelle von &lt;CURSOR&gt;


Darüber hinaus können Sie Trigger Logik app abrufen, lesen und Entfernen von Daten aus einer DB2-Tabelle mit einer API-Umfrage zusammengesetzten Datenoperation definieren. Angenommen, Sie erhalten eine oder mehrere neue Reihenfolge Datensätze, löschen Sie die Zeilen Logik app (vor löschen) ausgewählte Datensätze nach. Die Paket-app DB2 Verbindungszeichenfolge sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) von NEWORDERS Lieferdatum NULL ist
PollToReadData | Wählen Sie \* aus NEWORDERS Lieferdatum NULL für Aktualisierung ist
PollToAlterData | Löschen NEWORDERS, in der aktuellen &lt;CURSOR&gt;

In diesem Beispiel wird Logik app Umfrage lesen, aktualisieren und Daten in DB2-Tabelle erneut lesen.

1. Wählen Sie in Azure Startmenü **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2. Geben Sie den Namen (z.B. "ShipOrdersDb2"), App Service-Plan, andere Eigenschaften, und wählen Sie **Erstellen**.
3. Wählen Sie in Azure Startmenü die Logik app erstellten, **Standardeinstellungen**und **Trigger und Aktionen**.
4. Wählen Sie Blatt Auslöser und Aktionen Logik App Vorlagen **Erstellen** .
5. Wählen Sie im Bereich API-Apps **DB2 Verbinder aus**, legen Sie eine Häufigkeit und Intervall und **Häkchen**.
6. Wählen Sie im Bereich API-Apps **DB2 Verbinder aus**, erweitern Sie die Liste Vorgänge zum auswählen **von NEWORDERS**.
7. Wählen Sie das **Häkchen** zu der Einstellung, und **Speichern**. Die Einstellung sollte wie folgt aussehen:  
![][10]  
8. Klicken Sie, um das Blade **Auslöser und Aktionen** schließen und dann auf Blatt **Einstellungen** schließen.
9. Klicken Sie in der Liste **Alle** unter **Vorgänge**zuerst aufgeführt (die letzte Ausführung).
10. Klicken Sie auf **den Aktionspunkt** Blatt **Logik app ausgeführt** .
11. Das Blade **logische app Aktion** klicken Sie auf **Ausgaben verknüpfen**. Die Ergebnisse sollten wie folgt aussehen:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Logik-app mit DB2 Connector Daten entfernen ##
Sie können eine logische app Aktion Entfernen von Daten aus einer Entität OData-Operation mit einer API löschen oder Post DB2-Tabelle definieren. Beispielsweise können Sie einen neuer Auftrag Kundendatensatz durch Einfügen verarbeitet eine SQL INSERT-Anweisung für eine Tabelle mit einer Identitätsspalte definiert den Identitätswert oder Logik App (ORDID vom letzten Tabelle auswählen (Einfügen in NWIND betroffenen Zeilen zurückgibt. NEWORDERS (CUSTID LIEFERNAME AUFGENOMMENE PERSON, LIEFERORT, SHIPREG, SHIPZIP) WERTE (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Erstellen Sie Logik mit DB2 Connector Daten entfernen ##
Sie können eine neue Logik-app in Azure Marketplace erstellen und verwenden Sie DB2-Connector als Aktion Kundenaufträge entfernen. Beispielsweise können Sie bedingten Löschvorgang von DB2 Connector eine SQL DELETE-Anweisung verarbeitet (Löschen von NEWORDERS, ORDID > = 10000).

1. Klicken Sie im Hub Verwaltungsrat Azure **Starten** auf **+** (Pluszeichen) auf **Web + Mobile**und dann auf **app Logik**. 
2. Geben Sie einen **Namen**, z. B. **RemoveOrdersDb2**Blatt **app Logik erstellen** .
3. Wählen Sie oder definieren Sie Werte für die anderen Einstellungen (Serviceplan, Ressourcengruppe).
4. Die Einstellung sollte wie folgt aussehen. Klicken Sie auf **Erstellen**:  
![][12]  
5. Klicken Sie auf Blatt **Einstellungen** **Auslöser und Aktionen**.
6. Blatt **Auslöser und Aktionen** in der Liste **Logik app Vorlagen** klicken Sie auf **neu erstellen**.
7. Klicken Sie auf **Serientyp**, Blatt **Auslöser und Aktionen** im Bereich **API-Apps** in der Ressourcengruppe.
8. Auf der Entwurfsoberfläche der Anwendung Logik auf **Serie** Element, **Intervall** und **Intervall**, zum Beispiel **Tage** und **1**festgelegt und dann auf **Markierung** um die Serie Einstellungen speichern.
9. Klicken Sie auf **DB2 Connector**Blatt **Auslöser und Aktionen** im Bereich **API-Apps** in der Ressourcengruppe.
10. Klicken Sie auf der Entwurfsoberfläche Logik app auf das Aktionselement **DB2 Connector** , klicken Sie auf die Auslassungszeichen****um die Vorgänge erweitern und klicken Sie **bedingte n**.
11. Geben Sie für den Aktionspunkt DB2 Connector **ORDID ge 10000** für einen **Ausdruck, der eine Teilmenge von Einträgen bezeichnet**.
12. Klicken Sie auf die **Markierung** um die Einstellung zu speichern und dann auf **Speichern**. Die Einstellung sollte wie folgt aussehen:  
![][13]  
13. Klicken Sie, um das Blade **Auslöser und Aktionen** schließen und dann auf Blatt **Einstellungen** schließen.
14. Klicken Sie in der Liste **Alle** unter **Vorgänge**zuerst aufgeführt (die letzte Ausführung).
15. Klicken Sie auf **den Aktionspunkt** Blatt **Logik app ausgeführt** .
16. Das Blade **logische app Aktion** klicken Sie auf **Ausgaben verknüpfen**. Die Ergebnisse sollten wie folgt aussehen:  
![][14]

**Hinweis:** Logik app Designer kürzt Tabellennamen. **Bedingte löschen NEWORDERS** Vorgang wird z. B. **bedingte**Löschen von N abgeschnitten.


> [AZURE.TIP] Verwenden Sie die folgende SQL-Anweisung die Beispieltabelle und gespeicherten Prozeduren erstellen. 

Sie können die NEWORDERS Beispieltabelle mit folgenden DB2 SQL DDL-Anweisung erstellen:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



Sie können die SPOERID gespeicherte Beispielprozedur mit folgenden DB2 DDL-Anweisung erstellen:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Mischkonfiguration (Optional)

> [AZURE.NOTE] Dieser Schritt ist nur bei Verwendung von DB2 Connector lokal hinter der Firewall erforderlich.

App Service verwendet Hybrid-Konfigurations-Manager das sichere Verbinden mit Ihrem lokalen System. Anschluss einer lokalen IBM DB2 Server-für Windows verwendet, ist der Hybrid-Verbindungs-Manager erforderlich.

Siehe [Verwenden der Hybrid-Verbindungs-Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Möchten Sie den Connector
Der Connector erstellt wurde, können Sie es mit einer Logik app Arbeitsabläufe hinzufügen. Siehe [welche Logik apps?](app-service-logic-what-are-logic-apps.md).

API-Apps mit anderen APIs zu erstellen. [Anschlüsse und API-Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766)anzeigen

Sie können auch die Leistung Statistiken und Kontrolle der Sicherheit an den Anschluss überprüfen. [Verwalten und überwachen die integrierte API-Apps und Connectors](app-service-logic-monitor-your-connectors.md)anzeigen


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png


<properties
   pageTitle="Microsoft Azure App Service Informix-Connector bei | Microsoft Azure"
   description="Verwendung von Informix Connector mit Logik app Auslöser und Aktionen"
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

# <a name="informix-connector"></a>Informix-connector
>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion.

Microsoft Connector für Informix ist eine API-app mit Clientanwendungen nutzen Azure App Service auf Ressourcen in einer IBM Informix-Datenbank gespeichert. Connector enthält einen Microsoft Client Verbindung zu Remotecomputern Informix-Server in einem TCP/IP-Netzwerk, einschließlich Azure Hybrid an lokalen Informix Server Azure Service Bus Relay. Connector unterstützt die folgenden Datenbankvorgänge:

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


## <a name="create-the-informix-connector"></a>Informix-Connector erstellen
Sie können einen Connector Logik App oder Azure Marketplace wie im folgenden Beispiel definieren:  

1. Wählen Sie in Azure Startmenü **Marketplace**.
2. Blatt **Alles** Geben Sie **Informix** im Feld **Suche alles** , und klicken Sie dann auf die EINGABETASTE.
3. Die Suche ergibt alles Bereich, **Informix Verbinder**auswählen.
4. Wählen Sie Blatt Beschreibung Informix Connector **Erstellen**.
5. Blatt Paket Informix Connector Geben Sie den Namen (z.B. "InformixConnectorNewOrders"), App Service-Plan und andere Eigenschaften.
6. Bearbeiten Sie **Paket**, und geben Sie die folgenden Paket.

    Name | Erforderlich |  Beschreibung
--- | --- | ---
ConnectionString | Ja | Informix-Clientverbindungszeichenfolge (z. B. "Network Address = Servername; Netzwerk-Port = 9089; Benutzer-ID = Benutzername; Password = Kennwort; Initial Catalog = Nwind; Standardschema Informix = ").
Tabellen | Ja | Durch Kommas getrennte Liste der Tabelle, Ansicht und alias-Namen für OData-Operationen und generieren swagger Dokumentation mit Beispielen (z. B. "NEWORDERS").
Verfahren | Ja | Durch Kommas getrennte Liste von Prozedur- und Namen (z.B. "SPORDERID").
Lokale Edition | Nein | Bereitstellen von lokalen Azure Service Bus Relay verwenden.
ServiceBusConnectionString | Nein | Azure Service Bus Relay-Verbindungszeichenfolge.
PollToCheckData | Nein | SELECT COUNT-Anweisung mit Logik app Trigger (z. B. "SELECT COUNT (\*) von NEWORDERS WHERE Lieferdatum IS NULL").
PollToReadData | Nein | SELECT-Anweisung mit einer Logik app Trigger (z. B. "Wählen Sie \* aus NEWORDERS, Lieferdatum NULL für Aktualisierung ist").
PollToAlterData | Nein | Update- oder DELETE-Anweisung mit einer Logik app Trigger (z. B. "UPDATE NEWORDERS legen Lieferdatum = aktuelle Datum WHERE CURRENT OF &lt;CURSOR&gt;").

7. Wählen Sie **OK**und dann **Erstellen**.
8. Nach Abschluss des Vorgangs ähneln dem Paket Folgendes:  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>Logik-app mit Informix Connector Daten hinzufügen ##
Sie können eine logische app Aktion zum Hinzufügen zu einer Entität OData-Vorgang mit API einfügen oder Post Informix Tabelle definieren. Beispielsweise Sie können Kundendatensatz, durch die Verarbeitung einer SQL INSERT-Anweisung für eine Tabelle mit einer Identitätsspalte definiert den Identitätswert oder Logik App betroffenen Zeilen zurückgibt (ORDID vom letzten Tabelle auswählen (Werte einfügen in NEWORDERS (CUSTID LIEFERNAME aufgenommene Person, LIEFERORT, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

> [AZURE.TIP] Informix-Verbindung "*an EntitySet*" Wert für die Identitätsspalte und zurückgibt "*API einfügen*" Zeilen betroffen

1. Wählen Sie in Azure Startmenü **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2. Geben Sie den Namen (z.B. "NewOrdersInformix"), App Service-Plan, andere Eigenschaften, und wählen Sie **Erstellen**.
3. Wählen Sie in Azure Startmenü die Logik app erstellten, **Standardeinstellungen**und **Trigger und Aktionen**.
4. Wählen Sie Blatt Auslöser und Aktionen Logik App Vorlagen **Erstellen** .
5. Wählen Sie im Bereich API-Apps **Wiederholung**, eine Frequenz und Intervall und **Markierung**festgelegt.
6. Wählen Sie im Bereich API-Apps **Informix Verbinder aus**, erweitern Sie die Liste Vorgänge **NEWORDER einfügen**wählen.
7. Erweitern Sie die Parameterliste folgende Werte eingeben:  

    Name | Wert
--- | --- 
CUSTID | 10042
SHIPID | 10000
LIEFERNAME | Verzögerte K Kountry Speicher 
AUFGENOMMENE PERSON | 12 Orchester Terrasse
LIEFERORT | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Wählen Sie das **Häkchen** zu der Einstellung, und **Speichern**.
9. Die Einstellung sollte wie folgt aussehen:  
![][3]  
10. Wählen Sie in der Liste **Alle** unter **Vorgänge**zuerst aufgeführt (die letzte Ausführung). 
11. Wählen Sie im Blatt **Logik app ausgeführt** **Aktion** Element **Informixconnectorneworders**.
12. Wählen Sie Blatt **logische app Aktion** **EINGABEN LINK**. Informix-Connector verwendet die Eingaben eine parametrisierte INSERT-Anweisung verarbeitet.
13. Wählen Sie Blatt **logische app Aktion** **Ausgaben verknüpfen**. Eingaben sollten wie folgt aussehen:  
![][4]

#### <a name="what-you-need-to-know"></a>Was Sie wissen müssen

- Connector kürzt Informix Tabellennamen bei Logik app Aktionsnamen. **Einfügen NEWORDERS** Vorgang wird z. B. in **NEWORDER**abgeschnitten.
- Nach dem Speichern der Anwendung Logik **Auslöser und Aktionen**verarbeitet Logik app den Vorgang. Möglicherweise eine Verzögerung eine Anzahl von Sekunden (z. B. 3 bis 5 Sekunden) Logik app verarbeitet den Vorgang. Optional können Sie **Jetzt ausführen,** um den Vorgang zu verarbeiten klicken.
- Informix-Connector definiert EntitySet Member Attribute, ob das Mitglied eine Informix-Spalte mit einem Standardwert entspricht oder Spalten (z. B. Identity) generiert. Logik-app zeigt ein rotes Sternchen neben dem EntitySet ID Membernamen, Informix Spalten anzugeben, die Werte erfordern. Sie sollten keinen Wert eingeben, die für den Member ORDID Informix Identitätsspalte entspricht. Sie können Werte für andere optionale Elemente (Elemente, ORDDATE, REQDATE, SHIPID, Fracht, SHIPCTRY) eingeben Informix Spalten mit Standardwerten entsprechen. 
- Informix-Connector gibt Logik App die Antwort auf auf EntitySet, die enthält die Werte für Identitätsspalten DRDA SQLDARD (Antwort auf SQL Daten Bereich) abgeleitet wird auf die vorbereitete SQL INSERT-Anweisung. Informix-Server kehrt die Werte für Spalten mit Standardwerten.  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>Logik-app mit Informix Connector Daten hinzufügen ##
Sie können eine logische app-Aktion zum Hinzufügen zu einer Informix-Tabelle eine API Bulk Insert-Operation. Beispielsweise Sie können zwei neue Reihenfolge Datensätze, durch die Verarbeitung einer SQL INSERT-Anweisung mit einem Array der Werte eine Tabelle mit einer Identitätsspalte definiert Logik App betroffenen Zeilen zurückgibt (ORDID vom letzten Tabelle auswählen (Werte einfügen in NEWORDERS (CUSTID LIEFERNAME aufgenommene Person, LIEFERORT, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

1. Wählen Sie in Azure Startmenü **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2. Geben Sie den Namen (z.B. "NewOrdersBulkInformix"), App Service-Plan, andere Eigenschaften, und wählen Sie **Erstellen**.
3. Wählen Sie in Azure Startmenü die Logik app erstellten, **Standardeinstellungen**und **Trigger und Aktionen**.
4. Wählen Sie Blatt Auslöser und Aktionen Logik App Vorlagen **Erstellen** .
5. Wählen Sie im Bereich API-Apps **Wiederholung**, eine Frequenz und Intervall und **Markierung**festgelegt.
6. Wählen Sie im Bereich API-Apps **Informix Verbinder aus**, erweitern Sie die Liste Vorgänge **Bulk Insert in neu**aktivieren.
7. Geben Sie **Zeilen** als Array. Zum Beispiel kopieren Sie und fügen Sie die folgenden ein:  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
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

- Connector kürzt Informix Tabellennamen bei Logik app Aktionsnamen. Beispielsweise ist **Bulk Insert in NEWORDERS** Vorgang **Bulk Insert in neue**abgeschnitten.
- Informix-Datenbank möglicherweise groß-, Tabellen-und Spaltennamen. Bulk Insert-Vorgang Array Spaltennamen müssen z. B. in Kleinbuchstaben ("Custid") statt Großbuchstaben ("CUSTID") angegeben werden.
- Durch Auslassen Identitätsspalten (z.B. ORDID), Spalten NULL-Werte zulässt (z. B. Lieferdatum) und Spalten mit Standardwerten (ORDDATE, REQDATE, SHIPID, Fracht, SHIPCTRY), Informix Datenbank Werte generiert.
- Indem "heute" und "morgen" Informix Connector generiert "Datum" und "Aktuelle Datum + 1 Tag" Funktionen (z.B. REQDATE). 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>Logik app Informix Connector Trigger lesen, ändern oder Löschen von Daten ##
Sie können Trigger Logik app abrufen und Lesen von Daten aus einer Informix-Tabelle eine API Umfragedaten composite-Operation. Angenommen, Sie erhalten eine oder mehrere neue Reihenfolge Datensätze, Datensätze nach Anwendung Logik. Die Paket-app Informix Verbindungszeichenfolge sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) von NEWORDERS Lieferdatum NULL ist
PollToReadData | Wählen Sie \* aus NEWORDERS Lieferdatum NULL für Aktualisierung ist
PollToAlterData | <no value specified>


Außerdem können Sie Logik app Trigger abzurufen, lesen und ändern Sie Daten in einer Informix-Tabelle mit einer API-Umfrage zusammengesetzten Datenoperation definieren. Angenommen, Sie erhalten eine oder mehrere neue Reihenfolge Datensätze, Zeilenwerte Logik app (vor Aktualisierung) ausgewählte Datensätze nach aktualisieren. Die Paket-app Informix Verbindungszeichenfolge sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) von NEWORDERS Lieferdatum NULL ist
PollToReadData | Wählen Sie \* aus NEWORDERS Lieferdatum NULL für Aktualisierung ist
PollToAlterData | UPDATE NEWORDERS SET SHIPDATE = aktuelle Datum, aktuelle von &lt;CURSOR&gt;


Darüber hinaus können Sie Trigger Logik app abrufen, lesen und Entfernen von Daten aus einer Informix-Tabelle mit API-Umfragedaten zusammengesetzte Operationen definieren. Angenommen, Sie erhalten eine oder mehrere neue Reihenfolge Datensätze, löschen Sie die Zeilen Logik app (vor löschen) ausgewählte Datensätze nach. Die Paket-app Informix Verbindungszeichenfolge sollte wie folgt aussehen:

    App-Einstellung | Wert
--- | --- | ---
PollToCheckData | SELECT COUNT (\*) von NEWORDERS Lieferdatum NULL ist
PollToReadData | Wählen Sie \* aus NEWORDERS Lieferdatum NULL für Aktualisierung ist
PollToAlterData | Löschen NEWORDERS, in der aktuellen &lt;CURSOR&gt;

In diesem Beispiel wird Logik app Umfrage lesen, aktualisieren und Daten in der Tabelle Informix erneut lesen.

1. Wählen Sie in Azure Startmenü **+** (Pluszeichen) **Web + Mobile**, und anschließend **Logik der Anwendung**.
2. Geben Sie den Namen (z.B. "ShipOrdersInformix"), App Service-Plan, andere Eigenschaften, und wählen Sie **Erstellen**.
3. Wählen Sie in Azure Startmenü die Logik app erstellten, **Standardeinstellungen**und **Trigger und Aktionen**.
4. Wählen Sie Blatt Auslöser und Aktionen Logik App Vorlagen **Erstellen** .
5. Wählen Sie im Bereich API-Apps **Informix Verbinder aus**, legen Sie eine Häufigkeit und Intervall und **Häkchen**.
6. Wählen Sie im Bereich API-Apps **Informix Verbinder aus**, erweitern Sie die Liste Vorgänge zum auswählen **von NEWORDERS**.
7. Wählen Sie das **Häkchen** zu der Einstellung, und **Speichern**. Die Einstellung sollte wie folgt aussehen:  
![][10]
8. Klicken Sie, um das Blade **Auslöser und Aktionen** schließen und dann auf Blatt **Einstellungen** schließen.
9. Klicken Sie in der Liste **Alle** unter **Vorgänge**zuerst aufgeführt (die letzte Ausführung).
10. Klicken Sie auf **den Aktionspunkt** Blatt **Logik app ausgeführt** .
11. Das Blade **logische app Aktion** klicken Sie auf **Ausgaben verknüpfen**. Die Ergebnisse sollten wie folgt aussehen:  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>Logik-app mit Informix Connector Daten entfernen ##
Sie können eine logische app Aktion Entfernen von Daten aus einer Entität OData-Operation mit einer API löschen oder Post Informix-Tabelle definieren. Beispielsweise Sie können Kundendatensatz, durch die Verarbeitung einer SQL INSERT-Anweisung für eine Tabelle mit einer Identitätsspalte definiert den Identitätswert oder Logik App betroffenen Zeilen zurückgibt (ORDID vom letzten Tabelle auswählen (Werte einfügen in NEWORDERS (CUSTID LIEFERNAME aufgenommene Person, LIEFERORT, SHIPREG, SHIPZIP) (?,?,?,?,?,?))).

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>Erstellen Sie Logik mit Informix Connector Daten entfernen ##
Sie können eine neue Logik-app in Azure Marketplace erstellen und verwenden Sie Informix-Connector als Aktion Kundenaufträge entfernen. Beispielsweise können Sie bedingten Löschvorgang von Informix Connector eine SQL DELETE-Anweisung verarbeitet (Löschen von NEWORDERS, ORDID > = 10000).

1. Klicken Sie im Hub Verwaltungsrat Azure **Starten** auf **+** (Pluszeichen) auf **Web + Mobile**und dann auf **app Logik**. 
2. Geben Sie einen **Namen**, z. B. **RemoveOrdersInformix**Blatt **app Logik erstellen** .
3. Wählen Sie oder definieren Sie Werte für die anderen Einstellungen (Serviceplan, Ressourcengruppe).
4. Die Einstellung sollte wie folgt aussehen. Klicken Sie auf **Erstellen**:  
![][12]
5. Klicken Sie auf Blatt **Einstellungen** **Auslöser und Aktionen**.
6. Blatt **Auslöser und Aktionen** in der Liste **Logik app Vorlagen** klicken Sie auf **neu erstellen**.
7. Klicken Sie auf **Serientyp**, Blatt **Auslöser und Aktionen** im Bereich **API-Apps** in der Ressourcengruppe.
8. Auf der Entwurfsoberfläche der Anwendung Logik auf **Serie** Element, **Intervall** und **Intervall**, zum Beispiel **Tage** und **1**festgelegt und dann auf **Markierung** um die Serie Einstellungen speichern.
9. Klicken Sie auf **Informix Connector**Blatt **Auslöser und Aktionen** im Bereich **API-Apps** in der Ressourcengruppe.
10. Klicken Sie auf der Entwurfsoberfläche Logik app auf das Aktionselement **Informix Connector** , klicken Sie auf die Auslassungszeichen****um die Vorgänge erweitern und klicken Sie **bedingte n**.
11. Geben Sie für den Aktionspunkt Informix Connector **Ordid 10000 ge** für einen **Ausdruck, der eine Teilmenge von Einträgen bezeichnet**.
12. Klicken Sie auf die **Markierung** um die Einstellung zu speichern und dann auf **Speichern**. Die Einstellung sollte wie folgt aussehen:  
![][13]
13. Klicken Sie, um das Blade **Auslöser und Aktionen** schließen und dann auf Blatt **Einstellungen** schließen.
14. Klicken Sie in der Liste **Alle** unter **Vorgänge**zuerst aufgeführt (die letzte Ausführung).
15. Klicken Sie auf **den Aktionspunkt** Blatt **Logik app ausgeführt** .
16. Das Blade **logische app Aktion** klicken Sie auf **Ausgaben verknüpfen**. Die Ergebnisse sollten wie folgt aussehen:  
![][14]

**Hinweis:** Logik app Designer kürzt Tabellennamen. **Bedingte löschen NEWORDERS** Vorgang wird beispielsweise **bedingte**Löschen von N abgeschnitten.


> [AZURE.TIP] Verwenden Sie die folgende SQL-Anweisung die Beispieltabelle und gespeicherten Prozeduren erstellen. 

Sie können die NEWORDERS Beispieltabelle mit folgenden Informix SQL DDL-Anweisung erstellen:
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


Sie können die SPORDERID gespeicherte Beispielprozedur mit folgenden Informix DDL-Anweisung erstellen:
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>Mischkonfiguration (Optional)

> [AZURE.NOTE] Dieser Schritt ist nur bei Verwendung von DB2 Connector lokal hinter der Firewall erforderlich.

App Service verwendet Hybrid-Konfigurations-Manager das sichere Verbinden mit Ihrem lokalen System. Anschluss einer lokalen IBM DB2 Server-für Windows verwendet, ist der Hybrid-Verbindungs-Manager erforderlich.

Siehe [Verwenden der Hybrid-Verbindungs-Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Möchten Sie den Connector
Der Connector erstellt wurde, können Sie es mit einer Logik app Arbeitsabläufe hinzufügen. Siehe [welche Logik apps?](app-service-logic-what-are-logic-apps.md).

API-Apps mit anderen APIs zu erstellen. [Anschlüsse und API-Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766)anzeigen

Sie können auch die Leistung Statistiken und Kontrolle der Sicherheit an den Anschluss überprüfen. [Verwalten und überwachen die integrierte API-Apps und Connectors](app-service-logic-monitor-your-connectors.md)anzeigen


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png



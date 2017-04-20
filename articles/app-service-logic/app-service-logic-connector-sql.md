<properties
   pageTitle="Mit dem SQL-Connector Logik Apps | Microsoft Azure App Service"
   description="Zum Erstellen und Konfigurieren von SQL-Connector oder API-app und in einer Anwendung Logik in Azure App Service verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Erste Schritte mit Microsoft SQL Connector und Ihre Anwendung Logik hinzufügen
>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion. Die Schemaversion Azure SQL 2015-08-01-Vorschau klicken Sie auf [SQL Azure-API](../connectors/connectors-create-api-sqlazure.md).

Verbinden Sie mit einer lokalen SQL Server oder einer Azure SQL-Datenbank zum Erstellen und Ändern von Informationen oder Daten. Connectors können Logik Apps abgerufen wird oder Verschieben von Daten als Teil des "Workflow" verwendet werden. Bei Verwendung von SQL-Connector im Workflow erreichen Sie eine Vielzahl von Szenarien. Beispielsweise können Sie:

- Setzen Sie einen Teil der Daten in der SQL-Datenbank mit einem Web oder mobilen Anwendung.
- Einfügen von Daten in einer SQL-Datenbanktabelle für Speicher. Sie können z. B. Mitarbeiterdatensätze eingeben, Aufträge aktualisieren und usw..
- Abrufen von Daten aus SQL und in einem Geschäftsprozess. Beispielsweise können Sie Debitorendatensätze get und put die Kundendatensätze in SalesForce.

Sie können SQL-Connector Workflow- und Prozesskontrolle Geschäftsdaten als Teil dieser Workflows innerhalb einer Anwendung Logik hinzufügen. 

## <a name="triggers-and-actions"></a>Trigger und Aktionen
*Trigger* sind Ereignisse, die stattfinden. Beispielsweise wird ein Auftrag aktualisiert oder ein neuer Kunde hinzugefügt. Eine *Aktion* ist das Ergebnis des Triggers. Z. B. wenn ein Auftrag aktualisiert wird, eine Warnung an den Verkäufer gesendet. Wenn ein neuer Kunde hinzugefügt wird, Willkommen e-Mail für den neuen Debitor.

SQL-Connector kann als Trigger oder eine Aktion in einer Logik app und unterstützt in JSON und XML-Formaten verwendet werden. Für jede Tabelle in Ihrem Paket enthalten Settings (mehr dazu weiter unten in diesem Thema), gibt es eine Reihe von Aktionen JSON und XML-Maßnahmen.

SQL-Connector hat die folgenden Trigger und Aktionen:

Trigger | Aktionen
--- | ---
Daten abrufen | <ul><li>Tabelle einfügen</li><li>Tabelle aktualisieren</li><li>Tabelle auswählen</li><li>Tabelle löschen</li><li>Gespeicherte Prozedur aufrufen</li></ul>

## <a name="create-the-sql-connector"></a>Erstellen von SQL-Connector

Ein Connector Logik App erstellt werden oder direkt von Azure Marketplace erstellt werden. So erstellen Sie einen Connector aus dem Markt  

1. Wählen Sie in Azure Startmenü **Marketplace**.
2. Suchen Sie nach "SQL-Connector" Wählen Sie aus und **Erstellen**.
3. Geben Sie den Namen App Service-Plan und anderen Eigenschaften.
4. Geben Sie die folgenden Paket:

    Name | Erforderlich |  Beschreibung
--- | --- | ---
Servername | Ja | Geben Sie den SQL Server-Namen. Geben Sie z. B. *SQLserver-Sqlexpress* oder *SQLserver.mydomain.com*.
Anschluss | Nein | Standardwert ist 1433.
Benutzername | Ja | Geben Sie in SQL Server einen Benutzernamen anmelden kann. Wenn die Verbindung mit einer lokalen SQL Server Geben Sie SQL-Anmeldeinformationen ein
Kennwort | Ja | Geben Sie den Benutzer und das Kennwort ein.
Name der Datenbank | Ja | Geben Sie die Datenbank, die Sie verbinden. Sie können z. B. *Kunden* oder *Dbo-Aufträge*eingeben.
Vor Ort | Ja | Standard ist False. Geben Sie False, wenn einer Azure SQL-Datenbank herstellen. Geben Sie True, wenn eine lokale SQL Server-Verbindung.
Service Bus-Verbindungszeichenfolge | Nein | Wenn Sie lokal herstellen, geben Sie die Service Bus Relay-Verbindungszeichenfolge.<br/><br/>[Der Hybrid-Verbindungs-Manager verwenden](app-service-logic-hybrid-connection-manager.md)<br/>[Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/)
Partner-Servernamen | Nein | Wenn der primäre Server nicht verfügbar ist, können Sie einen Partnerserver als alternativen oder backup-Server eingeben.
Tabellen | Nein | Liste der Tabellen, die vom Connector aktualisiert werden kann. Geben Sie z. B. *OrdersTable* oder *EmployeeTable*. Wenn keine Tabellen eingegeben werden, können alle Tabellen verwendet werden. Gültiger Tabellen oder gespeicherte Prozeduren müssen diesen Connector als Aktion verwenden.
Gespeicherte Prozeduren | Nein | Geben Sie eine vorhandene gespeicherte Prozedur, die vom Connector aufgerufen werden kann. Geben Sie z. B. *Sp_IsEmployeeEligible* oder *Sp_CalculateOrderDiscount*. Gültiger Tabellen oder gespeicherte Prozeduren müssen diesen Connector als Aktion verwenden.
Verfügbare Datenabfrage | Trigger-Unterstützung | SQL-Anweisung, ob Daten für eine SQL Server-Datenbanktabelle abrufen verfügbar ist. Dies sollte einen nummerischen Wert für die Anzahl der Zeilen von Daten zurückgeben. Beispiel: SELECT COUNT(*) aus Table_name.
Umfrage Datenabfrage | Trigger-Unterstützung | Die SQL-Anweisung, die SQL Server-Datenbanktabelle abrufen. Sie können eine beliebige Anzahl von SQL-Anweisungen, die durch ein Semikolon getrennt eingeben. Diese Anweisung transaktionell ausgeführt und nur übergeben, wenn die Daten in Ihrer Anwendung Logik sicher gespeichert werden. Beispiel: Tabellenname *auswählen; DELETE FROM Tabellenname. <br/>**Note**<br/>Geben Sie eine Anweisung abrufen, die vermeidet eine Endlosschleife durch Löschen, verschieben oder ausgewählte Daten, um sicherzustellen, dass dieselbe Daten erneut abgefragt wird nicht aktualisiert.

5. Nach Abschluss des Vorgangs ähneln dem Paket Folgendes:  
![][1]  

6. Wählen Sie **Erstellen**. 


## <a name="use-the-connector-as-a-trigger"></a>Als Trigger verwenden
Sehen wir uns eine einfache Logik-app Ruft Daten aus einer SQL-Tabelle, die die Daten in einer anderen Tabelle hinzugefügt und aktualisiert die Daten.

Um den SQL-Connector als Trigger verwenden, geben Sie **Daten verfügbar** und **Umfrage Datenabfrage** . **Verfügbare Datenabfrage** erfolgt regelmäßig eingeben und bestimmt, ob Daten verfügbar ist. Da diese Abfrage nur einen skalaren Wert zurückgibt, können Sie optimiert und für die regelmäßige Ausführung optimiert werden.

**Umfrage Datenabfrage** nur ausgeführt, wenn die Abfrage verfügbar gibt an, dass Daten verfügbar sind. Diese Anweisung innerhalb einer Transaktion ausgeführt wird und nur wenn die extrahierten Daten im Workflow dauerhaft gespeichert ist. Es ist wichtig zu unendlich wieder dieselben Daten extrahieren. Des Transaktionsverhaltens der Ausführung kann verwendet werden, löschen oder Aktualisieren von Daten, um sicherzustellen, dass es nicht das nächste Mal gesammelt Daten abgefragt.

> [AZURE.NOTE] Von dieser Anweisung zurückgegebenen Schema gibt die verfügbaren Eigenschaften des Connectors. Alle Spalten müssen benannt werden.

#### <a name="data-available-query-example"></a>Beispiel für Daten verfügbaren Abfrage

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>Daten-Abfragebeispiel Umfrage

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Den Trigger hinzufügen
1. Beim Erstellen oder Bearbeiten einer Anwendung Logik, wählen Sie erstellten SQL-Connector als Trigger. Zeigt die verfügbaren Trigger: **Umfrage Daten (JSON)** und **Umfrage Daten (XML)**:  
![][5]

2. Wählen Sie die **Umfrage Daten (JSON)** , geben Sie die Häufigkeit und klicken Sie auf die ✓:  
![][6]

3. Der Trigger nunmehr in die Logik-app konfiguriert. Ausgaben des Triggers werden angezeigt und können als Eingaben in nachfolgenden Aktionen verwendet:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Als Aktion verwenden
Mit unseren einfachen Logik app, die Daten aus einer SQL-Tabelle abruft die Daten in einer anderen Tabelle hinzugefügt und aktualisiert die Daten.

Um den SQL-Connector als Aktion verwenden, geben Sie den Namen der Tabellen oder gespeicherte Prozeduren eingegebene SQL-Connector erstellt:

1. Nach der Trigger (oder "Logik manuell ausführen"), SQL Connector aus dem Katalog erstellt hinzufügen. Wählen Sie eine Insert-Aktionen wie *Einfügen in TempEmployeeDetails JSON ()*:  
![][8]

2. Geben Sie die Eingabewerte des Datensatzes eingefügt werden, und klicken auf der ✓:  
![][9]

3. Wählen Sie aus der Galerie des gleichen SQL-Connectors erstellten. Wählen Sie Aktion aktualisieren in derselben Tabelle wie *Update EmployeeDetails*als eine Aktion:  
![][11]

4. Geben Sie die Eingabewerte für Aktualisierungsaktion und die ✓ auf:  
![][12]

Testen die Anwendung Logik durch Hinzufügen eines neuen Datensatzes in der Tabelle abgefragt wird.

## <a name="what-you-can-and-cannot-do"></a>Was Sie und nicht

SQL-Abfrage | Unterstützt | Nicht unterstützt
--- | --- | ---
Wo Klausel | <ul><li>Operatoren: Und, oder, = <>, <, < =, >, > =, und wie</li><li>Mehrere Sub Konditionen können kombiniert werden "(" und")"</li><li>Zeichenfolgenliterale, Datetime (eingeschlossen in einfache Anführungszeichen), Zahlen (sollte nur numerische Zeichen enthalten)</li><li>Unbedingt sollte in einem Format binären Ausdruck wie ((operand operator operand) oder (Operand Operator Operand)) *</li></ul> | <ul><li>Operatoren: Zwischen IN</li><li>Alle integrierten Funktionen wie ADD(), MAX() NOW(), POWER() usw.</li><li>Mathematische Operatoren wie *, -, +, usw.</li><li>String-Verkettung mit +.</li><li>Alle Joins</li><li>IST NULL und ist nicht Null</li><li>Zahlen mit nicht numerischen Zeichen hexadezimale Zahlen</li></ul>
Felder (Select-Abfrage) | <ul><li>Gültige Namen durch Kommas getrennt. Präfixe keine Tabelle zulässig (Connector kann für eine Tabelle gleichzeitig).</li><li>Namen können mit Escapezeichen ' [' und ']'</li></ul> | <ul><li>Schlüsselwörter wie TOP, DISTINCT</li><li>Aliasing wie Straße, Ort + Zip als Adresse</li><li>Alle integrierten Funktionen wie ADD(), MAX() NOW(), POWER() usw.</li><li>Mathematische Operatoren wie *, -, +, usw.</li><li>String-Verkettung mit +</li></ul>

#### <a name="tips"></a>Tipps

- Erweiterte Abfragen wird empfohlen, eine gespeicherte Prozedur und ausführen Ausführen gespeicherte Prozedur API verwenden.
- Verwenden Sie innere Abfragen sie in gespeicherten Prozeduren.
- Dank mehrere Konditionen können 'Und' und 'Oder'-Operatoren.

## <a name="hybrid-configuration-optional"></a>Mischkonfiguration (Optional)

> [AZURE.NOTE] Dieser Schritt ist nur bei Verwendung von SQL Server lokal hinter der Firewall erforderlich.

App Service verwendet Hybrid-Konfigurations-Manager das sichere Verbinden mit Ihrem lokalen System. Connector verwendet eine lokale SQL Server sind, ist Hybrid Connection Manager erforderlich.

> [AZURE.NOTE] Wenn Sie mit Azure Logik Apps beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen Logik App](https://tryappservice.azure.com/?appservice=logic)sofort eine kurzlebige Starter Logik-app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

Siehe [Verwenden der Hybrid-Verbindungs-Manager](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Möchten Sie den Connector
Der Connector erstellt wurde, können Sie es mit einer Logik App Arbeitsabläufe hinzufügen. Siehe [welche Logik Apps?](app-service-logic-what-are-logic-apps.md).

Anzeigen der Swagger REST API Reference [Connectors und API-Apps](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Sie können auch die Leistung Statistiken und Kontrolle der Sicherheit an den Anschluss überprüfen. [Verwalten und überwachen die integrierte API-Apps und Connectors](app-service-logic-monitor-your-connectors.md)anzeigen


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png

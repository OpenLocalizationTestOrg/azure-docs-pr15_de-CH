<properties
    pageTitle="Arbeiten mit Node.js Backend-Server SDK für Mobile Apps | Azure App Service"
    description="Informationen Sie zum Arbeiten mit Node.js Backend-Server SDK für Azure App Service Mobile Apps."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Verwendung des Azure Mobile Apps Node.js-SDK

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Dieser Artikel enthält detaillierte Informationen und Beispiele zum Backend Node.js in Azure App Service Mobile Apps arbeiten.

## <a name="Introduction"></a>Einführung

Azure App Service Mobile Apps ermöglicht einen Mobil optimierte Datenzugriff Web API eine Anwendung hinzufügen.  ASP.NET und Node.js ASP.NET-Webanwendungen Azure App Service Mobile Apps SDK vorgesehen.  Das SDK enthält die folgenden Vorgänge:

- Tabellenvorgänge (Lesen, Insert, Update, Delete) für den Datenzugriff
- Benutzerdefinierte API-Operationen

Beide Vorgänge bieten für die Authentifizierung über alle Identitätsanbieter Azure App Service, einschließlich der sozialen Identitätsanbieter wie Facebook, Twitter, Google und Microsoft Azure Active Directory für Enterprise Identity zulässig.

Sie finden Beispiele für jeden Anwendungsfall im [Beispielverzeichnis GitHub].

## <a name="supported-platforms"></a>Unterstützte Plattformen

Azure Mobile Apps Knoten SDK unterstützt die aktuelle LTS Version von Knoten und höher.  Schreibe ist die neueste LTS Knoten v4.5.0.  Andere Versionen von Knoten funktioniert jedoch nicht unterstützt.

Azure Mobile Apps Knoten SDK unterstützt zwei Datenbank - Knoten Mssql-Treiber unterstützt SQL Azure und lokalen SQL Server-Instanzen.  Sqlite3-Treiber unterstützt SQLite-Datenbanken in einer einzigen Instanz.

### <a name="howto-cmdline-basicapp"></a>Gewusst wie: verwenden grundlegende Node.js Backend erstellen

Jede Azure App Service Mobile App Node.js Backend startet eine ExpressJS Anwendung.  ExpressJS ist der am häufigsten verwendeten Webdienst-Framework für Node.js.  Erstellen Sie ein [Express] -Anwendung wie folgt:

1. Erstellen Sie ein Verzeichnis für das Projekt in einem Befehl oder PowerShell-Fenster.

        mkdir basicapp

2. Führen Sie Npm Init um die Paketstruktur zu initialisieren.

        cd basicapp
        npm init

    Befehl Init Npm fordert eine Reihe von Fragen, um das Projekt nicht initialisieren.  Finden Sie in der Ausgabe:

    ![Die Init-Ausgabe npm][0]

3. Installieren Sie express und Azure-Mobile-apps Bibliotheken von Npm Repository.

        npm install --save express azure-mobile-apps

4. Erstellen Sie eine Datei app.js um einfache mobile Server implementieren.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Diese Anwendung erstellt Mobil optimierte WebAPI mit einem einzigen Endpunkt (`/tables/TodoItem`), nicht authentifizierten Zugriff auf einem zugrunde liegenden SQL-Datenspeicher mit einem dynamischen Schema bereitstellt.  Es ist für folgende Client Bibliothek schnell starten:

- [Schnelleinstieg Android-Client]
- [Apache Cordova Client Schnellstart]
- [iOS-Client Schnellstart]
- [Windows Store Client Schnellstart]
- [Xamarin.iOS Client-Schnellstart]
- [Xamarin.Android Client-Schnellstart]
- [Xamarin.Forms Client-Schnellstart]

Den Code finden Sie für diese einfache Anwendung in die [Basicapp Probe auf GitHub].

### <a name="howto-vs2015-basicapp"></a>Gewusst wie: Erstellen Sie Knoten Backend mit Visual Studio 2015

Visual Studio 2015 erfordert eine Erweiterung in der IDE Node.js Anwendungsentwicklung.  Installieren Sie dazu [Node.js Tools 1.1 für Visual Studio].  Nach der Installation Node.js-Tools für Visual Studio erstellen Sie eine Express 4.x Anwendung:

1. Das Dialogfeld **Neues Projekt** öffnen ( **Datei** > **neu** > **Projekt...**).

2. **Vorlagen** > **JavaScript** > **Node.js**.

3. Wählen Sie die **grundlegenden Azure Node.js Express 4 Anwendung**.

4. Geben Sie den Namen des Projekts.  Klicken Sie auf *OK*.

    ![Neues Projekt von Visual Studio 2015][1]

5. Den Knoten **Npm** Maustaste und wählen Sie **neue installieren... Npm Pakete**.

6. Sie müssen Npm-Katalog zum Erstellen Ihrer ersten Node.js-Anwendung aktualisieren.  Klicken Sie ggf. auf **Aktualisieren** .

7. _Azure-Mobile-apps_ in das Suchfeld eingeben.  **Azure-Mobile-apps 2.0.0** -Paket, klicken Sie auf **Paket installieren**.

    ![Neue Npm-Pakete installieren][2]

8. Klicken Sie auf **Schließen**.

9. Öffnen der _app.js_ Azure Mobile Apps SDK unterstützen.  In Zeile 6 zu der Bibliothek Anweisungen erforderlich, fügen Sie den folgenden Code hinzu:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Fügen Sie an rund 27 nach anderen app.use Anweisungen den folgenden Code:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Speichern Sie die Datei.

10. Führen Sie die Anwendung lokal (http://localhost: 3000 ist die API bereitgestellt) oder Azure veröffentlichen.

### <a name="create-node-backend-portal"></a>Gewusst wie: Node.js Backend mit Azure-Portal erstellen

Sie können Recht Backend-Mobile-Anwendung in [Azure-Portal]erstellen. Führen Sie die folgenden Schritte oder Client und Server gemeinsam nach dem [Erstellen einer Apps](app-service-mobile-ios-get-started.md) Tutorial erstellen. Das Lernprogramm enthält eine vereinfachte Version der Bedienungsanleitung und am besten Nachweis Konzept Projekte.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Wählen Sie im Blatt _Erste Schritte_ unter **Erstellen eines API** **Node.js** als **Back-End-Sprache**. Aktivieren Sie das Kontrollkästchen für "**ich bestätige, dass dadurch alle Website Inhalt. überschrieben**" und dann auf **Erstellen TodoItem-Tabelle**.

### <a name="download-quickstart"></a>Gewusst wie: Downloaden Node.js Backend-Schnellstart Codeprojekt mit Git

Beim Erstellen Backend Node.js Mobile-Anwendung über das Portal **Schnellstart** Blade Node.js-Projekt erstellt und auf Ihrer Website bereitgestellt. Sie können Tabellen und APIs hinzufügen und Codedateien für das Backend Node.js im Portal bearbeiten. Verschiedene Bereitstellungstools können Sie das Back-End-Projekt herunterladen, hinzufügen oder Ändern von Tabellen und APIs und veröffentlichen Sie das Projekt erneut. Weitere Informationen finden Sie im [Bereitstellungshandbuch für Azure App Service]. Die folgende Prozedur verwendet ein Git Repository Quickstart Projektcode herunterladen.

1. Installieren Sie Git, falls Sie dies nicht bereits getan haben. Die Schritte zum Installieren der Git unterschiedlich Betriebssysteme. Siehe [Installation von Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) für betriebssystemspezifische Verteilung und Installation.

2. Folgen Sie den Schritten Git Repository für die Back-End-Website eine Notiz Bereitstellung Benutzername und Kennwort aktivieren [App Service app Repository aktivieren](../app-service-web/app-service-deploy-local-git.md#Step3) .

3. Blatt für die Mobile Anwendung Backend Notieren der **Git Clone URL** -Einstellung.

4. Ausführen der `git clone` Befehl Git clone Passworteingabe bei Bedarf wie im folgenden Beispiel-URL:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Wechseln Sie zum lokalen Verzeichnis, das im vorherigen Beispiel /todolist ist und beachten Sie, dass Dateien heruntergeladen wurden. Suchen Sie die `todoitem.json` Datei in der `/tables` Verzeichnis.  Diese Datei definiert die Berechtigungen für die Tabelle.  Auch die `todoitem.js` -Datei im gleichen Verzeichnis definiert, CRUD-Vorgang Skripts für die Tabelle.

6. Nachdem Sie Projektdateien geändert haben, führen Sie die folgenden Befehle hinzufügen, übernehmen und dann die Änderungen auf die Website hochladen:

        $ git commit -m "updated the table script"
        $ git push origin master

    Wenn Sie das Projekt neue Dateien hinzufügen, müssen Sie zunächst führen die `git add .` Befehl.

Die Website wird veröffentlicht, jedes Mal ein neuer Satz von Commits auf der Website abgelegt ist.

### <a name="howto-publish-to-azure"></a>Gewusst wie: Backend Node.js in Azure veröffentlichen

Microsoft Azure bietet viele Mechanismen für Azure App Service Mobile Apps Node.js Backend Azure Service veröffentlichen.  Dazu gehören mit Bereitstellungstools in Visual Studio integriert, Befehlszeilenprogramme und kontinuierliche Bereitstellungsoptionen auf Datenquellen-Steuerelement.  Weitere Informationen zu diesem Thema finden Sie im [Bereitstellungshandbuch für Azure App Service].

Azure App Service hat Tipps für Node.js-Anwendung, die Sie vor der Bereitstellung überprüfen sollten:

- [Knoten-Version] angeben
- Wie Sie [Knoten Module]

### <a name="howto-enable-homepage"></a>Gewusst wie: Aktivieren einer Homepage für Ihre Anwendung

Viele sind eine Kombination von Web- und mobile apps und ExpressJS Framework ermöglicht zwei Facetten kombinieren.  In einigen Fällen können Sie jedoch nur eine mobile Schnittstelle implementieren.  Es ist vorgesehen, dass Zielseite um die app Service ausgeführt wird.  Geben Ihre eigene Homepage oder eine temporäre Homepage aktivieren.  Um eine temporäre Homepage zu aktivieren, verwenden Sie folgende Azure Mobile Apps instanziieren:

    var mobile = azureMobileApps({ homePage: true });

Wollen Sie nur diese Option verfügbar bei lokal, können Sie diese Einstellung, um Ihre `azureMobile.js` Datei.

## <a name="TableOperations"></a>Tabellenvorgänge 

Azure-Mobile-apps Node.js Server SDK bietet Mechanismen zum Verfügbarmachen von Datentabellen in Azure SQL-Datenbank als eine WebAPI gespeichert.  Fünf dienen.

| Vorgang | Beschreibung |
| --------- | ----------- |
| /Tables/_Tablename_ abrufen | Alle Datensätze in der Tabelle abrufen |
| /Tables/_Tabellenname_/:id abrufen | Abrufen eines bestimmten Datensatzes in der Tabelle |
| /Tables/_Tablename_ buchen | Erstellen Sie einen Datensatz in der Tabelle |
| PATCH /tables/_Tabellenname_/:id | Ein Datensatz in der Tabelle aktualisieren |
| /Tables/_Tabellenname_/:id löschen | Löschen eines Datensatzes in der Tabelle |

Diese WebAPI [OData] unterstützt und erweitert das Schema zur Unterstützung von [offline-Daten synchronisieren].

### <a name="howto-dynamicschema"></a>Gewusst wie: Definieren von Tabellen mit einem dynamischen Schema

Bevor eine Tabelle verwendet werden kann, muss er definiert.  Tabellen können definiert werden, mit einem statischen Schema (wo Entwickler die Spalten im Schema definiert) oder dynamisch (wo das SDK steuert das Schema basierend auf Anfragen). Darüber hinaus kann Entwickler bestimmte Aspekte der WebAPI steuern die Definition Javascript-Code hinzufügen.

Als bewährte Methode sollte jede Tabelle in einer Javascript-Datei im Verzeichnis Tabellen definieren Sie mithilfe der tables.import()-Methode die Tabellen importieren.  Basic-Anwendung erweitern, würde die Datei app.js angepasst werden:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definieren die Tabelle. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabellen mit dynamischen Schema Standardeinstellung.  Zum Deaktivieren des dynamischen Schema Global App Einstellung **MS_DynamicSchema** auf False festgelegt in Azure-Portal.

Ein vollständiges Beispiel finden Sie im [Beispiel Todo auf GitHub].

### <a name="howto-staticschema"></a>Gewusst wie: Definieren von Tabellen mit einem statischen Schema

Sie können Spalten über die WebAPI verfügbar machen explizit definieren.  Azure-Mobile-apps Node.js SDK fügt automatisch alle zusätzlichen Spalten erforderlich für offline-Daten synchronisieren der Liste, die Sie bereitstellen.  Schnellstart-Clientanwendungen erfordern beispielsweise eine Tabelle mit zwei Spalten: Text (String) und (boolesch).  
Die Tabelle kann in der Tabelle JavaScript Definitionsdatei (befindet sich im Verzeichnis Tabellen) wie folgt definiert:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Wenn Sie Tabellen statisch definieren, müssen Sie auch die tables.initialize()-Methode, um das Datenbankschema beim Start erstellen aufrufen.  Die tables.initialize()-Methode gibt ein [Versprechen] , damit der Webdienst keine Anfragen vor der Datenbank initialisiert bedienen.

### <a name="howto-sqlexpress-setup"></a>Gewusst wie: SQL Express als Datenspeicher Entwicklung auf dem lokalen Computer verwenden

Azure Mobile Apps die AzureMobile Apps Knoten SDK bietet drei Optionen für die Bereitstellung von Daten aus dem Feld: SDK bietet drei Optionen für die Bereitstellung von Daten aus dem Feld:

- Verwenden Sie den Treiber **Speicher** wird nicht beständige Speicher bereitstellen
- Verwenden Sie den **Mssql** -Treiber zu einem Datenspeicher SQL Express für die Entwicklung
- Verwenden Sie **Mssql** -Treiber zu einem Datenspeicher Azure SQL-Datenbank für die Produktion

Azure Mobile Apps Node.js SDK verwendet [Mssql Node.js Paket] und eine Verbindung zur SQL Express und SQL-Datenbank verwenden.  Dieses Paket erfordert, dass der SQL Express-Instanz TCP-Verbindungen aktivieren.

> [AZURE.TIP]Memory-Treiber bietet keine umfassende Funktionen zum Testen.  Die Back-End-lokal testen möchten, sollten die Verwendung von einem Datenspeicher SQL Express und der Mssql-Treiber.

1. Downloaden Sie und installieren Sie [Microsoft SQL Server 2014 Express].  Stellen Sie sicher, dass SQL Server 2014 Express Edition Tools installieren.  Wenn Sie explizit 64-Bit-Unterstützung benötigen, verbraucht 32-Bit-Version weniger Arbeitsspeicher beim Ausführen.

2. Führen Sie den Konfigurations-Manager von SQL Server 2014.

  1. Erweitern Sie in der linken Struktur Menü **SQL Server-Netzwerkkonfiguration** .
  2. Klicken Sie auf **Protokolle für SQLEXPRESS**.
  3. Maustaste auf **TCP/IP** und **Aktivieren**.  Klicken Sie im Popupfenster auf **OK** .
  4. **TCP/IP** Maustaste, und wählen Sie **Eigenschaften**.
  5. Klicken Sie auf die Registerkarte **IP-Adressen** .
  6. Suchen Sie den Knoten **IPAll** .  Geben Sie im Feld **TCP-Port** **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Klicken Sie auf **OK**.  Klicken Sie im Popupfenster auf **OK** .
  8. Klicken Sie im linken Struktur auf **SQL Server-Dienste** .
  9. **SQL Server (SQLEXPRESS)** Maustaste und wählen Sie **neu starten**
  10. Schließen Sie SQL Server 2014 Configuration Manager.

3. Führen Sie SQL Server 2014 Management Studio und Ihrer lokalen SQL Express-Instanz herstellen

  1. Maustaste auf die Instanz im Objekt-Explorer und wählen Sie **Eigenschaften**
  2. Wählen Sie die Seite **Sicherheit** .
  3. Stellen Sie sicher, dass **SQL Server und Windows-Authentifizierungsmodus** ausgewählt ist
  4. Klicken Sie auf **OK**

        ![Konfigurieren der SQL Express-Authentifizierung][4]

  5. Erweitern Sie **Sicherheit** > **Benutzernamen** im Objekt-Explorer
  6. Klicken Sie auf **Anmeldung** , und wählen Sie **Neue Anmeldung...**
  7. Geben Sie einen Benutzernamen ein.  Wählen Sie **SQL Server-Authentifizierung**.  Geben Sie ein Kennwort und Kennwort **bestätigen**Geben Sie dasselbe Kennwort ein.  Das Kennwort muss Windows Komplexität erfüllen.
  8. Klicken Sie auf **OK**

        ![Hinzufügen eines neuen Benutzers zu SQL Express][5]

  9. Ihr neuen Benutzername Maustaste, und wählen Sie **Eigenschaften**
  10. Die Seite **Serverrollen** auswählen
  11. Aktivieren Sie das Kontrollkästchen neben der Serverrolle **dbcreator**
  12. Klicken Sie auf **OK**
  13. Schließen Sie SQL Server 2015 Management Studio

Sicherstellen Sie, dass Sie Benutzername und Kennwort gewählte aufzeichnen.  Sie müssen zusätzliche Server-Rollen oder Berechtigungen je nach Bedarf bestimmte Datenbank zuweisen.

Node.js-Anwendung liest die Umgebungsvariable **SQLCONNSTR_MS_TableConnectionString** für die Verbindungszeichenfolge für die Datenbank.  Sie können diese Variable in Ihrer Umgebung festlegen.  PowerShell können Sie diese Umgebungsvariable festlegen:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Zugriff auf die Datenbank über eine TCP-Verbindung und einen Benutzernamen und ein Kennwort für die Verbindung.

### <a name="howto-config-localdev"></a>Gewusst wie: Konfigurieren des Projekts für lokale Entwicklung

Azure Mobile Apps liest eine JavaScript-Datei mit _azureMobile.js_ aus dem lokalen Dateisystem.  Verwenden Sie diese Datei nicht konfigurieren Azure Mobile Apps SDK in Produktion-Appeinstellungen in [Azure-Portal] verwenden.  Die _azureMobile.js_ -Datei sollte ein Konfigurationsobjekt exportieren.  Die gängigsten sind:

- Database Settings
- Diagnoseprotokolle Settings
- Alternative CORS Settings

Eine Implementierung der vorherigen datenbankeinstellungen _azureMobile.js_ Beispieldatei folgt:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Es wird empfohlen, die _.gitignore_ Datei _azureMobile.js_ hinzufügen (oder anderen Quellcode Datei ignorieren) zu verhindern, dass Kennwörter in der Cloud gespeichert werden.  Immer konfigurieren Sie Produktion im Anwendung in [Azure-Portal].

### <a name="howto-appsettings"></a>Vorgehensweise: Konfigurieren App für die Mobile Anwendung

Die meisten Einstellungen in der Datei _azureMobile.js_ haben eine entsprechende Einstellung der Anwendung in [Azure-Portal].  Anhand der folgenden Liste Ihre app im App konfigurieren:

| App-Einstellung                 | _azureMobile.js_ festlegen  | Beschreibung                               | Gültige Werte                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | Name                      | Der Name der Anwendung                       | Zeichenfolge                                      |
| **MS_MobileLoggingLevel**   | Logging.Level             | Minimale Protokollebene Nachrichten protokollieren      | Fehler, Warnung, Informationen, ausführliche Debug, dumm |
| **MS_DebugMode**            | Debuggen                     | Aktivieren oder deaktivieren Debugmodus              | True, false                                 |
| **MS_TableSchema**          | Data.Schema               | Standardname für SQL-Tabellen-schema        | Zeichenfolge (Standard: Dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Aktivieren oder deaktivieren Debugmodus              | True, false                                 |
| **MS_DisableVersionHeader** | Version (auf undefined gesetzt)| Deaktiviert den Header X-ZUMO-Server-Version | True, false                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Deaktiviert das Kontrollkästchen Client-API-version     | True, false                                 |

So richten Sie eine App-Einstellung

1. Auf der [Azure-Portal]anmelden.
2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen der Mobile-Anwendung.
3. Einstellungen-Blades wird standardmäßig geöffnet. **Wenn nicht, klicken Sie auf.**
4. Klicken Sie im Menü Allgemein auf **ApplicationSettings** .
5. Gehen Sie zum Abschnitt App-Einstellungen.
6. Wenn Ihre Anwendung Einstellung bereits vorhanden ist, klicken Sie auf den Wert der app-Einstellung den Wert bearbeiten.
7. Wenn Ihre app-Einstellung nicht vorhanden ist, geben Sie App-Einstellung in den Schlüssel und den Wert in das Feld Wert ein.
8. Sobald Sie abgeschlossen sind, klicken Sie auf **Speichern**.

Die meisten app Einstellungen erfordert einen Neustart der Dienste.

### <a name="howto-use-sqlazure"></a>Gewusst wie: SQL-Datenbank als Datenspeicher Produktion verwenden

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Verwendung von Azure SQL-Datenbank als Datenspeicher ist für alle Arten von Azure App Service-Anwendung identisch. Wenn dies noch nicht getan haben, folgendermaßen Sie vor, um Mobile App-Back-End erstellen.

1. Auf der [Azure-Portal]anmelden.

2. Oben links im Fenster, klicken Sie auf **+ neu** > **Web + Mobile** > **Mobile-Anwendung**, geben Sie einen Namen für Ihre Mobile App-Backend.

3. Geben Sie im Feld **Ressourcengruppe** den gleichen Namen wie Ihre app.

4. App-Standarddienst Plan ausgewählt ist.  Möchten Sie Ihren App Service-Plan ändern, Sie können dazu die App Service-Plan auf > **+ neu erstellen**.  Name der neuen App Service-Plan und wählen Sie eine geeignete Stelle.  Klicken Sie auf die Preisstufe und wählen Sie einen entsprechenden Tarif für den Dienst. Wählen Sie **Ansicht alle** Weitere Preisoptionen **frei** und **Shared**anzeigen.  Nach Auswahl den Tarif klicken Sie **Wählen** .  Klicken Sie in **App Service-Plan** -Blade auf **OK**.

5. Klicken Sie auf **Erstellen**. Eine Mobile Anwendung Back-End-Bereitstellung kann einige Minuten dauern.  Nach der Bereitstellung von Mobile-Anwendung Back-End-Öffnet das Portal Blatt **Einstellungen** für Mobile App-Backend.

Erstellte Mobile App-Back-End können Sie Ihre Mobile App-Back-End eine vorhandene SQL-Datenbank herzustellen oder eine neue SQL-Datenbank erstellen.  In diesem Abschnitt erstellen Sie eine SQL-Datenbank.

> [AZURE.NOTE]Haben Sie bereits eine Datenbank in demselben Speicherort wie die mobile Anwendung Back-End-, können stattdessen **vorhandene Datenbank verwenden** , und wählen Sie dann die Datenbank. Die Verwendung einer Datenbank an einem anderen Speicherort wird durch höhere Wartezeiten nicht empfohlen.

6. **Klicken Sie im neuen Mobile App Backend** > **Mobile-Anwendung** > **Daten** > **+ Hinzufügen**.

7. Blade **Datenquelle hinzufügen** klicken Sie **SQL - Einstellungen konfigurieren** > **eine neue Datenbank erstellen**.  Geben Sie im Feld **Name** den Namen der neuen Datenbank.

8. Klicken Sie auf **Server**.  **Neue Server** -Blade Geben Sie einen eindeutigen Namen im Feld **Servername** und einen geeigneten **Server Administrator-Benutzernamen** und **Kennwort**.  Sicherstellen Sie, dass **Zulassen Azure Services Zugriff auf Server** aktiviert ist.  Klicken Sie auf **OK**.

    ![Erstellen Sie eine SQL Azure-Datenbank][6]

9. Das Blade **neue Datenbank** klicken Sie auf **OK**.

10. Wählen Sie auf das Blade **Datenquelle hinzufügen** **Verbindungszeichenfolge**, Benutzernamen und Kennwort, die Sie beim Erstellen der Datenbank bereitgestellt.  Wenn Sie eine vorhandene Datenbank verwenden, bieten Sie die Anmeldeinformationen für die Datenbank.  Einmal eingegeben haben, klicken Sie auf **OK**.

11. Wieder auf die **Datenquelle hinzufügen** , klicken Sie auf **OK** , um die Datenbank zu erstellen.

<!--- END OF ALTERNATE INCLUDE -->

Erstellung der Datenbank kann einige Minuten dauern.  Verwenden **der Infobereich** den Fortschritt der Bereitstellung überwachen.  Keine ausgeführt, bis die Datenbank erfolgreich bereitgestellt wurde.  Nachdem erfolgreich bereitgestellt haben, wird eine Verbindungszeichenfolge für die SQL-Datenbankinstanz im mobilen Backend App-Einstellungen erstellt.  Diese app Einstellung **Einstellungen**Siehe > **ApplicationSettings** > **Verbindungszeichenfolgen**.

### <a name="howto-tables-auth"></a>Gewusst wie: Authentifizierung für den Zugriff auf Tabellen

Möchten Sie mit den Tabellen App-Authentifizierung verwenden, müssen Sie zuerst App-Authentifizierung in [Azure-Portal] konfigurieren.  Weitere Informationen zum Konfigurieren der Authentifizierung in Azure App Service finden Sie im Konfigurationshandbuch für Identitätsanbieter verwenden möchten:

- [Azure Active Directory-Authentifizierung konfigurieren]
- [Facebook-Authentifizierung konfigurieren]
- [Google-Authentifizierung konfigurieren]
- [Microsoft Authentication konfigurieren]
- [Twitter-Authentifizierung konfigurieren]

Jede Tabelle verfügt über eine Access-Eigenschaft, die zum Steuern des Zugriffs auf die Tabelle verwendet werden kann.  Im folgende Beispiel wird eine statisch definierte Tabelle mit Authentifizierung erforderlich.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Die Eigenschaft kann einen der drei Werte annehmen

  - *anonyme* bedeutet, dass die Clientanwendung ohne Authentifizierung lesen
  - *Authentifizierte* gibt an, dass die Clientanwendung ein gültigen Authentifizierungstokens mit der Anforderung gesendet werden müssen
  - *deaktiviert* bedeutet, dass diese Tabelle gesperrt ist

Wenn die Eigenschaft nicht definiert ist, ist der nicht authentifizierter Zugriff zulässig.

### <a name="howto-tables-getidentity"></a>Gewusst wie: Authentifizierung Ansprüche von Tabellen verwenden

Sie können verschiedene Ansprüche einrichten, die angefordert werden, wenn Authentifizierung eingerichtet wird.  Diese Anträge sind nicht normalerweise durch die `context.user` Objekt.  Aber sie können abgerufen werden mit der `context.user.getIdentity()` Methode.  Die `getIdentity()` -Methode gibt ein Versprechen in ein Objekt aufgelöst wird.  Das Objekt wird durch die Authentifizierungsmethode (Facebook, Google, Twitter, Microsoftaccount oder Aad) eingegeben.

Wenn Sie Microsoft Account Authentifizierung und Anforderung Anspruch die e-Mail-Adressen einrichten, können Sie die e-Mail-Adresse auf den Datensatz mit der folgenden Tabelle hinzufügen:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Um festzustellen, welche verfügbar sind, verwenden Sie einen Webbrowser an der `/.auth/me` Endpunkt der Website.

### <a name="howto-tables-disabled"></a>Gewusst wie: Deaktivieren des Zugriffs auf bestimmte Tabellenvorgänge

Zusätzlich in der Tabelle angezeigt werden, kann die Eigenschaft steuern einzelner Arbeitsgänge verwendet werden.  Es gibt vier Vorgänge aus:

  - *Lesen* ist RESTful GET-Vorgang für die Tabelle
  - *Legen Sie* ist den Rest POST-Vorgang für die Tabelle
  - RESTful PATCH-Vorgang für die Tabelle wird *aktualisiert*
  - *Löschen* ist der Arbeitsgang Rest Löschen der Tabelle

Möglicherweise möchten eine schreibgeschützte nicht authentifizierte Tabelle:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Gewusst wie: Anpassen die Abfrage, mit Tabelle

Eine allgemeine Anforderung für Tabellenvorgänge soll eine eingeschränkte Ansicht der Daten.  Beispielsweise können Sie eine Tabelle, die mit authentifizierten Benutzer-ID markiert ist, kann nur gelesen oder Ihre eigenen Datensätze aktualisieren.  Die folgende Tabellendefinition stellt diese Funktionalität bereit:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Vorgänge, die normalerweise eine Abfrage ist eine Abfrageeigenschaft, die Sie, mit einer anpassen können Klausel. Abfrageeigenschaft ist ein [QueryJS] -Objekt, eine OData-Abfrage in etwas zu konvertieren, das Backend Daten verarbeiten kann.  Einfache Übereinstimmungsvergleiche (wie die vorhergehenden) Fällen kann eine Zuordnung verwendet werden. Sie können auch SQL-Klauseln hinzufügen:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Gewusst wie: Weiches Löschen einer Tabelle konfigurieren

Weiche wird nicht tatsächlich Datensätze löschen.  Stattdessen werden sie in der Datenbank gelöscht, indem die gelöschte Spalte auf true festgelegt.  Azure Mobile Apps SDK entfernt automatisch Datensätze gelöscht Ergebnisse, wenn Mobile Client SDK IncludeDeleted() verwendet.  Zum Konfigurieren einer Tabelle für weiche löschen legen die `softDelete` -Eigenschaft in der Tabellendefinitionsdatei:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Sie sollten einen Mechanismus zum Löschen von Datensätzen – entweder von einer Clientanwendung über ein Webauftrag Azure-Funktion oder eine benutzerdefinierte API einrichten.

### <a name="howto-tables-seeding"></a>Gewusst wie: Seeding die Datenbank mit Daten

Beim Erstellen einer neuen Anwendung können Sie eine Tabelle mit Daten Startwert.  Dies kann in der Tabelle Definition JavaScript-Datei wie folgt erfolgen:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Das Seeding der Daten erfolgt nur bei Azure Mobile Apps SDK ist.  Wenn die Tabelle in der Datenbank vorhanden ist, werden keine Daten in die Tabelle eingefügt.  Wenn dynamische Schema aktiviert ist, wird das Schema vergebene Daten abgeleitet.

Wir empfehlen explizit aufrufen der `tables.initialize()` Methode, um die Tabelle zu erstellen, wenn der Dienst gestartet wird.

### <a name="Swagger"></a>Gewusst wie: Aktivieren der Unterstützung von stolz

Azure App Service Mobile Apps kommt mit eingebauten [Swagger] unterstützt.  Damit stolz Support zuerst installieren Sie stolz-Benutzeroberfläche als Abhängigkeit:

    npm install --save swagger-ui

Nach der Installation können Sie stolz Unterstützung in Azure Mobile Apps-Konstruktor:

    var mobile = azureMobileApps({ swagger: true });

Sie wahrscheinlich nur stolz Unterstützung in Development Edition aktivieren möchten.  Nutzen Sie dazu die `NODE_ENV` app-Einstellung:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Der stolz Endpunkt befindet sich unter http://_Standortname_.azurewebsites.net/swagger.  Swagger-Benutzeroberfläche über Zugriff der `/swagger/ui` Endpunkt.  Wunsch der gesamten Anwendung Authentifizierung erzeugt stolz einen Fehler.  Für optimale Ergebnisse festlegen, nicht authentifizierte Anfragen in Azure App-Authentifizierung / Autorisierung, dann Einstellungen Authentifizierung der `table.access` Eigenschaft.

Sie können auch die Option stolz Ihre `azureMobile.js` Datei, wenn Sie nur stolz Unterstützung bei lokal.

## <a name="a-namepushpush-notifications"></a><a name="push">Push-Benachrichtigung

Mobile Apps integriert Azure Notification Hubs können Millionen von Geräten für alle wichtigen Plattformen gezielte Pushbenachrichtigungen an. Mit Notification Hubs, schicken Sie Pushbenachrichtigungen zu iOS, Android und Windows. Sie erfahren können mit Notification Hubs, finden Sie unter [Übersicht über Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Gewusst wie: Senden von Pushbenachrichtigungen

Im folgenden Codebeispiel wird die Verwendung das Push-Objekt registrierten iOS Geräte broadcast Push-Benachrichtigung an:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Erstellen einer Vorlage Push Registrierung vom Client, können Sie stattdessen eine Vorlagennachricht Geräte auf allen unterstützten Plattformen senden. Der folgende Code zeigt, wie eine Vorlage Benachrichtigung:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Gewusst wie: Senden von Pushbenachrichtigungen für einen authentifizierten Benutzer mit Tags

Wenn ein authentifizierter Benutzer für Pushbenachrichtigungen registriert, ist eine Benutzer-ID-Tag die Registrierung automatisch hinzugefügt. Mithilfe dieses Tags können Sie alle Geräte, die von einem bestimmten Benutzer registriert Push-Benachrichtigung senden. Der folgende Code Ruft die SID des Benutzers und sendet eine Push-vorlagenbenachrichtigung jedes Gerät Registrierung für diesen Benutzer:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Anmeldung von einem authentifizierten Client Pushbenachrichtigungen, sicherzustellen Sie, dass die Authentifizierung abgeschlossen ist, bevor Sie versuchen, die Registrierung.

## <a name="CustomAPI"></a>Benutzerdefinierte APIs

###  <a name="howto-customapi-basic"></a>Gewusst wie: definieren eine benutzerdefinierte API


Neben der Datenzugriffs-API über den Endpunkt Tables bieten Azure Mobile Apps benutzerdefinierte API-Abdeckung.  Benutzerdefinierte APIs ähnlich die Tabellendefinitionen definiert und können die gleichen Funktionen, einschließlich Authentifizierung zugreifen.

Wenn Sie eine benutzerdefinierte API App-Authentifizierung verwenden möchten, müssen Sie zuerst App-Authentifizierung in [Azure-Portal] konfigurieren.  Weitere Informationen zum Konfigurieren der Authentifizierung in Azure App Service finden Sie im Konfigurationshandbuch für Identitätsanbieter verwenden möchten:

- [Azure Active Directory-Authentifizierung konfigurieren]
- [Facebook-Authentifizierung konfigurieren]
- [Google-Authentifizierung konfigurieren]
- [Microsoft Authentication konfigurieren]
- [Twitter-Authentifizierung konfigurieren]

Benutzerdefinierte APIs sind in fast genauso wie die Tabellen-API definiert.

1. Ein **api** -Verzeichnis erstellen
2. Erstellen Sie eine Definitionsdatei JavaScript API in **API-** Verzeichnis.
3. Verwenden Sie die Importmethode **API-** Verzeichnis importieren.

Hier ist der Prototyp-API-Definition basierend auf den zuvor verwendeten Basic-app.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Nehmen Sie eine Beispiel-API, die das Datum des Servers mithilfe der _Date.now()_ -Methode zurückgegeben.  Hier wird die api/date.js-Datei:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Jeder Parameter ist der Rest Standardverben - GET, POST, PATCH oder löschen.  Die Methode ist eine Standardfunktion [ExpressJS Middleware] die erforderliche Ausgabe gesendet.

### <a name="howto-customapi-auth"></a>Gewusst wie: Authentifizierung für den Zugriff auf eine benutzerdefinierte API

Azure Mobile Apps SDK implementiert Authentifizierung auf die gleiche Weise für die Tabellen Endpunkt und benutzerdefinierte APIs.  Um Authentifizierung API entwickelt im vorherigen Abschnitt hinzuzufügen, fügen Sie einer **Access** -Eigenschaft:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Sie können auch auf bestimmte Vorgänge Authentifizierung angeben:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Für Tabellen-Endpunkt wird auch muss für benutzerdefinierte Authentifikation APIs verwendet werden.

### <a name="howto-customapi-auth"></a>Gewusst wie: Behandeln von großen Dateiuploads

Azure Mobile Apps SDK verwendet [Text-Parser Middleware](https://github.com/expressjs/body-parser) akzeptieren und Inhalt Ihrer Einsendung decodieren.  Sie können Text-Parser, um größere Dateiuploads akzeptieren vorkonfigurieren:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Die Datei ist Base64-codierte vor der Übertragung.  Das tatsächliche Upload vergrößert und somit die Größe Sie berücksichtigt.

### <a name="howto-customapi-sql"></a>Gewusst wie: Ausführen von benutzerdefinierten SQL-Anweisung

Azure Mobile Apps SDK ermöglicht den Zugriff auf den gesamten Kontext durch das Anforderungsobjekt ermöglicht parametrisierte SQL-Anweisungen Anbieter definierten Daten problemlos ausführen:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Debuggen, einfache Tabellen und einfache APIs

### <a name="howto-diagnostic-logs"></a>Gewusst wie: Debuggen, diagnose und Problembehandlung bei Azure Mobile apps

Azure App Service bietet verschiedene Debuggen und Problembehandlungsverfahren für Node.js-Applikationen.
Finden Sie in den folgenden Artikeln bei Ihrem Node.js Mobile Back-End-Einstieg:

- [Überwachen von Azure App Service]
- [Diagnoseprotokoll in Azure App Service aktivieren]
- [Problembehandlung bei einer Azure App Service in Visual Studio]

Node.js Applikationen haben Zugriff auf eine Vielzahl von Tools Diagnoseprotokoll.  Intern verwendet Azure Mobile Apps Node.js SDK [Winston] für Diagnoseprotokoll.  Protokollierung ist Debug-Modus aktivieren oder **MS_DebugMode** app Einstellung true im [Azure-Portal]automatisch aktiviert. Generierte Protokolle in die Diagnoseprotokolle [Azure-Portal]angezeigt.

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Gewusst wie: Arbeiten mit einfachen Tabellen im Azure-Portal

Einfache Tabellen im Portal können Sie erstellen und Arbeiten mit Tabellen im Portal. Sie können auch Tabellenvorgänge im App Service-Editor bearbeiten.

Wenn Sie **einfache Tabellen** in der Back-End-Standortparameter klicken, können Sie hinzufügen, ändern oder Löschen einer Tabelle. Sie können auch Daten in der Tabelle angezeigt.

![Einfache Tabellen arbeiten](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Die folgenden Befehle stehen auf der Befehlsleiste eine Tabelle:

+ **Berechtigungen ändern** - ändern Sie die Berechtigung zum Lesen, einfügen, aktualisieren und löschen Vorgänge in der Tabelle. 
  Optionen sind anonymen Zugriff, Authentifizierung und Zugriff auf die Operation deaktivieren. 
+ **Skript bearbeiten** - Skriptdatei für die Tabelle wird im App Service-Editor geöffnet.
+ **Schema verwalten** - hinzufügen oder Löschen von Spalten oder ändern den Tabellenindex.
+ **Tabelle löschen** - schneidet eine vorhandene Tabelle löschen alle Datenzeilen aber Verlassen des Schemas unverändert.
+ **Zeilen löschen** - Löschen einzelner Datenzeilen.
+ **Streaming-Protokolle anzeigen** - Verbindung streaming Protokolldienst für Ihre Website.

###<a name="work-easy-apis"></a>Gewusst wie: Arbeiten mit einfachen APIs im Azure-Portal

Einfache APIs im Portal ermöglichen das Erstellen und Verwenden von benutzerdefinierten APIs direkt im Portal. Sie können API Skripts mit dem App-Service-Editor bearbeiten.

Klick **Einfach APIs** in der Back-End-Standortparameter können Sie hinzufügen, ändern oder Löschen einen benutzerdefinierten API-Endpunkt.

![Arbeiten mit einfachen APIs](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Im Portal können Sie Zugriffsberechtigungen für HTTP-Aktionen ändern, API-Skriptdatei im App Service-Editor bearbeiten oder Streaming-Protokolle anzeigen.

###<a name="online-editor"></a>Gewusst wie: Bearbeiten von Code im App Service-Editor

Azure-Portal können Sie Node.js Backend-Skriptdateien in App Service-Editor bearbeiten, ohne das Projekt auf dem lokalen Computer herunterladen. So bearbeiten Sie Skriptdateien in den online-editor

1. Klicken Sie in der Back-End-Blade Mobile-Anwendung auf **Alle** > **einfache Tabellen** oder **Einfach APIs**, einer Tabelle oder einer API klicken **Skript bearbeiten**. Die Skriptdatei wird der App Service-Editor geöffnet.

    ![App Service-Editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Ändern Sie die Datei in der online-Editor. Daten werden während der Eingabe automatisch gespeichert.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Schnelleinstieg Android-Client]: app-service-mobile-android-get-started.md
[Apache Cordova Client Schnellstart]: app-service-mobile-cordova-get-started.md
[iOS-Client Schnellstart]: app-service-mobile-ios-get-started.md
[Xamarin.iOS Client-Schnellstart]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android Client-Schnellstart]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms Client-Schnellstart]: app-service-mobile-xamarin-forms-get-started.md
[Windows Store Client Schnellstart]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[Offline-Daten synchronisieren]: app-service-mobile-offline-data-sync.md
[Azure Active Directory-Authentifizierung konfigurieren]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook-Authentifizierung konfigurieren]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google-Authentifizierung konfigurieren]: app-service-mobile-how-to-configure-google-authentication.md
[Microsoft Authentication konfigurieren]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter-Authentifizierung konfigurieren]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service Deployment Guide]: ../app-service-web/web-sites-deploy.md
[Überwachen von Azure App Service]: ../app-service-web/web-sites-monitor.md
[Diagnoseprotokoll in Azure App Service aktivieren]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Problembehandlung bei einer Azure App Service in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Geben Sie die Knoten-Version]: ../nodejs-specify-node-version-azure-apps.md
[Knoten Module]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Stolz]: http://swagger.io/

[Azure-portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[Basicapp-Beispiel GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[TODO-Beispiel GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[Beispielverzeichnis GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 für Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js-Paket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS-Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston

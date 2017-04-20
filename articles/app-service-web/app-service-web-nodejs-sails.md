<properties
    pageTitle="Bereitstellen Sie Sails.js Web app für Azure App Service"
    description="Informationen Sie zum Bereitstellen einer Anwendung Node.js Azure App Service. Dieses Lernprogramm zeigt Sails.js Web app bereitstellen."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Bereitstellen Sie Sails.js Web app für Azure App Service

Dieses Lernprogramm zeigt eine Sails.js app Azure App Service bereitstellen. Dabei können Sie allgemeine Kenntnisse über Ihre App Service ausgeführt Node.js app konfigurieren sammeln. 

Sie müssen Kenntnisse der Sails.js. In diesem Lernprogramm nicht soll Sie mit Sail.js im Allgemeinen ausgeführt.


## <a name="prerequisites"></a>Erforderliche Komponenten

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Git](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)
- Ein Microsoft Azure-Konto. Haben Sie ein Konto, können Sie [sich für eine kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F) oder [die Visual Studio-Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Azure App Service in Aktion vor der Anmeldung für ein Azure-Konto finden Sie unter [App](http://go.microsoft.com/fwlink/?LinkId=523751)-Dienst versuchen. Dort sofort können eine kurzlebige Starter-app in App Service – keine Kreditkarte, keine Zusagen.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Schritt 1: Erstellen einer Sails.js Anwendung lokal

Erstellen Sie schnell zunächst eine standardmäßige Sails.js app in Umgebung folgendermaßen:

1. Öffnen Sie das Befehlszeile Terminal Ihrer Wahl und `CD` in ein Arbeitsverzeichnis.

2. Sails.js-Anwendung erstellen und ausführen:

        sails new <appname>
        cd <appname>
        sails lift

    Stellen Sie sicher, dass Sie die Standard-Homepage unter Http://localhost:1377 navigieren können.

## <a name="step-2-create-the-azure-app-resource"></a>Schritt 2: Erstellen der Ressource Azure-Anwendung

Erstellen Sie dann die App-Dienstressource in Azure. Du wirst Sails.js app später darauf bereitstellen.

1. Melden Sie sich bei Azure wie so:
1. In demselben Terminal ASM Modus ändern und Azure anmelden:

        azure config mode asm
        azure login

    Folgen Sie der Aufforderung weiterhin den Benutzernamen in einem Browser mit einem microsoftkonto, das Ihr Azure-Abonnement verfügt.

2. Stellen Sie sicher, dass Sie weiterhin im Stammverzeichnis des Projekts Sails.js. Erstellen Sie die App-Dienstressource app in Azure mit einer eindeutigen Anwendungsname den nächsten Befehl. Ihre Web-app-URL ist http://&lt;Anwendungsname >. *.azurewebsites.NET.

        azure site create --git <appname>

    Führen Sie die Aufforderung Azure Region bereitstellen zu wählen. Wenn Sie für Ihre Azure-Abonnement nie Git/FTP-Bereitstellung Anmeldeinformationen eingerichtet haben, werden Sie auch aufgefordert erstellen.

    Sobald die app App Dienstressource erstellt:

    - Sails.js app ist Git initialisiert
    - Das lokale Repository Git initialisiert verbunden ist, der neuen App Service-app als eine remote Git Namen "Azure" und
    - Und iisnode.yml-Datei im Root-Verzeichnis erstellt. Diese Datei können Sie [Iisnode](https://github.com/tjanczuk/iisnode)konfigurieren die App Service ausgeführt Node.js apps verwendet.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Schritt 3: Konfigurieren Sie und Bereitstellen Sie Ihrer Anwendung Sails.js

 Mit einer Sails.js Anwendung in App Service besteht aus drei Hauptschritten:

 - Konfigurieren Sie Ihre Anwendung für diese App Service ausgeführt
 - App-Service bereitstellen
 - Lesen, Stdout und Stderr Protokolle Bereitstellungsprobleme beheben

Gehen Sie folgendermaßen vor:

1. Öffnen Sie die neue iisnode.yml-Datei im Stammverzeichnis und fügen Sie die beiden folgenden Zeilen hinzu:

        loggingEnabled: true
        logDirectory: iisnode

    Protokollierung ist jetzt für Iisnode aktiviert. Weitere Hinweise hierzu finden Sie unter  [Stdout und Stderr Protokolle von Iisnode](app-service-web-nodejs-get-started.md#iisnodelog).

2. Öffnen Sie zum Konfigurieren Ihrer produktiven Umgebung und config/env/production.js `port` und `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Dokumentation für diese Konfigurationen finden in der  [Dokumentation zu Sails.js](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Anschließend müssen Sie sicherstellen, dass [schwierige](https://www.npmjs.com/package/grunt) Azure Netzlaufwerke vereinbar ist. Schwierige Versionen kleiner 1.0.0 verwendet eine veraltete [Glob](https://www.npmjs.com/package/glob) Paket (unter 5.0.14) Netzlaufwerke nicht unterstützt. 

3. Package.json öffnen und ändern die `grunt` Version `1.0.0` und alle `grunt-*` Pakete. Die `dependencies` -Eigenschaft sollte wie folgt aussehen:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. Fügen Sie folgende package.json, `engines` -Eigenschaft Node.js-Version auf eine, die wir wollen.

        "engines": {
            "node": "6.6.0"
        },

6. Speichern Sie und Testen Sie Ihre Änderungen um sicherzustellen, dass Ihre Anwendung lokal läuft. Löschen Sie dazu die `node_modules` , und führen Sie dann:

        npm install
        sails lift

4. Jetzt mit der Git Ihrer Anwendung in Azure bereitstellen:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Schließlich starten Sie Ihre live Azure app im Browser einfach:

        azure site browse

    Die gleichen Sails.js Homepage sollte jetzt angezeigt werden.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Problembehandlung bei der Bereitstellung

Finden Sie die Sails.js-Anwendung in App Service fehlschlägt, Stderr Protokolle zu Problembehandlung.
Weitere Informationen finden Sie unter [Stdout und Stderr Protokolle von Iisnode](app-service-web-nodejs-sails.md#iisnodelog).
Wenn erfolgreich gestartet wurde, sollten Stdout Protokoll die vertraute Meldung angezeigt:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Granularität der Stdout-Protokolle in der Datei [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) steuern. 

## <a name="connect-to-a-database-in-azure"></a>Verbinden Sie mit einer Datenbank in Azure

Zum Verbinden mit einer Datenbank in Azure die Datenbank Ihrer Wahl in Azure Azure SQL-Datenbank MySQL, MongoDB, Azure (Redis) Cache, usw. erstellen und verwenden Sie den entsprechenden [Datenspeicher Adapter](https://github.com/balderdashy/sails#compatibility) herstellen. Die Schritte in diesem Abschnitt veranschaulichen die MySQL-Datenbank in Azure herstellen.

1. Führen Sie das Tutorial [hier](../store-php-create-mysql-database.md) eine MySQL-Datenbank in Azure erstellen.

2. Installieren Sie über die Befehlszeile Terminal MySQL-Adapter:

        npm install sails-mysql --save

3. Config/connections.js öffnen und das folgende Verbindungsobjekt hinzufügen: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Für jede Umgebungsvariable (`process.env.*`), müssen Sie es in App Service. Dazu führen Sie folgende Befehle aus der Terminaldienste. Alle Verbindungsinformationen müssen im Azure-Portal ist (siehe [mit der MySQL-Datenbank](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Azure-Anwendung Einstellung Einstellungen ins speichert vertrauliche Daten aus der Versionskontrolle (Git). Als Nächstes konfigurieren Sie Umgebung um die Verbindungsinformationen zu verwenden.

4. Öffnen Sie config/local.js und fügen Sie folgende Verbindungsobjekt:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Diese Konfiguration überschreibt die Einstellung in der Datei config/connections.js für die Umgebung. Diese Datei wird vom Standard-.gitignore in Ihrem Projekt ausgeschlossen, so dass nicht Git gespeichert wird. Jetzt können Sie Ihre MySQL-Datenbank Ihrer Azure Anwendung sowohl lokale Umgebung herstellen.

4. Config/env/production.js Konfigurieren Ihrer produktiven Umgebung öffnen, und fügen Sie den folgenden `models` Objekt:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Öffnen Sie config/env/development.js Umgebung konfigurieren, und fügen Sie den folgenden `models` Objekt:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`können Sie Datenbankfunktionen Migration zu Datenbanktabellen in Ihre MySQL einfach. Allerdings `migrate: 'safe'` für Ihre Umgebung Azure (Produktion) verwendet, da Sails.js nicht verwendet werden können `migrate: 'alter'` in einer (siehe  [Dokumentation zu Sails.js](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. Das Terminal [generieren](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) eine Sails.js [Vorlage API](http://sailsjs.org/documentation/concepts/blueprints) wie normalerweise würde, führen `sails lift` Sails.js Datenbank Migration die Datenbank erstellen. Zum Beispiel:

         sails generate api mywidget
         sails lift

    Die `mywidget` von diesem Befehl generierte Modell ist leer, aber wir können zeigen, dass wir Datenbankkonnektivität.
    Beim Ausführen von `sails lift`, fehlenden Tabellen für die Modelle erstellt Ihre app verwendet.

6. Zugriff auf den Plan API direkt im Browser erstellt. Zum Beispiel:

        http://localhost:1337/mywidget/create
    
    Die API sollte erstellten Eintrag zurück im Browserfenster zurück, sodass die Datenbank erfolgreich erstellt wurde.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Jetzt drücken Sie ändern in Azure, und wechseln Sie zu Ihrer Anwendung, um sicherzustellen, dass es funktioniert immer noch.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Die Blaupause API Ihrer Azure Anwendung zugreifen. Zum Beispiel:

        http://<appname>.azurewebsites.net/mywidget/create

    Kehrt die API einen weiteren neuen Eintrag spricht Ihrer Azure Anwendung auf Ihre MySQL-Datenbank.

## <a name="more-resources"></a>Weitere Ressourcen

- [Erste Schritte mit Node.js webapps in Azure App Service](app-service-web-nodejs-get-started.md)
- [Node.js-Module verwenden mit Azure](../nodejs-use-node-modules-azure-apps.md)

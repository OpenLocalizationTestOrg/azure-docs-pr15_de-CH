<properties
    pageTitle="Node.js-API-Anwendung in Azure App Service | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer RESTful API Node.js und API-App in Azure App Service bereitstellen."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Erstellen einer RESTful API Node.js und API-App in Azure bereitstellen

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Dieses Lernprogramm zeigt das Erstellen einer einfachen [Node.js](http://nodejs.org) API und [API-app](app-service-api-apps-why-best-platform.md) in [Azure App Service](../app-service/app-service-value-prop-what-is.md) mit [Git](http://git-scm.com)bereitstellen. Jedes Betriebssystem, das ausgeführt Node.js können verwenden und Sie alle Ihre Arbeit mit Befehlszeilentools wie cmd.exe oder bash.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Microsoft Azure-Konto ([Öffnen Sie ein kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/))
1. [Node.js](http://nodejs.org) installiert (in diesem Beispiel wird vorausgesetzt, dass Sie Node.js Version 4.2.2)
2. [Git](https://git-scm.com/) installiert
1. [GitHub](https://github.com/) Konto

App Service unterstützt vielfältige Code eine API-app Bereitstellen dieses Lernprogramm zeigt die Git und setzt grundlegende Kenntnisse zum Arbeiten mit Git. Informationen zu anderen Bereitstellungsmethoden finden Sie [Ihre app Azure App Service bereitstellen](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>Den Beispielcode zu erhalten

1. Öffnen Sie eine Befehlszeilenschnittstelle, die Node.js und Git Befehle ausführen können.

1. Navigieren Sie zu einem Ordner, den Sie für eine lokale Git Repository und Clone [GitHub Repository mit dem Beispielcode](https://github.com/Azure-Samples/app-service-api-node-contact-list)verwenden.

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    Die Beispiel-API bietet zwei Endpunkte: eine Get-Anforderung an `/contacts` gibt eine Liste von Namen und Adressen im JSON-Format beim `/contacts/{id}` gibt den ausgewählten Kontakt.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Gerüst (automatisch generiert) Node.js-Code auf der Grundlage stolz Metadaten

[Swagger](http://swagger.io/) ist ein Dateiformat für Metadaten zur Beschreibung einer RESTful API. Azure App Service bietet [integrierte Unterstützung für stolz Metadaten](app-service-api-metadata.md). In diesem Abschnitt des Lernprogramms Modelliert einen API-Entwicklungsworkflow mit stolz Metadaten zuerst erstellen und mithilfe dieses Gerüst (automatisch generiert) Servercode für die API. 

>[AZURE.NOTE] Wenn Sie Node.js-Code aus einer Metadatendatei stolz Gerüst erfahren möchten, können Sie diesen Abschnitt überspringen. Wenn Sie nur Beispielcode für eine neue API-app bereitstellen möchten, werden Sie direkt im Abschnitt [Erstellen einer API-app in Azure](#createapiapp) .

### <a name="install-and-execute-swaggerize"></a>Installieren und Ausführen von Swaggerize

1. Führen Sie die folgenden Befehle die **yo** **Generator swaggerize** NPM Module und Global installieren.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize ist eine Tool, das für eine API beschrieben eine Metadatendatei stolz Servercode generiert. Stolz-Datei, die Sie verwenden, heißt *api.json* und befindet sich im Ordner *start* des Repositorys, die Sie geklont.

2. Navigieren Sie zum Ordner *zu starten* , und führen Sie das `yo swaggerize` Befehl. Swaggerize wird eine Reihe von Fragen.  **Zu diesem Projekt**"Kontaktliste" Pfad **zum Dokument swagger**geben, "api.json", und "express" **Express, glücklich oder Restify**eingeben.

        yo swaggerize

    ![Swaggerize-Befehlszeile](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Hinweis**: Wenn Sie einen in diesem Schritt Fehler als Nächstes erläutert zu korrigieren.

    Swaggerize erstellt einen Anwendungsordner, Gerüste Handler und Konfigurationsdateien und generiert eine Datei **package.json** . Express-Ansichtsmodul dient zum Generieren der Hilfeseite stolz.  

3. Wenn die `swaggerize` Befehl schlägt fehl mit Fehlermeldung "Ungültige Escapesequenz" oder "unerwartetes Token", korrigieren Sie die Ursache des Fehlers durch Bearbeiten der Datei generierten *package.json* . In der `regenerate` Linie unter `scripts`, den umgekehrte Schrägstrich, der *api.json* ein Schrägstrich vorausgeht ändern, sodass die Zeile folgendermaßen aussieht:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Navigieren Sie zu dem Ordner mit den erstellten Code (in diesem Fall den Unterordner */start/ContactList* ).

1. Führen Sie `npm install`.
    
        npm install
        
2. Installieren der **Jsonpath** NPM. 

        npm install --save jsonpath
        
    ![Jsonpath installieren](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. Installieren der **swaggerize Benutzeroberfläche** NPM. 

        npm install --save swaggerize-ui
        
    ![Swaggerize Benutzeroberflächen installieren](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>Anpassen von erstellten code

1. Kopieren Sie **Lib** -Ordner aus dem Ordner **start** in der gerüstbauer erstellt **ContactList** -Ordner 

1. Ersetzen Sie den Code in der Datei **handlers/contacts.js** durch den folgenden Code. 

    Dieser Code verwendet die JSON-Daten in der Datei **lib/contacts.json** , die durch **lib/contactRepository.js**bereitgestellt. Der neue contacts.js Code reagiert auf HTTP-Anfragen, alle Kontakte und als JSON-Nutzlast. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. Ersetzen Sie den Code in der Datei **handlers/contacts/{id}.js** mit dem Fofllowing Code. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. Ersetzen Sie den Code in **server.js** durch folgenden Code. 

    Die geänderte Datei server.js sind aufgerufen Kommentare der Änderung zu sehen. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>Mit der API lokal testen

1. Mit dem Befehlszeilendienstprogramm Node.js Server aktivieren. 

        node server.js

1. Wenn Sie **http://localhost: 8000/Kontakte**suchen, erhalten Sie die JSON-Ausgabe der Kontaktliste oder aufgefordert, je nach Browser downloaden. 

    ![Alle Kontakte API-Aufruf](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Wenn Sie **http://localhost: 8000/Kontakte/2**suchen, sehen Sie den Kontakt, Id-Wert.

    ![Bestimmte Kontaktperson API-Aufruf](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Swagger JSON-Daten werden **über/swagger** Endpunkt bereitgestellt:

    ![Kontakte stolz Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Swagger-Benutzeroberfläche wird über den **/docs** -Endpunkt. Rich HTML-Client-Funktionen können Sie in der Benutzeroberfläche des Swagger API testen.

    ![Swagger-Benutzeroberfläche](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Neue API-App erstellen

In diesem Abschnitt verwenden Sie Azure-Portal eine neue API-App in Azure erstellen. Diese API-app stellt die Serverressourcen Azure Code ausführen kann. In späteren Abschnitten werden Sie Code in der neuen API-app bereitstellen.

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/). 

1. Klicken Sie auf **Neu > Web + Mobile > API-App**. 

    ![Neue API-app im portal](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Geben Sie einen **Anwendungsname** , der in der Domäne **.azurewebsites.NET* wie NodejsAPIApp plus eine Zahl zu eindeutigen eindeutig ist. 

    Beispielsweise wird der Name `NodejsAPIApp`, der URL `nodejsapiapp.azurewebsites.net`.

    Wenn Sie einen Namen, den einem anderen Benutzer bereits verwendet wurde eingeben, wird rechts ein rotes Ausrufezeichen angezeigt.

6. In der Dropdownliste **Ressourcengruppe** auf **neu**und dann in **Name für neue Ressourcengruppe** Geben Sie ein "NodejsAPIAppGroup" oder einen anderen Namen Sie. 

    Eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) ist eine Sammlung von Azure Ressourcen wie API-apps, Datenbanken und VMs. Für dieses Lernprogramm empfiehlt es sich, eine neue Ressourcengruppe erstellen, da können sie leicht in einem Schritt alle Azure-Ressourcen löschen, die Sie für das Lernprogramm erstellen.

4. Klicken Sie auf **App Plan/Speicherort**und klicken Sie dann auf **Neu erstellen**.

    ![App Service-Plan erstellen](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    In den folgenden Schritten erstellen Sie einen App Service-Plan für die neue Ressourcengruppe. Ein App Service-Plan gibt die API-app läuft auf Serverressourcen. Beispielsweise wählt die freie Ebene während der Ausführung API-app auf freigegebenen VMs für einige bezahlte Tiers auf dedizierten VMs Ausführung. Informationen zu App Service-Pläne Übersicht [App Service-Pläne](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. Blatt **App Service-Plan** Geben Sie ein "NodejsAPIAppPlan" oder einen anderen Namen Sie.

5. Wählen Sie in der Dropdown-Liste **Speicherort** den Speicherort, die Sie.

    Diese Einstellung gibt die Azure-Rechenzentrum Ihre Anwendung ausgeführt wird. In diesem Lernprogramm wird kein Region auswählen und es einen Unterschied. Aber für eine produktionsanwendung Ihre Server so nah wie möglich an die Clients, die diese [Wartezeit](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)minimieren zugreifen.

5. Klicken Sie auf **Preisstufe > Alle anzeigen > F1 kostenlose**.

    Für dieses Lernprogramm bietet kostenlose Tarif ausreichende Leistung.

    ![Wählen Sie frei Preisstufe](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. Das Blade **App Service-Plan** klicken Sie auf **OK**.

7. Klicken Sie auf das Blade **API-App** **Erstellen**.

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Die neue API-app Git Bereitstellung einrichten

Sie werden die API-app pushen Commits ein Git Repository in Azure App Service Code bereitstellen. In diesem Abschnitt des Lernprogramms erstellen Sie Anmeldeinformationen und Git Repository in Azure, die Sie für die Bereitstellung verwenden.  

1. Klicken Sie nach dem Erstellen die API-app **Anwendungsdienste > {API app}** von der Homepage des Portals. 

    Die **API-App** und **Einstellungen** -Blades angezeigt.

    ![Portal API-app und Einstellungen-Blades](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. Blatt **Einstellungen** Scrollen Sie Abschnitt **Veröffentlichen** , und klicken Sie auf **Bereitstellung Anmeldeinformationen**.
 
3. Blatt **Bereitstellung Benutzerinformationen** Geben Sie Benutzernamen und Kennwort ein und dann auf **Speichern**.

    Verwenden Sie diese Anmeldeinformationen für die Veröffentlichung von Node.js Code für Ihre API-Anwendung. 

    ![Anmeldeinformationen für die Bereitstellung](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. Blatt **Einstellungen** klicken Sie auf **bereitstellungsquelle > Quelle auswählen > Git-Repository**, klicken Sie auf **OK**.

    ![Git Repo erstellen](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Wenn Ihr Repository Git Blade ändert zum Anzeigen der aktiven Bereitstellung erstellt wurde. Da das Repository neu ist, müssen Sie in der Liste keine aktive Bereitstellung. 

    ![Keine aktive Bereitstellung](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Kopieren Sie Git Repository-URL Hierzu an die Blade für Ihre neue API-App navigieren Sie und sehen Sie die **Grundlagen** des Blades. **Git Clone URL** im Abschnitt **Essentials** beachten. Beim bewegen über diese URL sehen Sie ein Symbol auf der rechten Seite, die den URL in die Zwischenablage kopieren. Klicken Sie auf dieses Symbol, um die URL zu kopieren.

    ![Abrufen des URLs Git aus dem Portal](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Hinweis**: benötigen Sie Git Clone URL im nächsten Abschnitt stellen Sie daher sicher irgendwo im Moment zu speichern.

Jetzt haben Sie eine API App mit einem Git Repository sichern, schieben Sie Code in das Repository API-app den Code bereitstellen. 

## <a name="deploy-your-api-code-to-azure"></a>API Code in Azure bereitstellen

In diesem Abschnitt erstellen Sie ein lokales Repository Git, das der Servercode zur API enthält, und Sie drücken Code aus Repository Repository in Azure, die Sie zuvor erstellt haben.

1. Kopieren der `ContactList` Ordner, die Sie für ein neues lokales Git Repository verwenden können. Wenn Sie den ersten Teil des Lernprogramms haben, kopieren `ContactList` aus der `start` Ordner; Andernfalls kopieren Sie `ContactList` aus der `end` Ordner.

1. Ihre Befehlszeilentool den neuen Ordner, und führen Sie den folgenden Befehl zum Erstellen eines neuen lokalen Git Repository. 

        git init

     ![Neue lokale Git Repo](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Führen Sie den folgenden Befehl einen Git remote für Ihre API-app-Repository hinzufügen. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Hinweis**: Ersetzen Sie die Zeichenfolge "YOUR_GIT_CLONE_URL_HERE" mit Ihren eigenen Git Clone-URL, die Sie zuvor kopiert. 

1. Führen Sie die folgenden Befehle Commit erstellen, der die Code enthält. 

        git add .
        git commit -m "initial revision"

    ![Git Commit-Ausgabe](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Führen Sie den Befehl Code in Azure übertragen. Wenn Sie ein Kennwort aufgefordert werden, geben Sie in Azure-Portal erstellt.

        git push azure master

    Dadurch wird die Bereitstellung für Ihre API-Anwendung.  

1. In Ihrem Browser **Bereitstellung** Blade für Ihre API navigieren und Sie sehen, dass die Bereitstellung erfolgt. 

    ![Bereitstellung passiert](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Die Befehlszeilenschnittstelle stellt den Status der Bereitstellung gleichzeitig während es geschieht. 

    ![Knoten Js-Bereitstellung passiert](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Nach Abschluss die Bereitstellung gibt **Bereitstellung** Blade die erfolgreiche Bereitstellung von geändertem Code Ihrer API-App. 

## <a name="test-with-the-api-running-in-azure"></a>Mit der API im Azure testen
 
3. Kopieren Sie die **URL** im Abschnitt **Grundlagen** der API App-Blade. 

    ![Bereitstellung abgeschlossen](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. Mit einem REST API-Client oder Postbote oder Fiddler (Webbrowser), geben Sie die URL Ihrer Kontakte API-Aufruf wird die `/contacts` Endpunkt der API-app. Der URL`https://{your API app name}.azurewebsites.net/contacts`

    Ausgeben eine GET-Anforderung an diesen Endpunkt erhalten Sie die JSON-Ausgabe der API-app.

    ![Postbote Api auf](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. Wechseln Sie in einem Browser zu der `/docs` Endpunkt Swagger Benutzeroberfläche testen wie in Azure ausgeführt wird.

Nun, kontinuierlichen Bereitstellung eingerichtet haben, können Sie Code ändern und einfach indem Commits auf Azure Git-Repository in Azure bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt haben Sie erfolgreich erstellt eine API App und Node.js-API Code bereitgestellt. Die nächsten Lernprogramm zeigt wie [JavaScript-Clients mit CORS API-apps genutzt](app-service-api-cors-consume-javascript.md).

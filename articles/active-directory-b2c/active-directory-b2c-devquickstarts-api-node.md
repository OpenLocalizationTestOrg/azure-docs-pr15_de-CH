<properties
    pageTitle="Azure AD B2C: Secure Web API mit Node.js | Microsoft Azure"
    description="Wie Node.js-Web-API, die Token B2C Mieter akzeptiert"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Secure Web API mit Node.js

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mit Azure Active Directory (Azure AD) B2C sichern Sie eine Web-API mit OAuth 2.0 Zugriffstoken. Diese Token können Ihre Clientanwendungen, die Azure AD B2C verwendet die API authentifizieren. Dieser Artikel beschreibt, wie eine API "Aufgabenliste" erstellen, können Benutzer hinzufügen und Aufgaben. Web-API mit Azure AD B2C gesichert ist und nur authentifizierten Benutzern ihrer Aufgabenliste verwalten.

> [AZURE.NOTE]  In diesem Beispiel wurde geschrieben, mithilfe unserer [iOS B2C Anwendung](active-directory-b2c-devquickstarts-ios.md)verbunden werden. Führen Sie aktuelle Anleitung zuerst, und führen Sie dann mit dem Beispiel.

**Passport** ist Middleware für Node.js Authentifizierung. Flexibel und Modular Passport kann als installiert in einer Express-basierte oder Restify Anwendung. Umfassende Strategien unterstützt die Authentifizierung mit einem Benutzernamen und Kennwort, Facebook und Twitter. Wir haben eine Strategie für Azure Active Directory (Azure AD) entwickelt. Dieses Modul und fügen Sie Azure AD `passport-azure-ad` -Plug-in.

In diesem Beispiel hierzu müssen:

1. Registrieren einer Anwendung in Azure AD.
2. Richten Sie die Anwendung für Passport `azure-ad-passport` -Plug-in.
3. Konfigurieren einer Clientanwendung "Aufgabenliste" Web API aufrufen.


## <a name="get-an-azure-ad-b2c-directory"></a>Ein Azure AD B2C-Verzeichnis abrufen

Vor der Verwendung Azure AD B2C müssen Sie ein Verzeichnis oder Mieter.  Ein Verzeichnis ist ein Container für alle Benutzer, apps und Gruppen.  Haben Sie eine bereits Fortfahren [ein B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Anschließend müssen Sie eine Anwendung in Ihrem B2C-Verzeichnis erstellen, Azure AD Informationen gibt, die sichere Kommunikation mit Ihrer app muss. In diesem Fall werden die Clientanwendung und Web-API durch eine einzelne **Anwendung ID**dargestellt, da eine logische app bestehen aus. [Gehen folgendermaßen Sie vor, um eine Anwendung zu erstellen.](active-directory-b2c-app-registration.md) Achten Sie darauf,:

- Enthalten ein **web-app-Web api** in der Anwendung
- Geben Sie `http://localhost/TodoListService` als **Antwort-URL**. Es ist die Standard-URL für dieses Codebeispiel.
- Erstellen Sie einen **geheimen Schlüssel** für Ihre Anwendung, und kopieren Sie sie. Diese Daten benötigen später. Beachten Sie, dass dieser Wert [XML mit Escapezeichen versehen](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) werden, bevor Sie sie verwenden.
- Kopieren Sie die **ID der Anwendung** , die Ihre Anwendung zugewiesen ist. Diese Daten benötigen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen von Richtlinien

In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese Anwendung enthält zwei Identität Erfahrungen: registrieren und anmelden. Sie müssen eine Richtlinie jedes Typs erstellen gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Wenn Sie die drei Richtlinien erstellen, müssen Sie:

- Wählen Sie in der Registrierungsrichtlinie **Anzeigename** und andere Anmeldung Attribute.
- Ansprüche Anwendung **angezeigten Namen** und die **Objekt-ID** in jeder Richtlinie auswählen  Sie können auch andere Ansprüche.
- Notieren Sie den **Namen** jeder Richtlinie nach ihrer Erstellung. Das Präfix muss `b2c_1_`.  Diese Richtliniennamen benötigen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellt haben, können Sie Ihre app erstellen.

Informationen über die Funktionsweise von Richtlinien in Azure AD B2C mit [.NET Web app Einführung](active-directory-b2c-devquickstarts-web-dotnet.md)beginnen.

## <a name="download-the-code"></a>Den Code herunterladen

Der Code für dieses Lernprogramm [auf GitHub verwaltet](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Zum Beispiel wie Sie, können Sie [ein Skelett Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Sie können auch das Skelett Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Die abgeschlossene Anwendung wird auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) oder die `complete` dasselbe Repository-Zweig.

## <a name="download-nodejs-for-your-platform"></a>Node.js für Ihre Plattform herunterladen

Um dieses Beispiel erfolgreich verwenden zu können, benötigen Sie eine funktionierende Installation Node.js. 

Installieren Sie Node.js von [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>MongoDB für Ihre Plattform installieren

Um dieses Beispiel erfolgreich verwenden zu können, benötigen Sie eine funktionierende Installation von MongoDB. Wir verwenden MongoDB zu REST API persistente Server-Instanzen.

Installieren Sie MongoDB von [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Diese exemplarische Vorgehensweise wird vorausgesetzt, dass Installation und Standardendpunkte MongoDB, verwenden Sie zum Zeitpunkt der Erstellung dieses Dokuments ist `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Installieren Sie die Restify-Module in Ihrem Web API

Wir verwenden Restify, um die REST-API erstellen. Restify ist ein minimaler und flexible Node.js-Anwendungsframework von Express abgeleitet. Es verfügt über einen robusten Satz von Funktionen zum Erstellen von REST-APIs auf Verbinden.

### <a name="install-restify"></a>Restify installieren

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`. Wenn die `azuread` Verzeichnis nicht vorhanden ist, erstellen Sie ihn.

`cd azuread`oder`mkdir azuread;`

Geben Sie den folgenden Befehl ein:

`npm install restify`

Dieser Befehl installiert Restify.

#### <a name="did-you-get-an-error"></a>Haben Sie einen Fehler?

Bei einigen Betriebssystemen Verwendung `npm`, Sie erhalten die Fehlermeldung `Error: EPERM, chmod '/usr/local/bin/..'` und Sie das Konto als Administrator ausführen. Wenn dieses Problem auftritt, verwenden Sie die `sudo` Befehl ausführen `npm` auf einer höheren Berechtigungsebene.

#### <a name="did-you-get-a-dtrace-error"></a>Haben Sie einen DTrace-Fehler?

Sie sehen möglicherweise etwas Text Restify installieren:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify stellt ein leistungsstarken Mechanismus zum Verfolgen von REST mit DTrace aufgerufen. Viele Betriebssysteme haben jedoch nicht DTrace verfügbar. Sie können diese Fehler ignorieren.

Die Ausgabe des Befehls sollte dieser Text ähneln:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Installieren von Passport in der Web-API

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist.

Installieren Sie Passport mit dem folgenden Befehl:

`npm install passport`

Die Ausgabe des Befehls sollte dieser Text ähnlich sein:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Ihr Web API Azuread Passport hinzufügen

Fügen Sie die OAuth-Strategie mit `passport-azuread`, eine Reihe von Strategien, die Azure AD mit Passport verbunden. Verwenden Sie diese Strategie für trägertoken im REST API-Beispiel.

> [AZURE.NOTE] Obwohl OAuth2 ein Framework mit bekannten Tokentyp ausgestellt werden kann bietet, haben nur bestimmte Tokentypen weit verbreitet. Die Token für den Schutz von Endpunkten sind trägertoken. Diese Token werden am häufigsten ausgestellten in OAuth2. Viele Implementierungen angenommen, trägertoken nur Token ausgestellt sind.

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist.

Installieren den Passport `passport-azure-ad` Modul mit dem folgenden Befehl:

`npm install passport-azure-ad`

Die Ausgabe des Befehls sollte dieser Text ähnlich sein:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Der Web-API MongoDB Module hinzufügen

MongoDB verwendet als Datenspeicher. Für eine weit verbreitete Mungo installieren plug-in für Modelle und Schemas.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Installieren Sie zusätzliche Module

Installieren Sie dann die übrigen erforderlichen Module.

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist:

`cd azuread`

Installieren Sie die Module in Ihre `node_modules` Verzeichnis:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Erstellen Sie eine server.js-Datei mit der Abhängigkeit

Die `server.js` Datei enthält den Großteil der Funktionalität für Ihren Web-API-Server. 

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist:

`cd azuread`

Erstellen einer `server.js` Datei in einem Editor. Fügen Sie die folgende Informationen:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Speichern Sie die Datei. Sie zurückkehren später zu diesem.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Erstellen Sie eine config.js-Datei zum Speichern von Einstellungen Azure AD

Diese Codedatei übergibt die Konfigurationsparameter der Azure AD-Portal die `Passport.js` Datei. Diese Konfigurationswerte wird erstellt, wenn die Web-API an das Portal im ersten Teil der exemplarischen Vorgehensweise hinzugefügt. Erläutert, was die Werte dieser Parameter nach den Code zu kopieren.

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist:

`cd azuread`

Erstellen einer `config.js` Datei in einem Editor. Fügen Sie die folgende Informationen:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Erforderliche Werte

`clientID`Die Client-ID der Anwendung Web-API.

`IdentityMetadata`: Hierbei `passport-azure-ad` für die Konfigurationsdaten der Identitätsanbieter sucht. Es sucht auch nach Schlüssel JSON Web Token zu bestätigen. 

`audience`: Der uniform Resource Identifier (URI) aus dem Portal, das die aufrufende Anwendung identifiziert. 

`tenantName`Ihr Mieter Name (z. B. **contoso.onmicrosoft.com**).

`policyName`Die Richtlinie auf dem Server Token zu bestätigen. Diese Richtlinie sollte dieselbe Richtlinie, die die Clientanwendung-Anmeldung verwenden.

> [AZURE.NOTE] Verwenden Sie für unsere B2C-Vorschau dieselben Richtlinien auf Client und Server Setup. Wenn Sie bereits eine Anleitung abgeschlossen und diese Richtlinien erstellt haben, müssen Sie erneut. Da Sie die exemplarische Vorgehensweise abgeschlossen haben, sollte nicht müssen neue Richtlinien für Client Exemplarische Vorgehensweisen auf der Website einrichten.

## <a name="add-configuration-to-your-serverjs-file"></a>Konfiguration der Datei server.js hinzufügen

Zum Lesen der Werte aus der `config.js` Datei haben, fügen Sie die `.config` Datei als Ressource in der Anwendung, und legen Sie die globalen Variablen zu den `config.js` Dokument.

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist:

`cd azuread`

Öffnen der `server.js` -Datei in einem Editor. Fügen Sie die folgende Informationen:

```Javascript
var config = require('./config');
```
Fügt einen neuen Abschnitt zu `server.js` den folgenden Code enthält:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Als Nächstes fügen Sie Platzhalter für Benutzer wir unsere aufrufende Anwendung erhalten.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Nun, und unsere Protokollierung zu erstellen.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Fügen Sie die MongoDB-Modell und Schema mit Mongoose hinzu

Frühere Vorbereitung zahlt sich aus, wie Sie diese drei Dateien in einen REST-API-Dienst.

Verwenden Sie für diese exemplarische Vorgehensweise MongoDB speichern Ihre Aufgaben wie zuvor beschrieben.

In der `config.js` -Datei der Datenbank **Tasklist**aufgerufen. Dieser Name wurde auch was Sie am Ende der `mongoose_auth_local` Verbindungs-URL. Sie müssen diese Datenbank vorher MongoDB erstellen. Es erstellt die Datenbank bei der ersten Ausführung der Serveranwendung.

Nachdem Sie dem Server die zu verwendende Datenbank MongoDB mitteilen, müssen Sie zusätzlichen Code Model und Schema Serveraufgaben schreiben.

### <a name="expand-the-model"></a>Erweitern Sie das Modell

Dieses Schema ist einfach. Sie können nach Bedarf erweitert werden.

`owner`: Die Aufgabe zugewiesen wird. Dieses Objekt ist eine **Zeichenfolge**.  

`Text`Die Aufgabe selbst. Dieses Objekt ist eine **Zeichenfolge**.

`date`: Das Datum, das die Aufgabe fällig ist. Dieses Objekt ist **Datetime**.

`completed`Wenn der Vorgang abgeschlossen ist. Dieses Objekt ist **Boolean**.

### <a name="create-the-schema-in-the-code"></a>Erstellen Sie das Schema im Code.

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist:

`cd azuread`

Öffnen der `server.js` -Datei in einem Editor. Fügen Sie Folgendes unter den Konfigurationseintrag hinzu:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Erstellen des Schemas, und erstellen Sie ein Modellobjekt, das Sie zum Speichern Ihrer Daten im gesamten Code beim Definieren von **Routen**verwenden.

## <a name="add-routes-for-your-rest-api-task-server"></a>Routen für die REST API Taskserver hinzufügen

Jetzt haben Sie ein Datenbankmodell arbeiten, fügen Sie Routen für den REST API-Server verwenden.

### <a name="about-routes-in-restify"></a>Routen in Restify

Routen funktionieren in Restify auf die gleiche Weise, die sie bei der Express-Stack arbeiten. Routen zu definieren, mit dem URI die Clientanwendungen erwartet aufrufen. 

Eine Regel für eine Route Restify ist:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify und Express bietet viele Funktionen tiefer Anwendungstypen definieren und Ausführen komplexer routing über verschiedene Endpunkte. Für die Zwecke dieses Lernprogramms halten wir diese Routen einfach.

#### <a name="add-default-routes-to-your-server"></a>Standard-Routen zum Server hinzufügen

Fügen Sie jetzt die grundlegenden CRUD Routen ** **Erstellen** und** für unsere REST-API. Andere Routen finden Sie der `complete` Zweig der Probe.

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist:

`cd azuread`

Öffnen der `server.js` -Datei in einem Editor. Im folgenden fügen der Datenbankeinträge über folgende Informationen:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Fehlerbehandlung für Routen

Fügen Sie einige Fehler behandeln, damit Probleme an den Client so kommunizieren, die verarbeitet werden kann.

Fügen Sie den folgenden Code hinzu:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Erstellen Sie Ihre server

Jetzt Ihre Datenbank definiert und Ihre Routen setzen. Das letzte, was Sie werden Serverinstanz hinzufügen, die Ihre Anrufe verwaltet.

Restify und Express umfassender Anpassung für einen REST-API-Server bereitstellen, aber wir verwenden hier die einfachste Konfiguration. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Hinzufügen von Routen zum Server (ohne Authentifizierung)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Die REST-API-Server Authentifizierung hinzufügen

Jetzt haben Sie einen laufenden REST API-Server machen Sie es gegen Azure AD.

Über die Befehlszeile ändern Sie Ihr Verzeichnis `azuread`, wenn es nicht bereits vorhanden ist:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Mit dem Passport-Azure-Ad OIDCBearerStrategy


> [AZURE.TIP]
Beim Schreiben von APIs sollten Sie immer die Daten einen eindeutigen aus dem Token verknüpfen, die der Benutzer spoofen kann. Wenn der Server Aufgaben speichert, wird es **Oid** des Benutzers im Token (bezeichnet durch token.oid), der im Feld "Besitzer" wechselt. Dieser Wert wird sichergestellt, dass Benutzer ihre eigenen Aufgaben zugreifen kann. Gibt es keine API "Owner", ein externer Benutzer anderer Aufgaben erhalten, auch wenn sie authentifiziert.

Anschließend verwenden Sie die Träger Strategie, die mit `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport verwendet das gleiche Muster für alle Strategien. Geben sie einen `function()` hat `token` und `done` als Parameter. Die Strategie kommt zurück, nachdem es seine Arbeit abgeschlossen hat. Sie sollten den Benutzer speichern und Token speichern, sodass Sie nicht wieder Fragen müssen.

> [AZURE.IMPORTANT]
Der obige Code hat jeder Benutzer, der zur Authentifizierung an den Server geschieht. Dieser Prozess wird als Bildmaßstab bezeichnet. Lassen Sie bei Produktionsservern sich nicht in jedem Benutzerzugriff der API ohne eine Registrierung gehen. Dieser Prozess ist normalerweise das Muster sehen Sie im Consumer apps, mit denen Sie mit Facebook anmelden, aber dann aufgefordert, zusätzliche Informationen. Dieses Programm ein Befehlszeilenprogramm nicht wir konnte die e-Mail extrahieren aus dem token Objekt, die zurückgegeben und dann Benutzer zusätzliche Informationen aufgefordert. Dies ist ein Beispiel hinzufügen wir eine Datenbank im Arbeitsspeicher.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Führen Sie die Serveranwendung zu überprüfen, ob Sie abgelehnt

Sie können `curl` haben Sie nun OAuth2 Schutz gegen die Endpunkte an. Zurückgegebenen Header sollte ausreichen, um Sie auf dem richtigen Weg sind.

Stellen Sie sicher, dass die MongoDB ausgeführt:

    $sudo mongodb

Wechseln Sie zum Verzeichnis, und führen Sie den Server:

    $ cd azuread
    $ node server.js

Ein neues terminal-Fenster Ausführen`curl`

Führen Sie eine einfache Bereitstellung:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Ein 401-Fehler ist die gewünschte Reaktion. Es gibt an, dass die Passport-Ebene zum Autorisieren Endpunkt umleiten.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Sie haben nun einen REST-API-Dienst, der oauth2 verwendet

Sie haben eine REST-API mit Restify und OAuth implementiert. Sie haben nun genügend Code, um Ihren Dienst zu entwickeln und dabei fortgesetzt werden kann. Sie gegangen, soweit Sie mit diesem Server ohne OAuth2-kompatiblen Client können. Verwenden Sie für nächsten Schritt eine zusätzliche Anleitung wie unsere Exemplarische Vorgehensweise [mit einer Web-API mit iOS mit B2C](active-directory-b2c-devquickstarts-ios.md) .


## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt wie fortgeschritteneren Themen verschieben:

[Verbinden Sie mit einer API mit iOS B2C](active-directory-b2c-devquickstarts-ios.md)
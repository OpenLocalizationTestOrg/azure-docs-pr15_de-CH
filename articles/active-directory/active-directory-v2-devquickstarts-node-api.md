<properties
    pageTitle="Azure AD v2. 0 NodeJS Web API | Microsoft Azure"
    description="Erstellen einer NodeJS Web-API akzeptiert sowohl persönliche Microsoft Account-Token und Geschäfts-oder."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="secure-a-web-api-using-nodejs"></a>Sichern Sie eine Web-API mit node.js

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

Azure Active Directory v2. 0-Endpunkt eine Web-API mit [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) Zugriffstoken Anwender persönliche Microsoft-Konto sowohl Arbeit schützen oder Schule Konten sicher auf der Web-API.

**Passport** ist Middleware für Node.js Authentifizierung. Flexibel und modular Passport kann unauffällig gelöscht werden auf jedem Express oder Restify Anwendung. Umfassende Strategien unterstützt Authentifizierung mit Benutzername und Kennwort, Facebook und Twitter. Wir haben eine Strategie für Microsoft Azure Active Directory entwickelt. Wir installieren dieses Modul und fügen Sie die Microsoft Azure Active Directory `passport-azure-ad` -Plug-in.

## <a name="download"></a>Herunterladen
Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs)verwaltet.  Um folgen, können Sie [die Anwendung Skelett als .zip downloaden](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip) oder das Skelett Klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Die fertiggestellte Anwendung wird am Ende dieses Lernprogramms bereitgestellt.


## <a name="1-register-an-app"></a>1. Registrieren einer Anwendung
Erstellen Sie eine neue Anwendung auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)und befolgen Sie diese [Anleitung](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- Kopieren Sie die **Id der Anwendung** für Ihre Anwendung zugewiesen, Sie benötigen rasch.
- **Mobile** Plattform für Ihre Anwendung hinzufügen.
- Kopieren Sie den **URI umleiten** vom Portal aus Verwenden Sie den Standardwert `urn:ietf:wg:oauth:2.0:oob`.


## <a name="2-download-nodejs-for-your-platform"></a>2: node.js für Ihre Plattform herunterladen
Um dieses Beispiel erfolgreich verwenden zu können, benötigen Sie eine funktionierende Installation Node.js.

Installieren Sie Node.js von [http://nodejs.org](http://nodejs.org).

## <a name="3-install-mongodb-on-to-your-platform"></a>3: Installation MongoDB auf der Plattform

Um dieses Beispiel erfolgreich verwenden zu können, benötigen Sie eine funktionierende Installation von MongoDB. MongoDB verwendet wir unsere Autorisierungstypen REST-API über Server-Instanzen erstellen.

Installieren Sie MongoDB von [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] In dieser exemplarischen Vorgehensweise wird vorausgesetzt, dass Installation und Standardendpunkte MongoDB, verwenden Sie zum Zeitpunkt der Erstellung dieses Dokuments ist: Mongodb://localhost

## <a name="4-install-the-restify-modules-in-to-your-web-api"></a>4: Installieren Sie die Restify-Module in Ihrer Web-API

Resitfy wird verwenden wir unsere REST-API erstellen. Restify ist ein minimaler und flexible Node.js-Anwendungsframework abgeleitet Express, die einen robusten Satz von Funktionen zum Erstellen von REST-APIs auf Verbinden.

### <a name="install-restify"></a>Restify installieren

Über die Befehlszeile wechseln Sie in das Verzeichnis Azuread. Wenn das **Azuread** -Verzeichnis nicht vorhanden ist, erstellen Sie ihn.

`cd azuread`– oder –`mkdir azuread;`

Geben Sie folgenden Befehl ein:

`npm install restify`

Dieser Befehl installiert Restify.

#### <a name="did-you-get-an-error"></a>Haben Sie einen Fehler?

Bei Verwendung von Npm auf einigen Betriebssystemen möglicherweise Fehler Fehler: EPERM Chmod "/ Usr/lokalen/Bin /..." und versuchen, das Konto als Administrator. In diesem Fall Befehl Sudo Npm auf einer höheren Berechtigungsebene ausgeführt.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Haben Sie einen Fehler bezüglich DTrace?

Etwas möglicherweise angezeigt, wenn Restify installieren:

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


Restify bietet einen leistungsstarken Mechanismus zum REST-Aufrufe mit DTrace verfolgen. Viele Betriebssysteme haben jedoch nicht DTrace verfügbar. Sie können diese Fehler ignorieren.


Die Ausgabe dieses Befehls sollte etwa wie folgt aussehen:


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
    └── bunyan@0.22.0(mv@0.0.5)


## <a name="5-install-passportjs-into-your-web-api"></a>5: Passport.js in Ihr Web API installieren

[Passport](http://passportjs.org/) ist Middleware für Node.js Authentifizierung. Flexibel und modular Passport kann unauffällig gelöscht werden auf jedem Express oder Restify Anwendung. Umfassende Strategien unterstützt Authentifizierung mit Benutzername und Kennwort, Facebook und Twitter. Wir haben eine Strategie für Azure Active Directory entwickelt. Wir installieren dieses Modul und fügen Sie die Strategie Azure Active Directory plug-in.

Über die Befehlszeile wechseln Sie in das Verzeichnis Azuread.

Geben Sie den folgenden Befehl zum Installieren der passport.js

`npm install passport`

Die Ausgabe des Befehls sollte etwa wie folgt aussehen:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: Ihr Web API Passport-Azure-AD hinzufügen

Wir werden fügen OAuth-Strategie mit Passport-Azuread, eine Reihe von Strategien, die mit Passport Azure Active Directory verbinden. Wir verwenden diese Strategie für Inhaber Token in diesem Beispiel Rest-API.

> [AZURE.NOTE] Obwohl OAuth2 ein Framework mit bekannten Tokentyp ausgestellt werden kann bietet, haben nur bestimmte Tokentypen Einsatz. Zum Schutz von Endpunkten, die Träger Token erwiesen hat. Trägertoken sind die meisten ausgestellt weit Tokentyp in OAuth2 und viele Implementierungen davon ausgehen, dass trägertoken der einzige Typ von Token ausgestellt.

Über die Befehlszeile wechseln Sie in das Verzeichnis azuread

Geben Sie den folgenden Befehl Passport.js Passport-Azure-Ad-Modul installiert:

`npm install passport-azure-ad`

Die Ausgabe des Befehls sollte etwa wie folgt aussehen:

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: Web API MongoDB Module hinzufügen

Wir verwenden MongoDB als unsere Datenspeicher aus, müssen wir sowohl die weit verbreitete installieren Plug-in-Modelle und Schemas verwalten als Mungo sowie den Datenbanktreiber für MongoDB, auch als MongoDB.


* `npm install mongoose`
* `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: zusätzliche Module installieren

Installieren Sie dann die übrigen erforderlichen Module.


In der Befehlszeile wechseln Sie die **Azuread** Ordner nicht bereits vorhanden:

`cd azuread`


Geben Sie die folgenden Befehle die folgenden Module im Verzeichnis Node_modules installiert:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## <a name="9-create-a-serverjs-with-your-dependencies"></a>9: Erstellen einer server.js mit der Abhängigkeit

Die Datei server.js wird den Großteil unserer Funktionen für unsere Web-API bereitstellen. Wir werden die meisten Code in dieser Datei hinzufügen. Für die Produktion gestalten Sie die Funktion in kleinere Dateien wie separate Routen und Controller. Für diese Demo werden wir server.js für diese Funktionalität verwenden.

In der Befehlszeile wechseln Sie die **Azuread** Ordner nicht bereits vorhanden:

`cd azuread`

Erstellen einer `server.js` in unserer bevorzugten Editor-Datei und fügen Sie die folgende Informationen:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
```

Speichern Sie die Datei. Wir werden in Kürze darauf zurück.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: erstellen eine Konfigurationsdatei Einstellungen Azure AD speichern

Diese Codedatei übergibt die Konfigurationsparameter der Azure Active Directory Portal an Passport.js. Diese Konfigurationswerte wird erstellt, wenn die Web-API an das Portal im ersten Teil dieser exemplarischen Vorgehensweise hinzugefügt. Wird erläutert, was die Werte dieser Parameter nach den Code kopiert haben.


In der Befehlszeile wechseln Sie die **Azuread** Ordner nicht bereits vorhanden:

`cd azuread`

Erstellen einer `config.js` in unserer bevorzugten Editor-Datei und fügen Sie die folgende Informationen:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
issuer: 'https://sts.windows.net/**<your application id>**/',
audience: '<your redirect URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For using Microsoft you should never need to change this.
};

```



### <a name="required-values"></a>Erforderliche Werte

*IdentityMetadata*: Diese sucht Passport-Azure-Ad für die Konfigurationsdaten IdP sowie der Schlüssel JWT-Token zu bestätigen. Wahrscheinlich möchten nicht wenn Azure Active Directory ändern.

*Zielgruppe*: die URI-Umleitung vom Portal aus.

> [AZURE.NOTE]
Wir Rollen Schlüssel in regelmäßigen Abständen. Stellen Sie sicher, dass Sie immer über die URL "Openid_keys" ziehen und die Anwendung auf das Internet zugreifen kann.


## <a name="11-add-configuration-to-your-serverjs-file"></a>11: Konfiguration der Datei server.js hinzufügen

Wir müssen diese Werte aus der Konfigurationsdatei lesen gerade unsere Anwendung erstellten. Dazu wir fügen die config-Datei als Ressource in der Anwendung, und legen Sie die globalen Variablen, die im Dokument config.js

In der Befehlszeile wechseln Sie die **Azuread** Ordner nicht bereits vorhanden:

`cd azuread`

Öffnen der `server.js` in unserer bevorzugten Editor-Datei und fügen Sie die folgende Informationen:

```Javascript
var config = require('./config');
```
Fügen einen neuen Abschnitt zu `server.js` mit dem folgenden Code:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
identityMetadata: config.creds.identityMetadata,
issuer: config.creds.issuer,
audience: config.creds.audience
};
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="step-12-add-the-mongodb-model-and-schema-information-using-moongoose"></a>Schritt 12: Fügen Sie MongoDB Modell und Schemainformationen mittels Moongoose hinzu

Diese Vorbereitung nun zu tilgen, wie wir diese drei Dateien zusammen in einem REST API-Dienst wind.

Für diese exemplarische Vorgehensweise verwenden MongoDB wir unsere Aufgaben speichern, wie in ***Schritt 4***erläutert.

Wenn Sie aus der Datei config.js in Schritt 11 erstellten erinnern, genannt wir unsere Datenbank *Tasklist*war was wir am Ende unserer Mogoose_auth_local Verbindungs-URL platzieren. Sie müssen diese Datenbank vorher MongoDB erstellen erstellen, sondern wird dies für uns bei der ersten Ausführung unserer Server-Anwendung (sofern nicht bereits vorhanden ist).

Wir dem Server welche MongoDB-Datenbank eingerichtet haben, wir verwenden möchten, müssen wir schreiben Sie zusätzlichen Code zum Modell und Schema für unsere Server-Tasks erstellen.

#### <a name="discussion-of-the-model"></a>Beschreibung des Modells

Unsere Schemamodell ist sehr einfach, und Sie nach Bedarf erweitern.

NAME - der Name der, die dem Vorgang zugeordnet ist. Eine ***Zeichenfolge***

TASK - Aufgabe. Eine ***Zeichenfolge***

Datum – das Datum, das die Aufgabe fällig ist. ***DATETIME***

ABGESCHLOSSEN – Wenn die Aufgabe abgeschlossen ist. ***BOOLEAN***

#### <a name="creating-the-schema-in-the-code"></a>Erstellen des Schemas im code


In der Befehlszeile wechseln Sie die **Azuread** Ordner nicht bereits vorhanden:

`cd azuread`

Öffnen der `server.js` in unserer bevorzugten Editor-Datei und fügen Sie Folgendes unter dem Konfigurationseintrag:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
Mit MongoDB-Server verbinden wird und uns wieder ein Schema-Objekt übergeben.

#### <a name="using-the-schema-create-our-model-in-the-code"></a>Erstellen Sie mit dem Schema unser Modell im code

Fügen Sie unter dem Code oben schrieb den folgenden Code:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Wie aus dem Code sehen können, unsere Schema erstellen und erstellen Sie ein Modellobjekt, mit dem wir unsere Datenspeicher im gesamten Code beim Definieren von ***Routen***.

## <a name="step-13-add-our-routes-for-our-task-rest-api-server"></a>Schritt 13: Unsere Routen für unsere Aufgabe REST API-Server hinzufügen

Jetzt haben wir ein Datenbankmodell arbeiten, fügen Sie die Routen, die wir für unsere REST-API verwenden.

### <a name="about-routes-in-restify"></a>Routen in Restify

Routen funktionieren Restify auf genau dieselbe Weise wie Express Stapel. Sie definieren Routen mit dem URI die Clientanwendungen erwartet aufrufen. Normalerweise definieren Sie Ihre Routen in einer separaten Datei. Für unsere Zwecke stellen wir unsere Routen in der Datei server.js. Es wird empfohlen, dass Sie diese in ihre eigene Datei für die Produktion Faktor.

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


Dies ist das Muster auf der grundlegendsten Ebene. Resitfy (und Express) bieten viel tieferen kartenherstellern Anwendung definieren und Ausführen komplexer routing über verschiedene Endpunkte. Für unsere Zwecke halten wir diese Routen einfach.

#### <a name="add-default-routes-to-our-server"></a>Unsere Server Standard-Routen hinzufügen

Wir werden jetzt hinzufügen grundlegenden CRUD Routen erstellen, abrufen, aktualisieren und löschen.

In der Befehlszeile wechseln Sie die **Azuread** Ordner nicht bereits vorhanden:

`cd azuread`

Öffnen der `server.js` in unserer bevorzugten Editor-Datei und fügen folgende unterhalb der Datenbankeinträge oben aus:

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
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
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
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
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

### <a name="add-some-error-handling-for-the-routes"></a>Einige Fehlerbehandlung für Routen

Sinnvoll fügen einige Fehlerbehandlung, damit wir zurück an den Client das Problem kommunizieren kann auf eine Weise begegnet verstehen können.

Fügen Sie den folgenden Code unter der oben erstellten Code:

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


## <a name="step-14-create-your-server"></a>Schritt 14: Erstellen Sie Ihren Server!

Unsere Datenbank definiert, haben wir unsere Routen und die letzte ist unsere Server-Instanz hinzufügen, die unsere verwalten.

Restify (und Express) viel umfassender Anpassung für einen REST-API-Server kann jedoch erneut wir die grundlegende Einrichtung für unsere Zwecke verwenden.

```Javascript
/**
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
}));
```
## <a name="15-adding-the-routes-without-authentication-for-now"></a>15: Hinzufügen von Routen (ohne Authentifizierung jetzt)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-before-we-add-oauth-support-lets-run-the-server"></a>16: Bevor wir OAuth unterstützen wir Server ausführen.

Dem Server testen Sie, bevor wir Authentifizierung hinzufügen

Die einfachste Möglichkeit hierzu ist mit Kringel in einer Befehlszeile. Bevor wir dafür benötigen wir ein einfaches Dienstprogramm, das Ausgabe als JSON analysiert werden kann. Installieren Sie hierzu Json-Tool wie die folgenden Beispiele verwenden.

`$npm install -g jsontool`

Das JSON-Tool installiert weltweit. Da wir das erreicht haben wir Spielen mit dem Server:

Vergewissern Sie sich, dass die MonogoDB-Instanz ausgeführt wird.

`$sudo mongod`

Dann wechseln Sie zum Verzeichnis und gewelltem...

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 2.0OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Dann können wir einen Vorgang auf diese Weise hinzufügen:

`$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

Die Antwort sollte lauten:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
Und wir können Aufgaben Brandon so:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Wenn dies funktioniert, können wir OAuth REST API-Server hinzufügen.

**Sie haben einen REST-API Server MongoDB!**

## <a name="17-add-authentication-to-our-rest-api-server"></a>17: unsere REST API-Server Authentifizierung hinzufügen

Jetzt haben wir einen laufenden REST-API (Herzlichen übrigens!) lassen somit gegen Azure AD.

In der Befehlszeile wechseln Sie die **Azuread** Ordner nicht bereits vorhanden:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: Verwenden Sie die Passport-Azure-Ad enthaltene Oidcbearerstrategy

Bisher haben wir einen normalen REST erledigen Server ohne Autorisierung aufgebaut. Dies ist, wo wir beginnen, die zusammen.

Zunächst müssen wir angeben, dass wir Passport verwenden möchten. Nachdem der Server-Konfiguration haben:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
APIs schreiben sollten Sie immer die Daten einen eindeutigen aus dem Token verknüpfen, die der Benutzer spoofen kann. Wenn dieser Server Aufgaben speichert, speichert über die Abonnement-ID des Benutzers (durch token.sub genannt) Token im Feld "Besitzer" erwähnt. Dies gewährleistet, dass nur dieser Benutzer kann seine Aufgaben zugreifen niemand eingegebenen Aufgaben zugreifen können. Gibt es keine API "Owner", ein externer Benutzer anderen Aufgaben erhalten, selbst wenn sie authentifiziert.

Als Nächstes verwenden wir die Open ID verbinden Träger-Strategie, die mit Passport-Azure-Ad. Schauen Sie sich den Code jetzt, es wird kurz erläutert. Setzen Sie diese nach dem, was Sie auch über:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
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
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport verwendet ein ähnliches Muster für alles Strategien (Twitter, Facebook, etc.), die alle Autoren Strategie entsprechen. Die Strategie sehen Sie, dass es eine möchten übergeben, die ein Token und fertig als Parameter. Strategie pflichtgemäß zurück zu uns kommen nach allen ist die Arbeit. Sobald dies wollen wir den Benutzer speichern und das Token speichern, damit wir wieder Fragen müssen.

> [AZURE.IMPORTANT]
Der obige Code hat jeder Benutzer, der zur Authentifizierung an unseren Server geschieht. Dies wird als automatische Registrierung bezeichnet. Bei Produktionsservern möchte Sie niemanden ohne eine Registrierung gehen Sie entscheiden. Dies ist normalerweise das Muster, das Sie im Consumer apps anzeigen, die können Sie mit Facebook anmelden, aber dann aufgefordert, zusätzliche Informationen. Dieses Befehlszeilenprogramm nicht, könnten wir einfach die e-Mail aus dem token Objekt extrahieren haben, die zurückgegeben und dann aufgefordert, Weitere Informationen einzugeben. Da dies einen Testserver wir einfach die Speicherdatenbank hinzufügen.

### <a name="2-finally-protect-some-endpoints"></a>2. schließlich einige Endpunkte schützen

Schützen Sie Endpunkte durch Angabe des passport.authenticate()-Aufrufs mit dem Protokoll verwenden möchten.

Wir unsere Route in unserem Servercode etwas interessanter zu bearbeiten:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again-and-ensure-it-rejects-you"></a>18: die Serveranwendung erneut ausführen und sorgen Sie ablehnt

Wir verwenden `curl` erneut, um festzustellen, ob wir jetzt OAuth2 Schutz gegen Endpunkte verfügen. Wir tun dies vor ausgeführt eines Mandanten SDKs für diesen Endpunkt. Zurückgegebenen Header sollte ausreichen, um uns mitzuteilen, dass wir auf dem richtigen Weg sind.

Vergewissern Sie sich, dass die MonogoDB-Instanz ausgeführt wird.

    $sudo mongod

Dann wechseln Sie zum Verzeichnis und gewelltem...

    $ cd azuread
    $ node server.js

Führen Sie eine einfache Bereitstellung:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Ein 401 ist die Antwort hier gesuchte wie angibt, dass die Passport-Ebene Umleiten der Endpunkt autorisieren, das ist genau das, was Sie wollen.


## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Herzlichen Glückwunsch! Sie haben eine REST-API-Dienst mit OAuth2!

Sie haben ging, soweit Sie mit diesem Server können ohne einen kompatiblen Client OAuth2. Sie müssen eine zusätzliche exemplarische Vorgehensweise zu.

Wenn Sie lediglich Informationen eine REST-API mit Restify und OAuth2 implementiert wurden, müssen Sie mehr als genug Code Ihren Dienst entwickeln und lernen, wie dieses Beispiel erstellen.

## <a name="next-steps"></a>Nächste Schritte

Zu Referenzzwecken können vollständiges Beispiel (ohne Ihre Werte) [als eine .zip bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip)oder es von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Sie können jetzt auf Themen verschieben.  Möglicherweise möchten versuchen:

[Sichern eine Node.js Web app v2. 0-Endpunkt mit >>](active-directory-v2-devquickstarts-node-web.md)

Weitere Ressourcen zum Auschecken:
- [V2. 0-Entwicklerhandbuch >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Azure Active Directory" Tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [dieser Seite](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.

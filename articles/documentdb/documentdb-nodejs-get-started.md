<properties
  pageTitle="NoSQL Node.js Lernprogramm DocumentDB | Microsoft Azure"
  description="Ein NoSQL Node.js-Lernprogramm, das eine Knoten Datenbank und Konsole Anwendung DocumentDB Node.js SDK erstellt. DocumentDB ist eine NoSQL-Datenbank JSON."
    keywords="Node.js Lernprogramm Knotendatenbank"
  services="documentdb"
  documentationCenter="node.js"
  authors="AndrewHoh"
  manager="jhubbard"
  editor="monicar"/>

<tags
  ms.service="documentdb"
  ms.workload="data-services"
  ms.tgt_pltfrm="na"
  ms.devlang="node"
  ms.topic="hero-article"
  ms.date="08/11/2016"
  ms.author="anhoh"/>

# <a name="nosql-nodejs-tutorial-documentdb-nodejs-console-application"></a>NoSQL Node.js Tutorial: DocumentDB Node.js Konsolenanwendungsprojekt  

> [AZURE.SELECTOR]
- [.NET](documentdb-get-started.md)
- [Node.js](documentdb-nodejs-get-started.md)

Willkommen Sie Node.js-Lernprogramm für Azure DocumentDB Node.js SDK Nach Ausführung dieses Lernprogramm, müssen Sie ein Konsolenanwendungsprojekt erstellt und DocumentDB Ressourcen Node-Datenbank Abfragen.

Wir behandeln:

- Erstellen und Verbinden mit einem DocumentDB-Konto
- Einrichten der Anwendung
- Erstellen einer Knotendatenbank
- Erstellen einer Sammlung
- JSON-Dokumente erstellen
- Die Sammlung wird abgefragt
- Ersetzen eines Dokuments
- Löschen eines Dokuments
- Die Knotendatenbank löschen

Sie haben keine Zeit? Macht nichts! Die vollständige Lösung ist auf [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started)verfügbar. Kurze Anleitung finden Sie in der [vollständigen Lösung](#GetSolution) .

Nach Abschluss des Lernprogramms Node.js verwenden Sie die Abstimmungsschaltflächen am oberen und unteren Seitenrand um uns Feedback. Möchten Sie uns direkt kontaktieren, können Sie Ihre e-Mail-Adresse in Ihre Kommentare enthalten.

Jetzt beginnen!

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Erforderliche Komponenten für Node.js-Lernprogramm

Stellen Sie sicher, dass Sie über Folgendes verfügen:

- Ein aktives Azure-Konto. Wenn Sie haben, können Sie für eine [Kostenlose Testversion von Azure](https://azure.microsoft.com/pricing/free-trial/)anmelden.
- [Node.js](https://nodejs.org/) Version v0.10.29 oder höher.

## <a name="step-1-create-a-documentdb-account"></a>Schritt 1: Erstellen eines Kontos DocumentDB

Erstellen Sie ein Konto DocumentDB. Wenn Sie bereits über ein Konto, die Sie verwenden möchten verfügen, können Sie Setup [Node.js-Anwendung](#SetupNode)überspringen.

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="SetupNode"></a>Schritt 2: Setup Node.js-Anwendung

1. Öffnen Sie Ihre bevorzugten Terminalserver.
2. Suchen Sie den Ordner oder das Verzeichnis, in dem Sie Node.js speichern möchten.
3. Erstellen Sie zwei leere JavaScript-Dateien mit den folgenden Befehlen:
  - Windows:
      * ```fsutil file createnew app.js 0```
        * ```fsutil file createnew config.js 0```
  - Linux-OS X:
      * ```touch app.js```
        * ```touch config.js```
4. Installieren Sie das Modul Documentdb über Npm. Verwenden Sie den folgenden Befehl ein:
    * ```npm install documentdb --save```

Sehr gut. Da Sie eingerichtet haben, beginnen wir Code schreiben.

## <a id="Config"></a>Schritt 3: Festlegen der app-Konfigurationen

Open ```config.js``` in Ihrem bevorzugten Editor.

Dann, kopieren und Einfügen die folgenden Codeausschnitt und Eigenschaften ```config.endpoint``` und ```config.primaryKey``` DocumentDB Endpunkt-Uri und Primärschlüssel. Diese Konfigurationen finden in der [Azure-Portal](https://portal.azure.com).

![Node.js Tutorial - Screenshot Azure-Portal, aktiver Hub hervorgehoben, Schlüssel Schaltfläche hervorgehoben DocumentDB Konto Blade und die URI, primären und SEKUNDÄRSCHLÜSSEL Werte Schlüssel-Blade - Node-Datenbank markiert mit DocumentDB-Konto][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Kopieren und Einfügen der ```database id```, ```collection id```, und ```JSON documents``` , Ihre ```config``` Objekt darunter, legen, Ihre ```config.endpoint``` und ```config.authKey``` Eigenschaften. Haben Sie bereits Daten in Ihrer Datenbank speichern möchten, können Sie DocumentDBs [Data Migration Tools](documentdb-import-data.md) anstatt die Dokumentdefinitionen.

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


Datenbank, Sammlung und Dokumentdefinitionen werden die DocumentDB als ```database id```, ```collection id```, und Dokumente.

Exportieren Sie schließlich die ```config``` Objekt, sodass es zu verweisen die ```app.js``` Datei.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

##<a id="Connect"></a>Schritt 4: Verbinden Sie mit DocumentDB-Konto

Öffnen Sie die leere ```app.js``` Datei im Text-Editor. Kopieren und fügen Sie den Code nach Importieren der ```documentdb``` -Modul und die neu erstellten ```config``` Modul.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Kopieren und fügen Sie den Code verwenden, der zuvor gespeicherten ```config.endpoint``` und ```config.primaryKey``` eine neue "documentclient" erstellen.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Jetzt haben Sie den Code zum Initialisieren des Documentdb Clients, sehen wir uns mit DocumentDB Ressourcen.

## <a name="step-5-create-a-node-database"></a>Schritt 5: Erstellen einer Knoten
Kopieren Sie und fügen Sie den Code unten nicht gefunden, Datenbank-Url und die Url der HTTP-Status festgelegt. Diese Urls sind wie DocumentDB-Client die richtige Datenbank und Auflistung finden.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

Eine [Datenbank](documentdb-resources.md#databases) kann mithilfe der Funktion [CreateDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) der Klasse **"documentclient"** erstellt werden. Eine Datenbank ist der logische Container Speicherung von Dokumenten über Sammlungen partitioniert.

Kopieren und Einfügen die Funktion **GetDatabase** zum Erstellen der neuen Datenbank in der Datei app.js mit den ```id``` gemäß der ```config``` Objekt. Die Funktion prüft, ob die Datenbank mit dem gleichen ```FamilyRegistry``` Id nicht vorhanden. Wenn es vorhanden ist, kehren wir diese Datenbank anstatt eine neue zu erstellen.

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Kopieren Sie und fügen Sie den Code unten, legen Sie die **GetDatabase** Funktion der Helfer Funktion **Beenden** hinzufügen, die der Meldung zum Beenden und den Aufruf **GetDatabase** Funktion gedruckt.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal der ```app.js``` , und starten Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Erstellt eine DocumentDB-Datenbank

##<a id="CreateColl"></a>Schritt 6: Erstellen einer Auflistung  

> [AZURE.WARNING] **CreateDocumentCollectionAsync** Erstellen einer neuen Auflistung die Preise auswirken. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/documentdb/).

Eine [Auflistung](documentdb-resources.md#collections) kann mithilfe der [CreateCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) -Funktion der Klasse **"documentclient"** erstellt werden. Eine Auflistung ist ein Container für JSON-Dokumente und zugeordnete JavaScript Anwendungslogik.

Kopieren und Einfügen die Funktion **GetCollection** unter der **GetDatabase** -Funktion für die neue Sammlung mit den ```id``` gemäß der ```config``` Objekt. Wir werden kontrollieren, um sicherzustellen, dass eine Auflistung mit demselben ```FamilyCollection``` Id nicht vorhanden. Wenn es vorhanden ist, kehren wir, anstatt eine neue Auflistung.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Kopieren Sie und fügen Sie den Code unter dem Aufruf **GetDatabase** zum Ausführen der Funktion **GetCollection** .

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal der ```app.js``` , und starten Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Erstellt eine DocumentDB-Auflistung

##<a id="CreateDoc"></a>Schritt 7: Erstellen eines Dokuments
Ein [Dokument](documentdb-resources.md#documents) kann mit der Funktion [CreateDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) der Klasse **"documentclient"** erstellt werden. Dokumente sind benutzerdefinierte (beliebigen) JSON-Inhalt. Jetzt können Sie ein Dokument in DocumentDB einfügen.

Kopieren und Einfügen die Funktion **GetFamilyDocument** unter der **GetCollection** -Funktion zum Erstellen von Dokumenten mit JSON-Daten der ```config``` Objekt. Wir werden kontrollieren, um sicherzustellen, dass ein Dokument mit derselben Id nicht vorhanden ist.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Kopieren Sie und fügen Sie den Code unter dem Aufruf **GetCollection** zum Ausführen der Funktion **GetFamilyDocument** .

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal der ```app.js``` , und starten Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Erstellt ein DocumentDB Dokumente

![Node.js Tutorial - Diagramm zur Veranschaulichung der hierarchischen Beziehung zwischen dem Konto, die Datenbank der Auflistung und Dokumente - Node-Datenbank](./media/documentdb-nodejs-get-started/node-js-tutorial-account-database.png)

##<a id="Query"></a>Schritt 8: Abfrage DocumentDB Ressourcen

DocumentDB unterstützt [umfangreiche Abfragen](documentdb-sql-query.md) für JSON-Dokumente in jeder Auflistung. Der folgende Code zeigt eine Abfrage, die für die Dokumente in der Auflistung ausgeführt werden können.

Kopieren und Einfügen der Funktion **QueryCollection** unter der **GetFamilyDocument** -Funktion. DocumentDB unterstützt SQL-ähnliche Abfragen, wie unten dargestellt. Weitere Informationen zum Erstellen komplexer Abfragen suchen Sie [Abfrage Spielplatz](https://www.documentdb.com/sql/demo) und [Abfrage-Dokumentation](documentdb-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


Das folgende Diagramm veranschaulicht, wie die DocumentDB SQL-Abfragesyntax für die Auflistung Sie aufgerufen wird, erstellt.

![Node.js Tutorial - Diagramm zur Veranschaulichung des Umfang und die Bedeutung der Abfrage - Node-Datenbank](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Das [FROM](documentdb-sql-query.md#from-clause) -Schlüsselwort ist in der Abfrage optional, da DocumentDB Abfragen bereits auf eine Sammlung begrenzt werden. Daher "Von Familien f" austauschbar mit"vom Stamm" oder andere Variable benennen Sie auswählen. DocumentDB ableiten, Familien, Stamm oder der Name, den Sie ausgewählt haben, standardmäßig die aktuelle Auflistung verweisen.

Kopieren Sie und fügen Sie den Code unter dem Aufruf **GetFamilyDocument** zum Ausführen der Funktion **QueryCollection** .

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal der ```app.js``` , und starten Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! DocumentDB Dokumente wurden erfolgreich abgefragt werden.

##<a id="ReplaceDocument"></a>Schritt 9: Ersetzen eines Dokuments
DocumentDB unterstützt ersetzen JSON-Dokumente.

Kopieren und Einfügen der Funktion **ReplaceDocument** unter der **QueryCollection** -Funktion.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Kopieren Sie und fügen Sie den Code unter dem Aufruf **QueryCollection** zum Ausführen der Funktion **ReplaceDocument** . Außerdem fügen Sie den Code **QueryCollection** erneut aufrufen, um sicherzustellen, dass das Dokument geändert wurde.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal der ```app.js``` , und starten Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Sie haben erfolgreich ein DocumentDB Dokument ersetzt.

##<a id="DeleteDocument"></a>Schritt 10: Löschen eines Dokuments
DocumentDB unterstützt Löschen von JSON-Dokumente.

Kopieren und Einfügen der Funktion **DeleteDocument** unter der **ReplaceDocument** -Funktion.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Kopieren Sie und fügen Sie den Code unter dem Aufruf der zweiten **QueryCollection** zum Ausführen der Funktion **DeleteDocument** .

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal der ```app.js``` , und starten Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Ein DocumentDB Dokument gelöscht.

##<a id="DeleteDatabase"></a>Schritt 11: Die Knoten-Datenbank löschen

Die erstellte Datenbank wird die Datenbank und alle untergeordneten Ressourcen (Sammlungen, Dokumente usw.) löschen.

Kopieren Sie und fügen Sie den folgenden Codeausschnitt (Funktion **Cleanup**) die Datenbank und alle untergeordneten Ressourcen.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

Kopieren Sie und fügen Sie den Code unter dem **DeleteDocument** Aufruf **die Bereinigungsfunktion** ausgeführt.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

##<a id="Run"></a>Schritt 12: Ausführen der Anwendung Node.js zusammen!

Insgesamt soll die Sequenz für den Aufruf von Funktionen wie folgt aussehen:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal der ```app.js``` , und starten Sie den Befehl:```node app.js```

Die Ausgabe der Get gestartete Anwendung sollte angezeigt werden. Die Ausgabe sollte den folgenden Beispieltext übereinstimmen.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Herzlichen Glückwunsch! Sie erstellte Node.js-Lernprogramm abgeschlossen haben und Ihre ersten DocumentDB Konsole Anwendung!

## <a id="GetSolution"></a>Vollständige Node.js Tutorial Lösung
Die GetStarted-Lösung erstellen, die in den Beispielen in diesem Artikel enthält, benötigen Sie Folgendes:

-   [DocumentDB-Konto][documentdb-create-account].
-   Die [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) Lösung auf GitHub.

Installieren Sie das Modul **Documentdb** über Npm. Verwenden Sie den folgenden Befehl ein:
* ```npm install documentdb --save```

Im der ```config.js``` Datei, aktualisieren Sie die Werte config.endpoint und config.authKey gemäß [Schritt 3: Festlegen von app Konfigurationen](#Config).

## <a name="next-steps"></a>Nächste Schritte

-   Möchten Sie eine komplexere Node.js-Beispiel? Finden Sie unter [erstellen eine Node.js Webanwendung DocumentDB](documentdb-nodejs-application.md).
-  Erfahren Sie, wie [Monitor DocumentDB-Konto](documentdb-monitor-accounts.md).
-  Unser Beispiel Dataset auf dem [Spielplatz Abfrage](https://www.documentdb.com/sql/demo)Abfragen ausführen.
-  Erfahren Sie mehr über das Programmiermodell in Abschnitt Entwicklung [DocumentDB-Dokumentationsseite](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[documentdb-manage]: documentdb-manage.md

[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png

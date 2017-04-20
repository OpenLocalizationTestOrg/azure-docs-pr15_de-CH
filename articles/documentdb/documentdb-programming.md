<properties 
    pageTitle="DocumentDB Programmierung: gespeicherte Prozeduren, Datenbanktrigger und UDFs | Microsoft Azure" 
    description="Erfahren Sie, wie mit DocumentDB gespeicherte Prozeduren Datenbanktrigger und benutzerdefinierte Funktionen (UDFs) in JavaScript geschrieben. Erhalten Sie Datenbank Programmierung Tipps und mehr." 
    keywords="Datenbanktrigger gespeicherte Prozedur Datenbankprogramm Prozedur Documentdb Azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>DocumentDB serverseitige Programmierung: gespeicherte Prozeduren, Datenbanktrigger und UDFs

Integration von Azure DocumentDB Sprache lernen, transaktionale Ausführung von JavaScript kann Entwickler **gespeicherte Prozeduren**, **Trigger** und **benutzerdefinierte Funktionen (UDFs)** direkt in JavaScript geschrieben. Dadurch können Sie Datenbank Programmlogik Anwendung schreiben, die geliefert und direkt auf den Speicher Partitionen ausgeführt werden können 

Erste Schritte im folgenden Video, wo Andrew Liu kurz DocumentDBs serverseitigen Datenbank Programmiermodell bietet empfohlen. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Dann zurück zu diesem Artikel, Antworten auf folgenden Fragen erfahren Sie:  

- Wie schreibe ich eine eine gespeicherte Prozedur, Trigger oder UDF mit JavaScript?
- Wie gewährleistet DocumentDB Säure?
- Wie funktionieren Transaktionen in DocumentDB?
- Was sind bereits ausgelöst und nach dem Auslösen und wie schreibe ich eine?
- Wie registrieren und Ausführen eine gespeicherte Prozedur, Trigger oder UDF auf Rest mit HTTP?
- Was DocumentDB SDKs stehen zum Erstellen und Ausführen gespeicherte Prozeduren, Trigger und UDFs?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Einführung in die gespeicherte Prozedur und UDF-Programmierung

Dieser Ansatz *"JavaScript als modernen T-SQL"* frei Anwendungsentwickler Komplexität Typenkonflikte System und objektrelationale Zuordnung Technologien. Es hat auch eine Reihe von systeminternen Vorteilen genutzt werden kann, um umfangreiche Anwendung:  

-   **Prozedurale Logik:** JavaScript als hohe Programmiersprache bietet eine umfassende und vertraute Benutzeroberfläche Geschäftslogik auszudrücken. Sie können komplexe Sequenzen von Operationen näher auf die Daten ausführen.

-   **Atomaren Transaktionen:** DocumentDB garantiert, dass Datenbank-innerhalb einer gespeicherten Prozedur Operationen oder atomar sind. Dadurch kann eine Anwendung verknüpfte Vorgänge in einem Stapel zusammenfassen, entweder alle erfolgreich verlaufen oder keine davon erfolgreich. 

-   **Leistung:** Die Tatsache, dass JSON systemintern Typsystem der Javascript-Sprache zugeordnet und auch der Speicher im DocumentDB ist ermöglicht eine Reihe von Optimierungen wie verzögerte Materialisierung JSON-Dokumente Pufferpool und stehen bei Bedarf mit dem ausgeführten Code. Es gibt weitere Leistungsvorteile Versand Geschäftslogik in der Datenbank zugeordnet:
    -   Batchverarbeitung – Entwickler können Vorgänge wie Einfügen gruppiert und in Massen senden. Die Kosten Datenverkehr Netzwerklatenz und die Belastung des Speichers zu separaten Transaktionen werden wesentlich reduziert. 
    -   Vorkompilierung – enthält DocumentDB gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen (UDFs) JavaScript Kompilierung für jeden Aufruf vermeiden. Der Aufwand für das Erstellen der Bytecode prozedurale Logik ist ein minimaler Wert abgeschrieben.
    -   Sequenzierung – viele Vorgänge müssen Nebeneffekt ("Trigger), der potentiell umfasst eine oder mehrere sekundäre Speicher ausgeführt. Neben Unteilbarkeit ist leistungsstärker, wenn auf den Server verschoben. 
-   **Kapselung:** Gespeicherte Prozeduren können Geschäftslogik an einem Ort gruppieren verwendet werden. Dies hat zwei Vorteile:
    -   Es fügt eine Abstraktionsschicht auf die Rohdaten Datenarchitekten Entwickeln ihrer Anwendung unabhängig von den Daten ermöglicht. Dies ist besonders vorteilhaft, wenn die Daten mit weniger Schemata, durch spröden Annahmen, die in die Anwendung gebacken werden, wenn sie Daten direkt bearbeiten müssen.  
    -   Diese Abstraktion ermöglicht Unternehmen, ihre Daten zu schützen durch Optimierung der Skripts.  

Die Erstellung und Ausführung Datenbanktrigger gespeicherte Prozedur und benutzerdefinierte Operatoren wird durch die [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)und [Client SDKs](documentdb-sdk-dotnet.md) in vielen Plattformen einschließlich .NET, Node.js und JavaScript unterstützt.

Syntax und Verwendung von gespeicherten Prozeduren, Triggern und UDFs **dieses Lernprogramms [Node.js SDK mit Q](http://azure.github.io/azure-documentdb-node-q/) ** .   

## <a name="stored-procedures"></a>Gespeicherte Prozeduren

### <a name="example-write-a-simple-stored-procedure"></a>Beispiel: Schreiben einer einfachen gespeicherten Prozedur 
Beginnen wir mit einer einfachen gespeicherten Prozedur, die eine "Hello World"-Antwort zurückgibt.

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Gespeicherte Prozeduren pro Auflistung registriert und lässt alle Dokument und in der Auflistung vorhanden. Im folgenden Codeausschnitt veranschaulicht, wie sich die HelloWorld gespeicherte Prozedur mit. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Nachdem die gespeicherte Prozedur registriert ist, können wir für die Auflistung ausgeführt und die Ergebnisse an den Client lesen. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Das Context-Objekt bietet Zugriff auf alle Operationen auf DocumentDB Speicher können sowie Zugriff auf die Anforderung und Antwort-Objekte. In diesem Fall verwendet es das Antwortobjekt Hauptteil der Antwort fest, die an den Client gesendet wurde. Weitere Informationen finden Sie [DocumentDB JavaScript-Server-SDK-Dokumentation](http://azure.github.io/azure-documentdb-js-server/).  

Wir erweitern Sie dieses Beispiel, und fügen Sie mehrere Datenbank-Funktion an die gespeicherte Prozedur. Gespeicherte Prozeduren können erstellen, aktualisieren, lesen, Abfragen und löschen Dokumente und Anhänge innerhalb der Auflistung.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Beispiel: Schreiben Sie eine gespeicherte Prozedur zum Erstellen eines Dokuments 
Der nächste Ausschnitt gezeigt, wie das Kontextobjekt DocumentDB Ressourcen interagieren.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Diese gespeicherte Prozedur akzeptiert als Eingabe DocumentToCreate, ein Dokument in der aktuellen Auflistung erstellt werden. Solche Vorgänge asynchron und JavaScript Funktionsrückrufe abhängig. Die Rückruffunktion verfügt über zwei Parameter, das Fehlerobjekt bei schlägt der Vorgang fehl und das erstellte Objekt. Innerhalb des Rückrufs Benutzer die Ausnahme oder ein Fehler ausgelöst. Rückruf nicht bereitgestellt, und ein Fehler auftritt, löst die Runtime DocumentDB Fehler.   

Im obigen Beispiel löst der Rückruf einen Fehler bei einem fehlgeschlagenen Vorgang. Andernfalls wird die Id des erstellten Dokuments als Textkörper der Antwort an den Client. Hier ist wie diese gespeicherte Prozedur mit Parametern ausgeführt wird.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Beachten Sie, dass diese gespeicherte Prozedur geändert werden kann, um ein Array von Dokument stellen als Eingabe und in der gespeicherten Prozedur Ausführung statt mehrere Netzwerkanfragen zu jeweils einzeln erstellen. Hiermit können effiziente Bulk Einführer für DocumentDB (weiter unten in diesem Lernprogramm) implementieren.   

Beispiel demonstriert, wie gespeicherte Prozeduren verwendet. Trigger und benutzerdefinierte Funktionen (UDFs) wird später im Lernprogramm behandelt.

## <a name="database-program-transactions"></a>Programm Datenbanktransaktionen
Transaktion in einer Datenbank gewöhnlich kann als eine Abfolge von Operationen, die als einzelnen logischen Arbeitseinheit definiert. Jede Transaktion stellt **ACID gewährleistet**. ACID ist ein bekannter Akronym für vier Eigenschaften – Unteilbarkeit, Konsistenz, Isolation und Durability steht.  

Kurz Unteilbarkeit wird sichergestellt, dass die Arbeit in einer Transaktion als Einheit behandelt wird, entweder alle engagiert oder none. Konsistenz wird sichergestellt, dass die Daten stets in einem guten interne Transaktionen. Isolation garantiert, dass keine zwei Transaktionen störend – in der Regel die meisten kommerziellen Systeme bieten mehrere Isolationsstufen, die verwendet werden können in der Anwendung muss. Dauerhaftigkeit gewährleistet, dass jede Änderung in der Datenbank festgeschrieben immer vorhanden werden.   

JavaScript ist in DocumentDB im selben Speicherbereich wie die Datenbank gehostet. Daher Anträge in gespeicherten Prozeduren und Triggern innerhalb einer Sitzung des ausführen. DocumentDB Gewährleistung und für alle Vorgänge, die eine einzelne gespeicherte Prozedur/Trigger gehören können. Betrachten Sie die folgende gespeicherte Prozedurdefinition:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Diese gespeicherte Prozedur verwendet Transaktionen in einer app Spiele Handelsartikeln zwischen zwei Spieler in einem einzigen Vorgang. Die gespeicherte Prozedur versucht, zwei Dokumente lesen, die jeweils die Player-IDs als Argument übergeben. Wenn beide Spieler Dokumente gefunden werden, aktualisiert die gespeicherte Prozedur Dokumente durch Austausch ihrer Elemente. Wenn dabei Fehler auftreten, wird eine JavaScript-Ausnahme, die implizit die Transaktion abgebrochen wird.

Auflistung gespeicherte Prozedur registriert gegen besteht aus einer Partition, wenn Buchung auf alle Docuemnts innerhalb der Auflistung beschränkt. Partitioniert die Auflistung werden gespeicherte Prozeduren im Transaktionsbereich einzelne Partitionsschlüssel ausgeführt. Jede gespeicherte Prozedur ausführen muss einbeziehen einen Partitionswert entsprechend dem Umfang die Transaktion ausgeführt werden muss. Weitere Informationen finden Sie in der [DocumentDB zu partitionieren](documentdb-partition-data.md).

### <a name="commit-and-rollback"></a>Commit und rollback
Transaktionen sind tief und systemeigene JavaScript-Programmiermodell des DocumentDB integriert. In einer JavaScript-Funktion sind alle Operationen in einer einzigen Transaktion automatisch umbrochen. Wenn JavaScript ohne Ausnahme beendet wird, werden die Vorgänge in der Datenbank festgeschrieben "BEGIN TRANSACTION" und "COMMIT TRANSACTION" Anweisungen in relationalen Datenbanken sind im DocumentDB.  
 
Ist eine Ausnahme, die vom Skript weitergegeben wird, wird DocumentDBs JavaScript Laufzeit die gesamte Transaktion zurückgesetzt. Wie im vorherigen Beispiel gezeigt, ist eine Ausnahme entspricht "ROLLBACK TRANSACTION" in DocumentDB.
 
### <a name="data-consistency"></a>Datenkonsistenz
Gespeicherte Prozeduren und Trigger werden immer auf dem primären Replikat DocumentDB Auflistung ausgeführt. Dadurch Lesevorgängen in Prozeduren bieten hohe Konsistenz gespeichert. Abfragen mit benutzerdefinierten Funktionen können auf dem primären oder sekundären Replikat ausgeführt werden, aber wir sicher, dass Sie die gewünschten Konsistenz durch Auswählen des entsprechenden Replikats entsprechen.

## <a name="bounded-execution"></a>Gebundene Ausführung
Alle DocumentDB-Vorgänge müssen innerhalb des angegebenen Servers Timeoutdauer anfordern. Diese Einschränkung gilt auch für JavaScript-Funktionen (gespeicherte Prozeduren, Trigger und benutzerdefinierten Funktionen). Ein Vorgang mit dieser Frist nicht abgeschlossen wird, wird die Transaktion zurückgesetzt. JavaScript-Funktionen müssen fristgerecht fertig stellen oder eine Fortsetzung basierten Modell um Batch/Ausführung fortsetzen.  

Zur Vereinfachung der Entwicklung von gespeicherten Prozeduren und Triggern Fristen behandeln zurück alle Funktionen unter das Auflistungsobjekt (für erstellen, lesen, ersetzen und Löschen der Dokumente und Anhänge) einen booleschen Wert, der angibt, ob dieser Vorgang. Wenn dieser Wert false ist, ist ein Hinweis, dass das Zeitlimit abläuft und Ausführung die Prozedur einpacken muss.  Vorgänge in der Warteschlange vor den ersten Vorgang abgelehnten speichern unbedingt abgeschlossen, wenn die gespeicherte Prozedur rechtzeitig abgeschlossen und nicht mehr Anfragen Warteschlange.  

JavaScript-Funktionen werden auch auf Ressourcen begrenzt. DocumentDB reserviert Durchsatz pro Auflistung anhand der bereitgestellten Größe ein Konto. Eine normalisierte Einheit CPU, Speicher und e/a-Verbrauch anforderungseinheiten genannt RUs Durchsatz ausgedrückt. JavaScript-Funktionen können potenziell eine Vielzahl von RUs innerhalb kurzer Zeit und erhalten begrenzt, wenn die Auflistung erreicht ist. Ressource intensive gespeicherte Prozeduren können auch zur Sicherstellung der Verfügbarkeit von primitiven Datenbankoperationen isoliert werden.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Beispiel: Massenkopieren von Daten in einem Datenbankprogramm importieren
Es folgt ein Beispiel für eine gespeicherte Prozedur geschrieben, Bulk-Import von Dokumenten in einer Auflistung. Beachten Sie die gespeicherte Prozedur gebundene Ausführung Behandlung durch Überprüfen des booleschen Rückgabewert von CreateDocument, und verwendet dann die Anzahl der Dokumente in jedem Aufruf der gespeicherten Prozedur eingefügt zu Fortschritt in Batches fortsetzen.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Datenbanktrigger
### <a name="database-pre-triggers"></a>Vor Datenbanktrigger
DocumentDB enthält Trigger, die ausgeführt oder durch einen Vorgang für ein Dokument ausgelöst werden. Beispielsweise können Sie vor Trigger Erstellen eines Dokuments – dieser vor Trigger wird ausgeführt, bevor das Dokument erstellt wird. Nachfolgend ein Beispiel wie vor Trigger zum Überprüfen der Eigenschaften eines Dokuments, die erstellt wird:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


Und die entsprechenden Node.js clientseitige Registrierungscode für den Trigger:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Vor Trigger keinen Eingabeparameter. Das Anforderungsobjekt kann verwendet werden, die dem Vorgang zugeordnete Anforderung bearbeiten. Hier vor Trigger bei der Erstellung eines Dokuments ausgeführt wird und der Nachrichtentext der Anforderung enthält das Dokument im JSON-Format erstellt werden.   

Wenn Trigger registriert werden, können Benutzer die Vorgänge mit ausgeführt werden können. Dieser Trigger wurde mit TriggerOperation.Create, was bedeutet, dass Folgendes nicht zulässig ist.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Nach der Datenbanktrigger
Nach der Trigger wie vor Trigger einen Vorgang für ein Dokument zugeordnet sind und keine Eingabeparameter. Sie führen, **nachdem** der Vorgang abgeschlossen wurde und haben Zugriff auf die Antwortnachricht an den Client gesendet.   

Das folgende Beispiel zeigt nach der Trigger in Aktion:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Trigger kann registriert werden, wie im folgenden Beispiel gezeigt.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Dieser Trigger fragt das Metadatendokument und Details über das neu erstellte Dokument aktualisiert.  

Zu beachten ist ist die Ausführung von **Transaktionen** von Triggern in DocumentDB. Dieser Post-Trigger wird als Teil derselben Transaktion wie die Erstellung des Originaldokuments. Wenn wir eine Ausnahme nach Abzug (etwa wenn das Metadatendokument aktualisieren können), die gesamte Transaktion fehl und rückgängig gemacht werden. Kein Dokument erstellt, und eine Ausnahme zurückgegeben.  

##<a id="udf"></a>Benutzerdefinierte Funktionen
Benutzerdefinierte Funktionen (UDFs) zum Erweitern der DocumentDB SQL Query-Grammatik und benutzerdefinierte Geschäftslogik implementieren. Sie können nur aus aufgerufen werden in Abfragen. Sie haben keinen Zugriff auf das Kontextobjekt und als Compute nur JavaScript verwendet werden sollen. UDFs können daher sekundäre Kopien des DocumentDB-Dienstes ausgeführt werden.  
 
Im folgende Beispiel erzeugt ein UDF basierend auf Sätze für verschiedene Einnahmen Klammern steuern berechnen und anschließend innerhalb einer Abfrage verwendet, um alle Personen zu finden, die mehr als 20.000 steuern gezahlt.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


Der UDF-Datei kann anschließend in Abfragen wie im folgenden Beispiel verwendet werden:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScript LINQ-Abfrage-API
Neben Ausstellen von Abfragen mithilfe DocumentDBs-SQL-Grammatik, kann serverseitige SDK optimierte Abfragen über eine fluent JavaScript-Schnittstelle ohne SQL. Die Abfrage JavaScript API können Sie programmgesteuert Abfragen erstellen Prädikat Funktionen an verkettbare Funktion übergeben Ruft mit einer Syntax vertraut Array einbauten und gängige JavaScript-Bibliotheken wie Lodash des ECMAScript5. Abfragen werden von JavaScript Laufzeit auszuführenden effizient mit DocumentDB Indizes analysiert.

> [AZURE.NOTE]`__` (doppelte Unterstrich) ist ein Alias für `getContext().getCollection()`.
> <br/>
> Anders gesagt können Sie `__` oder `getContext().getCollection()` Abfrage JavaScript-API auf.

Unterstützte Funktionen umfassen:
<ul>
<li>
<b>... Chain(). Wert ([Rückruf] [, Optionen])</b>
<ul>
<li>
Startet einen verketteten Aufruf beendet werden muss mit value().
</li>
</ul>
</li>
<li>
<b>Filter (PredicateFunction [, Optionen] [, Rückruf])</b>
<ul>
<li>
Filtert die Eingabe mit einer Prädikatfunktion wahr/falsch, um filter ein-input Dokumente in der Ergebnismenge ab. Dies verhält sich ähnlich wie eine WHERE-Klausel in SQL.
</li>
</ul>
</li>
<li>
<b>Zuordnung (TransformationFunction [, Optionen] [, Rückruf])</b>
<ul>
<li>
Wendet eine Funktion Transformation der jedes Eingabeelements ein JavaScript-Objekt oder ein Wert zugeordnet ist. Dies verhält sich ähnlich wie eine SELECT-Klausel in SQL.
</li>
</ul>
</li>
<li>
<b>Rupfen ([PropertyName] [, Optionen] [, Rückruf])</b>
<ul>
<li>
Dies ist eine Kurzform für eine Karte, die den Wert einer einzelnen Eigenschaft jedes Eingabeelements extrahiert.
</li>
</ul>
</li>
<li>
<b>Glätten ([IsShallow] [, Optionen] [, Rückruf])</b>
<ul>
<li>
Kombiniert und fasst Arrays jedes Eingabeelements in einem einzigen Array. Dies verhält sich ähnlich wie SelectMany in LINQ.
</li>
</ul>
</li>
<li>
<b>SortBy ([Prädikat] [, Optionen] [, Rückruf])</b>
<ul>
<li>
Erstellen Sie einen neuen Satz von Dokumenten Dokumente im Eingabedokument Stream mit dem angegebenen Prädikat Aufsteigend sortieren. Dies verhält sich ähnlich wie eine ORDER BY-Klausel in SQL.
</li>
</ul>
</li>
<li>
<b>SortByDescending ([Prädikat] [, Optionen] [, Rückruf])</b>
<ul>
<li>
Erstellen Sie einen neuen Satz von Dokumenten Dokumente im Eingabedokument Stream mit dem angegebenen Prädikat Absteigend sortieren. Dies verhält sich ähnlich wie ein X DESC ORDER BY-Klausel in SQL.
</li>
</ul>
</li>
</ul>


Beim in-Prädikat oder Auswahl Funktionen enthalten, erhalten die folgenden JavaScript-Konstrukten automatisch optimiert auf DocumentDB Indizes:

* Einfache Operatoren: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literale, einschließlich der Objektliteral: {}
* Var Rückgabe

Folgende JavaScript erstellt werden DocumentDB Indizes nicht optimiert abrufen:

* Steuern (z. B. sollte, während)
* Funktionsaufrufe

Weitere Informationen finden Sie in unseren [Serverseitigen JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Beispiel: Schreiben Sie eine gespeicherte Prozedur mit der JavaScript-API-

Das folgende Codebeispiel ist ein Beispiel für die Verwendung der JavaScript-Abfrage-API im Kontext einer gespeicherten Prozedur. Die gespeicherte Prozedur fügt ein Dokument ein Eingabeparameter angegeben und aktualisiert einen Metadaten Dokuments die `__.filter()` -Methode mit MinSize, MaxSize und TotalSize anhand des Eingabedokuments Size-Eigenschaft.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL, Javascript Spickzettel:
Die folgende Tabelle zeigt verschiedene SQL-Abfragen und die entsprechenden JavaScript.

Wie bei SQL-Abfragen Eigenschaftsschlüssel (z.B. `doc.id`) beachtet werden.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>Abfrage der JavaScript-API</th>
<th>Details</th>
</tr>
<tr>
<td>
<pre>
Wählen Sie * aus Dokumenten
</pre>
</td>
<td>
<pre>
__.Map(Function(DOC) {return Doc;});
</pre>
</td>
<td>Alle Dokumente (mit Fortsetzungstoken paginiert) ist.</td>
</tr>
<tr>
<td>
<pre>
Wählen Sie docs.id, docs.message als msg docs.actions von Dokumenten
</pre>
</td>
<td>
<pre>
__.Map(Function(DOC) {zurückgeben {Id: doc.id, msg: doc.message, Aktionen: doc.actions};});
</pre>
</td>
<td>Projekte Id, Nachricht (Alias msg) und alle Dokumente handeln.</td>
</tr>
<tr>
<td>
<pre>
Wählen Sie * aus Dokumenten WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Filter(Function(DOC) {zurückgeben doc.id === "X998_Y998"});
</pre>
</td>
<td>Abfragen für Dokumente mit dem Prädikat: Id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Wählen Sie * aus Dokumenten, ARRAY_CONTAINS(docs. Tags 123)
</pre>
</td>
<td>
<pre>
__.Filter(Function(x) {zurückgeben x.Tags & & x.Tags.indexOf(123) > -1;});
</pre>
</td>
<td>Abfragen für Dokumente, die eine Eigenschaft Tags und Tag ist ein Array mit dem Wert 123.</td>
</tr>
<tr>
<td>
<pre>
Wählen Sie docs.id, docs.message als msg von Dokumenten WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {zurückgeben doc.id === "X998_Y998"}) .map(function(doc) {zurückgeben {Id: doc.id, msg: doc.message};}) .value();
</pre>
</td>
<td>Abfragen für Dokumente mit einem Prädikat Id = "X998_Y998" und geben dann die Id und die Meldung (Alias msg).</td>
</tr>
<tr>
<td>
<pre>
SELECT VALUE-Tag aus Dokumenten JOIN Tag IN Dokumente. Tags ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {return Doc. Tag & & Array.isArray (Doc. Tags); .sortBy(function(doc)}) {return Doc._ts;}) .pluck("tags") .flatten() .value()
</pre>
</td>
<td>Dokumente eine Arrayeigenschaft Tags, filtert und sortiert die resultierenden Dokumente nach der _ts Zeitstempel Systemeigenschaft und Projekte + fasst Tags Array.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Runtime-Unterstützung
[Serverseitige DocumentDB JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/) bietet Unterstützung für die meisten gängigen JavaScript Sprachfeatures wie standardisierte von [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Sicherheit
JavaScript gespeicherte Prozeduren und Trigger sind Sandbox, sodass Effekte ein Skript zum anderen Verbreitung werden ohne Transaktion Snapshotisolation auf Datenbankebene. Die Runtime-Umgebung sind gebündelt jedoch nach jeder Ausführung des Kontexts gereinigt. Daher sind sie unbedingt sicher unerwünschte Nebenwirkungen voneinander.

### <a name="pre-compilation"></a>Vorkompilierung
Gespeicherte Prozeduren, Trigger und UDFs werden implizit in vorkompiliert Byte-Code-Format bei jeder Skriptaufruf Kompilierung Kosten vermeiden. Dadurch Aufrufe gespeicherter Prozeduren sind schnell und haben einen geringen Speicherbedarf.

## <a name="client-sdk-support"></a>Client-SDK-Unterstützung
Neben dem Client [Node.js](documentdb-sdk-node.md) unterstützt DocumentDB [.NET,](documentdb-sdk-dotnet.md) [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)und [Python-SDKs](documentdb-sdk-python.md). Gespeicherte Prozeduren, Trigger und UDFs erstellt werden und diese SDKs sowie mit ausgeführt. Im folgenden Codebeispiel wird das Erstellen und Ausführen einer gespeicherten Prozedur mit .NET Client. Beachten Sie, wie .NET sind in der gespeicherten Prozedur als JSON übergeben und lesen.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Dieses Beispiel veranschaulicht, wie mit [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) vor Trigger erstellen und ein Dokument mit dem Trigger aktiviert. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


Und das folgende Beispiel zeigt, wie eine benutzerdefinierte Funktion (UDF) erstellt und in einer [DocumentDB-SQL-Abfrage](documentdb-sql-query.md)verwenden.

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST-API
Alle DocumentDB-Operationen können auf Rest ausgeführt werden. Gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen können in einer Auflistung mithilfe von HTTP POST registriert werden. Folgendes ist ein Beispiel für eine gespeicherte Prozedur zu registrieren:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Die gespeicherte Prozedur wird durch Ausführen einer POST-Anforderung mit dem URI Dbs/Testdb/colls/TestColl/Routeninformationen mit den Text der gespeicherten Prozedur erstellen registriert. Trigger und UDF-Dateien können ebenso mit POST /triggers und /udfs bzw. registriert werden.
Diese gespeicherte Prozedur kann dann eine POST-Anforderung gegen den Link Ressource ausgeführt werden:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Hier wird die Eingabe für die gespeicherte Prozedur im Hauptteil Anforderung übergeben. Beachten Sie, dass die Eingabe als JSON-Array der Eingabeparameter übergeben wird. Die gespeicherte Prozedur nimmt die erste Eingabe als Dokument eines Antworttexts. Die Antwort erhalten wir lautet wie folgt:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Trigger im Gegensatz zu gespeicherten Prozeduren werden nicht direkt ausgeführt. Stattdessen werden sie als Teil eines Vorgangs in einem Dokument ausgeführt. Wir können die Trigger mit einer Anforderung mit HTTP-Header angeben. Sieht die Anforderung zum Erstellen eines Dokuments.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Hier wird im x-ms-documentdb-pre-trigger-include-Header vor Trigger mit der Anforderung angegeben. Entsprechend werden nach der Trigger im x-ms-documentdb-post-trigger-include-Header angegeben. Beachten Sie, dass beide vor und nach der Trigger für eine bestimmte Anforderung angegeben werden.

## <a name="sample-code"></a>Beispielcode

Weitere serverseitigen Code-Beispiele (einschließlich [Massen löschen](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)und [Aktualisieren](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) finden Sie auf unserer [Github Repository](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Haasau gespeicherte Prozedur freigeben möchten? Bitte senden Sie uns eine Pull-Anforderung. 

## <a name="next-steps"></a>Nächste Schritte

Haben Sie eine oder mehrere gespeicherte Prozeduren, Trigger und benutzerdefinierten Funktionen erstellt, können sie laden und in der Azure-Portal mit Skript-Explorer anzeigen. Weitere Informationen finden Sie in der [Ansicht gespeicherte Prozeduren, Trigger und benutzerdefinierten Funktionen DocumentDB-Skript-Explorer verwenden](documentdb-view-scripts.md).

Sie können auch die folgenden Verweise und Ressourcen im Pfad DocumentDB serverseitige Programmierung erfahren hilfreich:

- [Azure DocumentDB SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript-JSON-Typsystem](http://www.json.org/js.html) 
- [Sichere und Portable Datenbank Erweiterbarkeit](http://dl.acm.org/citation.cfm?id=276339) 
- [Service-orientierte Architektur](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [Der Microsoft SQL Server hosten](http://dl.acm.org/citation.cfm?id=1007669)

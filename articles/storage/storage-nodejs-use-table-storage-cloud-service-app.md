<properties 
    pageTitle="Web-app mit Tabellenspeicher (Node.js) | Microsoft Azure" 
    description="Ein Lernprogramm Web App mit Express Tutorial Azure Storage Services und Azure-Modul erstellt." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>Node.js Web Application Speicher

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erweitern Sie die Anwendung mit Microsoft Azure-Clientbibliotheken für Node.js Datenmanagement Services arbeiten im Lernprogramm [Node.js Web Application mit Express] erstellt. Erweitern Sie Ihre Anwendung eine Aufgabenliste webbasierte Anwendung erstellen, die auf Azure bereitgestellt werden kann. Aufgabenliste kann Benutzer Aufgaben abrufen, neue Aufgaben hinzufügen und Aufgaben als erledigt markieren.

Die Aufgabenelemente werden in Azure-Speicher gespeichert. Azure-Speicher stellt unstrukturierte Datenspeicher fehlertolerante und hoher Verfügbarkeit. Azure-Speicher enthält mehrere Datenstrukturen, können gespeichert und Datenzugriff und Speicherdienste von Node.js oder über REST-APIs in Azure SDK enthaltenen APIs nutzen. Weitere Informationen finden Sie unter [Speichern von und Zugreifen auf Daten in Azure].

Diese praktische Einführung geht die [Anwendung Node.js] und [Node.js Express][Node.js Web Application using Express] Lernprogramme abgeschlossen haben.

Lernen Sie Folgendes:

-   Arbeiten mit Jade Vorlagenmodul
-   Arbeiten mit Azure Data Management services

Screenshot der abgeschlossenen Anwendung lautet wie folgt:

![Abgeschlossene Webseite in InternetExplorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Speicherung von Anmeldeinformationen festlegen in Web.Config

Zugriff auf Azure Storage müssen Storage Anmeldeinformationen übergeben. Dazu nutzen Sie web.config ApplicationSettings.
Diese Einstellung werden als übergeben werden Umgebungsvariablen Knoten die Azure SDK gelesen werden.

> [AZURE.NOTE] Speicher-Anmeldeinformationen werden nur verwendet, wenn die Anwendung in Azure bereitgestellt wird. Die Anwendung wird im Emulator ausgeführt Speicheremulator verwendet.

Führen Sie die folgenden Schritte zum Abrufen der Anmeldeinformationen für das Speicherkonto und web.config Settings hinzufügen:

1.  Wenn es nicht bereits geöffnet, starten Azure PowerShell im **Startmenü** **auf alle Programme, Azure**erweitern **Azure PowerShell**Maustaste und wählen Sie **Als Administrator ausführen**.

2.  Wechseln Sie zu dem Ordner mit der Anwendung. C:\\Knoten\\Tasklist\\WebRole1.

3.  Geben Sie das folgende Cmdlet Speicher Kontoinformationen Azure Powershell-Fenster:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    Dies ruft die Liste der Speicherkonten und Ihr gehosteter Dienst Schlüssel zugeordnet.

    > [AZURE.NOTE] Da Azure SDK ein Speicherkonto erstellt, wenn Sie einen Service bereitstellen, sollte ein Speicherkonto vorhanden aus Bereitstellen der Anwendung in den vorherigen Handbüchern.

4.  Die Datei **ServiceDefinition.csdef** enthält die Umgebung verwenden, die bei der Bereitstellung der Anwendungdes in Azure verwendet werden:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Legen Sie den folgenden Block **Umgebung** Element ersetzen {SPEICHERKONTO} und {SPEICHERZUGRIFFSSCHLÜSSEL} mit dem Kontonamen und der primäre Schlüssel für das Speicherkonto für die Bereitstellung verwendet werden soll:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![Inhalt der web.cloud.config-Datei](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Speichern Sie die Datei, und schließen Sie Editor.

### <a name="install-additional-modules"></a>Installieren Sie zusätzliche Module

2. Verwenden Sie den folgenden Befehl installieren [Azure] [Knoten Uuid] [Nconf] und [Async] lokal sowie einen Eintrag für sie in die Datei **package.json** zu speichern:

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    Die Ausgabe dieses Befehls sollte etwa wie folgt aussehen:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Verwenden den Dienst in einer Anwendung Knoten

In diesem Abschnitt wird die grundlegende Anwendung **express** Befehl erstellt, indem eine **task.js** -Datei enthält das Modell für die Vorgänge erweitern. Auch die vorhandenen **app.js** ändern und erstellen eine neue **tasklist.js** -Datei, die das Modell verwendet.

### <a name="create-the-model"></a>Modell erstellen

1. Erstellen Sie ein neues Verzeichnis **Modelle**im Verzeichnis **WebRole1** .

2. Erstellen Sie eine neue Datei namens **task.js**im Verzeichnis **Modelle** . Diese Datei enthält das Modell für die Aufgaben von der Anwendung erstellt.

3. Fügen Sie am Anfang der Datei **task.js** den folgenden Code erforderliche Bibliotheken verweisen:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Als Nächstes fügen Sie Code zur Definition und Task-Objekt exportieren. Dieses Objekt ist verantwortlich für die Verbindung mit der Tabelle.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Fügen Sie den folgenden Code zusätzliche Methoden für das Task-Objekt definieren die Interaktionen mit den Daten in der Tabelle:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Speichern Sie und schließen Sie die Datei **task.js** .

### <a name="create-the-controller"></a>Erstellen des Controllers

1. Im Verzeichnis **WebRole1-Routen** eine neue Datei mit dem Namen **tasklist.js** erstellen und in einem Texteditor öffnen.

2. Fügen Sie folgenden Code zum **tasklist.js**. Dadurch lädt Azure und Async-Module, die von **tasklist.js**verwendet werden. Außerdem definiert die **TaskList** -Funktion, die eine Instanz des **Task** -Objekts übergeben, die zuvor von uns definiert:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Weitere Datei **tasklist.js** durch Hinzufügen der Methoden **ShowTasks**, **AddTask**und **CompleteTasks**:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Speichern Sie die Datei **tasklist.js** .

### <a name="modify-appjs"></a>App.js ändern

1. Öffnen Sie im Verzeichnis **WebRole1** **app.js** Datei in einem Texteditor. 

2. Fügen Sie am Anfang der Datei folgenden Azure Modul geladen und Tabellenschlüssel Name und Partition:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. Scrollen Sie sich in der Datei app.js Sie die folgende Zeile finden:

        app.use('/', routes);
        app.use('/users', users);

    Ersetzen Sie die oben genannten Zeilen mit den folgenden Code. Das Initialisieren einer Instanz der <strong>Aufgabe</strong> mit Ihres Speicherkontos. Dies wird an <strong>TaskList</strong>übergeben sie mit dem Dienst kommunizieren mit:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Speichern Sie die Datei **app.js** .

### <a name="modify-the-index-view"></a>Index ändern

1. Wechseln Sie zum Verzeichnis **Ansichten** , und öffnen Sie die Datei **index.jade** in einem Texteditor.

2. Ersetzen Sie den Inhalt der Datei **index.jade** durch den folgenden Code. Definiert die Ansicht zum Anzeigen von vorhandenen Aufgaben sowie ein Formular für neue Aufgaben hinzufügen und vorhandene als abgeschlossen markieren.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Speichern Sie und schließen Sie die Datei **index.jade** .

### <a name="modify-the-global-layout"></a>Globale Layout ändern

Die Datei **layout.jade** im Verzeichnis **Ansichten** dient als globale Vorlage für andere **.jade** -Dateien. In diesem Schritt ändern Sie zur Verwendung [Twitter Bootstrap](https://github.com/twbs/bootstrap)ist ein Toolkit, die gut aussehende Website entwerfen vereinfacht.

1. Downloaden Sie und extrahieren Sie die Dateien für [Twitter Bootstrap](http://getbootstrap.com/). Kopieren Sie die Datei **bootstrap.min.css** aus dem **bootstrap\\Dist\\Css** Ordner die **öffentlichen\\Stylesheets** Verzeichnis der Anwendung Tasklist.

2. Im Ordner **Ansichten** **layout.jade** in Ihrem Texteditor öffnen Sie und Ersetzen Sie den Inhalt durch Folgendes:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Speichern Sie die Datei **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>Ausführen der Anwendung im Emulator

Verwenden Sie den folgenden Befehl zum Starten der Anwendung im Emulator.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

Der Browser wird geöffnet und zeigt die folgende Seite:

![Eine Web ausgelagert Titel Meine Aufgabenliste mit einer Tabelle mit den Tasks und Felder eine neue Aufgabe hinzuzufügen.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Verwenden Sie das Formular Elemente hinzufügen oder entfernen Sie vorhandene Elemente, indem Sie sie als erledigt markieren.

## <a name="publishing-the-application-to-azure"></a>Veröffentlichen Sie die Anwendung in Azure


Rufen Sie im Windows PowerShell-Fenster das folgende Cmdlet erneut Ihr gehosteten Dienst in Azure.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

Ersetzen Sie **Myuniquename** durch einen eindeutigen Namen für diese Anwendung. Ersetzen Sie **Datacentername** durch den Namen eines Azure Rechenzentrums wie **Westen der USA**.

Nachdem die Bereitstellung abgeschlossen ist, erhalten Sie eine Antwort ähnlich der folgenden:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Wie zuvor da Angabe der **-Starten** Option Browser wird geöffnet und zeigt die Anwendung in Azure ausgeführt wird, wenn die Veröffentlichung abgeschlossen ist.

![Ein Browserfenster Meine Aufgabenliste anzuzeigen. Die URL gibt an, dass die Seite nun in Azure gehostet wird.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Anhalten und Löschen der Anwendung

Nach dem Bereitstellen der Anwendung, sollten Sie ihn deaktivieren, damit Kosten oder erstellen und Bereitstellen von anderen Programmen innerhalb kostenlose Testversion.

Azure Rechnungen web Instanzen pro Stunde Server verbraucht.
Serverzeit verbraucht, nachdem die Anwendung bereitgestellt wird, auch wenn die Instanzen nicht ausgeführt und beendet werden.

Die folgenden Schritte zeigen, wie zu Ihrer Anwendung löschen.

1.  Stoppen Sie im Fenster Windows PowerShell Service-Bereitstellung, die im vorherigen Abschnitt mit dem folgenden Cmdlet erstellt:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    Beenden des Dienstes kann mehrere Minuten dauern. Wenn der Dienst angehalten wird, erhalten Sie eine Meldung beendet wurde.

3.  Um den Dienst zu löschen, rufen Sie das folgende Cmdlet:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Geben Sie bei Aufforderung **Y** ein, um den Dienst zu löschen.

    Löschen des Dienstes kann mehrere Minuten dauern. Nach dem Löschen des Dienstes erhalten Sie eine Meldung, dass der Dienst gelöscht wurde.

  [Node.js Web Application mit Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Speichern von und Zugreifen auf Daten in Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js Anwendung]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 

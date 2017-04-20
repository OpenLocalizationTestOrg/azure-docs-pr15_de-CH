<properties
    pageTitle="Erste Schritte mit Azure CDN SDK für Node.js | Microsoft Azure"
    description="Informationen Sie zum Node.js Azure CDN zu schreiben."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Erste Schritte mit Azure CDN

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

[Azure CDN SDK Node.js](https://www.npmjs.com/package/azure-arm-cdn) können Erstellung und Verwaltung von CDN Profile und Endpunkte.  Dieses Lernprogramm führt durch die Erstellung einer einfachen Node.js-Konsole-Anwendung, die mehrere verfügbare Vorgänge veranschaulicht.  Dieses Lernprogramm soll nicht alle Aspekte der Azure CDN SDK für Node.js ausführlich beschrieben.

Damit dieses Lernprogramm abgeschlossen, Sie sollten schon [Node.js](http://www.nodejs.org) **4.x.x** oder höher installiert und konfiguriert.  Sie können einen beliebigen Texteditor Node.js-Anwendung erstellen möchten.  Dieses Lernprogramm verwendet [Visual Studio Code](https://code.visualstudio.com)schreiben.  

> [AZURE.TIP] [Projekt aus diesem Lernprogramm](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) steht zum Download auf MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Erstellen Sie das Projekt und fügen NPM Abhängigkeiten hinzu

Unsere Azure AD Anwendung Erlaubnis CDN Profile und Endpunkte innerhalb der Gruppe verwalten und eine Ressourcengruppe für CDN Profile erstellt haben, können wir beginnen unsere Anwendung.

Erstellen Sie einen Ordner zum Speichern der Anwendung.  Eine Konsole mit den Tools Node.js im aktuellen Pfad festlegen Sie die aktuelle Position in diesem neuen Ordner und initialisieren Sie Projekt ausführen:
    
    npm init
    
Es wird dann eine Reihe von Fragen zum Projekt initialisieren angezeigt.  **Einstiegspunkt**verwendet dieses Lernprogramm *app.js*.  Sie können meine Optionen im folgenden Beispiel sehen.

![Init-Ausgabe NPM](./media/cdn-app-dev-node/cdn-npm-init.png)

Das Projekt wird jetzt mit einer *packages.json* -Datei initialisiert.  Unser Projekt soll einige Azure Bibliotheken im NPM-Pakete enthalten.  Wir verwenden die Laufzeit Azure Client für Node.js (ms Rest Azure) und Azure CDN-Clientbibliothek für Node.js (Azure-Arm-cd).  Fügen Sie diese dem Projekt als Abhängigkeit.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Anschließend die Pakete sieht installieren, die *package.json* Datei ähnlich wie dieses Beispiel (Version Zahlen abweichen):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Schließlich mit der Text-Editor, eine leere Textdatei erstellen und im Stammverzeichnis des unsere Projektordner als *app.js*speichern.  Wir nun können beginnen, Code zu schreiben.

## <a name="requires-constants-authentication-and-structure"></a>Erfordert Konstanten, Authentifizierung und Struktur

Mit Sie in unserem Editor öffnen die aus *app.js* lassen sich die grundlegende Struktur des Programms geschrieben.

1. Fügen Sie den "erfordert" für unsere NPM Pakete oben mit folgenden:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Wir müssen einige Konstanten definieren unserer Methoden verwenden.  Fügen Sie Folgendes hinzu.  Platzhalter, einschließlich ersetzen die ** &lt;Winkel&gt;**, mit eigenen Werten.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Dann wir CDN-Verwaltungsclient instanziieren und geben unseren Anmeldeinformationen.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Wenn Sie einzelne Benutzer verwenden, werden diese beiden Zeilen etwas anders aussehen.

    >[AZURE.IMPORTANT] Verwenden Sie dieses Codebeispiel nur, wenn Benutzer Authentifizierung statt Dienstprinzipal Auswahl.  Achten Sie darauf, Ihre Benutzer-Anmeldeinformationen und geheim halten.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Die Elemente ersetzen ** &lt;Winkel&gt; ** mit den entsprechenden Informationen.  Für `<redirect URI>`, verwenden Sie die Umleitung URI eingegeben, wenn die Anwendung in Azure AD registriert.
    

4.  Unsere Konsolenanwendungsprojekt Node.js wird einige Befehlszeilenparameter.  Überprüfen wir, dass mindestens ein Parameter übergeben wurde.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Das bringt uns zu den wichtigsten Teil unseres Programms, wo wir Verzweigung auf andere Funktionen Parameter übergeben wurden.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  An mehreren Stellen im Programm müssen wir die richtige Anzahl von Parametern übergeben wurden und Hilfe anzeigen, wenn sie nicht korrekt aussehen.  Erstellen Sie Funktionen dazu.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Schließlich sind wir auf dem CDN Management Client verwenden werden asynchron müssen sie eine Methode aufrufen, wenn sie fertig sind.  Lassen Sie die Ausgabe der CDN-Management-Client (sofern vorhanden) angezeigt und die Anwendung ordnungsgemäß beendet.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Steht die grundlegende Struktur des Programms sollten wir die Funktionen aufgerufen wurden basierend auf unsere Parameter erstellen.

## <a name="list-cdn-profiles-and-endpoints"></a>Liste CDN Profile und Endpunkte

Beginnen wir mit unseren vorhandenen Profile und Endpunkte.  Codekommentare bieten die erwartete Syntax, damit wir wissen, wo jeder Parameter geht.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Erstellen Sie CDN Profile und Endpunkte

Als Nächstes schreiben wir die Funktionen zum Erstellen von Profilen und Endpunkte.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Löschen Sie einen Endpunkt

Wenn der Endpunkt erstellt wurde, wird eine allgemeine Aufgabe, die wir in unserem Programm ausführen möchten Inhalte in unseren Endpunkt entfernt.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>Löschen von CDN Profile und Endpunkte

Die letzte Funktion anschaulich wir löscht Endpunkte und Profile.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Ausführen des Programms

Wir können jetzt unsere Node.js-Programm mithilfe unserer bevorzugten Debugger ausführen oder an der Konsole.

> [AZURE.TIP] Wenn Sie Visual Studio-Code als den Debugger verwenden, müssen Sie Ihre Umgebung einrichten, die Befehlszeilenparameter übergeben.  Visual Studio-Code wird in der Datei **lanuch.json** .  Eine Eigenschaft mit dem Namen **Args** gesucht und ein Array von String-Werten für die Parameter hinzufügen, sodass es aussieht: `"args": ["list", "profiles"]`.

Beginnen wir mit unserer Profile aufgelistet.

![Liste Profile](./media/cdn-app-dev-node/cdn-list-profiles.png)

Wir haben wieder ein leeres Array.  Da wir in unserer Ressourcengruppe Profile haben, erwartet.  Wir erstellen ein Profil.

![Profil erstellen](./media/cdn-app-dev-node/cdn-create-profile.png)

Nun fügen Sie einen Endpunkt.

![Erstellen](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Schließlich löschen Sie das Profil.

![Profil löschen](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Nächste Schritte

Das fertige Projekt aus dieser exemplarischen Vorgehensweise, [Laden Sie das Beispiel](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74)zu sehen.

Um die Referenz für Azure CDN SDK Node.js finden Sie unter [Referenz](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/)anzeigen.

Zusätzliche Dokumentation Azure SDK Node.js suchen, [vollständige Referenz](http://azure.github.io/azure-sdk-for-node/)anzeigen.

Verwalten Sie Ihre Ressourcen CDN mit [PowerShell](./cdn-manage-powershell.md).


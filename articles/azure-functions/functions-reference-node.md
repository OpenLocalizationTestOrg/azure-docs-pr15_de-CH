<properties
    pageTitle="Azure Funktionen NodeJS-Entwicklerreferenz | Microsoft Azure"
    description="Azure-Funktionen mithilfe von NodeJS zu verstehen."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktionen, Funktionen, Verarbeitung, Webhooks, dynamische Compute, serverlose Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure Funktionen NodeJS-Entwicklerreferenz

> [AZURE.SELECTOR]
- [C#-Skript](../articles/azure-functions/functions-reference-csharp.md)
- [F#-Skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Knoten-JavaScript-Erfahrung für Azure erleichtert die übergebene Funktion Exportieren eine `context` -Objekt für die Kommunikation mit der Common Language Runtime und zum Empfangen und Senden von Daten über Bindings.

Es wird vorausgesetzt, dass die [Entwicklerreferenz Azure Funktionen](functions-reference.md)bereits gelesen haben.

## <a name="exporting-a-function"></a>Exportieren einer Funktion

Alle JavaScript-Funktionen müssen exportiert einen einzelnen `function` über `module.exports` für die Laufzeit der Funktion und ausführen. Diese Funktion muss immer enthalten eine `context` Objekt.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Bindungen `direction === "in"` als Funktionsargumente, d. h. Sie können weitergegeben [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) dynamisch neue Eingaben behandeln (z. B. mithilfe von `arguments.length` durchlaufen alle Eingaben). Diese Funktion ist sehr praktisch, wenn Sie nur einen Trigger mit ohne weiteren Eingaben, ohne dazu Ihre Triggerdaten zugreifen kann Ihre `context` Objekt.

Argumente sind immer übergeben der Funktion in der Reihenfolge auftretende *function.json*, auch wenn Sie sie in der Exports-Anweisung angeben. Angenommen, Sie haben `function(context, a, b)` ändern sie die `function(context, a)`, Sie erhalten weiterhin den Wert der `b` Funktionscode auf `arguments[3]`.

Alle Bindungen, Richtung, werden auch übergeben an die `context` -Objekt (siehe unten). 

## <a name="context-object"></a>Context-Objekt

Die Laufzeit verwendet ein `context` Objekt Daten zu und von der Funktion übergeben und mit der Common Language Runtime kommunizieren.

Das Kontextobjekt ist immer der erste Parameter einer Funktion und sind enthalten, da es Methoden, wie enthält `context.done` und `context.log` , müssen ordnungsgemäß zur Laufzeit verwendet. Sie können das Objekt benennen beliebig (d. h. `ctx` oder `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>Context.Bindings

Die `context.bindings` Objekt erfasst alle Eingabe- und Daten. Daten wird auf der `context.bindings` Objekt über die `name` -Eigenschaft der Bindung. Beispielsweise folgenden Bindungsdefinition in *function.json*angegeben, Sie können auf den Inhalt des der Warteschlange über `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

Die `context.done` Funktion weist der Common Language Runtime, danach ausgeführt. Dies ist wichtig, rufen Sie mit der Funktion. Andernfalls wird die Laufzeit nie wissen, dass Ihre Funktion abgeschlossen. 

Die `context.done` Funktion können Sie wieder einen benutzerdefinierten Fehler die Common Language Runtime sowie eine Eigenschaftensammlung Eigenschaften die Eigenschaften überschrieben werden die `context.bindings` Objekt.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>Context.log(Message)

Die `context.log` -Methode können Sie Log-Anweisungen ausgeben, die zusammen für die Protokollierung korreliert werden. Verwenden Sie `console.log`, Ihre Nachrichten zeigen nur für Prozess-Protokollierung, die nicht so nützlich.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

Die `context.log` -Methode unterstützt denselben Parameter, der Knoten [util.format-Methode](https://nodejs.org/api/util.html#util_util_format_format) unterstützt. Also beispielsweise code wie folgt:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

kann wie folgt geschrieben werden:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP-Trigger: context.req und context.res

Bei HTTP-Trigger ist ein allgemeines Muster mit `req` und `res` für die HTTP-Anforderung und-Antwort Objekte wir beschlossen, einfach auf das Kontextobjekt anstatt der vollständige `context.bindings.name` Muster.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Knoten-Version und Verwalten von Paketen

Knoten-Version ist gesperrt auf `5.9.1`. Wir untersuchen Hinzufügen von Unterstützung für weitere Versionen und konfigurierbar machen.

Pakete zählen in Ihrer Funktion durch Hochladen einer Datei *package.json* Ihre Funktion Ordner im Dateisystem Funktion app. Datei hochladen Sie Anweisungen, finden Sie im Abschnitt **Funktion app aktualisieren** [Azure Funktionen Developer-Referenzthema](functions-reference.md#fileupdate). 

Sie können auch `npm install` in der Funktion Anwendung SCM (Kudu) über die Befehlszeilenschnittstelle:

1. Navigieren Sie zu: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klicken Sie auf **Debuggen Konsole > CMD**.

3. Navigieren Sie zu `D:\home\site\wwwroot\<function_name>`.

4. Führen Sie `npm install`.

Nach der Installation Sie müssen Pakete importieren sie Ihre Funktion auf die übliche Weise (über `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Umgebungsvariablen

Verwenden Sie zum Abrufen einer Umgebungsvariablen oder eine Anwendung Wert `process.env`, wie im folgenden Beispiel gezeigt:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Schreibmaschine-CoffeeScript-Unterstützung

Nicht vorhanden ist, jedoch keine direkte Unterstützung für Auto-Kompilierung Schreibmaschine-CoffeeScript zur Laufzeit so alle würde zum Zeitpunkt der Bereitstellung außerhalb der Runtime behandelt werden müssen. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in folgenden Ressourcen:

* [Azure Funktionen-Entwicklerreferenz](functions-reference.md)
* [Azure Funktionen C#-Entwicklerreferenz](functions-reference-csharp.md)
* [Azure Funktionen F#-Entwicklerreferenz](functions-reference-fsharp.md)
* [Azure Funktionen Trigger und Bindung](functions-triggers-bindings.md)

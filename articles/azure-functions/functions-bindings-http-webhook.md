<properties
    pageTitle="Azure Funktionen HTTP und Webhook Bindings | Microsoft Azure"
    description="Verstehen Sie, wie HTTP und Webhook Trigger und Bindungen in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktionen, Funktionen, Verarbeitung, Webhooks, dynamische Compute, serverlose Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure HTTP-Funktionen und Webhook Bindungen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert die konfigurieren und HTTP und Webhook Trigger und Bindungen in Azure-Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>Function.JSON für HTTP und Webhook Bindung

Die Datei *function.json* enthält Eigenschaften für die Anforderung und Antwort.

Eigenschaften der HTTP-Anforderung:

- `name`: Variablenname in den Code für das Request-Objekt (oder der Anforderungstext bei Node.js-Funktionen).
- `type`: Muss auf *HttpTrigger*festgelegt werden.
- `direction`: Muss *in*festgelegt werden. 
- `webHookType`Für WebHook-Trigger sind gültige Werte *Github* *Puffer*und *GenericJson*. Legen Sie für einen HTTP-Trigger, die eine WebHook diese Eigenschaft auf eine leere Zeichenfolge. Weitere Informationen zu WebHooks finden Sie in Abschnitt [WebHook Trigger](#webhook-triggers) .
- `authLevel`: WebHook Trigger gilt nicht. Legen Sie "Funktion", "Anonym" Drop API Voraussetzung oder "Admin" API-Hauptschlüssel erforderlich API-Schlüssel erforderlich. Weitere Informationen finden Sie unter [API-Schlüssel](#apikeys) .

Eigenschaften für die HTTP-Antwort:

- `name`: Variablenname in den Code für das Antwortobjekt.
- `type`: *Http*müssen festgelegt werden.
- `direction`: *Out*muss festgelegt werden. 
 
Beispiel *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook Trigger

Ein WebHook-Trigger ist ein HTTP-Trigger, der folgenden für WebHooks entwickelt Features:

* Für bestimmte WebHook Provider (zurzeit GitHub und Pufferzeit werden unterstützt), überprüft die Laufzeit Funktionen des Anbieters Signatur.
* Node.js-Funktionen stellt Funktionen Runtime Anforderungstext statt das Anforderungsobjekt. Keine spezielle Behandlung für C# Funktionen ist Sie steuern, was durch Angabe der Parameter bereitgestellt wird. Bei Angabe `HttpRequestMessage` man das Anforderungsobjekt. Wenn Sie POCO-Typ angeben, versucht die Laufzeit Funktionen, ein JSON-Objekt im Hauptteil der Anforderung zum Auffüllen der Objekteigenschaften analysieren.
* Zum Auslösen einer WebHook sind Funktion der HTTP-Anforderung ein API-Schlüssels. WebHook HTTP-Trigger ist dies optional.

Informationen zum Einrichten einer GitHub WebHook finden Sie unter [GitHub Developer - WebHooks erstellen](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>URL zum Aufrufen der Funktion

Um eine Funktion aufzurufen, wird eine HTTP-Anforderung an einen URL mit der Funktion app-URL und den Funktionsnamen aus senden:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>API-Schlüssel

Standardmäßig muss ein API-Schlüssel eine HTTP-Anforderung an eine HTTP- oder WebHook Funktion enthalten. Der Schlüssel kann in eine Abfragezeichenfolgenvariable mit dem Namen aufgenommen `code`, in aufgenommen werden ein `x-functions-key` -HTTP-Header. Für nicht WebHook Funktionen angeben, ein API-Schlüssel nicht durch Festlegen der `authLevel` -Eigenschaft "Anonym" in der Datei *function.json* .

API-Werte finden in den *D:\home\data\Functions\secrets* Ordner im Dateisystem Funktion App.  Hauptschlüssel und Funktionstasten werden in der Datei *host.json* festgelegt, wie im folgenden Beispiel gezeigt. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Die Funktionstaste aus *host.json* wird jede Funktion aber nicht deaktivierte Funktion ausgelöst. Der Hauptschlüssel wird jede Funktion und eine Funktion ausgelöst, auch wenn es deaktiviert ist. Sie können eine Funktion der Hauptschlüssel festlegen müssen die `authLevel` -Eigenschaft "Admin". 

Enthält der Ordner *Geheimnisse* JSON-Datei mit demselben Namen wie eine Funktion, die `key` -Eigenschaft in der Datei kann auch auf die Funktion verwendet und diese Taste funktioniert nur mit der Funktion auf. Zum Beispiel der API-Schlüssel für eine Funktion namens `HttpTrigger` im *HttpTrigger.json* im Ordner " *Geheimnisse* " angegeben ist. Hier ist ein Beispiel:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Wenn WebHook Trigger einrichten, teilen Sie nicht den Hauptschlüssel mit dem WebHook-Anbieter. Verwenden Sie einen Schlüssel, die nur mit der Funktion, die das WebHook verarbeitet.  Der Hauptschlüssel kann jede Funktion auslösen verwendet auch Funktionen deaktiviert.

## <a name="example-c-code-for-an-http-trigger-function"></a>Wird C#-Code für eine HTTP-Trigger-Funktion 

Beispielcode sucht nach einem `name` Parameter in der Abfragezeichenfolge oder den Hauptteil der HTTP-Anforderung.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>F#-Beispielcode für eine HTTP-Trigger-Funktion

Beispielcode sucht nach einem `name` Parameter in der Abfragezeichenfolge oder den Hauptteil der HTTP-Anforderung.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Sie benötigen eine `project.json` Datei mit NuGet auf der `FSharp.Interop.Dynamic` und `Dynamitey` Assemblys wie folgt:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Diese NuGet zum Abrufen der Abhängigkeiten und verweisen sie in Ihrem Skript.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Node.js Beispielcode für eine HTTP-Trigger-Funktion 

Dieser Beispielcode sucht nach einem `name` Parameter in der Abfragezeichenfolge oder den Hauptteil der HTTP-Anforderung.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Wird C#-Code für eine Funktion GitHub WebHook 

Dieser Beispielcode meldet GitHub Problem Kommentare.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>Beispielcode F# GitHub WebHook Funktion

Dieser Beispielcode meldet GitHub Problem Kommentare.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Node.js Beispielcode für GitHub WebHook-Funktion 

Dieser Beispielcode meldet GitHub Problem Kommentare.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 

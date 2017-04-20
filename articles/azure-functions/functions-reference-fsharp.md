<properties
    pageTitle="Azure Funktionen F#-Entwicklerreferenz | Microsoft Azure"
    description="Azure-Funktionen mit F# zu verstehen."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure Funktionen, Funktionen, Verarbeitung, Webhooks, dynamische Compute, serverlose Architektur, F#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Azure Funktionen F#-Entwicklerreferenz

> [AZURE.SELECTOR]
- [C#-Skript](../articles/azure-functions/functions-reference-csharp.md)
- [F#-Skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F# für Azure ist eine Lösung für kleine Stücke oder "Funktionen" einfach in der Cloud ausgeführt. Daten fließen in die F#-Funktion über Argumente. Argumentnamen gemäß `function.json`, und vordefinierte Namen für den Zugriff auf Elemente wie die Funktion Protokollierung und Abbruch Token.

Es wird vorausgesetzt, dass die [Entwicklerreferenz Azure Funktionen](functions-reference.md)bereits gelesen haben.

## <a name="how-fsx-works"></a>Funktionsweise von ".fsx"

Ein `.fsx` Datei ist ein F#-Skript. Es kann als F#-Projekt vorstellen, die in einer einzigen Datei enthalten ist. Die Datei enthält den Code für Ihr Programm (in diesem Fall Ihre Azure-Funktion) und Richtlinien für die Verwaltung von Abhängigkeiten.

Bei Verwendung einer `.fsx` für eine Funktion Azure häufig erforderlichen Assemblys werden automatisch für Sie Ihren Code anstelle "Textbausteine".

## <a name="binding-to-arguments"></a>Bindung an Argumenten

Jede Bindung unterstützt einige Argumente wie in [Azure Funktionen Trigger und Bindings-Entwicklerreferenz](functions-triggers-bindings.md). Beispielsweise ist eine Bindung Argument Blob Trigger unterstützt eine POCO, der einen F#-Datensatz mit ausgedrückt werden kann. Zum Beispiel:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Die F#-Funktion Azure wird ein oder mehrere Argumente. Wenn Azure Funktionen Argumente sprechen, meinen wir _Eingabeargumenten_ und _Ausgabeargumente_ . Eingabeargument ist genau wie es klingt: Eingabe Ihrer Azure-Funktion in F#. Ein _Ausgabe_ -Argument ist änderbare Daten oder `byref<>` Argument so übergeben wird Daten _aus_ Ihrer Funktion zurück.

Im obigen Beispiel `blob` ist ein Eingabeargument und `output` Ausgabe spricht. Hinweis verwendet `byref<>` für `output` (besteht keine Notwendigkeit zum Hinzufügen der `[<Out>]` Anmerkung). Mit einem `byref<>` ermöglicht die Funktion zum Ändern der Datensatz oder das Argument verweist auf Objekt.

F#-Datensatz wird als dem Eingabetyp muss Datensatzdefinition markiert mit `[<CLIMutable>]` um das Azure Funktionen Framework Felder entsprechend festlegen, bevor der Datensatz an die Funktion übergeben. Hinter den Kulissen `[<CLIMutable>]` Setter für die Eigenschaften generiert. Zum Beispiel:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Eine F#-Klasse kann auch für beide in- und out-Argumente verwendet werden. Für eine Klasse benötigen Eigenschaften normalerweise Getter und Setter. Zum Beispiel:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Protokollierung

Um die [streaming-Protokolle](../app-service-web/web-sites-streaming-logs-and-console.md) in F# Ausgabe anmelden, sollten Ihre Funktion ein Argument des Typs `TraceWriter`. Konsistenz, empfehlen wir für dieses Argument lautet `log`. Zum Beispiel:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Async

Die `async` Workflow verwendet werden, aber das Ergebnis zurückgeben muss ein `Task`. Dies kann mit `Async.StartAsTask`, beispielsweise:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Abbruchtoken

Die Funktion muss ordnungsgemäß herunterfahren behandeln, geben Sie es ein [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) Argument. Dies kann kombiniert mit `async`, beispielsweise:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Importieren von namespaces

Namespaces können auf die übliche Weise geöffnet werden:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Die folgenden Namespaces werden automatisch geöffnet:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Referenzieren von externen Assemblys

Ebenso Frameworkassembly Verweise hinzugefügt werden, mit der `#r "AssemblyName"` Richtlinie.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Die folgenden Assemblys werden automatisch durch die Hostingumgebung Azure Funktionen hinzugefügt:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Die folgenden Assemblys sind besondere Schreibweise und Simplename darauf verwiesen werden kann (z.B. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Benötigen Sie eine private Assembly verweisen, können Sie die Assemblydatei in Hochladen einer `bin` Ordner Ihre Funktion und Verweis mithilfe der Datei benennen (z.B.)  `#r "MyAssembly.dll"`). Informationen zum upload von Dateien in Ordner Funktion finden Sie im folgenden Abschnitt auf Paket.

## <a name="editor-prelude"></a>Editor-Einführung

Ein Editor, der F#-Compiler unterstützt nicht die Namespaces und Assemblys Azure automatisch enthält. So kann es hilfreich sein Auftakt enthalten, mit der die Assemblys finden Sie mit Editor und Namespaces explizit öffnen. Zum Beispiel:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Wenn Azure Funktionen Code ausgeführt wird, verarbeitet die Quelle mit `COMPILED` definiert, sodass der Editor Auftakt ignoriert.

## <a name="package-management"></a>Verwalten von Paketen

Um NuGet-Pakete in einer F#-Funktion zu verwenden, fügen eine `project.json` Datei in den Ordner im Dateisystem des app Funktion die Funktion. Beispiel `project.json` Datei, die ein NuGet-Paket auf fügt `Microsoft.ProjectOxford.Face` Version 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Nur.NET Framework 4.6 unterstützt, um sicherzustellen, dass Ihre `project.json` Datei `net46` wie folgt.

Beim Hochladen einer `project.json` Datei, die Common Language Runtime Ruft die Pakete ab und fügt automatisch Verweise auf die Assemblys Paket. Sie müssen hinzufügen `#r "AssemblyName"` Richtlinien. Fügen Sie einfach die `open` Aussagen auf Ihre `.fsx` Datei.

Sie können für die automatische verweisen auf Assemblys in der Einführung Editor Verbesserung Ihres Interaktion mit F# kompilieren.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Hinzufügen einer `project.json` Datei zu Ihrer Azure-Funktion

1. Zunächst sicherstellen Ihrer app Funktion wird ausgeführt, können Sie die Funktion in der Azure-Portal öffnen. Dadurch auch Zugang zu Streaming-Protokolle, wo Paket Ausgabe angezeigt werden.

2. Hochladen einer `project.json` Datei, verwenden Sie eine der Methoden aus der [Funktion app Dateien aktualisieren](functions-reference.md#fileupdate). Bei Verwendung von [Kontinuierlichen Bereitstellung von Azure-Funktionen](functions-continuous-deployment.md)können Sie einen `project.json` Datei staging Zweigstellen um experimentieren vor der Bereitstellung Verzweigung hinzufügen.

3. Nach dem `project.json` Datei hinzugefügt wird, wird der Ausgabe wie im folgenden Beispiel die Funktion des streaming-Protokoll:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Umgebungsvariablen

Verwenden Sie zum Abrufen einer Umgebungsvariablen oder eine Anwendung Wert `System.Environment.GetEnvironmentVariable`, beispielsweise:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Wiederverwenden von Code ".fsx"

Verwenden Sie Code aus anderen `.fsx` mithilfe einer `#load` Richtlinie. Zum Beispiel:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Pfade bietet die `#load` Richtlinie sind relativ zum Speicherort der `.fsx` Datei.

* `#load "logger.fsx"`Lädt eine Datei im Ordner "Funktion".

* `#load "package\logger.fsx"`Lädt eine Datei die `package` Ordner Funktion.

* `#load "..\shared\mylogger.fsx"`Lädt eine Datei die `shared` Ordner auf derselben Ebene wie die Funktion Ordner also direkt unter `wwwroot`.

Die `#load` Richtlinie funktioniert nur mit `.fsx` (F#) Skriptdateien nicht mit `.fs` Dateien.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in folgenden Ressourcen:

* [F#-Handbuch](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Azure Funktionen-Entwicklerreferenz](functions-reference.md)
* [Azure Funktionen C#-Entwicklerreferenz](functions-reference-csharp.md)
* [Azure Funktionen NodeJS-Entwicklerreferenz](functions-reference-node.md)
* [Azure Funktionen Trigger und Bindung](functions-triggers-bindings.md)
* [Azure Funktionen testen](functions-test-a-function.md)
* [Azure Funktionen skalieren](functions-scale.md)

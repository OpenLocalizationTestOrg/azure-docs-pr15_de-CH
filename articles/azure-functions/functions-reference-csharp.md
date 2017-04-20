<properties
    pageTitle="Azure Funktionen Entwicklerreferenz | Microsoft Azure"
    description="Azure-Funktionen mit C# zu verstehen."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktionen, Funktionen, Verarbeitung, Webhooks, dynamische Compute, serverlose Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure Funktionen C#-Entwicklerreferenz

> [AZURE.SELECTOR]
- [C#-Skript](../articles/azure-functions/functions-reference-csharp.md)
- [F#-Skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
C#-Erfahrung für Azure basiert auf Azure Webaufträge SDK. Daten fließen in die C#-Funktion über Methodenargumente. Argumentnamen gemäß `function.json`, und vordefinierte Namen für den Zugriff auf Elemente wie die Funktion Protokollierung und Abbruch Token.

Es wird vorausgesetzt, dass die [Entwicklerreferenz Azure Funktionen](functions-reference.md)bereits gelesen haben.

## <a name="how-csx-works"></a>Funktionsweise von csx

Die `.csx` Format weniger "Textbausteine" schreiben und nur eine C#-Funktion schreiben können. Azure-Funktionen, Sie alle Verweise auf Assemblys und Namespaces gehören oben müssen wie gewohnt einrichten und statt alles in einem Namespace und einer Klasse, definieren Sie nur die `Run` Methode. Möchten Sie beispielsweise alle Klassen gehören zum Definieren von POCO-Objekten kann eine Klasse in der gleichen Datei enthalten.

## <a name="binding-to-arguments"></a>Bindung an Argumenten

Verschiedene Bindungen sind verpflichtet, eine C#-Funktion über die `name` -Eigenschaft in der *function.json* -Konfiguration. Jede Bindung weist eigene unterstützten Typen pro Bindung dokumentiert; BLOB-Trigger kann beispielsweise eine Zeichenfolge, ein POCO oder andere Typen unterstützen. Sie können den Typ Bedarf am besten. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Protokollierung

Um die Streaming-Protokolle in C# Ausgabe anmelden gehören ein `TraceWriter` Argument eingegeben. Es wird empfohlen, Sie benennen `log`. Sie vermeiden sollten `Console.Write` in Azure-Funktionen.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Async

Verwenden, um eine asynchrone funktionieren die `async` Schlüsselwort und Rückgabe einer `Task` Objekt.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Abbruchtoken

In bestimmten Fällen müssen Sie Vorgänge die heruntergefahren. Während es immer am besten Programmieren der Absturz verarbeiten kann, soll bei ordnungsgemäßes Herunterfahren behandeln anfordert, Definieren einer [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) Argument eingegeben.  Ein `CancellationToken` bereitgestellt, wenn Host Herunterfahren ausgelöst wird. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Importieren von namespaces

Ggf. importieren Namespaces möglich so wie üblich mit dem `using` Klausel.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Die folgenden Namespaces werden automatisch importiert und sind optional:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Referenzieren von externen Assemblys

Fügen Sie Verweise für Framework-Assemblys mit der `#r "AssemblyName"` Richtlinie.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Benötigen Sie eine private Assembly verweisen, können Sie die Assemblydatei in Hochladen einer `bin` Ordner Ihre Funktion und Verweis mithilfe der Datei benennen (z.B.)  `#r "MyAssembly.dll"`). Informationen zum upload von Dateien in Ordner Funktion finden Sie im folgenden Abschnitt auf Paket.

## <a name="package-management"></a>Verwalten von Paketen

NuGet-Pakete in einer C#-Funktion verwenden, Hochladen einer *project.json* , die die Funktion Ordner im Dateisystem Funktion app. Hier ist eine *project.json* , die eine Microsoft.ProjectOxford.Face Version 1.1.0 hinzufügt:

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

Nur.NET Framework 4.6 unterstützt, um sicherzustellen, dass Ihre *project.json* Datei `net46` wie folgt.

Beim Hochladen einer Datei *project.json* die Common Language Runtime Ruft die Pakete ab und fügt automatisch Verweise auf die Assemblys Paket. Sie müssen hinzufügen `#r "AssemblyName"` Richtlinien. Fügen Sie einfach die `using` Aussagen zur Datei *run.csx* Typen in NuGet-Pakete verwenden.


### <a name="how-to-upload-a-projectjson-file"></a>Uploaden eine Datei project.json

1. Zunächst sicherstellen Ihrer app Funktion wird ausgeführt, können Sie die Funktion in der Azure-Portal öffnen. 

    Dadurch auch Zugang zu Streaming-Protokolle, wo Paket Ausgabe angezeigt werden. 

2. Verwenden Sie zum Hochladen einer Datei project.json im Abschnitt **Funktion app aktualisieren** [Azure Funktionen Entwickler Thema](functions-reference.md#fileupdate)beschriebenen Methoden. 

3. Nach dem Hochladen der Datei *project.json* angezeigt wie im folgenden Beispiel die Funktion Log streaming ist:

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

Verwenden Sie zum Abrufen einer Umgebungsvariablen oder eine Anwendung Wert `System.Environment.GetEnvironmentVariable`, wie im folgenden Beispiel gezeigt:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Wiederverwenden von Code csx

Sie können Klassen und Methoden in anderen *CSx* Dateien in der Datei *run.csx* definiert. Verwenden Sie hierzu `#load` Direktiven in der Datei *run.csx* wie im folgenden Beispiel gezeigt.

Beispiel *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Beispiel *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Sie können einen relativen Pfad mit dem `#load` Richtlinie:

* `#load "mylogger.csx"`Lädt eine Datei im Ordner "Funktion".

* `#load "loadedfiles\mylogger.csx"`Lädt eine Datei im Ordner der Funktion.

* `#load "..\shared\mylogger.csx"`Lädt eine Datei in einem Ordner auf der gleichen Ebene wie der Funktion Ordner also unter *Wwwroot*.
 
Die `#load` Richtlinie funktioniert nur mit Dateien *CSx* (C#-Skript) und nicht mit *cs* -Dateien. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in folgenden Ressourcen:

* [Azure Funktionen-Entwicklerreferenz](functions-reference.md)
* [Azure Funktionen F#-Entwicklerreferenz](functions-reference-fsharp.md)
* [Azure Funktionen NodeJS-Entwicklerreferenz](functions-reference-node.md)
* [Azure Funktionen Trigger und Bindung](functions-triggers-bindings.md)


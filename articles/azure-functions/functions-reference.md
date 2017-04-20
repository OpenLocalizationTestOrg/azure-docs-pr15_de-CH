<properties
    pageTitle="Azure Funktionen Entwicklerreferenz | Microsoft Azure"
    description="Kennen Sie Azure Funktionen Konzepte und Komponenten, die für alle Sprachen und Bindung."
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
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure Funktionen-Entwicklerreferenz

Azure Funktionen nutzen einige technische Grundbegriffe und Komponenten unabhängig von der Sprache oder Bindung verwendeten. Bevor Sie springen in Informationen für eine bestimmte Sprache oder Bindung, müssen Sie diese Übersicht lesen, die für alle gilt.

Es wird vorausgesetzt, dass [Übersicht über Azure Funktionen](functions-overview.md) bereits gelesen haben und mit [Konzepten wie Trigger, Bindung und JobHost Runtime Webaufträge SDK](../app-service-web/websites-dotnet-webjobs-sdk.md)vertraut sind. Azure Funktionen basiert auf dem WebJobs-SDK. 


## <a name="function-code"></a>Funktionscode

Eine *Funktion* ist das primäre Konzept in Azure-Funktionen. Sie schreiben Code für eine Funktion in einer Sprache Ihrer Wahl und Code-Dateien und eine Konfigurationsdatei im selben Ordner speichern. Konfiguration in JSON ist und die Datei mit dem Namen `function.json`. Eine Vielzahl von Sprachen unterstützt, und jeder bietet eine geringfügig optimiert am besten für die jeweilige Sprache. 

Die `function.json` Konfiguration spezifisch für eine Funktion mit der Bindung enthält. Die Laufzeit liest diese Datei Ereignisse auslösen von, welche Daten beim Aufruf der Funktion und wo Daten umfassen die Funktion selbst übergeben. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Sie können verhindern, dass die Common Language Runtime mit der Funktion durch Festlegen der `disabled` -Eigenschaft auf `true`.

Die `bindings` -Eigenschaft wird in dem Trigger und Bindungen konfigurieren. Jede Bindung teilt einige allgemeine und einige Einstellungen für eine bestimmte Art von Bindung. Jede Bindung erfordert Folgendes:

|Eigenschaft|Werte-Typen|Kommentare|
|---|-----|------|
|`type`|Zeichenfolge|Bindungstyp. Z. B. `queueTrigger`.
|`direction`|'in', 'out'| Gibt an, ob die Bindung für die Daten in die Funktion Daten empfängt oder sendet der Funktion.
| `name` | Zeichenfolge | Der Name für die gebundenen Daten in der Funktion verwendet wird. Für C# wird dies ein Argumentname sein. JavaScript werden sie den Schlüssel im Schlüssel-Wert-Liste.

## <a name="function-app"></a>Funktion app

Eine Funktion app besteht eine oder mehrere einzelne Funktionen, die zusammen von Azure App Service verwaltet werden. Alle Funktionen in eine Funktion app freigeben Planes Preisgestaltung, kontinuierliche Bereitstellung und Laufzeitversion. In mehreren Sprachen geschriebene Funktionen können alle dieselbe Funktion app freigeben. Vorstellen der Funktion app so organisieren und gemeinsam Ihre Funktionen verwalten 

## <a name="runtime-script-host-and-web-host"></a>Laufzeit (Skripthost und Web-Host)

Die Common Language Runtime oder Script Host ist der zugrunde liegende WebJobs SDK Host überwacht Ereignisse sammelt und sendet Daten und letztlich führt den Code. 

Um HTTP-Trigger zu erleichtern, ist auch ein Webhost vor dem Skripthost Produktionsszenarien vorgesehen ist. Dadurch um den Script Host verwaltet durch den Webhost front-End-Datenverkehr zu isolieren.

## <a name="folder-structure"></a>Ordnerstruktur

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Wenn ein Projekt für die Bereitstellung von Funktionen zu einer Funktion app in Azure App Service einrichten, können Sie diese Struktur als Website Code behandeln. Vorhandene Tools wie fortlaufende Integration und Bereitstellung verwenden oder benutzerdefinierte Bereitstellung Skripts dafür Zeit Paketinstallation bereitstellen oder code Transpilation.

>[AZURE.NOTE] Die `wwwroot` Ordner hier ist, Ihre Dateien zu bereitgestellt werden. Sie darf keine Ordner enthalten, in die Dateien bereitstellen, die Ende mit `wwwroot\wwwroot`. Stattdessen die `host.json` Ordner-Datei und Funktion sollte direkt am Stamm Sie bereitstellen.

## <a id="fileupdate"></a>Funktion app Dateien aktualisieren

Azure-Portal integrierte Funktions-Editor können Sie die Datei *function.json* und die Codedatei für eine Funktion aktualisieren. Hochladen oder andere *package.json* , *project.json* oder Abhängigkeiten aktualisieren, müssen Sie Bereitstellungsmethoden verwenden.

Funktion apps basieren auf App Service [Bereitstellungsoptionen für standard-Web-apps](../app-service-web/web-sites-deploy.md) für Funktion apps verfügbar sind. Hier sind einige Methoden, uploaden oder Funktion app Dateien aktualisieren. 

#### <a name="to-use-app-service-editor"></a>App Service-Editors

1. Klicken Sie im Portal Azure Funktionen auf **Funktion app**.

2. Klicken Sie im Bereich **Erweiterte Einstellungen** **App Service Einstellungen**.

3. Klicken Sie unter **ENTWICKLUNGSTOOLS**Anw.-Menü Nav **App Service-Editor** .

4.  Klicken Sie auf **OK**.

    Nach dem Laden App Service-Editor sehen Sie *host.json* Datei und Funktion Ordner *Wwwroot*. 

5. Öffnen Sie Dateien bearbeiten, oder Drag & drop aus dem Entwicklungscomputer Dateien hochladen.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Mit der Funktion Anwendung SCM (Kudu) Endpunkt

1. Navigieren Sie zu: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klicken Sie auf **Debuggen Konsole > CMD**.

3. Navigieren Sie zu `D:\home\site\wwwroot\` *host.json* aktualisieren oder `D:\home\site\wwwroot\<function_name>` eine Funktion Dateien aktualisieren.

4. Drag-and-Drop eine Datei, die Sie in den entsprechenden Ordner im Feld Datei hochladen möchten. Es gibt zwei Bereiche im Raster Datei Datei zugelassene. *ZIP-* Dateien angezeigt wird ein Feld mit der Bezeichnung "Ziehen hier hochladen und entpacken." Legen Sie für andere Dateitypen im Raster Datei jedoch außerhalb der "extrahieren".

#### <a name="to-use-ftp"></a>Mit FTP

1. Gehen Sie [hier](../app-service-web/web-sites-deploy.md#ftp) zu FTP konfiguriert.

2. Wenn Sie die Funktion app-Site verbunden sind, kopieren Sie eine aktualisierte *host.json* -Datei `/site/wwwroot` oder Funktion Dateien auf `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Kontinuierliche Bereitstellung verwenden

Führen Sie die Schritte im Thema [kontinuierliche Bereitstellung für Azure](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Parallele Ausführung

Wenn mehrere auslösende schneller Ereignisse als eine Funktion Singlethread-Laufzeit verarbeitet werden kann, kann die Laufzeit die Funktion mehrmals parallel aufrufen.  Funktion app [Dynamische Service-Plan](functions-scale.md#dynamic-service-plan)verwendet, kann die Funktion app automatisch skalieren.  Jede Instanz der app Funktion, ob die Anwendung auf dynamische Service Plan oder eine reguläre, [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)ausgeführt wird möglicherweise gleichzeitige Funktionsaufrufe mithilfe mehrerer Threads parallel verarbeiten.  Maximal gleichzeitige Funktionsaufrufe jeweils app Funktion hängt die Speichergröße Funktion App. 

## <a name="azure-functions-pulse"></a>Azure Funktionen Impuls  

Pulse ist ein live-Ereignis-Stream zeigt, wie oft die Funktion ausgeführt wird, sowie Erfolge und Fehler. Sie können auch die durchschnittliche Ausführungszeit überwachen. Wir werden weitere Features und Anpassung, mit der Zeit hinzufügen. Sie können die Registerkarte **Überwachen** **Pulse** -Seite zugreifen.

## <a name="repositories"></a>Repositories

Der Code für Azure Quelle öffnen und in GitHub Repositories gespeichert:

* [Azure Funktionen runtime](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Funktionen portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Funktionen Vorlagen](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure Webaufträge SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure Webaufträge SDK Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindung

Hier ist eine Tabelle mit allen unterstützten Bindings.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Probleme melden

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in folgenden Ressourcen:

* [Azure Funktionen C#-Entwicklerreferenz](functions-reference-csharp.md)
* [Azure Funktionen F#-Entwicklerreferenz](functions-reference-fsharp.md)
* [Azure Funktionen NodeJS-Entwicklerreferenz](functions-reference-node.md)
* [Azure Funktionen Trigger und Bindung](functions-triggers-bindings.md)
* [Azure Funktionen: die Reise](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) Azure App Service-Teamblog. Geschichte der Azure-Funktionen wie entwickelt wurde.






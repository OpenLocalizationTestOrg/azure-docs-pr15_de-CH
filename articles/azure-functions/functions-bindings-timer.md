<properties
    pageTitle="Azure Funktionen Timer Trigger | Microsoft Azure"
    description="Verstehen Sie, wie Timer Trigger in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktionen, Funktionen, Verarbeitung von Ereignissen, dynamische Compute serverlose Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure Funktionen Timer trigger

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Konfigurieren von Timer Trigger in Azure Funktionen erläutert. Timer Trigger Anruffunktionen basierend auf einem Zeitplan einmal oder wiederholt.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON für Timer trigger

Die *function.json* -Datei stellt einen Zeitplan Ausdruck. Der folgende Zeitplan läuft beispielsweise die Funktion pro Minute:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Timer Trigger verarbeitet automatisch mit mehreren Instanzen skalieren: nur eine Instanz einer bestimmten Timer-Funktion für alle Instanzen ausgeführt.

## <a name="format-of-schedule-expression"></a>Format des Zeitplans Ausdruck

Zeitplan-Ausdruck ist ein [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression) , der 6 Felder enthält: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Beachten Sie, dass viele Cron-Ausdruck finden Sie online im {zweite} Feld weglassen, so aus der kopieren Sie das zusätzliche Feld anpassen müssen. 

Beispiele anderer Zeitplan Ausdruck:

Alle 5 Minuten ausgelöst:

```json
"schedule": "0 */5 * * * *"
```

Einmal am Anfang jeder Stunde ausgelöst:

```json
"schedule": "0 0 * * * *",
```

Alle zwei Stunden ausgelöst:

```json
"schedule": "0 0 */2 * * *",
```

Stündlich von 9 bis 17 Uhr ausgelöst:

```json
"schedule": "0 0 9-17 * * *",
```

Täglich um 9:30 Uhr ausgelöst:

```json
"schedule": "0 30 9 * * *",
```

Jeden Wochentag um 9:30 Uhr ausgelöst:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Timer Trigger C#-Codebeispiel

In diesem C#-Codebeispiel schreibt ein Protokoll jedes Mal die Funktion ausgelöst wird.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 

<properties
   pageTitle="Logik app Szenario: erstellen einen Azure Funktionen Service Bus-Trigger | Microsoft Azure"
   description="Verwenden Sie Funktionen Azure Service Bus-Trigger für eine Logik-app erstellen"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Logik app Szenario: Azure Service Bus-Trigger mithilfe von Azure Funktionen erstellen

Azure-Funktionen können Sie einen Trigger für eine Anwendung Logik erstellen, wenn Sie ein langer Listener oder Aufgabe bereitstellen müssen. Sie können beispielsweise eine Funktion erstellen, die eine Warteschlange abhören und eine Logik der Anwendung als einen Push-Auslöser sofort ausgelöst.

## <a name="build-the-logic-app"></a>Erstellen der Anwendung Logik

In diesem Beispiel haben Sie eine Funktion für jede Anwendung Logik, die ausgelöst werden. Erstellen Sie zunächst eine Logik-app, die einen HTTP-Anforderung Trigger aufweist. Die Funktion ruft diesen Endpunkt bei jeder eine Warteschlange Nachricht.  

1. Erstellen Sie eine neue Anwendung Logik. Wählen Sie den Trigger **manuell – Wenn eine HTTP-Anforderung empfangen wird** .  
   Optional können Sie ein JSON-Schema mit der Warteschlange mithilfe eines Tools wie [jsonschema.net](http://jsonschema.net). Fügen Sie das Schema im Trigger. Dadurch wird des Designers die Form der Daten zu verstehen und die mehr einfach Eigenschaften durch den Workflow.
1. Fügen Sie keine zusätzlichen Schritte, die auftreten, wenn eine Warteschlange Nachricht. Senden Sie z. B. eine e-Mail über Office 365.  
1. Speichern Sie Logik app Callback-URL für den Trigger Logik App generiert. Die URL wird auf den Trigger.

![Der Rückruf wird URL auf der trigger][1]

## <a name="build-the-function"></a>Erstellen der Funktion

Als Nächstes müssen Sie eine Funktion erstellen, die als Trigger fungieren und die Warteschlange anhören.

1. [Funktionen der Azure-Portal](https://functions.azure.com/signin) **Neue**Funktion und wählen Sie die **ServiceBusQueueTrigger - C#** -Vorlage.

    ![Azure Funktionen portal][2]

2. Konfigurieren Sie die Verbindung zur Warteschlange Service Bus (verwendet die Azure Service Bus SDK `OnMessageReceive()` Listener).
3. Schreiben Sie eine einfache Funktion rufen Logik app Endpunkt (zuvor erstellt), mithilfe der warteschlangennachricht als Trigger. Hier ist ein vollständiges Beispiel für eine Funktion. Im Beispiel wird mit einem `application/json` Inhaltstyp angezeigt, aber Sie können diese bei Bedarf ändern.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Zum Testen fügen Sie eine warteschlangennachricht über ein Tool wie [Service Bus-Explorer hinzu](https://github.com/paolosalvatori/ServiceBusExplorer). Finden Sie die Logik app ausgelöst, sobald die Funktion die Nachricht empfängt.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG

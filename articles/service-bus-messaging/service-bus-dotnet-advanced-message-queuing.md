<properties 
    pageTitle="Verwendung von AMQP 1.0 mit .NET Service Bus-API | Microsoft Azure" 
    description="Erfahren Sie, wie erweiterte Message Queuing-Protodol (AMQP) 1.0 mit Azure .NET Service Bus-API verwendet." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Verwendung von AMQP 1.0 mit Service Bus .NET API

Die erweiterte Message Queuing Protocol (AMQP) 1.0 ist eine effiziente, zuverlässige und auf niedriger Ebene Messagingprotokoll, mit denen Sie stabile und plattformübergreifende Messaginganwendungen.

Unterstützung für AMQP 1.0 Service Bus bedeutet, dass Sie Warteschlangen und Veröffentlichen/Abonnieren von vermittelten Messagingfunktionen zwischen Plattformen mit effizienten binären Protokolls. Darüber hinaus können Sie Applikationen umfasst Komponenten durch eine Mischung von Sprachen, Frameworks und Betriebssystemen erstellen.

Erläutert, wie der Service Bus vermittelte Messaging-Features (Warteschlangen und Themen Veröffentlichen/Abonnieren) aus der Bus .NET API mit. Gibt einen [anderen Artikel](service-bus-java-how-to-use-jms-api-amqp.md) die Verwendung der gleichen mithilfe der standardmäßigen Java Message Service (JMS)-API. Diese beiden Führungslinien können zusammen Sie lernen, plattformübergreifende Nachrichtenübermittlung mit AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Erste Schritte mit Service Bus

Dieser Artikel setzt voraus, dass Sie bereits einen Service Bus-Namespace enthält eine Warteschlange mit dem Namen "Warteschlange1" Wenn nicht, können Sie den Namespace und die Warteschlange der [Azure-Portal][]erstellen. Weitere Informationen zu Service Bus Namespaces und Warteschlangen finden Sie unter [Erste Schritte mit Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal).

## <a name="download-the-service-bus-sdk"></a>Servicebus SDK herunterladen

AMQP 1.0-Unterstützung ist in Service Bus SDK, Version 2.1 oder höher verfügbar. Sie können die neuesten Bits von NuGet zur [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/).

## <a name="code-net-applications"></a>Code .NET applications

Standardmäßig kommuniziert die Clientbibliothek Service Bus .NET Service Bus-Dienst mit einem dedizierten SOAP-Protokoll. AMQP 1.0 anstelle der Verwendung erfordert Protokoll expliziten Konfiguration auf Service Bus Verbindungszeichenfolge wie im nächsten Abschnitt beschrieben. Als diese Änderung unverändert Anwendungscode grundsätzlich bei AMQP 1.0 verwenden.

In der aktuellen Version sind einige API-Funktionen, die nicht unterstützt werden, wenn Sie AMQP verwenden. Diese nicht unterstützten Funktionen sind im Abschnitt [nicht unterstützte Features und Einschränkungen](#unsupported-features-and-restrictions)aufgeführt. Einige erweiterte Konfigurationen haben eine andere Bedeutung, wenn Sie AMQP verwenden. Keine Optionen in diesem Artikel verwendet werden, aber Details finden in der [Übersicht über die Service Bus AMQP](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Konfigurieren Sie über App.config

Es wird empfohlen, Applikationen Konfigurationsdatei App.config Einstellungen speichern verwenden. Für Service Bus Applikationen können App.config Service Bus **ConnectionString**speichern. Diese Anwendung verwendet auch App.config speichern den Namen des Service Bus messaging Entität verwendet.

Ein Beispiel-App ist hier dargestellt:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>Konfigurieren Sie die Verbindungszeichenfolge Service Bus

Der Wert der **Microsoft.ServiceBus.ConnectionString** -Einstellung ist Service Bus-Verbindungszeichenfolge, mit der die Verbindung zum Service Bus. Das Format lautet wie folgt:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Wo `[namespace]` und `[SAS key]` stammen aus dem [Azure-Portal][]. Weitere Informationen finden Sie unter [Servicebuswarteschlangen verwenden] [].

Bei Verwendung von AMQP mit die Verbindungszeichenfolge angefügt `;TransportType=Amqp`, weist die Clientbibliothek Service Bus Verbindung zu AMQP 1.0 verwenden.

### <a name="configure-the-entity-name"></a>Konfigurieren der Entitätsname

Diese Anwendung verwendet die `EntityName` , den Namen der Warteschlange konfigurieren mit der Anwendung Nachrichten austauscht im **AppSettings** -Abschnitt der Datei App.config festlegen.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Einfache .NET Anwendung Service Bus-Warteschlange

Im folgende Beispiel sendet und empfängt Nachrichten aus einer Warteschlange Service Bus.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Führen Sie die Anwendung

Ausführen der Anwendung die Ausgabe des Formulars:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Plattformübergreifende Nachrichtenübermittlung JMS und .NET

In diesem Thema gezeigt wie Service Bus mit .NET Nachrichten senden und Empfangen von Nachrichten mithilfe von .NET. Jedoch ist einer der wichtigsten Vorteile von AMQP 1.0 Anwendung aus Komponenten mit zuverlässig und originalgetreue ausgetauschten Nachrichten in verschiedenen Sprachen geschrieben werden können.

Die oben beschriebene Beispiel .NET Anwendung mit einer ähnlichen Begleithandbuch [wie Java Message Service (JMS) API Service Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)entnommen Java-Anwendung kann die zum Austausch von Nachrichten zwischen .NET und Java. 

Weitere Informationen zu den Details der plattformübergreifende Nachrichtenübermittlung Service Bus mit AMQP 1.0 [Service Bus AMQP 1.0 Übersicht](service-bus-amqp-overview.md)anzeigen

### <a name="jms-to-net"></a>JMS zu .NET

JMS .NET messaging zeigen:

* Starten der Anwendung .NET Beispiel ohne Befehlszeilenargumente.
* Starten Sie die Java-beispielanwendung mit dem Befehlszeilenargument "Sendonly". In diesem Modus empfängt die Anwendung keine Nachrichten aus der Warteschlange nur sendet.
* Drücken Sie die **EINGABETASTE** mehrmals in der Java-Anwendung-Konsole gesendeten Nachrichten führen.
* Diese Nachrichten werden von der Anwendung .NET.

### <a name="output-from-jms-application"></a>Ausgabe von JMS-Anwendung

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Ausgabe aus

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET JMS

.NET JMS-messaging zeigen:

* Starten Sie Application Sample .NET mit dem Befehlszeilenargument "Sendonly". In diesem Modus empfängt die Anwendung keine Nachrichten aus der Warteschlange nur sendet.
* Starten Sie die Java-beispielanwendung ohne Befehlszeilenargumente.
* Drücken Sie die **EINGABETASTE** mehrmals in der .NET Anwendung Konsole gesendeten Nachrichten führen.
* Diese Nachrichten werden durch die Java-Anwendung.

#### <a name="output-from-net-application"></a>Ausgabe aus

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Ausgabe von JMS-Anwendung

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Nicht unterstützte Features und Einschränkungen

Die folgenden Features von .NET Service Bus-API werden derzeit nicht unterstützt, wenn Sie AMQP verwenden:

 * Transaktionen
 * Übertragungsziel per

Weitere Informationen finden Sie unter [nicht unterstützte Features und Einschränkungen Verhaltensunterschiede](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel zeigte auf Service Bus vermittelte Messagingfunktionen (Warteschlangen und Themen Veröffentlichen/Abonnieren) von .NET AMQP 1.0 und Service Bus .NET API.

Sie können auch Service Bus AMQP 1.0 aus anderen Sprachen wie Java, C#, Python und PHP. Komponenten in diesen Sprachen können Nachrichten zuverlässig und Service Bus bei AMQP 1.0 originalgetreue austauschen. Weitere Informationen finden Sie unter [Übersicht über Service Bus AMQP](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Nächste Schritte

Damit eine Übersicht über Service Bus und AMQP mit .NET gelesen haben, finden Sie unter folgenden Links für Weitere Informationen.

* [AMQP 1.0-Unterstützung in Azure Service Bus](service-bus-amqp-overview.md)
* [Verwendung von Java Message Service (JMS) API mit Service Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Wie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
 
[Azure-portal]: https://portal.azure.com

<properties 
    pageTitle="AMQP 1.0 Dienstbus Java API verwenden | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Java Message Service (JMS) Azure Service Bus mit Advanced Message Queuing"
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"  
    manager="timlt" 
    editor="" />

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Verwendung von Java Message Service (JMS) API mit Service Bus und AMQP 1.0

Die erweiterte Message Queuing Protocol (AMQP) 1.0 ist eine effiziente, zuverlässige und auf niedriger Ebene Messagingprotokoll, mit denen Sie stabile und plattformübergreifende Messaginganwendungen. AMQP 1.0-Unterstützung wurde im Oktober 2012 Azure Service Bus hinzugefügt und auf allgemeine Verfügbarkeit im Mai 2013 gewechselt.

Das Hinzufügen von AMQP 1.0 bedeutet, dass jetzt kann Warteschlangen und Veröffentlichen/Abonnieren von vermittelten Messagingfunktionen Service Bus zwischen Plattformen mit effizienten binären Protokolls. Darüber hinaus können Sie Applikationen umfasst Komponenten durch eine Mischung von Sprachen, Frameworks und Betriebssystemen erstellen.

Diese Anleitung erläutert, wie Service Bus vermittelte Messagingfunktionen (Warteschlangen und Themen Veröffentlichen/Abonnieren) Java-Anwendung mit dem beliebten Java Message Service (JMS) API-Standard.

## <a name="get-started-with-service-bus"></a>Erste Schritte mit Service Bus

Dieses Handbuch setzt voraus, dass Sie bereits einen Service Bus-Namespace enthält eine Warteschlange mit dem Namen **queue1**. Wenn nicht, können Sie mithilfe der [Azure-Portal](https://portal.azure.com) [den Namespace und die Warteschlange erstellen](service-bus-create-namespace-portal.md) . Weitere Informationen zu Service Bus Namespaces und Warteschlangen finden Sie unter [Service Bus-Warteschlangen verwenden](service-bus-dotnet-get-started-with-queues.md).

### <a name="downloading-the-amqp-10-jms-client-library"></a>AMQP 1.0 JMS-Clientbibliothek herunterladen

Informationen über das Herunterladen der Apache Qpid JMS AMQP 1.0-Clientbibliothek finden Sie auf [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Sie müssen die folgenden vier JAR-Dateien aus dem Archiv Apache Qpid JMS AMQP 1.0 Java-CLASSPATH hinzufügen beim Erstellen und Ausführen von JMS-Applikationen mit Service Bus:

*    Geronimo Jms\_1.1\_Spec 1.0.jar
*    qpid-amqp-1-0-Client-[Version].jar
*    qpid-amqp-1-0-Client-JMS-[Version].jar
*    qpid-amqp-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Codierung Java-Anwendung

### <a name="java-naming-and-directory-interface-jndi"></a>Java Naming and Directory Interface (JNDI)

JMS verwendet Java Naming und Directory Interface (JNDI) eine Trennung zwischen logischen und physischen Namen zu erstellen. Zwei Arten von JMS-Objekten mit JNDI aufgelöst werden: ConnectionFactory und Ziel. JNDI verwendet ein Anbietermodell, in den anderen Verzeichnisdiensten Name Resolution Aufgaben behandeln anschließen können. Formatieren Sie Apache Qpid JMS AMQP 1.0 Library enthält eine einfache Eigenschaften dateibasierte JNDI-Provider, die mit einer Eigenschaftsdatei Folgendes konfiguriert:

```
# servicebus.properties – sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[username]:[password]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Konfigurieren der ConnectionFactory

Der Eintrag **ConnectionFactory** in der Qpid Eigenschaften JNDI Ereignisanbieter definiert hat Folgendes Format:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Wobei **[Jndi_name]** und **[ConnectionURL]** folgende Bedeutung haben:

- **[Jndi_name]**: der logische Name der ConnectionFactory. Dies ist der Name, der in der Java-Anwendung Methode JNDI-IntialContext.lookup() aufgelöst werden.
- **[ConnectionURL]**: ein URL für die JMS-Bibliothek benötigt AMQP Broker.

Das Format des **ConnectionURL** lautet wie folgt:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Wobei **[Namespace]**und **[Benutzername]** **[Kennwort]** folgende Bedeutung haben:

- **[Namespace]**: der Service Bus-Namespace.
- **[Benutzername]**: Ausstellername der Service-Bus.
- **[Kennwort]**: URL-kodierte Form des Schlüssels Aussteller Service Bus.

> [AZURE.NOTE] Sie müssen URL-Kodierung das Kennwort manuell. Ein nützliches Dienstprogramm URL-Codierung ist unter [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp)verfügbar.

#### <a name="configure-destinations"></a>Ziele konfigurieren

Ein Ziel in der Qpid Eigenschaften JNDI Ereignisanbieter definiert Eintrag hat Folgendes Format:

```
queue.[jndi_name] = [physical_name]
```
oder

```
topic.[jndi_name] = [physical_name]
```

Wo **[Jndi\_Name]** und **[physischen\_Name]** haben folgende Bedeutung:

- **[Jndi_name]**: der logische Name des Ziels. Dies ist der Name, der in der Java-Anwendung Methode JNDI-IntialContext.lookup() aufgelöst werden.
- **[Physical_name]**: der Name der Entität Service Bus, die Anwendung sendet oder empfängt Nachrichten.

> [AZURE.NOTE] Beim Empfangen von einem Service Bus Thema Abonnement entsprechen der physische Namen JNDI gemäß den Namen des Themas. Der Abonnementname wird das permanente Abonnement im Anwendungscode JMS erstellt wird bereitgestellt. [Service Bus AMQP 1.0-Entwicklerhandbuch](service-bus-amqp-dotnet.md) enthält weitere Informationen zum Arbeiten mit Service Bus Thema Abonnements von JMS.

### <a name="write-the-jms-application"></a>JMS-Anwendung

Es sind keine speziellen APIs oder erforderlichen Service Bus JMS mit Optionen. Es gibt jedoch einige Einschränkungen, die weiter unten behandelt werden. Ist eine JMS-Anwendung zuerst erforderliche Konfiguration der Umgebung JNDI **ConnectionFactory** und Ziele zu.

#### <a name="configure-the-jndi-initialcontext"></a>JNDI-InitialContext konfigurieren

JNDI-Umgebung sieht eine Hashtabelle Konfigurationsinformationen an den Konstruktor der Klasse javax.naming.InitialContext übergeben. Die beiden erforderlichen Elemente in der Hashtabelle den Klassennamen des ersten Kontextfactory und die Provider-URL. Im folgenden Codebeispiel zeigt die JNDI-Umgebung Qpid Eigenschaften Datei konfigurieren JNDI-Anbieter mit einer Eigenschaftsdatei namens **servicebus.properties**.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Einfache JMS-Anwendung eine Service Bus-Warteschlange

Das folgende Beispielprogramm JMS-Textnachrichten in eine Warteschlange Service Bus mit den logischen Namen JNDI Warteschlange sendet und empfängt Nachrichten zurück.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Lookup ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Führen Sie die Anwendung

Die Anwendung erzeugt die folgende Ausgabe:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Plattformübergreifende Nachrichtenübermittlung JMS und .NET

Dieses Handbuch zeigte, wie senden und Empfangen von Nachrichten über Service Bus mit JMS. Jedoch ist einer der wichtigsten Vorteile von AMQP 1.0 Anwendung aus Komponenten mit zuverlässig und originalgetreue ausgetauschten Nachrichten in verschiedenen Sprachen geschrieben werden können.

Das oben beschriebene Beispiel JMS-Anwendung mit einer ähnlichen .NET Anwendung entnommen Begleithandbuch [zum AMQP 1.0 mit .NET Service Bus .NET API verwenden](service-bus-dotnet-advanced-message-queuing.md), können Sie Nachrichten zwischen .NET und Java austauschen. 

Weitere Informationen zu den Details der plattformübergreifende Nachrichtenübermittlung Service Bus mit AMQP 1.0 finden Sie unter [Service Bus AMQP 1.0-Entwicklerhandbuch](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS zu .NET

JMS .NET messaging zeigen:

* Starten der Anwendung .NET Beispiel ohne Befehlszeilenargumente.
* Starten Sie die Java-beispielanwendung mit dem Befehlszeilenargument "Sendonly". In diesem Modus empfängt die Anwendung keine Nachrichten aus der Warteschlange nur sendet.
* Drücken Sie die **EINGABETASTE** mehrmals in der Java-Anwendung-Konsole gesendeten Nachrichten führen.
* Diese Nachrichten werden von der Anwendung .NET.

#### <a name="output-from-jms-application"></a>Ausgabe von JMS-Anwendung

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Ausgabe aus

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET JMS

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

Die folgenden Einschränkungen vorhanden Wenn JMS über AMQP 1.0 Service Bus, nämlich:

* Pro **Sitzung**kann nur eine **MessageProducer** oder **MessageConsumer** . Benötigen Sie mehrere **MessageProducer** oder **MessageConsumer** in einer Anwendung erstellen, Erstellen einer dedizierten **Sitzung** jeweils.
* Flüchtige Thema Abonnements werden derzeit nicht unterstützt.
* **MessageSelectors** werden nicht unterstützt.
* Temporäre Ziele, z.B. **TemporaryQueue**, **TemporaryTopic** sind derzeit nicht unterstützt und die **QueueRequestor** **TopicRequestor** APIs, die sie verwenden.
* Durchgeführte Sessions und verteilte Transaktionen werden nicht unterstützt.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel zeigte, wie Service Bus messaging-Funktionen (Warteschlangen und Themen Veröffentlichen/Abonnieren) von Java mit gängigen JMS-API und AMQP 1.0.

Sie können auch Service Bus AMQP 1.0 aus anderen Sprachen, einschließlich .NET, C#, Python und PHP. Komponenten mit diesen Sprachen können Nachrichten zuverlässig und Service Bus originalgetreue mit AMQP 1.0 unterstützen. Weitere Informationen finden Sie unter [Service Bus AMQP 1.0-Entwicklerhandbuch](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Nächste Schritte

* [AMQP 1.0-Unterstützung in Azure Service Bus](service-bus-amqp-overview.md)
* [Verwendung von AMQP 1.0 mit Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)
* [Das Entwicklerhandbuch von Service Bus AMQP 1.0](service-bus-amqp-dotnet.md)
* [Wie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
 

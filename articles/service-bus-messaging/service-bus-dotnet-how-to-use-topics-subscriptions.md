<properties
    pageTitle="Verwenden Sie Service Bus-Themen mit .NET | Microsoft Azure"
    description="Erfahren Sie, wie .NET in Azure Service Bus Topics und Abonnements mit. Codebeispiele sind für .NET Applications geschrieben."
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
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Zum Service Bus Topics und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieser Artikel beschreibt, wie Service Bus-Themen und Abonnements. Die Beispiele sind in C# geschrieben und .NET APIs verwenden. Die Szenarios können Themen und Abonnements Abonnementfilter erstellen, Senden von Nachrichten an ein Thema, ein Abonnement Nachrichten erhalten und Themen und Abonnements werden gelöscht. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Weiter](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Konfigurieren Sie die Anwendung mit Service Bus

Wenn Sie eine Anwendung erstellen, die Service Bus verwendet, Sie fügen einen Verweis auf die Assembly Service Bus und fügen Sie die entsprechenden Namespaces. Die einfachste Möglichkeit hierzu ist das entsprechende [NuGet](https://www.nuget.org) -Paket herunterladen.

## <a name="get-the-service-bus-nuget-package"></a>Erhalten Sie Service Bus NuGet-Paket

[Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) ist am einfachsten zu Service Bus-API und die Anwendung mit allen erforderlichen Service Bus Abhängigkeiten konfigurieren. Gehen Sie wie folgt vor, um Service Bus NuGet-Paket im Projekt zu installieren:

1.  Im Projektmappen-Explorer mit der rechten Maustaste **Verweise**und dann auf **NuGet-Pakete verwalten**.
2.  "Service Bus" Suchen Sie, und wählen Sie **Microsoft Azure Service Bus** . Klicken Sie auf **Installieren** , um die Installation abzuschließen und schließen Sie das Dialogfeld zu:

    ![][7]

Sie können nun Code für Service Bus schreiben.

## <a name="create-a-service-bus-connection-string"></a>Erstellen einer Verbindungszeichenfolge Service Bus

Service Bus verwendet eine Verbindungszeichenfolge Endpunkte und Anmeldeinformationen zu speichern. Sie stellen die Verbindungszeichenfolge in einer Konfigurationsdatei statt programmieren sie:

- Bei Azure Services sollten Sie die Verbindungszeichenfolge mithilfe der Azure Service Configuration (.csdef und .cscfg-Dateien) speichern.
- Bei Azure Websites oder Azure Virtual Machines empfohlen speichern die Verbindungszeichenfolge mithilfe der .NET Konfiguration (z. B. die Datei Web.config).

In beiden Fällen können Sie abrufen, die Verbindung mit der `CloudConfigurationManager.GetSetting` -Methode, wie weiter unten in diesem Artikel.

### <a name="configure-your-connection-string"></a>Konfigurieren Sie die Verbindungszeichenfolge

Service Configuration Mechanismus ermöglicht die Konfiguration von [Azure-Portal][] dynamisch ändern, ohne erneutes Bereitstellen der Anwendung. Fügen Sie beispielsweise eine `Setting` die Bezeichnung auf die Service-Definitionsdatei (**.csdef**), wie im nächsten Beispiel gezeigt.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Geben Sie dann Werte in der Service-Konfigurationsdatei (.cscfg).

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Verwenden Sie freigegebene Access Signatur (SAS) Namen und Werte aus dem Portal abgerufen, wie zuvor beschrieben.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfigurieren der Verbindungszeichenfolge bei Verwendung von Azure Websites oder Azure Virtual Machines

Bei Websites oder virtuellen Computern empfohlen Konfigurationssystem .NET (z. B. Web.config) verwenden. Speichern der Verbindung mit dem `<appSettings>` Element.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Verwenden Sie SAS-Name und Werte, die von [Azure-Portal][]abgerufen, wie zuvor beschrieben.

## <a name="create-a-topic"></a>Erstellen eines Themas

Sie können Management Service Bus Topics und Abonnements [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) Klasse Operationen. Diese Klasse stellt Methoden zum Erstellen, auflisten und Löschen von Themen.

Im folgenden Beispiel wird ein `NamespaceManager` mithilfe der Azure `CloudConfigurationManager` mit einer Verbindungszeichenfolge aus der Basisadresse des Service Bus-Namespace und angemessene SAS Anmeldeinformationen Berechtigungen verwalten. Diese Verbindungszeichenfolge hat das folgende Format:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Anhand des folgenden Beispiels Konfigurationen im vorherigen Abschnitt angegeben.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Es gibt Überladung der Methode [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) , mit denen Eigenschaften des Themas festlegen. um die Standardeinstellung Wert (Time to live, TTL) Nachrichten zum Thema ausgeglichen werden. Diese Werte werden mithilfe der [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) -Klasse angewendet. Im folgenden Beispiel wird veranschaulicht, wie Thema **TestTopic** mit einer maximalen Größe von 5 GB und eine Standardnachricht TTL-Wert 1 Minute erstellt.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] Können die [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) -Methode [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) Objekte überprüft, ob eine Thema mit einem bestimmten Namen in einem Namespace bereits vorhanden ist.

## <a name="create-a-subscription"></a>Erstellen eines Abonnements

Sie können auch Thema Abonnements [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse erstellen. Abonnements können werden benannt und einen optionalen Filter, der Nachrichten an das Abonnement virtuelle Warteschlange übergeben beschränkt.

> [AZURE.IMPORTANT] In Reihenfolge für Nachrichten zu einem Abonnement müssen Sie dieses Abonnement vor dem Senden von Nachrichten an das Thema erstellen. Sind keine Abonnements zu einem Thema, verwirft das Thema dieser Nachrichten.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit der Standardfilter (MatchAll)

Kein Filter festgelegt, wenn ein neues Abonnement erstellt wird, ist der **MatchAll** Filter der Standardfilter verwendet wird. Bei Verwendung des **MatchAll** -Filters befinden alle veröffentlichte Nachrichten zu dem Thema virtuelle Warteschlange für das Abonnement. Im folgenden Beispiel wird ein Abonnement mit dem Namen "AllMessages" erstellt und den **MatchAll** Filter verwendet.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Abonnements mit Filtern erstellen

Sie können auch festlegen, mit denen Sie angeben, welche Nachrichten zu einem Thema sollten Filter innerhalb eines bestimmten Themas Abonnements angezeigt.

Die flexibelste Art von Abonnements unterstützt Filter ist die [SqlFilter][] -Klasse, die eine Teilmenge der SQL92 implementiert. SQL-Filter werden für die Eigenschaften der Nachrichten, die das Thema veröffentlicht werden. Weitere Informationen zu Ausdrücken, die mit SQL-Filter finden Sie unter [SqlFilter.SqlExpression][] Syntax.

Im folgende Beispiel wird ein Abonnement mit dem Namen **HighMessages** und ein [SqlFilter][] -Objekt, das nur Nachrichten ausgewählt, die eine benutzerdefinierte **MessageNumber** -Eigenschaft größer als 3 erstellt.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Ebenso wird im folgenden Beispiel wird ein Abonnement mit dem Namen **LowMessages** mit [SqlFilter][] , die nur Nachrichten ausgewählt, die eine **MessageNumber** -Eigenschaft kleiner oder gleich 3 erstellt.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Jetzt bei einer Nachricht `TestTopic`, wird immer **AllMessages** Thema Abonnement abonniert und selektiv Empfänger abonniert die **HighMessages** und **LowMessages** Thema Abonnements (abhängig vom Inhalt Nachricht) an Empfänger übermittelt.

## <a name="send-messages-to-a-topic"></a>Nachrichten zu einem Thema

Um eine Nachricht zu einem Thema Service Bus senden, erstellt die Anwendung ein [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) -Objekt mit der Verbindungszeichenfolge.

Der folgende Code veranschaulicht, wie ein [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) -Objekt für **TestTopic** Thema erstellt, über die [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API-Aufruf.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Nachrichten Service Bus-Themen sind Instanzen der Klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) -Objekte haben eine Reihe von Standardeigenschaften (wie [Beschriftung](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) und [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) ein Wörterbuch mit benutzerdefinierten anwendungsspezifische Eigenschaften sowie eine beliebige Anwendung Daten. Eine Anwendung kann den Hauptteil der Nachricht serialisierbares Objekt an den Konstruktor des [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) -Objekts übergeben, und die entsprechenden **DataContractSerializer** wird zum Serialisieren des Objekts verwendet. Alternativ können **System.IO.Stream** bereitgestellt werden.

Im folgenden Beispiel wird veranschaulicht, wie das Objekt **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) im vorherigen Codebeispiel 5 Testnachrichten an. Hinweis Die Iteration der Schleife [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) Eigenschaftswert der Nachricht abhängig (Dies bestimmt, welche Abonnements erhalten).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB in [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einem Thema jedoch gibt die Gesamtgröße der Nachrichten durch ein Thema ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung mit einem oberen Grenzwert von 5 GB definiert. Wenn Partitionierung aktiviert ist, liegt die Höchstgrenze. Weitere Informationen finden Sie unter [partitionierte messaging Entitäten](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Wie ein Abonnement Nachrichten

Empfohlen, ein Abonnement Nachrichten ist ein [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) -Objekt verwenden. [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) -Objekte können in zwei verschiedenen Modi arbeiten: [ *ReceiveAndDelete* und *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Mit dem Modus **ReceiveAndDelete** empfangen ist ein Einzelbild Betrieb; also wenn Service Bus in einem Abonnement eine Leseoperation für eine Nachricht empfängt, es markiert die Nachricht als verbraucht wird und an die Anwendung zurückgegeben. **ReceiveAndDelete** ist das einfachste und am besten für Szenarios, in denen eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus als verbraucht, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, die Nachricht gekennzeichnet, wird es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Im **PeekLock** -Modus (der Standardmodus) empfangen wird ein zweistufiger Vorgang Support-Anwendung ermöglicht, die fehlende Nachrichten tolerieren. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nachdem die Anwendung beendet Verarbeitung der Nachricht (oder zuverlässig zur späteren Verarbeitung speichert), abgeschlossen die zweite Phase des Prozesses empfangen fordert die empfangene Nachricht [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ist. Service Bus rufen Sie **Complete** sieht, markiert die Nachricht als verbraucht und das Abonnement entfernt.

Im folgenden Beispiel wird veranschaulicht, wie Nachrichten empfangen und verarbeitet **PeekLock** Standardmodus. Um einen anderen [Abschlussaufruf](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) Wert angeben, können Sie eine andere Überladung für [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). Dieses Beispiel verwendet den [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) -Rückruf zum Verarbeiten von Nachrichten eintreffen in der **HighMessages** -Abonnement.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Das folgende Beispiel konfiguriert [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) Rückruf mithilfe eines [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) -Objekts. [AutoVervollständigen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) ist auf **false** festgelegt, um manuell steuern beim Aufruf [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) in der empfangenen Nachricht. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) auf 1 Minute, wodurch den Client bis zu einer Minute vor Feature automatische Erneuerung warten und der Client ruft neue Nachrichten überprüfen. Dieser Wert reduziert oft, die Client fakturierbar aufruft, die keine Nachrichten abrufen.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Wenn eine empfangende Anwendung die Nachricht aus irgendeinem Grund verarbeiten kann, können sie die [Abandon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) -Methode in der empfangenen Nachricht (nicht [vollständig](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ) aufrufen. Dadurch Service Bus zu entsperren der Nachricht innerhalb des Abonnements zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in das Abonnement gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Sperren Ablauf des Timeouts (z. B. wenn die Anwendung stürzt ab) und Service Bus Nachricht automatisch entsperrt Verfügung erneut empfangen werden.

Die Anwendung stürzt nach dem Verarbeiten der Nachricht, aber vor Anforderung [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ist, wird die Nachricht an die Anwendung zurückgeliefert werden beim Neustart. Dies wird häufig als *einmal verarbeiten*bezeichnet; Jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler ihre Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Dies geschieht häufig mithilfe der [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) -Eigenschaft der Nachricht über Übermittlungsversuche konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Themen und Abonnements löschen

Im folgenden Beispiel wird veranschaulicht, wie das Thema **TestTopic** **HowToSample** Service Namespace löschen.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Ein Thema löschen auch Abonnements mit dem Thema registriert sind. Abonnements können auch einzeln gelöscht werden. Der folgende Code veranschaulicht, wie ein Abonnement mit dem Namen **HighMessages** aus dem **TestTopic** Thema löschen.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Service Bus-Themen und Abonnements bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

-   [Warteschlangen, Themen und Abonnements][].
-   [Thema Filter-Beispiel][]
-   API-Referenz für [SqlFilter][].
-   Eine funktionierende Anwendung, die sendet und empfängt Nachrichten aus einer Warteschlange Service Bus: [Service Bus messaging .NET Lernprogramm vermittelt][].
-   Service Bus Beispiele: von [Azure Proben][] oder finden Sie unter [Übersicht über](service-bus-samples.md).

  [Azure-portal]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [Thema Filter-Beispiel]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Service Bus messaging .NET Lernprogramm vermittelt]: service-bus-brokered-tutorial-dotnet.md
  [Azure-Beispiele]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2

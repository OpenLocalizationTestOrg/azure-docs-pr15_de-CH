<properties
   pageTitle="Optimieren von Azure Code in Visual Studio | Microsoft Azure"
   description="Erfahren Sie, wie Azure Code optimieren von Tools in Visual Studio Code robuster und leistungsfähigere machen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Azure Code optimieren

Wenn Sie apps, mit dem Microsoft Azure programmieren, sind einige Codierungstechniken, die Sie befolgen sollten, um Probleme mit der app Skalierbarkeit, Verhalten und die Leistung in einer Cloud-Umgebung zu vermeiden. Microsoft bietet eine Azure Codeanalysetool, die erkannt und gibt mehrere Probleme häufig auftreten und sie lösen. Sie können die Tools in Visual Studio über NuGet.

## <a name="azure-code-analysis-rules"></a>Azure Codeanalyseregeln

Das Codeanalysetool Azure verwendet die folgenden Regeln automatisch Azure Code kennzeichnen, wenn die gefundenen Probleme behoben. Probleme werden als eine Warnung festgestellt oder Compilerfehler. Code-Updates oder Vorschläge zum Auflösen der Warnung oder Fehler werden oft über ein Glühbirnensymbol bereitgestellt.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Vermeiden Sie die Verwendung der Standard-Sitzungszustandsmodus (in Bearbeitung)

### <a name="id"></a>ID

AP0000

### <a name="description"></a>Beschreibung

Standard (in Bearbeitung) Sitzungszustandsmodus für Cloudanwendungen verwenden, kann der Sitzungszustand verloren gehen.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Standardmäßig ist in der Datei web.config angegebenen Sitzungszustandsmodus. Auch standardmäßig ohne Eintrag in der Konfigurationsdatei angegeben, Modus Sitzungsstatus im Prozess. Der prozessinterne Modus speichert den Sitzungszustand im Arbeitsspeicher auf dem Webserver. Wenn eine Instanz oder eine neue Instanz für Lastenausgleich und Failover-Unterstützung verwendet werden, nicht der Sitzungszustand auf dem Webserver gespeichert gespeichert. Dies verhindert, dass die Anwendung die Cloud zu skalierbar.

ASP.NET Sitzungszustand unterstützt verschiedene Speicheroptionen für Sitzungszustandsdaten: InProc StateServer SQLServer, Benutzerdefiniert und. Es wird empfohlen, benutzerdefinierten Modus auf Hostdaten auf eine externe Sitzungszustandsspeicher, z. B. [Sitzungsstatus Azure Anbieter für Redis](http://go.microsoft.com/fwlink/?LinkId=401521)verwenden.

### <a name="solution"></a>Lösung

Eine empfohlene Lösung ist Sitzungszustands verwalteten Cache-Dienst. Informationen Sie zum [Sitzungsstatus Azure Provider für Redis](http://go.microsoft.com/fwlink/?LinkId=401521) verwenden, den Sitzungsstatus gespeichert. Sie können auch den Sitzungszustand speichern, an anderen stellen sicher, dass die Anwendung in der Cloud skalierbar ist. Weitere Informationen über Alternative lesen Lösungen [Sitzungszustandsmodi](https://msdn.microsoft.com/library/ms178586)bitte.

## <a name="run-method-should-not-be-async"></a>Run-Methode sollte nicht asynchron sein.

### <a name="id"></a>ID

AP1000

### <a name="description"></a>Beschreibung

Erstellen Sie asynchrone Methoden (wie [erwartet](https://msdn.microsoft.com/library/hh156528.aspx)) außerhalb [der Run()-Methode](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) und rufen Sie die asynchronen Methoden [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) Methode als asynchroner deklariert wird die Worker-Rolle eine Neustartschleife eingeben.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Asynchrone Methoden innerhalb der Methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) wird Cloud Service Runtime Recycling Worker-Rolle. Beginnt eine Worker-Rolle findet alle Ausführung innerhalb der Methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Beenden [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) Methode wird Worker-Rolle neu gestartet. Trifft die Arbeitskraft Rolle Runtime die asynchrone Methode, alle Vorgänge nach der asynchronen Methode sendet und dann wieder. Dadurch wird die Worker-Rolle [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) Methode beenden und neu starten. In der nächsten Iteration der Ausführung Worker-Rolle trifft die asynchrone Methode wieder und Neustart verursacht wieder Recycling Worker-Rolle.

### <a name="solution"></a>Lösung

Setzen Sie alle asynchronen Vorgänge außerhalb [der Run()-Methode](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Rufen Sie dann die umgestalteten Async-Methode in der Methode RunAsync () ...Warte [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Das Codeanalysetool Azure helfen bei der Behebung des Problems.

Der folgende Codeausschnitt veranschaulicht das Codeupdate für dieses Problem:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Service Bus SAS-Authentifizierung verwenden

### <a name="id"></a>ID

AP2000

### <a name="description"></a>Beschreibung

Shared Access Signatur (SAS) für die Authentifizierung verwenden. Access Control Service (ACS) wird zur Dienstauthentifizierung Bus ersetzt.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Aus Sicherheitsgründen wird Active Directory Azure ACS-Authentifizierung mit SAS-Authentifizierung ersetzt. Der Übergangsplan Informationen finden Sie unter [Azure Active Directory ist die Zukunft des ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) .

### <a name="solution"></a>Lösung

SAS-Authentifizierung in Ihren apps verwenden. Im folgenden Beispiel wird veranschaulicht, wie mithilfe einen vorhandenen SAS-Token ein Service Bus-Namespace oder Entität zugreifen.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Finden Sie unter den folgenden Themen Weitere Informationen.

- Eine Übersicht finden Sie unter [Freigegebene Signatur Authentifizierung mit Service Bus](https://msdn.microsoft.com/library/dn170477.aspx)

- [Verwendung von Shared Signatur Authentifizierung mit Service Bus](https://msdn.microsoft.com/library/dn205161.aspx)

- Ein Beispielprojekt finden Sie unter [Verwendung von freigegebenen Zugriff Signatur (SAS) Authentifizierung mit Service Bus](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Verwenden Sie OnMessage-Methode zu "Empfangsschleife"

### <a name="id"></a>ID

AP2002

### <a name="description"></a>Beschreibung

Zu gehen in einem "Empfangsschleife" **OnMessage** Methode ist eine bessere Lösung zum Empfangen von Nachrichten als Aufruf der **Receive** -Methode. Jedoch verwenden Sie die **Receive** -Methode, und geben ein Standardserver Wartezeit, sollten Sie sicher, dass die Serverzeit länger als eine Minute.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Wenn **OnMessage**aufrufen, beginnt der Client einen internen Nachrichtensystem, der ständig die Warteschlange oder das Abonnement abfragt. Dieses Nachrichtensystem enthält eine unendliche Schleife, die einen zum Empfangen von Nachrichten Aufruf. Wenn der Aufruf abgelaufen ist, gibt einen neuen Anruf. Das Timeoutintervall bestimmt den Wert der [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) -Eigenschaft der [Fall](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx), der verwendet wird.

Der Vorteil **OnMessage** gegenüber **empfangen** wird, dass Benutzer nicht manuell Nachrichten abrufen sowie Ausnahmen behandeln, mehrere Nachrichten gleichzeitig verarbeiten und Nachrichten.

Aufruf von **Receive** ohne Standardwert unbedingt der *ServerWaitTime* -Wert ist länger als eine Minute. Festlegen von *ServerWaitTime* auf verhindert mehr als eine Minute Server überschritten wurde, bevor die Nachricht vollständig empfangen.

### <a name="solution"></a>Lösung

Finden Sie die folgenden Codebeispiele für empfohlene Verwendung. Weitere Informationen finden Sie unter [QueueClient.OnMessage (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)und [QueueClient.Receive-Methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Zur Leistungssteigerung der Azure Messaginginfrastruktur anzeigen Sie Entwurfsmuster [Asynchronen Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx)

Nachfolgend ein Beispiel **OnMessage** Nachrichten empfangen.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Folgendes ist ein Beispiel mit Standard-Server-Wartezeit **empfangen** .

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Folgendes ist ein Beispiel mit einer nicht standardmäßigen Server-Wartezeit **empfangen** .

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Erwägen Sie asynchrone Service Bus-Methoden

### <a name="id"></a>ID

AP2003

### <a name="description"></a>Beschreibung

Verwenden Sie asynchrone Methoden Service Bus Leistung vermittelten Messaging.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Asynchrone Methoden können Anwendung Programm Parallelität, da jeder Aufruf ausführen nicht den Hauptthread blockiert. Bei Verwendung von Service Bus messaging-Methoden, eine Operation (senden, empfangen, löschen usw.) dauert. Diesmal enthält der Vorgang mit Service Bus neben die Wartezeit der Anforderung und der Antwort. Erhöhen Sie die Anzahl der Vorgänge pro müssen Vorgänge gleichzeitig ausgeführt werden. Weitere Informationen finden Sie unter [Best Practices für die Leistung verbessert mithilfe Service Bus vermittelte Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Lösung

Finden Sie Informationen zur Verwendung von asynchronen empfohlen [QueueClient Klasse (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Zur Leistungssteigerung der Azure Messaginginfrastruktur anzeigen Sie Entwurfsmuster [Asynchronen Messaging Primer](https://msdn.microsoft.com/library/dn589781.aspx)

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Betrachten Sie Partitionierung Servicebuswarteschlangen und Themen

### <a name="id"></a>ID

AP2004

### <a name="description"></a>Beschreibung

Partition Servicebuswarteschlangen und Themen zur Leistungssteigerung mit messaging Service Bus.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Performance-Durchsatz und Service-Verfügbarkeit Partitionieren Servicebuswarteschlangen und Themen erhöht werden, da der Gesamtdurchsatz einer partitionierten Warteschlange oder das Thema nicht mehr durch die Leistung einer einzelnen nachrichtenbroker oder Nachrichtenspeicher begrenzt ist. Darüber hinaus wird ein zeitweilige Ausfall eines messaging eine partitionierte Warteschlange oder ein Thema verfügbar. Weitere Informationen finden Sie unter [Messaging Entitäten Partitionierung](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Lösung

Der folgende Codeausschnitt zeigt messaging Entitäten zu partitionieren.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Weitere Informationen finden Sie unter [partitioniert Service Bus Warteschlangen und | Microsoft Azure-Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) und [Microsoft Azure Service Bus partitioniert Queue](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) -Beispiel.

## <a name="do-not-set-sharedaccessstarttime"></a>SharedAccessStartTime nicht festgelegt

### <a name="id"></a>ID

AP3001

### <a name="description"></a>Beschreibung

Sie sollten sofort freigegeben Zugriffsrichtlinie mit SharedAccessStartTimeset auf die aktuelle Zeit. Sie müssen diese Eigenschaft festlegen, wenn Sie die freigegebene Richtlinie zu einem späteren Zeitpunkt starten möchten.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Synchronisierung der Uhr wird eine leichte Zeitdifferenz zwischen Rechenzentren. Z. B. logisch scheint Festlegen der Startzeit eines Speichers SAS-Richtlinie als die aktuelle Zeit mit DateTime.Now oder eine ähnliche Methode bewirkt, dass die SAS-Richtlinie sofort wirksam. Allerdings können leichte Zeitdifferenz zwischen Rechenzentren Probleme verursachen manchmal Datacenter geringfügig höher als die Zeit, während andere vor möglicherweise. Daher die SAS-Richtlinie ablaufen schnell (oder sogar unmittelbar) Wenn Richtlinie Gültigkeitsdauer zu kurz festgelegt ist.

Weitere Informationen zum Verwenden von SAS auf Azure-Speicher finden Sie unter [Einführung in Tabelle SAS (SAS), Warteschlange SAS und Aktualisierung Blob SAS - Microsoft Azure Storage Team Blog - Homepage - MSDN-Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Lösung

Entfernen Sie die Anweisung, die Startzeit der freigegebene Richtlinie festgelegt. Das Codeanalysetool Azure bietet eine Lösung für dieses Problem. Weitere Informationen zu Sicherheits-Management finden Sie unter den Entwurfsmuster [Valet Schlüssel](https://msdn.microsoft.com/library/dn568102.aspx).

Der folgende Codeausschnitt veranschaulicht das Codeupdate für dieses Problem.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Freigegebene Richtlinien Ablaufzeit mehr als fünf Minuten sein muss

### <a name="id"></a>ID

AP3002

### <a name="description"></a>Beschreibung

Es kann als fünf Minuten Unterschied Uhren von Rechenzentren an verschiedenen Standorten aufgrund einer Bedingung als "Zeitunterschieden." Damit die SAS legen Richtlinie Token ablaufen früher als geplant, die Ablaufzeit mehr als fünf Minuten.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Rechenzentren an verschiedenen Standorten auf der ganzen Welt synchronisieren ein Taktsignal. Da es Taktsignal an verschiedenen Orten Reisen dauert, darf eine Abweichung Zeit zwischen Rechenzentren an verschiedenen geografischen Standorten obwohl angeblich synchronisiert. Diese Differenz kann der Shared Access Start Time und Ablaufdatum Richtlinienintervall auswirken. Daher, um sicherzustellen, dass gemeinsame Richtlinien sofort wirksam wird, geben Sie die Startzeit. Darüber hinaus stellen Sie sicher, dass die Ablaufzeit mehr als 5 Minuten zu früh Timeout ist.

Weitere Informationen zur Verwendung von SAS auf Azure-Speicher finden Sie unter [Einführung in Tabelle SAS (SAS), Warteschlange SAS und Aktualisierung Blob SAS - Microsoft Azure Storage Team Blog - Homepage - MSDN-Blogs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Lösung

Weitere Informationen zu Sicherheits-Management finden Sie unter den Entwurfsmuster [Valet Schlüssel](https://msdn.microsoft.com/library/dn568102.aspx).

Folgendes ist ein Beispiel für eine Shared Access Richtliniestartzeit nicht angeben.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Folgendes ist ein Beispiel für eine Shared Access Richtliniestartzeit Richtlinie Ablauf dauert länger als fünf Minuten angeben.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Weitere Informationen finden Sie unter [Erstellen und Verwenden einer SAS](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>CloudConfigurationManager verwenden

### <a name="id"></a>ID

AP4000

### <a name="description"></a>Beschreibung

Die [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) -Klasse wird nicht für Projekte Azure mobile Dienste wie Azure-Website Laufzeitproblemen führen. Als bewährte Methode ist es jedoch als einheitliche Methode zum Verwalten von Konfigurationen für alle Cloudanwendungen Azure Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) verwenden sollten.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

CloudConfigurationManager liest die Konfigurationsdatei die Umgebung entsprechend.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Lösung

Gestalten Sie den Code, um die [CloudConfigurationManager-Klasse](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)verwenden. Ein Codeupdate für dieses Problem wird durch das Codeanalysetool Azure bereitgestellt.

Der folgende Codeausschnitt veranschaulicht das Codeupdate für dieses Problem. Ersetzen

`var settings = ConfigurationManager.AppSettings["mySettings"];`

mit

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Hier ist ein Beispiel für die Einstellung in der Datei App.config oder Web.config speichern. AppSettings-Abschnitt der Konfigurationsdatei Einstellungen hinzugefügt. So sieht der Datei Web.config für das vorherige Codebeispiel.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Verwenden Sie keine hartcodierten Verbindungszeichenfolgen

### <a name="id"></a>ID

AP4001

### <a name="description"></a>Beschreibung

Wenn Sie hartcodierte Verbindungszeichenfolgen und später aktualisieren müssen, müssen Sie den Quellcode ändern und die Anwendung neu kompilieren. Aber wenn Sie Verbindungszeichenfolgen in einer Konfigurationsdatei speichern, können Sie diese später ändern durch Aktualisieren der Konfigurationsdatei.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Programmieren Verbindungszeichenfolgen ist kein empfehlenswertes Verfahren führt Probleme bei Verbindungszeichenfolgen müssen schnell geändert werden. Darüber hinaus soll das Projekt zur Versionskontrolle eingecheckt, Sicherheitslücken hartcodierten Verbindungszeichenfolgen Sicherheit Da Zeichenfolgen im Quellcode angezeigt werden können.

### <a name="solution"></a>Lösung

Speichern Sie Verbindungszeichenfolgen in Konfigurationsdateien oder Azure-Umgebung.

- Verwenden Sie für eigenständige Applikationen app.config Zeichenfolge Einstellungen speichern.

- Verwenden Sie für IIS-gehosteten ASP.NET-Webanwendungen web.config zum Speichern von Verbindungszeichenfolgen.

- ASP.NET vNext handelt verwenden Sie configuration.json zum Speichern von Verbindungszeichenfolgen.

Informationen zur Verwendung von Konfigurationsdateien wie web.config oder app.config finden Sie unter [ASP.NET Web Konfigurationsrichtlinien](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Informationen zur wie Azure Umgebung Variablen finden Sie unter [Azure Websites: wie Anwendungszeichenfolgen und Connection Strings arbeiten](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Informationen zum Speichern von Verbindungszeichenfolge im Datenquellen-Steuerelement finden Sie unter [vermeiden Sie vertrauliche Informationen wie Verbindungszeichenfolgen in Dateien, die in Quellcode-Repository gespeichert sind](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Verwenden Sie Diagnose-Konfigurationsdatei

### <a name="id"></a>ID

AP5000

### <a name="description"></a>Beschreibung

Statt konfigurieren Diagnose in Ihrem Code z. B. mit Microsoft.WindowsAzure.Diagnostics API programmieren, sollten Sie Diagnose in der Datei diagnostics.wadcfg konfigurieren. (Oder diagnostics.wadcfgx verwenden Sie Azure SDK 2.5). Dadurch können Sie diagnoseeinstellungen ändern, ohne den Code zu kompilieren.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

Bevor Azure SDK 2.5 (der Azure-Diagnose 1.3 verwendet), Azure Diagnostics (Bündel) mithilfe verschiedene Methoden konfiguriert werden konnte: Konfiguration BLOB im Speicher mit imperativem Code, deklarativen Konfiguration oder die Standardkonfiguration hinzufügen. Ist jedoch die bevorzugte Methode zum Konfigurieren der Diagnose mithilfe eine XML-Konfigurationsdatei (diagnostics.wadcfg oder diagnositcs.wadcfgx für SDK 2.5 oder höher) im Anwendungsprojekt. In diesem Beispiel die diagnostics.wadcfg-Datei vollständig definiert die Konfiguration aktualisiert und bereitgestellt werden. Mischen die Verwendung der diagnostics.wadcfg-Konfigurationsdatei mit Programmiermethoden Einstellung Konfigurationen mit [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)oder [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)-Klassen kann zu Verwirrung führen. Weitere Informationen finden Sie unter [initialisieren oder Azure Diagnostics-Konfiguration ändern](https://msdn.microsoft.com/library/azure/hh411537.aspx) .

Beginnend mit Bündel 1.3 (in Azure SDK 2.5 enthalten) kann nicht mit Code Diagnose konfigurieren. Daher können Sie nur die Konfiguration beim Anwenden oder die diagnoseerweiterung Aktualisieren bereitstellen.

### <a name="solution"></a>Lösung

Designer Konfiguration Diagnose Diagnose Einstellungen der Diagnose-Konfigurationsdatei (diagnositcs.wadcfg oder diagnositcs.wadcfgx für SDK 2.5 oder höher) zu verwenden. Auch wird empfohlen, [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) installieren und die neuesten Diagnose verwenden.

1. Das Kontextmenü für die Rolle, die Sie konfigurieren möchten, wählen Sie Eigenschaften und wählen Sie die Registerkarte Konfiguration.

1. Abschnitt **Diagnose** stellen Sie sicher, dass das Kontrollkästchen **Diagnose aktivieren** .

1. Wählen Sie die Schaltfläche **Konfigurieren** .

  ![Zugriff auf die Option Diagnose aktivieren](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Weitere Informationen finden Sie unter [Diagnose Azure Cloud Services und virtuellen Computer konfigurieren](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Vermeiden Sie DbContext Objekte als statisch deklarieren

### <a name="id"></a>ID

AP6000

### <a name="description"></a>Beschreibung

Um Arbeitsspeicher zu sparen vermeiden Sie DBContext Objekte als statisch deklarieren.

Teilen Sie Ihre Ideen und ihr Feedback [Azure Codeanalyse](http://go.microsoft.com/fwlink/?LinkId=403771)Feedback.

### <a name="reason"></a>Grund

DBContext Objekte enthalten die Abfrageergebnisse jeden Aufruf. Statische DBContext-Objekte werden nicht freigegeben, bis die Anwendungsdomäne entladen wird. Daher kann ein statisches Objekt DBContext viel Arbeitsspeicher beanspruchen.

### <a name="solution"></a>Lösung

Deklarieren Sie DBContext als eine lokale Variable oder ein Instanzfeld nicht statische, für einen Vorgang verwenden Sie und lassen Sie ihn nach der Verwendung freigegeben werden.

Die folgenden MVC-Controller-Klasse veranschaulicht, wie mit DBContext-Objekt.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Nächste Schritte

Optimzing und Problembehandlung bei Azure apps finden Sie unter [Problembehandlung bei Web-app in Azure App Service mithilfe von Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

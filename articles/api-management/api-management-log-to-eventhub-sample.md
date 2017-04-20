<properties
    pageTitle="Überwachen der APIs Azure API Management Event Hubs und Runscope"
    description="Beispiel-Anwendung zeigt Log Eventhub Richtlinie verbinden Azure API Management, Azure Ereignis Hubs und Runscope für HTTP und-Protokollierung"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Überwachen der APIs Azure API Management Event Hubs und Runscope

Der [Verwaltungsdienst API](api-management-key-concepts.md) bietet zahlreiche Funktionen zur Verbesserung der Verarbeitung von HTTP-Anfragen, die HTTP-API. Das Vorhandensein von Anfragen und Antworten sind jedoch vorübergehend. Der Anforderung und fließt durch die API-Verwaltungsdienst Backend API. Ihre API verarbeitet die Anforderung und Antwort durch API Verbraucher fließt. API-Verwaltungsdienst hält einige wichtigen Statistiken über die APIs für die Anzeige im Dashboard Portal Publisher aber darüber hinaus, entfernt werden.

Mithilfe der [Protokolldatei Eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [Richtlinie](api-management-howto-policies.md) im API-Verwaltungsdienst können Sie alle Details von Anforderung und Antwort in eine [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md)senden. Es gibt zahlreiche Gründe kann zum Generieren von Ereignissen von HTTP-Nachrichten an APIs. Überwachungsliste Updates, Verwendungsanalysen Ausnahme Alarmierung und 3rd Party Integrationen zählen.   

Dieser Artikel veranschaulicht das Erfassen der gesamten HTTP-Anforderung und die Antwort angezeigt, an Event Hub senden und dann die Nachricht an einen Dienst Drittanbieter, die HTTP-Protokollierung und Überwachung bereitstellt.

## <a name="why-send-from-api-management-service"></a>Warum Senden von API-Verwaltungsdienst?
Es ist möglich, HTTP-Middleware schreiben, der HTTP-API-Frameworks HTTP-Anfragen und Antworten und Protokollierung und Überwachung feed stecken können. Der Nachteil dieses Ansatzes ist HTTP-Middleware in Back-End-API integriert werden und muss die Plattform der API. Gibt es mehrere APIs muss jeweils die Middleware bereitstellen. Häufig gibt es Gründe Warum Back-End-APIs nicht aktualisiert werden.

Integration Protokollierungsinfrastruktur Azure API Management Nutzen bietet eine zentrale und plattformunabhängige Lösung. Es ist darüber hinaus skalierbar, teilweise durch [geometrische](api-management-howto-deploy-multi-region.md) Replikationsfunktionen Azure API Management.

## <a name="why-send-to-an-azure-event-hub"></a>Warum Azure Event Hub senden?
Es ist angemessen, Fragen, warum eine Richtlinie für Azure Ereignis Hubs erstellen? Gibt es viele verschiedene Orte, ich kann meine Anfragen protokollieren möchten. Warum senden nur die Anfragen direkt an das endgültige Ziel?  Das ist eine Option. Allerdings ist bei Protokollierung Anfragen von API Management-Dienst es müssen Auswirkungen Protokollierungsnachrichten auf die Leistung der API. Schrittweise Erhöhung laden können durch Erhöhung der verfügbare Instanzen von Systemkomponenten oder Geo-Replikation behandelt werden. Kurze Spitzen im Datenverkehr kann Anfragen erheblich verzögert werden, wenn Anfragen Protokollierungsinfrastruktur Belastung langsam beginnen.

Azure Ereignis Hubs soll Eindringen großer Datenmengen mit wesentlich mehr Ereignisse als die Anzahl der HTTP-die meisten APIs Prozess Anfragen zu. Ereignis-Hub dient als eine Art Puffer zwischen Ihrem API Management-Dienst und der Infrastruktur, die gespeichert und verarbeitet die Nachrichten anspruchsvolle. Dadurch Ihre API-Leistung durch die Infrastruktur für die Protokollierung nicht beeinträchtigt werden.  

Sobald die Daten an einen Hub Ereignis übergeben wurde wird beibehalten und wartet auf Ereignis Hub Verbraucher verarbeitet. Ereignis-Hub nicht wie er bearbeitet, wohlgeformte XML nur sicherstellen, dass die Nachricht erfolgreich übermittelt werden.     

Ereignis-Hubs können mehrere Verbrauchergruppen Stream Ereignisse. Dadurch können Ereignisse von vollständig unterschiedliche Systeme verarbeitet werden. Dadurch unterstützt viele Integrationsszenarios ohne Zusatz verzögert die Verarbeitung der Anforderung API in API Management-Dienst nur ein Ereignis generiert werden muss.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Eine Richtlinie Anwendung/HTTP-Nachrichten
Event-Hub akzeptiert Daten als einfache Zeichenfolge. Der Inhalt dieser Zeichenfolge sind Sie vollständig. Um zu Verpacken einer HTTP-Anforderung Absenden an Ereignis Hubs müssen mit Anforderung oder Antwort Formatzeichenfolge. In Situationen wird ein vorhandenes Format wiederverwenden wir und wir können keinen eigenen Analysecode schreiben. Als ich mit [HAR](http://www.softwareishard.com/blog/har-12-spec/) zum Senden von HTTP-Anfragen und Antworten. Dieses Format ist jedoch optimiert für eine Sequenz von HTTP-Anfragen in einem JSON-basierten Format gespeichert. Sie enthalten eine Anzahl obligatorisch unnötigen Komplexität Szenario, HTTP-Nachricht über das Netzwerk übergeben hinzugefügten Elemente.  

Alternativ wurde mit der `application/http` Media type in der HTTP-Spezifikation [RFC 7230](http://tools.ietf.org/html/rfc7230)beschrieben. Dieser Medientyp verwendet genaue dem Format HTTP-Nachrichten tatsächlich über das Netzwerk senden, aber die gesamte Nachricht kann in den Textkörper der andere HTTP-Anforderung. In unserem Fall gehen wir nur Text als unsere Nachricht zum Senden an Event Hubs verwenden. Befindet sich ein Parser in [Microsoft ASP.NET Web API 2.2](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) Clientbibliotheken vorhanden ist, die dieses Format analysieren und Konvertieren in die systemeigene `HttpRequestMessage` und `HttpResponseMessage` Objekte.

Um diese Nachricht erstellen wir zu C# basierte [Richtlinienausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management. Hier ist die sendet eine http-Request-Nachricht an Azure Ereignis Hubs.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Erklärung
Gibt es bestimmte Punkte erwähnt zu dieser Richtlinienausdruck. Protokoll Eventhub Richtlinie hat ein Attribut namens Logger-Id bezieht sich auf den Namen der Protokollierung, die in der API Management-Dienst erstellt wurde. Wie ein Event Hub-Protokollierung im API-Verwaltungsdienst setup finden im Dokument [Protokollieren Ereignisse Azure Ereignis Hubs in Azure API Management](api-management-howto-log-event-hubs.md). Das zweite Attribut ist ein optionaler Parameter, der Event Hubs weist der partition im Nachrichtenspeicher. Ereignis-Hubs verwenden Skalierbarkeit und mindestens zwei Partitionen. Die geordnete Übermittlung von Nachrichten ist nur innerhalb einer Partition garantiert. Wenn wir kein Ereignis Hub in welcher Partition die Nachricht anzuweisen, wird einen Round-Robin-Algorithmus Verteilung die Last verwendet. Jedoch möglicherweise, die einige unserer Nachrichten nicht verarbeitet werden.  

### <a name="partitions"></a>Partitionen
Um unsere Nachrichten an Verbraucher geliefert werden und die Verteilung Belastbarkeit Partitionen nutzen, habe ich eine Partition und eine zweite Partition HTTP-Antwortnachrichten http-Request-Nachrichten an. Dadurch eine sogar Verteilung und wir garantieren, dass alle Anfragen in der Reihenfolge verarbeitet und alle Antworten in der Reihenfolge verarbeitet. Auf die entsprechende Anforderung verwendet werden kann, aber kein ist nicht wie ein anderen Mechanismus für das Abgleichen Anfragen zu Antworten und wir wissen, dass Anfragen immer vor dem Antworten.

### <a name="http-payloads"></a>HTTP-Nutzlasten
Nach Erstellung der `requestLine` wir überprüfen, ob der Anforderungstext abgeschnitten werden soll. Der Anforderungstext ist nur 1024 abgeschnitten. Dadurch gesteigert werden, aber einzelne Ereignis Hub Nachrichten auf 256 KB begrenzt so wahrscheinlich ist, dass sich einige HTTP-Nachricht wird nicht in eine Nachricht passen. Protokollierung und Analyse kann eine erhebliche Informationsmenge nur HTTP-Anforderungszeile und Kopfzeilen abgeleitet werden. Auch viele API Anfragen nur kleine zurück und so der Wert abgeschnitten große ist recht gering im Vergleich zu der Rückgang der Übertragung, Verarbeitung und Speicherkosten zu allen Textinhalt. Ein letzter Hinweis zur Verarbeitung von Text ist, dass wir übergeben `true` auf der<string>()-Methode da wir den Textinhalt lesen war aber auch Back-End-API Text gelesen werden soll. Getreu dieser Methode übergeben führen wir Text gepuffert werden, damit sie ein zweites Mal gelesen werden kann. Dies ist wichtig, wenn Sie eine API verfügen, die sehr große Dateien uploaden oder long Polling verwendet. In diesen Fällen wäre es vermeiden, den Text zu lesen.   

### <a name="http-headers"></a>HTTP-Header
HTTP-Header können einfach in das Nachrichtenformat in einem einfachen Schlüssel-Wert-Paar Format übertragen werden. Wir haben bestimmte vertrauliche Felder Sicherheit vermeiden unnötige Verluste im Anmeldeinformationen entfernen. Es ist unwahrscheinlich, dass API-Schlüsseln und anderen Anmeldeinformationen zu Analysezwecken verwendet werden würde. Wollen wir Analyse auf Benutzer und Produkt werden dann konnte man in der `context` -Objekt und, die zur Nachricht hinzufügen.     
### <a name="message-metadata"></a>Nachrichten-Metadaten
Die vollständige Nachricht senden an Event Hub zu erstellen, die erste Zeile wird nicht Bestandteil der `application/http` angezeigt. Die erste Zeile ist zusätzliche Metadaten aus, ob die Meldung ist eine Anforderung oder Antwort und eine Nachrichten-Id dient zum Korrelieren von Anfragen zu antworten. Die Nachrichten-Id wird erstellt, mit einer anderen Richtlinie, die folgendermaßen aussieht:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

Wir könnte haben die Anforderungsnachricht erstellt, gespeichert, bis die Antwort zurückgegeben und dann einfach Anforderung und Antwort einer einzelnen Nachricht gesendete in einer Variablen. Aber man durch Senden der Anforderung und Antwort unabhängig voneinander und verwenden eine Wechselbeziehung beider zueinander, flexibler Nachrichtengröße Möglichkeit nutzen mehrere Partitionen während Nachricht Ordnung und die Anforderung in unserem Dashboard Protokollierung früher erscheinen. Außerdem möglicherweise einige Szenarios, in denen eine gültige Antwort niemals an den Hub Ereignis möglicherweise aufgrund schwerwiegender Anforderungsfehler im API-Verwaltungsdienst gesendet aber noch haben wir einen Datensatz der Anforderung.

Die Richtlinie zum Senden der HTTP-Antwortnachricht ähnelt der Anforderung und so die vollständige Konfiguration sieht folgendermaßen aus:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

Die `set-variable` Richtlinie erstellt einen Wert sowohl über die `log-to-eventhub` Richtlinie in der `<inbound>` Abschnitt und `<outbound>` Abschnitt.  

## <a name="receiving-events-from-event-hubs"></a>Empfang von Ereignissen von Ereignis
Empfangen von Ereignissen von Azure Event Hub mit dem [AMQP-Protokoll](http://www.amqp.org/). Das Microsoft Service Bus Team haben Client Bibliotheken verwendeten Ereignisse erleichtern. Es gibt zwei verschiedene Ansätze unterstützt, wird eine *Direkte Consumer* und einem anderen der `EventProcessorHost` Klasse. Beispiele für diese beiden Ansätze finden im [Ereignis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md). Die kurze Version der Unterschiede ist `Direct Consumer` haben Sie vollständige Kontrolle und `EventProcessorHost` einige grundliegende Arbeit für Sie jedoch von bestimmten Annahmen über wie Ereignisse verarbeitet hat.  

### <a name="eventprocessorhost"></a>EventProcessorHost
In diesem Beispiel verwenden wir die `EventProcessorHost` Einfachheit, es kann jedoch nicht die beste Wahl für dieses spezielle Szenario. `EventProcessorHost`die Festplatte funktioniert sicherzustellen, müssen Threadingprobleme innerhalb einer bestimmten Ereignisklasse Prozessor kümmern. Jedoch in diesem Szenario wir die Nachricht in ein anderes Format konvertieren und an einen anderen Dienst eine asynchrone Methode weiterleitet. Gibt es keine freigegebenen Status und daher keine Threadingprobleme aktualisiert werden muss. In den meisten Fällen `EventProcessorHost` ist wahrscheinlich die beste Wahl und sicher einfacher.     

### <a name="ieventprocessor"></a>IEventProcessor
Zentraler Bestandteil der Verwendung `EventProcessorHost` ist die Erstellung einer eine Implementierung der `IEventProcessor` Schnittstelle enthält die Methode `ProcessEventAsync`. Diese Methode ist hier dargestellt:

  asynchrone Aufgabe IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) {}

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Eine Liste der EventData-Objekte an die Methode übergebenen und durchlaufen wir die Liste. Jede Methode Bytes in ein HttpMessage Objekt analysiert und das Objekt in eine Instanz von IHttpMessageProcessor übergeben.

### <a name="httpmessage"></a>HttpMessage
Die `HttpMessage` Instanz enthält drei Datenelemente:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

Die `HttpMessage` Instanz enthält eine `MessageId` GUID, die ermöglicht es uns, die HTTP-Anforderung die entsprechende HTTP-Antwort und ein boolescher Wert, der angibt, wenn das Objekt eine Instanz einer HttpRequestMessage und HttpResponseMessage. Mithilfe der integrierten HTTP-Klassen von `System.Net.Http`, konnte ich zu den `application/http` gehört Analysecode `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
Die `HttpMessage` Instanz weitergeleitet wird Implementierung von `IHttpMessageProcessor` eine Oberfläche zum Empfang und Interpretation des Ereignisses von Azure Event Hub und die tatsächliche Verarbeitung entkoppeln habe.


## <a name="forwarding-the-http-message"></a>Die HTTP-Nachricht weiterleiten
Für dieses Beispiel entschied ich wäre es interessant, über die HTTP-Anforderung an [Runscope](http://www.runscope.com)übertragen. Runscope ist ein Cloud-basierten Dienst, der HTTP-Debuggen, Protokollierung und Überwachung spezialisiert. Sie haben freie Tier leicht versuchen und ermöglicht auf HTTP-Anfragen in Echtzeit übertragen API Management Service.

Die `IHttpMessageProcessor` Implementierung sieht

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Ich konnte eine [vorhandene Clientbibliothek Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) nutzen, die Push erleichtert `HttpRequestMessage` und `HttpResponseMessage` Instanzen deren Inbetriebnahme. Um die Runscope-API zugreifen benötigen Sie ein Konto und einen API-Schlüssel. Informationen zum Abrufen eines API-Schlüssels finden Sie im Screencast [Applications to Access Runscope API erstellen](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) .

## <a name="complete-sample"></a>Vollständiges Beispiel
[Quellcode](https://github.com/darrelmiller/ApimEventProcessor) und Tests für das Beispiel sind Github. Sie benötigen einen [API-Verwaltungsdienst](api-management-get-started.md) [Verbundenen Ereignis-Hub](api-management-howto-log-event-hubs.md)und ein [Konto](../storage/storage-create-storage-account.md) zum Ausführen des Beispiels selbst.   

Das Beispiel ist nur eine einfache überwacht für Ereignisse Event Hub konvertiert in Konsolenanwendung einen `HttpRequestMessage` und `HttpResponseMessage` -Objekte und leitet sie an die Runscope-API.

In der folgenden Animation eine Anforderung an eine API Entwicklerportal Konsolenanwendungsprojekt die Nachricht empfangen, verarbeitet und weitergeleitet und dann die Anforderung und Antwort im Runscope Traffic Inspector angezeigt.

![Demonstration der Anforderung an Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Zusammenfassung
Azure API Management-Dienst kann HTTP-Datenverkehr von APIs zu erfassen. Azure Event Hubs ist eine hoch skalierbare, kostengünstige Lösung zur Erfassung von Datenverkehr und Einspeisung in sekundäre Systeme für die Protokollierung, Überwachung und andere anspruchsvolle Analytics. Verbindung mit 3rd Party Datenverkehr Überwachungssysteme wie Runscope ein einfaches ein Dutzend Zeilen von Code.

## <a name="next-steps"></a>Nächste Schritte
-   Erfahren Sie mehr über Azure Ereignis Hubs
    -   [Erste Schritte mit Azure Ereignis Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Nachrichten mit EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Ereignis-Hubs Programmierhandbuch](../event-hubs/event-hubs-programming-guide.md)
-   Weitere Informationen zu API-Management und Event Hubs integration
    -   [Ereignisse Azure Ereignis Hubs in Azure API Management anmelden](api-management-howto-log-event-hubs.md)
    -   [Entitätsverweis Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [Protokoll Eventhub Richtlinie Bezug](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    
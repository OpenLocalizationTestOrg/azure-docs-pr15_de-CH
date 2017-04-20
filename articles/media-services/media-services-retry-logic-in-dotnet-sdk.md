<properties
    pageTitle="Wiederholungslogik in Media Services SDK für .NET | Microsoft Azure"
    description="Das Thema Überblick Wiederholungslogik in Media Services SDK für .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Wiederholungslogik in Media Services SDK für .NET

Beim Arbeiten mit Microsoft Azure Services können vorübergehende Fehler auftreten. Wenn ein vorübergehender Fehler in der Regel tritt erfolgreich nach ein paar versuchen der Vorgang. Media Services SDK für .NET implementiert die Wiederholungslogik behandeln vorübergehende Fehler zugeordnete Ausnahmen und Fehler, die Webanfragen werden Abfragen speichern, ändern und Speicherbetrieb.  Media Services SDK für .NET führt standardmäßig vier Versuche vor Ihrer Anwendung die Ausnahme erneut ausgelöst. Der Code in der Anwendung muss dann diese ordnungsgemäß behandeln.  
  
 Im folgenden finden eine kurze Richtlinie Web Request, Speicherung, Abfrage und SaveChanges:  
  
-   Die Speicherrichtlinie wird für Blob Speichervorgänge (Upload oder Download von Objektdateien) verwendet.  
  
-   Die Web Request wird für generische Webanfragen (beispielsweise immer ein Authentifizierungstoken und Auflösen von Benutzern Cluster Endpunkt) verwendet.  
  
-   Die Abfragerichtlinie wird zum Abfragen von Entitäten von anderen (z. B. mediaContext.Assets.Where(...)).  
  
-   Die SaveChanges wird verwendet für etwas, der Daten innerhalb des Dienstes (z. B. Erstellen einer Entität Aktualisieren einer Entität für einen Vorgang eine servicefunktion aufrufen) ändert.  
  
 Dieses Thema listet Ausnahmetypen und Wiederholungslogik Fehlercodes, die von Media Services SDK für .NET behandelt werden.  
  
## <a name="exception-types"></a>Ausnahmetypen  

Die folgende Tabelle beschreibt die Ausnahmen, die Media Services SDK für .NET behandelt oder behandelt nicht für einige Vorgänge, die vorübergehende Fehler verursachen.  
  
Ausnahme|Web-Anforderung|Speicher|Abfrage|SaveChanges
----|------|----|---|---
WebException<br/>Weitere Informationen finden Sie im Abschnitt [WebException Statuscodes](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Ja|Ja|Ja|Ja  
DataServiceClientException<br/> Weitere Informationen finden Sie unter [http-Fehlercodes Status](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nein|Ja|Ja|Ja
DataServiceQueryException<br/> Weitere Informationen finden Sie unter [http-Fehlercodes Status](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nein|Ja|Ja|Ja  
DataServiceRequestException<br/> Weitere Informationen finden Sie unter [http-Fehlercodes Status](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nein|Ja|Ja|Ja  
DataServiceTransportException|Nein|Nein|Ja|Ja
TimeoutException|Ja|Ja|Ja|Nein
SocketException|Ja|Ja|Ja|Ja  
StorageException|Nein|Ja|Nein|Nein 
IOException|Nein|Ja|Nein|Nein
  
###  <a name="WebExceptionStatus"></a>WebException-Statuscodes  

Die folgende Tabelle zeigt die Fehlercodes WebException die Wiederholungslogik implementiert wird. [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) -Enumeration definiert Statuscodes.  
  
Status|Web-Anforderung|Speicher|Abfrage|SaveChanges  
-----|-----------------|-------------|-----------|----------  
Verbindungsfehler|Ja|Ja|Ja|Ja
NameResolutionFailure|Ja|Ja|Ja|Ja  
ProxyNameResolutionFailure|Ja|Ja|Ja|Ja  
SendFailure|Ja|Ja|Ja|Ja
PipelineFailure|Ja|Ja|Ja|Nein  
ConnectionClosed|Ja|Ja|Ja|Nein  
KeepAliveFailure|Ja|Ja|Ja|Nein  
UnknownError|Ja|Ja|Ja|Nein 
ReceiveFailure|Ja|Ja|Ja|Nein  
RequestCanceled|Ja|Ja|Ja|Nein  
Zeitlimit|Ja|Ja|Ja|Nein
Protokollfehler <br/>HTTP-Status Code behandeln die Wiederholung Protokollfehler gesteuert. Weitere Informationen finden Sie unter [http-Fehlercodes Status](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ja|Ja|Ja|Ja|  
  
###  <a name="HTTPStatusCode"></a>HTTP-Status Fehlercodes  

Beim Lösen der Abfrage und SaveChanges Vorgänge DataServiceClientException, DataServiceQueryException oder DataServiceQueryException wird in der Eigenschaft StatusCode der HTTP-Fehlerstatuscode zurückgegeben.  Die folgende Tabelle zeigt die Fehlercodes die Wiederholungslogik implementiert wird.  
  
 
Status|Web-Anforderung|Speicher|Abfrage|SaveChanges 
---|----|----|----|----
401|Nein|Ja|Nein|Nein
403|Nein|Ja<br/>Behandeln von Wiederholungsversuche mit längeren Wartezeiten.|Nein|Nein  
408|Ja|Ja|Ja|Ja
429|Ja|Ja|Ja|Ja  
500|Ja|Ja|Ja|Nein  
502|Ja|Ja|Ja|Nein  
503|Ja|Ja|Ja|Ja  
504|Ja|Ja|Ja|Nein  
  
Wenn Sie wollen ein Blick auf die Implementierung von Media Services SDK für .NET Wiederholungslogik, siehe [Azure Sdk für Media Services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

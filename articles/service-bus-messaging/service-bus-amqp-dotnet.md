<properties 
    pageTitle="Service Bus mit AMQP 1.0 | Microsoft Azure"
    description="Verwendung des Servicebus von .NET mit AMQP"
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>Verwenden von .NET Servicebus mit AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Servicebus SDK herunterladen

AMQP 1.0-Unterstützung ist in Service Bus SDK, Version 2.1 oder höher verfügbar. Sie können sicherstellen, dass Sie die neueste Version downloaden Service Bus Bits von [NuGet][].

## <a name="configuring-net-applications-to-use-amqp-10"></a>Konfigurieren von .NET Applications AMQP 1.0 verwenden

Standardmäßig kommuniziert die Clientbibliothek Service Bus .NET Service Bus-Dienst mit einem dedizierten SOAP-Protokoll. AMQP 1.0 anstelle der Verwendung erfordert Protokoll expliziten Konfiguration Verbindungszeichenfolge Service Bus wie im nächsten Abschnitt beschrieben. Als diese Änderung unverändert Anwendungscode bei AMQP 1.0 verwenden.

In der aktuellen Version sind einige API-Funktionen, die nicht unterstützt werden, wenn Sie AMQP verwenden. Diese nicht unterstützten Funktionen sind im Abschnitt [nicht unterstützte Features und Einschränkungen Verhaltensunterschiede](#unsupported-features-restrictions-and-behavioral-differences)aufgeführt. Einige erweiterte Konfigurationen haben eine andere Bedeutung, wenn Sie AMQP verwenden.

### <a name="configuration-using-appconfig"></a>Konfiguration mit App.config

Es wird empfohlen für Applikationen mit der Konfigurationsdatei App.config gespeichert. App.config können Sie Service Bus handelt die für den Service Bus **ConnectionString** -Wert gespeichert. Eine Beispiel-App lautet wie folgt:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Der Wert der `Microsoft.ServiceBus.ConnectionString` Einstellung ist die Service Bus-Verbindungszeichenfolge, mit der die Verbindung zum Service Bus. Das Format lautet wie folgt:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Wo `[namespace]` und `SharedAccessKey` stammen aus dem [Azure-Portal][]. Weitere Informationen finden Sie unter [Erste Schritte mit Servicebuswarteschlangen][].

Bei AMQP fügen Sie die Verbindungszeichenfolge mit `;TransportType=Amqp`. Diese Notation informiert die Clientbibliothek die Verbindung mit AMQP 1.0 Service Bus zu.

## <a name="message-serialization"></a>Meldungsserialisierung

Das Standardprotokoll verwenden, wird das Standardverhalten für die Serialisierung der Clientbibliothek .NET mit [DataContractSerializer][] Typ eine [BrokeredMessage][] -Instanz für die Übertragung von der Clientbibliothek und den Service Bus serialisiert. Verwendung im Transportmodus AMQP verwendet die Clientbibliothek Typsystem AMQP für die Serialisierung der [vermittelten Nachricht][BrokeredMessage] in einer AMQP-Nachricht. Diese Serialisierung ermöglicht die Meldung empfangen und von der empfangenden Anwendung möglicherweise auf einer anderen Plattform, z. B. eine Java-Anwendung, die JMS-API ausführt auf Service Bus verwendet, interpretiert.

Wenn Sie eine [BrokeredMessage][] -Instanz erstellen, können Sie ein als Parameter an den Konstruktor zu als Textkörper der Nachricht bereitstellen. Für Objekte, die AMQP primitiven Typen zugeordnet werden können, wird der Text in AMQP Datentypen serialisiert. Wenn das Objekt direkt in einen primitiven Typ AMQP zugeordnet werden kann; ein benutzerdefinierter Typ das Objekt serialisiert wird mit [DataContractSerializer][]und die serialisierten Bytes von der Anwendung definiert werden, also in einer AMQP Daten gesendet.

Um die Interoperabilität mit nicht verwenden Sie nur .NET-Typen, die direkt in AMQP Typen für den Textkörper der Meldung serialisiert werden können. In der folgenden Tabelle werden die Typen und die entsprechende Zuordnung zu AMQP-Typsystem.

| .NET Body-Objekttyp          | Zugeordnete AMQP-Typ                          | AMQP Body Typ                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| bool                           | boolescher Wert                                   | AMQP Wert                                                                                                                                                |
| Byte                           | ubyte                                     | AMQP Wert                                                                                                                                                |
| ushort                         | ushort                                    | AMQP Wert                                                                                                                                                |
| uint                           | uint                                      | AMQP Wert                                                                                                                                                |
| ulong                          | ulong                                     | AMQP Wert                                                                                                                                                |
| SByte                          | Byte                                      | AMQP Wert                                                                                                                                                |
| kurze                          | kurze                                     | AMQP Wert                                                                                                                                                |
| int                            | int                                       | AMQP Wert                                                                                                                                                |
| lange                           | lange                                      | AMQP Wert                                                                                                                                                |
| float                          | float                                     | AMQP Wert                                                                                                                                                |
| Doppelte                         | Doppelte                                    | AMQP Wert                                                                                                                                                |
| Dezimal                        | decimal128                                | AMQP Wert                                                                                                                                                |
| Char                           | Char                                      | AMQP Wert                                                                                                                                                |
| DateTime                       | Zeitstempel                                 | AMQP Wert                                                                                                                                                |
| GUID                           | UUID                                      | AMQP Wert                                                                                                                                                |
| Byte]                         | Binär                                    | AMQP Wert                                                                                                                                                |
| Zeichenfolge                         | Zeichenfolge                                    | AMQP Wert                                                                                                                                                |
| System.Collections.IList       | Liste                                      | AMQP Wert: Elemente in der Auflistung können nur diejenigen sein, die in dieser Tabelle definiert sind.                                                             |
| System.Array                   | Array                                     | AMQP Wert: Elemente in der Auflistung können nur diejenigen sein, die in dieser Tabelle definiert sind.                                                             |
| System.Collections.IDictionary | Karte                                       | AMQP Wert: Elemente in der Auflistung können nur diejenigen sein, die in dieser Tabelle definiert sind. Hinweis: nur Zeichenfolgenschlüssel werden unterstützt.                        |
| URI                            | Zeichenfolge beschrieben (siehe folgende Tabelle) | AMQP Wert                                                                                                                                                |
| DateTimeOffset                 | Lange beschrieben (siehe folgende Tabelle)   | AMQP Wert                                                                                                                                                |
| TimeSpan                       | Lange beschrieben (siehe folgende)         | AMQP Wert                                                                                                                                                |
| Stream                         | Binär                                    | AMQP Daten (mehrere möglicherweise). Die Abschnitte enthalten unformatierten Bytes aus dem Streamobjekt gelesen.                                                           |
| Anderes Objekt                   | Binär                                    | AMQP Daten (mehrere möglicherweise). Enthält die serialisierte Binärdatei des Objekts, das DataContractSerializer oder ein Serialisierungsprogramm von der Anwendung bereitgestellte verwendet. |

| .NET-Typ      | Zugeordnete AMQP beschriebenen Typ                                                                                              | Notizen                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| TimeSpan       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nicht unterstützte Features eingeschränkt und Verhaltensunterschiede

Die folgenden Features von Service Bus .NET API werden derzeit nicht unterstützt, wenn Sie AMQP verwenden:

-   Transaktionen

-   Übertragungsziel per

Gibt es auch einige kleine Unterschiede im Verhalten der Bus .NET API Verwendung AMQP im Vergleich zu Standard-Protokoll:

-   Die [OperationTimeout][] -Eigenschaft wird ignoriert.

-   `MessageReceiver.Receive(TimeSpan.Zero)`als implementiert `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Sperrtoken Nachrichten zukommen kann nur von den Empfängern der Nachricht erfolgen, die zunächst die Nachrichten empfangen.

## <a name="controlling-amqp-protocol-settings"></a>AMQP Protokoll Einstellungen

.NET APIs verfügen über mehrere Optionen steuern das Verhalten des Protokolls AMQP:

-   **MessageReceiver.PrefetchCount**: steuert die anfängliche Gutschrift auf eine Verbindung angewendet. Der Standardwert ist 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: die maximale Datenblockgröße AMQP angeboten während der Aushandlung an Steuerelemente öffnen. Der Standardwert ist 65.536 Byte.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Übertragung batchfähig sind, bestimmt dieser Wert die maximale Verzögerung für das Senden von Anlagen. Vom Absender/Empfänger vererbt standardmäßig. Einzelne Sender-Empfänger können die Standardeinstellung überschreiben die 20 Millisekunden.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: Legt fest, ob AMQP über eine SSL-Verbindung Verbindungen. Der Standardwert ist **true**.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Service Bus AMQP Übersicht]
- [AMQP 1.0-Unterstützung für partitionierte Servicebuswarteschlangen und Themen]
- [AMQP Service Bus für WindowsServer]

  [Erste Schritte mit Servicebuswarteschlangen]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure-portal]: https://portal.azure.com
[Service Bus AMQP Übersicht]: service-bus-amqp-overview.md
[AMQP 1.0-Unterstützung für partitionierte Servicebuswarteschlangen und Themen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP Service Bus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx

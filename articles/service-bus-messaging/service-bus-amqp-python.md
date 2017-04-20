<properties 
    pageTitle="Service Bus und Python mit AMQP 1.0 | Microsoft Azure"
    description="Verwendung des Servicebus Python mit AMQP."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-python-with-amqp-10"></a>Verwendung des Servicebus Python mit AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton Python ist eine Python sprachbindung an Proton-C; also Proton Python als Wrapper um ein Modul implementiert in c implementiert

## <a name="download-the-proton-client-library"></a>Die Clientbibliothek Proton herunterladen

Sie können [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)Proton-C und seine zugeordneten Bindings (einschließlich Python) herunterladen. Der Download wird in Form von Quellcode. Um den Code zu erstellen, gehen Sie das heruntergeladene Paket enthaltenen.

Beachten Sie, dass zum Zeitpunkt der Erstellung dieses Dokuments die SSL-Unterstützung in Proton C nur für Linux-Betriebssysteme. Azure Service Bus verlangt die Verwendung von SSL Proton C (und Sprache Bindings) nur lässt sich Service Bus von Linux zugreifen. Aktivieren von SSL auf Windows Proton-C ist derzeit so überprüfen Sie regelmäßig nach Updates.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Arbeiten mit Warteschlangen, Themen und Abonnements von Python Service Bus

Der folgende Code veranschaulicht Nachrichten senden und Empfangen von Service Bus messaging Entität.

### <a name="send-messages-using-proton-python"></a>Nachrichten Sie mit Proton Python

Der folgende Code veranschaulicht eine Nachricht an einen Service Bus messaging Entität.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Nachrichten Sie mit Proton Python

Der folgende Code veranschaulicht eine Meldung von einem Service Bus messaging Entität.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>Nachrichtenübermittlung zwischen .NET und Python Proton

### <a name="application-properties"></a>Anwendungseigenschaften

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python Service Bus .NET APIs

Proton-Python-Nachrichten unterstützen Anwendungseigenschaften folgende: **Int**, **long**, **Float**, **Uuid**, **Bool**, **String**. Python-Code veranschaulicht, wie Eigenschaften für eine Nachricht festlegen, unter Verwendung einer dieser Typen.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

Service Bus .NET API werden Anwendungseigenschaften in der **Properties** -Auflistung des [BrokeredMessage][]ausgeführt. Der folgende Code veranschaulicht die Anwendungseigenschaften einer Meldung von einem Client Python lesen.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

Der folgenden Tabelle werden Python Eigenschaftentypen zu der Eigenschaft.

| Typ der Python-Eigenschaft | .NET Typ |
|----------------------|--------------------|
| int                  | int                |
| float                | Doppelte             |
| lange                 | Int64              |
| UUID                 | GUID               |
| bool                 | bool               |
| Zeichenfolge               | Zeichenfolge             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Servicebus .NET APIs Proton Python

Die [BrokeredMessage][] unterstützt folgende Anwendungseigenschaften: **Byte**, **Sbyte**, **Char**, **short**, **Ushort**, **Int**, **Uint**, **long**, **Ulong**, **Float**, **double**, **decimal**, **Bool**, **Guid**, **Zeichenfolge**, **Uri**, **DateTime**, **DateTimeOffset**, und **TimeSpan**. Folgende .NET Code veranschaulicht, wie Eigenschaften für ein [BrokeredMessage][] -Objekt unter Verwendung der folgenden Typen festlegen.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Python-Code veranschaulicht, wie die Anwendungseigenschaften von einer Meldung von einem Service Bus lesen.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

In der folgenden Tabelle werden Python Eigenschaftentypen Eigenschaftentypen .NET zugeordnet.

| .NET Typ | Typ der Python-Eigenschaft | Notizen                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Byte               | int                  | -                                                                                                                                                                     |
| SByte              | int                  | -                                                                                                                                                                     |
| Char               | Char                 | Proton-Python-Klasse                                                                                                                                                 |
| kurze              | int                  | -                                                                                                                                                                     |
| ushort             | int                  | -                                                                                                                                                                     |
| int                | int                  | -                                                                                                                                                                     |
| uint               | int                  | -                                                                                                                                                                     |
| lange               | int                  | -                                                                                                                                                                     |
| ulong              | lange                 | Proton-Python-Klasse                                                                                                                                                 |
| float              | float                | -                                                                                                                                                                     |
| Doppelte             | float                | -                                                                                                                                                                     |
| Dezimal            | Zeichenfolge               | Decimal ist proton derzeit nicht unterstützt.                                                                                                                     |
| bool               | bool                 | -                                                                                                                                                                     |
| GUID               | UUID                 | Proton-Python-Klasse                                                                                                                                                 |
| Zeichenfolge             | Zeichenfolge               | -                                                                                                                                                                     |
| DateTime           | Zeitstempel            | Proton-Python-Klasse                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | DateTimeOffset.UtcTicks AMQP Typ zugeordnet:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| TimeSpan           | DescribedType        | Timespan.Ticks AMQP Typ zugeordnet:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType        | Uri.AbsoluteUri AMQP Typ zugeordnet:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Standardeigenschaften

Die folgende Tabelle enthält die Zuordnung zwischen standard Nachrichteneigenschaften Proton Python und standard Nachrichteneigenschaften [BrokeredMessage][] .

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python Service Bus .NET APIs

| Proton-Python        | Servicebus .NET         | Notizen                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| robuste              | n/a                      | Service Bus unterstützt nur permanente Nachrichten.               |
| Priorität             | n/a                      | Service Bus unterstützt nur eine einzige Priorität.      |
| Gültigkeitsdauer                  | Message.TimeToLive       | Konvertierung von Proton Python TTL ist in Millisekunden definiert. |
| erste\_Erwerber      | n/a                      | -                                                           |
| Lieferung\_Anzahl      | n/a                      | -                                                           |
| ID                   | Message.MessageID        | -                                                           |
| Benutzer\_Id             | n/a                      | -                                                           |
| Adresse              | Message.To               | -                                                           |
| Betreff              | Message.Label            | -                                                           |
| Antwort\_,            | Message.ReplyTo          | -                                                           |
| Korrelation\_Id      | Message.CorrelationID    | -                                                           |
| Inhalt\_Typ        | Message.ContentType      | -                                                           |
| Inhalt\_Codierung    | n/a                      | -                                                           |
| Ablauf\_Zeit         | n/a                      | -                                                           |
| Erstellung\_Zeit       | n/a                      | -                                                           |
| Gruppe\_Id            | Message.SessionId        | -                                                           |
| Gruppe\_Folge      | n/a                      | -                                                           |
| Antwort\_,\_Gruppe\_Id | Message.ReplyToSessionId | -                                                           |
| Format               | n/a                      | -                                                           |

| Servicebus .NET        | Proton                       | Notizen                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.Content\_Typ        | -                                                           |
| CorrelationId           | Message.Correlation\_Id      | -                                                           |
| EnqueuedTimeUtc         | n/a                          | -                                                           |
| Bezeichnung                   | Message.Subject              | -                                                           |
| MessageId               | Message.ID                   | -                                                           |
| ReplyTo                 | Message.Reply\_,            | -                                                           |
| ReplyToSessionId        | Message.Reply\_,\_Gruppe\_Id | -                                                           |
| ScheduledEnqueueTimeUtc | n/a                          | -                                                           |
| SessionId               | Message.Group\_Id            | -                                                           |
| TimeToLive              | Message.TTL                  | Konvertierung von Proton Python TTL ist in Millisekunden definiert. |
| An                      | Message.Address              | -                                                           |

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Service Bus AMQP Übersicht]
- [AMQP Service Bus für WindowsServer]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP Service Bus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx

[Service Bus AMQP Übersicht]: service-bus-amqp-overview.md

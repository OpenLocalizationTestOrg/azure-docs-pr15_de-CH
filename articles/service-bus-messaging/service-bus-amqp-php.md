<properties 
    pageTitle="Service Bus und PHP mit AMQP 1.0 | Microsoft Azure"
    description="Verwendung des Servicebus von PHP mit AMQP."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Verwendung des Servicebus von PHP mit AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton PHP ist eine PHP sprachbindung an Proton-C; also Proton PHP als Wrapper um ein Modul implementiert in c implementiert

## <a name="downloading-the-proton-client-library"></a>Die Clientbibliothek Proton herunterladen

Sie können [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)Proton-C und seine zugeordneten Bindings (einschließlich PHP) herunterladen. Der Download wird in Form von Quellcode. Um den Code zu erstellen, gehen Sie das heruntergeladene Paket enthalten.

> [AZURE.IMPORTANT] Zum Zeitpunkt der Erstellung dieses Dokuments steht die SSL-Unterstützung in Proton C nur für Linux-Betriebssysteme. Azure Service Bus verlangt die Verwendung von SSL Proton C (und Sprache Bindings) nur lässt sich Service Bus von Linux zugreifen. Proton-C mit SSL auf Windows aktivieren ist, so dass Sie regelmäßig nach Updates.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Arbeiten mit Service Bus-Warteschlangen, Themen und Abonnements von PHP

Der folgende Code veranschaulicht Nachrichten senden und Empfangen von Service Bus messaging Entität.

### <a name="sending-messages-using-proton-php"></a>Senden von Nachrichten mit Proton PHP

Der folgende Code veranschaulicht eine Nachricht an einen Service Bus messaging Entität.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Empfang von Nachrichten mit Proton PHP

Der folgende Code veranschaulicht eine Meldung von einem Service Bus messaging Entität.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>Nachrichtenübermittlung zwischen .NET und Proton PHP

### <a name="application-properties"></a>Anwendungseigenschaften

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP Service-Bus .NET APIs

Proton-PHP-Nachrichten unterstützen Anwendungseigenschaften folgende: **Integer**, **double**, **Boolean**, **String**und **Objekt**. Folgende PHP-Code veranschaulicht, wie Eigenschaften für eine Nachricht festlegen, unter Verwendung einer dieser Typen.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

Service Bus .NET APIs werden Anwendungseigenschaften in der **Properties** -Auflistung des [BrokeredMessage][]ausgeführt. Im folgenden Codebeispiel wird die Anwendungseigenschaften einer Meldung von einem PHP-Client zu lesen.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

In der folgenden Tabelle die Eigenschaftentypen .NET Eigenschaftentypen PHP zugeordnet.

| Typ der PHP-Eigenschaft | .NET Typ |
|-------------------|--------------------|
| ganze Zahl           | int                |
| Doppelte            | Doppelte             |
| boolescher Wert           | bool               |
| Zeichenfolge            | Zeichenfolge             |
| Objekt            | Objekt             |

#### <a name="service-bus-net-apis-to-php"></a>Servicebus .NET APIs PHP

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
```

Folgende PHP-Code veranschaulicht, wie die Anwendungseigenschaften von einer Meldung von einem Service Bus lesen.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

In der folgenden Tabelle die Eigenschaftentypen PHP Eigenschaftentypen .NET zugeordnet.

| .NET Typ | Typ der PHP-Eigenschaft | Notizen                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Byte               | ganze Zahl           | -                                                                                                                                                                     |
| SByte              | ganze Zahl           | -                                                                                                                                                                     |
| Char               | Char              | Proton PHP-Klasse                                                                                                                                                    |
| kurze              | ganze Zahl           | -                                                                                                                                                                     |
| ushort             | ganze Zahl           | -                                                                                                                                                                     |
| int                | ganze Zahl           | -                                                                                                                                                                     |
| uint               | Ganze Zahl           | -                                                                                                                                                                     |
| lange               | ganze Zahl           | -                                                                                                                                                                     |
| ulong              | ganze Zahl           | -                                                                                                                                                                     |
| float              | Doppelte            | -                                                                                                                                                                     |
| Doppelte             | Doppelte            | -                                                                                                                                                                     |
| Dezimal            | Zeichenfolge            | Decimal ist proton derzeit nicht unterstützt.                                                                                                                     |
| bool               | boolescher Wert           | -                                                                                                                                                                     |
| GUID               | UUID              | Proton PHP-Klasse                                                                                                                                                    |
| Zeichenfolge             | Zeichenfolge            | -                                                                                                                                                                     |
| DateTime           | ganze Zahl           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks AMQP Typ zugeordnet:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| TimeSpan           | DescribedType     | Timespan.Ticks AMQP Typ zugeordnet:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI                | DescribedType     | Uri.AbsoluteUri AMQP Typ zugeordnet:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Standardeigenschaften

Die folgende Tabelle enthält die Zuordnung zwischen Proton PHP Standardnachrichteneigenschaften und standard Nachrichteneigenschaften [BrokeredMessage][] .

| Proton-PHP           | Servicebus .NET         | Notizen                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Robuste              | n/a                      | Service Bus unterstützt nur permanente Nachrichten.          |
| Priorität             | n/a                      | Service Bus unterstützt nur eine einzige Priorität. |
| Gültigkeitsdauer                  | Message.TimeToLive       | Konvertierung von Proton PHP TTL ist in Millisekunden definiert.   |
| erste\_Erwerber      | -                          | -                                                          |
| Lieferung\_Anzahl      | -                          | -                                                          |
| ID                   | Message.Id               | -                                                          |
| Benutzer\_Id             | -                          | -                                                          |
| Adresse              | Message.To               | -                                                          |
| Betreff              | Message.Label            | -                                                          |
| Antwort\_,            | Message.ReplyTo          | -                                                          |
| Korrelation\_Id      | Message.CorrelationId    | -                                                          |
| Inhalt\_Typ        | Message.ContentType      | -                                                          |
| Inhalt\_Codierung    | n/a                      | -                                                          |
| Ablauf\_Zeit         | Message.ExpiresAtUTC     | -                                                          |
| Erstellung\_Zeit       | n/a                      | -                                                          |
| Gruppe\_Id            | Message.SessionId        | -                                                          |
| Gruppe\_Folge      | -                          | -                                                          |
| Antwort\_,\_Gruppe\_Id | Message.ReplyToSessionId | -                                                          |
| Format               | n/a                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Servicebus .NET APIs Proton PHP

| Servicebus .NET        | Proton-PHP                                             | Notizen                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Nachrichten -\>Inhalt\_Typ                                | -                                                        |
| CorrelationId           | Nachrichten -\>Korrelation\_Id                              | -                                                        |
| EnqueuedTimeUtc         | Nachrichten -\>Kommentare [X-opt-Warteschlange-Time]             | -                                                        |
| Bezeichnung                   | Nachrichten -\>Thema                                      | -                                                        |
| MessageId               | Nachrichten -\>Id                                           | -                                                        |
| ReplyTo                 | Nachrichten -\>Antwort\_,                                    | -                                                        |
| ReplyToSessionId        | Nachrichten -\>Antwort\_,\_Gruppe\_Id                         | -                                                        |
| ScheduledEnqueueTimeUtc | Nachrichten -\>Kommentare ["X-opt-geplant-Warteschlange-Time"] | -                                                        |
| SessionId               | Nachrichten -\>Gruppe\_Id                                    | -                                                        |
| TimeToLive              | Nachrichten -\>Ttl                                          | Konvertierung von Proton PHP TTL ist in Millisekunden definiert. |
| An                      | Nachrichten -\>Adresse                                      | -                                                        |

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Service Bus AMQP Übersicht]
- [AMQP Service Bus für WindowsServer]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP Service Bus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx
[Service Bus AMQP Übersicht]: service-bus-amqp-overview.md

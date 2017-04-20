<properties
    pageTitle="Wie Sie Service Bus Topics (Ruby) | Microsoft Azure"
    description="Informationen Sie zum Azure Service Bus Topics und Abonnements verwenden. Codebeispiele sind für Ruby Applikationen geschrieben."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Wie Bus Themen/Daueraufträge

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieser Artikel beschreibt, wie Service Bus Topics und Abonnements Ruby Applications. Die Szenarios enthalten ein Thema **Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements** **Erstellen von Themen und Abonnements Abonnementfilter Nachrichten erstellen** . Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Weiter](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Service Bus-Themen und Abonnements

Service Bus-Themen und Abonnements unterstützen eine *Veröffentlichen/Abonnieren* Kommunikationsmodell messaging. Themen und Abonnements sind Komponenten einer verteilten Anwendung nicht direkt miteinander kommunizieren; Diese Nachrichten stattdessen über ein Thema, das als Vermittler fungiert.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Themen und Abonnements bieten im Gegensatz zu Servicebuswarteschlangen, wobei jede Nachricht ein Verbraucher verarbeitet wird, eine **1: n -** Form der Kommunikation Veröffentlichen-Abonnieren-Musters. Es ist möglich, mehrere Abonnements zu einem Thema zu registrieren. Wenn eine mit einem Thema Nachricht sie dann für jedes Abonnement erfolgt unabhängig voneinander verarbeitet.

Ein Thema Abonnement ähnelt eine virtuelle Warteschlange, die Nachrichten empfängt, die dem Thema gesendet wurden. Optional können Sie Filterregeln zu einem Thema auf einer Basis pro Abonnement registrieren Filter oder beschränkt zu einem Thema durch welche Abonnements Thema empfangen werden können.

Service Bus-Themen und Abonnements können Sie eine große Anzahl von Nachrichten über eine große Anzahl von Benutzern und Applikationen zu skalieren.

## <a name="create-a-namespace"></a>Erstellen eines Namespaces

Zunächst in Azure Service Bus-Warteschlangen verwenden, müssen Sie zuerst einen Namespace erstellen. Ein Namespace bietet ein Bereichseinheit Service Bus Ressourcen in der Anwendung. Sie müssen den Namespace über die Befehlszeilenschnittstelle erstellen, da [Azure-Portal][] den Namespace nicht mit ACS Verbindung erstellen.

So erstellen Sie einen namespace

1. Öffnen Sie ein Konsolenfenster Azure Powershell.

2. Geben Sie den folgenden Befehl einen Namespace erstellen. Eigener Wert Geben Sie und den gleichen Bereich wie die Anwendung.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Namespace erstellen](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Erhalten Sie standardmäßig Management Anmeldeinformationen für den namespace

Zur Durchführung von Verwaltungsoperationen wie Erstellen einer Warteschlange für den neuen Namespace benötigen Sie Anmeldeinformationen Management für den Namespace.

Das PowerShell-Cmdlet Service Bus-Namespace erstellen Lief zeigt die Taste, die Sie verwenden können, um den Namespace zu verwalten. Kopieren Sie den Wert **DefaultKey** . Dieser Wert wird im Code später in diesem Lernprogramm verwenden werden.

![Schlüssel kopieren](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> Diese Schlüssel können auch herausfinden, ob Sie [Azure-Portal][] anmelden und die Verbindungsinformationen für den Namespace.

## <a name="create-a-ruby-application"></a>Ruby-Anwendung erstellen

Informationen finden Sie unter [Erstellen einer Anwendung Ruby Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Sie Ihre Anwendung mit Service Bus

Um Service Bus verwenden, downloaden Sie und verwenden Sie das Paket Ruby Azure Komfort Bibliotheken enthält, die mit Storage REST-Dienste kommunizieren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems des Pakets zu verwenden

1. Verwenden Sie eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster Gem und Abhängigkeiten installieren "Gem installieren Azure".

### <a name="import-the-package"></a>Paket importieren

Verwenden einen Texteditor, fügen Sie Folgendes zu der Dateianfang Ruby Speicher verwendet werden soll:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Service Bus Verbindung

Azure-Modul liest die Umgebungsvariablen **AZURE\_SERVICEBUS\_NAMESPACE** und **AZURE\_SERVICEBUS\_Zugriff\_Schlüssel** für die Verbindung zu Ihrem Namespace erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie Namespaceinformationen vor **Azure::ServiceBusService** mit dem folgenden Code:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Legen Sie den Wert Wert, den gesamten URL erstellt. Z. B. **"Yourexamplenamespace"**, nicht "yourexamplenamespace.servicebus.windows.net".

## <a name="create-a-topic"></a>Erstellen eines Themas

Das **Azure::ServiceBusService** -Objekt können Sie mit Themen arbeiten. Der folgende Code erstellt ein **Azure::ServiceBusService** -Objekt. Erstellen Sie ein Thema mithilfe der **Erstellen\_topic()** Methode. Das folgende Beispiel erstellt ein Thema oder Fehler ausgegeben, falls vorhanden.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Sie können auch ein **Azure::ServiceBus::Topic** -Objekt mit zusätzlichen Optionen Thema Standardeinstellungen wie Nachricht Live überschreiben können oder maximale Warteschlangengröße übergeben. Das folgende Beispiel zeigt die maximale Warteschlangengröße auf 5GB und Zeit 1 Minute festlegen:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Abonnements erstellen

Thema Abonnements werden auch mit dem **Azure::ServiceBusService** -Objekt erstellt. Abonnements können werden benannt und einen optionalen Filter, der das Abonnement virtuelle Warteschlange übermittelt Nachrichten beschränkt.

Abonnements bleiben und weiterhin bis entweder sie oder das Thema sie sind, werden gelöscht. Enthält anwendungsspezifische Logik ein Abonnement erstellen, wird zunächst überprüft, wenn das Abonnement bereits vorhanden ist, mit der GetSubscription-Methode.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit der Standardfilter (MatchAll)

**MatchAll** Filter ist der Filter, der verwendet wird, kein Filter festgelegt wird, wenn ein neues Abonnement erstellt wird. Bei der **MatchAll** Filter befinden alle veröffentlichte Nachrichten zu dem Thema virtuelle Warteschlange für das Abonnement. Im folgenden Beispiel wird ein Abonnement mit dem Namen "alle Nachrichten" erstellt und den **MatchAll** Filter verwendet.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Abonnements mit Filtern erstellen

Sie können auch Filter, mit denen Sie angeben, welche Nachrichten zu einem Thema sollte innerhalb eines bestimmten Dauerauftrags angezeigt.

Die flexibelste Art von Abonnements unterstützt Filter ist **Azure::ServiceBus::SqlFilter**, das wenigen SQL92 implementiert. SQL-Filter werden für die Eigenschaften der Nachrichten, die das Thema veröffentlicht werden. Weitere Informationen über Ausdrücke, die mit SQL-Filter überprüfen Sie [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) Syntax.

Ein Abonnement Filter hinzufügen möchten, mithilfe der **Erstellen\_rule()** Methode des **Azure::ServiceBusService** -Objekts. Diese Methode können Sie ein vorhandenes Abonnement neue Filter hinzufügen.

Da der Filter automatisch auf alle neuen Abonnements angewendet wird, entfernen Sie zuerst den Standardfilter oder **MatchAll** überschreiben alle Filter, die Sie angeben können. Entfernen die Standard-Regel mithilfe der **Löschen\_rule()** Methode für das **Azure::ServiceBusService** -Objekt.

Im folgenden Beispiel wird ein Abonnement mit dem Namen "High-Nachrichten" mit **Azure::ServiceBus::SqlFilter** , die nur Nachrichten ausgewählt, die eine benutzerdefinierte **Meldung\_Anzahl** -Eigenschaft größer als 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Ebenso wird im folgenden Beispiel wird ein Abonnement mit dem Namen "Low-Nachrichten" mit **Azure::ServiceBus::SqlFilter** , die nur Nachrichten ausgewählt, die eine **Message_number** Eigenschaft kleiner oder gleich 3 erstellt:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Beim Senden einer Nachricht "Test-Thema" nun werden immer erfolgt an Empfänger "alle Nachrichten" Thema Abonnement abonniert und selektiv an Empfänger abonniert die "High-Nachrichten" und "Low-Nachrichten" Thema Abonnements (je nach Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Nachrichten zu einem Thema

Um eine Nachricht zu einem Thema Service Bus Ihrer Anwendung muss die **Senden\_Thema\_message()** Methode für das **Azure::ServiceBusService** -Objekt. Nachrichten Service Bus-Themen sind Instanzen von **Azure::ServiceBus::BrokeredMessage** -Objekten. **Azure::Servicebus::BrokeredMessage** Objekte weisen eine Reihe von Standardeigenschaften (wie **Beschriftung** und **Zeit\_,\_live**), ein Wörterbuch mit benutzerdefinierten Anwendung spezifischen Eigenschaften und ein Zeichenfolge Daten. Eine Anwendung kann den Hauptteil der Nachricht durch einen Wert übergeben Festlegen der **Senden\_Thema\_message()** Methode und alle erforderlichen Standardeigenschaften mit Standardwerten aufgefüllt.

Im folgenden Beispiel wird veranschaulicht, wie fünf "Test-Thema" Testnachrichten senden. Hinweis der Wert der benutzerdefinierten Eigenschaft **Message_number** jeder Nachricht Iteration der Schleife variieren (dadurch bestimmt, welches Abonnement empfängt):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB in [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einem Thema jedoch gibt die Gesamtgröße der Nachrichten durch ein Thema ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung mit einem oberen Grenzwert von 5 GB definiert.

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten über ein Abonnement

Nachrichten aus einem Abonnement mithilfe der **empfangen\_Abonnement\_message()** Methode für das **Azure::ServiceBusService** -Objekt. In der Standardeinstellung Nachrichten sind read(peak) und ohne das Abonnement löschen gesperrt. Lesen und Löschen der Nachricht aus dem Abonnement durch Festlegen der **Blick\_Sperren** Option auf **false festgelegt**.

Standardmäßig wird das Lesen und Löschen von einem zweistufigen Vorgang dadurch auch möglich, die fehlende Nachrichten tolerieren unterstützen. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nach Anwendung Verarbeitung der Meldung beendet (oder zuverlässig zur späteren Verarbeitung speichert) schließt Sie die zweite Phase des Prozesses empfangen durch Aufrufen von **Löschen\_Abonnement\_message()** -Methode und mit der Nachricht als Parameter gelöscht werden. Die **Löschen\_Abonnement\_message()** -Methode markiert verbraucht und das Abonnement aufheben.

Wenn der **: Peek\_Sperren** Parametersatz auf **false**, lesen und Löschen der Nachricht wird die einfachste und eignet sich für Szenarios, in dem eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht als verbraucht markiert haben wird, wird dann die Anwendung neu gestartet und beginnt, Nachrichten erneut es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Das folgende Beispiel veranschaulicht, wie die Nachrichten empfangen werden können und verarbeiteten mit **empfangen\_Abonnement\_message()**. Im Beispiel wird zuerst empfängt und Löschen einer Nachricht aus der "niedrig-Nachrichten" mit **: Peek\_Sperren** auf **false**festgelegt, wird eine andere von "High-Nachrichten Meldung" und löscht die Nachricht mit **Löschen\_Abonnement\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Wenn eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund, rufen sie die **Entsperren\_Abonnement\_message()** Methode für das **Azure::ServiceBusService** -Objekt. Dadurch Service Bus zu entsperren der Nachricht innerhalb des Abonnements zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in das Abonnement gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung stürzt ab), Service Bus wird die Nachricht automatisch entsperrt und Verfügbarmachen erneut empfangen werden.

Die Anwendung stürzt nach Verarbeitung der Nachricht vor der **Löschen\_Abonnement\_message()** wird aufgerufen, und die Nachricht wird beim Neustart der Anwendung neuzugestellt. Dies wird häufig **Mindestens einmal verarbeiten**bezeichnet; Jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler ihre Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Diese Logik erfolgt häufig über die **Nachricht\_Id** -Eigenschaft der Meldung über Übermittlungsversuche konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Themen und Abonnements löschen

Themen und Abonnements sind beständig und müssen über das [Azure-Portal][] oder programmgesteuert explizit gelöscht werden. Im folgenden Beispiel veranschaulicht, wie mit dem Namen "Test-Thema" Thema löschen.

```
azure_service_bus_service.delete_topic("test-topic")
```

Ein Thema löschen auch Abonnements mit dem Thema registriert sind. Abonnements können auch einzeln gelöscht werden. Der folgende Code veranschaulicht, wie mit dem Namen "High-Nachrichten" des Themas "Test-Thema" Abonnement löschen:

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Service Bus Topics bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

- [Warteschlangen, Themen und Abonnements](service-bus-queues-topics-subscriptions.md)anzeigen
- API-Referenz für [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx).
- Besuchen Sie das [Azure SDK Ruby](https://github.com/Azure/azure-sdk-for-ruby) Repository GitHub.
 
[Azure-portal]: https://portal.azure.com

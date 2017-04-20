<properties
    pageTitle="Verwendung von Servicebuswarteschlangen mit Ruby | Microsoft Azure"
    description="Erfahren Sie in Azure Service Bus-Warteschlangen verwenden. Codebeispiele in Ruby geschrieben."
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

# <a name="how-to-use-service-bus-queues"></a>Wie Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dieses Handbuch beschreibt die Servicebuswarteschlangen verwenden. Die Proben sind in Ruby geschrieben und Azure Gem verwenden. Die Szenarios enthalten **Warteschlangen erstellen, senden und Empfangen von Nachrichten**und **Löschen von Warteschlangen**. Weitere Informationen zu Servicebuswarteschlangen finden Sie im Abschnitt [Weiter](#next-steps) .

## <a name="what-are-service-bus-queues"></a>Was sind Servicebuswarteschlangen?

Servicebuswarteschlangen unterstützen *vermittelten messaging* Kommunikationsmodell. Mit Warteschlangen sind Komponenten einer verteilten Anwendung nicht direkt miteinander kommunizieren; Stattdessen tauschen sie Nachrichten über eine Warteschlange als Vermittler fungiert. Nachricht Erzeuger (Absender) gibt eine Meldung an die Warteschlange und dann die Verarbeitung.
Asynchrone Nachricht Verbraucher (Empfänger) Ruft die Nachricht aus der Warteschlange und verarbeitet. Der Hersteller muss nicht warten auf eine Antwort vom Consumer um verarbeiten und weitere Nachrichten senden. Warteschlangen bieten **erste, erste Out (FIFO)** Nachrichten über einen oder mehrere konkurrierende Consumer. D. h. Nachrichten normalerweise empfangen und vom Empfänger in der Reihenfolge, in der sie der Warteschlange hinzugefügt wurden und jede Nachricht empfangen und verarbeitet nur eine Nachricht Verbraucher, verarbeitet.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Servicebuswarteschlangen sind eine allgemeine Technologie für eine Vielzahl von Szenarios verwendet werden kann:

-   Kommunikation zwischen Web- und Workerrollen in [Azure-Anwendung mit mehreren Ebenen](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md).
-   Kommunikation zwischen lokalen apps und Azure gehostet apps in einer [Hybrid-Lösung](service-bus-dotnet-hybrid-app-using-service-bus-relay.md).
-   Kommunikation zwischen Komponenten einer verteilten Anwendung in verschiedenen Organisationen oder Abteilung einer Organisation lokal ausgeführt.

Warteschlangen kann können Sie Ihre Anwendung besser skalieren, und mehr Flexibilität der Architektur.

## <a name="create-a-namespace"></a>Erstellen eines Namespaces

Zunächst in Azure Service Bus-Warteschlangen verwenden, müssen Sie zuerst einen Namespace erstellen. Ein Namespace bietet ein Bereichseinheit Service Bus Ressourcen in der Anwendung. Sie müssen den Namespace über die Befehlszeilenschnittstelle erstellen, da Azure-Portal den Namespace nicht mit ACS Verbindung erstellen.

So erstellen Sie einen namespace

1. Eine Azure Powershell-Konsole zu öffnen.

2. Geben Sie den folgenden Befehl einen Service Bus-Namespace erstellen. Eigener Wert Geben Sie und den gleichen Bereich wie die Anwendung.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Stellen Sie Anmeldeinformationen für den Namespace management

Zur Durchführung von Verwaltungsoperationen wie Erstellen einer Warteschlange für den neuen Namespace benötigen Sie Anmeldeinformationen Management für den Namespace.

Das PowerShell-Cmdlet Azure Service Bus-Namespace erstellen Lief zeigt die Taste, die Sie verwenden können, um den Namespace zu verwalten. Kopieren Sie den Wert **DefaultKey** . Dieser Wert wird im Code später in diesem Lernprogramm verwenden werden.

![Schlüssel kopieren](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] Diese Schlüssel können auch herausfinden, ob Sie [Azure-Portal](https://portal.azure.com/) anmelden und die Verbindungsinformationen für den Service Bus-Namespace.

## <a name="create-a-ruby-application"></a>Ruby-Anwendung erstellen

Ruby-Anwendung zu erstellen. Informationen finden Sie unter [Erstellen einer Anwendung Ruby Azure](/develop/ruby/tutorials/web-app-with-linux-vm/).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Sie die Anwendung für Service Bus

Verwendung von Azure Service Bus Paket herunterladen Sie und verwenden Sie das Ruby Azure, das benutzerfreundliche Bibliotheken enthält, die mit Storage REST-Dienste kommunizieren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems des Pakets zu verwenden

1. Verwenden Sie eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster Gem und Abhängigkeiten installieren "Gem installieren Azure".

### <a name="import-the-package"></a>Paket importieren

Verwenden einen Texteditor, fügen Sie Folgendes zu der Dateianfang Ruby Speicher verwendet werden soll:

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Ein Azure Service Bus-Verbindung einrichten

Azure-Modul liest die Umgebungsvariablen **AZURE\_SERVICEBUS\_NAMESPACE** und **AZURE\_SERVICEBUS\_ACCESS_KEY** für die Verbindung zu Ihrem Service Bus-Namespace erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie Namespaceinformationen vor **Azure::ServiceBusService** mit dem folgenden Code:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Legen Sie den Wert Wert, den gesamten URL erstellt. Z. B. **"Yourexamplenamespace"**, nicht "yourexamplenamespace.servicebus.windows.net".

## <a name="how-to-create-a-queue"></a>Erstellen eine Warteschlange

Das **Azure::ServiceBusService** -Objekt können Sie mit Warteschlangen arbeiten. Um eine Warteschlange zu erstellen, verwenden Sie die **create_queue()** -Methode. Das folgende Beispiel erstellt eine Warteschlange oder Fehler ausgegeben.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Sie können auch ein **Azure::ServiceBus::Queue** -Objekt mit zusätzlichen Optionen übergeben die Warteschlange Standardeinstellungen maximale Warteschlangengröße oder Nachrichtzeit überschreiben ermöglicht. Im folgenden Beispiel wird veranschaulicht, wie die maximale Warteschlangengröße auf 5 GB und Zeit 1 Minute festgelegt:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Gewusst wie: Senden von Nachrichten an eine Warteschlange

Service Bus-Warteschlange Anwendung ruft eine Nachricht an die **Senden\_Warteschlange\_message()** Methode für das **Azure::ServiceBusService** -Objekt. Nachrichten an (und erhielt von) Servicebuswarteschlangen **Azure::ServiceBus::BrokeredMessage** Objekte und verfügen über eine Reihe von Standardeigenschaften (wie **Beschriftung** und **Zeit\_,\_live**), ein Wörterbuch, das die benutzerdefinierte Anwendung spezifischen Eigenschaften und eine beliebige Anwendung Daten. Eine Anwendung kann den Hauptteil der Nachricht übergeben einen Zeichenfolgenwert wie die Nachricht und alle erforderlichen standard Eigenschaften mit Standardwerten aufgefüllt.

Im folgenden Beispiel wird veranschaulicht, wie eine Testnachricht an die Warteschlange mit dem Namen "Test-Warteschlange" **Senden\_Warteschlange\_message()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Servicebuswarteschlangen unterstützt eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB im [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Gibt es keine Beschränkung für die Anzahl der Nachrichten in einer Warteschlange, jedoch ist es die Gesamtgröße der Nachrichten von einer Warteschlange. Diese Größe wird zum Zeitpunkt der Erstellung mit einem oberen Grenzwert von 5 GB definiert.

## <a name="how-to-receive-messages-from-a-queue"></a>Nachrichten aus einer Warteschlange empfangen

Nachrichten mit der **wird\_Warteschlange\_message()** Methode für das **Azure::ServiceBusService** -Objekt. Standardmäßig werden Nachrichten gelesen und gesperrt werden, ohne aus der Warteschlange gelöscht. Aber Sie können Nachrichten aus der Warteschlange gelesen werden durch Festlegen der **: Peek_lock** Option auf **false festgelegt**.

Standardmäßig wird das Lesen und Löschen von einem zweistufigen Vorgang dadurch auch möglich, die fehlende Nachrichten tolerieren unterstützen. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nach Anwendung Verarbeitung der Meldung beendet (oder zuverlässig zur späteren Verarbeitung speichert) schließt Sie die zweite Phase des Prozesses empfangen durch Aufrufen von **Löschen\_Warteschlange\_message()** -Methode und mit der Nachricht als Parameter gelöscht werden. Die **Löschen\_Warteschlange\_message()** Methode verbraucht markiert und aus der Warteschlange entfernt.

Wenn der **: Peek\_Sperren** Parametersatz auf **false**, lesen und Löschen der Nachricht wird die einfachste und eignet sich für Szenarios, in dem eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus markiert haben, die Nachricht verarbeitet, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, wird es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Im folgenden Beispiel wird veranschaulicht, wie empfangen und Verarbeiten von Nachrichten mit **empfangen\_Warteschlange\_message()**. Im Beispiel wird zuerst empfängt und löscht eine Nachricht mit **: Peek\_Sperren** auf **false**festgelegt, wird eine andere Meldung und löscht die Nachricht mit **Löschen\_Warteschlange\_message()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Wenn eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund, rufen sie die **Entsperren\_Warteschlange\_message()** Methode für das **Azure::ServiceBusService** -Objekt. Dadurch Service Bus zu entsperren der Nachricht in der Warteschlange zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in der Warteschlange gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung stürzt ab), und Service Bus Nachricht automatisch entsperrt Verfügung erneut empfangen werden.

Die Anwendung stürzt nach Verarbeitung der Nachricht vor dem **Löschen\_Warteschlange\_message()** wird aufgerufen, und die Nachricht wird beim Neustart der Anwendung erneut. Dies wird häufig **Mindestens einmal verarbeiten**bezeichnet; Jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler ihre Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Dies geschieht häufig mit der **Nachricht\_Id** -Eigenschaft der Meldung über Übermittlungsversuche konstant bleibt.

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Servicebuswarteschlangen bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

-   Übersicht über [Warteschlangen, Themen und Abonnements](service-bus-queues-topics-subscriptions.md).
-   Besuchen Sie das [Azure SDK Ruby](https://github.com/Azure/azure-sdk-for-ruby) Repository GitHub.

Ein Vergleich zwischen Artikel und Azure-Warteschlangen [wie Warteschlangenspeicher von Ruby](../storage/storage-ruby-how-to-use-queue-storage.md) Artikel ausführlich erörtert Azure Service Bus-Warteschlangen finden Sie unter [Azure-Warteschlangen und Azure Service Bus Warteschlangen - verglichen und Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 

<properties
   pageTitle="Azure Funktionen Übersicht | Microsoft Azure"
   description="Verstehen Sie, wie Azure Funktionen zur Optimierung der asynchroner Arbeitslasten in Minuten."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure Funktionen, Funktionen, Verarbeitung, Webhooks, dynamische Compute, serverlose Architektur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Übersicht über Azure Funktionen

Azure Funktionen ist eine Lösung für problemlos mit kleinen Teilen oder "Funktionen" in der Cloud. Sie können den Code schreiben müssen Sie das Problem, ohne eine vollständige Anwendung bzw. Infrastruktur. Damit kann Entwicklung produktiver, und Entwicklungssprache, z. B. C#, F#, Node.js, Python oder PHP verwenden. Bezahlen Sie nur für die Zeit der Code vertrauen Azure zu skalieren.

Dieses Thema enthält eine Übersicht der Azure-Funktionen. Möchten Sie direkt und mit Azure Funktionen, beginnen Sie mit [erstellen Ihre erste Azure-Funktion](functions-create-first-azure-function.md). Wenn Sie weitere technische Informationen zu Funktionen suchen, finden Sie unter [Referenz für Entwickler](functions-reference.md).

## <a name="features"></a>Funktionen

Hier sind einige der wichtigsten Features von Azure-Funktionen:
    
* **Sprache** - Schreibfunktionen mit C#, F#, Node.js, Python, PHP, Batch, Bash, Java oder eine ausführbare Datei.
* **Bezahlung pro Einsatz Preismodell** – Zahlen nur für die Zeit Ihren Code ausführt. Dynamische App Service-Plan Option in die [Preise Abschnitt](#pricing) unten angezeigt.  
* **Bringen Sie Ihre eigenen Abhängigkeiten** - Funktionen unterstützt NuGet und NPM, so können Sie Ihre bevorzugten Bibliotheken.  
* **Integrierte Sicherheit** - Funktionen mit OAuth wie Active Directory Azure, Facebook, Google, Twitter und Microsoft Account HTTP-Schutz ausgelöst.  
* **Vereinfachte Integration** - Nutzung leicht Azure Services und Software-as-a-Service (SaaS) angeboten. Finden Sie im [Abschnitt Integrations](#integrations) unter Beispiele.  
* **Flexible Entwicklung** - Code die Funktionen direkt im Portal oder fortlaufende Integration einrichten und Bereitstellen von Code über GitHub, Visual Studio Team Services und andere [Entwicklungstools unterstützt](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Open-Source** - Laufzeit der Funktionen ist Open Source und [auf GitHub.](https://github.com/azure/azure-webjobs-sdk-script)  

## <a name="what-can-i-do-with-functions"></a>Was kann ich mit?

Azure Funktionen ist eine großartige Lösung für Datenverarbeitung, Integration von Systemen, mit der Internet-von-(IoT) arbeiten und einfache APIs und Microservices. Sie können Funktionen für Aufgaben wie das Image oder Auftrag verarbeiten, Wartung, lang andauernde Vorgänge in einem Hintergrundthread ausgeführt oder für alle Aufgaben, die nach einem Zeitplan ausgeführt werden soll. 

Funktionen bietet Vorlagen erhalten Sie wichtige Szenarien, einschließlich der folgenden:

* **BlobTrigger** - Prozess Azure Storage Blobs beim Container hinzugefügt werden. Sie können dies für Bildgröße.
* **EventHubTrigger** - reagieren auf Ereignisse an den Azure Event Hub. Anwendungsinstrumentation Benutzer Erfahrung oder Workflow-Verarbeitung und Internet der Dinge (IoT) Szenarien besonders nützlich.
* **Generische Webhook** - Prozess Webhook HTTP-Anfragen von jedem Dienst, der Webhooks unterstützt.
* **GitHub Webhook** - reagieren auf Ereignisse in GitHub Repositories. Ein Beispiel finden Sie unter [Erstellen einer Webhook oder API-Funktion](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - Trigger die Ausführung des Codes mit einer HTTP-Anforderung.
* **QueueTrigger** - auf Nachrichten in einer Warteschlange Azure Storage eintreffen. Ein Beispiel finden Sie unter [Erstellen einer Azure-Funktion der Azure-Dienst gebunden wird](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - Code anderen Azure Services verbinden oder lokale Dienste durch Warteschlangen überwachen. 
* **ServiceBusTopicTrigger** - Code anderen Azure Services verbinden oder lokale Dienste abonnieren Themen. 
* **TimerTrigger** - Bereinigung oder andere Stapelverarbeitungsaufgaben nach einem vordefinierten Zeitplan ausführen. Ein Beispiel finden Sie unter [Erstellen Sie eine Ereignisverarbeitungsregel Funktion](functions-create-an-event-processing-function.md).

Azure Funktionen unterstützt *Trigger*, die Ausführung des Codes zu werden, und *Bindungen*Methoden zur Vereinfachung der Programmierung für ein-und Ausgabe. Eine ausführliche Beschreibung von Triggern und Bindungen Azure Funktionen finden Sie unter [Azure Funktionen Trigger und Bindings-Entwicklerreferenz](functions-triggers-bindings.md).


## <a name="integrations"></a>Integrationen

Azure Funktionen integriert mit einer Vielzahl von Azure und 3rd Party-Dienste. Diese können Sie Ihre Funktion ausgelöst und starten die Ausführung zu dienen als Eingabe und Ausgabe für den Code. Die folgenden Service-Integrationen Azure-Funktionen unterstützt. 

* Azure DocumentDB
* Azure Event Hubs 
* Azure Mobile Apps (Tabellen)
* Azure Notification Hubs
* Azure Service Bus (Warteschlangen und Themen)
* Azure-Speicher (BLOBs, Queues und Tabellen) 
* GitHub (Webhooks)
* Lokal (mit Service Bus)

## <a name="pricing"></a>Wie viel Kosten funktioniert?

Azure Funktionen besitzt zwei Arten von Preisplänen wählen, die Ihren Bedürfnissen am besten entspricht: 

* **Dynamische Hosting Plan** - Wenn die Funktion ausführt, Azure stellt alle erforderlichen Datenverarbeitungsressourcen. Müssen Ressourcenmanagement kümmern und Sie Zahlen nur für die Zeit, die der Code ausgeführt wird. Vollständige Preisangaben sind auf der [Seite Funktionen Preise](/pricing/details/functions). 

* **App Service-Plan** - Ihre Funktionen wie die Web, Mobile und API-apps ausführen. Wenn Sie bereits App Service für Ihre Anwendung verwenden, führen Sie Ihre Funktionen Ertrag ohne zusätzliche Kosten. Einzelheiten finden auf der [Seite Anwendung Preisen](/pricing/details/app-service/).

Weitere Informationen zur Skalierung Ihrer Funktionen finden Sie unter [wie Azure-Funktionen](functions-scale.md).

##<a name="next-steps"></a>Nächste Schritte

+ [Erstellen Sie Ihre erste Azure-Funktion](functions-create-first-azure-function.md)  
Direkt, und erstellen Sie die erste Funktion mit Schnellstart Azure-Funktionen. 
+ [Azure Funktionen-Entwicklerreferenz](functions-reference.md)  
Stellt weitere technische Informationen über die Funktionen von Azure Runtime und einen Verweis für die Codierung von Funktionen und Trigger und Bindung definieren.
+ [Testen der Azure-Funktionen](functions-test-a-function.md)  
Beschreibt die verschiedenen Tools und Techniken zum Testen der Funktionen.
+ [Wie Azure Funktionen](functions-scale.md)  
Beschreibt Servicepläne mit Azure-Funktionen, einschließlich dynamische Service-Plan und den richtigen Plan auswählen. 
+ [Erfahren Sie mehr über Azure App](../app-service/app-service-value-prop-what-is.md)  
Azure Funktionen nutzt Azure App Service Plattform für Kernfunktionen wie Bereitstellung, Umgebungsvariablen und Diagnose. 
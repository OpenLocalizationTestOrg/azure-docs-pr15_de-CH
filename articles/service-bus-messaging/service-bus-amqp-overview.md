<properties 
    pageTitle="Übersicht über Service Bus AMQP | Microsoft Azure" 
    description="Informationen Sie zur Verwendung der erweiterten Message Queuing Protocol (AMQP) 1.0 in Azure." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>



# <a name="amqp-10-support-in-service-bus"></a>AMQP 1.0-Unterstützung in Service Bus

Die Azure Service Bus Cloud-Dienst und lokalen [Service Bus für Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) unterstützt die erweiterte Message Queueing Protokoll (AMQP) 1.0. AMQP können Sie plattformübergreifende Hybrid Applications öffnen Standardprotokoll verwenden. Erstellen Sie Komponenten, die mit anderen Sprachen und Frameworks integriert sind und die auf verschiedenen Betriebssystemen ausgeführt, Applications. Alle diese Komponenten Service Bus und nahtlose Verbindung können Nachrichten strukturierter originalgetreue und effizient.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Einführung: Was AMQP 1.0 ist und warum es wichtig ist?

Nachrichtenbasierte Middleware-Produkte haben bisher proprietäre Protokolle für die Kommunikation zwischen Clientanwendungen und Broker verwendet. Dies bedeutet, dass Sie nach der Auswahl eines bestimmten Herstellers messaging Broker des Kreditors Bibliotheken Clientanwendungen, Broker Verbindung verwenden müssen. Dadurch einen Grad der Abhängigkeit von dem Anbieter da Portieren einer Anwendung zu einem anderen Produkt Ändern von Code in der verbundenen Anwendung erforderlich ist. 

Darüber hinaus ist messaging Broker von unterschiedlichen Herstellern schwierig. Dies erfordert in der Regel auf Nachrichten von einem System in ein anderes verschieben und ihre proprietären Nachrichtenformate übersetzen bridging. Dies ist ein häufig auftretendes Erfordernis. z. B. Wenn Sie müssen eine neue einheitliche Schnittstelle zu älteren heterogener Systeme bereitstellen oder eine Fusion IT-Systeme integrieren.

Der Software-Branche ist ein Unternehmen schnell. neuen Programmiersprachen und Anwendungsframeworks werden manchmal verwirrenden Tempo eingeführt. Ebenso an IT-Systemen langfristig weiterentwickelt und Entwickler die neuesten Plattformfeatures nutzen. Jedoch unterstützt manchmal messaging ausgewählte Kreditor nicht diesen Plattformen. Da Messagingprotokolle proprietäre sind, kann keine andere Bibliotheken für diese neuen Plattformen bieten. Daher verwenden Sie Methoden wie Gateways oder Brücken, mit denen Sie weiterhin messaging verwenden.

Die Entwicklung von der Advanced Message Queuing Protocol (AMQP) 1.0 wurde dazu motiviert. JP Morgan Chase, wie die meisten finanziellen Dienstleistungsunternehmen nutzen nachrichtenbasierte Middleware zurückgeht. Ziel war: ein offener Standard Protokoll erstellen, die Nachrichten basierende Anwendung Komponenten mit unterschiedlichen Sprachen, Frameworks und Betriebssystemen erstellt werden alle Best-of-Breed-Komponenten verschiedener Anbieter verwenden.

## <a name="amqp-10-technical-features"></a>AMQP 1.0-Leistungsmerkmale

AMQP 1.0 ist eine effiziente, zuverlässige und auf niedriger Ebene Messagingprotokoll, mit denen Sie stabile und plattformübergreifende Messaginganwendungen. Das Protokoll hat ein einfaches Ziel: die Mechanismen der sichere, zuverlässige und effiziente Übertragung von Nachrichten zwischen zwei Parteien definieren. Die Nachrichten selbst werden codiert eine portable Datentypen Darstellung, die heterogene Sendern und Empfängern originalgetreue strukturierter Nachrichten austauschen können. Folgendes ist eine Zusammenfassung der wichtigsten Funktionen:

*    **Effizient**: AMQP 1.0 ist ein verbindungsorientiertes Protokoll, eine binäre Codierung Protokoll Informationen und geschäftliche Nachrichten übertragen sie, verwendet. Es enthält hochentwickelte Flusskontrolle Schemas Maximieren Sie die Nutzung des Netzwerks und die verbundenen Komponenten. Andererseits das Protokoll soll ein Gleichgewicht zwischen Effizienz, Flexibilität und Interoperabilität.
*    **Zuverlässig**: der AMQP 1.0-Protokoll ermöglicht Nachrichtenaustausch mit Zuverlässigkeitsgarantien aus auslösen und vergessen, zuverlässig und genau-einmal Lieferung bestätigt.
*    **Flexibel**: AMQP 1.0 ist ein flexibles Protokoll mit verschiedene Topologien unterstützt. Client-zu-Client, Client Broker und Broker Broker kann dasselbe Protokoll verwendet werden.
*    **Broker - unabhängig**: die AMQP 1.0-Spezifikation macht alle Vorschriften auf das messaging Modell, ein Broker. Dies bedeutet, dass vorhandene messaging Broker leicht AMQP 1.0-Unterstützung hinzugefügt wird.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 ist ein Standard (mit des Großbuchstaben ")

AMQP 1.0 ist eine internationale Norm ISO und IEC als ISO/IEC 19464:2014 genehmigt.

AMQP 1.0 wurde 2008 durch eine Kerngruppe von mehr als 20 Unternehmen Technologielieferanten und Endbenutzer Unternehmen. Während dieser Zeit Unternehmen haben reale Erfahrung und Technologie-Anbieter das Protokoll die Anforderungen entwickelt. Dabei haben Kreditoren Workshops teilgenommen in denen sie zusammengearbeitet, um die Interoperabilität zwischen ihrer Implementierung zu überprüfen.

Im Oktober 2011 wurde die Entwicklungsarbeit zur technischen Ausschusses innerhalb der Organisation für die Advancement of Structured Information Standards (OASIS) und OASIS AMQP 1.0 Standard im Oktober 2012 veröffentlicht. Die folgenden Unternehmen beteiligt technischen Ausschuss während der Entwicklung des Standards:

*    **Anbieter**: Axway Software, Huawei Technologies, IT Software, INETCO Systeme, Kaazing, Microsoft, Mitre Corporation, Primeton Technologies, Fortschritt Software, Red Hat, SITA, Software AG Trost Systeme, VMware, WSO2, Zenika.
*    **Unternehmen**: die Bank of America, Credit Suisse, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.

Häufig Vorteile offener Standards gehören:

*    Chance kleiner Anbieter
*    Interoperabilität
*    Umfassende Verfügbarkeit von Bibliotheken und Tools
*    Schutz vor veralten
*    Verfügbarkeit von Fachpersonal
*    Untere und verwaltbare Risiko

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 und Service Bus

AMQP 1.0-Unterstützung in Azure Service Bus bedeutet, dass Sie Service Bus queuing nutzen und Veröffentlichen/Abonnieren von vermittelten Messagingfunktionen zwischen Plattformen mit effizienten binären Protokolls. Darüber hinaus können Sie Applikationen umfasst Komponenten durch eine Mischung von Sprachen, Frameworks und Betriebssystemen erstellen.

Die folgende Abbildung zeigt eine beispielbereitstellung in der Java-Clients unter Linux mit standardmäßigen Java Message Service (JMS) API und Clients unter Windows .NET geschrieben über Service Bus AMQP 1.0 Nachrichtenaustausch.

![][0]

**Abbildung 1: Beispiel-Bereitstellungsszenario mit plattformübergreifende Nachrichtenübermittlung Service Bus mit AMQP 1.0**

Zurzeit sind die folgenden Clientbibliotheken arbeiten mit Service Bus bekannt:

| Sprache | Bibliothek                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Apache Qpid Java Message Service (JMS) client<br/>IT-Software SwiftMQ Java-client |
| C        | Apache Qpid Proton-C                                                          |
| PHP      | Apache Qpid Proton PHP                                                        |
| Python   | Apache Qpid Proton Python                                                     |
| C#       | AMQP .net Lite                                                                |

**Abbildung 2: Tabelle Clientbibliotheken für AMQP 1.0**

## <a name="summary"></a>Zusammenfassung

*    AMQP 1.0 ist ein open, reliable messaging-Protokoll, plattformübergreifende hybride Anwendung erstellen. AMQP 1.0 ist ein OASIS-Standard.
*    AMQP 1.0-Unterstützung steht bei Azure Service Bus, Service Bus für Windows Server (Service Bus 1.1). Preisgestaltung entspricht der vorhandenen Protokolle.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Verwendung des Servicebus von .NET mit AMQP]
- [Verwendung des Servicebus von Java mit AMQP]
- [Verwendung des Servicebus Python mit AMQP]
- [Verwendung des Servicebus von PHP mit AMQP]
- [Installation von Apache Qpid Proton-C auf Azure Linux VM]
- [AMQP Service Bus für WindowsServer]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[Verwendung des Servicebus von .NET mit AMQP]: service-bus-amqp-dotnet.md
[Verwendung des Servicebus von Java mit AMQP]: service-bus-amqp-java.md
[Verwendung des Servicebus Python mit AMQP]: service-bus-amqp-python.md
[Verwendung des Servicebus von PHP mit AMQP]: service-bus-amqp-php.md
[Installation von Apache Qpid Proton-C auf Azure Linux VM]: service-bus-amqp-apache.md
[AMQP Service Bus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx
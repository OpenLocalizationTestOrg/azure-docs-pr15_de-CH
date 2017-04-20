<properties 
    pageTitle="Service Bus Preise und Abrechnung | Microsoft Azure"
    description="Überblick über Service Bus Preisstruktur."
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
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Service Bus Preise und Abrechnung

Service Bus bietet Basic, Standard und [Premium](service-bus-premium-messaging.md) -Ebenen. Dienstebene für jeden Service Servicebus-Namespace, die Sie erstellen können, und diese Ebene Auswahl gilt für alle Personen in diesem Namespace erstellt.

>[AZURE.NOTE] Detaillierte Informationen über aktuelle Service Bus Preise finden Sie in [Azure Service Bus Preisseite](https://azure.microsoft.com/pricing/details/service-bus/)und [Service Bus FAQ](service-bus-faq.md#service-bus-pricing).

Service Bus verwendet die folgenden zwei Meter für Warteschlangen und Themen-Abonnements:

1. **Messaging-Operationen**: als API-Aufrufe an Warteschlange oder ein Thema-Abonnement-Endpunkte definiert. Dieses Messgerät ersetzt Nachrichten gesendet oder empfangen als Primäreinheit berechenbare Auslastung für Warteschlangen und Themen-Abonnements.

2. **Vermittelte Verbindung**: definiert die Höchstzahl der ständige Verbindung mit Warteschlangen, Themen oder Abonnements während einer bestimmten einstündige Abtastzeitabschnitts öffnen. Dieses Messgerät gilt nur in der Standard, in dem Sie zusätzliche Verbindungen öffnen (früher waren Verbindungen auf 100 pro Warteschlange/Thema/Abonnement) gegen eine geringe pro Verbindung.

Die **Standard** -Stufe führt abgestufte Preisgestaltung für Operationen mit Warteschlangen und Themen/Abonnements resultierende Volume-basierte Rabatte von bis zu 80 % bei höchster Auslastung. Steht ein Standard-Tier Grundgebühr $ 10 pro Monat, wodurch Sie bis zu 12,5 Mio. Operationen pro Monat ohne zusätzliche Kosten.

Die **Premium** -Stufe bietet Ressource Isolierung Ebene CPU- und damit jede Arbeitslast Kunden isoliert ausgeführt wird. Diese Ressourcencontainer wird eine *Einheit messaging*bezeichnet. Jeder Namespace Premium ist mindestens eine Messaging-Einheit zugewiesen. Sie können 1, 2 oder 4 messaging Einheiten für jeden Premium-Service Bus-Namespace erwerben. Eine einzelne Arbeitslast oder Entität kann mehrere messaging Einheiten umfassen und messaging Stückzahl zwar Abrechnung 24 Stunden oder die täglichen Kosten wird, geändert werden. Das Ergebnis ist vorhersehbar und wiederholbare Leistung für Ihr Service Bus-basierte Lösung. Nicht nur ist diese Leistung besser vorhersagbar und verfügbar, sondern ist außerdem schneller. Azure Service Bus Premium messaging basiert auf dem Speichermodul in Azure Ereignis Hubs eingeführt. Spitzenleistung ist schneller als die Standard-Stufe Premium-messaging.

Beachten Sie, dass der standard Grundgebühr nur einmal pro Monat und Azure-Abonnement ist. Dies bedeutet, dass nach dem Erstellen eines einzelnen Standard oder Premium Tier Service Bus Namespaces Sie so viele zusätzliche Standard oder Premium Tier Namespaces werden beliebig unter diesem gleichen Azure-Abonnement ohne zusätzliche Grundgebühren erstellen.

Alle vorhandenen Service Bus Namespaces vor dem 1. November 2014 erstellt wurden automatisch in die Standard-Stufe. Dadurch, dass Sie weiterhin Zugriff auf alle Funktionen mit Service Bus. [Azure-Verwaltungsportal][] können Sie anschließend bei Bedarf, die grundlegende Stufe heruntergestuft.

In der folgenden Tabelle werden die funktionalen Unterschiede zwischen den Stufen Basic und Standard-Premium zusammengefasst.

|Funktion|Grundlegende|Standard-Premium|
|---|---|---|
|Warteschlangen|Ja|Ja|
|Geplante Nachrichten|Ja|Ja|
|Themen-Abonnements|Nein|Ja|
|Relais|Nein|Ja|
|Transaktionen|Nein|Ja|
|Deduplizierung|Nein|Ja|
|Sessions|Nein|Ja|
|Große Nachrichten|Nein|Ja|
|ForwardTo|Nein|Ja|
|SendVia|Nein|Ja|
|Brokerverbindungen (enthalten)|100 Service Bus-namespace|1.000 pro Azure-Abonnement|
|Brokerverbindungen (Überschuss zulässig)|Nein|Ja (berechenbar)|

## <a name="messaging-operations"></a>Messaging-Abläufen

Abrechnung für Warteschlangen und Themen-Abonnements wird als Teil der neuen Modell ändern. Diese Entitäten Übergang von Abrechnung Meldung Abrechnungsinformationen pro Vorgang. Eine "Operation" bezieht sich auf API-Aufrufe gegen eine Warteschlange oder ein Thema-Abonnement-Endpunkt. Hierzu zählen auch Management, Übermittlung und Sitzung Zustand Vorgänge.

|Vorgangstyp|Beschreibung|
|---|---|
|Management|Erstellen Sie, lesen, aktualisieren und löschen (CRUD) Warteschlangen oder Themen-Abonnements.|
|Messaging|Senden und Empfangen von Nachrichten mit Warteschlangen oder Themen-Abonnements.|
|Sitzungszustand|Abrufen oder Festlegen der Sitzungszustand für eine Warteschlange oder ein Thema-Abonnement.|

Preise waren wirksam ab 1. November 2014:

|Grundlegende|Kosten|
|---|---|
|Vorgänge|0,05 pro million Vorgänge|

|Standard|Kosten|
|---|---|
|Grundgebühr|$10 pro Monat|
|Zuerst 12,5 Mio. Operationen pro Monat|Enthalten|
|12,5 100 Millionen Operationen pro Monat|$0,80 pro million Operationen|
|100-2.500 Millionen Operationen pro Monat|0,50 $ pro million Vorgänge|
|Über 2.500 Millionen Operationen pro Monat|0,20 $ pro million Operationen|

|Premium|Kosten|
|---|---|
|Täglich|$11.13 festen Preis pro Nachricht|

## <a name="brokered-connections"></a>Vermittelte Verbindung

*Vermittelt Verbindung* aufnehmen Verwendungsmuster der Kunden, die eine große Anzahl von "ständig verbundene" Absender/Empfänger Warteschlangen, Themen oder Abonnements. Permanent verbundene Absender/Empfänger sind solche, die eine Verbindung über AMQP oder HTTP mit einer NULL Timeout (beispielsweise HTTP long polling) erhalten. HTTP-Sendern und Empfängern mit einer sofortigen Timeout vermittelte Verbindung generieren.

Bisher Warteschlangen und Themen-Abonnements maximal 100 gleichzeitige Verbindungen pro URL. Das aktuelle Abrechnung Schema pro URL-Beschränkung für Warteschlangen und Themen-Abonnements entfernt und Kontingente und Messung vermittelten Verbindungen auf Service Bus-Namespace und Azure-Abonnement implementiert.

Die grundlegende Ebene enthält und ist streng auf 100 brokerverbindungen pro Service Bus-Namespace. Verbindung über diese Nummer werden in der grundlegenden abgelehnt. Die Standard-Stufe pro Namespace Grenze entfernt und aggregate vermittelte Verbindung Verwendung in Azure-Abonnement zählt. Im Standard-Tier können 1.000 vermittelte Verbindungen pro Azure-Abonnement kostenlos (außer Grundgebühr). Über Standard-Tier Service Bus höchstens 1.000 vermittelte Verbindung werden Namespaces in Azure-Abonnement abgestufter Zeitplan abgerechnet wie in der folgenden Tabelle dargestellt.

|Brokerverbindungen (Standard-Tier)|Kosten|
|---|---|
|Ersten 1.000|Grundgebühr enthalten|
|1.000 100.000 pro Monat|0,03 $ pro Verbindung monatlich|
|100.000-500.000 pro Monat|0,025 $ pro Verbindung monatlich|
|Über 500.000-Monat|$0,015 pro Verbindung pro Monat|

>[AZURE.NOTE] 1.000 vermittelten enthaltenen Standard messaging-Schicht (über die Grundgebühr) und in allen Warteschlangen, Themen und Abonnements innerhalb des zugeordneten Azure Abonnements gemeinsam genutzt werden.

<br />

>[AZURE.NOTE] Abrechnung basiert auf die maximale Anzahl von gleichzeitigen Zugriffen und stündlich basierend auf 744 Stunden pro Monat fällig ist.

|Premium-Ebene
|---|
|Brokerverbindungen Zahlen in der Premium-Ebene.|

Weitere Informationen zu brokerverbindungen finden Sie unter [FAQ](#faq) im Abschnitt weiter unten in diesem Thema.

## <a name="relay"></a>Relay

Relays sind nur im Standard-Tier-Namespaces verfügbar. Andernfalls, Preise und Verbindung Kontingente für Relays bleiben unverändert. Dies bedeutet, dass Relays weiterhin berechnet die Anzahl der Nachrichten (nicht Operations) und Stunden weiterzuleiten.

|Relay-Preise|Kosten|
|---|---|
|Relay-Stunden|0,10 $ 100 Relay-Stunden|
|Nachrichten|0,01 für alle 10.000 Nachrichten|

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="how-is-the-relay-hours-meter-calculated"></a>Wie wird das Messgerät Relaystunden berechnet?

[Thema.](service-bus-faq.md#how-is-the-relay-hours-meter-calculated)

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Welche Verbindung ausgehandelt und wie werden mir in Rechnung gestellt?

Vermittelte Verbindung wird wie folgt definiert:

1. Eine AMQP-Verbindung von einem Client eine Service Bus-Warteschlange oder ein Thema-Abonnement.

2. Einen HTTP-Aufruf zum Empfangen einer Meldung aus einer Service Bus-Thema oder Warteschlange mit empfangen Timeout größer als 0 (null).

Service Bus Gebühren für die maximale Anzahl der gleichzeitigen vermittelten Verbindungen, die der enthalten (1.000 im Standard-Tier) übersteigen. Spitzen stündlich gemessen durch Division durch 744 Stunden im Monat fällig und monatliche Abrechnung Zeitraum addiert. Die Menge enthalten (1.000 vermittelten Verbindungen pro Monat) wird am Ende des Abrechnungszeitraums für die Summe der anteiligen stündlichen Spitzen.

Zum Beispiel:

1. 10.000 Geräte über eine einzige AMQP verbindet und Service Bus-Thema Befehle empfängt. Die Geräte senden Telemetrie-Ereignisse an Event Hub. Wenn alle 12 Stunden pro Tag Geräte die folgende Verbindung anfallen (neben anderen Service Bus Thema Gebühren): 10.000 Verbindungen *12 Stunden* 31 Tage / 744 = 5.000 vermittelte Verbindung. Nach monatliche Vergütung von 1.000 vermittelte Verbindung würden Sie 4.000 brokerverbindungen in Höhe von 0,03 $ pro vermittelte Verbindung für insgesamt 120 $ berechnet.

2. 10.000 Geräte erhalten Nachrichten aus einer Warteschlange Service Bus über HTTP ein NULL Zeitlimit Wenn alle 12 Stunden täglich Geräte sehen Sie die folgenden Gebühren (neben anderen Service Bus Gebühren): 10.000 HTTP-Empfangsfunktion Verbindungen *12 Stunden pro Tag* 31 Tage / 744 h = 5.000 vermittelte Verbindung.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Gelten vermittelte Verbindungsgebühren Warteschlangen und Themen-Abonnements?

Ja. Es fallen keine Verbindung zum Senden von Ereignissen über HTTP unabhängig von der Anzahl der Systeme oder Geräte senden. Empfang von Ereignissen mit HTTP mit größer Null bezeichnet "long polling," Timeout vermittelten Kosten generiert. AMQP Verbindung generieren vermittelten Verbindungsgebühren unabhängig davon, ob die Verbindung zum Senden oder empfangen verwendet werden. Beachten Sie, dass 100 vermittelte Verbindung kostenlos in einem grundlegenden Namespace. Dies ist auch die maximale Verbindungsanzahl vermittelten für Azure-Abonnements zulässig. Die ersten 1.000 vermittelten Verbindungen über alle Standard-Namespaces in Azure-Abonnement sind kostenlos (außer Grundgebühr) enthalten. Da diese Zertifikate zu viele Dienst-messaging Szenarien sind erst vermittelten Verbindungsgebühren normalerweise relevant, wenn Sie mit einer großen Anzahl von Clients AMQP oder HTTP Long Polling verwenden möchten; z. B. effizienter Ereignis streaming oder bidirektionale Kommunikation mit vielen Geräten oder Instanzen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Service Bus Preise finden Sie in [Azure Service Bus Preisseite](https://azure.microsoft.com/pricing/details/service-bus/).

- Allgemeine FAQs um Servicebus Preise und Abrechnung finden Sie unter [Häufig gestellte Fragen zu Service Bus](service-bus-faq.md#service-bus-pricing) .

[Azure-Verwaltungsportal]: http://manage.windowsazure.com
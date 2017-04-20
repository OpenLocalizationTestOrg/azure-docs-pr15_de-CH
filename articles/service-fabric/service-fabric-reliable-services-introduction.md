<properties
   pageTitle="Übersicht über zuverlässige Fabric-Dienst-Programmiermodell | Microsoft Azure"
   description="Service Fabric zuverlässig Programmiermodell lernen und eigene Dienste Schreiben beginnen."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Zuverlässige Dienste (Übersicht)
Azure Service Fabric vereinfacht das Schreiben und Verwalten von statusfreie und statusbehaftete zuverlässige Dienste. Dieses Dokument wird sprechen:

- Statusfreie und statusbehaftete Programmiermodell zuverlässige Dienste.
- Die Auswahlmöglichkeiten bei zuverlässig geschrieben haben.
- Einige Szenarien und Beispiele für zuverlässige Dienste verwenden und wie sie geschrieben werden.

Zuverlässige Dienste gehört zur Service Fabric Programmiermodelle. Informationen über zuverlässige Akteure Programmiermodell finden Sie unter [Einführung in Service Fabric zuverlässige Akteure](service-fabric-reliable-actors-introduction.md).

Fabric Service Dienst besteht aus Anwendungscode-Konfiguration und Status (optional).

Fabric-Dienst verwaltet die Lebensdauer der Dienste von Bereitstellung aktualisieren und löschen über [Fabric Service Application Management](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Was sind zuverlässige Dienste?
Zuverlässige Dienste bietet ein einfaches, leistungsstarkes und der obersten Ebene Programmiermodell zu express zur Anwendung kommt. Die zuverlässige Dienste Programmiermodell bietet:

- Statusbehaftete Dienste ermöglicht das zuverlässige Dienste Programmiermodell konsistent und zuverlässig speichern Ihre Zustand direkt in Ihren Dienst mit zuverlässigen Sammlungen. Dies ist eine einfache Gruppe hochverfügbare Auflistungsklassen vertraut werden, die C# Sammlungen verwendet hat. Traditionell erforderlich Services externer Systeme für zuverlässige Verwaltung. Mit zuverlässigen Sammlungen können Sie Ihren neben Ihrem Compute speichern, dieselbe hohe Verfügbarkeit und Zuverlässigkeit, die Sie uns erwarten von hochverfügbaren externe Speicher und zusätzliche Latenz verbessern, die Positionierung der Compute und bereitstellen.

- Ein einfaches Modell zum Ausführen eigener Codes ähnelt Programmiermodelle, verwendeten. Der Code enthält einen definierten Einstiegspunkt und einfach zu verwaltenden Lebenszyklus.

- Austauschbare Kommunikationsmodell. Verwenden Sie den Transport Ihrer Wahl HTTP [Web API](service-fabric-reliable-services-communication-webapi.md), WebSockets, benutzerdefinierten TCP-Protokolle. Zuverlässige Dienste bieten einige große der vordefinierten Optionen können oder Ihren eigenen bereitstellen.

## <a name="what-makes-reliable-services-different"></a>Zuverlässige Dienste macht?
Zuverlässige Dienste Service Fabric unterscheidet sich von Dienstleistungen, die Sie vor geschrieben haben. Service Fabric bietet Zuverlässigkeit, Verfügbarkeit, Konsistenz und skalierbar.  

- **Zuverlässigkeit**– Ihr Dienst auch in unzuverlässig, Ihr Computer möglicherweise fehl oder Netzwerkprobleme Treffer, bleiben.

- **Verfügbarkeit**der Dienst werden erreichbar und reagieren. (Dies bedeutet, dass Dienste haben kann, die nicht gefunden oder vom außerhalb.)

- **Skalierbar**– Dienste sind spezielle Hardware entkoppelt und sie vergrößert oder verkleinert werden nach Bedarf durch Hinzufügen oder Entfernen von Hardware oder virtuellen Ressourcen. Dienste werden (insbesondere bei statusbehafteten) problemlos partitioniert, unabhängige Teile des Diensts können skaliert und einzeln auf Fehler reagieren. Schließlich legt Service Fabric Services zu leicht Tausende von Diensten innerhalb eines einzelnen Prozesses bereitgestellt werden anstatt dass oder eine Instanz einer bestimmten Arbeitslast ganze Betriebssysteminstanzen zugewiesen.

- **Konsistenz**– in diesem Dienst gespeicherten Informationen garantiert Konsistenz (gilt nur für statusbehaftete - mehr dazu später)

## <a name="service-lifecycle"></a>Service-Lebenszyklus
Ob Ihr Dienst statusbehaftet oder statusfrei ist, bieten zuverlässige Dienste einen einfachen Lebenszyklus, mit dem Sie Code schnell anschließen und loslegen.  Es gibt nur ein oder zwei Methoden, die Sie implementieren, um den Dienst laufen.

- **CreateServiceReplicaListeners-CreateServiceInstanceListeners** -, in dem der Dienst Kommunikationsstapel definiert, die er verwenden möchte. Kommunikationsstapel wie [Web-API](service-fabric-reliable-services-communication-webapi.md)wird definiert abhörende Endpunkt oder Endpunkte für den Dienst (wie Clients es erreicht werden). Darüber hinaus werden wie die Nachrichten am Ende mit der Servicecode.

- **RunAsync** - Dies ist dem Ihr Dienst die Geschäftslogik ausgeführt wird. Bereitgestellte Abbruchtokens ist ein Signal bei dieser Arbeit beendet werden soll. Beispielsweise haben Sie einen Dienst, der ständig Nachrichten aus einer zuverlässigen Warteschlange und verarbeiten muss, ist dies, wo diese Arbeit wäre.

### <a name="service-startup"></a>Dienst starten

Die wichtigsten Ereignisse im Lebenszyklus von zuverlässig sind:

1. Das Objekt (was des statusfreien oder statusbehafteten Diensts abgeleitet) wird erstellt.

2. Die `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` -Methode aufgerufen wird, die Möglichkeit des Diensts mindestens kommunikationslistener seiner Wahl zurückgegeben.
  - Beachten Sie, dass dies optional, obwohl die meisten Dienste einige Endpunkt direkt verfügbar.

3. Sobald kommunikationslistener erstellt wurden, wird es geöffnet.
  - Kommunikation haben eine Methode namens `OpenAsync()`, die zu diesem Zeitpunkt aufgerufen und gibt die listening-Adresse für den Dienst. Wenn Ihnen zuverlässige eines integrierten Icommunicationlistener verwendet, erfolgt dies für Sie.

4. Nachdem der kommunikationslistener geöffnet ist die `RunAsync()` auf dem Hauptdienst wird aufgerufen.
  - Beachten Sie, dass `RunAsync()` ist optional. Wenn der Dienst widmet seine Arbeit direkt auf Benutzer nur ruft besteht keine Notwendigkeit, implementieren `RunAsync()`.

### <a name="service-shutdown"></a>Herunterfahren

Wenn der Dienst heruntergefahren wird (um, aktualisierten oder verschobene gelöscht) spiegelt die Reihenfolge: zuerst das Abbruchtoken frei `RunAsync()` abgebrochen. dann `CloseAsync()` für die kommunikationslistener aufgerufen.

Es gibt einige wichtige Dinge zu Herunterfahren für statusbehaftete Dienste:

- Service Fabric fördert nicht einem anderen Replikat des Dienstes primäre Status bis `CloseAsync` und `RunAsync` zurückgegeben. Verwenden Sie integrierte kommunikationslistener, die `CloseAsync` -Methode für Sie behandelt.

- Während kein Zeitlimit ist auf diese Methoden sofort nicht mehr zuverlässig Sammlungen schreiben und wirkliche Arbeit deshalb kann nicht abgeschlossen werden. Es wird empfohlen, so schnell wie möglich nach Erhalt der Stornierung zurück.

## <a name="example-services"></a>Beispiel-services
Dieses Programmiermodell kennen, nehmen wir einen Blick auf zwei verschiedene Dienste zu sehen, wie diese Teile zusammenpassen.

### <a name="stateless-reliable-services"></a>Statusfreie zuverlässige Dienste
Ein statusfreier Service ist wo kein Status innerhalb des Dienstes ist oder der Zustand vorhanden ist vollständig und erfordert Synchronisierung, Replikation, Dauerhaftigkeit und hohe Verfügbarkeit.

Angenommen Sie, einen Rechner, der kein Speicher und empfängt alle Begriffe und Vorgänge gleichzeitig ausführen.

RunAsync() des Dienstes können in diesem Fall leer sein, da gibt es keine Task-Hintergrundprozesse, die der Dienst tun. Beim Erstellen der rechnerdienst wird ein ICommunicationListener (z. B. [Web-API](service-fabric-reliable-services-communication-webapi.md)) zurückgegeben, der abhörende Endpunkt auf einige Ports geöffnet. Diese abhörende Endpunkt an die verschiedenen Methoden verknüpfen (Beispiel: "Hinzufügen (n1, n2)"), die öffentliche API des Rechners definieren.

Wenn von einem Client aufgerufen wird, die entsprechende Methode aufgerufen und rechnerdienst führt die Operationen auf Daten und gibt das Ergebnis zurück. Es speichern keinen Zustand.

Keine internen Status speichern macht dieses Beispiel Rechner sehr einfach. Aber die meisten Dienste sind nicht wirklich statusfrei. Stattdessen externalisieren sie ihren Zustand in einem anderen Speicher. (Beispielsweise ist jede Web-app basiert auf der Sitzungszustand in einem Sicherungsspeicher oder Cache nicht vollständig statusfreien.)

Ein gängiges Beispiel für Verwendung zustandsloser Dienste in Service Fabric ist ein Front-End, das die öffentliche API für eine Anwendung verfügbar macht. Der Front-End-Dienst kommuniziert dann mit statusbehaftete Dienste vollständig ein. In diesem Fall Aufrufe von Clients an einen bekannten Port wie 80 richten, der statusfreie Dienst abhört. Diese statusfreie Dienst den Aufruf erhält und bestimmt, ob der Aufruf von einer vertrauenswürdigen Partei als sowie der Service für bestimmt ist.  Statusfreie Service dann leitet den Anruf an die richtige Partition der statusbehaftete Dienst und wartet auf eine Antwort. Bei der statusfreie Dienst eine Antwort erhält, antwortet er zurück an den ursprünglichen Client.

### <a name="stateful-reliable-services"></a>Statusbehaftete zuverlässige Dienste
Statusbehafteter Dienst gehört, die einen Teil des Staates Funktion konsistent und Bestellung für den Dienst gespeichert. Berücksichtigen Sie einen Dienst, der ständig berechnet Durchschnitt einige Updates erhaltenen Wert. Dazu müssen sie den aktuellen Satz von Anfragen Prozess sowie den aktuellen Durchschnitt muss. Jedem Dienst, der abgerufen, verarbeitet und speichert Informationen in einem externen Speicher (z. B. ein Azure Blob oder Tabelle Store) ist statusbehaftet. Hält nur den Status im externen Statusspeicher.

Die meisten Dienste speichern heute Zustand, da der externe Speicher was ist Zuverlässigkeit, Verfügbarkeit, skalierbar und Konsistenz für diesen Zustand. Statusbehaftete Dienste müssen nicht in Service Fabric ihren Zustand extern speichern. Service Fabric übernimmt diese Vorschriften für den Code und den Dienststatus.

Angenommen, es soll einen Dienst, der akzeptiert Anfragen für eine Reihe von Umwandlungen, die für ein Bild und das Bild konvertiert werden müssen.  Bei diesem Service zurückgegeben es kommunikationslistener (angenommen, Web-API), die einen Kommunikationsanschluss wird geöffnet und ermöglicht Übermittlungen über eine API wie `ConvertImage(Image i, IList<Conversion> conversions)`. In dieser API konnte der Dienst die Informationen die Anforderung in eine zuverlässige Warteschlange speichern und dann ein Token an den Client zurückgegeben, sodass es die Anforderung mitverfolgen kann (da die Anfragen einige Zeit dauern).

In diesem Dienst möglicherweise komplexer RunAsync. Der Dienst möglicherweise eine Schleife in der RunAsync, die Anfragen aus IReliableQueue abruft, führt die Konvertierung aufgeführt und speichert die Ergebnisse in IReliableDictionary, damit, wenn der Client zurückkehrt, ihre Bilder erhalten können. Um sicherzustellen, dass bei Ausfall ein Bild nicht verloren, zuverlässigen Service die Konvertierung durchführen, ziehen Sie aus der Warteschlange und Speichern der Ergebnisse in einer Transaktion würde. In diesem Fall die Nachricht tatsächlich nur aus der Warteschlange entfernt, und die Ergebnisse werden im Ergebnis Wörterbuch gespeichert, wenn die Konvertierung abgeschlossen ist. Fällt etwas in der Mitte (wie der Computer, den diese Instanz des Codes ausgeführt wird), bleibt die Anforderung in der Warteschlange erneut aufbereitet werden.

Zu diesem Dienst ist klingt wie eine normale .NET. Der einzige Unterschied ist verwendeten Datenstrukturen (IReliableQueue und IReliableDictionary) Service Fabric bereitgestellt und daher sind sehr zuverlässig, verfügbar und konsistent.

## <a name="when-to-use-reliable-services-apis"></a>Zuverlässige Dienste APIs verwenden
Wenn eines der folgenden Service-Bedarf der Anwendung kennzeichnen, sollten Sie APIs zuverlässige Dienste berücksichtigen:

- Sie müssen Anwendung auf mehrere Einheiten (z.B. Aufträgen und Auftragspositionen) bereitzustellen.

- Der Anwendungszustand kann natürlich als zuverlässige Wörterbücher und Warteschlangen modelliert werden.

- Der Zustand muss mit hoher Verfügbarkeit mit geringer Latenz.

- Die Anwendung muss über mindestens eine zuverlässige Sammlungen Parallelität oder Genauigkeit der transaktiven Vorgängen steuern.

- Sie möchten die Kommunikation verwalten oder steuern das Partitionierungsschema für Ihren Dienst.

- Benötigt Ihr Code eine Freethread-Runtime-Umgebung.

- Die Anwendung muss dynamisch erstellt oder löscht zuverlässig Wörterbücher oder Warteschlangen zur Laufzeit.

- Sie müssen programmgesteuert steuern bereitgestellter Service Fabric Backup und restore-Funktionen für Ihren Diensts Zustand *.

- Die Anwendung muss für die Einheiten von Zustand * Änderungsprotokoll beibehalten.

- Sie möchten entwickeln oder dritte Partei entwickelten benutzerdefinierten Zustand Anbieter *.

> [AZURE.NOTE] * Bei allgemeiner Verfügbarkeit SDK verfügbaren Funktionen.


## <a name="next-steps"></a>Nächste Schritte
+ [Zuverlässige Dienste Schnellstart](service-fabric-reliable-services-quick-start.md)
+ [Zuverlässige Dienste fortgeschrittene Verwendung](service-fabric-reliable-services-advanced-usage.md)
+ [Das Programmiermodell zuverlässige Akteure](service-fabric-reliable-actors-introduction.md)

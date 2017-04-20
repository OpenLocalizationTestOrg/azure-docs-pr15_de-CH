<properties
   pageTitle="Definieren und Verwalten von Status | Microsoft Azure"
   description="Das definieren und verwalten Dienststatus in Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="service-state"></a>Dienststatus
**Dienststatus** bezieht sich auf die Daten, die der Dienst erforderlich. Sie enthält Datenstrukturen und Variablen, die der Dienst liest und schreibt funktionieren.

Betrachten Sie beispielsweise einen einfachen Rechner-Dienst. Dieser Dienst nimmt zwei Zahlen und gibt die Summe zurück. Dies ist ein rein statusfreie Dienst, der keine zugeordnet Daten.

Betrachten Sie nun den gleichen Rechner, aber neben computing Summe hat auch eine Methode zum Zurückgeben der letzten Summe berechnet wurde. Dieser Dienst ist nun Stateful - Zustand, die darin in geschrieben (wenn es eine neue Summe berechnet) und liest (wenn die zuletzt berechnete Summe gibt).

In Azure Service Fabric heißt der erste Dienst einen statusfreien Service. Der zweite Dienst ist einen statusbehafteten Dienst bezeichnet.

## <a name="storing-service-state"></a>Dienststatus speichern
Status werden ausgelagert oder wie der Code, der den Status bearbeiten. Auslagerung Zustand erfolgt normalerweise mit einer externen Datenbank oder Speicher. In unserem Beispiel könnte dies eine SQL-Datenbank sein, in der das aktuelle Ergebnis in einer Tabelle gespeichert ist. Jede Anforderung an die Summe berechnen führt ein Update für diese Zeile.

Status kann auch mit dem Code sein, die diesen Code verändert. Statusbehaftete Dienste Service Fabric mit diesem Modell basieren. Service Fabric bietet Infrastruktur sicherstellen, dass dieser Status hohe Verfügbarkeit und Fehlertoleranz bei einem Ausfall.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Service Fabric-Konzepte finden Sie unter:

- [Verfügbarkeit von Service Fabric-services](service-fabric-availability-services.md)

- [Skalierbar von Service Fabric-services](service-fabric-concepts-scalability.md)

- [Partitionierung Service Fabric-services](service-fabric-concepts-partitioning.md)

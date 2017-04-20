<properties
 pageTitle="IoT Hub HA und DR | Microsoft Azure"
 description="Beschreibt Funktionen für hochverfügbare IoT Lösungen mit Disaster Recovery-Funktionen."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub hohe Verfügbarkeit und Disaster recovery

IoT Hub bietet ein Azure Service hohen Verfügbarkeit (HA) Redundanzen Ebene Azure Bereich ohne die Lösung erforderlich. Azure bietet eine Reihe von Features, mit denen ggf. Cross-Region Verfügbarkeit und Disaster Recovery (DR) Funktionen erläuterten. Sie entwerfen und Ihre Lösungen nutzen DR-Funktionen Wenn Sie global bereitstellen möchten, Cross-Region hohe Verfügbarkeit für Geräte oder Benutzer. Artikel [Azure Business Continuity technische Anleitung](../resiliency/resiliency-technical-guidance.md) beschreibt die Funktionen in Azure für Business Continuity und Disaster Recovery. [Disaster Recovery und hohe Verfügbarkeit für Azure Applications][] Papier erfahren Sie Architektur Strategien für Azure Applications HA und DR.

## <a name="azure-iot-hub-dr"></a>Azure IoT Hub DR
Neben Intra-Region HA implementiert IoT Hub Failovermechanismen für Disaster Recovery, die kein des Benutzers eingreifen. IoT Hub DR selbst initiiert und hat eine Recovery Time Objective (RTO) 2-26 Stunden und die folgenden Recovery Point Objectives (RPO).

| Funktionen | RPO |
| ------------- | --- |
| Service-Verfügbarkeit für Registrierung und Kommunikation | CName-Verlust |
| Identitätsdaten in geräteidentitätsregistrierung | 0-5 Minuten Datenverlust |
| Gerät-zu-Cloud-Nachrichten | Alle ungelesene Nachrichten verloren. |
| Nachrichten Überwachen von Operationen | Alle ungelesene Nachrichten verloren. |
| Cloud-zu-Gerät-Nachrichten | 0-5 Minuten Datenverlust |
| Cloud-zu-Gerät-Feedback-Warteschlange | Alle ungelesene Nachrichten verloren. |

## <a name="regional-failover-with-iot-hub"></a>Regionale Failover mit IoT Hub

Eine ausführliche Erläuterung von Bereitstellungstopologien IoT Solutions ist außerhalb des Bereichs dieses Artikels, aber um hohe Verfügbarkeit und Disaster Recovery wir *regionale Failover* -Bereitstellungsmodell berücksichtigt.

In einem Modell regionalen Failover Backend Lösung hauptsächlich in einem Datencenter-Standort ausgeführt wird, jedoch zusätzliche IoT Hub als Back-End an einem anderen Rechenzentrum Zwecken Failover bereitgestellt werden, IoT Hub im primären Rechenzentrum leidet einen Ausfall oder die Netzwerkkonnektivität vom Gerät zum primären Datencenter irgendwie unterbrochen. Geräte verwenden sekundäre Dienstendpunkt primäre Gateway erreicht werden kann. Mit einem Kreuz Region Failover kann die Verfügbarkeit bei Lösung über die hohe Verfügbarkeit einer Region verbessert werden.

Auf einer hohen Ebene um regionale Failover-Modell mit IoT Hub implementieren benötigen Sie.

* **Sekundäre IoT Hub- and -Gerät die Routinglogik**: bei einer serviceunterbrechung Ihre primäre Region müssen Geräte gestartet, sekundäre Region an. Angesichts der-Unterstützung die meisten Dienste beteiligt sind, gilt die Lösung Administratoren die Inter-Region Failover auszulösen. Am besten neuen Endpunkt Geräte kommunizieren und gleichzeitig die Kontrolle des Prozesses, wird Sie regelmäßig einen *Concierge* -Dienst für den aktuellen aktiven Endpunkt. Concierge-Dienst kann eine einfache Anwendung, die ist repliziert und erreichbar DNS-Umleitung Techniken (z. B. [Azure Traffic Manager][]).
* **Registrierungsreplikation Identity** - nutzbaren zu sekundären IoT Hub alle Geräte Identitäten enthalten muss, die auf die Lösung zugreifen können. Die Lösung sollte geografisch repliziert Backups Gerät Identitäten beibehalten und sekundäre IoT Hub hochladen, schalten Sie den aktiven Endpunkt für die Geräte. Export-Funktion des Geräts Identität IoT Hub ist dabei sehr hilfreich. Weitere Informationen finden Sie im [Entwicklerhandbuch für IoT Hub - identitätsregistrierung][].
* **Zusammenführen von Logik** - Wenn die primäre Region wieder verfügbar ist, den Status und die Daten in den sekundären Standort erstellten migriert werden müssen, primäre Region zurück. Dies betrifft hauptsächlich Gerät Identitäten und Meta-Anwendungsdaten primären IoT Hub und andere anwendungsspezifische Informationsspeicher im Bereich für primäre zusammengeführt werden müssen. Um diesen Schritt zu vereinfachen, wird normalerweise empfohlen Idempotent Operationen verwenden. Nebenwirkungen nicht aus tatsächlichen konsistente Ereignisse, nur Duplikate oder außerhalb der Reihenfolge Senden von Ereignissen wird minimiert. Darüber hinaus sollte die Anwendungslogik soll potenzielle Inkonsistenzen oder "leicht" aus Datum tolerieren. Dies aufgrund der zusätzlichen Zeitaufwand für das System "heilen" Recovery Point Objectives (RPO) basiert.

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie diesen Links, um weitere Informationen zu Azure IoT Hub:

- [Erste Schritte mit IoT Hubs (Lernprogramm)][lnk-get-started]
- [Was ist Azure IoT Hub?][]

[Disaster Recovery und hohe Verfügbarkeit für Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[Entwicklerhandbuch IoT Hub - identitätsregistrierung]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Was ist Azure IoT Hub?]: iot-hub-what-is-iot-hub.md

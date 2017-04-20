<properties
    pageTitle="Traffic Manager - Datenverkehr Verteilermethoden | Microsoft Azure"
    description="Dieser Artikel hilft Ihnen die verschiedenen Verteilermethoden verwendet von Traffic Manager zu verstehen"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Traffic Manager Streckenführung Methoden

Azure Traffic Manager unterstützt drei Datenverkehrs-routing Informationen zum Netzwerkverkehr an die verschiedenen Dienstendpunkte weiterleiten. Traffic Manager wendet die Weiterleitung von Datenverkehr-Methode allen DNS-Abfragen, die er erhält. Die Methode Datenverkehr routing bestimmt der Endpunkt in der DNS-Antwort zurückgegeben.

Azure-Ressourcen-Manager Unterstützung für Traffic Manager verwendet andere Terminologie als klassische Bereitstellungsmodell. Die folgende Tabelle zeigt die Unterschiede zwischen den Ressourcen-Manager und Classic:

| Ressourcenmanager-Begriff | Klassische Begriff |
|-----------------------|--------------|
| Streckenführung Methode | Netzwerklastenausgleich Methode |
| Priorität-Methode | Failover-Methode |
| Gewichtete Methode | Round-Robin-Methode |
| Performance-Methode | Performance-Methode |

Basierend auf Feedback von Kunden, haben wir die Terminologie zu verbessern die Klarheit Häufige Missverständnisse verringern. Es gibt keinen Unterschied in der Funktionalität.

Es stehen drei Datenverkehr Verteilermethoden in Traffic Manager:

- **Priorität:** Wählen Sie 'Priority', wenn Sie einen primären Endpunkt für den gesamten Datenverkehr verwenden und Backups bei primären backup Endpunkte nicht verfügbar sind.
- **Gewichtet:** Wählen "gewichteten" Wenn Endpunkte Datenverkehr verteilt werden soll, definieren entweder gleichmäßig oder Gewicht
- **Leistung:** Wählen Sie "Leistung", wenn Sie Endpunkte an unterschiedlichen geographischen Standorten und Endbenutzer "nächstliegende" Endpunkt in der niedrigsten Netzwerklatenz verwendet werden soll.

Alle Traffic Manager-Profile sind Endpunkt Gesundheits- und automatische Endpunkt Failover. Weitere Informationen finden Sie unter [Traffic Manager Endpunkt überwachen](traffic-manager-monitoring.md). Ein einzelnes Traffic Manager-Profil können nur eine Datenverkehr Routingmethode. Sie können jederzeit eine unterschiedliche Daten Routingmethode für Ihr Profil auswählen. Änderungen innerhalb einer Minute und ohne Ausfallzeiten entstehen. Datenverkehr weiterleiten können kombiniert werden mit geschachtelten Traffic Manager. Schachtelung ermöglicht anspruchsvolle und flexible Datenverkehr weiterleiten Konfigurationen, die den Bedürfnissen der größere, komplexe Clientanwendungen. Weitere Informationen finden Sie unter [geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Priorität Streckenführung Methode

Oft will eine Organisation Zuverlässigkeit für seine Dienste durch eine oder mehrere backup-Services bereitstellen, bei ihren primäre Dienst ausfällt. 'Priority' Streckenführung Methode ermöglicht Azure Kunden dieses Muster Failover problemlos implementieren.

![Azure Traffic Manager 'Priority' Streckenführung Methode][1]

Traffic Manager-Profil enthält eine priorisierte Liste der Endpunkte. Standardmäßig sendet Traffic Manager alle Verkehr an den primären (höchste Priorität). Wenn die primären nicht verfügbar ist, leitet Traffic Manager den Datenverkehr an den zweiten Endpunkt. Wenn die primären und sekundären Endpunkte nicht verfügbar sind, wird der Datenverkehr an Dritte usw.. Verfügbarkeit des Endpunkts anhand der konfigurierten Status (aktiviert oder deaktiviert) und der laufenden Endpunkt.

### <a name="configuring-endpoints"></a>Konfigurieren von Endpunkten

Mit Azure-Ressourcen-Manager konfigurieren Endpunkt Priorität explizit mit 'Priority'-Eigenschaft für jeden Endpunkt. Diese Eigenschaft ist ein Wert zwischen 1 und 1000. Niedrigere Werte stellen eine höhere Priorität. Prioritätswerte gemeinsam nicht verwenden. Die Eigenschaft ist optional. Wenn nicht angegeben, wird standardmäßig die Priorität basierend auf der Reihenfolge Endpunkt verwendet.

Mit der Oberfläche Classic ist implizit Endpunkt Priorität konfiguriert. Die Priorität basiert auf der Reihenfolge, in der Endpunkte in der Profildefinition aufgeführt sind.

## <a name="weighted-traffic-routing-method"></a>Gewichtete Streckenführung Methode

Gewichtete"Streckenführung Methode können Sie Verkehr gleichmäßig zu verteilen oder eine vordefinierte Gewichtung.

![Azure Traffic Manager gewichteten"Streckenführung Methode][2]

Gewichtete Datenverkehr weiterleiten-Methode wird jeder Endpunkt Profilkonfiguration Traffic Manager eine Gewichtung zuweisen. Das Gewicht ist eine Ganzzahl zwischen 1 und 1000. Dieser Parameter ist optional. Wenn nicht angegeben, verwendet Traffic Manager Standardgewicht '1'.

Traffic Manager wählt für jedes empfangene DNS-Abfragen nach dem Zufallsprinzip einen Endpunkt. Die Wahrscheinlichkeit, einen Endpunkt basiert auf der alle verfügbaren Endpunkte zugeordnet. Dasselbe Gewicht verwenden über alle Endpunkte führt eine datenverkehrsverteilung auch. Höhere oder niedrigere Gewichtung wird auf bestimmte Endpunkte diese Endpunkte mehr oder weniger häufig in die DNS-Antworten zurückgegeben werden.

Der gewichtete-Methode können einige Szenarien nützlichen:

- Schrittweise anwendungsaktualisierung: Prozentsatz der Datenverkehr zu einem neuen Endpunkt weiterleiten reservieren und erhöhen den Datenverkehr auf 100 %.
- Anwendungsmigration in Azure: Profil mit Azure und externe Endpunkte erstellen. Passen Sie das Gewicht der Endpunkte neue Endpunkte bevorzugt an
- Für zusätzliche Kapazität Cloud trennen: eine lokale Bereitstellung in der Cloud schnell einfügen hinter einem Traffic Manager erweitern. Wenn Sie zusätzliche Kapazität in der Cloud benötigen, können hinzufügen oder weitere Endpunkte aktivieren und angeben, welcher Teil des Datenverkehrs an jedem Endpunkt geht.

Neue Azure-Portal unterstützt die Konfiguration der gewichteten Datenverkehr weiterleiten. Gewicht können im klassischen Portal konfiguriert werden. Sie können auch mit dem Ressourcen-Manager und klassischen Versionen des Azure PowerShell, CLI und den REST-APIs Gewicht konfigurieren.

Es ist wichtig zu verstehen, dass DNS-Antworten zwischengespeichert werden, Clients und rekursive DNS-Server, mit denen Clients DNS-Namen auflösen. Dieses Zwischenspeichern kann auf gewichteten Datenverkehr Distributionen auswirken. Wird die Anzahl der Clients und rekursive DNS-Server große erwartungsgemäß datenverkehrsverteilung. Allerdings wird die Anzahl der Kunden oder rekursive DNS-Servern kleine kann Zwischenspeichern erheblich datenverkehrsverteilung verzerren.

Übliche Anwendungsbeispiele umfassen:

- Entwicklung und testing
- Anwendung-Kommunikation
- Anwendung abzielen, eine schmale Benutzergemeinschaft, die allgemeine rekursive DNS-Infrastruktur (z. B. Mitarbeiter des Unternehmens über Proxy verbinden)

Diese DNS-caching-Effekte sind für alle DNS-basierte verkehrsleitsysteme, nicht nur Azure Traffic Manager. In einigen Fällen können explizit Löschen des DNS-Caches umgehen. In anderen Fällen kann Datenverkehr routing Alternativ besser geeignet sein.

## <a name="performance-traffic-routing-method"></a>Leistung Streckenführung Methode

Bereitstellen von Endpunkten in zwei oder mehreren Standorten weltweit kann die Reaktionszeit der vielen durch routing des Datenverkehrs an den Speicherort verbessern 'am nächsten'. "Leistung" Streckenführung Methode bietet diese Möglichkeit.

![Azure Traffic Manager "Leistung" Streckenführung Methode][3]

'Am nächsten' Endpunkt liegt nicht notwendigerweise geografische Entfernung gemessen. Stattdessen bestimmt die "Leistung" Datenverkehr weiterleiten den nächsten Endpunkt durch Netzwerklatenz messen. Traffic Manager verwaltet ein Internet Wartezeit Tabelle die Roundtripzeit zwischen IP-Adressbereiche und jede Azure-Rechenzentrum.

Traffic Manager sucht die IP-Quelladresse der eingehenden DNS-Anforderung im Internet Wartezeit Tabelle. Traffic Manager wählt einen Endpunkt in der Azure-Rechenzentrum, die niedrigste Latenz, IP-Adressbereich ist diesen Endpunkt in der DNS-Antwort zurückgegeben.

Wie beschrieben unter [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md)erhält Traffic Manager DNS-Abfragen direkt von Clients. DNS-Abfragen stammen vielmehr rekursiven DNS-Dienst, dass die Clients konfiguriert sind. Daher verwendet die IP-Adresse bestimmen 'am nächsten' Endpunkt ist nicht die IP-Adresse des Clients, aber es ist die IP-Adresse der rekursiven DNS-Dienst. In der Praxis ist diese IP-Adresse ein guter Indikator für den Client.

Traffic Manager aktualisiert Internet Wartezeit Tabelle globalen Internet und neue Azure Regionen berücksichtigt. Allerdings hängt Anwendungsperformance Echtzeit Variationen im Laden über das Internet. Performance-Streckenführung überwacht Last auf einen bestimmten Endpunkt nicht. Wenn ein Endpunkt verfügbar, jedoch Traffic Manager nicht in Antworten auf DNS-Abfragen.

Punkte zu beachten:

- Enthält Ihr Profil mehrere Endpunkte in derselben Azure-Region verteilt dann Traffic Manager Verkehr gleichmäßig auf die verfügbaren Endpunkte in diesem Bereich. Auf Wunsch eine anderen datenverkehrsverteilung innerhalb eines Bereichs können Sie [geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

- Wenn alle aktivierten Endpunkte in einer bestimmten Region Azure beschädigt sind, verteilt Traffic Manager Verkehr auf anderen verfügbaren Endpunkte statt weiter am nächsten Endpunkt. Diese Logik verhindert kaskadierenden Fehlers durch überladen nicht weiter am nächsten Endpunkt. Definieren eine bevorzugten Failover-Sequenz, verwenden Sie [verschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

- Bei der Leistung Datenverkehr Routingmethode mit externer Endpunkte oder geschachtelte Endpunkte müssen Sie den Speicherort der diese Endpunkte angeben. Wählen Sie Ihre Azure Region der Bereitstellung. Diese Speicherorte sind vom Internet Wartezeit Tabelle unterstützt.

- Der Algorithmus, der den Endpunkt wählt ist deterministisch. Wiederholte DNS-Abfragen vom selben Client werden an dieselbe IP-Adresse weitergeleitet. Normalerweise verwenden Clients unterschiedliche rekursiven DNS-Servern unterwegs. Der Client kann einen anderen Endpunkt weitergeleitet werden. Routing kann auch Updates für Internet Wartezeit Tabelle betroffen sein. Daher unbedingt Leistung Streckenführung Methode kein Client immer dieselbe IP-Adresse weitergeleitet werden.

- Wechselt Internet Wartezeit Tabelle bemerken Sie, dass einige Kunden zu einem anderen Endpunkt weitergeleitet werden. Dazu routing ist genauer basierend auf aktuellen Daten zur Replikationswartezeit. Diese Updates sind für die Leistung Routing von Datenverkehr Genauigkeit wie das Internet ständig weiterentwickelt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Anwendungsentwicklung Hochverfügbarkeits- [Traffic Manager Endpunkt](traffic-manager-monitoring.md) Überwachung

Informationen zum [Erstellen eines Profils Traffic Manager](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png

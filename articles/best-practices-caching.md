<properties
   pageTitle="Zwischenspeichern Hinweise | Microsoft Azure"
   description="Hinweise zur Verbesserung der Leistung und Erweiterbarkeit Zwischenspeichern."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Zwischenspeichern der Anleitung

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Zwischenspeichern ist eine gängige Technik, die Leistung und Skalierung eines Systems zu verbessern. Dazu kopieren vorübergehend genutzten Daten schnell Speicher nahe liegt der Anwendung. Ist diese schnelle Datenspeicher näher an die Anwendung als die ursprüngliche Quelle, verbessern dann caching erheblich Reaktionszeiten für Clientanwendungen mit Daten schneller.

Zwischenspeichern ist am effektivsten, wenn eine Clientinstanz wiederholt dieselben Daten liest besonders trifft die folgende ursprünglichen Datenspeicher:
- Relativ statisch bleibt.
- Es ist langsam, verglichen mit der Geschwindigkeit des Caches.
- Es ist eine hohe Konflikte.
- Es ist weit Netzwerklatenz auf langsam führen kann.

## <a name="caching-in-distributed-applications"></a>In verteilten Anwendung Zwischenspeichern

Verteilte Anwendung implementieren in der Regel eine oder beide der folgenden Strategien beim Zwischenspeichern von Daten:

- Verwenden privaten Cache, wo Daten lokal auf dem Computer stattfindet, die eine Instanz einer Anwendung oder Dienst ausgeführt wird.
- Verwenden von gemeinsam genutzten Cache als eine gemeinsame Quelle die von mehreren Prozessen oder Computern zugegriffen werden kann.

In beiden Fällen Zwischenspeichern von Client und Server erfolgt. Clientseitiges Zwischenspeichern des Prozesses erfolgt, die die Benutzeroberfläche für ein System oder einen Webbrowser desktop-Anwendung.
Serverseitige Zwischenspeichern der Prozess erfolgt, der die Geschäftsdienste anbietet, die Remote ausgeführt werden.

### <a name="private-caching"></a>Private Zwischenspeichern

Die einfachste Art der Cache ist ein Speicher. Es wurde in den Adressraum eines Prozesses und direkt von der in diesem Prozess ausgeführte Code zugegriffen. Dieser Cache ist sehr schnell zu erreichen. Außerdem liefert ein sehr effektives Mittel für vergleichsweise geringen Mengen statischer Daten speichern, da die Größe des Cache normalerweise Volumenprozent Speicher eingeschränkt, die auf dem Computer den Hostprozess verfügbar ist.

Benötigen Sie mehr Informationen als physisch im Arbeitsspeicher zwischenspeichern, können Sie das lokale Dateisystem zwischengespeicherte Daten schreiben. Langsamer Zugriff als Daten, die im Speicher gehalten wird dies sollte schneller und zuverlässiger als das Abrufen von Daten über ein Netzwerk.

Haben Sie mehrere Instanzen einer Anwendung, die dieses Modell ausgeführt wird, verfügt jede Anwendungsinstanz eigene unabhängigen Cache seine eigene Kopie der Daten.

Stellen Sie sich einen Cache als Snapshot der ursprünglichen Daten irgendwann in der Vergangenheit. Wenn diese Daten nicht statisch ist, ist wahrscheinlich, dass verschiedene Instanzen verschiedene Versionen der Daten in die Caches halten. Daher kann dieselbe Abfrage diese Instanzen durchgeführte andere Ergebnisse zurück, wie in Abbildung 1 dargestellt.

![Verwenden einen Speichercache in verschiedenen Instanzen einer Anwendung](media/best-practices-caching/Figure1.png)

_Abbildung 1: Verwenden eines speicherinternen Caches in verschiedenen Instanzen einer Anwendung_

### <a name="shared-caching"></a>Freigegebene Zwischenspeichern

Mit gemeinsam genutzten Cache kann lindern Bedenken Daten in jedem Cache, unterscheiden sich möglicherweise im Arbeitsspeicher Zwischenspeichern kann auftreten. Freigegebene Zwischenspeichern wird sichergestellt, dass verschiedene Instanzen der gleichen zwischengespeicherten Daten anzuzeigen. Dazu suchen Cache an einer anderen Stelle in der Regel als Teil einer separaten Dienst gehostet, wie in Abbildung 2 dargestellt.

![Verwendung von gemeinsam genutzten cache](media/best-practices-caching/Figure2.png)

_Abbildung 2: Gemeinsam genutzten Cache verwenden_

Ein wichtiger Vorteil der freigegebenen Zwischenspeichern Ansatz ist Erweiterbarkeit bietet. Viele gemeinsam genutzten Cache Services werden einem Cluster von Servern und Nutzung von Software, die Daten innerhalb des Clusters in transparenter Weise verteilt. Eine Instanz der Anwendung sendet eine Anforderung einfach an den Cachedienst.
Die zugrunde liegende Infrastruktur ist zuständig für das Ermitteln der Position der zwischengespeicherten Daten im Cluster. Den Cache kann einfach durch Hinzufügen weiterer Server skaliert werden.

Es gibt zwei Hauptnachteile des freigegebenen Zwischenspeichern Ansatz:
- Der Cache ist langsamer zugreifen, da für jede Anwendungsinstanz nicht mehr lokal gehalten wird.
- Die Anforderung zur Implementierung eines Dienstes separaten Cache möglicherweise Komplexität der Projektmappe hinzufügen.

## <a name="considerations-for-using-caching"></a>Aspekte der Verwendung zwischenspeichern

Den folgenden Abschnitten ausführlicher Aspekte zum Entwerfen und verwenden einen Cache.

### <a name="decide-when-to-cache-data"></a>Wann Sie Daten

Zwischenspeichern kann Leistung, Skalierbarkeit und Verfügbarkeit verbessern. Je mehr Daten Sie haben und je größer die Anzahl der Benutzer, die Zugriff auf diese Daten, desto größer die Vorteile des Zwischenspeicherns werden. Deshalb Zwischenspeichern reduziert die Latenz und Konflikte, die mit großen Datenmengen gleichzeitige Anfragen im ursprünglichen Datenspeicher zugeordnet ist.

Eine Datenbank unterstützen möglicherweise eine begrenzte Anzahl von gleichzeitigen Zugriffen. Abrufen von Daten aus einem gemeinsam genutzten Cache ermöglicht jedoch die zugrunde liegende Datenbank für eine Clientanwendung auf diese Daten zugreifen, selbst wenn die Anzahl der verfügbaren Verbindungen derzeit erschöpft ist. Darüber hinaus wird die Datenbank nicht verfügbar, Clientanwendungen möglicherweise weiterhin mit den Daten, die statt im Cache.

Betrachten Sie Zwischenspeichern häufig lesen jedoch selten (z. B. Daten, die einen höheren Anteil der Lesevorgänge als Schreibzugriffe) geändert. Jedoch empfohlen nicht, Cache als autorisierend Speicher wichtiger Informationen verwenden. Stellen Sie stattdessen sicher, dass alle Änderungen, die Ihre Anwendung nicht verlieren immer eine persistente Datenspeicher gespeichert werden. Dies bedeutet, dass der Cache nicht verfügbar ist, Ihre Anwendung kann weiterhin angewendet werden, indem der Datenspeicher und wichtige Informationen verlieren nicht.

### <a name="determine-how-to-cache-data-effectively"></a>Feststellen Sie, wie effektiv die Daten zwischenspeichern

Effektiv mit einem Cache Schlüssel bestimmen die am besten geeigneten Daten Cache und rechtzeitig Zwischenspeichern. Die Daten können für den Cache auf Nachfrage beim ersten Abrufen von einer Anwendung hinzugefügt werden. Dies bedeutet, dass die Anwendung nur einmal die Daten aus dem Datenspeicher abzurufen muss und diese nachfolgenden Zugriff mit dem Cache erfüllt werden.

Alternativ kann Cache teilweise oder vollständig mit Daten, in der Regel aufgefüllt werden beim Start der Anwendung (Ansatzes als seeding bezeichnet). Jedoch kann es nicht ratsam implementieren seeding für einen großen Cache, da dadurch eine plötzliche, hohe Auslastung im ursprünglichen Datenspeicher durchsetzen kann, beim Start der Anwendung ausgeführt.

Oft hilft eine Analyse der Verwendungsmuster Sie entscheiden, ob vollständig oder teilweise vorab Cache und Daten zum Cache auswählen. Beispielsweise kann es nützlich sein soll mit statischen Benutzerprofildaten für Kunden, die regelmäßig (z. B. täglich) Anwendung, aber nicht für Benutzer von der Anwendung nur einmal pro Woche.

In der Regel Zwischenspeichern funktioniert gut mit Daten unveränderlich oder selten geändert. Beispiele sind Informationen wie Produkt- und Preisinformationen in einer e-Commerce-Anwendung oder freigegebene statische Ressourcen zu erstellen. In den Cache beim Anwendungsstart Minimierung Nachfrage nach Ressourcen und Leistung können einige oder alle Daten geladen werden. Auch möglicherweise angemessen, Hintergrund, die regelmäßig Verweis Daten im Cache auf aktuell ist, dass den Cache aktualisiert beim Verweisen auf Daten ändert.

Zwischenspeichern ist weniger nützlich für dynamic Data Ausnahmen dieser Aspekt (siehe Abschnitt hochgradig dynamischen Cachedaten in diesem Artikel Weitere Informationen). Wenn die ursprünglichen Daten regelmäßig ändert, zwischengespeicherte Informationen sehr schnell überholt oder Aufwand von Cache mit den ursprünglichen Datenspeicher reduziert die Effektivität des Zwischenspeicherns.

Beachten Sie, dass ein Cache nicht die vollständigen Daten für eine Entität enthalten. Z. B. steht ein Datenelement eine mehrwertige Objekt wie eine Bank mit Namen, Adresse und Konto möglicherweise einige Elemente (wie Name und Adresse), statische bleiben und (wie der Kontostand) mehr dynamisch. Diese Situation kann für statische Teile der Daten zwischenzuspeichern und die restliche Daten abrufen (oder berechnen) bei.

Wir empfehlen Sie Leistungsanalyse testen und Verwendung auf Vorinstallation oder bei Bedarf laden Cache oder einer Kombination aus beiden entsprechenden durchführen. Die Entscheidung Volatilität und Verwendungsmuster der Daten beruhen. Cache-Auslastung und Performance-Analyse ist besonders wichtig, die Lasten auftreten und hochgradig skalierbar sein. Beispielsweise könnte in hochgradig skalierbaren Szenarien es Startwert für den Cache, um den Datenspeicher zu Spitzenzeiten entlasten sinnvoll.

Zwischenspeichern kann auch verwendet werden, zu wiederholten Berechnungen, während die Anwendung ausgeführt wird. Wenn ein Daten transformiert oder eine komplizierte Berechnung ausführt, können sie die Ergebnisse der Operation im Cache speichern. Wenn dieselbe Berechnung danach erforderlich ist, kann die Anwendung einfach die Ergebnisse aus dem Cache abrufen.

Eine Anwendung kann Daten ändern, die in einem Cache gespeichert. Wir empfehlen jedoch an den Cache als vorübergehende Datenspeicher, der jederzeit verschwinden konnte. Speichern Sie wichtige Daten nicht nur im Cache. Stellen Sie sicher, dass die Daten im ursprünglichen Datenspeicher sowie zu pflegen. Dadurch wird der Cache nicht verfügbar, das Risiko eines Datenverlustes minimieren.

### <a name="cache-highly-dynamic-data"></a>Hochgradig dynamischen Cache-Daten

Speichern von sich schnell ändernden Informationen in einem permanenten können als Aufwand auf dem System auferlegt werden. Angenommen Sie, ein Gerät, das ständig Status oder eine Maßeinheit meldet. Wenn eine Anwendung nicht auf diese Daten zwischenzuspeichern, werden die zwischengespeicherte Informationen fast immer veraltet, könnte dann desselben Betrags beim Speichern und Abrufen dieser Informationen aus dem Datenspeicher zutreffen. Zeit zum Speichern und Abrufen dieser Daten, es möglicherweise geändert.

Sollten Sie in einer Situation wie dieser die Vorteile von dynamischen Informationen direkt im Cache anstatt im persistenten Datenspeicher gespeichert. Wenn die Daten nicht kritisch und erfordert keine Überwachung, ist es egal, wenn die Änderung verloren geht.

### <a name="manage-data-expiration-in-a-cache"></a>Ablauf von Daten in einem Cache verwalten

In den meisten Fällen werden Daten im Cache gehalten werden eine Kopie der Daten, die in den ursprünglichen Datenspeicher gespeichert. Die Daten im ursprünglichen Datenspeicher möglicherweise ändern, nachdem sie die zwischengespeicherten Daten veralten zwischengespeichert wurde. Viele Zwischenspeichern Systeme können zum Konfigurieren des Caches Daten ablaufen, und verkürzen Sie die Aufbewahrungsdauer für die Daten möglicherweise nicht mehr aktuell.

Ablauf zwischengespeicherte Daten aus dem Cache entfernt wird und die Anwendung muss die Daten aus den ursprünglichen Datenspeicher (sie können die neu abgerufenen Informationen wieder Cache abzulegen) abrufen. Sie können eine Ablaufrichtlinie standardmäßig beim Konfigurieren des Caches festlegen. Viele Cache-Services können Sie auch festlegen die Gültigkeitsdauer für einzelne Objekte beim programmgesteuert im Cache speichern.
Einige Caches können Sie an die Gültigkeitsdauer als absoluten Wert oder als einen gleitenden Wert, bei dem das Element aus dem Cache entfernt werden, wenn er nicht innerhalb des angegebenen Zeitraums erfolgt. Diese Einstellung überschreibt jede Cache-Wide Ablaufrichtlinie, jedoch nur für die angegebenen Objekte.

> [AZURE.NOTE] Sollten Sie die Gültigkeitsdauer für den Cache und sorgfältig enthaltenen Objekte. Nehmen Sie es kurz, Objekte zu schnell läuft und reduziert die Vorteile der Verwendung des Caches. Nehmen Sie lange Zeit, riskieren Sie die Daten veralten.

Es kann auch der Cache möglicherweise voll Wenn Daten lange beibehalten. In diesem Fall möglicherweise Fragen Hinzufügen neuer Einträge im Cache einige Elemente so Entfernung zwangsweise entfernt werden. Cache Dienste normalerweise Daten zumindest zuletzt verwendet (LRU) entfernen, aber Sie normalerweise diese Richtlinie überschreiben und verhindern, dass Elemente entfernt werden. Wenn dieser Ansatz riskieren Sie überschreiten den Speicher im Cache verfügbar ist. Eine Anwendung, die versucht, ein Element zum Cache hinzufügen wird mit einer Ausnahme fehlschlagen.

Einige Zwischenspeichern Implementierung können zusätzliche Auslagerungsrichtlinien bereitstellen. Es gibt verschiedene Auslagerungsrichtlinien. Dazu gehören:
- Eine zuletzt verwendete Richtlinie (in der Annahme, dass die Daten nicht erneut erforderlich ist).
- Eine Richtlinie für First in First Out (ältesten zuerst entfernt).
- Eine explizite entfernen Politik ausgelöste Ereignis (beispielsweise geändert wird).

### <a name="invalidate-data-in-a-client-side-cache"></a>Daten in einem clientseitigen Cache ungültig

Daten, die im clientseitigen Cache reserviert gilt außerhalb der Schirmherrschaft des Dienstes sein, die Daten an dem Client bereitstellt. Ein Dienst kann nicht direkt einen Client hinzufügen oder Entfernen von Informationen aus einem clientseitigen Cache erzwingen.

Daher ist es für einen Client, die eine schlecht konfigurierte weiterhin veralteten Informationen verwendet. Beispielsweise kann die Ablaufrichtlinien Cache ordnungsgemäß implementiert werden kein Client veralteten Informationen verwenden, der lokal zwischengespeichert werden, wenn die Informationen in der ursprünglichen Datenquelle geändert hat.

Wenn Sie eine Anwendung, die Daten über eine HTTP-Verbindung dient erstellen, können Sie einen Webclient (wie ein Browser oder Webproxy) implizit erzwingen, aktuelle Informationen abrufen. Dies ist möglich, wenn eine Ressource durch eine Änderung in der URI der Ressource, die aktualisiert wird. Webclients verwenden in der Regel den URI einer Ressource als Schlüssel im clientseitigen Cache, damit ändert der URI der Webclient ignoriert zuvor zwischengespeicherte Versionen einer Ressource und stattdessen die neue Version abgerufen.

## <a name="managing-concurrency-in-a-cache"></a>Verwalten von Parallelität im cache

Caches dienen häufig mehrere Instanzen einer Anwendung freigegeben werden. Jede Anwendungsinstanz lesen und Ändern von Daten im Cache. Der Fehler, die mit gemeinsamen Datenspeicher gelten daher auch für Cache. In einer Situation benötigt eine Anwendung zum Ändern von Daten im Cache gehalten wird, müssen Sie sicherstellen, dass Aktualisierungen durch eine Instanz der Anwendung keine Änderung von einer anderen Instanz überschreiben.

Je nach der Art der Daten und die Wahrscheinlichkeit von Kollisionen können Sie zwei Ansätze, Parallelität übernehmen:

- __Optimistisch.__ Unmittelbar vor dem Aktualisieren der Daten, überprüft die Anwendung, ob die Daten im Cache geändert wurde, nachdem sie abgerufen wurden. Wenn die Daten unverändert sind, kann die Änderung vorgenommen. Andernfalls muss die Anwendung entscheiden, ob Sie aktualisieren. (Die Geschäftslogik, die dieses Beschlusses Laufwerke werden anwendungsspezifische.) Dieser Ansatz eignet sich für Situationen, wo Updates selten sind oder wo Konflikte werden.
- __Pessimistisch.__ Wenn sie die Daten abruft, sperrt die Anwendung es im Cache zu einer anderen Instanz ändern. Dadurch wird sichergestellt, dass Konflikte können nicht auftreten, jedoch können sie auch andere Instanzen, die die gleichen Daten blockieren. Eingeschränkte Parallelität Skalierbarkeit einer Lösung beeinflussen und sollte nur für kurzlebige Prozesse. Dieser Ansatz ist für Situationen geeignet, Konflikte wahrscheinlich sind, insbesondere, wenn eine Anwendung mehrere Elemente im Cache aktualisiert und muss sicherstellen, dass diese Änderungen konsistent angewendet werden.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Implementieren von hoher Verfügbarkeit und Skalierung und Leistung

Verwenden Sie einen Cache als primäre Daten-Repository; Dies ist die Rolle des ursprünglichen Datenspeicher aus dem Cache gefüllt wird. Im ursprünglichen Datenspeicher obliegt die Dauerhaftigkeit der Daten.

Achten Sie darauf, nicht kritische Abhängigkeit von der Verfügbarkeit eines gemeinsam genutzten Cache Service in Projektmappen einführen. Eine Anwendung sollte weiterhin funktionieren, wenn der Dienst gemeinsam genutzten Cache nicht verfügbar ist. Die Anwendung sollte hängen oder warten Cachedienst wieder fehl.

Daher muss die Anwendung vorbereitet werden, erkennen die Verfügbarkeit des Dienstes Cache und den ursprünglichen Datenspeicher zurückgreifen, wenn der Cache nicht verfügbar ist. Die [Leistungsschalter Muster](http://msdn.microsoft.com/library/dn589784.aspx) eignet sich für dieses Szenario. Der Dienst den Cache zurückgewonnen und verfügbar wird, kann der Cache Daten lesen im ursprünglichen Datenspeicher Strategie wie [Cache-Aside-Muster](http://msdn.microsoft.com/library/dn589799.aspx)bilden aufgefüllt sein.

Jedoch wäre gibt es sich skalierbar auf dem System greift die Anwendung wieder in den ursprünglichen Datenspeicher wird der Cache vorübergehend nicht verfügbar.
Wiederherstellung der Datenspeicher im ursprünglichen Datenspeicher konnte mit Daten Timeouts überschwemmt und Verbindung fehlgeschlagen.

Sollten Sie einen lokalen privaten Cache jeweils mit gemeinsam genutzten Cache, der Zugriff auf alle Instanzen einer Anwendung implementieren. Wenn die Anwendung ein Element abruft, sie können zuerst im lokalen Cache und in den cache und schließlich in die ursprünglichen Daten speichern. Lokale Cache kann mithilfe der Daten im gemeinsam genutzten Cache oder in der Datenbank ist der gemeinsam genutzten Cache aufgefüllt werden.

Dieser Ansatz erfordert sorgfältige Konfiguration zu verhindern, dass den lokalen Cache bei gemeinsam genutzten Cache veraltet. Der lokale Cache fungiert jedoch einen Puffer ist der gemeinsam genutzten Cache nicht erreichbar. Abbildung 3 illustriert diese Struktur.

![Eine lokale, private Cache mit gemeinsam genutzten Cache](media/best-practices-caching/Caching3.png)
_Abbildung 3: gemeinsam genutzten Cache eine lokale, private Cache mit_

Um große Caches zu unterstützen, die relativ langlebigen Daten Dienstleistungen einige Cache eine Option für hohe Verfügbarkeit, die Automatisches Failover implementiert, wird der Cache nicht verfügbar. Dieser Ansatz in der Regel beinhaltet replizieren die zwischengespeicherten Daten auf primären Cacheserver sekundärer Cache-Server gespeichert, und auf den sekundären Server wechseln, wenn der primäre Server ausfällt oder die Konnektivität verloren.

Die Wartezeit reduzieren, die an mehrere Ziele schreiben zugeordnet wurde, kann die Replikation auf den sekundären Server asynchron auftreten, wenn Daten auf dem primären Server in den Cache geschrieben werden. Dieser Ansatz führt zu der Möglichkeit, dass einige der zwischengespeicherten Informationen im Fehlerfall verloren aber Teil dieser Daten sollten kleine gegenüber der Gesamtgröße des Cache.

Wenn gemeinsam genutzten Cache groß ist, kann es vorteilhaft sein Partitionieren Sie die zwischengespeicherten Daten auf Knoten zu verringern die Gefahr von Konflikten skalierbar. Viele gemeinsame Caches können Knoten dynamisch hinzufügen (und entfernen) und Ausgleich von Daten über Partitionen. Dieser Ansatz kann in der Auflistung von Knoten für Clientanwendungen als Cache nahtlosen, einzelne präsentiert umfassen clustering. Intern werden jedoch die Daten verteilt zwischen Knoten vordefinierte Verteilung Strategie, die die Last gleichmäßig verteilt. [Partitionierung Leitfaden Daten](http://msdn.microsoft.com/library/dn589795.aspx) auf der Microsoft-Website enthält weitere Informationen zu möglichen Partitionierungsstrategien.

Clustering kann auch die Verfügbarkeit des Cache erhöhen. Wenn ein Knoten ausfällt, ist der Rest des Caches noch verfügbar.
Clustering wird häufig in Verbindung mit Replikation und Failover verwendet. Jeder Knoten repliziert werden kann und das Replikat kann schnell online geschaltet werden, wenn der Knoten ausfällt.

Gelesen und Schreibvorgänge werden einzelne Daten oder Objekte beinhalten. Aber manchmal es möglicherweise speichern oder große Datenmengen schnell abzurufen.
Z. B. könnte seeding Cache Hunderte oder Tausende von Elementen in den Cache geschrieben. Eine Anwendung muss möglicherweise zahlreiche verwandte Elemente als Teil derselben Anforderung aus dem Cache abzurufen.

Viele große Caches bereitstellen Batchoperationen für diese Zwecke. Dies ermöglicht eine Clientanwendung auf eine große Anzahl von Elementen in einer einzelnen Anforderung Paket und verringert den Aufwand, der mit zahlreiche kleine Anfragen verknüpft ist.

## <a name="caching-and-eventual-consistency"></a>Zwischenspeichern und eventual consistency

Cache-Aside-Muster zu müssen die Instanz der Anwendung, die den Cache füllt auf neuere und konsistente Version der Daten. In einem System, die eventual Consistency (wie replizierter Datenspeicher) implementiert möglicherweise dies nicht der Fall.

Eine Instanz einer Anwendung kann ein Datenelement ändern und die zwischengespeicherte Version dieses Elements ungültig. Eine andere Instanz der Anwendung könnte versuchen, lesen Sie dieses Element aus einem Cache verursacht einen Cachefehler liest die Daten aus dem Datenspeicher und dem Cache hinzugefügt. Datenspeicher voll nicht mit anderen Replikaten synchronisiert wurden, konnte die Anwendungsinstanz jedoch lesen und Füllen mit den alten Wert.

Weitere Informationen zum Behandeln von Datenkonsistenz finden Sie unter [Daten Konsistenz Primer](http://msdn.microsoft.com/library/dn589800.aspx) -Seite auf der Microsoft-Website.

### <a name="protect-cached-data"></a>Zwischengespeicherte Daten schützen

Unabhängig von der Cachedienst verwenden, wie die Daten schützen, die im Cache vor nicht autorisiertem Zugriff stattfindet. Es gibt zwei Hauptanliegen:

- Die Daten im cache
- Die Vertraulichkeit von Daten zwischen dem Cache und der Cache mit fließt

Zum Schutz der Daten im Cache kann der Cachedienst Authentifizierungsmechanismus implementieren, der erfordert, dass die Anwendung Folgendes angeben:
- Welche Identitäten können Daten im Cache zugreifen.
- Welche Vorgänge (Lese- und Schreibzugriff), die diese Identitäten ausführen.

Um Aufwand verringern, der zugeordnet lesen und Schreiben von Daten, nachdem eine Identität schreiben bzw. lesen, Sie alle Daten im Cache verwenden Cache gewährt wurde.

Benötigen Sie Zugriff auf eine Teilmenge der zwischengespeicherten Daten, führen Sie eine der folgenden:

- Cache (mithilfe von anderen Cacheserver) in Partitionen aufgeteilt und nur Zugriff auf Identitäten für Partitionen, die sie dürfen verwenden.
- Verschlüsseln Sie die Daten in jeder Teilmenge mit anderen Tasten und Bereitstellen Sie die Verschlüsselungsschlüssel nur die Identitäten, die Zugang zu jeder Teilmenge. Eine Clientanwendung möglicherweise noch alle Daten im Cache abgerufen, aber es werden nur Entschlüsseln der Daten für die Schlüssel verfügt.

Sie müssen auch Daten schützen, fließt in den Cache. Hierzu werden die Sicherheitsfeatures von der Netzwerkinfrastruktur, die Clientanwendungen verwenden für die Verbindung im Cache abhängig. Wenn Cache mit vor-Ort-Server innerhalb derselben Organisation hostet die Clientanwendungen implementiert wird, kann dann die Isolation des Netzwerkes nicht zusätzliche Schritte erforderlich. Wenn der Cache im Remote erfordert eine TCP- oder HTTP-Verbindung über ein öffentliches Netzwerk (wie das Internet), sollten Sie SSL implementieren.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Aspekte bei der Implementierung mit Microsoft Azure caching

Azure bietet Azure Redis Cache. Dies ist eine Implementierung des open-Source-Redis-Cache, der als Dienst in Azure-Rechenzentrum ausgeführt wird. Es bietet zwischenspeichern, die jede Azure-Anwendung zugegriffen werden kann, ob die Anwendung als Cloud-Dienst, eine Website oder ein Azure virtuellen Computer implementiert ist. Caches können von Clientanwendungen verwendet werden, die die entsprechende Taste.

Azure Redis Cache ist eine leistungsfähige caching Lösung Verfügbarkeit, Skalierung und Sicherheit. Es wird normalerweise als Dienst auf mindestens einem dedizierten Computer verteilt. Er versucht, so viele Informationen wie möglich im Arbeitsspeicher schnellen Zugriff speichern. Diese Architektur soll geringe Latenz und hohen Durchsatz durch langsame e/a-Vorgänge reduzieren.

 Azure Redis Cache ist kompatibel mit vielen verschiedenen APIs, die von Clientanwendungen verwendet werden. Haben Sie vorhandene Anwendung, die bereits mit lokalen Azure Redis Cache verwenden, bietet Azure Redis Cache einen Schnellmigration Pfad zum Zwischenspeichern in der Cloud.

> [AZURE.NOTE] Azure bietet auch Managed Cache Service. Dieser Service basiert auf Azure Service Fabric Cache-Engine. Sie können einen verteilten Cache erstellen, der lose Anwendung freigegeben werden können. Der Cache wird auf leistungsstarke Server Azure-Rechenzentrum gehostet.
Diese Option jedoch nicht mehr empfohlen und erfolgt nur vorhandene Applikationen unterstützen, die erstellt wurden, um sie zu verwenden. Verwenden Sie für alle Neuentwicklungen stattdessen Azure Redis Cache.
>
> Darüber hinaus unterstützt Azure caching Rolle. Diese Funktion können Sie einen Cache erstellen einen Cloud-Dienst.
Cache von Instanzen einer Web- oder Worker-Rolle gehostet und kann nur über Rollen, die als Teil der gleichen Cloud-Bereitstellungseinheit betrieben werden. (Eine Bereitstellungseinheit ist ein Satz von Instanzen, die als Cloud-Dienst in einen bestimmten Bereich bereitgestellt werden.) Cache gruppiert, und alle Instanzen der Rolle innerhalb der gleichen Bereitstellung, die den Cache hostet Teil desselben Clusters Cache. Diese Option jedoch nicht mehr empfohlen und erfolgt nur vorhandene Applikationen unterstützen, die erstellt wurden, um sie zu verwenden. Verwenden Sie für alle Neuentwicklungen stattdessen Azure Redis Cache.
>
> Azure Managed Cache Service und Azure-Rolle Cache sind derzeit Alter am 16. November 2016 geplant.
Es wird empfohlen, Azure Redis Cache in Vorbereitung dieser Rente zu migrieren. Weitere Informationen finden Sie auf der Seite   [Neuigkeiten Angebot Azure Redis Cache und welche Größe sollte verwendet werden?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) auf der Microsoft-Website.


### <a name="features-of-redis"></a>Features von Redis

 Redis ist mehr als eine einfache Cacheserver. Einen umfassenden Befehlssatz, der viele Szenarien unterstützt bietet eine verteilte Datenbank im Arbeitsspeicher. Diese werden weiter unten in diesem Dokument im Abschnitt Redis Verwendung zwischenspeichern beschrieben. In diesem Abschnitt werden einige der wichtigsten Features Redis bietet zusammengefasst.

### <a name="redis-as-an-in-memory-database"></a>Eine Datenbank im Arbeitsspeicher redis

Redis unterstützt Lese- und Schreibvorgänge. In Redis können schreibt Systemfehler entweder in regelmäßigen Abständen gespeichert werden, lokale Snapshot-Datei oder eine Datei anhängen nur geschützt werden. Dies ist nicht der Fall in vielen Caches (die vorübergehende Datenspeicher berücksichtigt werden sollen).

 Alle Schreibvorgänge sind asynchron und Clients lesen und Schreiben von Daten nicht blockiert. Wenn Redis läuft, liest die Daten aus der Momentaufnahme oder Protokoll und Cache im Arbeitsspeicher erstellt wird. Weitere Informationen finden Sie auf der Website Redis [Redis Dauerhaftigkeit](http://redis.io/topics/persistence) .

> [AZURE.NOTE] Redis garantiert nicht, dass alle Schreibvorgänge im Katastrophenfall gespeichert, aber Sie schlimmstenfalls nur wenige Sekunden Daten verlieren. Beachten Sie, dass ein Cache nicht als autorisierende Datenquelle fungieren soll, und es obliegt Applikationen mit Cache kritische Daten erfolgreich in einen entsprechenden Datenspeicher gespeichert werden. Weitere Informationen finden Sie unter [Cache-Aside-Muster](http://msdn.microsoft.com/library/dn589799.aspx).

#### <a name="redis-data-types"></a>Redis Datentypen

Redis ist ein Schlüssel-Wert-Speicher, einfache oder komplexe Datenstrukturen wie Hashwerte Listen Werte können und festgelegt. Eine Reihe von atomaren Operationen für diese Datentypen unterstützt. Schlüssel kann permanent oder markiert mit einem Time-to-live, bei der Schlüssel und den entsprechenden Wert automatisch aus dem Cache entfernt werden. Weitere Informationen über Redis-Schlüsseln und Werten Seite [eine Einführung in die Datentypen und Abstraktionen Redis](http://redis.io/topics/data-types-intro) auf der Website Redis.

#### <a name="redis-replication-and-clustering"></a>Redis Replikation und clustering

Redis unterstützt die Master/Slave-Replikation Verfügbarkeit und Wartung Durchsatz. Schreiben Sie auf einem Masterknoten Redis für einen oder mehrere untergeordnete Knoten repliziert werden. Lesevorgänge können vom Kapitän oder eines der untergeordneten Elemente bereitgestellt werden.

Bei einer Partition Netzwerk können untergeordnete Elemente weiterhin Daten und anschließend transparent synchronisieren mit dem Wenn die Verbindung wiederhergestellt ist. Weitere Informationen finden Sie auf der [Replikation](http://redis.io/topics/replication) auf der Website Redis.

Redis stellt auch clustering Sie transparent Daten in Splitter auf Server und verteilt die Last. Dieses Feature erhöht skalierbar, da neue Redis-Server hinzugefügt werden und die Daten als die Größe des Caches neu partitioniert.

Darüber hinaus kann jeder Server im Cluster mithilfe der Master-Slave-Replikation repliziert werden. Dadurch werden Verfügbarkeit auf jedem Knoten im Cluster. Weitere Informationen zu Clustern und Sharding finden Sie auf der Website Redis [Redis Cluster Schulungsseite](http://redis.io/topics/cluster-tutorial) .

### <a name="redis-memory-use"></a>Redis Speicher

Redis-Cache hat eine endliche Größe hängt von den verfügbaren Ressourcen auf dem Hostcomputer. Wenn Sie einen Redis-Server konfigurieren, können Sie die maximale Speichergröße angeben, verwendet werden kann. Sie können einen Schlüssel auch in einem Redis-Cache zu eine Ablaufzeit, nach der sie automatisch aus dem Cache entfernt wird. Diese Funktion kann in-Memory-Caches mit alten oder veraltete Daten verhindern.

Als Speicher füllt, kann Redis automatisch Schlüssel und deren Werte Entfernen von folgenden Richtlinien. Der Standardwert ist LRU (zuletzt verwendet), aber Sie können auch andere Richtlinien wie Schlüssel nach dem Zufallsprinzip entfernen oder Deaktivieren der Entfernung insgesamt (, Fall versucht der Cache nicht Elemente hinzugefügt werden, wenn er voll ist). Die [Verwendung als LRU-Cache Redis](http://redis.io/topics/lru-cache) Seite enthält weitere Informationen.

### <a name="redis-transactions-and-batches"></a>Redis Transaktionen und Stapel

Redis kann eine Clientanwendung eine Reihe von Vorgängen zu übermitteln, das Lesen und Schreiben von Daten in den Cache als atomare Transaktion. Alle Befehle in der Transaktion unbedingt nacheinander ausgeführt und keine anderen gleichzeitigen Befehle zwischen interwoven.

Diese sind jedoch nicht true Transaktionen eine relationale Datenbank durchführen würden. Transaktion besteht aus zwei Phasen – die erste ist die Befehle werden in der Warteschlange und die zweite Ausführung der Befehle. Phase Queuing-Befehl werden die Befehle, die die Transaktion vom Client gesendet. Tritt eine Fehlermeldung zu diesem Zeitpunkt (z. B. einen Syntaxfehler, oder die falsche Anzahl von Parametern) dann Redis will die gesamte Transaktion verarbeitet und verworfen.

Während der Ausführungsphase führt Redis jeder Befehl in der Warteschlange nacheinander. Schlägt ein Befehl in dieser Phase Redis nächsten Befehl in der Warteschlange fort und rollt nicht wieder die Effekte der Befehle, die bereits ausgeführt wurden. Diese vereinfachte Form der Transaktion kann Leistung und durch Konflikte verursacht Leistungsprobleme vermeiden.

Redis implementiert eine Form der Parallelität um Konsistenz. Detaillierte Informationen zu Transaktionen mit Redis finden Sie auf [Seite Transaktionen](http://redis.io/topics/transactions) auf der Website Redis.

Redis unterstützt nicht transaktionale Batchverarbeitung von Anfragen. Redis-Protokoll, das Clients verwenden, um Befehle an einen Redis-Server senden ermöglicht einem Client, eine Reihe von Vorgängen als Teil der gleichen Anforderung senden. Können dadurch Paketfragmentierung im Netzwerk. Bei der Verarbeitung des Stapels wird jeder Befehl ausgeführt. Wenn diese Befehle fehlerhaft sind, die verbleibenden Befehle wird sie abgelehnt (dies mit einer Transaktion nicht) Es wird auch keine Garantie der Reihenfolge, in der die Befehle im Batch verarbeitet werden.

### <a name="redis-security"></a>Redis Sicherheit

Redis konzentriert sich ausschließlich auf den schnellen Zugriff auf Daten und in einer vertrauenswürdigen Umgebung ausgeführt, die nur von vertrauenswürdigen Clients zugegriffen werden kann. Redis unterstützt eine begrenzte Sicherheitsmodell basierend auf Kennwortauthentifizierung. (Es kann Authentifizierung zu entfernen, obwohl dies nicht empfohlen.)

Alle authentifizierten Clients globale dasselbe Kennwort verwenden und haben Zugriff auf dieselben Ressourcen. Benötigen Sie umfassenderen Sicherheit, implementieren Sie eine eigene Sicherheitsstufe vor dem Redis-Server und alle Clientanforderungen sollten diese zusätzlichen durchlaufen. Redis sollte nicht direkt an nicht vertrauenswürdige oder nicht authentifizierte Clients verfügbar gemacht.

Sie können Zugriff auf Befehle deaktivieren oder umbenennen (und privilegierten Clients mit den neuen Namen) einschränken.

Redis unterstützt nicht direkt jede Form der Verschlüsselung, damit alle Codierung von Clientanwendungen ausgeführt werden muss. Darüber hinaus bietet Redis keine keinerlei Sicherheit. Sie müssen Daten über das Netzwerk fließt, sollten SSL-Proxy implementiert.

Weitere Informationen finden Sie auf der [Redis Sicherheit](http://redis.io/topics/security) auf der Website Redis.

> [AZURE.NOTE] Azure Redis Cache bietet eine eigene Sicherheit durch die Clients eine Verbindung herstellen. Die zugrunde liegende Redis-Server sind nicht mit dem öffentlichen Netzwerk verfügbar.

### <a name="using-the-azure-redis-cache"></a>Mithilfe von Azure Redis cache

Azure Redis Cache ermöglicht den Zugriff auf Redis Server auf Servern Azure Datencenter; Sie fungiert als Fassade, die Zugriffskontrolle Sicherheit und. Sie können einen Cache mit der Azure-Verwaltungsportal bereitstellen. Das Portal bietet eine Reihe von vordefinierten Konfigurationen, 53GB Cache als einen dedizierten Dienst ohne Replikation (keine Verfügbarkeit garantiert) auf gemeinsam genutzter Hardware unterstützt SSL-Kommunikation (Datenschutz) und Master-Slave-Replikation mit einer SLA von 99,9 % Verfügbarkeit auf 250 MB cache.

Azure-Verwaltungsportal verwenden, können Sie auch Entfernungsrichtlinie des Caches zu konfigurieren und steuern den Zugriff auf den Cache Hinzufügen von Benutzern zu den Rollen; Besitzer, Contributor Reader. Diese Rollen definieren Operationen Mitglieder ausführen können. Beispielsweise Mitglieder der Rolle Eigentümer haben vollständige Kontrolle über den Cache (einschließlich Sicherheit) und seinen Inhalt, Mitglieder der Beitragendenrolle lesen und Schreiben von Informationen in den Cache und Mitglieder der Rolle können nur Daten aus dem Cache abgerufen.

Die meisten Verwaltungsaufgaben Azure-Verwaltungsportal durchgeführt werden, und deshalb sind viele administrative Befehle in der Standardversion von Redis nicht verfügbar, einschließlich der Möglichkeit die Konfiguration programmgesteuerte Herunterfahren des Servers Redis konfigurieren Sie zusätzliche Slaves oder das Speichern von Daten auf Festplatten.

Azure-Verwaltungsportal enthält praktische grafisch dargestellt, die die Leistung des Cache überwachen kann. Beispielsweise können die Anzahl der Verbindungen wird die Anzahl der Anfragen durchgeführt, das Volumen der Lese- und Schreibvorgänge und die Anzahl der Treffer im Vergleich zu Cachefehler. Mithilfe dieser Informationen die Wirksamkeit des Cache bestimmen können und ggf. zu einer anderen Konfiguration wechseln oder die Entfernungsrichtlinie ändern. Darüber hinaus können Sie Alerts erstellen, die e-Mails an den Administrator senden, wenn ein oder mehrere wichtige Metriken außerhalb des erwarteten Bereichs liegen. Beispielsweise überschreitet die Anzahl der Cachefehler angegebenen Wert in der letzten Stunde, konnte ein Administrator benachrichtigt Cache möglicherweise zu klein oder Daten möglicherweise werden vertrieben zu schnell.

Sie können auch CPU, Arbeitsspeicher und Netzwerkverwendung für den Cache überwachen.

Weitere Informationen und Beispiele zum Erstellen und Konfigurieren einer Azure Redis Cache Seite [runde Azure Redis Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure Blog.

## <a name="caching-session-state-and-html-output"></a>Zwischenspeichern von Sitzungsstatus und HTML-Ausgabe

Wenn Sie ASP.NET Applications web, mit Azure Webrollen ausführen, Sitzungszustandsinformationen und HTML-Ausgabe in einem Azure Redis Cache gespeichert werden kann. Sitzungszustandsanbieter Azure Redis Cache können Sitzungsinformationen zwischen verschiedenen Instanzen einer ASP.NET Web-Anwendung gemeinsam und in Webfarm Situationen, in denen Client-Server-Affinität ist nicht verfügbar und Sitzung Daten im Arbeitsspeicher Zwischenspeichern nicht wäre, sehr nützlich ist.

Verwenden der Sitzungszustandsanbieter mit Azure Redis Cache bietet mehrere Vorteile:

- Der Sitzungszustand für eine große Anzahl von Instanzen einer ASP.NET Web-Anwendung bietet verbesserte Skalierbarkeit gemeinsam verwendet werden kann,
- Kontrollierte, gleichzeitig auf die gleichen Sitzungszustandsdaten für mehrere Reader und einzelne Writer unterstützt und
- Sie können Komprimierung speichern Speicher und Netzwerk-Performance verbessern.

Weitere Informationen finden Sie in [ASP.NET Sitzungszustandsanbieter Azure Redis Cache](redis-cache/cache-aspnet-session-state-provider.md) -Seite auf der Microsoft-Website.

> [AZURE.NOTE] Verwenden Sie nicht die Sitzungszustandsanbieter Azure Redis Cache für ASP.NET Applications, die außerhalb der Azure-Umgebung ausgeführt. Die Wartezeit für den Zugriff auf den Cache von Azure eliminieren die Leistungsvorteile durch Zwischenspeichern von Daten.

Ebenso können Ausgabecacheanbieter für Azure Redis Cache speichern HTTP-Antworten von einer ASP.NET Web-Anwendung generiert. Azure Redis Cache mit den Ausgabecacheanbieter kann die Reaktionszeiten der Anwendung verbessern, die komplexe HTML-Ausgabe dargestellt. Generieren von ähnliche Antworten stellen können Instanzen verwenden gemeinsame Ausgabe Fragment im Cache, anstatt diese HTML-Ausgabe neu generieren.  Weitere Informationen finden Sie in [ASP.NET Ausgabecacheanbieter für Azure Redis Cache](redis-cache/cache-aspnet-output-cache-provider.md) -Seite auf der Microsoft-Website.

### <a name="azure-redis-cache"></a>Azure Redis cache

Azure Redis Cache bietet Zugriff auf Redis-Server, die in Azure-Rechenzentrum befinden. Sie fungiert als Fassade, die Zugriffskontrolle Sicherheit und. Sie können einen Cache mit Azure-Portal bereitstellen.

Das Portal bietet eine Reihe von vordefinierten Konfigurationen. Diese reichen von einer 53 GB Cache als einen dedizierten Dienst, SSL-Kommunikation (Datenschutz) und Master/Slave-Replikation mit einer SLA von 99,9 % Verfügbarkeit bis 25 0 MB-Cache ohne Replikation (keine Verfügbarkeit garantiert) auf gemeinsam genutzter Hardware unterstützt.

Azure-Portal verwenden, können Sie auch Entfernungsrichtlinie des Caches zu konfigurieren und steuern den Zugriff auf den Cache Hinzufügen von Benutzern zu Rollen bereitgestellt.  Diese Rollen, die Vorgänge, die Mitgliedern ausführen können definieren, enthalten Besitzer, Mitwirkende und Leser. Beispielsweise Mitglieder der Rolle Eigentümer haben vollständige Kontrolle über den Cache (einschließlich Sicherheit) und seinen Inhalt, Mitglieder der Beitragendenrolle lesen und Schreiben von Informationen in den Cache und Mitglieder der Rolle können nur Daten aus dem Cache abgerufen.

Die meisten Verwaltungsaufgaben erfolgt über das Azure-Portal. Aus diesem Grund sind viele administrative Befehle, die in der Standardversion von Redis nicht verfügbar, einschließlich der Möglichkeit, programmgesteuert ändern Sie die Konfiguration, Redis-Server Herunterfahren, konfigurieren Sie weitere untergeordnete Elemente oder das Speichern auf der Festplatte.

Azure-Portal enthält praktische grafisch dargestellt, die die Leistung des Cache überwachen kann. Beispielsweise können Sie die Anzahl der Verbindungen vorgenommen, die Anzahl der ausgeführten Anfragen, das Volumen der Lese- und Schreibvorgänge und die Anzahl der Cachetreffer gegenüber Cachefehler anzeigen. Mithilfe dieser Informationen können Sie bestimmen die Wirksamkeit des Cache und bei Bedarf eine andere Konfiguration oder Ändern der Entfernungsrichtlinie.

Darüber hinaus können Sie Alerts erstellen, die e-Mails an den Administrator senden, wenn ein oder mehrere wichtige Metriken außerhalb des erwarteten Bereichs liegen. Sie möchten z. B. ein Administrator eine Warnung übersteigt die Anzahl der Cachefehler angegebenen Wert in der letzten Stunde, da dadurch möglicherweise des Caches zu klein oder Daten möglicherweise werden vertrieben zu schnell.

Sie können auch CPU, Arbeitsspeicher und Netzwerkverwendung für den Cache überwachen.

Weitere Informationen und Beispiele zum Erstellen und Konfigurieren einer Azure Redis Cache Seite [runde Azure Redis Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure Blog.

## <a name="caching-session-state-and-html-output"></a>Zwischenspeichern von Sitzungsstatus und HTML-Ausgabe

Wenn Sie erstellen state ASP.NET Web Applications, Ausführen mit Azure Webrollen Sitzung speichern Informationen und HTML-Ausgabe in einem Azure Redis Cache. Sitzungszustandsanbieter Azure Redis Cache kann Sitzungsinformationen zwischen verschiedenen Instanzen einer ASP.NET Web-Anwendung gemeinsam und in Webfarm Situationen, in denen Client-Server-Affinität ist nicht verfügbar und Sitzung Daten im Arbeitsspeicher Zwischenspeichern nicht wäre, sehr nützlich ist.

Verwenden der Sitzungszustandsanbieter mit Azure Redis Cache bietet mehrere Vorteile:

- Freigeben von Sitzungsstatus mit einer großen Anzahl von Instanzen der ASP.NET Web Applications.
- Verbesserte Skalierbarkeit bereitstellen.
- Kontrollierte, gleichzeitig auf die gleichen Sitzungszustandsdaten unterstützt für mehrere Reader und einzelne Writer.
- Verwenden von Komprimierung speichern Speicher und Netzwerk-Performance verbessern.

Weitere Informationen finden Sie in [ASP.NET Sitzungszustandsanbieter Azure Redis Cache](redis-cache/cache-aspnet-session-state-provider.md) -Seite auf der Microsoft-Website.

> [AZURE.NOTE] Nicht mit der Sitzungszustandsanbieter Azure Redis Cache ASP.NET Applications, die außerhalb der Azure-Umgebung ausgeführt. Die Wartezeit für den Zugriff auf den Cache von Azure eliminieren die Leistungsvorteile durch Zwischenspeichern von Daten.

Ebenso kann Ausgabecacheanbieter für Azure Redis Cache speichern HTTP-Antworten von einer ASP.NET Web-Anwendung generiert. Azure Redis Cache mit den Ausgabecacheanbieter kann die Reaktionszeiten der Anwendung verbessern, die komplexe HTML-Ausgabe dargestellt. Instanzen, die ähnliche Antworten generieren können mit freigegebenen Ausgabe Fragment im Cache, anstatt diese HTML-Ausgabe neu generieren. Weitere Informationen finden Sie in der [ASP.NET Ausgabecacheanbieter für Azure Redis Cache](redis-cache/cache-aspnet-output-cache-provider.md) -Seite auf der Microsoft-Website.

## <a name="building-a-custom-redis-cache"></a>Erstellen eines benutzerdefinierten Redis-Caches

Azure Redis Cache fungiert als Fassade für die zugrunde liegenden Redis-Server. Derzeit unterstützt festgelegten Konfigurationen jedoch bietet keine Clusterunterstützung Redis. Erweiterte Konfiguration erfordern, die nicht von Azure Redis Cache (z. B. eine größer als 53 GB Cache) Sie erstellen und eigene Redis Server Azure virtuellen Maschinen.

Deswegen einen komplexen Prozess erstellen mehrere VMs als Masterserver und untergeordnete Knoten handeln Sie Replikation implementieren müssen. Außerdem möchten Sie einen Cluster erstellen, brauchen dann Sie mehrere Master und untergeordneten Servern. Eine minimale gruppierten Replikationstopologie, die hohe Verfügbarkeit und Skalierung umfasst sechs VMs als drei Master/Slave-Server (ein Cluster muss mindestens einem master-Knoten).

Jedes Paar Master/Slave sollte nah beieinander befinden, um Wartezeit zu minimieren. Jedoch kann jeder Satz von Paaren in verschiedenen Azure Rechenzentren in unterschiedlichen Regionen befindet ausgeführt werden zwischengespeicherte Daten nahe der Anwendung finden, die sie nutzen möchten. Seite auf der Microsoft-Website [Auf einer CentOS Linux VM in Azure Redis ausführen](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) durchläuft ein Beispiel zum Erstellen und Konfigurieren eines Redis Knotens als ein Azure-VM.

[AZURE.NOTE] Bitte beachten Sie, dass bei der Implementierung von Redis Cache so zum Überwachen, verwalten und Schützen des Diensts zuständig.

## <a name="partitioning-a-redis-cache"></a>Redis Cache-Partitionierung

Cache-Partitionierung umfasst den Cache auf mehreren Computern aufteilen. Diese Struktur hat mehrere Vorteile über einen einzigen Cache Server, einschließlich:

- Erstellen eines Cache ist viel größer als auf einem Server gespeichert werden können.
- Verteilen von Daten auf Servern, Verbesserung der Verfügbarkeit. Wenn ein Server ausfällt oder nicht zugegriffen werden mehr, können die Daten, die nicht verfügbar ist, aber die Daten auf den verbleibenden Servern zugegriffen werden. Ein Cache ist dies nicht entscheidend, werden die zwischengespeicherten Daten nur eine vorübergehende Kopie der Daten, die in einer Datenbank gespeichert. Zwischengespeicherte Daten auf einem Server zugänglich können stattdessen auf einem anderen Server zwischengespeichert werden.
- Verteilen die Last auf Server, wodurch Leistung und Erweiterbarkeit.
- Geolocating Daten für Benutzer, die Zugriff, wodurch Latenz schließen.

Ein Cache ist die häufigste Form der Partitionierung Sharding. Bei dieser Strategie wird jede Partition (oder Splitter) einen Redis Cache eigene. Daten richtet sich an einer bestimmten Partition mit Sharding-Logik, die unterschiedliche Methoden zum Verteilen von Daten verwenden kann. [Sharding Muster](http://msdn.microsoft.com/library/dn589797.aspx) enthält weitere Informationen zum Implementieren von Sharding.

Zum Implementieren der Partitionierung in einem Redis-Cache können Sie eine der folgenden Methoden ausführen:

- _Serverseitige Abfrage weiterleiten._ Bei diesem Verfahren sendet eine Clientanwendung eine Anforderung an Redis-Server, die den Cache (wahrscheinlich den nächsten Server) umfassen. Jeder Redis-Server speichert die Metadaten, die die Partition beschreibt, enthält auch Informationen über die Partitionen befinden sich auf anderen Servern. Redis-Server überprüft die Clientanforderung. Wenn es lokal aufgelöst werden kann, führen sie den Vorgang. Andernfalls leitet er die Anforderung an den entsprechenden Server. Dieses Modell wird von Redis-clustering implementiert und wird ausführlich auf [Redis Cluster Lernprogramm](http://redis.io/topics/cluster-tutorial) auf der Redis-Website. Redis-clustering für Clientanwendungen transparent ist und zusätzliche Redis Server können Cluster (und die Daten neu partitioniert) ohne die Clients neu konfigurieren hinzugefügt werden.

- _Clientseitige Partitionierung._ In diesem Modell enthält die Clientanwendung Logik (möglicherweise in Form einer Bibliothek), die leitet Anfragen an den entsprechenden Redis-Server. Dieser Ansatz kann mit Azure Redis Cache verwendet werden. Erstellen Sie mehrere Azure Redis Cache (einer für jede Datenpartition) und implementieren Sie die clientbasierte Logik, die Anfragen an die richtige Cache weiterleitet. Wenn das Partitionierungsschema ändert (Wenn beispielsweise zusätzliche Azure Redis Cache erstellt werden), müssen Clientanwendungen neu konfiguriert werden.

- _Proxy geförderten Partitionierung._ In diesem Schema Redis Client Applications senden Anfragen an einen zwischengeschalteten Proxy-Dienst versteht, wie die Daten partitioniert und leitet die Anforderung an den entsprechenden Server. Dieser Ansatz kann auch mit Azure Redis Cache verwendet werden. der Proxy-Dienst kann als Azure-Cloud-Dienst implementiert. Dieser Ansatz erfordert zusätzliche Komplexität für die Implementierung des Service, und fordert möglicherweise länger dauern als mit clientseitigen Partitionierung.

Die Seite [Partitionierung: wie Daten zwischen mehreren Instanzen Redis](http://redis.io/topics/partitioning) auf der Redis Website bietet Informationen über das Implementieren von mit Redis.

### <a name="implement-redis-cache-client-applications"></a>Redis Cache-Clientanwendungen implementieren

Redis unterstützt Clientanwendungen in zahlreichen Sprachen. Erstellen neue Applikationen mit.NET Framework wird empfohlen StackExchange.Redis-Clientbibliothek verwenden. Diese Bibliothek bietet eine.NET Framework-Objektmodell, das die Details für einen Redis-Server herstellen, Befehle senden und Empfangen von Antworten abstrahiert. Steht in Visual Studio als NuGet-Paket. Diese gleichen Bibliothek können Sie Verbindung mit einer Azure Redis Cache oder einen benutzerdefinierten Redis-Cache auf einer VM gehostet.

Verbindung mit einem Redis-Server mithilfe der statischen `Connect` Methode der `ConnectionMultiplexer` Klasse. Die Verbindung, die diese Methode erstellt die Lebensdauer der Clientanwendung verwendet werden soll und die Verbindung von mehreren Threads gleichzeitig verwendet werden. Verbinden Sie und trennen Sie jedes Mal eine Redis Operation durchführen, da dies die Leistung beeinträchtigen kann nicht.

Sie können die Parameter, beispielsweise die Adresse der Host Redis und das Kennwort angeben. Bei Verwendung von Azure Redis Cache ist das Kennwort entweder der primären oder sekundären Schlüssel für Azure Redis Cache wird mithilfe des Azure-Verwaltungsportals.

Nachdem Sie mit dem Redis-Server verbunden haben, erhalten Sie ein Handle für die Redis-Datenbank, die als Cache fungiert. Redis-Verbindung bietet die `GetDatabase` -Methode dazu. Sie können Elemente aus dem Cache abrufen und speichern im Cache mit den `StringGet` und `StringSet` Methoden. Diese Methoden erwarten einen Schlüssel als Parameter und gibt das Element zurück in den Cache mit dem entsprechenden Wert (`StringGet`) oder das Element im Cache mit diesem Schlüssel (`StringSet`).

Je nach Lage des Redis-Servers möglicherweise viele Vorgänge entstehen Latenz eine Anforderung an den Server gesendet und eine Antwort an den Client zurückgegeben. Asynchrone Versionen der Clientanwendungen dauernde unterstützen macht Methoden stellt die StackExchange-Bibliothek. Diese Methoden unterstützen das [Asynchrone Muster aufgabenbasierte](http://msdn.microsoft.com/library/hh873175.aspx) in.NET Framework.

Der folgende Codeausschnitt zeigt eine Methode namens `RetrieveItem`. Es zeigt eine Implementierung des Musters Cache-Aside basierend auf Redis und StackExchange-Bibliothek. Die Methode akzeptiert einen Zeichenfolgen-Schlüsselwert und versucht, das entsprechende Element aus dem Cache Redis durch Aufruf abrufen die `StringGetAsync` -Methode (die asynchrone Version der `StringGet`).

Wenn das Element nicht gefunden wird, wird abgerufen von der zugrunde liegenden Datenquelle mithilfe der `GetItemFromDataSourceAsync` Methode (die eine lokale Methode und nicht die StackExchange-Bibliothek). Es wird dann dem Cache hinzugefügt, mit dem `StringSetAsync` Methode, sodass sie schneller wieder abgerufen werden können.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

Die `StringGet` und `StringSet` Methoden sind nicht abrufen oder speichern Zeichenfolgenwerte auf. Sie können Elemente, die als Bytearray serialisiert werden. Benötigen Sie ein speichern, können Sie als einen Datenstrom serialisiert und verwenden die `StringSet` -Methode in den Cache geschrieben.

Sie können auch ein Objekt aus dem Cache gelesen, mit dem `StringGet` Methode und als ein deserialisieren. Der folgende Code zeigt eine Reihe von Erweiterungsmethoden für die IDatabase-Schnittstelle (die `GetDatabase` -Methode einer Redis Verbindung gibt eine `IDatabase` Objekt), und Beispielcode, die diese Methoden zum Lesen und Schreiben verwendet ein `BlogPost` Objekt im Cache:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

Der folgende Code veranschaulicht eine Methode namens `RetrieveBlogPost` , verwendet diese Erweiterungsmethoden zum Lesen und Schreiben einer serialisierbaren `BlogPost` Objekt im Cache der Cache-Aside-Muster:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Redis unterstützt Befehl pipelining, wenn eine Anwendung mehrere asynchrone Anfragen sendet. Redis kann die Anfragen über die Verbindung des statt empfangen und reagieren auf Befehle in einer strikten Sequenz bündeln.

Dadurch Wartezeit reduzieren, indem eine effizientere Nutzung des Netzwerks. Der folgende Codeausschnitt zeigt ein Beispiel, das gleichzeitig zwei Kunden abruft. Der Code sendet zwei Anfragen und führt einige andere Verarbeitung (nicht dargestellt) vor der Ergebnisse auf. Die `Wait` -Methode des Cache-Objekts ist vergleichbar mit der.NET Framework- `Task.Wait` Methode:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

Die Seite [Azure Redis Cache-Dokumentation](https://azure.microsoft.com/documentation/services/cache/) auf der Microsoft-Website enthält weitere Informationen über das Schreiben von Clientanwendungen, die Azure Redis Cache verwendet werden kann. Weitere Informationen zur [grundlegenden Verwendungsseite](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) auf der Website StackExchange.Redis.

Die [Rohrleitungen und Multiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) auf derselben Website finden Sie weitere Informationen über asynchrone Vorgänge und pipelining mit Redis und StackExchange-Bibliothek.  Im nächste Abschnitt dieses Artikels verwenden Redis zwischenspeichern, enthält Beispiele für einige erweiterten Techniken, die auf Daten angewendet werden können, die in einem Redis-Cache vorhanden ist.

## <a name="using-redis-caching"></a>Mithilfe von Redis Zwischenspeichern

Die einfachste Verwendung Redis Zwischenspeichern Anliegen ist Schlüssel-Wert-Paare der Wert eine nicht interpretierte Folge von beliebiger Länge auf, der binären Daten enthalten kann. (Es ist im Wesentlichen ein Bytearray, das als eine Zeichenfolge behandelt). Dieses Szenario ist im Abschnitt implementieren Redis Cache-Clientanwendungen weiter oben in diesem Artikel dargestellt.

Beachten Sie, dass Schlüssel auch nicht interpretierte Daten enthalten binären Informationen als Schlüssel verwendet werden kann. Je länger der Schlüssel ist jedoch mehr Speicherplatz dauert speichern und desto länger dauert Suchvorgänge durchführen. Nutzbarkeit und Wartungsfreundlichkeit entworfen Sie Ihrer erledigten sorgfältig und mit aussagekräftigen (aber nicht ausführlich).

Z. B. strukturierte Schlüssel wie "Customer: 100" den Schlüssel für die Kunden-ID 100 anstatt einfach "100" dargestellt. Dieses Schema können Sie leicht zwischen Werten unterscheiden, die unterschiedliche Datentypen zu speichern. Z. B. auch können den Schlüssel "Aufträge: 100" Sie den Schlüssel für den Auftrag mit der ID 100 darstellen.

Neben eindimensionale Binärzeichenfolgen kann ein Wert im Schlüssel-Wert-Paar Redis auch enthalten strukturierter (Informationen, einschließlich Listen sortiert und nicht sortiert) und Hash. Redis bietet einen umfassenden Befehlssatz, der diese Typen bearbeiten können, und viele dieser Befehle zur.NET Framework-Anwendung über eine Clientbibliothek wie StackExchange. Die Seite [eine Einführung in die Datentypen und Abstraktionen Redis](http://redis.io/topics/data-types-intro) Redis Website bietet eine detailliertere Übersicht über diese Typen und die Befehle zum Bearbeiten.

In diesem Abschnitt werden einige übliche Anwendungsbeispiele für diese Datentypen und Befehle zusammengefasst.

### <a name="perform-atomic-and-batch-operations"></a>Atomare durchführen und batch-Vorgänge

Redis unterstützt eine Reihe von atomaren Get und Set-Operationen für Werte. Diese Vorgänge entfernen möglich Rennen Gefahren, die bei separaten mit `GET` und `SET` Befehle. Die verfügbaren Operationen gehören:

- `INCR`, `INCRBY`, `DECR`, und `DECRBY`, die Operationen atomaren Inkrementieren und Dekrementieren auf Ganzzahlwerte für numerische Daten. StackExchange-Bibliothek bietet überladene Versionen der `IDatabase.StringIncrementAsync` und `IDatabase.StringDecrementAsync` Methoden zu diesen Operationen der resultierende Wert im Cache gespeichert. Der folgende Codeausschnitt zeigt, wie diese Methoden:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, die Ruft den Wert ab, der einem Schlüssel zugeordnet ist und auf einen neuen Wert geändert. Bibliothek StackExchange macht diesen Vorgang durch das `IDatabase.StringGetSetAsync` Methode. Der folgende Codeausschnitt zeigt ein Beispiel für diese Methode. Dieser Code gibt den aktuellen Wert, der mit dem Schlüssel "Data: Counter-Beispiel verknüpft ist. Dann wird den Wert für diesen Schlüssel auf NULL als Teil der gleichen Operation:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`und `MSET`, die zurückgegeben oder mehrere Werte in einem Vorgang ändern. Die `IDatabase.StringGetAsync` und `IDatabase.StringSetAsync` Methoden zur Unterstützung dieser Funktionalität überladen werden, wie im folgenden Beispiel gezeigt:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Mehrere Vorgänge in einer Transaktion Redis können auch kombiniert werden, wie Redis Transaktionen und Batches Abschnitt in diesem Artikel beschrieben. StackExchange-Bibliothek unterstützt Transaktionen über die `ITransaction` Schnittstelle.

Erstellen einer `ITransaction` Objekt mithilfe der `IDatabase.CreateTransaction` Methode. Befehle der Buchung mithilfe der bereitgestellten Methoden Aufrufen der `ITransaction` Objekt.

Die `ITransaction` Schnittstelle ermöglicht den Zugriff auf eine Reihe von Methoden, die ähnlich zugegriffen wird die `IDatabase` Schnittstelle, mit der Ausnahme, dass alle Methoden asynchron sind. Dies bedeutet, dass sie nur ausgeführt, wenn die `ITransaction.Execute` -Methode aufgerufen. Der zurückgegebene Wert der `ITransaction.Execute` -Methode gibt an, ob die Buchung erstellt wurde (true) oder (false) ist fehlgeschlagen.

Der folgende Codeausschnitt zeigt ein Beispiel, vergrößert und verkleinert zwei Indikatoren als Teil derselben Transaktion:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Beachten Sie, dass Redis Transaktionen im Gegensatz zu Transaktionen in relationalen Datenbanken. Die `Execute` -Methode fügt einfach alle Befehle, die der Transaktion ausgeführt werden und ist diese fehlerhaften dann die Transaktion beendet. Wenn alle Befehle erfolgreich in die Warteschlange gestellt haben, wird jeder Befehl asynchron ausgeführt.

Wenn jeder Befehl fehlschlägt, weiterhin andere Verarbeitung weiter. Möchten Sie überprüfen, ob ein Befehl erfolgreich ausgeführt wurde, müssen Sie die Ergebnisse des Befehls abrufen, mit der **Result** -Eigenschaft des entsprechenden Vorgangs wie im obigen Beispiel gezeigt. Lesen die **Result** -Eigenschaft blockiert den aufrufenden Thread, bis die Aufgabe abgeschlossen wurde.

Weitere Informationen finden Sie unter [Transaktionen in Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) -Seite auf der Website StackExchange.Redis.

Wenn Sie Batchoperationen ausführen, können Sie die `IBatch` Schnittstelle der StackExchange-Bibliothek. Diese Schnittstelle ermöglicht den Zugriff auf einen Satz von Methoden ähnlich zugreifen die `IDatabase` Schnittstelle, mit der Ausnahme, dass alle Methoden asynchron sind.

Erstellen einer `IBatch` Objekt mit der `IDatabase.CreateBatch` Methode, und führen Sie die Stapelverarbeitung mit der `IBatch.Execute` -Methode, wie im folgenden Beispiel gezeigt. Dieser Code setzt einen Zeichenfolgenwert, vergrößert und verkleinert Leistungsindikatoren im vorherigen Beispiel verwendet einfach und zeigt die Ergebnisse an:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

Es ist wichtig zu verstehen, dass im Gegensatz zu einer Transaktion schlägt ein Befehl in einem Batch ist fehlerhaft, andere weiterhin ausführen können. Die `IBatch.Execute` -Methode keine Anzeichen von Erfolg oder Fehlschlagen zurück.

### <a name="perform-fire-and-forget-cache-operations"></a>Führen Sie Feuer und vergessen Sie Cache-Operationen

Redis Feuer unterstützt und Vorgänge mit Befehlsflags vergessen. In diesem Fall der Client einfach startet eine aber kein Interesse im Ergebnis und wartet nicht auf den Befehl ausgeführt werden. Das folgende Beispiel zeigt, wie INCR Befehls Feuer und vergessen Vorgang:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Automatisch ablaufende Schlüssel angeben

Beim Speichern eines Elements in einem Redis-Cache können Sie einen Timeout angeben, nach dem das Element automatisch aus dem Cache entfernt wird. Sie können auch Abfragen, wie viel Zeit ein Schlüssel hat, Ablauf mit den `TTL` Befehl. Dieser Befehl steht StackExchange Applikationen mit der `IDatabase.KeyTimeToLive` Methode.

Der folgende Codeausschnitt zeigt, wie eine Ablaufzeit von 20 Sekunden auf eine Taste und Gültigkeitsdauer des Schlüssels Abfragen:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

Sie können auch die Ablaufzeit auf ein bestimmtes Datum und eine Uhrzeit festlegen, Befehl laufen, die in der Bibliothek StackExchange der `KeyExpireAsync` Methode:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Tipp:_ Sie können manuell entfernen ein Elements aus dem Cache mit dem Befehl DEL über StackExchange Bibliothek als das `IDatabase.KeyDeleteAsync` Methode.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Verwenden von Tags zum Cross-zwischengespeicherte Elemente korrelieren

Ein Redis ist eine Auflistung mehrere Elemente, einen Schlüssel. Mit dem Befehl SADD können Sie eine Gruppe erstellen. Mit dem Befehl SMEMBERS können Sie die Elemente in einer Gruppe abrufen. StackExchange-Bibliothek implementiert den Befehl SADD der `IDatabase.SetAddAsync` -Methode und den SMEMBERS-Befehl mit der `IDatabase.SetMembersAsync` Methode.

Sie können auch vorhandene legt neue erstellen mithilfe von SDIFF (Differenz), SINTER (Schnittmenge) und SUNION (Vereinigungsmenge) Befehle kombinieren. StackExchange Library vereint diese Vorgänge in den `IDatabase.SetCombineAsync` Methode. Der erste Parameter für diese Methode gibt die Set-Operation durchzuführen.

Die folgenden Codeausschnitte zeigen, wie Gruppen für schnell speichern und Abrufen von Sammlungen verwandter Elemente. Dieser Code verwendet die `BlogPost` Typ im Abschnitt implementieren Redis Cache-Clientanwendungen weiter oben in diesem Artikel beschrieben wurde.

Ein `BlogPost` Objekt enthält vier Felder-ID, einen Titel, eine Rangfolge Bewertung und eine Sammlung von Tags. Der erste nachfolgende Codeausschnitt zeigt die Beispieldaten zum Auffüllen einer C#-Liste der dient `BlogPost` Objekte:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

Speichern der Tags für jedes `BlogPost` als Satz in einem Redis-Cache-Objekt, und ordnen Sie jede mit der ID der `BlogPost`. Dadurch kann eine Anwendung, die einen bestimmten Blogbeitrag gehören die Tags schnell finden. Zu aktivieren, in der entgegengesetzten Richtung alle Blogbeiträge, die ein bestimmtes Tag gemeinsam nutzen, können Sie einen anderen Satz erstellen, Blogeinträge verweisen auf die Tag-ID im Schlüssel:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Diese Strukturen können Sie viele allgemeine Abfragen sehr effizient. Sie können z. B. Suchen und alle Tags für Blogbeitrag 1 wie folgt:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

Finden Sie alle Tags, die in Blog Blog und 1 2 post anhand einer Schnittmenge wie folgt:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

Und alle Blogbeiträge, die bestimmte Tags enthalten:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Elemente suchen, die kürzlich

Eine häufige Aufgabe in vielen Programmen erforderlich ist, finden die meisten zuletzt zugegriffen. Beispielsweise könnte eine Blogwebsite Informationen zum zuletzt gelesenen Blogbeiträge anzeigen möchten.

Sie können dies mithilfe einer Liste Redis implementieren. Eine Redis-Liste enthält mehrere Elemente, die den gleichen Schlüssel. Die Liste dient als Warteschlange Doppelspitze. Sie können Elemente zum Ende der Liste mit LPUSH (links drücken) und RPUSH (rechts drücken) Befehle drücken. Sie können Elemente vom Ende der Liste mit den Befehlen LPOP und RPOP abrufen. Eine Gruppe von Elementen gelangen mit den Befehlen LRANGE und ANORDNEN.

Die folgenden Codeausschnitte anzeigen wie Sie diese Vorgänge können mithilfe der StackExchange-Bibliothek Dieser Code verwendet die `BlogPost` aus den vorherigen Beispielen. Ein Blogbeitrag von einem Benutzer gelesen wird die `IDatabase.ListLeftPushAsync` -Methode legt den Titel des Blogbeitrags auf eine Liste, die mit dem Schlüssel "Blog:recent_posts" im Redis Cache verknüpft ist.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Weitere Blogbeiträge gelesen werden, werden ihre Titel auf derselben Liste abgelegt. Die Liste ist die Reihenfolge geordnet in dem Titel hinzugefügt wurden. Zuletzt gelesenen Blogbeiträge sind linken Ende der Liste. (Dieselbe Blogbeitrag mehrmals gelesen werden, es mehrere Einträge in der Liste haben.)

Zeigen Sie die Titel der zuletzt gelesenen Beiträge mit dem `IDatabase.ListRange` Methode. Diese Methode verwendet den Schlüssel, der Liste einen Anfangspunkt und Endpunkt enthält. Der folgende Code Ruft die Titel 10 Blogbeiträge (Elemente von 0 bis 9) am linken Ende der Liste:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Beachten Sie, dass die `ListRangeAsync` Methode Elemente nicht aus der Liste entfernt. Hierzu können Sie die `IDatabase.ListLeftPopAsync` und `IDatabase.ListRightPopAsync` Methoden.

Damit die Liste nicht unbegrenzt wachsen, können Sie Elemente in regelmäßigen Abständen durch Trimmen der Liste ausgemerzte. Der folgende Codeausschnitt zeigt alle entfernen aber die fünf am weitesten links Elemente aus der Liste:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Implementieren einer Rangliste

Standardmäßig werden die Elemente in einer Gruppe nicht in einer bestimmten Reihenfolge statt. Erstellt eine geordnete Menge Befehl dann (der `IDatabase.SortedSetAdd` -Methode in der StackExchange-Bibliothek). Die Elemente werden mit einem numerischen Wert bezeichnet einen Faktor, die als Parameter für den Befehl bereitgestellt wird sortiert.

Der folgende Codeausschnitt hinzugefügt eine geordnete Liste den Titel eines Blogbeitrags. In diesem Beispiel hat jeder Blogbeitrag ein Faktor Feld mit der Positionierung des Blogbeitrags.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

Sie können abrufen Blog Post Titel und Ergebnisse aufsteigend Bewertung mit dem `IDatabase.SortedSetRangeByRankWithScores` Methode:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] Außerdem stellt die Bibliothek StackExchange die `IDatabase.SortedSetRangeByRankAsync` -Methode gibt die Daten in der Reihenfolge der Bewertung, aber die Ergebnisse nicht zurück.

Sie können auch Elemente in absteigender Reihenfolge der Ergebnisse abrufen und die Anzahl der Elemente zurückgegeben werden, indem zusätzliche Parameter, die `IDatabase.SortedSetRangeByRankWithScoresAsync` Methode. Das nächste Beispiel zeigt die Titel und unzählige Top 10 Platz Blogbeiträge:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

Das nächste Beispiel verwendet die `IDatabase.SortedSetRangeByScoreWithScoresAsync` -Methode, die Sie verwenden können, um die Artikel einzuschränken, die den zurückgegeben werden, die innerhalb einer bestimmten Bewertung Bereich:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Nachricht durch Verwenden von channels

Neben fungiert als einen Datencache bietet einen Server Redis messaging über leistungsfähige Verleger/Abonnent-Mechanismus. Clientanwendungen können einen Channel abonnieren und andere Programme oder Dienste können Nachrichten an den Kanal veröffentlichen. Clientanwendungen abonnieren erhalten Sie diese Nachrichten und verarbeiten Sie können.

Redis bietet der Befehl abonnieren Clientanwendungen Channels abonnieren. Dieser Befehl erwartet Namen einen oder mehrere Kanäle, auf die die Anwendung Nachrichten akzeptieren. Die Bibliothek StackExchange enthält die `ISubscription` -Schnittstelle, die eine.NET Framework-Anwendung abonnieren und Kanäle veröffentlichen kann.

Erstellen einer `ISubscription` Objekt mithilfe der `GetSubscriber` Methode der Verbindung mit dem Server Redis. Und hören Nachrichten über einen Kanal mit dem `SubscribeAsync` -Methode dieses Objekts. Im folgenden Codebeispiel wird veranschaulicht, wie ein Kanal mit dem Namen "Nachrichten: Blogeinträge" abonnieren:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

Der erste Parameter für die `Subscribe` Methode ist der Name des Kanals. Dieser Name folgt die gleichen Konventionen, die Schlüssel im Cache verwendet. Der Namen kann mit relativ kurzen, aussagekräftigen Zeichenfolgen gute Leistung und Wartungsfreundlichkeit ist binären Daten enthalten.

Beachten Sie, dass Kanäle verwendete Namespace vom Schlüssel getrennt ist. Daher haben Sie Kanäle und Schlüssel, die den gleichen Namen haben, obwohl dies Anwendungscode zu erschweren kann.

Der zweite Parameter ist ein Action-Delegaten. Dieser Delegat wird asynchron ausgeführt, wenn eine neue auf den Kanal Meldung. Dieses Beispiel zeigt einfach die Meldung auf der Konsole (die Nachricht wird ein Blogbeitrag enthalten).

Um einen Kanal zu veröffentlichen, können eine Anwendung mit dem Befehl Veröffentlichen Redis. StackExchange-Bibliothek stellt die `IServer.PublishAsync` Methode, um diesen Vorgang auszuführen. Der nächste Codeausschnitt zeigt, wie eine Nachricht an den Kanal "Nachrichten: Blogeinträge" veröffentlicht:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

Es gibt einige Punkte zu veröffentlichen-abonnieren-Mechanismus verstehen sollten:

- Mehrere Abonnenten können denselben Channel abonnieren und alle erhalten die Nachrichten, die an diesen Kanal veröffentlicht werden.
- Abonnenten empfangen nur Nachrichten, die veröffentlicht wurden, nachdem sie abonniert haben. Kanäle sind nicht gepuffert und veröffentlichte eine Nachricht Redis Infrastruktur legt die Nachricht an jeden Abonnenten und anschließend wieder entfernt.
- Standardmäßig werden Nachrichten von Abonnenten in der Reihenfolge empfangen in denen sie gesendet werden. In einem sehr aktiven System mit einer großen Anzahl von Nachrichten und viele Abonnenten und Verlegern kann sequenzieller Zustellung von Nachrichten Leistung des Systems beeinträchtigen. Wenn jede Nachricht ist die Reihenfolge unwichtig ist, können parallele Verarbeitung vom Redis-System Sie die um Reaktionsfähigkeit zu verbessern kann. Sie können in einem StackExchange Client dazu PreserveAsyncOrder verwendete vom Abonnenten auf False festlegen:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Verwandte Muster und Anleitung

Das folgende Muster könnten auch für Ihr Szenario beim Implementieren einer Anwendung Zwischenspeichern:

- [Cache-Aside Muster](http://msdn.microsoft.com/library/dn589799.aspx): Dieses Muster beschreibt, wie Daten bei Bedarf in den Cache aus einem Datenspeicher geladen. Dieses Muster ist auch hilfreich, Konsistenz zwischen Daten im Cache gehalten werden und die Daten im ursprünglichen Datenspeicher.
- [Sharding Muster](http://msdn.microsoft.com/library/dn589797.aspx) enthält Informationen zur Implementierung der horizontalen Partitionierung um Skalierbarkeit bei Speicherung und Zugriff auf große Mengen von Daten zu verbessern.

## <a name="more-information"></a>Weitere Informationen

- Die [MemoryCache](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) Seite auf der Microsoft-website
- [Azure Redis Cache](https://azure.microsoft.com/documentation/services/cache/) -Dokumentationsseite auf der Microsoft-website
- [Azure Redis Cache FAQ](redis-cache/cache-faq.md) -Seite auf der Microsoft-website
- Die Seite [Konfigurationsmodell](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) auf der Microsoft-website
- Die Seite [Aufgabe ereignisbasierte asynchrone Muster](http://msdn.microsoft.com/library/hh873175.aspx) auf der Microsoft-website
- Die [Rohrleitungen und Multiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) Seite REPO-StackExchange.Redis GitHub
- Die [Persistenz Redis](http://redis.io/topics/persistence) Seite auf der Website Redis
- Der [Replikation-Seite](http://redis.io/topics/replication) auf der Website Redis
- Die [Cluster-Lernprogramm Redis](http://redis.io/topics/cluster-tutorial) Seite auf der Website Redis
- Die [Partitionierung: wie Daten zwischen mehreren Instanzen Redis](http://redis.io/topics/partitioning) auf der Website Redis
- Die [Verwendung als LRU-Cache Redis](http://redis.io/topics/lru-cache) Seite auf der Website Redis
- [Transaktionen](http://redis.io/topics/transactions) -Seite auf der Website Redis
- [Sicherheit Redis](http://redis.io/topics/security) Seite auf der Website Redis
- Die Seite [runde Azure Redis Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure-blog
- Die [Ausführung auf einer CentOS Linux VM in Azure Redis](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) Seite auf der Microsoft-website
- [ASP.NET Sitzungszustandsanbieter Azure Redis Cache](redis-cache/cache-aspnet-session-state-provider.md) -Seite auf der Microsoft-website
- [ASP.NET Ausgabecacheanbieter für Azure Redis Cache](redis-cache/cache-aspnet-output-cache-provider.md) -Seite auf der Microsoft-website
- [Eine Einführung in die Datentypen und Abstraktionen Redis](http://redis.io/topics/data-types-intro) Seite auf der Website Redis
- Die [grundlegende Verwendung](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) Seite auf der Website StackExchange.Redis
- Die [Transaktionen in Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) Seite REPO-StackExchange.Redis
- [Data Partitionierung Guide](http://msdn.microsoft.com/library/dn589795.aspx) auf der Microsoft-website

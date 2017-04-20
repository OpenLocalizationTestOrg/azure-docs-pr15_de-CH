<properties
   pageTitle="Daten in DocumentDB mit Gültigkeitsdauer abläuft | Microsoft Azure"
   description="Mit TTL ermöglicht Microsoft Azure DocumentDB Dokumente automatisch nach kurzer Zeit aus dem System gelöscht."
   services="documentdb"
   documentationCenter=""
   keywords="Gültigkeitsdauer"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Daten in DocumentDB Sammlungen automatisch Gültigkeitsdauer abläuft

Applikationen können erzeugen und Daten speichern. Einige dieser Daten wie Computer generierten Daten, Protokolle und Benutzer Veranstaltung Informationen nur für einen begrenzten Zeitraum nützlich ist. Nachdem die Daten zu der Anwendung überschüssige gefahrlos dieser Daten bereinigen und verringern den Speicherbedarf einer Anwendung.

Mit "Live" oder TTL ermöglicht Microsoft Azure DocumentDB Dokumente automatisch nach einiger Zeit aus der Datenbank gelöscht. Die standardmäßige Gültigkeitsdauer kann auf Auflistungsebene festgelegt und jeweils pro Dokument überschrieben. Gültigkeitsdauer festgelegt wird, standardmäßig Sammlung oder auf einer Dokumentebene werden DocumentDB automatisch Dokumente entfernen, die nach dieser Zeitspanne in Sekunden, seit der letzten Änderung vorhanden sind.

DocumentDB Gültigkeitsdauer verwendet einen Offset, wann das Dokument zuletzt geändert wurde. Dazu verwendet das _ts-Feld existiert in jedem Dokument. _Ts ist ein Unix-Format Epoche Zeitstempel, Datum und Uhrzeit darstellt. Das Feld _ts wird jedes Mal aktualisiert ein Dokument geändert wird. 

## <a name="ttl-behavior"></a>TTL-Verhalten

TTL-Eigenschaften auf zwei Ebenen - die Ebene und auf Dokumentebene TTL-Eigenschaft gesteuert. Die Werte werden in Sekunden festgelegt und werden als ein Delta aus der letzten des Dokuments am Änderung _ts.

 1.  DefaultTTL für die Auflistung
  * Fehlt (oder auf null festgelegt) Dokumente werden nicht automatisch gelöscht.
  
  * Ist und der Wert "-1" = unendlich – Dokumente standardmäßig läuft nicht ab
  
  * Wenn Dokumente ablaufen und der Wert ist eine Zahl ("n") Sekunden nach der letzten Änderung "n"

 2.  Gültigkeitsdauer für Dokumente: 
  * Eigenschaft ist nur anwendbar, wenn DefaultTTL der übergeordneten Auflistung vorhanden.
  
  * Überschreibt den Wert DefaultTTL der übergeordneten Auflistung.

Sobald das Dokument abgelaufen ist (Ttl + _ts > = aktuelle Serverzeit), wird das Dokument als "abgelaufen" markiert. Kein Vorgang kann auf diese Dokumente nach dieser Zeit und sie werden nicht in den Suchergebnissen Abfragen ausgeführt. Dokumente werden im System physisch gelöscht und werden gelöscht Ausführung zu einem späteren Zeitpunkt im Hintergrund. Dies verbraucht [Anforderungseinheiten (RUs)](documentdb-request-units.md) aus der Auflistung.

Die obige Logik kann in der folgenden Tabelle dargestellt:

|       | DefaultTTL fehlt nicht in der Auflistung festlegen | DefaultTTL =-1 auf | DefaultTTL = "n" auf|
| ------------- |:-------------|:-------------|:-------------|
| TTL Dokument fehlt| Nichts auf Dokumentebene überschrieben, da das Dokument und Auflistung kein Konzept Gültigkeitsdauer haben. | Keine Dokumente in dieser Auflistung läuft. | Dokumente in dieser Auflistung läuft, wenn n Intervall verstrichen ist. |
| TTL =-1 Dokument | Nichts auf der Ebene überschrieben, da die Auflistung DefaultTTL-Eigenschaft definiert, die ein Dokument überschreiben können. Gültigkeitsdauer für ein Dokument wird vom System nicht interpretiert. | Keine Dokumente in dieser Auflistung läuft. | Das Dokument mit TTL =-1 in dieser Auflistung abläuft nie. Alle anderen Dokumente verfallen nach "n"-Intervall. |
|  TTL = n Dokument | Nichts auf der Ebene der überschreiben. TTL-Wert für ein Dokument in das System nicht interpretiert. | Das Dokument mit TTL = n n Intervall in Sekunden läuft. Andere Dokumente erben Intervall 1 und läuft nie ab. | Das Dokument mit TTL = n n Intervall in Sekunden läuft. Andere Dokumente erben "n" Intervall aus der Auflistung. |


## <a name="configuring-ttl"></a>Konfigurieren der Gültigkeitsdauer

Gültigkeitsdauer ist standardmäßig in allen DocumentDB Sammlungen und alle Dokumente deaktiviert.

## <a name="enabling-ttl"></a>Aktivieren der Gültigkeitsdauer

Um TTL auf eine Auflistung oder die Dokumente in einer Auflistung zu aktivieren, müssen Sie DefaultTTL-Eigenschaft einer Auflistung-1 oder eine positive Zahl ungleich NULL festgelegt. Festlegen der DefaultTTL auf-1 wird, standardmäßig alle Dokumente in der Auflistung immer Leben der DocumentDB sollte Dienst dieser Sammlung für Dokumente überwachen, die diese Standardeinstellung überschrieben haben.

## <a name="configuring-default-ttl-on-a-collection"></a>Konfigurieren der Standardgültigkeitsdauer einer Auflistung

Sie können eine Standardzeit Live auf einer Ebene konfigurieren. 

Um eine Auflistung die Gültigkeitsdauer festzulegen, müssen Sie eine positive Zahl ungleich NULL angeben, die die Zeitspanne in Sekunden, alle Dokumente in der Auflistung nach den letzten geänderten Zeitstempel des Dokuments (_ts) ablaufen angibt.

Oder Sie legen die Standard-1, was bedeutet, dass alle Dokumente in der Auflistung eingefügt unbegrenzt standardmäßig Leben.

## <a name="setting-ttl-on-a-document"></a>Einstellung Gültigkeitsdauer für ein Dokument

Neben einem Standard-TTL für eine Auflistung können Sie bestimmte TTL Ebene Dokument festlegen. Dadurch wird die Standardeinstellung der Auflistung überschreiben.

Um die Gültigkeitsdauer eines Dokuments festzulegen, müssen Sie eine positive Zahl ungleich NULL angeben, die womit die Zeitspanne in Sekunden nach der letzten geänderten Zeitstempel des Dokuments (_ts) das Dokument abläuft.

Um diesen Ablauf Offset festzulegen, legen Sie das TTL-Feld im Dokument.

Hat ein Dokument kein TTL-Feld wird standardmäßig der Auflistung anwenden.

Wenn TTL auf Auflistungsebene deaktiviert ist, ignoriert das TTL-Feld im Dokument bis TTL für die Auflistung erneut aktiviert wird.


## <a name="extending-ttl-on-an-existing-document"></a>Erweitern der Gültigkeitsdauer auf einem vorhandenen Dokument

Jeder Schreibvorgang für das Dokument ausführen können Sie die Gültigkeitsdauer eines Dokuments zurücksetzen. Dadurch wird die _ts auf die aktuelle Zeit eingestellt und beginnt der Countdown bis zum Ablauf Dokument wie die Gültigkeitsdauer wieder.

Wenn Sie die Gültigkeitsdauer eines Dokuments ändern möchten, können Sie wie andere einstellbare Feld Feld aktualisieren.


## <a name="removing-ttl-from-a-document"></a>TTL entfernen aus einem Dokument

Wenn eine Gültigkeitsdauer für ein Dokument festgelegt wurde und nicht mehr das Dokument ablaufen soll, können Sie das Dokument abrufen, entfernen das TTL-Feld und das Dokument auf dem Server ersetzen.

Wenn das TTL-Feld aus dem Dokument entfernt wird, wird der Standardwert der Auflistung angewendet.

Ein Dokument abläuft und erbt nicht von der Auflistung müssen Sie den TTL-Wert auf-1 gesetzt.


## <a name="disabling-ttl"></a>Deaktivieren der Gültigkeitsdauer

Eine Auflistung TTL vollständig deaktivieren und beenden Hintergrundprozess aus für abgelaufene Dokumente, die DefaultTTL-Eigenschaft der Auflistung gelöscht werden soll.

Das Löschen dieser Eigenschaft ist anders auf-1 festgelegt. Einstellung-1 bedeutet, dass neue Dokumente, die der Auflistung hinzuzufügenden Leben, jedoch können Sie überschreiben, auf bestimmte Dokumente in der Auflistung.

Diese Eigenschaft vollständig aus der Auflistung entfernt also keine Dokumente ablaufen, auch wenn Dokumente, die explizit eine frühere Standard überschrieben haben.


## <a name="faq"></a>Häufig gestellte Fragen

**Was wird TTL Kosten?**

Es ist ohne zusätzliche Kosten zum Festlegen einer Gültigkeitsdauer für ein Dokument.

**Wie lange dauert das Dokument löschen, nachdem die Gültigkeitsdauer abgelaufen ist?**

Die Dokumente werden als nicht verfügbar gekennzeichnet, sobald das Dokument abgelaufen ist (Ttl + _ts > = aktuelle Serverzeit). Kein Vorgang kann auf diese Dokumente nach dieser Zeit und sie werden nicht in den Suchergebnissen Abfragen ausgeführt. Die Dokumente werden physisch vom System im Hintergrund gelöscht. Dies verbraucht keine RUs aus der Auflistung.

**Dauert eine Zeitspanne Dokumente löschen zählen sie meine Kontingent (und Stückliste) bis sie gelöscht werden?**

Nein, sobald das Dokument abgelaufen ist Sie nicht für die Speicherung dieser Dokumente belastet und Größe der Dokumente wird nicht auf das Speicherkontingent für eine Auflistung.

**Wird Gültigkeitsdauer eines Dokuments auf RU auswirken?**

Nein, es werden keine Auswirkung RU Zuschläge für Operationen auf jedes Dokument in DocumentDB.

**Wird das Löschen der Dokumente Auswirkung auf den Durchsatz, auf Meine Bereitstellung?**

Nein, gegen die Sammlung mit Priorität erhalten Hintergrundprozess zum Löschen von Dokumenten ausgeführt. Jedes Dokument TTL hinzufügen wird dies nicht auswirken.

**Wenn ein Dokument abläuft, wie lange bleibt es in meiner Sammlung bis gelöscht wird?**

Sobald das Dokument abläuft werden nicht mehr zugegriffen. Die Uhrzeit wird ein Dokument in der Auflistung bleiben, bevor tatsächlich gelöscht nicht deterministisch ist und basiert auf wird im Hintergrundprozess das Dokument löschen.

**Werden abgelaufene Dokumente auf allen Knoten gelöscht oder ist "schließlich Consistent?"**

Das Dokument wird gleichzeitig auf allen Knoten und in allen Regionen verfügbar.

**Gibt es eine RU Kosten für Hintergrundaufgaben TTL überwacht?**

Nein, es gibt keine RU Kosten für diese.

**Wie oft werden TTL Ablaufzeiten überprüft?**

TTL Ablaufzeiten Überprüfen auftreten nicht als Hintergrundprozess. Als Reaktion auf eine Back-End-Dienst tun Inline überprüft und schließen alle Dokumente, die abgelaufen sind. Das Löschen des physischen Dokuments ist der einzige Prozess, der asynchron im Hintergrund ausgeführt wird. Die Häufigkeit dieses Vorgangs bestimmt der verfügbaren RUs für die Auflistung.

**Gilt TTL-Eigenschaft nur für ganze Dokumente oder können Eigenschaftswerte Dokument abläuft?**

TTL gilt für das gesamte Dokument. Möchten Sie nur einen Teil eines Dokuments abläuft, wird empfohlen, den Teil aus dem Hauptdokument in ein separates Dokument "verknüpft" extrahieren und anschließend das extrahierte Dokument TTL verwenden.

**Verfügt die TTL Funktion Indizierung erfüllen müssen?**

Ja. Die Auflistung muss auf Lazy oder konsistent [Indizierung Richtlinie festgelegt](documentdb-indexing-policies.md) . DefaultTTL in einer Auflistung mit soll keine Indizierung festlegen führt ein Fehler in einer Auflistung mit einem bereits DefaultTTL Indizierung deaktivieren möchten.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Azure DocumentDB finden Sie in [*der Service-Dokumentationsseite*](https://azure.microsoft.com/documentation/services/documentdb/) .





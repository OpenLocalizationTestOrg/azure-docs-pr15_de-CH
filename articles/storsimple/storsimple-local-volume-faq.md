<properties 
   pageTitle="StorSimple lokal fixiert Volumes FAQ | Microsoft Azure"
   description="Enthält Antworten auf häufig gestellte Fragen zu lokal fixiert StorSimple-Volumes."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple lokal fixiert Volumes: häufig gestellte Fragen (FAQ)

## <a name="overview"></a>Übersicht

Folgende befinden Fragen und Antworten, die Sie lokal fixiert StorSimple Volume erstellen, tiered Volume auf lokalen Volumes und umgekehrt konvertieren oder Sichern und Wiederherstellen eines lokales Volumes.

Fragen und Antworten sind in die folgenden Kategorien eingeteilt.

- Erstellen von lokalen Volumes
- Ein lokales sichern
- Konvertieren eines gestuften Volumes zu lokalen Volumes
- Ein lokales Volume wiederherstellen
- Failover von lokalen Volumes

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Fragen zum Erstellen von lokalen Volumes

**Q.** Was ist die maximale Größe eines lokalen Volumes auf Geräten 8000-Serie erstellbaren?

**Eine** Bereitstellung Sie lokale Volumes bis zu 8,5 TB oder mehrstufige Volumes bis zu 200 TB auf dem Gerät 8100. Auf größeren 8600-Gerät können Sie lokale Volumes bereitstellen bis zu 22,5 TB oder mehrstufige Volumes bis zu 500 TB.

**Q.** Ich habe kürzlich meinen 8100 Gerät Update 2 und beim Erstellen eines lokales Volumes ist der maximal verfügbare Speicherplatz nur 6 TB und nicht 8,5 TB. Warum werden kann nicht 8,5 TB-Volume erstellt?

**Eine** Bereitstellung Sie lokale Volumes bis zu 8,5 TB oder Ebenen Volumes auf dem Gerät 8100 bis zu 200 TB. Wenn Ihr Gerät bereits Volumes werden Speicherplatz zum Erstellen von lokalen Volumes proportional niedriger als dieser Höchstgrenze gestuft wurde. Beispielsweise wenn 100 TB mehrstufige Volumes bereits auf Ihrem Gerät 8100 bereitgestellt werden (die Hälfte der tiered Kapazität), wird dann die maximale Größe eines lokalen Datenträgers auf 8100 Gerät erstellen entsprechend auf 4 TB reduziert (etwa angeheftet halber lokal Kapazität).

Da das Workingset mehrstufige Volumes hosten lokalen Speicherplatz auf dem Gerät verwendet wird, wird Speicherplatz zum Erstellen von lokalen Volumes reduziert, wenn das Gerät Volumes gestuft wurde. Erstellen von lokalen Volumes proportional verringert dagegen Platz für mehrstufige Volumes. In der folgenden Tabelle werden verfügbare tiered Kapazität auf der 8100 und 8600 zusammengefasst, wenn lokale Volumes erstellt werden.

|Lokale Volumes bereitgestellten Kapazität|Verfügbare Kapazität für mehrstufige Volumes - 8100 bereitgestellt werden|Verfügbare Kapazität für mehrstufige Volumes - 8600 bereitgestellt werden|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176,5 TB | 477,8 TB|
|4 TB | 105,9 TB | 411,1 TB |
|8.5 TB | 0 TB | 311,1 TB|
|10 TB | NA | 277,8 TB |
|15 TB | NA | 166.7 TB |
|22,5 TB | NA | 0 TB |


**Q.** Warum lokales Volume-Erstellung langwierigen Operationen ist? 

**A.** Lokale Volumes werden dicht bereitgestellt. Um Speicherplatz auf lokalen Ebenen des Geräts zu erstellen, möglicherweise einige Daten aus vorhandenen tiered Bereitstellungsprozess in die Cloud abgelegt werden. Und da dies auf die Größe des Volumes bereitgestellt werden, die vorhandenen Daten auf dem Gerät und der verfügbaren Bandbreite in der Cloud Die Zeit zum Erstellen eines lokalen Volumes mehrere Stunden.

**Q.** Wie lange dauert ein lokales Volume erstellen?

**A.** Da lokale Volumes dick bereitgestellt werden, können vorhandenen Daten aus tiered Bereitstellungsprozess in der Cloud abgelegt. Daher hängt die Zeit zum Erstellen eines lokalen Volumes mehrere Faktoren die Größe des Datenträgers, die Daten auf dem Gerät und der verfügbaren Bandbreite. Auf eine neu installierte Gerät, das keine Volumes enthält, ist ein lokales Volume erstellt ungefähr 10 Minuten pro Terabyte an Daten. Erstellung von lokalen Volumes kann jedoch auf einem Gerät wird oben erläuterten Faktoren mehrere Stunden dauern.

**Q.** Ein lokales Volume erstellt werden soll. Gibt es bewährte Verfahren zu müssen?

**A.** Lokale Volumes sind für Arbeitslasten, die erfordern lokaler Garantien Daten jederzeit und Wartezeiten cloud sind. Berücksichtigung der Verwendung von lokalen Volumes für alle Ihre Arbeitslasten Beachten Sie Folgendes:

- Lokale Volumes dick bereitgestellt und Erstellen von lokalen Volumes Speicherplatz für mehrstufige Volumes auswirkt. Daher empfehlen wir mit kleineren starten und zunehmender Bedarf der Speicher skalieren.

- Bereitstellung von lokalen Datenträgern ist eine umfangreiche Operation, bei denen möglicherweise vorhandene Daten aus tiered in die Cloud verschieben. Daher möglicherweise Leistungseinbußen auf diesen Datenträger.

- Bereitstellung von lokalen Volumes ist verriegelter. Die tatsächliche Zeit hängt von mehreren Faktoren ab: der Größe des Volume bereitgestellt wird, Daten auf dem Gerät und Bandbreite. Wenn Sie in der Cloud nicht vorhandenen Datenträger gesichert wurden, ist Volumeerstellung langsamer. Wir empfehlen Sie Schnappschüsse Cloud der vorhandenen Volumes vor einem lokalen Datenträger bereitstellen.
 
- Lokale Volumes vorhandenen mehrstufige Volumes konvertieren und diese Konvertierung beinhaltet Bereitstellung von Speicherplatz auf dem Gerät für die resultierende lokalen Volumes (zusätzlich zu bringen tiered Daten [sofern vorhanden] aus der Cloud). Dies ist wiederum eine umfangreiche Operation, die oben besprochenen Faktoren abhängig. Wir empfehlen, Ihre bestehenden Volumes vor der Konvertierung zu sichern, wie der Prozess auch langsamer, wenn vorhandene Volumes nicht gesichert werden. Das Gerät kann auch Leistung während dieses Vorgangs auftreten.
    
Weitere Informationen zum [Erstellen von lokalen Volumes](storsimple-manage-volumes-u2.md#add-a-volume)

**Q.** Kann ich mehrere lokale Volumes gleichzeitig erstellen?

**A.** Ja, aber alle lokalen Volumes auf- und Ausbau der Aufträge werden sequenziell verarbeitet.

Lokale Volumes dick bereitgestellt und erfordert Erstellung von lokalen Speicherplatz auf dem Gerät (wodurch vorhandene Daten mehrstufige Volumes bei der Bereitstellung in der Cloud abgelegt werden kann). Daher Wenn einen Bereitstellungsauftrag ausgeführt wird, werden andere Aufträge zum Erstellen von lokalen Volumes in der Warteschlange werden bis zum Abschluss dieses Auftrags.

Ebenso wird ein lokales Volume erweitert oder Volumina tiered zu lokalen Volumes konvertiert wird, dann die Erstellung einer neuen lokalen Volumes zurückgestellt, bis der vorherige Einzelvorgang abgeschlossen ist. Vergrößerung des lokalen Volumes umfasst die Erweiterung der vorhandenen lokalen Speicherplatz für das Volume. Konvertierung von tiered lokal fixiert Volume erfordert die Erstellung von lokalen Speicherplatz für das resultierende lokal Volumen fixiert. In beide dieser Operationen, Erstellung oder Erweiterung des lokalen langer Auftrag ausgeführt wird.

Sie können diese Aufträge auf **Aufträge** Azure StorSimple Manager-Dienst anzeigen. Auftrag, der aktiv verarbeitet wird ständig aktualisiert den Status der Bereitstellung von Speicherplatz. Die übrigen lokalen Volumes Druckaufträge als ausgeführt markiert aber deren Status angehalten und werden entnommen, in der sie in die Warteschlange gestellt wurden.

**Q.** Ein lokales Volume gelöscht. Warum angezeigt nicht freigegebenen Bereich wirkt sich der verfügbare Speicherplatz beim Erstellen eines neuen Datenträgers? 

**A.** Wenn Sie ein lokales Volume löschen, kann Platz für neue Volumes nicht sofort aktualisiert. StorSimple Manager-Dienst wird im lokalen Speicherplatz ca. stündlich aktualisiert. Wir empfehlen, warten Sie eine Stunde vor das neue Volume erstellen.

**Q.** Unterstützt lokale Volumes auf der Cloud Appliance werden?

**A.** Lokale Volumes werden auf Cloud Appliance (ehemals StorSimple virtuelles Gerät 8010 und 8020-Geräte) nicht unterstützt.

**Q.** Kann ich die Azure PowerShell-Cmdlets erstellt und verwaltet lokale Volumes verwenden? 

**A.** Leider kann keine lokale Volumes über Azure PowerShell-Cmdlets erstellen (ein Volume über Azure PowerShell erstellten Ebenen). Außerdem sollten Sie nicht Azure PowerShell-Cmdlets verwenden, um Eigenschaften des lokalen Volumes ändern wie den unerwünschten Effekt den Datenträgertyp ändern müssen zu tiered.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Fragen zum Sichern von lokalen Volumes

**Q.** Werden lokale Snapshots lokale Volumes unterstützt?

**A.** Ja, nehmen Sie lokale Snapshots der lokalen Volumes. Allerdings empfehlen wir, dass Sie regelmäßig sichern Ihre lokale Volumes mit Cloud-Snapshots sicherstellen, dass Ihre Daten im Falle eines Notfalls geschützt ist.

**Q.** Gibt es Richtlinien für lokale Snapshots für lokale Volumes verwalten?

**A.** Häufige lokale Snapshots neben hohe Daten Abwanderung in lokalen Volumes möglicherweise lokalen Speicherplatz auf dem Gerät zu schnell verbraucht werden Daten aus tiered zur Cloud verschoben. Wir empfehlen daher die Anzahl der lokale Snapshots minimiert.

**Q.** Ich erhalten eine Benachrichtigung, dass meine lokale Snapshots lokale Volumes möglicherweise ungültig gemacht. Wann kann das passieren?

**A.** Häufige lokale Snapshots neben hohe Daten Abwanderung in lokalen Volumes möglicherweise lokalen Speicherplatz auf dem Gerät schnell verbraucht werden. Bei lokalen Ebenen des Geräts stark Ausfalls erweiterte Cloud möglicherweise das Gerät voll und Ungültigkeit Snapshots führen eingehenden Schreibvorgänge auf dem Datenträger (wie kein Platz zum Aktualisieren der Snapshots auf ältere Datenblöcke, die überschrieben wurden). In einer solchen Situation die Schreibvorgänge auf dem Datenträger weiterhin bedient werden jedoch lokale Snapshots ist möglicherweise ungültig. Es gibt keine Auswirkung auf die vorhandenen Cloud-Snapshots.

Die Warnung wird benachrichtigt, dass eine solche Situation entstehen und sicherzustellen, dass Sie der Adresse rechtzeitig Ihre lokale Snapshots Zeitpläne seltener lokale Snapshots zu überprüfen oder löschen ältere lokale Snapshots nicht mehr benötigt werden.

Lokale Snapshots ungültig sind, erhalten Sie eine Warnung, dass der lokale Snapshots für bestimmte Sicherungsrichtlinie neben der Liste der Zeitstempel die lokale Snapshots ungültig gewordene, die ungültig wurden. Diese Snapshots werden automatisch gelöscht und nicht mehr in der Seite **Backup-Kataloge** im klassischen Azure-Portal angezeigt werden.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Fragen zur lokalen Volumes tiered Volume umwandeln

**Q.** Ich beobachte einige Langsamkeit auf dem Gerät beim Konvertieren einer tiered Volumes auf lokalen Volumes. Warum geschieht dies? 

**A.** Der Konvertierungsvorgang besteht aus zwei Schritten: 

  1. Bereitstellung von Speicherplatz auf dem Gerät für die bald-zu--konvertiert werden lokal fixiert Volume.
  2. Garantiert tiered Datendownload aus der Cloud lokalen sicherstellen.

Beide Schritte werden lange Betrieb, der die Größe der konvertierten Daten auf dem Gerät verfügbare Bandbreite Datenträger abhängig sind. Da einige Daten aus vorhandenen tiered in die Cloud als Teil des Bereitstellungsprozesses verschütten können, kann das Gerät Leistung während dieser Zeit auftreten. Darüber hinaus kann der Konvertierungsprozess langsamer wenn:

- Vorhandenen Volumes wurden nicht in die Cloud gesichert; Daher wir sichern Ihre Volumes empfehlen vor der Konvertierung starten.

- Drosselung Bandbreitenrichtlinien gelten die kann die verfügbare Bandbreite in die Cloud einschränken; Daher wird empfohlen, einen dedizierten 40 Mbit/s oder mehr in die Cloud haben.

- Konvertierung dauert mehrere Stunden durch mehrere Faktoren oben; Wir empfehlen daher zum Ausführen dieses Vorgangs nicht Spitzen Zeiten oder am Wochenende zu die Auswirkung auf die Verbraucher.

Weitere Informationen zum [Konvertieren eines gestuften Volumes auf lokalen Volumes](storsimple-manage-volumes-u2.md#change-the-volume-type)

**Q.** Kann den Konvertierungsvorgang Volume Abbrechen?

**A.** Nein, nicht abbrechen den Konvertierungsvorgang einmal ausgelöst. Wie in der vorherigen Frage erläutert Beachten Sie Leistungsprobleme während des Prozesses auftreten können, und führen Sie beim Planen der Konvertierung aufgeführten Vorgehensweisen.

**Q.** Was passiert mit meinem, wenn die Konvertierung nicht durchgeführt?

**A.** Volume-Konvertierung kann durch Cloud-Verbindungsproblemen fehl. Das Gerät reagiert schließlich Konvertierung nach einer Reihe fehlgeschlagener Versuche, tiered Daten in die Cloud bringen. In diesem Fall der Datenträgertyp weiterhin die Herkunftsart Volumes vor der Konvertierung und:

- Wichtige Warnung erhöht Sie Konvertierungsfehler Volume benachrichtigt. Weitere Informationen über [Alarme Bezug auf lokale volumes](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Wenn Sie eine mehrschichtige auf lokalen Volumes konvertieren, wird der weiter Eigenschaften eines gestuften Volumes aufweisen, wie Daten noch auf die Cloud befinden können. Wir empfehlen die Verbindungsprobleme beheben und wiederholen Sie den Konvertierungsvorgang.
 
- Auch wenn Konvertierung ein lokales tiered Volume fehlschlägt, obwohl das Volume als lokales Volume markiert werden funktioniert als tiered Datenträger es (da Daten in der Cloud verschüttet haben könnte). Jedoch weiterhin Platz auf lokalen Ebenen des Geräts. Dieser Speicherplatz wird für andere lokale Volumes nicht verfügbar. Wiederholen Sie diesen Vorgang, um sicherzustellen, dass die Volume-Konvertierung abgeschlossen ist und lokale Speicherplatz auf dem Gerät freigegeben werden sollten.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Fragen zum lokalen Volumes wiederherstellen

**Q.** Lokale Volumes sofort wiederhergestellt werden?

**A.** Ja, lokale Volumes werden sofort wiederhergestellt. Als die Metadaten für das Volume aus der Cloud als Teil des Wiederherstellungsvorgangs gezogen wird, der Datenträger online geschaltet und vom Host zugegriffen werden kann. Jedoch verringert lokale Garantien für das Volume Daten nicht vorhanden, bis alle Daten aus der Cloud heruntergeladen wurde, treten unter Umständen Leistung auf diese Volumes für die Dauer der Wiederherstellung.

**Q.** Wie lange dauert ein lokales Volume wiederherstellen?

**A.** Lokale Volumes werden sofort wiederhergestellt und online geschaltet, wie Volume-Metadaten aus der Cloud abgerufen wird, während die Datenmengen weiterhin im Hintergrund heruntergeladen werden. Dieser letzte Teil der Wiederherstellung – lokalen Garantien für die Volume-Daten wieder ist eine umfangreiche Operation und kann mehrere Stunden dauern aller Daten auf lokalen erneut gestellt werden. Zeit dasselbe abgeschlossen hängt von mehreren Faktoren wie die Größe des Volumes wiederhergestellt wird und die verfügbare Bandbreite. Wenn das ursprüngliche Volume wiederhergestellt wird gelöscht, zusätzliche Zeit erfolgt zu lokalen Speicherplatz auf dem Gerät als Teil des Wiederherstellungsvorgangs.

**Q.** Muss ich meine vorhandenen lokalen Volumes Wiederherstellen einer älteren Momentaufnahme (wenn der Datenträger gestuft wurde). Wird die Lautstärke werden wiederhergestellt, wie in diesem Fall tiered?

**A.** Nein, wird das Volume als lokales Volume wiederhergestellt. Obwohl Snapshot Daten auf die Zeit, wenn das Volume gestuft wurde, während der Wiederherstellung vorhandenen Volumes StorSimple Typ des Datenträgers auf dem Datenträger immer verwendet aktuell vorhanden.

**Q.** Ich meine lokalen Volumes kürzlich erweitert, aber nun muss kleiner das Volumen als der Daten nacheinander. Wird das aktuelle Volume wiederherstellen angepasst und müssen die Größe des Volumes erweitern, nachdem die Wiederherstellung abgeschlossen ist?

**A.** Ja, die Wiederherstellung wird die Lautstärke angepasst und Sie müssen die Größe des Volumes erweitern, nachdem die Wiederherstellung abgeschlossen ist.

**Q.** Kann ich den Typ eines Volumes während der Wiederherstellung ändern?

**A.** Nein, können Sie den Datenträgertyp während der Wiederherstellung ändern.

- Gelöschte Volumes werden Dateityp gespeichert in der Momentaufnahme wiederhergestellt.

- Vorhandenen Volumes wiederhergestellt werden basierend auf ihrer aktuellen, unabhängig von der Art des Snapshots gespeichert (siehe die vorherigen zwei Fragen).
 
**Q.** Ich meine lokale Volumes wiederherstellen, aber falschen Point in Time-Snapshot ausgewählt. Kann den aktuellen Wiederherstellungsvorgang abbrechen?

**A.** Ja, können Sie eine Wiederherstellung bei Bedarf Abbrechen. Der Status des Volumes wird in den Zustand zu Beginn der Wiederherstellung zurückgesetzt. Alle Schreibvorgänge auf dem Datenträger wurden während die Wiederherstellung wurde werden unterbrochen.

**Q.** Habe eine Wiederherstellung eines Meine lokale Volumes und habe einen Snapshot in meinem Backlog-Katalog, die ich nicht erstellen erinnern. Wozu dient dies?

**A.** Dies ist die temporäre Snapshots, die vor der Wiederherstellung erstellt und Rollback bei die Wiederherstellung abgebrochen wird oder fehlschlägt. Dieser Snapshot nicht gelöscht. Es wird automatisch gelöscht, wenn die Wiederherstellung abgeschlossen ist. Dieses Verhalten kann auftreten, wenn der Wiederherstellungsauftrag nur lokale Volumes oder eine Kombination aus lokal fixierte und tiered-Volumes fixiert wurde. Wenn der Wiederherstellungsauftrag mehrstufige Volumes enthält, wird dieses Verhalten nicht auftreten.

**Q.** Kann ein lokales Volume Klonen?

**A.** Ja, Sie können. Lokales Volume wird jedoch standardmäßig als tiered Volume geklont werden. Weitere Informationen zum [Klonen einer lokalen Volumes](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Fragen zur Failover lokalen Volumes

**Q.** Muss ein Failover zu einem anderen Gerät. Werden meine lokale Volumes werden ausgeführt wie lokal fixiert oder Ebenen?

**A.** Je nach Software des Zielgeräts werden lokale Volumes als Failover:

- Lokal fixiert läuft das Zielgerät StorSimple 8000 aktualisieren Serie 2
- Läuft das Gerät StorSimple Tiered aktualisieren 8000-Serie 1.x
- Ebenen ist das Zielgerät Cloud Appliance (Version Softwareupdate 2 oder update 1.x)

Weitere Informationen zum [Failover und DR fixiert Volumes Versionen lokal](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**Q.** Werden lokale Volumes während Disaster Recovery (DR) sofort wiederhergestellt?

**A.** Ja, werden lokale Volumes sofort bei einem Failover wiederhergestellt. Als die Metadaten für das Volume aus der Cloud als Teil eines Failover gezogen wird, das Volume auf dem Zielgerät online geschaltet und vom Host zugegriffen werden kann. In der Zwischenzeit die Datenmengen weiterhin im Hintergrund downloaden und treten Leistungseinbußen auf diese Volumes für die Dauer des Failovers.

**Q.** Der Failover abgeschlossen, wie ich verfolgen den Fortschritt des lokalen Datenträgers wiederhergestellt wird auf dem Zielgerät angezeigt?

**A.** Während eines Failover wird der Failover-Auftrag als einmal alle Volumes in den Failover sofort wiederhergestellt und auf dem Zielgerät online geschaltet wurden abgeschlossen markiert. Dazu gehören alle lokalen Volumes möglicherweise Failover ausgeführt wurde. lokale Garantien der Daten ist jedoch nur verfügbar, wenn alle Daten für den Datenträger heruntergeladen wurde. Sie können diese für jeden lokalen Volumes verfolgen, die Fehler wurde durch Überwachung der entsprechenden Aufträge, die als Teil des Failovers erstellt werden. Diese individuelle Wiederherstellungsaufträge werden nur für lokale Volumes erstellt werden.

**Q.** Kann ich den Typ eines Volumes während des Failovers ändern?

**A.** Nein, können Sie den Datenträgertyp während eines Failovers ändern. Wenn Sie nicht über auf einem anderen physischen Gerät StorSimple 8000 2 Serie aktualisieren, Volumes werden Failover basierend auf dem Volume in der Momentaufnahme gespeichert. Wenn in einer beliebigen anderen Gerät Version, finden Sie die Frage über den Datenträgertyp nach einem Failover.

**Q.** Kann ein volumecontainer mit lokale Volumes zur Cloud Appliance werden Failover?

**A.** Ja, Sie können. Lokale Volumes werden als mehrstufige Volumes Failover. Weitere Informationen zum [Failover und DR fixiert Volumes Versionen lokal](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)



<properties
    pageTitle=": Azure Premium Speicherentwurf Leistung | Microsoft Azure"
    description="Entwerfen Sie Azure Premium-Speicher mit hoher Performance Applications. Premium-Speicher unterstützt hohe Leistung, niedriger Latenz Disk I/O-Intensive Arbeitslasten auf Azure virtuellen Computer ausgeführt."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>: Azure Premium Speicherdesign für hohe Performance

## <a name="overview"></a>Übersicht  
Dieser Artikel enthält Richtlinien zum Erstellen von hochleistungsfähigen Azure Premium Speicher. Können Sie die Anleitung in diesem Dokument zusammen mit best Practices zur Performance für Technologie, die von der Anwendung verwendet. Zur Veranschaulichung der Richtlinien haben wir Premium Speicher so in diesem Dokument unter SQL Server verwendet.

Während wir Performance für die Speicherschicht in diesem Artikel Szenarios, müssen Sie die Anwendungsebene zu optimieren. Z. B. Wenn Sie eine SharePoint-Farm Azure Premium Speicher verwalten, können die SQL Server-Beispiele aus diesem Artikel Sie Datenbankserver zu optimieren. Darüber hinaus optimieren Sie Webserver und Anwendungsserver zu den meisten Performance der SharePoint-Farm.

In diesem Artikel werden allgemeine Fragen zum Optimieren der Leistung der Anwendung in Azure Premium Storage beantworten,

-   Wie messen Sie die Leistung Ihrer Anwendung?  
-   Warum sehen Sie nicht erwartete hohe Leistung?  
-   Welche Faktoren beeinflussen die Leistung Ihrer Anwendung auf Premium-Speicher?  
-   Wie beeinflussen diese Faktoren Leistung Ihrer Anwendung auf Premium-Speicher?  
-   Wie optimieren Sie IOPS, Bandbreite und Latenz?  

Wir haben diese Richtlinien für Premium-Speicher bereitgestellt, da Arbeitslasten auf Premium-Speicher sehr empfindlich sind. Wir haben gegebenenfalls Beispiele. Sie können diese Richtlinien auf Applikationen IaaS VMs mit Standard-Datenträger anwenden.

Bevor Sie beginnen, können Sie neue Premium-Speicher sind zuerst lesen die [Premium Speicher: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](storage-premium-storage.md) Artikel und [Azure Premium Speicher Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Anwendung-Leistungsindikatoren  
Wir prüfen, ob eine Anwendung funktioniert nicht mit Performance-Indikatoren wie, wie schnell eine Anwendung Benutzeranfrage eine Anwendung pro Anforderung verarbeitet Daten verarbeitet, wie viele fordert eine anwendungsverarbeitung in einem bestimmten Zeitraum, wie lange ist ein Benutzer warten auf eine Antwort nach ihrer Anfrage erhalten. Technische Begriffe für diese Leistungsindikatoren sind IOPS, Durchsatz, Bandbreite und Latenz.

In diesem Abschnitt werden allgemeine Leistungsindikatoren im Bereich Storage Premium besprochen. Im folgenden Abschnitt Sammeln von Anforderungen erfahren Sie, wie diese Leistungsindikatoren für Ihre Anwendung. In Anwendungsleistung optimieren lernen Sie die Faktoren, die diese Leistungsindikatoren und Vorschläge zu optimieren.

## <a name="iops"></a>IOPS  
IO ist der Anfragen, die die Anwendung auf dem Datenträger pro Sekunde sendet. Ein e/a-Vorgang konnte lesen oder schreiben, sequenziell oder zufällig. OLTP-Anwendung wie eine Website Online-Handel müssen gleichzeitigen Benutzeranfragen sofort zu verarbeiten. Die Benutzer werden einfügen und aktualisieren intensive Datenbanktransaktionen, die Anwendung schnell verarbeiten müssen. OLTP-Anwendung erfordert daher sehr hohe IOPS. Anträge verarbeiten Millionen kleine und zufälligen e/a-Anfragen. Wenn Sie eine Anwendung haben, müssen Sie Anwendungsinfrastruktur IOPS optimieren entwerfen. Im späteren Abschnitt *Anwendungsperformance optimieren*besprechen wir ausführlich die Faktoren, die zu hohe IOPS berücksichtigt werden müssen.

Wenn Sie einen Premium-Speicher Datenträger einen hohen Maßstab VM Azure Vorschriften für Sie eine garantierte Anzahl von IOPS gemäß der Spezifikation Datenträger zuordnen. P30 Datenträger stellt z. B. 5000 IOPS. Hohe Skalierung virtueller Speicher ist auch bestimmte IOPS beschränkt, die er verarbeiten kann. Beispielsweise hat eine standardmäßige GS5 VM 80.000 IOPS beschränken.

## <a name="throughput"></a>Durchsatz  
Durchsatz oder Bandbreite ist die Menge der Daten, die die Anwendung auf dem Datenträger in einem angegebenen Intervall senden. Wenn Ihre Anwendung Input-Output-Operationen mit großen e/a-Gerät durchführen, erfordert einen hohen Durchsatz. Data Warehouse-Anwendung tendenziell intensive Scanvorgänge aus, die Zugriff auf große Teile der Daten gleichzeitig und häufig Massenvorgänge ausführen. Anträge müssen also höheren Durchsatz. Haben Sie eine Anwendung entwerfen Sie die Infrastruktur Durchsatz zu optimieren. Im nächsten Abschnitt besprechen wir ausführlich Faktoren dazu optimieren muss.

Wenn Sie einen Premium Speicher Datenträger einen hohen Maßstab VM Azure Vorschriften Durchsatz gemäß dieser Spezifikation Datenträger installieren. P30 Datenträger stellt beispielsweise 200 MB pro zweite Festplatte Durchsatz. Hohe Skalierung virtueller Speicher hat auch bestimmte Durchsatz Beschränkung, die er verarbeiten kann. Standard GS5 VM hat beispielsweise einen maximalen Durchsatz von 2.000 MB pro Sekunde.

Gibt es eine Beziehung zwischen Durchsatz und IOPS wie in der folgenden Formel dargestellt.

![](media/storage-premium-storage-performance/image1.png)

Daher ist es wichtig, die optimalen Durchsatz und IOPS-Werte bestimmt, die Ihre Anwendung benötigt. Wenn Sie versuchen, eine Optimierung, ruft die andere ebenfalls betroffen. In einem späteren Abschnitt *Anwendungsperformance optimieren*, besprechen Weitere Details wir IOPS und Durchsatz optimieren.

## <a name="latency"></a>Wartezeit  
Latenz ist die Zeit, die eine Anwendung zum Empfangen einer Anforderung, die Datenträger senden und die Antwort an den Client senden. Dies ist eine wichtige Angabe für die Leistung einer Anwendung neben IOPS und Durchsatz. Die Latenz eines Datenträgers Storage Premium ist die Zeit für eine Anforderung abrufen und an die Anwendung kommunizieren. Premium bietet konsistente niedrige Wartezeiten. Aktivieren Sie ReadOnly Host Zwischenspeichern auf Premium-Datenträgern erhalten Sie viel niedriger Latenz beim Lesen. Zwischenspeichern erörtert im späteren Abschnitt zur *Leistung der Anwendung optimieren*.

Beim Optimieren der Anwendung höhere IOPS und Durchsatz wirkt sich die Latenz der Anwendung. Nach der Optimierung der Leistung der Anwendung, immer die Wartezeit der Anwendung unerwartet hohe Latenz Verhalten zu bewerten.

## <a name="gather-application-performance-requirements"></a>Leistungsbedarf Anwendung sammeln  
Der erste Schritt beim Entwerfen hohe Performance Applikationen auf Azure Premium-Speicher ist die Performance der Anwendung Anforderungen. Nach dem Sammeln von Anforderungen optimieren Sie Ihre Anwendung die optimale Leistung zu erzielen.

Im vorherigen Abschnitt wurde die allgemeine Leistungsindikatoren IOPS, Durchsatz und Wartezeit erklärt. Sie müssen angeben, der diese Leistungsindikatoren für Ihre Anwendung die gewünschte Funktionalität zu werden. Beispielsweise macht hohe IO die Millionen von Transaktionen in einer Sekunde Verarbeitung bereit. Hoher Durchsatz für Data Warehouse-Anwendung Verarbeitung große Datenmengen in einer Sekunde ist. Äußerst geringer Latenz ist unabdingbar für Echtzeit-Applikationen wie live-Video-Streaming-Websites.

Als Nächstes messen Sie Höchstleistung an Ihre Anwendung während seiner Lebensdauer. Verwenden Sie die Prüfliste Beispiel unten starten. Die maximale Leistungsbedarf während des normalen, Spitze und Arbeitszeiten Arbeitslast aufzeichnen Vorschriften für alle Arbeitslasten identifizieren, werden Sie feststellen, die allgemeine leistungsanforderung der Anwendung. Beispielsweise werden die normale Arbeitsbelastung der e-Commerce-Website die Transaktionen dient den meisten Tagen im Jahr. Spitzenarbeitslast der Website werden die Transaktionen bei Feiertage oder Sonderangebot dient. Die Spitzenlast ist in der Regel erfahrene beschränkt, aber mehrmals Normalbetrieb Skalierung die Anwendung, benötigen. Die 50 Perzentil, 90-Perzentil und 99 Quantil herausfinden. Dadurch werden keine Ausreißer Anforderungen herausfiltern und konzentrieren sich auf die richtigen Werte optimieren.

**Anwendung Leistung Anforderungscheckliste**

| **Leistungsbedarf** | **50 %-Quantil** | **90 %-Quantil** | **99 %-Quantil** |
|---|---|---|---|
| Max. Transaktionen pro Sekunde | | | |
| % Lesevorgänge            | | | |
| % Schreibvorgänge           | | | |
| % Zufällige Prozesse          | | | |
| % Sequenzielle Prozesse      | | | |
| Größe der e/a-Anforderung              | | | |
| Durchschnittliche Durchsatz           | | | |
| Max. Durchsatz              | | | |
| Min. Wartezeit                 | | | |
| Durchschnittliche Wartezeit              | | | |
| Max. CPU                     | | | |
| Durchschnittliche CPU                  | | | |
| Max. Speicher                  | | | |
| Durchschnittliche Speicher               | | | |
| Tiefe der Warteschlange                  | | | |

>**Wichtiger Hinweis:**  
>Sie sollten diese Zahlen basierend auf erwarteten Wachstum der Anwendung skalieren. Es empfiehlt sich Wachstum Zeit einplanen, da es schwieriger sein könnte, die Infrastruktur zur Verbesserung der Leistung später ändern.

Wenn Sie eine vorhandene Anwendung und Premium-Speicher verschieben möchten, zunächst erstellen Sie die Checkliste über für die Anwendung. Ein Prototyp der Anwendung auf Premium-Speicher, und die Anwendung basierend auf Richtlinien *Optimieren der Leistung der Anwendung* in einem späteren Abschnitt dieses Dokuments beschrieben. Der nächste Abschnitt beschreibt die Tools für die Messung der Leistung sammeln.

Erstellen Sie eine Prüfliste Ihre vorhandene Anwendung für den Prototyp ähnlich. Benchmarking verwenden Sie simulieren die Auslastung und Messen der Prototyp-Anwendung. Abschnitt [Benchmarking](#benchmarking) mehr. Damit Sie bestimmen können, ob Storage Premium entsprechen oder die Anwendung Leistungsbedarf übertreffen können. Dann können Sie die Richtlinien für die produktionsanwendung implementieren.

### <a name="counters-to-measure-application-performance-requirements"></a>Leistungsindikatoren messen Leistungsbedarf Anwendung  
Die beste Möglichkeit zum Messen der Leistung an die Anwendung ist mit Leistungsüberwachungstools vom Betriebssystem des Servers bereitgestellt. Sie können PerfMon für Windows und Iostat für Linux verwenden. Diese Tools erfassen Leistungsindikatoren für jedes Measure im obigen Abschnitt erläutert. Sie müssen die Werte dieser Leistungsindikatoren beim Ausführen der Anwendung die Normal, Spitzen- und Arbeitszeiten Arbeitslasten erfassen.

Die Leistungsindikatoren stehen für Prozessor, Speicher, jeden logischen und physischen Datenträger des Servers. Bei Verwendung von Premium-Datenträger mit einer VM die physikalischen Leistungsindikatoren sind für jeden Premium Speicher Datenträger und Leistungsindikatoren für logische Datenträger für jedes Volume auf den Premium-Datenträger erstellt werden. Sie müssen die Werte für die Datenträger erfassen, die Anwendungsarbeitslast hosten. Besteht eine Eins-Zuordnung zwischen logischen und physischen Datenträgern, finden Sie auf physikalischen Leistungsindikatoren; Andernfalls finden Sie die Leistungsindikatoren für logische Datenträger. Unter Linux erzeugt der Befehl Iostat einen Bericht CPU- und Nutzung. Der Datenträgerauslastungsbericht stellt Statistiken pro Gerät oder Partition. Haben Sie einen Datenbankserver mit der Daten- und Protokolldateien auf separaten Datenträgern sammeln dieser Daten für beide Datenträger. Tabelle beschreibt die Leistungsindikatoren für Datenträger, Prozessor und Speicher:

| Zähler | Beschreibung | PerfMon | Iostat |
|---|---|---|---|
| **IOPS oder Transaktionen pro Sekunde** | Anzahl der e/a-Anfragen auf den Datenträger pro Sekunde ausgegeben. | Lesevorgänge/s <br> Schreibvorgänge/s | TPS <br> R/s <br> w/s |
| **Lesevorgänge und Schreibvorgänge** | % von Lese- und Schreibvorgängen auf dem Datenträger durchgeführt. | Lesezeit % <br> Zeit (%) schreiben | R/s <br> w/s |
| **Durchsatz** | Datenmenge Lese- und Schreibzugriffe auf den Datenträger pro Sekunde. | Datenträger-Bytes gelesen/s <br> Bytes geschrieben/s | kB_read/s <br> kB_wrtn/s |
| **Wartezeit** | Gesamtzeit Datenträger e/a-Anforderung abgeschlossen. | Durchschnittliche Datenträger Sek./Lesevorgänge <br> Durchschnittliche Schreibvorgänge/s | Warten auf <br> svctm |
| **E/a-Größe** | Die Größe der e/a fordert Probleme auf dem Datenträger. | Durchschnittliche Datenbytes/Lesevorgang <br> Durchschnittliche Bytes/Schreibvorgang | Avgrq sz |
| **Tiefe der Warteschlange** | Anzahl der ausstehenden e/a fordert zu Formular gelesen oder auf den Datenträger geschrieben werden. | Aktuelle Warteschlangenlänge | Avgqu sz |
| **Max. Speicher** | Anwendung reibungslos erforderliche Speichermenge | % Zugesicherte verwendete Bytes verwendet | Vmstat verwenden |
| **Max. CPU** | Betrag CPU reibungslos Anwendung erforderlich | Prozessorzeit (%) | % util |

Erfahren Sie mehr über [Iostat](http://linuxcommand.org/man_pages/iostat1.html) [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).


## <a name="optimizing-application-performance"></a>Optimale Anwendungsperformance  
Wichtigsten Faktoren für die Leistung einer Anwendung auf Storage Premium sind Art von e/a-Anfragen virtueller Speicher, Datenträgergröße, Anzahl der Festplatten, zwischenspeichern, Multithreading und Tiefe der Warteschlange. Sie können einige dieser Faktoren mit vom System bereitgestellten steuern. Meisten können Ihnen nicht die Option e/a-Größe und der Warteschlangenlänge direkt ändern. Wenn Sie SQL Server verwenden, können nicht Sie die e/a-Größe und Warteschlange Tiefe auswählen. SQL Server wählt die optimale e/a-Größe Warteschlange Tiefenwerte und die meisten Leistung zu erzielen. Unbedingt die Auswirkungen beider Faktoren auf die Leistung Ihrer Anwendung, sodass Ressourcen Bedürfnisse Leistung bereitstellen können.

Finden Sie in diesem Abschnitt in der Anwendung Anforderungscheckliste die erstellt, erkennt, wie viel Sie optimieren Sie die Leistung Ihrer Anwendung. Auf dieser Grundlage werden Sie feststellen, welche Faktoren aus diesem Abschnitt müssen Sie optimieren. Um die Effekte der einzelnen Faktoren auf die Leistung Ihrer Anwendung Zeugen führen Sie benchmarking Tools auf Setup Ihrer Anwendung aus. Siehe Abschnitt [Benchmarking](#Benchmarking) am Ende dieses Artikels benchmarking Werkzeugen unter Windows und Linux VMs ausgeführt.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimieren der IOPS, Durchsatz und Wartezeit auf einen Blick  
Die folgende Tabelle führt alle Leistungsfaktoren sowie zu IOPS, Durchsatz und Wartezeit zu optimieren. Nach dieser Zusammenfassung Abschnitte beschreiben jede ist mehr Tiefe.

| | **IOPS** | **Durchsatz** | **Wartezeit** |
|---|---|---|---|
| **Beispielszenario** | Enterprise OLTP-Anwendung erfordert hohe Transaktionen pro Sekunde.                                                                                                                                 | Enterprise Data warehousing Anwendung Verarbeitung großer Datenmengen. | In Echtzeit Applikationen erfordern sofortige Reaktionen auf Benutzeranfragen wie Onlinespiele. |
| Performancefaktoren  | | | |
| **E/a-Größe** | Kleinere IO ergibt höhere IOPS.                                                                                                                                                                           | Größere e/a-Größe führt zu höheren Durchsatz. | |
| **Virtueller Speicher** | Verwenden Sie eine VM-Größe, die größer als Ihre anwendungsanforderung IOPS bietet. VM-Größen und ihre Grenzen IOPS anzeigen | Verwenden Sie eine VM-Größe mit Durchsatz Ihrer anwendungsanforderung größer. VM-Größen und deren durchsatzgrenzen anzeigen | Verwenden Sie eine VM-Größe, bietet Grenzen Ihrer anwendungsanforderung größer skaliert. Siehe VM Größen und ihre Grenzen. |
| **Festplattengröße** | Verwenden Sie eine Größe, die größer als Ihre anwendungsanforderung IOPS bietet. Siehe Datenträger und ihre Grenzen IOPS. | Verwenden Sie einen Wert mit Durchsatz Ihrer anwendungsanforderung größer. Siehe Datenträger und deren durchsatzgrenzen. | Verwenden Sie eine Größe, dass Angebote Grenzen Ihrer anwendungsanforderung größer skaliert. Siehe Datenträger und ihre Grenzen. |
| **VM und Skalierungsgrenzen** | IOPS maximal gewählte VM-Größe sollte insgesamt IOPS angetrieben Premium-Datenträger angeschlossen größer sein. | Grenze für Anwendungsdurchsatz VM Größe gewählt sollte größer als Gesamtdurchsatz angetrieben Premium-Datenträger zugeordnet sein. | Skalierung müssen gewählte VM-Größe insgesamt Skalierungsgrenzen angeschlossenen Premium-Datenträger größer sein. |
| **Zwischenspeichern** | Premium-Datenträger mit hoher Lesevorgänge zu höheren lesen IOPS aktivieren Sie ReadOnly Cache. | | Premium-Datenträger mit können schwer zu sehr niedrigen lesen Wartezeiten aktivieren Sie ReadOnly Cache. |
| **Festplatten-Striping** | Verwenden Sie mehrere Festplatten und Streifen sie zusammen, um eine kombinierte höhere IOPS und Durchsatz Grenze. Beachten Sie, dass kombinierte Grenzwert pro VM kombinierten Grenzen angeschlossenen Premium Festplatten größer sein soll. | |
| **Stripe-Größe** | Kleinere Stripe-Größe für zufällige kleine e/a-Muster bereit. Verwenden Sie z.B. Stripe-Größe von 64KB für SQL Server OLTP-Anwendung. | Größere Stripe-Größe für sequenzielle große e/a-Muster in Data Warehouse-Anwendung. Verwenden Sie z. B. 256KB Stripe-Größe für SQL Server Data Warehouse-Anwendung. | |
| **Multithreading** | Verwenden Sie multithreading, mehr Anfragen zu Premium-Speicher ablegen, die IOPS und Durchsatz führt. Festgelegt auf SQL Server z. B. MAXDOP hochwertigen, SQL Server mehrere CPUs zuzuweisen. | |
| **Tiefe der Warteschlange** | Größere Warteschlangentiefe liefert höhere IOPS. | Größere Warteschlangentiefe führt zu höheren Durchsatz. | Kleinere Warteschlangentiefe ergibt niedriger Latenz. |

## <a name="nature-of-io-requests"></a>Art der e/a-Anfragen  
Eine e/a-Anforderung ist ein e/a-Operation, die Ihre Anwendung ausführen werden. Identifiziert die Art von e/a-Anfragen random oder sequenzieller hilft Lese- oder Schreibvorgang, kleine oder große Leistung der Anwendung bestimmen Ihnen. Es ist wichtig, die Art der e/a-Anfragen Entscheidungen beim Entwerfen der Anwendungsinfrastruktur für Ihre.

E/a-Größe ist einer der wichtigsten Faktoren. E/a-Größe ist die Größe der vorgangsanforderung e/a-von der Anwendung generiert. Die e/a-Größe wirkt sich deutlich auf die Leistung besonders auf IOPS und Bandbreite die Anwendung zu. Die folgende Formel zeigt die Beziehung zwischen IOPS, e/a-Größe und Bandbreite-Durchsatz.  
    ![](media/storage-premium-storage-performance/image1.png)

Einige Programme können einige Programme nicht e/a-Größe ändern. Beispielsweise SQL Server bestimmt die optimale e/a-Größe und bietet keinen Benutzer mit jeder ändern. Auf der anderen Seite bietet Oracle einen Parameter namens [DB\_blockieren\_Größe](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) mit, die Sie konfigurieren können e/a-Anforderungsgröße der Datenbank.

Verwenden Sie eine Anwendung, die nicht die e/a-Größe ändern können, verwenden Sie die Richtlinien in diesem Artikel zur Optimierung der Leistung KPI, die am besten zu Ihrer Anwendung. Zum Beispiel

-   OLTP-Anwendung generiert Millionen kleine zufällige e/a-Anfragen. Um diese e/a-Anfragen zu behandeln, müssen Sie Ihre Anwendungsinfrastruktur höhere IOPS zu entwerfen.  
-   Ein Data Warehouse-Anwendung generiert große und sequenzielle e/a-Anfragen. Um diese Art von e/a-Anfragen zu behandeln, müssen Sie Ihre Anwendungsinfrastruktur höhere Bandbreite oder Durchsatz entwerfen.

Verwenden Sie eine Anwendung, die e/a-Größe ändern können, verwenden Sie diese Faustregel für die e/a-Größe neben anderen Leistungsrichtlinien,

-   Kleinere IO zu höhere IOPS. Beispielsweise 8 KB für eine OLTP-Anwendung.  
-   Größere IO zu höheren Bandbreite-Durchsatz. Beispielsweise 1024 KB für ein Data Warehouse-Anwendung.

Hier ist ein Beispiel für wie IOPS und Durchsatz-Bandbreite für die Anwendung berechnen können. Betrachten Sie eine Anwendung mit einer P30. Die maximale IOPS und Durchsatz-Bandbreite P30 Datenträger erreichen ist 5000 IOPS und 200 MB pro Sekunde. Nun ist die Anwendung erfordert maximale IOPS vom Datenträger P30 und Sie IO kleiner wie 8 KB, resultierende Bandbreite zu werden 40 MB pro Sekunde. Wenn Ihre Anwendung maximale Durchsatz/Bandbreite von P30 Datenträger erfordert eine größere e/a-Größe wie 1024 KB verwenden, die resultierende IOPS werden weniger 200 IOPS. Daher stimmen Sie die e/a-Größe wird sowohl die Anwendung IOPS und Durchsatz-Bandbreite erforderlich. Tabelle fasst P30 Datenträger e/a-Größen und ihre entsprechenden IOPS und Durchsatz.

| **Anwendung erforderlich** | **E/a-Größe** | **IOPS** | **Durchsatz-Bandbreite** |
|-----------------------------|--------------|----------|--------------------------|
| Max IOPS                    | 8 KB         | 5.000    | 40 MB pro Sekunde         |
| Max. Durchsatz              | 1024 KB      | 200      | 200 MB pro Sekunde        |
| Max. Throughput + hohe IO/s  | 64 KB        | 3200    | 200 MB pro Sekunde        |
| Max IOPS + hohen Durchsatz  | 32 KB        | 5.000    | 160 MB pro Sekunde        |

IOPS und Bandbreite höher als der Höchstwert einer einzigen Prämie Storage Festplatte verwenden Sie mehrere Premium Festplatten zusammen verteilt. Z. B. Streifen zwei P30 Festplatten zu einem kombinierten IOPS von 10.000 IOPS oder einen kombinierten Durchsatz von 400 MB pro Sekunde. Wie im nächsten Abschnitt erläutert wird, verwenden Sie eine VM-Größe, die die kombinierten unterstützt disk IOPS und Durchsatz.

>**Hinweis:**  
>IOPS oder anderen erhöht Durchsatz erhöhen, stellen Sie sicher, dass Sie nicht Durchsatz oder IOPS Limits der Datenträger oder die VM treffen bei einem.

Zeugen die Effekte der e/a-Größe zur Leistung der Anwendung führen Sie benchmarking Tools VM und Festplatten. Mehrere Testläufe erstellen und Verwenden von anderen e/a-Größe für alle um zu sehen. Siehe Abschnitt [Benchmarking](#Benchmarking) am Ende dieses Artikels Weitere Informationen.

## <a name="high-scale-vm-sizes"></a>Hohe Skaliergröße VM  
Beim Entwerfen einer Anwendung, ist eines der ersten Dinge zu tun, wählen Sie einen virtuellen Computer zum Hosten der Anwendung. Premium-Speicher kommt mit hoher Skalierung VM, die Anwendung erfordert höhere rechenleistung und lokal hohe i/o-Leistung führen können. Die VMs bieten schnellere Prozessoren, ein höheres Verhältnis Speicher Core, und ein Solid-State Drive (SSD) der Festplatte. Hohe Skalierung VMs unterstützt Storage Premium sind die DS, DSv2 und GS-Serie VMs.

Hohe Skalierung VMs werden in verschiedenen Größen mit verschiedene Kerne CPU, Speicher, Betriebssystem und temporäre Größe. Jede VM Größe hat auch maximale Anzahl der Datenträger, die für die VM anfügen können. Daher die VM Größe wirkt wie viel Verarbeitung, Speicher und Speicherkapazität für die Anwendung verfügbar ist. Es wirkt sich auch auf die Compute und Speicherkosten. Beispielsweise sind unter Spezifikationen die größten VM DS-Serie und DSv2 Serie GS-Serie:

| Virtueller Speicher | CPU-Kerne | Speicher | VM-Datenträgergrößen | Max. Datenträger | Cache-Größe | IOPS | E/a-Cache Bandbreitengrenzwerte |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GB | OS = 1023 GB <br> Lokale SSD = 224 GB | 32 | 576 GB | 50.000 IOPS <br> 512 MB pro Sekunde | 4.000 IOPS und 33 MB pro Sekunde |
| Standard_GS5 | 32 | 448 GB | OS = 1023 GB <br> Lokale SSD = 896 GB | 64 | 4224 GB | 80.000 IOPS <br> 2.000 MB pro Sekunde | 5.000 IOPS und 50 MB pro Sekunde |

Finden Sie eine vollständige Liste aller verfügbaren Azure VM-Größen [Windows VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md) oder [Linux VM-Größen](../virtual-machines/virtual-machines-linux-sizes.md). Wählen Sie eine VM-Größe, die erfüllen und auf die gewünschte Leistung skaliert werden kann. Darüber hinaus tragen Sie folgende wichtige Aspekte bei VM-Größen.


*Skalierung Grenzwerte*  
IOPS Höchstmengen pro VM und Datenträger sind unterschiedlich und unabhängig voneinander. Stellen Sie sicher, dass die Anwendung IOPS innerhalb der VM sowie Premium-Datenträgern zu fördern. Andernfalls wird Anwendungsperformance Drosselung.

Beispielsweise Angenommen Sie, eine anwendungsanforderung maximal 4.000 IOPS. Zu diesem Zweck bereitgestellt einen Datenträger P30 DS1 VM. P30 Datenträger bieten bis zu 5.000 IOPS. Jedoch ist die VM DS1 3.200 IOPS auf. Daher die Anwendungsperformance VM Beschränkung 3.200 IOPS eingeschränkt und Leistung verringert werden. Um dies zu verhindern, wählen Sie eine VM und die Festplattengröße, die beide Anforderungen Anwendung.

*Betriebskosten*  
In vielen Fällen ist es möglich, dass die Gesamtkosten der Operation Premium-Speicher mit niedriger als Standardspeicher verwenden.

Angenommen Sie, eine Anwendung, 16.000 IOPS. Um diese Leistung zu erzielen, benötigen Sie einen Standard\_D14 Azure-Neuerung, maximale IOPS 16.000 32 Standardspeicher 1 TB Laufwerke verwenden zu können. Jedes Laufwerk 1TB Standardspeicher kann maximal 500 IOPS erzielen. Die Kosten dieser VM pro Monat werden $1.570. Die monatliche Kosten für 32 standard Datenträger werden $1.638. Die geschätzte monatliche Gesamtkosten werden $3,208.

Jedoch gehostet derselben Anwendung auf Storage Premium benötigen VM kleiner Sie und weniger Premium-Datenträger, wodurch die Gesamtkosten. Standard\_DS13 VM kann Anforderung der 16.000 IOPS mit vier P30 Festplatten. DS13 VM eine maximale IOPS 25.600 und jedes Laufwerk P30 maximale IOPS 5.000. Diese Konfiguration kann insgesamt 5.000 x 4 = 20.000 erzielen IOPS. Die Kosten dieser VM pro Monat werden $1003. Die monatliche Kosten für vier P30 Premium Datenträger werden $544.34. Die geschätzte monatliche Gesamtkosten werden $1,544.

Tabelle fasst die Kosten Aufteilung dieses Szenarios für Standard- und Premium-Speicher.

| | **Standard** | **Premium** |
|---|---|---|
| **Kosten pro Monat in der VM** | $1,570.58 (standard\_D14)   | $1,003.66 (standard\_DS13) |
| **Kosten der Festplatten pro Monat** | $1,638.40 (32 x 1 TB Laufwerke) | $544.34 (4 x P30 Festplatten) |
| **Gesamtkosten pro Monat**  | $3,208.98 | $1,544.34 |

*Linux-Distributionen*  

Mit Azure Premium-Speicher erhalten Sie die gleiche Leistung virtueller Computer unter Windows und Linux. Wir unterstützen viele Arten von Linux-Distributionen und vollständige Liste [hier](../virtual-machines/virtual-machines-linux-endorsed-distros.md). Es ist wichtig zu beachten, dass verschiedene Distributionen für unterschiedliche Workloads besser geeignet sind. Sie sehen unterschiedlich je nach Distribution, die Ihre Arbeitslast ausgeführt wird. Testen Sie Linux-Distributionen mit Ihrer Anwendung und wählen Sie das am besten geeignet ist.


Beim Ausführen von Linux mit Premium-Speicher überprüfen Sie die neuesten Updates zum erforderlichen Treiber auf hohe Leistung zu gewährleisten.

## <a name="premium-storage-disk-sizes"></a>Premium Speichergrößen Datenträger  
Azure Storage Premium bietet drei Datenträgergrößen. Jede Festplattengröße hat einen anderen Maßstab Grenzwert für IOPS, Bandbreite und Speicher Wählen Sie die richtige Prämie Datenträger Größe je nach den Erfordernissen der Anwendung und der hohe virtueller Speicher. In der folgenden Tabelle werden die drei Laufwerke und deren Funktionen

| **Datenträgertyp**       | **P10**           | **P20**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Festplattengröße           | 128 giB           | 512 giB           | GiB 1024 (1 TB)   |
| IOPS pro Datenträger       | 500               | 2300              | 5000              |
| Durchsatz pro Datenträger | 100 MB pro Sekunde | 150 MB pro Sekunde | 200 MB pro Sekunde |

Wie viele Laufwerke hängt die Festplatte die Größe ausgewählt. Eine einzelne P30 oder mehrere P10 mehrerer Festplatten können Sie Ihre Anwendung Anforderung. Bei den unten aufgeführten Aspekte berücksichtigen.

*Skalierung begrenzt (IOPS und Durchsatz)*  
Jede Festplattengröße Premium IOPS und Durchsatz Grenzen ist anders und unabhängige Skalierung aus VM. Achten Sie insgesamt IOPS und Durchsatz vom Datenträger Rahmen Skalieren der VM Größe.

Wenn beispielsweise eine anwendungsanforderung maximal 250 MB/s Durchsatz und DS4 VM mit nur einem Datenträger P30 verwenden. DS4 VM können bis zu 256 MB/s Durchsatz. Allerdings hat eine Festplatte P30 Grenzwert von 200 MB/s Durchsatz. Daher wird die Anwendung mit 200 MB/s aufgrund der Festplatte beschränkt. Um diese Beschränkung zu umgehen, stellen Sie mehrere Datenträger für die VM.

>**Hinweis:**  
>Lesen vom Cache bedient die Laufwerks-IOPS und Durchsatz Grenzen daher nicht Gegenstand gehören nicht. Cache wurde das separate IOPS und Durchsatz Limit pro VM.
>
>Beispielsweise werden zunächst die Lese- und Schreibvorgänge 60MB/s und 40MB/s. Im Laufe der Zeit der Cache erwärmt und dient mehr der Lesevorgänge aus dem Cache. Dann können Sie höhere schreiben Durchsatz vom Datenträger abrufen.

*Anzahl der Festplatten*  
Bestimmen Sie die Anzahl der Laufwerke mit der Bewertung der Anforderungen müssen. Jede VM-Größe ist auch begrenzt die Anzahl der Festplatten, die an den virtuellen Computer angefügt werden kann. In der Regel ist dies zweimal die Anzahl der Kerne. Stellen Sie sicher, dass die VM-Größe wählen Sie die erforderliche Anzahl von Datenträgern unterstützt.

Denken Sie daran, die Premium-Datenträger höhere Leistungsfähigkeit im Vergleich zu Standard-Datenträger. Wenn Sie die Anwendung von Azure Neuerung mit Standardspeicher Premium Speicher migrieren, daher Sie wahrscheinlich weniger Premium Festplatten die gleiche oder eine höhere Leistung für Ihre Anwendung.

## <a name="disk-caching"></a>Zwischenspeichern  
Hohe Skalierung VMs, die Azure Premium Storage nutzen haben mehrstufige Cache-Technologie BlobCache. BlobCache verwendet eine Kombination aus virtuellen Arbeitsspeicher und lokalen SSD Zwischenspeichern. Dieser Cache ist für die permanente Premium Speicherdatenträger und VM-Festplatten. Standardmäßig wird dieser Cache zum Lesen/Schreiben für Betriebssystemlaufwerke und ReadOnly für Datenträger auf Premium Speicher gehostet eingestellt. Mit Datenträgerspeicher Premium Speicherdatenträger aktiviert erreichen hohe Skalierung VMs extrem hohe Leistung, die die zugrunde liegenden Datenträger nicht überschreiten.

>[AZURE.WARNING] Ein Azure Datenträger Cache ändern trennt und die Zieldiskette angefügt. Ist dies der Betriebssystem-CD wird die VM neu gestartet. Beenden Sie alle Programme/Dienste, die von dieser Störung vor dem Ändern der Datenträger-Cache-Einstellung betroffen sein könnten.

Erfahren Sie mehr über BlobCache finden Sie im Blogbeitrag [Azure Premium-Speicher](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) .

Es ist wichtig, Cache auf die richtigen Datenträger aktivieren. Ob auf einem Datenträger Premium zwischenspeichern soll oder nicht hängt die Arbeitslast Muster dieser Festplatte behandelt. Folgende Tabelle zeigt die Cache-Einstellungen für Betriebssystem und Daten von Festplatten.

| **Datenträgertyp** | **Standardeinstellung für Cache** |
|---|---|
| Betriebssystem-Datenträger | Lesen/Schreiben |
| Datenträger | Keine |

Benötigte werden Cache folgende für Datenträger,

| **Festplatten-Cache-Einstellung** | **Empfehlung bei Verwendung dieser Einstellung** |
|---|---|
| Keine | Konfigurieren Sie Host-Cache keine nur-Schreibzugriff Heavy Datenträger. |
| Schreibgeschützt | Konfigurieren Sie Host Cache als ReadOnly für nur-Lese- und Schreibzugriff Datenträger. |
| Lesen/Schreiben | Konfigurieren Sie Host-Cache als ReadWrite nur, wenn die Anwendung ordnungsgemäß permanenten Datenträger behandelt bei Bedarf zwischengespeicherte Daten schreiben. |

*Schreibgeschützt*  
ReadOnly Zwischenspeichern Premium Daten Datenträger konfigurieren, können Sie erreichen lesen Latenz und hohe lesen IOPS und Durchsatz für Ihre Anwendung. Dies ist aus zwei Gründen

1.  Lesevorgänge durchgeführt vom Cache auf VM-Speicher und lokale SSD, schneller liest die Datenträger auf Azure BLOB-Speicher.  
2.  Premium-Speicher zählt keine Lesevorgänge aus dem Cache auf dem Datenträger IOPS und Durchsatz bereitgestellt. Daher ist Ihre Anwendung höhere Gesamt IOPS und Durchsatz zu erreichen.

*Lesen/Schreiben*  
Standardmäßig verfügen die Betriebssystemlaufwerke ReadWrite Zwischenspeichern aktiviert. Wir haben vor kurzem ReadWrite Zwischenspeichern von Daten sowie Festplatten unterstützt. Bei Verwendung von ReadWrite zwischenspeichern müssen Sie richtig schreiben die Daten vom Cache auf permanente Datenträger. SQL Server übernimmt beispielsweise zwischengespeicherte Daten auf den Datenträgern Speicherung selbst schreiben. Eine Anwendung, die die erforderlichen Daten beibehalten nicht behandelt ReadWrite Cache mit führt zu Datenverlust, stürzt die VM.

Beispielsweise können gelten diese Richtlinien für SQL Server Storage Premium folgt, unter

1.  Premium-Datenträgern Daten Dateien konfigurieren Sie "ReadOnly" Cache.  
    ein.  Schnell liest aus Zwischenspeicher unteren Abfragezeit SQL Server seit Seiten schneller aus dem Cache abgerufen werden direkt aus den Daten Festplatten gegenüber.  
    b.  Für Lesevorgänge aus dem Cache, bedeutet zusätzlichen Durchsatz von Premium-Datenträger. SQL Server kann mithilfe dieser zusätzlichen Durchsatz auf Weitere Datenseiten und andere Vorgänge wie Backup/Wiederherstellung abrufen batch geladen und Index neu erstellt.  
2.  Konfigurieren Sie "None" cache auf Premium-Datenträgern die Protokoll Dateien.  
    ein.  Protokolldateien arbeiten hauptsächlich Heavy schreiben. Daher nutzen sie nicht aus dem Cache ReadOnly.

## <a name="disk-striping"></a>Festplatten-Striping  
Bei einer hohen Maßstab VM mit mehreren Premium permanente Datenträger zugeordnet ist, können die Festplatten zusammen verteilt werden, um die IOPs Bandbreite und Speicherkapazität aggregieren.

Unter Windows können Sie Storage Spaces auf Stripeset-Datenträger zusammen verwenden. Sie müssen eine Spalte für jeden Datenträger in einem Pool konfigurieren. Andernfalls kann die allgemeine Leistung von Stripesetvolumes niedriger als erwartet durch ungleichmäßige Verteilung des Datenverkehrs auf dem Datenträger.

Wichtig: Mit Server-Manager-UI, können Sie die Gesamtanzahl der Spalten bis 8 für ein Stripeset-Volume festlegen. Beim Anfügen von mehr als 8 Laufwerke mithilfe von PowerShell um das Volume zu erstellen. PowerShell verwenden, können Sie die Anzahl der Spalten gleich der Anzahl der Datenträger festlegen. Beispielsweise gibt es 16 Festplatten in einem einzigen Stripeset. Geben Sie 16 Spalten *NumberOfColumns* -Parameter *Neu VirtualDisk* PowerShell-Cmdlets.


Verwenden Sie unter Linux das Dienstprogramm MDADM Stripeset-Datenträger zusammen. Detaillierte Schritte Striping Festplatten unter Linux finden Sie unter [Konfigurieren des Software-RAID auf Linux](../virtual-machines/virtual-machines-linux-configure-raid.md).


*Stripe-Größe*  
Eine wichtige Konfiguration Festplattenstriping ist die Stripe-Größe. Die Stripe-Größe oder Blockgröße ist die kleinste Datenmenge, die Anwendung auf einem Stripesetvolume adressiert. Zu konfigurierende Stripe-Größe hängt vom Typ der Anwendung und die Anforderung. Bei Auswahl die falschen Stripe-Größe könnte es zu e/a-Versatz führt zu Leistungseinbußen der Anwendung.

Beispielsweise ist eine e/a-Anforderung von der Anwendung generierte größer als die Blockgröße der Festplatte schreibt das Speichersystem sie Grenzen Einheit Streifen auf mehrere Datenträger. Wenn auf diese Daten zugegriffen werden, müssen über den Vorgang mehrere Stripe-Einheiten suchen. Kumulative Wirkung eines solchen Verhaltens kann zu erheblichen Leistungseinbußen führen. Auf der anderen Seite können der e/a-Anforderung Stripe-Größe kleiner ist und ist zufällig, e/a-Anfragen auf die Festplatte einen Engpass verursachen und schließlich die e/a-Leistung beeinträchtigt addiert.


Je nach Arbeitslast die Anwendung ausgeführt wird, wählen Sie eine entsprechende Stripe-Größe. Verwenden Sie für zufällige kleine e/a-Anfragen Stripe kleiner. Während Anfragen für große sequenzielle e/a Stripe größer verwenden. Ermitteln der Streifen Größe Vorschläge für die Anwendung, dass Premium Speicher ausgeführt. Konfigurieren Sie für SQL Server-Stripe-Größe von 64KB für OLTP-Arbeitslasten und 256KB für Data warehousing Arbeitslasten. [Best Practices zur Performance für SQL Server auf Azure VMs](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) mehr anzeigen


>**Hinweis:**  
>Sie können maximal 32 Datenträger Premium DS Serie VM und 64 Premium-Datenträger GS-Serie VM zusammen Streifen.

## <a name="multi-threading"></a>Multi-threading  
Azure verfügt über Premium-Speicherplattform zu parallelen entwickelt. Daher wird eine Multithreadanwendung wesentlich höhere Leistung als Singlethread-Anwendung erreicht. Eine Multithreadanwendung teilt seine Aufgaben über mehrere Threads und steigert die Effizienz der Ausführung von der VM und Datenträger Ressourcen maximal.

Wenn die Anwendung auf einer einzelnen VM mit zwei Threads ausgeführt wird, kann CPU zwischen den beiden Threads Wirkungsgrad wechseln. Während ein Thread auf einem Datenträger-e/a wartet abgeschlossen kann andere Threads CPU wechseln. Auf diese Weise erreichen zwei Threads mehr als ein einzelnen Thread. Verfügt die VM mehr als ein Kern, verkürzt sich weitere Laufzeit, da jeder Kern Aufgaben parallel ausgeführt werden kann.

Nicht möglicherweise ändern eine einsatzbereite Anwendung single implementiert-threading oder Multi-threading. SQL Server kann beispielsweise Multi-CPU-und Multi-Core. SQL Server entscheidet, unter welchen Umständen sie Threads zum Verarbeiten einer Abfrage nutzen. Sie können Abfragen und Indizes mit Multi-threading. SQL Server wird für eine Abfrage, bei der große Tabellen verknüpfen und Sortieren von Daten vor der Rückgabe an die Benutzer, wahrscheinlich mehrere Threads verwendet. Ein Benutzer kann nicht jedoch steuern, ob SQL Server eine Abfrage mit einem einzelnen Thread oder mehreren Threads ausgeführt wird.

Gibt Konfigurationsdateien, die Sie ändern können, um dies Multithreading oder parallele Verarbeitung einer Anwendung. Beispielsweise ist es bei SQL Server die maximale Degree of Parallelism-Konfiguration. Diese Einstellung MAXDOP, können Sie die maximale Anzahl von Prozessoren konfigurieren, SQL Server beim parallelen können verarbeiten. Sie können für einzelne Abfragen oder Index MAXDOP konfigurieren. Dies ist vorteilhaft, wenn Sie Ressourcen des Systems für eine wichtige Anwendung Leistung ausgleichen möchten.

Angenommen Sie, die Anwendung mit SQL Server eine umfangreiche Abfrage und gleichzeitig eine Index-Operation ausgeführt wird. Nehmen wir an, daß den Vorgang leistungsfähiger große Abfrage verglichen werden. In diesem Fall können Sie MAXDOP Wert der Vorgang höher als die MAXDOP-Wert für die Abfrage festlegen. Dadurch verfügt SQL Server mehr Prozessoren, die sie nutzen kann, für den Vorgang im Vergleich zu der Anzahl der Prozessoren, große Abfrage widmen können. Beachten Sie, dass Sie SQL Server für jeden Vorgang verwendete Threadanzahl nicht steuern. Sie steuern die maximale Anzahl von Prozessoren wird für Multithreading.

Erfahren Sie mehr über den [Grad der Parallelität](https://technet.microsoft.com/library/ms188611.aspx) in SQL Server. Finden Sie solche beeinflussen Einstellungen Multithreading in der Anwendung und deren Konfigurationen optimieren

## <a name="queue-depth"></a>Tiefe der Warteschlange  
Tiefe der Warteschlange oder Warteschlange oder Warteschlange ist die Anzahl der ausstehenden e/a-Anfragen im System. Der Wert der Warteschlangenlänge bestimmt wie viele i/o-Vorgänge die Anwendung richten kann, die den Datenträger verarbeitet. Es wirkt sich auf alle drei Anwendung-Leistungsindikatoren, die in diesem Artikel nämlich IOPS, Durchsatz und Wartezeit besprochen.

Warteschlange Tiefe und Multi-threading sind eng. Der Warteschlangentiefe Wert gibt Multithreading kann von der Anwendung erreicht werden. Ist die Warteschlangentiefe große Anwendung führen mehrere Operationen gleichzeitig also mehr Multi-threading. Obwohl Multi-Thread-Anwendung ist die Warteschlangentiefe gering ist, haben sie nicht genügend Anfragen für die gleichzeitige Ausführung aufgereiht.

In der Regel aus dem Sortiment Applikationen nicht erlauben die Warteschlangentiefe ändern da Wenn Set falsch zwar mehr Schaden als nutzen. Den richtigen Wert der Warteschlangenlänge, die optimale Leistung wird festgelegt. Allerdings ist es wichtig, dieses Konzept verstehen, Leistungsprobleme mit der Anwendung zu beheben. Die Effekte der Warteschlangentiefe beobachten durch benchmarking Tools auf Ihrem System.

Einige Programme bieten die Warteschlangentiefe beeinflussen. Beispielsweise die MAXDOP (maximalen Grad an Parallelität) Einstellung in SQL Server, die im vorherigen Abschnitt erläutert. MAXDOP ist eine Möglichkeit, Warteschlangentiefe beeinflussen und Multi-threading, obwohl sie nicht direkt Warteschlangentiefe Wert von SQL Server ändert.

*Hohe Warteschlangenlänge*  
Eine hohe Warteschlangenlänge Zeilen mehr Vorgänge auf dem Datenträger. Der Datenträger kann die nächste Anforderung in der Warteschlange voraus. Daher der Datenträger Vorgänge voraus planen und optimale nacheinander verarbeiten. Da die Anwendung mehr auf der Festplatte versendet, kann der Datenträger mehr parallele IOs verarbeiten. Schließlich werden die Anwendung höhere IOPS erzielen. Da Anwendung mehr Anfragen verarbeitet, erhöht der Gesamtdurchsatz der Anwendung auch.

In der Regel eine Anwendung erzielen Sie maximalen Durchsatz mit 8-16 + ausstehenden i/o pro Festplatte. Wird eine Warteschlangenlänge, Anwendung treibt nicht genügend IOs an das System und Verarbeitung weniger in einem bestimmten Zeitraum. Also weniger Durchsatz.

Beispielsweise informiert SQL Server die MAXDOP-Wert für eine Abfrage auf "4" SQL Server, bis zu vier Kerne verwendet werden kann, um die Abfrage auszuführen. SQL Server wird bestimmt optimale Warteschlange Tiefenwert und die Anzahl der Kerne für die Ausführung der Abfrage.

*Optimale Warteschlangentiefe*  
Sehr große Warteschlangen Tiefenwert hat auch Nachteile. Wenn Warteschlange Tiefenwert zu hoch ist, versucht die Anwendung sehr hohe IOPS Laufwerk. Sofern Anwendung permanente Datenträger mit ausreichend bereitgestellte IOPS, kann diese Anwendung Wartezeiten beeinträchtigen. Folgende Formel wird die Beziehung zwischen IOPS, Latenz und Tiefe der Warteschlange.  
    ![](media/storage-premium-storage-performance/image6.png)

Konfigurieren Sie nicht Warteschlangentiefe hoher Wert, sondern optimalen Wert, der genügend IOPS für die Anwendung ohne Wartezeiten liefern können. Z. B. Anwendung Latenz muss 1 Millisekunde Warteschlangentiefe zu 5.000 IOPS erforderlich ist, QD = 5000 x 0,001 = 5.

*Warteschlangenlänge für Stripesetvolumes*  
Ein Stripesetvolume pflegen Sie eine ausreichend hohe Warteschlangenlänge, jeder Festplatte eine maximale Warteschlangenlänge einzeln hat. Angenommen, eine Anwendung, die Warteschlangentiefe 2 legt und gibt 4 Festplatten im Stripe. Zwei e/a-Anfragen werden auf zwei Festplatten und verbleibenden zwei Festplatten werden im Leerlauf. Daher konfigurieren Sie die Warteschlangentiefe so, dass alle Datenträger belegt werden können. Formel zeigt die Warteschlangentiefe Stripesetvolumes ermitteln.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Drosselung  
Azure Storage Premium Vorschriften angegebene Anzahl von IOPS und Durchsatz VM-Größen und Datenträgergrößen gewählte. Jedes Mal, wenn die Anwendung versucht auf IOPS oder Durchsatz über diese Grenzen wie VM oder Datenträger verarbeiten kann, wird Sie von Storage Premium drosseln. Dies äußert sich in Form von Leistungseinbußen in der Anwendung. Bedeuten höheren Latenz kann, Durchsatz verringern oder geringere IOPS. Premium-Speicher nicht einschränken, fehlschlagen die Anwendung vollständig, überschritten, was seine Ressourcen erreichen können. So vermeiden Sie Leistungsprobleme durch Einschränkung immer bereitstellen Sie über ausreichende Ressourcen für die Anwendung. Berücksichtigen Sie was wir in den VM-Größen und Datenträger Größen oben besprochen. Benchmarking ist am einfachsten herausfinden, welche Ressourcen zum Hosten der Anwendung benötigen.

## <a name="benchmarking"></a>Benchmark-Tests  
Benchmarking wird die unterschiedliche Arbeitslasten auf Ihre Anwendung simulieren und messen die Leistung der Anwendung für jede Arbeitslast. Sie haben die zuvor beschriebenen Schritte den Erfordernissen der Anwendung Leistung gesammelt. Benchmarking-Tools auf die VMs Hosten der Anwendungdes ausgeführt wird, bestimmen Sie die Leistung, die Ihre Anwendung mit Premium erreichen. In diesem Abschnitt bieten wir Ihnen Beispiele für Benchmark-Tests eine standardmäßige DS14 VM Azure Premium-Datenträgern bereitgestellt.

Wir haben Benchmark Werkzeugen Iometer und FIO, für Windows und Linux verwendet. Diese Tools erscheinen mehrere Threads eine Produktion wie Workload simuliert und die Systemleistung. Mithilfe der Tools können Sie auch Parameter wie Größe und Warteschlange Tiefe Block konfigurieren das normalerweise für eine Anwendung geändert werden kann. Dadurch mehr Flexibilität auf maximale Leistung auf hoher Ebene VM mit Premium-Festplatten für unterschiedliche Arbeitslasten bereitgestellt. Weitere Informationen finden Sie über jede Benchmarktool [Iometer](http://www.iometer.org/) und [FIO](http://freecode.com/projects/fio).

Um die folgenden Beispiele Standard DS14 VM und ordnen Sie 11 Premium-Datenträger für die VM. Mit 11 Laufwerken 10 Festplatten mit als "None" Cache konfigurieren und in einem Volume mit dem Namen NoCacheWrites Streifen. Konfigurieren Sie Host als "ReadOnly" auf der verbleibenden Festplatte Zwischenspeichern und erstellen Sie ein Volume mit dem Namen "CacheReads" mit dieser Diskette. Mit diesem Setup werden können Sie maximale Performance für Lesen und Schreiben von Standard DS14 VM Sie. Weitere Informationen zum Erstellen einer VM DS14 mit Premium-Festplatten gehen Sie [Storage Premium-Konto für einen virtuellen Datenträger](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)zu erstellen.

*Cache-Vorwärmung*  
Datenträger mit ReadOnly Zwischenspeichern werden können höhere IOPS als das Limit erreicht. Um diese maximale Leistung bei Lesevorgängen vom hostcache abzurufen, müssen zuerst Sie den Cache des Laufwerks warm. Dadurch die Benchmarktool auf "CacheReads" Laufwerk lesen IOs tatsächlich den Cache und die Festplatte direkt trifft. Cache-Treffer Ergebnis in zusätzliche IOPS aus dem Cache aktiviert Datenträger.

>**Wichtig:**  
>Müssen Aufwärmen des Caches vor jedem Neustart VM ausführen benchmarking.

#### <a name="iometer"></a>Iometer   
[Laden Sie Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) auf dem virtuellen Computer.

*Datei prüfen*  
Iometer verwendet eine Testdatei, die auf dem Volume gespeichert sind auf dem benchmarking Test ausgeführt wird. Es Laufwerken liest und schreibt auf dieser Datei auf die Festplatte messen IOPS und Durchsatz. Iometer erstellt dieser Datei, wenn Sie nicht angegeben haben. Erstellen einer 200GB-Testdatei Volumes "CacheReads" und NoCacheWrites iobw.tst aufgerufen.

*Access-Spezifikationen*  
Die Spezifikationen Anforderung e/a-Größe % Schreib-, % zufällige/sequenzielle sind konfiguriert über die Registerkarte "Access-Spezifikationen" Iometer. Erstellen einer Zugriffsspezifikation für jedes der nachfolgend beschriebenen Szenarien. Erstellen Sie die Access-Spezifikationen und "Speichern" mit Namen wie RandomWrites\_8 KB, RandomReads\_8 KB. Wählen Sie die entsprechende Spezifikation Ausführung des Testszenarios.

Unten finden Sie ein Beispiel von Zugriffsspezifikationen für maximale IOPS schreiben Szenario,  
    ![](media/storage-premium-storage-performance/image8.png)

*Maximale IOPS Spezifikationen*  
Verwenden Sie zur Veranschaulichung maximale IOPs Anforderung kleiner. 8K Anforderungsgröße verwenden und erstellen für zufällige schreibt und liest.

| Spezifikation für den Datenzugriff | Anforderung | Zufällige % | Lesen % |
|----------------------|--------------|----------|--------|
| RandomWrites\_8K     | 8K           | 100      | 0      |
| RandomReads\_8K      | 8K           | 100      | 100    |

*Maximaler Durchsatz Spezifikationen*  
Verwenden Sie zur Veranschaulichung maximalen Durchsatzes Anforderung größer. Verwenden Sie Anforderungsgröße 64 KB und Spezifikationen für zufällige schreibt und liest.

| Spezifikation für den Datenzugriff | Anforderung | Zufällige % | Lesen % |
|----------------------|--------------|----------|--------|
| RandomWrites\_64K    | 64 KB          | 100      | 0      |
| RandomReads\_64K     | 64 KB          | 100      | 100    |

*Die Iometer auszuführen*  
Führen Sie die erwärmen cache

1.  Erstellen Sie zwei Zugriffsspezifikationen Werte unten,

  	| Name              | Anforderung | Zufällige % | Lesen % |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1MB | 1MB          | 100      | 0      |
  	| RandomReads\_1MB  | 1MB          | 100      | 100    |

2.  Iometer Test zur Initialisierung Cachedatenträger mit folgenden Parametern ausgeführt. Verwenden Sie drei Worker-Threads für das Ziel-Volume und eine Warteschlangenlänge von 128. Auf der Registerkarte "Testen der Installation" "Laufzeit" Dauer des Tests auf 2 Stunden festgelegt.

  	| Szenario              | Ziel-Volume | Name              | Dauer |
  	|-----------------------|---------------|-------------------|----------|
  	| Cachedatenträger initialisieren | "CacheReads"    | RandomWrites\_1MB | 2 Stunden     |

3.  Führen Sie den Test Iometer für die Erwärmung Cachedatenträger mit folgenden Parametern. Verwenden Sie drei Worker-Threads für das Ziel-Volume und eine Warteschlangenlänge von 128. Auf der Registerkarte "Testen der Installation" "Laufzeit" Dauer des Tests auf 2 Stunden festgelegt.

  	| Szenario           | Ziel-Volume | Name             | Dauer |
  	|--------------------|---------------|------------------|----------|
  	| Cachedatenträger Aufwärmen | "CacheReads"    | RandomReads\_1MB | 2 Stunden     |

Nachdem Cachedatenträger erwärmt, mit folgenden Testszenarien fortfahren. Verwenden Sie zum Ausführen des Tests Iometer mindestens drei Worker-Threads für **jedes** Ziel-Volume. Jeder Arbeitsthread wählen Sie das Ziel-Volume, Warteschlangentiefe festgelegt und eines gespeicherten Testspezifikationen führen Sie entsprechende Tests Szenario in der Tabelle angezeigt. Die Tabelle auch Ergebnisse IOPS und Durchsatz beim Ausführen von Tests. Für alle Szenarios wird ein kleines e/a-Größe von 8 KB und hohe Warteschlangenlänge von 128 verwendet.

| Testszenario      | Ziel-Volume | Name              | Ergebnis       |
|--------------------|---------------|-------------------|--------------|
| Max. Lesen Sie IOPS     | "CacheReads"    | RandomWrites\_8K  | 50.000 IOPS  |
| Max. Schreib-IOPS    | NoCacheWrites | RandomReads\_8K   | 64.000 IOPS  |
| Max. Kombinierte IOPS | "CacheReads"    | RandomWrites\_8K  | 100.000 IOPS |
|                    | NoCacheWrites | RandomReads\_8K   |              |
| Max. MB/Sek.   | "CacheReads"    | RandomWrites\_64K | 524 MB/s   |
| Max. MB/s schreiben  | NoCacheWrites | RandomReads\_64K  | 524 MB/s   |
| Kombinierte MB/s    | "CacheReads"    | RandomWrites\_64K | 1000 MB/Sek.  |
|                    | NoCacheWrites | RandomReads\_64K  |              |

Weiter unten finden Sie Screenshots der Iometer Testergebnisse für kombinierte IOPS und Durchsatz.

*Kombinierte Lese- und Schreibvorgänge maximale IOPS*  
![](media/storage-premium-storage-performance/image9.png)

*Kombinierte Lese- und Schreibvorgänge maximalen Durchsatz*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO ist ein beliebtes Tool Benchmark-Speicher unter Linux VMs. Es hat die e/a-Größen, sequenzielle auswählen oder zufällige Lese- und schreibt. Es erzeugt Worker-Threads oder Prozesse angegebenen e/a-Operationen. Sie können e/a-Vorgänge geben, die jeder Arbeitsthread mit Auftragsdateien ausführen muss. Es erstellt eine Auftragsdatei pro Szenario in den folgenden Beispielen dargestellt. Sie können Spezifikationen in Auftragsdateien auf unterschiedliche Arbeitslasten auf Premium Speicher benchmark ändern. In den Beispielen verwenden wir einen Standard DS-14-VM unter **Ubuntu**. Verwenden Sie das gleiche Layout Anfang [Benchmarking Abschnitt](#Benchmarking) beschrieben und aussichtsreich Cache, bevor Sie Benchmark-Tests ausgeführt.

Bevor Sie beginnen, [FIO downloaden](https://github.com/axboe/fio) und auf dem virtuellen Computer installieren.

Führen Sie den folgenden Befehl für Ubuntu,

        apt-get install fio

Wir verwenden vier Arbeitsthreads für Schreibvorgänge fahren und vier Arbeitsthreads für steuernde Lesevorgänge auf dem Datenträger. Arbeitskräfte schreiben fahren Datenverkehr auf dem Volume "Nocache" mit 10 Festplatten mit Cache auf "None" festgelegt. Mitarbeiter lesen fahren Datenverkehr auf dem Datenträger "Readcache" 1 Datenträger mit Cache "ReadOnly" hat.

*Maximale Schreib-IOPS*  
Die Auftragsdatei mit folgenden Spezifikationen maximale schreiben IOPS zu erstellen. Nennen Sie sie "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Beachten Sie die wichtigsten Punkte folgen, die in vorherigen Abschnitten vorgestellten Entwurfsrichtlinien entsprechen. Diese Spezifikationen sind für maximale IOPS Laufwerk,  
-   Eine hohe Warteschlangenlänge von 256.  
-   Eine kleine Blockgröße von 8KB.  
-   Mehrere Threads zufällige Schreibvorgänge durchführen.

Führen Sie den folgenden Befehl zum Auftakt des FIO Tests für 30 Sekunden,  

    sudo fio --runtime 30 fiowrite.ini

Während des Tests werden Sie können die Anzahl der Schreib-IOPS VM und Premium-Datenträger liefern. Wie im folgenden Beispiel gezeigt, liefert die DS14 VM die maximale Schreib-IOPS maximal 50.000 IOPS.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maximale lesen IOPS*  
Die Auftragsdatei mit folgenden Spezifikationen maximale lesen IOPS zu erstellen. Nennen Sie sie "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Beachten Sie die wichtigsten Punkte folgen, die in vorherigen Abschnitten vorgestellten Entwurfsrichtlinien entsprechen. Diese Spezifikationen sind für maximale IOPS Laufwerk,

-   Eine hohe Warteschlangenlänge von 256.  
-   Eine kleine Blockgröße von 8KB.  
-   Mehrere Threads zufällige Schreibvorgänge durchführen.

Führen Sie den folgenden Befehl zum Auftakt des FIO Tests für 30 Sekunden,

    sudo fio --runtime 30 fioread.ini

Während des Tests werden Sie können die Anzahl der gelesenen IOPS VM und Premium-Datenträger liefern. Wie im folgenden Beispiel gezeigt, liefert die DS14 VM mehr als 64.000 lesen IOPS. Dies ist eine Kombination und Cache-Performance.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maximalen Lese- und Schreib-IOPS*  
Erstellen Sie die Auftragsdatei mit folgenden Spezifikationen erhalten kombiniert, lesen und Schreiben IOPS. Nennen Sie sie "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Beachten Sie die wichtigsten Punkte folgen, die in vorherigen Abschnitten vorgestellten Entwurfsrichtlinien entsprechen. Diese Spezifikationen sind für maximale IOPS Laufwerk,

-   Eine hohe Warteschlangenlänge von 128.  
-   Eine kleine Größe von 4KB.  
-   Mehrere Threads ausführen zufällige Lese- und Schreibvorgänge.

Führen Sie den folgenden Befehl zum Auftakt des FIO Tests für 30 Sekunden,

    sudo fio --runtime 30 fioreadwrite.ini

Während der Test ausgeführt wird, wird möglicherweise die Anzahl der kombinierten Lese- und Schreib-IOPS VM und Premium-Datenträger liefern. Wie im folgenden Beispiel gezeigt, liefert der DS14 VM 100.000 kombinierten lesen und Schreiben IOPS. Dies ist eine Kombination und Cache-Performance.  
    ![](media/storage-premium-storage-performance/image13.png)

*Maximale kombiniert Durchsatz*  
Lesen und Schreiben Durchsatz zu maximal kombiniert, eine größere Größe und große Warteschlangentiefe mit mehreren Threads Lese- und Schreibvorgänge durchführen. Sie können einer Blockgröße von 64 KB und Warteschlangentiefe 128.

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen zu Azure Premium-Speicher:

- [Premium-Speicher: Hochleistungsspeicher für Azure Virtual Machine-Arbeitslasten](storage-premium-storage.md)  

SQL Server-Benutzer Artikel auf Best Practices zur Performance für SQL Server:

- [Best Practices zur Performance für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure Storage Premium bietet höchste Leistung für SQL Server in Azure VM](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 

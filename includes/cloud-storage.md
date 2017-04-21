#<a name="data-management-and-business-analytics"></a>Daten-Management und Business Analytics

Verwalten und Analysieren von Daten in der Cloud ist genauso wichtig wie anderswo. Sie können bietet Azure Technologien zum Arbeiten mit relationalen und nicht relationalen Daten. Dieser Artikel führt die Optionen. 

##<a name="table-of-contents"></a>Inhaltsverzeichnis      

- [BLOB-Speicher](#blob)
- [Ein DBMS auf einem virtuellen Computer ausgeführt](#dbinvm)
- [SQL-Datenbank](#sqldb)
    - [SQL Data Sync](#datasync)
    - [SQL Data Reporting mit virtuellen Maschinen](#datarpt)
- [Tabelle speichern](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>BLOB-Speicher

Das Wort "Blob" ist für "BLOB" und genau beschreibt ein Blob ist: eine Sammlung von Binärdaten. Obwohl sie einfach sind, sind Blobs noch nützlich. [Abbildung 1](#Fig1) zeigt die Grundlagen von Azure BLOB-Speicher.

<a name="Fig1"></a>![Diagramm des Blobs][blobs]
 
**Abbildung 1: Azure BLOB-Speicher speichert binäre Daten - Blobs - in Containern.**

Um Blobs zu verwenden, erstellen Sie zuerst ein Azure- *Konto*. Im Rahmen dieser Geben Sie Azure-Rechenzentrum, das Objekte erstellen Sie mit diesem Konto gespeichert werden. Wo befindet, gehört jedes Blob erstellen einige Container Ihres Speicherkontos. Zugriff auf ein Blob stellt eine Anwendung eine URL der Form:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*Container*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; ist eine eindeutige Kennung beim Erstellen ein neuen Speicherkontos während &lt; *Container* &gt; und &lt; *BlobName* &gt; den Namen eines bestimmten Containers und ein Blob innerhalb des Containers. 

Azure bietet zwei Arten von Blobs. Die Optionen sind:

- *Block* -Blobs kann, die jeweils bis zu 200 GB Daten enthalten. Wie der Name andeutet, ist ein Anzahl Blöcke unterteilt. Tritt während der Übertragung ein Fehler kann erneute Übertragung mit den letzten Block anstatt des vollständige BLOBs wieder fortsetzen. Block-Blobs sind ein allgemein Speicher und sind heute am häufigsten verwendete BLOB-Typ.

- *Seite* -Blobs, die als ein Terabyte groß sein kann. Seitenblobs für RAM vorgesehen und so jeweils Anzahl Seiten aufgeteilt. Eine Anwendung kann lesen und Schreiben von Seiten nach dem Zufallsprinzip im Blob. In Azure Virtual Machines verwenden beispielsweise VMs erstellen Seitenblobs als permanente Speicher für Betriebssystem-Datenträger und Datenträger.

Ob Block-Blobs oder Seitenblobs, können Clientanwendungen BLOB-Daten auf verschiedene Weise zugreifen. Die folgenden: Optionen

- Direkt über eine RESTful (z.B. HTTP-basiert) Zugriffsprotokoll. Azure Applications und externen Applikationen, einschließlich apps auf lokal, können diese Option verwenden.
- Azure Storage-Clientbibliothek verwenden, die eine weitere Entwickler-Schnittstelle auf raw RESTful Blob-Zugriffsprotokoll bietet. Wiederum können Azure Applications und externen Applikationen Blobs mit dieser Bibliothek zugreifen.
- Verwenden Azure Laufwerke, eine Option, mit der eine Azure-Anwendung ein Seitenblob als lokales Laufwerk mit NTFS-Dateisystem behandeln können. Die Anwendung sieht aus wie eine normale Windows-Dateisystem mit standardmäßigen Dateioperationen zugegriffen Seitenblob. In der Tat liest und schreibt in der zugrunde liegenden Seitenblob gesendet werden, die das Laufwerk Azure implementiert. 

Schutz vor Hardware-Ausfälle und bessere Verfügbarkeit wird jedes Blob auf drei Computern in Azure-Rechenzentrum repliziert. Schreiben in ein Blob aktualisiert alle drei Kopien später liest inkonsistente Ergebnisse sehen. Sie können auch angeben, dass ein Blob-Daten in einer anderen Azure-Rechenzentrum in der gleichen Geo jedoch mindestens 500 Meilen entfernt kopiert werden sollen. Dieser Kopiervorgang aufgerufen *Geo-Replikation*in den Blob innerhalb weniger Minuten eine Aktualisierung geschieht und ist nützlich für die Disaster Recovery.

Daten in Blobs können auch über Azure *Content Delivery Network (CDN)*bereitgestellt werden. Kopien von BLOB-Daten bei Dutzenden von Servern weltweit Zwischenspeichern kann CDN Zugriff zu beschleunigen, die wiederholt aufgerufen wird. 

Blobs sind sind einfach, die richtige Wahl in vielen Situationen. Speichern und streaming-video und audio sind Beispiele, Backups und andere Arten von Daten archivieren. Entwickler können auch Blobs zu unstrukturierten Daten wie sie. Eine einfache Möglichkeit zum Speichern und Abrufen von Binärdaten kann überraschend nützlich sein.


## <a name="dbinvm"></a>Ein DBMS auf einem virtuellen Computer ausgeführt

Vielen beruhen heute auf Datenbankmanagementsystem (DBMS). Relationale Systeme wie SQL Server sind die am häufigsten verwendeten Wahl, aber nicht relationalen Ansätze, bekannt als *NoSQL* Technologies beliebter täglich erhalten. Cloudanwendungen Verwaltungsoptionen diese Daten verwenden lassen, Azure Virtual Machines können Sie ein DBMS (relationale oder NoSQL) in einer VM. [Abbildung 2](#Fig2) zeigt diese Darstellung mit SQL Server.

<a name="Fig2"></a>![Diagramm der SQL Server auf einem virtuellen Computer][SQLSvr-vm]
 
**Abbildung 2: Azure virtuelle Maschinen können mit einem DBMS in einer VM mit BLOBs bereitgestellt.**

Für Entwickler und Datenbankadministratoren weitgehend dabei dieselbe Software in ihrem eigenen Rechenzentrum ausgeführt. In dem hier gezeigten Beispiel beispielsweise fast alle Funktionen von SQL Server verwendet werden können, und haben vollen administrativen Zugriff auf das System. Sie können die Verantwortung für die Verwaltung des Datenbankservers, als ob es lokal ausgeführt wurden.

Wie in [Abbildung 2](#Fig2) dargestellt, werden die Datenbanken auf der lokalen Festplatte des virtuellen Computers gespeichert werden, der Server läuft in angezeigt. Im Hintergrund wird jedoch die Datenträger in Azure Blob geschrieben. (Es ist wie ein Blob viel agiert wie eine LUN SAN in Ihrem eigenen Datencenter mit) Jede Azure BLOB enthaltenen Daten dreimal in einem Rechenzentrum repliziert werden und wenn es auf einem anderen Rechenzentrum in derselben Region Geo repliziert anfordern. Es kann auch Optionen wie SQL Server-Datenbank Spiegelung für erhöhte Zuverlässigkeit. 

Können mithilfe von SQL Server in einer VM lebt eine hybridanwendung erstellen, Daten auf Azure während die Anwendungslogik lokal ausgeführt wird. Beispielsweise kann dies aussagekräftig Programme an mehreren Standorten oder auf verschiedenen mobilen Geräten dieselben Daten gemeinsam nutzen müssen. Um Kommunikation zwischen Cloud-Datenbank und lokalen Logik einfacher zu machen, können eine Organisation Azure Virtual Network Sie ein virtuelles privates Netzwerk (VPN) Verbindung zwischen Azure-Rechenzentrum und eigenen lokalen Datencenter.


## <a name="sqldb"></a>SQL-Datenbank

Für viele Personen ist ein DBMS in einer VM ausführen die erste Option für die Verwaltung strukturierter Daten in der Cloud Gedanke. Es ist nicht die einzige Wahl, noch ist es immer die beste Wahl. In einigen Fällen ist das Verwalten von Daten mit einer Plattform als Service (PaaS) Ansatz sinnvoller. Azure bietet PaaS-Technologie SQL-Datenbank, die Sie für relationale Daten ermöglicht. [Abbildung 3](#Fig3) zeigt diese Option. 

<a name="Fig3"></a>![Diagramm der SQL-Datenbank][SQL-db]
 
**Abbildung 3: SQL-Datenbank bietet einen gemeinsamen PaaS relationale Speicher.**

SQL-Datenbank geben nicht jeder Kunde eine eigenen physische Instanz von SQL Server. Stattdessen bietet es einen Multi-Tenant-Dienst einen logischen Datenbank SQL Server für jeden Kunden. Alle Kunden Teilen Computing- und Speicherkapazitäten, der der Dienst bereitstellt. Und mit BLOB-Speicher auf drei separaten Computern innerhalb einer Azure-Rechenzentrum mit Datenbanken integrierte hohe Verfügbarkeit (HA) alle Daten in SQL-Datenbank gespeichert.

Eine Anwendung weitgehend SQL Datenbank SQL Server. Applikationen können SQL-Abfragen in relationalen Tabellen ausgeben T-SQL gespeicherte Prozeduren verwenden und Ausführen von Transaktionen über mehrere Tabellen hinweg. Und da Applikationen SQL mit dem Tabular Data Stream (TDS)-Protokoll dasselbe Protokoll verwendet, auf SQL Server-Datenbank können sie Daten mit Entity Framework, ADO.NET und JDBC, andere bekannte Datenzugriffsschnittstellen arbeiten. 

Aber da SQL Datenbank einen Cloud-Dienst in Azure Rechenzentren ausgeführt wird, müssen Sie das System physischen Aspekte wie Datenträger verwalten. Sie müssen Software aktualisieren oder andere systemnahe administrative Aufgaben kümmern. Jeder Kundenorganisation steuert weiterhin eine eigene Datenbanken, einschließlich Schemas und Benutzernamen, aber das langweilige Verwaltungsaufgaben für Sie erledigt. 

Während SQL Datenbank viel SQL Server-Anwendung aussieht, es genau wie ein DBMS auf einer physikalischen oder virtuellen Computer verhält sich nicht. Da es auf gemeinsam genutzter Hardware ausgeführt wird, variiert die Leistung aller Kunden auf Hardware Auslastung. Dies bedeutet, dass die Leistung, beispielsweise eine gespeicherte Prozedur in SQL-Datenbank von einem Tag zu variieren. 

Heute können SQL-Datenbank eine Datenbank bis zu 150 GB erstellen. Benötigen Sie größere Datenbanken arbeiten, bietet der Dienst eine Option namens *Föderation*. Zu diesem Zweck erstellt ein Datenbankadministrator mindestens zwei *Mitglieder*, jeweils einer separaten Datenbank mit eigenen Schema. Daten verteilt diese Member, die *Sharding*mit einer eindeutigen *Verbundschlüssel*zugewiesen genannt wird. Eine Anwendung Probleme SQL Abfragen Daten durch, der Föderation Member der Abfrage identifiziert Verbundschlüssel angeben sollten. Dadurch können mit einem herkömmlichen relationalen Datenmengen. Wie immer gibt es Nachteile. weder Abfragen noch Transaktionen können Mitglieder, beispielsweise umfassen. Relationale PaaS-Diensts ist am besten, diese Kompromisse sind zulässig mit SQL Föderation kann jedoch eine gute Lösung.

SQL-Datenbank kann durch Programme auf Azure, wie im lokalen Datencenter verwendet. Dies ist für Cloudanwendungen, die relationale Daten sowie lokale Anwendung, die Daten in der Cloud speichern profitieren. Eine mobile Anwendung hängen eventuell von SQL-Datenbank zur Verwaltung von freigegebener relationaler Daten, beispielsweise als möglicherweise Inventory-Anwendung, die auf mehrere Händler weltweit.

Denken SQL Datenbank löst ein Problem offensichtlich (und wichtige): Wenn führen Sie SQL Server in einer VM und SQL Datenbank besser ist? Wie gewohnt sind Kompromisse und Ihre Bedürfnisse hängt sich besser ist. 

Eine einfache Möglichkeit zum Nachdenken ist SQL-Datenbank als neue Anwendung SQL Server auf einem virtuellen Computer besser ist, wenn Sie eine vorhandene Anwendung in die Cloud lokalen verschieben anzeigen. Es kann auch sich diese Entscheidung auf eine differenziertere Weise jedoch nützlich. SQL-Datenbank ist z. B. einfacher, da es minimal ist, Einrichtung und Verwaltung. Aber mit SQL Server in einer VM können vorhersagbare Leistung ist nicht freigegebener Dienst --unterstützt auch größere nicht verbundene Datenbanken als SQL-Datenbank. SQL-Datenbank enthält jedoch integrierte Replikation von Daten und Verarbeitung effektiv ein DBMS hohe Verfügbarkeit mit sehr geringem Aufwand. Während SQL Server Ihnen mehr Kontrolle und ein etwas breiteres Optionen bietet, ist SQL Datenbank einfacher und deutlich weniger Arbeit verwalten.

Schließlich ist es wichtig darauf hinweisen, dass SQL-Datenbank nicht nur PaaS Datendienst in Azure verfügbar ist. Microsoft-Partner bieten weitere Optionen. ClearDB bietet eine MySQL PaaS bietet Cloudant eine Option NoSQL verkauft. PaaS Data Services sind in vielen Situationen die richtige Lösung, und so ist dieser Ansatz für das Datenmanagement Bestandteil Azure.


### <a name="datasync"></a>SQL Data Sync

Während SQL Datenbank drei Kopien von jeder Datenbank innerhalb eines einzigen Datencenters Azure betreibt, replizieren nicht automatisch Daten zwischen Azure-Datencentern. Stattdessen stellt SQL Data Sync, ein Dienst, dazu. [Abbildung 4](#Fig4) zeigt, wie diese aussieht.

<a name="Fig4"></a>![Diagramm der SQL Daten synchronisieren][SQL-datasync]
 
**Abbildung 4: SQL Data Sync synchronisiert Daten in SQL-Datenbank mit Daten in anderen Azure und lokale Rechenzentren.**

Wie das Diagramm zeigt, kann SQL Data Sync Daten an unterschiedlichen Standorten synchronisieren. Nehmen Sie eine Anwendung in mehrere Azure Rechenzentren beispielsweise mit SQL-Datenbank ausführen. SQL Data Sync können Sie die Daten synchronisiert bleiben. SQL Data Sync können auch zwischen Azure-Rechenzentrum und einer Instanz von SQL Server auf einem lokalen Rechenzentrum synchronisieren. Möglicherweise hilfreich, eine lokale Kopie der Daten lokal Applikationen und eine Cloud auf Windows Azure ausgeführte Anwendung verwendet werden. Und obwohl es nicht in der Abbildung dargestellt ist, SQL Data Sync kann auch verwendet werden Synchronisieren von Daten zwischen SQL-Datenbank und SQL Server in einer VM auf Azure oder ausgeführt.

Synchronisierung kann bidirektional und bestimmen, welche Daten synchronisiert werden und wie häufig erfolgt. (Synchronisierung zwischen Datenbanken ist nicht atomar, -gibt es immer zumindest einige Verzögerung) Und jedoch verwendet wird, die Synchronisierung mit SQL Data Sync ist vollständig konfigurierbare; Es ist kein Code schreiben.


### <a name="datarpt"></a>SQL Data Reporting mit virtuellen Maschinen

Sobald eine Datenbank Daten enthält, wird jemand wahrscheinlich mit Daten Berichte erstellen. Azure führen SQL Server Reporting Services (SSRS) in Azure Virtual Machines ist funktional dem lokalen SQL Server Reporting Services ausgeführt. SSRS können Sie dann in einer Azure SQL-Datenbank gespeicherten Daten Berichte ausführen.  [Abbildung 5](#Fig5) zeigt den Prozess.

<a name="Fig5"></a>![Diagramm der SQL reporting][SQL-report]
 
**Abbildung 5: SQL Server Reporting Services mit einer Azure Virtual Machines bietet reporting Services auf SQL-Datenbank. .**

Bevor ein Benutzer einen Bericht sehen, definiert eine Person Bericht (Schritt 1) aussehen. Mit SSRS auf einer VM kann dies mit zwei Tools: SQL Server Data Tools, Teil von SQL Server 2012 oder Vorgänger Business Intelligence (BI) Development Studio. Diese Berichtsdefinitionen werden mit SSRS, in der Berichtsdefinitionssprache (RDL) ausgedrückt. Nachdem die RDL-Dateien für einen Bericht erstellt haben, werden sie in einer VM in der Cloud (Schritt 2) hochgeladen. Die Berichtsdefinition ist jetzt einsatzbereit.

Ein Benutzer der Anwendung greift dann auf Bericht (Schritt 3). Die Anwendung übergibt diese Anforderung der SSRS-VM (Schritt 4) die Kontakte SQL-Datenbank oder anderen Datenquellen Daten muss (Schritt 5). SSRS verwendet diese Daten und relevanten RDL-Dateien zum Rendern des Berichts (Schritt 6), dann gibt Bericht der Anwendung (Schritt 7) für den Benutzer (Schritt 8) angezeigt wird.

Einen Bericht in eine Anwendung einbetten, hier dargestellte Szenario ist nicht die einzige Option. Es kann auch zum Anzeigen von Berichten im Berichts-Manager auf dem virtuellen Computer SharePoint auf dem virtuellen Computer oder auf andere Weise. Berichte können mit einem mit einem Link zu einem anderen Bericht auch kombiniert werden.

SSRS auf Azure-VM bietet volle Funktionalität als reporting-Lösung in der Cloud. Berichte können alle Datenquellen SSRS unterstützt. Anwendung und Berichte gehören eingebetteten Code oder Assemblys unterstützen benutzerdefinierte Verhaltensweisen. Ausführung und Darstellung sind schnell, da Berichtinhalt und Modul zusammen auf dem gleichen virtuellen Server ausgeführt.



## <a name="tblstor"></a>Tabelle speichern

Relationale Daten ist in vielen Situationen hilfreich, aber es ist nicht immer die richtige Wahl. Wenn Ihre Anwendung schnellen, einfachen Zugriff auf sehr große Mengen von lose strukturierten Daten, beispielsweise funktionieren eine relationale Datenbank gut nicht. NoSQL-Technologie ist eine bessere Option sein.

Azure Table Storage ist ein Beispiel für diese Art von NoSQL-Ansatz. Trotz seines Namens unterstützt nicht Tabellenspeicher standard relationale Tabellen. Stattdessen wird ein *Schlüssel-Wert-speichern*Daten mit einem bestimmten Schlüssel zuordnen und dass eine Zugriff Daten genannte von Key angeben. [Abbildung 6](#Fig6) zeigt die Grundlagen.

<a name="Fig6"></a>![Diagramm der Tabellenspeicher][SQL-tblstor]
 
**Abbildung 6: Azure Table Storage ist ein Schlüssel-Wert-Speicher, der schnellen, einfachen Zugriff auf große Mengen von Daten bietet.**

Wie Blobs ist Azure Storage-Konto zugeordnet jede Tabelle. Tabellen werden auch ähnlich wie Blobs URL eines Formulars bezeichnet.

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*Tabellenname*&gt;

Wie die Abbildung zeigt jede Tabelle eine Anzahl von Partitionen, gliedert die auf einem separaten Computer gespeichert werden können. (Dies ist eine Form von Sharding, wie mit SQL.) Azure Applications und Applikationen an anderer Stelle können eine Tabelle mit den Rest OData-Protokoll oder Azure Storage-Clientbibliothek zugreifen.

Jede Partition in einer Tabelle enthält eine Anzahl von *Entitäten*mit bis zu 255 *Eigenschaften*. Jede Eigenschaft hat einen Namen, einen Typ (wie Binary, Bool, DateTime, Int oder String) und einen Wert. Im Gegensatz zu relationalen Speicher dieser Tabellen haben keine feste Schema und so verschiedene Entitäten in derselben Tabelle können mit unterschiedlichen Eigenschaften enthalten. Eine Entität möglicherweise nur eine Zeichenfolgeneigenschaft mit einem Namen und einer anderen Entität in der gleichen Tabelle zwei Int-Eigenschaften mit einer Kundennummer und Kreditwürdigkeit.

Zum Identifizieren einer bestimmten Entität in einer Tabelle stellt eine Anwendung diese Entitätsschlüssel. Der Schlüssel besteht aus zwei Teilen: einem *Partitionsschlüssel* , identifiziert eine bestimmte Partition und eine *Zeilenschlüssel* , eine Entität innerhalb der Partition identifiziert. In [Abbildung 6](#Fig6)beispielsweise fordert der Client die Entität mit dem Partition A und Zeilenschlüssel 3 und Tabellenspeicher gibt die Entität, einschließlich aller darin enthaltenen Eigenschaften.

Dadurch kann große Tabellen - eine einzelne Tabelle kann bis zu 100 TB Daten – enthalten und ermöglicht schnellen Zugriff auf die darin enthaltenen Daten. Es bringt jedoch auch eingeschränkt. Beispielsweise ist keine Unterstützung für transaktionale Updates, die Tabellen oder sogar Partitionen in einer einzelnen Tabelle umfassen. Eine Reihe von Updates für eine Tabelle kann nur in einer atomaren Transaktion gruppiert werden, wenn alle Beteiligten in der gleichen Partition. Es ist auch keine Möglichkeit zum Abfragen einer Tabelle basierend auf dem Wert seiner Eigenschaften es gibt Unterstützung für Verknüpfungen über mehrere Tabellen hinweg. Und im Gegensatz zu relationalen Datenbanken Tabellen keine Unterstützung für gespeicherte Prozeduren.

Azure Table Storage ist eine gute Wahl für Applikationen, die schnellen, preiswerte Zugriff auf große Mengen von lose strukturierten Daten. Beispielsweise kann eine internetanwendung, die Profilinformationen für viele Benutzer speichert Tabellen verwenden. Schneller Zugriff ist in diesem Fall wichtig, und die Anwendung möglicherweise nicht die volle Leistungsfähigkeit von SQL. Geschwindigkeit und Größe dieser Funktionalität geben kann manchmal sinnvoll und Tabellenspeicher ist genau die richtige Lösung für Probleme.


## <a name="hadoop"></a>Hadoop

Organisationen haben jahrzehntelang Datawarehouses aufgebaut. Diese Sammlungen häufig in relationalen Tabellen gespeicherten Informationen können Personen arbeiten und lernen Sie auf viele verschiedene Arten von Daten. Mit SQL Server ist es beispielsweise üblich, Tools wie SQL Server Analysis Services verwenden.

Aber angenommen, Sie möchten Analyse auf nicht-relationalen Daten. Ihre Daten können viele Formen annehmen: Informationen von Sensoren oder RFID-Tags, Protokolldateien in Serverfarmen Clickstreamdaten von ASP.NET-Webanwendungen Bilder aus medizinischen Diagnose-Geräte und mehr. Diese Daten möglicherweise sehr groß, zu groß, um effektiv mit einem herkömmlichen Datawarehouse verwendet werden. Große Probleme so selten vor ein paar Jahren geworden häufig.

Große Daten analysieren, wurde weitgehend Branche eine Projektmappe zusammengeführt: Open Source-Technologie Hadoop. Die Daten auf auf diesen Computern funktioniert und parallele Verarbeitung Hadoop in einem Cluster aus physischen oder virtuellen Computern ausgeführt. Weitere Computer hat Hadoop verwenden, schneller können sie unabhängig arbeiten bietet.

Diese Art von Problem ist ideal für die öffentliche Cloud. Statt der Pflege einer Armee von lokalen Servern ungenutzt möglicherweise viel Zeit mit Hadoop in der Cloud erstellen kann (und bezahlen) VMs nur bei Bedarf. Sogar mehr big Data Hadoop analysieren möchten entsteht in der Cloud speichern die Mühe verschoben wird. Um diese Synergien zu erleichtern, stellt Microsoft Hadoop Dienst auf Azure. [Abbildung 7](#Fig7) zeigt die wichtigsten Komponenten dieses Dienstes.

<a name="Fig7"></a>![Diagramm der hadoop][hadoop]

**Abbildung 7: Hadoop auf Azure wird MapReduce-Aufträgen, die Daten über mehrere virtuelle Computer gleichzeitig ausgeführt.**

Um Hadoop auf Azure zu verwenden, Fragen Sie diese Cloud-Plattform zum Erstellen eines Clusters Hadoop die Anzahl virtueller Computer Sie müssen. Hadoop Cluster selbst ist eine wichtige Aufgabe, und so lassen Sie Azure sinnvoll. Sie Cluster verwenden, Sie Herunterfahren. Besteht keine Serverressourcen bezahlen, die Sie nicht verwenden.

Eine Hadoop, ein *Auftrag*, der so genannte Anwendung ein Programmiermodell *MapReduce*genannt. Wie in die Abbildung dargestellt, wird die Logik für einen MapReduce-Auftrag gleichzeitig über viele VMs ausgeführt. Daten Parallel verarbeiten kann Hadoop Daten schneller als einzelne Computer Solutions analysieren.

Azure Daten einen MapReduce Auftrag funktioniert in der Regel im BLOB-Speicher bleibt. Hadoop erwarten, MapReduce Aufträge Daten in *Hadoop verteilt Datei System bietet*. BIETET ist BLOB-Speicher in mancher Hinsicht ähnlich. repliziert Daten auf mehreren physischen Servern, beispielsweise. Anstatt diese Funktionen macht Hadoop auf Azure BLOB-Speicher über die API bietet stattdessen wie in der Abbildung dargestellt. Während die Logik eines MapReduce-Auftrags denkt, normale bietet Dateien zugreift, funktioniert der Auftrag tatsächlich mit Daten von Blobs zu übertragen. Und um unterstützen mehrere Aufträge über dieselben Daten Ausführungsort, Hadoop auf Azure kopieren Daten aus Blobs in vollständigen bietet im virtuellen Computer ausgeführt. 

MapReduce-Jobs werden häufig in Java heute eine Methode geschrieben, die Hadoop auf Azure unterstützt. Microsoft hat außerdem MapReduce Arbeitsplätze in anderen Sprachen, einschließlich C#, F# und JavaScript unterstützt. Soll dieser großen Technologie eine größere Gruppe von Entwicklern leichter zugänglich machen.

BIETET und MapReduce enthält Hadoop Technologien, die Daten zu analysieren, ohne einen MapReduce-Auftrag sich Besucher. Schwein ist z. B. eine übergeordnete Sprache zum Analysieren von großen Daten und Struktur eine SQL-ähnliche Sprache namens HiveQL. Schweine und Struktur tatsächlich Arbeitsplätze MapReduce, die Prozessdaten bietet aber sie dieser Komplexität Benutzer ausblenden. Beide werden mit Hadoop auf Azure bereitgestellt.

Microsoft bietet auch ein HiveQL für Excel. Mit Excel-add-in können die Ergebnisse mit PowerPivot und andere Excel-Tools Geschäftsanalysten HiveQL Abfragen (und MapReduce Aufträge) direkt aus Excel erstellen und verarbeiten und visualisieren. Hadoop auf Azure enthält anderen Technologien, wie Bibliotheken Mahout, Graph Mining System Pegasus und mehr Computer.

Große Datenanalyse ist wichtig und Hadoop gilt auch. Mit Hadoop als verwalteter Dienst Azure sowie Links zu bekannten Tools wie Excel soll Microsoft diese Technologie für eine größere Gruppe von Benutzern.

Alle Arten von Daten ist, wichtig. Deshalb Azure eine Reihe von Optionen für Daten-Management und Business Analytics. Anwendung versuchen zu erstellen, ist es wahrscheinlich finden Sie etwas in dieser Cloud-Plattform funktionieren Sie.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png

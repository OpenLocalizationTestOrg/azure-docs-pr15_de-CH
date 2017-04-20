<properties
    pageTitle="DocumentDB Speicherplatz und Leistung | Microsoft Azure" 
    description="Informationen Sie zum Datenspeicher und Speicherung von Dokumenten mit DocumentDB wie DocumentDB Kapazität Bedürfnisse Ihrer Anwendung skaliert werden kann." 
    keywords="Speicherung von Dokumenten"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Erfahren Sie mehr über Speicher und vorhersehbare Performance Bereitstellung DocumentDB
Azure DocumentDB ist eine vollständig verwaltete, skalierbare dokumentorientiert NoSQL Datenbankdienst für JSON-Dokumente. Mit DocumentDB müssen Sie virtuelle Computer Mieten, Software bereitstellen oder Datenbanken überwachen. DocumentDB wird und von Microsoft-Experten weltweit Verfügbarkeit, Leistung und Datenschutz zu überwacht.  

Sie können DocumentDB erstellen [ein Konto](documentdb-create-account.md) mit einer [DocumentDB-Datenbank](documentdb-create-database.md) über das [Azure-Portal](https://portal.azure.com/)beginnen. DocumentDB-Datenbanken werden in Einheiten von Solid-State Drive (SSD) unterstützt Speicher- und Durchsatz angeboten. Diese Lagereinheiten werden in Ihrem Konto, jede Auflistung mit reservierten [Datenbank Sammlungen](documentdb-create-collection.md) bereitgestellt, die den Anforderungen der Anwendung jederzeit nach oben oder unten skaliert werden kann. 

Überschreitet die Anwendung den reservierten Durchsatz einer oder mehrerer Sammlungen, werden Anfragen pro pro Sammlung begrenzt. Dies bedeutet, dass einige Anträge möglicherweise fehlschlagen, während andere möglicherweise eingeschränkt.

Dieser Artikel bietet eine Übersicht über Ressourcen und Metriken zur Speicherkapazität und Planen Sie die Speicherung von Daten. 

## <a name="database-account"></a>Konto
Als Abonnent Azure können Sie ein oder mehrere DocumentDB Datenbankkonten verwalten Sie Ihre Ressourcen bereitstellen. Jedes Abonnement ist einzelne Azure-Abonnement zugeordnet. 

DocumentDB Konten können über das [Azure-Portal](documentdb-create-account.md)oder mit [einer Azure-CLI oder ARM-Vorlage](documentdb-automation-resource-manager-cli.md)erstellt werden.

## <a name="databases"></a>Datenbanken
Eine einzelne DocumentDB Datenbank kann praktisch unbegrenzte Speicherung gruppiert Sammlungen enthalten. Sammlungen bieten Performance-Isolierung – jede Auflistung Durchsatz, die nicht mit anderen in derselben Datenbank oder Konto freigegeben bereitgestellt werden kann. DocumentDB Datenbank ist elastisch Größe von GB bis TB SSD unterstützt Speicherung und bereitgestellten Durchsatz. Im Gegensatz zu einer herkömmlichen RDBMS-Datenbank eine Datenbank in DocumentDB nicht auf einen einzelnen Computer begrenzt und kann mehrere Computer oder Cluster umfassen.  

Mit DocumentDB können müssen Sie Ihre Anwendung skalieren Sie weitere Sammlungen und/oder Datenbanken erstellen. Datenbanken können über das [Azure-Portal](documentdb-create-database.md) oder über eine [DocumentDB-SDKs](documentdb-dotnet-samples.md)erstellt werden.   

## <a name="database-collections"></a>Datenbanksammlungen
Jeder DocumentDB-Datenbank kann eine oder mehrere Sammlungen enthalten. Sammlungen verhalten sich wie Partitionen für die Speicherung und Verarbeitung hoher Verfügbarkeit. Jede Auflistung kann Dokumente mit heterogenen Schema speichern. DocumentDB die automatische Indizierung und Abfragefunktionen können einfach filtern und Abrufen von Dokumenten. Eine Auflistung enthält den Bereich Dokument Datenspeicher- und Ausführung. Eine Auflistung ist ebenfalls eine Transaktion Domäne für alle darin enthaltenen Dokumente. Sammlungen werden basierend auf dem Wert in der Azure-Portal oder über SDKs Durchsatz zugeordnet. 

Sammlungen werden durch DocumentDB automatisch in einem oder mehreren physischen Servern aufgeteilt. Wenn Sie eine Auflistung erstellen, können Sie bereitgestellten Durchsatz Anforderung Einheiten pro Sekunde und eine Schlüsseleigenschaft Partition angeben. Der Wert dieser Eigenschaft wird von DocumentDB zum Verteilen von Dokumenten zwischen Partitionen und Anfragen wie Abfragen. Der Schlüsselwert Partition fungiert auch als Transaktionsgrenze für gespeicherte Prozeduren und Trigger. Jede Auflistung hat eine reservierte Durchsatz für diese Auflistung nicht mit anderen Sammlungen im selben Konto freigegeben wird. Daher können Sie Ihre Anwendung Speicher und Durchsatz geeignet. 

Sammlungen können über das [Azure-Portal](documentdb-create-collection.md) oder über eine [DocumentDB-SDKs](documentdb-sdk-dotnet.md)erstellt werden.   
 
## <a name="request-units-and-database-operations"></a>Einheiten und Datenbankoperationen anfordern

Wenn Sie eine Auflistung erstellen, reservieren Sie Durchsatz [anforderungseinheiten (RU)](documentdb-request-units.md) pro Sekunde. Stattdessen und Verwalten von Hardware-Ressourcen, Sie können eine **Anforderung Einheit** als ein Maß für die Ressourcen vorstellen zu verschiedenen Datenbankoperationen eine Anwendung Anforderung erforderlich. Lesen eines Dokuments 1 KB verwendet dasselbe 1 RU unabhängig von der Anzahl der Elemente in der Auflistung oder die Anzahl der gleichzeitigen Anfragen gleichzeitig ausführen gespeichert. Alle Anfragen für DocumentDB, einschließlich komplexer Vorgänge wie SQL-Abfragen einen vorhersagbaren RU-Wert, der zum Zeitpunkt der Entwicklung ermittelt werden können. Sollten Sie die Größe Ihrer Dokumente und die Häufigkeit der einzelnen Vorgänge (Lesevorgänge, Schreibvorgänge und Abfragen) für Ihre Anwendung unterstützen, können Sie den genauen Betrag der Durchsatz der Anwendung Bedürfnisse bereitstellen und Ihrer Datenbank nach oben oder unten skalieren, wie Ihre Leistung ändern. 

Jede Sammlung kann mit in Blöcken von 100 anforderungseinheiten pro Sekunde aus Hunderten Millionen anforderungseinheiten pro Sekunde reserviert werden. Der bereitgestellte Durchsatz kann angepasst werden, während der gesamten Lebensdauer einer Auflistung zu verarbeiten ändern und Muster der Anwendung zugreifen. Weitere Informationen finden Sie unter [Leistung DocumentDB](documentdb-performance-levels.md). 

>[AZURE.IMPORTANT] Sammlungen sind berechenbare Entitäten. Kosten bestimmt den bereitgestellten Durchsatz gemessen in anforderungseinheiten pro Sekunde mit insgesamt belegte Speicher in Gigabyte Auflistung. 

Wie viele anforderungseinheiten verbrauchen ein bestimmtes Vorgangs wie einfügen, löschen, Abfrage oder gespeicherte Prozedur? Eine Anforderung Einheit misst eine normalisierte Verarbeitungskosten Anforderung. Lesen eines Dokuments 1 KB ist 1 RU jedoch eine Anforderung einfügen, ersetzen oder löschen Sie das gleiche Dokument verbrauchen mehr Verarbeitungseinheiten vom Dienst und damit weitere Anforderung. Jede Antwort vom Dienst umfasst einen benutzerdefinierten Header (`x-ms-request-charge`) meldet, dass die anforderungseinheiten für die Anforderung verwendet. Dieser Header ist auch über [SDKs](documentdb-sdk-dotnet.md). [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) in .NET SDK ist eine Eigenschaft des [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) -Objekts. Ggf. der Durchsatz muss vor Aufruf schätzen können [Kapazitätsplaner](documentdb-request-units.md#estimating-throughput-needs) Sie mithilfe dieser Schätzung. 

>[AZURE.NOTE] Basisplan 1 Anforderung Einheit für ein Dokument 1 KB entspricht einer einfachen GET des Dokuments mit [Session-Konsistenz](documentdb-consistency-levels.md). 

Es gibt mehrere Faktoren, die die anforderungseinheiten für eine Operation gegen eine Datenbank DocumentDB verbraucht auswirken. Zu diesen Faktoren zählen:

- Dokumentgröße. Zunehmender Dokumentgröße Einheiten zum Lesen oder schreiben die Daten erhöht auch verbraucht.
- Eigenschaftenanzahl. Wenn Standard Indizieren aller Eigenschaften der Einheiten verwendet, um ein Dokument zu schreiben wird zunehmender Count-Eigenschaft erhöht.
- Datenkonsistenz. Bei Ebenen Konsistenz Data starke oder Alterung begrenzt werden zusätzliche Einheiten verbraucht Dokumente lesen.
- Indizierte Eigenschaften. Eine Index-Richtlinie für jede Auflistung bestimmt Eigenschaften standardmäßig indiziert werden. Ihre Anforderung Einheitenverbrauch reduzieren durch Beschränken der Anzahl indizierter Eigenschaften. 
- Dokument zu indizieren. Standardmäßig alle Dokumente automatisch indiziert, verbrauchen Sie weniger anforderungseinheiten möchten Sie nicht einige Ihrer Dateien indizieren.

Weitere Informationen finden Sie unter [DocumentDB anforderungseinheiten](documentdb-request-units.md). 

Hier ist z. B. eine Tabelle mit wie viele anforderungseinheiten Bereitstellung in drei unterschiedliche Größen (1 KB, 4 KB und 64 KB) und zwei unterschiedliche Leistungsstufen (500 Lesevorgänge pro Sekunde 100 Schreibvorgänge pro Sekunde und 500 Lesevorgänge/s + 500 Schreibvorgänge pro Sekunde). Die Datenkonsistenz Sitzung konfiguriert wurde und die Indizierung Richtlinie auf None festgelegt wurde.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Dokumentgröße</strong></p></td>
            <td valign="top"><p><strong>Lesevorgänge pro Sekunde</strong></p></td>
            <td valign="top"><p><strong>Schreibzugriffe pro Sekunde</strong></p></td>
            <td valign="top"><p><strong>Anforderungseinheiten</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1.000 RU-s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3.000 RU-s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1.350 RU-s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4150 RU-s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9800 RU-s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29.000 RU-s</p></td>
        </tr>
    </tbody>
</table>

Abfragen, gespeicherten Prozeduren und Triggern nutzen anforderungseinheiten je nach Komplexität der Operationen. Überprüfen Sie während der Anwendungsentwicklung Anforderungsheader Zuschlag zum besseren Verständnis wie jede Operation Anforderung Einheit Kapazität verbraucht.  


## <a name="choice-of-consistency-level-and-throughput"></a>Auswahl Konsistenz und Durchsatz
Die Wahl der Standardebene Konsistenz wirkt sich auf den Durchsatz und Wartezeit. Sie können die Standardstufe Konsistenz programmgesteuert sowohl Azure-Portal festlegen. Sie können auch auf Konsistenz pro Anforderung überschreiben. Standardmäßig wird auf Konsistenz **Sitzung**monotone Lese-/Schreibvorgänge bereit festlegen und lesen die Garantien schreiben. Session-Konsistenz ist für benutzerzentrierte Applikationen und bietet eine perfekte Konsistenz und Leistung Kompromisse.    

Informationen zum Ändern der Konsistenz der Azure-Portal finden Sie unter [DocumentDB-Konto verwalten](documentdb-manage-account.md#consistency). Oder Weitere Informationen auf Konsistenz [mit konsistenzebenen](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Bereitgestellte Dokument Speicher- und Indexdateien overhead
DocumentDB unterstützt die Erstellung einer Partition und partitioniert. Jede Partition DocumentDB unterstützt bis zu 10 GB Speicher SSD unterstützt. 10GB Speicherplatz Dokument enthält Dokumente sowie Speicher für den Index. Standardmäßig ist eine DocumentDB Auflistung konfiguriert automatisch alle Dokumente indiziert ohne explizit alle sekundären Indizes oder Schema. Applikationen mit DocumentDB basiert auf der normalen Indizierungsaufwand zwischen 2 und 20 %. Die Indizierung von DocumentDB verwendete Technologie wird sichergestellt, dass unabhängig von den Werten der Eigenschaften der Indizierungsaufwand nicht mehr als 80 % der Größe der Dokumente mit den Standardeinstellungen. 

Standardmäßig werden alle Dokumente automatisch vom DocumentDB indiziert. Wenn Sie den Indizierungsaufwand optimieren möchten, können Sie bestimmte Dokumente indiziert gleichzeitig einfügen oder ein Dokument beschriebenen [DocumentDB Indexierungsrichtlinien](documentdb-indexing-policies.md)entfernen. Sie können eine DocumentDB Auflistung aller Dokumente in der Auflistung von der Indizierung ausschließen. Sie können auch eine DocumentDB Auflistung selektiv nur bestimmte Eigenschaften oder Pfade mit Platzhaltern Ihrer Dokumente JSON Indizierung Siehe [Konfigurieren der Indizierung einer Auflistung](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection). Ausschließen von Eigenschaften oder Dokumente verbessert auch Schreibdurchsatz – d. h. weniger anforderungseinheiten genutzt wird.   

## <a name="next-steps"></a>Nächste Schritte

Weiter Kennenlernen der Funktionsweise von DocumentDB finden Sie unter [Partitioning und Skalierung in Azure DocumentDB](documentdb-partition-data.md).

Informationen zur Überwachung der Leistung der Azure-Portal finden Sie unter [Monitor DocumentDB-Konto](documentdb-monitor-accounts.md). Weitere Informationen zum Auswählen von Leistungsniveau Sammlungen finden Sie unter [Leistungsmerkmale in DocumentDB](documentdb-performance-levels.md).
 

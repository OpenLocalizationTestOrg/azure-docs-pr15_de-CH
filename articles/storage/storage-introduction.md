<properties
    pageTitle="Einführung in Speicher | Microsoft Azure"
    description="Übersicht der Azure-Speicher, Microsoft Online-Datenspeicher in der Cloud. Erfahren Sie, wie die beste verfügbare Cloud Storage-Lösung in Ihrer Anwendung verwenden."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Einführung in Microsoft Azure-Speicher

## <a name="overview"></a>Übersicht

Azure-Speicher ist der Cloud-Storage-Lösung für moderne, die auf Dauerhaftigkeit, Verfügbarkeit und Skalierbarkeit für die Bedürfnisse ihrer Kunden. Lesen Sie diese Artikel, Entwickler, IT-Spezialisten und geschäftliche Entscheidung erfahren Entscheidungsträger:

- Was Azure-Speicher ist, und wie können in der Cloud, mobile nutzen, Server und desktop-Applikationen
- Welche Daten Sie in Azure Storage Services speichern können: BLOB-Daten (Objekt), Tabellendaten NoSQL Warteschlangennachrichten und Dateifreigaben.
- Verwaltung der Zugriff auf Ihre Daten in Azure-Speicher
- Wie ist Ihre Azure Daten über Redundanz- und Replikationsoptionen haltbar gemacht
- Wo Sie als Nächstes Ihre erste Azure-Speicher-Anwendung erstellen

Betrieb mit Azure-Speicher schnell finden Sie unter [Erste Schritte mit Azure Storage in fünf Minuten](storage-getting-started-guide.md).

Informationen zu Tools, Bibliotheken und anderen Ressourcen für die Arbeit mit Azure Storage finden Sie [Weiter](#next-steps) unten.

## <a name="what-is-azure-storage"></a>Was ist der Azure-Speicher?

Cloud-computing ermöglicht neue Szenarien für Applikationen, skalierbare, robuste und hochverfügbare Storage für ihre Daten genau warum Microsoft Azure-Speicher entwickelt. Zusätzlich ermöglichen Entwicklern umfangreiche Anwendung neue Szenarios unterstützen, hinaus Azure Storage Storage Foundation für Azure Virtual Machines, ein weiterer Beweis für Stabilität.

Azure-Speicher ist hochgradig skalierbar, können gespeichert und Hunderte Terabyte Daten die großen Szenarien erforderlich wissenschaftliche, finanzielle Analyse und Mediaprogramme. Oder Sie kleinen Mengen von Daten für small Business-Website. Wo Ihre Bedürfnisse liegen bezahlen Sie nur die Daten, die Sie speichern. Azure-Speicher derzeit Zehntausende Billionen Kundenobjekte speichert und Millionen von Anfragen pro Sekunde durchschnittlich behandelt.

Azure-Speicher ist elastisch, Anträge auf einem globalen Publikum entwerfen und skalieren Anwendungsbereiche - gespeicherte Datenmenge und Anträge vor. Sie bezahlen nur für Sie verwenden und nur, wenn Sie es verwenden.

Azure-Speicher verwendet eine automatische Partitionierung System, die automatisch Daten basierend auf Datenverkehr Lastausgleich. Dies bedeutet, dass als Belastung der Anwendung vergrößern Azure-Speicher automatisch die entsprechenden Ressourcen zu ihnen weist.

Azure-Speicher kann von überall auf der Welt aus jeder Art von Anwendung, ob die Ausführung in der Cloud, auf dem Desktop auf einem lokalen Server oder auf einem mobilen oder Tablet. Azure-Speicher können in mobilen Szenarios, in dem die Anwendung eine Teilmenge der Daten auf dem Gerät gespeichert und mit vollständigen Daten in der Cloud synchronisiert.

Azure-Speicher unterstützt Clients mit unterschiedlichen Betriebssystemen (einschließlich Windows und Linux) und eine Vielzahl von Programmiersprachen (einschließlich .NET Java, Node.js, Python, Ruby, PHP und C++ und mobile Programmiersprachen) für einfache Entwicklung. Azure-Speicher stellt Datenressourcen über einfache REST-APIs, die jeder Client senden und Empfangen von Daten über HTTP/HTTPS.

Azure Storage Premium bietet hohe Leistung, niedriger Latenz Datenträger Unterstützung für e/a-intensive Arbeitslasten auf Azure virtuellen Computer ausgeführt. Mit Azure Premium Speicher können mehrere dauerhafte Festplatten zu einer virtuellen Maschine Anfügen und konfigurieren, um Ihre Performance zu erfüllen. Jeder Datenträger wird durch eine SSD-Festplatte in Azure Premium Speicher für maximale e/a-Leistung unterstützt. Siehe [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](storage-premium-storage.md) Weitere Informationen.

## <a name="introducing-the-azure-storage-services"></a>Einführung in Azure-Speicherdienste

Azure-Speicher bietet die folgenden vier Dienste: BLOB-Speicher, Table Storage Queue Storage und Dateispeicher.

- BLOB-Speicher speichert unstrukturierte Daten. Ein Blob kann beliebigen Text oder Binärdaten, Dokument, Datei oder Anwendung Installer. BLOB-Speicher wird auch als Objektspeicher bezeichnet.
- Tabellenspeicher speichert strukturierte Datasets. Tabellenspeicher ist ein NoSQL Schlüsselattribut Datenspeicher, schnelle Entwicklung und schnellen Zugriff auf große Mengen von Daten ermöglicht.
- Queue Storage bietet zuverlässiges messaging für Workflow-Verarbeitung und Kommunikation zwischen Komponenten eines Cloud-Dienste.
- Dateispeicher bietet freigegebenen Speicher für Legacyanwendungen mit SMB-Standardprotokoll. Azure virtuelle Computer und Clouddienste Daten über Anwendungskomponenten über bereitgestellte Freigaben freigeben und lokale Programme können Daten in einer Freigabe über den Dienst REST-API zugreifen.

Ein Azure Storage-Konto ist ein sicheres Konto, das Ihnen Zugriff auf Dienste in Azure-Speicher. Das Speicherkonto stellt den eindeutigen Namespace für Ihre Speicherressourcen. Die Abbildung unten zeigt die Beziehung zwischen Azure-Speicher-Ressourcen in ein Speicherkonto:

![Azure Storage-Ressourcen](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>BLOB-Speicher

BLOB-Speicher bietet für Benutzer mit großen Mengen von unstrukturierten Daten in der Cloud speichern kostengünstige und skalierbare Lösung. BLOB-Speicher können Sie Inhalte speichern:

- Dokumente
- Soziale Daten wie Fotos, Videos, Musik und blogs
- Sicherungskopien von Dateien, Computer, Datenbanken und Geräte
- Bilder und Text für ASP.NET-Webanwendungen
- Konfigurationsdaten für Cloudanwendungen
- Große Daten wie Protokolle und andere große datasets

Jedes Blob ist in Container unterteilt. Container stellen auch eine praktische Möglichkeit, Sicherheitsrichtlinien Objekte zuweisen. Ein Speicherkonto kann eine beliebige Anzahl von Containern enthalten und Container dürfen eine beliebige Anzahl von Blobs bis 500 TB Kapazitätsgrenze des Speicherkontos.  

BLOB-Speicher bietet drei Arten von Blobs Blobs blockieren, Blobs und Seitenblobs (Datenträger) anfügen.

- Block-Blobs für streaming und Cloud-Objekte speichern optimiert und eignen sich gut zum Speichern von Dokumenten, Mediendateien, Backups etc..
- Append Blobs Block-Blobs ähneln, sind aber für Vorgänge anfügen. Ein Blob anhängen kann aktualisiert werden, werden am Ende einen neuen Block hinzufügen. Append Blobs sind eine gute Wahl für Szenarien wie Protokollierung, wo neue Daten nur bis zum Ende des Blobs geschrieben werden.
- Seitenblobs für die Darstellung von IaaS Festplatten optimiert und zufällige schreibt und kann bis zu 1 TB groß sein. Ein Azure virtuellen Netzwerk angefügt IaaS Datenträger eine virtuelle Festplatte als Seitenblob gespeichert ist.

Für große Datasets Engpässe stellen hochladen oder Herunterladen von Daten auf BLOB-Speicher über die Leitung unrealistisch liefern Ihnen eine Festplatte Microsoft importieren oder Exportieren von Daten direkt vom Rechenzentrum. Siehe [Verwenden der Microsoft Azure Import/Export-Dienst Datenübertragung BLOB-Speicher](storage-import-export-service.md).

## <a name="table-storage"></a>Tabelle speichern

Moderne Applikationen erfordern oft Datenspeichern breiter skalierbar und flexibel als vorherige Software erforderlich. Tabellenspeicher bietet hochverfügbaren, massiv skalierbaren Speicher, so dass die Anwendung automatisch Benutzer Bedarf skalieren kann. Tabellenspeicher Microsoft NoSQL Key-Attribut Speicher – hat eine schemalose Design, das sich von herkömmlichen relationalen Datenbanken. Mit einem schemalos Datenspeicher ist einfach Daten müssen Ihre Anwendung entwickeln. Tabellenspeicher ist benutzerfreundlich, damit Entwickler Programme erstellen können. Zugriff auf Daten ist schnell und kostengünstig für alle Anwendungstypen.  Tabellenspeicher ist normalerweise deutlich Kosten als herkömmliche SQL für ähnliche Datenmengen.

Tabellenspeicher ist ein Schlüsselattribut Speicher bedeutet, dass jeder Wert in einer Tabelle mit einem typisierten Eigenschaftennamen gespeichert wird. Der Eigenschaftenname kann zum Filtern und Auswahlkriterien verwendet werden. Eine Auflistung von Eigenschaften und deren Werten besteht eine Entität aus. Da Tabellenspeicher schemalos ist zwei Entitäten in derselben Tabelle enthalten verschiedene Sammlungen von Eigenschaften und Eigenschaften können verschiedener Art sein.

Table Storage können Sie flexible Datasets wie Benutzerdaten für Web Applications, Adressbücher, Informationen und eine andere Art von Metadaten, die der Dienst benötigt speichern.  Sie können eine beliebige Anzahl von Entitäten in einer Tabelle speichern und ein Speicherkonto kann eine beliebige Anzahl von Tabellen, bis die Kapazitätsgrenze des Speicherkontos enthalten.

Entwickler können Blobs und Warteschlangen verwalten und Zugriff auf Tabelle Speicher mit standard REST Protokolle jedoch Tabellenspeicher auch eine Teilmenge der OData-Protokoll unterstützt erweiterte Abfragefunktionen und Aktivieren von JSON und AtomPub (XML Basis) vereinfachen Formate.

Für heutige Internet-basierte Programme bieten NoSQL-Datenbanken Tabellenspeicher eine beliebte Alternative zum herkömmlichen relationalen Datenbanken.

## <a name="queue-storage"></a>Warteschlangenspeicher

Entwerfen Sie Applikationen für Skalierung, Anwendungskomponenten oft entkoppelt, damit sie unabhängig voneinander skaliert werden können. Warteschlangenspeicher bietet reliable Messaging-Lösung für die asynchrone Kommunikation zwischen Komponenten, ob sie in der Cloud, auf dem Desktop auf einem lokalen Server oder auf einem mobilen Gerät ausgeführt werden. Warteschlangenspeicher unterstützt asynchrone Aufgaben und Prozess-Workflows erstellen.

Ein Speicherkonto kann eine beliebige Anzahl von Warteschlangen enthalten. Eine Warteschlange kann eine beliebige Anzahl von Nachrichten, bis die Kapazitätsgrenze des Speicherkontos enthalten. Einzelne Nachrichten können bis zu 64 KB groß sein.

## <a name="file-storage"></a>Dateispeicher

Azure Dateispeicher bietet SMB-Dateifreigaben cloudbasierten Dateifreigaben in Azure schnell und ohne teure schreibt auf Legacyanwendungen migriert werden können. Mit Azure Speicherung können Programme Azure virtuelle Computer oder Cloud-Dienste eine Dateifreigabe in der Cloud bereitstellen, wie eine desktop-Anwendung eine normale SMB-Freigabe hängt. Eine beliebige Anzahl von Anwendungskomponenten kann bereitgestellt und gleichzeitig Zugriff auf die Dateifreigabe Speicher.

Da eine Dateifreigabe Storage standard SMB-Dateifreigabe ist, können in Azure ausgeführt Daten in die Freigabe über Dateisystem I/O APIs zugreifen. Entwickler können daher ihre vorhandenen Code und wissen zum Migrieren der vorhandenen Anwendung nutzen. IT-Experten können PowerShell-Cmdlets erstellen, bereitstellen und Verwalten von freigegebenen Speicher als Teil der Verwaltung von Azure Applications.

Wie andere Azure Storage Services macht Dateispeicher REST API für den Zugriff auf Daten in einer Freigabe. Lokale Programme können den Dateispeicher REST-API zugreifen auf Daten in einer Dateifreigabe aufrufen. Auf diese Weise können Unternehmen einige Legacyanwendungen in Azure migrieren und andere im Unternehmen ausgeführt. Beachten Sie, dass eine Dateifreigabe bereitstellen nur möglich in Azure ausgeführt wird; eine lokale Anwendung kann nur die Dateifreigabe über die REST-API zugreifen.

Verteilte Programme können auch Dateispeicher speichern und Freigeben von Daten nützlich, Entwicklung und Test-Tools. Beispielsweise kann eine Anwendung Konfigurationsdateien speichern und Diagnosedaten Protokolle Metriken und Crash dumps in einer Datei Speicher freigeben, so dass sie mehrere virtuelle Computer oder Rollen verfügbar sind. Entwickler und Administratoren können Dienstprogramme, die sie erstellen oder Verwalten einer Anwendung in eine Dateifreigabe für Speicher, die alle Komponenten zur speichern auf jedem virtuellen Computer oder die Rolleninstanz installieren.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Zugriff auf Blob, Tabelle, Warteschlange und Ressourcen

Standardmäßig kann nur der Kontobesitzer Speicher im Speicherkonto zugreifen. Für die Sicherheit Ihrer Daten muss jeder Anforderung für Ressourcen in Ihrem Konto authentifiziert werden. Authentifizierung basiert auf einem Modell Shared Key. BLOBs können auch anonyme Authentifizierung konfiguriert werden.

Das Speicherkonto erhält zwei private Zugriffstasten erstellen, die für die Authentifizierung verwendet werden. Zwei Schlüssel stellt sicher, dass die Anwendung verfügbar bleiben, wenn die Schlüssel regelmäßig als gemeinsame Sicherheitsmethode Schlüsselmanagement regenerieren.

Wenn Sie müssen Benutzer gesteuerten Zugriff auf die Speicherressourcen ermöglichen eine SAS erstellen können. SAS (SAS) ist ein Token, das eine URL angehängt werden kann, die delegierten auf eine Speicherressource Zugriff. Wer das Token besitzt kann auf die Ressource zugreifen, zeigt die Berechtigungen angibt, für den Zeitraum dieser gültig ist. Ab Version 2015-04-05 Azure-Speicher unterstützt zwei Arten von SAS: service SAS und SAS berücksichtigen.

Dienst-SAS delegiert Zugriff auf eine Ressource nur einen Speicher-Services: Service Blob, Warteschlange, Tabelle oder Datei.

Eine Konto-SAS delegiert Zugriff auf Ressourcen in einem oder mehreren Speicher-Services. Mit einem SAS-Dienst keine stehen kann Zugriff auf Dienstebene Vorgänge delegiert werden. Sie können auch Zugriffsrechte für Stellvertretung zum Lesen, schreiben und Löschen von Blob-Container, Tabellen, Warteschlangen und Dateifreigaben, die nicht mit einem SAS-Dienst.

Schließlich können Sie angeben, dass ein Container und dessen Blobs oder ein bestimmtes BLOB-öffentlich zugänglicher. Wenn Sie angeben, dass ein Container oder Blob öffentlich ist, kann jeder Benutzer anonym gelesen werden. Es ist keine Authentifizierung erforderlich.  Öffentlichen Container und Blobs sind hilfreich für die Bereitstellung von Ressourcen wie Medien und Dokumente, die auf Websites gehostet werden.  Zum Verringern der Netzwerkwartezeiten für eine globale Zielgruppe können BLOB-Daten von Websites mit Azure CDN Zwischenspeichern.

Finden Sie weitere Informationen zu SAS [Verwenden freigegebene Access-Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md) . Weitere Informationen zum sicheren Zugriff auf das Speicherkonto finden Sie unter [Manage anonymen Lesezugriff Container und Blobs](storage-manage-access-to-resources.md) und [Authentifizierung für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179428.aspx) .

## <a name="replication-for-durability-and-high-availability"></a>Replikation auf Haltbarkeit und hohe Verfügbarkeit

Die Daten in Ihrem Microsoft Azure Storage-Konto werden immer repliziert, Haltbarkeit und hohe Verfügbarkeit. Replikation kopiert die Daten im selben Rechenzentrum oder in einem zweiten Rechenzentrum je nach der Replikationsoption. Replikation schützt Ihre Daten und Ihre Anwendungszeit bei transienten Hardwarefehler behält. Wenn Ihre Daten in einem zweiten Rechenzentrum repliziert werden, schützt auch gegen ein schwerwiegender Fehler im primären Standort.

Replikation wird sichergestellt, dass das Speicherkonto [Service Level Agreement (SLA) für die Speicherung](https://azure.microsoft.com/support/legal/sla/storage/) trotz Fehlern entspricht. Finden Sie die SLA Informationen zum Azure-Speicher für Langlebigkeit und Verfügbarkeit gewährleistet. 

Wenn Sie ein Speicherkonto erstellen, können Sie eine der folgenden Optionen für die Replikation auswählen:  

- **Lokal redundanter Speicher (LRS).** Lokal redundanter Speicher verwaltet drei Datenkopien. LRS ist dreimal in einem einzigen Rechenzentrum in einer Region repliziert. LRS schützt Ihre Daten aus normalen Hardwarefehler, nicht jedoch von der Ausfall eines einzelnen Rechenzentrums.  

    LRS wird zu einem ermäßigten Preis angeboten. Für maximale Haltbarkeit empfiehlt Geo-redundanten Speicher beschrieben verwenden.


- **Zone-redundanten Speicher (ZRS).** Zone-redundanten Speicher verwaltet drei Datenkopien. ZRS ist dreimal in zwei bis drei Funktionen in einem einzigen Bereich oder zwei Regionen repliziert bietet höhere Haltbarkeit als LRS. ZRS stellt sicher, dass Ihre Daten in einem einzigen Bereich ist.  

    ZRS bietet eine höhere Haltbarkeit als LRS. Allerdings empfiehlt für maximale Haltbarkeit Geo-redundanten Speicher beschrieben verwenden.  

    > [AZURE.NOTE] ZRS ist derzeit nur für Block-Blobs und ist nur für Versionen 2014-02-14 und höher unterstützt.
    >
    > Nachdem Sie das Speicherkonto erstellt und ZRS ausgewählt haben, kann nicht konvertieren in einen anderen Typ von Replikation verwendet oder umgekehrt.

- **Geo-redundanten Speicher (GRS)**. GRS verwaltet sechs Kopien Ihrer Daten. Mit GRS Ihre Daten ist dreimal im Bereich für primäre und auch dreimal in einer sekundären Hunderte Meilen von der primären Region auf höchstem Niveau Haltbarkeit repliziert werden. Bei einem Ausfall der primären Region wird Azure Storage Failover auf den sekundären Bereich. G wird sichergestellt, dass Daten in zwei separaten Bereichen haltbar ist.

    Informationen über primäre und sekundäre Kombinationen nach Region finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

- **Lesezugriff Geo redundanten Speicher (RA-GRS)**. Lesezugriff Geo redundante Speicherung die Daten auf einen sekundären Standort repliziert und zudem Lesezugriff auf die Daten an den sekundären Standort. Lesezugriff Geo redundante Speicherung ermöglicht Zugriff von der primären oder sekundären Standort, wenn ein Standort ausfällt. Lesezugriff Geo redundante Speicherung ist die Standardoption für das Speicherkonto standardmäßig beim Erstellen. 

    > [AZURE.IMPORTANT] Sie können ändern, wie die Daten repliziert werden, nach dem Erstellen Ihres Speicherkontos, wenn Sie beim Erstellen des Kontos ZRS angegeben. Beachten Sie jedoch, dass Sie eine zusätzliche einmalige Datenübertragung wechseln von LRS g oder RA-GRS Kosten entstehen können.

Weitere Informationen zu Optionen für die Replikation finden Sie unter [Azure Storage Replication](storage-redundancy.md) .

Preisinformationen für Speicherreplikation Konto finden Sie unter [Azure Storage Preise](https://azure.microsoft.com/pricing/details/storage/). Weitere Informationen, welche Dienste verfügbar, in jeder Region sind finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/#services) .

Architektonische Details Lebensdauer Azure-Speicher finden Sie unter [SOSP Papier - Azure Storage: ein hochgradig verfügbaren Cloud-Speicherservice mit starken Konsistenz](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Datenübertragung zu und von Azure-Speicher

Das Befehlszeilenprogramm AzCopy können BLOB-Datei und die Daten in das Speicherkonto oder Konten Speicher kopieren. Weitere Informationen finden Sie in der [Datenübertragung mit AzCopy Befehlszeilenprogramm](storage-use-azcopy.md) .

AzCopy ist der derzeit in der Vorschau auf [Azure Data Bewegung Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)basiert.

Azure Import/Export-Dienst ermöglicht das BLOB-Daten importieren oder Exportieren von BLOB-Daten aus dem Speicherkonto über eine Festplatte auf Azure Data Center gesendet. Weitere Informationen zum Import/Export-Dienst finden Sie unter [Verwenden des Microsoft Azure Importieren/Exportieren von Blob-Speicher übertragen](storage-import-export-service.md).

## <a name="pricing"></a>Preisgestaltung

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Speicher-APIs, Bibliotheken und Tools

Azure Speicherressourcen durch jede beliebige Sprache möglich, die HTTP/HTTPS-Anfragen stellen können. Darüber hinaus bietet Azure Storage Programmierbibliotheken für verschiedene gängige Sprachen. Diese Bibliotheken vereinfachen viele Aspekte der Arbeit mit Azure Storage Details wie synchrone und asynchrone Aufruf behandeln, Batchverarbeitung Operationen, Verwaltung von Ausnahmen, automatische Wiederholungsfunktion Ausführungsverhalten usw.. Bibliotheken sind derzeit für die folgenden Sprachen und Plattformen mit anderen in der Pipeline:

### <a name="azure-storage-data-services"></a>Azure Storage Data Services

- [Storage Services REST-API](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Speicher-Clientbibliothek für .NET, Windows Phone und Windows-Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Speicher-Clientbibliothek für C++](https://github.com/Azure/azure-storage-cpp)
- [Speicher-Clientbibliothek für Java-Android](/develop/java/)
- [Speicher-Clientbibliothek für Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Speicher-Clientbibliothek für PHP](/develop/php/)
- [Speicher-Clientbibliothek für Ruby](/develop/ruby/)
- [Speicher-Clientbibliothek für Python](/develop/python/)
- [Speicher-Cmdlets für PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure Storage Management Services

- [Storage Resource Provider REST-API-Referenz](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Storage Provider Client Ressourcenbibliothek für .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Storage Resource Provider Cmdlets für PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Storage Service Management REST API (klassisch)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure-Speicher Datentransfer-Services

- [Storage Import/Export-Dienst REST-API](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Speicher Daten Bewegung-Clientbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Tools und Dienstprogramme

- [Azure-Speicher-Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure-Speicher-Clienttools](storage-explorers.md)
- [Azure SDKs und Tools](https://azure.microsoft.com/tools/)
- [Azure Speicheremulator](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [AzCopy-Befehlszeilen-Dienstprogramm](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie erfahren Sie mehr über Azure-Speicher:

### <a name="documentation"></a>Dokumentation

- [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Für Administratoren

- [Verwenden von Azure PowerShell mit Azure-Speicher](storage-powershell-guide-full.md)
- [Verwenden von Azure CLI mit Azure-Speicher](storage-azure-cli.md)

### <a name="for-net-developers"></a>Für .NET Entwickler

- [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md)
- [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md)
- [Erste Schritte mit Azure Queue Storage mit .NET](storage-dotnet-how-to-use-queues.md)
- [Erste Schritte mit Windows Azure Dateispeicher](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Für Java-Android-Entwickler

- [Verwendung von Java-BLOB-Speicher](storage-java-how-to-use-blob-storage.md)
- [Wie Tabellenspeicher aus Java](storage-java-how-to-use-table-storage.md)
- [Verwendung von Java Warteschlangenspeicher](storage-java-how-to-use-queue-storage.md)
- [Verwendung von Java Dateispeicher](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Node.js-Entwickler

- [Verwendung von Node.js-BLOB-Speicher](storage-nodejs-how-to-use-blob-storage.md)
- [Verwendung von Node.js Tabellenspeicher](storage-nodejs-how-to-use-table-storage.md)
- [Verwendung von Node.js Warteschlangenspeicher](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Für PHP-Entwickler

- [Verwendung von PHP-BLOB-Speicher](storage-php-how-to-use-blobs.md)
- [Verwendung von PHP Tabellenspeicher](storage-php-how-to-use-table-storage.md)
- [Verwendung von PHP Warteschlangenspeicher](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Ruby-Entwickler

- [Verwendung von Ruby-BLOB-Speicher](storage-ruby-how-to-use-blob-storage.md)
- [Verwendung von Ruby Tabellenspeicher](storage-ruby-how-to-use-table-storage.md)
- [Verwendung von Ruby Warteschlangenspeicher](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python-Entwickler

- [Verwendung von Python-BLOB-Speicher](storage-python-how-to-use-blob-storage.md)
- [Verwendung von Python Tabellenspeicher](storage-python-how-to-use-table-storage.md)
- [Verwendung von Python Warteschlangenspeicher](storage-python-how-to-use-queue-storage.md)
- [Verwendung von Python Dateispeicher](storage-python-how-to-use-file-storage.md)

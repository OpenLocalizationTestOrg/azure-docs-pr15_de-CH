<properties 
    pageTitle="Verschieben von Daten zwischen Clouddatenbanken skalierten | Microsoft Azure" 
    description="Erläutert die Splitter verändern und Verschieben von Daten über ein lokal gehosteter Dienst elastische Datenbank-APIs verwenden." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Verschieben von Daten zwischen Clouddatenbanken skalierten

Wenn Sie Software als Service-Entwickler, und Ihre app plötzlich Nachfrage wird, müssen Sie das Wachstum. So fügen Sie weitere Datenbanken (Splitter). Verteilen Daten auf die neuen Datenbanken wie, ohne die Datenintegrität beeinträchtigen? **Split - Dienstprogramm** zum Verschieben von Daten aus eingeschränkten Datenbanken auf die neuen Datenbanken verwenden.  

Split-Dienstprogramm wird als Azure Webdienst ausgeführt. Ein Administrator oder Entwickler verwendet das Tool Shardlets (Daten aus einen) zwischen verschiedenen Datenbanken (Splitter) verschieben. Das Tool verwendet Splitter Karte Management die Metadaten-Datenbank und konsistente Zuordnung sicherzustellen.

![Übersicht][1]

## <a name="download"></a>Herunterladen
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Dokumentation
1. [Elastische Split-Merge Tool Lernprogramm](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Split-Merge-Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Split-Merge-Sicherheitsaspekte](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Splitter Karte management](sql-database-elastic-scale-shard-map-management.md)
* [Migrieren Sie bestehende Datenbanken nach Dezentrales Skalieren](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Elastische Datenbanktools](sql-database-elastic-scale-introduction.md)
* [Elastische Datenbank Tools Glossar](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Warum verwenden Split-Dienstprogramm?

**Flexibilität**

Muss flexibel Grenzen einer Azure SQL DB einzelnen Strecken. Verwenden Sie das Tool zum Verschieben von Daten als neue Datenbanken zur Beibehaltung der Integrität.

**Teilen zu** 

Gesamtkapazität explosionsartig behandeln zu müssen. Dazu erstellen Sie zusätzlichen Kapazität von Sharding Daten und auf inkrementell weitere Datenbanken verteilt, bis Kapazitätsbedarf erfüllt werden. Dies ist ein gutes Beispiel der Funktion "Teilen". 

**Seriendruck verkleinern**

Kapazität muss saisonale aufgrund eines Unternehmens verkleinert. Das Werkzeug weniger Maßeinheiten verkleinern, wenn Business verlangsamt. Das Feature 'Merge' in elastische Skalierung Split-Merge-Dienst umfasst diese Anforderung. 

**Verwalten Sie Hotspots verschieben shardlets**

Mit mehreren Mandanten pro Datenbank führt die Zuordnung von Shardlets zu Splitter zu Kapazitätsengpässen auf einige Splitter. Dies erfordert erneute Reservierung von Shardlets oder ausgelastet Shardlets in neue oder weniger genutzte Splitter verschieben. 

## <a name="concepts--key-features"></a>Konzepte und wichtige Funktionen

**Kunden-gehosteten Diensten**

Split-Zusammenführen wird als Kunden gehosteten Dienst übermittelt. Sie müssen bereitstellen und hosten Sie den Dienst in Ihrem Microsoft Azure-Abonnement. Von NuGet herunterladen Paket enthält eine Vorlage mit Informationen für Ihre spezielle Bereitstellung. [Split-Merge-Lernprogramm](sql-database-elastic-scale-configure-deploy-split-and-merge.md) Details anzeigen Der Dienst in Azure-Abonnement läuft, können steuern und konfigurieren die meisten Sicherheitsaspekte des Dienstes. Die Standardvorlage enthält Optionen zum Konfigurieren von SSL, zertifikatbasierten Authentifizierung, Verschlüsselung für gespeicherte Anmeldeinformationen, gegen DoS und IP-Einschränkungen. Weitere Informationen zu Sicherheitsaspekten finden Sie im folgenden Dokument [Teilen zusammenführen Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md).

Standardmäßig bereitgestellt wird eine Arbeitskraft mit einer Webrolle ausgeführt. Jede verwendet A1 virtueller Speicher in Azure Cloud Services. Wenn Sie diese Einstellung ändern, können Sie beim Bereitstellen des Pakets konnte nach einer erfolgreichen Bereitstellung im laufenden Cloud-Dienst (über das Azure-Portal) geändert werden. Beachten Sie, dass die Worker-Rolle aus technischen Gründen nicht für mehr als eine Instanz konfiguriert werden muss. 

**Splitter kartenintegration**

Split-Merge-Dienst interagiert mit der shardzuordnung der Anwendung. Bei Verwendung den Split-Merge-Dienst zu trennen oder Bereiche oder Shardlets zwischen Splitter informiert der Dienst automatisch die shardzuordnung. Hierzu der Dienst verbindet mit der Datenbank Splitter Map-Manager der Anwendung und Bereiche und Mappings Fortschreiten Zusammenführen teilen sich Anfragen verwaltet. Dadurch Splitter Karte stellt immer eine aktuelle Ansicht beim Teilen Zusammenführungsoperationen werden. Teilen, sind zusammen und Shardlet Bewegung Vorhaben durch Verschieben einer Partie Shardlets von Splitter Quelle Ziel-Splitter. Während der Shardlet Verwaltungsvorgang Shardlets unter dem aktuellen Stapel werden in der shardzuordnung als und nicht mit **OpenConnectionForKey** API datenabhängigen Routingverbindungen. 

**Konsistente Shardlet Verbindung**

Beim Starten von Daten für einen neuen Stapel Shardlets Splitter-Schema datenabhängiges routing Verbindungskabel zum Speichern der Shardlet Splitter getötet und nachfolgende Verbindungen sind Splitter Karte APIs diese Shardlets werden gesperrt, während der Datentransfer durchgeführt wird, um Inkonsistenzen zu vermeiden. Verbindung zu anderen Shardlets auf derselben Splitter wird auch getötet, aber wieder erfolgreich sofort auf "Wiederholen". Nachdem der Stapel verschoben wird, die Shardlets werden online für Ziel-Splitter gekennzeichnet, und die Daten aus der Quelle Splitter entfernt. Der Dienst durchläuft diese Schritte für jede Charge bis alle Shardlets verschoben wurden. Dies führt zu mehreren Verbindungsoperationen Kill Verlauf der Zusammenführung teilen sich Vorgang abzuschließen.  

**Shardlet verwalten**

Beschränken die Verbindung zum aktuellen Stapel Shardlets wie oben beschrieben Töten schränkt den Bereich nicht verfügbar für eine Charge Shardlets gleichzeitig. Dies ist ein Ansatz, vollständige Splitter offline für alle Shardlets im Verlauf einer Operation Teilen oder Verbinden bliebe. Die Größe einer Charge als die Anzahl der unterschiedlichen Shardlets gleichzeitig verschieben ist ein Konfigurationsparameter. Sie können für jeden Vorgang verbinden und Teilen je nach Bedarf Verfügbarkeit und Leistung der Anwendung definiert werden. Beachten Sie, dass Bereich in der shardzuordnung gesperrt ist möglicherweise größer als die angegebene Batchgröße. Ist der Dienst nimmt die Größe, die Anzahl der Werte in den Daten Sharding etwa die Batchgröße entspricht. Dies gilt insbesondere für wenig gefüllten Sharding Schlüssel merken. 

**Metadaten-Speicher**

Split-Merge-Dienst verwendet eine Datenbank den Status beibehalten und Protokolle während der anforderungsverarbeitung. Der Benutzer dieser Datenbank im Abonnement erstellt und die Verbindungszeichenfolge bereit, in der Konfigurationsdatei für die Service-Bereitstellung. Administratoren der Organisation des Benutzers können auch mit dieser Datenbank Anforderung Fortschritte und detaillierte Informationen zu möglichen Fehlern.

**Sharding Bewusstsein**

Split-Merge-Dienst unterscheidet (1) Sharding Tabellen Referenztabellen (2) und (3) normale Tabellen. Die Semantik einer Zusammenführung teilen sich Operation hängen vom Typ der verwendeten Tabelle und wie folgt definiert: 

* **Sharding Tabellen**: Teilen, Zusammenführen und verschieben ans Shardlets Quelle Ziel Splitter. Nach dem erfolgreichen Abschluss der allgemeinen Anforderung sind die Shardlets nicht mehr in der Quelle vorhanden. Beachten Sie, dass die Zieltabellen auf Ziel Splitter müssen müssen keine Daten in den Zielbereich vor der Verarbeitung der Operation. 

* **Tabellen**: für Tabellen, die Teilung zusammenführen und Operationen die Daten von der Quelle zum Ziel Splitter verschieben. Beachten Sie jedoch, dass keine Änderungen auf Splitter Ziel für eine Tabelle auftreten, wenn eine Zeile in dieser Tabelle auf dem Ziel bereits vorhanden. Die Tabelle enthält für jede Referenz Tabelle Kopiervorgang verarbeitet werden.

* **Andere Tabellen**: andere Tabellen für die Quelle oder das Ziel eines Vorgangs verbinden und Teilen vorhanden sein können. Split-Merge-Dienst ignoriert diese Tabellen für Daten oder kopieren. Beachten Sie jedoch, dass sie diese Vorgänge bei Einschränkungen beeinträchtigen können.

Auf vs. Sharding Tabellen durch **SchemaInfo** APIs auf die shardzuordnung erfolgt. Das folgende Beispiel veranschaulicht die Verwendung dieser APIs in einer bestimmten Splitter Karte Manager-Objekt Smm: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Die Tabellen "Region" und "Land" als Tabellen definiert und mit Seriendruck teilen sich kopiert werden. 'Debitor' und 'Orders' sind wiederum als Sharding Tabellen definiert. C_CUSTKEY und O_CUSTKEY dienen als der shardingschlüssel. 

**Referenzielle Integrität**

Split-Merge-Diensts Bezüge zwischen Tabellen analysiert und Primärschlüssel Fremdschlüssel zu Tabellen und Shardlets Vorgänge verwendet. Im Allgemeinen werden Tabellen in der Reihenfolge ihrer Abhängigkeit zuerst kopiert, Shardlets in der Reihenfolge ihrer Abhängigkeit innerhalb jeder kopiert werden. Dies ist erforderlich, damit FREMDSCHLÜSSEL-Einschränkungen Splitter Ziel wie die neuen Daten berücksichtigt werden. 

**Splitter Karte Konsistenz und eventuelle Abschluss**

Von Fehlern Split-Merge-Diensts Vorgänge nach einem Ausfall wieder und in Statusabfragen durchführen soll. Es kann jedoch nicht behebbare Situationen z.B. beim Ziel Splitter verloren oder irreparabel beschädigt. Unter diesen Umständen können einige Shardlets, die verschoben werden sollen weiterhin auf Quelle Splitter befinden. Der Dienst wird sichergestellt, dass Shardlet Zuordnung nur aktualisiert werden, nachdem Daten erfolgreich an das Ziel kopiert wurden. Shardlets werden nur in der Quelle gelöscht, ihre Daten in das Ziel kopiert wurden und die entsprechende Zuordnung wurden erfolgreich aktualisiert. Der Löschvorgang erfolgt im Hintergrund, während der Bereich bereits Splitter Ziel online ist. Split-Merge-Dienst gewährleistet immer Richtigkeit der Zuordnung in der shardzuordnung gespeichert.


## <a name="the-split-merge-user-interface"></a>Split-Merge-Benutzeroberfläche

Die Split-Merge-Servicepaket eine Worker-Rolle und eine Webrolle. Die Web-Rolle dient zum Teilen zusammenführen interaktiv einzureichen. Die wichtigsten Komponenten der Benutzeroberfläche sind wie folgt:

-    Vorgangstyp: Den Typ der Operation ist ein Optionsfeld, das steuert die Art von Dienst für diese Anforderung durchgeführt. Sie können zwischen den Teilen zusammenführen und Verschieben von Szenarien. Sie können auch einen zuvor übermittelten Vorgang abbrechen. Sie trennen, Zusammenführen und Verschiebevorgänge Bereich Splitter Karten. Liste Splitter ordnet nur verschieben Maßnahmen.

-    Shardzuordnung: Anforderungsparameter im nächste Abschnitt behandelt der shardzuordnung zu hosten der shardzuordnung Datenbank. Sie müssen den Namen der Azure SQL-Datenbankserver und Datenbank Hosten der Shardmap von Anmeldeinformationen zur Verbindung mit der Datenbank Splitter und schließlich der Name der shardzuordnung. Der Vorgang nimmt derzeit nur einen einzigen Satz von Anmeldeinformationen. Diese Anmeldeinformationen müssen über ausreichende Berechtigungen zum Ändern der Splitter Karte, die Benutzerdaten auf den Splitter durchführen.

-    Quellbereich (zum Teilen und Zusammenführen): Verbinden und Teilen Vorgang verarbeitet einen Bereich mit der niedrigen und hohen Schlüssel. Um einen Vorgang mit einer unbegrenzten hohe Schlüsselwert angeben, das Kontrollkästchen "hohe Schlüssel ist max" und hohe Schlüsselfeld leer. Der Bereichswerte, die Sie angeben müssen nicht genau eine Zuordnung und ihre Grenzen in der shardzuordnung entsprechen. Wenn Grenzen überhaupt Angabe wird der Dienst den nächsten Bereich für Sie automatisch ableiten. GetMappings.ps1-PowerShell-Skript können Sie die aktuellen Zuordnungen in bestimmten shardzuordnung abzurufen.

-    Teilen (Split) Source-Verhalten: für geteilte Vorgänge definieren den Quellbereich teilen. Dazu bietet shardingschlüssel unterbrochen werden soll. Das Optionsfeld verwenden, ob Sie den unteren Teil des Bereichs (ausgenommen geteilten Schlüssel) verschieben möchten, oder soll im oberen Teil (einschließlich des geteilten Schlüssels) verschieben.

-    Shardlet (verschieben) Quelle: Operationen sind verschiedene aus Teilen oder Verbinden sie einen Bereich zum Beschreiben der Quelle nicht erforderlich. Quelle für verschieben wird einfach durch das shardingschlüsselwerts identifiziert, die Sie verschieben möchten.

-    Ziel Splitter (Split): Wenn Sie die Quelle der Teilung Informationen müssen definieren, in der Daten in der Azure SQL Db Server- und Namen für das Ziel kopiert werden sollen.

-    Zielbereich (Zusammenführen): Zusammenführungsoperationen Shardlets zu einer vorhandenen Splitter verschieben. Sie identifizieren den vorhandenen Splitter mit den Grenzen des bestehenden Bereichs, die Sie zusammenführen möchten.

-    Stapelgröße: Die Batchgröße steuert die Anzahl der Shardlets, die jeweils bei der Daten offline gehen wird. Dies ist ein Ganzzahlwert wo Sie kleinere Werte können Sie lange Ausfallzeiten für Shardlets sind. Höhere Werte erhöhen die Zeit einen bestimmten Shardlet offline aber kann die Performance verbessern.

-    Vorgangs-Id (Abbrechen): Haben Sie eine laufende Operation, die nicht mehr benötigt wird, können Sie den Vorgang Abbrechen mit der Vorgangs-ID in diesem Feld. Vorgangs-ID aus der Statustabelle Anforderung abrufen (siehe Abschnitt 8.1) bzw. von der Ausgabe in einem Webbrowser, in dem die Anforderung übermittelt.


## <a name="requirements-and-limitations"></a>Requirements and Limitations 

Die aktuelle Implementierung der Split-Merge-Dienst ist mit folgenden Auflagen und Nachteile: 

* Der Splitter müssen vorhanden und in der shardzuordnung registriert werden, bevor eine geteilte Zusammenführen dieser Splitter ausgeführt werden können. 

* Der Dienst erstellt keine Tabellen oder andere Datenbankobjekte automatisch als Teil der Vorgänge. Dies bedeutet, dass das Schema für alle Sharding Tabellen und Tabellen auf Ziel Splitter vor jedem Zusammenführen teilen sich. Sharding Tabellen müssen insbesondere im Bereich leer werden neue Shardlets Merge teilen sich Vorgang hinzugefügt werden. Die anfängliche Konsistenzprüfer auf Ziel-Splitter, fehlschlagen die. Beachten Sie diesen Verweis Daten den Verweis Tabelle leer ist und keine Konsistenz Garantien in Bezug auf andere gleichzeitige Vorgänge für den Referenztabellen schreiben nur kopiert. Empfohlen: Wenn Split/Merge Betrieb keine andere Vorgänge ändern den Referenztabellen.

* Der Dienst setzt auf Zeilenidentität durch einen eindeutigen Index oder Schlüssel, die shardingschlüssel zur Verbesserung der Leistung und Zuverlässigkeit für große Shardlets enthält. Dadurch wird den Dienst Daten sogar eine feinere Granularität als nur die shardingschlüsselwerts verschieben. Dadurch reduzieren die maximale Speicherplatz und sperren, die während des Vorgangs erforderlich sind. Sollten Sie erstellen, einen eindeutigen Index oder Primärschlüssel einschließlich shardingschlüssel für eine gegebene Tabelle Tabelle zusammenführen teilen sich Anfragen verwendet werden soll. Aus Leistungsgründen sollte die shardingschlüssel der führenden Spalte den Schlüssel oder den Index.

* Im Verlauf der Anforderung möglicherweise einige Shardlet Daten sowohl auf der Quell- und Ziel-Splitter. Dies ist zum Schutz vor Ausfällen bei der Shardlet erforderlich. Die Integration von Teilen der Zusammenführung mit der Splitter wird sichergestellt, dass Verbindungen über APIs mit der **OpenConnectionForKey** -Methode der shardzuordnung routing von Daten inkonsistente Zustände angezeigt werden. Jedoch können bei der Verbindung ohne Verwendung der **OpenConnectionForKey** -Methode mit der Quelle oder Ziel-Splitter inkonsistente Zustände sichtbar, wenn Zusammenführen teilen sich Anfragen werden. Diese Verbindung können teilweise oder doppelte Ergebnisse je nach der Fälligkeit oder die zugrunde liegende Verbindung Splitter angezeigt. Diese Einschränkung betrifft derzeit die Verbindungen elastische Maßstab Multi-Shard-Abfragen.

* Metadaten-Datenbank für das Teilen der Zusammenführung muss zwischen den verschiedenen Rollen nicht freigegeben werden. Die Rolle des Staging mit Split-Merge-Dienst muss z. B. unterschiedlichen Metadaten-Datenbank als Rolle Produktion.
 

## <a name="billing"></a>Abrechnung 

Split-Merge-Dienst führt als Cloud-Dienst in Ihrem Microsoft Azure-Abonnement. Die Instanz des Diensts daher zuweisen Zuschläge für Cloud-Services. Wenn regelmäßig zusammenführen teilen sich Vorgänge ausführen, empfehlen wir den Teilen zusammenführen Cloud-Dienst löschen. Das spart ausgeführt oder Dienstinstanzen Cloud bereitgestellt. Sie können erneut bereitstellen und leicht ausführbare Konfiguration Bedarf Teilen oder Verbinden Operationen starten. 
 
## <a name="monitoring"></a>Überwachung 
### <a name="status-tables"></a>Statustabellen 

Split-Merge-Dienst bietet **RequestStatus** Tabelle in den Metadaten gespeichert Datenbank abgeschlossen und kontinuierliche Überwachung. Die Tabelle enthält eine Zeile für jede Split-Merge-Anforderung, die diese Instanz von Split-Merge-Dienst gesendet wurde. Es gibt für jede Anforderung die folgende Informationen:

* **Zeitstempel**: Uhrzeit und Datum die Anforderung gestartet wurde.

* **OperationId**: eine GUID, die die Anforderung eindeutig identifiziert. Diese Anforderung kann auch verwendet werden, um den Vorgang abzubrechen, während noch läuft.

* **Status**: den aktuellen Status der Anforderung. Laufende Anfragen werden auch die aktuelle Phase die Anforderung ist.

* **CancelRequest**: ein Flag, das angibt, ob die Anforderung abgebrochen wurde.

* **Status**: eine Schätzung Prozentsatz der Fertigstellung für den Vorgang. Ein Wert von 50 bedeutet, dass der Vorgang ungefähr 50 % abgeschlossen ist.

* **Details**: XML-Wert, einen detaillierteren Bericht ausgeführt. Der Bericht wird regelmäßig aktualisiert, wie Zeilen aus Quelle zum Ziel kopiert werden. Fehler oder Ausnahmen enthält diese Spalte auch detailliertere Informationen über den Fehler.


### <a name="azure-diagnostics"></a>Azure-Diagnose

Split-Merge-Diensts verwendet Azure Diagnostics basierend auf Azure SDK 2.5 für Überwachung und Diagnose. Die Diagnosekonfiguration zu steuern, wie nachfolgend beschrieben: [Diagnose in Azure Cloud Services und virtuelle Computer aktivieren](../cloud-services/cloud-services-dotnet-diagnostics.md). Das Downloadpaket enthält zwei Diagnose-Konfigurationen – für die Web-Rolle und eine Worker-Rolle. Diese Diagnose Konfigurationen für den Service Folgen von [Microsoft Azure Cloud Service Grundlagen](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649)der Anleitung. Es enthält Definitionen Leistungsindikatoren, IIS-Protokolle, Windows-Ereignisprotokolle und Teilen zusammenführen Anwendungsereignisprotokolle protokollieren. 

## <a name="deploy-diagnostics"></a>Bereitstellen der Diagnose 

Um Überwachung und Diagnose mit der Diagnose Konfiguration für die Web- und Workerrollen Rollen NuGet-Paket zu aktivieren, führen Sie die folgenden Befehle mit Azure PowerShell: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Weitere Informationen zum Konfigurieren und Bereitstellen von diagnoseeinstellungen: [Diagnose in Azure Cloud Services und virtuelle Computer aktivieren](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Abrufen der Diagnose 

Sie können Visual Studio Server-Explorer in Azure Teil der Server-Explorer-Struktur einfach Dell Diagnostics zugreifen. Öffnen Sie eine Instanz von Visual Studio, und klicken Sie in der Menüleiste auf Ansicht und Server-Explorer. Klicken Sie zum Herstellen der Azure-Abonnement auf Azure. Navigieren Sie zum Azure -> Storage -> <your storage account> -> Tabellen -> WADLogsTable. Weitere Informationen finden Sie unter [Durchsuchen von Speicherressourcen mit Server-Explorer](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

In der Abbildung oben hervorgehoben WADLogsTable enthält detaillierte Ereignisse aus dem Anwendungsprotokoll Split-Merge-Dienst. Beachten Sie, dass eine Bereitstellung in der Produktion die Standardkonfiguration des heruntergeladenen Pakets ausgerichtet ist. Daher ist das Intervall mit Protokollen und Leistungsindikatoren Dienstinstanzen entnommen werden große (5 Minuten). Für Tests und Entwicklung Verringern des Intervalls durch diagnoseeinstellungen im Web oder Worker-Rolle an Ihre Bedürfnisse anpassen. Mit der rechten Maustaste auf die Rolle in der Visual Studio-Server-Explorer (siehe oben), und passen Sie dann übertragen Zeitraum im Dialogfeld für die Diagnose-Konfigurationsdateien: 

![Konfiguration][3]


## <a name="performance"></a>Leistung

Im Allgemeinen ist eine bessere Leistung zu, desto mehr Benutzer Dienstebenen in Azure SQL-Datenbank. Höhere i/o, CPU und Speicher Umlagen für höhere Service-Tiers hinweg nutzen das Massenkopieren und Löschvorgängen, die Split-Merge-Dienst verwendet. Deshalb erhöhen Sie die Dienstebene für diese Datenbanken für einen Zeitraum definiert ist, begrenzt.

Der Dienst führt auch Validierung Abfragen als Teil des normalen Betriebs. Diese Validierung Abfragen unerwartete Vorhandensein von Daten im Zielbereich überprüfen und sicherstellen, dass jede Operation Zusammenführen teilen sich einen konsistenten Zustand beginnt. Alle diese Abfragen arbeiten über Sharding Schlüsselbereiche durch den Gültigkeitsbereich der Operation definiert und als Teil der Anforderungsdefinition Batchgröße. Diese Abfragen am besten bei Index vorhanden ist, die shardingschlüssel als führender Spalte. 

Eine Eigenschaft Eindeutigkeit mit shardingschlüssel als führender Spalte lässt den Dienst einen optimierten Ansatz verwendet, der Ressourcenverbrauch Speicherplatz und Arbeitsspeicher beschränkt. Diese Eindeutigkeit Eigenschaft muss große Datenmengen (in der Regel über 1 GB) verschieben. 

## <a name="how-to-upgrade"></a>Aktualisieren

1. Gehen Sie in [eine Split-Merge-Dienst bereitstellen](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Ändern der Cloud Service-Konfigurationsdatei für Ihre Bereitstellung Teilen zusammenführen entsprechend neue Konfigurationsparameter. Informationen über das Zertifikat für die Verschlüsselung ist ein neuer erforderlichen Parameter. Eine einfache Möglichkeit dazu ist die neue Konfigurationsdatei Vorlage herunterladen mit Ihrer bestehenden Konfiguration zu vergleichen. Stellen Sie sicher, dass Sie die Einstellung für "DataEncryptionPrimaryCertificateThumbprint" und "DataEncryptionPrimary" für das Web und Worker-Rolle hinzufügen.
3. Vor die Aktualisierung auf Azure bereitstellen, daß alle derzeit laufenden Split-Zusammenführungsoperationen haben. Sie können hierzu durch Abfrage der Tabellen RequestStatus und PendingWorkflows in der Datenbank Teilen zusammenführen Metadaten laufenden Anfragen.
4. Aktualisieren der vorhandenen Cloud Service-Bereitstellung für Split-Seriendruck in Azure-Abonnement Konfigurationsdatei aktualisierte Service mit dem neuen Paket.

Sie brauchen eine neue Metadaten-Datenbank für Split-Merge Aktualisieren bereitstellen. Die neue Version wird automatisch die vorhandenen Metadaten-Datenbank auf die neue Version aktualisiert. 

## <a name="best-practices--troubleshooting"></a>Best Practices und Fehlerbehebung
 
-    Test Mieter definieren und Ihre wichtigsten Zusammenführen teilen sich Vorgänge mit dem Test Mieter über mehrere Splitter ausüben. Sicherstellen Sie, dass alle Metadaten in der Splitter-Karte korrekt definiert wird und Operationen Einschränkungen oder Fremdschlüssel nicht verletzen.
-    Test Mieter bleiben Fragen Größe über die maximale Größe der größten Mieter stoßen Sie nicht Größe der Daten sicherstellen. Damit können Sie eine Obergrenze Zeit beurteilen zu einzelnen Mandanten bewegen. 
-    Stellen Sie sicher, dass Ihr Schema unbedruckte Bereiche. Split-Merge-Dienst erfordert die Möglichkeit, Daten aus der Quelle Splitter entfernen, nachdem Daten erfolgreich an das Ziel kopiert wurden. Z. B. **delete-Trigger** können verhindern, dass den Dienst das Löschen der Daten in der Quelle und kann nicht ordnungsgemäß funktionieren.
-    Der shardingschlüssel sollte die führenden Spalten im Primärschlüssel oder eindeutigen Indexdefinition. Gewährleistet die optimale Leistung für Abfragen Validierung Teilen oder verbinden und für die tatsächlichen Daten verschieben und löschen die Schlüsselbereiche Sharding betreiben.
-    Zusammengestellte des Split-Merge-Diensts im Bereich und Daten sich die Datenbanken befinden. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 

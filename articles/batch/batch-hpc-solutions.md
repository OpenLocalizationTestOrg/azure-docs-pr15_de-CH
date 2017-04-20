<properties
   pageTitle="Stapel und HPC Solutions in die Cloud | Microsoft Azure"
   description="Batch und High Performance computing (HPC und Big Compute) Szenarien und Lösungsmöglichkeiten in Azure erfahren"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="07/27/2016"
   ms.author="danlep"/>

# <a name="batch-and-hpc-solutions-in-the-azure-cloud"></a>Stapel und HPC Solutions in der Azure-cloud

Azure bietet effiziente und skalierbare Cloudlösungen für Stapel und High Performance computing (HPC) - auch als *Große berechnen*. Lernen Sie hier Big Compute-Workloads und Azure Services zur Unterstützung oder direkt zum [Lösungsszenarios](#scenarios) in diesem Artikel. Dieser Artikel ist in erster Linie für technische Entscheidungsträger, IT-Manager und unabhängige Softwareanbieter, aber andere IT-Experten und Entwickler können mit dieser Lösung vertraut machen.

Unternehmen haben große computing Probleme: Entwurf und Analyse, Bild Rendern komplexer Modellierung, Monte-Carlo-Simulationen, finanzielle Risiken und andere engineering. Azure hilft Unternehmen lösen mit Ressourcen, Skalierung und planen, die sie benötigen. Mit Azure können Organisationen:

* Lösungen Sie hybride, erweitern einen lokalen HPC Cluster Spitzenarbeitslast in die Cloud verschieben

* HPC-clustertools und Arbeitslasten in Azure ausführen

* Dienste verwaltet und ist skalierbar Azure wie [Batch](https://azure.microsoft.com/documentation/services/batch/) auszuführenden rechenintensive Arbeitslasten ohne bereitstellen und Verwalten von Compute-Infrastruktur

Obwohl Gegenstand dieses Artikels Azure Entwicklern bietet und Partnern umfassende Funktionen, Architektur Auswahlmöglichkeiten und Entwicklungstools umfangreiche, benutzerdefinierte Big Compute Workflows erstellen. Und eine wachsende Partnerökosystem zu Big Compute-Workloads in Azure Cloud produktiv arbeiten können.


## <a name="batch-and-hpc-applications"></a>Stapel und HPC-Anwendung

Im Gegensatz zu Web Applications und viele Line of Business Applications, Stapel und HPC-Anwendung einen definierten Anfang und Ende, und sie können nach einem Zeitplan oder bei Bedarf, manchmal Stunden oder mehr ausführen. Die meisten fallen in zwei Hauptkategorien unterteilt: *systemintern parallel* (manchmal auch "peinlich parallel", da sie lösen Probleme eignen sich parallel auf mehreren Computern oder Prozessoren ausgeführt) und *eng gekoppelt*. In der folgenden Tabelle Weitere Informationen zu dieser Anwendung anzeigen Einige Azure Lösungsansätze für den einen oder anderen besser.

>[AZURE.NOTE] Stapel und HPC-Lösung eine ausgeführte Instanz einer Anwendung heißt in der Regel einen *Auftrag*und jedes Projekt kann in *Aufgaben*unterteilt erhalten. Und Cluster-computeressourcen für die Anwendung sind oft *compute-Knoten*.

Typ | Merkmale | Beispiele
------------- | ----------- | ---------------
**Systemintern parallel**<br/><br/>![Systemintern parallel][parallel] |• Einzelne Computer unabhängig Anwendungslogik ausführen<br/><br/> • Hinzufügen von Computern kann die Anwendung und Zeitaufwand für die Berechnung<br/><br/>• Anwendung umfasst separate Programmdateien oder gliedert sich in eine Gruppe von Diensten, die von einem Client (Service-orientierte Architektur oder SOA Anwendung) aufgerufen |• Finanzielle risikomodellierung<br/><br/>• Bild rendern und Image-processing<br/><br/>• Media Codierung und Umcodierung<br/><br/>• Monte-Carlo-Simulationen<br/><br/>• Testen von software
**Eng**<br/><br/>![Eng][coupled] |• Anwendung erfordert Computeknoten interagieren oder Zwischenergebnisse<br/><br/>• Compute-Knoten möglicherweise kommunizieren die Message Passing Interface (MPI), allgemeine Kommunikationsprotokoll für parallele Datenverarbeitung<br/><br/>• Die Anwendung reagiert auf Netzwerk-Latenz und Bandbreite<br/><br/>Application Performance können verbessert werden mit High-Speed-Technologien wie InfiniBand und remote direct Memory Access (RDMA) |• Öl- und Wasserspeichermodelle<br/><br/>• Technisches Design und Analyse wie computational Fluid dynamics<br/><br/>• Physische Simulationen Auto Abstürze und nuklearen Reaktionen<br/><br/>• Wettervorhersage

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>Beim Ausführen der Stapelverarbeitung und HPC-Anwendung in der cloud

Sie können leicht vielen migrieren, die im lokalen HPC-Cluster Azure oder eine hybride (standortübergreifende) Umgebung ausgeführt werden. Es kann jedoch einige Nachteile oder Aspekte wie:


* **Verfügbarkeit von Cloudressourcen** - je nach Cloud Computing Ressourcen, nicht möglicherweise kontinuierliche Verfügbarkeit auf, während ein Auftrag ausgeführt wird. Behandlung von Status und Fortschritt überprüfen zeigen Techniken in der Cloud Ressourcen möglichen Ausfällen und mehr behandelt.


* **Datenzugriff** - Datenzugriffsverfahren häufig für Enterprise-Clustern wie NFS, erfordern spezielle Konfiguration in der Cloud. Oder Sie müssen verschiedene Methoden und Muster für die Cloud.

* **Datentransfer** - Anwendung, die große Datenmengen Strategien verarbeiten werden die Daten in der Cloud-Speicher und Ressourcen berechnet. Schnelle standortübergreifende Vernetzung wie [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)benötigen. Berücksichtigen Sie rechtliche Auflagen, oder Richtlinie Grenzen zum Speichern oder auf diese Daten zugreifen.


* **Lizenzierung** - Kontrollkästchen Hersteller kommerzielle Anwendung Lizenzierung oder andere Einschränkungen für die Ausführung in der Cloud. Nicht alle Hersteller bieten Pay-Lizenzierung. Sie müssen einen Lizenzserver in der Cloud für Ihre Lösung planen oder zu einem lokalen Lizenzserver herstellen.


### <a name="big-compute-or-big-data"></a>Große COMPUTE- oder Big Data?

Die Trennlinie zwischen Groß berechnen und Big Data ist nicht immer klar und einige Programme möglicherweise Merkmale beider. Beide beinhalten umfangreiche Berechnungen normalerweise auf Gruppen von Computern ausgeführt. Lösungsansätze und unterstützende Tools können jedoch abweichen.

• **Big Compute** tendenziell Applikationen beinhalten, die CPU-Leistung und Speicher engineering Simulationen finanzielles Risiko modellieren und digitale Wiedergabe auf. Die Infrastruktur für eine große berechnen Lösung gehören Computer mit speziellen Mehrkernprozessoren raw Berechnung und spezialisierte, schnelle Netzwerkhardware zum Verbinden der Computer.

• **Big Data** löst Analysis-Probleme, die große Mengen von Daten umfassen, die von einem einzelnen Computer oder die Datenbank-Verwaltungssystem verwaltet werden kann. Beispiele sind große Mengen Webprotokollen oder andere Business Intelligence-Daten. Big Data ist sich mehr Speicherkapazität und e/a-Leistung als CPU-Leistung. Es gibt auch spezielle Big Data Tools wie Apache Hadoop Cluster und Partition Daten verwalten. (Informationen zu Azure HDInsight und andere Azure Hadoop finden Sie [Hadoop](https://azure.microsoft.com/solutions/hadoop/).)

## <a name="compute-management-and-job-scheduling"></a>Management und Planung von Aufträgen zu berechnen

Stapel und HPC-Anwendung häufig ausführen enthält *Clustermanager* und einem *Objektaufrufplaner* gruppierten Serverressourcen verwalten und zuweisen, die die Einzelvorgänge ausgeführt Applications. Diese Funktionen können durch separate Tools oder integriertes Tool oder Service erfolgen.

* **Cluster-Manager** - Vorschriften frei und verwaltet Datenverarbeitungsressourcen (oder compute-Knoten). Clustermanager möglicherweise automatisieren die Installation des Betriebssystem-Images und auf Compute-Knoten Datenverarbeitungsressourcen nach Bedarf skalieren und Überwachen der Leistung der Knoten.

* **Objektaufrufplaner** - Ressourcen (z. B. Prozessoren oder Speicher) gibt eine Anwendung benötigt, und die Ausführung. Ein Job Scheduler verwaltet eine Reihe von Aufträgen und Ressourcen entsprechend eine Priorität zugewiesen oder auf anderen Merkmalen.

Clustering und Job scheduling-Tools für Windows- und Linux-Cluster können auch in Azure migrieren. Beispielsweise bietet [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029)Microsoft kostenlose Compute Cluster-Lösung für Windows und Linux HPC-Arbeitslasten mehrere Optionen für die Ausführung in Azure. Sie bauen auch Linux-Clustern, um Open Source-Tools wie Drehmoment und SLURM auszuführen. Kommerzielle Raster Solutions bringen Sie in Azure [TIBCO DataSynapse GridServer](http://www.tibco.com/company/news/releases/2016/tibco-to-accelerate-cloud-adoption-of-banking-and-capital-markets-customers-via-microsoft-collaboration), [IBM Platform Symphony](http://www-01.ibm.com/support/docview.wss?uid=isg3T1023592)und [Univa Grid Engine](http://www.univa.com/products/grid-engine).

Wie in den folgenden Abschnitten dargestellt, können Sie auch nutzen von Azure Compute-Ressourcen und herkömmlichen Clusterverwaltungstools Aufträge ohne (oder zusätzlich) planen.


## <a name="scenarios"></a>Szenarien

Hier sind drei Szenarien Big Compute-Workloads in Azure mithilfe von vorhandenen HPC-Cluster-Lösungen, Azure Services oder eine Kombination der beiden auszuführen. Wichtige Faktoren für die Auswahl jedes Szenario aufgeführt sind, aber nicht vollständig. Ist über die verfügbaren Azure Dienste in der Projektmappe verwenden können später im Artikel.

  | Szenario | Warum Sie es?
------------- | ----------- | ---------------
**Ein HPC-Cluster in Azure Burst**<br/><br/>[! [Cluster Burst] [Burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Weitere Informationen:<br/>• [Azure Worker-Instanzen mit HPC Pack burst](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Richten Sie eine hybride compute Cluster mit HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>• [Azure Batch mit HPC Pack burst](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/>|• Kombinieren Ihre [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) oder anderen lokalen Cluster mit zusätzlichen Azure-Ressourcen in einer Hybrid-Lösung.<br/><br/>• Erweiterung der Big Compute-Workloads auf Plattform als Service (PaaS) Instanzen virtueller Computer (derzeit nur Windows Server) ausgeführt.<br/><br/>• Zugriff auf einen lokalen Server- oder Lizenzspeicher über eine optionale Azure virtuelles Netzwerk|• Sie einen vorhandenen HPC-Cluster und weitere Ressourcen <br/><br/>• Möchten erwerben und zusätzliche HPC-Cluster verwalten<br/><br/>• Sie transiente Spitzenzeiten Perioden oder spezielle Projekte
**HPC-Cluster vollständig in Azure erstellen**<br/><br/>[! [Cluster IaaS] [Iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Weitere Informationen:<br/>• [HPC-Cluster-Lösungen in Azure](./big-compute-resources.md)<br/><br/>|• Schnell und dauerhaft einsetzen Ihre Applikationen und clustertools auf standardmäßigen oder benutzerdefinierten Windows- oder Linux-Infrastruktur als ein Service (IaaS) virtuelle Computer.<br/><br/>• Verschiedene Arbeitslasten mit großen berechnen mithilfe den Job scheduling-Lösung ausführen.<br/><br/>• Dienste zusätzliche Azure einschließlich Netzwerk- und umfassende Cloud-basierte Lösung erstellt. |• Möchten erwerben und zusätzliche Linux oder Windows HPC-Cluster-Infrastruktur verwalten<br/><br/>• Sie transiente Spitzenzeiten Perioden oder spezielle Projekte<br/><br/>Sie benötigen einen zusätzlichen • cluster Zeit aber zu Computern und Speicherplatz bereitstellen möchten<br/><br/>• Die rechenintensive Anwendung abnimmt, damit als Dienst in der Cloud ausgeführt werden soll
**Eine parallele Anwendung in Azure skalieren**<br/><br/>[! [Azure Batch] [Batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Weitere Informationen:<br/>• [Grundkenntnisse Azure Batch](./batch-technical-overview.md)<br/><br/>[Erste Schritte mit der Bibliothek Azure Batch für .NET](./batch-dotnet-get-started.md) •|• Entwickeln mit [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) skalieren verschiedene Big Compute-Workloads auf Pools von Windows oder Linux virtuellen Maschinen ausführen.<br/><br/>• Mit der Bereitstellung und Skalierung von virtuellen Maschinen, Auszubildende, Disaster Recovery, Datentransfer, Abhängigkeit Management und Bereitstellung verwaltet Azure Platform-Dienst.|• Möchten nicht verwalten berechnen Ressourcen oder ein Objektaufrufplaner; Stattdessen möchten Sie sich zum Ausführen der Anwendung<br/><br/>• Die rechenintensive Anwendung abnimmt, damit als Dienst in der Cloud ausgeführt werden soll<br/><br/>• Die Serverressourcen entsprechend Compute-Arbeitslast automatisch skalieren möchten


## <a name="azure-services-for-big-compute"></a>Azure Services für große berechnen

Hier ist mehr berechnen, Daten, Netzwerke und Dienstleistungen kombinieren kann für große berechnen Solutions und Workflows. Detaillierte Azure Services Siehe Azure Services- [Dokumentation](https://azure.microsoft.com/documentation/). Die [Szenarien](#scenarios) in diesem Artikel anzeigen genau wie diese Dienste

>[AZURE.NOTE] Azure führt regelmäßig neue Dienste, die für Ihr Szenario hilfreich sein können. Wenn Sie Fragen haben, wenden Sie sich an eine [Azure Partner](https://pinpoint.microsoft.com/en-US/search?keyword=azure) oder e-Mail *bigcompute@microsoft.com*.

### <a name="compute-services"></a>Berechnen von services

Azure Compute-Services sind das Herzstück einer Lösung Big Compute und anderen Compute-Dienste bieten Vorteile für verschiedene Szenarien. Auf der Basisebene bieten diese verschiedene Modi für die Ausführung auf virtuellen Maschinen Serverinstanzen, die Azure bietet Windows Server Hyper-V-Technologie verwenden. Standardmäßige und benutzerdefinierte Linux- und Windows-Betriebssystemen und Tools können diese Instanzen ausführen. Azure bietet eine Auswahl an [Instanzen](../virtual-machines/virtual-machines-windows-sizes.md) mit unterschiedlichen Konfigurationen des CPU-Kerne, Speicher, Speicherkapazität und andere Merkmale. Bei Bedarf können Sie Instanzen für Tausende von Kernen skaliert und dann verkleinern, wenn Sie weniger Ressourcen benötigen.

>[AZURE.NOTE] Nutzen Sie Azure rechenintensive Instanzen zur Verbesserung der Leistung und Erweiterbarkeit HPC-Arbeitslasten einschließlich parallel MPI-Anwendung, die eine niedrige Latenz und hohen Durchsatz Anwendung Netzwerk erfordern. [H-Serie und rechenintensiver eine Reihe VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)finden Sie unter.  

Dienst | Beschreibung
------------- | -----------
**[Virtuelle Computer](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Bieten berechnen Infrastruktur als Service (IaaS) mit Microsoft Hyper-V-Technologie<br/><br/>• Können Sie flexibel bereitstellen und permanente Cloud Computer aus Windows Server oder Linux Standardbilder [Azure Marketplace](https://azure.microsoft.com/marketplace/), Bilder oder Datenträger eingegebene verwalten<br/><br/>• Bereitgestellt und als [VM Maßstab legt](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) großen Builddienste identischen virtuellen Maschinen mit Skalierung vergrößern oder verkleinern Kapazität automatisch verwaltet werden können<br/><br/>• Führen Sie lokale compute clustertools und Applikationen in der cloud<br/><br/>
**[Cloud-Dienste](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Können große berechnen Programme in der Rolleninstanzen, ausführen, virtuelle Computer eine Version von Windows Server von Azure verwaltet<br/><br/>• Skalierbare, zuverlässige Applikationen mit geringen Verwaltungsaufwand auf einer Plattform als (PaaS) Servicemodell aktivieren<br/><br/>• Erfordern zusätzliche Tools oder Entwicklung lokal HPC-Cluster-Lösungen integrieren
**[Stapelverarbeitung](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Umfangreiche Parallel und Batch-Arbeitslasten in eine vollständig verwaltete Dienst ausgeführt<br/><br/>• Bietet Auszubildende und Skalierung des verwalteten Pool virtueller Computer<br/><br/>• Ermöglicht Entwicklern das Erstellen Applikationen als Dienst ausgeführt und vorhandene Applikationen Cloud aktivieren<br/>

### <a name="storage-services"></a>Speicher-services

Big Compute-Lösung normalerweise auf Daten arbeitet und Daten für die Ergebnisse generiert. Azure-Speicherdienste Big Compute-Lösungen verwendet gehören:

* [Blob, Tabelle und Warteschlangenspeicher](https://azure.microsoft.com/documentation/services/storage/) - bzw. verwalten große Mengen an unstrukturierten Daten, NoSQL Daten und Nachrichten für Workflows und Kommunikation. Beispielsweise können BLOB-Speicher für große technische Daten und Bilder oder Mediendateien der Anwendungsprozesse. Sie können Warteschlangen für die asynchrone Kommunikation in einer Lösung. Finden Sie unter [Einführung in Microsoft Azure-Speicher](../storage/storage-introduction.md).

* [Azure-Dateispeicher](https://azure.microsoft.com/services/storage/files/) - Freigaben gemeinsame Dateien und Daten in Azure über standardmäßige SMB-Protokoll für einige HPC-Cluster-Lösungen.

* [Datenspeicher See](https://azure.microsoft.com/services/data-lake-store/) - bietet eine Hyperscale Apache Hadoop DFS für die Cloud für Stapel in Echtzeit und interaktive Analyse.

### <a name="data-and-analysis-services"></a>Daten und Analysis services

Einige große berechnen Szenarien beinhalten umfangreiche Daten oder Daten, die weitere Verarbeitung oder Analyse generieren. Azure bietet mehrere Daten und Analysis Services, einschließlich:

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) - Builds datengesteuerten Workflows Rohrleitungen (), Join, Aggregate und Transformieren von Daten aus lokalen, Cloud-basierte und Internet-Datenspeicher.

* [SQL Datenbank](https://azure.microsoft.com/documentation/services/sql-database/) - enthält die wichtigsten Features von einer Microsoft SQL Server Datenbank-Managementsystem in managed Service.

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) - bereitstellt und Windows Server oder Linux-basierten Apache Hadoop Cluster in der Cloud zu verwalten, analysieren und große Daten melden.

* [Computerlernen](https://azure.microsoft.com/documentation/services/machine-learning/) – können Sie erstellen, testen, betreiben und vorbeugende analytische Lösungen vollständig verwaltete Dienst verwalten.

### <a name="additional-services"></a>Zusätzliche Dienste

Projektmappe Big Compute möglicherweise andere Azure Services Ressourcen lokal oder in einer anderen Umgebung. Beispiele:

* [Virtuelles Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) - erstellt einen logisch isolierten Abschnitt in Azure Azure Ressourcen oder Ihr Rechenzentrum vor Ort herstellen. Standortübergreifende virtual Network möglich Big Compute Applikationen für lokale Daten, Active Directory-Dienste und Lizenzserver

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) - erstellt eine private Verbindung zwischen Microsoft-Rechenzentren und Infrastruktur lokal oder in einer Umgebung mit gemeinsam. ExpressRoute bietet höhere Sicherheit, mehr Zuverlässigkeit, beschleunigt und niedriger Latenz als normalen Verbindungen über das Internet.

* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) - stellt mehrere Mechanismen für Applikationen oder Austauschen von Daten, ob sie in Azure auf einer anderen Cloudplattform oder in einem Rechenzentrum befinden.

## <a name="next-steps"></a>Nächste Schritte

* Finden Sie [Technische Ressourcen für Stapel und HPC](big-compute-resources.md) technische Anleitung zum Erstellen einer Lösung gefunden.

* Besprechen Sie Azure Optionen mit Partnern Zyklus Computing und UberCloud.

* Informationen Sie zu großen Azure Compute Lösungen von [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/) [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)und [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).

* Aktuellen Ankündigungen finden Sie im [Microsoft HPC und Batch-Teamblog](http://blogs.technet.com/b/windowshpc/) und [Azure-Blog](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png

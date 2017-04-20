<properties
    pageTitle="Azure Batch Service Grundlagen | Microsoft Azure"
    description="Weitere Informationen Sie zur Verwendung von Azure Batch Service für umfangreiche Parallel und HPC-Arbeitslasten"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Grundlagen von Azure Batch

Azure Batch können Sie umfangreiche parallel und High-Performance computing (HPC) Applikationen effizient in der Cloud ausgeführt. Ist ein Plattformdienst, der rechenintensiver Arbeit auf einer verwalteten Auflistung von virtuellen Maschinen plant und kann automatisch skalieren Datenverarbeitungsressourcen auf die Bedürfnisse von Aufträgen.

Mit der Stapelverarbeitung Service definieren Sie Azure computeressourcen zum Ausführen Ihrer Anwendung parallel und Ebene. Führen Sie bei Bedarf oder geplante Aufträge und müssen manuell erstellen, konfigurieren und verwalten eine HPC-Cluster, einzelne virtuelle Maschinen, virtuelle Netzwerke oder eine komplexe Aufgabe und task planen Infrastruktur.

## <a name="use-cases-for-batch"></a>Anwendungsfälle für Stapel

Batch ist eine verwaltete Azure Service, der *Stapelverarbeitung* oder *Batch computing*– mit großer ähnliche Aufgaben auf einige gewünschte Ergebnis verwendet wird. Batch computing wird am häufigsten von Organisationen verwendet, die regelmäßig verarbeiten, Transformieren und große Datenmengen analysieren.

Batch funktioniert gut mit Arbeitslasten und systemintern parallele Programme (auch bekannt als "rezeptbuchähnliche"). Systemintern parallele Arbeitslasten werden mehrere Aufgaben leicht aufgeteilt, die Arbeit auf mehreren Computern gleichzeitig ausführen.

![Parallele Aufgaben][1]<br/>

Arbeitslasten, die häufig verarbeitet werden mit diesem Verfahren zählen:

* Finanzielle risikomodellierung
* Analyse Klima und hydrologie
* Bild rendern, Analyse und Verarbeitung
* Medien codieren und Umcodierung
* Genetische Sequenzanalyse
* Ermüdungsanalyse Engineering
* Testen von Software

Stapel kann parallele Berechnungen mit am Ende reduzieren und komplexere HPC-Arbeitslasten wie [Message Passing Interface (MPI)](batch-mpi.md) ausführen.

Finden Sie einen Vergleich zwischen und anderen Lösungsmöglichkeiten HPC in Azure [Batch und HPC-Lösung](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Entwickeln mit

Parallele Arbeitslasten mit Batch Processing programmgesteuert erfolgt normalerweise mit einer [Batch-APIs](#batch-development-apis). Mit Batch-APIs Sie erstellen, verwalten Pools von Compute-Knoten (virtuelle Maschinen) und Planen von Aufträgen und Aufgaben auf diesen Knoten. Eine Client-Anwendung oder Dienst erstellen verwendet Batch-APIs zur Kommunikation mit der Batch-Dienst.

Sie können effizient für Ihre Organisation große Arbeitslasten verarbeiten oder Frontend Service für Ihre Kunden bereitstellen, damit Aufträge und Tasks auf Anforderung oder nach Zeitplan - auf eine, Hunderte oder sogar Tausende von Knoten ausgeführt werden. Sie können auch Stapel als Teil eines größeren Workflows durch Tools wie [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md)verwaltet.

> [AZURE.TIP] Wenn der Batch-API für ausführlichere Informationen zu den Features beschäftigen, bietet bereit sind, überprüfen Sie [Batch Übersicht über die Features für Entwickler](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Azure-Konten müssen Sie

Wenn Stapel Lösungen verwenden Sie die folgenden Konten in Microsoft Azure.

- **Ein Azure-Konto und Abonnement** - haben Sie bereits ein Azure-Abonnement kann Ihre [MSDN-Abonnenten nutzen][msdn_benefits], oder melden Sie sich für ein [kostenloses Azure-Konto][free_account]. Wenn Sie ein Konto erstellen, wird ein Standardabonnement für Sie erstellt.

- **Batch-Konto** - Anwendung die Interaktion mit der Batch-Dienst den Kontonamen der URL des Kontos und Zugriffstaste dienen als Anmeldeinformationen. Alle Stapel Ressourcen wie Pools compute-Knoten, Projekte und Aufgaben ein Batch-Konto zugeordnet. Sie können im Azure-Portal [Batch-Konto erstellen](batch-account-create-portal.md) .

- **Konto** - Batch enthält integrierten Unterstützung für das Arbeiten mit Dateien in [Azure Storage][azure_storage]. Fast jeder Batch Szenario verwendet Azure Storage - staging-Programme, die Ihre Aufgaben ausgeführt und die Daten, die sie verarbeiten und Ausgabe von Daten, die sie generieren. Erstellen Sie ein Speicherkonto anzeigen Sie [zum Azure-Speicherkonten](./../storage/storage-create-storage-account.md)

### <a name="batch-development-apis"></a>Stapelverarbeitung APIs

Ihrer Programme und Dienste können direkte REST API-Aufrufe ausstellen, Ressourcen und zur parallelen Arbeitslasten Ebene mit der Batch-Dienst ein oder mehrere der folgenden Clientbibliotheken oder eine Kombination aus beidem zu berechnen.

| API    | API-Referenz | Herunterladen | Code-Beispiele |
| ----------------- | ------------- | -------- | ------------ |
| **Batch REST** | [MSDN][batch_rest] | N/A | [MSDN][batch_rest] |
| **Batch .NET**    | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Batch Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Batch Node.js** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Batch Java** (Vorschau) | [github.IO][api_java] | [Maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Batch-Ressourcenmanagement

Neben Client-APIs auch können folgende Sie Ressourcen in Ihrem Konto Batch.

- [Batch-PowerShell-Cmdlets][batch_ps]: The Azure Batch Cmdlets in [Azure PowerShell](../powershell-install-configure.md) -Modul können Sie Batch-Ressourcen mit PowerShell.

- [Azure-CLI](../xplat-cli-install.md): der Azure-Befehlszeilenschnittstelle (CLI Azure) ist eine plattformübergreifende Toolset, Shell-Befehle für die Interaktion mit vielen Azure Services einschließlich Batch bereitstellt.

- [Batch Management.NET](batch-management-dotnet.md) Client Library: erhältlich über [NuGet][api_net_mgmt_nuget], können Sie die Clientbibliothek Batch Management .NET Batch Konten Kontingente und Anwendungspakete programmgesteuert verwalten. Verweisen auf [MSDN]Library Management ist[api_net_mgmt].

### <a name="batch-tools"></a>Batch-tools

Obwohl Lösungen mit Batch nicht erforderlich, sind hier einige nützliche Tools beim Erstellen und Debuggen Ihrer Batch Programme und Dienste.

 - [Azure-Portal][portal]: erstellen, überwachen und löschen Batch Pools, Projekte und Aufgaben in Azure-Portal Batch Blades. Die Statusinformationen für diese anzeigen und andere Ressourcen beim Ausführen Ihrer Aufträge und sogar Dateien von Computeknoten in Pools (download einer fehlgeschlagenen Aufgabe `stderr.txt` während der Fehlerbehebung beispielsweise). Sie können auch Remote Desktop (RDP)-Dateien, mit denen Sie sich im Knoten berechnet.

 - [Azure Batch Explorer][batch_explorer]: Batch-Explorer ermöglicht Batch Resource Management Azure-Portal, sondern in einer eigenständigen Anwendung Windows Presentation Foundation (WPF). Eines der Batch .NET Beispiel auf [GitHub][github_samples], Visual Studio 2015 oder höher erstellen und durchsuchen und Ressourcen im Batch Konto beim Entwickeln und von Batch-Projektmappen Debuggen verwenden. Auftrag anzeigen, Pool und Aufgabendetails, Dateien von Compute-Knoten und eine Remoteverbindung zu Knoten mit Remote Desktop (RDP)-Dateien können Sie Batch-Explorer.

 - [Microsoft Azure Storage Explorer][storage_explorer]: beim nicht unbedingt ein Azure Batch Tool Speicher-Explorer ist ein weiteres wertvolles Tool während entwickeln und Debuggen von Batch-Projektmappen.

## <a name="scenario-scale-out-a-parallel-workload"></a>Szenario: Eine parallele Arbeitslast skalieren

Eine allgemeine Lösung, die die Batch-APIs verwendet für die Interaktion mit der Batch-Dienst umfasst Skalierung systemintern parallel arbeiten – wie das Rendering von Bildern für 3D-Szenen – in einem Pool von Compute-Knoten. Dieser Pool von Compute-Knoten kann die renderfarm"", Dutzende, Hunderte oder sogar Tausende von Kernen Ihren Job Rendering beispielsweise bereitstellt.

Das folgende Diagramm zeigt einen allgemeinen Stapel Workflow mit einer Clientanwendung oder gehosteten Dienst mit Batch Parallele Aufgaben ausführen.

![Batch-Lösung workflow][2]

Die Anwendung oder der Dienst verarbeitet eine rechnerische Arbeitslast in Azure Batch dabei häufig durch Ausführen der folgenden Schritte:

1. Hochladen der **Dateien** und die **Anwendung** , die diese Dateien Azure Storage-Konto verarbeitet. Die Dateien enthalten keine Daten, die die Anwendung verarbeitet, wie Finanzmodelle Daten oder Videodateien transcodiert werden. Die Anwendungsdateien kann jede Anwendung, die für die Datenverarbeitung oder eine Anwendung 3D-Rendering Media Transcoder verwendet wird.

2. Erstellen eines Batch- **Pool** von Computeknoten in Ihrem Konto Batch - werden diese Knoten den virtuellen Computern, die Ihre Aufgaben ausführen. Sie geben wie [Knotengröße](./../cloud-services/cloud-services-sizes-specs.md), Betriebssystem und den Speicherort Azure-Speicher der Anwendung installieren, wenn die Knoten beitreten Pool (die Anwendung, die Sie in Schritt 1 # hochgeladen). Sie können auch den Pool [automatisch](batch-automatic-scaling.md)– dynamisch konfigurieren die Anzahl von Compute-Knoten im Pool - auf die Arbeitslast, die Ihre Aufgaben generieren.

3. Erstellen Sie einen **Auftrag** zum Ausführen der Arbeitslast auf den Compute-Knoten. Wenn Sie einen Auftrag erstellen, ordnen Sie es mit Batch.

4. Das Projekt **Vorgänge** hinzufügen. Beim Hinzufügen von Tasks zu einem Auftrag plant Batch-Dienst automatisch Aufgaben zur Ausführung auf den Compute-Knoten im Pool. Jede Aufgabe verwendet die Anwendung hochgeladen werden, um die Dateien zu verarbeiten.

    - 4a. Bevor eine Aufgabe ausgeführt wird, können sie Daten (input-Dateien), die den Prozess der Compute-Knoten zugeordnet ist. Wenn die Anwendung noch nicht auf dem Knoten installiert wurde (siehe Schritt #2) Download hier stattdessen. Wenn der Download abgeschlossen ist, führen Aufgaben auf den zugeordneten Knoten.

5. Ausführung der Tasks können Sie Stapel um Fortschritte und seine Aufgaben Abfragen. Die Clientanwendung oder ein Dienst kommuniziert mit dem Batch-Dienst über HTTPS, und da Sie Tausende von Aufgaben auf Tausende von Compute-Knoten überwachen können, müssen [effiziente Batch-Dienst](batch-efficient-list-queries.md)Abfragen.

6. Vollständige Aufgaben können sie ihre Daten in Azure-Speicher hochladen. Sie können Dateien auch direkt von Compute-Knoten abrufen.

7. Die Überwachung erkennt, dass die Vorgänge in Ihrem Projekt abgeschlossen haben, kann die Clientanwendung oder ein Dienst die Ausgabedaten für weitere Verarbeitung oder Auswertung herunterladen.

Halten Sie dabei Dies ist nur eine Möglichkeit Stapel verwenden und dieses Szenario beschreibt nur einige der verfügbaren Funktionen. Beispielsweise können [mehrere Vorgänge parallel](batch-parallel-node-tasks.md) in jeder Compute-Knoten ausgeführt und können Sie [Projektaufgaben Vorbereitung und Durchführung](batch-job-prep-release.md) Ihrer Aufträge Knoten vorbereiten und danach bereinigen.

## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie eine Übersicht der Batch-Dienst ist Zeit tiefer erfahren, wie Sie Ihre rechenintensiven parallelen Arbeitslasten zu verwenden.

- Lesen Sie [Batch Übersicht über die Features für Entwickler](batch-api-basics.md), wichtige Informationen für jeden Batch Verwendung vorbereiten. Der Artikel enthält ausführlichere Informationen zu Batch Serviceressourcen-Pools und Knoten, Projekte, Aufgaben und viele API-Funktionen, mit denen Sie beim Erstellen Ihrer Anwendung Batch.

- [Erste Schritte mit der Bibliothek Azure Batch für .NET](batch-dotnet-get-started.md) erfahren, wie mit C# und .NET Batch Library eine einfache Arbeitslast mithilfe eines gemeinsamen Batch Workflows ausgeführt. Dieser Artikel sollte eines der ersten hält beim Umgang mit der Batch-Dienst. Zudem ist eine [Python-Version](batch-python-tutorial.md) des Lernprogramms.

- [Codebeispiele für GitHub] downloaden[ github_samples] wie C# und Python mit Zeitplan und Prozess Beispiel Arbeitslasten Schnittstelle anzeigen.

- [Batch-Lernpfad] Auschecken[ learning_path] einen Überblick über die verfügbaren Ressourcen Sie zu lernen mit arbeiten.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png

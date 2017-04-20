<properties
    pageTitle="Daten Wissenschaft virtuelle Computer in Azure | Microsoft Azure"
    description="Einrichten einer Daten Wissenschaft virtuellen Maschine"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Daten Wissenschaft virtuelle Computer in Azure

Hier Anleitung, die zum Einrichten einer Azure VM und Azure-VM mit SQL IPython Notebook-Server beschreiben. Windows Virtual Machine ist mit Tools wie IPython Notebook, Azure Storage Explorer und AzCopy sowie andere Dienstprogramme für Forschungsprojekte Daten konfiguriert. Azure Storage Explorer und AzCopy, bieten beispielsweise bequem von Ihrem lokalen Computer Daten Azure-Speicher hochgeladen oder von Speicher auf Ihrem lokalen Computer herunterladen. 

Dieses enthält Menülinks zu Themen zum Einrichten der verschiedenen Science-Umgebung von [Team Daten Wissenschaft Prozess (TDSP)](data-science-process-overview.md)verwendet werden.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Verschiedene Azure virtueller Maschinen können bereitgestellt und konfiguriert als Teil einer Umgebung Wissenschaft cloudbasierten verwendet werden. Die Entscheidung über die Art des virtuellen Computers für maschinelles lernen und das Ziel für die Daten in der Cloud modelliert werden Art und Menge der Daten abhängig. 

* Fragen zu dieser Entscheidung berücksichtigen Siehe [Planen Ihrer Azure Computer Daten Wissenschaft Lernumgebung](machine-learning-data-science-plan-your-environment.md). 
* Einen Katalog von Szenarien, die auftreten können, wenn erweiterte Analyse finden Sie unter [Szenarien für die erweiterte Analyse und Technologie in Azure maschinelles lernen](machine-learning-data-science-plan-sample-scenarios.md)

Zwei Gruppen von Anweisungen stehen zur Verfügung:

* [Richten Sie einen virtuellen Azure-Computer als Server IPython Notebook für erweiterte Analyse](machine-learning-data-science-setup-virtual-machine.md) veranschaulicht Bereitstellung Azure VM IPython Notebook mit anderen Tools verwendet datenwissenschaft Fällen in dem Formular als SQL Azure-Speicher zum Speichern der Daten verwendet werden kann.

* [Richten Sie einen virtuellen Computer Azure SQL Server als Server für erweiterte Analyse IPython Notebook](machine-learning-data-science-setup-sql-server-virtual-machine.md) veranschaulicht Bereitstellung eine virtuellen Maschine Azure SQL Server IPython Notebook mit anderen Tools verwendet datenwissenschaft für Fälle mit eine SQL-Datenbank zum Speichern der Daten verwendet werden kann.

Bereitgestellt und konfiguriert, können diese virtuellen Computer für die Verwendung als IPython Notebook Server für die Untersuchung und Verarbeitung der Daten und andere Aufgaben in Verbindung mit Azure Machine Learning und Team Daten Wissenschaft Prozess (TDSP). Die nächsten Schritte im Prozess Wissenschaft in der [TDSP-Lernpfad](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zugeordnet und Verschieben von Daten in SQL Server oder HDInsight, verarbeitet und es zur Vorbereitung von Daten mit Azure Computer lernen probieren Schritte einschließen.


> [AZURE.NOTE] Azure Virtual Machines werden als **nur bezahlen, was Sie verwenden**Preis. Um sicherzustellen, dass Sie nicht beim virtuellen Computer nicht verwenden abgerechnet werden, muss es den Zustand **beendet (Deallocated)** aus dem [Azure-Verwaltungsportal](http://manage.windowsazure.com/). Anleitung bzw. zum virtuellen Computer freigeben, finden Sie unter [Herunterfahren und Freigeben virtueller Computer nicht](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 

<properties
    pageTitle="Wie identifizieren Sie Szenarien und erweiterte Verarbeitung der Daten | Microsoft Azure"
    description="Plan für die erweiterte Analyse anhand einer Reihe von Fragen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Wie identifizieren Sie Szenarien und erweiterte Verarbeitung der Daten

Welche Ressourcen sollten Sie beim Einrichten einer Umgebung für erweiterte Analysen auf ein Dataset enthalten? In diesem Artikel werden eine Reihe von Fragen, die die Vorgänge und Ressourcen relevanten Szenario erkennen. Die Reihenfolge der Schritte für Vorhersageanalysen gemäß [Was Team Daten Wissenschaft Prozess (TDSP) ist?](data-science-process-overview.md). Jeder dieser Schritte benötigen bestimmte Ressourcen für Ihr spezielles Szenario für Aufgaben. Wichtigen Fragen zu Ihrem Szenario betreffen datenlogistik, Merkmale, die Qualität der Datasets, Tools und Sprachen, die Sie Analyse möchten.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistische Fragen: von Standorten und Bewegung
Die logistischen Fragen betreffen den Speicherort der **Datenquelle**und das **Ziel Ziel** in Azure und Verschieben von Daten, einschließlich Zeitplan, Betrag und Ressourcen an. Die Daten müssen bei der Analyse mehrere Male verschoben werden. Häufig werden lokale Daten in eine Form von Speicher in Azure und dann in Machine Learning Studio verschoben.

1. **Was ist eine Datenquelle?** Handelt es sich um lokale oder in die Cloud? Zum Beispiel:
    - Die Daten sind verfügbar auf eine HTTP-Adresse.
    - Die Daten verbleiben ein Speicherort lokal/Netzwerk.
    - Die Daten sind in einer SQL Server-Datenbank.
    - Die Daten werden in einem Container Azure-Speicher gespeichert.

2. **Was ist das Ziel Azure?** Wo muss es für die Verarbeitung oder Modellierung? Zum Beispiel:
    - Azure BLOB-Speicher
    - SQL Azure-Datenbanken
    - SQL Server auf Azure VM
    - HDInsight (Hadoop auf Azure) oder Hive-Tabellen
    - Azure maschinelles lernen
    - Montage Azure virtuelle Festplatten.

3. **Wie wollen Sie die Daten verschieben?** In den folgenden Themen werden die Verfahren und Ressourcen zur Aufnahme oder Daten in einer Vielzahl von unterschiedlichen Massenspeicher- und Umgebung beschrieben.

    -  [Laden von Daten in Speicherausfällen für Analysen](machine-learning-data-science-ingest-data.md)
    -  [Trainingsdaten in Azure Machine Learning Studio aus verschiedenen Quellen importieren](machine-learning-data-science-import-data.md).

4. **Müssen die Daten regelmäßig verschoben oder während der Migration geändert werden?** Verwenden Sie Azure Data Factory (ADF), wenn Daten kontinuierlich migriert werden, insbesondere wenn ein hybridszenario zugreift, die sowohl lokale Cloudressourcen beteiligt und wenn die Daten Transaktionen oder geändert werden oder Geschäftslogik bei migrierten hinzugefügt. Weitere Informationen finden Sie unter [Verschieben von Daten von einem lokalen SQL Server SQL Azure Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md)

5. **Wie viel Daten ist in Azure verschoben werden?** Sehr große Datasets darf die Speicherkapazität eines bestimmten Umgebungen. Ein Beispiel finden Sie der Grenzwert für Machine Learning Studio im nächsten Abschnitt. In solchen Fällen kann ein Beispiel für die Daten während der Analyse verwendet werden. Details zum Beispiel unten ein Dataset in verschiedenen Azure-Umgebung finden Sie unter [Beispieldaten beim Team wissenschaftliche Daten](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Daten Merkmale Fragen: Typ, Format und Größe
Diese Fragen sind für Ihren Speicher planen und Verarbeitung Umgebung jeweils verschiedene Arten von Daten eignen und verfügen Einschränkungen.

1. **Was sind die Datentypen?** Zum Beispiel:
    - Numerische
    - Kategorieliste
    - Zeichenfolgen
    - Binäre

2. **Wie formatiert die Daten?** Zum Beispiel:
    - Kommas (CSV) oder tabstoppgetrenntem (TSV) Dateien
    - Komprimiert oder nicht komprimiert
    - Azure-blobs
    - Hadoop Hive-Tabellen
    - SQL Server-Tabellen

2. **Wie groß ist Ihre Daten?**
    - Klein: weniger als 2GB
    - Mittel: Größer als 2GB und weniger als 10GB
    - Große: Größer als 10GB

Nehmen Sie zum Beispiel die Azure Machine Learning Studio-Umgebung:

- Eine Liste der Datenformate und Typen von Azure Machine Learning Studio Siehe [Datentypen Formate und Daten unterstützt](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) .
- Informationen zu dateneinschränkungen für Azure Machine Learning Studio finden Sie unter der **wie groß die Datengruppe kann meine Module?** Teil [Importieren und Exportieren von Daten für maschinelles lernen](machine-learning-faq.md#machine-learning-studio-questions)

Informationen über die Grenzen der anderen Azure-Diensten in den Analyseprozess finden Sie unter [Azure-Abonnement und Service Grenzen, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Daten Qualitätsfragen: Untersuchung und Vorbehandlung

1. **Was wissen Sie über Ihre Daten?** Wenn Sie zu einem Verständnis der grundlegenden Eigenschaften müssen Datenanalyse. Was Muster oder trends Exponate Ausreißer hat oder wie viele Werte fehlen. Dieser Schritt ist wichtig für die Bestimmung der Vorbehandlung für die Formulierung von Hypothesen, die am besten geeigneten Features vorschlagen oder Art der Analyse und für die Ausarbeitung der Pläne für die Sammlung zusätzlicher Daten erforderlich. Beschreibende Statistik Berechnung und Darstellung von Visualisierung sind nützliche Techniken zur Überprüfung von Daten. Detaillierte Informationen zu einem Dataset in verschiedenen Azure-Umgebung finden Sie unter [Team wissenschaftliche Daten untersuchen](machine-learning-data-science-explore-data.md).

2. **Erfordert die Daten vorverarbeitet oder bereinigen?**
Vorbereitung und Bereinigen von Daten sind wichtige Aufgaben, die in der Regel durchgeführt werden müssen, bevor Dataset für maschinelles lernen effektiv verwendet werden kann. Rohdaten ist oft laut und unzuverlässig und Werte fehlen. Mithilfe dieser Daten für die Modellierung kann irreführende Ergebnisse. Eine Beschreibung finden Sie unter [Aufgaben auf Daten für erweiterte Computer lernen](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Tools und Sprachen Fragen
Es gibt viele Optionen je nachdem, welche Sprachen und Entwicklung oder Tools Sie benötigen oder die entsprechende Verwendung.

1. **Welche Sprachen bevorzugen Sie für die Analyse verwenden?**  
    - R
    - Python
    - SQL

2. **Welche Tools sollten Sie für die Datenanalyse verwenden?**
    - [Microsoft Azure Powershell](powershell-install-configure.md) - eine Skriptsprache zur Verwaltung Ihrer Azure-Ressourcen in einer Skriptsprache verwendet.
    - [Azure Machine Learning Studio](machine-learning-what-is-ml-studio/)
    - [Revolution Analytics](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python-Tools für Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter notebooks](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Modernste Analytik Szenario identifizieren
Nachdem Sie die vorangegangenen Fragen beantwortet haben, können Sie bestimmen, welches Szenario am besten Ihren Fall. Beispielszenarien werden Szenarien [für die erweiterte Analyse in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md)beschrieben.

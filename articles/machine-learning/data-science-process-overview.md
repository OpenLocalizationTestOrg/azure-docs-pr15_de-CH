<properties
    pageTitle="Was ist Team wissenschaftliche Daten?  | Microsoft Azure"
    description="Team von Daten Wissenschaft ist einer systematischen Methode zum Erstellen von intelligenter Clientanwendungen, die modernste Analytik nutzen."
    keywords="wissenschaftliche Daten, Daten Wissenschaft teams"
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


# <a name="what-is-the-team-data-science-process-tdsp"></a>Was ist das Team Daten Wissenschaft Prozess (TDSP)?

Das Team Daten Wissenschaft Prozess (TDSP) bietet einen systematischen Ansatz zum Erstellen von intelligenter Clientanwendungen, der ermöglicht Teams Datenanalysten Zusammenarbeit über den gesamten Lebenszyklus von Aktivitäten, um diesen zu aktivieren.

Die TDSP bietet derzeit Daten Wissenschaft Teams:

- **Methode**: eine Sequenz von Schritten, die Hilfestellung zum Definieren des Problems, analysieren Daten erstellen und auswerten Vorhersagemodelle Entwicklungszyklus definieren und Bereitstellen dieser Modelle in der Anwendung beschrieben.
- **Ressourcen**: Tools und Technologien wie Data Science-VM Einrichtung Umgebung Daten Wissenschaft Aktivitäten und praktische Hinweise zur Einführung neuer vereinfachen.

Hier ist Entwicklungszyklus Team wissenschaftliche Daten:

![Diagramm: Wissenschaftliche Daten für teams ](./media/data-science-process-overview/data-science-process-for-teams-diagram.png)


Der Prozess ist **iterativ**: das Verständnis des neuen und vorhandenen Weiterentwicklungen im Modell entwickelt und erfordert Überarbeitung Schritte vorher in der Sequenz abgeschlossen. Vorhandene Organisation und Planung sind der TDSP definierten Schrittfolge arbeiten **leicht angepasst** .

Die Schritte in diesem Prozess werden Routerplan [TDSP Lernpfad](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) verknüpft und beschrieben.  


## <a name="planning-and-preparation-steps"></a>Planung und Vorbereitung Schritte

## <a name="p1-business-and-technology-planning"></a>P1. Geschäfts- und Technologie planen

Starten eines Projekts Analytics Ihre Unternehmensziele und Probleme. Sie werden in **Unternehmen**angegeben. Hauptziel dieses Schritts werden entscheidende Variablen (Umsatzprognose oder die Wahrscheinlichkeit ein Auftrag z. B. betrügerische) identifizieren, die die Analyse vorherzusagen, um diesen Ansprüchen gerecht. Zusätzliche Planung ist normalerweise für ein Verständnis der **Datenquellen** an die Ziele des Projekts aus Sicht der analytischen erforderlich. Es ist nicht ungewöhnlich, beispielsweise um vorhandene Systeme müssen erfassen und protokollieren zusätzliche Arten von Daten zu dem Problem der Projektziele. Siehe [planen die Umgebung für das Team wissenschaftliche Daten](machine-learning-data-science-plan-your-environment.md) und [Szenarien für die erweiterte Analyse in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).  


## <a name="p2-plan-and-prepare-infrastructure"></a>P2. Planen und Vorbereiten der Infrastruktur

Umfeld Analytics Daten Wissenschaft Team umfasst mehrere Komponenten:

- **Daten Arbeitsbereiche** , in denen die Daten für die Analyse und Modellierung bereitgestellt wird
- eine **Verarbeitung Infrastruktur** vorverarbeitet erforschen und Modellierung der Daten
- eine **Laufzeitinfrastruktur** analytische Modelle durchsetzen und intelligente Clientanwendungen ausführen, verbrauchen die Modelle.  

Infrastruktur der Analytik, die eingerichtet werden, ist häufig Teil einer Umgebung von Core-Systeme. Aber in der Regel nutzt Daten aus mehreren Systemen im Unternehmen und außerhalb des Unternehmens. Die Infrastruktur der Analytik kann rein cloudbasierte oder einer lokalen Installation oder eine Kombination aus beiden. Optionen finden Sie unter [Daten Wissenschaft Umgebung für die Verwendung in Team wissenschaftliche Daten einrichten](machine-learning-data-science-environment-setup.md).


## <a name="analytics-steps"></a>Analytics Schritte:  

## <a name="1-ingest-the-data-into-the-data-platform"></a>1 nehmen Sie 1 die Daten in der Plattform

Der erste Schritt ist die relevanten Daten aus verschiedenen Quellen, entweder innerhalb oder von außerhalb des Unternehmens in einer Umgebung Analytics bringen, wo die Daten verarbeitet werden können. Das **Format** der Daten an der Quelle abweichen vom Ziel erforderliche Format. So müssen einige Datentransformation auch durch Aufnahme-Tools erfolgen. Optionen finden Sie unter [Laden von Daten in Speicherausfällen für Analysen](machine-learning-data-science-ingest-data.md)

Neben der ersten Aufnahme Daten müssen viele intelligente Clientanwendungen die Daten als Teil einer fortlaufenden Lernprozess regelmäßig aktualisieren. Dies erfolgt durch eine **Datenpipeline** oder Workflow einrichten. Bestandteil Teil der iterative Prozess, die neu und intelligente Anwendung bereitstellen der Lösung Analysemodelle neu bewerten. Finden Sie beispielsweise [Daten von einem lokalen SQL Server SQL Azure mit Azure Daten verschieben](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-visualize-the-data"></a>2. durchsuchen und Visualisieren von Daten

Im nächste Schritt wird einem besseres Verständnis der Daten untersucht die **Zusammenfassende Statistiken**Beziehungen und Techniken diese **Visualisierung**erhalten. Diese werden auch Themen **Qualität** und Integrität wie fehlende Werte, Datentypenkonflikten und inkonsistente datenbeziehungen behandelt werden. Vorverarbeitet Transformationen werden verwendet, um Rohdaten vor weiteren Analysen bereinigen und Modellierung erfolgen. Eine Beschreibung finden Sie unter [Aufgaben auf Daten für erweiterte Computer lernen](machine-learning-data-science-prepare-data.md).


## <a name="3-generate-and-select-features"></a>3. erstellen und Funktionen auswählen

Datenwissenschaftler in Zusammenarbeit mit, müssen die Funktionen identifizieren, erfassen die wichtigsten Eigenschaften der Daten und die besten entscheidende Variablen während Planung Vorhersagen verwendet werden. Diese neuen Funktionen vorhandener Daten abgeleitet werden können oder müssen zusätzliche Daten gesammelt werden. Dieser Prozess als **Feature Engineering** bezeichnet und ist einer der wichtigsten Schritte beim Erstellen einer effektiven Vorhersageanalytik System. Dieser Schritt erfordert eine kreative Kombination von Fachwissen und der Schritt bei der Untersuchung gewonnenen Erkenntnisse. Siehe [Engineering Team von wissenschaftlichen Daten verfügen](machine-learning-data-science-create-features.md).


## <a name="4-create-and-train-machine-learning-models"></a>4. erstellen Sie und Trainieren Sie maschinelles lernen Modelle

Datenwissenschaftler erstellen analytische Modelle Vorhersagen Schlüsselvariablen Business Anforderungen in die Planung mit Daten bereinigen und Featurized identifiziert. Learning Systeme unterstützen mehrere **Algorithmen modellieren** , die für eine Vielzahl von Fällen. Anleitung finden Sie unter [Algorithmen für Azure Machine Learning auswählen](machine-learning-algorithm-choice.md).

Datenanalysten müssen das geeignetste Modell für ihre Vorhersagetasks auswählen und es ist nicht ungewöhnlich, dass Ergebnisse aus mehreren Modellen werden, um optimale Ergebnisse zu erhalten kombiniert müssen. Die Eingabedaten für die Modellierung ist normalerweise zufällig in drei Teile unterteilt:

- eine Trainingsmenge Daten
- ein Dataset Validierung
- ein testdataset

Die Modelle werden mit **Trainingsdataset**erstellt. Die optimale Kombination von Modellen (mit Parametern optimiert) ist Modelle und messen Vorhersagefehler **Validierung Datensatz**ausgewählt. Schließlich **Testen Sie Daten** dient die Leistung des ausgewählten Modells Daten auswerten, die nicht oder Validieren des Modells verwendet wurde.  Prozeduren finden Sie unter [modellleistung in Azure Machine Learning zu bewerten](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-the-models-in-the-product"></a>5. bereitstellen Sie und nutzen Sie die Modelle im Produkt

Haben wir eine Reihe von Modellen, die auch ausführen, können sie **operationalisiert** für andere Programme verwendet werden. Je nach Unternehmen erfolgen Vorhersagen in **Echtzeit** oder **auf** . Um operationalisiert werden, die Modelle **Öffnen API-Schnittstelle** verfügbar gemacht werden, die problemlos aus verschiedenen Programmen genutzt haben diese Website, Kalkulationstabellen, Dashboards oder Zeile Business und Back-End. Finden Sie unter [Bereitstellen einer Azure Machine Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).


## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

[Team wissenschaftliche Daten](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) als Abfolge von Schritten durchlaufene modelliert, **Unterstützung** für Vorgänge mit erweiterten Analytics eine intelligente Anwendung erstellen erforderlich. Jeder Schritt enthält auch Details mit verschiedenen Microsoft Technologies beschriebenen Aufgaben.

Während TDSP bestimmte **Dokumentation** Artefakte nicht vorschreiben, empfiehlt sie dokumentieren die Ergebnisse Durchsuchen von Daten, Modellierung und Bewertung und relevanten Code speichern, damit die Analyse durchlaufen werden kann, wenn erforderlich. Dadurch auch Wiederverwendung Analytics Arbeit bei der Arbeit an andere Programme mit ähnlichen Daten und Vorhersagetasks.

Vollständige End-to-End alle Schritte im Prozess für **bestimmte Szenarien** exemplarischen Vorgehensweisen für werden ebenfalls bereitgestellt. Sie werden mit Miniaturansichten im [Team Daten Wissenschaft Prozessdurchläufe](data-science-process-walkthroughs.md) Thema aufgeführt.

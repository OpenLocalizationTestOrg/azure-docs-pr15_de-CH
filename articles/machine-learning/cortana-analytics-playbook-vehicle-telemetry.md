<properties 
    pageTitle="Fahrzeug Telemetrie Analytics Lösung Playbook | Microsoft Azure" 
    description="Verwenden Sie die Cortana Intelligenz in Echtzeit und prädiktive Einblicke Fahrzeug Gesundheits-und fahren Gewohnheiten." 
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
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Fahrzeug Telemetrie Analytics Lösung playbook

Dieses **Menü** enthält Links zu den Kapiteln dieses Playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Übersicht
Super-Computer aus dem Labor verschoben haben und jetzt in der Garage geparkt! Diese modernste Automobile enthalten eine Vielzahl von Sensoren, Ihnen die Fähigkeit zu überwachen Millionen Ereignisse pro Sekunde. Wir erwarten 2020 die meisten dieser Autos mit dem Internet verbunden sein werden. Stellen Sie sich zu dieser Fülle von Daten am besten in Klasse Sicherheit, Zuverlässigkeit und fahren! Microsoft hat diesen Traum mit Cortana.

Microsoft Cortana Intelligence ist eine vollständig verwaltete Datenmenge und erweiterte Analyse-Suite, die intelligente Aktion Daten verwandeln kann. Wir möchten Cortana Intelligence Fahrzeug Telemetrie Analytics Lösungsvorlage vorgestellt. Dies veranschaulicht das Autohändler, Hersteller und Unternehmen können die Funktionen Cortana Intelligenz in Echtzeit zu und Gewohnheiten vorhersehbaren Einblicke Fahrzeug Gesundheits-und fahren. 

Die Lösung wird als [Lambda-Architekturmuster](https://en.wikipedia.org/wiki/Lambda_architecture) mit vollständigen Cortana Intelligence Plattform für potenzielle implementiert in Echtzeit und Batch-Verarbeitung. Die Lösung: 

- Stellt einen Fahrzeug Telematik-simulator
- nutzt Ereignis Hubs für Millionen von simulierten Fahrzeugs Telemetrie-Ereignisse in Azure Einnahme 
- Stream Analytics verwendet Fahrzeug Gesundheit in Echtzeit Einblicke
-  speichert Daten in Langzeitspeicher für umfangreichere Batch Analysen. 
- nutzt der Computerlernen zur anomalieerkennung in Echtzeit und batch-Verarbeitung vorhersehbaren Einblicke.
- nutzt die HDInsight zum Transformieren von Daten und Daten Factory Orchestrierung planen, Ressourcenmanagement und Überwachung der Stapelverarbeitung Pipeline behandeln 
- bietet diese Lösung rich Dashboard für Echtzeitdaten und Vorhersageanalysen Visualisierung mit Power BI

## <a name="architecture"></a>Architektur

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Abbildung 1 – Fahrzeug Telemetrie Analytics Lösungsarchitektur*

Diese Lösung umfasst die folgenden **Komponenten Cortana Intelligence** und zeigt deren Ende-integration


- **Event Hubs** für Millionen von Fahrzeug Telemetrie-Ereignisse in Azure Einnahme.
- **Stream Analytics** für Echtzeit-Einblick Fahrzeug Gesundheit und speichert diese Daten in Langzeitspeicher für umfangreichere Batch Analysen.
- Erkennen von Netzwerkanomalien in Echtzeit **Maschinelles lernen** und Stapelverarbeitung vorhersehbaren Einblicke.
- **HDInsight** zum Transformieren von Daten genutzt
- **Data Factory** behandelt Orchestrierung, Planung, Ressourcen-Management und Überwachung der Stapelverarbeitung Pipeline.
- **Power BI** bietet diese Lösung rich Dashboard für Echtzeitdaten und Vorhersageanalysen Visualisierung.

Diese Lösung greift auf zwei verschiedenen **Datenquellen**: 

- **Simulierte fahrzeugsignale und Diagnose**: ein Fahrzeug Telematik Simulator Diagnoseinformationen und Status und fahrmuster zu einem bestimmten Zeitpunkt entsprechen Signale rechtzeitig ausgegeben. 
- **Katalog Fahrzeuge**: einem Verweisdataset FIN, Zuordnung enthält.

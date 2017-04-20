<properties 
    pageTitle="Anwendungsfall - Kunden Profiling" 
    description="Erfahren Sie, wie Azure Data Factory zum Erstellen eines datengesteuerten Workflows (Pipeline) so Gaming-Kunden verwendet wird." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>Anwendungsfall - Kunden Profiling

Azure Data Factory ist einer der vielen Dienste Cortana Intelligence Suite von Solution Accelerators implementiert.  Weitere Informationen zu Cortana Intelligence finden Sie [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). Dieses Dokument beschreibt einen einfache Anwendungsfall helfen Ihnen beim Einstieg zu verstehen, wie Azure Data Factory Analytics Probleme lösen können.

Alles zugreifen und diese einfachen Anwendungsfall ist ein [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/).  Sie können ein Beispiel bereitstellen, die diesen Anwendungsfall implementiert in Artikel [Beispiele](data-factory-samples.md) beschriebenen Schritte befolgen.

## <a name="scenario"></a>Szenario

Contoso ist ein Gaming-Unternehmen, die Spiele für mehrere Plattformen erstellt: game-Konsolen, tragbare Geräte und Personal Computer (PCs). Als Spieler spielen, entsteht große Menge von Daten, die das Verwendungsmuster Spielstil und Voreinstellungen des Benutzers nachverfolgt.  Mit demographischen, regionale und Produktdaten Contoso kann Analytics führen sie Informationen zu der Spieler zu verbessern und Ziel für Upgrades und im Spiel kauft. 

Contoso Ziel ist verkaufen/Cross-Chancen Gaming-Geschichte der Spieler und Wachstum überzeugende Features hinzufügen und optimal für Kunden. Für diesen Fall verwenden wir als Beispiel ein Unternehmen Unternehmen spielen. Das Unternehmen möchte seine Spiele Spieler Verhalten zu optimieren. Diese Prinzipien gelten für Unternehmen, die ihre Kunden waren und Services und Kunden verbessern möchte.

## <a name="challenges"></a>Herausforderung


## <a name="solution-overview"></a>Lösungsübersicht

Dieser einfache Anwendungsfall kann so Verwendung Azure Data Factory Aufnahme vorbereiten, transformieren, analysieren und Veröffentlichen von Daten verwendet werden.

![End-to-End-workflow](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Diese Abbildung zeigt, wie die Datenpipelines im Azure-Portal angezeigt, nachdem sie bereitgestellt wurden.

1.  **PartitionGameLogsPipeline** liest die rohen Spiel Ereignisse aus dem BLOB-Speicher und Partitionen basierend auf Jahr, Monat und Tag erstellt.
2.  **EnrichGameLogsPipeline** partitionierten Spiel Ereignisse mit geometrische Referenz verknüpft und bereichert die Daten zu den entsprechenden Geo-IP-Adressen zuordnen.
3.  **AnalyzeMarketingCampaignPipeline** -Pipeline verwendet erweiterte Daten und Daten Werbung, die endgültige Ausgabe erstellen marketing Kampagneneffektivität verarbeitet.

In diesem Beispiel dient Data Factory Aktivitäten zu koordinieren, die Daten transformieren und die Daten kopieren und endgültige Daten mit einer Azure SQL-Datenbank.  Sie können auch das Netzwerk Datenpipelines visualisieren verwalten und Überwachen von deren Status in der Benutzeroberfläche.

## <a name="benefits"></a>Vorteile

Optimieren ihre Benutzer Profil Analysen und Unternehmensziele ausrichten, wird Gesellschaft schnell Verwendungsmuster sammeln und analysieren die Effektivität der Marketingkampagnen.





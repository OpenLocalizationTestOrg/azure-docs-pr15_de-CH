
<properties
   pageTitle="Elasticsearch Azure Anleitung | Microsoft Azure"
   description="Elasticsearch Azure Anleitung."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch Azure Anleitung 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch ist eine hochgradig skalierbare Open-Source-Suchmaschine und Datenbank. Es eignet sich für Situationen, in denen schnelle Analyse und Erkennung von Informationen in großen Datasets. Dieser Leitfaden untersucht einige wichtige Aspekte zu berücksichtigen beim Entwerfen eines Clusters Elasticsearch mit Schwerpunkt zum Optimieren und Testen der Konfiguration.

> [AZURE.NOTE]Diese Anleitung setzt voraus, einige grundlegende Kenntnisse von Elasticsearch. Ausführlichere Informationen finden Sie auf der [Website Elasticsearch](https://www.elastic.co/products/elasticsearch). 

- **[Ausführen Elasticsearch Azure][]** bietet eine kurze Einführung in die allgemeine Struktur des Elasticsearch und einen Elasticsearch Cluster mit Azure implementieren beschrieben. 
- **[Tuning Daten Einnahme Leistung für Elasticsearch auf Azure][]** beschreibt die Bereitstellung eines Clusters Elasticsearch und eingehende Analysen der verschiedenen Konfigurationsoptionen erwarten eine hohe Einnahme Daten berücksichtigen.
- **[Datenaggregation Tuning und abfrageleistung für Elasticsearch auf Azure][]** bietet eine gründliche Analyse der Optionen zu berücksichtigen wie Ihr System Abfragen und Leistung zu optimieren.
- **[Stabilität und Recovery Elasticsearch in Azure konfigurieren][]** beschreibt einige wichtige Features von einem Elasticsearch-Cluster, der Minimierung von Datenverlusten und erweiterte Daten Recovery-Zeiten.
- **[Performance testing Rahmenbedingungen für Elasticsearch auf Azure][]** beschreibt das Einrichten einer Umgebung für die Performance-Daten-Erfassung und Abfrage Arbeitslasten in einem Cluster Elasticsearch. 
- **[Implementieren einer JMeter Testplan für Elasticsearch][]** fasst ausgeführten Leistungstests mit Testplänen mit Java-Code als JUnit-Test für Aufgaben wie das Hochladen von Daten in Elasticsearch Cluster JMeter implementiert.
- **[Bereitstellen einen JMeter JUnit Sampler Leistungstests Elasticsearch][]** beschreibt zu erstellen, der generiert und upload von Daten zu einem Cluster Elasticsearch JUnit Sampler. Dadurch sehr flexibel zum Laden von Tests, die große Mengen an Daten ohne je nach externen Dateien generieren können. 
- **[Die automatisierte Elasticsearch Stabilität Tests][]** zusammengefasst, wie in dieser Serie verwendeten Resiliency-Tests ausführen. 
- **[Ausführen automatisierter Elasticsearch Webleistungstests][]** zusammengefasst, wie in dieser Serie verwendeten Leistungstests ausführen.


[Elasticsearch auf Windows Azure ausgeführte]: guidance-elasticsearch-running-on-azure.md
[Optimieren der Leistung von Daten Einnahme für Elasticsearch auf Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Erstellen einer Umgebung-Leistungstests für Elasticsearch auf Azure]: guidance-elasticsearch-creating-performance-testing-environment.md
[Implementieren eines Testplans JMeter für Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Bereitstellen von JMeter JUnit Sampler für Leistungstests Elasticsearch]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Datenaggregation und Abfrageleistung für Elasticsearch auf Azure optimieren]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Stabilität und Recovery auf Elasticsearch in Azure konfigurieren]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Ausführen von automatisierten Elasticsearch Stabilität Tests]: guidance-elasticsearch-running-automated-resilience-tests.md
[Automatisierte Elasticsearch Leistungstests ausführen]: guidance-elasticsearch-running-automated-performance-tests.md

<properties
   pageTitle="Arbeiten mit Datenquellen "big Data" | Microsoft Azure"
   description="Muster "big Data" Datenquellen Azure BLOB-Speicher und Azure Data See Hadoop HDFS Azure Data Catalog mit Hervorhebung Artikel."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Arbeiten mit großen Datenquellen in Azure Data Catalog

## <a name="introduction"></a>Einführung
**Microsoft Azure Data Catalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und Entdeckung für Enterprise-Datenquellen fungiert. **Azure Data Catalog** ist also über Menschen entdecken, verstehen und mit der Datenquellen und helfen Unternehmen mehr Nutzen aus ihren vorhandenen Datenquellen Datenmengen.

**Azure Data Catalog** unterstützt die Registrierung von Azure Blog Storage Blobs und Verzeichnisse sowie Hadoop HDFS Dateien und Verzeichnisse. Teilweise strukturierte Art dieser Datenquellen ermöglicht Flexibilität bedeutet auch, dass Benutzer berücksichtigen wie Datenquellen angeordnet werden, um vom maximalen Nutzen profitieren mit **Azure Data Catalog**registriert.

## <a name="directories-as-logical-data-sets"></a>Verzeichnisse als logische Datensätze

Ein allgemeines Muster für die Organisation von großen Datenquellen werden als logische Datensätze Verzeichnisse behandeln. Verzeichnisse der obersten Ebene dienen zum Definieren eines Datensatzes und Unterordner Partitionen definieren, die darin enthaltenen Dateien speichern die Daten selbst.

Ein Beispiel für dieses Muster könnten:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

In diesem Beispiel darstellen Vehicle_maintenance_events und Location_tracking_events logische Datensätze. Dieser Ordner enthält Dateien, die nach Jahr und Monat in Unterordnern organisiert werden. Jeder dieser Ordner kann potenziell Hunderte oder Tausende von Dateien enthalten.

In diesem Muster sinnvoll registrieren einzelne Dateien mit **Azure Data Catalog** wahrscheinlich nicht. Die Verzeichnisse, die Daten darstellen, die für die Benutzer die Daten registriert.

## <a name="reference-data-files"></a>Referenz-Dateien

Eine ergänzende wird Verweis Datensätze als einzelne Dateien gespeichert. Diese Datensätze kann man sich als "klein" big Data und sind oft mit Dimensionen in einem Modell analytischen Daten. Referenz-Datendateien enthalten Datensätze verwendet werden, um Kontext für den Großteil der Datendateien an einer anderen Stelle im großen Datenspeicher bereitzustellen.

Ein Beispiel für dieses Muster könnten:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Beim Arbeiten eines Wissenschaftlers Analyst oder Daten mit Daten in größeren Verzeichnisstrukturen Daten in diesen Referenzdateien dienen zu detailliertere Informationen für Personen, die nur durch den Namen oder die ID im größeren Dataset bezeichnet werden.

In diesem Muster wird es sinnvoll, die einzelbetriebliche Datendateien bei **Azure Data Catalog**registrieren. Jede Datei stellt ein Dataset und jeweils versehen und einzeln ermittelt werden kann.

## <a name="alternate-patterns"></a>Alternative Muster

Oben beschriebenen Muster sind nur zwei Möglichkeiten, die große Datenspeicher organisiert werden, aber jede Implementierung unterscheiden. Unabhängig davon, wie Ihre Datenquellen strukturiert werden beim Registrieren von großen Datenquellen mit **Azure Data Catalog**, konzentrieren sich auf Dateien und Verzeichnisse, die Daten darstellen, die relevant für andere in Ihrem Unternehmen registriert. Registrieren alle Dateien und Verzeichnisse unübersichtlich Katalog ausführenden Benutzer zu finden.

## <a name="summary"></a>Zusammenfassung
Registrieren von Datenquellen mit **Azure Data Catalog** erleichtert das Erkennen und verstehen. Registrieren und Kommentieren von großen Dateien und Verzeichnisse, die logische Datensätze darstellen, unterstützen Sie Benutzer suchen und die große Datenquellen, die sie benötigen.

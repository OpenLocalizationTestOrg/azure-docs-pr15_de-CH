<properties
    pageTitle="Azure SQL-Datenbank Benchmark-Übersicht"
    description="Azure SQL-Datenbank Maßstab zur Messung der Leistung von Azure SQL-Datenbank beschrieben."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Azure SQL-Datenbank Benchmark-Übersicht

## <a name="overview"></a>Übersicht
Microsoft Azure SQL-Datenbank bietet drei [Dienstebenen](sql-database-service-tiers.md) mehrere Leistung. Leistungsfähigen enthält Ressourcen oder "Power" bietet einen immer höheren Durchsatz erhöhen.

Es ist wichtig zu quantifizieren, wie die wachsende macht der leistungsfähigen Erhöhte Datenbankperformance übersetzt. Diese Microsoft hat Azure SQL Datenbank Benchmark (ASDB) entwickelt. Benchmark führt grundlegende Operationen OLTP-Arbeitslasten aus. Wir messen den Durchsatz für Datenbanken in jeder Leistungsstufe erreicht.

[Datenbank Buchungseinheiten (DTUs)](sql-database-technical-overview.md#understand-dtus)werden die Ressourcen und jeder Dienstebene Tier und Leistung ausgedrückt. DTUs bieten die Möglichkeit, die relative Kapazität eines Leistung basierend auf einem kombinierten CPU, Arbeitsspeicher, beschreiben und lesen und Schreiben von leistungsfähigen Kurse angeboten. Verdoppelung der DTU Bewertung einer Datenbank entspricht verdoppelt die Leistungsfähigkeit der Datenbank. Der Maßstab ermöglicht die Auswirkung auf die Datenbankperformance zunehmende Leistung angebotenen leistungsfähigen Ausübung eigentlichen Datenbankoperationen beim Skalieren Größe, Anzahl der Benutzer und Transaktionsraten entsprechend der Ressourcen, die Datenbank zu.

Mit den Durchsatz der Standard-Stufe verwenden Transaktionen pro Stunde, Transaktionen pro Minute mit Service-Tier und Premium-Service-Tier Transaktionen pro Sekunde, die Leistung schnell sich von jeder Dienstebene an eine Anwendung erleichtert.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Korrelation von Benchmark-Ergebnisse auf reale Datenbank-performance
Es ist wichtig zu verstehen, dass ASDB, wie alle Benchmarks repräsentativ und vorläufige ist. Erreicht die Anwendung Transaktionsraten werden nicht identisch, die mit einer anderen Anwendung erreicht werden kann. Der Maßstab umfasst eine Sammlung von anderen Transaktion ausführen Typen anhand eines Schemas, die eine Reihe von Tabellen und Datentypen. Während die Benchmark dieselben grundlegenden Operationen, die alle OLTP-Arbeitslasten ausführt, stellt keine Datenbank oder Anwendung eine bestimmte Klasse dar. Das Ziel des Benchmark-ist eine angemessene Anleitung die relative Leistung einer Datenbank bereitstellen, die für die Skalierung nach oben oder unten zwischen Leistung erwartet werden kann. Datenbanken in Wirklichkeit werden unterschiedliche Größen und Komplexität, verschiedene gemischten Arbeitslasten auftreten und unterschiedlich reagieren. Beispielsweise eine e/a-Intensive Anwendung möglicherweise früher IO Schwellenwerte erreicht oder eine CPU-Intensive Anwendung kann CPU-Limits früher erreicht. Es gibt keine Garantie, die eine bestimmte Datenbank wie Benchmark unter Last vergrößern vergrößert.

Benchmark- und seine Methoden werden im folgenden ausführlich beschrieben.

## <a name="benchmark-summary"></a>Benchmark-Zusammenfassung
ASDB misst Performance aus grundlegenden Vorgänge die häufigsten online Transaction processing (OLTP) Arbeitslasten auftreten. Obwohl die Benchmark mit Cloud-computing berücksichtigen, Datenbankschema, Datenbestand vorgesehen und Transaktionen entwickelt wurden, um die grundlegenden Elemente, die am häufigsten in OLTP-Arbeitslasten allgemein repräsentativ sein.

## <a name="schema"></a>Schema
Das Schema soll ausreichend Vielfalt und Komplexität eine Vielzahl von Operationen unterstützen. Der Benchmark-Durchläufe für eine Datenbank besteht aus sechs Tabellen. Tabellen können in drei Kategorien: feste Größe, Skalierung und Wachstum. Es gibt zwei Tabellen fester Größe. drei Tabellen skalieren; und eine wachsende. Fester Größe Tabellen haben eine Konstante Anzahl an Zeilen. Skalierung Tabellen haben eine Kardinalität, die proportional zu Datenbank-Performance, aber nicht während der Maßstab ändern. Die wachsende Tabelle wird angepasst, wie eine Skalierung Tabelle laden, aber die Kardinalität ändert sich während der Ausführung der Benchmark Zeilen eingefügt oder gelöscht werden.

Das Schema enthält eine Mischung von Datentypen, z. B. Integer, numerischen Zeichen und Zeitpunkt. Das Schema der primären und sekundären Schlüssel jedoch keine Fremdschlüssel enthält – gibt also keine referenziellen Integritätsregeln zwischen Tabellen.

Eine Programm Daten generiert Daten für die ursprüngliche Datenbank. Integer und numerischen Daten werden mit verschiedenen Strategien generiert. In manchen Fällen werden Werte über einen zufällig verteilt. In anderen Fällen ist eine Wertemenge zufällig permutiert, um sicherzustellen, dass eine bestimmte beibehalten wird. Textfelder werden aus eine gewichtete Liste von Wörtern, realistische suchen Daten generiert.

Die Datenbank wird angepasst basierend auf einem "Skalierungsfaktor". Der Skalierungsfaktor (abgekürzt SF) bestimmt die Kardinalität der Skalierung und Tabellen. Wie unten im Abschnitt Benutzer und Tempo, die Datenbankgröße Anzahl von Benutzern und maximale Leistung alle proportional zueinander skalieren.

## <a name="transactions"></a>Transaktionen
Die Arbeitslast besteht aus neun Buchungsarten wie in der folgenden Tabelle dargestellt. Jede Transaktion dient eine bestimmte Gruppe von Systemeigenschaften in der Datenbank-Engine und System Hardware mit hohem Kontrast von anderen Transaktionen. Dieser Ansatz vereinfacht die Auswirkungen verschiedene Komponenten gesamt-Performance. "Read Heavy" Buchung ergibt beispielsweise zahlreiche Lesevorgänge von der Festplatte.

| Buchungsart | Beschreibung |
|---|---|
| Lese | WÄHLEN. im Speicher; schreibgeschützt |
| Medium lesen | WÄHLEN. meist im Arbeitsspeicher; schreibgeschützt |
| Dicke lesen | WÄHLEN. nicht im Speicher; schreibgeschützt |
| Update Lite | AKTUALISIEREN; im Speicher; Lese-/ Schreibzugriff |
| Dicke aktualisieren | AKTUALISIEREN; nicht im Speicher; Lese-/ Schreibzugriff |
| Lite einfügen | EINFÜGEN; im Speicher; Lese-/ Schreibzugriff |
| Legen Sie hohe | EINFÜGEN; nicht im Speicher; Lese-/ Schreibzugriff |
| Löschen | LÖSCHEN; Mischung aus im Arbeitsspeicher und nicht im Arbeitsspeicher. Lese-/ Schreibzugriff |
| CPU-schwer | WÄHLEN. im Speicher; relativ hohe CPU-Auslastung; schreibgeschützt |

## <a name="workload-mix"></a>Arbeitslast-mix
Transaktionen werden aus einer gewichteten mit den folgenden allgemeinen zufällig ausgewählt. Anbieten hat Lese-/Schreibverhältnis ungefähr 2:1.

| Buchungsart | % der Mischung |
|---|---|
| Lese | 35 |
| Medium lesen | 20 |
| Dicke lesen | 5 |
| Update Lite | 20 |
| Dicke aktualisieren | 3 |
| Lite einfügen | 3 |
| Legen Sie hohe | 2 |
| Löschen | 2 |
| CPU-schwer | 10 |

## <a name="users-and-pacing"></a>Benutzer und Tempo
Benchmark-Arbeitslast wird durch ein Tool gesteuert, die Transaktionen über mehrere Verbindungen simuliert das Verhalten der Anzahl gleichzeitiger Benutzer übermittelt. Alle Verbindungen und Transaktionen Computer generiert zwar gemeint Einfachheit diese Verbindung als "Benutzer". Obwohl jeder Benutzer unabhängig von anderen Benutzern ausgeführt wird, führen alle Benutzer gleichen Zyklus der unten aufgeführten Schritte:

1. Herstellen einer Verbindungs.
2. Wiederholen Sie bis zum Beenden signalisiert:
    - Wählen Sie eine Transaktion (aus einem gewogenen Verteilung).
    - Führen Sie die ausgewählte Buchung, und messen Sie die Antwortzeit.
    - Warten Sie auf Tempo Verzögerung.
3. Schließen Sie die Verbindung.
4. Beenden.

Die Geschwindigkeitsangabe Verzögerung (in Schritt 2c) nach dem Zufallsprinzip ausgewählt, aber mit durchschnittlich 1,0 Sekunden hat. Daher kann jeder Benutzer durchschnittlich maximal eine Transaktion pro Sekunde generieren.

## <a name="scaling-rules"></a>Skalierung von Regeln
Die Anzahl der Benutzer bestimmt die Datenbankgröße (in Skalierungsfaktor Einheiten). Gibt eine für alle fünf Skalierungsfaktor Einheiten. Wegen der Tempo kann ein Benutzer maximal eine Transaktion pro Sekunde durchschnittlich generieren.

Beispielsweise einen-Skalierungsfaktor 500 (SF = 500) Datenbank haben 100 Benutzer und eine maximale Rate von 100 TPS erreichen. Um eine höhere TPS Laufwerk erfordert Rate Benutzer und einer größeren Datenbank.

In der folgenden Tabelle zeigt die Anzahl der Benutzer tatsächlich für jedes Tier und Performance Service unterstützt.

| Service-Tier (Performance Level) | Benutzer | Datenbankgröße |
|---|---|---|
| Grundlegende | 5 | 720 MB |
| Standard (S0) | 10 | 1 GB |
| Standard (S1) | 20 | 2.1 GB |
| Standard (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6/P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Messung Dauer
Gültige Benchmarkdurchlaufs erfordert eine stationäre Messung Dauer von mindestens eine Stunde.

## <a name="metrics"></a>Metriken
Die Kennzahlen Benchmark sind Durchsatz und Reaktionszeit.

- Durchsatz ist das wesentliche Leistungsmeasure Benchmark. Durchsatz wird in Transaktionen pro von-Zeit, zählen alle Buchungsarten gemeldet.
- Antwortzeit misst Performance Vorhersagbarkeit. Response Time Einschränkung variiert mit Service mit höheren Service mit strengeren Antwort Zeit erforderlich, wie unten dargestellt.

| Klasse  | Durchsatz messen | Antwort-Zeitlimit |
|---|---|---|
| Premium | Transaktionen pro Sekunde | 95. Quantil 0,5 Sekunden |
| Standard | Transaktionen pro minute | 90. Perzentil bei 1,0 Sekunden |
| Grundlegende | Transaktionen pro Stunde | 80. Quantil 2.0 Sekunden |

## <a name="conclusion"></a>Abschluss
Azure SQL-Datenbank Benchmark misst die relative Leistung der Azure SQL-Datenbank über den Bereich der verfügbaren Dienstebenen und Performance. Benchmark führt eine Mischung von grundlegenden Operationen die häufigsten online Transaction processing (OLTP) Arbeitslasten auftreten. Durch Messen der tatsächlichen Leistung bietet Benchmark aussagekräftigere Bewertung der Auswirkung auf Durchsatz ändern Leistung als Liste nur die Ressourcen jeder Ebene wie CPU-Geschwindigkeit, Speicher und IO. In Zukunft werden wir weiterhin entwickeln Benchmark zum Anwendungsbereich und Erweitern der Daten.

## <a name="resources"></a>Ressourcen
[Einführung in SQL-Datenbank](sql-database-technical-overview.md)

[Dienstebenen und Performance](sql-database-service-tiers.md)

[Leitfaden zur Leistung für einzelne Datenbanken](sql-database-performance-guidance.md)

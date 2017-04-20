<properties
   pageTitle="Zum Registrieren von Datenquellen | Microsoft Azure"
   description="Markieren zum Registrieren von Datenquellen mit Azure Data Catalog, einschließlich der Metadaten während der Registrierung extrahiert Artikel."
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


# <a name="how-to-register-data-sources"></a>Zum Registrieren von Datenquellen

## <a name="introduction"></a>Einführung
**Microsoft Azure Data Catalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und Entdeckung für Enterprise-Datenquellen fungiert. Also darum **Azure Data Catalog** Menschen entdecken, verstehen und Verwenden von Datenquellen und helfen Unternehmen mehr Nutzen aus vorhandenen Daten. Und der erste Schritt zu einer Datenquelle über **Azure Data Catalog** erkennbaren Datenquelle registrieren.
## <a name="registering-data-sources"></a>Registrieren von Datenquellen
Registrierung ist die Metadaten aus der Datenquelle und Kopieren von Daten in **Azure Data Catalog** Service. Die Daten bleiben, wo es derzeit befindet, und unter der Kontrolle der Administratoren und Richtlinien des aktuellen Systems bleibt.

Starten Sie zum Registrieren einer Datenquelle **Azure Data Catalog** Data Source Registrierungstool **Azure Data Catalog** -Portal. Melden Sie sich mit der Arbeit oder schulkonto (gleichen Azure Active Directory Anmeldeinformationen im Portal anmelden) und dann die Datenquelle zu registrieren.
[Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md) Lernprogramm für weitere Anleitung anzeigen

Nach der Registrierung der Datenquelle wird der Katalog verfolgt seine Position und seine Metadaten indiziert, sodass Benutzer suchen, durchsuchen und ermitteln die Datenquelle und dann Standort für die Verbindung mit der Anwendung oder einem Tool ihrer Wahl verwenden.

## <a name="sources-supported"></a>Unterstützte Datenquellen
Die Liste der derzeit unterstützten Datenquellen finden Sie unter [Daten Katalog DSR](data-catalog-dsr.md) .
<br/>


## <a name="structural-metadata"></a>Strukturelle Metadaten
Wenn Sie eine Datenquelle erfassen, das Registrierungstool extrahiert Informationen über die Struktur der ausgewählten Objekte – bezeichnet als strukturelle Metadaten.

Für alle Objekte wird diese strukturelle Metadaten Speicherort des Objekts enthalten, sodass Benutzer Daten ermitteln Informationen Verbindung auf das Objekt in die Clienttools ihrer Wahl verwenden können. Andere strukturelle Metadaten enthält Name und Typ und Spalte Attribut Name und Datentyp.

## <a name="descriptive-metadata"></a>Beschreibende Metadaten
Neben den zentralen strukturelle Metadaten aus der Datenquelle extrahiert wird das Quell-Registrierungstool auch sowie beschreibende Metadaten extrahiert. Für SQL Server Analysis Services und SQL Server Reporting Services ist dies die Beschreibung Eigenschaften von diesen Diensten entnommen. Für SQL Server werden Werte über erweiterte Eigenschaft Ms_description extrahiert. Für Oracle-Datenbank extrahiert das Quell-Registrierungstool Spalte Kommentare aus der ALL_TAB_COMMENTS.

Neben den beschreibenden Metadaten aus der Datenquelle extrahiert können Benutzer beschreibende Metadaten Data Source-Registrierungstool auch eingeben. Benutzer können Tags und Experten wird registrierten Objekte identifizieren. Diese beschreibenden Metadaten wird in **Azure Data Catalog** Service mit strukturellen Metadaten kopiert.

## <a name="including-previews"></a>Einschließlich Vorschau

Standardmäßig nur Metadaten aus Datenquellen extrahiert und in **Azure Data Catalog** Service kopiert, aber Verständnis eine Datenquelle häufig erleichtert sehen Sie ein Beispiel der Daten enthält.

**Azure Data Catalog** Data Source-Registrierungstool kann Benutzer eine Vorschau Snapshot der Daten in jeder Tabelle und Ansicht registriert ist. Wenn der Benutzer entscheidet, Vorschau während der Registrierung enthalten, wird das Registrierungstool bis zu 20 Datensätze aus jeder Tabelle oder Ansicht enthalten. Dieser Snapshot wird dann in den Katalog mit strukturellen und beschreibenden Metadaten kopiert.


> [AZURE.NOTE]  Große Tabellen mit vielen Spalten möglicherweise weniger als 20 Datensätze in die Vorschau.


## <a name="including-data-profiles"></a>Einschließlich datenprofile

Wie einschließlich Vorschau wertvolle Kontext für Datenquellen in **Azure Data Catalog**suchen Benutzer bereitstellen können, erleichtern einschließlich ein Datenprofil auch ermittelten Daten verstehen.

**Azure Data Catalog** Data Source-Registrierungstool kann Benutzer Datenprofil für jede Tabelle und Ansicht registriert ist. Wählt der Benutzer ein Datenprofil während der Registrierung enthalten, enthalten das Registrierungstool Statistiken über die Daten in jeder Tabelle und Ansicht, einschließlich:

* Die Anzahl der Zeilen und die Größe der Daten im Objekt
* Das Datum der letzten Aktualisierung der Daten und das Schema des Objekts
* Die Anzahl der Datensätze mit Nullwerten und unterschiedliche Werte für Spalten
* Die Werte Minimum, Maximum, Mittelwert und Standardabweichung für Spalten

Diese Statistiken werden in den Katalog mit strukturellen und beschreibenden Metadaten kopiert.

> [AZURE.NOTE]  Text- und Spalten enthalten keine Mittelwert oder der Standardabweichung Statistiken im Datenprofil.

## <a name="updating-registrations"></a>Registrierung aktualisieren

Registrieren einer Datenquelle werden gefunden in **Azure Data Catalog** mit Metadaten und optionale Vorschau während der Registrierung extrahiert. Wenn die Datenquelle im Katalog aktualisiert werden (z. B. wenn das Schema eines Objekts geändert hat, oder ein Benutzer Daten in der Vorschau aktualisieren Tabellen ursprünglich ausgeschlossen werden muss) kann das Quell-Registrierungstool erneut ausführen.

Erneutes Registrieren einer bereits registrierten Datenquelle eine Zusammenführung "Upsert" durchführt: vorhandene Objekte aktualisiert, neue Objekte erstellt werden. Alle Benutzer über das Portal **Azure Data Catalog** Metadaten erhalten.

## <a name="summary"></a>Zusammenfassung
Registrieren einer Datenquelle **Azure Data Catalog** erleichtert die Datenquelle zu entdecken, strukturelle und beschreibenden Metadaten aus der Datenquelle in der Katalog-Dienst kopiert. Nach der Registrierung einer Datenquelle kann es dann, verwalteten und ermittelten mithilfe von **Azure Data Catalog** -Portal ergänzt.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md) -Lernprogramm schrittweise Informationen zum Registrieren von Datenquellen.

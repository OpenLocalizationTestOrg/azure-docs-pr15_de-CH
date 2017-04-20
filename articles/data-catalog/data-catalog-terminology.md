<properties
   pageTitle="Azure Data Catalog Terminologie | Microsoft Azure"
   description="Dieser Artikel enthält eine Einführung in die Konzepte und Begriffe in Azure Data Catalog-Dokumentation."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure Data Catalog-Terminologie

## <a name="catalog"></a>Katalog

Azure Data Catalog werden Cloud-Metadaten in der Quellen und Datenressourcen registriert werden können. Der Katalog dient als zentraler Speicherort für strukturelle Metadaten aus Datenquellen extrahiert und beschreibende Metadaten von Benutzern hinzugefügt wurden.

## <a name="data-source"></a>Datenquelle

Eine Datenquelle ist ein System oder ein Container, der Daten verwaltet. Beispiele sind SQL Server-Datenbanken, Oracle-Datenbanken, SQL Server Analysis Services-Datenbanken (tabellarischen oder mehrdimensionale) und SQL Server Reporting Services-Server.

## <a name="data-asset"></a>Daten-Anlage

Daten sind Objekte in Datenquellen, die mit dem Katalog registriert werden können. Beispiele für SQL Server-Tabellen und Ansichten, Oracle-Tabellen und Ansichten, SQL Server Analysis Services Kennzahlen, Dimensionen und KPIs und SQL Server Reporting Services-Berichte.

## <a name="data-asset-location"></a>Speicherort der Anlage

Der Katalog speichert den Speicherort der Datenquelle oder Daten-Anlage, die mit der Verwendung einer Clientanwendung verwendet werden kann. Die Format und den Speicherort variieren je nach Typ der Datenquelle. Z. B. eine SQL Server-Tabelle erkennen anhand des Namens vier-Servername, Datenbankname, Objektname-Schemanamen während eines SQL Server Reporting Services-Berichts mit der URL identifiziert werden kann.

## <a name="structural-metadata"></a>Strukturelle Metadaten

Strukturelle Metadaten sind Metadaten extrahiert aus einer Datenquelle, die die Struktur eines Vermögenswertes Daten beschreibt. Dazu gehören Anlagen Speicherort der Objektname und Typ und zusätzliche typspezifische Merkmale. Die strukturelle Metadaten für Tabellen und Ansichten enthält z. B. die Namen und Datentypen für Spalten des Objekts.

## <a name="descriptive-metadata"></a>Beschreibende Metadaten

Beschreibende Metadaten sind Metadaten, die den Zweck oder die Absicht eines Vermögenswertes Daten beschreibt. In der Regel beschreibende Metadaten Katalog Benutzer mithilfe von Azure Data Catalog-Portal hinzugefügt, aber es kann auch aus der Datenquelle extrahiert werden, während der Registrierung. Beispielsweise wird Azure Data Catalog-Registrierungstool Beschreibung Description-Eigenschaft in SQL Server Analysis Services und SQL Server Reporting Services und [Erweiterte Eigenschaft Ms_description](https://technet.microsoft.com/library/ms190243.aspx) in SQL Server-Datenbanken extrahieren Sie diese Eigenschaften mit Werten gefüllt wurde.

## <a name="request-access"></a>Zugriff beantragen

Eine Datenressource beschreibende Metadaten zählen Informationen zum Zugriff auf Daten Anlage oder Datenquelle anzufordern. Diese Informationen umfassen eine oder mehrere der folgenden Optionen und den Speicherort der Anlage erhält.

- Die e-Mail-Adresse des Benutzers oder Teams verantwortlich für den Zugriff auf die Datenquelle.
- Der URL des dokumentierten Prozesses, die Benutzer ausführen müssen, um auf die Datenquelle zugreifen.
- Die URL des ein Identitäts- und Management-Tools (wie Microsoft Identity Manager), die zum Zugriff auf die Datenquelle verwendet werden kann.
- Ein Freitext-Eintrag, der beschreibt, wie Benutzer auf die Datenquelle zugreifen können.

## <a name="preview"></a>Vorschau

Eine Vorschau in Azure Data Catalog ist eine Momentaufnahme der bis zu 20 Datensätze, die während der Registrierung aus der Datenquelle extrahiert und im Katalog mit Anlage Metadaten gespeichert werden können. Die Vorschau können Benutzer eine Datenressource erkennen dessen Funktion und Zweck besser zu verstehen. Also sehen Beispieldaten als nur die Spaltennamen und Datentypen anzeigen kann.
Vorschau nur für Tabellen und Sichten unterstützt und müssen explizit ausgewählt werden vom Benutzer bei der Registrierung.

## <a name="data-profile"></a>Datenprofil

Ein Datenprofil in Azure Data Catalog ist eine Momentaufnahme der Tabelle und Spalte Metadaten eines Vermögenswertes erfassten Daten, das während der Registrierung aus der Datenquelle extrahiert und im Katalog mit Anlage Metadaten gespeichert werden kann. Das Datenprofil können Benutzer eine Datenressource erkennen dessen Funktion und Zweck besser zu verstehen. Ähnlich Vorschau müssen datenprofile explizit vom Benutzer bei der Registrierung ausgewählt werden.

> [AZURE.NOTE] Ein Datenprofil extrahieren kann ein kostspieliger Vorgang für große Tabellen und Ansichten und kann erheblich erhöhen die Datenquelle registrieren.

## <a name="user-perspective"></a>Benutzerperspektive

In Azure Data Catalog kann jeder Benutzer beschreibende Metadaten für einen Vermögenswert erfassten Daten bieten. Jeder Benutzer hat eine unterschiedliche Perspektive der Daten und die Verwendung. Beispielsweise können für einen Server zuständige Administrator die Details der Vereinbarung zum Servicelevel (SLA) oder backup-Fenster; Links zur Dokumentation für Unternehmen unterstützt Daten verarbeitet verfügen Daten verantwortungsvoll; und Analysten kann eine Beschreibung in der wichtigsten andere Analysten sind und was kann zu die Benutzer, die zu entdecken und die Daten.

Diese Perspektiven sind grundsätzlich wertvolle und bei Azure Data Catalog bieten jeden Benutzer Informationen sinnvoll, alle diese Informationen können zwar Daten und ihren Zweck verstehen.

## <a name="expert"></a>Experte

Ein Experte ist ein Benutzer, der eine fundierte "Experte" Perspektive für eine Datenressource festgestellt wurde. Alle Benutzer kann sich selbst oder andere Benutzer als Experte für eine Anlage hinzufügen. Als Experte aufgeführten zusätzlichen Berechtigungen in Azure Data Catalog überträgt; die Anwender einfach die Perspektiven zu suchen, die am wahrscheinlichsten nützlich sein, wenn eine Anlage beschreibende Metadaten überprüfen.

## <a name="owner"></a>Besitzer

Besitzer ist ein Benutzer zusätzliche Berechtigungen für die Verwaltung eines Vermögenswertes Daten in Azure Data Catalog. Benutzer übernehmen registrierten Daten und Besitzer können andere Benutzer als Eigentümer. Weitere Informationen finden Sie unter [Verwalten von Daten](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Eigentum und Management stehen nur in der Standard Edition von Azure Data Catalog.

## <a name="registration"></a>Registrierung

Registrierung ist die Anlage Metadaten aus einer Datenquelle extrahiert und in Azure Data Catalog Service kopieren. Datenbestände, die erfasst wurden, können dann versehen und entdeckt.

## <a name="see-also"></a>Siehe auch

- [Was ist Azure Data Catalog?](data-catalog-what-is-data-catalog.md) -Dieser Artikel bietet eine Übersicht über Azure Data Catalog Service den nutzen und die Szenarien unterstützt.

- [Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md) - dieser Artikel bietet eine End-to-End-Lernprogramm, das wie Quelle Datenermittlung mit Azure Data Catalog.  

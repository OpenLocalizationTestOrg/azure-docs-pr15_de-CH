<properties
   pageTitle="Azure Data Catalog häufig gestellte Fragen | Microsoft Azure"
   description="Häufig gestellte Fragen zur Azure Data Catalog, einschließlich Funktionen zur Erkennung von Quelle, Anmerkung und Management."
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

# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure Data Catalog häufig gestellte Fragen

Dieser Artikel beantwortet häufig gestellte Fragen zu Microsoft **Azure Data Catalog** Service.

## <a name="q-what-is-azure-data-catalog"></a>Was ist Azure Data Catalog?

A: Microsoft Azure Data Catalog ist ein vollständig verwalteter Dienst Microsoft Azure-Cloud, die als ein System der Registrierung und Entdeckung für Enterprise-Datenquellen fungiert. Azure Data Catalog bietet Funktionen, mit denen jeder Benutzer von Analysten Datenanalysten Entwicklern – registrieren, entdecken, verstehen und Datenquellen verwenden.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>Welche Kunden fordert wird Azure Data Catalog beheben?

Azure Data Catalog stellt die Herausforderung Data Source und "Dunkle Daten" Benutzer zu entdecken Enterprise-Datenquellen.

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>F: Wer sind die Zielgruppe für Azure Data Catalog?

Azure Data Catalog bietet Funktionen für technische und nichttechnische Benutzer:

- Datenentwickler, BI und Analytics-Experten: Daten und Analysen Inhalte zu verwenden sind
-   Daten-Verwalter: Diejenigen, die wissen über Daten, was es bedeutet und wie es verwendet werden soll und für welche Zwecke
- Datennutzer: Wer können leicht erkennen, verstehen und Daten für ihre Arbeit mit dem Tool ihrer Wahl erforderlich
- Zentrale IT: diejenigen, die Hunderte von Datenquellen für die Benutzer sichtbar zu machen, und die müssen Kontrolle wie Daten verwendet werden und von wem,

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>Was ist der Azure Data Catalog Region Verfügbarkeit?

Azure Data Catalog-Dienste sind derzeit in folgenden Rechenzentren:

- Westen der USA
- Osten der USA
- Westeuropa
- Nordeuropa
- Australien OST
- Südostasien

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>Was sind die Grenzwerte für die Anzahl der Datenbestände in Azure Data Catalog?

Die kostenlose Edition von Azure Data Catalog beträgt 5.000 registrierten Daten.

Die Standard Edition von Azure Data Catalog unterstützt bis zu 100.000 registrierte Datenbestände.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>Was sind die unterstützten Datentypen Quell- und Anlage?

Die Liste der derzeit unterstützten Datenquellen finden Sie unter [Daten Katalog DSR](data-catalog-dsr.md) .

## <a name="q-how-do-i-request-support-for-another-data-source"></a>F: wie wird anfordern Unterstützung für eine andere Datenquelle?

Anfragen zu Features und Sonstiges Feedback können in [Azure Data Catalog Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)gesendet werden.

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>F: wie bei Azure Data Catalog beginne ich?

Der beste Ausgangspunkt ist gemäß der Anleitung in [Erste Schritte mit Data Catalog](data-catalog-get-started.md). Dieser Artikel ist eine End-to-End-Übersicht über die Funktionen im Dienst.

## <a name="q-how-do-i-register-my-data"></a>F: Wie registriere ich meine Daten?

Starten Sie zum Registrieren Ihrer Daten in Azure Data Catalog Registrierungstool Azure Data Catalog aus dem Azure Data Catalog-Portal "Veröffentlichen". Melden Sie in Azure Data Catalog-publishing-Anwendung auf Azure Data Catalog-Portal verwenden dieselben Anmeldeinformationen an, und wählen Sie die Datenquelle und bestimmten Anlagen, die Sie registrieren möchten.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>Welche Eigenschaften werden für Daten extrahiert, das registriert werden?

Die spezifischen Eigenschaften variiert von Datenquelle Datenquelle, aber im Allgemeinen extrahieren Azure Data Catalog-Publishingdienst Folgendes:

- Name der Anlage
- Asset-Typ
- Anlage Beschreibung
- Attribut-Spalte
- Spalte Attribut Datentypen
- Spalte Attribut Beschreibung

> [AZURE.IMPORTANT] Erfassen von Daten mit Azure Data Catalog nicht verschieben oder Daten in die Cloud. Registrieren von Ressourcen aus einer Datenquelle kopiert die Anlagen Metadaten in Azure, aber die Daten bleiben in den vorhandenen Speicherort der Datenquelle. Die einzige Ausnahme von dieser Regel ist ein Benutzer hochladen Vorschau der Datensätze oder ein Datenprofil Anlagen registrieren. Wenn einschließlich einer Vorschau der bis zu 20 Datensätze von jeder Anlage kopiert werden und als Snapshot in Azure Data Catalog gespeichert. Wenn ein Datenprofil, werden zusammengefasste Informationen (wie Tabellen, null Prozentwerte pro Spalte und die minimale, maximale und durchschnittliche Werte für Spalten) berechnet und enthalten in den Metadaten im Katalog gespeichert.

<br/>

> [AZURE.NOTE] Für Datenquellen wie SQL Server Analysis Services, die erstklassige **Description** -Eigenschaft wird Azure Data Catalog veröffentlichen Anwendung dieser Eigenschaftswert extrahiert. Für SQL Server relationalen Datenbanken **eine erstklassige Beschreibungseigenschaft fehlt** extrahiert Azure Data Catalog veröffentlichenden Anwendung den Wert von Ms_description erweiterte Eigenschaft für die Objekte und Spalten. Weitere Informationen finden Sie in TechNet [Using Extended Properties on Database Objects](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>F: dauert wie lange für neu registrierte Anlagen in Azure Data Katalog es?

Nach dem Registrieren von Anlagen mit Azure Data Catalog möglicherweise 5 bis 10 Sekunden in Azure Data Catalog-Portal angezeigt.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>F: wie Kommentieren und gestalten Metadaten für meine Daten-Ressourcen?

Am einfachsten zu Metadaten für registrierte Anlagen ist die Anlage im Azure Data Catalog-Portal und geben Metadaten im Eigenschaftenfenster oder im Schema für das ausgewählte Objekt.

Sie können auch einige Metadaten Experten wie Tags während der Registrierung bereitstellen. Die Werte in Azure Data Catalog-Publishingdienst gilt für alle Elemente gleichzeitig registriert. Zum Anzeigen der zuletzt registrierten Objekte im Portal für zusätzliche Anmerkung wählen Sie **Ansicht Portal** auf dem letzten Bildschirm des veröffentlichenden Azure Data Catalog-Anwendung.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>F: wie löschen Sie registrierte Objekte

Sie können ein Objekt von Azure Data Catalog löschen Objekts im Portal auswählen und dann auf die Schaltfläche **Löschen** . Entfernt die Metadaten für das Objekt von Azure Data Catalog Dies wirkt sich nicht auf die tatsächlichen zugrunde liegende Datenquelle.

## <a name="q-what-is-an-expert"></a>Was ist ein Experte?

Ein Experte ist jemand über Sicht über ein Objekt. Ein Objekt kann mehrere Experten haben. Experte muss nicht "Besitzer" für ein Objekt. der Experte ist einfach Wer weiß, wie die Daten können und sollte verwendet werden.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>F: wie Teile ich Informationen Azure Data Catalog-Team, wenn Probleme auftreten?

Verwenden von Azure Data Catalog Forum für Probleme, Informationen und Fragen. Das Forum finden Sie unter http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>F: Azure Data Catalog funktioniert mit dieser Datenquelle, die mich interessiert?
Wir arbeiten aktiv Azure Data Catalog Weitere Datenquellen hinzufügen. Ist eine Datenquelle möchten Sie unterstützt, Sie schlägt (oder den Support voice, wenn bereits vorgeschlagen) in [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>F: wie bezieht Azure Data Catalog Data Catalog in Power BI für Office 365?

Azure Data Catalog vorstellen als Weiterentwicklung des Katalogs Daten. Azure Data Catalog bietet ähnliche Funktionen Data Source veröffentlichen und ist jedoch auf umfassendere Szenarien und nicht von Office 365. Danach Azure Data Catalog allgemein verfügbar werden ein einzelner Dienst die beiden Kataloge zusammenführen.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>Welche Berechtigungen benötigt ein Benutzer Elemente bei Azure Data Catalog registrieren?

Der Benutzer das Registrierungstool Azure Data Catalog benötigt Berechtigungen in der Datenquelle, mit der die Metadaten aus der Quelle lesen können. Wählt der Benutzer auch auf eine Vorschau, muss der Benutzer auch berechtigt, die ihm ermöglichen, die Daten aus den Objekten registriert.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>F: erfolgen Azure Data Catalog zur lokalen Bereitstellung sowie?

Azure Data Catalog ist Cloud, die Cloud und lokalen Datenquellen arbeiten kann Hybrid Data Source Discovery Lösung. Zurzeit keine Pläne für eine Version von Azure Data Catalog Service, die lokal ausgeführt werden.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>F: extrahiert können mehr / mehr Metadaten aus Datenquellen, die wir registrieren?

Wir arbeiten aktiv erweitern die Funktionen von Azure Data Catalog. Sollten Sie zusätzliche Metadaten extrahierten Daten aus der Datenquelle während der Registrierung Siehe möchten ist Sie (oder Abstimmung Wenn bereits vorgeschlagen) in [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). In Zukunft können wir dritte neue Arten von Datenquellen durch eine Erweiterbarkeits-API hinzugefügt.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>F: beschränken ich Sichtbarkeit registrierte Datenbestände, so dass nur bestimmte Personen entdecken?

A: Wählen Sie die Datenbestände in Azure Data Catalog, und klicken Sie auf "Übernehmen". Besitzer der Datenbestände in Azure Data Catalog können sichtbarkeitseinstellungen, um entweder alle katalogbenutzer Besitzer Ressourcen Ermittlung oder die Sichtbarkeit auf bestimmte Benutzer einschränken können.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>F: wie aktualisiere ich die Registrierung für eine Anlage Daten, die in der Datenquelle geändert werden im Katalog angezeigt?

A: um Metadaten für Daten aktualisieren, die bereits im Katalog registriert werden, erneut registrieren der Datenquelle, die Anlagen enthält. Jede Änderung in der Datenquelle z. B. Spalten hinzugefügt oder aus Tabellen oder Ansichten entfernt aktualisiert werden im Katalog, aber jeder Benutzer Kommentare beibehalten.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>Meine Frage ist nicht hier beantwortet Was soll ich tun?

In [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Es Fragen finden ihren Weg.

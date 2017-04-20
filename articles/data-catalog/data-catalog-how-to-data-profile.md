<properties
    pageTitle="Wie Daten Profildatenquellen"
    description="Gewusst wie-Artikel hervorheben wie auf Tabelle und Spalte datenprofile beim Registrieren von Datenquellen in Azure Data Catalog und wie Sie datenprofile um Datenquellen zu verstehen."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Profildatenquellen Daten

## <a name="introduction"></a>Einführung

**Microsoft Azure Data Catalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und Entdeckung für Enterprise-Datenquellen fungiert. Also darum **Azure Data Catalog** Menschen entdecken, verstehen und Verwenden von Datenquellen und helfen Unternehmen mehr Nutzen aus vorhandenen Daten. Beim Registrieren einer Datenquelle in **Azure Data Catalog**seine Metadaten kopiert und vom Dienst indiziert, aber die Geschichte nicht zu Ende.

Funktion **Data Profiling** **Azure Data Catalog** überprüft die Daten in Ihrem Katalog unterstützten Datenquellen und sammelt Statistiken und Informationen zu diesen Daten. Es ist einfach ein Profil Ihrer Datenbestände. Beim Registrieren eines Vermögenswertes Daten wählen Sie im Data Source-Registrierungstool **Datenprofil enthalten** .

## <a name="what-is-data-profiling"></a>Was ist Data Profiling

Daten-profiling überprüft die Daten in der Datenquelle registriert und sammelt Statistiken und Informationen zu diesen Daten. Während der Erkennung von Quelle unterstützen diese Statistiken Sie die Eignung der Daten ihrer geschäftlichen Problems.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Die folgenden Datenquellen unterstützt Daten-profiling:

- SQL Server (einschließlich Azure SQL DB und Azure SQL Data Warehouse) Tabellen und Ansichten
- Oracle-Tabellen und Ansichten
- Teradata Tabellen und Ansichten
- Struktur von Tabellen

Einschließlich datenprofile beim Erfassen Daten Benutzer kann Fragen zu Datenquellen, einschließlich:

-   Lässt es sich mein geschäftliches Problem lösen?
-   Entspricht die Daten bestimmte Standards oder Muster?
-   Was sind einige Anomalien der Datenquelle
-   Was sind die möglichen Probleme diese Daten in die Anwendung integrieren?

> [AZURE.NOTE] Sie können auch Dokumentation hinzufügen, um zu beschreiben, wie Daten in eine Anwendung integriert werden können. Finden Sie unter [Datenquellen zu dokumentieren](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Wie ein Datenprofil beim Registrieren einer Datenquelle

Es ist einfach ein Profil Ihrer Datenquelle enthalten. Beim Registrieren einer Datenquelle im Bereich **registriert werden Objekte** des Data Source-Registrierungstools **Enthalten Datenprofil**auswählen.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Weitere Informationen zum Registrieren von Datenquellen finden Sie unter [Datenquellen registrieren](data-catalog-how-to-register.md) und [mit Azure Data Catalog](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtern nach Daten, die datenprofile einschließen
Daten um zu ermitteln, die ein Datenprofil, zählen `has:tableDataProfiles` oder `has:columnsDataProfiles` als einen der Suchbegriffe.

> [AZURE.NOTE] **Daten-Profil einschließen** in das Quell-Registrierungstool auswählen enthält Tabelle und Profilinformationen auf. Allerdings kann die Katalog-API Datenbestände mit nur einem Profilinformationen registriert werden.

## <a name="viewing-data-profile-information"></a>Daten-Profilinformationen anzeigen

Wenn Sie eine geeignete Datenquelle mit einem Profil finden, Profildetails Daten angezeigt. Um Datenprofil anzuzeigen, wählen Sie eine Datenressource und **Datenprofil** im Fenster Data Catalog Portal.

![](media\data-catalog-data-profile\data-catalog-view.png)

Ein Datenprofil in **Azure Data Catalog** zeigt Tabelle und Spalte Profil Informationen, darunter:

### <a name="object-data-profile"></a>Objekt Datenprofil

-   Anzahl Zeilen
-   Tabellengröße
-   Wenn das Objekt zuletzt aktualisiert wurde

### <a name="column-data-profile"></a>Spalte Datenprofil

- Datentyp der Spalte
- Anzahl eindeutiger Werte
- Anzahl der Zeilen mit NULL-Werten
- Minimum, Maximum, Mittelwert und Standardabweichung für Werte

## <a name="summary"></a>Zusammenfassung
Data profiling stellt Statistiken und Informationen zu erfassten Daten können Sie die Eignung der Daten zu lösen. Kommentieren und Dokumentieren von Datenquellen, geben Daten Profilen Benutzer ein besseres Verständnis Ihrer Daten.


## <a name="see-also"></a>Siehe auch
-   [Zum Registrieren von Datenquellen](data-catalog-how-to-register.md)
-   [Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md)


<properties
    pageTitle="Synchronisierung mit Azure AD Connect Health | Microsoft Azure"
    description="Dies ist der Azure AD Connect Health-Seite, die von Azure AD Connect Sync überwachen erläutert."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>Verwenden von Azure AD Connect Health für Synchronisierung
Die folgende Dokumentation ist spezifisch für Azure AD Connect (synchronisiert) mit Azure AD Connect Health monitoring.  Informationen zum Überwachen von AD FS mit Azure AD verbinden finden Sie unter [Verwendung von Azure AD Connect Health mit AD FS](active-directory-aadconnect-health-adfs.md). Darüber hinaus Informationen zum Überwachen von Active Directory-Domänendienste mit Azure AD verbinden finden Sie unter [Verwendung von Azure AD Connect Health in AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD Connect Health für Synchronisierung](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Alarme für Azure AD Connect Health für Synchronisierung
Der Azure AD verbinden Gesundheit Alarme für Synchronisierung Abschnitt enthält eine Liste der aktiven Warnungen. Jede Warnung enthält Informationen, die Schritte und Links zu verwandter Dokumentation. Auswählen einer aktiven oder gelösten Warnung sehen Sie ein neues Blatt mit zusätzlichen Informationen sowie die aufzulösende Warnung und Links zu weiterführenden Dokumentationen Schritte. Sie können auch Verlaufsdaten Warnungen anzeigen, die in der Vergangenheit gelöst wurden.

Durch Auswählen einer Warnung werden Sie mit zusätzlichen Informationen und Schritte erhalten Sie die um Warnung und Links zu weiterführenden Dokumentationen zu beheben.

![Azure AD Connect Synchronisierungsfehler](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Begrenzte Auswertung von Alarmen
Wenn Azure AD Connect nicht die Standardkonfiguration (z. B. Attribute Filtern von der Standardkonfiguration in einer benutzerdefinierten Konfiguration geändert wird), lädt der Azure AD Connect Health Agent nicht Bezug auf Azure AD Connect Fehlerereignisse.

Dadurch wird die Auswertung von Alarmen vom Dienst. Sie würden einen Banner sehen, der diese Bedingung im Azure-Portal unter Ihren Dienst angibt.

![Azure AD Connect Health für Synchronisierung](./media/active-directory-aadconnect-health-sync/banner.png)

Durch Klicken auf "Settings" und Azure AD Connect Health Agent alle Fehlerprotokolle hochladen, können Sie dies ändern.

![Azure AD Connect Health für Synchronisierung](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Sync Einblick
Administratoren möchten häufig kennen Azure AD Sync geändert und die Änderungen benötigte Zeit. Dieses Feature bietet eine einfache Möglichkeit, dies mit visualisieren die folgenden Diagramme:   

- Wartezeit der Synchronisierungsvorgänge
- Objekt ändern trend

### <a name="sync-latency"></a>Sync-Wartezeit
Diese Funktion stellt grafischen Trend Wartezeit Synchronisierungsvorgänge (Import, Export usw.) Anschlüsse.  Dadurch schnell und einfach zu verstehen, nicht nur die Wartezeit Operationen (größer haben eine Reihe von Änderungen) sondern auch eine Möglichkeit, in der Anomalien erkennen, die weitere Untersuchung erfordern.

![Sync-Wartezeit](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Standardmäßig wird nur die Latenz der Operation "Export" für den Azure AD-Connector angezeigt.  Um mehr Vorgänge auf dem Connector bzw. Vorgänge andere Connectors anzeigen mit der Maustaste auf das Diagramm, wählen Sie Diagramm bearbeiten oder klicken Sie auf die Schaltfläche "Wartezeit Diagramm bearbeiten" und wählen Sie die bestimmte Operation und Connectors.

### <a name="sync-object-changes"></a>Sync-Objekt ändern
Diese Funktion stellt grafischen Trend nderungen werden ausgewertet und in Azure AD exportiert.  Versuch, diese Informationen aus den Protokollen Sync ist heutzutage schwierig.  Die Tabelle gibt, nicht nur einfacher überwachen die Anzahl der Änderungen, die in Ihrer Umgebung auftreten, sondern auch eine visuelle Ansicht der Fehler auftreten.

![Sync-Wartezeit](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Objekt auf Synchronisierung Fehlerbericht (Vorschau)
Diese Funktion bietet einen Bericht über Fehler, die auftreten können, wenn Daten zwischen Windows Server Active Directory Azure AD mit Azure AD verbinden synchronisiert werden.

- Der Bericht umfasst Sync Client erfassten Fehler (Azure AD Connect Version 1.1.281.0 oder höher)
- Sie enthält den Fehler bei der letzten Synchronisierung auf dem Synchronisierungsmodul. ("Exportieren Sie" auf den Azure Active Directory Connector.)
- Azure AD Connect Health Agent zur Synchronisierung müssen ausgehende Verbindungen auf die erforderlichen Endpunkte für den Bericht mit den neuesten Daten. Finden Sie im [Abschnitt](active-directory-aadconnect-health-agent-install.md#Requirements) Details.
- Der Bericht wird **alle 30 Minuten aktualisiert** Daten von Azure AD Connect Health Agent für Synchronisation hochgeladen.
Bietet die folgenden wichtigsten Funktionen

    - Kategorisierung von Fehlern
    - Liste der Objekte mit Fehler pro Kategorie
    - Alle Daten zu Fehlern an einem Ort
    - Vergleich von Objekten mit Fehler aufgrund eines Konflikts nebeneinander
    - Herunterladen Sie den Bericht als eine CVS (demnächst verfügbar)

### <a name="categorization-of-errors"></a>Kategorisierung von Fehlern
Der Bericht kategorisiert die vorhandenen Synchronisierungsfehler in folgenden Kategorien:

| Kategorie | Beschreibung |
| -------------- | ----------- |
| Doppeltes Attribut. | Fehler bei Azure AD-Verbindung versucht erstellen oder aktualisieren Objekte mit doppelten Werten ein oder mehrere Attribute in Azure AD in einem Mandanten wie ProxyAddresses UserPrincipalName eindeutig sein muss. |
| Falsche Zuordnung | Fehler beim Soft-Übereinstimmung schlägt fehl mit Fehler führen Objekte. |
| Validierungsfehler Daten | Fehler aufgrund ungültiger Daten wie nicht unterstützte Zeichen in wichtige Attribute wie UserPrincipalName, formatieren Sie Fehler, die validiert Schreibvorgangs in Azure AD.|
| Große Attribut | Fehler bei einem oder mehreren Attributen Größe, Länge oder Anzahl überschreiten.|
| Andere | Alle anderen Fehler, die in den oben genannten Kategorien passen. Aufgrund des Feedbacks werden dieser Kategorie in Unterkategorien aufgeteilt.

![Zusammenfassung des Fehlers synchronisieren](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Fehler Berichtskategorien synchronisieren](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Liste der Objekte mit Fehler pro Kategorie
Drilldown für jede Kategorie bietet die Liste der Objekte des Fehlers in dieser Kategorie.
![Fehler Bericht Synchronisierungsliste](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Fehlerdetails
Folgende Daten steht in der Detailansicht für jeden Fehler

- Bezeichner für das *AD-Objekt* an
- Bezeichner des *Azure AD-Objekt* an (falls zutreffend)
- Beschreibung und Behebung
- Verwandte Artikel

![-Computernamen synchronisieren](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Den Bericht als CSV-Datei herunterladen
Diese Funktion ist in Kürze. Achten Sie auf Weitere Updates.



## <a name="related-links"></a>Verwandte links
* [Problembehandlung bei Fehlern während der Synchronisierung](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Doppeltes Attribut Stabilität](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Agent-Installation](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Operationen](active-directory-aadconnect-health-operations.md)
* [Schließen Sie Gesundheit mit Azure AD mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Gesundheit in AD DS verbinden über Azure AD](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Version Gesundheitsdaten](active-directory-aadconnect-health-version-history.md)

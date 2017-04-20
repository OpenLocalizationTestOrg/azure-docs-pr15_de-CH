<properties
    pageTitle="Azure AD Connect-Synchronisierung: Synchronisierung Service Manager-UI | Microsoft Azure"
    description="Verstehen der Registerkarte Vorgänge im Synchronisation-Manager Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect-Synchronisierung: Synchronisierung Service Manager

[Vorgänge](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Anschlüsse](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverse-Suche](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Registerkarte "Vorgänge" zeigt die Ergebnisse aus der letzten. Diese Registerkarte ist Schlüssel zu verstehen und Probleme zu beheben.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Informationen Sie auf der Registerkarte Vorgänge angezeigt
Die obere Hälfte zeigt alle chronischen nacheinander. Standardmäßig das Protokoll enthält Informationen zu den letzten sieben Tagen, aber diese Einstellung kann mit dem [Taskplaner](active-directory-aadconnectsync-feature-scheduler.md). Möchten Sie alle ausführen suchen, die keine Erfolgsmeldung angezeigt. Sie können die Sortierung auf die Header ändern.

Die Spalte **Status** der wichtigsten Informationen und zeigt das schwerwiegendste Problem laufen. Hier wird eine Übersicht über die häufigsten Status nach Priorität zu (wobei * mögliche Fehlermeldungen anzuzeigen).

Status | Kommentar
--- | ---
gestoppt: * | Ausführen konnte nicht abgeschlossen werden. Wenn z. B. das Remotesystem ist heruntergefahren und nicht erreichbar.
beendet-Fehler-limit | Es sind mehr als 5.000. Die Ausführung wurde aufgrund der großen Anzahl Fehler automatisch beendet.
abgeschlossen -\*-Fehler | Die Ausführung abgeschlossen, jedoch Fehler (weniger als 5.000), der untersucht werden sollte.
abgeschlossen -\*-Warnung | Die Ausführung abgeschlossen, aber einige Daten ist nicht im erwarteten Zustand. Wenn Fehler auftreten, wird diese Nachricht normalerweise ein Symptom. Bis Sie Fehler behoben haben, sollten Sie keine Warnungen untersuchen.
Erfolg | Keine Probleme.

Beim Auswählen einer Zeile wird aktualisiert unten die Einzelheiten ausgeführt. Links unten müssen Sie eine Liste **Schritt #**sagen. Diese Liste wird nur angezeigt, wenn Sie mehrere Domänen in der Gesamtstruktur, wobei jede Domäne einen Schritt angegeben ist. Der Domänenname finden unter der Überschrift **Partition**. Unter **Synchronisationsstatistik**finden Sie weitere Informationen über die Anzahl der Änderungen, die verarbeitet wurden. Klicken Sie auf die Links, um eine Liste der geänderten Objekte. Wenn Objekte mit Fehlern Fehler unter **Synchronisierungsfehler**angezeigt.

## <a name="troubleshoot-errors-in-operations-tab"></a>Problembehandlung bei Fehlern in Registerkarte "Vorgänge"
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Wenn Fehler auftreten, das Objekt Fehler und der Fehler selbst sind Links, die Weitere Informationen.

Starten der Fehlerzeichenfolge (**Sync-Regel-Fehler-Funktion ausgelöst** in der Abbildung). Zunächst eine Übersicht über das Objekt angezeigt. Um den tatsächlichen Fehler anzuzeigen, klicken Sie **Stapelrahmen**. Diese Verfolgung bietet Debug-Informationen für den Fehler.

**Tipp:** Sie können mit der rechten Maustaste im Feld **aufgerufen Stapelinformationen** , **Alles markieren**und **Kopieren**. Sie können den Stapel kopiert und betrachten Sie den Fehler in Ihrem bevorzugten Editor wie Notepad.

- Ist der Fehler aus **SyncRulesEngine**, hat Aufruflisteninformationen zunächst eine Liste aller Attribute, auf das Objekt. Gehen Sie die Überschrift **InnerException = >**.  
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
Die Zeile nach zeigt den Fehler. In der obigen Abbildung ist der Fehler von einem benutzerdefinierten Sync Regel Fabrikam erstellt.

Der Fehler selbst nicht genügend Informationen erhalten, ist es Zeit, um die Daten selbst. Klicken Sie auf den Link mit dem Objektbezeichner und [Führen Sie ein Objekt und seine Daten über das System](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

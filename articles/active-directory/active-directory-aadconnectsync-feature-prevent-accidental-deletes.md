<properties
   pageTitle="Azure AD Verbindung synchronisieren: verhindert versehentliche Löschen | Microsoft Azure"
   description="Dieses Thema beschreibt die Funktion verhindern versehentliches Löschen (verhindert versehentliche Löschen) in Azure AD verbinden."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Verbindung synchronisieren: versehentliche Löschen sperren
Dieses Thema beschreibt die Funktion verhindern versehentliches Löschen (verhindert versehentliche Löschen) in Azure AD verbinden.

Beim Installieren von Azure AD Connect zu verhindern, dass versehentlich löscht standardmäßig aktiviert und konfiguriert keinen Export mit mehr als 500 löscht. Diese Funktion dient Sie versehentlich Konfiguration Änderungen in das lokale Verzeichnis vor, die viele Benutzer und anderen Objekte auswirkt.

Häufige Szenarien beim finden Sie gehören viele löscht:

- Wechselt zu [Filtern](active-directory-aadconnectsync-configure-filtering.md) , in denen eine ganze [Organisationseinheit](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) oder [Domäne](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) deaktiviert ist.
- Alle Objekte in einer Organisationseinheit werden gelöscht.
- Eine Organisationseinheit wird umbenannt, damit alle Objekte gelten Gültigkeitsbereich für die Synchronisierung.

Der Standardwert von 500 Objekten kann mit PowerShell mit `Enable-ADSyncExportDeletionThreshold`. Konfigurieren Sie diesen Wert, um die Größe der Organisation. Da der Planer synchronisieren alle 30 Minuten ausgeführt wird, ist der Wert Anzahl löscht innerhalb 30 Minuten.

Sind zu viele löscht um Azure Anzeige exportierenden bereitgestellt, danach exportieren und eine e-Mail wie folgt:

![Verhindern, dass versehentlich löscht e-Mail](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hallo (technische Kontakt). Der Synchronisierungsdienst Identität erkannt (Zeit), die Anzahl von Löschvorgängen konfigurierten löschen, für (Name der Organisation überschritten). Insgesamt (Anzahl) Objekte wurden Löschen dieser Identität Synchronisierung gesendet. Dies erreicht oder überschritten Schwellenwert konfigurierten Löschen von Objekten (Zahl). Sie bestätigen, dass die gelöschten Informationen verarbeitet werden sollen, bevor wir fortfahren. Finden Sie unter verhindert versehentlichen Löschen für Weitere Informationen über den Fehler in dieser e-Mail-Nachricht aufgeführt.*

Sie sehen auch den Status `stopped-deletion-threshold-exceeded` Wenn Sie suchen im **Dienst Synchronisierung** UI Profil exportieren.
![Keine versehentliche Löschung Sync Service Manager-UI](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Wenn dies unerwartet, untersuchen und Maßnahmen ergreifen. Um anzuzeigen, welche Objekte gelöscht werden sollen, führen Sie folgende Schritte aus:

1. Starten Sie **Synchronisierungsdienst** im Startmenü.
2. Wechseln Sie zu **Connectors**.
3. Markieren Sie den Verbinder mit **Azure Active Directory**.
4. Wählen Sie unter **Aktionen** rechts **Connectorspace suchen**.
5. Im Popupfenster **Rahmen** **Getrennt, da** wählen Sie, und wählen Sie einen Zeitpunkt in der Vergangenheit. Klicken Sie auf **Suchen**. Diese Seite enthält eine Ansicht aller Objekte gelöscht werden sollen. Auf jedes Element klicken, erhalten Sie weitere Informationen über das Objekt. Klicken Sie auf **Spalte** Hinzufügen zusätzlicher Attribute im Raster angezeigt werden.

![Connectorspace suchen](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Gegebenenfalls alle löscht dann folgendermaßen Sie vor:

1. Vorübergehend deaktivieren dieser Schutz und die löscht durchlaufen, das PowerShell-Cmdlet ausführen: `Disable-ADSyncExportDeletionThreshold`. Bereitstellen Sie ein Azure AD globales Administratorkonto und Kennwort.
![Anmeldeinformationen](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. Mit Azure Active Directory Connector ausgewählt wählen Sie die Aktion **Ausführen** und **Exportieren**.
3. Um den Schutz wieder zu aktivieren, führen Sie das PowerShell-Cmdlet: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Nächste Schritte

**Themen (Übersicht)**

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

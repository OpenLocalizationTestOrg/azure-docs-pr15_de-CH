<properties
    pageTitle="Azure AD Verbindung synchronisieren: Ausführen des Installationsassistenten erneut | Microsoft Azure"
    description="Erläutert die Funktionsweise der Installations-Assistent der zweite Ausführung es."
    keywords="Der Installationsassistent Azure AD Connect können Sie Wartung erneut konfigurieren ausführen"
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Verbindung synchronisieren: den Installationsassistenten erneut ausführen
Beim ersten Ausführen von Azure AD Connect-Installationsassistent führt es Sie durch die Installation konfigurieren. Wenn Sie den Installationsassistenten erneut ausführen, bietet Optionen für die Wartung.

Der Installationsassistent finden Sie im Startmenü mit dem Namen **Azure AD Connect**.

![Startmenü](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Beim Starten von der Installations-Assistent eine Seite mit den folgenden Optionen angezeigt:

![Auf der Seite eine Liste zusätzlicher Vorgänge](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Wenn Sie Azure AD Connect ADFS installiert haben, müssen Sie noch weitere Optionen. Zusätzlichen Optionen für ADFS sind in [ADFS Management](active-directory-aadconnect-federation-management.md#ad-fs-management)dokumentiert.

Wählen Sie eine der Aufgaben, und klicken Sie auf **Weiter** .

> [AZURE.IMPORTANT] Der Installationsassistent geöffnet haben, werden alle Arbeitsgänge in dem Synchronisierungsmodul angehalten. Stellen Sie sicher, dass der Installations-Assistent zu schließen, sobald Sie die geänderte Konfiguration abgeschlossen haben.

## <a name="view-current-configuration"></a>Aktuelle Konfiguration
Diese Option bietet Ihnen einen schnellen Überblick über die aktuell konfigurierten Optionen.

![Seite mit einer Liste aller Optionen und deren Status](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Klicken Sie auf **zurück** , um zurückzukehren. Wenn Sie **Beenden**auswählen, schließen Sie den Installationsassistenten.

## <a name="customize-synchronization-options"></a>Synchronisierungsoptionen anpassen
Diese Option dient der Sync-Konfiguration ändern. Eine Teilmenge der Optionen vom Installationspfad Konfiguration angezeigt. Diese Option wird angezeigt, auch wenn Sie anfänglich express-Installation verwendet.

- [Mehrere Verzeichnisse](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Entfernen eines Verzeichnisses finden Sie unter [Löschen einer Verbindung](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
- [Domäne ändern und Organisationseinheit Filtern](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Entfernen Sie Filter
- [Optionale Features ändern](active-directory-aadconnect-get-started-custom.md#optional-features).

Die Optionen von der ersten Installation nicht geändert werden und sind nicht verfügbar. Diese Optionen sind:

- Ändern Sie das Attribut für UserPrincipalName und SourceAnchor.
- Ändern Sie die beitretenden für Objekte aus anderen Gesamtstruktur.
- Gruppenbasierte Filtern aktivieren.

## <a name="refresh-directory-schema"></a>Verzeichnisschema aktualisieren
Diese Option wird verwendet, wenn Sie das Schema in einer der lokalen geändert haben AD DS-Gesamtstrukturen. Angenommen, Sie möglicherweise Exchange installiert oder einem Schema Windows Server 2012 Gerät aktualisiert. In diesem Fall müssen Sie anweisen, Azure AD mit Schema erneut aus AD DS lesen und seinen Cache aktualisieren. Diese Aktion generiert auch die Synchronisierungsregeln. Wenn Sie das Exchange-Schema so hinzufügen, werden die Konfiguration Synchronisierungsregeln für Exchange hinzugefügt.

Bei dieser Option werden alle Verzeichnisse in der Konfiguration aufgeführt. Behalten die Standardeinstellung und allen Gesamtstrukturen aktualisieren, oder heben Sie die Auswahl einiger.

![Seite mit einer Liste aller Verzeichnisse in der Umgebung](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Staging-Modus konfigurieren
Mit dieser Option können Sie aktivieren und Deaktivieren von staging-Modus auf dem Server. Informationen zu staging-Modus und wie sie genutzt werden kann [Vorgänge](active-directory-aadconnectsync-operations.md#staging-mode)gefunden.

Die Option wird angezeigt, wenn Staging derzeit aktiviert oder deaktiviert ist:  
![Option, die auch den aktuellen Zustand des staging-Modus angezeigt wird](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Um den Zustand zu ändern, wählen Sie diese Option und aktivieren Sie oder deaktivieren Sie das Kontrollkästchen.  
![Option, die auch den aktuellen Zustand des staging-Modus angezeigt wird](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Benutzer-Anmeldung ändern
Diese Option ermöglicht das Kennwort Sync Föderation oder umgekehrt ändern. Sie können **nicht**konfigurieren nicht ändern.

Weitere Informationen zu dieser Option finden Sie unter [Benutzer anmelden](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das Konfigurationsmodell von Azure AD Connect Sync [Verständnis deklarative](active-directory-aadconnectsync-understanding-declarative-provisioning.md)Bereitstellung.

**Themen (Übersicht)**

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

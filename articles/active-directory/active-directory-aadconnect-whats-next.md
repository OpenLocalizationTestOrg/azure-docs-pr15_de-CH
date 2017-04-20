<properties
    pageTitle="Azure AD Connect: Nächste Schritte und Azure AD Connect verwalten | Microsoft Azure"
    description="Erfahren Sie, wie die Standardkonfiguration und Betriebsaufgaben für Azure AD Connect erweitern."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Nächste Schritte und Azure AD Connect verwalten
Folgende sind operative Themen erweiterte die Azure Active Directory verbinden Ihrer Organisation benötigt und anpassen können.  

## <a name="add-additional-sync-administrators"></a>Zusätzliche Synchronisierung Administratoren hinzufügen
Standardmäßig wird nur der Benutzer, die die Installation und die lokalen Administratoren installierte Synchronisierungsmodul verwalten können. Weitere Personen können verwaltet und das Synchronisierungsmodul suchen Gruppe namens ADSyncAdmins auf dem lokalen Server und dieser Gruppe hinzufügen.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Zuweisen von Lizenzen Azure AD Premium und Mobilität für Unternehmen

Nun, dass die Benutzer in die Cloud synchronisiert wurden, müssen sie eine Lizenz zuweisen, damit sie Apps wie Office 365 Cloud loslegen.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Ein Azure AD Premium oder Enterprise Mobility Suite-Lizenz
--------------------------------------------------------------------------------
1. Anmelden bei Azure-Portal als Administrator.
2. Wählen Sie auf der linken Seite **Active Directory**.
3. Doppelklicken Sie auf der Seite Active Directory auf das Verzeichnis, das die Benutzer aktivieren möchten.
4. Wählen Sie am oberen Rand der Verzeichnisseite **Lizenzen**.
5. Wählen Sie auf der Seite Lizenzen Active Directory Premium oder Enterprise Mobility Suite aus und dann auf **zuweisen**.
6. Klicken Sie im Dialogfeld Wählen Sie die Benutzer, die Lizenzen zuweisen möchten und klicken Sie auf das Häkchensymbol zu speichern.


## <a name="verifying-the-scheduled-synchronization-task"></a>Überprüfen die geplante Synchronisierungsaufgabe
Möchten Sie den Status einer Synchronisation überprüfen hierzu Sie Azure-Portal einchecken.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Die geplante Synchronisierungsaufgabe überprüfen
--------------------------------------------------------------------------------
1. Anmelden bei Azure-Portal als Administrator.
2. Wählen Sie auf der linken Seite **Active Directory**.
3. Doppelklicken Sie auf der Seite Active Directory auf das Verzeichnis, das die Benutzer aktivieren möchten.
4. Wählen Sie am oberen Rand der Verzeichnisseite **Verzeichnisintegration**.
5. Unter Integration mit lokalen active Directory die Uhrzeit der letzten Synchronisierung.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Starten einer geplanten Synchronisierungsaufgabe
Benötigen Sie eine Synchronisierungsaufgabe führen hierzu Sie Azure AD Connect-Assistenten erneut ausführen.  Sie müssen Ihre Anmeldeinformationen Azure AD.  Wählen Sie im Assistenten den Vorgang **Anpassen Synchronisierungsoptionen** und auf Weiter durch den Assistenten. Ende daß das **Starten der Synchronisierung als Abschluss der anfängliche Konfiguration** aktiviert ist.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Weitere Informationen zu Azure AD-Verbindung synchronisieren: Planer, siehe [Azure AD verbinden Planer](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Weitere Aufgaben in Azure AD Connect verfügbar
Nach der ersten Installation von Azure AD Connect können Sie immer den Assistenten erneut Kontextmenü Azure AD verbinden Start Seite oder Desktop starten.  Sie werden feststellen, dass durch den Assistenten wieder einige neuen Optionen im Formular zusätzliche Aufgaben enthält.  

Die folgende Tabelle enthält eine Zusammenfassung dieser Aufgaben und eine kurze Beschreibung auf.

![Regel beitreten](./media/active-directory-aadconnect-whats-next/addtasks.png)


Zusätzliche Aufgabe | Beschreibung
------------- | ------------- |
Das ausgewählte Szenario anzeigen  |Können Sie Ihre aktuellen Azure AD Connect-Lösung.  Dazu gehören allgemeine Einstellungen, synchronisierten Verzeichnissen, Synch Settings.
Synchronisierungsoptionen anpassen | Können Sie die aktuelle Konfiguration einschließlich zusätzliche Active Directory-Gesamtstrukturen auf die Konfigurations- oder aktivieren Synchronisierungsoptionen wie Benutzer, Gruppen, Gerät oder Kennwort zurückschreiben.
Stagingmodus aktivieren |  Auf diese Weise können Sie Phase Informationen, die später synchronisiert aber nichts in Azure AD exportiert oder Active Directory.  Dadurch werden die Synchronisationen Vorschau anzeigen, bevor sie auftreten.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

<properties
    pageTitle="Azure AD Connect Health Operationen."
    description="Dieser Artikel beschreibt weitere Vorgänge durchgeführt werden können, nachdem Sie Azure AD Connect Health bereitgestellt haben."
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
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD Connect Health-Operationen

Die folgenden Themen beschreiben die verschiedenen Operationen mit Azure AD Connect Health ausgeführt werden können.

## <a name="enable-email-notifications"></a>E-Mail-Benachrichtigung aktivieren
Sie können Azure AD verbinden Gesundheitswesen Benachrichtigungen senden, wenn Warnungen erzeugt werden, dass Ihre Identitätsinfrastruktur nicht fehlerfrei ist. Dies wird eine Warnung generiert wird, als auch wenn als behoben gekennzeichnet ist. Gehen Sie zum Konfigurieren von e-Mail-Benachrichtigungen.

![Azure AD Connect Health E-Mail Notification entdecken](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] E-Mail-Benachrichtigungen sind standardmäßig deaktiviert.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure AD verbinden Gesundheit e-Mail-Benachrichtigung aktivieren

1. Öffnen Sie das Blade Alarme für den Dienst für den e-Mail-Benachrichtigung erhalten möchten.
2. Klicken Sie auf die Schaltfläche "Benachrichtigung Settings" auf der Aktionsleiste.
3. Schalten Sie die e-Mail-Benachrichtigung auf.
4. Aktivieren Sie das Kontrollkästchen alle globalen Administratoren um e-Mail-Benachrichtigungen konfiguriert.
5. Wenn Sie e-Mail-Benachrichtigungen auf e-Mail-Adressen erhalten möchten, können Sie diese im Feld Weitere e-Mail-Empfänger angeben. Um eine e-Mail-Adresse aus der Liste zu entfernen, klicken Sie mit der rechten Maustaste auf den Eintrag und wählen Sie löschen.
6. Klicken Sie zum Abschließen der ändert sich auf "Speichern". Alle Änderungen treten Effekte erst nach Auswahl von "Speichern".

## <a name="delete-a-server-or-service-instance"></a>Eine Server oder Dienst Instanz löschen

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Löschen eines Servers aus Azure AD Connect Health-Dienst
In einigen Fällen möchten möglicherweise einen Server überwachten entfernen. Gehen Sie zum Entfernen eines Servers von Azure AD Health Service herstellen.

Beim Löschen eines Servers werden Sie die folgenden Punkte beachten:

- Diese Aktion beendet Sammeln von Daten von diesem Server. Dieser Server wird aus der Überwachungsdienst entfernt. Nach dieser Aktion nicht werden neue Alarme anzeigen Überwachung oder Verwendung Analysedaten für diesen Server.
- Diese Aktion wird nicht deinstallieren oder Entfernen der Agent vom Server. Wenn Sie vor diesem Schritt nicht der Agent deinstalliert haben, sehen Sie auf dem Server Health Agents Fehlerereignisse.
- Diese Aktion löscht nicht bereits Daten von diesem Server. Die Daten werden nach Microsoft Azure Data Retention Policy gelöscht.
- Nach dieser Aktion möchten Sie Überwachen von demselben Server erneut starten müssen Sie deinstallieren und neu Installieren des Health Agents auf diesem Server.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>So löschen Sie einen Server aus Azure AD Connect Health-Dienst

Azure AD Connect Health von AD FS und Azure AD (synchronisiert) hergestellt:

1. Geöffnet das Server-Blade aus dem Blade Server Liste werden der Servername entfernt werden.
2. Das Server-Blade klicken Sie auf die Schaltfläche "Löschen" auf der Aktionsleiste.
3. Bestätigen Sie die Aktion zum Löschen des Servers den Servernamen im Bestätigung eingeben.
4. Klicken Sie auf die Schaltfläche "Löschen".

Azure AD Connect Health für AD DS:

1. Öffnen Sie das Dashboard Domänencontroller.
2. Wählen Sie den Domänencontroller entfernt werden.
3. Klicken Sie auf die Schaltfläche "Ausgewählte löschen" auf der Aktionsleiste.
4. Bestätigen Sie die Aktion zum Löschen des Servers.
5. Klicken Sie auf die Schaltfläche "Löschen".

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Löschen Sie eine Instanz von Azure AD Connect Health-Dienst

In einigen Fällen möchten möglicherweise eine Instanz entfernen. Gehen Sie auf eine Instanz von Azure AD Connect Health-Dienst entfernen.

Beim Löschen einer Dienstinstanz werden Sie die folgenden Punkte beachten:

- Diese Aktion entfernt die aktuelle Dienstinstanz aus der Überwachungsdienst.
- Diese Aktion nicht deinstallieren oder Entfernen der Agent vom Server, der diese Dienstinstanz überwacht wurden. Wenn Sie vor diesem Schritt nicht der Agent deinstalliert haben, sehen Sie auf dem Server Health Agents Fehlerereignisse.
- Alle wird von dieser Dienstinstanz nach Microsoft Azure Data Retention Policy gelöscht.
- Nach dem Ausführen dieser Aktion möchten Sie den Dienst Überwachung starten, deinstallieren und neu installieren Sie Health Agent auf allen Servern, die überwacht werden sollen. Nach dieser Aktion möchten Sie Überwachen von demselben Server erneut starten müssen Sie deinstallieren und neu Installieren des Health Agents auf diesem Server.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Eine Instanz von Azure AD Connect Health-Dienst löschen

1. Öffnen Sie das Blatt aus der Liste Blatt auswählen die Service-Identifier (Farmnamen), die Sie entfernen möchten.
2. Das Server-Blade klicken Sie auf die Schaltfläche "Löschen" auf der Aktionsleiste.
3. Bestätigen Sie den Namen im bestätigen eingeben. (Beispiel: sts.contoso.com)
4. Klicken Sie auf die Schaltfläche "Löschen".
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Verwalten mit rollenbasierte Zugriffskontrolle
### <a name="overview"></a>Übersicht
[Rolle basierte Zugriffskontrolle](role-based-access-control-configure.md) für Azure AD Connect Health bietet Azure AD Connect Health Service Zugriff auf Benutzer und/oder Gruppen außerhalb von globalen Administratoren. Dies wird erreicht, indem die vorgesehenen Benutzer und/oder Gruppen Rollen zuweisen und bietet einen Mechanismus zum globalen Administratoren im Verzeichnis einschränken.

#### <a name="roles"></a>Rollen
Azure AD Connect Health unterstützt die folgenden integrierten Funktionen.

| Rolle | Berechtigungen |
| ----------- | ---------- |
| Besitzer | Besitzer können ***Verwalten des Zugriffs*** (z. B. Zuweisen der Rolle einer Benutzergruppe), ***alle Informationen*** (z. B. Ansicht Alarme) aus dem Portal und zu ***Ändern*** (z. B. e-Mail-Benachrichtigungen) in Azure AD Connect Health. <br>Standardmäßig Azure AD globalen Administratoren werden dieser Rolle zugewiesen und kann nicht geändert werden.  |
|Teilnehmer|  Beitragende können ***alle Informationen anzeigen*** (z. B. Ansicht Alarme) aus dem Portal und zu ***Ändern*** (z. B. e-Mail-Benachrichtigungen) in Azure AD Connect Health.|
|Reader| Leser können ***alle Informationen anzeigen*** (z. B. Ansicht Alarme) über das Portal in Azure AD Connect Health.|

Alle anderen Rollen (z. B. "Benutzeradministratoren" oder "DevTest Labs Users"), auch wenn die Portalfunktionalität für haben keinen Einfluss auf in Azure AD Connect Health.

#### <a name="access-scope"></a>Access-Bereich

Azure AD Connect unterstützt das Verwalten des Zugriffs auf zwei Ebenen:

- ***Alle Dienstinstanzen***: Dies ist die empfohlene Vorgehensweise für die meisten Kunden und steuert den Zugriff für alle Instanzen (z. B. eine Farm ADFS) über alle Typen, die von Azure AD Connect Health überwacht werden.

- ***Instanz***: In einigen Fällen möglicherweise Zugriff auf Typen oder eine Dienstinstanz aufteilen. In diesem Fall können Sie Zugriff auf Instanzebene Dienst verwalten.  

Berechtigung wird erteilt, wenn ein Benutzer hat auf Verzeichnis oder Instanz zugreifen.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Wie Benutzer oder Gruppen Zugriff auf Azure AD Connect Health
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Schritt 1: Wählen Sie den entsprechenden Bereich
Um einen Benutzerzugriff auf *alle Instanzen* in Azure AD Connect Health öffnen Sie das Haupt-Blade in Azure AD Connect Health.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Schritt 2: Hinzufügen von Benutzern, Gruppen und Rollen zuweisen
1. Klicken Sie auf die "Benutzer" im Abschnitt konfigurieren.<br>
![Azure AD Connect Health RBAC Main Blade](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Wählen Sie "Hinzufügen"
3. Wählen Sie "Rolle" wie "Owner"<br>
![Azure AD Connect Health RBAC Benutzer hinzufügen](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Geben Sie den Namen oder Bezeichner der Zielgruppen oder Gruppe. Sie können Benutzer oder Gruppen gleichzeitig auswählen. Klicken Sie auf "auswählen".
![Azure AD Connect Health RBAC Benutzer](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Wählen Sie "Ok".<br>

6. Nach Abschluss die Zuweisung der Sicherheitsrolle werden Benutzern oder Gruppen in der Liste angezeigt.<br>
![Azure AD Connect Health RBAC-Benutzerliste](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Diese Schritte ermöglichen aufgeführten Benutzer und Gruppenzugriff nach ihre zugewiesenen Rollen.
>[AZURE.NOTE]
- Globale Administratoren haben vollen Zugriff auf alle Vorgänge immer jedoch globale Administratorkonten nicht in der obigen Liste vorhanden sein.
- Feature "Benutzer einladen" wird in Azure AD Connect Health nicht unterstützt.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Schritt 3: Freigeben Sie Blade-Speicherort für Benutzer oder Gruppen
1. Nach dem Zuweisen von Berechtigungen kann ein Benutzer Azure AD Connect Health auf [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth)zugreifen.
2. Einmal kann auf dem Blatt Benutzer Blade oder Teile der Dashboard angeheftet werden durch Klicken auf "Pin Dashboard"<br>
![Azure AD verbinden Gesundheit RBAC Pin blade](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Ein Benutzer mit der Rolle "Leser" wird zum Ausführen des Vorgangs "create" Azure Marketplace zu Azure AD Connect Health-Erweiterung nicht. Benutzer kann noch auf den Link an die Blade erhalten. Für spätere Verwendung kann der Benutzer das Blade in das Dashboard angeheftet.

### <a name="remove-users-andor-groups"></a>Benutzer oder Gruppen entfernen
Entfernen Sie einen Benutzer oder eine Gruppe durch Rechtsklick und Auswahl entfernen Azure AD verbinden Rolle basiert Zugang Gesundheitskontrolle Teil hinzugefügt.<br>
![Azure AD Connect Health RBAC entfernen Benutzer](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Verwandte links

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Agent-Installation](active-directory-aadconnect-health-agent-install.md)
* [Schließen Sie Gesundheit mit Azure AD mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health für Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Gesundheit in AD DS verbinden über Azure AD](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Version Gesundheitsdaten](active-directory-aadconnect-health-version-history.md)

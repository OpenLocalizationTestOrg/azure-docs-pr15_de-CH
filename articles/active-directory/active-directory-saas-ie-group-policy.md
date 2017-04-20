<properties
    pageTitle="Erweiterung des Bereichs für Internet Explorer mithilfe von Gruppenrichtlinien bereitstellen | Microsoft Azure"
    description="Verwendung von Gruppenrichtlinien Internet Explorer Add-Ons für meine Apps Portal bereitstellen."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Erweiterung des Bereichs für Internet Explorer mithilfe von Gruppenrichtlinien bereitstellen

Dieses Lernprogramm zeigt, wie Gruppenrichtlinien Remote Access Panel-Erweiterung für Internet Explorer auf Ihrem Computer installieren. Diese Erweiterung ist für Benutzer von Internet Explorer, die sich in apps, die mithilfe von [kennwortbasierten einmaliges Anmelden](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)konfiguriert werden.

Es wird empfohlen, dass Administratoren die Bereitstellung dieser Erweiterung zu automatisieren. Andernfalls haben Benutzer herunterladen und Installieren der Erweiterung, die Benutzer fehleranfällig und erfordert Administratorrechte. Dieses Lernprogramm umfasst eine Methode der Automatisierung der softwarebereitstellung von mithilfe von Gruppenrichtlinien. [Mehr zum Thema Gruppenrichtlinien.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Die Abdeckung Erweiterung ist auch für [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) und [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), beide keine Administratorberechtigungen installieren erforderlich sind.

##<a name="prerequisites"></a>Erforderliche Komponenten

- [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)eingerichtet haben und die Domäne haben Ihre Computer hinzugefügt.
- Sie müssen die Berechtigung "Bearbeiten Settings" Bearbeitung von Gruppenrichtlinienobjekten (GPOs). Mitglied der folgenden Sicherheitsgruppen verfügen standardmäßig über diese Berechtigung: Domänenadministratoren, Organisationsadministratoren und Richtlinien-Ersteller-Besitzer. [Weitere Informationen.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Schritt 1: Erstellen der Verteilungspunkt

Zunächst müssen Sie das Installer-Paket auf einem Netzlaufwerk platzieren, die von allen Computern zugegriffen werden kann und Sie Remote die Erweiterung installieren möchten. Gehen Sie hierzu folgendermaßen vor:

1. Melden Sie sich als Administrator auf dem server

2. Klicken Sie im **Server-Manager** werden Sie **Dateien und**.

    ![Geöffnete Dateien und Speicherdienste](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Wechseln Sie zur Registerkarte **Freigaben** . Klicken Sie auf **Aufgaben** > **Neue Freigabe...**

    ![Geöffnete Dateien und Speicherdienste](./media/active-directory-saas-ie-group-policy/shares.png)

4. Führen Sie den **Assistenten für neue Freigaben** und Festlegen von Berechtigungen, um sicherzustellen, dass Ihr Computer darauf zugreifen können. [Erfahren Sie mehr über Aktien.](https://technet.microsoft.com/library/cc753175.aspx)

5. Downloaden Sie das folgende Microsoft Windows Installer-Paket (MSI-Datei): [Access Bereich Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Kopieren Sie das Installer-Paket an die gewünschte Position auf der Freigabe.

    ![Kopieren Sie die MSI-Datei sind die Freigabe.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Überprüfen Sie die Client-Computer auf die Freigabe des Installationspakets zugreifen. 

##<a name="step-2-create-the-group-policy-object"></a>Schritt 2: Erstellen des Gruppenrichtlinienobjekts

1. Melden Sie sich auf dem Server, der die Active Directory-Domänendienste (AD DS) Installation hostet.

2. Im Server-Manager gehen Sie zu **Extras** > **Verwaltung**.

    ![Klicken Sie auf Extras > Gruppe Richtlinien-Management](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. Zeigen Sie im linken Bereich des Fensters **Verwaltung** der Organisationseinheit (OU) Hierarchie an und bestimmen Sie, auf welchen Bereich möchten Sie die Gruppenrichtlinie angewendet. Beispielsweise möchten eine kleine Organisationseinheit Benutzer bereitstellen für Tests auswählen oder Sie können eine Organisationseinheit auf oberster Ebene in der gesamten Organisation bereitstellen wählen.

    > [AZURE.NOTE] Möchten Sie erstellen oder Bearbeiten der Organisationseinheiten (OUs), wechseln Sie wieder in den Server-Manager und **Extras** > **Active Directory-Benutzer und -Computer**.

4. Nach Auswahl eine Organisationseinheit mit der rechten Maustaste darauf und wählen Sie **erstellen ein Gruppenrichtlinienobjekt, und verknüpfen Sie hier...**

    ![Erstellen eines neuen Gruppenrichtlinienobjekts](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. Geben Sie in der Aufforderung **Gruppenrichtlinienobjekt** einen Namen für das neue Gruppenrichtlinienobjekt.

    ![Nennen Sie das neue Gruppenrichtlinienobjekt](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Mit der rechten Maustaste auf das Gruppenrichtlinienobjekt, das Sie gerade erstellt haben, und wählen Sie **Bearbeiten**.

    ![Das neue Gruppenrichtlinienobjekt bearbeiten](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>Schritt 3: Zuweisen des Installationspakets

1. Bestimmen Sie, ob die Erweiterung **Computerkonfiguration** oder **Benutzerkonfiguration**bereitstellen. Bei [Konfiguration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx)wird die Erweiterung auf dem Computer unabhängig von den angemeldeten Benutzer es installiert. Auf der anderen Seite [Benutzerkonfiguration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx)haben Benutzer die Erweiterung installiert Sie unabhängig von dem Computer sie sich anmelden.

2. Gehen Sie im linken Bereich des Fensters **Gruppenrichtlinienverwaltungs-Editor** auf die folgenden Ordnerpfade je nach Konfiguration haben:
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Mit der rechten Maustaste auf **Softwareinstallation**, und wählen Sie **neu** > **Paket...**

    ![Neue Software-Installationspaket erstellen](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Klicken Sie auf den freigegebenen Ordner, der das Installationspaket von enthält [Schritt 1: Erstellen der Verteilungspunkt](#step-1-create-the-distribution-point)wählen Sie die MSI-Datei, und klicken Sie auf **Öffnen**.

    > [AZURE.IMPORTANT] Wenn die Freigabe auf diesem Server befindet, überprüfen Sie Zugriff auf die MSI-Datei über den Netzwerkpfad der Datei statt der lokalen Dateipfad.

    ![Wählen Sie das Installationspaket aus dem freigegebenen Ordner.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. Wählen Sie in der Aufforderung **Software bereitstellen** für Bereitstellungsmethoden **zugewiesen** . Klicken Sie auf **OK**.

    ![Zugewiesen, klicken Sie auf OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

Die Erweiterung ist nun mit der Organisationseinheit bereitgestellt, die Sie ausgewählt. [Erfahren Sie mehr über Gruppenrichtlinien-Softwareinstallation.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Schritt 4: Auto-Enable Erweiterung für InternetExplorer 

Neben den Installer, muss jede Erweiterung für Internet Explorer explizit aktiviert werden, bevor es verwendet werden kann. Schritte Erweiterung des Bereichs mithilfe von Gruppenrichtlinien aktivieren:

1. Im **Gruppenrichtlinienverwaltungs-Editor** -Fenster wechseln Sie zu einem der folgenden Pfade, je nach Konfiguration in haben [Schritt 3: Zuweisen des Installationspakets](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. Maustaste auf **Add-On-Liste**, und wählen Sie **Bearbeiten**.
    ![Bearbeiten Sie Add-On-Liste.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. Wählen Sie im Listenfenster **Add-on** **aktiviert**. Klicken Sie unter **Optionen** **... angezeigt**.

    ![Aktivieren klicken anzeigen...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. Führen Sie im Fenster **Inhalt anzeigen** die folgenden Schritte aus:

    1. Kopieren Sie für die erste Spalte (das Feld **Wertname** ) und fügen Sie die folgenden Klassen-ID ein:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. Die zweite Spalte (das Feld **Wert** ) Geben Sie den folgenden Wert:`1`

    3. Klicken Sie auf **OK** zum Schließen des Fensters **Inhalt anzeigen** .

    ![Füllen Sie die Werte wie oben angegeben.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Klicken Sie auf **OK** , um übernehmen und Schließen der **Add-On-Liste aufgeführt** .

Die Erweiterung sollte jetzt für die Computer in der ausgewählten Organisationseinheit aktiviert werden. [Weitere Informationen zur Verwendung von Gruppenrichtlinien aktivieren oder Deaktivieren von Add-ons für Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>Schritt 5 (Optional): Meldung "Kennwort speichern" deaktivieren

Wenn Benutzer Websites mit Erweiterung des Bereichs anmelden, kann Internet Explorer zeigen folgenden gefragt werden "möchten Sie Ihr Kennwort speichern?"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Möchten Sie verhindern, dass Benutzer diese Meldung sehen, dann gehen Sie zu AutoVervollständigen von Passwörter:

1. Gehen Sie im Fenster **Gruppenrichtlinienverwaltungs-Editor** aufgeführten Pfad. Beachten Sie, dass diese Einstellung nur unter **Benutzerkonfiguration**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. Finden Sie die Einstellung mit dem Namen **AutoVervollständigen - Feature für Benutzernamen und Kennwörter für Formulare aktivieren**.

    > [AZURE.NOTE] Vorherige Versionen von Active Directory können diese Einstellung mit dem Namen **AutoVervollständigen Speichern von Kennwörtern nicht zulassen**aufgeführt. Die Konfiguration dieser Einstellung unterscheidet sich von der Einstellung beschrieben.

    ![Beachten Sie dies unter Benutzerkonfiguration.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. Klicken Sie auf die oben genannte Einstellung, und wählen Sie **Bearbeiten**.

4. Wählen Sie im Fenster **die Funktion AutoVervollständigen für Benutzernamen und Kennwörter für Formulare aktivieren** **deaktiviert**.

    ![Wählen Sie deaktivieren](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Klicken Sie auf **OK** , um sich und das Fenster zu schließen.

Benutzer werden nicht mehr zum Speichern ihrer Anmeldeinformationen oder automatische Vervollständigung verwenden zuvor gespeicherte Anmeldeinformationen zugreifen können. Jedoch erlaubt diese Richtlinie Benutzern von AutoVervollständigen für andere Felder, wie Suchfelder verwenden.

> [AZURE.WARNING] Wenn diese Richtlinie aktiviert ist, nach Benutzer zum Speichern von Anmeldeinformationen, diese Richtlinie wird *nicht* die bereits gespeicherten Anmeldeinformationen löschen.

##<a name="step-6-testing-the-deployment"></a>Schritt 6: Testen der Bereitstellung

Gehen Sie folgendermaßen vor, um festzustellen, ob die Erweiterung Bereitstellung erfolgreich war:

1. Wenn **Computer**Konfiguration bereitgestellt, melden Sie sich bei einem Clientcomputer zur Organisationseinheit, die im ausgewählten gehört [Schritt2: Erstellen Sie das Gruppenrichtlinienobjekt](#step-2-create-the-group-policy-object). Wenn **Benutzer**Konfiguration bereitgestellt wird, müssen Sie als Benutzer anmelden, der Organisationseinheit angehört.

2. Es dauert ein Paare Zeichen ins für die Gruppenrichtlinien vollständig Änderungen Aktualisieren mit diesem Computer. Um die Aktualisierung zu erzwingen, öffnen Sie **ein Eingabeaufforderungsfenster** , und führen Sie den folgenden Befehl:`gpupdate /force`

3. Sie müssen den Computer für die Installation zu starten. Booten dauert wesentlich länger als gewöhnlich bei der Erweiterung installiert.

4. Öffnen Sie **Internet Explorer**nach dem Neustart. Oben rechts im Fenster klicken Sie auf **Tools** (Zahnradsymbol), und wählen Sie **Add-ons verwalten**.

    ![Klicken Sie auf Extras > Add-Ons verwalten](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. Überprüfen Sie im Fenster **Add-ons verwalten** **Bereich Erweiterung** installiert wurde und der **Status** auf **aktiviert**festgelegt wurde.

    ![Überprüfen Sie, ob der Bereich Erweiterung installiert und aktiviert ist.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Problembehandlung bei Erweiterung des Bereichs für InternetExplorer](active-directory-saas-ie-troubleshooting.md)

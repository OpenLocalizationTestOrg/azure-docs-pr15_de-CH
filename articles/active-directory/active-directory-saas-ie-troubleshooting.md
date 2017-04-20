<properties
    pageTitle="Erweiterung des Bereichs für InternetExplorer Problembehandlung | Microsoft Azure"
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

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Problembehandlung bei Erweiterung des Bereichs für InternetExplorer

Diese Artikel helfen Ihnen die folgenden Probleme beheben:

- Sie können Ihre apps mit Internet Explorer über meine Apps-Portal zugreifen.
- Meldung der "Software installieren", obwohl Sie bereits die Software installiert haben.

Wenn Sie Administrator sind, siehe auch: [Erweiterung des Bereichs für Internet Explorer mithilfe von Gruppenrichtlinien bereitstellen.](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Das Diagnoseprogramm auszuführen

Installationsprobleme mit Erweiterung des Bereichs zu diagnostizieren von herunterladen und Abdeckung Diagnoseprogramm ausführen:

1. [Klicken Sie hier, um das Diagnoseprogramm herunterladen.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Öffnen Sie die Datei und **Alle** drücken.

    ![Drücken Sie alle](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Klicken Sie dann auf **extrahieren** möchten.

    ![Drücken Sie extrahieren](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Zum Ausführen des Tools und wählen dann mit der rechten Maustaste der Datei **AccessPanelExtensionDiagnosticTool** **mit > Microsoft Windows-basierten Skripthost**.

    ![Mit > Microsoft Windows Script Host basiert](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Sie sehen dann folgende Diagnosefenster beschreibt, was bei der Installation falsch sein könnte.

    ![Ein Beispiel für das Diagnosefenster](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Klicken Sie auf "**Ja**", um das Programm beheben, die gefunden wurden.

7. Um diese Änderung zu speichern, schließen Sie alle Internet Explorer-Fenster und öffnen Sie Internet Explorer erneut.<br />Sollten Sie weiterhin Ihre apps zugreifen können, die folgenden Schritte aus.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Überprüfen Sie, dass der Bereich Erweiterung aktiviert ist

Um sicherzustellen, dass die Erweiterung des Bereichs in Internet Explorer aktiviert ist:

1. Klicken Sie in Internet Explorer auf das **Zahnradsymbol** rechts oben im Fenster. Wählen Sie **Internetoptionen**aus.<br />(In älteren Versionen von Internet Explorer finden Sie unter **Extras > Internetoptionen**.

    ![Klicken Sie auf Extras > Internetoptionen](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Klicken Sie auf die Registerkarte **Programme** und dann auf die Schaltfläche " **Add-ons verwalten** ".

    ![Klicken Sie auf Add-Ons verwalten](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. Wählen Sie in diesem Dialogfeld **Bereich Erweiterung** und klicken Sie dann auf die Schaltfläche **Aktivieren** .

    ![Klicken Sie auf Aktivieren](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Um diese Änderung zu speichern, schließen Sie alle Internet Explorer-Fenster und öffnen Sie Internet Explorer erneut.

##<a name="enable-extensions-for-inprivate-browsing"></a>Erweiterung für InPrivate-Browsen aktivieren

Wenn Sie den InPrivate-Browsen Modus verwenden:

1. Klicken Sie in Internet Explorer auf das **Zahnradsymbol** rechts oben im Fenster. Wählen Sie **Internetoptionen**aus.<br />(In älteren Versionen von Internet Explorer finden Sie unter **Extras > Internetoptionen**.

    ![Ein Beispiel für das Diagnosefenster](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Gehen Sie auf die Registerkarte **Datenschutz** , dann **Deaktivieren Sie** das Kontrollkästchen **deaktivieren, Symbolleisten und Erweiterungen beim InPrivate-Browsen starten**</p>

    ![Deaktivieren Sie das Kontrollkästchen Sie Disable Symbolleisten und Erweiterungen beim InPrivate-Browsen starten](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Um diese Änderung zu speichern, schließen Sie alle Internet Explorer-Fenster und öffnen Sie Internet Explorer erneut.

##<a name="uninstall-the-access-panel-extension"></a>Deinstallieren Sie die Erweiterung des Bereichs

Die Erweiterung Abdeckung von Ihrem Computer zu deinstallieren:

1. Drücken Sie auf der Tastatur die **Windows-Taste** das Startmenü öffnen. Wenn das Menü geöffnet ist, geben Sie nichts zu suchen. Geben Sie "Control Panel" und dann das **Bedienfeld** in den Suchergebnissen erscheint.

    ![Suche nach Bedienfeld](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. Oben rechts im Steuerelement ändern Sie **Anzeigen nach** Option **große**Symbole. Suchen Sie und klicken Sie auf **Programme und Funktionen** .

    ![Ändern der Ansicht große Symbole anzeigen](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Wählen Sie aus der Liste **Bereich Erweiterung**und klicken Sie auf die Schaltfläche **Deinstallieren** .

    ![Klicken Sie auf Deinstallieren](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Sie können dann versuchen, installieren Sie die Erweiterung erneut, um festzustellen, ob das Problem behoben wurde.

Wenn die Erweiterung deinstallieren Probleme auftreten, können Sie auch mit [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) -Tool entfernen.

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Erweiterung des Bereichs für Internet Explorer mithilfe von Gruppenrichtlinien bereitstellen](active-directory-saas-ie-group-policy.md)

<properties
    pageTitle="Föderation Zertifikate in Azure AD verwalten | Microsoft Azure"
    description="Erfahren Sie, wie das Ablaufdatum für die Föderation Zertifikate angepasst und Zertifikate erneuern, die bald ablaufen."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Verwalten von Zertifikaten für föderierte einmaliges Anmelden in Azure Active Directory

Dieser Artikel behandelt allgemeine Fragen im Zusammenhang mit Zertifikaten Azure Active Directory erstellt, um den Clientanwendungen SaaS verbundenen einmaliges Anmelden (SSO) einrichten.

Dieser Artikel ist nur für apps **Azure AD einmaliges Anmelden**, konfiguriert sind wie im folgenden Beispiel gezeigt:

![Azure AD einmaliges Anmelden](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>Das Ablaufdatum für das Zertifikat Föderation anpassen

Standardmäßig werden Zertifikate auf zwei Jahre festgelegt. Sie können ein anderes Ablaufdatum für das Zertifikat anhand der folgenden Schritte. Diese Schritte können gelten für alle verbundenen SaaS-app enthalten Screenshots verwenden Salesforce Beispiel.

1. Azure Active Directory auf Schnellstart für Ihre Anwendung klicken Sie auf **Configure einmaliges Anmelden**.

    ![Öffnen des SSO-Assistenten.](./media/active-directory-sso-certs/config-sso.png)

2. **Azure AD einmaliges**wählen Sie aus und dann auf **Weiter**.

3. **Zeichen-URL** der Anwendung geben Sie, und aktivieren Sie das Kontrollkästchen für das **Konfigurieren des Zertifikats für föderierte einmaliges Anmelden verwendet**. Klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. Auf der nächsten Seite Wählen Sie **ein neues Zertifikat generieren**und wie lange das Zertifikat gültig sein soll. Klicken Sie auf **Weiter**.

    ![Erstellen Sie ein neues Zertifikat](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Klicken Sie auf **Download des Zertifikats**. Klicken Sie auf **Ansicht Konfigurationshinweise**Informationen zum Zertifikat bestimmten SaaS-app hochladen.

    ![Laden Sie dann das Zertifikat hochladen](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. Zertifikat nicht aktiviert, bis Sie Kontrollkästchen unten im Dialogfeld Bestätigung senden drücken.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Ein Zertifikat erneuern, die bald ablaufen

Die Erneuerung Schritte unten sollte im Idealfall keine erhebliche Ausfallzeiten für Ihre Benutzer führen. Die Screenshots in diesem Abschnitt Salesforce als Beispiel folgendermaßen verwendet können alle verbundenen SaaS-app zuweisen.

1. Azure Active Directory auf Schnellstart für Ihre Anwendung klicken Sie auf **Konfigurieren Sie einmaliges Anmelden**.

    ![Öffnen des SSO-Assistenten](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. Auf der ersten Seite des Dialogfelds **Azure AD Single Sign-On** bereits sollte ausgewählt haben, klicken Sie auf **Weiter**.

3. Aktivieren Sie auf der zweiten Seite das Kontrollkästchen für die **Konfiguration für föderierte einmaliges verwendete Zertifikat**. Klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. Auf der nächsten Seite Wählen Sie **ein neues Zertifikat generieren**und wie lange das neue Zertifikat gültig sein soll. Klicken Sie auf **Weiter**.

    ![Erstellen Sie ein neues Zertifikat](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Klicken Sie auf **Download des Zertifikats**. Um erfolgreich Rewnew Ihr Zertifikat erforderlich die folgenden zwei Schritte:

    - Laden Sie das neue Zertifikat SaaS-app Konfiguration für einzelne Zeichen Bildschirm. Klicken Sie auf **Ansicht Konfigurationshinweise**Informationen für bestimmte SaaS-app.

    - In Azure AD das Kontrollkästchen Sie Bestätigung unten im Dialogfeld das neue Zertifikat aktivieren und dann auf **Weiter** zu senden.

    > [AZURE.IMPORTANT] Einmaliges Anmelden die App deaktiviert einen Moment zwei Schritte abgeschlossen, aber es wird wieder aktiviert werden nach Abschluss der zweite Schritt. Daher, um Ausfallzeiten zu minimieren, bereiten Sie, beide Schritte innerhalb kurzer Zeit voneinander.

    ![Laden Sie dann das Zertifikat hochladen](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Problembehandlung bei SAML-basierte Single Sign-On](active-directory-saml-debugging.md)

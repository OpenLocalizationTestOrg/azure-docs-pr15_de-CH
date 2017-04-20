<properties
    pageTitle="Azure Active Directory B2C: Erweiterbare Richtlinienframework | Microsoft Azure"
    description="Ein Thema, das erweiterbare Richtlinienframework Azure Active Directory B2C und verschiedenen Richtlinientypen erstellen"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Erweiterbare Richtlinienframework

## <a name="the-basics"></a>Die Grundlagen

Das erweiterbare Richtlinienframework B2C Azure Active Directory (Azure AD) ist die Stärke des Dienstes. Richtlinien vollständig beschreiben Verbraucher Identität Erfahrungen oder Anmeldung anmelden Profil bearbeiten. Beispielsweise können mit eine Registrierungsrichtlinie Verhalten steuern, indem Sie Folgendes konfigurieren:

- Kontotypen (soziale wie Facebook oder lokale Konten wie e-Mail-Adresse), mit dem Verbraucher bei der Anwendung anmelden.
- Attribute (z. B. Vorname, Postleitzahl und Schuhgröße) an die Verbraucher bei der Anmeldung.
- Verwendung der kombinierten Authentifizierung.
- Das Aussehen und Verhalten von allen Anmeldeseiten.
- Informationen (was als Ansprüche in einem Token), dass die Anwendung beim Ausführen beendet Richtlinie empfängt.

Sie können mehrere Richtlinien verschiedener in Ihrem Mandanten erstellt und Bedarf in Ihrer Anwendung verwenden. Richtlinien können anwendungsübergreifend wiederverwendet werden. Dadurch können Entwickler definieren und Consumer Identität Erfahrungen mit minimalem oder unverändert ihren Code ändern.

Richtlinien sind über eine einfache Developer-Schnittstelle verfügbar. Die Anwendung löst eine Richtlinie einer standardmäßigen HTTP-Authentifizierung-Anforderung (Richtlinienparameter in der Anforderung übergeben) und benutzerdefinierte Token als Antwort empfängt. Z. B. der einzige Unterschied zwischen fordert eine Registrierungsrichtlinie aufrufen und Aufrufen einer Richtlinie ist der Richtlinienname in dem "p" Abfragezeichenfolgen-Parameter verwendet:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Weitere Informationen über das Richtlinienframework anzeigen Sie diesen [Blog-Eintrag](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx)

## <a name="create-a-sign-up-policy"></a>Erstellen einer Registrierungsrichtlinie

Aktivieren Sie in der Anwendung anmelden, müssen Sie eine Registrierungsrichtlinie erstellen. Diese Richtlinie beschreibt, die Verbraucher bei der Anmeldung werden durch Erfahrung und den Inhalt von Token, die die Anwendung erfolgreich Anmeldungen erhalten.

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung**.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Der **Name** bestimmt den Registrierungsrichtlinie von der Anwendung verwendet. Geben Sie beispielsweise "SiUp".
5. **Identitätsprovider** und wählen Sie "Email registrieren". Optional, können sozialen Identitätsanbieter bereits konfiguriert. Klicken Sie auf **OK**.
6. Klicken Sie auf **Anmeldung**. Hier wählen Sie Attribute, die vom Consumer während des Anmeldevorgangs sammeln möchten. Wählen Sie beispielsweise "Land", "Display Name" und "Postleitzahl". Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie die Ansprüche im Token gesendet, um die Anwendung nach einer erfolgreichen Anmeldung zurückgegeben werden sollen. Wählen Sie beispielsweise "Display Name", "Identitätsanbieter", "Postleitzahl", "Benutzer ist neu" und "Des Benutzers Objekt-ID".
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SiUp**" (die **B2C\_1\_ ** Fragment automatisch hinzugefügt) Blatt **Anmeldung Richtlinien** .
9. Öffnen Sie die Richtlinie auf "**B2C_1_SiUp**".
10. Wählen Sie "Contoso B2C app" in der Dropdownliste **Applikationen** und `https://localhost:44321/` in der **Antwort-URL / URI umleiten** Dropdown-.
11. Klicken Sie auf **jetzt ausführen**. Eine neue Registerkarte geöffnet, und führen Sie das Kundenerlebnis der Anmeldung für die Anwendung.

    > [AZURE.NOTE]
    Es dauert bis zu einer Minute für die Erstellung von Richtlinien und Updates wirksam.

## <a name="create-a-sign-in-policy"></a>Erstellen einer Richtlinie

Um Anmeldung in der Anwendung zu aktivieren, müssen Sie eine Richtlinie erstellen. Diese Richtlinie beschreibt die Verbraucher durchlaufen anmelden und den Inhalt des Tokens die Anwendung erhält auf erfolgreiche Anmeldungen Erfahrungen.

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmelden**.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Der **Name** bestimmt den von der Anwendung verwendeten anmelden. Geben Sie beispielsweise "SiIn".
5. **Identitätsanbieter** auf und wählen Sie "Konto-Anmeldung". Optional, können sozialen Identitätsanbieter bereits konfiguriert. Klicken Sie auf **OK**.
6. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie die Ansprüche im Token gesendet, um die Anwendung nach einer erfolgreichen Anmeldung zurückgegeben werden sollen. Wählen Sie beispielsweise "Display Name", "Identitätsanbieter", "PLZ" und "Des Benutzers Objekt-ID". Klicken Sie auf **OK**.
7. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SiIn**" (die **B2C\_1\_ ** Fragment automatisch) im Blade **- in Policies** .
8. Öffnen Sie die Richtlinie auf "**B2C_1_SiIn**".
9. Wählen Sie "Contoso B2C app" in der Dropdownliste **Applikationen** und `https://localhost:44321/` in der **Antwort-URL / URI umleiten** Dropdown-.
10. Klicken Sie auf **jetzt ausführen**. Eine neue Registerkarte geöffnet, und führen Sie das Kundenerlebnis in Ihre Anwendung signieren.

    > [AZURE.NOTE]
    Es dauert bis zu einer Minute für die Erstellung von Richtlinien und Updates wirksam.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Erstellen einer Richtlinie anmelden oder Einloggen

Diese Richtlinie behandelt sowohl Consumer Anmeldung & Anmeldung Erfahrungen mit einer einzelnen Konfiguration. Verbraucher werden auf dem richtigen Weg (Anmeldung oder Anmeldung) kontextabhängige geführt. Außerdem wird der Inhalt von Token, die die Anwendung bei erfolgreicher Anmeldungen oder Anmeldungen erhalten beschrieben.  Ein Codebeispiel für die Anmeldung oder Anmeldung Richtlinie ist [verfügbar](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **anmelden oder Einloggen**.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Der **Name** bestimmt den Registrierungsrichtlinie von der Anwendung verwendet. Geben Sie beispielsweise "SiUpIn".
5. **Identitätsprovider** und wählen Sie "Email registrieren". Optional, können sozialen Identitätsanbieter bereits konfiguriert. Klicken Sie auf **OK**.
6. Klicken Sie auf **Anmeldung**. Hier wählen Sie Attribute, die vom Consumer während des Anmeldevorgangs sammeln möchten. Wählen Sie beispielsweise "Land", "Display Name" und "Postleitzahl". Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie die Ansprüche im Token gesendet, um die Anwendung nach einer erfolgreichen Anmeldung oder Anmeldung zurückgegeben werden sollen. Wählen Sie beispielsweise "Display Name", "Identitätsanbieter", "Postleitzahl", "Benutzer ist neu" und "Des Benutzers Objekt-ID".
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SiUpIn**" (die **B2C\_1\_ ** Fragment automatisch hinzugefügt) **anmelden oder Einloggen Richtlinien** Blatt.
9. Öffnen Sie die Richtlinie auf "**B2C_1_SiUpIn**".
10. Wählen Sie "Contoso B2C app" in der Dropdownliste **Applikationen** und `https://localhost:44321/` in der **Antwort-URL / URI umleiten** Dropdown-.
11. Klicken Sie auf **jetzt ausführen**. Eine neue Registerkarte geöffnet, und führen Sie Benutzerfunktionen Anmeldung oder Anmeldung konfiguriert.

    > [AZURE.NOTE]
    Es dauert bis zu einer Minute für die Erstellung von Richtlinien und Updates wirksam.

## <a name="create-a-profile-editing-policy"></a>Erstellen eines Profils Richtlinie bearbeiten

Um Profil bearbeiten in der Anwendung zu aktivieren, müssen Sie ein Profil bearbeiten Richtlinie erstellen. Diese Richtlinie beschreibt die Verbraucher durchlaufen Profil bearbeiten und den Inhalt von Token, die die Anwendung bei erfolgreichem Abschluss erhalten Erfahrungen.

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Profil Bearbeiten von Richtlinien**.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Der **Name** bestimmt das Profil von der Anwendung verwendeten Richtlinienname bearbeiten. Geben Sie beispielsweise "SiPe".
5. **Identitätsanbieter** auf, und wählen Sie "E-Mail". Optional, können sozialen Identitätsanbieter bereits konfiguriert. Klicken Sie auf **OK**.
6. Klicken Sie auf **Profil**. Hier wählen Sie Attribute, die der Verbraucher anzeigen und bearbeiten kann. Wählen Sie beispielsweise "Land", "Display Name" und "Postleitzahl". Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie die Ansprüche im Token zurück an die Anwendung nach einer erfolgreichen Bearbeitung Profil gesendet zurückgegeben werden sollen. Wählen Sie beispielsweise "Display Name" und "Postleitzahl".
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SiPe**" (die **B2C\_1\_ ** Fragment automatisch hinzugefügt) Blatt **Profil Bearbeiten von Richtlinien** .
9. Öffnen Sie die Richtlinie auf "**B2C_1_SiPe**".
10. Wählen Sie "Contoso B2C app" in der Dropdownliste **Applikationen** und `https://localhost:44321/` in der **Antwort-URL / URI umleiten** Dropdown-.
11. Klicken Sie auf **jetzt ausführen**. Eine neue Registerkarte geöffnet, und führen Sie das Profil Consumer Erfahrung in der Anwendung.

    > [AZURE.NOTE]
    Es dauert bis zu einer Minute für die Erstellung von Richtlinien und Updates wirksam.
    
## <a name="create-a-password-reset-policy"></a>Erstellen einer Kennwortrichtlinie zurücksetzen

Um abgestimmte Kennwortrücksetzdiskette in der Anwendung zu aktivieren, müssen Sie eine Kennwortrichtlinie zurücksetzen erstellen. Beachten Sie, dass der Mieter Wide angegebene Option Passwort [hier](active-directory-b2c-reference-sspr.md) gilt weiterhin für-in Richtlinien. Diese Richtlinie beschreibt Funktionen, die der Verbraucher werden durch das Zurücksetzen von Kennwörtern und den Inhalt von Token, die die Anwendung bei erfolgreichem Abschluss erhalten.

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Kennwort zurücksetzen Richtlinien**.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Der **Name** bestimmt Passwort Richtlinienname von der Anwendung verwendet. Geben Sie beispielsweise "SSPR".
5. **Identitätsprovider** und wählen Sie "Passwort e-Mail-Adresse". Klicken Sie auf **OK**.
6. Klicken Sie auf **Anwendung Ansprüche**. Hier wählen Sie die Ansprüche im Token zurück an die Anwendung gesendet wird, nachdem eine erfolgreiche Erfahrung Passwort zurückgegeben werden sollen. Wählen Sie beispielsweise "Des Benutzers Objekt-ID".
7. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als "**B2C_1_SSPR**" (die **B2C\_1\_ ** Fragment automatisch hinzugefügt) Blatt **Passwort Richtlinien** .
8. Öffnen Sie die Richtlinie auf "**B2C_1_SSPR**".
9. Wählen Sie "Contoso B2C app" in der Dropdownliste **Applikationen** und `https://localhost:44321/` in der **Antwort-URL / URI umleiten** Dropdown-.
10. Klicken Sie auf **jetzt ausführen**. Eine neue Registerkarte geöffnet, und führen Sie Kennwort zurücksetzen Benutzerfunktionen in Ihrer Anwendung.

    > [AZURE.NOTE]
    Es dauert bis zu einer Minute für die Erstellung von Richtlinien und Updates wirksam.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Token, Sitzung und Konfiguration für einzelne Zeichen](active-directory-b2c-token-session-sso.md).

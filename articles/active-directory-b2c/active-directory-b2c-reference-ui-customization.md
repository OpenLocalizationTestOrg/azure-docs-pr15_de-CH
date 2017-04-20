<properties
    pageTitle="Azure Active Directory B2C: Anpassen der Benutzeroberfläche (UI) | Microsoft Azure"
    description="Auf die Anpassungsfeatures der Benutzeroberfläche (UI) in Azure Active Directory B2C"
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
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Anpassen der Azure AD B2C-Benutzeroberfläche (UI)

Benutzer ist ein kundenorientierter Anwendung. Es ist der Unterschied zwischen einer gut und ein sowie zwischen nur aktive Verbraucher und wirklich engagiert. Azure Active Directory (Azure AD) B2C können Verbraucher Anmeldung anmelden (*Siehe Hinweis unten*) Profil bearbeiten, anpassen und Passwort mit pixelgenaue Steuerung.

> [AZURE.NOTE]
Derzeit Seiten lokales Konto anmelden Seiten begleitende Kennwort zurücksetzen und Überprüfung e-Mails können nur mithilfe des [Unternehmens branding-Funktion](../active-directory/active-directory-add-company-branding.md) und nicht in diesem Artikel beschriebenen Verfahren angepasst werden.

In diesem Artikel lesen Sie:

- Die Seite Benutzer Benutzeroberfläche Anpassungsfunktion.
- Ein Tool, mit dem Sie testen Seite Anpassung Benutzeroberflächenfunktion mit unseren Beispielinhalte.
- Die Core-Benutzeroberflächenelemente in den einzelnen Seite.
- Optimale diese Funktion ausüben.

## <a name="the-page-ui-customization-feature"></a>Die Seite Anpassung Benutzeroberflächenfunktion

Benutzeroberflächenfunktion Anpassung der Seite können Sie das Erscheinungsbild des Verbrauchers Anmeldung, Anmeldung, Kennwort zurücksetzen und das Profil bearbeiten (indem Sie [Richtlinien](active-directory-b2c-reference-policies.md)konfigurieren) Seiten. Ihre Kunden haben nahtlose Erfahrung beim Navigieren zwischen Ihrer Anwendung und Seiten von Azure AD B2C bedient.

Im Gegensatz zu anderen Diensten, Benutzeroberflächenoptionen begrenzt oder nur über APIs, nutzt Azure AD B2C eine moderne (und einfachere) Anpassung der Benutzeroberfläche.

Es funktioniert: Azure AD B2C führt Code im Browser des Consumers und verwendet modernen Ansatz [Cross-Ursprung Ressource freigeben (CORS)](http://www.w3.org/TR/cors/) Inhalt von einer URL geladen, die in einer Richtlinie angeben. Sie können verschiedene URLs für unterschiedliche Seiten angeben. Der Code führt Benutzeroberflächenelemente von Azure AD B2C mit dem Inhalt von der URL geladen und zeigt die Seite an den Consumer. Sie müssen lediglich:

1. Erstellen wohlgeformten HTML5 mit einer `<div id="api"></div>` Element (muss ein leeres Element) irgendwo der `<body>`. Dieses Element markiert, Azure AD B2C-Inhalt eingefügt wird.
2. Hosten von Inhalten auf einer HTTPS-Endpunkt (mit CORS zulässig).
3. Formatieren der Benutzeroberflächenelemente, denen in Azure AD B2C einfügt.

## <a name="test-out-the-ui-customization-feature"></a>Testen Sie die Benutzeroberflächenfunktion Anpassung

Ggf. Anpassung Benutzeroberflächenfunktion Testen mit Beispiel HTML und CSS-Inhalt haben wir Sie [einfache Hilfsprogramm](active-directory-b2c-reference-ui-customization-helper-tool.md) bereitgestellt, uploads Beispielinhalte in Azure Blob-Speicher konfiguriert.

> [AZURE.NOTE]
Sie hosten Ihre Benutzeroberfläche Inhalte überall: auf Webservern CDNs, AWS S3 Freigabe Dateisysteme usw.. Solange der Inhalt auf einer öffentlich zugänglichen HTTPS-Endpunkt (mit CORS zulässig) gehostet wird, sind Sie startklar. Nur zur Veranschaulichung verwenden wir Azure BLOB-Speicher.

### <a name="the-most-basic-html-content-for-testing"></a>Grundlegende HTML-Inhalt für Tests

Unten wird der grundlegende HTML-Inhalte, die Sie verwenden können, um diese Funktion zu testen. Verwenden Sie dieselbe Hilfsprogramm zur zuvor hochladen und diese Inhalte im Azure BLOB-Speicher konfigurieren. Sie können dann überprüfen, einfache, nicht stilisierten Schaltflächen & Formularfelder auf jeder Seite angezeigt werden und funktionsfähig sind.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Core-UI-Elemente in den einzelnen Seite

In den folgenden Abschnitten finden Sie Beispiele für HTML5-Fragmente, die in Azure AD B2C führt die `<div id="api"></div>` Element im Inhalt gefunden. **Legen Sie diese Fragmente nicht in den Inhalt der HTML 5.** Azure AD B2C-Dienst fügt sie zur Laufzeit. Verwenden Sie diese Beispiele verwendete Stylesheets entwerfen.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure AD B2C Inhalte in der "Identität Anbieter Auswahlseite"

Diese Seite enthält eine Liste Identitätsanbieter, die der Benutzer bei der Anmeldung oder Anmeldung auswählen können. Dies sind sozialen Identitätsanbieter wie Facebook und Google oder lokale Konten (basierend auf e-Mail-Adresse oder Benutzer).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure AD B2C Inhalte in "lokales Konto Anmeldeseite"

Diese Seite enthält ein Anmeldeformular, die der Benutzer ausfüllen, wenn für ein lokales Konto, das auf eine e-Mail-Adresse oder einen Benutzernamen. Das Formular kann anderen Eingabesteuerelemente Texteingabefeld Kennworteingabefeld, Optionsfeld, Einfachauswahl Dropdown-Listenfelder und mehreren Kontrollkästchen enthalten.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure AD B2C-Inhalt "" soziale Konto Anmeldeseite"eingefügt

Diese Seite enthält ein Anmeldeformular, die der Verbraucher ausfüllen, wenn Sie mit einem vorhandenen Konto von sozialen Identitätsanbieter wie Facebook oder Google. Diese Seite ähnelt lokales Konto Anmeldeseite (siehe vorherigen Abschnitt) mit Ausnahme der Kennwortfelder.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure AD B2C Inhalte in Unified anmelden oder Einloggen Seite""

Diese Seite behandelt Anmeldung und der Verbraucher Identität wie Facebook Google oder lokale Konten Provider anmelden.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure AD B2C Inhalte auf der Seite"mehrstufige Authentifizierung"

Auf dieser Seite können Benutzer ihre Telefonnummern (mit Text oder Stimme) während der Anmeldung oder Anmeldung überprüfen.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure AD B2C Inhalte in "Fehlerseite" "

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Dinge beim eigene Inhalte erstellen

Wenn Sie Seite Anpassung Benutzeroberflächenfunktion verwenden, überprüfen Sie die folgenden best Practices:

- Nicht kopieren Sie Azure AD B2C-Standardinhalt und versuchen Sie, es zu ändern. Es wird empfohlen, den HTML5-Inhalt neu erstellen und Standardinhalt als Referenz verwenden.
- Alle die Seiten (außer Fehlerseiten) anmelden, Anmeldung und Richtlinien Profil bearbeiten, Stylesheets, die Sie bereitstellen müssen die Standardformatvorlagen überschreiben, die wir in diesen Seiten hinzufügen der <head> Fragmente. Auf allen Seiten der Anmeldung oder Anmeldung zurücksetzen Kennwortrichtlinien und Fehlerseiten auf alle Richtlinien bereitgestellt, haben alle Stile zu sich selbst.
- Aus Sicherheitsgründen können nicht wir Sie JavaScript in den Inhalt enthalten. Die meisten Sie müssen sollten vorhanden sein. Wenn dies nicht der Fall ist, verwenden [Benutzer Voice](http://feedback.azure.com/forums/169401-azure-active-directory) neue Funktionen anfordern.
- Unterstützte Browser-Versionen:
    - InternetExplorer 11
    - Internet Explorer 10
    - Internet Explorer 9 (begrenzt)
    - Internet Explorer 8 (begrenzt)
    - Google Chrome 43,0
    - Google Chrome 42,0
    - Mozilla Firefox 38,0
    - Mozilla Firefox 37,0

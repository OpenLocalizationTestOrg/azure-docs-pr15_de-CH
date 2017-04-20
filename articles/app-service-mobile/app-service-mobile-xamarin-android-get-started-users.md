<properties
    pageTitle="Erste Schritte mit Authentifizierung für Mobile Apps in Xamarin Android"
    description="Erfahren Sie, wie Mobile Apps zum Authentifizieren von Benutzern der Anwendung Xamarin Android mithilfe verschiedener Identitätsanbieter wie AAD, Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Authentifizierung Ihrer Anwendung Xamarin.Android hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Thema veranschaulicht die Authentifizierung von Benutzern eine Mobile Anwendung in der Clientanwendung. In diesem Lernprogramm wird die Authentifizierung mithilfe von Azure Mobile Apps unterstützt Identitätsanbieter Schnellstart-Projekt hinzufügen. Authentifiziert und autorisiert in die Mobile-Anwendung erfolgreich, wird der Benutzer-ID-Wert angezeigt.

Dieses Lernprogramm basiert auf Schnellstart Mobile-Anwendung. Sie müssen auch das Lernprogramm [Xamarin.Android app erstellen]. Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, müssen Sie das Erweiterung Authentifizierungspaket zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register"></a>Registrieren Sie Ihre Anwendung für die Authentifizierung und konfigurieren Anwendungsdienste

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Schränken Sie die Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Führen Sie in Visual Studio oder Xamarin Studio Clientprojekt auf einem Gerät oder Emulator. Stellen Sie sicher, dass eine nicht behandelte Ausnahme mit dem Statuscode 401 (nicht autorisiert) nach dem Starten der Anwendung ausgelöst wird. Dies geschieht, da die Anwendung versucht, Mobile App Backend als nicht authentifizierter Benutzer zugreifen. Die Tabelle *TodoItem* benötigt nun Authentifizierung.

Danach aktualisieren Sie die Clientanwendung auf Anforderung Ressourcen vom Back-End Mobile-Anwendung mit einem authentifizierten Benutzer.

##<a name="add-authentication"></a>Authentifizierung für die Anwendung hinzufügen

Die Anwendung aktualisiert Benutzer die Schaltfläche **Anmelden** und authentifizieren, bevor Daten angezeigt werden.

1. Der **TodoActivity** -Klasse den folgenden Code hinzufügen:

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Dies erstellt eine neue Methode zum Authentifizieren Benutzer und Methodenhandler für eine neue Schaltfläche **Anmelden** . Der im obigen Codebeispiel wird Benutzerauthentifizierung mit Facebook anmelden. Ein Dialogfeld zum einmal authentifizierte Benutzer-ID anzeigen.

    > [AZURE.NOTE] Wenn ein Identitätsprovider als Facebook ändern, **LoginAsync** , eine der folgenden übergebenen Wert: _MicrosoftAccount_, _Twitter_, _Google_oder _WindowsAzureActiveDirectory_.

3. Löschen Sie, oder Auskommentieren der folgenden Codezeile, in der **OnCreate** -Methode:

        OnRefreshItemsSelected ();

4. Fügen Sie in der Datei Activity_To_Do.axml die folgende *LoginUser* Schaltfläche Definition vor vorhandenen *AddItem* -Schaltfläche hinzu:

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Fügen Sie das folgende Element in die Ressourcendatei Strings.xml:

        <string name="login_button_text">Sign in</string>

6. In Visual Studio oder Xamarin Studio das Clientprojekt auf einem Gerät oder Emulator ausgeführt und der gewählten Identitätsanbieter anmelden.

    Wenn Sie sich erfolgreich angemeldet haben, zeigt die Anwendung Ihre Anmelde-ID und die Liste der Aufgaben und können Sie Updates auf die Daten.


<!-- URLs. -->
[Erstellen einer Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md

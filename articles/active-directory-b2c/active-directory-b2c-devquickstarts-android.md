<properties
    pageTitle="Azure Active Directory B2C: Rufen Sie Web-API eine Android Anwendung | Microsoft Azure"
    description="In diesem Artikel erfahren Sie, wie Android "Aufgabenliste" erstellt app die OAuth 2.0 trägertoken mit Node.js Web API aufgerufen. Die Android und Web-API verwenden Azure Active Directory B2C Benutzeridentitäten verwalten und Benutzer authentifizieren."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C: Rufen Sie Web-API eine Android Anwendung

> [AZURE.WARNING] Dieses Lernprogramm erfordert einige wichtige Updates speziell für die Verwendung von ADAL Android B2C entfernen.  Werden wir neue Gebrauchsanweisung Azure AD B2C in apps in der nächsten Woche veröffentlichen und empfehlen wir Sie bis dahin.  Aber wenn Sie Dinge versuchen möchten, die folgenden Artikel fortsetzen.



Fügen Sie mithilfe von Azure Active Directory (Azure AD) B2C leistungsstarke Self-service-Management bietet Ihre apps und Web-APIs in wenigen Schritten. In diesem Artikel erfahren Sie, wie Android "Aufgabenliste" erstellt app die OAuth 2.0 trägertoken mit Node.js Web API aufgerufen. Die Android und Web-API verwenden Azure AD B2C Benutzeridentitäten verwalten und Benutzer authentifizieren.

Dieser Schnellstart erfordert eine Web-API, die von Azure AD mit B2C voll geschützt. Wir haben für .NET und Node.js Verwendung erstellt. Diese exemplarische Vorgehensweise wird vorausgesetzt, dass Node.js Web API Beispiel konfiguriert ist. Weitere Informationen finden Sie in [Azure AD B2C Web API Node.js-Lernprogramm](active-directory-b2c-devquickstarts-api-node.md).

Android-Clients, die Zugriff auf geschützte Ressourcen bietet Azure AD Active Directory Authentifizierung Library (ADAL). Zweck der ADAL ist Ihre app Zugriffstoken zu erleichtern. Veranschaulicht, wie einfach es ist, in diesem Handbuch werden wir Android Aufgabe listenanwendung erstellen, die:

- Ruft Zugriffstoken, die Vorgangsliste API-Aufrufen über das [Authentifizierungsprotokoll OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
- Benutzer Vorgangslisten ruft.
- Benutzer meldet.

> [AZURE.NOTE] In diesem Artikel behandelt nicht zum Implementieren, anmelden, Anmeldung und Verwaltung von Profilen mithilfe von Azure AD B2C. Der Schwerpunkt liegt wie Web-APIs aufrufen, nachdem der Benutzer authentifiziert ist. Wenn nicht bereits geschehen, sollten Sie lernen Sie die Grundlagen von Azure AD B2C mit [.NET Web app Einführung](active-directory-b2c-devquickstarts-web-dotnet.md) beginnen.

## <a name="get-an-azure-ad-b2c-directory"></a>Ein Azure AD B2C-Verzeichnis abrufen

Vor der Verwendung Azure AD B2C müssen Sie ein Verzeichnis oder Mieter. Ein Verzeichnis ist ein Container für alle Benutzer, apps und Gruppen. Haben Sie eine bereits, [B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie fortfahren, in diesem Handbuch.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine Anwendung in Ihrem B2C-Verzeichnis erstellen. Dies informiert Azure AD, um die sichere Kommunikation mit Ihrer Anwendung. Die app und Web-API werden durch eine einzelne **Anwendung ID** in diesem Fall dargestellt da bestehen aus einer logischen Anwendung. [Gehen folgendermaßen Sie vor, um eine Anwendung zu erstellen.](active-directory-b2c-app-registration.md) Achten Sie darauf,:

- **WebApp**enthalten/**Web-API** in der Anwendung.
- Geben Sie `urn:ietf:wg:oauth:2.0:oob` als **Antwort-URL**. Es ist die Standard-URL für dieses Codebeispiel.
- Erstellen Sie einen **geheimen Schlüssel** für Ihre Anwendung, und kopieren Sie sie. Sie werden ihn später benötigen. Beachten Sie, dass dieser Wert [XML mit Escapezeichen versehen](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) werden, bevor Sie sie verwenden.
- Kopieren Sie die **ID der Anwendung** , die Ihre Anwendung zugewiesen ist. Sie auch benötigen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen von Richtlinien

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese Anwendung enthält drei Identität Erfahrungen: anmelden, anmelden und mit Facebook anmelden.  Sie müssen eine Richtlinie jedes Typs erstellen gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wenn Sie die drei Richtlinien erstellen, müssen Sie:

- Wählen Sie in der Registrierungsrichtlinie **Anzeigename** und andere Anmeldung Attribute.
- Ansprüche Anwendung **angezeigten Namen** und die **Objekt-ID** in jeder Richtlinie auswählen Sie können auch andere Ansprüche.
- Kopieren Sie den **Namen** jeder Richtlinie nach ihrer Erstellung. Das Präfix muss `b2c_1_`.  Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellen, können Sie Ihre app erstellen.

Beachten Sie, dass dieser Artikel nicht wie mit Richtlinien, die Sie gerade erstellt haben. Informationen über die Funktionsweise von Richtlinien in Azure AD B2C mit [.NET Web app Einführung](active-directory-b2c-devquickstarts-web-dotnet.md)beginnen.

## <a name="download-the-code"></a>Den Code herunterladen

Der Code für dieses Lernprogramm [auf GitHub verwaltet](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android). Zum Beispiel wie Sie, können Sie [das Skelette Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Sie können auch das Skelett Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Sie müssen das Skelett um dieses Lernprogramm zu downloaden.** Aufgrund der Komplexität eine voll funktionsfähige Android-Anwendung implementieren hat das Skelett UX-Code, der ausgeführt wird, nachdem Sie dieses Lernprogramm abgeschlossen haben. Dies ist eine zeitsparende für Entwickler. UX-Code ist nicht für das Thema B2C Android Anwendung hinzu.

Die abgeschlossene Anwendung wird auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) oder die `complete` dasselbe Repository-Zweig.

Um Maven zu erstellen, können Sie `pom.xml` auf der obersten Ebene.

  1. Folgen Sie den Schritten im [Abschnitt erforderliche Maven für Android einrichten](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
  2. Richten Sie einen Emulator SDK 21.
  3. Wechseln Sie in das Stammverzeichnis, in dem das Repo geklont.
  4. Führen Sie den Befehl `mvn clean install`.
  5. Wechseln Sie im Schnellstart-Beispiel `cd samples\hello`.
  6. Führen Sie den Befehl `mvn android:deploy android:run`.

Anwendung starten sollte angezeigt werden. Geben Sie Anmeldeinformationen des Testbenutzers um es auszuprobieren.

Java-Archiv (JAR) Pakete werden auch neben Android Archiv (Tätigkeitsbericht) Paket gesendet.

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Android ADAL downloaden und Android Studio Arbeitsbereich hinzufügen

Sie haben Optionen zum projektinternen Android-Bibliothek verwenden:

* Den Quellcode können Sie die Bibliothek Eclipse und Verknüpfung der Anwendung importieren.
* Wenn Android Studio verwenden, können Sie Paketformat Tätigkeitsbericht verwenden und Binärdateien verweisen.

### <a name="option-1-binaries-via-gradle-recommended"></a>Option 1: Binärdateien über Gradle (empfohlen)

Sie erhalten die Binärdateien aus dem zentralen Maven Repo. Tätigkeitsbericht Paket in Ihrem Projekt Android Studio enthalten sein (z. B. in `build.gradle`) folgendermaßen:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>Option 2: Tätigkeitsbericht über Maven

Verwenden Sie die `m2e` in Eclipse-Plug-in, Sie können die Abhängigkeit in der `pom.xml` Datei:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>Option 3: Quelle über Git (letzter Ausweg)

Um den Quellcode des SDK über Git abzurufen, geben Sie Folgendes ein:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Verwenden Sie den Zweig **Konvergenz.**

## <a name="set-up-your-configuration-file"></a>Konfigurationsdatei einrichten

Die Konfiguration, die Sie einrichten, früher B2C-Portal Android Projekt konfigurieren.

Open `helpes/Constants.java` und geben Sie Werte für die folgenden:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: Die Bereiche, die Sie an den Server übergeben, die Sie vom Server, wenn ein Benutzer möchten anmeldet. Übergeben Sie für die Vorschau B2C `client_id`. Allerdings soll diese Änderung `read scopes` in der Zukunft. Dieses Dokument wird aktualisiert, wenn vorhanden.
- `ADDITIONAL_SCOPES`: Weitere Bereiche, die Sie für Ihre Anwendung verwenden möchten. Sie sollen verwendet werden.
- `CLIENT_ID`: Die-ID aus dem Portal haben.
- `REDIRECT_URL`Die Umleitung, erwartet das Token zurückgesendet werden.
- `EXTRA_QP`: Etwas möchten in eine URL-codierte Format an den Server übergeben.
- `FB_POLICY`Die Richtlinie, die Sie aufrufen. Dies ist der wichtigste Teil dieser exemplarischen Vorgehensweise.
- `EMAIL_SIGNIN_POLICY`Die Richtlinie, die Sie aufrufen. Dies ist der wichtigste Teil dieser exemplarischen Vorgehensweise.
- `EMAIL_SIGNUP_POLICY`Die Richtlinie, die Sie aufrufen. Dies ist der wichtigste Teil dieser exemplarischen Vorgehensweise.

## <a name="add-references-to-android-adal-to-your-project"></a>Fügen Sie dem Projekt Verweise auf Android ADAL

> [AZURE.NOTE]  ADAL für Android verwendet ein Modell Absicht basierende Authentifizierung aufrufen. Absichten legen"" app zu arbeiten. Diese gesamte Beispiel und alle ADAL für Android, auf Verwaltung Absichten und Informationen zwischen ihnen übergeben.

Erzählen Sie Android zuerst das Layout der Anwendung einschließlich Absichten verwenden möchten. Diese Absichten werden später in diesem Lernprogramm ausführlich erläutert.

Aktualisieren Sie das Projekt `AndroidManifest.xml` Datei alle Ihre Absichten:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Wie Sie sehen können, definieren Sie fünf Aktivitäten. Sie verwenden alle.

- `AuthenticationActivity`: Stammt aus ADAL und stellt die Webansicht anmelden.
- `LoginActivity`: Zeigt die Richtlinien und die Schaltflächen für jede Richtlinie.
- `SettingsActivity`Sie können die Anwendung zur Laufzeit ändern.
- `AddTaskActivity`Sie können die REST-API, die von Azure AD geschützt sind Aufgaben hinzufügen.
- `ToDoActivity`: Dies ist die wichtigste Aktivität, die Aufgaben angezeigt.

## <a name="create-the-sign-in-activity"></a>Erstellen der Aktivität

Erstellen einer Hauptaktivität und rufen sie `LoginActivity`.

Erstellen Sie eine Datei namens `LoginActivity.java`.

Sie müssen die Aktivität Initialisieren und einige Schaltflächen hinzufügen, die die Benutzeroberfläche steuern. Dies ist vertraut, wenn Android Code geschrieben haben.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Sie haben jetzt die Schaltflächen Aufrufen der `ToDoActivity` Absicht (die ADAL aufruft, wenn einen Token benötigt). Sie tun dies mit Ihrer Aktivität einen zusätzlichen Parameter als Referenz. Dieser zusätzliche Parameter durch Übergeben der `intent.putExtra()` Methode. Definieren Sie `"thePolicy"` mit Angabe in `Constants.java`. Dadurch dem Zweck der Richtlinie während der Authentifizierung aufgerufen.

## <a name="create-the-settings-activity"></a>Settings-Aktivität erstellen

Dies ist eine Aktivität, die Ihre Einstellungsbenutzeroberfläche füllt.

Erstellen Sie eine Datei namens `SettingsActivity.java` für einfaches erstellen, lesen, aktualisieren und löschen böte.

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>Aufgabe hinzufügen-Aktivität erstellen

Diese Aktivität können die REST API-Endpunkt eine Aufgabe hinzu.

Erstellen Sie eine Datei namens `AddTaskActivity.java` und schreiben.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>To-Do-Liste Aktivität erstellen

Dies ist die wichtigste Aktivität. Sie können es ein Token von Azure AD für eine Richtlinie zu beschäftigen, und verwenden Tokens Aufgabe REST API-Server aufrufen.

Erstellen Sie eine Datei namens `ToDoActivity.java` und schreiben. (Die Aufrufe werden weiter unten erläutert.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Möglicherweise haben Sie bemerkt, dass diese Methoden abhängig, die noch geschrieben haben. Dazu gehören `updateLoggedInUser()`, `clearSessionCookie()`, und `getTasks()`. Schreiben Sie die in diesem Handbuch. Sie können die Fehler in Android Studio vorerst ignorieren.

Erläuterung der Parameter:

  - `SCOPES`: Erforderlich, die Bereiche Zugriff angefordert werden soll. B2C-Vorschau ist dies identisch mit `client_id`, aber voraussichtlich in der Zukunft ändern.
  - `POLICY`Die Richtlinie für den Benutzer authentifizieren möchten.
  - `CLIENT_ID`: Kommt in Azure AD-Portal erforderlich.
  - `redirectUri`Kann als Ihren Paketnamen eingerichtet werden. Es ist nicht erforderlich, werden die `acquireToken` aufrufen.
  - `getUserInfo()`: Die Möglichkeit zu sehen, ob ein Benutzer bereits im Cache. Der Parameter beschrieben, wie einen Benutzer aufgefordert, wenn diese Benutzer nicht gefunden wird oder Zugriffstoken des Benutzers ungültig ist. Diese Methode wird in diesem Handbuch geschrieben.
  - `PromptBehavior.always`: Abfrage der Anmeldedaten Cache und Cookies zu überspringen können.
  - `Callback`: Aufgerufen, nachdem ein Autorisierungscode für ein Token ausgetauscht werden. Sie haben ein Objekt `AuthenticationResult`, das Zugriffstoken, Ablaufdatum und ID-Informationen enthält.

> [AZURE.NOTE]  Die Windows Intune Unternehmensportal-app bietet die Broker-Komponente und kann auf dem Gerät des Benutzers installiert werden. Die Anwendung Zugriff einzelne anmelden (SSO) über alle Programme auf dem Gerät. Entwickler sollten Windows Intune zu vorbereitet. ADAL für Android verwendet das brokerkonto ist ein Benutzerkonto in der Authentifikator erstellt. Verwendung den Broker die Entwickler sich eine besondere `redirectUri` für Broker verwenden. `redirectUri`wird im Format msauth://packagename/Base64UrlencodedSignature. Sie erhalten `redirectUri` für Ihre Anwendung mithilfe des Skripts `brokerRedirectPrint.ps1` oder API-Aufruf `mContext.getBrokerRedirectUri()`. Die Signatur bezieht sich auf die Signaturzertifikate Google Play Store.

 Sie können Benutzer Broker mit überspringen:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Um diese B2C-Schnellstart zu vereinfachen, haben wir uns entschieden, um überspringen Vermittler in unserem Beispiel.

Anschließend erstellen Sie Hilfsmethoden, die das Token während der Authentifizierung Task-API-Aufrufe nur abrufen.

In der gleichen `ToDoActivity.java` Datei, schreiben.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Auch Methoden, die "set" und "get" hinzufügen `AuthenticationResult` (hat Ihr Token) zum `Constants`. Obwohl `ToDoActivity.java` verwendet `sResult` in die Abläufe müssen diese Methoden hinzufügen. Anderenfalls haben Ihre Aktivitäten keinen Zugriff auf das Token arbeiten (z. B. Hinzufügen einer Aufgabe im `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>Erstellen Sie eine Methode zum Zurückgeben einer Benutzer-ID

ADAL für Android stellt den Benutzer aus einer `UserIdentifier` Objekt. Dieses Objekt verwaltet den Benutzer. Das Objekt können Sie ermitteln, ob derselbe Benutzer Anrufe verwendet wird. Mithilfe dieser Informationen können Sie verlassen sich auf den Cache, anstatt neue rufen Sie den Server. Um dies zu erleichtern, erstellt es die `getUserInfo()` -Methode gibt `UserIdentifier`. Sie können mit `acquireToken()`. Auch ein `getUniqueId()` -Methode, die die ID des `UserIdentifier` im Cache.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Hilfsmethoden schreiben

Schreiben Sie einige Hilfsmethoden zum Löschen von Cookies und bieten `AuthenticationCallback`. Diese Methoden werden ausschließlich für das Beispiel verwendet, um sicherzustellen, dass Sie sauber, beim Aufrufen sind der `ToDo` Aktivitäten.

In der gleichen Datei namens `ToDoActivity.java`, schreiben.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Task-API-aufrufen

Nachdem Sie Ihre Aktivität bereit Token abgerufen haben, können Sie Ihre API Zugriff auf den Taskserver schreiben.

`getTasks`Stellt ein Array, das die Aufgaben auf dem Server darstellt.

Beginnen Sie mit `getTasks`.

In der gleichen Datei namens `ToDoActivity.java`, schreiben.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Außerdem schreiben Sie eine Methode, die bei der ersten Ausführung Tabellen initialisieren.

In der gleichen Datei namens `ToDoActivity.java`, schreiben.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Sie können sehen, dass dieser Code einige zusätzlichen Methoden erfordert arbeiten. Schreiben Sie diese weiter.

### <a name="create-an-endpoint-url-generator"></a>Einen Endpunkt-URL-Generator erstellen

Sie müssen die Endpunkt-URL generieren, die eine Verbindung herstellen. Dazu in die gleiche Klassendatei.

**In der gleichen Datei** `ToDoActivity.java`, schreiben.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Beachten Sie, dass die Anforderung im Code im nächsten Abschnitt erläutert das Zugriffstoken hinzugefügt.

## <a name="write-some-ux-methods"></a>Einige UX Schreibmethoden

Android erfordert einige Rückrufe Betrieb die Anwendung zu behandeln. Sind `createAndShowDialog` und `onResume()`. Dies ist vertraut, wenn Android Code geschrieben haben.

In der gleichen Datei namens `ToDoActivity.java`, schreiben.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Als Nächstes verwalten Sie die Dialogfeld-Rückrufe.

In der gleichen Datei namens `ToDoActivity.java`, schreiben.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Sie haben jetzt eine `ToDoActivity.java` Datei kompiliert. Das gesamte Projekt sollten auch zu diesem Zeitpunkt kompilieren.

## <a name="run-the-sample-app"></a>Führen Sie die Beispiel-app

Schließlich erstellen Sie, und führen Sie die app in Android Studio oder Eclipse. Oder sich in der Anwendung anmelden. Erstellen von Tasks für den angemeldeten Benutzer. Melden Sie ab und melden Sie als anderer Benutzer an und erstellen Sie Aufgaben für diesen Benutzer.

Beachten Sie, dass die Aufgaben gespeicherte pro Benutzer auf die API da die API die Identität des Benutzers aus dem Zugriffstoken extrahiert erhaltenen.

Zu Referenzzwecken vollständiges Beispiel [als ZIP-Datei bereitgestellt wird](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip). Sie können auch von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Wichtige Informationen


### <a name="encryption"></a>Verschlüsselung

ADAL verschlüsselt Token und Speicher in `SharedPreferences` standardmäßig. Können die `StorageHelper` -Klasse, um die Details anzuzeigen. Sichere Speicherung privater Schlüssel **AndroidKeyStore für 4.3(API18)** , Android eingeführt. ADAL verwendet, API18 und höher. Wenn Sie ADAL für niedrigere SDK-Versionen verwenden möchten, müssen Sie bieten einen geheimen Schlüssel an `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Sitzungscookies in der Webansicht

Android Webansicht deaktivieren Sitzungscookies nicht, nachdem die Anwendung geschlossen wird. Sie können dies mit dem folgenden Beispielcode behandeln.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[Weitere Informationen zu Cookies](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

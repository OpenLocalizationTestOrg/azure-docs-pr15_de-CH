<properties
    pageTitle="Aktivieren von SSO Cross-app für iOS mit ADAL | Microsoft Azure"
    description="Verwendung der Features ADAL SDK SSO in Ihrer Anwendung aktivieren. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Aktivieren von SSO Cross-app für iOS mit ADAL


Die Single Sign-On (SSO) damit Benutzer nur einmal seine Anmeldeinformationen eingeben und diese Anmeldeinformationen automatisch arbeiten Applikationen wird erwartet von Kunden. Die Schwierigkeit bei der Eingabe von Benutzername und Kennwort auf einem kleinen Bildschirm, häufig Zeiten mit hinzu (2FA) wie ein Telefonanruf oder SMS Code kombiniert schnell Unzufriedenheit Benutzer Treffern bei mehreren für Ihr Produkt.

Wenn Sie eine Identitätsplattform, die andere Programme wie Microsoft Accounts oder ein Geschäftskonto aus Office365 verwenden nutzen, erwarten Kunden darüber hinaus, dass die Anmeldeinformationen über ihre Anwendung unabhängig vom Kreditor verwendet werden.

Die Plattform Microsoft Identity sowie unsere SDKs Microsoft Identity erledigt all diese Arbeit für Sie und gibt Ihnen die Möglichkeit, Ihre Kunden mit SSO in eigene Anwendungssuite begeistern und unsere Broker-Funktion und Authentifikator Applikationen über das Gerät.

In dieser exemplarischen Vorgehensweise erfahren Sie konfigurieren unserer SDK in die Anwendung diese nutzen für Ihre Kunden.

In dieser exemplarischen Vorgehensweise gilt für:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Bedingte Azure Active Directory-Zugriff


Beachten Sie, dass die folgenden ausgegangen Sie wissen [Bereitstellung Applikationen im legacy-Portal für Azure Active Directory](./develop/active-directory-how-to-integrate.md) sowie die Anwendung [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc)integriert haben.

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO-Konzepte der Plattform Microsoft Identity

### <a name="microsoft-identity-brokers"></a>Microsoft Identity Broker

Microsoft bietet für jede Plattform, die zur Überbrückung von Anmeldeinformationen anwendungsübergreifend von verschiedenen Anbietern ermöglichen sowie spezielle erweiterte Features, die eine einzelne Vorrichtung auf Anmeldeinformationen erfordern kann. Wir bezeichnen diese **Broker**. Auf IOS- und Android diese über herunterladbare Programme erhalten Kunden separat installieren oder eine Firma, die einige oder alle der Geräte für ihre Mitarbeiter verwaltet auf dem Gerät abgelegt werden können. Diese Broker unterstützen Verwalten von Sicherheit für einige Programme oder das gesamte Gerät basierend auf was IT-Administratoren wünschen. In Windows wird diese Funktion durch ein Konto Auswahl des Betriebssystems bezeichnet, technisch als Web-Authentifizierung integriert bereitgestellt.

Verstehen, wie diese Broker und wie Ihre Kunden sie ihre Anmeldung Fluss für Weitere Informationen Microsoft Identity-Plattform sehen können.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Muster für die Anmeldung auf mobilen Geräten

Zugriff auf die Anmeldeinformationen auf Geräten führen Sie zwei grundlegende Muster für die Plattform Microsoft Identity:

* Broker nicht unterstützte Benutzernamen
* Broker-unterstützte Benutzernamen

#### <a name="non-broker-assisted-logins"></a>Broker nicht unterstützte Benutzernamen

Broker nicht unterstützte Logins sind Login-Erfahrung, die mit der Anwendung auftreten und lokalen Speicher auf dem Gerät für die Anwendung. Dieser Speicher kann anwendungsübergreifend freigegeben, aber die Anmeldeinformationen sind eng mit der app oder Suite Apps mithilfe dieser Anmeldeinformationen. Dies ist die wahrscheinlich in vielen mobilen erlebt haben, geben Sie Benutzernamen und Kennwort in der Anwendung selbst.

Diese Benutzernamen haben folgende Vorteile:

-  Benutzer ist vollständig in der Anwendung vorhanden.
-  Anmeldeinformationen können anwendungsübergreifend freigegeben werden, die von demselben Zertifikat bietet eine einzelne Anmeldung an Ihrem Anwendungssuite signiert sind.
-  Steuerelement um die Erfahrung der Anmeldung erfolgt vor und nach dem anmelden.

Diese Benutzernamen haben folgende Nachteile:

- Benutzer kann nicht Single Sign-On für alle apps auftreten, Microsoft Identity nur über die Microsoft Identities verwenden, die Ihre Anwendung besitzt und konfiguriert haben.
- Die Anwendung kann nicht mit erweiterte Business-Funktionen wie bedingte oder InTune-Produktsuite verwenden.
- Die Anwendung kann nicht Zertifikatauthentifizierung für Unternehmen unterstützt.

Hier ist eine Darstellung der Funktionsweise von Microsoft Identity SDKs mit gemeinsam genutztem Speicher der Anwendung zum Aktivieren von SSO:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Broker-unterstützte Benutzernamen

Broker unterstützt Logins sind Login-Erfahrung, die innerhalb der Broker-Anwendungdes und die Speicherung und Sicherheit von Broker Anmeldeinformationen über alle Programme auf dem Gerät freigeben, die Microsoft Identity-Plattform nutzen. Dies bedeutet, dass Ihrer Anwendung Benutzer bei der Anmeldung für den Broker verlassen. Auf IOS- und Android diese über herunterladbare Programme erhalten Kunden separat installieren oder eine Firma, die das Gerät für ihre Benutzer verwaltet auf dem Gerät abgelegt werden können. Ein Beispiel für diese Anwendung ist die Anwendung Azure Authenticator IOS. In Windows wird diese Funktion durch ein Konto Auswahl des Betriebssystems bezeichnet, technisch als Web-Authentifizierung integriert bereitgestellt.
Die Erfahrung variiert je nach Plattform und kann Benutzern unterbrechungsfreie nicht richtig verwaltet. Sie kennen wahrscheinlich die dieses Muster die Facebook Anwendung installiert und Facebook Anmeldefunktionalität in einer anderen Anwendung verwendet. Microsoft Identity-Plattform nutzt das gleiche Muster.

Für iOS führt dies zu einem "Übergang" kommt Animation, wohin die Anwendung im Hintergrund bei Azure Authenticator-Applikationen, in den Vordergrund der Benutzer Konto wählen sie anmelden möchten.  

Konto Auswahl wird für Android und Windows auf die Anwendung ist weniger störend für den Benutzer.

#### <a name="how-the-broker-gets-invoked"></a>Wie der Broker aufgerufen wird

Kompatibler Broker auf dem Gerät Azure Authenticator-Anwendung installiert wird Microsoft Identity SDKs automatisch erledigen die Arbeit Broker für Sie aufrufen, wenn ein Benutzer gibt an, dass sie ein Konto von der Plattform Microsoft Identity anmelden möchten. Möglicherweise persönliche Microsoft Account ein oder schulkonto oder ein Konto, das Sie bereitstellen und Host in Azure unsere B2C und B2B-Produkte. Mit äußerst sicheren Algorithmen und Verschlüsselung stellen wir sicher, dass die Anmeldeinformationen angefordert und auf sichere Weise an die Anwendung übermittelt. Die genaue technische Einzelheiten dieser Mechanismen nicht veröffentlicht, aber wurden mit Apple und Google.

**Der Entwickler hat die Wahl zwischen Microsoft Identity SDK ruft Broker oder persönlichen-Broker-Fluss.** Entscheidet sich der Entwickler nicht mit den Fluss Broker unterstützt verlieren sie jedoch den Vorteil der Nutzung der SSO-Anmeldeinformationen, der Benutzer möglicherweise bereits auf dem Gerät hinzugefügt und verhindert, dass ihre Anwendung mit Business-Funktionen bietet Microsoft seinen Kunden wie Zugangskontrolle Intune Verwaltungsfunktionen sowie Authentifizierung zertifikatbasierte.

Diese Benutzernamen haben folgende Vorteile:

-  SSO Benutzererlebnis über ihre Anwendung unabhängig vom Kreditor.
-  Die Anwendung kann erweiterte Business-Funktionen wie bedingte nutzen oder InTune-Produktsuite.
-  Die Anwendung kann Zertifikatauthentifizierung für geschäftliche Benutzer unterstützen.
- Broker Anwendung zusätzliche Algorithmen mit Verschlüsselung sicherer Anmelden als Identität für die Anwendung und der Benutzer überprüft.

Diese Benutzernamen haben folgende Nachteile:

- In iOS ist der Benutzer Anmeldeinformationen gewählt werden aus der Anwendung gewechselt.
- Verlust des Anmeldeverfahrens für Ihre Kunden innerhalb Ihrer Anwendung verwalten.



Hier ist eine Darstellung der Funktionsweise von Microsoft Identity SDKs mit der Broker zum Aktivieren von SSO:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Anhand dieser Informationen können Sie sollten besser verstehen und Implementieren von SSO in der Anwendung mit den Microsoft Identity Platform SDKs.


## <a name="enabling-cross-app-sso-using-adal"></a>Cross-app-SSO mithilfe von ADAL aktivieren

Wir verwenden hier die ADAL iOS SDK auf:

- Einschalten-Broker unterstützt SSO für apps-suite
- Aktivieren der Unterstützung für SSO Broker unterstützt

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>SSO für nicht-Broker einschalten unterstützt SSO

Unterstützte SSO-Broker anwendungsübergreifend verwalten Microsoft Identity SDKs noch SSO für Sie. Dabei finden den richtigen Benutzer im Cache und eine Liste der angemeldeten Benutzer Abfragen.

Zum Aktivieren von SSO anwendungsübergreifend besitzen, müssen Sie die folgenden Schritte ausführen:

1. Sicherzustellen, dass Ihre Anwendung Benutzer dieselbe Client-ID oder ID Anwendung.
* Sicherstellen Sie, dass alle Anwendung dasselbe Zertifikat signieren von Apple freigeben Freigabe Schlüsselbunde
* Fordern Sie den gleichen Schlüssel Anspruch für jede Anwendung.
* Informieren Sie Microsoft Identity SDKs über freigegebene Schlüsselbund verwenden möchten.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Verwenden dieselbe Client-ID / Anwendung ID für alle Programme in der Suite Apps

Für die Plattform Microsoft Identity zu wissen, dass zulässige Token Anwendung freigeben müssen jede Anwendung die gleichen Client-ID oder ID Anwendung. Dies ist der eindeutige Bezeichner für Sie bereitgestellt wurde, wenn Sie Ihre erste Anwendung im Portal registriert.

Sie sich vielleicht, wie Sie verschiedene apps mit dem Dienst Microsoft Identity identifiziert verwendet die gleiche Anwendung-ID. Die Antwort ist mit den **URIs umleiten**. Jede Anwendung kann mehrere umleiten URIs im Onboarding-Portal registriert haben. Jede Anwendung in der Suite haben eine andere URI-Umleitung. Nachfolgend ein Beispiel dieses:

App1-Umleitung URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 URI-Umleitung:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 Umleitung URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Diese verschachtelt unter demselben Client-ID / ID der Anwendung und die Umleitung URI zu uns in der SDK-Konfiguration zurück abhängig.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Beachten Sie, dass das Format dieser URIs umleiten erläutert. URI umleiten können, wenn die Broker unterstützen soll in diesem Fall müssen sie solche aussehen*



#### <a name="create-keychain-sharing-between-applications"></a>Erstellen Sie Schlüsselbund zwischen Applikationen

Aktivieren der Freigabe Schlüsselbund den Rahmen dieses Dokuments sprengen und unter Apple in ihrem Dokument [Funktionen hinzufügen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Wichtig ist, dass Sie entscheiden Schlüsselbund aufgerufen werden und diese Funktion daran.

Wenn Sie haben Berechtigungen richten Sie richtig sollte finden Sie unter einer Datei im Projektverzeichnis Anspruch `entitlements.plist` enthält etwa wie folgt aussieht:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Sobald Sie Schlüsselbund Ansprüche in jede Anwendung aktiviert, und Sie SSO verwenden können, sagen Microsoft Identity SDK Schlüsselbund mithilfe der folgenden Einstellung in der `ADAuthenticationSettings` mit der folgenden Einstellung:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Wenn Sie einen Schlüsselbund Anwendung freigeben kann jeder Anwendung löschen oder schlimmer alle Token der Anwendung löschen. Dies ist besonders fatal haben Sie Applikationen, die auf die Token Hintergrund arbeiten. Einen Schlüsselbund Freigabe entfernen bedeutet, dass Sie in alle sehr vorsichtig sein müssen durch Microsoft Identity SDKs.

Das wars! Microsoft Identity SDK nutzen jetzt Anmeldeinformationen für Ihre Anwendung. Die Benutzerliste wird auch Instanzen gemeinsam genutzt werden.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Aktivieren der SSO für Broker unterstützt SSO

Für eine Anwendung jegliche Broker, die auf dem Gerät installiert ist **standardmäßig deaktiviert**. Um Ihre Anwendung mit der müssen Sie einige zusätzliche Konfiguration und Ihrer Anwendung Code hinzufügen.

Die Schritte sind:

1. Aktivieren Sie Broker-Modus in den Anwendungscode Aufruf MS SDK.
2. Eine neue URI-Umleitung einrichten und die Anwendung und Ihre app-Registrierung bereit.
3. Ein URL-Schema registriert.
4. iOS9 unterstützt: die Datei "Info.plist" eine Berechtigung hinzufügen.


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Schritt 1: Aktivieren Sie Broker-Modus in der Anwendung
Die Möglichkeit für die Anwendung den Broker ist beim Erstellen der "Kontext" oder Erstinstallation des Objekts Authentifizierung aktiviert. Hierzu Ihre Anmeldeinformationstyp im Code festlegen:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
Die `AD_CREDENTIALS_AUTO` Einstellung ermöglicht Microsoft Identity SDK versuchen der Broker Aufrufen `AD_CREDENTIALS_EMBEDDED` verhindert Microsoft Identity SDK Broker aufrufen.

#### <a name="step-2-registering-a-url-scheme"></a>Schritt 2: URL-Schema registrieren
Die Plattform Microsoft Identity verwendet URLs Broker und anschließend die Steuerung an die Anwendung zurückzugeben. Diese Schleife abschließend benötigen Sie ein URL-Schema für die Plattform Microsoft Identity wissen Anwendung registriert. Dies ist neben anderen app-Schemas, die Sie möglicherweise bereits mit der Anwendung registriert sind.

> [AZURE.WARNING]
Wir empfehlen das URL-Schema minimiert die Wahrscheinlichkeit von einer anderen Anwendung mit dem gleichen URL relativ eindeutig. Apple erzwingt keine Eindeutigkeit der URL-Schemas, die im Store registriert sind.

Es folgt ein Beispiel, wie dies in der Projektkonfiguration angezeigt wird. Sie können auch XCode sowie dazu:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Schritt 3: Erstellen einer neuen umleitungs URI mit URL-Schema

Um sicherzustellen, dass wir immer an die richtige Anwendung Anmeldeinformationen Token zurückgegeben, müssen wir sicherstellen, dass wir Ihre Anwendung zurückmelden, die das Betriebssystem iOS überprüfen können. IOS-Betriebssystem meldet Microsoft Broker Clientanwendungen die Paket-ID der Anwendung aufrufen. Dies kann nicht durch eine schädigende Anwendung gefälscht sein. Daher nutzen wir diese mit URI unsere Broker Anwendung Token an die Anwendung zurückgegeben werden. Wir müssen eindeutige umgeleitet URI sowohl in Ihrer Anwendung und als URI umleiten in unserer Entwicklerportal.

Die URI-Umleitung muss die richtige Form des:

`<app-scheme>://<your.bundle.id>`

ex: *X-Msauth-mytestiosapp://com.myapp.mytestapp*

Dieser URI umleiten muss in Ihre app-Registrierung mithilfe von [Azure-Verwaltungsportal](https://manage.windowsazure.com/)angegeben werden. Weitere Informationen zu Azure AD app-Registrierung finden Sie unter [Integration mit Active Directory Azure](./develop/active-directory-how-to-integrate.md).


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Schritt 3a: hinzufügen eine Umleitung URI im Portal app und Entwickler zur Unterstützung der Authentifizierung Zertifikat

Cert Unterstützung basierte Authentifizierung muss eine zweite "Msauth" in der Anwendung und der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) Zertifikatauthentifizierung behandeln möchten, unterstützen in der Anwendung registriert werden.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Schritt 4: iOS9: einen Konfigurationsparameter für Ihre Anwendung hinzufügen

ADAL verwendet – CanOpenURL: überprüft, ob der Broker auf dem Gerät installiert ist. In iOS dedizierten 9 Apple was Schemas eine Anwendung abgefragt werden können. Müssen hinzufügen "Msauth" im Abschnitt LSApplicationQueriesSchemes der `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>Msauth</string>
</array>

### <a name="youve-configured-sso"></a>SSO konfiguriert haben.

Jetzt Microsoft Identity SDK automatisch sowohl Anmeldeinformationen für Ihre Anwendung freigeben und Broker aufzurufen, wenn auf dem Gerät vorhanden ist.

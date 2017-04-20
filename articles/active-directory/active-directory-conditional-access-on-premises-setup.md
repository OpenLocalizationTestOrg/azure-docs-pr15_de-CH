<properties
    pageTitle="Lokal bedingten Zugriff mit Azure Active Directory Geräteregistrierung | Microsoft Azure"
    description="Eine schrittweise Anleitung zum bedingten Zugriff auf lokale Anwendung mit Active Directory-Verbunddienst (AD FS) in Windows Server 2012 R2 ermöglichen."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>Lokal Zugriff bedingten mit Azure Active Directory Geräteregistrierung

Geräte der Benutzer können persönlich bekannt für Ihre Organisation die Benutzer Arbeitsplatz Join ihre Geräte mit dem Registrierungsdienst Azure Active Directory Gerät gekennzeichnet werden. Es folgt eine schrittweise Anleitung zum bedingten Zugriff auf lokale Anwendung mit Active Directory-Verbunddienst (AD FS) in Windows Server 2012 R2 ermöglichen.

> [AZURE.NOTE]
> Office 365-Lizenz oder Azure AD Premium-Lizenz ist erforderlich, wenn Geräte in Azure Active Directory Geräteregistrierung Richtlinien für bedingten Zugriff registriert. Dazu gehören Richtlinien für lokale Ressourcen von Active Directory-Verbunddienste (AD FS) erzwungen.

Weitere Informationen zu bedingten Zugriffsszenarien lokale finden Sie unter [Arbeitsplatz aus alle Geräte für SSO und nahtlose zweite Faktor Authentifizierung über Unternehmen beitreten](https://technet.microsoft.com/library/dn280945.aspx).

Diese Funktionen stehen Kunden, die eine Azure Active Directory Premium-Lizenz erwerben.

<a name="supported-devices"></a>Unterstützte Geräte
-------------------------------------------------------------------------
* Windows 7 Domäne Geräte.
* Windows 8.1 Personal und Domäne verbunden Geräte.
* iOS 6 und höher für Safari-browser
* Android 4.0 oder höher, Samsung GS3 oder über Telefone, Samsung Note2 oder über Tablets.


<a name="scenario-prerequisites"></a>Szenario-Komponenten
------------------------------------------------------------------------
* Abonnement für Office 365 oder Azure Active Directory Premium
* Ein Mieter Azure Active Directory
* Windows Server Active Directory (WindowsServer 2008 oder höher)
* Aktualisierte Schema in Windows Server 2012 R2
* Lizenz für Azure Active Directory Premium
* Windows Server 2012 R2 Federation Services für SSO in Azure AD konfiguriert
* Windows Server 2012 R2 Web Application Proxy Microsoft Azure Active Directory verbinden (Azure AD verbinden). [Hier herunterladen Azure AD verbinden](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Domäne überprüft.

<a name="known-issues-in-this-release"></a>Bekannte Probleme in dieser Version
-------------------------------------------------------------------------------
* Gerät bedingte Richtlinien erforderlich Gerät Objekt Zurückschreiben in Active Directory von Azure Active Directory. Es dauert bis zu 3 Stunden Gerät Objekte in Active Directory geschrieben zurück.
* iOS 7 Geräten fordert immer Benutzer ein Zertifikat während Clientzertifikatsauthentifizierung auswählen.
* Einige Versionen von iOS8, bevor iOS 8.3 nicht funktionieren.

## <a name="scenario-assumptions"></a>Annahmen im Szenario
Dieses Szenario wird vorausgesetzt, dass Sie eine hybridumgebung bestehend aus Azure AD-Mandanten und eine lokale Active Directory. Diese Mieter mit Azure AD-Verbindung verbunden werden sollen und mit einer Domäne überprüft und ADFS für SSO. Die folgende Checkliste hilft Ihnen beschriebenen Phase Ihrer Umgebung konfigurieren.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Prüfliste: Voraussetzung für bedingten Zugriff
--------------------------------------------------------------
Verbinden Sie Azure AD-Mandanten mit lokalen Active Directory.

## <a name="configure-azure-active-directory-device-registration-service"></a>Gerät-Registrierungsdienst Azure Active Directory konfigurieren
Verwenden Sie dieses Handbuch zum Bereitstellen und Konfigurieren von Azure Active Directory Gerät-Registrierungsdienst für Ihre Organisation.

Dieses Handbuch setzt voraus, dass Windows Server Active Directory konfiguriert haben und Microsoft Azure Active Directory abonniert haben. Siehe obigen Komponenten.

Zum Bereitstellen von Azure Active Directory Gerät-Registrierungsdienst mit Ihrem Mandanten Azure Active Directory die Aufgaben Sie in der folgenden Prüfliste in der Reihenfolge. Wenn ein Verweis zum Thema hierbei, zurück zu dieser Prüfliste nach dem Thema mit den restlichen Aufgaben in dieser Prüfliste fortfahren überprüfen. Einige Aufgaben umfassen einen Validierungsschritt Szenario, mit denen Sie bestätigen, dass der Schritt erfolgreich abgeschlossen wurde.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Teil 1: Aktivieren Azure Active Directory Geräte-Registrierung

Führen Sie die Prüfliste unten aktivieren und konfigurieren den Registrierungsdienst Azure Active Directory Gerät.

| Aufgabe                                                                                                                                                                                                                                                                                                                                                                                             | Referenz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Registrierung des Geräts in Ihrem Mandanten Azure Active Directory Geräte an den Arbeitsplatz zu aktivieren. Standardmäßig ist die kombinierte Authentifizierung für den Dienst nicht aktiviert. Mehrstufige Authentifizierung wird jedoch empfohlen, wenn ein Gerät zu registrieren. Sicherzustellen Sie vor der Aktivierung der kombinierten Authentifizierung in Adr, dass AD FS für eine kombinierte Authentifizierung konfiguriert ist. | [Azure Active Directory Geräteregistrierung aktivieren](active-directory-conditional-access-device-registration-overview.md)               |
| Geräte entdecken Azure Active Gerät Registrierung Verzeichnisdienst bekannten DNS-Datensätzen suchen. Sie müssen Unternehmens DNS konfigurieren, sodass Geräte Azure Active Gerät Registrierung Verzeichnisdienst finden.                                                                                                                                                   | [Konfigurieren Sie Azure Active Directory Geräteregistrierung Discovery.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Teil 2: Bereitstellen und Konfigurieren von Windows Server 2012 R2 Active Directory Federation Services eine Föderation Verbindung mit Azure

| Aufgabe                                                                                                                                                                                                                                                                                                                                                                                             | Referenz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Bereitstellen von Active Directory-Domänendienste-Domäne mit Windows Server 2012 R2 Schema Extensions. Sie müssen nicht die Domänencontroller auf Windows Server 2012 R2 aktualisiert. Schema-Upgrade ist Voraussetzung. | [Das Active Directory Domain Services Schema aktualisieren](#upgrade-your-active-directory-domain-services-schema)               |
| Geräte entdecken Azure Active Gerät Registrierung Verzeichnisdienst bekannten DNS-Datensätzen suchen. Sie müssen Unternehmens DNS konfigurieren, sodass Geräte Azure Active Gerät Registrierung Verzeichnisdienst finden.                                                                                                                                                   | [Vorbereiten der Active Directory-Unterstützung-Geräte](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>Teil 3: Aktivieren Gerät Rückschreiben in Azure AD

| Aufgabe                                                                                                                                                                                                                                                                                                                                                                                             | Referenz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Abschnitt 2 aktivieren Gerät Rückschreiben in Azure AD Connect abgeschlossen. Nach Abschluss dieser dabei zurück. | [Rückschreiben in Azure AD Connect Gerät aktivieren](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Optional] Teil 4: Enable Multi-Authentifizierung

Es empfiehlt sich, eine von mehreren Optionen für mehrstufige Authentifizierung zu konfigurieren. MFA erforderlich ist, finden Sie unter [Wählen Sie die kombinierte Lösung für Sie](../multi-factor-authentication/multi-factor-authentication-get-started.md). Es enthält eine Beschreibung jedes Lösung und Links für die Lösung zu konfigurieren.

## <a name="part-5-verification"></a>Teil 5: Überprüfung

Die Bereitstellung ist abgeschlossen. Sie können nun einige Szenarios ausprobieren. Folgen Sie den Links unten zu experimentieren mit den Features vertraut


| Aufgabe                                                                                                                                                                                                                         | Referenz                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Teilnehmen Sie einige Geräte zu Ihrem Arbeitsplatz mit Azure Active Directory Geräteregistrierung. Verknüpfen von iOS, Windows und Android-Geräte                                                                                         | [Beitreten Sie Geräte zu Ihrem Arbeitsplatz mit Azure Active Directory Geräteregistrierung](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Sie können und registrierte Geräte der Administratorportal aktivieren. Dabei werden einige registrierte Geräte mit der Administratorportal anzeigen.                                                        | [Azure Active Directory Gerät Registrierung-Übersicht](active-directory-conditional-access-device-registration-overview.md)                             |
| Überprüfen Sie, ob das Gerät von Azure Active Directory in Windows Server Active Directory zurückgeschrieben werden.                                                                                                                  | [Überprüfen Sie, ob registrierte Geräte geschrieben zurück in Active Directory](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Damit Benutzer ihre Geräte registrieren können, erstellen Sie Anwendung in AD FS, der nur registrierte Geräte ermöglichen können. In dieser Aufgabe Sie erstellen Zugriff auf eine Zugriffsregel Anwendung und eine benutzerdefinierte abgelehnte Nachricht | [Erstellen Sie eine Anwendungsrichtlinie Zugriff und benutzerdefinierten Zugriff verweigert](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Lokale Active Directory integrieren Sie Azure Active Directory
Dies hilft Ihnen Ihre lokale active Directory mit Azure AD verbinden Azure AD-Mandanten integrieren. Die Schritte im klassischen Azure-Portal verfügbar sind, notieren Sie spezielle Anleitung in diesem Abschnitt aufgeführten.

1.  Melden Sie sich beim klassischen Azure-Portal mit einem Konto als globaler Administrator in Azure AD.
2.  Wählen Sie im linken Bereich **Active Directory**.
3.  Wählen Sie auf der Registerkarte **Verzeichnis** das Verzeichnis ein.
4.  Wählen Sie die Registerkarte **Verzeichnisintegration** .
5.  **Bereitstellen und Verwalten von** Abschnitt die Schritte 1 bis 3 in Ihrem lokalen Verzeichnis Azure Active Directory integrieren.
  1.    Domänen hinzufügen.
  2.    Installieren und Ausführen von Azure AD Connect: Azure installieren AD Verbindung gehen [benutzerdefinierte Installation von Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md)verwenden.
  3. Überprüfen Sie und verwalten Sie Verzeichnis synchronisieren. Anleitung für einzelne Zeichen stehen in diesem Schritt.

  > [AZURE.NOTE]
  > Konfigurieren Sie Föderation mit AD FS, wie im Dokument über verknüpft. Sie müssen nicht die Vorschau-Features konfigurieren.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Active Directory-Domänendienste Schema aktualisieren

> [AZURE.NOTE]
> Aktualisieren Ihre Active Directory-Schema kann nicht storniert werden. Es wird empfohlen, zunächst das in einer Umgebung auszuführen.

1. Melden Sie sich bei dem Domänencontroller über ein Konto mit Organisations-Admins und Schema-Admin-Rechte.
2. Kopieren Sie **[Media] \support\adprep** Verzeichnis einschließlich Unterverzeichnissen eines Active Directory-Domänencontroller.
3. Dabei ist [Media] Pfad zu Windows Server 2012 R2-Installationsmedium.
4. Navigieren Sie zum Verzeichnis Adprep eine Befehlszeile und führen: **adprep.exe/ForestPrep**. Successfully Schema-Upgrade abgeschlossen.

## <a name="prepare-your-active-directory-to-support-devices"></a>Vorbereiten von Active Directory für Geräte

>[AZURE.NOTE] Dies ist einmalig ausgeführt werden müssen, um Geräte unterstützen Active Directory-Gesamtstruktur vorzubereiten. Sie müssen mit Organisationsadministratorrechten angemeldet sein und Active Directory-Gesamtstruktur müssen Windows Server 2012 R2-Schema für dieses Verfahren.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Vorbereiten von Active Directory-Gesamtstruktur für Geräte

> [AZURE.NOTE]
>Dies ist einmalig ausgeführt werden müssen, um Geräte unterstützen Active Directory-Gesamtstruktur vorzubereiten. Sie müssen mit Organisationsadministratorrechten angemeldet sein und Active Directory-Gesamtstruktur müssen Windows Server 2012 R2-Schema für dieses Verfahren.

### <a name="prepare-your-active-directory-forest"></a>Active Directory-Gesamtstruktur vorbereiten

1.  Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehl und geben: Initialize-ADDeviceRegistration
2.  Für **ServiceAccountName**Geben Sie den Namen des Dienstkontos gewählte als Dienstkonto für AD FS. Es ist ein gMSA Konto geben Sie das Konto im Format$ **Schreibweise Domäne\Benutzername an** . Verwenden Sie für ein Domänenkonto Format **Schreibweise Domäne\Benutzername an**.



### <a name="enable-device-authentication-in-ad-fs"></a>In AD FS Geräteauthentifizierung aktivieren

1. Der Verbundserver öffnen die AD FS-Verwaltungskonsole, und navigieren Sie zu **AD FS** > **Authentifizierungsrichtlinien**.
2. Wählen Sie **Bearbeiten globale Authentifizierung...** im **Aktionsbereich** .
3. Überprüfen Sie **Geräteauthentifizierung aktivieren** , und klicken Sie auf**OK**.
4. Standardmäßig wird AD FS nicht verwendete Geräte regelmäßig aus Active Directory entfernt. Diese Aufgabe muss deaktiviert werden, wenn Azure Active Directory Geräteregistrierung mit Geräten in Azure verwaltet werden können.


### <a name="disable-unused-device-cleanup"></a>Deaktivieren Sie nicht verwendete Geräte bereinigen
1. Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehl und geben: Set-AdfsDeviceRegistration - MaximumInactiveDays-0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Gerät Rückschreiben Azure AD Connect vorbereiten

1.  Führen Sie Teil 1: Vorbereiten Azure AD verbinden.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Beitreten Sie Geräte zu Ihrem Arbeitsplatz mit Azure Active Directory Geräteregistrierung

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Verknüpfen Sie mit Azure Active Directory Geräteregistrierung iOS-Gerät

Azure Active Directory Geräteregistrierung verwendet Registrierungsprozess drahtlose Profil für iOS-Geräte. Dieser Prozess beginnt mit der Benutzer Profil Registrierung URL mit dem Webbrowser Safari. Das URL-Format lautet wie folgt:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Wo `yourdomainname` ist der Name, den Sie in Azure Active Directory konfiguriert haben. Beispielsweise ist der Domänenname contoso.com, wäre URL:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Es gibt viele Arten zu dieser URL an Ihre Benutzer. Eine Möglichkeit ist an diesen URL in einer benutzerdefinierten Anwendungszugriff in AD FS Nachricht abgelehnt. Dies wird im nächsten Abschnitt behandelt: [Erstellen einer Anwendungsrichtlinie Zugriff und benutzerdefinierten Zugriff verweigert](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Beitreten Sie Windows 8.1 Geräts Azure Active Directory Geräteregistrierung

1. Navigieren Sie auf Ihrem Gerät Windows 8.1 **PC**Einstellungen > **Netzwerk** > **Arbeitsplatz**.
2. Geben Sie den Benutzernamen im UPN-Format. Z. B. dan@contoso.com..
3. Wählen Sie **Verknüpfung**aus.
4. Bei Aufforderung anmelden Sie mit Ihren Anmeldeinformationen. Das Gerät ist angeschlossen.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Beitreten Sie Windows 7 Geräts Azure Active Directory Geräteregistrierung
Um Windows 7 registrieren verbunden Geräte, die Sie das Gerät Registrierung Software-Paket bereitstellen müssen. Das Software-Paket heißt Arbeitsplatz Join für Windows 7 und steht als Download auf der [Microsoft Connect-Website](https://connect.microsoft.com/site1164). Anleitung zum Verwenden des Pakets finden Sie unter [Konfigurieren automatische Registrierung für Windows 7-Domäne angeschlossen Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md).

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Beitreten Sie ein Android-Gerät Azure Active Directory Geräteregistrierung

[Azure Authenticator für Android Thema](active-directory-conditional-access-azure-authenticator-app.md) hat Informationen zu Azure Authenticator app Android Geräts ein Geschäftskonto hinzufügen. Wenn ein Geschäftskonto auf Android-Gerät erstellt wird, ist das Gerät Arbeitsplatz für die Organisation.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Überprüfen Sie, ob registrierte Geräte geschrieben zurück in Active Directory
Anzeigen und überprüfen, ob Ihre Geräteobjekte zurück in Active Directory geschrieben wurden mit LDP.exe oder ADSI Edit. Beide sind mit Active Directory-Verwaltung.

Standardmäßig werden geschrieben-von Azure Active Directory sind Geräteobjekte in derselben Domäne wie der AD FS-Farm befinden.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Erstellen Sie eine Anwendungsrichtlinie Zugriff und benutzerdefinierten Zugriff verweigert
Das folgende Szenario: eine Anwendung sich Partei Vertrauen in AD FS erstellen und Konfigurieren einer Ausstellungsautorisierungsregel, nur registrierte Geräte. Jetzt dürfen nur registrierte Geräte auf die Anwendung zugreifen. Um den Benutzern Zugriff auf die Anwendung zu erleichtern, konfigurieren Sie eine benutzerdefinierte Meldung mit Informationen an ihr Gerät kein Zugriff. Die Benutzer haben jetzt nahtlos auf ihren Geräten anmelden, um auf eine Anwendung zugreifen.

Die folgenden Schritte zeigen Ihnen, wie dieses Szenario implementiert.

>[AZURE.NOTE]
In diesem Abschnitt wird vorausgesetzt, dass Sie bereits eine verlassen Partei vertrauen für Ihre Anwendung in AD FS konfiguriert haben.

1. AD FS MMC-Tool öffnen und navigieren zu AD FS > Vertrauensstellungen > Vertrauen verlassen.
2. Suchen Sie die Anwendung, für die diese neue Zugriffsregel angewendet wird. Maustaste auf die Anwendung, und wählen Sie Anspruchsregeln bearbeiten...
3. Wählen Sie die Registerkarte **Ausgabe Autorisierungsregeln** und wählen Sie **Regel hinzufügen**
4. **Anspruchsregel** Vorlage Dropdown-Liste Wählen Sie **Zulassen oder verweigern Benutzern eines eingehenden Anspruchs**. Wählen Sie **Weiter**.
5. Im Antrag Regelnamen: Feldtyp: **Zugriff von registrierten Geräte**
6. Die eingehenden Anspruchstyp: Dropdownliste, wählen Sie **Registrierte Benutzer ist**.
7. Der eingehende Anspruchswert: Feld: **true**
8. Aktivieren Sie das Optionsfeld **Zugriff auf Benutzer mit diesem eingehenden Anspruch** .
9. Wählen Sie **Fertig stellen** und dann **Übernehmen**.
10. Entfernen Sie alle Regeln mehr Berechtigungen als die Regel, die Sie gerade erstellt haben. Z. B. Entfernen der Standardregel **Zugriff für alle Benutzer zulassen** .

Die Anwendung wird jetzt Zugriff konfiguriert nur, wenn der Benutzer ein Gerät stammt, die sie registriert und Arbeitsplatz. Erweiterte Richtlinien, finden Sie unter [Risiko mit kombinierten Zugriff verwalten](https://technet.microsoft.com/library/dn280949.aspx).

Als Nächstes konfigurieren Sie eine benutzerdefinierte Fehlermeldung für die Anwendung. Die Fehlermeldung können Benutzer wissen, dass sie ihr Gerät am Arbeitsplatz beitreten müssen, bevor sie Zugriff auf die Anwendung gewährt. Sie können eine benutzerdefinierte Anwendungszugriff verweigert mit benutzerdefinierten HTML-Windows PowerShell erstellen.

Der Verbundserver öffnen Sie ein Windows PowerShell-Befehl, und geben Sie folgenden Befehl. Elemente, die für Ihr System sind ersetzen Sie Teile des Befehls:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Bevor Sie diese Anwendung zugreifen können, müssen Sie Ihr Gerät registrieren.

Sie **verwenden ein Gerät iOS wählen Sie diesen Link an Ihrem Gerät**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Fügen Sie dieses Gerät iOS Arbeitsplatz.


**Verwenden ein Windows 8.1**beitreten Sie Ihr Gerät vom **PC **Einstellungen >  **Netzwerk** > **Arbeitsplatz**.


"**Vertrauensstellung Parteinamen verlassen**" den Namen des Objekts verlassen Partei vertrauen Applications in AD FS ist.
**IhreDomäne.com** der Domänenname ist, die in Azure Active Directory konfiguriert haben. Z. B. contoso.com.
Achten Sie darauf, dass Sie an das Cmdlet " **Set-AdfsRelyingPartyWebContent** " der HTML-Inhalt (falls vorhanden) Zeilenumbrüche entfernen.


Jetzt greifen Benutzer Ihre Anwendung von einem Gerät, das nicht den Registrierungsdienst Azure Active Directory Gerät registriert ist, erhalten sie eine, der den Screenshot ähnelt.

![Screeshot Fehler beim Benutzer ihr Gerät mit Azure registriert haben](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)

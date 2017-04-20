<properties
    pageTitle="Problembehandlung bei Zugriffsproblemen Azure Active Directory | Microsoft Azure"
    description="Erfahren Sie Schritte Access Probleme mit online-Ressourcen der Organisation."
    services="active-directory"
    keywords="Registrierung des Geräts, geräteregistrierung und MDM gerätebasierte Zugangskontrolle, geräteregistrierung aktivieren"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Problembehandlung für Active Directory Azure Fragen

Sie versuchen, SharePoint Online Intranet Ihrer Organisation zugreifen und Sie eine Fehlermeldung "Zugriff verweigert". Was machst du?

Dieser Artikel beschreibt Schritte, die Sie in Access Probleme mit online-Ressourcen der Organisation unterstützen.

Hilfe Azure Active Directory (Azure AD) Zugriff auf Probleme, lesen Sie den Abschnitt in dem Artikel, der die Geräteplattform abdeckt:

-   Windows-Gerät
-   iOS-Gerät (Überprüfen Sie bald wieder Unterstützung iPhone und iPad.)
-   Android-Gerät (schauen Sie bald Unterstützung für Android-Telefone und Tablet-Computer.)

## <a name="access-from-a-windows-device"></a>Zugriff von einem Windows-Gerät

Wenn das Gerät eine der folgenden Plattformen ausgeführt wird, suchen Sie in den nächsten Abschnitten die Fehlermeldung angezeigt wird, wenn Sie versuchen, eine Anwendung oder ein Dienst zuzugreifen:

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7
- WindowsServer 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Gerät ist nicht registriert.

Wenn Ihr Gerät nicht mit Azure registriert und die Anwendung mit gerätebasierte geschützt ist, sehen Sie eine Seite, die eine der folgenden Fehlermeldungen:

!["Es hier man nicht" Nachrichten für nicht registrierte Geräte] (./media/active-directory-conditional-access-device-remediation/01.png "Szenario")

Ist das Gerät Domäne in Active Directory in Ihrer Organisation, versuchen Sie Folgendes:

1.  Stellen Sie sicher, dass Sie an Windows anmelden Konto bei der Arbeit (der Active Directory-Konto).
2.  Verbindung mit dem Unternehmensnetzwerk über ein virtuelles privates Netzwerk (VPN) oder DirectAccess.
3.  Nachdem Sie verbunden sind, drücken Sie die Windows-Taste + Taste L Ihre Windows-Sitzung gesperrt.
4.  Geben Sie Ihre Arbeit Anmeldeinformationen entsperren Ihre Windows-Sitzung.
5.  Warten Sie eine Minute und wiederholen Sie dann die Anwendung oder den Dienst auf.
6.  Wenn dieselbe Seite, klicken Sie auf **Details** , und bitten Sie den Administrator mit den Details angezeigt.

Wenn Ihr Gerät nicht Domäne und Windows 10 ausgeführt wird, haben Sie zwei Optionen:

- Azure AD Join ausführen
- Fügen Sie Ihr Konto Geschäfts- oder Windows

Informationen, wie diese Optionen unterscheiden finden Sie unter [verwenden Windows 10 Geräte am Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).

Führen Sie Azure AD Join folgendermaßen Sie für die Plattform, die auf das Gerät ausgeführt wird. (Azure AD Join ist nicht für Windows Phone verfügbar.)

**Windows Update 10 Jahre**

1.  Öffnen Sie die **Einstellungen** app.
2.  Klicken Sie auf **Konten** > **arbeiten oder Schule**.
3.  Klicken Sie auf **Verbinden**.
4.  Klicken Sie auf **dieses Gerät Azure AD teilnehmen**.
5.  Ihre Organisation authentifizieren Sie, mehrstufige Authentifizierung Wenn aufgefordert, und folgen Sie den Schritten, die angezeigt werden.
6.  Melden Sie ab, und melden Sie sich mit Ihrem Konto arbeiten.
7.  Versuchen Sie erneut, auf die Anwendung zugreifen.


**Windows Update 10. November 2015**

1.  Öffnen Sie die **Einstellungen** app.
2.  Klicken Sie auf **System** > **zu**.
3.  Klicken Sie auf **Azure AD teilnehmen**.
4.  Ihre Organisation authentifizieren Sie, mehrstufige Authentifizierung Wenn aufgefordert, und folgen Sie den Schritten, die angezeigt werden.
5.  Melden Sie ab, und melden Sie sich mit der Arbeit (Konto Ihre Azure AD).
6.  Versuchen Sie erneut, auf die Anwendung zugreifen.

Gehen Sie folgendermaßen vor, um Ihre Arbeit oder Schule Konto hinzuzufügen:

**Windows Update 10 Jahre**

1.  Öffnen Sie die **Einstellungen** app.
2.  Klicken Sie auf **Konten** > **arbeiten oder Schule**.
3.  Klicken Sie auf **Verbinden**.
4.  Ihre Organisation authentifizieren Sie, mehrstufige Authentifizierung Wenn aufgefordert, und folgen Sie den Schritten, die angezeigt werden.
5.  Versuchen Sie erneut, auf die Anwendung zugreifen.


**Windows Update 10. November 2015**

1.  Öffnen Sie die **Einstellungen** app.
2.  Klicken Sie auf **Konten** > **Ihrer Konten**.
3.  Klicken Sie auf **Arbeit Schule Konto**.
4.  Ihre Organisation authentifizieren Sie, mehrstufige Authentifizierung Wenn aufgefordert, und folgen Sie den Schritten, die angezeigt werden.
5.  Versuchen Sie erneut, auf die Anwendung zugreifen.

Gehen Sie folgendermaßen vor, wenn Ihr Gerät nicht Domäne und führt Windows 8.1 Arbeitsplatz beitreten und Windows Intune anmelden:

1.  **PC-Einstellungen**zu öffnen.
2.  Klicken Sie auf **Netzwerk** > **Arbeitsplatz**.
3.  Klicken Sie auf **beitreten**.
4.  Ihre Organisation authentifizieren Sie, mehrstufige Authentifizierung Wenn aufgefordert, und folgen Sie den Schritten, die angezeigt werden.
5.  Klicken Sie auf **Aktivieren**.
6.  Versuchen Sie erneut, auf die Anwendung zugreifen.


### <a name="browser-is-not-supported"></a>Browser wird nicht unterstützt.

Access kann verweigert werden, wenn Sie versuchen auf eine Anwendung oder ein Dienst mithilfe einer der folgenden Browser:

- Chrome, Firefox oder einen anderen Browser als Microsoft Edge oder Microsoft Internet Explorer 10 für Windows oder Windows Server 2016
- Firefox unter Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2

Sie sehen eine Fehlerseite, die folgendermaßen aussieht:

!["Sie können nicht hier dorthin" Nachricht nicht unterstützte Browser] (./media/active-directory-conditional-access-device-remediation/02.png "Szenario")

Nur Behebung ist einen Browser verwenden, den die Anwendung für Ihre Plattform unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[Azure Active Directory Zugangskontrolle](active-directory-conditional-access.md)

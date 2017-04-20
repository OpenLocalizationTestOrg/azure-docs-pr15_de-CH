<properties
    pageTitle="Server mit Windows Server 2012 R2 AD FS MFA | Microsoft Azure"
    description="Dieser Artikel beschreibt Schritte Azure mehrstufige Authentifizierung mit AD FS in Windows Server 2012 R2."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Sichern Sie AD FS unter Windows Server 2012 R2 Azure mehrstufige Authentifizierungsserver mit Cloud und lokalen Ressourcen

Wenn Active Directory Federation Services (AD FS) verwenden und sichere Cloud lokalen Ressourcen bzw. können Sie Azure mehrstufige Authentifizierungsserver gemeinsam mit AD FS konfigurieren. Diese Konfiguration werden zwei Überprüfung für hochwertige Endpunkte.

In diesem Artikel erläutern wir Azure mehrstufige Authentifizierungsserver mit AD FS in Windows Server 2012 R2. Lesen Sie weitere Informationen über das [sichere Cloud und lokale Ressourcen mithilfe von Azure mehrstufige Authentifizierungsserver mit AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>Sichern von Windows Server 2012 R2 AD FS Azure Multi-Factor Authentication Server

Bei der Installation von Azure mehrstufige Authentifizierungsserver haben Sie folgenden Optionen:

- Installationsserver Azure mehrstufige Authentifizierung lokal auf demselben Server wie AD FS
- Installieren Sie Azure mehrstufige Authentifizierung Adapter auf dem AD FS-Server lokal und installieren Sie mehrstufige Authentifizierungsserver auf einem anderen Computer.

Bevor Sie beginnen, beachten Sie folgende Informationen:

- Sie müssen nicht Azure mehrstufige Authentifizierungsserver auf dem AD FS-Server installieren. Jedoch müssen Sie mehrstufige Authentifizierung Adapter AD FS auf einem Windows Server 2012 R2 installieren, auf dem AD FS ausgeführt wird. Eine unterstützte Version und Installation des AD FS-Adapters separat auf der ADFS-Verbundserver, können Sie den Server auf einem anderen Computer installieren. Nachfolgend erfahren, wie den Adapter separat installieren angezeigt.

- Bei der MFA Server AD FS-Adapter wurde wurde erwartet, dass AD FS den Namen der relying Party an dem Adapter übergeben konnte. Dann konnte der relying Party Name als Anwendungsname für eine verwendet. Jedoch war dies nicht zu. Wenn Ihre Organisation Text- oder mobilen Anwendung Verfahren verwendet, enthalten einen Platzhalter, < $*Application_name*> im Unternehmen definierten Zeichenfolgen. Dieser Platzhalter wird nicht automatisch ersetzt, wenn AD FS-Adapter verwenden. Es wird empfohlen, sichere AD FS Platzhalter aus entsprechenden Zeichenfolgen entfernen.

- Das Konto, das Sie zum Anmelden verwenden müssen Rechte zum Erstellen von Sicherheitsgruppen in Active Directory-Dienst.

- Mehrstufige Authentifizierung AD FS Adapter-Installationsassistent erstellt eine Sicherheitsgruppe PhoneFactor-Admins in der Instanz von Active Directory. Dann werden dieser Gruppe AD FS-Dienstkonto von Ihrem Verbunddienst hinzugefügt. Wir empfehlen Sie auf dem Domänencontroller überprüfen, ob die Gruppe PhoneFactor-Admins tatsächlich erstellt und AD FS-Dienstkonto ein Mitglied dieser Gruppe ist. Fügen Sie ggf. den AD FS-Dienstkonto manuell PhoneFactor Admins-Gruppe auf dem Domänencontroller.

- Informationen zum Installieren von Web Service SDK mit User Portal, [User Portal für Azure mehrstufige Authentifizierungsserver bereitstellen.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Installationsserver Azure mehrstufige Authentifizierung lokal auf dem AD FS-server

1. Downloaden und Installieren von Azure mehrstufige Authentifizierungsserver auf dem AD FS-Server. Finden Sie Informationen zum [Einstieg in Azure mehrstufige Authentifizierungsserver](multi-factor-authentication-get-started-server.md).
2. Klicken Sie in Azure mehrstufige Authentifizierungsserver-Managementkonsole auf **AD FS** und wählen Sie die Optionen **Benutzer-Registrierung zulassen** und **Benutzern erlauben, Methode**.
3. Wählen Sie zusätzliche Optionen, die Sie für Ihre Organisation festlegen möchten.
4. Klicken Sie auf **AD FS Adapter installieren**.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Wenn das Active Directory-Fenster angezeigt wird, bedeutet, dass zwei Dinge. Der Computer ist Mitglied einer Domäne und Active Directory-Konfiguration zur Absicherung der Kommunikation zwischen den AD FS-Adapter und dem Dienst mehrstufige Authentifizierung ist unvollständig. Klicken Sie auf **Weiter** um automatisch diese Konfiguration abzuschließen, oder wählen Sie die **automatisch Active Directory-Konfiguration überspringen und Einstellungen manuell** , und klicken Sie dann auf **Weiter**.
6. Lokale Gruppe Windows angezeigt, bedeutet dies zweierlei. Ihr Computer ist nicht Mitglied einer Domäne und die Gruppenkonfiguration zur Absicherung der Kommunikation zwischen den AD FS-Adapter und dem Dienst mehrstufige Authentifizierung ist unvollständig. Klicken Sie auf **Weiter** um automatisch diese Konfiguration abzuschließen, oder wählen Sie die **Automatische Lokalgruppen-Konfiguration überspringen und Einstellungen manuell** , und klicken Sie dann auf **Weiter**.
7. Klicken Sie im Installations-Assistenten auf **Weiter**. Azure mehrstufige Authentifizierungsserver PhoneFactor Admins-Gruppe erstellt und der Gruppe PhoneFactor-Admins AD FS-Dienstkonto hinzugefügt.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Klicken Sie auf der Seite **Installationsprogramm starten** auf **Weiter**.
9. Mehrstufige Authentifizierung AD FS Adapter Installer klicken Sie auf **Weiter**.
10. Klicken Sie auf **Schließen** , wenn die Installation abgeschlossen ist.
11. Wenn der Adapter installiert ist, müssen Sie es mit AD FS registrieren. Öffnen Sie Windows PowerShell, und führen Sie den folgenden Befehl:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Um die neu registrierten Adapter verwenden, bearbeiten Sie die globale Authentifizierungsrichtlinie in AD FS Gehen Sie in der Verwaltungskonsole AD FS **Authentifizierungsrichtlinien** Knoten. Klicken Sie im Abschnitt **Mehrstufige Authentifizierung** auf den Link **Bearbeiten** neben den **Globalen** Einstellungen. Wählen Sie im Fenster **Bearbeiten globale Authentifizierungsrichtlinie** **Mehrstufige Authentifizierung** als weitere Authentifizierungsmethode aus und dann auf **OK**. Der Adapter wird als WindowsAzureMultiFactorAuthentication registriert. Den AD FS-Dienst für die Registrierung zu starten.

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

An diesem Punkt ist mehrstufige Authentifizierungsserver eingerichtet zusätzliche Authentifizierungsanbieter mit AD FS.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Installieren Sie eine eigenständige Instanz des AD FS-Adapters mithilfe des Web Service SDK
1. Installieren Sie Web Service SDK auf dem Server, der mehrstufige Authentifizierungsserver ausgeführt wird.
2. Die folgenden Dateien aus dem \Program Files\Multi-Factor Authentication Server Verzeichnis auf dem Server den AD FS-Adapter installieren möchten:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - MultiFactorAuthenticationAdfsAdapter.ps1 registrieren
  - Registrierung MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Führen Sie die Datei MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. Mehrstufige Authentifizierung AD FS Adapter Installer klicken Sie auf **Weiter** um die Installation zu starten.
5. Klicken Sie auf **Schließen** , wenn die Installation abgeschlossen ist.

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>Bearbeiten Sie die Datei MultiFactorAuthenticationAdfsAdapter.config

Gehen Sie die MultiFactorAuthenticationAdfsAdapter.config-Datei

1. Den Knoten **UseWebServiceSdk** auf **true**festgelegt.  
2. Legen Sie den Wert für **WebServiceSdkUrl** auf die URL der kombinierten Authentifizierung Web Service SDK. Beispiel: *https://contoso.com/&lt;Certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, wobei Certificatename der Name des Zertifikats.  
3. Bearbeiten Sie das Skript MultiFactorAuthenticationAdfsAdapter.ps1 Register durch Hinzufügen von *- ConfigurationFilePath &lt;Pfad&gt; * am Ende der `Register-AdfsAuthenticationProvider` Befehl, * &lt;Pfad&gt; * ist der vollständige Pfad zur Datei MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>Konfigurieren der Web Service SDK mit Benutzername und Kennwort

Es gibt zwei Optionen zum Konfigurieren der Web Service SDK. Der erste ist ein Benutzername und Kennwort, der zweite mit einem Clientzertifikat. Diese Schritte für die erste Option, oder fahren Sie für die zweite.  

1. Legen Sie den Wert für **WebServiceSdkUsername** auf ein Konto, das Mitglied der Sicherheitsgruppe PhoneFactor-Admins ist. Verwenden der &lt;Domäne&gt;& 92; &lt;Benutzername&gt; Format.  
2. Legen Sie den Wert für **WebServiceSdkPassword** auf das entsprechende Kennwort ein.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>Konfigurieren der Web Service SDK mit einem Clientzertifikat

Möchten Sie Benutzernamen und Kennwort verwenden, folgendermaßen Sie vor, um Web Service SDK mit einem Clientzertifikat zu konfigurieren.

1. Ein Client-Zertifikat von einer Zertifizierungsstelle für die Server mit Web Service SDK zu erhalten. Erfahren Sie, wie Sie [Clientzertifikate erhalten](https://technet.microsoft.com/library/cc770328.aspx).  
2. Importieren Sie das Client-Zertifikat in der lokalen persönlichen Zertifikatspeicher auf dem Server mit Web Service SDK. Stellen Sie sicher, dass das Zertifikat der öffentlichen Zertifizierungsstelle im Zertifikatspeicher für vertrauenswürdige Stammzertifikate.  
3. Exportieren Sie die öffentlichen und privaten Schlüssel des Clientzertifikats in eine PFX-Datei.  
4. Exportieren Sie den öffentlichen Schlüssel im Base64-Format in eine CER-Datei.  
5. Überprüfen Sie im Server-Manager, ob das Webserver (IIS) \Web Server\Security\IIS Clientzertifikatzuordnung-Authentifizierung-Feature installiert ist. Nicht installiert ist, wählen Sie **Hinzufügen von Rollen und Features** dieses Feature.  
6. Doppelklicken Sie im IIS-Manager auf **Konfigurations-Editor** in die Website, die das virtuelle Verzeichnis Web Service SDK enthält. Es ist auf der Website und nicht auf der Ebene des virtuellen Verzeichnisses wichtig.  
7. Gehen Sie zum Abschnitt **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Festgelegt auf **true**aktiviert.  
9. OneToOneCertificateMappingsEnabled auf **true**festgelegt.  
10. Klicken Sie neben OneToOneMappings **...** , und klicken Sie auf den Link **Hinzufügen** .  
11. Öffnen Sie die Base64 CER-Datei, die Sie zuvor exportiert. Entfernen Sie *--beginnen Zertifikat--*, *--Ende Zertifikat---*und alle Zeilenumbrüche Kopieren Sie die resultierende Zeichenfolge.  
12. Zertifikat im vorherigen Schritt kopierten Zeichenfolge festlegen.  
13. Festgelegt auf **true**aktiviert.  
14. Festlegen des Benutzernamens auf ein Konto, das Mitglied der Sicherheitsgruppe PhoneFactor-Admins ist. Verwenden der &lt;Domäne&gt;& 92; &lt;Benutzername&gt; Format.  
15. Legen Sie das Kennwort das entsprechende Kennwort und schließen Sie Konfigurations-Editor.  
16. Klicken Sie auf **Übernehmen** .  
17. Doppelklicken Sie im virtuellen Verzeichnis Web Service SDK auf **Authentifizierung**.  
18. Stellen Sie sicher, dass ASP.NET Impersonation und Standardauthentifizierung **aktiviert**sind und alle anderen Elemente auf **deaktiviert**gesetzt.  
19. Doppelklicken Sie auf **SSL-Einstellungen**im Web Service SDK virtuellen Verzeichnis.  
20. Clientzertifikate **akzeptieren**soll, und klicken Sie auf **Übernehmen**.  
21. Kopieren Sie die PFX-Datei, die Sie zuvor auf dem Server exportiert, die AD FS-Adapter ausgeführt wird.  
22. Importieren Sie die PFX-Datei auf dem persönlichen Zertifikatspeicher des lokalen Computers.  
23. Mit der rechten Maustaste und wählen Sie **Privatschlüssel verwalten**und gewähren Sie Lesezugriff auf das Konto, mit dem AD FS-Dienst anmelden.  
24. Öffnen Sie das Clientzertifikat und kopieren Sie den Fingerabdruck über die Registerkarte **Details** .  
25. Legen Sie die **WebServiceSdkCertificateThumbprint** in der Datei MultiFactorAuthenticationAdfsAdapter.config die Zeichenfolge im vorherigen Schritt kopiert.  


Zum Registrieren des Adapters führen Sie schließlich die \Program Files\Multi-Factor Authentication-Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1-Skript in PowerShell. Der Adapter wird als WindowsAzureMultiFactorAuthentication registriert. Den AD FS-Dienst für die Registrierung zu starten.

## <a name="related-topics"></a>Verwandte Themen

Hilfe zur Problembehandlung finden Sie unter [Azure mehrstufige Authentifizierung FAQs](multi-factor-authentication-faq.md)

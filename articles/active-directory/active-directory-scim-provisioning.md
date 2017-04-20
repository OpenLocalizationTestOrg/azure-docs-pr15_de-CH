<properties
    pageTitle="SCIM ermöglichen die automatische Bereitstellung von Benutzern und Gruppen in Active Directory Azure Clientanwendungen mit | Microsoft Azure"
    description="Azure Active Directory können Benutzer und Gruppen zu Anwendung oder die Identität, die von einem Webdienst mit definierten SCIM-Protokollspezifikation Oberfläche konfrontiert wird automatisch bereitgestellt"
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

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>So aktivieren Sie die automatische Bereitstellung von Benutzern und Gruppen aus dem Active Directory Azure Applications verwenden SCIM

##<a name="overview"></a>Übersicht

Azure Active Directory können Benutzer und Gruppen zu Anwendung oder die Identität, die von einem Webdienst mit der Schnittstelle definierten [SCIM 2.0-Protokollspezifikation](https://tools.ietf.org/html/draft-ietf-scim-api-19)konfrontiert wird automatisch bereitgestellt. Azure Active Directory können Anfragen erstellen, ändern und löschen zugeordneten Benutzer und Gruppen zu diesen Webdienst, die dann diese Anfragen in Vorgängen Identitätsspeicher Ziel übersetzen kann. 

![][1]
*Abbildung: Bereitstellung von Azure Active Directory in einem Identitätsspeicher über einen Webdienst*

Diese Funktion kann verwendet werden in Verbindung mit der Funktion "[bringen Sie Ihre Anwendung](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" in Azure AD einmaliges und automatische Bereitstellung für Applikationen, die bereitstellen oder einen Webdienst SCIM konfrontiert Benutzer.

Es sind zwei für SCIM in Azure Active Directory:

* **Benutzer und Gruppen, die SCIM unterstützen, Clientanwendungen provisioning** - Applikationen, die Unterstützung SCIM 2.0 und OAuth trägertoken zur Authentifizierung funktioniert mit Azure AD Feld.

* **Erstellen Ihrer eigenen Bereitstellung Lösung für Applikationen, die andere API-basierte Bereitstellung unterstützt** - nicht SCIM handelt, können einen SCIM Endpunkt zwischen Azure AD SCIM Endpunkt und was übersetzen unterstützt API der Anwendung für Benutzer bereitstellen.  Um Hilfe bei der Entwicklung eines Endpunkts SCIM bieten wir CLI-Bibliotheken und Codebeispiele, die zeigen, wie SCIM Endpunkt bereitstellen und übersetzen SCIM Nachrichten.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Bereitstellen von Benutzern und Gruppen zu Programmen, die SCIM unterstützen

Azure Active Directory automatisch zugewiesene Benutzer konfiguriert werden und Clientanwendungen, die eine [System für die domänenübergreifende Identitätsmanagement 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) Web implementieren service und OAuth trägertoken zur Authentifizierung akzeptiert. In SCIM 2.0-Spezifikation müssen Programme diese Anforderungen:

* Unterstützt das Erstellen von Benutzern und Gruppen gemäß Abschnitt 3.3 des Protokolls SCIM.  

* Ändern von Benutzern oder Gruppen mit Patch gemäß Abschnitt 3.5.2 SCIM Protokoll unterstützt.  

* Abrufen einer bekannten Ressource gemäß Abschnitt 3.4.1 des Protokolls SCIM unterstützt.  

*  Abfragen von Benutzern oder Gruppen gemäß Abschnitt 3.4.2 SCIM-Protokoll unterstützt.  ExternalId Benutzer abgefragt werden, und Gruppen von DisplayName abgefragt werden.  

* Benutzer-ID und Manager gemäß Abschnitt 3.4.2 des Protokolls SCIM Abfragen unterstützt.  

* Gruppen-ID und Mitglied gemäß Abschnitt 3.4.2 des Protokolls SCIM Abfragen unterstützt.  

* OAuth trägertoken Zulassungsantrag gemäß Abschnitt 2.1 des Protokolls SCIM akzeptiert.

Überprüfen Sie mit dem Anbieter der Anwendung oder Ihrem Softwarehändler Dokumentation Anweisungen Kompatibilität mit diesen Vorschriften.
 
###<a name="getting-started"></a>Erste Schritte

Azure Active Directory mithilfe der Funktion "custom" app in der Galerie Azure AD können Programme, die das SCIM Profil oben beschriebenen verbunden. Nachdem die Verbindung hergestellt ist, führt Azure AD eine Synchronisierung alle 5 Minuten, fragt die Anwendung SCIM Endpunkt zugeordneten Benutzer und Gruppen, und erstellt oder nach die Aufgabendetails bearbeitet.

**Zum Verbinden einer Anwendung, die SCIM unterstützt:**

1.  Starten Sie in einem Webbrowser Azure-Verwaltungsportal an https://manage.windowsazure.com.
2.  Navigieren Sie zu **Active Directory > Verzeichnis > [Ihr Verzeichnis] > Applications**, und wählen Sie **Hinzufügen > eine Anwendung aus der Galerie hinzufügen**.
3.  Wählen Sie die **benutzerdefinierte** Registerkarte links, geben Sie einen Namen für die Anwendung und klicken Sie auf das Häkchensymbol ein app-Objekt erstellen.

![][2]

4.  Wählen Sie im daraufhin eingeblendeten Bildschirm die zweite Schaltfläche **Bereitstellung konfigurieren** .
5.  Geben Sie die URL der Anwendungsendpunkt SCIM im Feld **Endpunkt-URL bereitstellen** .
6.  SCIM Endpunkt ein Token OAuth Träger von Emittenten als Azure AD benötigt, kopieren Sie erforderliche OAuth trägertoken im Feld **Authentifizierungstoken (optional)** . Ist dieses Feld leer und Azure AD umfasst ein OAuth trägertoken ausgestellt von Azure AD mit jeder Anforderung. Apps, die als ein Idenity Anbieter Azure AD überprüfen Azure AD verwenden-ausgestellte Token.
7.  Klicken Sie auf **Weiter**, und klicken Sie auf die Schaltfläche **Test starten** zu Azure Active Directory Verbindung zur SCIM Endpunkt. Wenn die Versuche fehlschlagen, werden Diagnoseinformationen angezeigt.  
8.  Wenn **die Verbindungsversuche mit Erfolg Anwendung klicken auf** die verbleibenden Bildschirme und klicken Sie auf **abschließen** , um das Dialogfeld zu schließen.
9.  Wählen Sie im daraufhin eingeblendeten Bildschirm die dritte Schaltfläche **Konten zuweisen** . Zuordnen Sie in der resultierenden Abschnitt Benutzer und Gruppen die Benutzer oder Gruppen zu der Anwendung
10. Nach Benutzern und Gruppen zugewiesen werden, klicken Sie auf die Registerkarte **Konfigurieren** im oberen Bereich des Bildschirms.
11. Nach der **Bereitstellung**sicher, dass der Status aktiviert ist. 
12. Klicken Sie unter **Tools**zum Starten der Bereitstellungsprozess **Bereitstellung neu starten** .

Beachten Sie, dass 5 bis 10 Minuten vor der Bereitstellungsprozess werden Anfragen an den Endpunkt SCIM verstreichen kann.  Eine Zusammenfassung der Verbindungsversuche für Dashboard-Registerkarte für die Anwendung bereitgestellt, und einen Bericht zur Bereitstellung Aktivität und Bereitstellung Fehler von Teilnehmerverzeichnis Berichte heruntergeladen werden können.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Erstellen Ihre eigene Lösung für jede Anwendung bereitgestellt

Indem eine SCIM-Webdienst, der Schnittstellen mit Azure Active Directory können einzelne anmelden und automatische benutzerbereitstellung für nahezu jede Anwendung, die ein REST oder SOAP-Benutzer API bereitstellen.

So geht es:

1.  Azure AD bietet eine gemeinsame Sprache Infrastrukturbibliothek namens [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Systemintegratoren und Entwickler können diese Bibliothek erstellen und Bereitstellen eines SCIM-basierten Webdienst-Endpunkts kann jede Anwendung Identitätsspeicher Azure AD mit.
2.  Pfade werden im Webdienst der Benutzerschema und das Protokoll von der Anwendung benötigten Benutzer standardisierten Schema zuordnen implementiert.
3.  Die Endpunkt-URL ist in Azure AD als Teil einer benutzerdefinierten Anwendung in der Galerie registriert.
4.  Diese Anwendung in Azure AD werden Benutzern und Gruppen zugewiesen. Nach Zuweisung stellen sie in eine Warteschlange mit der Anwendung synchronisiert werden. Der Synchronisierungsvorgang Behandlung der Warteschlange wird alle 5 Minuten ausgeführt.

###<a name="code-samples"></a>Code-Beispiele

Zur Vereinfachung dieses Vorgangs mehrere [code-Beispiele](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) erhalten, erstellen Sie eine SCIM und veranschaulichen die automatische Bereitstellung. Ein Beispiel ist ein Anbieter, der eine Datei mit kommagetrennten Werten darstellt, Benutzer und Gruppen verwaltet.  Die andere ist ein Anbieter, der auf den Dienst Amazon Web Services Identitäts- und Zugriffsmanagement.  

**Erforderliche Komponenten**

* Visual Studio 2013 oder höher
* [Azure SDK für .NET](https://azure.microsoft.com/downloads/)
* Windows-Computer, die ASP.NET Framework 4.5 unterstützt, als SCIM Endpunkt verwendet werden soll. Dieser Computer muss aus der Cloud zugegriffen werden.
* [Ein Azure-Abonnement mit einer Testversion oder die lizenzierte Version von Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* Das Amazon AWS Beispiel erfordert Bibliotheken [AWS-Toolkit für Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Finden Sie in der Infodatei im Beispiel Weitere Informationen

###<a name="getting-started"></a>Erste Schritte

Die SCIM Endpunkt implementiert, der Anfragen von Azure AD akzeptieren kann am einfachsten erstellen und Bereitstellen der Beispielcode, der eine durch Kommas getrennte Werte (CSV) bereitgestellten Benutzer ausgibt.

**Beispiel SCIM erstellen:**

1.  Code-Beispiel Paket am [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2.  Entpacken Sie das Paket und auf Ihrem Windows-Computer an einem Standort wie C:\AzureAD-BYOA-Provisioning-Samples\.
3.  Starten Sie in diesem Ordner die FileProvisioningAgent Lösung in Visual Studio.
4.  Wählen Sie **Tools > Bibliothek Paket-Manager > Paket-Manager Konsole**, und führen Sie die Befehle unter FileProvisioningAgent Projekt der Projektmappe Verweise aufzulösen:

    Install-Package Microsoft.SystemForCrossDomainIdentityManagement Installationspaket Microsoft.IdentityModel.Clients.ActiveDirectory Installationspaket Installationspaket Microsoft.Owin.Diagnostics Microsoft.Owin.Host.SystemWeb

5.  Erstellen Sie das Projekt FileProvisioningAgent.
6.  Starten Sie die Anwendung Eingabeaufforderungsfenster in Windows (als Administrator) und Befehl **cd** das Verzeichnis **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** Ordner ändern.
7.  Führen Sie den Befehl unter < Adresse > IP oder Domänennamen Namens des Windows-Computers ersetzt.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  Unter Windows **Windows Settings > Netzwerk- und Internet-Einstellungen**wählen die **Windows-Firewall > Erweiterte Einstellungen**, und eine **Eingehende Regel** eingehenden Zugriff auf Port 9000.
9.  Ist der Windows-Computer hinter einem Router, müssen der Router übersetzen Netzwerk Zugriff zwischen den Anschluss 9000, die mit dem Internet verbunden und Anschluss 9000 auf dem Windows-Computer konfiguriert werden. Dies ist erforderlich für Azure Anzeige auf diesen Endpunkt in der Cloud.


**So registrieren Sie Beispiel SCIM Endpunkt in Azure AD**

1.  Starten Sie in einem Webbrowser Azure-Verwaltungsportal an https://manage.windowsazure.com.
2.  Navigieren Sie zu **Active Directory > Verzeichnis > [Ihr Verzeichnis] > Applications**, und wählen Sie **Hinzufügen > eine Anwendung aus der Galerie hinzufügen**.
3.  Wählen Sie die **benutzerdefinierte** Registerkarte links, geben Sie einen Namen wie "SCIM Test App" und klicken Sie auf das Häkchensymbol ein app-Objekt erstellen. Beachten Sie, dass das Anwendungsobjekt erstellt die Ziel-app Sie Bereitstellung und Implementierung einmaliges Anmelden für und nicht nur die SCIM Endpunkt darstellen möchten.

![][2]

4.  Wählen Sie im daraufhin eingeblendeten Bildschirm die zweite Schaltfläche **Bereitstellung konfigurieren** .
5.  Geben Sie im Dialogfeld Internet ausgesetzt URL und Port Ihres Endpunkts SCIM. Dies wäre so etwas wie Http://testmachine.contoso.com:9000 oder http://<ip-address>:9000/, wobei < Ip-Adresse > das Internet ist IP ausgesetzt Adresse.  
6.  Klicken Sie auf **Weiter**, und klicken Sie auf die Schaltfläche **Test starten** zu Azure Active Directory Verbindung zur SCIM Endpunkt. Wenn die Versuche fehlschlagen, werden Diagnoseinformationen angezeigt.  
7.  Gelingt die Verbindungsversuche mit Ihrem Web Service klicken Sie auf den verbleibenden Seiten auf **Weiter** , und klicken Sie auf **abschließen** , um das Dialogfeld zu schließen.
8.  Wählen Sie im daraufhin eingeblendeten Bildschirm die dritte Schaltfläche **Konten zuweisen** . Zuordnen Sie in der resultierenden Abschnitt Benutzer und Gruppen die Benutzer oder Gruppen zu der Anwendung
9.  Nach Benutzern und Gruppen zugewiesen werden, klicken Sie auf die Registerkarte **Konfigurieren** im oberen Bereich des Bildschirms.
10. Nach der **Bereitstellung**sicher, dass der Status aktiviert ist. 
11. Klicken Sie unter **Tools**zum Starten der Bereitstellungsprozess **Bereitstellung neu starten** .

Beachten Sie, dass 5 bis 10 Minuten vor der Bereitstellungsprozess werden Anfragen an den Endpunkt SCIM verstreichen kann.  Eine Zusammenfassung der Verbindungsversuche für Dashboard-Registerkarte für die Anwendung bereitgestellt, und einen Bericht zur Bereitstellung Aktivität und Bereitstellung Fehler von Teilnehmerverzeichnis Berichte heruntergeladen werden können.

Der letzte Schritt bei der Überprüfung im Beispiels wird zum Öffnen der Datei TargetFile.csv im Ordner \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug auf Ihrem Windows-Computer. Nach der Bereitstellungsprozess ausführen zeigt diese Datei alle zugewiesen und Benutzer und Gruppen bereitgestellt.

###<a name="development-libraries"></a>Entwicklungsbibliotheken

Um eigene Webdienst entwickeln, der die SCIM-Spezifikation entspricht, zunächst machen Sie sich mit den folgenden Bibliotheken von Microsoft die Entwicklung zu beschleunigen: 

**1:**  Common Language Infrastructure-Bibliotheken Angeboten für die Verwendung mit Sprachen wie C#, Infrastruktur.  Diese Bibliotheken [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)deklariert die Schnittstelle Microsoft.SystemForCrossDomainIdentityManagement.IProvider, in der folgenden Abbildung dargestellt.  Ein Entwickler die Bibliotheken würde diese Schnittstelle einer Klasse implementieren, die, Allgemein als Anbieter bezeichnet werden kann.  Bibliotheken ermöglichen es dem Entwickler einfach einen Webdienst entspricht der Spezifikation SCIM bereitstellen, entweder in Internet Information Services oder einer ausführbaren Common Language Infrastructure Assembly gehostet.  Anfragen an den Webdienst werden Aufrufe von Methoden der Anbieter übersetzt werden vom Entwickler auf einige Identitätsspeicher programmiert wird.    

![][3]

**2:** [Express Routenhandler](http://expressjs.com/guide/routing.html) stehen für die Analyse node.js Anforderungsobjekte versucht, node.js Webdienst aufrufen (definiert durch die SCIM-Spezifikation) darstellt.     

###<a name="building-a-custom-scim-endpoint"></a>Erstellen eines benutzerdefinierten SCIM-Endpunkts

Mit den oben beschriebenen Bibliotheken, können Entwickler diese Bibliotheken ihre Dienste in einer ausführbaren Common Language Infrastructure Assembly oder in Internet Information Services hosten.  Hier ist Beispielcode für hosting-Dienst in eine ausführbare Assembly bei der Adresse http://localhost: 9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Es ist wichtig zu beachten, dass dieser Dienst Authentifizierungszertifikat für eine HTTP-Adresse und Server muss der Stammzertifizierungsstelle eine der folgenden ist: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Ein Serverauthentifizierungszertifikat an einen Anschluss gebunden werden kann, auf einem Windows-Host mit Netzwerk-Shell-Dienstprogramm wie folgt: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Hier ist der Wert für das Argument Certhash Fingerabdruck des Zertifikats, für die Appid-Argument angegebene Wert zufälliger global eindeutigen Bezeichner.  

Um den Dienst in IIS hosten, würde Entwickler eine Common Language Infrastructure Library Codeassembly mit einer Klasse mit dem Namen Start in den Standardnamespace der Assembly erstellen.  Hier ist ein Beispiel für eine solche Klasse: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Behandlung Endpunktauthentifizierung

Anfragen von Azure Active Directory enthalten einen OAuth 2.0 trägertoken.   Jeder Dienst empfängt die Anforderung authentifiziert den Aussteller als Azure Active Directory für erwartete Azure Active Directory Pächter für den Zugriff auf Active Directory Azure Graph-Webdienst.  Das Token der Aussteller einen Anspruch Iss wie "Iss" identifizierten: "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  In diesem Beispiel die Basisadresse der Anspruchswert https://sts.windows.net, Azure Active Directory als der Aussteller identifiziert, während Segment relative Adresse cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, ist ein eindeutiger Bezeichner des Mieters Azure Active Directory für die Token ausgestellt wurde.  Das Token für den Zugriff auf den Azure Active Directory Graph Webdienst ausgestellt, sollte der Bezeichner des Dienstes, 00000002-0000-0000-c000-000000000000, in das Token Aud Forderung sein.  

Entwickler von Microsoft zum Erstellen von SCIM Dienst bereitgestellten Common Language Infrastructure-Bibliotheken können von Azure Active Directory mithilfe des Microsoft.Owin.Security.ActiveDirectory-Pakets wie folgt authentifizieren: 

**1:**  Implementieren Sie in einem Anbieter die Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior-Eigenschaft durch die Zurückgeben einer Methode aufgerufen, wenn der Dienst gestartet wird: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Fügen Sie folgenden Code für diese Methode müssen jede Anforderung an einen der Endpunkte des Diensts wie ein Token ausgestellt von Azure Active Directory im Namen angegebene Mieter für den Zugriff auf Active Directory Azure Graph Webdienst authentifiziert: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Benutzer und Gruppe Schema

Azure Active Directory können zwei Typen von Ressourcen SCIM Webdienste bereitstellen.  Diese Ressourcen sind Benutzer und Gruppen.  

Ressourcen identifizierten Schemabezeichner Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User dieser Protokollspezifikation enthalten: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Standardzuordnung der Attribute von Benutzern in Active Directory Azure Attribute Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User Ressourcen in Tabelle 1 unten.  

Ressourcen werden durch die Schema-ID identifiziert http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabelle 2 unten zeigt das Standardmapping Ressourcengruppen in Azure Active Directory Attribute http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group Attribute.  

###<a name="table-1-default-user-attribute-mapping"></a>Tabelle 1: Standard-Attribut Benutzerzuordnungsdatei

| Azure Active Directory-Benutzer | Urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | aktiv |
| displayName | displayName |
| Fax TelephoneNumber | PhoneNumbers [Typ Eq "Fax"] Wert |
| Vorname | name.givenName |
| jobTitle | Titel |
| e-Mail-Nachrichten | e-Mails [Typ Eq "Arbeit"] Wert |
| mailNickname | externalId |
| Manager | Manager |
| Mobile | PhoneNumbers [Typ Eq "Mobile"] Wert |
| objectId | ID |
| Postleitzahl | Adressen [Typ Eq "Arbeit"] .postalCode |
| Proxy-Adressen | [Geben Sie Eq "andere"] e-Mails. Wert |
| physische Lieferung OfficeName | [Geben Sie Eq "andere"] Adressen. Formatiert |
| streetAddress | Adressen [Typ Eq "Arbeit"] .streetAddress |
| Nachname | name.familyName |
| Telefonnummer | PhoneNumbers [Typ Eq "Arbeit"] Wert |
| Benutzer PrincipalName | Benutzername |


###<a name="table-2-default-group-attribute-mapping"></a>Tabelle 2: Standardmapping Group-Attribut

| Azure Active Directory-Gruppe | http://Schemas.Microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| displayName | externalId |
| e-Mail-Nachrichten | e-Mails [Typ Eq "Arbeit"] Wert |
| mailNickname | displayName |
| Mitglieder | Mitglieder |
| objectId | ID |
| proxyAddresses | [Geben Sie Eq "andere"] e-Mails. Wert |


##<a name="user-provisioning-and-de-provisioning"></a>Benutzer erteilen und entziehen

Die folgende Abbildung zeigt speichern Nachrichten, Azure Active Directory zum Verwalten des Lebenszyklus von einem Benutzer in einer anderen Identität SCIM Dienst sendet.  Das Diagramm zeigt auch, wie ein SCIM Dienst der Common Language Infrastructure Bibliotheken zum Erstellen solcher Dienste von Microsoft bereitgestellten diese Anfragen in Aufrufe der Methoden eines Anbieters übersetzen.  

![][4]
*Abbildung: Benutzer erteilen und entziehen Sequenz*

**1:**  Azure Active Directory wird den Dienst für einen Benutzer mit ExternalId Attributwert das MailNickname-Attribut Wert eines Benutzers in Azure Active Directory Abfragen.  Die Abfrage wird als Hypertext Transfer Protocol-Anforderung, wobei Jyoung ein Beispiel für ein MailNickname eines Benutzers in Azure Active Directory wird ausgedrückt werden: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Wenn der Dienst in der Common Language Infrastructure Bibliotheken von Microsoft für die Implementierung SCIM Services erstellt wurde, wird ein Aufruf der Query-Methode der Dienstanbieter die Anforderung übersetzt.  Hier ist die Signatur dieser Methode: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Hier ist die Definition der Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters-Schnittstelle: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

Im vorstehenden Beispiel einer Abfrage für einen Benutzer mit einem bestimmten Wert für das Attribut ExternalId werden die Werte der Query-Methode übergebenen Argumente wie folgt: 

* Parameter. AlternateFilters.Count: 1
* Parameter. AlternateFilters.ElementAt(0). AttributePath: "ExternalId"
* Parameter. AlternateFilters.ElementAt(0). Vergleichsoperator: ComparisonOperator.Equals
* Parameter. AlternateFilter.ElementAt(0). ComparisonValue: "Jyoung"
* CorrelationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. RequestId"] 

**2:**  Wenn die Antwort auf eine Abfrage an den Dienst für Benutzer mit ExternalId Attributwert das MailNickname-Attribut Wert eines Benutzers in Azure Active Directory keine Benutzer zurückgegeben werden Azure Active Directory anfordern, dass der Dienst in Azure Active Directory für Benutzer bereitstellen.  Hier ist ein Beispiel für einen solchen Antrag: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

Die Common Language Infrastructure Bibliotheken von Microsoft für die Implementierung SCIM Services würde einen Aufruf der Create-Methode der Dienstanbieter die Anforderung übersetzen.  Die Create-Methode hat diese Signatur: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

Bei einer Anforderung eines Benutzers werden der Wert des Arguments Ressource eine Instanz der Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser-Klasse, in der Microsoft.SystemForCrossDomainIdentityManagement.Schemas-Bibliothek definiert.  Die Anforderung zum Benutzer erfolgreich, wenn die Implementierung der Methode muss eine Instanz von der der Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser-Klasse mit dem Wert der ID-Eigenschaft legen den eindeutigen Bezeichner des Benutzers neu bereitgestellt.  

**3:**  Aktualisieren Sie einen Benutzer in einem Identitätsspeicher konfrontiert durch eine SCIM vorhanden gehen Azure Active Directory durch den aktuellen Zustand des Benutzers mit gleichartigen vom Dienst anfordern: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

In einem Dienst mit der Common Language Infrastructure Bibliotheken von Microsoft für die Implementierung SCIM Services erstellt übersetzt die Anforderung in einem Aufruf der Methode Abrufen der Dienstleister.  Hier ist die Signatur der Methode abrufen: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

Die vorstehenden Beispiel einer Anforderung den aktuellen Zustand eines Benutzers abgerufen werden die Werte der Eigenschaften des Objekts als Wert des Arguments Parameter wie folgt: 

* Kennung: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Ist ein Verweisattribut aktualisiert werden Azure Active Directory ab, um festzustellen, ob der aktuelle Wert des Attributs Verweis Identitätsspeicher bereits vom Dienst Produktkatalogsystem Abfrage entspricht der Wert dieses Attributs in Azure Active Directory.  Bei Benutzern ist das einzige Attribut, das der aktuelle Wert so abgefragt werden Manager-Attribut.  Hier ist ein Beispiel einer Anforderung zu bestimmen, ob das Manager-Attribut eines bestimmten Benutzerobjekts derzeit einen bestimmten Wert hat: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Der Wert des Abfrageparameters Attribute Id gibt an, dass wenn ein Benutzerobjekt vorhanden ist, die dem Ausdruck als Wert des Parameters Query Filter entspricht, wird der Dienst reagieren bei einer Urn: Ietf:params:scim:schemas:core:2.0:User oder Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User einschließlich den Wert dieser Ressource ID-Attribut erwartet.  Natürlich ist der Wert des ID-Attributs an den Requestor bekannt – befindet sich auf den Wert des Abfrageparameters Filter; fordert dient tatsächlich entsprechen den Filterausdruck als eine Angabe, ob ein Objekt eine minimale Darstellung einer Ressource anfordern.   

Wenn der Dienst in der Common Language Infrastructure Bibliotheken von Microsoft für die Implementierung SCIM Services erstellt wurde, wird ein Aufruf der Query-Methode der Dienstanbieter die Anforderung übersetzt.  Der Wert der Eigenschaften des Objekts als Wert des Arguments Parameter werden wie folgt: 

* Parameter. AlternateFilters.Count: 2
* Parameter. AlternateFilters.ElementAt(x). AttributePath: "Id"
* Parameter. AlternateFilters.ElementAt(x). Vergleichsoperator: ComparisonOperator.Equals
* Parameter. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* Parameter. AlternateFilters.ElementAt(y). AttributePath: "Manager"
* Parameter. AlternateFilters.ElementAt(y). Vergleichsoperator: ComparisonOperator.Equals
* Parameter. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* Parameter. RequestedAttributePaths.ElementAt(0): "Id"
* Parameter. SchemaIdentifier: "Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User"

Hier möglicherweise der Wert des Indexes X 0 und möglicherweise der Wert y Index 1, oder möglicherweise des Wert von X 1 und der Wert von y möglicherweise 0, abhängig von der Reihenfolge der Ausdrücke des Abfrageparameters Filter.   

**5:**  Hier ist ein Beispiel für eine Anforderung von Azure Active Directory an einen SCIM-Dienst einen Benutzer: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Die Microsoft Common Language Infrastructure-Bibliotheken für SCIM Dienste implementieren würde einen Aufruf der Update-Methode der Dienstanbieter die Anforderung übersetzen.  Hier ist die Signatur dieser Methode: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



Vorstehenden Beispiel einer Anforderung ein Benutzer haben das Objekt als Wert des Arguments Patch diese Eigenschaftswerte: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest als PatchRequest2). Operations.Count: 1
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "Manager"
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referenz: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Wert: 2819c223-7f76-453a-919d-413861904646

**6:**  Einen Benutzer aus einem Identitätsspeicher Produktkatalogsystem von einem Dienst SCIM de bereitstellen sendet Azure Active Directory eine Anforderung folgendermaßen: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Wenn der Dienst in der Common Language Infrastructure Bibliotheken von Microsoft für die Implementierung SCIM Services erstellt wurde, wird ein Aufruf der Delete-Methode der Dienstanbieter die Anforderung übersetzt.   Diese Methode hat diese Signatur: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Das Objekt als Wert des Arguments ResourceIdentifier hat diese Eigenschaftswerte der vorstehenden Beispiel eine Anforderung zum Benutzer aufheben: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Gruppe erteilen und entziehen

Die folgende Abbildung zeigt speichern Nachrichten, Azure Active Directory zum Verwalten des Lebenszyklus von einer Gruppe in einer anderen Identität SCIM Dienst sendet.  Diese Nachrichten unterscheiden sich von Nachrichten für Benutzer auf drei Arten: 

* Das Schema einer Ressource wird als http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group identifiziert werden.  
* Anfragen Gruppen abgerufen werden, dass das Attribut Member jeder Ressource bereitgestellt wird als Antwort auf die Anforderung ausgeschlossen vorgesehen.  
* Anfragen zu bestimmen, ob ein Verweisattribut einen bestimmter Wert werden Anfragen zum Member-Attribut.  

![][5]
*Abbildung: Gruppe erteilen und entziehen Sequenz*

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Benutzer Bereitstellung/aufheben der SaaS-Apps zu automatisieren](active-directory-saas-app-provisioning.md)
- [Anpassen der Attribute Mappings für Benutzer bereitstellen](active-directory-saas-customizing-attribute-mappings.md)
- [Schreiben von Ausdrücken für Attribute Mappings](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereichsfilter für Benutzer bereitstellen](active-directory-saas-scoping-filters.md)
- [Bereitstellen von Benachrichtigungen Konto](active-directory-saas-account-provisioning-notifications.md)
- [Liste der Lernprogramme zur Integration SaaS-Apps](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG

<properties
    pageTitle="Azure AD Key Rollover anmelden | Microsoft Azure"
    description="Dieser Artikel beschreibt Signieren Key Rollover-Tipps für Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Key Rollover in Azure Active Directory signieren

Dieses Thema behandelt müssen die öffentlichen Schlüssel kennen, die sich Sicherheitstoken in Azure Active Directory (Azure AD) verwendet werden. Es ist wichtig zu beachten, dass diese Tasten Rollover in regelmäßigen Abständen und im Notfall kann sofort über zusammengefasst werden. Alle Programme, die Azure AD verwenden sollen programmgesteuert behandeln die wichtigsten Rollover oder eine periodische manuelle Rollover Prozess. Lesen Sie zum Verständnis der Funktionsweise der Schlüssel Auswirkungen Rollover zur Anwendung und zum Aktualisieren Ihrer Anwendung oder einen regelmäßigen manuellen Rollover Prozess Schlüssel Rollover bei Bedarf herstellen.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Übersicht über Signaturschlüssel in Azure AD

Azure AD verwendet Kryptographie Branchenstandards, vertrauen zwischen sich selbst und die Programme, die sie verwenden. In der Praxis funktioniert folgendermaßen: Azure AD verwendet einen Schlüssel, der öffentliche und private Schlüsselpaar besteht. Wenn ein Benutzer eine Anwendung anmeldet, Azure AD für die Authentifizierung verwendet, erstellt Azure AD ein Sicherheitstoken, das Informationen über den Benutzer enthält. Dieses Token ist signiert von Azure AD mithilfe des privaten Schlüssels aus, bevor sie zurück an die Anwendung gesendet werden. Überprüfen, ob das Token gültig und von Azure AD tatsächlich entstanden ist, muss die Anwendung das Token Signatur mit dem öffentlichen Schlüssel von Azure AD des Mieters [OpenID verbinden Discovery](http://openid.net/specs/openid-connect-discovery-1_0.html) oder SAML, WS-Fed [Verbundmetadaten-Dokument innerhalb](active-directory-federation-metadata.md)verfügbar gemacht überprüfen.

Aus Sicherheitsgründen konnte Azure AD Signieren wichtige Rollen in regelmäßigen Abständen und im Notfall sein verlängerter sofort. Jede Anwendung, die in Azure AD integriert sollte ein Schlüssel Rollover Ereignis egal wie oft auftreten vorbereitet werden. Wenn die Anwendung versucht, abgelaufenen Schlüssel die Signatur auf ein Token nicht, schlägt die Anforderung fehl.

Gibt immer mehrere gültiger Schlüssel im Discoverydokument OpenID verbinden und das Verbundmetadaten-Dokument. Die Anwendung sollten bereit sein, der im Beleg angegebenen Schlüssel verwenden, da ein Schlüssel schnell rückgängig gemacht werden könnte, anderen kann der Ersatz usw..

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Um festzustellen, ob Ihre Anwendung beeinflusst und zu tun

Wie hängt die Anwendung wichtige Rollover Variablen wie die Anwendung oder welcher Identität Protokoll und Bibliothek verwendet wurde. In den folgenden Abschnitten beurteilen, ob die häufigsten Anwendungstypen Key Rollover betroffen und Leitlinien zur Aktualisierung der Anwendung unterstützt automatische Rollover oder den Schlüssel manuell zu aktualisieren.

* [Systemeigene Clientanwendungen Zugriff auf Ressourcen](#nativeclient)
* [Web-Applikationen / APIs Zugriff auf Ressourcen](#webclient)
* [Web-Applikationen und APIs Ressourcen mithilfe von Azure App Services erstellt](#appservices)
* [Web-Applikationen / Ressourcen mit .NET owin-OpenID verbinden, WS eingezogen oder WindowsAzureActiveDirectoryBearerAuthentication-Middleware-APIs](#owin)
* [Web-Applikationen / Ressourcen mit .NET Core OpenID verbinden oder JwtBearerAuthentication-Middleware-APIs](#owincore)
* [Web-Applikationen / APIs Ressourcen mit Node.js Passport-Azure-Ad-Modul](#passport)
* [Web-Applikationen und APIs Ressourcen erstellt Visual Studio 2015](#vs2015)
* [ASP.NET-Webanwendungen Ressourcen schützen und mit Visual Studio 2013](#vs2013)
* [Web-APIs Ressourcen schützen und mit Visual Studio 2013](#vs2013_webapi)
* [ASP.NET-Webanwendungen Ressourcen schützen und mit Visual Studio 2012](#vs2012)
* [ASP.NET-Webanwendungen Ressourcen schützen und mit Visual Studio 2010 mit Windows Identity Foundation 2008 o](#vs2010)
* [Web-Applikationen / APIs Ressourcen mit anderen Bibliotheken oder manuell Implementieren eines der unterstützten Protokolle](#other)

Diese Anleitung gilt **nicht** für:

* Anwendung von Azure AD Anwendung Gallery (einschließlich benutzerdefinierte) hinzugefügt haben separate Anleitung den Signaturschlüssel. [Weitere Informationen.](active-directory-sso-certs.md)
* Lokale Anwendung veröffentlicht Anwendungsproxy Signaturschlüssel sorgen besitzen.

### <a name="nativeclient"></a>Systemeigene Clientanwendungen Zugriff auf Ressourcen

Programme, die nur Ressourcen (z.B. zugreifen Microsoft Graph, Schlüsseltresor-API Outlook und anderen Microsoft-APIs) im Allgemeinen nur ein Token abrufen und übergeben es an Besitzer der Ressource. Da sie keine Ressourcen schützen, sie untersuchen Sie das Token und müssen daher nicht sicherstellen, dass diese ordnungsgemäß signiert ist.

Systemeigene Clientanwendungen, desktop oder mobilen, fallen in diese Kategorie und sind daher nicht betroffen das Rollover.

### <a name="webclient"></a>Web-Applikationen / APIs Zugriff auf Ressourcen

Programme, die nur Ressourcen (z.B. zugreifen Microsoft Graph, Schlüsseltresor-API Outlook und anderen Microsoft-APIs) im Allgemeinen nur ein Token abrufen und übergeben es an Besitzer der Ressource. Da sie keine Ressourcen schützen, sie untersuchen Sie das Token und müssen daher nicht sicherstellen, dass diese ordnungsgemäß signiert ist.

Web-Applikationen und web-APIs, die den Fluss nur app verwenden (Clientanmeldeinformationen / Client-Zertifikat), fallen in diese Kategorie und daher nicht durch das Rollover beeinträchtigt.

### <a name="appservices"></a>Web-Applikationen und APIs Ressourcen mithilfe von Azure App Services erstellt

Azure Anwendungsdienste Authentifizierung Autorisierung (EasyAuth) Funktionen hat bereits die notwendige Logik Key Rollover automatisch behandelt.

### <a name="owin"></a>Web-Applikationen / Ressourcen mit .NET owin-OpenID verbinden, WS eingezogen oder WindowsAzureActiveDirectoryBearerAuthentication-Middleware-APIs

Wenn Ihre Anwendung die .NET owin-OpenID verbinden, WS eingezogen oder WindowsAzureActiveDirectoryBearerAuthentication-Middleware wird die notwendige Logik automatisch mit Schlüssel Rollover bereits.

Sie können bestätigen, dass Ihre Anwendung diese anhand einer der folgenden Codeausschnitte in Ihrer Anwendung Startup.cs oder Startup.Auth.cs

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Web-Applikationen / Ressourcen mit .NET Core OpenID verbinden oder JwtBearerAuthentication-Middleware-APIs

Wenn Ihre Anwendung die Middleware .NET Core owin-OpenID verbinden oder JwtBearerAuthentication ist, wird die notwendige Logik automatisch mit Schlüssel Rollover bereits.

Sie können bestätigen, dass Ihre Anwendung diese anhand einer der folgenden Codeausschnitte in Ihrer Anwendung Startup.cs oder Startup.Auth.cs

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Web-Applikationen / APIs Ressourcen mit Node.js Passport-Azure-Ad-Modul

Wenn Ihre Anwendung Node.js Passport Ad-Modul ist, wird die notwendige Logik automatisch mit Schlüssel Rollover bereits.

Bestätigen Sie, dass Ihre Anwendung Passport-Anzeige der folgenden Ausschnitt in der Anwendung app.js suchen

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Web-Applikationen und APIs Ressourcen erstellt Visual Studio 2015

Wenn die Anwendung wurde mit einer Webvorlage Anwendung in Visual Studio 2015 und **Und Schule Geschäftskonten** Menü **Authentifizierung ändern** , wird die notwendige Logik automatisch mit Schlüssel Rollover bereits. Diese Logik in die Middleware owin-OpenID verbinden eingebettet ruft speichert den Schlüssel Discoverydokument OpenID verbinden und regelmäßig aktualisiert.

Wenn Authentifizierung Projektmappe manuell hinzugefügt, möglicherweise die Anwendung nicht die erforderlichen Schlüssel Rollover-Logik. Sie müssen selbst schreiben oder führen die Schritte [Web-Applikationen / APIs mit anderen Bibliotheken oder manuell Implementieren eines der unterstützten Protokolle.](#other).

### <a name="vs2013"></a>ASP.NET-Webanwendungen Ressourcen schützen und mit Visual Studio 2013

Wenn Ihre Anwendung unter Verwendung einer Webvorlage Anwendung Visual Studio 2013 erstellt wurde und **Organisationseinheiten Konten** Menü **Authentifizierung ändern** , wird die notwendige Logik automatisch mit Schlüssel Rollover bereits. Diese Logik speichert der Organisation eindeutigen Bezeichner sowie die Unterzeichnung Informationen in zwei Tabellen mit dem Projekt verbundenen. Sie finden die Verbindungszeichenfolge für die Datenbank in der Datei Web.config des Projekts.

Wenn Authentifizierung Projektmappe manuell hinzugefügt, möglicherweise die Anwendung nicht die erforderlichen Schlüssel Rollover-Logik. Sie müssen selbst schreiben oder führen die Schritte [Web-Applikationen / APIs mit anderen Bibliotheken oder manuell Implementieren eines der unterstützten Protokolle.](#other).

Die folgenden Schritte können Sie überprüfen, ob die Logik in Ihrer Anwendung ordnungsgemäß funktioniert.

1. Visual Studio 2013 öffnen Sie die Projektmappe, und klicken Sie dann auf die Registerkarte **Server-Explorer** im rechten Fensterbereich.
2. Erweitern Sie **Data Connections**, **DefaultConnection**und **Tabellen**. Suchen Sie **IssuingAuthorityKeys** Tabelle, rechtsklicken Sie darauf und dann auf **Tabellendaten anzeigen**.
3. In der Tabelle **IssuingAuthorityKeys** werden mindestens eine Zeile der Fingerabdruckwert für den Schlüssel entspricht. Löschen Sie alle Zeilen in der Tabelle.
4. Klicken Sie **Mieter** Tabelle und dann auf **Daten anzeigen**.
5. In der Tabelle **Mieter** werden mindestens eine Zeile auf ein eindeutiges Verzeichnis Mieter Bezeichner entspricht. Löschen Sie alle Zeilen in der Tabelle. Wenn Sie Zeilen in der Tabelle **Mieter** und **IssuingAuthorityKeys** Tabelle nicht löschen, erhalten Sie Fehler zur Laufzeit.
6. Erstellen Sie und führen Sie die Anwendung. Nachdem Sie Ihrem Konto angemeldet haben, können Sie die Anwendung beenden.
7. Zurück zum **Server-Explorer** und die Werte in der Tabelle **IssuingAuthorityKeys** und **Mieter** . Sie werden feststellen, dass sie mit den entsprechenden Informationen aus den Verbundmetadaten-Dokument automatisch aufgefüllt wurde.

### <a name="vs2013"></a>Web-APIs Ressourcen schützen und mit Visual Studio 2013

Wenn Sie eine Web API-Anwendung in Visual Studio 2013 mithilfe der Web-API-Vorlage erstellt und dann **Organisationseinheiten Konten** Menü **Ändern Authentifizierung** ausgewählt, verfügen Sie bereits über die notwendige Logik in Ihre Anwendung.

Wenn Sie Authentifizierung manuell konfiguriert haben, gehen Sie Informationen zum Konfigurieren der Web-API die Informationen automatisch aktualisieren.

Der folgende Codeausschnitt veranschaulicht, wie die neuesten Schlüssel aus den Verbundmetadaten-Dokument und verwenden Sie den [JWT Tokenhandler](https://msdn.microsoft.com/library/dn205065.aspx) Token überprüft. Der Codeausschnitt wird vorausgesetzt, dass caching Mechanismus zum Beibehalten des Schlüssels verwendet zukünftige Token von Azure AD zu bestätigen sei in einer Konfigurationsdatei oder anderswo.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>ASP.NET-Webanwendungen Ressourcen schützen und mit Visual Studio 2012

Wenn die Anwendung in Visual Studio 2012 erstellt wurde, verwendet Sie wahrscheinlich die Identität und die Access-Tool zum Konfigurieren einer Anwendung. Dürfte auch mit [Validierung Aussteller Namen Registrierung (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). Die VINR ist verantwortlich für Informationen zu vertrauenswürdigen Identitätsanbieter (Azure AD) und der Schlüssel verwendet, die von ihnen ausgestellte Token überprüft. Die VINR erleichtert auch die automatisch der wichtigen Informationen in der Datei Web.config durch Herunterladen des neuesten Verbundmetadaten-Dokuments mit dem Verzeichnis verbunden wird überprüft, ob die Konfiguration mit dem aktuellen Dokument veraltet ist und Aktualisieren der Anwendung auf den neuen Schlüssel verwenden.

Wenn Sie Ihre Anwendung Codebeispiele oder Exemplarische Vorgehensweise Dokumentation von Microsoft erstellt, ist Key Rollover-Logik bereits im Projekt enthalten. Sie werden feststellen, dass im folgenden Code bereits im Projekt vorhanden ist. Wenn die Anwendung nicht diese Logik folgt noch hinzufügen und überprüfen, ob es ordnungsgemäß funktioniert.

1. Fügen Sie einen Verweis auf die **System.IdentityModel** -Assembly für das entsprechende Projekt im **Projektmappen-Explorer**.
2. Öffnen Sie die Datei **Global.asax.cs** und fügen Sie die folgende using-Direktiven:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Fügen Sie die folgende Methode der **Global.asax.cs** -Datei:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Rufen Sie die Methode **RefreshValidationSettings()** in der **Application_Start()** -Methode **Global.asax.cs** dargestellt:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Nachdem diese Schritte werden Web.config Ihrer Anwendung mit den neuesten Informationen aus der Verbundmetadaten-Dokument, einschließlich der neuesten aktualisiert. Dieses Update erfolgt jedes Mal der Anwendungspool in IIS-Freigabe; IIS ist standardmäßig Applikationen alle 29 Stunden wiederverwenden.

Gehen Sie folgendermaßen vor, um sicherzustellen, dass die wichtigsten Rollover-Logik arbeiten.

1. Nachdem Sie überprüft haben, dass den obige Code Ihrer Anwendung die Datei **Web.config** , und navigieren Sie zu den **<issuerNameRegistry>** Block gezielt für die folgenden Zeilen:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. In der **<add thumbprint=””>** festlegen, ändern Sie den Fingerabdruckwert jedes Zeichen durch eine andere ersetzen. Speichern Sie die Datei **Web.config** .

3. Erstellen Sie die Anwendung, und führen Sie es. Wenn Sie den Vorgang abschließen können, wird die Anwendung den Schlüssel erfolgreich aktualisiert, erforderliche Informationen aus Ihrem Verzeichnis Verbundmetadaten-Dokument herunterladen. Wenn Sie beim Anmelden Probleme auftreten, sorgen die Änderungen in der Anwendung korrekt Thema [Hinzufügen Anmeldung Ihr Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) lesen oder herunterladen und überprüfen das folgende Codebeispiel: [Multi-Tenant Cloudanwendung Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>ASP.NET-Webanwendungen Ressourcen schützen und mit Visual Studio 2008 oder 2010 erstellt und Windows Identity Foundation (WIF) V1. 0 für .NET 3.5

Wenn Sie eine Anwendung auf WIF 1.0 erstellt, gibt keinen bereitgestellte Mechanismus Anwendungskonfiguration Verwendung eines neuen Schlüssels automatisch aktualisiert.

- *Einfachste Methode* Verwenden Sie FedUtil Werkzeuge im WIF-SDK, das aktuelle Dokument abrufen und aktualisieren Sie Ihre Konfiguration enthalten.
- Aktualisieren Sie die Anwendung auf .NET 4.5 enthält die neueste Version von WIF im System-Namespace befindet. [Überprüfen der Aussteller Namen Registrierung (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) können Sie automatische Updates der Anwendungskonfiguration.
- Führen Sie eine manuelle Rollover gemäß der Anleitung am Ende dieser Leitfaden.

Anleitung mit der FedUtil die Konfiguration aktualisiert:

1. Überprüfen Sie die WIF 1.0 SDK für Visual Studio 2008 oder 2010 auf dem Entwicklungscomputer installiert. Sie können [hier herunterladen](https://www.microsoft.com/en-us/download/details.aspx?id=4451) , wenn Sie nicht noch installiert haben.
2. Öffnen Sie in Visual Studio die Projektmappe mit der rechten Maustaste in des entsprechenden Projekts und wählen Sie **Update Verbundmetadaten**. Wenn diese Option nicht verfügbar ist, wurde FedUtil oder WIF 1.0 SDK nicht installiert.
3. Wählen Sie aus dem **Update** aktualisiert die Verbundmetadaten beginnen. Haben Sie Zugriff auf die Server-Umgebung, in dem die Anwendung gehostet wird, können Sie optional FedUtils [automatischen Metadaten Planer aktualisieren](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klicken Sie auf **Fertig stellen** , um den Vorgang abzuschließen.

### <a name="other"></a>Web-Applikationen / APIs Ressourcen mit anderen Bibliotheken oder manuell Implementieren eines der unterstützten Protokolle

Verwenden eine anderen Bibliothek oder manuell implementiert unterstützte Protokolle müssen Sie überprüfen, die Bibliothek oder die Implementierung, um sicherzustellen, dass der Schlüssel Discoverydokument OpenID herstellen oder das Verbundmetadaten-Dokument abgerufen werden. Eine Möglichkeit zu prüfen ist eine Suche in der oder der Bibliothek Code für alle Aufrufe OpenID Discovery-Dokument oder das Verbundmetadaten-Dokument.

Wenn Schlüssel irgendwo gespeichert oder in die Anwendung hartcodiert manuell Abrufen der Schlüssel und aktualisieren sie entsprechend ausführen manuelle Rollover gemäß der Anleitung am Ende dieser Leitfaden. **Wird dringend empfohlen, verbessern die Anwendung unterstützt automatische Rollover** mit einer der Methoden in diesem Artikel zu zukünftigen Störungen Gliedern und Aufwand steigt Azure AD Rollover Trittfrequenz oder ein Notfall Out-of-Band-Rollover.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Testen die Anwendung bestimmen, ob sie betroffen sind

Sie können überprüfen, ob die Anwendung automatisch wichtige Rollover unterstützt die Skripts downloaden und im folgenden beschrieben [dieses Repository GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Manuelle Rollover durchführen, wenn Sie Anwendung automatische Rollover nicht unterstützt

Wenn Ihre Anwendung **nicht** automatische Rollover-Unterstützung verfügt, müssen Sie einen Prozess einrichten, der regelmäßig überwacht Azure AD-Signaturschlüssel und manuelle Rollover entsprechend führt. [Diese GitHub Repository](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) enthält Skripts und Anleitung dazu.

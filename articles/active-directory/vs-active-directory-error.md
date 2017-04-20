<properties 
    pageTitle="Fehler bei der Authentifizierung Erkennung" 
    description="Active Directory festgestellt-Verbindung inkompatible Authentifizierungstyp" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Fehler bei der Authentifizierung Erkennung

Während der vorherigen Authentifizierungscode erkennen, ermittelt der Assistent inkompatible Authentifizierungstyp.   

##<a name="what-is-being-checked"></a>Was wird überprüft?

**Hinweis:** Um frühere Authentifizierungscode in einem Projekt erkannt, muss das Projekt erstellt werden.  Wenn dieser Fehler und keiner vorherigen Authentifizierungscode in Ihrem Projekt, neu erstellen und versuchen Sie es erneut.

###<a name="project-types"></a>Projekttypen

Der Assistent überprüft, welche Art von Projekt entwickeln, damit diese Rechte Authentifizierungslogik in das Projekt einfügen kann.  Ist Controller aus `ApiController` im Projekt Projekt gilt ein WebAPI Projekt.  Wenn nur Domänencontroller, die Ableitung `MVC.Controller` im Projekt Projekt gilt ein MVC-Projekt.  Alles andere wird vom Assistenten nicht unterstützt.  WebForms-Projekte werden derzeit nicht unterstützt.

###<a name="compatible-authentication-code"></a>Kompatible Authentifizierungscode

Der Assistent überprüft auch für die Einstellung, die zuvor mit dem Assistenten konfiguriert wurden oder mit dem Assistenten kompatibel.  Sind alle vorhanden, eintrittsinvarianter Fall gilt, und der Assistent wird geöffnet und zeigt die Einstellungen.  Sind nur einige der Einstellungen vorhanden, gilt einen Fehlerfall.

Ein MVC-Projekt überprüft der Assistent für alle folgenden Einstellungen aus der vorherigen Verwendung des Assistenten:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Darüber hinaus überprüft der Assistent die folgenden Einstellungen im Web API-Projekt aus der vorherigen Verwendung des Assistenten:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Inkompatible Authentifizierungscode

Abschließend versucht der Assistent, Versionen von Authentifizierungscode erkennen, die mit früheren Versionen von Visual Studio konfiguriert wurden. Diese Fehlermeldung bedeutet dies, dass das Projekt einen inkompatible Authentifizierungstyp enthält. Der Assistent erkennt die folgenden Arten von Authentifizierung aus früheren Versionen von Visual Studio:

* Windows-Authentifizierung 
* Einzelne Benutzerkonten 
* Organisationseinheiten Konten 
 

Um Windows-Authentifizierung in einer MVC-Projekt erkennen, sucht der Assistent nach dem `authentication` Element aus der Datei **web.config** .

<pre>
    &lt;Konfiguration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;Authentication Mode = "Windows" /&gt;</span>
        &lt;System.Web&gt;
    &lt;/Configuration&gt;
</pre>

Zum Erkennen von Windows-Authentifizierung in einem Projekt Web API sucht der Assistent nach dem `IISExpressWindowsAuthentication` Element **csproj** -Datei des Projekts:

<pre>
    &lt;Project&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;aktiviert&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/Projekt        &gt;
</pre>

Um einzelne Benutzerkonten Authentifizierung erkennen, sucht der Assistent nach Package-Elements aus der Datei **Packages.config** .

<pre>
    &lt;Pakete&gt;
        <span style="background-color: yellow">&lt;id="Microsoft.AspNet.Identity.EntityFramework-Paket" Version = "2.1.0" TargetFramework = "net45" /&gt;</span>
    &lt;/Pakete&gt;
</pre>

Um eine alte Form der Organisationseinheit Kontoauthentifizierung erkennen, sucht der Assistent für das folgende Element in **web.config**:

<pre>
    &lt;Konfiguration&gt;
        &lt;AppSettings&gt;
            <span style="background-color: yellow">&lt;fügen Sie Key = "Ida: Bereich" Wert = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/Configuration&gt;
</pre>

Ändern Sie den Authentifizierungstyp inkompatible Authentifizierungstyp entfernen und den Assistenten erneut ausführen.

Weitere Informationen finden Sie unter [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md).
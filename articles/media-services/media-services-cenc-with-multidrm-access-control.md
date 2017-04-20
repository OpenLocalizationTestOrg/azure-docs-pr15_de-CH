<properties 
    pageTitle="CENC Zugriffskontrolle mit Multi-DRM: eine Referenzdesign und Implementierung auf Azure und Azure Media Services | Microsoft Azure" 
    description="Weitere Informationen über das Microsoft® reibungslose Streaming Client Portieren der Lizenzierung." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC Zugriffskontrolle mit Multi-DRM: eine Referenzdesign und Implementierung auf Azure und Azure Media Services

##<a name="key-words"></a>Schlüsselwörter
 
Azure Active Directory Azure Media Services Azure Media Player dynamische Verschlüsselung Lizenz Lieferung PlayReady Widevine, FairPlay, allgemeine Encryption(CENC), Multi-DRM, Axinom, Bindestrich, EME, MSE, JSON Web Token (JWT), Ansprüche, Browser, Key Rollover, symmetrischen Schlüssel, asymmetrischen Schlüssel, OpenID verbinden X509 Zertifikat. 

##<a name="in-this-article"></a>In diesem Artikel

In diesem Artikel werden die folgenden Themen behandelt:

- [Einführung](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Übersicht über die in diesem Artikel](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Referenz-design](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Mapping-Design-Technologie für die Implementierung](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Implementierung](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Umsetzung](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [Einige Probleme in Implementierung](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [Weitere Themen für die Implementierung](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [HTTP oder HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Azure Active Directory signieren Key rollover](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Das Zugriffstoken ist?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Was Live-Streaming?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Was Lizenzserver außerhalb Azure Media Services?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Was passiert, wenn ich einen benutzerdefinierten STS verwenden?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [Abgeschlossene und testen](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Benutzername](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Mit verschlüsselten Medien Extensions for PlayReady](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [Verwenden von EME für Widevine](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Nicht berechtigt Benutzer](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Ausführen von benutzerdefinierten Secure Token Service](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Zusammenfassung](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>Einführung

Es ist ist eine komplexe Aufgabe entworfen und erstellt ein DRM-Subsystem für eine OTT oder online-Streaming-Lösung. Und es ist üblich für Operatoren/video Anbieter diese an speziellen DRM-Dienstanbieter überlassen. Das Ziel dieses Dokuments ist eine Referenz-Design und Implementierung von End-to-End-DRM-Subsystem OTT oder Online-Streaming-Lösung.

Gezielte Leser dieses Dokuments sind Ingenieure DRM-Subsystem OTT online streaming/multi-screen Solutions oder Reader DRM-Subsystem interessiert. Die Annahme ist, dass Leser mit mindestens einem DRM-Technologie auf dem Markt wie PlayReady, Widevine, FairPlay oder Adobe Access vertraut sind.

Durch DRM auch wir CENC (Common Verschlüsselung) mit Multi-DRM. Ein Trend im Online-streaming und OTT Branche ist mit CENC mit multi-native-DRM auf verschiedenen Clientplattformen ist eine Schicht aus der vorherigen Trend mit einem DRM Client SDK für Clientplattformen. Bei CENC mit mehreren native DRM PlayReady und Widevine gemäß der Spezifikation [Allgemeine Verschlüsselung (ISO/IEC 23001 7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) verschlüsselt.

CENC Vorteile mit mehreren DRM wie folgt:

1. Da eine einzelnes Verschlüsselung Verarbeitung verwendet wird für verschiedene Plattformen mit systemeigenen DRM Verschlüsselung reduziert;
1. Reduzierung der Kosten für verschlüsselte Ressourcen verwalten, da nur eine einzige Kopie der verschlüsselten Elemente erforderlich ist;
1. Eliminiert Kosten DRM Clientlizenzen systemeigene DRM-Client in der Regel kostenlos auf seiner einheitlichen Plattform ist.

Microsoft wurde eine aktive Promoter STRICHS und CENC mit einigen wichtigen Branche. Microsoft Azure Media Services bietet Unterstützung von Strich und CENC. Aktuelle Ankündigungen finden Sie unter Mingfeis Blogs: [Ankündigung von Google Widevine Lizenz Lieferdienste in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)und [Azure Media Services fügt Google Widevine Verpackung Multi-DRM Strom](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Übersicht über die in diesem Artikel

Dieser Artikel enthält Folgendes:

1. Stellt ein Verweis von Multi-DRM-CENC mit DRM-Subsystem;
1. Bietet eine Implementierung des Verweis auf Microsoft Azure-Azure Media Services-Plattform.
1. Entwurf und Implementierung von Themen behandelt.

In diesem Artikel behandelt "Multi-DRM" Folgendes:

1. Microsoft PlayReady
1. Google-Widevine
1. Apple FairPlay (nicht unterstützt von Azure Media Services)

In der folgenden Tabelle werden die native Plattform-Native Anwendung und jedes DRM unterstützten Browser.

**Client-Plattform**|**Unterstützung für native DRM**|**Browser-App**|**Streaming-Formate**
----|------|----|----
**Smart-TVs, Operator STB OTT STB**|PlayReady hauptsächlich Widevine und/oder andere|Linux, Opera, WebKit, andere|Verschiedene Formate
**Windows 10 Geräte (Windows-PC Windows Tablets Windows Phone, Xbox)**|PlayReady|MS Edge/IE11/EME<br/><br/><br/>UWP|Strich (für HLS, PlayReady wird nicht unterstützt)<br/><br/>Strich, Smooth Streaming (für HLS, PlayReady wird nicht unterstützt) 
**Geräte (Telefon, Tablet, TV)**|Widevine|Chrome-EME|STRICH
**iOS (iPhone, iPad), OS X-Clients und Apple TV**|FairPlay|Safari 8 / EME|HLS
**Plugin: Adobe Primetime**|Primetime Zugriff|Browser-plugin|HDS HLS

In Anbetracht der derzeitigen Bereitstellung für jede DRM Dienst normalerweise möchten 2 oder 3 DRM um sicherzustellen, dass alle Endpunkte optimal Adresse implementieren.

Es gibt ein Kompromiss zwischen der Komplexität der Logik und der Komplexität auf der Clientseite ein gewisses Benutzerfunktionalität auf den verschiedenen Clients zu.

Um Ihre Auswahl zu treffen, beachten Sie Folgendes:

- PlayReady erfolgt direkt in jedem Windows-Gerät einige Android-Geräte und Software-SDKs auf nahezu jeder Plattform erhältlich
- Widevine wird direkt in alle Android-Geräte, Chrom und andere Geräte implementiert.
- FairPlay steht nur für iOS und Mac OS-Clients oder über iTunes.

Dies wäre normalerweise Multi-DRM:

- Option 1: PlayReady und Widevine
- Option 2: PlayReady, Widevine und FairPlay


## <a name="a-reference-design"></a>Referenz-design

In diesem Abschnitt präsentieren wir eine Referenzdesign Technologie zur Implementierung unabhängig ist.

Ein DRM-Subsystem kann folgenden Komponenten enthalten:

1. Schlüsselmanagement
1. DRM-Verpackung
1. DRM-Lizenz Lieferung
1. Berechtigungen überprüfen
1. Authentifizierung
1. Player
1. Ursprungs-CDN

Das folgende Diagramm veranschaulicht das hohe Zusammenwirken der Komponenten in einem DRM-Subsystem.

![DRM-Subsystem mit CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
Es gibt drei grundlegende "Ebenen" im Entwurf:

1. Sichern von Office-Ebene (in Schwarz) nicht extern verfügbar gemacht werden.
1. "DMZ" Layer (Blau), die alle Endpunkte mit öffentlichen;
1. Öffentliche Internet-Schicht (hellblau) mit CDN mit Datenverkehr über Internetzugang.
 
Soll ein Content Management-Tool zur Steuerung von DRM-Schutz, egal ist statische oder dynamische Verschlüsselung. Die Eingaben für DRM-Verschlüsselung sollte Folgendes umfassen:

1. MBR-Videoinhalte;
1. Schlüssel;
1. Lizenz Übernahme URLs.

Zeit der Wiedergabe erfolgt die auf hoher Ebene:

1. Benutzerauthentifizierung;
1. Autorisierungstoken wird für den Benutzer erstellt.
1. DRM-geschützte Inhalte (Manifest) ist Player heruntergeladen.
1. Player sendet Übernahme Lizenzabfrage an Lizenzserver mit ID und Autorisierung token.

Bevor Sie fortfahren mit dem nächsten Thema kurz das Design des Schlüsselverwaltungsdienstes.

**ContentKey – Asset**|**Szenario**
------|---------------------------
1: 1|Der einfachste Fall. Es bietet die beste Kontrolle. Aber in der Regel dadurch die höchste Lizenz Versandkosten. Anforderung ist auf mindestens eine Lizenz für jede geschützte Ressource erforderlich.
1-n|Sie können demselben Inhaltsschlüssel für mehrere Anlagen. Für alle Anlagen in einer logischen Gruppe oder ein Genre Teil Genre (oder Film gen) verwenden Sie z. B. einen Content Schlüssel.
N-1|Mehreren Schlüsseln werden für jede Anlage. <br/><br/>Benötigen Sie dynamische CENC Schutz mit Multi-DRM für MPEG-DASH und dynamische Verschlüsselung AES-128 HLS, benötigen Sie zwei separate Schlüsseln, jeder mit seiner eigenen ContentKeyType. (Für dynamische CENC Schutz Content Schlüssel ContentKeyType.CommonEncryption sollte für Content für dynamische AES-128-Verschlüsselung verwendeten Schlüssel verwendet, sollte ContentKeyType.EnvelopeEncryption verwendet werden.)<br/><br/>Beispielsweise CENC Schutz Strich Inhalt theoretisch ein Schlüssel zu Videodaten und ein anderer Inhalte zu audio-Stream verwendet werden kann. 
Viele-zu - viele|Kombination der beiden obigen Szenarios: eine Gruppe von Schlüsseln für jeweils mehrere Elemente in derselben Anlage "Gruppe" verwendet.


Ein weiterer wichtiger Faktor ist die Verwendung der permanente und nicht permanente Lizenzen.

Warum sind diese Informationen wichtig? 

Sie haben direkten Einfluss auf Lizenzkosten öffentliche Cloud für die Übermittlung der Lizenz verwenden. Betrachten wir zwei verschiedene Design Fällen veranschaulicht:



1. Abonnement: permanente Lizenz und 1: n-Content Schlüssel Anlage Zuordnung verwenden. Z.b. für alle kinderfilme verwenden wir einen einzelnen Content Schlüssel für die Verschlüsselung. In diesem Fall: 

    Angefordert für alle Kinder Filme/Gerät # Lizenzen insgesamt = 1

1. Abonnement: nicht dauerhafte Lizenz und 1: 1-Zuordnung zwischen Schlüssel und Ressourcen verwenden. In diesem Fall:

    Angefordert für alle Kinder Filme/Gerät # Lizenzen insgesamt = [# Filme beobachtete] x [# Sessions]

Siehe einfach, ergeben zwei verschiedene Entwürfe daher sehr unterschiedliche Lizenz Anforderung Muster Lizenz Übermittlung Wenn Zustelldienst Lizenz von öffentlichen Cloud wie Azure Media Services bereitgestellt wird.

## <a name="mapping-design-to-technology-for-implementation"></a>Mapping-Design-Technologie für die Implementierung

Anschließend ordnen wir unsere Allgemeine Entwurfsaspekte auf Microsoft Azure-Azure Media Services-Plattform durch Angabe der Technologie für jeden Baustein.

Die folgende Tabelle zeigt die Zuordnung:

**Baustein**|**Technologie**
------|-------
**Player**|[Azure MediaPlayer](https://azure.microsoft.com/services/media-services/media-player/)
**Identitätsprovider (IDP)**|Azure Active Directory
**Sicherheitstokendiensts (STS)**|Azure Active Directory
**DRM-Schutz Workflow**|Azure Media Services dynamischen Schutz
**DRM-Lizenz Lieferung**|1. Azure Media Services Lizenz Lieferung (PlayReady, Widevine, FairPlay) oder <br/>2. Axinom Lizenzserver <br/>3. benutzerdefinierte PlayReady-Lizenzserver
**Ursprung**|Azure Media Services Streaming-Endpunkt
**Schlüsselmanagement**|Für die Implementierung des Verweis benötigt nicht
**Content-Management**|Ein C#-Konsolenanwendungsprojekt

In anderen Worten werden Identität Anbieter (IDP) und Secure Token Service (STS) Azure AD. Für Player verwenden wir [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/). Azure Media Services und Azure Media Player unterstützen Strich und CENC mit Multi-DRM.

Das folgende Diagramm zeigt die allgemeine Struktur und mit über Technologie-Zuordnung.

![CENC auf AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Um dynamische CENC Verschlüsselung einzurichten, verwendet das Content-Management-Tool die folgenden Eingaben:

1. Inhalt öffnen;
1. Schlüssel aus Schlüssel generieren/Management;
1. Lizenz Übernahme URLs;
1. Eine Liste mit Informationen von Azure AD.

Content-Management-Tool wird ausgegeben:

1. ContentKeyAuthorizationPolicy mit der Spezifikation auf wie Lizenz Delivery JWT-Token und DRM-Lizenz Spezifikationen überprüft;
1. AssetDeliveryPolicy mit Spezifikationen Streaming Format, DRM-Schutz und Lizenz Übernahme URLs.

Während der Laufzeit ist als unten:

1. Bei der Benutzerauthentifizierung wird ein JWT-Token generiert.
1. JWT-Token enthaltenen Ansprüche gehört die Gruppe Objekt-ID "EntitledUserGroup" mit "Gruppen" Anspruch. Dies wird für die Übergabe von "Anspruch prüfen" verwendet.
1. Player Downloads Clientmanifest ein CENC geschützte Inhalte und "sieht" Folgendes:
    1. Schlüssel-ID 
    1. der Inhalt ist CENC geschützt,
    1. Lizenz Übernahme URLs.

1. Player fordert Lizenz Übernahme basierend auf dem Browser/DRM unterstützt. Anforderung Übernahme License key ID und JWT-Token auch gesendet. Lizenz-Zustelldienst überprüft JWT-Token und die Erteilung der erforderlichen Lizenz enthalten.

##<a name="implementation"></a>Implementierung

###<a name="implementation-procedures"></a>Umsetzung

Die Implementierung umfasst die folgenden Schritte aus:

1. Test Anlage(n) vorbereiten: ein Testvideo zur Multi-Bitrate codieren/Paket fragmentiert MP4 in Azure Media Services. Diese Anlage ist nicht DRM-geschützt. DRM-Schutz von dynamischen Schutz später erfolgt.
1. Schlüssel-ID und Inhalt erstellen Key (optional Schlüsselwert). Zweck Schlüsselverwaltungssystem ist nicht erforderlich, da mit einem einzigen Satz von geht Schlüssel-ID und Schlüssel für eine Reihe von Tests Vermögenswerte;
1. Verwenden Sie AMS-API Multi-DRM-Lizenz Lieferdienste für die Test-Anlage konfiguriert. Bei Verwendung von benutzerdefinierten Lizenzserver durch Ihr Unternehmen oder Kreditoren des Unternehmens statt Lizenz Dienste in Azure Media Services können Sie diesen Schritt überspringen und Lizenz Übernahme URLs Schritt zum Konfigurieren der Lizenz Lieferung angeben. AMS-API musste einige detaillierte Konfigurationen Autorisierung richtlinieneinschränkung, Lizenz Antwort Vorlagen für unterschiedliche DRM-Lizenz usw. angeben. Zu diesem Zeitpunkt stellt Azure-Portal keine noch benötigte UI für diese Konfiguration bereit. API-Ebene Informationen und Beispielcode Julia Kornichs Dokuments: [PlayReady verwenden oder Widevine dynamische gemeinsame Verschlüsselung](media-services-protect-with-drm.md). 
1. Verwenden Sie AMS-API konfigurieren Anlage Lieferung Richtlinie für die Test-Anlage. API-Ebene Informationen und Beispielcode Julia Kornichs Dokuments: [PlayReady verwenden oder Widevine dynamische gemeinsame Verschlüsselung](media-services-protect-with-drm.md).
1. Erstellen Sie und konfigurieren Sie einen Mandanten Azure Active Directory in Azure.
1. Einige Benutzerkonten und Gruppen in Ihrem Mandanten Azure Active Directory erstellen: Erstellen Sie mindestens "EntitledUser" zu gruppieren und einen Benutzer zu dieser Gruppe hinzufügen. Benutzer in dieser Gruppe Lizenzerwerb Anspruch Kontrollkästchen übergibt und Benutzer nicht in dieser Gruppe nicht Absenderauthentifizierung übergeben werden keine Lizenz. Mitglied der Gruppe "EntitledUser" ist erforderlich "Gruppen" Anspruch ausgestellt von Azure AD JWT-Token. Konfigurieren von Multi-DRM-Lizenz Delivery Services Schritt sollte dieses Anspruchs angegeben werden.
1. Erstellen einer ASP.NET MVC-Anwendung, die Hosten von video-Player. ASP.NET app wird mit Benutzerauthentifizierung gegen Mieter Azure Active Directory geschützt werden. Ordnungsgemäße Anträge werden im Zugriffstoken nach Authentifizierung erhalten berücksichtigt. OpenID verbinden-API wird für diesen Schritt empfohlen. Sie müssen die folgenden NuGet-Pakete installieren:
    - Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
    - Install-Package Microsoft.Owin.Security.OpenIdConnect
    - Install-Package Microsoft.Owin.Security.Cookies
    - Install-Package Microsoft.Owin.Host.SystemWeb
    - Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
1. Erstellen Sie einen Player mit [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/). [Azure Media Player ProtectionInfo-API](http://amp.azure.net/libs/amp/latest/docs/) können Sie die DRM-Technologie auf andere DRM-Plattform an.
1. Testmatrix:

**DRM**|**Browser**|**Ergebnis für berechtigt Benutzer**|**Ergebnis für den Benutzer nicht berechtigt**
---|---|-----|---------
**PlayReady**|MS Kante oder IE11 unter Windows 10|Erfolgreich|Fehler
**Widevine**|Chrome unter Windows 10|Erfolgreich|Fehler
**FairPlay** |TBD||

George Trifonov Azure Media Services Team geschrieben hat einen Blog bietet detaillierte Schritte in Azure Active Directory für eine ASP.NET MVC-Player-app einrichten: [Integration von Azure Media Services owin-MVC Anwendung in Azure Active Directory basieren und Bereitstellung von Inhalten basierend auf JWT Ansprüche beschränken](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George hat einen Blog auf [JWT token Authentifizierung in Azure Media Services und dynamische Verschlüsselung](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)geschrieben. Und seine [Beispiel Azure AD-Integration in Azure Media Services Key Lieferung](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Informationen zu Azure Active Directory:

- Entwicklerinformationen finden Sie im [Entwicklerhandbuch für Azure Active Directory](../active-directory/active-directory-developers-guide.md).
- Informationen für Administratoren finden Sie in [Der Azure AD-Verzeichnis zu verwalten](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Einige Probleme in Implementierung

Es gibt einige "Probleme" in der Implementierung. Hoffentlich kann folgende "Punkte" Ihnen zur Problembehandlung, falls Probleme auftreten.

1. **Aussteller** URL muss mit **"/"**enden.  

    **Zielgruppe** sollte die Player-Client-ID und auch hinzufügen **"/"** am Ende des URL Aussteller.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    In [JWT Decoder](http://jwt.calebb.net/)sollte **also** und **Iss** als unten im JWT-Token angezeigt werden:
    
    ![1. Problem](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Berechtigungen für die Anwendung in AAD (Register Konfigurieren der Anwendung) hinzufügen. Dies ist erforderlich für jede Anwendung (lokale und bereitgestellten Versionen).

    ![2. Problem](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. Verwenden Sie rechten Emittenten dynamische CENC schützen:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    Die folgenden funktioniert nicht:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    Die GUID ist die AAD Tenant-ID. Die GUID finden Endpunkte Popup in Azure-Portal.

4. Gruppenmitgliedschaft gewähren Ansprüche Berechtigungen. Vergewissern Sie sich in AAD Anwendungsmanifestdatei haben wir die folgenden

    "GroupMembershipClaims": "All" (der Standardwert ist null)

5. Einstellung korrekte TokenType bei Einschränkung Anforderungen.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Nach Hinzufügen der Unterstützung der JWT (AAD) neben SWT (ACS), ist die Standardeinstellung TokenType TokenType.JWT. Wenn Sie SWT-ACS verwenden, müssen Sie TokenType.SWT festlegen.

## <a name="additional-topics-for-implementation"></a>Weitere Themen für die Implementierung
Weiter werden wir einige weiteren Themen in unser Entwurf und Implementierung uss disc.

###<a name="http-or-https"></a>HTTP oder HTTPS?

ASP.NET MVC-Playersoftware haben wir muss Folgendes unterstützen:

1. Benutzerauthentifizierung über Azure AD unter HTTPS werden;
1. JWT token Datenaustausch zwischen Client und Azure AD unter HTTPS werden;
1. DRM-Lizenzerwerb vom Client zu HTTPS unterliegen, wenn Lizenz Übermittlung von Azure Media Services bereitgestellt wird. PlayReady-Produktreihe Mandat natürlich nicht HTTPS für die Übermittlung der Lizenz. Ist der PlayReady-Lizenzserver außerhalb Azure Media Services konnte entweder HTTP oder HTTPS verwendet werden.

Daher wird ASP.NET Player Anwendung HTTPS als bewährte Methode verwenden. Dies bedeutet Azure Media Player auf einer Seite unter HTTPS. Für streaming wir bevorzugen HTTP, daher müssen wir gemischte Inhalte Frage.

1. Browser erlaubt keine gemischten Inhalt. Aber Silverlight und OSMF-Plugin für glätten und Strich-Plug-Ins. Gemischter Inhalt ist ein Sicherheitsproblem - Gefahr zu einschleusen böswilliger JS Kundendaten Risiko führen.  Browser standardmäßig sperren und bisher die einzige Möglichkeit, umgehen auf Server (Ursprung) zu allen Domänen (unabhängig Https oder http). Dies ist entweder nicht empfehlenswert.
1. Wir sollten gemischten Inhalt: beide verwenden HTTP oder HTTPS verwenden. Wenn Sie gemischten Inhalt wiedergeben, erfordert SilverlightSS Tech gemischte Inhalte Warnung deaktivieren. FlashSS Tech behandelt gemischten Inhalt ohne Warnung gemischten Inhalt.
1. Wenn der Streaming-Endpunkt vor August 2014 erstellt wurde, wird HTTPS nicht unterstützt. In diesem Fall Bitte erstellen Sie und einen neuen Streaming-Endpunkt für HTTPS.

In der referenzimplementierung für DRM-geschützte Inhalte werden Anwendung und streaming unter HTTTPS. Offene Inhalte braucht der Player nicht Authentifizierung oder Lizenz haben die Freiheit, HTTP oder HTTPS verwenden.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory signieren Key rollover

Dies ist ein wichtiger Punkt Ihrer Implementierung berücksichtigt werden. Wenn Sie dies nicht in Ihrer Implementierung berücksichtigt, stoppt das System abgeschlossene Arbeiten schließlich vollständig innerhalb von höchstens 6 Wochen.

Azure AD verwendet Industriestandard vertrauen zwischen sich selbst und Applikationen mit Azure AD. Azure AD verwendet, einen Schlüssel, der eine öffentliche und private Schlüsselpaar besteht. Azure AD ein Sicherheitstoken erstellt, die Informationen über den Benutzer enthält, wird dieses Token signiert von Azure AD mithilfe des privaten Schlüssels aus, bevor sie zurück an die Anwendung gesendet werden. Überprüfen, ob das Token gültig und von Azure AD tatsächlich entstanden ist, muss die Anwendung das Token Signatur mit dem öffentlichen Schlüssel von Azure AD des Mieters Verbundmetadaten-Dokument innerhalb verfügbar gemacht überprüfen. Dieser öffentliche Schlüssel und Signaturschlüssel aus dem ableitet ist derselbe für alle Mandanten in Azure AD verwendet.

Detaillierte Informationen zur Azure AD Key Rollover finden Sie im Dokument: [Wichtige Informationen zum Signieren von Key Rollover in Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Zwischen [Öffentlich-privates Schlüsselpaar](https://login.windows.net/common/discovery/keys/) 

- Der private Schlüssel wird von Azure Active Directory zu JWT Token verwendet.
- Der öffentliche Schlüssel wird von einer Anwendung wie DRM-Lizenz Delivery Services AMS JWT Token überprüfen verwendet.
 
Aus Sicherheitsgründen wird Azure Active Directory regelmäßig dieses Zertifikat (6 Wochen). Bei Sicherheitslücken kann wichtige Rollover jederzeit auftreten. Daher müssen der Lizenz Lieferdienste AMS Azure AD dreht das Schlüsselpaar, andernfalls token AMS-Authentifizierung schlägt fehl und keine Lizenz ausgestellt verwendeten öffentlichen Schlüssel aktualisieren. 

Dies wird erreicht, indem TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument beim DRM-Lizenz Dienste konfigurieren.

JWT token ist als unten:

1.  Azure AD wird das JWT-Token mit dem aktuellen privaten Schlüssel für einen authentifizierten Benutzer erteilen.
2.  Wenn ein Spieler ein CENC mit mehreren DRM geschützten Inhalt sieht, wird zuerst das JWT-Token ausgestellt von Azure AD suchen.
3.  Der Player übermittelt Lizenz Lieferdienste AMS Lizenz Übernahme Anforderung mit der JWT;
4.  Aktuelle/gültigen öffentlichen Schlüssel von Azure AD verwendet der Lizenz Lieferdienste AMS JWT-Token vor Lizenzen überprüfen.

DRM-Lizenz Service werden immer aktuelle/gültigen öffentlichen Schlüssel von Azure AD überprüft werden. Der öffentliche Schlüssel von Azure AD präsentiert wird der zur Überprüfung von JWT-Token ausgestellt von Azure AD verwendet.

Was geschieht, wenn das Schlüssel Rollover nach AAD generiert einen JWT-Token vor JWT Token zu DRM-Lizenz Lieferdienste AMS zur Überprüfung von Spielern gesendet? 

Da eine Taste jederzeit rückgängig gemacht werden könnte, gibt es immer mehr als ein gültiger öffentlicher Schlüssel für das Verbundmetadaten-Dokument. Azure Media Services Lizenz Übermittlung können im Dokument angegebenen Schlüssel seit Tasten bald werden möglicherweise andere möglicherweise ersetzt werden usw..

### <a name="where-is-the-access-token"></a>Das Zugriffstoken ist?

Wenn Sie herauszufinden, wie eine Webanwendung eine app API unter [Anwendungsidentität mit OAuth 2.0 Client Anmeldeinformationen](active-directory-authentication-scenarios.md#web-application-to-web-api)aufruft, Authentifizierung läuft wie unten:

1.  Ein Benutzer Azure AD in die Anwendung signiert (siehe [Webbrowser Web Application](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Azure AD autorisierungsendpunkt leitet den Benutzer-Agent an der Clientanwendung einen Autorisierungscode. Der Benutzer-Agent gibt Autorisierungscode an die Clientanwendung Umleitung URI zurück.
3.  Die Anwendung muss ein Zugriffstoken erwerben Web API authentifizieren und die gewünschte Ressource abrufen können. Er fordert Azure AD token Endpunkt, Anmeldeinformationen, Client-ID und Web API-Anwendung URI-ID bereitstellen. Es stellt den Autorisierungscode zu beweisen, dass der Benutzer zugestimmt hat.
4.  Azure AD Anwendung authentifiziert und ein Zugriffstoken JWT, mit der Web-API-aufrufen, zurückgegeben.
5.  Über HTTPS verwendet die Anwendung zurückgegebene Zugriffstoken JWT Web API JWT Zeichenfolge mit "Träger" in der Authorization-Header der Anforderung hinzu. Web-API das JWT-Token überprüft und wenn Validierung erfolgreich ist, gibt die gewünschte Ressource.

Dieser Fluss "Anwendungsidentität" vertraut Web API, dass die Anwendung den Benutzer authentifiziert. Aus diesem Grund heißt dieses Musters ein vertrauenswürdiges Subsystem. [Auf dieser Seite](http://msdn.microsoft.com/library/azure/dn645542.aspx/) beschreibt wie Autorisierung erteilt Fluss funktioniert.

Lizenzerwerb mit token folgen wir dasselbe Muster vertrauenswürdiges Subsystem. Und der Lizenz Zustelldienst in Azure Media Services Web API-Ressource, eine Anwendung für den Zugriff benötigt "Back-End-Ressource". Wo ist das Zugriffstoken?

Tatsächlich sind wir von Azure AD Zugriffstoken abrufen. Nach der erfolgreichen Authentifizierung wird Autorisierungscode zurückgegeben. Der Autorisierungscode ist dann Client-ID und app Schlüssel verwendet Zugriffstoken austauschen. Und das Zugriffstoken für den Zugriff auf eine Anwendung "Zeiger" verweist oder Azure Media Services Lizenz Zustelldienst darstellt.

Wir müssen registrieren und Konfigurieren der "Zeiger" Anwendung in Azure AD folgt:

1.  In Azure AD-Mandanten

    - Hinzufügen einer Anwendung (Ressource) anmelden URL: 

    https://[resource_name].azurewebsites.NET/ und 

    - App-ID-URL: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Hinzufügen eines neuen Schlüssels für die Ressourcen app;
3.  Die app-manifest-Datei aktualisieren, sodass die GroupMembershipClaims Eigenschaft folgenden Wert hat: "GroupMembershipClaims": "Alle"  
4.  Player Web app auf Azure AD-app im Abschnitt "Berechtigungen an andere Programme" Fügen Sie Ressource app hinzu, die in Schritt 1 oben hinzugefügt wurde. Überprüfen Sie unter "Berechtigungen delegiert" Häkchen "Access [Resource_name]". Dies ermöglicht Web app Berechtigung zum Erstellen von Zugriffstoken für den Zugriff auf die Ressourcen app. Sie sollten lokale und bereitgestellte Version der Web-app dazu Entwickeln mit Visual Studio und Azure WebApp.
    
Deshalb JWT Token ausgestellt von Azure AD tatsächlich das Zugriffstoken für den Zugriff auf diese Ressource "Zeiger".

### <a name="what-about-live-streaming"></a>Was Live-Streaming?

Oben konzentriert unsere Diskussion auf bei Bedarf. Was live-streaming?

Glücklicherweise ist, dass Sie genau demselben Design und der Implementierung können zum Schutz von live-streaming in Azure Media Services Programm "VOD Anlage" Anlage behandelt.

Insbesondere ist bekannt, dass dazu live-streaming in Azure Media Services Sie einen Kanal und ein Programm unter den Kanal erstellen müssen. Um die Anwendung zu erstellen, müssen Sie eine Anlage erstellen live-Archiv für das Programm enthalten. Um CENC Multi-DRM-Schutz von live-Inhalten zu ermöglichen, Sie müssen, gilt die gleiche Einrichtung und Verarbeitung der Anlage als würde er "VOD Anlage" bevor Sie das Programm starten.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Was Lizenzserver außerhalb Azure Media Services?

Häufig Kunden möglicherweise investiert in Lizenz Serverfarm in ihre eigenen Daten zentrieren oder gehostet von DRM-Dienstanbietern. Glücklicherweise Azure Media Services Content Protection ermöglicht im Hybrid-Modus: Inhalt gehostet und dynamisch in Azure Media Services geschützt, während DRM-Lizenzen von Servern außerhalb Azure Media Services übermittelt werden. In diesem Fall gibt Folgendes geändert:

1. Secure Token Service muss Token ausstellen, werden von der Serverfarm Lizenz überprüft werden. Axinom gemäß Widevine Lizenzserver erfordert beispielsweise ein spezifisches JWT-Token "Nachricht Anspruch" enthält. Daher müssen Sie ein STS solche JWT-Token ausstellen. Die Autoren haben diese Implementierung und Details finden Sie im folgenden Dokument [Azure Dokumentation Center](https://azure.microsoft.com/documentation/): [Axinom Widevine Lizenzen Azure Media Services zu verwenden](media-services-axinom-integration.md). 
1. Sie müssen keine Lizenz Lieferservice (ContentKeyAuthorizationPolicy) in Azure Media Services. Müssen Sie zu der Lizenz Übernahme URLs (PlayReady, Widevine und FairPlay) werden beim Konfigurieren von AssetDeliveryPolicy in CENC mit mehreren DRM einrichten.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Was passiert, wenn ich einen benutzerdefinierten STS verwenden?

Es kann einige Gründe, die ein Kunde einen benutzerdefinierten STS (Secure Token Service) für die Bereitstellung von JWT-Token verwenden kann. Einige davon sind:

1.  Die vom Kunden verwendete Identität Anbieter (IDP) unterstützt STS nicht. In diesem Fall möglicherweise einen benutzerdefinierten STS eine Option.
2.  Der Kunde möglicherweise flexible oder eine engere Steuerelement im Kunden-Abonnenten Abrechnungssystem STS integrieren. Beispielsweise kann ein Operator MVPD mehrere OTT Abonnenten wie grundlegende Premium, Sport usw. bieten. Der Operator sollten die Ansprüche im Token mit einem Abonnenten übereinstimmen nur Inhalte in das richtige zur Verfügung gestellt wird. In diesem Fall einen benutzerdefinierten STS bietet die erforderliche Flexibilität und Kontrolle.

Zwei müssen bei Verwendung eines benutzerdefinierten STS:

1.  Beim Konfigurieren der Lizenz Zustelldienst für eine Anlage müssen Sie Sicherheitsschlüssel zur Überprüfung von benutzerdefinierten STS (siehe unten) statt des aktuellen Schlüssels von Azure Active Directory angeben.
2.  Wenn ein JTW Token generiert wird, ein Sicherheitsschlüssel anstelle des privaten Schlüssels des aktuellen X509 angegeben Zertifikat in Azure Active Directory.

Es gibt zwei Arten von Sicherheitsschlüsseln aus:

1.  Symmetrischen Schlüssel: denselben Schlüssel dient zum Generieren und einen JWT-Token überprüfen
2.  Asymmetrische Schlüssel: ein öffentliches / privates Schlüsselpaar in ein X509 Zertifikat mit privaten Schlüssel zur Verschlüsselung/JWT-Token und öffentliche Schlüssel für das Token bestätigt.

####<a name="tech-note"></a>Technische Hinweise

Verwenden Sie.NET Framework / C# als Ihre Plattform, die X509 Zertifikat für asymmetrische Schlüssel muss die Länge mindestens 2048. Dies ist eine Anforderung der System.IdentityModel.Tokens.X509AsymmetricSecurityKey im.NET Framework-Klasse. Andernfalls wird die folgende Ausnahme ausgelöst:

IDX10630: Die System.IdentityModel.Tokens.X509AsymmetricSecurityKey zum Signieren von weniger als 2048' nicht möglich. 

## <a name="the-completed-system-and-test"></a>Abgeschlossene und testen

Wir werden einige Szenarien abgeschlossene End-to-End-System durchlaufen, damit Leser davor ein Anmeldekonto Basic das Verhalten "Bild" haben.

Der Web-Player und den Benutzernamen finden Sie [hier](https://openidconnectweb.azurewebsites.net/).

Wenn "nicht integrierte" Szenario ist: Videodaten gehostet in Azure Media Services sind entweder nicht oder DRM geschützten ohne token-Authentifizierung (Erteilung einer Lizenz, wer anfordert), (durch den Wechsel zu HTTP ist der video-streaming über HTTP) ohne Anmeldung testen.

Wenn integrierte End-to-End-Szenario ist: Videos unter dynamische DRM-Schutz in Azure Media Services mit token und JWT-Token von Azure AD, müssen Sie sich anmelden.

### <a name="user-login"></a>Benutzername

Um die End-to-End integrierte DRM-System zu testen, müssen Sie ein "Konto" erstellt oder hinzugefügt. 

Welches Konto?

Auch Azure ursprünglich nur von Microsoft-Konto zugreifen, können sie nun Benutzer aus beiden Systemen. Dies geschah durch das Azure Eigenschaften vertrauen Azure AD mit Azure AD Organisationseinheit Benutzer authentifizieren Authentifizierung und Verbund Beziehung, Azure AD Account Consumer-Identitätssystem Consumer Benutzer authentifizieren vertraut. Daher kann Azure AD "Gast" Microsoft sowie "native" Azure AD-Konten authentifizieren.

Da Azure AD Microsoft-Konto (MSA) Domäne vertraut, können Konten aus einer der folgenden Domänen zu benutzerdefinierten Azure AD Mieter und das Konto für die Anmeldung:

**Domänenname**|**Domäne**
-----------|----------
**Benutzerdefinierte Azure AD Mieter Domäne**|somename.onmicrosoft.com
**Unternehmensdomäne**|Microsoft.com
**Microsoft-Konto (MSA) Domäne**|Outlook.com live.com, hotmail.com

Sie wenden eine Autoren ein Konto erstellt oder hinzugefügt. 

Nachfolgend finden Sie die Screenshots anderen Anmeldeseiten verschiedene Domänenkonten verwendet.

**Benutzerdefinierte Azure AD Mieter Domänenkonto**: In diesem Fall finden Sie die angepasste Login-Seite des benutzerdefinierten Azure AD Tenant-Domäne.

![Benutzerdefinierte Azure AD Mieter Domänenkonto](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft-Domänenkonto mit Smartcard**: In diesem Fall finden Sie die Anmeldeseite von Microsoft corporate angepasst mit zwei-Faktor-Authentifizierung.

![Benutzerdefinierte Azure AD Mieter Domänenkonto](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft-Konto (MSA)**: In diesem Fall die Anmeldeseite von Microsoft Account für Verbraucher angezeigt.

![Benutzerdefinierte Azure AD Mieter Domänenkonto](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Mit verschlüsselten Medien Extensions for PlayReady

Moderne Browser mit verschlüsselten Media Extensions (EME) PlayReady-Unterstützung wie Internet Explorer 11 unter Windows 8.1 und höher und Microsoft Edge-Browser unter Windows 10 werden PlayReady zugrunde liegenden DRM für EME.

![EME verwenden für PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Dunkle Spielerbereich ist, PlayReady Schutz verhindert eine Bildschirmaufnahme geschützten Video vorgibt. 

Bildschirm Player-Plug-Ins und MSE-EME unterstützt.

![EME verwenden für PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME in Microsoft Edge und IE 11 unter Windows 10 ermöglicht das Aufrufen der [PlayReady-SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) bei Windows 10 unterstützt. PlayReady SL3000 entsperrt erweiterte Premium Content-Fluss (4K, HDR, usw.) und neue Inhalte (frühe Fenster für erweiterte Inhalte).

Konzentrieren Sie sich auf Windows-Geräten: PlayReady ist die einzige DRM HW auf Windows-Geräten (PlayReady SL3000). Ein streaming-Dienst mit PlayReady durch EME oder durch eine UWP und bieten eine höhere Bildqualität mit PlayReady SL3000 als ein anderes DRM. 2 K Inhalt fließt in der Regel durch Chrome oder Firefox und 4 K Inhalt über Microsoft Edge/IE11 oder eine UWP-Anwendung auf demselben Gerät (je nach Einstellungen Implementierung).


#### <a name="using-eme-for-widevine"></a>Verwenden von EME für Widevine

Moderne Browser mit EME-Widevine Unterstützung wie 41 + Windows 10, Windows 8.1, Mac OSX Yosemite und Chrome auf Android 4.4.4 ist Google Widevine DRM hinter EME.

![Verwenden von EME für Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Beachten Sie, dass Widevine eine Bildschirmaufnahme geschützten Video daran nicht verhindert.

![Verwenden von EME für Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Nicht berechtigt Benutzer

Ist ein Benutzer nicht Mitglied der Gruppe "Benutzer berechtigt", der Benutzer werden nicht "Ansprüche" bestanden und Multi-DRM-Lizenz-Dienst lehnt die angeforderte Lizenz ausstellen, wie unten dargestellt. Die ausführliche Beschreibung "Lizenz fehlgeschlagen", also wie vorgesehen.

![Nicht berechtigt Benutzer](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Ausführen von benutzerdefinierten Secure Token Service

Szenario mit benutzerdefinierten Secure Token Service (STS) JWT-Token benutzerdefinierten STS erteilt symmetrische oder asymmetrische Schlüssel verwenden. 

Bei symmetrischen Schlüssel (mit Chrom):

![Ausführen von benutzerdefinierten STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Bei asymmetrischen Schlüssel über eine X509 mit Zertifikat (mit modernen Browser Microsoft).

![Ausführen von benutzerdefinierten STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

In beiden Fällen bleibt der Benutzerauthentifizierung – bei Azure AD. Der einzige Unterschied ist, dass dem benutzerdefinierten STS statt Azure AD JWT-Token ausgestellt werden. Natürlich beim dynamischen CENC Schutz konfigurieren Beschränkung der Lizenz Zustelldienst gibt den JWT-Token symmetrische oder asymmetrische Schlüssel.

## <a name="summary"></a>Zusammenfassung

In diesem Dokument besprochenen CENC mit mehreren native DRM und Zugriff über token Authentifizierung: Design und die Implementierung mithilfe von Azure, Azure Media Services und Azure Media Player.

- Referenz-Design stellt enthält alle notwendigen Komponenten in einem Subsystem DRM-CENC;
- Eine referenzimplementierung Azure Azure Media Services und Azure Media Player.
- Einige unmittelbar in Entwurf und Implementierung werden auch Themen.


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Danksagungen 

William Zhang Mingfei Yan Roland Le Franc, Kilroy Hughes, Julia Kornich

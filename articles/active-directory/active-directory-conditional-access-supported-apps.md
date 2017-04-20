<properties
    pageTitle="Bedingte Regeln in Azure Active Directory verwenden | Microsoft Azure"
    description="Mit bedingten Zugriffskontrolle überprüft Azure Active Directory für bestimmte Situationen den Benutzer authentifiziert und Zugriff zu ermöglichen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Bedingte Regeln in Azure Active Directory verwenden

Bedingte Regeln werden in Azure Active Directory (Azure AD)-Applikationen verbunden, vorinstalliert verbundenen Software als Service (SaaS) Applications, verwenden Kennwort einmaliges Anmelden (SSO) Line of Business Applications und, Azure AD-Anwendungsproxy verwenden. Eine detaillierte Liste der Anträge für die Zugangskontrolle verwenden können, finden Sie unter [Dienste mit Zugangskontrolle aktiviert](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Zugangskontrolle funktioniert sowohl mit mobilen und desktop, die moderne Authentifizierung verwenden. In diesem Artikel untersuchen wir wie bedingte Zugriff funktioniert in mobilen und Desktop-apps.

Sie können in Azure AD-Seiten verwenden, die moderne Authentifizierung verwenden. Mehrstufige Authentifizierung wird ein Benutzer mit einer Seite Anmelden aufgefordert. Eine Meldung wird angezeigt, wenn der Benutzerzugriff blockiert wird. Moderne Authentifizierung ist für das Gerät bei Azure AD authentifizieren, sodass gerätebasierte bedingte Richtlinien ausgewertet werden.

Es ist wichtig zu wissen, welche Anwendung verwenden kann, bedingte Regeln und die Schritte, die Sie zu anderen Anwendung Einstiegspunkte schützen müssen.

## <a name="applications-that-use-modern-authentication"></a>Moderne Authentifizierung verwenden

> [AZURE.NOTE] Haben Sie eine bedingte Richtlinie in Azure AD, die eine Entsprechung in Office 365, konfigurieren Sie beide bedingten Richtlinien. Dies würde, z. B. bedingte Richtlinien für Exchange Online und SharePoint Online zuweisen.

Die folgenden Programme unterstützen Zugangskontrolle für Office 365 und anderen Azure AD connected-Service:

| Zieldienst  | Plattform  | Anwendung                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365 Exchange Online | Windows 10|E-Mail, Kalender, Personen Anwendung Outlook 2016 Outlook 2013 (moderne Authentifizierung), Skype (mit modernen Authentifizierung)|
|Office 365 Exchange Online| Windows 8.1, Windows 7 |Outlook 2016 Outlook 2013 (moderne Authentifizierung), Skype für Unternehmen (mit modernen Authentifizierung)|
|Office 365 Exchange Online|iOS, Android|  Outlook mobile-app|
|Office 365 Exchange Online|Mac OS X| Outlook 2016 für mehrstufige Authentifizierung und Lage; Gerät-basierte Unterstützung geplant, Skype Unterstützung geplant|
|Office 365 SharePoint Online|Windows 10| Office 2016 apps, Universal Office apps, Office 2013 (moderne Authentifizierung) OneDrive für Business-app (Next Generation Sync Client oder NGSC) unterstützt geplant, Office Gruppen Unterstützung geplant, SharePoint Support für Apps geplant|
|Office 365 SharePoint Online|Windows 8.1, Windows 7|Office 2016 apps, Office 2013 (moderne Authentifizierung), OneDrive für Business-Anwendung (Groove Sync Client)|
|Office 365 SharePoint Online|iOS, Android|  Office mobile apps |
|Office 365 SharePoint Online|Mac OS X| Office 2016 apps für mehrstufige Authentifizierung und Lage; Gerät-basierte Unterstützung für die Zukunft geplant|
|Office 365 Yammer|Windows 10, IOS- und Android | Office Yammer app|
|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, iOS und Android | Dynamics CRM-app|
|PowerBI-Dienst|Windows 10, Windows 8.1, Windows 7, iOS und Android | PowerBI-Anwendung|
|Azure RemoteApp-service|Windows 10, Windows 8.1, Windows 7, iOS, Android und Mac OS X |Azure RemoteApp|
|Alle meine Apps app-Dienst|Android und iOS|Alle meine Apps app-Dienst |


## <a name="applications-that-do-not-use-modern-authentication"></a>Programme, die nicht die moderne Authentifizierung verwenden

Andere Methoden verwenden Sie zurzeit um Zugriff auf apps zu blockieren, die moderne Authentifizierung nicht verwenden. Zugriffsregeln für apps, die moderne Authentifizierung verwenden werden vom bedingten Zugriff nicht erzwungen. Dies ist in erster Linie eine Überlegung für Exchange und SharePoint. Die meisten frühere Versionen von apps verwenden ältere Protokolle Steuerelement.

### <a name="control-access-in-office-365-sharepoint-online"></a>Steuern des Zugriffs in Office 365 SharePoint Online
Mit dem Set-SPOTenant-Cmdlet können Sie ältere Protokolle für SharePoint-Zugriff deaktivieren. Verwenden Sie dieses Cmdlet zu Office-Clients, die nicht modernen Authentifizierungsprotokollen auf SharePoint Online-Ressourcen zugreifen.

**Beispiel**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Steuerelement auf Office 365 Exchange Online

Exchange bietet zwei Hauptkategorien von Protokollen. Überprüfen Sie die folgenden Optionen, und wählen Sie die Richtlinie, die für Ihr Unternehmen.

-   **Exchange ActiveSync**. Standardmäßig werden bedingte Richtlinien für mehrstufige Authentifizierung und Standort für Exchange ActiveSync nicht erzwungen. Sie müssen Zugriff auf diese Dienste von Exchange ActiveSync-Richtlinie direkt konfigurieren oder Exchange ActiveSync mithilfe von Active Directory-Verbunddienste (AD FS) Regeln blockieren schützen.
-   **Ältere Protokolle**. Sie können ältere Protokolle mit AD FS blockieren. Diese blockiert den Zugriff auf ältere Office-Clients wie Office 2013 ohne moderne Authentifizierung aktiviert und früheren Versionen von Office.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>Mithilfe von AD FS herkömmliches Protokoll blockieren

Folgendes Beispiel können ältere Protokollzugriff auf AD FS blockieren. Zwei allgemeine Konfigurationen auswählen.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Option 1: Kann Exchange ActiveSync und Legacyanwendungen, aber nur im Intranet zulassen

Die folgenden drei Regeln auf AD FS verlassen Partei vertrauen für Microsoft Office 365 Identität anwenden, können Exchange ActiveSync-Datenverkehr und Browser und moderne Authentifizierungsdatenverkehr zugreifen. Legacyanwendungen extranet blockiert.

##### <a name="rule-1"></a>Regel 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Regel 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Regel 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Option 2: Exchange ActiveSync lassen Sie zu oder blockieren Sie Legacyanwendungen

Die folgenden drei Regeln auf AD FS verlassen Partei vertrauen für Microsoft Office 365 Identität anwenden, können Exchange ActiveSync-Datenverkehr und Browser und moderne Authentifizierungsdatenverkehr zugreifen. Legacyanwendungen werden von einem beliebigen Standort blockiert.

##### <a name="rule-1"></a>Regel 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regel 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regel 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

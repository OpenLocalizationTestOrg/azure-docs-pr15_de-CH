<properties
    pageTitle="Azure Zugangskontrolle für SaaS-Apps | Microsoft Azure"
    description="Zugangskontrolle in Azure AD können Sie Zugriffsregeln pro Anwendung mehrstufige Authentifizierung und Zugriff für Benutzer nicht in einem vertrauenswürdigen Netzwerk blockieren. "
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Erste Schritte mit Azure bedingten Zugriff auf Active Directory

Azure Active Directory Zugangskontrolle für [SaaS](https://azure.microsoft.com/overview/what-is-saas/) -apps und Azure AD verbunden apps können bedingte Zugriff basierend auf Gruppe, Speicherort und Empfindlichkeit der Anwendung konfigurieren. 

Bedingter Zugriff Sensibilität der Anwendung können Sie Zugriffsregeln mehrstufige Authentifizierung (MFA) pro Anwendung festlegen. MFA pro Anwendung bietet die Möglichkeit für Benutzer, die nicht in einem vertrauenswürdigen Netzwerk blockieren. Sie können MFA-Regeln für alle Benutzer gelten, die für die Anwendung oder nur für Benutzer in bestimmten Sicherheitsgruppen zugewiesen sind.  Benutzer können MFA-Anforderung ausgeschlossen werden, wenn sie die Anwendung eine IP-Adresse zugreifen, die im Netzwerk der Organisation.

Diese Funktionen werden für Kunden verfügbar, die eine Azure Active Directory Premium-Lizenz erworben haben.

## <a name="scenario-prerequisites"></a>Szenario-Komponenten
* Lizenz für Azure Active Directory Premium

* Föderations- oder verwaltete Azure Active Directory Mieter

* Föderierte Mieter erfordern die kombinierte Authentifizierung aktiviert werden.

## <a name="configure-per-application-access-rules"></a>Pro Anwendung Zugriffsregeln konfigurieren

Dieser Abschnitt beschreibt pro Anwendung Zugriffsregeln konfigurieren.

1. Melden Sie sich im klassischen Azure-Portal mit einem Konto ein globaler Administrator für Azure AD.
2. Wählen Sie im linken Bereich **Active Directory**.
3. Wählen Sie auf der Registerkarte Verzeichnis das Verzeichnis ein.
4. Wählen Sie die Registerkarte **Applications** .
5. Wählen Sie die Anwendung, der für die Regel festgelegt wird.
6. Wählen Sie die Registerkarte **Konfigurieren** .
7. Scrollen Sie im Abschnitt Zugriff. Wählen Sie die gewünschte Regel.
8. Geben Sie die Benutzer, denen die Regel angewendet wird.
9. Aktivieren Sie die Richtlinie **aktiviert zu**wählen.

##<a name="understanding-access-rules"></a>Grundlegendes zu Regeln

Dieser Abschnitt enthält eine ausführliche Beschreibung der Zugriffsregeln in Azure bedingte Anwendungszugriff unterstützt.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Zugriffsregeln anwenden auf Benutzer

Standardmäßig gilt die Richtlinie für alle Benutzer, die Zugriff auf die Anwendung haben. Sie können jedoch auch die Richtlinie für Benutzer einschränken, die Mitglieder der angegebenen Sicherheitsgruppen sind. **Gruppe hinzufügen** -Schaltfläche dient zum Dialogfeld für die Gruppenauswahl eine oder mehrere Gruppen auswählen, für die die Regel gilt. Dieses Dialogfeld kann auch verwendet werden, ausgewählte Gruppen entfernen. Die Regeln auf Gruppen anwenden ausgewählt, werden die Zugriffsregeln nur für Benutzer erzwungen, die einer der angegebenen Sicherheitsgruppen angehören.

Sicherheitsgruppen können auch explizit aus der Richtlinie ausgeschlossen werden durch Auswahl der Option **außer** und eine oder mehrere Gruppen angeben. Benutzer Mitglied einer Gruppe in der Liste **mit Ausnahme** der Verpflichtung mehrstufige Authentifizierung nicht selbst sind sie Mitglied einer Gruppe, für die die Regel gilt.
Die nachfolgend aufgeführte Regel benötigen alle Benutzer in der Manager-Gruppe mehrstufige Authentifizierung beim Zugriff auf die Anwendung verwendet.

![MFA bedingten Zugriffsregeln festlegen](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>MFA bedingten Zugriffsregeln
Wenn Benutzer mithilfe des Features pro Benutzer mehrstufige Authentifizierung konfiguriert wurde, wird diese Einstellung für Benutzer mit mehrstufige Authentifizierungsregeln der app kombinieren. Dies bedeutet, dass ein Benutzer, der für die kombinierte Authentifizierung pro Benutzer konfiguriert mehrstufige Authentifizierung durchführen, auch wenn die Anwendungsregeln mehrstufige Authentifizierung ausgenommen wurden benötigen. Erfahren Sie mehr über mehrstufige Authentifizierung und benutzerspezifischen Einstellungen.

### <a name="access-rule-options"></a>Zugriff auf Optionen
Die folgenden Optionen werden unterstützt:

* **Mehrstufige Authentifizierung**: Benutzer, denen die Zugriffsregeln anwenden, werden vollständige mehrstufige Authentifizierung vor dem Zugriff auf die Anwendung, die die Richtlinie gilt.

* **Mehrstufige Authentifizierung nicht bei der Arbeit**: ein Benutzer aus einer vertrauenswürdigen IP-Adresse ist nicht müssen Mehrfaktor-Authentifizierung durchzuführen. Vertrauenswürdige IP-Adressbereiche können auf die kombinierte Authentifizierung konfiguriert werden.

* **Zugriff nicht am Arbeitsplatz**: Benutzer, die nicht aus einer vertrauenswürdigen IP-Adresse ist blockiert. Vertrauenswürdige IP-Adressbereiche können auf die kombinierte Authentifizierung konfiguriert werden.

### <a name="setting-rule-status"></a>Regelstatus festlegen
Regelstatus Zugriff ermöglicht das Aktivieren oder Deaktivieren von Regeln. Wenn die Zugriffsregeln deaktiviert sind, wird die mehrstufige Authentifizierung erforderlich nicht erzwungen.

### <a name="access-rule-evaluation"></a>Access Regel Bewertung

Regeln werden ausgewertet, wenn ein Benutzer eine verbundene Anwendung zugreift, die OAuth 2.0, OpenID Connect, SAML oder WS-Verbund verwendet. Darüber hinaus werden Zugriffsregeln ausgewertet, wenn OAuth 2.0 und OpenID verbinden Aktualisierungstoken Zugriffstoken zu verwenden. Wenn Auswertung fehlschlägt, wird ein Aktualisierungstoken, Fehler **Invalid_grant** zurückgegeben, dies bedeutet, dass der Benutzer erneut an den Client authentifizieren muss.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Konfigurieren Sie Verbunddienste um mehrstufige Authentifizierung

Föderierte Mieter kann MFA Azure Active Directory oder die lokal durchgeführt werden AD FS-Server.

Standardmäßig erfolgt die MFA eine Seite von Azure Active Directory gehostet. Konfigurieren Sie lokal MFA muss die **SupportsMFA-** Eigenschaft auf **true** in Azure Active Directory mithilfe der Azure AD-Moduls für Windows PowerShell festgelegt werden.

Das folgende Beispiel zeigt lokale MFA mithilfe des [Set-MsolDomainFederationSettings-Cmdlet](https://msdn.microsoft.com/library/azure/dn194088.aspx) contoso.com Mieter aktivieren:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Zusätzlich zum Festlegen dieses Flags muss verbundenen Mieter AD FS-Instanz für die mehrstufige Authentifizierung konfiguriert werden. Anleitung für die [Bereitstellung von Azure mehrstufige Authentifizierung lokal](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Verwandte Artikel

- [Zugang zu Office 365 und anderen apps verbunden in Azure Active Directory](active-directory-conditional-access.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)

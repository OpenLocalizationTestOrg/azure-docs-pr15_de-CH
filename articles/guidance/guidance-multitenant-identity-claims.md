<properties
   pageTitle="Arbeiten mit Identitäten in mandantenfähigen Applikationen Anspruch basierenden | Microsoft Azure"
   description="Verwendung für Aussteller Überprüfung und Autorisierung wie Ansprüche"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Anspruchsbasierte Identität mandantenfähigen Applikationen arbeiten

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie]. Außerdem ist eine vollständige [Beispiel] , das dieser Serie begleitet.

## <a name="claims-in-azure-ad"></a>Ansprüche in Azure AD

Wenn sich ein Benutzer anmeldet, sendet Azure AD ein ID-Token, das einen Satz von Ansprüchen über den Benutzer enthält. Ein Anspruch ist eine Hardwarekomponente Informationen als Schlüssel-Wert-Paar. Beispielsweise `email` = `bob@contoso.com`.  Ansprüche haben einen Aussteller &mdash; in diesem Fall Azure AD &mdash; der Entität, authentifiziert den Benutzer und die Ansprüche erstellt, wird. Da Sie dem Aussteller vertrauen, vertrauen die Ansprüche. (Umgekehrt, wenn den Aussteller vertrauen vertrauen Sie nicht Ansprüche!)

Hoch:

1.  Der Benutzer authentifiziert.
2.  IDP sendet ein.
3.  Die app normalisiert oder erweitert die Ansprüche (optional).
4.  Die Anwendung verwendet die Ansprüche zu Autorisierung.

OpenID Connect Sie erhalten Anspruchssatz [Bereichsparameter] Authentifizierung Anforderung gesteuert. Azure AD gibt allerdings eine begrenzte Anzahl von Ansprüchen über OpenID verbinden. [unterstützt und Anspruchstypen]sehen. Weitere Informationen über den Benutzer gegebenenfalls müssen Sie Azure AD Graph-API verwenden.

Hier sind einige Ansprüche aus AAD app normalerweise interessieren könnte:

Anspruchstyp Token-ID |    Beschreibung
-----------------------|--------------
AUD | Die für das Token ausgestellt wurde. Dadurch werden der Anwendung Client-ID. Im Allgemeinen sollte nicht müssen Sie sorgen dafür, da die Middleware automatisch überprüft. Beispiel:`"91464657-d17a-4327-91f3-2ed99386406f"`
Gruppen   | Eine Liste der AAD Gruppen der Benutzer angehört. Beispiel:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
ISS  | Der [Aussteller] des Tokens OIDC. Beispiel:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
Name    | Der Name des Benutzers angezeigt. Beispiel:`"Alice A."`
OID | Die ID für den Benutzer im AAD. Dieser Wert ist unveränderlich und nicht wieder verwendbare ID des Benutzers. Verwenden Sie diesen Wert, keine e-Mail als eindeutiger Bezeichner für Benutzer. e-Mail-Adressen ändern können. Bei Verwendung von Azure AD Graph-API in Ihrer app Wert Objekt-ID dieses Profil Informationen verwendet. Beispiel:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
Rollen   | Eine Liste der app-Rollen für den Benutzer. Beispiel:`["SurveyCreator"]`
TID | Mandanten-ID Dieser Wert ist ein eindeutiger Bezeichner für den Mandanten in Azure AD. Beispiel:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Einen Menschen lesbaren Anzeigenamen des Benutzers. Beispiel:`"alice@contoso.com"`
UPN | Benutzerprinzipalname. Beispiel:`"alice@contoso.com"`

Diese Tabelle enthält die Anspruchstypen in Token-ID angezeigt werden. In ASP.NET Core 1.0 konvertiert Middleware OpenID verbinden einige Anspruchstypen Wenn die Claims-Sammlung für den Benutzerprinzipal füllt:

-   OID >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   Unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   UPN >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Die Anspruchstransformation

Während der Fluss Authentifizierung möchten Sie die Ansprüche ändern, die von der IDP erhalten. Führen Sie in ASP.NET Core 1.0 Anspruchstransformation in das **AuthenticationValidated** -Ereignis von Middleware OpenID verbinden. (Siehe [Authentifizierungsereignisse].)

Ansprüche, die während der **AuthenticationValidated** hinzugefügt werden in das Authentifizierungscookie Sitzung gespeichert. Sie nicht wieder in Azure AD übertragen.

Einige Beispiele der Anspruchstransformation:

-   **Ansprüche Normalisierung**und die verschiedenen Benutzer Ansprüche. Dies ist besonders wichtig, wenn Sie Ansprüche aus mehreren vertriebenen der anderen Anspruchstypen ähnliche Informationen möglicherweise erhalten.
Beispielsweise sendet Azure AD eine Forderung "Upn", die e-Mail des Benutzers enthält. Andere vertriebenen schicken einen Anspruch "e-Mail". Der folgende Code konvertiert in einen Anspruch "email" Anspruch "Upn":

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Hinzufügen **Standard Anspruch Werte** für Ansprüche, die nicht vorhanden &mdash; einen Benutzer beispielsweise eine Standardrolle zuweisen. In einigen Fällen kann dies Autorisierungslogik vereinfachen.
- **Benutzerdefinierte Ansprüche** anwendungsspezifische Informationen über den Benutzer hinzufügen. Beispielsweise können Sie bestimmte Informationen über die Benutzer in einer Datenbank speichern. Das Authentifizierungsticket konnte einen benutzerdefinierten Anspruch mit diesen Informationen hinzugefügt werden. Die Forderung wird in einem Cookie gespeichert, müssen Sie nur die Datenbank einmal pro Sitzung erhalten. Auf der anderen Seite möchten Sie auch übermäßig große Cookies erstellen, so dass das Verhältnis zwischen Cookiegröße und Datenbankabfragen berücksichtigt werden müssen.   

Nach Abschluss der Authentifizierungsablauf stehen die Ansprüche in `HttpContext.User`. Nun, Sie sollten sie behandeln als schreibgeschützte Auflistung &mdash; z. B. zu Autorisierung verwenden.

## <a name="issuer-validation"></a>Aussteller-Validierung
OpenID Connect identifiziert der Aussteller Anspruch ("Iss") IDP, die Token-ID ausgestellt. Teil der OIDC Authentifizierung wird sichergestellt, dass der Aussteller Anspruch tatsächlichen Aussteller übereinstimmt. OIDC-Middleware übernimmt.

In Azure AD Issuer-Wert ist eindeutig pro AD-Mandanten (`https://sts.windows.net/<tenantID>`). Daher müssen eine Anwendung eine zusätzliche Überprüfung, um sicherzustellen, dass der Emittent Mieter darstellt, die bei der Anwendung anmelden.

Für eine Anwendung einem Mandanten können Sie nur überprüfen Emittenten eigene Mieter. Tatsächlich wird die OIDC-Middleware automatisch standardmäßig. In einer Multi-Tenant-app müssen Sie mehrere Emittenten entspricht anderen Mandanten zu ermöglichen. Hier ist ein allgemeiner Ansatz verwenden:

-   **ValidateIssuer** Optionen Middleware OIDC auf False festgelegt. Dadurch wird die automatische Prüfung deaktiviert.
-   Wenn ein Mieter anmeldet, speichern Sie Mieter und der Emittent in den DB.
-   Wenn sich ein Benutzer anmeldet, der Emittent in der Datenbank suchen. Wenn der Aussteller nicht gefunden wird, bedeutet dies, Pächter angemeldet hat. Sie können auf eine Anmeldeseite umgeleitet werden.
-  Sie können auch bestimmte Mieter blacklist; beispielsweise Kunden, die ihr Abonnement bezahlt haben.

Ausführlichere Informationen finden Sie unter [Anmeldung und Mieter Onboarding in einer mehrinstanzenfähigen Anwendung][signup].

## <a name="using-claims-for-authorization"></a>Ansprüche für die Autorisierung verwenden

Mit Ansprüchen entfällt der Benutzeridentität monolithische Entität. Beispielsweise müssen Benutzer eine e-Mail-Adresse, Telefonnummer, Geburtstag, Geschlecht, usw.. Vielleicht speichert IDP des Benutzers diese Informationen. Aber wenn die Benutzerauthentifizierung in der Regel eine Teilmenge dieser als Ansprüche. In diesem Modell ist die Identität des Benutzers einfach eine Ansprüche. Wenn Sie Autorisierung über Benutzer entscheiden, sehen Sie für bestimmte Gruppen von Ansprüchen. In anderen Worten, die Frage "Kann Benutzer X Aktion Y" schließlich werden "Benutzer X beansprucht Z".

Hier sind einige grundlegende Muster für die Überprüfung der Ansprüche.

-  So überprüfen Sie, dass der Benutzer einen bestimmten Antrag mit einem bestimmten Wert

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Dieser Code überprüft, ob der Benutzer einen Rollenanspruch mit dem Wert "Admin". Den Fall der Benutzer keine Rollenanspruch oder mehrere Rollenansprüche hat korrekt verarbeitet.

    Die **ClaimTypes** -Klasse definiert Konstanten für häufig verwendete Anspruchstypen. Allerdings können Sie eine Zeichenfolge für den Anspruchstyp.

-   Um einen einzelnen Wert für einen Anspruchstyp gibt höchstens einen Wert erwartet:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Die Werte für einen Anspruchstyp abrufen:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Weitere Informationen finden Sie unter [und ressourcenbasierte Autorisierung in mandantenfähigen Applikationen][authorization].

## <a name="next-steps"></a>Nächste Schritte

- Im nächsten Artikel dieser Reihe zu lesen: [Anmeldung und Mieter Onboarding in einer mehrinstanzenfähigen Anwendung][signup]
- Weitere Informationen zu [anspruchsbasierter Autorisierung] in ASP.NET Core 1.0-Dokumentation

<!-- Links -->
[Teil einer Serie]: guidance-multitenant-identity.md
[Bereichsparameter]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Unterstützt Token und Ansprüche]: ../active-directory/active-directory-token-and-claims.md
[Aussteller]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Authentifizierungsereignisse]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Anspruchsbasierte Autorisierung]: https://docs.asp.net/en/latest/security/authorization/claims.html
[Beispiel]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md

<properties
    pageTitle="Azure API Management – häufig gestellte Fragen | Microsoft Azure"
    description="Enthält Antworten auf häufig gestellte Fragen, Muster und Vorgehensweisen in Azure API Management."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure API Management FAQs

Antworten Sie auf häufig gestellte Fragen, Muster und Vorgehensweisen für Azure-API.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

-   [Wie kann ich Microsoft Azure API Management-Team eine Frage stellen?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Was bedeutet es, wenn ein Feature in der Vorschau ist?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Wie kann die Verbindung zwischen API Management Gateway und Meine Back-End-Dienste sichern?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Wie kopiere ich meine API Management-Dienstinstanz auf eine neue Instanz?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Kann ich meine API Management-Instanz programmgesteuert verwalten?](#can-i-manage-my-api-management-instance-programmatically)
-   [Wie füge ich einen Benutzer der Gruppe Administratoren?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Warum ist die Richtlinie im Systemrichtlinien-Editor nicht verfügbar soll?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Wie verwende ich Versioning API in API Management?](#how-do-i-use-api-versioning-in-api-management)
-   [Wie richte ich mehrere Umgebungen in einer einzigen API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Können SOAP-API Management werden verwendet?](#can-i-use-soap-with-api-management)
-   [Wird die Konstante API Management Gateway IP-Adresse? Kann ich es in Firewall-Regeln verwenden?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Kann ich einen autorisierungsserver OAuth 2.0 mit AD FS konfigurieren?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Welche Verteilungsmethode verwendet API Management bei Installationen an mehreren Standorten?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Kann ich eine Azure-Ressourcen-Manager-Vorlage zum Erstellen einer Instanz der API Management-Dienst verwenden?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Kann ich ein selbstsigniertes SSL-Zertifikat für einen Back-End verwenden?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Warum erhalte ich ein Authentifizierungsfehler beim Klonen GIT Repository?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Funktioniert API-Management mit Azure ExpressRoute](#does-api-management-work-with-azure-expressroute)
-   [Kann ich einen API Management-Dienst von einem Abonnement zu einem anderen verschieben?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Wie kann ich Microsoft Azure API Management-Team eine Frage stellen?

Sie können uns mit einer der folgenden Optionen:

-   Fragen Sie im [API Management MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Senden Sie eine e-Mail an <apimgmt@microsoft.com>.
-   Anfrage ein Feature in [Azure Bewertungsportal](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Was bedeutet es, wenn ein Feature in der Vorschau ist?

Ein Feature in der Vorschau ist, bedeutet, dass wir aktiv sind Feedback wie das Feature für Sie arbeiten. Eine Vorschau ist funktional abgeschlossen, aber ist es möglich, dass wir eine wichtige Reaktion auf Kundenfeedback geändert werden. Wir empfehlen, dass Sie auf eine Vorschau in Ihrem Unternehmen verlassen nicht. Haben Sie Feedback zu Vorschaufunktionen, Bitte teilen Sie uns über eine Kontaktoptionen in [wie kann ich Microsoft Azure API Management-Team eine Frage?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Wie kann die Verbindung zwischen API Management Gateway und Meine Back-End-Dienste sichern?

Sie haben mehrere Optionen zum Sichern der Verbindung zwischen dem API Management Gateway und der Back-End-Services. Sie können:

-   Verwenden Sie HTTP-Standardauthentifizierung. Weitere Informationen finden Sie unter [Konfigurieren API Settings](api-management-howto-create-apis.md#configure-api-settings).
- Verwenden Sie gegenseitige Authentifizierung SSL, wie im [Back-End-Services mit Clientzertifikatsauthentifizierung in Azure API Management schützen](api-management-howto-mutual-certificates.md).
- Verwenden Sie mithilfe der IP-Back-End-Dienst. Haben Sie eine Standard- oder Premium Tier API Management Instanz bleibt die IP-Adresse des Gateways. Sie können diese IP-Adresse zu Ihrer weißen Liste festlegen. Sie erhalten die IP-Adresse der API Management-Instanz auf dem Dashboard im Azure-Portal.
- Die API Management-Instanz zu einem virtuellen Azure-Netzwerk anschließen Weitere Informationen finden Sie unter [Einrichten von VPN-Verbindungen in Azure API Management](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Wie kopiere ich meine API Management-Dienstinstanz auf eine neue Instanz?

Wenn Sie eine API Management-Instanz auf eine neue Instanz kopieren möchten, stehen Ihnen verschiedene Optionen. Sie können:

-   Sollten die Sicherung und Wiederherstellung Funktion in API Management. Weitere Informationen finden Sie unter [Implementierung von Disaster Recovery mit Service-Sicherung und Wiederherstellung in Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
-   Erstellen Sie eigene Sicherungskopie und Wiederherstellen Sie Feature mithilfe der [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Verwenden Sie die REST-API zum Speichern und Wiederherstellen von Entitäten von der Dienstinstanz soll.
-   Herunterladen Sie die Dienstkonfiguration mit Git und Hochladen Sie einer neuen Instanz. Weitere Informationen finden Sie unter [Speichern und Ihre Konfiguration mit Git API Management konfigurieren](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Kann ich meine API Management-Instanz programmgesteuert verwalten?

Ja, können Sie die API-Management mit programmgesteuert verwalten:

-   Die [API Management REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   [Microsoft Azure ApiManagement Service Management Library SDK](http://aka.ms/apimsdk).
-   [Bereitstellung](https://msdn.microsoft.com/library/mt619282.aspx) und [Servicemanagement](https://msdn.microsoft.com/library/mt613507.aspx) -PowerShell-Cmdlets.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Wie füge ich einen Benutzer der Gruppe Administratoren?

Sieht aus wie einen Benutzer zur Administratorengruppe hinzufügen können:

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2. Gehen Sie die Ressourcengruppe mit der API Management-Instanz, die Sie aktualisieren möchten.
3. Benutzer in API Management weisen Sie **Api Management** Teilnehmerrolle zu.

Die neu hinzugefügten Teilnehmer kann nun Azure PowerShell- [Cmdlets](https://msdn.microsoft.com/library/mt613507.aspx)verwenden. Hier wird als Administrator anmelden:

1. Verwenden der `Login-AzureRmAccount` -Cmdlet zum Anmelden.
2. Das Abonnement, das den Dienst mit den Kontext festlegen `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Abrufen eine URL Anmelden mit `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Verwenden Sie die URL auf der Administratorportal.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Warum ist die Richtlinie im Systemrichtlinien-Editor nicht verfügbar soll?

Wird die Richtlinie, die Sie hinzufügen möchten, abgeblendet oder schattierten im Systemrichtlinien-Editor, sicher sein, dass Sie im richtigen Bereich für die Richtlinie. Jede Erklärung dient zur Verwendung in bestimmten Bereichen und Richtlinien. Richtlinie Abschnitte und Bereiche für eine Richtlinie finden Sie in der Richtlinie im Abschnitt Verwendung in [API-Richtlinien](https://msdn.microsoft.com/library/azure/dn894080.aspx).


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Wie verwende ich Versioning API in API Management?

Sie haben einige Optionen mit Versionskontrolle API in API Management:

-   API-Verwaltung können Sie APIs stellen verschiedene Versionen konfigurieren. Beispielsweise müssen Sie zwei verschiedene APIs, MyAPIv1 und MyAPIv2. Entwickler können die Version, die der Entwickler verwenden.
-   Sie können auch Ihre API mit einer URL, die eine Versionssegment beispielsweise https://my.api enthält. Konfigurieren Sie ein Versionssegment für jede Operation [Schreiben URL](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) -Vorlage. Beispielsweise haben Sie einen Vorgang mit einer [URL-Vorlage](api-management-howto-add-operations.md#url-template) namens/Resource und [URL schreiben](api-management-howto-add-operations.md#rewrite-url-template) Vorlage namens /v1/Resource. Sie können Version Segment Wert für jede Operation separat ändern.
-   Möchten die API Service URL Standardsegment "Version" zu ausgewählten Vorgänge eine Richtlinie festgelegt, die Richtlinie [Back-End-Dienst](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) verwendet, um den Pfad der Back-End-Anforderung ändern.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Wie richte ich mehrere Umgebungen in einer einzigen API?

Um mehreren Umgebungen eine produktionsumgebung in einer einzigen API einrichten und haben Sie zwei Optionen. Sie können:

-   Host verschiedene APIs auf demselben Mandanten.
-   Die gleichen APIs auf verschiedenen Mandanten zu hosten.

### <a name="can-i-use-soap-with-api-management"></a>Können SOAP-API Management werden verwendet?

[SOAP-Pass-Through-](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) Unterstützung steht. Administratoren können die WSDL SOAP-Dienst, und Azure API Management erstellen SOAP-front-End. Portal Entwicklerdokumentation, Test-Konsole Richtlinien und Analysen stehen für SOAP-Dienste.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Wird die Konstante API Management Gateway IP-Adresse? Kann ich es in Firewall-Regeln verwenden?

Auf der Standard- und Premium ist die öffentliche IP-Adresse (VIP) API Management Mieter lang Mieter Ausnahmen statisch. IP-Adresse ändert in folgenden Fällen:

-   Der Dienst wird gelöscht und neu erstellt.
-   Dauerauftrags ist (z.B. Nichtzahlung) angehalten und dann fortgesetzt.
-   Hinzufügen oder Entfernen von Azure Virtual Network (virtuelles Netzwerk nur auf Premium-Stufe können).

Für Installationen mit mehreren regionale Adresse ändert, wenn der Bereich verlassen und dann wieder (können mit mehreren Bereitstellung nur auf Premium-Stufe).

Premium-Tier Mieter, die mit mehreren Bereitstellung konfiguriert werden eine öffentliche IP-Adresse pro Region zugewiesen.

Sie können die IP-Adresse (oder Adressen in einer Bereitstellung mit mehreren) auf Mieter im Azure-Portal abrufen.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Kann ich einen autorisierungsserver OAuth 2.0 mit AD FS konfigurieren?

Konfigurieren einer OAuth 2.0 Autorisierung Servers mit Active Directory-Verbunddienste (AD FS) finden Sie unter [ADFS API-Management verwenden](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Welche Verteilungsmethode verwendet API Management bei Installationen an mehreren Standorten?

API Management verwendet die [Leistung Datenverkehr Routingmethode](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) in Installationen an mehreren Standorten. Eingehender Datenverkehr wird an der nächsten API-Gateway weitergeleitet. Wenn eine Region offline geht, wird eingehender Datenverkehr automatisch zum nächsten nächsten Gateway weitergeleitet. Weitere Informationen über Verteilermethoden [Traffic Manager](../traffic-manager/traffic-manager-routing-methods.md)Routing Methoden.

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Kann ich eine Azure-Ressourcen-Manager-Vorlage zum Erstellen einer Instanz der API Management-Dienst verwenden?

Ja. [Azure API-Verwaltungsdienst](http://aka.ms/apimtemplate) Schnellstart Vorlagen anzuzeigen.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Kann ich ein selbstsigniertes SSL-Zertifikat für einen Back-End verwenden?

Ja. Hier ist ein selbstsigniertes Zertifikat (SSL = Secure Sockets Layer) für Back-End verwenden:

1. Erstellen Sie [Back-End-](https://msdn.microsoft.com/library/azure/dn935030.aspx) Entität mithilfe der API-Verwaltung.
2. Die **SkipCertificateChainValidation** -Eigenschaft auf **true**festgelegt.
3. Wenn Sie nicht mehr selbst signierte Zertifikate zulassen möchten, Back-End-Entität löschen oder **SkipCertificateChainValidation** -Eigenschaft auf **false**festgelegt.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Warum erhalte ich ein Authentifizierungsfehler beim Klonen Git Repository?

Git Anmeldeinformations-Manager verwenden oder wenn Sie ein Repository Git Klonen mit Visual Studio können Sie in ein bekanntes Problem mit der Windows-Anmeldeinformationen ausführen. Das Dialogfeld auf 127 Zeichen beschränkt und kürzt Microsoft generierten Kennwort. Wir arbeiten Verkürzung des Kennworts. Jetzt verwenden Sie Git Bash Git-Repository klonen.

### <a name="does-api-management-work-with-azure-expressroute"></a>Funktioniert API-Management mit Azure ExpressRoute

Ja. API-Management arbeitet mit Azure ExpressRoute.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Kann ich einen API Management-Dienst von einem Abonnement zu einem anderen verschieben?

Ja. Um zu erfahren, finden Sie [Ressourcen auf eine neue Ressourcengruppe oder Abonnement](../resource-group-move-resources.md).

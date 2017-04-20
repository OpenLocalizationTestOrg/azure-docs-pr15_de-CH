<properties
   pageTitle="Konfigurierbare Token Lebensdauer in Azure Active Directory | Microsoft Azure"
   description="Dieses Feature wird von Administratoren und Entwickler an die Lebensdauer von Azure AD ausgestellten Token verwendet."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/06/2016"
   ms.author="billmath"/>


# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Konfigurierbare Token Lebensdauer in Azure Active Directory (Public Preview)

>[AZURE.NOTE]
>Diese Funktion ist derzeit public Preview-Version.  Sie sollten bereit sein, zurücksetzen oder Löschen geändert.  Öffnen wir dieses Feature für alle während der öffentlichen Vorschau versuchen, bestimmte Aspekte können jedoch ein [Azure AD Premium-Abonnement](active-directory-get-started-premium.md) nach erhältlich.


## <a name="introduction"></a>Einführung
Dieses Feature wird von Administratoren und Entwickler an die Lebensdauer von Azure AD ausgestellten Token verwendet. Token Lebensdauer können für alle apps in einem Mandanten, Multi-Tenant-Anwendung oder für einen bestimmten Dienstprinzipalnamen in einem Mandanten konfiguriert werden.

In Azure AD stellt ein Gruppenrichtlinienobjekt einen Satz von Regeln für einzelne Programme oder alle Programme in einem Mandanten erzwungen.  Jede Richtlinie hat eine eindeutige Struktur mit einem Satz von Eigenschaften, die auf Objekte angewendet werden, die sie zugeordnet sind.

Eine Richtlinie kann ein als Standard festgelegt werden. Diese Richtlinie hat Einfluss auf jede Anwendung, die in befindet, die Mieter als durch eine Richtlinie mit höherer Priorität nicht überschrieben wird. Richtlinien können auch bestimmte Applikationen zugewiesen werden. Die Rangfolge hängt vom Typ ab.

Für Aktualisierungstoken, Zugriffstoken Sitzungstoken und ID-Token können Tokengültigkeitsdauer Richtlinien konfiguriert werden.


### <a name="access-tokens"></a>Zugriffstoken
Zugriffstoken wird von einem Client verwendet, auf eine geschützte Ressource zugreifen. Ein Zugriffstoken kann nur für eine bestimmte Kombination von Benutzer, Clients und Ressourcen verwendet werden. Zugangs-Token können nicht gesperrt werden und bis zu ihrem Ablauf gültig sind. Ein bösartiger Akteur, der ein Zugriffstoken abgerufen wurde können für Erweiterung der Lebensdauer.  Anpassen des Zugriffs Tokengültigkeitsdauer ist ein Kompromiss zwischen bessere Systemleistung und wie lange der Client Access behält, nachdem das Benutzerkonto deaktiviert ist.  Verbesserte Systemleistung erzielten oft Cient frische Zugriffstoken zu reduzieren. 

### <a name="refresh-tokens"></a>Aktualisieren von Token
Wenn ein Client ein Zugriffstoken Zugriff auf eine geschützte Ressource erwirbt, erhält ein Aktualisierungstoken und ein Zugriffstoken. Aktualisierungstoken wird verwendet, zu neuen Access-Aktualisierung token-Paare beim aktuellen Zugriffstoken abgelaufen ist. Aktualisieren Token sind Kombinationen aus Benutzer und Kunden verpflichtet. Können gesperrt werden und ihre Gültigkeit überprüft jedes Mal verwendet werden.

Es ist wichtig, vertraulich und öffentliche Clients unterscheiden. Vertrauliche Clients sind die Client-Kennwort sicher speichern zu beweisen, dass Anfragen aus der Clientanwendung und nicht bösartige Akteur. Wie diese sicherer, die standardmäßige Lebensdauer der Aktualisierung sind diese Datenflüsse ausgestellte Token liegen und können mithilfe von Gruppenrichtlinien geändert werden.

Öffentliche Auftraggeber können aufgrund der Umgebung, die in der Anwendung ausgeführt, ein Client sicher zu speichern. Richtlinien können auf Ressourcen Aktualisierungstoken öffentliche Auftraggeber, die älter als eine bestimmte Periode verhindern erhalten ein neues token Access-aktualisieren-Paar (Token Max inaktiv Aktualisierungszeit) festgelegt werden.  Darüber hinaus Richtlinien lässt sich eine Frist, Token sind nicht mehr aktualisieren (Refresh Token Max Age) akzeptiert.  Anpassen Tokengültigkeitsdauer aktualisieren können Sie steuern, wann und wie oft der Benutzer Anmeldeinformationen nicht automatisch erneut authentifiziert einen öffentlich-Anwendung erneut eingeben muss.


### <a name="id-tokens"></a>ID-Token
ID-Token zu Websites und systemeigenen Clients übergeben und Profilinformationen über einen Benutzer enthalten. Ein ID-Token wird an eine bestimmte Kombination von Benutzer und Client gebunden. ID-Token sind bis zum Ablauf gültig.  Normalerweise entspricht eine Anwendung einen Benutzer der Gültigkeitsdauer der Anwendung an die Lebensdauer des Token-ID für den Benutzer ausgegeben.  Anpassen Tokengültigkeitsdauer ID können Sie steuern, wie oft die Anwendung anwendungssitzung ab und Benutzer mit Azure (automatisch oder interaktiv) erneut authentifiziert werden.

### <a name="single-sign-on-session-token"></a>Sitzung für einzelne Zeichen
Wenn Benutzerauthentifizierung mit Azure ist eine einzige Sitzung anmelden mit der Browser des Benutzers und Azure AD hergestellt.  Single Sign-On Sitzung Token, in Form eines Cookies, stellt dieser Sitzung. Es ist wichtig zu beachten, dass Sitzungstoken SSO nicht an eine bestimmte Ressource-Client-Anwendung gebunden ist. SSO Sitzungstoken gesperrt werden und ihre Gültigkeit werden jedes Mal verwendet werden.

Es gibt zwei Arten von SSO Sitzungstoken. Permanente Sitzungstoken werden vom Browser und nicht persistent Sitzungstoken als Sitzungscookies gespeichert sind (diese werden zerstört, wenn der Browser geschlossen wird) gespeichert.

Nicht persistent Sitzungstoken haben eine Lebensdauer von 24 Stunden permanente Token Lebensdauer 180 Tage. Jederzeit verwendeten Sitzungstoken SSO innerhalb der Gültigkeitsdauer wird die Gültigkeitsdauer einer anderen 24 Stunden oder 180 Tage erweitert. SSO-Sitzung verwendeten Token nicht innerhalb der Gültigkeitsdauer Auffassung ist abgelaufen und nicht mehr akzeptiert. 

Richtlinien dienen zum Festlegen einer Zeitspanne, nach die erste Sitzung Token ausgestellt wurde, die Sitzungstoken nicht mehr akzeptiert (Sitzung Token Max Age).  Anpassen des token Sitzungsdauer können Sie steuern, wann und wie oft der Benutzer muss Anmeldeinformationen statt automatisch authentifiziert werden, wenn eine Web-Anwendung erneut eingeben.

### <a name="token-lifetime-policy-properties"></a>Tokengültigkeitsdauer Eigenschaften
Ein token Lebensdauerrichtlinien ist ein Richtlinienobjekt, die Tokengültigkeitsdauer Regeln enthält.  Die Eigenschaften der Richtlinie zum angegebenen token Lebensdauer steuern.  Wenn keine Richtlinie festgelegt ist, setzt das System Standard-Gültigkeitsdauer.


### <a name="configurable-token-lifetime-properties"></a>Konfigurierbare Tokengültigkeitsdauer Eigenschaften
Eigenschaft|Eigenschaftenzeichenfolge Richtlinie|Betrifft|Standard|Mindestens|Maximale|
----- | ----- | ----- |----- | ----- | ----- |
Tokengültigkeitsdauer Zugriff|  AccessTokenLifetime|Zugriffstoken, ID-Token SAML2 Token|1 Stunde|10 Minuten|1 Tag|
Aktualisieren von Token Höchstzeit inaktiv|    MaxInactiveTime |Aktualisieren von Token |14 Tage|10 Minuten|    90 Tage|
Einfache Aktualisierung Token Höchstalter|    MaxAgeSingleFactor  |Aktualisieren von Token (für alle Benutzer) |90 Tage|10 Minuten |Bis widerrufen *|
Token Höchstalter mehrstufige aktualisieren| MaxAgeMultiFactor|  Aktualisieren von Token (für alle Benutzer)| 90 Tage|10 Minuten|Bis widerrufen *|
Einfache Sitzung Token Höchstalter |MaxAgeSessionSingleFactor **    |Sitzungstoken (persistent und nicht persistent)| Bis widerrufen   |10 Minuten |Bis widerrufen *|
Mehrstufige Sitzung Token Höchstalter| MaxAgeSessionMultiFactor ***|    Sitzungstoken (persistent und nicht persistent)| Bis widerrufen|  10 Minuten| Bis widerrufen *



- * 365 Tage sind explizite maximal, die für diese Attribute festgelegt werden kann.
- ** Wenn MaxAgeSessionSingleFactor nicht festgelegt hat dieser Wert der MaxAgeSingleFactor-Wert. Keine Parameter festgelegt ist, nimmt die Eigenschaft auf den Standardwert (bis-gesperrt).
- Wenn MaxAgeSessionMultiFactor nicht festgelegt hat dieser Wert der MaxAgeMultiFactor-Wert. Keine Parameter festgelegt ist, nimmt die Eigenschaft auf den Standardwert (bis-gesperrt).

### <a name="exceptions"></a>Ausnahmen
Eigenschaft|Betrifft|Standard|
----- | ----- | ----- |
Aktualisierungszeit Token Max inaktiv (Föderationsbenutzer mit unzureichender Sperrinformationen)|Aktualisieren von Token (für Verbundbenutzer mit unzureichender Sperrinformationen ausgegeben)|12 Stunden|
Aktualisierungszeit Token Max inaktiv (vertrauliche Clients)| Aktualisieren von Token (ausgestellt für vertrauliche Clients)|90 Tage|
Aktualisieren von token Max Age (ausgestellt für vertrauliche Clients) |   Aktualisieren von Token (ausgestellt für vertrauliche Clients) |Bis widerrufen

### <a name="priority-and-evaluation-of-policies"></a>Priorität und Auswertung von Richtlinien

Token Lifetime-Richtlinien können erstellt und bestimmte Programme, Mieter und Service Hauptbenutzer zugewiesen. Dies bedeutet, dass mehrere Richtlinien auf eine bestimmte Anwendung angewendet werden. Die Tokengültigkeitsdauer-Richtlinie, die wirksam gelten folgende Regeln:


- Wenn eine Richtlinie der Dienstprinzipalnamen explizit zugewiesen ist, werden erzwungen. 
- Wenn keine Richtlinie Service Principal explizit zugewiesen ist, wird eine übergeordnete Mieter Service principal explizit zugewiesene Richtlinie erzwungen. 
- Wenn Service principal oder Pächter explizit keine Richtlinie zugewiesen ist, wird der Anwendung zugewiesene Richtlinie erzwungen. 
- Wenn keine der Dienstprinzipalnamen, Pächter oder Application-Objekt zugewiesen wurde, werden die Standardwerte erzwungen (siehe obige Tabelle).

Finden Sie weitere Informationen zur Beziehung zwischen Anwendung und Service principal-Objekten in Azure AD [Anwendung und Service principal-Objekte in Active Directory Azure](active-directory-application-objects.md).

Gültigkeitsdauer des Tokens wird gleichzeitig ausgewertet wird. Wirkt sich die Richtlinie mit der höchsten Priorität in der Anwendung zugegriffen wird.


>[AZURE.NOTE]
>Beispiel
>
>Ein Benutzer möchte auf 2 ASP.NET-Webanwendungen A und B. 
>
>
>- Beide sind im gleichen übergeordneten Mandanten. 
>- Token Lebensdauerrichtlinien 1 mit einer Sitzung Token Max Age 8 Stunden wird standardmäßig der übergeordneten Mieter festgelegt.
>- Eine Anwendung ist eine normale Verwendung und ist nicht für Richtlinien verknüpft. 
>- Anwendung B für vertrauliche Prozesse verwendet und der Dienstprinzipalnamen wird mit token Lebensdauerrichtlinien 2 mit einer Sitzung Token Max-Age von 30 Minuten.
>
>12:00 Uhr der Benutzer eine neue Browsersitzung geöffnet und versucht, Anwendung a Zugriff der Benutzer in Azure AD umgeleitet und zum Anmelden aufgefordert. Dadurch wird einen Cookie mit einem Sitzungstoken im Browser. Der Benutzer wird umgeleitet, um Anwendung mit einem Token-ID Webserver, die auf die Anwendung zugreifen können.
>
>Um 12:15 Uhr versucht der Benutzer dann auf Anwendung B. Der Browser leitet Azure AD Sitzungscookie erkennt. Web Application B Dienstprinzipalnamen Richtlinie 1 verknüpft, jedoch auch Teil der übergeordneten Mieter Standardrichtlinie 2. Richtlinie 2 wirksam, da Maßnahmen für Service Prinzipale eine höhere Priorität als Pächter Standardrichtlinien haben. Das Sitzungstoken wurde ursprünglich innerhalb der letzten 30 Minuten ausgestellt, damit es gültig ist. Der Benutzer wird mit ihnen Zugriff gewähren ID-Token an Anwendung B umgeleitet.
>
>1:00 Uhr versucht er zu a Web Application. Der Benutzer ist in Azure AD umgeleitet. Eine Anwendung nicht für Richtlinien verknüpft, aber ist in einem Mandanten mit Standard 1 dieser Richtlinie wirksam. Sitzung innerhalb von 8 Stunden und der Benutzer ursprünglich ausgegebene Cookie erkannt wird automatisch ins Web-Anwendung eine neue ID-Token ohne Authentifizierung umgeleitet.
>
>Der Benutzer sofort auf Anwendung B. Der Benutzer ist in Azure AD umgeleitet. Als Richtlinie 2, wirksam wird. Länger als 30 Minuten das Token ausgestellt wurde, der Benutzer wird aufgefordert, ihre Anmeldeinformationen erneut eingeben, und eine neue Sitzung und Token-ID ausgestellt. Der Benutzer kann dann Anwendung b zugreifen.

## <a name="configurable-policy-properties-in-depth"></a>Konfigurierbare Eigenschaften: ausführliche

### <a name="access-token-lifetime"></a>Tokengültigkeitsdauer Zugriff

**Zeichenfolge:** AccessTokenLifetime

**Betrifft:** Zugriffstoken, ID-Token

**Zusammenfassung:** Diese Richtlinie steuert, wie lange Zugriff und ID-Token für diese Ressource als gültig betrachtet werden. Reduzieren der Gültigkeitsdauer Access token vermindert das Risiko von Access- oder Token-ID verwendeten bösartige Schauspieler für einen längeren Zeitraum (wie sie gesperrt werden können) jedoch beeinträchtigt die Leistung auch als Token häufiger ersetzt werden müssen.

### <a name="refresh-token-max-inactive-time"></a>Aktualisierungszeit token max inaktiv

**Zeichenfolge:** MaxInactiveTime

**Betrifft:** Aktualisieren von Token

**Zusammenfassung:** Diese Richtlinie steuert, wie ALT ein Aktualisierungstoken sein kann, bevor ein Client nicht mehr beim Zugriff auf diese Ressource ein neues Access-Aktualisierung Paar token abrufen verwenden kann. Da ein neues Aktualisierungstoken Aktualisierungstoken verwendet in der Regel zurückgegeben wird, muss der Client nicht erreicht haben auf eine Ressource mit dem aktuellen Aktualisierungstoken für den angegebenen Zeitraum vor dieser Richtlinie Zugriff verhindern würden. 

Diese Richtlinie erzwingt Benutzern, die nicht auf ihren Client neu authentifizieren ein neues Aktualisierungstoken abzurufen. 

Es ist wichtig zu beachten, dass das Token Max inaktiv Aktualisierungszeit auf einen niedrigeren Wert als einfache Token Max Age und mehrstufige aktualisieren Token Max Age festgelegt werden muss.

### <a name="single-factor-refresh-token-max-age"></a>Einfache Aktualisierung token Höchstalter

**Zeichenfolge:** MaxAgeSingleFactor

**Betrifft:** Aktualisieren von Token

**Zusammenfassung:** Diese Richtlinie steuert, wie lange kann Benutzer weiterhin Aktualisierungstoken verwenden, um neue Access-Aktualisierung token Paare nach der letzten Authentifizierung erfolgreich mit nur einem einzigen Faktor. Nachdem ein Benutzer authentifiziert und ein neue Aktualisierungstoken erhält, sie werden aktualisieren token Flow verwenden (solange das aktuelle Aktualisierungstoken nicht gesperrt ist und nicht für länger inaktiv gelassen) für den angegebenen Zeitraum. An diesem Punkt werden Benutzer gezwungen erneut authentifizieren, um ein neues Aktualisierungstoken zu erhalten. 

Reduzieren das maximale Alter zwingt Benutzer mehr authentifizieren. Da eine Einfaktorauthentifizierung weniger sicher als eine mehrstufige Authentifizierung gilt, wird empfohlen, dass diese Richtlinie auf einen Wert gleich oder kleiner als die kombinierten aktualisieren Token maximale Alter festgelegt ist.

### <a name="multi-factor-refresh-token-max-age"></a>Token Höchstalter mehrstufige aktualisieren

**Zeichenfolge:** MaxAgeMultiFactor

**Betrifft:** Aktualisieren von Token

**Zusammenfassung:** Diese Richtlinie steuert, wie lange kann Benutzer weiterhin Aktualisierungstoken verwenden, um neue Access-Aktualisierung token Paare nach der letzten Authentifizierung erfolgreich mit mehreren Faktoren. Nachdem ein Benutzer authentifiziert und ein neue Aktualisierungstoken erhält, sie werden aktualisieren token Flow verwenden (solange das aktuelle Aktualisierungstoken nicht gesperrt ist und nicht für länger inaktiv gelassen) für den angegebenen Zeitraum. An diesem Punkt werden Benutzer gezwungen erneut authentifizieren, um ein neues Aktualisierungstoken zu erhalten. 

Reduzieren das maximale Alter zwingt Benutzer mehr authentifizieren. Da eine Einfaktorauthentifizierung weniger sicher als eine mehrstufige Authentifizierung gilt, wird empfohlen, dass diese Richtlinie auf einen Wert gleich oder größer als die einfache aktualisieren Token maximale Alter festgelegt ist.

### <a name="single-factor-session-token-max-age"></a>Einfache Sitzung token Höchstalter

**Zeichenfolge:** MaxAgeSessionSingleFactor

**Betrifft:** Sitzungstoken (persistent und nicht persistent)

**Zusammenfassung:** Diese Richtlinie steuert, wie lange kann Benutzer weiterhin Sitzungstoken zu neuer Token-ID und die Sitzung nach der letzten erfolgreich mit nur einem einzigen Faktor Authentifizierung verwenden. Nachdem ein Benutzer authentifiziert und einen neue Sitzungstoken erhält, werden sie mithilfe den Sitzung token Fluss (solange das aktuelle Sitzungstoken nicht gesperrt oder abgelaufen ist) für den angegebenen Zeitraum. An diesem Punkt werden Benutzer gezwungen erneut authentifizieren, um neue Sitzung zu erhalten. 

Reduzieren das maximale Alter zwingt Benutzer mehr authentifizieren. Da eine Einfaktorauthentifizierung weniger sicher als eine mehrstufige Authentifizierung gilt, wird empfohlen, dass diese Richtlinie auf einen Wert gleich oder kleiner als die kombinierten Sitzung Token maximale Alter festgelegt ist.

### <a name="multi-factor-session-token-max-age"></a>Mehrstufige Sitzung token Höchstalter

**Zeichenfolge:** MaxAgeSessionMultiFactor

**Betrifft:** Sitzungstoken (persistent und nicht persistent)

**Zusammenfassung:** Diese Richtlinie steuert, wie lange kann Benutzer weiterhin Sitzungstoken zu neuer Token-ID und die Sitzung nach der letzten erfolgreich mit mehreren Authentifizierung verwenden. Nachdem ein Benutzer authentifiziert und einen neue Sitzungstoken erhält, werden sie mithilfe den Sitzung token Fluss (solange das aktuelle Sitzungstoken nicht gesperrt oder abgelaufen ist) für den angegebenen Zeitraum. An diesem Punkt werden Benutzer gezwungen erneut authentifizieren, um neue Sitzung zu erhalten. 

Reduzieren das maximale Alter zwingt Benutzer mehr authentifizieren. Da eine Einfaktorauthentifizierung weniger sicher als eine mehrstufige Authentifizierung gilt, wird empfohlen, dass diese Richtlinie auf einen Wert gleich oder größer als die einfache Sitzung Token maximale Alter festgelegt ist.

## <a name="sample-token-lifetime-policies"></a>Beispiel-token Lifetime-Richtlinien

Möglichkeit zum Erstellen und Verwalten von token Lebensdauer für apps, Service Prinzipalen und Ihren gesamten Mandanten macht alle Arten von neuen Szenarien in Azure AD möglich.  Wir werden einige häufige Szenarios für Richtlinien durchgehen, mit denen Sie die neuen Regeln für:


- Token Lebensdauer
- Token Max inaktive Zeit
- Token Höchstalter

Wir gehen einige Szenarios, einschließlich:


- Verwalten des Mieters Standardrichtlinie
- Erstellen einer Richtlinie für Web-Anmeldung
- Erstellen einer Richtlinie für eine Web-API Aufrufen systemeigenen Apps
- Verwalten von Richtlinie Erweitert 

### <a name="prerequisites"></a>Erforderliche Komponenten
In den Beispielszenarien werden wir werden erstellen, aktualisieren, verknüpfen und Richtlinien apps Service Prinzipalen und Ihren gesamten Mandanten löschen.  Wenn Sie in Azure AD Auschecken [dieses Artikels](active-directory-howto-tenant.md) sind, bevor Sie mit diesen Beispielen Einstieg.  


1. Zunächst herunterladen Sie die neuesten [Azure AD PowerShell-Cmdlet Preview](https://www.powershellgallery.com/packages/AzureADPreview) 
2.  Haben Sie den Azure AD PowerShell-Cmdlets, Befehl Verbinden Ihr Azure AD-Administratorkonto anmelden. Sie müssen dieses Verfahren bei jedem einer neuen Sitzung starten.
        
        Connect-AzureAD -Confirm

3.  Führen Sie den folgenden Befehl, um alle Richtlinien anzuzeigen, die in Ihrem Mandanten erstellt wurden.  Dieser Befehl sollte nach den meisten Operationen in den folgenden Szenarios verwendet werden.  Es wird auch der **Objekt-ID** der Richtlinien helfen. 
        
        Get-AzureADPolicy

### <a name="sample-managing-a-tenants-default-policy"></a>Beispiel: Verwalten des Mieters Standardrichtlinie

In diesem Beispiel wird eine Richtlinie erstellt, die den Benutzern ermöglicht, über Ihre gesamte Mieter seltener anmelden. 

Dazu erstellen wir ein token Lebensdauerrichtlinien für einfache aktualisieren Token, die in Ihrem Mandanten angewendet wird. Diese Richtlinie wird auf jede Anwendung in Ihrem Mandanten und jeden Service principal, der eine Richtlinie, noch nicht angewendet werden. 

1.  **Erstellen einer Richtlinie Tokengültigkeitsdauer.** 

Legen Sie einfache aktualisieren Token "bis-widerrufen" Bedeutung ablaufen wird nicht erst Zugriff gesperrt wird.  Policy-Definition unten ist, was wir schaffen:
        
        @("{
          `"TokenLifetimePolicy`":
              {
                 `"Version`":1, 
                 `"MaxAgeSingleFactor`":`"until-revoked`"
              }
        }")

Führen Sie den folgenden Befehl zur Erstellung dieser Richtlinie. 

        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1, `"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName TenantDefaultPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
        
Führen Sie den folgenden Befehl, um die neue Richtlinie und ObjectID.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **Aktualisieren Sie die Richtlinie**

Sie haben entschieden, dass die erste Richtlinie nicht sehr strenge und Ihren Dienst erfordert, haben einfache Aktualisieren von Token, die in zwei Tagen ablaufen soll. Führen Sie den folgenden Befehl ein. 
        
    Set-AzureADPolicy -ObjectId <ObjectID FROM GET COMMAND> -DisplayName TenantDefaultPolicyUpdatedScenario -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"2.00:00:00`"}}")
        
&nbsp;&nbsp;3. **Fertig!** 

### <a name="sample-creating-a-policy-for-web-sign-in"></a>Beispiel: Erstellen einer Richtlinie für Web-Anmeldung

In diesem Beispiel wird eine Richtlinie erstellt, die die Benutzer authentifizieren häufiger in Ihrer Anwendung benötigen. Diese Richtlinie wird die Lebensdauer des Zugriffs-Id-Token und Max Age eine mehrstufige Sitzung Token auf Dienstprinzipalnamen von Ihrer Anwendung festgelegt.

1.  **Erstellen einer Richtlinie Tokengültigkeitsdauer.**

Diese Richtlinie Web-Anmeldung wird die Access-Id-Token und die einfache Sitzung Token Höchstalter auf 2 Stunden festgelegt.

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"AccessTokenLifetime`":`"02:00:00`",`"MaxAgeSessionSingleFactor`":`"02:00:00`"}}") -DisplayName WebPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy

Führen Sie den folgenden Befehl, um die neue Richtlinie und ObjectID.

    Get-AzureADPolicy
&nbsp;&nbsp;2. **weisen die Richtlinie der Dienstprinzipalnamen.**

Wir werden dieser neue Richtlinie mit einem Service verknüpfen.  Sie benötigen auch die **ObjectId** des Service principal zugreifen. [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) abgefragt oder besuchen Sie unser [Diagramm Explorertools](https://graphexplorer.cloudapp.net/) , und Azure AD-Konto an Ihr Mieters Service Hauptbenutzer anmelden. 

Haben Sie die **ObjectId**, führen Sie den folgenden Befehl ein.
        
    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
&nbsp;&nbsp;3. **Fertig!** 

 

### <a name="sample-creating-a-policy-for-native-apps-calling-a-web-api"></a>Beispiel: Erstellen einer Richtlinie für eine Web-API Aufrufen systemeigenen apps

>[AZURE.NOTE]
>Richtlinien für Clientanwendungen verknüpfen deaktiviert.  Wir arbeiten auf diesem kurz.  Diese Seite wird aktualisiert, sobald die Funktion verfügbar ist.

In diesem Beispiel werden erstellen eine Richtlinie, die weniger Authentifizierung erfordert und verlängert die Zeitdauer, die sie inaktiv sein können, ohne erneut authentifizieren. Die Richtlinie gelten für die Web-API so, wenn systemeigene Anwendung es eine Ressource anfordert, die diese Richtlinie angewendet wird.

1.  **Erstellen einer Richtlinie Tokengültigkeitsdauer.** 

Dieser Befehl erstellt eine strenge Richtlinien für eine Web-API. 
        
    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"30.00:00:00`",`"MaxAgeMultiFactor`":`"until-revoked`",`"MaxAgeSingleFactor`":`"180.00:00:00`"}}") -DisplayName WebApiDefaultPolicyScenario -IsTenantDefault $false -Type TokenLifetimePolicy
         
Führen Sie den folgenden Befehl, um die neue Richtlinie und ObjectID.

    Get-AzureADPolicy

&nbsp;&nbsp;2. **weisen Sie die Richtlinie Ihrer Web-API**.

Wir werden dieser neue Richtlinie mit einer Anwendung verknüpft.  Sie benötigen auch die **ObjectId** der Anwendung zugreifen. Die beste Möglichkeit, Ihre app **ObjectId** ist der [Azure-Portal](https://portal.azure.com/)verwenden. 

Haben Sie die **ObjectId**, führen Sie den folgenden Befehl ein.

    Add-AzureADApplicationPolicy -ObjectId <ObjectID of the App> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **Fertig!** 

### <a name="sample-managing-an-advanced-policy"></a>Beispiel: Eine erweiterte Richtlinie verwalten 

In diesem Beispiel erstellen wir ein paar Richtlinien demonstrieren die Priorität Funktionsweise und Verwaltung mehrerer Richtlinien auf mehrere Objekte angewendet. Diese geben einen Einblick in die Priorität der oben erläuterten und hilft auch kompliziertere verwalten. 

1.  **Erstellen einer Richtlinie Tokengültigkeitsdauer**

Bisher ziemlich einfach. Wir haben eine Standardrichtlinie Mieter, die Gültigkeitsdauer des Tokens einfache aktualisieren auf 30 Tage festgelegt. 

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"30.00:00:00`"}}") -DisplayName ComplexPolicyScenario -IsTenantDefault $true -Type TokenLifetimePolicy
Um die neue Richtlinie und es ist ObjectID den folgenden Befehl ausführen.
 
    Get-AzureADPolicy

&nbsp;&nbsp;2. **weisen Sie die Richtlinie auf einen Dienstprinzipal**

Jetzt haben wir eine gesamte Mandanten.  Angenommen, wir möchten dieser 30-Tage-Richtlinie für einen bestimmten Dienstprinzipalnamen beibehalten, aber die Standardrichtlinie Mieter die Obergrenze des "bis-widerrufen" zu ändern. 

Zunächst werden wir diese neue Richtlinie mit Service Principal verknüpfen.  Sie benötigen auch die **ObjectId** des Service principal zugreifen. [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) abgefragt oder besuchen Sie unser [Diagramm Explorertools](https://graphexplorer.cloudapp.net/) , und Azure AD-Konto an Ihr Mieters Service Hauptbenutzer anmelden. 

Haben Sie die **ObjectId**, führen Sie den folgenden Befehl ein.

    Add-AzureADServicePrincipalPolicy -ObjectId <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>

&nbsp;&nbsp;3. **IsTenantDefault Flag false mit dem folgenden Befehl festlegen**. 

    Set-AzureADPolicy -ObjectId <ObjectId of Policy> -DisplayName ComplexPolicyScenario -IsTenantDefault $false
&nbsp;&nbsp;4. **erstellen eine neue Tenant-Standardrichtlinie**

    New-AzureADPolicy -Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxAgeSingleFactor`":`"until-revoked`"}}") -DisplayName ComplexPolicyScenarioTwo -IsTenantDefault $true -Type TokenLifetimePolicy

&nbsp;&nbsp;5. **Fertig!** 

Sie haben nun die ursprüngliche Richtlinie die Dienstprinzipalnamen und neuer als die Standardrichtlinie Mieter verknüpft.  Es ist wichtig zu bedenken, dass Richtlinien für Service Prinzipale Mieter Standardrichtlinien Vorrang haben. 


## <a name="cmdlet-reference"></a>Cmdlet-Referenz

### <a name="manage-policies"></a>Verwalten von Richtlinien
Die folgenden Cmdlets können zum Verwalten von Richtlinien.</br></br>



#### <a name="new-azureadpolicy"></a>Neue AzureADPolicy
Erstellt eine neue Richtlinie.

    New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsTenantDefault <boolean> -Type <Policy Type> 

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-Definition| Das Array als Zeichenfolgen dargestellt JSON, die alle Regeln der Richtlinie enthält.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-DisplayName|Im Namen der Richtlinie|DisplayName - MyTokenPolicy
-IsTenantDefault|Wenn True die Richtlinie legt keine Mieters Standardrichtlinie falsch|-IsTenantDefault $true
-Typ|Richtlinie für token Lebensdauer immer verwenden "TokenLifetimePolicy"|-TokenLifetimePolicy Typ
-AlternativeIdentifier [Optional]|Legt eine alternative Id der Richtlinie.|AlternativeIdentifier - myAltId
</br></br>

#### <a name="get-azureadpolicy"></a>AzureADPolicy abrufen         
Ruft alle AzureAD Richtlinien oder angegebene Richtlinie 
        
    Get-AzureADPolicy 

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId [Optional]|Die Objekt-Id der Richtlinie, die Sie abrufen möchten. |ObjectId - &lt;ObjectID-Richtlinie&gt; 
</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>AzureADPolicyAppliedObject abrufen         
Ruft alle apps und Prinzipale Service mit Richtlinien verknüpft
        
    Get-AzureADPolicyAppliedObject -ObjectId <object id of policy> 

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Richtlinie, die Sie abrufen möchten.|ObjectId - &lt;ObjectID-Richtlinie&gt;
</br></br>

#### <a name="set-azureadpolicy"></a>AzureADPolicy festlegen
Aktualisiert eine vorhandene Richtlinie
        
    Set-AzureADPolicy -ObjectId <object id of policy> -DisplayName <string> 
 
Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Richtlinie, die Sie abrufen möchten.|ObjectId - &lt;ObjectID-Richtlinie&gt;
-DisplayName|Im Namen der Richtlinie|DisplayName - MyTokenPolicy
-Definition [Optional]|Das Array als Zeichenfolgen dargestellt JSON, die alle Regeln der Richtlinie enthält.|-Definition @("{`"TokenLifetimePolicy`":{`"Version`":1,`"MaxInactiveTime`":`"20:00:00`"}}") 
-IsTenantDefault [Optional]|Wenn True die Richtlinie legt keine Mieters Standardrichtlinie falsch|-IsTenantDefault $true
-Typ [Optional]|Richtlinie für token Lebensdauer immer verwenden "TokenLifetimePolicy"|-TokenLifetimePolicy Typ
-AlternativeIdentifier [Optional]|Legt eine alternative Id der Richtlinie.|AlternativeIdentifier - myAltId
</br></br>

#### <a name="remove-azureadpolicy"></a>AzureADPolicy entfernen         
Löscht die angegebene Richtlinie

     Remove-AzureADPolicy -ObjectId <object id of policy>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Richtlinie, die Sie abrufen möchten.|ObjectId - &lt;ObjectID-Richtlinie&gt;
</br></br>

### <a name="application-policies"></a>Anwendungsrichtlinien
Die folgenden Cmdlets können für Richtlinien verwendet werden.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Hinzufügen AzureADApplicationPolicy         
Die angegebene Richtlinie verknüpft mit einer Anwendung

    Add-AzureADApplicationPolicy -ObjectId <object id of application> -RefObjectId <object id of policy>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID Anwendung&gt; 
-RefObjectId|Die Objekt-Id der Richtlinie. |RefObjectId - &lt;ObjectID-Richtlinie&gt;
</br></br>

#### <a name="get-azureadapplicationpolicy"></a>AzureADApplicationPolicy abrufen        
Ruft eine Anwendung zugewiesene Richtlinie

    Get-AzureADApplicationPolicy -ObjectId <object id of application>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID Anwendung&gt; 
</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>AzureADApplicationPolicy entfernen        
Eine Richtlinie entfernt aus einer Anwendung

    Remove-AzureADApplicationPolicy -ObjectId <object id of application> -PolicyId <object id of policy>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID Anwendung&gt; 
-PolicyId| Die ObjectId-Richtlinie.|PolicyId - &lt;ObjectID-Richtlinie&gt;
</br></br>

### <a name="service-principal-policies"></a>Richtlinien für Hauptbenutzer
Die folgenden Cmdlets kann für die wichtigsten Richtlinien verwendet werden.</br></br>

#### <a name="add-azureadserviceprincipalpolicy"></a>Hinzufügen AzureADServicePrincipalPolicy         
Verknüpft die angegebene Richtlinie auf einen Dienstprinzipal zu

    Add-AzureADServicePrincipalPolicy -ObjectId <object id of service principal> -RefObjectId <object id of policy>

Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID Anwendung&gt; 
-RefObjectId|Die Objekt-Id der Richtlinie. |RefObjectId - &lt;ObjectID-Richtlinie&gt;
</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>AzureADServicePrincipalPolicy abrufen        
Ruft alle angegebenen Dienstprinzipalnamen verknüpfte Richtlinie

    Get-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>
 
Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID Anwendung&gt; 
</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>AzureADServicePrincipalPolicy entfernen         
Die Richtlinie entfernt angegebenen Dienstprinzipal

    Remove-AzureADServicePrincipalPolicy -ObjectId <object id of service principal>  -PolicyId <object id of policy>
 
Parameter|Beschreibung|Beispiel|
-----| ----- |-----|
-ObjectId|Die Objekt-Id der Anwendung.|ObjectId - &lt;ObjectID Anwendung&gt; 
-PolicyId| Die ObjectId-Richtlinie.|PolicyId - &lt;ObjectID-Richtlinie&gt;

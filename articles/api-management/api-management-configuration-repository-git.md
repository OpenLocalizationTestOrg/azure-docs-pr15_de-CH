<properties 
    pageTitle="Das Speichern und Konfiguration der API Management Service mit Git" 
    description="Informationen Sie zum Speichern und Konfiguration der API Management Service mit Git." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Das Speichern und Konfiguration der API Management Service mit Git

>[AZURE.IMPORTANT] Git ist für API Management derzeit in der Vorschau. Funktion abgeschlossen ist, aber in der Vorschau ist da wir Feedback dieses Feature aktiv sind. Es ist möglich, dass wir möglicherweise eine wichtige Reaktion auf Feedback von Kunden ändern, abhängig von der Funktion für die Verwendung in der Produktion nicht empfohlen. Wenn Sie Feedback oder Fragen haben, Bitte teilen Sie uns unter `apimgmt@microsoft.com`.

Jede API Management Service Instanz verwaltet eine Datenbank, die Informationen zur Konfiguration und Metadaten für die Instanz enthält. Die Dienstinstanz können geändert werden durch Ändern einer Einstellung im Publisher-Portal, mithilfe von PowerShell-Cmdlets oder REST-API aufrufen. Zusätzlich können Sie die Konfiguration der Webdienstinstanz Git verwenden, Szenarien Service Management wie auch verwalten:

-   Konfiguration Versioning - herunterladen und speichern verschiedene Versionen Ihrer Service-Konfiguration
-   Konfiguration BULK - mehrere Teile der Konfiguration im lokalen Repository ändern und Änderungen an den Server mit einem einzigen Vorgang integrieren
-   Bekannte Git Toolchain und Workflow - verwenden Git Werkzeuge und Workflows, denen Sie bereits kennen

Das folgende Diagramm zeigt eine Übersicht über die verschiedenen Methoden zum Konfigurieren der API Management Service-Instanz.

![Git konfigurieren][api-management-git-configure]

Wenn Sie den Dienst mit Publisher-Portal, PowerShell-Cmdlets oder die REST-API ändern, Sie verwalten den Service Configuration Datenbank mit der `https://{name}.management.azure-api.net` Endpunkt auf der rechten Seite des Diagramms angezeigt. Das Diagramm links zeigt Verwaltung der Dienstkonfiguration mit Git und Git Repository für den Dienst in `https://{name}.scm.azure-api.net`.

Die folgenden Schritte bieten einen Überblick über die Verwaltung der API Management-Dienstinstanz mit Git.

1.  Git auf Ihren Dienst aktivieren
2.  Speichern Sie die Konfigurationsdatenbank Service in der Git repository
3.  Git Repo auf den lokalen Computer kopieren
4.  Ziehen Sie das neueste Repo auf Ihrem lokalen Computer sowie Commit und Push zu Ihrem Repository
5.  Bereitstellen Sie die Unterschiede zwischen der Repo die Konfigurationsdatenbank service

Dieser Artikel beschreibt das Aktivieren und Git um die Dienstkonfiguration zu verwalten und dient als Referenz für die Dateien und Ordner im Repository Git.

## <a name="to-enable-git-access"></a>Git Zugriff aktivieren

Sie können den Status der Git Konfiguration schnell Git-Symbol in der oberen rechten Ecke des Portals Publisher anzeigen anzeigen. In diesem Beispiel hat Git Access noch nicht aktiviert wurde.

![Git status][api-management-git-icon-enable]

Anzeigen und konfigurieren Sie Ihre Git Konfiguration können Sie Git klicken, oder Sie klicken im Menü **Sicherheit** und navigieren Sie zur Registerkarte **Konfigurationsrepository** .

![GIT aktivieren][api-management-enable-git]

Git Zugriff das Kontrollkästchen Sie **Zugriff Git aktivieren** .

Kurz die Änderung gespeichert und eine Meldung angezeigt. Beachten Sie, dass Git Symbol Farbe Git Zugriff aktiviert und der Statusnachricht jetzt deutet, es ändert das Repository ungespeicherte geändert hat. Ist die API Management Service-Konfigurationsdatenbank noch nicht im Repository gespeichert wurde.

![Git aktiviert][api-management-git-enabled]

>[AZURE.IMPORTANT] Vertrauliche Daten, die nicht definierte Eigenschaften im Repository gespeichert und bleibt im Verlauf bis Sie deaktivieren und reaktivieren Git Zugriff. Eigenschaften ermöglichen das sichere verwalten Konstantenzeichenfolge Werten, geheime Daten API-Konfiguration und-Richtlinien, so Sie diese direkt in Ihre Richtlinien zu speichern müssen. Weitere Informationen finden Sie unter [Eigenschaften in Azure API Management-Richtlinien verwenden](api-management-howto-properties.md).

Informationen zum Aktivieren oder Deaktivieren der Git Zugriff über die REST-API finden Sie unter [Aktivieren oder deaktivieren Git die REST-API verwenden](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Die Konfiguration der Git Repository speichern

Der erste Schritt vor dem Klonen Repository ist der derzeitigen Konfiguration des Dienstes im Repository speichern. Klicken Sie auf **Speichern in Repository-Konfiguration**.

![Konfiguration speichern][api-management-save-configuration]

Gewünschte Änderungen an der Bestätigungsdialog angezeigt, und klicken Sie auf **Ok** , speichern.

![Konfiguration speichern][api-management-save-configuration-confirm]

Nach einigen Augenblicken wird die Konfiguration und der Konfigurationsstatus des Repository angezeigt, einschließlich Datum und Uhrzeit der letzten Änderung der Konfiguration und der letzten Synchronisation zwischen der Konfiguration und dem Repository.

![Konfigurationsstatus][api-management-configuration-status]

Nachdem die Konfiguration im Repository gespeichert wurde, kann es geklont.

Informationen zu diesem Vorgang mithilfe der REST-API finden Sie unter [Commit der Snapshot mit der REST-API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Das Repository auf Ihren lokalen Computer klonen

Klonen ein Repository benötigen Sie den URL auf das Repository, einen Benutzernamen und ein Kennwort. Den Namen und die URL werden im oberen Bereich der Registerkarte **Konfigurationsrepository** angezeigt.

![Git clone][api-management-configuration-git-clone]

Das Kennwort wird am unteren Rand der Registerkarte **Konfigurationsrepository** generiert.

![Kennwort erstellen][api-management-generate-password]

Um ein Kennwort zu generieren, zunächst sicherstellen Sie, dass der **Ablauf** zum gewünschten Datum und Uhrzeit festgelegt wird, und dann auf **Token zu generieren**.

![Kennwort][api-management-password]

>[AZURE.IMPORTANT] Notieren Sie sich dieses Kennwort. Verlassen dieser Seite wird das Kennwort nicht erneut angezeigt.

Die folgenden Beispiele verwenden das Tool Git Bash [Git für Windows](http://www.git-scm.com/downloads) können alle Git-Tool, das Sie kennen.

Öffnen Sie das Tool Git in den gewünschten Ordner und führen Sie den folgenden Befehl zum Klonen Git Repository auf dem lokalen Computer mit dem Befehl von Publisher-Portal bereitgestellt.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Geben Sie Benutzernamen und Kennwort ein.

Wenn Sie Fehlermeldungen erhalten, versuchen Sie die `git clone` Befehl einzuschließende Benutzernamen und Kennwort, wie im folgenden Beispiel gezeigt.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Sollten Sie dadurch Fehler URL-Codierung Kennwortteil des Befehls. Eine schnelle Möglichkeit hierzu ist, öffnen Sie Visual Studio, und führen Sie folgenden Befehl in das **Direktfenster**. Um das **Direktfenster**zu öffnen, öffnen Sie eine Projektmappe oder ein Projekt in Visual Studio (oder erstellen Sie eine neue leere) und **Windows** **direkt** im Menü **Debuggen** auswählen.

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Verwenden Sie das verschlüsselte Kennwort mit Ihrem Namen und Repository Speicherort erstellen Befehl Git.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

Nachdem das Repository dupliziert können Sie anzeigen und arbeiten in Ihrem lokalen Dateisystem. Weitere Informationen finden Sie unter [Datei- und Struktur Referenz Git-Repository](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Das lokale Repository mit der aktuellen Instanz Dienstkonfiguration aktualisieren

Wenn Sie die API Management-Dienstinstanz in Publisher-Portal oder über die REST-API ändern, müssen Sie vor der Aktualisierung des lokalen Repository mit den neuesten geändert im Repository speichern. Dazu klicken Sie auf die Registerkarte **Konfigurationsrepository** im Publisher-Portal **Konfiguration Repository speichern** , und führen Sie folgenden Befehl im lokalen Repository.

    git pull

Vor dem Ausführen `git pull` sich im Ordner für das lokale Repository. Wenn Sie gerade abgeschlossen haben die `git clone` Befehl, Sie mit einem Befehl wie dem folgenden Verzeichnis zu Ihrem Repository ändern müssen.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Ändern von lokalen Repo an Repo Server übertragen

Zum Ändern von Ihrem lokalen Repository zum Server-Repository müssen Sie Ihre Änderungen und drücken zum Server-Repository. Um Ihre Änderungen das Git Befehlstool öffnen, wechseln Sie in das Verzeichnis des lokalen Repository und folgende Befehle.

    git add --all
    git commit -m "Description of your changes"

Führen Sie den folgenden Befehl, um alle Commits auf den Server verschieben.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Änderungen Configuration Service API Management-Dienstinstanz bereitstellen

Sobald die lokalen Änderungen übergeben und zum Server-Repository abgelegt, können Sie sie Ihrer API Management Service-Instanz bereitstellen.

![Bereitstellen][api-management-configuration-deploy]

Informationen zu diesem Vorgang mithilfe der REST-API finden Sie unter [Bereitstellen Git Änderungen Konfigurationsdatenbank mithilfe der REST-API](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Dateien und Ordner-Struktur Verweis Git-repository

Dateien und Ordner im lokalen Git Repository enthalten die Informationen zur Service-Instanz.

| Artikel                       | Beschreibung                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| Stammordner api-management | Enthält die Konfiguration für die Dienstinstanz auf oberster Ebene                                  |
| APIs Ordner                | Enthält die Konfiguration für die Dienstinstanz-apis                            |
| Ordner und Gruppen              | Enthält die Konfiguration für die Gruppen der Dienstinstanz                          |
| Richtlinienordner            | Enthält die Richtlinien in der Dienstinstanz                                              |
| PortalStyles Ordner        | Enthält die Konfiguration für die Developer Portal Anpassungen in der Dienstinstanz |
| Produkte-Ordner            | Enthält die Konfiguration der Produkte der Dienstinstanz                        |
| Ordner           | Enthält die Konfiguration für die e-Mail-Vorlagen in der Dienstinstanz                 |

Jeder Ordner darf eine oder mehrere Dateien und in einigen Fällen einen oder mehrere Ordner, beispielsweise eines Ordners für jede API, Produkt oder Gruppe. Die Dateien in jedem Ordner sind spezifisch für den Entitätstyp der Ordnername beschrieben.

| Dateityp | Zweck                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Informationen zur jeweiligen Person                  |
| HTML      | Eine Beschreibung der Einheit, häufig in Entwicklerportal angezeigt |
| XML       | Richtlinien                                                      |
| CSS       | Stylesheets für Developer Portal anpassen                        |

Diese Dateien können erstellt, gelöscht, bearbeitet, und auf Ihrem lokalen Dateisystem verwaltet und bereitgestellt ändert zurück die die Dienstinstanz API Management.

>[AZURE.NOTE] Folgende Elemente sind in der Git Repository nicht enthalten und können mit Git konfiguriert werden.
>
>-    Benutzer
>-    Abonnements
>-    Eigenschaften
>-    Developer Portal Entitäten als Stile

### <a name="root-api-management-folder"></a>Stammordner api-management

Stamm `api-management` Ordner enthält eine `configuration.json` Datei, die Informationen über die Dienstinstanz in folgendem Format auf oberster Ebene enthält.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

Die ersten vier Optionen (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, und `UserRegistrationTermsConsentRequired`) Folgendes auf der Registerkarte **Identitäten** im Bereich **Sicherheit** zugeordnet.

| Identität                     | Wird zugeordnet                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | Kontrollkästchen **anonyme Benutzern Anmeldeseite umgeleitet** |
| UserRegistrationTerms                | **Rechtliche Hinweise auf Benutzer registrieren** textbox               |
| UserRegistrationTermsEnabled         | Kontrollkästchen **auf der Anmeldeseite anzeigen**         |
| UserRegistrationTermsConsentRequired | Kontrollkästchen **Zustimmung**                          |

![Identität settings][api-management-identity-settings]

Die nächsten vier Settings (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, und `DelegationValidationKey`) Folgendes auf der Registerkarte **Delegierung** im Bereich **Sicherheit** zugeordnet.

| Delegierung festlegen           | Wird zugeordnet                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | Kontrollkästchen **Delegaten anmelden und Anmeldung**    |
| DelegationUrl                | **Delegierung Endpunkt-URL** -Textfeld        |
| DelegatedSubscriptionEnabled | **Delegaten Abonnement** Kontrollkästchen |
| DelegationValidationKey      | **Delegaten Validierungsschlüssel** textbox        |

![Delegierung settings][api-management-delegation-settings]

Die endgültige Einstellung `$ref-policy`, globale Anweisungen Richtliniendatei für die Dienstinstanz zugeordnet.

### <a name="apis-folder"></a>APIs Ordner

Die `apis` Ordner enthält einen Ordner für jede API Dienstinstanz enthält die folgenden Elemente.

-   `apis\<api name>\configuration.json`-Dies ist die Konfiguration für die API und enthält Informationen über die Back-End-URL und die Vorgänge. Dies ist die gleichen Informationen, die zurückgegeben würde, wären Sie mit [Get eine bestimmte API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) Aufrufen `export=true` in `application/json` Format.
-   `apis\<api name>\api.description.html`-Dies ist die Beschreibung der API und entspricht der `description` -Eigenschaft der [API-Entität](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
-   `apis\<api name>\operations\`-Ordner enthält `<operation name>.description.html` Dateien, die die Vorgänge in der API zugeordnet. Jede Datei enthält den Arbeitsgang in der API der zugeordnet ist die `description` -Eigenschaft der [Operation Entität](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in die REST-API.

### <a name="groups-folder"></a>Ordner und Gruppen

Die `groups` Ordner enthält einen Ordner für jede Gruppe gemäß der Dienstinstanz.

-   `groups\<group name>\configuration.json`-Dies ist die Konfiguration für die Gruppe. Dies ist die gleichen Informationen, die zurückgegeben wird, würden Sie die [einer bestimmten Gruppe](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) aufrufen.
-   `groups\<group name>\description.html`-Dies ist die Beschreibung der Gruppe und entspricht der `description` -Eigenschaft der [Gruppe](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>Richtlinienordner

Die `policies` Ordner Aussagen der Richtlinie für die Dienstinstanz.

-   `policies\global.xml`-enthält Richtlinien, die im globalen Gültigkeitsbereich für die Dienstinstanz definiert.
-   `policies\apis\<api name>\`-Wenn Sie Richtlinien auf API-Ebene definiert haben, werden sie in diesem Ordner enthaltenen.
-   `policies\apis\<api name>\<operation name>\`Ordner - haben alle Richtlinien im Gültigkeitsbereich der Operation definiert sie in diesem Ordner enthaltenen `<operation name>.xml` Dateien, die auf die Richtlinie für jeden Vorgang zuordnen.
-   `policies\products\`-haben alle Richtlinien im Produktumfang definiert sie in diesem Ordner enthaltenen `<product name>.xml` Dateien, die die richtlinienanweisungen für jedes Produkt zuordnen.

### <a name="portalstyles-folder"></a>PortalStyles Ordner

Die `portalStyles` Ordner Konfiguration und Stylesheets für Developer Portal Anpassungen für die Dienstinstanz.

-   `portalStyles\configuration.json`-enthält die Namen der Formatvorlagen Entwicklerportal verwendet
-   `portalStyles\<style name>.css`-Jede `<style name>.css` Datei enthält Formatvorlagen für Developer-Portal (`Preview.css` und `Production.css` standardmäßig).

### <a name="products-folder"></a>Produkte-Ordner

Die `products` Ordner enthält einen Ordner für jedes Produkt in die Dienstinstanz definiert.

-   `products\<product name>\configuration.json`-Dies ist die Konfiguration für das Produkt. Dies ist die gleichen Informationen, die zurückgegeben wird, wenn [ein bestimmtes Produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) Vorgang aufgerufen wurden.
-   `products\<product name>\product.description.html`-Dies ist die Beschreibung des Produkts und entspricht der `description` -Eigenschaft der [Produktentität](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in die REST-API.

### <a name="templates"></a>Vorlagen

Die `templates` Ordner Konfiguration für [e-Mail-Vorlagen](api-management-howto-configure-notifications.md) der Dienstinstanz.

-   `<template name>\configuration.json`-Dies ist die Konfiguration für die e-Mail-Vorlage.
-   `<template name>\body.html`-Dies ist der Textkörper der e-Mail-Vorlage.

## <a name="next-steps"></a>Nächste Schritte

Informationen über weitere Methoden zum Verwalten der Dienstinstanz finden Sie unter:

-   Verwalten Sie die Dienstinstanz mit dem folgenden PowerShell-cmdlets
    -   [Bereitstellung PowerShell-Cmdlet Verweis](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Servicemanagement PowerShell-Cmdlet Verweis](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Die Dienstinstanz in der Publisher-Portal verwalten
    -   [Die erste API verwalten](api-management-get-started.md)
-   Verwalten der Dienstinstanz über die REST-API
    -   [API-Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Einen video ansehen

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png





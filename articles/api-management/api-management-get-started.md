<properties
    pageTitle="Die erste API in Azure API Management verwalten | Microsoft Azure"
    description="Erfahren Sie, wie APIs erstellen, Vorgänge hinzufügen und mit API beginnen."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Die erste API in Azure API Management verwalten

## <a name="overview"> </a>(Übersicht)

Dieses Handbuch zeigt, wie schnell mit Azure API Management und Ihre erste API-Aufruf.

## <a name="concepts"> </a>Was Azure API Management ist?

Azure API Management können alle Back-End-und eine vollständige API-Programm basieren.

Allgemeine Beispiele:

* **Sichern von mobilen Infrastruktur** gating Zugriff mit API-Schlüssel DOS verhindern Angriffe Drosselung oder erweiterte Sicherheit Richtlinien wie JWT-token-Prüfung.
* **Aktivieren der ISV-Partner-Ökosysteme** schnell Partner Onboarding Developer Portal und Erstellen einer API Fassade interne Implementierung entziehen, die nicht für Partner Verbrauch.
* **Eine interne API-Programm** bietet einen zentralen Ort für die Organisation über die Verfügbarkeit und Neuheiten APIs Zugriff auf Grundlage der Organisationseinheit Accounts gating Grundlage alle gesicherten Kanal zwischen der API-Gateway und der Back-End.


Das System besteht aus folgenden Komponenten:

* Das **API-Gateway** ist der Endpunkt, der:
  * API-Aufrufe und leitet sie an die Downloadzeit akzeptiert.
  * API-Schlüssel JWT Token, Zertifikate und anderer Anmeldeinformationen überprüft.
  * Erzwingt die verwendungskontingente und Limits.
  * Transformiert die API ohne Code ändern.
  * Wo Backend-Antworten zwischengespeichert.
  * Protokolle werden Metadaten zu Analysezwecken aufrufen.

* **Publisher-Portal** bildet die Verwaltungsschnittstelle, Ihre API-Anwendung einrichten. Verwendungszweck:
    * Definieren oder API-Schema importieren.
    * Paket APIs Produkte.
    * Richten Sie wie Quoten und Transformationen auf die APIs.
    * Einblicke aus Analysen.
    * Verwalten von Benutzern.

* **Entwicklerportal** dient wie Haupt-Entwickler können sie:
    * Lese-API-Dokumentation.
    * Probieren Sie eine API über interaktive Konsole.
    * Erstellen Sie ein Konto und API-Schlüssel zu abonnieren.
    * Analytics Zugriff auf eigene Verwendung.


## <a name="create-service-instance"> </a>Eine API Management-Instanz erstellen

>[AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Konto in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion][].

Der erste Schritt beim Arbeiten mit API-Management ist eine Instanz erstellen. Der [Azure-Verwaltungsportal][] und auf **neue** **App Services** **API Management**, **Erstellen**.

![Neue Instanz-API-Management][api-management-create-instance-menu]

Geben Sie für **URL**eindeutiger Ebene einen Domänennamen für die Dienst-URL verwenden an.

Wählen Sie das gewünschte **Abonnement** und die **Region** für die Dienstinstanz. Klicken Sie nach der Auswahl auf die Schaltfläche **Weiter** .

![Neue API-Verwaltungsdienst][api-management-create-instance-step1]

Geben Sie **Contoso Ltd.** **Name der Organisation**, und geben Sie Ihre e-Mail-Adresse im Feld **E-Mail-Administrator** .

>[AZURE.NOTE] Diese e-Mail-Adresse wird für Benachrichtigungen aus der System-API verwendet. Weitere Informationen finden Sie unter [Benachrichtigung und e-Mail-Vorlagen in Azure API Management konfigurieren][].

![Neue API-Verwaltungsdienst][api-management-create-instance-step2]

Dienstinstanzen API Management stehen in drei Stufen: Developer, Standard und Premium. Standardmäßig werden neue API Management Dienstinstanzen in der Developer erstellt. Wählen Standard oder Premium-Ebene und **Erweitert** das Kontrollkästchen die gewünschte Ebene auf dem folgenden Bildschirm auswählen.

>[AZURE.NOTE] Entwickler-Ebene ist für die Entwicklung, Erprobung und API Pilotprogramme hoher Verfügbarkeit kein Belang ist. Im Standard- und Premium-Ebenen können Sie die reservierte Anzahl mehr Datenverkehr behandeln skalieren. Standard- und Premium-Ebenen bieten Ihnen API Management mit die Leistung und der Leistung. Abschluss des Lernprogramms kann mit einer beliebigen Ebene. Weitere Informationen zu API Management Ebenen finden Sie unter [API Management Preise][].

Klicken Sie auf das Kontrollkästchen, um die Dienstinstanz erstellen.

![Neue API-Verwaltungsdienst][api-management-instance-created]

Nachdem die Instanz erstellt wurde, besteht der nächste Schritt zum Erstellen oder Importieren einer API.

## <a name="create-api"> </a>API importieren

Eine API besteht aus einer Reihe von Operationen, die von einer Clientanwendung aufgerufen werden kann. API-Vorgänge sind als vorhandene Webdienste.

APIs erstellt werden (und Operationen hinzugefügt werden können) manuell oder importiert werden. In diesem Lernprogramm werden die API für ein Beispiel Rechner Web Service von Microsoft bereitgestellt und auf Azure importiert.

>[AZURE.NOTE] Erstellen einer API und Vorgänge manuell hinzufügen Siehe [APIs erstellen](api-management-howto-create-apis.md) und [Hinzufügen zu einer API](api-management-howto-add-operations.md).

APIs werden von Publisher-Portal konfiguriert der Azure-Verwaltungsportal zugegriffen wird. Klicken Sie auf **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst zu Publisher-Portal.

![Publisher-portal][api-management-management-console]

Zum Importieren des Rechners API klicken Sie **APIs** der **API** Menü auf der linken Seite und dann auf **API importieren**.

![API importieren][api-management-import-api]

Die folgenden Schritte Rechner API konfigurieren:

1. **Von URL**auf, geben Sie **http://calcapi.cloudapp.net/calcapi.json** in das Textfeld **Spezifikation URLs** und klicken Sie auf die Schaltfläche **Swagger** .
2. Geben Sie im Textfeld **API-Web-URL-Suffix** **Calc** .
3. Klicken Sie im Feld **Produkte (optional)** , und wählen Sie **Starter**.
4. Klicken Sie auf **Speichern** , um die API importieren.

![Neues API hinzufügen][api-management-import-new-api]

>[AZURE.NOTE] **API-Management** unterstützt derzeit Version 1.2 und 2.0 stolz Dokument für den Import. Stellen Sie sicher, dass, obwohl [Swagger 2.0-Spezifikation](http://swagger.io/specification) erklärt `host`, `basePath`, und `schemes` Eigenschaften optional sind, **müssen** Swagger 2.0 Dokument enthalten die Eigenschaften; Andernfalls wird nicht es importiert. 

Nach dem Importieren der API wird die Zusammenfassungsseite für die API im Publisher-Portal angezeigt.

![API-Zusammenfassung][api-management-imported-api-summary]

API-Abschnitt enthält mehrere Registerkarten. Die Registerkarte **Zusammenfassung** zeigt grundlegende Metriken und Informationen über die API. [Die Registerkarte](api-management-howto-create-apis.md#configure-api-settings) zum Anzeigen und Bearbeiten der Konfiguration einer API. Registerkarte " [Vorgänge](api-management-howto-add-operations.md) " wird verwendet, um die API verwalten. Die Registerkarte **Sicherheit** kann Gateway-Authentifizierung für den Back-End-Server mit Standardauthentifizierung oder [Zertifikatauthentifizierung](api-management-howto-mutual-certificates.md)konfigurieren und [benutzerautorisierung mit OAuth 2.0](api-management-howto-oauth2.md)konfigurieren verwendet werden.  Registerkarte **Probleme** zur Anzeige Probleme Entwickler APIs verwenden. Registerkarte **Produkte** dient zum Konfigurieren der Produkte, die diese API enthalten.

Jede API Management-Instanz steht zwei Beispiel-Produkte:

-   **Starter**
-   **Unbegrenzt**

In diesem Lernprogramm wurde der grundlegenden API Rechner beim Import der API Starter Produkt hinzugefügt.

Um eine API aufrufen, müssen Entwickler zunächst ein Produkt abonnieren, die sie auf sie zugreifen können. Entwickler können Produkte Entwicklerportal abonnieren oder Administratoren können Entwickler Erzeugnisse in der Publisher-Portal anmelden. Sie sind Administrator-API Management-Instanz in den vorherigen Schritten im Lernprogramm erstellt werden, damit Sie bereits standardmäßig jedes Produkt abonniert haben.

## <a name="call-operation"> </a>Eine Operation im Entwicklerportal aufrufen

Vorgänge können direkt aus dem Entwicklerportal bietet eine bequeme Möglichkeit zu testen die Vorgänge einer API aufgerufen werden. In diesem Lernprogramm Schritt rufen Sie grundlegende Rechner API **Hinzufügen zwei Ganzzahlen** Vorgang. Klicken Sie im Menü oben **Entwicklerportal** rechts von Publisher-Portal.

![Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im oberen Menü auf **APIs** , und klicken Sie auf **Rechner** verfügbaren Operationen finden Sie unter.

![Entwicklerportal][api-management-developer-portal-calc-api]

Hinweis Beispiel Beschreibung und Parameter, die importiert wurden, und der API-Operationen Dokumentation für Entwickler, die diesen Vorgang verwenden. Diese Beschreibung können auch hinzugefügt werden, wenn Vorgänge manuell hinzugefügt werden.

Klicken Sie zum Aufrufen der Operation **zwei Ganzzahlen hinzufügen** auf **ausprobieren**.

![Versuch es][api-management-developer-portal-calc-api-console]

Sie können einige Werte für die Parameter eingeben oder übernehmen Sie die Standardeinstellungen und klicken Sie dann auf **Senden**.

![HTTP Get][api-management-invoke-get]

Nachdem ein Vorgang aufgerufen wird, zeigt Entwicklerportal **Antwortstatus**, **Antwortheader**und **Antwortinhalt**.

![Antwort][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Analyse anzeigen

Um Analytics Rechner anzuzeigen, wechseln Sie zu Publisher-Portal **Verwalten** auswählen im Menü oben rechts vom Entwicklerportal.

![Verwalten][api-management-manage-menu]

Die Standardansicht für das Publisher-Portal ist **Dashboard**bietet eine Übersicht über die API Management-Instanz.

![Dashboard][api-management-dashboard]

Mauszeiger über das Diagramm **Rechner** bestimmten Kriterien für die Verwendung der API für einen bestimmten Zeitraum anzeigen.

>[AZURE.NOTE] Wenn Positionen im Diagramm nicht angezeigt wird, wechseln Sie zurück zum Entwicklerportal einige Aufrufe in die API, warten und dann zum Dashboard zurück.

Klicken Sie auf **Details anzeigen** , um die Zusammenfassungsseite für die API, einschließlich einer größeren Version angezeigten Metriken anzuzeigen.

![Analytics][api-management-mouse-over]

![Zusammenfassung][api-management-api-summary-metrics]

Klicken Sie für detaillierte Metriken und Berichte auf **Analytics** **API Management** -Menü auf der linken Seite.

![Übersicht][api-management-analytics-overview]

**Analytics** -Abschnitt enthält die folgenden vier Registerkarten:

-   **Auf einen Blick** bietet allgemeine Verwendung und Gesundheit Metriken sowie die besten Entwickler, Spitzenprodukte Top APIs und obersten Operationen.
-   **Verwendung** bietet Detail API-Aufrufe und Bandbreite, einschließlich der geografischen Verteilung.
-   **Health** Status Codes Erfolgsraten Reaktionszeiten und API-cache und service-Reaktionszeit.
-   **Aktivität** bietet Drilldown Berichten zu bestimmten Aktivitäten durch Entwickler, Produkt, API und Betrieb.

## <a name="next-steps"> </a>Nächste Schritte

- Erfahren Sie, wie Sie [schützen Ihre API mit Rate](api-management-howto-product-with-rules.md).

[Azure-Testversion]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Benachrichtigung und e-Mail-Vorlagen in Azure API Management konfigurieren]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[API Management Preise]: http://azure.microsoft.com/pricing/details/api-management/

[Azure-Verwaltungsportal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png

<properties
    pageTitle="Zwischenspeichern zur Leistungssteigerung in Azure API Management hinzufügen | Microsoft Azure"
    description="Informationen Sie zum Latenz, Bandbreite und Auslastung Servicebesuche API Management zu verbessern."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Fügen Sie Zwischenspeichern zur Leistungssteigerung in Azure API Management hinzu

Antwort zwischenspeichern können Operationen in API Management konfiguriert werden. Antwort zwischenspeichern kann erheblich API Latenz, Bandbreite, und web Auslastung für Daten, die nicht häufig geändert werden.

Dieses Handbuch veranschaulicht Antwort Zwischenspeichern für Ihre API und Richtlinien für Operationen Echo API für. Sie können den Vorgang dann Entwicklerportal überprüfen Zwischenspeichern in Aktion aufrufen.

>[AZURE.NOTE] Informationen Zwischenspeichern Elemente mit Richtlinienausdrücken finden Sie unter [benutzerdefinierte Zwischenspeichern in Azure API Management](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Vor dem Durchführen der Schritte in diesem Handbuch muss eine Dienstinstanz API Management mit einer API und ein Produkt konfiguriert werden. Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

## <a name="configure-caching"> </a>Eine Operation zum Zwischenspeichern konfigurieren

In diesem Schritt überprüfen Sie Zwischenspeichern Einstellungen des Vorgangs **Erhalten Ressource (zwischengespeichert)** der Probe Echo-API.

>[AZURE.NOTE] Jede API Management-Dienstinstanz vorkonfiguriert ein Echo-API, das experimentieren und API-Management verwendet werden kann. Weitere Informationen finden Sie unter [Erste Schritte mit Azure API Management][].

Klicken Sie zunächst auf **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst. Dadurch gelangen Sie zu Publisher API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

Klicken Sie auf **APIs** der **API-** Menü auf der linken Seite und dann auf **Echo-API**.

![Echo-API][api-management-echo-api]

Klicken Sie auf die Registerkarte **Vorgänge** und klicken Sie dann auf **Ressource abrufen (zwischengespeichert)** Vorgang **Vorgänge** aus.

![Echo API-Vorgänge][api-management-echo-api-operations]

Klicken Sie auf der Registerkarte **Caching** Zwischenspeichern Einstellungen für diesen Vorgang anzeigen.

![Caching-Registerkarte][api-management-caching-tab]

Zum Zwischenspeichern einer Operation, aktivieren Sie das Kontrollkästchen **Aktivieren** . In diesem Beispiel ist das Zwischenspeichern aktiviert.

Jeder Vorgangsantwort wird basierend auf den Werten in den Feldern **Unterscheidung nach Abfragezeichenfolgeparametern** und **verschieden Header** eingegeben. Wenn Abfragezeichenfolgen-Parameter oder Header anhand mehrerer Antworten zwischengespeichert werden soll, können Sie sie in diese beiden Felder konfigurieren.

**Dauer** gibt das Ablaufintervall zwischengespeicherten Antworten. In diesem Beispiel ist das Intervall **3600** Sekunden einer Stunde entspricht.

Mit caching Konfiguration in diesem Beispiel gibt die erste Anforderung der Operation **GET Resource (zwischengespeichert)** eine Antwort vom Back-End-Dienst. Diese Antwort wird vom angegebenen Header und Abfragezeichenfolgen-Parameter eingegeben zwischengespeichert. Nachfolgende Aufrufe Vorgang mit entsprechenden Parametern sind die zwischengespeicherte Antwort zurückgegeben, bis das Cache Dauer Intervall abgelaufen ist.

## <a name="caching-policies"> </a>Zwischenspeichern Richtlinien

In diesem Schritt überprüfen Sie Zwischenspeichern Einstellungen für den Arbeitsgang **Ressource abrufen (zwischengespeichert)** der Probe Echo-API.

Wenn für einen Vorgang auf der Registerkarte **Caching** Zwischenspeichern Einstellungen konfiguriert sind, werden Cacherichtlinien für den Vorgang hinzugefügt. Diese Richtlinien können angezeigt und im Systemrichtlinien-Editor bearbeitet werden.

Klicken Sie auf **Richtlinien** aus der **API-** Menü auf der linken Seite und wählen Sie dann **Echo-API / GET Ressource (zwischengespeichert)** aus Dropdown- **Vorgang** .

![Bereich Richtlinienvorgangs][api-management-operation-dropdown]

Richtlinien für diesen Vorgang im Systemrichtlinien-Editor angezeigt.

![API Management Systemrichtlinien-editor][api-management-policy-editor]

Policy-Definition für diesen Vorgang enthält Richtlinien über die **Caching** -Registerkarte im vorherigen Schritt überprüft, die die Zwischenspeicherungskonfiguration definieren.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Geänderte Cacherichtlinien im Gruppenrichtlinien-Editor werden auf der Registerkarte **Caching** ein und umgekehrt berücksichtigt.

## <a name="test-operation"> </a>Eine Operation und Zwischenspeichern

Um das Zwischenspeichern in Aktion zu sehen, können wir die Operation Entwicklerportal aufrufen. Klicken Sie im Menü oben rechts auf **Entwicklerportal** .

![Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im oberen Menü auf **APIs** , und wählen Sie die **Echo-API**.

![Echo-API][api-management-apis-echo-api]

>Haben Sie nur eine API konfiguriert oder Ihrem Konto sichtbar, gelangen auf APIs direkt in die Operationen für diese API.

Wählen Sie die **Ressource abrufen (zwischengespeichert)** -Operation, und dann auf **Geöffnete Konsole**.

![Konsole öffnen][api-management-open-console]

Die Konsole können Sie Vorgänge direkt aus dem Entwicklerportal aufrufen.

![Konsole][api-management-console]

Die Standardwerte für **param1** und **param2**beibehalten.

**Abonnement-Schlüssel** Dropdown-Liste die gewünschte Taste auswählen. Wenn Ihr Konto nur ein Abonnement verfügt, ist es bereits ausgewählt.

Geben Sie **Sampleheader:value1** im Feld **Anfrage-Header** .

Klicken Sie auf **HTTP Get** , und notieren Sie sich die Antwortheader.

Geben Sie **Sampleheader:value2** im Textfeld **Anfrage-Header** , und klicken Sie dann auf **HTTP Get**.

Beachten Sie, dass der Wert des **Sampleheader** noch **value1** in der Antwort. Versuchen Sie unterschiedliche Werte, und beachten Sie, dass die zwischengespeicherte Antwort vom ersten Aufruf zurückgegeben wird.

Geben Sie im Feld **param2** **25** und dann auf **HTTP Get**.

Beachten Sie, dass der Wert des **Sampleheader** in der Antwort jetzt **value2**. Da die Ergebnisse von Abfragezeichenfolge eingegeben werden, wurde nicht die vorherige zwischengespeicherte Antwort zurückgegeben.

## <a name="next-steps"> </a>Nächste Schritte

-   Weitere Informationen Cacherichtlinien Siehe [API Management Policy Verweis][] [Zwischenspeichern Richtlinien][] .
-   Informationen Zwischenspeichern Elemente mit Richtlinienausdrücken finden Sie unter [benutzerdefinierte Zwischenspeichern in Azure API Management](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure API Management]: api-management-get-started.md

[API Management Policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Zwischenspeichern von Richtlinien]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps

<properties
    pageTitle="Schützen Sie Ihre API mit Azure API Management | Microsoft Azure"
    description="Informationen Sie zum Schützen Ihrer API Quoten und Drosselung (Bandbreitenkontrolle) Richtlinien."
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Schützen Sie Ihre API mit Rate Bereiche Azure API Management

Diese Anleitung erfahren Sie, wie einfach es ist Schutz für Ihre Back-End-API hinzugefügt Rate Limit und ein Kontingent von Richtlinien mit Azure-API.

In diesem Lernprogramm erstellen Sie ein Produkt "Kostenlose Testversion" API, die Entwicklern die API bis zu 10 Aufrufe pro Minute und maximal 200 Anrufe pro Woche zu ermöglicht mit der [Beschränkung Aufruf pro Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) [Satz Verbrauch pro Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) Datenträgerkontingentrichtlinien. Anschließend veröffentlichen die API und testen die Richtlinie Rate.

Erweiterte Drosselung Szenarios [Rate Limit von Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) und [Kontingent Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) Richtlinien finden Sie unter [Erweiterte Anforderung mit Azure API-Drosselung](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Erstellen eines Produkts

In diesem Schritt erstellen Sie eine kostenlose Testversion, die keine Abonnements Genehmigung erforderlich.

>[AZURE.NOTE] Wenn Sie bereits ein Produkt konfiguriert haben, sie für dieses Lernprogramm möchten können wechseln vor Aufruf [Konfigurieren Rate Limit und ein Kontingent von Richtlinien][] und Lernprogramm aus Ihrem Produkt anstelle des Produkts kostenlose Testversion.

Klicken Sie zunächst auf **Verwalten** in Azure Classic für Ihre API-Verwaltungsdienst. Dadurch gelangen Sie zu Publisher API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [verwalten Ihre erste API in Azure API Management][] [Erstellen API Management Service][] .

Klicken Sie links auf die Seite **Produkte** anzeigen im **API Management** auf **Produkte** .

![Produkt hinzufügen][api-management-add-product]

Klicken Sie auf **Produkt hinzufügen** , um das Dialogfeld **Neues Produkt hinzufügen** anzuzeigen.

![Neues Produkt hinzufügen][api-management-new-product-window]

Geben Sie im Feld **Titel** **Kostenlose Testversion**.

Geben Sie im Feld **Beschreibung** den folgenden Text:  **Abonnenten können 10 Aufrufe pro Minute bis maximal 200 Anrufe pro Woche nach der Zugriff verweigert werden.**

Produkte in API Management geschützt oder öffnen. Geschützte Produkte müssen abonniert werden, bevor sie verwendet werden können. Offene Produkte können ohne Abonnement verwendet werden. Sicherstellen Sie, dass **Abonnements erfordern** ein geschütztes Produkt erstellen, das ein Abonnement erfordert, aktiviert ist. Dies ist die Standardeinstellung.

Wählen Sie Administrator überprüfen und akzeptieren oder ablehnen Abonnement dieses Produkt versucht werden soll, **Abonnement genehmigt**. Wenn das Kontrollkästchen nicht aktiviert ist, werden Versuche Abonnement automatisch genehmigt. In diesem Beispiel werden Abonnements automatisch genehmigt, damit das Kontrollkästchen nicht.

Damit entwicklerkonten mehrmals das neue Produkt abonnieren können, aktivieren Sie das Kontrollkästchen **mehrere gleichzeitige Abonnements zulassen** . In diesem Lernprogramm nicht mehrere gleichzeitige Abonnements nutzen, so lassen Sie es deaktiviert.

Nachdem alle Werte eingegeben sind, klicken Sie auf **Speichern** , um das Produkt zu erstellen.

![Produkt][api-management-product-added]

Standardmäßig sind neue Produkte für Benutzer in der Gruppe **Administratoren** sichtbar. Wir werden **die Entwicklergruppe** hinzufügen. Klicken Sie auf **Kostenlose Testversion**, und klicken Sie auf der Registerkarte **Sichtbarkeit** .

>API-Verwaltung werden Gruppen zum Verwalten der Sichtbarkeit von Produkten für Entwickler. Produkte gewähren Sichtbarkeit für Gruppen und Entwickler anzeigen und abonnieren, die Produkte, die sie gehören zu den Gruppen angezeigt werden. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Gruppen in Azure API Management][].

![Entwicklergruppe hinzufügen][api-management-add-developers-group]

**Entwickler** das Kontrollkästchen, und klicken Sie dann auf **Speichern**.

## <a name="add-api"> </a>Eine API zum Produkt hinzufügen

In diesem Schritt des Lernprogramms fügen wir die kostenlose Testversion-API Echo hinzufügen.

>Jede API Management Service-Instanz enthält vorkonfigurierte ein Echo-API, das experimentieren und API-Management verwendet werden kann. Weitere Informationen finden Sie unter [Verwalten der erste API in Azure API Management][].

Klicken Sie auf **Produkte** von der **API-** Menü auf der linken Seite und dann auf **Kostenlose Testversion** des Produkts konfigurieren.

![Produkt konfigurieren][api-management-configure-product]

Klicken Sie auf **Produkt -API hinzugefügt**.

![API Produkt hinzufügen][api-management-add-api]

Wählen Sie **Echo-API aus**und dann auf **Speichern**.

![Echo API hinzufügen][api-management-add-echo-api]

## <a name="policies"> </a>Aufruf Rate Limit und Kontingent Richtlinien konfigurieren

Begrenzung der Datenübertragungsrate und Kontingente sind in der Gruppenrichtlinien-Editor konfiguriert. **Klicken Sie auf Menü **-API-Management** auf der linken Seite.** Klicken Sie in **der Produktliste** auf **Kostenlose Testversion**.

![Produkt-Richtlinien][api-management-product-policy]

Klicken Sie auf **Richtlinie hinzufügen** , um die Vorlage importieren und erstellen die Rate Limit und Datenträgerkontingent-Richtlinien.

![Richtlinie hinzufügen][api-management-add-policy]

Um Richtlinien einzufügen, positionieren Sie den Cursor in der **eingehenden** oder **ausgehenden** Teil der Vorlage. Rate Limit und Kontingent sind eingehende Richtlinien, so setzen Sie den Cursor in das eingehende Element.

![Gruppenrichtlinien-editor][api-management-policy-editor-inbound]

Zwei Richtlinien, die wir in diesem Lernprogramm hinzufügen sind die [Grenze Aufruf pro Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) und [Verbrauch pro Abonnement gesetzt](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Richtlinien][api-management-limit-policies]

Nachdem der Cursor im **eingehenden** Richtlinienelement Pfeil neben **aufruflimit pro Abonnement** die Vorlage einfügen.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Aufruflimit pro Abonnement** auf Produktebene verwendet werden und kann auch auf die API und einzelnen Namen verwendet werden. In diesem Lernprogramm nur Produkt-Richtlinien verwendet werden, löschen Sie die **api** und **Operation** Elemente so aus dem **Grenze** Element nur die äußere **Grenze** Element bleibt wie im folgenden Beispiel gezeigt.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

In die kostenlose Testversion der maximal zulässigen Frequenz 10 Aufrufe pro Minute ist so **10** Geben Sie als Wert für das Attribut **aufrufen** und **60** **Erneuerungszeitraum** Attribut.

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Zum Konfigurieren der **Verwendung pro Abonnement gesetzt** Richtlinie positionieren Sie den Cursor direkt unter der neu hinzugefügten **Ratenlimit** Element in das **eingehende** Element, und klicken Sie auf den Pfeil links vom **Verbrauch pro Abonnement gesetzt**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Da diese Richtlinie auch auf Produktebene sein soll, löschen Sie Elemente Name **api** und **Operation** wie im folgenden Beispiel gezeigt.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Kontingente können die Anzahl der Aufrufe pro Intervall oder Bandbreite basieren. In diesem Lernprogramm werden wir nicht auf Bandbreite Drosselung, löschen Sie das Attribut **Bandbreite** .

    <quota calls="number" renewal-period="seconds">
    </quota>

Kostenlose Testversion ist das Kontingent 200 Anrufe pro Woche. Geben Sie **200** als Wert für das Attribut **aufrufen** , und geben Sie **604800** als Wert für den **Erneuerungszeitraum** Attribut.

    <quota calls="200" renewal-period="604800">
    </quota>

>Richtlinienintervalle werden in Sekunden angegeben. Um das Intervall für eine Woche zu berechnen, Multiplizieren Sie Anzahl der Tage (7) durch die Anzahl der Stunden pro Tag (24) durch die Anzahl der Minuten pro Stunde (60) durch die Anzahl der Sekunden in eine Minute (60): 7 *24* 60 * 60 = 604800.

Abschluss der Konfiguration der Richtlinie sollten sie entsprechend dem folgenden Beispiel.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Nachdem die gewünschten Richtlinien konfiguriert sind, klicken Sie auf **Speichern**.

![Richtlinie speichern][api-management-policy-save]

## <a name="publish-product"></a> , Das Produkt veröffentlichen

Nachdem die APIs hinzugefügt und die Richtlinien konfiguriert sind, das Produkt muss veröffentlicht werden, damit es von Entwicklern verwendet werden kann. Klicken Sie auf **Produkte** von der **API-** Menü auf der linken Seite und dann auf **Kostenlose Testversion** des Produkts konfigurieren.

![Produkt konfigurieren][api-management-configure-product]

Klicken Sie auf **Veröffentlichen**und dann auf **Ja, veröffentlichen** zu bestätigen.

![Produkt veröffentlichen][api-management-publish-product]

## <a name="subscribe-account"> </a>Ein Entwicklerkonto zum Produkt abonnieren

Da das Produkt veröffentlicht wird, steht abonniert und von Entwicklern verwendet werden.

>Administratoren einer API Management-Instanz werden automatisch jedes Produkt abonniert. In diesem Lernprogramm Schritt werden wir eine kostenlose Testversion entwicklerkonten ohne Administratorrechte abonnieren. Wenn Ihr Entwicklerkonto Administratorrolle angehört, können Sie mit diesem Schritt ausführen, obwohl Sie bereits angemeldet sind.

Der **API-** Menü auf der linken Seite auf **Benutzer** und klicken Sie auf den Namen des Entwicklerkontos. In diesem Beispiel verwenden wir das **Clayton Gragg** Entwickler-Konto.

![Entwickler konfigurieren][api-management-configure-developer]

Klicken Sie auf **Abonnements**.

![Abonnement hinzufügen][api-management-add-subscription-menu]

Wählen Sie **Kostenlose Testversion aus**und dann auf **Abonnieren**.

![Abonnement hinzufügen][api-management-add-subscription]

>[AZURE.NOTE] In diesem Lernprogramm werden mehrere gleichzeitige Abonnements für die kostenlose Testversion nicht aktiviert. Wären, würden Sie aufgefordert das Abonnement Name wie im folgenden Beispiel gezeigt.

![Abonnement hinzufügen][api-management-add-subscription-multiple]

Klicken Sie **Abonnieren**, wird das Produkt in der Liste **Abonnement** für den Benutzer angezeigt.

![Abonnement hinzugefügt][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Auf einen Vorgang und das Ratenlimit

Konfiguriert und veröffentlicht die kostenlose Testversion können wir einige Vorgänge und das aufruflimit.
Wechseln Sie zum Entwicklerportal von **Entwicklerportal** im Menü rechts auf.

![Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im oberen Menü auf **APIs** , und dann auf **Echo-API**.

![Entwicklerportal][api-management-developer-portal-api-menu]

Klicken Sie auf **Ressource erhalten**und dann auf **ausprobieren**.

![Konsole öffnen][api-management-open-console]

Übernehmen Sie den Standardnamen Parameterwerte und wählen Sie dann Ihr Abonnementschlüssel für die kostenlose Testversion.

![Abonnement][api-management-select-key]

>[AZURE.NOTE] Wenn Sie mehrere Abonnements werden Sie sicher, dass den Schlüssel für **Kostenlose Testversion**, sonst die Richtlinien, die in den vorherigen Schritten konfiguriert wurden nicht aktiviert.

Klicken Sie auf **Senden**, und zeigen Sie die Antwort. Der **Antwortstatus** der **200 OK**.

![Ergebnisse][api-management-http-get-results]

**Klicken Sie auf Geschwindigkeit größer als Richtlinie die Rate von 10 Aufrufe pro Minute.** Nachdem das aufruflimit überschritten wird, wird Antwortstatus **429 zu viele Anfragen** zurückgegeben.

![Ergebnisse][api-management-http-get-429]

**Antwortinhalt** gibt an, dass die verbleibenden Versuche werden erfolgreich.

Wenn die Rate von 10 Aufrufe pro Minute Richtlinie gilt, nachfolgende Aufrufe kann erst nach Ablauf von 60 Sekunden von der ersten 10 erfolgreiche Aufrufe des Produkts vor die Rate überschritten wurde. In diesem Beispiel ist das restliche Intervall 54 Sekunden.

## <a name="next-steps"> </a>Nächste Schritte

-   Sehen Sie festlegen von Limits für die Rate und Kontingente im folgenden Video Demo an.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Die erste API in Azure API Management verwalten]: api-management-get-started.md
[Zum Erstellen und Verwenden von Gruppen in Azure API Management]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Aufruf Rate Limit und Kontingent Richtlinien konfigurieren]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota

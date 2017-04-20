<properties
    pageTitle="Mithilfe von API-Verwaltungsdienst zu HTTP-Anfragen"
    description="Anforderung und Antwort Richtlinien in API Management Verwendung externer Dienste Ihre API aufrufen"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="using-external-services-from-the-azure-api-management-service"></a>Externe Dienste von Azure API Management service

Die Richtlinien in Azure API Management Service kann zahlreicher Vorgänge anhand ausschließlich der eingehenden Anforderung, ausgehende Antwort und grundlegende Informationen. Allerdings Interaktion mit externen Diensten von API Management Policies eröffnet viele weitere Möglichkeiten.

Wir haben bereits gesehen, wie wir [Azure Event Hub Service für Protokollierung, Überwachung und Analyse](api-management-log-to-eventhub-sample.md)interagieren können. In diesem Artikel zeigen wir Richtlinien, mit denen Sie interagieren mit externen HTTP Service basiert. Diese Richtlinien dienen für remote-Ereignisse auslösen oder zum Abrufen von Informationen verwendet wird, um die ursprüngliche Anforderung und Antwort in irgendeiner Weise bearbeiten.

## <a name="send-one-way-request"></a>Senden einer Möglichkeit Anforderung
Möglicherweise die einfachste externe Interaktion ist das Auslösen und vergessen Anforderung, die einen externen Dienst eines wichtigen Ereignisses benachrichtigt werden kann. Richtlinie Flow Control können `choose` beliebige Bedingung erkennen, wir interessieren, dann, wenn die Bedingung erfüllt ist, wir einer externen HTTP-Anforderung [senden eine Möglichkeit Anforderung](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) Richtlinie verwenden können. Dies könnte eine Anforderung mit einem Messagingsystem wie Hipchat Puffer oder Mail-API wie SendGrid oder MailChimp oder für kritische Supportfälle etwas PagerDuty. Alle diese Messagingsysteme haben einfachen HTTP-APIs, die wir einfach aufrufen können.

### <a name="alerting-with-slack"></a>Benachrichtigung mit Pufferzeit
Im folgenden Beispiel wird veranschaulicht, wie eine Nachricht an einen Puffer Chatraum Senden der HTTP-Antwortstatuscode ist größer als oder gleich 500. 500 Bereichsfehler zeigt ein Problem mit unserem Back-End-API, die der Client unsere API selbst auflösen kann. Normalerweise wird eine Art Eingriff Ihrerseits erfordert.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Puffer hat das Konzept der eingehenden Web Hooks. Beim Konfigurieren einer eingehenden Web Hook generiert Puffer einen speziellen URL eine POST-Anfrage und eine Nachricht in den Puffer Kanal übergeben kann. JSON-Text, den Sie erstellen, basiert auf Pufferzeit definierte Format.

![Pufferzeit Web Hook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Feuer und vergessen gut genug?
Gibt bestimmte Kompromisse einen Stil auslösen und vergessen Anforderung verwenden. Wenn aus irgendeinem Grund die Anforderung fehlschlägt und der Fehler werden nicht gemeldet. In dieser speziellen Situation haben einen sekundären Fehler reporting System und die zusätzlichen Leistungskosten warten auf die Antwort nicht gerechtfertigt ist. Szenarien, in denen er die Antwort prüfen, ist die Richtlinie [Anforderung senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) eine bessere Option.

## <a name="send-request"></a>Anfrage senden
Die `send-request` Richtlinien können mit einem externen Dienst zu komplexe Verarbeitung Funktionen Daten an die API service zur Weiterverarbeitung Richtlinie verwendet werden.

### <a name="authorizing-reference-tokens"></a>Autorisieren von Verweis Token
Eine wichtige Funktion von API Management schützt Back-End-Ressourcen. Autorisierungsserver verwendet die API [JWT-Token](http://jwt.io/) als Teil seiner fortlaufenden OAuth2 erstellt wie [Azure Active Directory](../active-directory/active-directory-aadconnect.md) verwenden, können Sie die `validate-jwt` Richtlinie die Gültigkeit des Tokens überprüfen. Jedoch erstellen einige autorisierungsserver genannte [Verweis Token](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , die ohne einen Aufruf an den autorisierungsserver überprüft werden kann.

### <a name="standardized-introspection"></a>Standardisierte introspection
In der Vergangenheit wurde keine standardisierten Verfahren ein Verweis Token Autorisierung Server zu überprüfen. Jedoch wurde kürzlich vorgeschlagener standard [RFC 7662](https://tools.ietf.org/html/rfc7662) von der IETF veröffentlicht, die definiert, wie ein Ressourcenserver die Gültigkeit eines Tokens überprüfen kann.

### <a name="extracting-the-token"></a>Extrahieren das token
Der erste Schritt ist das Token aus der Authorization-Header extrahieren. Der Headerwert formatiert werden sollen, mit den `Bearer` Authentifizierungsschema, ein Leerzeichen und das Authentifizierungstoken gemäß [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Leider gibt es Fälle, in denen das Authentifizierungsschema fehlt. Für dieses Konto beim Analysieren, wir teilen den Headerwert auf und wählen die letzte Zeichenfolge aus dem zurückgegebenen Array von Zeichenfolgen. Dies bietet einen Workaround für falsch formatierte Authorization-Header.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Die Validierung Anforderung
Wir haben das Authentifizierungstoken stellen wir die Anforderung zur Überprüfung des Tokens. RFC 7662 wird dieser Prozess Introspection und erfordert, dass Sie `POST` ein HTML-Formular Introspection Ressource. Das HTML-Formular muss mindestens ein Schlüssel-Wert-Paar mit dem Schlüssel `token`. Diese Anforderung an den autorisierungsserver muss auch um sicherzustellen, dass böswillige Clients für gültige Token Schleppnetzfangs gehen können nicht authentifiziert werden.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Antwort überprüfen
Die `response-variable-name` -Attributs zurückgegebene Antwort zugreifen. In dieser Eigenschaft definierte Name als Schlüssel verwendet werden die `context.Variables` Wörterbuch auf der `IResponse` Objekt.

Über das Antwortobjekt wir den Text abrufen und RFC 7622 besagt, dass die Antwort muss ein JSON-Objekt muss mindestens eine Eigenschaft namens `active` , ein boolescher Wert. Wenn `active` gilt dann das Token gültig ist.

### <a name="reporting-failure"></a>Fehler meldet
Wir verwenden ein `<choose>` Richtlinie erkennen, wenn das Token ungültig ist und eine 401-Antwort zurück.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Gemäß [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) beschreibt wie `bearer` Token verwendet werden sollte, wir zurücksenden einer `WWW-Authenticate` Header die 401-Antwort. WWW-Authenticate soll anweisen, einen Client wie ordnungsgemäß autorisierte Anforderung erstellen. Aufgrund der vielfältigen Methoden mit OAuth2 Framework ist es schwierig, die benötigte Informationen zu kommunizieren. Glücklicherweise gibt es derzeit zu helfen [Kunden erfahren Sie, wie Anfragen an einen Ressourcenserver ordnungsgemäß autorisiert](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Endgültige Lösung
Zusammensetzen der Teile, erhalten wir die folgende Richtlinie:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Dies ist nur eines von vielen Beispielen gezeigt, wie die `send-request` Richtlinie wird der Anfragen und Antworten durch API-Verwaltungsdienst nützliche externe Dienste integrieren.

## <a name="response-composition"></a>Antwort-Komposition
Die `send-request` Richtlinie dienen zur Verbesserung der primären Anforderung an ein Backend-System wie wir im vorherigen Beispiel gesehen haben, oder als vollständige ersetzen für Back-End-Aufruf verwendet werden. Mithilfe dieser Technik können wir einfach erstellen zusammengesetzte Ressourcen aggregiert werden von mehreren unterschiedlichen Systemen.

### <a name="building-a-dashboard"></a>Erstellen eines Dashboards   
Manchmal möchten Sie möglicherweise Informationen verfügbar machen, die beispielsweise in mehreren Back-End-Systemen vorhanden ist, auf einem Dashboard. KPIs stammen alle anderen Back-Ends aber lieber nicht direkten Zugriff auf diese, und es wäre schön, wenn die Informationen in einer einzelnen Anforderung abgerufen werden konnte. Vielleicht Backend-Informationen benötigt einige schneiden und analysieren und zunächst etwas bereinigen! Zusammengesetzte Ressource Zwischenspeichern wäre sinnvoll, die Back-End-Last zu reduzieren, wie Sie, dass Benutzer haben hammering die F5-Taste wissen, um festzustellen, ob deren dieser Kriterien ändern.    

### <a name="faking-the-resource"></a>Fälschen der Ressource
Zunächst zu unserem Dashboard-Ressource ist eine neue Operation im Publisher-API Verwaltungsportal zu konfigurieren. Ein Platzhalter-Vorgang Grundsätzen Komposition erstellen unsere dynamische Ressource konfiguriert werden.

![Dashboard-Vorgang](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Die Anfragen
Einmal die `dashboard` Vorgang wurde können wir eine Richtlinie für diesen Vorgang. 

![Dashboard-Vorgang](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Der erste Schritt besteht aus der eingehenden Anforderung Abfrageparameter extrahieren, damit wir sie auf unsere-Back-End weiterleiten können. In diesem Beispiel unserem Dashboard zeigt Informationen über einen Zeitraum eine muss ein `fromDate` und `toDate` Parameter. Wir können die `set-variable` Richtlinie Informationen vom Request-URL zu extrahieren.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Sobald wir diese Informationen haben können wir Anfragen an die Back-End-Systeme. Jede Anforderung erstellt eine neue URL mit den Informationen und ruft den entsprechenden Server und speichert die Antwort in einer Kontextvariablen.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Diese Anfragen führt nacheinander ist nicht ideal. In einer zukünftigen Version werden wir eine neue Richtlinie mit `wait` können alle diese Anfragen parallel ausführen.

### <a name="responding"></a>Antworten

Zum Erstellen von zusammengesetzten Antwort können wir die Richtlinie [zurück](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) . Die `set-body` Element können Ausdruck eine neue `JObject` mit allen die Komponente als Eigenschaften.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Die gesamte Richtlinie sieht wie folgt aus:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

In der Konfiguration des Platzhalters Vorgang konfigurieren wir Dashboard Ressource für mindestens eine Stunde zwischengespeichert werden, da wir wissen, dass die Daten auch wenn also Stunde veraltet, es noch wertvolle Informationen für die Benutzer ausreichend wirksam werden.

## <a name="summary"></a>Zusammenfassung
Azure API Management-Dienst bietet flexible Richtlinien selektiv auf HTTP-Datenverkehr angewendet werden können und Zusammensetzung des Back-End-Services ermöglicht. Soll der API-Gateway mit Warnfunktion Funktionen, Überprüfung, Validierung Funktionen oder zusammengesetzte Ressourcen basierend auf mehrere Back-End-Dienste, die `send-request` und Politiken Möglichkeiten.

## <a name="watch-a-video-overview-of-these-policies"></a>Dieser Richtlinien einen video ansehen
Sehen Sie für Weitere Informationen [senden eine Möglichkeit Anforderung](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [Anforderung senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)und [Antwort zurück](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) Richtlinien in diesem Artikel behandelt im folgende Video.

> [AZURE.VIDEO send-request-and-return-response-policies]

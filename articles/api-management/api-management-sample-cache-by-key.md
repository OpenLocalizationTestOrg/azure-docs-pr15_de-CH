<properties
    pageTitle="Benutzerdefinierte Zwischenspeichern in Azure API Management"
    description="Erfahren Sie, wie Elemente in Azure API Management Schlüssel zwischengespeichert"
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

# <a name="custom-caching-in-azure-api-management"></a>Benutzerdefinierte Zwischenspeichern in Azure API Management
Azure API Management-Dienst bietet integrierte Unterstützung für das [Zwischenspeichern der HTTP-Antwort](api-management-howto-cache.md) mit der Ressourcen-URL als Schlüssel. Der Schlüssel kann von Anforderungsheadern mit geändert werden die `vary-by` Eigenschaften. Dies ist nützlich für das Zwischenspeichern der gesamte HTTP-Antworten (aka Darstellungen), aber manchmal ist es sinnvoll, nur einen Teil einer Darstellung Cache. Der neue [Cache Suchwert](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) und [Cache-Speicher-Wert](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) ermöglichen das Speichern und Abrufen von beliebiger Teile von Daten in Policy-Definitionen. Diese Möglichkeit auch Wert der zuvor eingeführten [Anforderung senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) Richtlinie hinzugefügt da jetzt Antworten von externen Diensten zwischengespeichert werden können.

## <a name="architecture"></a>Architektur  
API Management-Dienst verwendet einen pro Mieter freigegebenen Daten-Cache, bis Skalieren mehrerer Einheiten weiterhin Zugriff auf denselben erhalten Daten zwischengespeichert. Beim Arbeiten mit einer Bereitstellung mit mehreren gibt jedoch unabhängig Caches in allen Regionen. Aus diesem Grund ist es wichtig nicht Cache als einen Datenspeicher behandelt, die einzige Quelle von Datensenderegeln ist. Wenn Sie haben und später mit mehreren Bereitstellung nutzen möchte, können Kunden Reisen Benutzer Zugriff auf die zwischengespeicherten Daten verlieren.

## <a name="fragment-caching"></a>Zwischenspeichern von Fragmenten
Es gibt Fälle, in denen Antworten zurückgegeben wird einen Teil der Daten enthalten, bestimmt ist und bleibt noch für einen angemessenen Zeitraum. Beispielsweise sollten Sie eine Dienstleistung Fluggesellschaft, die Flugtickets flugstatus usw. Informationen bereitstellt. Wenn der Benutzer das Programm Airlines angehört, würden sie auch Informationen zu ihren aktuellen Status Kilometerleistung gesammelt haben. Diese benutzerspezifischen Informationen möglicherweise in einem anderen System gespeichert, aber es wünschenswert Antworten Reservierung zu flugstatus zurückgegeben werden. Dies kann unter Verwendung der sogenannten Fragmentcaches. Die primäre Darstellung kann zurückgegeben werden, vom Ursprungsserver Verwendung des Tokens anzugeben, wo die Benutzer Informationen eingefügt werden. 

Berücksichtigen Sie die folgende JSON-Antwort vom Backend-API.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

Und sekundäre am `/userprofile/{userid}` , die folgendermaßen aussieht,

     { "username" : "Bob Smith", "Status" : "Gold" }

Ermittlung die entsprechenden Benutzerinformationen enthalten müssen, die Benutzer zu identifizieren. Dieser Mechanismus ist implementierungsabhängig. Als Beispiel verwende ich die `Subject` Anspruch auf eine `JWT` token. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

Wir speichern diese `enduserid` Wert in einer Kontextvariable für die spätere Verwendung. Der nächste Schritt ist, ob eine vorherige Anforderung bereits die Benutzerinformationen abgerufen und im Cache gespeichert. Wir verwenden die `cache-lookup-value` Richtlinie.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Liegt kein Eintrag im Cache der Wert dann keine entspricht `userprofile` Kontextvariable erstellt. Wir überprüfen den Erfolg der Suche mit der `choose` Fluss Richtlinie steuern.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Wenn die `userprofile` Kontextvariable ist nicht vorhanden, dann werden wir eine HTTP-Anforderung abrufen müssen.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Wir verwenden die `enduserid` URL der Ressource Benutzer Profil erstellen. Wir haben die Antwort können wir ziehen Sie den Text aus der Antwort und in eine Kontextvariable speichern.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Zur Vermeidung von uns diese HTTP-Anforderung erneut aus, wenn der gleiche Benutzer eine weitere Anforderung, können wir das Benutzerprofil im Cache speichern.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Wir speichern den Wert im Cache mit genau demselben Schlüssel, den wir ursprünglich mit abrufen. Die Dauer den Wert gespeichert werden sollen sollten abhängig von häufig ändern und wie tolerant Benutzer sind auf veraltete Informationen. 

Es ist zu beachten, dass noch eine Out-of-Process Netzwerkanfrage ist und möglicherweise noch zehn Millisekunden auf die Anforderung können aus dem Cache abgerufen. Die Vorteile kommen bei die Benutzerprofilinformationen dauert erheblich länger durch Abfragen oder aggregierte Informationen aus mehreren Back-Ends Datenbank müssen.

Der letzte Schritt im Prozess ist die zurückgegebene Antwort mit unseren Benutzerprofilinformationen aktualisiert.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Ich habe die Anführungszeichen als Teil des Token enthalten, so dass auch bei ersetzen nicht die Antwort noch gültigen JSON. Dies war vor allem soll das Debuggen vereinfacht.

Nachdem Sie sämtliche Schritte kombinieren, ist das Endergebnis eine Richtlinie, die wie folgt aussieht.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Dadurch Zwischenspeichern wird hauptsächlich in Websites, HTML auf der Serverseite besteht, als einzelne Seite gerendert werden kann. Aber es kann auch nützlich sein in APIs, nicht Clients Client, side HTTP-caching oder es wird nicht in die Verantwortung auf dem Client.

Dieses Fragment caching dieselben erreichen Sie auch auf den Back-End-Webservern mit einem Redis Cachingserver, mithilfe den API-Verwaltungsdienst zum Durchführen dieser Aufgaben ist jedoch hilfreich bei zwischengespeicherten Fragmente aus verschiedenen Back-End als primären Antworten.

## <a name="transparent-versioning"></a>Transparente Versionskontrolle
Es ist üblich für mehrere andere Implementierung Versionen einer API jeweils unterstützt werden. Dies ist möglicherweise auf unterschiedlichen Umgebungen, Entwicklung, Test, Produktion usw., oder es unterstützt ältere API Zeit API Verbraucher auf neuere Versionen zu migrieren. 

Ein Verfahren zur Behandlung dieser anstatt Client Entwickler die URLs ändern `/v1/customers` , `/v2/customers` in der Consumer Daten die Version der API speichern sie derzeit verwenden und den entsprechenden Back-End-URL aufrufen möchten. Ermittlung die richtigen Back-End-URL für einen bestimmten Client aufrufen muss einige Konfigurationsdaten abgefragt. Durch diese Konfigurationsdaten zwischenspeichern, können wir die Leistungseinbußen durch das Ausführen dieser Suche minimieren.

Zunächst wird der Identifier, konfigurieren Sie die gewünschte Version zu bestimmen. In diesem Beispiel habe ich die Version der Abonnement-Produktschlüssel zuordnen. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Dann machen wir eine Cache-Suche, um festzustellen, ob wir die gewünschte Clientversion bereits abgerufen haben.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Anschließend überprüfen wir, wenn wir nicht im Cache gefunden.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Wenn wir nicht und wir gehen abrufen.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Den Textkörper der Antwort aus der Antwort zu extrahieren.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Im Cache für zukünftige Verwendung speichern.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

Und schließlich die Backend-URL die Version vom Client gewünschten Service.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Die Richtlinie vollständig lautet wie folgt.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Verbrauchern API transparent steuern die Back-End-Version zugegriffen wird von Clients ohne aktualisieren und erneut bereitstellen Clients ist eine elegante Lösung API Versioning sorgen.

## <a name="tenant-isolation"></a>Mieter Isolation

In größeren, mehrinstanzenfähigen Bereitstellung erstellen einige Unternehmen separate Gruppen Mieter unterschiedliche Bereitstellung von Back-End-Hardware. Dies minimiert die Anzahl der Kunden durch ein Hardwareproblem auf dem Back-End betroffen sind. Außerdem können neue Softwareversionen in Phasen eingeführt werden. Im Idealfall sollten diese Back-End-Architektur API Verbraucher transparent. Dies ähnlich transparent Versioning lässt da basieren auf der gleichen Technik Konfigurationsstatus pro API-Schlüssel mit Backend-URL zu bearbeiten.  

Anstatt einer bevorzugten Version der API für jedes Abonnementschlüssel müsste einen Bezeichner zurückgegeben werden, der der Gruppe zugewiesenen Hardware Mieter beziehen. Dieser Bezeichner kann verwendet werden, die entsprechenden Back-End-URL erstellen.

## <a name="summary"></a>Zusammenfassung
Die Freiheit den Azure-API Management Cache zum Speichern von Daten ermöglicht den effizienten Zugriff auf Konfigurationsdaten, die sich auf können eine eingehende Anforderung verarbeitet wird. Auch kann verwendet werden, um Daten speichern, die vom Back-End-API zurückgegebene Antworten erweitern.

## <a name="next-steps"></a>Nächste Schritte
Bitte geben Sie uns Feedback im Disqus-Thread zu diesem Thema gibt es andere Szenarien, die diese Richtlinien für Sie aktiviert haben, ob gibt es Szenarios erzielen möchten aber nicht fühlen derzeit.

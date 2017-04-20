<properties
   pageTitle="Wie Sie Power BI eingebettete mit | Microsoft Azure"
   description="Erfahren Sie mit Power BI Embedded verwenden "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Verwendung von Power BI eingebettete mit REST


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI eingebettet: Was ist und es
Überblick über Power BI eingebettete offizielle [Power BI Embedded-Site](https://azure.microsoft.com/services/power-bi-embedded/)beschrieben, aber einen Blick werfen, bevor wir mit weiteren Informationen erhalten.

Es ist wirklich ganz einfach. Ein ISV oft dynamische Daten Visualisierung von [Power BI](https://powerbi.microsoft.com) in ihrer eigenen Anwendung als Bausteine der Benutzeroberfläche verwenden möchte.

Jedoch wissen, Power BI-Berichten oder Kacheln in Webseiten einbetten bereits ohne Strom BI eingebettete Azure Service mit **Power BI-API**. Wenn Sie die Berichte in der gleichen Organisation freigeben möchten, können Sie die Berichte mit Azure AD-Authentifizierung einbetten. Der Benutzer, die Berichte anzeigen muss mit eigenen Azure AD-Konto anmelden. Wenn Sie Berichte für alle Benutzer (auch externe Benutzer) freigeben möchten, können Sie einfach mit anonymem Zugriff einbetten.

Aber Sie sehen dieses einfache einbetten Lösung nicht ganz genau auf die Bedürfnisse der ISV-Anwendung.
Meisten ISV müssen die Daten für ihre eigenen Kunden, nicht in der eigenen Organisation bereitzustellen. Beispielsweise wenn Sie Dienste für Unternehmen A und Unternehmen B liefern, sollten Benutzer in Unternehmen A nur Daten für ihre eigene Firma A. finden Sie unter Mehrere Mandanten ist also für die Bereitstellung erforderlich.

ISV-Anwendung kann auch einen eigenen Authentifizierungsmethoden wie Forms-Authentifizierung, Standardauthentifizierung bietet... Dann muss die Einbettung Lösung sicher mit dieser bestehenden Authentifizierungsmethoden zusammenarbeiten. Es ist auch für Benutzer können die ISV-Applikationen ohne zusätzlichen Kauf oder Lizenzierung von Power BI-Abonnement erforderlich.

 **Power BI eingebettete** für genau diese Art von ISV Szenarien entwickelt. So jetzt haben wir diese kurze Einführung beiseite, erfahren Sie in einige details

Sie können .NET \(C#) oder Node.js SDK einfach die Anwendung mit Power BI Embedded erstellen. Aber in diesem Artikel wir erklären über HTTP Bewegung \(inkl. AuthN) Power BI ohne SDKs. Grundlegendes zu diesem Datenstrom, Ihre Anwendung **in einer beliebigen Programmiersprache**erstellen und können kennen tief die Essenz des Power BI eingebettete.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Erstellen Sie Power BI Arbeitsbereich Sammlung und Get Zugriffstaste \(Provisioning)
Power BI eingebettete ist der Azure-Dienste. ISV, Azure-Portal verwendet, für Gebühren belastet \(pro Stunde Sitzung), und der Benutzer den Bericht nicht geladen oder sogar benötigen Azure-Abonnement.
Vor der Entwicklung unserer Anwendung müssen **Power BI Arbeitsbereich Auflistung** mithilfe von Azure-Portal erstellt werden.

Jeder Arbeitsbereich Power BI Embedded ist der Arbeitsbereich für jeden Debitor (Mandant) und viele Arbeitsbereiche können in jeder Arbeitsbereich-Auflistung hinzufügen. Dieselbe Tastenkombination ist in jeder Arbeitsbereich-Auflistung verwendet. In Effekt ist die Workspace-Auflistung Sicherheitsgrenze für Power BI Embedded.

![](media\power-bi-embedded-iframe\create-workspace.png)

Wir abschließend Arbeitsbereich Sammlung kopieren Sie Zugriffstaste Azure-Portal.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Wir können auch bereitstellen die Workspace-Auflistung und Zugriffstaste über REST API. Informationen finden Sie unter [Power BI Ressource Anbieter APIs](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>.Pbix Datei mit Power BI Desktop erstellen
Als Nächstes müssen wir Verbindung und Berichte eingebettet werden erstellen.
Für diesen Vorgang ist keine Programmierung oder Code unterschieden. Wir verwenden einfach Power BI Desktop.
In diesem Artikel wird nicht wir die Details zur Verwendung von Power BI Desktop wechseln Wenn Sie hier Hilfe benötigen, finden Sie unter [Erste Schritte mit Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). In unserem Beispiel verwenden wir nur im [Einzelhandel Analyse Beispiel](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Erstellen eines Arbeitsbereichs Power BI

Da die Bereitstellung alle erfolgt, beginnen des Debitors Arbeitsbereich in der Workspace-Auflistung über REST APIs erstellen. Die folgenden HTTP-POST anfordern (REST) ist den neuen Arbeitsbereich in unseren vorhandenen Arbeitsbereich Auflistung erstellen. In unserem Beispiel ist der Auflistungsname Arbeitsbereich **Mypbiapp**.
Wir legen nur Zugriffstaste, die wir zuvor kopiert, als **AppKey**. Es ist sehr einfache Authentifizierung!

**HTTP-Anforderung**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Der zurückgegebene **WorkspaceId** ist für die folgenden weiteren API-Aufrufe verwendet. Die Anwendung muss diesen Wert beibehalten.

## <a name="import-pbix-file-into-the-workspace"></a>Importieren der .pbix in den Arbeitsbereich
Jeder Arbeitsbereich bietet eine einzelne Power BI Desktop-Datei mit einem Dataset \(einschließlich Datasource) und Berichte. Wir können unsere .pbix-Datei auf den Arbeitsbereich, wie im folgenden Code gezeigt. Wie Sie sehen können, laden wir binäre .pbix Datei MIME Multipart HTTP.

Uri-Fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** ist die WorkspaceId und Abfrage Parameter **DatasetDisplayName** wird der Dataset-Name erstellt. Erstellte Dataset enthält alle Daten im Zusammenhang mit Artefakte in .pbix Datei z. B. als Zeiger auf die Datenquelle importierten Daten usw...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Dieses Importtasks möglicherweise eine Weile ausgeführt. Nach Abschluss können unsere Anwendung Vorgangsstatus importieren-Id verwenden. In unserem Beispiel ist die Import-Id **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Die folgenden Fragen diese Import ID Status:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Wenn die Aufgabe abgeschlossen ist, könnte die HTTP-Antwort wie folgt:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Wenn der Vorgang abgeschlossen ist, könnte die HTTP-Antwort wie folgt:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Datenquellen-Konnektivität \(und mehrere Mandanten Daten)
Fast alle Artefakte in .pbix Datei in unserem Arbeitsbereich importiert werden, sind die Anmeldeinformationen für Datenquellen nicht. Daher kann nicht **DirectQuery**Modus eingebettete Bericht richtig angezeigt werden. Aber bei **Importmodus**können wir den Bericht mit den vorhandenen importierten Daten anzeigen. In diesem Fall müssen wir die Anmeldeinformationen mit den folgenden Schritten über REST Aufrufe festlegen.

Erstens müssen die Gateway-Datenquelle abgerufen werden. Wir wissen, dass die Dataset- **Id** zuvor zurückgegebene Id ist.

**HTTP-Anforderung**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Verwenden die zurückgegebene Gateway und Datasource-Id \(finden Sie im vorherigen **GatewayId** und **Id** des zurückgegebenen Ergebnisses), können wir die Anmeldeinformationen dieser Datenquelle wie folgt ändern:

**HTTP-Anforderung**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-Antwort**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

In der Produktion können wir auch andere Verbindungszeichenfolge für jeden Arbeitsbereich REST API festlegen. \(also, wir die Datenbank für jeden Kunden trennen.)

Folgendes ändert die Verbindungszeichenfolge der Datenquelle über REST.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Können wir Zeile Sicherheitsstufe in Power BI Embedded oder wir separate Daten für jeden Benutzer in einem Bericht. Wir können damit, jeden Bericht Debitor mit demselben .pbix bereitstellen \(Benutzeroberfläche, etc..) und anderen Datenquellen.

> [AZURE.NOTE] Wenn Sie **Importmodus** statt **DirectQuery-Modus**verwenden, besteht keine Möglichkeit Modelle über API zu aktualisieren. Und lokalen Datenquellen über Power BI Gateway nicht noch in Power BI Embedded unterstützt. Aber Sie wollen wirklich [Power BI-Blog](https://powerbi.microsoft.com/blog/) für Neuigkeiten im Auge behalten und was in Zukunft kommt frei.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Authentifizierung und hosting (einbetten) Berichte in Webseite

In der vorherigen REST-API können wir die Zugriffstaste **AppKey** selbst als der Authorization-Header. Da diese Aufrufe auf dem Back-End-Server behandelt werden können, ist sicher.

Aber wenn wir den Bericht in Webseite einbetten, diese Art von Informationen wäre behandelt werden mit JavaScript \(Front-End). Dann muss der Headerwert Autorisierung gesichert werden. Unsere Schlüssel von einem böswilligen Benutzer oder bösartiger Code entdeckt, können sie mit diesem Schlüssel Operationen aufrufen.

Wenn wir den Bericht in Webseite einbetten, müssen wir das berechnete Token statt Zugriffstaste **AppKey**verwenden. Unsere Anwendung muss OAuth Json Web Token erstellen \(JWT) der Ansprüche und berechnete Signatur besteht. Wie unten dargestellt, wird diese OAuth JWT codierte Zeichenfolge Punkt getrennten Token.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Zunächst müssen wir den Eingabewert vorbereiten, die später signiert wird. Dieser Wert ist die Zeichenfolge base64 codierte Url (rfc4648) die folgenden Json und diese durch den Punkt getrennten \(.) Zeichen. Erläutern Sie, wie die Berichts-Id abgerufen.

> [AZURE.NOTE] Wenn wir Zeile Level Security (RLS) mit Power BI Embedded verwenden möchten, müssen wir **Benutzernamen** und **Rollen** in den Ansprüchen angeben.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Anschließend legen wir die base64-codierte Zeichenfolge HMAC \(Signatur) mit SHA256-Algorithmus. Diese signierte Eingabewert ist die vorhergehende Zeichenfolge.

Muss die Wert und die Signatur Eingabezeichenfolge mit Punkt kombiniert \(.) Zeichen. Abgeschlossene Zeichenfolge ist die app Token Bericht eingebettet. Auch wenn app Token von böswilligen Benutzern erkannt wird, kann nicht die ursprünglichen Zugriffstaste abgerufen werden. Dieses Token app läuft schnell.

Hier ist eine PHP beispielsweise folgendermaßen:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Schließlich den Bericht in die Webseite einbetten

Für den Bericht einbetten, müssen wir die Url einbetten und Bericht **Id** mit folgenden REST-API.

**HTTP-Anforderung**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-Antwort**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Wir können in unserer Web app mit dem vorherigen app Token des Berichts einbetten.
Betrachten wir den nächsten Beispielcode, ist das erste Teil im vorherigen Beispiel identisch. Im zweiten Teil dieses Beispiels zeigt **EmbedUrl** \(das vorherige Ergebnis) in den Iframe und ist das app-Token in den Iframe.

> [AZURE.NOTE] Sie müssen eines eigenen Berichts ID-Wert ändern. Außerdem ist aufgrund eines Fehlers in unser Content Managementsystem Iframe-Tag im Codebeispiel buchstäblich schreibgeschützt. Entfernen Sie den begrenzten Text aus dem Tag kopieren und fügen Sie folgenden Code.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

Und hier ist das Ergebnis:

![](media\power-bi-embedded-iframe\view-report.png)

Zu diesem Zeitpunkt zeigt Power BI eingebettete nur der Bericht im Iframe. Aber, [Power BI-Blog](). Verbesserungen können neue clientseitigen APIs, die werden wir Informationen in den Iframe sowie Informationen aus. Aufregend!


## <a name="see-also"></a>Siehe auch
- [Authentifizierung und Autorisierung in Power BI Embedded](power-bi-embedded-app-token-flow.md)

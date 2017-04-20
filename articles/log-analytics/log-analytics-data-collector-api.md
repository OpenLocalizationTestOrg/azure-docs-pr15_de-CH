<properties
    pageTitle="Analytics HTTP Datensammler API anmelden | Microsoft Azure"
    description="Log Analytics HTTP Data Collector-API können Sie nach JSON-Daten Protokollanalyse-Repository von einem beliebigen Client hinzufügen, die die REST-API aufrufen können. Dieser Artikel beschreibt, wie die API und Beispiele für Daten mit verschiedenen Programmiersprachen veröffentlichen."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Log Analytics HTTP Datensammler API

Bei Verwendung von Azure Protokoll Analytics HTTP Data Collector API können Sie POST JavaScript Objekt Notation (JSON) Daten Protokollanalyse-Repository von einem beliebigen Client hinzufügen, die die REST-API aufrufen können. Mithilfe dieser Methode können Sie Daten von Drittanbietern oder Skripts, wie aus einem Runbook in Azure Automation senden.  

## <a name="create-a-request"></a>Erstellen einer Anforderung

Die nächsten beiden Tabellen Listen die Attribute, die für jede Anforderung an die Protokolldatei Analytics HTTP-Kollektor-API. Jedes Attribut später in diesem Artikel ausführlicher beschrieben.

### <a name="request-uri"></a>Anfrage-URI

| Attribut | Eigenschaft |
|:--|:--|
| -Methode | Bereitstellen |
| URI | https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Inhaltstyp | Anwendung/json |

### <a name="request-uri-parameters"></a>Anfrage-URI-Parameter
| Parameter | Beschreibung |
|:--|:--|
| CustomerID  | Der eindeutige Bezeichner für den Arbeitsbereich Microsoft Operations Management Suite. |
| Ressource    | Der Name der API: / API-Protokolle. |
| API-Version | Die Version der API mit dieser Anforderung. Derzeit ist 2016-04-01. |

### <a name="request-headers"></a>Anfrage-Header
| Kopfzeile | Beschreibung |
|:--|:--|
| Autorisierung | Die Signatur der Autorisierung. Später in diesem Artikel erhalten Sie Informationen zum Erstellen eines HMAC SHA256-Headers. |
| Protokoll-Typ | Geben Sie den Datensatz der Daten, die gesendet werden. Der Protokolltyp unterstützt derzeit nur Buchstaben. Es unterstützt nicht numerische Zeichen oder Sonderzeichen. |
| X-ms-Datum | Das Datum, das die Anforderung, im Format RFC 1123 verarbeitet wurde. |
| Zeit generiert Feld | Der Name eines Felds in den Daten mit Zeitstempel des Datenelements. Wenn Sie ein Feld angeben, werden dessen Inhalt für **TimeGenerated**verwendet. Wenn dieses Feld nicht angegeben wird, ist die Standardeinstellung für **TimeGenerated** die Zeit, die die Nachricht aufgenommen wird. Der Inhalt des Feldes Meldung muss ISO 8601-Format YYYY-MM-: ssZ. |


## <a name="authorization"></a>Autorisierung

Jede Anforderung an die Protokolldatei Analytics HTTP-Kollektor-API muss einem Autorisierungsheader enthalten. Um eine Anforderung zu authentifizieren, müssen Sie mit der primären oder sekundären Schlüssel für den Arbeitsbereich, der die Anforderung signieren. Übergeben Sie dann die Signatur als Teil der Anforderung.   

Hier wird das Format der Authorization-Header:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* ist der eindeutige Bezeichner für den Arbeitsbereich Operations Management Suite. *Signatur* ist eine [Hash-based Message Authentication Code (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , der von der Anforderung erstellt und dann mit dem [SHA256-Algorithmus](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx)berechnet. Dann, codieren Sie es mit Base64-Codierung.

Verwenden Sie dieses Format Signaturzeichenfolge **SharedKey** codieren:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Hier ist ein Beispiel einer Signatur:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Wenn die Signaturzeichenfolge haben mit dem HMAC SHA256-Algorithmus auf UTF-8-codierten Zeichenfolge codiert und dann das Ergebnis als Base64 codiert. Verwenden Sie dieses Format:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Die Beispiele in den nächsten Abschnitten haben zu einem Autorisierungsheader erstellen.

## <a name="request-body"></a>Anfragetext

Der Hauptteil der Nachricht muss in JSON. Sie müssen einen oder mehrere Datensätze mit den Name-Wert-Paare im folgenden Format enthalten:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Mit dem folgenden Format können Sie mehrere Datensätze in einer einzelnen Anforderung batch. Alle Datensätze muss denselben Datensatztyp auswählen.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Datensatztyp und Eigenschaften

Sie definieren einen benutzerdefinierten Datensatztyp beim Senden von Daten über die Log Analytics HTTP-Kollektor-API. Derzeit kann nicht zum Schreiben Daten in vorhandenen Datensatztypen, die erstellt wurden mit anderen Datentypen Solutions. Protokollanalyse liest eingehende Daten und erstellt dann Eigenschaften die Datentypen der Werte, die Sie eingeben.

Jede Anforderung Protokoll Analytics API muss einen **Protokoll-Type** -Header mit dem Namen für den Datensatz enthalten. Suffix **_CL** ergänzt den Namen geben Sie zur Unterscheidung von anderen Protokoll als ein benutzerdefiniertes Protokoll. Wenn Sie die **MyNewRecordType**eingeben, erstellt der Protokollanalyse einen Datensatz mit dem **MyNewRecordType_CL**an. Dadurch wird sichergestellt, dass keine zwischen Namen von Benutzer erstellt und in aktuellen oder zukünftigen Microsoft geliefert Konflikte.

Um eine Eigenschaft zu identifizieren, wurde der Eigenschaftenname Protokollanalyse ein Suffix hinzugefügt. Wenn eine Eigenschaft einen null-Wert enthält, wird die Eigenschaft nicht in diesem Datensatz enthalten. Diese Tabelle enthält die Eigenschaft und die entsprechenden Suffix:

| Eigenschaft | Suffix |
|:--|:--|
| Zeichenfolge    | _S |
| Boolescher Wert   | _B |
| Double    | _D |
| Datum/Uhrzeit | _T |
| GUID      | _g |


Der Datentyp für jede Eigenschaft Protokollanalyse verwendet, hängt davon ab, ob der Datensatztyp für den neuen Datensatz bereits vorhanden ist.

- Wenn der Datensatztyp nicht existiert, erstellt Protokollanalyse eine neue. Protokollanalyse wird mit JSON Typrückschluss den Datentyp für jede Eigenschaft für den neuen Datensatz bestimmt.
- Wenn der Eintrag vorhanden ist, versucht Protokollanalyse zu einem neuen Datensatz basierend auf vorhandenen Eigenschaften. Wenn der Datentyp für eine Eigenschaft im neuen Datensatz stimmt nicht überein und in den vorhandenen Typ konvertiert werden kann oder enthält der Datensatz eine Eigenschaft, die nicht vorhanden ist, erstellt Protokollanalyse eine neue Eigenschaft mit den entsprechenden Suffix.

Dieser Eintrag senden würden beispielsweise einen Datensatz mit drei Eigenschaften, **Number_d**, **Boolean_b**und **String_s**erstellen:

![Beispiel-Datensatz 1](media/log-analytics-data-collector-api/record-01.png)

Wenn Sie alle Werte als Zeichenfolgen formatiert dann diesen nächsten Eintrag gesendet, würde die Eigenschaften nicht ändern. Diese Werte können vorhandene Datentypen konvertiert werden:

![Beispiel-Datensatz 2](media/log-analytics-data-collector-api/record-02.png)

Aber wenn Sie dann diese nächste Übermittlung, Protokollanalyse, erstellen, neue Eigenschaften **Boolean_d** und **String_d**. Diese Werte können nicht konvertiert werden:

![Beispieleintrag 3](media/log-analytics-data-collector-api/record-03.png)

Wenn Sie den folgenden Eintrag dann vor der Datensatztyp gesendet, würde Protokollanalyse einen Datensatz mit drei Eigenschaften **Zahl_Erfolge**, **Boolean_s**und **String_s**erstellen. In diesem Eintrag wird die ursprüngliche Werte als Zeichenfolge formatiert:

![Beispieleintrag 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Daten-Grenzwerte
Es gibt einige Einschränkungen Daten Log Analytics-Datensammlung API bereitgestellt.

- Maximal 30 MB pro Protokoll Analytics Data Collector-API an. Dies ist eine maximale Größe für einen Beitrag. Wenn die Daten aus einem buchen 30 MB überschreitet, sollten Sie die Daten in kleinere Größe Stücke aufteilen und gleichzeitig senden. 
- Maximal 32 KB-Limit für Feldwerte. Wenn der Feldwert größer als 32 KB ist, werden die Daten abgeschnitten. 
- Empfohlene maximale Anzahl von Feldern für einen bestimmten Typ ist 50. Dies ist eine praktische Beschränkung Nutzbarkeit und Suche Erfahrung.  


## <a name="return-codes"></a>Rückgabecodes

Der HTTP-Statuscode 202 bedeutet, dass die Anforderung wurde zum Verarbeiten angenommen, aber die Verarbeitung noch nicht abgeschlossen. Dies bedeutet, dass der Vorgang erfolgreich abgeschlossen.

Diese Tabelle enthält den vollständigen Satz von Statuscodes, die der Dienst zurückgeben kann:

| Code | Status | Fehlercode | Beschreibung |
|:--|:--|:--|:--|
| 202 | Akzeptiert |  | Die Anforderung wurde angenommen. |
| 400 | Ungültige Anforderung | InactiveCustomer | Der Arbeitsbereich wurde geschlossen. |
| 400 | Ungültige Anforderung | InvalidApiVersion | API-Version, die Sie angegeben wurde vom Dienst nicht erkannt. |
| 400 | Ungültige Anforderung | InvalidCustomerId | Die angegebene Arbeitsbereich-ID ist ungültig. |
| 400 | Ungültige Anforderung | InvalidDataFormat | Ungültiger JSON wurde übermittelt. Der Antworttext enthalten weitere Informationen zum Beheben des Fehlers. |
| 400 | Ungültige Anforderung | InvalidLogType | Der Protokolltyp enthaltenen Sonderzeichen oder numerische Werte angegeben. |
| 400 | Ungültige Anforderung | MissingApiVersion | Die API-Version wurde nicht angegeben. |
| 400 | Ungültige Anforderung | MissingContentType | Der Inhaltstyp wurde nicht angegeben. |
| 400 | Ungültige Anforderung | MissingLogType | Der erforderliche Wert Protokolltyp wurde nicht angegeben. |
| 400 | Ungültige Anforderung | UnsupportedContentType | Der Inhaltstyp wurde nicht auf **Application/Json**festgelegt. |
| 403 | Verboten | InvalidAuthorization | Der Dienst konnte die Anforderung authentifiziert. Überprüfen Sie der Arbeitsbereich ID und Verbindung Schlüssel gültig. |
| 500 | Interner Serverfehler | UnspecifiedError | Das hat einen internen Fehler festgestellt. Wiederholen Sie die Anforderung. |
| 503 | Dienst nicht verfügbar | ServiceUnavailable | Der Dienst steht zurzeit Anfragen. Wiederholen Sie die Anforderung. |

## <a name="query-data"></a>Abfragen von Daten

Log Analytics HTTP Data Collector-API übermittelten Daten Abfragen, nach Datensätzen suchen **Typ** **LogType** Wert entspricht, die Sie angegeben, mit angefügten **_CL**. Beispielsweise wird **MyCustomLog**dann Sie zurück alle Datensätze mit **Type = MyCustomLog_CL**.


## <a name="sample-requests"></a>Beispiel-Anfragen

In den folgenden Abschnitten finden Sie Beispiele wie zum Senden von Daten an die Protokolldatei Analytics HTTP-Kollektor-API mit verschiedenen Programmiersprachen.

Führen Sie diese Schritte aus, um die Variablen für Authorization-Header für jede Probe:

1. Wählen Sie im Portal Operations Management Suite **Einstellungen** Kachel, und wählen Sie die Registerkarte **Datenquellen verbunden** .
2. Neben **Arbeitsplatz-ID**wählen Sie das Symbol "Kopieren" und fügen Sie die ID als Wert der Variablen **Kundennummer** .
3. Rechts des **Primärschlüssels**wählen Sie das Symbol "Kopieren" und fügen Sie die ID als Wert für die Variable **Shared Key** .

Alternativ können Sie die Variablen für den Protokolltyp und JSON-Daten ändern.

### <a name="powershell-sample"></a>PowerShell-Beispiel

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C#-Beispiel

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Python-Beispiel

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Ansicht-Designer](log-analytics-view-designer.md) benutzerdefinierte Ansichten der Daten erstellen, die Sie senden.

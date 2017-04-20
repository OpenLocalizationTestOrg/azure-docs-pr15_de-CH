<properties
    pageTitle="Shared Access Signatures – Übersicht | Microsoft Azure"
    description="Was Shared Access Signatures sind, wie sie arbeiten, und wie sie Knoten, PHP und C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>SAS

*Shared Access Signaturen* (SAS) sind primäre Standardsicherheitsmechanismus für Service Bus einschließlich Ereignis Hubs, vermittelte messaging (Warteschlangen und Themen) und messaging weitergeleitet. Dieser Artikel beschreibt Shared Access Signatures wie sie funktionieren und wie sie plattformunabhängiger nutzen.

## <a name="overview-of-sas"></a>Übersicht über SAS

Freigegebene Access-Signaturen sind basierend auf secure Hash SHA-256 oder URIs Authentifizierungsmechanismus. SAS ist ein äußerst leistungsfähiges Mechanismus, der von allen Service Bus-Diensten verwendet wird. Tatsächlich SAS besteht aus zwei Komponenten: einer *Zugriffsrichtlinie freigegeben* und *SAS* (häufig als *token*bezeichnet).

Ausführlichere Informationen zu freigegebenen Zugriff Signaturen mit Service Bus finden im [SAS-Authentifizierung mit Service Bus](service-bus-shared-access-signature-authentication.md).

## <a name="shared-access-policy"></a>Freigegebene Richtlinien

Wichtig zum Verständnis von SAS ist, dass alles mit beginnt. Für jede Richtlinie entscheiden Sie drei Angaben: **Name**, **Bereich**und **Berechtigungen**. Der **Name** ist genau das. ein eindeutiger Name in diesem Bereich. Der Bereich ist einfach: Es ist der URI der Ressource. Service Bus-Namespace ist der Bereich den vollqualifizierten Domänennamen (FQDN), wie `https://<yournamespace>.servicebus.windows.net/`.

Die verfügbaren Berechtigungen für eine Richtlinie sind weitgehend selbsterklärend:

  + Senden
  + Überwachen
  + Verwalten

Nach dem Erstellen der Richtlinie erhält es einen *Primärschlüssel* und *Sekundärschlüssel*. Kryptografisch starken Schlüssel sind. Nicht verlieren oder sie Speicherverluste - sie immer in der [Azure-Portal][]verfügbar. Verwenden Sie entweder die generierten Schlüssel und können sie jederzeit wiederherstellen. Jedoch wenn Regenerieren oder Ändern des Primärschlüssels in der Richtlinie erstellt freigegebene Access Unterschriften ungültig.

Beim Erstellen eines Service Bus-Namespaces eine Richtlinie automatisch den gesamten Namespace namens **RootManageSharedAccessKey**erstellt und diese Richtlinie verfügt über alle Berechtigungen. Keine Anmeldung als **Root**, damit diese Richtlinie ein wirklich guten Grund verwenden. Zusätzliche Richtlinien können in das Register für den Namespace im Portal **Konfigurieren** . Es ist wichtig, beachten Sie, dass eine einzelne Ebene Service Bus Struktur (Warteschlange Event Hub-Namespace usw.) können nur bis zu 12 Richtlinien zugeordnet.

## <a name="shared-access-signature-token"></a>Freigegebene SAS (Token)

Die Richtlinie selbst ist nicht das Zugriffstoken für Service Bus. Es ist, das Zugriffstoken mit der primären oder sekundären Schlüssel generiert wird-Objekt. Das Token wird durch eine Zeichenfolge im folgenden Format sorgfältig erstellen:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Wo `signature-string` ist der Hash SHA-256 Anwendungsbereich Token (**Bereich** wie im vorherigen Abschnitt beschrieben) ein CRLF hinzugefügt und eine Ablaufzeit (in Sekunden seit der Epoche: `00:00:00 UTC` am 1. Januar 1970).

Der Hash ähnelt der folgenden Pseudocode und 32 Byte zurückgegeben.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Nicht gehashte Werte in der Zeichenfolge **SharedAccessSignature** , damit der Empfänger den Hash mit denselben Parametern, um sicherzustellen, dass das gleiche Ergebnis berechnen kann. Der URI Gibt den Bereich und der Schlüsselnamen bezeichnet die Richtlinie zum Berechnen des Hash verwendet werden. Dies ist Sicherheit wichtig. Wenn die Signatur nicht der Empfänger (Service Bus) berechnet entsprechen, wird der Zugriff verweigert. An diesem Punkt können Sie sicher sein, dass der Absender Zugriff auf den Schlüssel und in der Richtlinie angegebenen Rechte gewährt werden sollte.

## <a name="generating-a-signature-from-a-policy"></a>Erstellen einer Signatur aus einer Richtlinie

Wie gehen Sie tatsächlich im Code dazu? Betrachten wir nun einige davon.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Verwenden die SAS (auf HTTP-Ebene)
 
Jetzt wissen Sie wie freigegebene Access Signaturen für alle Entitäten in Service Bus erstellen, können Sie HTTP POST durchführen:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Beachten Sie, dass dies alles funktioniert. SAS können für eine Warteschlange, Thema, Abonnements, Event Hub oder weiterleiten. Verwenden Sie pro Verleger Identität für Ereignis Hubs, Sie fügen `/publishers/< publisherid>`.

Wenn Sie einen Absender oder Client ein SAS-Token, sie haben den Schlüssel direkt, und sie können nicht den Hash zu es stornieren. So haben Sie Kontrolle über welche sie zugreifen können, und wie lange. Ein wichtiger ist, ändert den Primärschlüssel in der Richtlinie erstellt freigegebene Access Unterschriften ungültig wird.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Verwenden die SAS (auf AMQP)

Im vorherigen Abschnitt wurde gezeigt, wie mithilfe SAS-Token mit HTTP POST-Anforderung zum Senden von Daten an den Service Bus. Wie Sie wissen, können Sie Service Bus mit der erweiterte Message Queuing Protocol (AMQP) das bevorzugte Protokoll aus Leistungsgründen in vielen Szenarios verwenden zugreifen. SAS-token Verwendung mit AMQP wird im Dokument [AMQP Claim-Based Security Version 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) beschrieben, die für den Entwurf von Azure unterstützt seit 2013 ist.

Vor Service Bus Daten senden, muss der Verleger SAS-Token in einer AMQP-Nachricht an definierten AMQP Knoten **$cbs** senden (Sie sehen es als "speziellen" und überprüfen die SAS-Token vom Dienst verwendete Warteschlange). Der Verleger muss **ReplyTo** -Feld innerhalb der AMQP-Nachricht angeben. Dies ist der Knoten mit dem Ergebnis der Validierung token (eine einfache Anforderung/Antwort-Muster zwischen Verleger und Service) der Dienst antwortet. Diese Antwort Knoten erstellt "zu" sprechen "dynamische Erstellung Remoteknoten" wie AMQP 1.0-Spezifikation. Sichergestellt, dass das SAS-Token gültig ist, Verleger wechseln und zum Senden von Daten an den Dienst zu starten.

Die folgenden Schritte zeigen das SAS-Token mit AMQP-Protokoll [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) Bibliothek senden. Dies ist nützlich, wenn Sie offizielle Service Bus SDK verwenden können (z. B. in WinRT .net Compact Framework, .net Micro Framework und Mono) in C# entwickeln\#. Diese Bibliothek ist natürlich besser zu verstehen, wie anspruchsbasierte Sicherheit für Ebene AMQP funktioniert, wie Sie funktioniert auf HTTP-Ebene (mit HTTP POST-Anforderung gesendeten Header "Autorisierung" SAS-Token). Wenn solche Fachwissen über AMQP benötigen, können Sie die offizielle Service Bus SDK mit .net Framework-Clientanwendungen, die es für Sie.

### <a name="c35"></a>C & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Die `PutCbsToken()` -Methode empfängt die *Verbindung* (AMQP Verbindung Klasseninstanz gemäß [AMQP .NET Lite Library](https://github.com/Azure/amqpnetlite)), die die TCP-Verbindung mit dem Dienst und Parameters *SasToken* SAS-Token senden darstellt. 

> [AZURE.NOTE] Es ist wichtig, dass die Verbindung mit **SASL-Authentifizierungsmechanismus auf externe** (und nicht die Standard-Ebene mit Benutzernamen und Kennwort verwendet, wenn Sie das SAS-Token senden müssen).

Anschließend erstellt der Publisher zwei AMQP Links für SAS-Token senden und Empfangen der Antwort (token Prüfergebnisses) aus dem Dienst.

Die AMQP-Nachricht enthält eine Reihe von Eigenschaften und mehr Informationen als ein einfaches. Das SAS-Token ist der Hauptteil der Nachricht (mit seinem Konstruktor). Die Eigenschaft **"ReplyTo"** soll den Knotennamen für das Ergebnis der Validierung auf Empfänger Link (Sie können den Namen ändern, wenn er dynamisch vom Dienst erstellt und sollen) empfangen. Die letzten drei Anwendung-benutzerdefinierte Eigenschaften wird vom Dienst an, welche Operation ausgeführt werden muss. Wie die CBS-Spezifikation beschrieben, muss sie **Vorgangsname** ("Put-Token"), die **Art von Token** (in diesem Fall "Servicebus.Windows.NET:sastoken"") und der **"Name"des Publikums** , das Token (die gesamte Entität) gilt.

Nach dem Senden des SAS-Tokens auf den Link Absender, muss Verleger die Antwort auf den Link Empfänger lesen. Die Antwort ist eine einfache AMQP-Nachricht mit eine Eigenschaft mit dem Namen **"Statuscode"** , die die gleichen Werte wie ein HTTP-Statuscode enthalten kann. 

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Service Bus REST-API-Referenz](https://msdn.microsoft.com/library/azure/hh780717.aspx) Informationen mit SAS-Token möglich.

Weitere Informationen über Service Bus-Authentifizierung finden Sie unter [Service Bus Authentifizierung und Autorisierung](service-bus-authentication-and-authorization.md). 

In [diesem Blogbeitrag](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx)sind weitere Beispiele für SAS in C# und Java Script.

[Azure-portal]: https://portal.azure.com
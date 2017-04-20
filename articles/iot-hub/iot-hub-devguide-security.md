<properties
 pageTitle="Entwicklerhandbuch - Steuern des Zugriffs auf IoT Hub | Microsoft Azure"
 description="Azure IoT Hub Developer guide - zum Steuern des Zugriffs auf IoT Hub und Sicherheit"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>Steuern des Zugriffs auf IoT Hub

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt die Optionen zum Absichern des IoT Hubs. IoT Hub verwendet *Berechtigungen* gewähren Zugriff auf jede IoT Hub Endpunkte. Berechtigungen beschränken den Zugriff auf einen IoT Hub basierend auf Funktionen.

Dieser Artikel beschreibt:

- Die unterschiedlichen Berechtigungen, Gerät oder Back-End-Anwendung auf Ihrem IoT Hub gewähren können.
- Der Authentifizierungsprozess und Token verwendet, um Berechtigungen zu überprüfen.
- Zum Bereich Anmeldeinformationen Zugriff auf bestimmte Ressourcen beschränkt.
- IoT Hub unterstützt x. 509-Zertifikate.
- Benutzerdefiniertes Gerät Authentifizierungsmechanismen, die vorhandenen Gerät Identität Register oder Authentifizierungsschemas.

### <a name="when-to-use"></a>Verwendung

Sie müssen die erforderlichen Berechtigungen für den Zugriff auf die Endpunkte IoT Hub. Ein Gerät muss beispielsweise einen Token mit Anmeldeinformationen zusammen mit jeder Nachricht, die an IoT Hub.

## <a name="access-control-and-permissions"></a>Zugriffskontrolle und Berechtigungen

[Sie können Berechtigungen auf folgende Weise:](#iot-hub-permissions)

* **Hub-Ebene gemeinsame Richtlinien**. Freigegebene Richtlinien können eine beliebige Kombination von [Berechtigungen](#iot-hub-permissions)erteilen. Sie können Richtlinien in [Azure-Portal][lnk-management-portal], oder programmgesteuert mithilfe des [Ressourcenproviders IoT Hub REST-APIs][lnk-resource-provider-apis]. Ein neu erstellte IoT Hub hat die folgenden Standardrichtlinien:

    - **Iothubowner**: mit allen Berechtigungen.
    - **Service**: mit ServiceConnect-Berechtigung.
    - **Gerät**: mit DeviceConnect-Berechtigung.
    - **RegistryRead**: mit RegistryRead-Berechtigung.
    - **RegistryReadWrite**: mit Berechtigungen RegistryRead und RegistryWrite.


* **Anmeldeinformationen pro Gerät**. Jede IoT Hub enthält eine [geräteidentitätsregistrierung][lnk-identity-registry]. Für jedes Gerät in dieser Registrierung können Sie Anmeldeinformationen, die Berechtigungen **DeviceConnect** auf das entsprechende Gerät Endpunkte beschränkt.

In einer normalen IoT-Lösung:

- Geräte-Management-Komponente verwendet die *RegistryReadWrite* -Richtlinie.
- Die Prozessorkomponente Ereignis verwendet die *Service* -Richtlinie.
- Die Runtime Geräts Business Datenzugriffslogik-Komponente verwendet die *Service* -Richtlinie.
- Einzelne Geräte Anmeldeinformationen in IoT Hub identitätsregistrierung

## <a name="authentication"></a>Authentifizierung

Azure IoT Hub gewährt Endpunkte einen Token freigegebenen Richtlinien und Gerät Identität Registrierung Anmeldeinformationen überprüfen.

Anmeldeinformationen wie symmetrische Schlüssel werden niemals über das Netzwerk gesendet.

> [AZURE.NOTE] Der Ressourcenanbieter Azure IoT Hub Azure Abonnements gesichert, wie alle Anbieter in der [Azure-Ressourcen-Manager][lnk-azure-resource-manager].

Weitere Informationen zum Erstellen und Verwenden von Sicherheitstokens finden Sie unter [IoT Hub Sicherheitstoken][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokoll-Besonderheiten

Jedes unterstütztes Protokoll wie MQTT, AMQP und HTTP transportiert Token unterschiedlich.

Bei MQTT hat das Paket verbinden DeviceId ClientId {Iothubhostname} / {DeviceId} in das Feld Benutzername und ein SAS-Token im Feld Kennwort. {Iothubhostname} sollte die vollständige CName IoT Hub (z. B. contoso.azure-devices.net).

Wenn [AMQP]mit[lnk-amqp], IoT Hub unterstützt [Nur SASL-] [ lnk-sasl-plain] und [AMQP Ansprüche-basierten Sicherheit][lnk-cbs].

Verwenden Sie AMQP Ansprüche-basierten Sicherheit, gibt an, wie diese Token übertragen der Standard.

SASL-Ebene kann der **Benutzername** :

* `{policyName}@sas.root.{iothubName}`Wenn Token Hub auf.
* `{deviceId}@sas.{iothubname}`Wenn Token Gerät begrenzt.

In beiden Fällen das Kennwortfeld enthält das Token [IoT Hub Sicherheitstoken][lnk-sas-tokens].

HTTP-Authentifizierung implementiert durch ein gültiges Token im Anforderungsheader **Autorisierung** einschließen.

#### <a name="example"></a>Beispiel

Benutzername (DeviceId beachtet werden):`iothubname.azure-devices.net/DeviceId`

Kennwort (SAS mit Gerät Explorer generiert):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] [Azure IoT Hub SDKs] [ lnk-sdks] Token beim Verbinden mit dem Dienst automatisch generieren. In einigen Fällen unterstützen die SDKs nicht alle Protokolle oder alle Authentifizierungsmethoden.

### <a name="special-considerations-for-sasl-plain"></a>Besondere Hinweise für SASL-Ebene

Bei SASL nur mit AMQP können mit einem IoT Hub ein Token für jede TCP-Verbindung. Ablauf des Tokens die TCP-Verbindung trennt vom Dienst und löst eine Verbindung. Dieses Verhalten ist während einer Back-End-Komponente, nicht problematisch sehr schädlich für eine Anwendung auf dem Gerät aus den folgenden Gründen:

*  Gateways werden normalerweise für viele Geräte verbinden. Wenn SASL nur verwenden, müssen sie eine unterschiedliche TCP-Verbindung für jedes Gerät Verbindung IoT Hub zu erstellen. Dieses Szenario erheblich erhöht den Verbrauch und Netzwerkressourcen und die Wartezeit für jede Verbindung.
* Ressourcenbeschränkte Geräte werden durch höhere Nutzung der Ressourcen Verbindung nach jeder Tokengültigkeitsdauer beeinträchtigt.

## <a name="scope-hub-level-credentials"></a>Bereich-Hub-Anmeldedaten

Hub auf Sicherheitsrichtlinien können Sie festlegen, indem Sie Token mit eingeschränkten Ressourcen-URI. Beispielsweise ist der Endpunkt Gerät Cloud-Nachrichten von einem Gerät **/devices/ {DeviceId} Nachrichten/Ereignisse**. Sie können auch einen Hub auf freigegebenen Richtlinien mit **DeviceConnect** Berechtigungen einen Token signiert, deren ResourceURI **/devices/ {DeviceId}**ist. Dieser Ansatz schafft ein Token, das nur zum Senden von Nachrichten im Namen Gerät **DeviceId**verwendet werden kann.

Dieser Mechanismus ist die [Herausgeberrichtlinie Ereignis Hubs]mit[lnk-event-hubs-publisher-policy], und Sie können benutzerdefinierte Authentifizierung implementieren.

## <a name="security-tokens"></a>Sicherheitstokens

IoT Hub verwendet Sicherheitstoken um Geräte und Dienste zu vermeiden Schlüssel bei der Übertragung zu authentifizieren. Darüber hinaus sind Sicherheitstoken Gültigkeitszeitraum und Bereich beschränkt. [Azure IoT Hub SDKs] [ lnk-sdks] Token ohne besondere Konfiguration automatisch generieren. Einige Szenarien erfordern jedoch den Benutzer generieren und Sicherheitstoken direkt verwenden. Diese umfassen die direkte Verwendung der MQTT, AMQP oder HTTP Flächen oder Implementierung des Musters Sicherheitstokendienst Siehe [benutzerdefinierte Geräteauthentifizierung][lnk-custom-auth].

IoT Hub können auch Geräte IoT Hub Authentifizierung mithilfe von [x. 509-Zertifikaten][lnk-x509]. 

### <a name="security-token-structure"></a>Token Sicherheitsstruktur
Sicherheitstokens werden Zeit begrenzten Zugriff auf Geräte und Dienste auf bestimmte Funktionen in IoT Hub verwenden. Um sicherzustellen, dass nur autorisierte Geräte und Dienste eine Verbindung herstellen können, müssen mit einem gemeinsamen Zugriff Richtlinienschlüssel oder einen symmetrischen Schlüssel mit einem Gerät in der identitätsregistrierung gespeichert Sicherheitstoken angemeldet sein.

Ein Token signiert mit einer gemeinsamen Zugriff Richtlinie Schlüssel gewährt Zugriff auf alle Funktionen, die die freigegebene Richtlinienberechtigungen zugeordnet. Auf der anderen Seite gewährt ein Token mit einem geräteidentität symmetrischen Schlüssel signiert nur die **DeviceConnect** Identität des zugeordneten Geräts.

Das Sicherheitstoken hat das folgende Format:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Die erwarteten Werte sind:

| Wert | Beschreibung |
| ----- | ----------- |
| {Signatur} | Ein HMAC-SHA256 Signaturzeichenfolge der Form: `{URL-encoded-resourceURI} + "\n" + expiry`. **Wichtig**: der Schlüssel von base64 decodiert und HMAC-SHA256 Berechnung ausführen als Schlüssel verwendet. |
| {ResourceURI} | URI-Präfix (nach Segment) der Endpunkte, die mit diesem Token mit Hostnamen IoT Hub (kein Protokoll) zugegriffen werden kann. Zum Beispiel`myHub.azure-devices.net/devices/device1` |
| {Ablauf} | UTF8-Zeichenfolgen für die Anzahl der Sekunden seit der Epoche 00:00:00 UTC am 1. Januar 1970. |
| {URL-codiert-ResourceURI} | Niedriger Fall URL-Codierung von Kleinbuchstaben Ressourcen-URI |
| {PolicyName} | Der Name der Richtlinie gemeinsamen Zugriff auf dieses Token. Bei Token auf Registrierung Geräts Anmeldeinformationen fehlen. |

**Hinweis auf Präfix**: das URI-Präfix wird berechnet, Segment und keine Zeichen. Beispielsweise `/a/b` ist ein Präfix für `/a/b/c` , jedoch nicht für `/a/bc`.

Im folgenden Node.js-Codeausschnitt wird eine Funktion namens **GenerateSasToken** , die das Token aus den Eingaben berechnet `resourceUri, signingKey, policyName, expiresInMins`. In den nächsten Abschnitten erläutert verschiedene Eingänge für unterschiedliche token Anwendungsfälle zu initialisieren.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Zum Vergleich wird der entsprechende Python Code zum Generieren eines Sicherheitstokens:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Da die zeitliche Gültigkeit des Tokens auf IoT Hub überprüft, unbedingt Drift auf der Uhr des Computers, der das Token generiert sein.

### <a name="use-sas-tokens-in-a-device-client"></a>SAS-Token in einem Gerät Client verwenden

Zweierlei **DeviceConnect** Berechtigungen mit IoT Hub mit Sicherheitstoken zu: [symmetrische Geräteschlüssel Gerät Identität Registrierung](#use-a-symmetric-key-in-the-identity-registry)oder [gemeinsamen Zugriff Richtlinienschlüssel](#use-a-shared-access-policy).

Beachten Sie, dass alle Funktionen von Geräten standardmäßig auf Endpunkten mit Präfix ausgesetzt ist `/devices/{deviceId}`.

> [AZURE.IMPORTANT] Die einzige Möglichkeit, IoT Hub ein bestimmtes Gerät authentifiziert verwendet Gerät Identität symmetrischen Schlüssel. Bei freigegebenen Zugriffsrichtlinie Gerät Funktionen zugreifen wird die Lösung die Ausstellung von Sicherheitstoken als vertrauenswürdige Unterkomponente Komponente berücksichtigen.

Gerät verbundenen Endpunkte sind (unabhängig vom Protokoll):

| Endpunkt | Funktionen |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Senden Sie Gerät-zu-Cloud-Nachrichten. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Empfangen Sie Cloud-Gerät. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Verwenden Sie einen symmetrischen Schlüssel in der identitätsregistrierung

Wenn Sie ein geräteidentität symmetrischen Schlüssel Generierung eines Tokens der Richtlinienname (`skn`) Element des Tokens nicht angegeben.

Beispielsweise müsste ein Token erstellt, um Zugriff auf alle Geräte-Funktionen die folgenden Parameter:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* Signaturschlüssel: symmetrischen Schlüssel für die `{device id}` Identität
* kein Richtlinienname
* Ablaufzeit.

Ein Beispiel zur Verwendung der Funktion Knoten wäre:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Gewährt Zugriff auf alle Funktionen für Gerät1 das Ergebnis wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Es ist möglich, eine sichere Token mithilfe der .NET Framework-Konfigurationstool [Gerät Explorer]generiert[lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Verwenden Sie einen freigegebenen Richtlinien

Beim Erstellen eines Tokens aus freigegebenen Zugriffsrichtlinie Benennen der Feld `skn` muss auf den Namen der verwendeten Richtlinie festgelegt werden. Es ist auch erforderlich, die Richtlinie **DeviceConnect** Berechtigungen erteilt.

Die beiden wichtigsten Szenarien für die Verwendung von freigegebenen Richtlinien auf Funktionalität sind:

* [Protokoll-Gateways Cloud][lnk-endpoints],
* [Token Services] [ lnk-custom-auth] verwendet, um benutzerdefinierte Authentifizierungsschemas zu implementieren.

Da die freigegebene Richtlinie möglicherweise Zugriff wie andere Geräte verbinden kann, ist es wichtig, die richtige Ressource URI beim Erstellen von Sicherheitstokens verwendet. Dies ist besonders wichtig für token-Services, die das Token an ein bestimmtes Gerät mithilfe den Ressourcen-URI Bereich. Dieser Punkt ist weniger wichtig für Protokoll-Gateways, sie Datenverkehr für alle Geräte bereits vermitteln.

Beispielsweise würde ein token Service mithilfe der zuvor erstellten freigegebenen Richtlinie mit der Bezeichnung **Gerät** einen Token mit den folgenden Parametern erstellen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* Signaturschlüssel: eines der von der `device` Richtlinie
* Richtlinienname: `device`,
* Ablaufzeit.

Ein Beispiel zur Verwendung der Funktion Knoten wäre:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Gewährt Zugriff auf alle Funktionen für Gerät1 das Ergebnis wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Protokollgateway können auch für alle Geräte einfach festlegen den Ressourcen-URI, `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Sicherheitstokens von Komponenten verwenden

Komponenten erzeugen nur Sicherheitstoken mit freigegebenen Richtlinien die entsprechenden Berechtigungen erteilen, wie zuvor beschrieben.

Servicefunktionen Endpunkte verfügbar sind:

| Endpunkt | Funktionen |
| ----- | ----------- |
| `{iot hub host name}/devices` | Erstellen Sie, aktualisieren Sie, rufen Sie ab und löschen Sie Gerät Identitäten. |
| `{iot hub host name}/messages/events` | Erhalten Sie Gerät-zu-Cloud-Nachrichten. |
| `{iot hub host name}/servicebound/feedback` | Empfangen Sie Feedback für Cloud-zu-Gerät-Nachrichten. |
| `{iot hub host name}/devicebound` | Senden Sie Cloud-zu-Gerät-Nachrichten. |

Beispielsweise würde ein Dienst generiert mithilfe der zuvor erstellten freigegebenen Richtlinie mit der Bezeichnung **RegistryRead** Erstellen eines Tokens mit folgenden Parametern:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices`,
* Signaturschlüssel: eines der von der `registryRead` Richtlinie
* Richtlinienname: `registryRead`,
* Ablaufzeit.

    Var Endpunkt = "myhub.azure-devices.net/devices";   PolicyName Var = 'Gerät';   Var PolicyKey = '...'.

    Var Token = GenerateSasToken (Endpunkt, PolicyKey, PolicyName 60)

Das Ergebnis, das alle Geräte Identitäten Lesezugriff erteilen würde, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Unterstützt x. 509-Zertifikate

Ein x. 509-Zertifikat können Sie ein Gerät mit IoT Hub authentifizieren. Dazu gehören:

-   **Vorhandenes x. 509-Zertifikat**. Ein Gerät möglicherweise bereits ein x. 509-Zertifikat zugeordnet. Dieses Zertifikat können das Gerät mit IoT Hub authentifizieren.

-   **Einem X-509 selbst und selbstsignierte Zertifikat**. Ein Hersteller oder interne Bereitsteller kann diese Zertifikate und die entsprechenden privaten Schlüssel (und Zertifikat) auf dem Gerät speichern. Verwenden Sie Tools wie [OpenSSL] [ lnk-openssl] und [Windows SelfSignedCertificate] [ lnk-selfsigned] -Programm für diesen Zweck.

-   **CA-signierten x. 509-Zertifikat**. Sie können auch ein x. 509-Zertifikat erstellt und von einer Zertifizierungsstelle (CA) signiert ein Gerät und ein Gerät mit IoT Hub authentifizieren.

Ein Gerät kann entweder ein x. 509-Zertifikat oder ein Sicherheitstoken Authentifizierung, jedoch nicht beide verwenden.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Ein x. 509-Clientzertifikat ein registrieren

[Azure IoT Service SDK für C#] [ lnk-service-sdk] (Version 1.0.8+) unterstützt registrieren ein Gerät ein x. 509-Clientzertifikat für die Authentifizierung verwendet. Andere APIs wie Import und Export von Geräte unterstützen auch x. 509-Clientzertifikate.

### <a name="c-support"></a>C\# Support

Die **RegistryManager** -Klasse stellt eine programmgesteuerte Möglichkeit, ein Gerät zu registrieren. Insbesondere können die Methoden **AddDeviceAsync** und **UpdateDeviceAsync** Benutzer registrieren und Aktualisieren eines Geräts in Iot Hub geräteidentitätsregistrierung. Diese beiden Methoden akzeptieren als Eingabe **eine Geräteinstanz** . **Die Klasse** enthält **eine Eigenschaft, die primären und sekundären x. 509-Zertifikat Fingerabdrücke angeben kann** . Der Fingerabdruck ist SHA-1-Hash des x. 509-Zertifikats (mit DER binären Codierung gespeichert). Benutzer können primäre Fingerabdruck oder sekundäre Fingerabdruck oder beides angeben. Primäre und sekundäre Fingerabdrücke sind um Zertifikat Rollover-Szenarios unterstützt.

> [AZURE.NOTE] IoT Hub ist nicht erforderlich oder das gesamte x. 509-Clientzertifikat des Fingerabdrucks gespeichert.

Beispiel C\# ein Gerät mit einem x. 509-Clientzertifikat Codeausschnitt:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Verwenden Sie ein x. 509-Clientzertifikat bei Laufzeit

Das [Gerät IoT Azure SDK für .NET] [ lnk-client-sdk] (Version 1.0.11+) unterstützt die Verwendung von x. 509-Clientzertifikate.

### <a name="c-support"></a>C\# Support

Die Klasse **DeviceAuthenticationWithX509Certificate** unterstützt die Erstellung von  **DeviceClient** Instanzen mit einem x. 509-Clientzertifikat.

Hier ist ein Beispielcode-Ausschnitt:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Benutzerdefinierte Geräteauthentifizierung

IoT Hub [geräteidentitätsregistrierung] können[ lnk-identity-registry] Anmeldeinformationen pro Gerät konfigurieren und Zugriffskontrolle mit [Token][lnk-sas-tokens]. Allerdings weist IoT-Lösung bereits eine erhebliche Investition in benutzerdefiniertes Gerät Identität Registrierung und Authentifizierung ein, integrieren diese Infrastruktur mit IoT Hub Sie erstellen einen *Sicherheitstokendienst*. Auf diese Weise können Sie andere IoT-Funktionen in der Projektmappe.

Ein Sicherheitstokendienst ist eine benutzerdefinierte Cloud-Dienst. Verwendet eine IoT Hub *freigegebenen Richtlinien* mit **DeviceConnect** Berechtigungen *Gerät begrenzt* Token erstellt. Diese Token ermöglichen ein Verbindung IoT Hub.

  ![Schritte des Sicherheitstokendienst-Muster][img-tokenservice]

Dies sind die Hauptschritte Sicherheitstokendienst Muster:

1. Erstellen Sie eine Zugriffsrichtlinie IoT Hub freigegeben mit **DeviceConnect** Berechtigungen für IoT Hub. Erstellen dieser Richtlinie in der [Azure-Portal] [ lnk-management-portal] oder programmgesteuert. Der Sicherheitstokendienst verwendet diese Richtlinie sich Token erstellt.
2. Wenn ein Gerät auf Ihrem IoT Hub muss, fordert er ein signiertes Token der Sicherheitstokendienst. Das Gerät kann mit Ihr benutzerdefiniertes Gerät Identität Registrierung-Authentifizierung Identität des Geräts bestimmen, die den Sicherheitstokendienst verwendet das Token zu, authentifizieren.
3. Der Sicherheitstokendienst gibt ein Token zurück. Erstellen des Tokens mit `/devices/{deviceId}` als `resourceURI`, mit `deviceId` als das Gerät authentifiziert. Der Sicherheitstokendienst verwendet die freigegebene Richtlinie Token erstellt.
4. Das Gerät verwendet das Token direkt mit IoT Hub.

> [AZURE.NOTE] Verwenden Sie die Klasse .NET [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] oder die Java-Klasse [IotHubServiceSasToken] [ lnk-java-sas] Erstellen eines Tokens in der Sicherheitstokendienst.

Der Sicherheitstokendienst kann die Tokengültigkeitsdauer wie gewünscht festzulegen. Wenn das Token abläuft, trennt IoT Hub die Verbindung zum Gerät. Das Gerät muss dann ein neues Token von token Service anfordern. Verwenden Sie kurze Ablaufzeit, erhöht dies die Last auf dem Gerät und dem Sicherheitstokendienst.

Ein Haupt-Verbindung muss noch hinzugefügt IoT Hub Gerät Identität Registrierung – auch wenn das Gerät ein Token und nicht Geräteschlüssel verwendet die Verbindung. Sie können daher pro Gerät Zugriffskontrolle aktivieren und Deaktivieren von Identitäten in der [IoT Hub identitätsregistrierung] Gerät verwenden[ lnk-identity-registry] Wenn das Gerät mit einem Token authentifiziert. Dies senkt das Risiko Token mit langen Laufzeiten.

### <a name="comparison-with-a-custom-gateway"></a>Vergleich mit einem benutzerdefinierten gateway

Sicherheitstokendienst-Muster ist die empfohlene Methode zum Implementieren einer benutzerdefinierten Identität Registrierung/Authentifizierungsschema mit IoT Hub. Es wird empfohlen, denn IoT Hub Lösung Verkehr verarbeiten. Es gibt jedoch Fälle, in denen das benutzerdefinierte Authentifizierungsschema so mit dem Protokoll verknüpft ist, dass eine Verarbeitung des Datenverkehrs (*benutzerdefinierten Gateways*) erforderlich ist. Ein Beispiel hierfür ist [Transport Layer Security (TLS) und vorinstallierte Schlüssel (PSK)][lnk-tls-psk]. Weitere Informationen finden Sie unter [Protokollgateway] [ lnk-protocols] Thema.

## <a name="reference-topics"></a>Themen:

Die folgenden Themen bieten weitere Informationen zum Steuern des Zugriffs auf Ihre IoT Hub.

## <a name="iot-hub-permissions"></a>IoT Hub Berechtigungen

Die folgende Tabelle listet die Berechtigungen, die zum Steuern des Zugriffs auf Ihre IoT Hub verwenden können.

| Berechtigung            | Notizen |
| --------------------- | ----- |
| **RegistryRead**      | Zuschüsse Lesezugriff auf die Registrierung des Geräts Identität. Weitere Informationen finden Sie unter [geräteidentitätsregistrierung][lnk-identity-registry]. |
| **RegistryReadWrite** | Gewährt Lese- und Schreibzugriff auf die Registrierung des Geräts Identität. Weitere Informationen finden Sie unter [geräteidentitätsregistrierung][lnk-identity-registry]. |
| **ServiceConnect**    | Gewährt Zugriff auf cloud-Dienst-Kommunikation und Überwachung von Endpunkten. Z. B. erteilt die Berechtigung zum Backend-Clouddienste Gerät Cloud empfangen, Cloud-Gerät senden und die entsprechende Lieferung Danksagungen abrufen. |
| **DeviceConnect**     | Erteilt Zugriff auf verbundene Gerät Kommunikationsendpunkte. Z. B. erteilt die Berechtigung zum Gerät Cloud-Nachrichten senden und Empfangen von Nachrichten Cloud-Gerät. Diese Berechtigung wird von Geräten verwendet. |

## <a name="additional-reference-material"></a>Zusätzliches Referenzmaterial

Andere Referenzthemen in Developer Guide enthalten:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschreibt die verschiedenen Endpunkte jedes IoT Hub Runtime und Management macht.
- [Kontingente und Drosselung] [ lnk-quotas] beschreibt die Quoten für den Dienst IoT Hub und Drosselung Verhalten gelten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub-Gerät und SDKs] [ lnk-sdks] Listet die verschiedenen SDKs Sie können Wenn Sie Anwendungsentwicklung Geräte und Dienste, die interagieren mit IoT Hub.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] beschreibt die Abfragesprache IoT Hub über Geräte im Vergleich, Methoden und Aufträge abrufen können.
- [IoT Hub MQTT Unterstützung] [ lnk-devguide-mqtt] MQTT Protokoll IoT Hub unterstützt Weitere Informationen bereit.

## <a name="next-steps"></a>Nächste Schritte

Jetzt für die Zugriffskontrolle IoT Hub gelernt haben, können Sie in den folgenden Themen Entwicklerhandbuch interessiert sein:

- [Gerät im Vergleich mit dem Zustand und Konfigurationen synchronisiert][lnk-devguide-device-twins]
- [Eine direkte Methode auf einem Gerät][lnk-devguide-directmethods]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Möchten Sie einige der in diesem Artikel beschriebenen Konzepte zu testen, können Sie die folgenden Lernprogramme IoT Hub interessiert.

- [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]
- [Wie Cloud-Gerät mit IoT Hub senden][lnk-c2d-tutorial]
- [IoT Hub Gerät Cloud-Nachrichten verarbeitet][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

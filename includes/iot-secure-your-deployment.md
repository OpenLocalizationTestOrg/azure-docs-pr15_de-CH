# <a name="securing-your-iot-deployment"></a>Sichern Ihrer Bereitstellung IoT

Dieser Artikel enthält die nächste Detailebene NBI Azure IoT-basierte Internet der Dinge (IoT). Auf Ebene Implementierungsdetails für Konfiguration und Bereitstellung von jeder Komponente verweist. Darüber hinaus Vergleiche und die Wahl zwischen verschiedenen konkurrierenden.

Sichern der Bereitstellung Azure IoT kann in die folgenden drei Bereiche unterteilt:

- **Sicherheit**: schützen IoT-Gerät im Umlauf bereitgestellt.
- **Connection Security**: alle Daten zwischen dem IoT Gerät und IoT Hub ist vertraulich und fälschungssicher.
- **Cloud-Sicherheit**: Möglichkeit, Daten zu schützen, durchläuft, und in der Cloud gespeichert.

![Drei Sicherheitsbereiche][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Sicheres gerätebereitstellung und Authentifizierung

Azure IoT Suite schützt IoT-Geräte mit den folgenden zwei Methoden:

- Anhand eines eindeutigen Identitätsschlüssels (Security Token) für jedes Gerät mit IoT Hub Kommunikation vom Gerät verwendet werden kann.

- Ein auf dem Gerät [x. 509-Zertifikat] mit[ lnk-x509] und den privaten Schlüssel zu Gerät IoT Hub authentifizieren. Diese Authentifizierungsmethode wird sichergestellt, dass der private Schlüssel auf dem Gerät nicht außerhalb des Geräts jederzeit bekannt ist eine höhere Sicherheit bietet.

Die token Methode bietet Authentifizierung für jeden Anruf, vom Gerät IoT Hub durch Zuordnen des symmetrischen Schlüssels für die einzelnen Aufrufe. X. 509-Authentifizierung ermöglicht die Authentifizierung eines Geräts IoT auf der physischen Ebene als Teil der TLS-Verbindungsaufbau. Security-Token-basierten Methode kann ohne x. 509-Authentifizierung ist ein sicherer Muster verwendet werden. Die Wahl zwischen zwei Methoden richtet sich hauptsächlich wie sicher Gerät Authentifizierung muss und Verfügbarkeit von sicheren Speicher auf dem Gerät (den privaten Schlüssel sicher zu speichern).

## <a name="iot-hub-security-tokens"></a>Sicherheitstokens IoT Hub

IoT Hub verwendet Sicherheitstoken um Geräte und Dienste zu vermeiden Schlüssel im Netzwerk authentifizieren. Darüber hinaus sind Sicherheitstoken Gültigkeitszeitraum und Bereich beschränkt. Automatisch generieren Azure IoT Hub SDKs Token ohne besondere Konfiguration. Einige Szenarien erfordern jedoch den Benutzer generieren und Sicherheitstoken direkt verwenden. Diese umfassen die direkte Verwendung der MQTT, AMQP oder HTTP Flächen oder die Implementierung des Musters Sicherheitstokendienst.

Weitere Informationen zur Struktur der Sicherheitstoken und ihrer Verwendung finden in den folgenden Artikeln:

-   [Token Sicherheitsstruktur][lnk-security-tokens]
-   [SAS-Token verwenden als ein Gerät][lnk-sas-tokens]

Jede IoT Hub ist ein [Geräteidentitätsregistrierung] [ lnk-identity-registry] pro Gerät Ressourcen in den Dienst wie eine Warteschlange erstellen Flight Cloud-zu-Gerät-Nachrichten und Zugriff auf Gerät verbundenen Endpunkte verwendet werden können. IoT Hub identitätsregistrierung bietet sicheren Speicher Gerät Identitäten und Sicherheitsschlüssel für eine Lösung. Einzelne oder Gruppen Gerät Identitäten können eine Liste oder eine Sperrliste hinzugefügt werden vollständige Kontrolle über Zugriff auf. Die folgenden Artikeln finden Sie weitere Informationen zur Struktur der Registrierung des Geräts Identität und Operationen unterstützt.

[IoT Hub unterstützt Protokolle wie HTTP, MQTT und AMQP][lnk-protocols]. Jedes dieser Protokolle verwenden Sicherheitstoken vom Gerät IoT IoT Hub unterschiedlich:

- AMQP: SASL und AMQP anspruchsbasierte Sicherheit ({policyName}@sas.root.{iothubName} bei Token-Hub auf; {DeviceId} bei Token Gerät begrenzt).

- MQTT: Verbinden Paket verwendet {DeviceId} {ClientId} {IoThubhostname} / {DeviceId} in das Feld **Benutzername** und ein SAS-Token im Feld **Kennwort** .

- HTTP: Gültiges Token im Anforderungsheader Autorisierung ist.

IoT Hub-Geräteidentitätsregistrierung kann Anmeldeinformationen pro Gerät konfigurieren und Zugriffskontrolle verwendet werden. Wenn eine Lösung IoT eine erhebliche Investition in ein [benutzerdefiniertes Gerät Identität Registrierung und Authentifizierung Schema bereits][lnk-custom-auth], in eine vorhandene Infrastruktur mit IoT Hub erstellen einen Sicherheitstokendienst integriert werden.

### <a name="x509-certificate-based-device-authentication"></a>Geräteauthentifizierung mit x. 509-Zertifikat

Die Verwendung eines [x. 509-Zertifikat gerätebasierte] [ lnk-use-x509] und seine zugeordneten privaten und öffentlichen Schlüsselpaar ermöglicht zusätzliche Authentifizierung auf der physischen Ebene. Der private Schlüssel werden sicher auf dem Gerät gespeichert und ist nicht sichtbar außerhalb des Geräts. X. 509-Zertifikat enthält Informationen über das Gerät wie Geräte-ID und Organisationseinheiten. Signatur des Zertifikats wird mit dem privaten Schlüssel generiert.

Allgemeine Geräte Bereitstellung Flow:

- Ordnen Sie einen Bezeichner, der einem physischen Gerät – geräteidentität oder x. 509-Zertifikat auf dem Gerät während der Produktion oder Inbetriebnahme Gerät zu

- Erstellen Sie entsprechende Identität Eintrag IoT Hub-geräteidentität und zugeordneten Informationen in der Registrierung IoT Hub-Gerät

- Speichern Sie x. 509-Zertifikatfingerabdruck in IoT Hub geräteregistrierung sicher.

### <a name="root-certificate-on-device"></a>Stammzertifikat auf dem Gerät

Beim Einrichten einer sicheren TLS-Verbindungs IoT Hub, authentifiziert IoT Gerät IoT Hub mit einem Stammzertifikat, das Gerät SDK gehört. Für den C-Client SDK befindet sich das Zertifikat im Ordner "\\c\\Zertifikate" Repo-Stammverzeichnis. Obwohl diese Stammzertifikate langlebig sind diese weiterhin können ablaufen oder gesperrt werden. Besteht keine Möglichkeit, das Zertifikat auf dem Gerät aktualisieren, das Gerät die Verbindung anschließend IoT Hub (oder andere Cloud-Dienst) möglicherweise nicht. Bedeutet das Stammzertifikat aktualisiert, sobald IoT-Gerät bereitgestellt wird diese Risiken effektiv.

## <a name="securing-the-connection"></a>Absicherung der Kommunikation

Internet-Verbindung IoT Gerät und IoT Hub ist gesichert mit dem Transport Layer Security (TLS)-Standard. Azure IoT unterstützt [TLS 1.2][lnk-tls12], TLS 1.1 und TLS 1.0 in dieser Reihenfolge. TLS 1.0 wird nur für Abwärtskompatibilität unterstützt. Es wird empfohlen, TLS 1.2 verwenden, da Sicherheit bietet.

Azure IoT-Suite unterstützt diese folgenden Verschlüsselungsverfahren.

| Chiffre-Suite | Länge |
|--------------|--------|
| TLS\_ECDHE\_RSA\_mit\_AES\_256\_CBC\_SHA384 ECDH-secp384r1 (0xc028) (EQ. 7680-Bit RSA) FS | 256    |
| TLS\_ECDHE\_RSA\_mit\_AES\_128\_CBC\_SHA256 ECDH-secp256r1 (0xc027) (EQ. 3072-Bit RSA) FS | 128    |
| TLS\_ECDHE\_RSA\_mit\_AES\_256\_CBC\_(0xc014) SHA ECDH secp384r1 (EQ. 7680-Bit RSA) FS           | 256    |
| TLS\_ECDHE\_RSA\_mit\_AES\_128\_CBC\_(0xc013) SHA ECDH secp256r1 (EQ. 3072-Bit RSA) FS           | 128    |
| TLS\_RSA\_mit\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_mit\_AES\_128\_GCM\_SHA256 (0x9C MACHINE_CHECK_EXCEPTION) | 128    |
| TLS\_RSA\_mit\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_mit\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_mit\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_mit\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_mit\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Sichern der cloud

Azure IoT Hub ermöglicht die Definition von [Zugriffsrichtlinien] [ lnk-protocols] für jeden Sicherheitsschlüssel. Die folgende Gruppe von Berechtigungen verwendet gewähren Zugriff auf die einzelnen Endpunkte IoT Hub. Berechtigungen beschränken den Zugriff auf einen IoT Hub basierend auf Funktionen.

- **RegistryRead**. Die Registrierung des Geräts Identität Zugriff gewährt. Weitere Informationen finden Sie unter [Geräteidentitätsregistrierung][lnk-identity-registry].

- **RegistryReadWrite**. Gewährt Lese- und Schreibzugriff auf die Registrierung des Geräts Identität. Weitere Informationen finden Sie unter [Geräteidentitätsregistrierung][lnk-identity-registry].

- **ServiceConnect**. Gewährt Zugriff auf cloud-Dienst-Kommunikation und Überwachung von Endpunkten. Z. B. erteilt die Berechtigung zum Backend-Clouddienste Gerät Cloud empfangen, Cloud-Gerät senden und die entsprechende Lieferung Danksagungen abrufen.

- **DeviceConnect**. Erteilt Zugriff auf verbundene Gerät Kommunikationsendpunkte. Z. B. erteilt die Berechtigung zum Gerät Cloud-Nachrichten senden und Empfangen von Nachrichten Cloud-Gerät. Diese Berechtigung wird von Geräten verwendet.

Zweierlei **DeviceConnect** Berechtigungen mit IoT Hub mit [Sicherheitstoken]zu[lnk-sas-tokens]: mit einem Gerät Identitätsschlüssel oder ein Richtlinienschlüssel gemeinsamen Zugriff. Außerdem ist zu beachten, dass alle Funktionen von Geräten standardmäßig auf Endpunkten mit Präfix ausgesetzt ist `/devices/{deviceId}`.

[Komponenten können nur Sicherheitstoken] [ lnk-service-tokens] gemeinsam genutzter Richtlinien die entsprechenden Berechtigungen erteilen.

Azure IoT Hub und andere Dienste möglicherweise Teil der Lösung zulassen Verwaltung von Benutzern mithilfe von Azure Active Directory

Aufgenommen von Azure IoT Hub Daten können von verschiedenen Azure Stream Analytics und Azure BLOB-Speicher genutzt werden. Diese Dienste ermöglichen Management-Zugriff. Weitere Informationen über diese Dienste und die folgenden Optionen:

- [Azure DocumentDB][lnk-docdb]: eine skalierbare, vollständig indiziert Datenbankdienst halbstrukturierten Daten, die Metadaten für die Geräte verwaltet Sie bereitstellen, wie Attribute, Konfiguration und Sicherheitseigenschaften. DocumentDB bietet leistungsfähige und hohem Durchsatz Verarbeitung Schema agnostischen Indexierung von Daten und eine umfangreiche SQL-Abfrage-Oberfläche.

- [Azure Stream Analytics][lnk-asa]: Echtzeit-Stream Verarbeitung in der Cloud, mit dem Sie schnelle Entwicklung und Bereitstellung einer kostengünstigen Analytics Lösung Echtzeit Einblicke aus Geräten, Sensoren, Infrastruktur und Applikationen aufdecken. Daten aus diesem Dienst vollständig verwaltet skalierbar, keinem Volume noch erreichen hoher Durchsatz, geringe Latenz und Stabilität.

- [Azure App Services][lnk-appservices]: eine Cloud leistungsfähige Web und mobile apps, die eine zu Daten Verbindung; in der Cloud oder lokal. Erstellen einer Zusammenarbeit apps für iOS, Android und Windows. Die Software als Service (SaaS) Anwendung mit Out-of-the-Box Dutzende von Cloud-basierte Services und Anwendung integrieren. Code in Ihrer bevorzugten Sprache und IDE (.NET, NodeJS, PHP, Python oder Java) zu Web-apps und APIs schneller.

- [Logik Apps][lnk-logicapps]: die Logik Apps von Azure App Service hilft IoT-Lösung für Ihre vorhandene LOB Systeme integrieren und Workflow-Prozesse zu automatisieren. Apps ermöglicht es Entwicklern, Design-Workflows, die von einem Trigger starten und dann eine Reihe von Schritten ausführen, Regeln und Aktionen, die leistungsstarke Connectors verwenden, um Ihre Geschäftsprozesse integrieren. Logik Apps bietet Out-of-the-Box-Konnektivität zu einem riesigen Ökosystem SaaS Cloud-basierte und lokalen Anwendung.

- [Azure BLOB-Speicher][lnk-blob]: zuverlässige und ökonomische Cloud-Speicher für Daten, die Ihre Geräte in die Cloud zu senden.

## <a name="conclusion"></a>Abschluss

In diesem Artikel Überblick Implementierung auf Informationen zum Entwerfen und Bereitstellen einer IoT-Infrastruktur mithilfe von Azure IoT. Konfigurieren jede Komponente sicher ist die Gesamtinfrastruktur IoT sichern. Entwurf der Auswahlmöglichkeiten in Azure IoT bieten einige Flexibilität und Auswahl; Allerdings kann jede Auswahl Sicherheit auswirken. Es wird empfohlen, jede dieser Optionen durch eine Bewertung der Risiken und Kosten bewertet werden.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/

<properties 
    pageTitle="Split-Merge-Sicherheitskonfiguration | Microsoft Azure" 
    description="Einrichten von X409 Zertifikate für Verschlüsselung" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Split-Merge-Sicherheitskonfiguration  

Um Split-Merge-Dienst verwenden, müssen Sie Security ordnungsgemäß konfigurieren. Der Dienst ist Bestandteil der elastische Skalierung von Microsoft Azure SQL-Datenbank. Weitere Informationen finden Sie unter [flexible Skalierung Teilen und Zusammenführen praktischen Einführung](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Konfigurieren von Zertifikaten

Zertifikate werden auf zwei Arten konfiguriert. 

1. [So konfigurieren Sie das SSL-Zertifikat](#To-Configure-the-SSL#Certificate)
2. [So konfigurieren Sie Client-Zertifikate](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Beziehen von Zertifikaten

Zertifikate werden von öffentlichen Zertifizierungsstellen (CAs) oder von der [Windows-Zertifikatdienste](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx)erhalten. Dies sind die bevorzugten Methoden Zertifikate erhalten.

Wenn diese Optionen nicht verfügbar sind, können **selbstsignierte Zertifikate**generieren.
 
## <a name="tools-to-generate-certificates"></a>Tools zum Generieren von Zertifikaten

* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [Pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Die Werkzeuge

* Vom Entwickler Befehlszeile Visual Studios finden Sie unter [Visual Studio Command Prompt](http://msdn.microsoft.com/library/ms229859.aspx) 

    Wenn installiert ist, gehen Sie zu:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* Abrufen von WDK [Windows 8.1: Download Kits und Tools](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>So konfigurieren Sie das SSL-Zertifikat
Eine SSL-Zertifikat wird zum Verschlüsseln der Kommunikation und Authentifizieren des Servers erforderlich. Wählen Sie die am besten für die folgenden drei Szenarios und führt alle Schritte:

### <a name="create-a-new-self-signed-certificate"></a>Neues selbstsigniertes Zertifikat erstellen

1.    [Ein selbstsigniertes Zertifikat erstellen](#Create-a-Self-Signed-Certificate)
2.    [PFX-Datei für selbstsignierten SSL-Zertifikat erstellen](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [Hochladen Sie SSL-Zertifikat Service Cloud](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [SSL-Zertifikat im Dienst aktualisieren](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [SSL-Zertifizierungsstelle importieren](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Vorhandenes Zertifikat aus dem Zertifikatspeicher verwenden
1. [SSL-Zertifikat aus dem Zertifikatspeicher exportieren](#Export-SSL-Certificate-From-Certificate-Store)
2. [Hochladen Sie SSL-Zertifikat Service Cloud](#Upload-SSL-Certificate-to-Cloud-Service)
3. [SSL-Zertifikat im Dienst aktualisieren](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Verwenden Sie ein vorhandenes Zertifikat in eine PFX-Datei

1. [Hochladen Sie SSL-Zertifikat Service Cloud](#Upload-SSL-Certificate-to-Cloud-Service)
2. [SSL-Zertifikat im Dienst aktualisieren](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>So konfigurieren Sie Client-Zertifikate
Clientzertifikate erforderlich, den Dienst authentifizieren. Wählen Sie die am besten für die folgenden drei Szenarios und führt alle Schritte:

### <a name="turn-off-client-certificates"></a>Deaktivieren Sie Clientzertifikate
1.    [Zertifikatbasierte Clientauthentifizierung deaktivieren](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Neue Client selbstsignierte Zertifikate
1.    [Erstellen Sie eine selbstsigniertes Zertifizierungsstelle](#Create-a-Self-Signed-Certification-Authority)
2.    [Hochladen Sie Zertifizierungsstellenzertifikat Service Cloud](#Upload-CA-Certificate-to-Cloud-Service)
3.    [CA-Zertifikat in Dienst aktualisieren](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Clientzertifikate ausstellen](#Issue-Client-Certificates)
5.    [Erstellen von PFX-Dateien für Clientzertifikate](#Create-PFX-files-for-Client-Certificates)
6.    [Client-Zertifikat importieren](#Import-Client-Certificate)
7.    [Zertifikatfingerabdruck Client kopieren](#Copy-Client-Certificate-Thumbprints)
8.    [Die Dienstkonfigurationsdatei zulässigen Clients konfigurieren](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Verwenden Sie vorhandene Zertifikate
1.    [Zertifizierungsstelle öffentlichen Schlüssel](#Find-CA-Public Key)
2.    [Hochladen Sie Zertifizierungsstellenzertifikat Service Cloud](#Upload-CA-certificate-to-cloud-service)
3.    [CA-Zertifikat in Dienst aktualisieren](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Zertifikatfingerabdruck Client kopieren](#Copy-Client-Certificate-Thumbprints)
5.    [Die Dienstkonfigurationsdatei zulässigen Clients konfigurieren](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Client Certificate Revocation-Check konfigurieren](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Zugelassene IP-Adressen

Zugriff auf die Dienstendpunkte kann auf bestimmte Bereiche von IP-Adressen beschränkt.

## <a name="to-configure-encryption-for-the-store"></a>Konfigurieren der Verschlüsselung für Speicher

Die im Metadatenspeicher gespeicherten Anmeldeinformationen verschlüsselt ist ein Zertifikat erforderlich. Wählen Sie die am besten für die folgenden drei Szenarios und führt alle Schritte:

### <a name="use-a-new-self-signed-certificate"></a>Ein neues selbstsigniertes Zertifikat verwenden

1.     [Ein selbstsigniertes Zertifikat erstellen](#Create-a-Self-Signed-Certificate)
2.     [PFX-Datei für selbstsignierten Verschlüsselungszertifikat erstellen](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Hochladen Sie Verschlüsselungszertifikat Service Cloud](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Verschlüsselungszertifikat in Dienst aktualisieren](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Verwenden Sie ein vorhandenes Zertifikat aus dem Zertifikatspeicher

1.     [Verschlüsselungszertifikat aus dem Zertifikatspeicher exportieren](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Hochladen Sie Verschlüsselungszertifikat Service Cloud](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Verschlüsselungszertifikat in Dienst aktualisieren](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Verwenden Sie ein vorhandenes Zertifikat in eine PFX-Datei

1.     [Hochladen Sie Verschlüsselungszertifikat Service Cloud](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Verschlüsselungszertifikat in Dienst aktualisieren](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>Die Standardkonfiguration

Die Standardkonfiguration verweigert den Zugriff auf den HTTP-Endpunkt. Dies ist die empfohlene Einstellung, da Anfragen an diese Endpunkte vertrauliche Informationen wie Anmeldeinformationen ausführen können.
Die Standardkonfiguration ermöglicht Zugriff auf HTTPS-Endpunkt. Diese Einstellung kann weiter eingeschränkt werden.

### <a name="changing-the-configuration"></a>Ändern der Konfiguration

Gruppe von Zugriffssteuerungsregeln zugewiesen und Endpunkt konfiguriert sind die **<EndpointAcls>** Abschnitt der **Dienstkonfigurationsdatei**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Die Regeln in einer Zugriffsgruppe konfiguriert sind ein <AccessControl name=""> Abschnitt der Konfigurationsdatei Service. 

Das Format wird in Network Access Control Lists-Dokumentation erläutert.
Beispielsweise würde damit nur IP-Adressen im Bereich 100.100.0.0, 100.100.255.255 auf HTTPS-Endpunkt können Regeln folgendermaßen:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>DOS-Abwehr

Es gibt zwei unterschiedliche Mechanismen unterstützt erkennen und verhindern von Denial-of-Service-Angriffe:

*    Beschränken der Anzahl gleichzeitiger Abfragen pro Remotehost (standardmäßig deaktiviert)
*    Rate pro Remotehost einschränken (in der Standardeinstellung)

Diese weitere dynamische IP-Sicherheit in IIS dokumentierten Funktionen basieren. Wenn diese Konfiguration ändern Vorsicht folgenden Faktoren:

* Das Verhalten von Proxys und NAT Geräte über remote-Host-Informationen
* Jede Anforderung an eine Ressource in der Webrolle gilt (z.B. Laden von Skripts, Bilder usw.)

## <a name="restricting-number-of-concurrent-accesses"></a>Anzahl gleichzeitiger Zugriffe einschränken

Die Einstellungen, die dieses Verhalten sind:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Ändern Sie DynamicIpRestrictionDenyByConcurrentRequests auf True, um diesen Schutz aktivieren.

## <a name="restricting-rate-of-access"></a>Bei Zugriff einschränken

Die Einstellungen, die dieses Verhalten sind:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Die Antwort auf eine verweigerte Anforderung

Die folgende Einstellung konfiguriert die Antwort auf eine verweigerte Anforderung:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Lesen Sie die Dokumentation für dynamische IP-Sicherheit in IIS für andere unterstützte Werte.

## <a name="operations-for-configuring-service-certificates"></a>Vorgänge für Dienstzertifikate konfigurieren
Dieses Thema dient nur zur Referenz. Führen Sie die Konfigurationsschritte:

* Konfigurieren Sie das SSL-Zertifikat
* Konfigurieren der Clientzertifikate

## <a name="create-a-self-signed-certificate"></a>Ein selbstsigniertes Zertifikat erstellen
Ausführen:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Anpassen:

*    -n die Dienst-URL. Platzhalter ("CN = * .cloudapp .net") und alternative Namen ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net) unterstützt.
*    -e mit Ablaufdatum des Zertifikats ein sicheres Kennwort erstellen und der Aufforderung angeben.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>PFX-Datei selbst signiertes Zertifikat erstellen

Ausführen:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Geben Sie Kennwort und exportieren Sie Zertifikat mit den folgenden Optionen:
* Ja, privaten Schlüssel exportieren
* Alle erweiterten Eigenschaften exportieren

## <a name="export-ssl-certificate-from-certificate-store"></a>SSL-Zertifikat aus dem Zertifikatspeicher exportieren

* Zertifikat suchen
* Klicken Sie auf Aktionen -> alle Aufgaben -> Exportieren...
* Zertifikat exportieren ein. PFX-Datei mit den folgenden Optionen:
    * Ja, privaten Schlüssel exportieren
    * Wenn möglich, alle Zertifikate im Zertifizierungspfad einbeziehen * alle erweiterte Eigenschaften exportieren

## <a name="upload-ssl-certificate-to-cloud-service"></a>SSL-Zertifikat auf Cloud-Dienst hochladen

Hochladen mit den vorhandenen Zertifikat oder generiert. PFX-Datei mit dem SSL-Schlüsselpaar:

* Geben Sie das Kennwort schützt die private Schlüsselinformationen

## <a name="update-ssl-certificate-in-service-configuration-file"></a>SSL-Zertifikat im Dienst aktualisieren

Aktualisieren Sie Fingerabdruckwert für die folgende Einstellung in der Konfigurationsdatei Service mit dem Fingerabdruck des Zertifikats zum Cloud-Dienst hochgeladen:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL-Zertifizierungsstelle importieren

Folgendermaßen Sie in alle Konto-Maschine, die mit dem Dienst kommunizieren

* Doppelklicken Sie auf die. CER-Datei in Windows Explorer
* Klicken Sie im Dialogfeld Zertifikat auf Zertifikat installieren.
* Zertifikat im Speicher Vertrauenswürdige Stammzertifizierungsstellen importieren

## <a name="turn-off-client-certificate-based-authentication"></a>Zertifikatbasierte Clientauthentifizierung deaktivieren

Nur Clientauthentifizierung unterstützt und deaktivieren können öffentlichen Zugriff auf die Dienstendpunkte, wenn andere Mechanismen (z. B. Microsoft Azure Virtual Network) sind.

Ändern Sie diese Einstellung False in der Service-Konfigurationsdatei, das Feature zu deaktivieren:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Kopieren Sie denselben Fingerabdruck wie das SSL-Zertifikat in der Zertifizierungsstellen-Zertifikat:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Erstellen Sie eine selbstsigniertes Zertifizierungsstelle
Führen Sie die folgenden Schritte zum Erstellen eines selbstsignierten Zertifikats als Zertifizierungsstelle fungieren:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Angepasst

*    e - Zertifizierung abläuft


## <a name="find-ca-public-key"></a>Zertifizierungsstelle öffentlichen Schlüssel

Alle Client-Zertifikate müssen von einer vom Dienst vertrauenswürdige Zertifizierungsstelle ausgegeben worden sein. Suchen Sie den öffentlichen Schlüssel der Zertifizierungsstelle, die den Client, die für die Authentifizierung verwendet werden Zertifikate, um Cloud-Dienst hinzufügen.

Wenn die Datei mit dem öffentlichen Schlüssel nicht verfügbar ist, exportieren sie aus dem Zertifikatspeicher:

* Zertifikat suchen
    * Suchen Sie ein Client-Zertifikat von derselben Zertifizierungsstelle ausgestellt
* Doppelklicken Sie auf das Zertifikat.
* Wählen Sie im Dialogfeld Zertifikat die Registerkarte Zertifizierungspfad.
* Doppelklicken Sie auf den Pfad CA.
* Notieren Sie sich den Zertifikateigenschaften.
* Das Dialogfeld **Zertifikat** zu schließen.
* Zertifikat suchen
    * Suchen Sie nach oben CA.
* Klicken Sie auf Aktionen -> alle Aufgaben -> Exportieren...
* Zertifikat exportieren ein. CER mit den folgenden Optionen:
    * **Nein, privaten Schlüssel nicht exportieren**
    * Fügen Sie alle Zertifikate im Zertifizierungspfad.
    * Exportieren Sie alle erweiterten Eigenschaften.

## <a name="upload-ca-certificate-to-cloud-service"></a>CA-Zertifikat in Cloud-Dienst hochladen

Hochladen mit den vorhandenen Zertifikat oder generiert. CER-Datei mit dem öffentlichen Schlüssel der CA.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Update-Zertifizierungsstellenzertifikat in Dienst

Aktualisieren Sie Fingerabdruckwert für die folgende Einstellung in der Konfigurationsdatei Service mit dem Fingerabdruck des Zertifikats zum Cloud-Dienst hochgeladen:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Aktualisieren Sie den Wert der Einstellung mit demselben Daumenabdruck:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Clientzertifikate ausstellen

Jede Person, die Zugriff auf den Dienst muss ein Client-Zertifikat ausgestellt für Ihre exklusive verwenden und sollten Ihre eigenen Kennwörter zum Schutz des privaten Schlüssels auswählen. 

Die folgenden Schritte müssen auf dem gleichen Computer ausgeführt werden, wo CA Zertifikat generiert und gespeichert wurde:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Anpassen:

* n - ID für die Kunden, die mit diesem Zertifikat authentifiziert werden
* e - Ablaufdatum des Zertifikats
* MyID.pvk und MyID.cer mit eindeutigen Dateinamen für diese Clientzertifikat

Dieser Befehl fordert ein Kennwort erstellt und dann einmal verwendet werden. Verwenden Sie ein sicheres Kennwort.

## <a name="create-pfx-files-for-client-certificates"></a>PFX-Dateien für Client-Zertifikate erstellen

Für jede generierte Clientzertifikat ausführen:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Anpassen:

    MyID.pvk and MyID.cer with the filename for the client certificate

Geben Sie Kennwort und exportieren Sie Zertifikat mit den folgenden Optionen:

* Ja, privaten Schlüssel exportieren
* Alle erweiterten Eigenschaften exportieren
* Die Person, für die das Zertifikat ausgestellt wurde, sollte der Export Passwort

## <a name="import-client-certificate"></a>Client-Zertifikat importieren

Jedes einzelnen, für die ein Client-Zertifikat ausgestellt wurde, sollte das Schlüsselpaar auf den Computern importieren, mit dem er mit dem Dienst kommunizieren:

* Doppelklicken Sie auf die. PFX-Datei in Windows Explorer
* Importieren in den persönlichen Zertifikatspeicher mit mindestens diese Option:
    * Aktiviert erweiterte Eigenschaften einbeziehen

## <a name="copy-client-certificate-thumbprints"></a>Zertifikatfingerabdruck Client kopieren
Jedes einzelnen, für die ein Client-Zertifikat ausgestellt wurde, gehen um den Fingerabdruck des muss erhalten Zertifikate die Dienstkonfigurationsdatei hinzugefügt werden:
* Certmgr.exe ausführen
* Wählen Sie die Registerkarte Persönliche Angaben
* Doppelklicken Sie auf das Client-Zertifikat für die Authentifizierung verwendet werden
* Wählen Sie im Dialogfeld Zertifikat, das geöffnet wird, die Registerkarte Details
* Stellen Sie sicher, dass anzeigen Zeigt alle
* Wählen Sie das Feld mit der Fingerabdruck in der Liste
* Kopieren Sie den Wert der Fingerabdruck **nicht sichtbare Unicode-Zeichen vor der ersten Ziffer löschen** löschen alle Leerzeichen

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Zugelassene Kunden Dienstkonfigurationsdatei konfigurieren

Aktualisieren Sie den Wert für die folgende Einstellung in der Konfigurationsdatei Service mit einer kommagetrennten Liste Fingerabdrücke Clientzertifikate Zugriff auf den Dienst:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Client Certificate Revocation-Check konfigurieren

Bei der Zertifizierungsstelle Client Zertifikat Sperrstatus überprüft nicht die Standardeinstellung. Ändern Sie zum Aktivieren der Überprüfung Zertifizierungsstelle, die Zertifikate der Client Kontrollen unterstützt folgende Einstellung mit einem der Werte in der X509RevocationMode-Enumeration definiert:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>PFX-Datei für selbstsignierte Verschlüsselungszertifikate erstellen

Für ein Verschlüsselungszertifikat ausführen:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Anpassen:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Geben Sie Kennwort und exportieren Sie Zertifikat mit den folgenden Optionen:
*    Ja, privaten Schlüssel exportieren
*    Alle erweiterten Eigenschaften exportieren
*    Sie benötigen das Kennwort, das Zertifikat zum Cloud-Dienst übertragen.

## <a name="export-encryption-certificate-from-certificate-store"></a>Verschlüsselungszertifikat aus dem Zertifikatspeicher exportieren

*    Zertifikat suchen
*    Klicken Sie auf Aktionen -> alle Aufgaben -> Exportieren...
*    Zertifikat exportieren ein. PFX-Datei mit den folgenden Optionen: 
  *    Ja, privaten Schlüssel exportieren
  *    Wenn möglich, alle Zertifikate im Zertifizierungspfad einbeziehen 
*    Alle erweiterten Eigenschaften exportieren

## <a name="upload-encryption-certificate-to-cloud-service"></a>Verschlüsselungszertifikat auf Cloud-Dienst hochladen

Hochladen mit den vorhandenen Zertifikat oder generiert. PFX-Datei mit dem Verschlüsselungsschlüsselpaar:

* Geben Sie das Kennwort schützt die private Schlüsselinformationen

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Verschlüsselungszertifikat in Dienst aktualisieren

Aktualisieren Sie Fingerabdruckwert folgende Einstellung in der Konfigurationsdatei Service mit dem Fingerabdruck des Zertifikats zum Cloud-Dienst hochgeladen:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Allgemeine Zertifikatvorgänge

* Konfigurieren Sie das SSL-Zertifikat
* Konfigurieren der Clientzertifikate

## <a name="find-certificate"></a>Zertifikat suchen

Gehen Sie folgendermaßen vor:

1. Führen Sie mmc.exe.
2. Datei-Snap-in hinzufügen/entfernen...
3. Wählen Sie **Zertifikate**aus.
4. Klicken Sie auf **Hinzufügen**.
5. Wählen Sie den Speicherort des Zertifikats.
6. Klicken Sie auf **Fertig stellen**.
7. Klicken Sie auf **OK**.
8. Erweitern Sie **Zertifikate**.
9. Erweitern Sie Certificate Store.
10. Zertifikat untergeordneten Knoten zu erweitern.
11. Wählen Sie ein Zertifikat aus der Liste.

## <a name="export-certificate"></a>Zertifikat exportieren
Der **Zertifikatsexport-Assistent**:

1. Klicken Sie auf **Weiter**.
2. Wählen Sie **Ja**, **privaten Schlüssel exportieren**.
3. Klicken Sie auf **Weiter**.
4. Wählen Sie das gewünschte Ausgabeformat.
5. Wählen Sie die gewünschten Optionen.
6. **Überprüfen.**
7. Geben Sie ein Kennwort ein, und bestätigen Sie es.
8. Klicken Sie auf **Weiter**.
9. Geben Sie oder einem Dateinamen suchen, wo das Zertifikat gespeichert (verwenden Sie ein. Die Erweiterung PFX).
10. Klicken Sie auf **Weiter**.
11. Klicken Sie auf **Fertig stellen**.
12. Klicken Sie auf **OK**.

## <a name="import-certificate"></a>Zertifikat importieren

Im Assistenten:

1. Wählen Sie den Speicherort.

    * Wählen Sie **Aktuelle Benutzer** nur Prozesse unter Aktueller Benutzer den Dienst zugreifen
    * Wählen Sie **Lokalen Computer** aus, wenn andere Prozesse auf diesem Computer den Dienst zugreifen
2. Klicken Sie auf **Weiter**.
3. Importieren aus einer Datei bestätigen Sie den Pfad.
4. Importieren einer. PFX-Datei:
    1.     Geben Sie das Kennwort schützt den privaten Schlüssel
    2.     Importoptionen auswählen
5.     Wählen Sie "" Zertifikate in folgendem Speicher speichern
6.     Klicken Sie auf **Durchsuchen**.
7.     Wählen Sie den gewünschten Speicher.
8.     Klicken Sie auf **Fertig stellen**.
       
    * Wenn der Speicher vertrauenswürdiger Stammzertifizierungsstellen ausgewählt wurde, klicken Sie auf **Ja**.
9.     Klicken Sie auf **OK** , auf alle Dialogfelder.

## <a name="upload-certificate"></a>Zertifikat hochladen

In der [Azure-Portal](https://portal.azure.com/)

1. **Cloud-Dienste**auswählen
2. Cloud-Dienst auswählen
3. Klicken Sie im oberen Menü auf **Zertifikate**.
4. Klicken Sie in der Leiste unten auf **Hochladen**.
5. Wählen Sie die Zertifikatsdatei.
6. Es ist ein. PFX-Datei, geben Sie das Kennwort für den privaten Schlüssel.
7. Abschluss, kopieren Sie den Fingerabdruck des Zertifikats aus den neuen Eintrag in der Liste.

## <a name="other-security-considerations"></a>Weitere Sicherheitsaspekte
 
SSL-Einstellungen in diesem Dokument beschriebenen Verschlüsseln von Kommunikation zwischen dem Dienst und seinen Clients Wenn HTTPS-Endpunkt verwendet wird. Dies ist wichtig, da die Anmeldeinformationen für den Zugriff auf und andere vertrauliche Informationen in der Mitteilung befinden. Beachten Sie jedoch, dass der Dienst weiterhin internen Status, einschließlich der Anmeldeinformationen in seine internen Tabellen in Microsoft Azure SQL-Datenbank, die Sie in Ihrem Microsoft Azure-Abonnement für Metadaten-Speicher bereitgestellt haben. Die Datenbank wurde im Rahmen der folgenden Einstellung in der Konfigurationsdatei Service definiert (. Mithilfe Datei): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

In dieser Datenbank gespeicherte Anmeldeinformationen werden verschlüsselt. Jedoch empfohlen, damit Web- und Workerrollen Rollen der Service-Bereitstellung sind aktuell und sicher, wie sie sowohl Zugriff auf Metadaten-Datenbank und das Zertifikat für die Verschlüsselung und Entschlüsselung der gespeicherten Anmeldeinformationen verwendet. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<properties 
    pageTitle="Konfigurieren von SSL für einen Clouddienst | Microsoft Azure" 
    description="Erfahren Sie wie einen HTTPS-Endpunkt für eine Webrolle und ein SSL-Zertifikat zur Sicherung Ihrer Anwendung hochladen. Diese Beispiele verwenden Azure-Portal." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>Konfigurieren von SSL für eine Anwendung in Azure

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-configure-ssl-certificate-portal.md)
- [Azure-Verwaltungsportal](cloud-services-configure-ssl-certificate.md)

Secure Socket Layer (SSL)-Verschlüsselung ist die am häufigsten verwendete Methode zum Sichern von Daten über das Internet gesendet. Diese Aufgabe wird beschrieben, wie einen HTTPS-Endpunkt für eine Webrolle und ein SSL-Zertifikat zur Sicherung Ihrer Anwendung hochladen.

> [AZURE.NOTE] Die Verfahren in dieser Aufgabe gelten für Azure Cloud Services; App-Dienste finden Sie [diese](../app-service-web/web-sites-configure-ssl-certificate.md).

Dieser Task verwendet eine Bereitstellung in der Produktion; Informationen zur Verwendung einer Stagingdatenbank Bereitstellung erfolgt am Ende dieses Themas.

Lesen Sie [diese](cloud-services-how-to-create-deploy-portal.md) zuerst haben Sie nicht noch einen Cloud-Dienst erstellt.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a>Schritt 1: Abrufen eines SSL-Zertifikats

Zum Konfigurieren von SSL für eine Anwendung müssen Sie ein SSL-Zertifikat abzurufen, das von einer Zertifizierungsstelle (CA), einer vertrauenswürdigen dritten Partei signiert wurde, der zu diesem Zweck Zertifikate ausstellt. Wenn nicht bereits haben müssen Sie von einem Unternehmen erhalten, die SSL-Zertifikate verkauft.

Das Zertifikat muss die folgenden Zertifikate in Azure erfüllen:

-   Das Zertifikat muss einen privaten Schlüssel enthalten.
-   Das Zertifikat muss für den Schlüsselaustausch exportiert eine privater Informationsaustausch (PFX) erstellt werden.
-   Das Zertifikat muss für den Cloud-Dienst Zugriff auf Domäne. Sie können kein SSL-Zertifikat von einer Zertifizierungsstelle (CA) für die Domäne cloudapp.net erhalten. Einen benutzerdefinierten Domänennamen benutzen müssen erhalten Zugriff auf den Dienst. Wenn ein von einer Zertifizierungsstelle Zertifikat muss das Zertifikat Domainnamen verwendet, um auf die Anwendung zugreifen. Beispielsweise ist Ihre benutzerdefinierten Domänennamen **"contoso.com"** Sie bitten ein Zertifikat der Zertifizierungsstelle für * **. contoso.com** oder * *www.contoso.com**.
-   Das Zertifikat muss mindestens 2048-Bit-Verschlüsselung verwenden.

Zu Testzwecken können Sie [Erstellen](cloud-services-certs-create.md) und verwenden Sie ein selbstsigniertes Zertifikat. Ein selbstsigniertes Zertifikat wird nicht durch eine Zertifizierungsstelle authentifiziert und kann die cloudapp.net Domäne als die Website-URL. Beispielsweise verwendet die Aufgabe ein selbstsigniertes Zertifikat in dem Allgemeine Namen (CN) im Zertifikat verwendet **sslexample.cloudapp.net**ist.

Anschließend müssen Sie die Zertifikatinformationen in der Dienstdefinition und Service-Konfigurationsdateien einschließen.

<a name="modify"> </a>
## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Schritt 2: Ändern der Service- Konfiguration Dateien

Die Anwendung muss konfiguriert das Zertifikat und ein HTTPS-Endpunkt hinzugefügt werden muss. Daher müssen die Service-Definition und Service-Konfigurationsdateien aktualisiert werden.

1.  In Ihrer Entwicklungsumgebung Service-Definitionsdatei (CSDEF) öffnen, Hinzufügen eines Abschnitts **Zertifikate** im Bereich **WebRole** und enthalten die folgenden Informationen über das Zertifikat (und Zwischenzertifikate):

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    **Zertifikate** definiert den Namen Zertifikat, Standort und den Namen des Speichers, in dem sie sich befindet.
    
    Berechtigungen (`permisionLevel` Attribut) kann auf eine der folgenden festgelegt werden:

  	| Berechtigungswert  | Beschreibung |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Standard)** Alle Rollen Prozesse können den privaten Schlüssel zugreifen. |
  	| Erhöhte          | Erweiterte Prozesse können den privaten Schlüssel zugreifen.|

2.  Fügen Sie in Ihre Service-Definitionsdatei ein **InputEndpoint** Element im Abschnitt **Endpunkte** HTTPS aktivieren:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  Fügen Sie in der Definitionsdatei Service eine **Bindung** Element im Abschnitt **Sites hinzu** Eine HTTPS-Bindung zuordnen den Endpunkt zu Ihrer Website hinzugefügt:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Alle erforderlichen Änderungen zur Definitionsdatei abgeschlossen, aber auch noch die Zertifikatsinformationen Dienstkonfigurationsdatei hinzufügen.

4.  Fügen Sie in die Dienstkonfigurationsdatei (mithilfe) ServiceConfiguration.Cloud.cscfg einen Abschnitt **Zertifikate** im Bereich **Rolle** den Probe Fingerabdruckwert unten mit dem Zertifikat ersetzen:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(Im obigen Beispiel **sha1** Fingerabdruck-Algorithmus verwendet. Geben Sie den entsprechenden Wert für Ihr Zertifikat Fingerabdruck Algorithmus.)

Da die Service-Definition und werden aktualisiert, Verpacken Sie die Bereitstellung in Azure hochladen. Wenn Sie **Cspack**verwenden Sie nicht das Flag **/generateConfigurationFile** , wie die Zertifikatinformationen überschrieben soeben hinzugefügt.

## <a name="step-3-upload-a-certificate"></a>Schritt 3: Hochladen eines Zertifikats

Verbinden mit dem Portal und...

1. Wählen Sie den Clouddienst im Portal, wählen Sie den **Cloud-Dienst**. (Die im Abschnitt **alle Ressourcen** ist.) 
    
    ![Cloud-Dienst veröffentlichen](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Klicken Sie auf **Zertifikate**.

    ![Klicken Sie auf Zertifikate](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Die **Datei**, **Kennwort**, und klicken Sie auf **Hochladen**.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Schritt 4: Verbindung zu der Rolleninstanz über HTTPS

Nachdem die Bereitstellung in Azure ausgeführt ist, können Sie mit HTTPS verbinden.
    
1.  Klicken Sie auf der **Website-URL** , um den Webbrowser zu öffnen.

    ![Klicken Sie auf Site-URL](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2.  In Ihrem Webbrowser ändern der Verknüpfung zur Verwendung von **Https** anstelle von **http**, und finden Sie auf der Seite.

    >[AZURE.NOTE] Wenn Sie ein selbstsigniertes Zertifikat verwenden, wenn Sie HTTPS-Endpunkt suchen, die das selbstsignierte Zertifikat zugeordnet ist möglicherweise einen Zertifikatfehler im Browser angezeigt. Mit einem Zertifikat von einer vertrauenswürdigen Zertifizierungsstelle signiert wird dieses Problem beseitigt; in der Zwischenzeit können Sie den Fehler ignorieren. (Eine andere Option ist das selbstsignierte Zertifikat Zertifikatspeicher des Benutzers vertrauenswürdiges Zertifikat hinzufügen.)

    ![Vorschau Website](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

    >[AZURE.TIP] Wenn Sie SSL für eine Stagingdatenbank Bereitstellung statt einer Bereitstellung in der Produktion verwenden möchten, müssen Sie zuerst bestimmen Sie die URL für den staging-Bereitstellung verwendet. Nach der Bereitstellung Cloud-Dienst der URL in die Stagingumgebung **Bereitstellung ID** GUID in diesem Format bestimmt:`https://deployment-id.cloudapp.net/`  
      
    >Erstellen Sie ein Zertifikat mit dem allgemeinen Namen (CN) gleich GUID-basierte URL (z. B. **328187776e774ceda8fc57609d404462.cloudapp.net**). Verwenden Sie das Portal bereitgestellten Clouddienst Zertifikat hinzufügen. Fügen Sie die Zertifikatsinformationen auf Ihre CSDEF und mithilfe Verpacken Sie Ihrer Anwendung und aktualisieren Sie die stufenweise Bereitstellung verwenden Sie das neue Paket.

## <a name="next-steps"></a>Nächste Schritte

* [Konfiguration des Cloud-Dienst](cloud-services-how-to-configure-portal.md).
* Erfahren Sie, wie [einen Cloud-Dienst bereitgestellt](cloud-services-how-to-create-deploy-portal.md).
* Konfigurieren Sie einen [benutzerdefinierten Domänennamen](cloud-services-custom-domain-name-portal.md).
* [Verwalten der Cloud-Dienst](cloud-services-how-to-manage-portal.md).

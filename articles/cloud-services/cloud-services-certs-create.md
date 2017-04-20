<properties 
    pageTitle="Cloud-Services und Verwaltungszertifikate | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Verwenden von Zertifikaten mit Microsoft Azure" 
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Übersicht über Zertifikate für Azure Cloud Services
Zertifikate werden in Azure Cloud Services ([Dienstzertifikate](#what-are-service-certificates)) und Authentifizierung mit Management-API ([Verwaltungszertifikate](#what-are-management-certificates) der Azure-Verwaltungsportal und nicht ARM) verwendet. Dieses Thema bietet eine allgemeine Übersicht über beide Zertifikattypen zum [Erstellen](#create) und [Bereitstellen](#deploy) , um Azure.

Zertifikate in Azure verwendet x. 509 v3-Zertifikate werden und können von anderen vertrauenswürdigen Zertifikat signiert oder selbstsignierte sein. Ein selbstsigniertes Zertifikat wird durch eine eigene Creator, und signiert, sind standardmäßig nicht vertrauenswürdig. Die meisten Browser können dies ignorieren. Selbstsignierte Zertifikate sollten nur selbst beim Entwickeln und Testen von Cloud-Diensten verwendet werden. 

Zertifikate von Azure können einen privaten oder einen öffentlichen Schlüssel enthalten. Zertifikate haben einen Fingerabdruck, der sie in eindeutiger Weise identifizieren kann. Dieser Fingerabdruck wird in Azure [Konfigurationsdatei](cloud-services-configure-ssl-certificate.md) verwendet, welches Zertifikat identifizieren ein Cloud-Dienst verwenden soll. 

## <a name="what-are-service-certificates"></a>Was sind Dienstzertifikate?
Dienstzertifikate werden um Clouddienste und sichere Kommunikation mit dem Dienst zugeordnet. Wenn Sie eine Webrolle bereitgestellt, sollten Sie ein Zertifikat bereitstellen, die einen verfügbar gemachten HTTPS-Endpunkt authentifizieren können. Dienstzertifikate in der Dienstdefinition definiert werden automatisch mit dem virtuellen Computer bereitgestellt, die eine Instanz der Rolle ausgeführt wird. 

Sie können Dienstzertifikate klassischen Azure-Portal entweder mit klassischen Azure-Portal oder mithilfe der Service Management API hochladen. Dienstzertifikate werden bestimmte Clouddienst zugeordnet und eine Bereitstellung in der dienstdefinitionsdatei zugewiesen.

Dienstzertifikate aus Ihrer Dienste separat verwaltet werden und können von verschiedenen Personen verwaltet werden. Ein Entwickler kann z. B. ein Service-Paket hochgeladen, die ein Zertifikat auf, das ein IT-Manager bereits in Azure hochgeladen. IT-Manager verwalten und erneuern Sie das Zertifikat des Dienstes zu ändern, ohne ein neues Servicepaket hochladen. Dies ist möglich, da der logische Name für das Zertifikat und die Namen und Speicherort sind in Service-Definitionsdatei angegeben, während der Fingerabdruck des Zertifikats in der Service-Konfigurationsdatei angegeben ist. Um das Zertifikat zu aktualisieren, müssen sie nur uploaden ein neues Zertifikat und den Fingerabdruckwert in der Service-Konfigurationsdatei ändern.

## <a name="what-are-management-certificates"></a>Was sind Verwaltungszertifikate?
Verwaltungszertifikate können die Service Management API von Azure Classic authentifizieren. Viele Programme und Tools (wie Visual Studio oder Azure SDK) verwendet diese Zertifikate Konfiguration und Bereitstellung von verschiedenen Azure Services automatisieren. Diese wirklich hängen nicht mit cloud-Services. 

>[AZURE.WARNING] Sei vorsichtig! Diese Zertifikate können wer authentifiziert, damit das Abonnement verwalten, denen, dem Sie zugeordnet sind. 

### <a name="limitations"></a>Grenzen
Es sind maximal 100 Verwaltungszertifikate pro Abonnement. Es gibt auch maximal 100 Verwaltungszertifikate für alle Abonnements unter einen bestimmten Dienst-Administrator-Benutzer-ID. Wenn Konto-Administrator die Benutzer-ID bereits 100 Verwaltungszertifikate hinzufügen verwendet wurde, eine weitere Zertifikate besteht können Sie Co-Administrator Hinzufügen zusätzlicher Zertifikate hinzufügen. 

Lesen Sie vor dem 100 Zertifikate hinzufügen, wenn Sie ein vorhandenes Zertifikat wiederverwenden können. Co-Administratoren mit hinzugefügt der Zertifikat-Prozess möglicherweise überflüssige Komplexität.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Neues selbstsigniertes Zertifikat erstellen
Sie können alle Tools ein selbstsigniertes Zertifikat erstellen, solange diese Einstellungen folgen:

* Ein x. 509-Zertifikat.
* Enthält einen privaten Schlüssel.
* Für Schlüsselaustausch (PFX-Datei) erstellt.
* Antragstellername muss die Domäne zur Cloud-Dienst zugreifen. 
    > Eine SSL-Zertifikat für die cloudapp.net (oder für alle Azure verknüpft) Domäne kann nicht abgerufen werden; das Zertifikat muss Domainnamen verwendet, um auf die Anwendung zugreifen. Zum Beispiel **contoso.net**, nicht **contoso.cloudapp.net**.
* Mindestens 2048-Bit-Verschlüsselung.
* **Zertifikat nur Service**: Client-Zertifikat im *persönlichen* Zertifikatspeicher gespeichert.

Es gibt zwei einfache Methoden, erstellen Sie ein Zertifikat unter Windows mit der `makecert.exe` -Dienstprogramm oder IIS.

### <a name="makecertexe"></a>MakeCert.exe

Dieses Dienstprogramm ist veraltet und wird hier nicht dokumentiert. [MSDN-Artikel](https://msdn.microsoft.com/library/windows/desktop/aa386968) anzeigen

### <a name="powershell"></a>PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Das Zertifikat einer IP-Adresse statt einer Domäne verwenden, verwenden Sie die IP-Adresse in der DNS-Name - Parameter.


Wenn Sie das [Zertifikat mit dem Management-Portal](../azure-api-management-certs.md)verwenden möchten, in eine **CER** -Datei exportieren:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet-Informationsdienste (IIS)

Es gibt viele Seiten im Internet, die mit IIS behandelt. [Hier](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) ist ein habe ich denke auch erklärt. 

### <a name="java"></a>Java
Java können Sie [ein Zertifikat](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate)erstellen.

### <a name="linux"></a>Linux
[Dieser](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) Artikel beschreibt, wie Zertifikate mit SSH erstellen.

## <a name="next-steps"></a>Nächste Schritte

[Ihr Zertifikat zu klassischen Azure-Portal hochladen](cloud-services-configure-ssl-certificate.md) oder der [Azure-Portal](cloud-services-configure-ssl-certificate-portal.md).

Hochladen Sie eine [Management-API Zertifikat](../azure-api-management-certs.md) zum klassischen Azure-Portal.

>[AZURE.NOTE] Azure-Portal verwenden Management Zertifikate nicht auf die API zugreifen, sondern Benutzerkonten verwendet.

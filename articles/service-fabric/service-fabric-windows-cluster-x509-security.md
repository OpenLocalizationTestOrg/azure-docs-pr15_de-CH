<properties
   pageTitle="Verbinden mit einem sicheren privaten Cluster | Microsoft Azure"
   description="Dieser Artikel beschreibt das Sichern der Kommunikation in eigenständigen oder private sowie zwischen Clients und dem Cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Sichern eines Clusters eigenständige Windows x. 509-Zertifikate

Dieser Artikel beschreibt zur Absicherung der Kommunikation zwischen den verschiedenen Knoten Ihres Clusters eigenständige Windows ebenso wie zum Authentifizieren von Clients, Herstellen einer Verbindung mit diesem Cluster mit x. 509-Zertifikaten. Dadurch wird sichergestellt, dass nur autorisierte Benutzer Zugriff auf den Cluster bereitgestellte Anwendung und Verwaltungsaufgaben ausführen können.  Zertifikatschutz sollte im Cluster aktiviert werden, wenn der Cluster erstellt wird.  

Weitere Informationen zur Clustersicherheit wie Knoten-Client-zu-Knoten-Sicherheit und rollenbasierte Zugriffskontrolle finden Sie unter [Cluster Sicherheitsszenarios](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Die Zertifikate müssen?

Mit [eigenständigen Cluster Paket](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) auf einem der Knoten im Cluster gestartet wird. Im Download-Paket finden Sie eine Datei **ClusterConfig.X509.MultiMachine.json** . Öffnen Sie und Lesen Sie den Abschnitt für die **Sicherheit** im Abschnitt **Eigenschaften** :

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

Dieser Abschnitt beschreibt die Zertifikate, die Sie für die Absicherung von eigenständigen Windows Cluster. Legen die Werte **ClusterCredentialType** und **ServerCredentialType** auf *X509*Zertifikaten basierende Sicherheit aktivieren.

>[AZURE.NOTE] [Fingerabdruck](https://en.wikipedia.org/wiki/Public_key_fingerprint) ist die primäre Identität eines Zertifikats. Lesen Sie [Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695.aspx) zu den Fingerabdruck der Zertifikate, die Sie erstellen.

Die folgende Tabelle enthält die Clusterinstallation müssen Zertifikate:

|**CertificateInformation festlegen**|**Beschreibung**|
|-----------------------|--------------------------|
|ClusterCertificate|Dieses Zertifikat muss zur Absicherung der Kommunikation zwischen den Knoten in einem Cluster. Zwei verschiedene Zertifikate, eine primäre und eine sekundäre können für aktualisieren. Der Zertifikatfingerabdruck primäre **Fingerabdruck** -Abschnitt und der sekundäre **ThumbprintSecondary** Variablen festgelegt.|
|ServerCertificate|Dieses Zertifikat wird an den Client angezeigt, wenn versucht wird, eine Verbindung mit diesem Cluster. Der Einfachheit halber können Sie dasselbe Zertifikat für *ClusterCertificate* und *ServerCertificate*verwenden. Zwei verschiedene Serverzertifikate, eine primäre und eine sekundäre können für aktualisieren. Der Zertifikatfingerabdruck primäre **Fingerabdruck** -Abschnitt und der sekundäre **ThumbprintSecondary** Variablen festgelegt. |
|ClientCertificateThumbprints|Dies ist ein Satz von Zertifikaten auf authentifizierte Clients installieren möchten. Sie können zahlreiche verschiedene Client-Zertifikate auf dem Computer, die Zugriff auf den Cluster, installiert. Legen Sie den Fingerabdruck des Zertifikats in der Variablen **CertificateThumbprint** . Wenn Sie **IsAdmin** auf *true*festgelegt, kann mit diesem Zertifikat installiert Administrator Aktivitäten im Cluster führen. Ist **IsAdmin** *false*, kann mit diesem Zertifikat nur zulässigen für Benutzerzugriff normalerweise schreibgeschützt Aktionen ausführen. Weitere Informationen zu Rollen finden Sie unter [Rollenbasierte Zugriffskontrolle (RBAC)](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|Den allgemeinen Name des ersten Clientzertifikat für **CertificateCommonName**festgelegt. **CertificateIssuerThumbprint** ist der Fingerabdruck für den Aussteller des Zertifikats. Lesen Sie [mit Zertifikaten](https://msdn.microsoft.com/library/ms731899.aspx) zu allgemeinen Namen und des Ausstellers.|
|HttpApplicationGatewayCertificate|Dies ist eine optionale Zertifikat ist angegeben, wenn Sie die HTTP-Application Gateway schützen möchten. Stellen Sie sicher, dass ReverseProxyEndpointPort in NodeTypes festgelegt wird, wenn Sie dieses Zertifikat verwenden.|

Hier ist Beispiel Clusterkonfiguration, Cluster, Server und Client-Zertifikate bereitgestellt wurde.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>X. 509 Zertifikaten
Zum Sichern der Kommunikation innerhalb des Clusters müssen Sie zunächst für die Clusterknoten x. 509-Zertifikate. Außerdem um Verbindung mit diesem Cluster Druckprozesse Maschinen einzuschränken, müssen Sie beziehen und Installieren von Zertifikaten für den Client-Computern.

Für Cluster, die Produktions-Workloads ausgeführt werden, verwenden Sie eine [Zertifizierungsstelle (Certificate Authority, CA)](https://en.wikipedia.org/wiki/Certificate_authority) signiert x. 509-Zertifikat zum Sichern des Clusters. Ausführliche Informationen über diese Zertifikate Gehe zu [wie: Abrufen eines Zertifikats](http://msdn.microsoft.com/library/aa702761.aspx).

Für Cluster, die für Testzwecke verwenden, können Sie ein selbstsigniertes Zertifikat verwenden.

## <a name="optional-create-a-self-signed-certificate"></a>Optional: Erstellen Sie ein selbstsigniertes Zertifikat
Eine Möglichkeit, ein selbstsigniertes Zertifikat erstellen, die ordnungsgemäß gesichert wird mithilfe des *CertSetup.ps1* -Skripts im Service Fabric-SDK-Ordner im Verzeichnis *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*. Bearbeiten Sie diese Datei und ein Zertifikat mit einem geeigneten Namen erstellen können.

Jetzt exportieren Sie das Zertifikat in eine PFX-Datei mit einem Kennwort geschützt. Zuerst müssen Sie den Fingerabdruck des Zertifikats zu erhalten. Certmgr.exe Anwendung ausführen. Den **Lokalen Computer\Personal** Ordner und suchen Sie das Zertifikat, das Sie gerade erstellt haben. Doppelklicken Sie auf das Zertifikat zu öffnen, wählen Sie die Registerkarte " *Details* " und scrollen Feld *Fingerabdruck* . Kopieren Sie den Fingerabdruckwert in der PowerShell-Befehl, die Leerzeichen entfernt.  Ändern Sie den Wert *$pswd* in ein sicheres Kennwort Eignung und die PowerShell ausführen:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Führen Sie zum Anzeigen eines Zertifikats auf dem Computer installiert den folgenden PowerShell-Befehl:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Sie haben ein Azure-Abonnement folgen Sie im Abschnitt [Hinzufügen von Zertifikaten Key Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault)

## <a name="install-the-certificates"></a>Installieren der Zertifikate
Haben Zertifikate können Sie diese auf den Clusterknoten installieren. Die Knoten müssen die neuesten Windows PowerShell 3.x installiert. Sie müssen diese Schritte auf jedem Knoten zum Cluster und Serverzertifikate und sekundäre Zertifikate wiederholen.

1. Kopieren Sie PFX-Dateien auf den Knoten.
2. Öffnen Sie ein PowerShell als Administrator aus, und geben Sie die folgenden Befehle. Ersetzen Sie *$pswd* durch das zum Erstellen dieses Zertifikats verwendete Kennwort. Ersetzen Sie *$PfxFilePath* durch den vollständigen Pfad der PFX-Datei auf diesem Knoten kopiert.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Als Nächstes müssen Sie die Zugriffskontrolle auf diesem Zertifikat festgelegt, sodass die Fabric-Dienst wird unter dem Netzwerkdienstkonto ausgeführt, durch Ausführen des folgenden Skripts verwendet werden kann. Geben Sie den Fingerabdruck des Zertifikats und "Netzwerkdienst" für das Dienstkonto. Sie können überprüfen die ACLs auf dem Werkzeug certmgr.exe und Privatschlüssel verwalten auf das Zertifikat.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Verbindung ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation CurrentUser - Speichername Meine
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Verbindung ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```

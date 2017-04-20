<properties
   pageTitle="Interaktion mit Service Fabric mit CLI | Microsoft Azure"
   description="Wie Sie Azure CLI Interaktion mit einem Cluster Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Verwendung der Azure-CLI Interaktion mit einem Service Fabric-Cluster

Sie können mit Service Fabric von Azure-CLI unter Linux mit Linux-Computern interagieren.

Der erste Schritt ist die neueste Version der CLI aus der Git Rep und richten im Pfad mit den folgenden Befehlen:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Für jeden Befehl unterstützt, geben Sie den Namen des Befehls, der Hilfe für den Befehl. Automatische Vervollständigung ist für die Befehle unterstützt. Beispielsweise gibt der folgende Befehl Sie Hilfe für die Anwendungsbefehle. 

```sh
 azure servicefabric application 
```

Sie können die Hilfe zu einem bestimmten Befehl wie im folgenden Beispiel gezeigt filtern:

```sh
 azure servicefabric application  create
```

Um AutoVervollständigen in der CLI zu aktivieren, führen Sie die folgenden Befehle:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Die folgenden Befehle zum Cluster herstellen und zeigt die Knoten des Clusters:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Eingabe - Hilfe finden sie benannte Parameter verwenden und nach einem Befehl. Zum Beispiel:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Beim Verbinden mit einem Cluster mit mehreren Computern von einem Computer **, der nicht Teil des Clusters**verwenden Sie den folgenden Befehl ein:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Ersetzen des PublicIPorFQDN Tags echte IP oder FQDN entsprechend. Beim Verbinden mit einem Cluster mit mehreren Computern von einem Computer **, Teil des Clusters**verwenden Sie den folgenden Befehl ein:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Mithilfe von PowerShell oder CLI Interaktion mit Linux Service Fabric Cluster über Azure-Portal erstellt. 

**Vorsicht:** Diese Cluster sind nicht sicher, so Sie möglicherweise öffnen, die ein Feld öffentliche IP-Adresse im clustermanifest hinzufügen.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Verwendung der Azure-CLI Verbindung zu einem Cluster Service Fabric

Die folgenden Azure-CLI-Befehle beschrieben Verbindung zu einem sicheren Cluster. Die Zertifikatdetails muss ein Zertifikat auf den Clusterknoten.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Verfügt Ihr Zertifikat Zertifizierungsstellen (CAs), müssen Sie Parameter - ca-Zertifikat-Pfad wie folgt hinzufügen: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Wenn Sie über mehrere Zertifizierungsstellen verfügen, verwenden Sie ein Komma als Trennzeichen.
 
Wenn der allgemeine Name im Zertifikat den Verbindungsendpunkt nicht übereinstimmen, können Sie den Parameter `--strict-ssl` zum Umgehen der Überprüfung wie im folgenden dargestellt: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Möchten Sie die CA-Überprüfung überspringen, können hinzufügen--ablehnen nicht autorisierte Parameter wie im folgenden dargestellt: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Nachdem Sie eine Verbindung herstellen, sollten Sie andere CLI-Befehle Interaktion mit Cluster ausführen können. 

## <a name="deploying-your-service-fabric-application"></a>Bereitstellung der Service Fabric-Anwendung

Führen Sie die folgenden Befehle kopieren, registrieren und Starten der Service Fabric-Anwendung:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Aktualisieren der Anwendung

Der Prozess ähnelt der [Prozess in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Erstellen, kopieren, registrieren und Projekt-Stammverzeichnis die Anwendung erstellt. Wenn die Anwendungsinstanz Fabric heißt: / MySFApp und den MySFApp, wäre die Befehle wie folgt:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Ändern Sie Ihre Anwendung und erstellen den geänderten Dienst neu.  Aktualisieren der geänderten Dienst Manifestdatei (ServiceManifest.xml) mit den aktualisierten Versionen für den Dienst (und Code Config oder gegebenenfalls Daten). Ändern Sie das Anwendungsmanifest (ApplicationManifest.xml) mit der aktualisierten Versionsnummer für die Anwendung und den geänderten Dienst auch.  Jetzt kopieren Sie und registrieren Sie die aktualisierte Anwendung mit den folgenden Befehlen:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Jetzt können Sie die Aktualisierung der Anwendung mit dem folgenden Befehl starten:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Sie können jetzt die Aktualisierung der Anwendung mit SFX überwachen. In wenigen Minuten würde die Anwendung aktualisiert wurden.  Sie können auch versuchen, eine aktualisierte Anwendung mit einem Fehler und automatische Funktion Service Fabric.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopieren des Anwendungspakets erfolgreich nicht

Überprüfen Sie, ob `openssh` installiert ist. In der Standardeinstellung ist nicht Ubuntu-Desktop installiert. Installieren Sie mit dem folgenden Befehl:

```
 sudo apt-get install openssh-server openssh-client**
```

Wenn das Problem weiterhin besteht, deaktivieren Sie PAM ssh durch Ändern der Datei **Sshd_config** mit den folgenden Befehlen:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Wenn das Problem weiterhin auftritt, erhöhen Sie die Anzahl der ssh Sessions durch Ausführen der folgenden Befehle:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Tasten für ssh Authentifizierung (im Gegensatz zu Kennwörtern) noch nicht unterstützt wird (da die Plattform ssh verwendet, um Pakete zu kopieren), so Kennwortauthentifizierung verwenden.


## <a name="next-steps"></a>Nächste Schritte

Der Development Environment einrichten und Bereitstellen einer Anwendung Fabric Service auf einem Linux-Cluster.

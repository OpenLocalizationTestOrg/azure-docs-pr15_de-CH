<properties 
   pageTitle="Eine Remoteverbindung mit dem Gerät StorSimple | Microsoft Azure"
   description="Erläutert die Konfiguration für die Remoteverwaltung sowie für StorSimple über HTTP oder HTTPS eine Verbindung zu Windows PowerShell."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Eine Remoteverbindung mit dem Gerät StorSimple

## <a name="overview"></a>Übersicht

Windows PowerShell-Remoting können Sie Ihr StorSimple-Gerät herstellen. Wenn Sie auf diese Weise verbinden, sehen Sie ein Menü nicht. (Sie sehen ein Menü, nur wenn Sie die serielle Konsole auf dem Gerät herstellen.) Mit Windows PowerShell-Remoting verbinden Sie mit einem bestimmten Runspace. Sie können auch die Anzeigesprache. 

Weitere Informationen zur Verwendung von Windows PowerShell-Remoting zu Ihrem Gerät finden Sie [Mit Windows PowerShell für StorSimple StorSimple-Gerät verwalten](storsimple-windows-powershell-administration.md).

Dieses Tutorial erklärt das Gerät für die Remoteverwaltung konfigurieren und für StorSimple eine Verbindung zu Windows PowerShell. HTTP oder HTTPS können über Windows PowerShell-Remoting verbinden. Wenn Sie für StorSimple eine Verbindung zu Windows PowerShell entscheiden, beachten Sie Folgendes: 

- Die serielle Konsole über Netzwerk-Switches ist nicht direkt an die serielle Konsole Gerät sicher ist Seien Sie auf Sicherheitsrisiko, beim Verbinden mit der seriellen Konsole Gerät über Netzwerk-Switches. 

- Verbindung über eine HTTP-Sitzung bieten möglicherweise mehr Sicherheit als über die serielle Konsole über das Netzwerk verbinden. Obwohl dies nicht die sicherste Methode ist, ist es auf vertrauenswürdige Netzwerke. 

- Verbindung über eine HTTPS-Sitzung mit einem selbstsignierten Zertifikat ist die sicherste und empfohlen.

Remote möglich Windows PowerShell-Schnittstelle. Remote-Zugriff auf Ihr StorSimple Gerät über die Windows PowerShell-Schnittstelle wird jedoch nicht standardmäßig aktiviert. Müssen Sie zunächst die Remoteverwaltung auf dem Gerät aktivieren, und klicken Sie dann auf dem Client, mit dem Gerät.

In diesem Artikel beschriebenen Schritte wurden auf einem Hostsystem mit Windows Server 2012 R2 ausgeführt.

## <a name="connect-through-http"></a>Verbindung über HTTP

Mit Windows PowerShell für StorSimple über eine HTTP-Sitzung bietet mehr Sicherheit als über die serielle Konsole des Geräts StorSimple verbinden. Obwohl dies nicht die sicherste Methode ist, ist es auf vertrauenswürdige Netzwerke.

Klassischen Azure-Portal oder die serielle Konsole können Sie remote-Verwaltung konfigurieren. Wählen Sie die folgenden Verfahren:

- [Verwenden der Azure-Verwaltungsportal Remoteverwaltung über HTTP aktivieren](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [Verwendung der seriellen Konsole Remoteverwaltung über HTTP aktivieren](#use-the-serial-console-to-enable-remote-management-over-http)

Nachdem Sie die Remoteverwaltung aktivieren, gehen Sie Client für eine Fernverbindung vorbereiten.

- [Remoteverbindung vorzubereiten](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Verwenden der Azure-Verwaltungsportal Remoteverwaltung über HTTP aktivieren 

Die folgenden Schritte im klassischen Azure-Portal Remoteverwaltung über HTTP aktivieren.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Remote-Management über die klassischen Azure-Portal aktivieren

1. Zugriff auf **Geräte** > für Ihr Gerät**Konfigurieren** .

2. Blättern Sie zum Abschnitt **Remoteverwaltung** .

3. **Remoteverwaltung aktivieren** auf **Ja**festgelegt.

4. Sie können nun eine Verbindung über HTTP. (Standardmäßig werden über HTTPS verbinden.) Stellen Sie sicher, dass HTTP aktiviert ist.

    >[AZURE.NOTE] Verbindung über HTTP herstellen ist nur in vertrauenswürdigen Netzwerken akzeptabel.

6. Klicken Sie am unteren Rand der Seite **Speichern** .

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Verwendung der seriellen Konsole Remoteverwaltung über HTTP aktivieren

Die folgenden Schritte auf der seriellen Konsole Gerät Remoteverwaltung aktivieren.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Remote-Management über die serielle Konsole Gerät aktivieren

1. Wählen Sie im Menü seriellen Konsole Option 1. Weitere Informationen über die serielle Konsole auf dem Gerät wechseln Sie [mit Windows PowerShell für StorSimple über serielle Konsole Gerät verbinden](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Geben Sie bei der Aufforderung`Enable-HcsRemoteManagement –AllowHttp`

3. Sie werden über Sicherheitsrisiken über HTTP Verbindung zum Gerät benachrichtigt. Bei Aufforderung bestätigen Sie, indem Sie **Y**eingeben.

4. Überprüfen Sie, dass HTTP eingeben aktiviert ist:`Get-HcsSystem`

5. Überprüfen Sie, ob das Feld **RemoteManagementMode** **HttpsAndHttpEnabled**zeigt. Die folgende Abbildung zeigt diese Einstellungen in kitten.

     ![Serielle HTTPS und HTTP aktiviert](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Remoteverbindung vorzubereiten

Die folgenden Schritte auf dem Client Remoteverwaltung aktivieren.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Client für remote-Verbindung vorbereitet

1. Starten Sie Windows PowerShell-Sitzung als Administrator.

2. Geben Sie den folgenden Befehl der Liste des Clients vertrauenswürdige Hosts die IP-Adresse des Geräts StorSimple hinzu: 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     Ersetzen Sie <*Device_ip*> durch die IP-Adresse des Geräts. Zum Beispiel: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Geben Sie den folgenden Befehl geräteanmeldeinformationen in einer Variablen speichern: 

     *$cred = Get-Credential*

4. Klicken Sie im Dialogfeld, das angezeigt wird:

    1. Geben Sie den Benutzernamen im folgenden Format: *Device_ip\SSAdmin*.
    2. Geben Sie das Administratorkennwort für Geräte, das festgelegt wurde, wenn das Gerät mit dem Setupassistenten konfiguriert wurde. Das Standardkennwort lautet *Kennwort1*.

7. Starten Sie Windows PowerShell-Sitzung auf dem Gerät durch Eingabe dieses Befehls:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Zum Erstellen einer Windows PowerShell-Sitzung für die Verwendung mit StorSimple virtuelles Gerät Anhängen der `–Port` Parameter und geben Sie den öffentlichen Port Remoting für StorSimple virtuelle Appliance konfiguriert.

     Zu diesem Zeitpunkt haben Sie eine aktive remote Windows PowerShell-Sitzung auf das Gerät.

    ![PowerShell-Remoting über HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Verbindung über HTTPS

Windows PowerShell ist für StorSimple über eine HTTPS-Sitzung die sicherste und empfohlene Methode Remote Microsoft Azure StorSimple-Gerät. Die folgenden Verfahren erläutert die serielle Konsole und Client-Computer so einrichten, dass Sie HTTPS für StorSimple eine Verbindung zu Windows PowerShell verwenden können.

Klassischen Azure-Portal oder die serielle Konsole können Sie remote-Verwaltung konfigurieren. Wählen Sie die folgenden Verfahren:

- [Verwenden der Azure-Verwaltungsportal Remoteverwaltung über HTTPS aktivieren](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [Verwendung der seriellen Konsole, remote-Management über HTTPS aktivieren](#use-the-serial-console-to-enable-remote-management-over-https)

Nachdem Sie die Remoteverwaltung aktivieren, gehen Sie eine Remoteverwaltung Host vorzubereiten und das Gerät vom Remotehost herstellen.

- [Vorbereiten des Hosts für die Remoteverwaltung](#prepare-the-host-for-remote-management)

- [Verbindung zum Gerät vom Remotehost](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Verwenden der Azure-Verwaltungsportal Remoteverwaltung über HTTPS aktivieren

Die folgenden Schritte im klassischen Azure-Portal Remoteverwaltung über HTTPS aktivieren.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Remoteverwaltung von klassischen Azure-Portal über HTTPS aktivieren

1. Zugriff auf **Geräte** > für Ihr Gerät**Konfigurieren** .

2. Blättern Sie zum Abschnitt **Remoteverwaltung** .

3. **Remoteverwaltung aktivieren** auf **Ja**festgelegt.

4. Sie können nun über HTTPS verbinden. (Standardmäßig werden über HTTPS verbinden.) Stellen Sie sicher, dass HTTPS aktiviert ist. 

5. Klicken Sie auf **Remote-Management-Zertifikat herunterladen**. Geben Sie den Speicherort dieser Datei. Sie müssen dieses Zertifikat auf dem Client oder Server, mit der das Gerät herstellen.

6. Klicken Sie am unteren Rand der Seite **Speichern** .

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Verwendung der seriellen Konsole, remote-Management über HTTPS aktivieren

Die folgenden Schritte auf der seriellen Konsole Gerät Remoteverwaltung aktivieren.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Remote-Management über die serielle Konsole Gerät aktivieren

1. Wählen Sie im Menü seriellen Konsole Option 1. Weitere Informationen über die serielle Konsole auf dem Gerät wechseln Sie [mit Windows PowerShell für StorSimple über serielle Konsole Gerät verbinden](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. Geben Sie bei der Aufforderung 

     `Enable-HcsRemoteManagement`

    Dies sollte HTTPS auf Ihrem Gerät aktivieren.

3. Überprüfen Sie, dass HTTPS eingeben aktiviert wurde: 

     `Get-HcsSystem`

    Stellen Sie sicher, dass das Feld **RemoteManagementMode** **HttpsEnabled**zeigt. Die folgende Abbildung zeigt diese Einstellungen in kitten.

     ![Serielle HTTPS aktiviert](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. Aus der Ausgabe von `Get-HcsSystem`, kopieren Sie die Seriennummer des Geräts und zur späteren Verwendung speichern.

    >[AZURE.NOTE] Die Seriennummer wird der Name im Zertifikat zugeordnet.

5. Fordern Sie ein Zertifikat Remoteverwaltung eingeben: 
 
     `Get-HcsRemoteManagementCert`

    Ein Zertifikat, das ähnlich der folgenden wird angezeigt.

    ![Remoteverwaltung Zertifikat](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Kopiert die Informationen im Zertifikat aus **---BEGIN CERTIFICATE---** **---** beenden Zertifikat--in einem Text-Editor wie Editor, und speichern Sie es als eine CER-Datei. (Kopieren Sie diese Datei auf den remote Host Vorbereitung.)

    >[AZURE.NOTE] Ein neues Zertifikat generieren, die `Set-HcsRemoteManagementCert` Cmdlet.

### <a name="prepare-the-host-for-remote-management"></a>Vorbereiten des Hosts für die Remoteverwaltung

Den Hostcomputer eine Verbindung Vorbereitung, die HTTPS-Sitzung verwendet, führen Sie die folgenden Schritte:

- [Importieren Sie die CER-Datei in dem Stammspeicher des Clients oder remote-Host](#to-import-the-certificate-on-the-remote-host).

- [Die Seriennummern der Geräte zur Hosts-Datei auf dem remote-Host hinzufügen](#to-add-device-serial-numbers-to-the-remote-host).

Jedes dieser Verfahren wird nachfolgend beschrieben.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>So importieren Sie das Zertifikat auf dem Remotehost

1. Maustaste auf die CER-Datei und wählen Sie **Zertifikat installieren**. Dadurch wird der Zertifikatimport-Assistent gestartet.

    ![Zertifikatimport-Assistent 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **Speicherort**für Wählen Sie **Lokalen Computer aus**und dann auf **Weiter**.

3. Wählen Sie **Alle Zertifikate in folgendem Speicher speichern**, und klicken Sie auf **Durchsuchen**. Navigieren Sie zu dem Stammspeicher des remote-Host, und klicken Sie auf **Weiter**.

    ![Zertifikatimport-Assistent 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Klicken Sie auf **Fertig stellen**. Erscheint eine Meldung, die besagt, dass der Import erfolgreich war.

    ![Zertifikatimport-Assistent 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Der Remotehost Seriennummern Gerät hinzufügen

1. Starten Sie den Editor als Administrator, und öffnen Sie die Datei Hosts unter \Windows\System32\Drivers\etc.

2. Die folgenden drei Einträge in der Hostsdatei hinzufügen: **Daten 0 IP Adresse**, **Controller 0 feste IP-Adresse**und **Controller 1 feste IP-Adresse**.

3. Geben Sie die Geräte, die Sie zuvor gespeichert. Zuordnen dieser IP-Adresse wie in der folgenden Abbildung dargestellt. Append-Controller 0 und Controller 1 **Controller0** und **Controller1** am Ende der Seriennummer (CN Name).

    ![Hostdatei CN Name hinzufügen](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Speichern Sie die Hostsdatei.

### <a name="connect-to-the-device-from-the-remote-host"></a>Verbindung zum Gerät vom Remotehost

Geben Sie eine SSAdmin-Sitzung auf dem Gerät von einem Remotehost oder -Client mit Windows PowerShell und SSL. SSAdmin-Sitzung wird Option 1 im Menü [seriellen Konsole](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) des Geräts.

Führen Sie die folgenden Schritte auf dem Computer, von dem die Windows PowerShell-Verbindung erstellt werden soll.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Geben Sie eine SSAdmin-Sitzung auf dem Gerät mithilfe von Windows PowerShell und SSL

1. Starten Sie Windows PowerShell-Sitzung als Administrator.

2. Hinzufügen der IP-Adresse des Geräts den Client vertrauenswürdigen Hosts eingeben:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    <*Device_ip*> ist, die IP-Adresse des Geräts. Zum Beispiel: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Erstellen Sie neue Anmeldeinformationen eingeben: 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Wobei <*IP-Adresse des Zielgeräts*> die IP-Adresse für Ihr Gerät Daten 0 ist. beispielsweise **10.126.173.90** wie in der obigen Abbildung der Hosts-Datei. Geben Sie auch das Administratorkennwort für Ihr Gerät.

4. Erstellen einer Sitzung eingeben:

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    Der Parameter - ComputerName in das Cmdlet vorsehen Sie <*Seriennummer des Zielgeräts*>. Diese Seriennummer wurde die IP-Adresse der Daten in der Datei Hosts auf dem entfernten Host 0 zugeordnet. beispielsweise **SHX0991003G44MT** wie in der folgenden Abbildung dargestellt.

5. Typ: 

     `Enter-PSSession $session`

6. Müssen einige Minuten warten und dann Sie Verbindung Geräts über HTTPS über SSL. Sie sehen eine Meldung, die angibt, dass Sie mit dem Gerät verbunden sind.

    ![PowerShell-Remoting über HTTPS und SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur [Verwendung von Windows PowerShell zum Verwalten von StorSimple-Gerät](storsimple-windows-powershell-administration.md).

- Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)

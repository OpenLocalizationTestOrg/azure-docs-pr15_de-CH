<properties 
   pageTitle="Selbstsignierte Zertifikate für virtuelle Punkt-zu-Standort-Netzwerk Verbindungen standortübergreifende Makecert mit erstellen | Microsoft Azure"
   description="Dieser Artikel enthält Schritte um Makecert selbstsignierte Zertifikate auf Windows 10 erstellen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Arbeiten mit selbst signierten Zertifikaten für Punkt-zu-Standort-Verbindung

Dieser Artikel hilft Ihnen ein selbstsigniertes Zertifikat verwenden **Makecert**erstellen und generieren Clientzertifikate aus. Die Schritte sind für Makecert Windows 10 geschrieben. MakeCert wurde überprüft, um Zertifikate zu erstellen, die mit P2S kompatibel sind. 

P2S Verbindung ist die bevorzugte Methode für Zertifikate Ihrer Enterprise zertifikatlösung Sicherstellen der Client Zertifikate mit common Name-Wert-Format verwenden 'name@yourdomain.com', das Format "Domänenname\Benutzername NetBIOS".

Haben Sie eine Lösung für Großunternehmen, muss ein selbstsigniertes Zertifikat P2S Clients die Verbindung zu einem virtuellen Netzwerk ermöglichen. Wir wissen, dass Makecert veraltet, aber es ist noch eine gültige Methode zum Erstellen von selbstsignierten Zertifikaten mit P2S kompatibel sind. Wir arbeiten an einer anderen Lösung zum Erstellen von selbstsignierten Zertifikaten zurzeit Makecert ist jedoch die bevorzugte Methode.

## <a name="create-a-self-signed-certificate"></a>Ein selbstsigniertes Zertifikat erstellen

MakeCert ist eine Möglichkeit, ein selbstsigniertes Zertifikat erstellen. Folgenden Schritte führen Sie durch das Erstellen eines selbstsignierten Zertifikats Makecert mit. Diese Schritte sind nicht Bereitstellungsmodell spezifisch. Sie gelten für Ressourcenmanager und Classic.

### <a name="to-create-a-self-signed-certificate"></a>Ein selbstsigniertes Zertifikat erstellen

1. Downloaden Sie auf einem Computer unter Windows 10 und installieren Sie das [Windows Software Development Kit (SDK) für Windows 10](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. Nach der Installation finden Sie unter diesem Pfad das Dienstprogramm makecert.exe: C:\Program Files (x86) \Windows Kits\10\bin\<Arch >. 
        
    Beispiel:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Als Nächstes erstellen und Installieren eines Zertifikats in den persönlichen Zertifikatspeicher auf dem Computer. Das folgende Beispiel erstellt eine entsprechende *CER* -Datei, die Sie beim Konfigurieren von P2S in Azure hochladen. Führen Sie den folgenden Befehl als Administrator an. Ersetzen Sie *ARMP2SRootCert* und *ARMP2SRootCert.cer* mit dem Namen für das Zertifikat verwenden möchten.<br><br>Das Zertifikat wird in Ihre Zertifikate - Aktueller Benutzer\Persönlich\Zertifikate befinden.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>Der öffentliche Schlüssel

Als Teil der VPN-Gateway-Konfiguration für Punkt-zu-Standort-Verbindung wird der öffentliche Schlüssel für das Stammzertifikat in Azure hochgeladen.

1. Um eine CER-Datei aus dem Zertifikat zu erhalten, öffnen Sie **certmgr.msc**. Maustaste selbstsigniertes Zertifikat, klicken Sie auf **Alle Tasks**und dann auf **Exportieren**. Der **Zertifikatsexport-Assistent**wird geöffnet.

2. **Klicken**Sie im Assistenten klicken Sie auf **Weiter**und wählen Sie **Nein, privaten Schlüssel nicht exportieren**.

3. Auf der Seite **Exportdateiformat** wählen **Base-64-codiert x. 509 (. CER).** Klicken Sie dann auf **Weiter**. 

4. Auf die **zu exportierende Datei**, **Suchen** Sie das Zertifikat exportieren möchten an. **Dateiname**Dateinamen Sie den Zertifikat aus. Klicken Sie auf **Weiter**.

5. Klicken Sie auf **Fertig stellen** , um das Zertifikat exportieren.

 
### <a name="export-the-self-signed-certificate-optional"></a>Exportieren Sie das selbstsignierte Zertifikat (optional)

Sie möchten das selbstsignierte Zertifikat exportieren und sicher aufbewahren. Wenn werden Sie später auf einem anderen Computer installieren und weitere Clients Zertifikate oder anderen CER-Datei exportieren. Jeder Computer ein Clientzertifikat installiert und ist auch mit dem entsprechenden VPN-Client konfiguriert Einstellungen auf das virtuelle Netzwerk über P2S herstellen können. Deshalb möchten Sie sicherstellen, dass Client-Zertifikate generiert und installiert nur bei Bedarf und das selbstsignierte Zertifikat sicher gespeichert sind.

Um das selbstsignierte Zertifikat als einer PFX-Datei exportieren, wählen Sie das Stammzertifikat und mit denselben Schritten Siehe [Exportieren eines Clientzertifikats](#clientkey) exportieren.

## <a name="create-and-install-client-certificates"></a>Erstellen und Installieren von Clientzertifikaten

Das selbstsignierte Zertifikat nicht direkt auf dem Clientcomputer installiert werden. Sie müssen ein Clientzertifikat selbstsigniertes Zertifikat zu generieren. Sie exportieren und das Clientzertifikat auf dem Clientcomputer installieren. Die folgenden Schritte sind nicht Bereitstellungsmodell spezifisch. Sie gelten für Ressourcenmanager und Classic.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Teil 1 – erzeugen Sie ein Client-Zertifikat aus einem selbstsignierten Zertifikat

Die folgenden Schritte durchgehen können ein Clientzertifikat aus ein selbstsigniertes Zertifikat zu generieren. Sie können mehrere Clientzertifikate von demselben Zertifikat erstellen. Jeder Client-Zertifikat kann exportiert und auf dem Clientcomputer installiert sein. 

1. Öffnen Sie auf demselben Computer verwendet das selbstsignierte Zertifikat erstellen ein Eingabeaufforderungsfenster als Administrator.

2. In diesem Beispiel bezieht sich auf das selbstsignierte Zertifikat, das Sie generiert "ARMP2SRootCert". 
    - Ändern Sie *"ARMP2SRootCert"* auf den Namen des selbstsignierten Stamm erstellen das Client-Zertifikat aus. 
    - Ändern Sie *ClientCertificateName* in den Namen ein Clientzertifikats zu generieren. 


    Ändern Sie, und führen Sie das Beispiel um ein Clientzertifikat zu generieren. Wenn Sie das folgende Beispiel ausführen, ohne Sie zu ändern, ist das Ergebnis ein Client-Zertifikat mit dem Namen ClientCertificateName in Ihrem persönlichen Zertifikatspeicher, das Stammzertifikat ARMP2SRootCert generiert wurde.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Alle Zertifikate werden in der "Zertifikate - Aktueller Benutzer\Persönlich\Zertifikate" auf Ihrem Computer speichern. Sie können wie viele Clientzertifikate auf dieses Verfahren.

### <a name="clientkey"></a>Teil 2 – exportieren Sie ein Clientzertifikat

1. Um ein Clientzertifikat zu exportieren, öffnen Sie **certmgr.msc**. Klicken Sie auf das Clientzertifikat zu exportieren, klicken Sie auf **Alle Tasks**und klicken Sie dann auf **Exportieren**. Der **Zertifikatsexport-Assistent**wird geöffnet.

2. **Klicken**Sie im Assistenten klicken Sie auf **Weiter**, und wählen Sie **Ja, privaten Schlüssel exportieren**.

3. Lassen Sie auf der Seite **Exportdateiformat** die Standardwerte übernehmen. Klicken Sie auf **Weiter**. 
 
4. Auf der Seite **Sicherheit** müssen Sie den privaten Schlüssel schützen. Wenn Sie ein Kennwort verwenden, stellen Sie sicher und leicht zu merken, die Sie für dieses Zertifikat festlegen. Klicken Sie auf **Weiter**.

5. Auf die **zu exportierende Datei**, **Suchen** Sie das Zertifikat exportieren möchten an. **Dateiname**Dateinamen Sie den Zertifikat aus. Klicken Sie auf **Weiter**.

6. Klicken Sie auf **Fertig stellen** , um das Zertifikat exportieren.  

### <a name="part-3---install-a-client-certificate"></a>Teil 3: Installieren eines Clientzertifikats

Jeder Client, der über eine Punkt-zu-Standort-Verbindung mit dem virtuellen Netzwerk herstellen möchten, müssen ein Clientzertifikat installiert. Dieses Zertifikat ist neben den erforderlichen VPN-Konfiguration. Die folgenden Schritte führen Sie durch Clientzertifikat manuell installieren.

1. Suchen Sie und kopieren Sie die *PFX* -Datei auf dem Clientcomputer. Doppelklicken Sie auf dem Clientcomputer auf die *PFX* -Datei zu installieren. Lassen Sie den **Speicherort** als **Aktueller Benutzer**, und klicken Sie auf **Weiter**.

2. Die **Datei** auf der Seite Importieren ändern Sie nicht. Klicken Sie auf **Weiter**.

3. Geben Sie auf der Seite **Sicherheit** das Kennwort für das Zertifikat verwendet, oder Überprüfen Sie das Sicherheitsprinzipal, das Zertifikat installieren, Sie klicken Sie auf **Weiter**.

4. Belassen Sie auf der Seite **Zertifikatspeicher** am Standardspeicherort, und klicken Sie auf **Weiter**.

5. Klicken Sie auf **Fertig stellen**. **Sicherheitshinweis** für das Zertifikat installieren klicken Sie auf **Ja**. Das Zertifikat ist nun erfolgreich importiert.

## <a name="next-steps"></a>Nächste Schritte

Die Punkt-zu-Standort-Konfiguration fort. 

- **Ressourcenmanager** Bereitstellung Modell finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung, ein VNet mithilfe von PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md). 
- **Klassische** Modell Schritte finden Sie unter [Konfigurieren einer Punkt-zu-Standort-VPN-Verbindung auf ein VNet mit dem Verwaltungsportal](vpn-gateway-point-to-site-create.md).
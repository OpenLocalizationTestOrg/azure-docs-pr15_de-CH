<properties 
    pageTitle="Data Management Gateway für "Data Factory" | Microsoft Azure"
    description="Ein datengateway zum Verschieben von Daten zwischen lokalen und der Cloud einrichten. Verwenden Sie Daten-Management Gateway in Azure Data Factory Daten verschieben." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="jingwang"/>

# <a name="data-management-gateway"></a>Data Management Gateway
Data Management Gateway eines Client-Agents, die Sie in Ihrer lokalen Umgebung Kopieren installieren wird zwischen Cloud und lokalen Datenspeicher. Die lokalen Datenspeicher von Data Factory unterstützt finden Sie im Abschnitt [unterstützte Datenquellen](data-factory-data-movement-activities.md##supported-data-stores) . 

> [AZURE.NOTE] Gateway unterstützt derzeit nur kopieraktivität und gespeicherte Aktivität in Data Factory. Kann nicht mit dem Gateway eine benutzerdefinierte Aktivität lokalen Datenquellen zugreifen. 

Dieser Artikel ergänzt die exemplarische Vorgehensweise in die [Verschieben von Daten zwischen lokalen und Datenspeichern](data-factory-move-data-between-onprem-and-cloud.md) Artikel. In der exemplarischen Vorgehensweise erstellen Sie eine Pipeline, die das Gateway zum Verschieben von Daten aus einer lokalen SQL Server-Datenbank in Azure Blob verwendet. Dieser Artikel enthält detaillierte ausführliche Informationen zu Data Management Gateway.   

## <a name="overview"></a>Übersicht

### <a name="capabilities-of-data-management-gateway"></a>Funktionen des Daten-Management-Gateways
Data Management Gateway bietet die folgenden Funktionen:

- Modell für lokale Datenquellen cloud Datenquellen innerhalb der gleichen Data Factory und Verschieben von Daten.
- Haben Sie zentralen Konsole zur Überwachung und Verwaltung Einblick in Gateway-Status von Blatt Data Factory.
- Verwalten Sie Zugriff auf lokale Daten sicher.
    - Keine Änderung erforderlich Unternehmensfirewall. Gateway wird nur ausgehende HTTP-basierte Verbindung Internet geöffnet.
    - Anmeldeinformationen für Ihre lokalen Datenspeicher mit dem Zertifikat verschlüsselt.
- Verschieben Sie Daten effizient – Daten parallel gegen zeitweise Netzwerkprobleme mit Wiederholungslogik.

### <a name="command-flow-and-data-flow"></a>Befehl und Daten
Wenn Sie eine kopieraktivität Kopieren von Daten zwischen lokalen und Cloud verwenden, verwendet die Aktivität Gateway übertragen von Daten aus lokalen Datenquelle auf Cloud und umgekehrt.

Hier allgemeine Daten für Fluss- und Zusammenfassung der Schritte mit Daten-Gateway: ![Datenfluss über Gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1.  Daten Entwickler erstellt ein Gateway mit dem [Azure-Portal](https://portal.azure.com) oder [PowerShell-Cmdlets](https://msdn.microsoft.com/library/dn820234.aspx)für eine Azure Daten. 
2.  Daten-Entwickler erstellt verknüpften Dienst für einen lokalen Datenspeicher durch das Gateway angeben. Zum Einrichten des verknüpften Diensts setzt Entwickler Daten Anwendung Anmeldeinformationen festlegen Authentifizierungstypen und Anmeldeinformationen an.  Dialogfeld Anwendung Anmeldeinformationen festlegen kommuniziert mit dem Datenspeicher Verbindung und das Gateway zum Speichern von Anmeldeinformationen testen.
3. Gateway verschlüsselt die Anmeldeinformationen mit dem Zertifikat zugeordneten Gateway (bereitgestellt durch Entwickler Daten), vor dem Speichern der Anmeldeinformationen in der Cloud.
4. Data Factorydienst kommuniziert mit dem Gateway für Planung und Verwaltung von Aufträgen über einen Kanal, der eine freigegebene Azure Service Bus-Warteschlange verwendet. Wenn ein Auftrag zum Kopieren von Aktivität gestartet muss, Warteschlangen Data Factory Anforderung mit Anmeldeinformationen. Gateway startet den Auftrag, nachdem die Warteschlange.
5.  Das Gateway entschlüsselt die Anmeldeinformationen mit demselben Zertifikat und verbindet mit der lokalen Datenspeicher mit entsprechenden Authentifizierungstyp und die Anmeldeinformationen.
6.  Das Gateway kopiert Daten aus einem lokalen Speicher, Cloud-Speicher (oder umgekehrt) je nach Konfiguration kopieren-Aktivität in der Datenpipeline. Für diesen Schritt kommuniziert das Gateway direkt mit Cloud-basierte Services wie Azure BLOB-Speicher über einen sicheren Kanal (HTTPS).

### <a name="considerations-for-using-gateway"></a>Aspekte der Verwendung von gateway
- Eine einzelne Instanz Data Management Gateway kann für mehrere lokale Datenquellen verwendet werden. Aber **eine einzelnes Gateway-Instanz ist nur eine Azure Data Factory gebunden** und kann nicht mit anderen Daten freigegeben werden.
- Sie können **nur eine Instanz des Daten-Management-Gateways** auf einem einzigen Computer installiert haben. Angenommen, Sie zwei Data Factorys, die lokalen Datenquellen zugreifen, müssen Sie Gateways auf lokalen Computern installieren. In anderen Worten Gateway an eine bestimmte Daten Factory gebunden
- **Gateway muss nicht auf demselben Computer wie der Datenquelle**. Allerdings schneller mit Gateway näher an die Datenquelle für das Gateway für die Verbindung zur Datenquelle. Wir empfehlen das Gateway auf einem Computer installieren, der anders ist, die lokale Datenquelle hostet. Bei dem Gateway und der Datenquelle auf unterschiedlichen Computern konkurriert Gateway nicht für Ressourcen für die Datenquelle.
- Sie können **mehrere Gateways auf verschiedenen Computern identisch mit lokalen Datenquelle**. Z. B. wird möglicherweise zwei Gateways mit zwei Data Factorys für lokale Datenquelle jedoch mit den Daten-Factorys.
- Haben Sie bereits ein Gateway installiert auf Ihrem Computer mit **Power BI** -Szenario, installieren Sie eine **separate Gateway für Azure Data Factory** auf einem anderen Computer.
- Gateway muss verwendet werden, wenn Sie **ExpressRoute**verwenden.
- Datenquelle als eine lokale Datenquelle (Firewall) behandeln auch, wenn Sie **ExpressRoute**verwenden. Mithilfe des Gateways Konnektivität zwischen dem Dienst und der Datenquelle her.
- Ist der Datenspeicher in der Cloud eine **Neuerung Azure**müssen **Gateway verwenden** . 

## <a name="installation"></a>Installation

### <a name="prerequisites"></a>Erforderliche Komponenten
- **Unterstützte Betriebssysteme** sind Windows 7, Windows 8 / 8.1 10 Windows, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Installation des Daten-Management-Gateways auf einem Domänencontroller wird derzeit nicht unterstützt.
- .NET Framework 4.5.1 oder höher ist erforderlich. Wenn Sie Gateway auf einem Computer mit Windows 7 installieren, installieren Sie.NET Framework 4.5 oder höher. Einzelheiten finden Sie unter [.NET Framework System Requirements](https://msdn.microsoft.com/library/8z6watww.aspx) . 
- Die empfohlene **Konfiguration** für den Gatewaycomputer ist mindestens 2 GHz, 4 Kernen, 8 GB RAM und 80-GB-Festplatte.
- Wenn der Hostcomputer im Ruhezustand reagiert das Gateway nicht auf Anfragen. Konfigurieren Sie daher einen entsprechenden **Energiesparplan** auf dem Computer vor der Installation des Gateways. Wenn der Computer den Ruhezustand konfiguriert ist, aufgefordert die Gateway-Installation.
- Sie müssen ein Administrator auf dem Computer installieren und konfigurieren Data Management Gateway erfolgreich sein. Sie können **Daten Management Gateway Benutzer** lokale Windows-Gruppe weitere Benutzer hinzufügen. Mitglieder dieser Gruppe können Daten Management Gateway-Konfigurations-Manager-Tool verwenden, um das Gateway zu konfigurieren. 

Wie Kopie Aktivität führt auf eine bestimmte Frequenz, folgt die Ressourcenverwendung (CPU, Speicher) auf dem Computer auch das gleiche Muster mit Spitzen- und Nebenzeiten. Ressourcenverwendung hängt auch die Menge der Daten verschoben werden. Wenn mehrere Einzelvorgänge ausgeführt werden, sehen Sie Ressourcenverwendung gehen während der Spitzenzeiten. 

### <a name="installation-options"></a>Installationsoptionen
Data Management Gateway kann auf folgenden Arten installiert werden: 

- Setup-Paket vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717)downloaden eine MSI-Datei.  Die MSI-Datei kann auch verwendet werden, erhalten Einstellungen vorhandenen Data Management Gateway auf die neueste Version aktualisieren.
- Über **herunterladen und Installieren von Daten-Gateway** Link manuelle Installation oder unter EXPRESS-Installation **auf diesem Computer installieren** . Siehe [Verschieben von Daten zwischen lokalen und](data-factory-move-data-between-onprem-and-cloud.md) eine schrittweise Anleitung zur Verwendung von express Setup. Manuelle Schritt gelangen Sie im Download Center.  Im nächsten Abschnitt werden die Schritte zum Herunterladen und Installieren von Gateway-Downloadcenter. 

### <a name="installation-best-practices"></a>Bewährte Methoden:
1.  Konfigurieren Sie Energiesparplan auf dem Hostcomputer für das Gateway, sodass der Computer nicht in den Ruhezustand. Wenn der Hostcomputer im Ruhezustand reagiert das Gateway nicht auf Anfragen.
2.  Sichern Sie das Zertifikat mit dem Gateway.

### <a name="install-gateway-from-download-center"></a>Gateway-Downloadcenter installieren
1. Navigieren Sie zur [Downloadseite für Microsoft Data Management Gateway](https://www.microsoft.com/download/details.aspx?id=39717). 
2. Klicken Sie auf **herunterladen**, wählen Sie die entsprechende Version (**32-Bit-** oder **64-Bit**) und klicken Sie auf **Weiter**. 
3. Führen Sie die **MSI-Datei** direkt auf der Festplatte speichern Sie und ausführen.
4. Klicken Sie auf der Seite **Willkommen** auf **Weiter**wählen Sie eine **Sprache** .
5. **Annehmen** der Endbenutzer-Lizenzvertrag, und klicken Sie auf **Weiter**. 
6. Wählen Sie **Ordner** installieren Sie das Gateway und auf **Weiter**. 
7. Klicken Sie auf der Seite **bereit zum Installieren** auf **Installieren**. 
8. Klicken Sie auf **Fertig stellen** , um die Installation abzuschließen.
9. Azure-Portal erhalten Sie den Schlüssel. Abschnitt der nächsten Anleitung. 
10. Führen Sie auf der Seite **Registrieren Gateway** **Data Management Gateway Configuration Manager** auf Ihrem Computer die folgenden Schritte aus: 
    1. Fügen Sie den Schlüssel im Text.
    2. Klicken Sie gegebenenfalls auf **Gateway-Schlüssel anzeigen** , um den Schlüssel Text.
    3. Klicken Sie auf **Registrieren**. 

### <a name="register-gateway-using-key"></a>Registrieren Sie Gateway mit Schlüssel

#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Wenn Sie bereits eine logische Gateway im Portal erstellt haben
Zum Erstellen eines Gateways im Portal und den Schlüssel aus dem Blade **Konfigurieren** , Schritte im Artikel [Verschieben von Daten zwischen lokalen und](data-factory-move-data-between-onprem-and-cloud.md) exemplarischen Vorgehensweise.    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Wenn Sie bereits das logische Gateway im Portal erstellt haben
1. Azure-Portal Blatt **Data Factory** navigieren Sie, und klicken Sie auf **Verknüpfte Dienste** .

    ![Data Factory blade](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Wählen Sie den logischen **Gateway** im Portal erstellten Blatt **Verknüpften Diensten** . 

    ![logische gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
2. Klicken Sie auf Blatt **Data Gateway** **herunterladen und Installieren von Daten-Gateway**.

    ![Download-Link im portal](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
3. Klicken Sie auf **neu erstellen Schlüssel**Blatt **Konfigurieren** . Klicken Sie auf Ja in der Warnung nach sorgfältig lesen.

    ![Schlüssel erstellen](media/data-factory-data-management-gateway/recreate-key-button.png)
4. Klicken Sie auf Schaltfläche neben der Taste kopieren. Der Schlüssel wird in die Zwischenablage kopiert.
    
    ![Schlüssel kopieren](media/data-factory-data-management-gateway/copy-gateway-key.png) 

### <a name="system-tray-icons-notifications"></a>Taskleistensymbole / Anträge
Die folgende Abbildung zeigt einige der Symbole, die Sie sehen. 

![Taskleistensymbole](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Wenn Sie System Tray Icon-Benachrichtigung Nachricht Cursor, Details über den Status der Operation Gateway-Update in einem Popup-Fenster angezeigt.

### <a name="ports-and-firewall"></a>Ports und firewall
Zwei Firewalls berücksichtigt werden müssen: **Unternehmensfirewall** ausgeführt zentralen Router der Organisation und der **Windows-Firewall** als Daemon auf dem lokalen Computer konfiguriert, auf dem Gateway installiert.  

![Firewalls](./media/data-factory-data-management-gateway/firewalls.png)

Ebene der Unternehmensfirewall müssen die folgenden Domänen und ausgehende Ports konfigurieren:

| Domänennamen | Anschlüsse | Beschreibung |
| ------ | --------- | ------------ |
| *. servicebus.windows.net | 443, 80 | Listener Service Bus Relay über TCP (443 für Zugriffskontrolle token Übernahme erforderlich) | 
| *. servicebus.windows.net | 5671 9350-9354 | Optionaler Service Bus Relay über TCP | 
| *. von Core.Windows.NET befinden. | 443 | HTTPS | 
| *. clouddatahub.net | 443 | HTTPS | 
| Graph.Windows.NET | 443 | HTTPS |
| Login.Windows.NET | 443 | HTTPS | 

Ebene der Windows-Firewall sind diese ausgehende Ports normalerweise aktiviert. Wenn nicht, Domänen und Ports entsprechend konfigurieren können auf Gatewaycomputer.

#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Daten Sie aus einem Datenspeicher Quelle eine Senke Datenspeicher

Sicherstellen Sie, dass die Firewall-Regeln auf der Windows-Firewall auf dem Gateway Unternehmensfirewall ordnungsgemäß aktiviert sind und der Datenspeicher selbst. Diese Regeln aktivieren ermöglicht das Gateway sowohl und erfolgreich auffangen. Aktivieren Sie Regeln für jeden Datenspeicher der Kopiervorgang beteiligt ist.

Z. B. um aus **ein lokalen Datenspeicher einer Azure SQL-Datenbank-Senke oder einer Azure SQL Data Warehouse Auffangen**kopieren, folgendermaßen Sie vor: 

- Ausgehende **TCP-** Kommunikation auf Port **1433** für die Windows-Firewall und Unternehmens-Firewall zulassen
- Konfigurieren der Firewall von SQL Azure-Server hinzufügen die IP-Adresse des Gateway-Computers zu der Liste der zulässigen IP-Adressen. 


### <a name="proxy-server-considerations"></a>Proxy Server-Aspekte
Wenn Ihr Unternehmen Netzwerk einen Proxyserver auf das Internet zugreifen, konfigurieren Sie Data Management Gateway zum entsprechenden Proxyeinstellungen. Sie können den Proxy Phase Registrierung festlegen. 

![Set Proxy während der Registrierung](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Gateway verwendet den Proxyserver zum Cloud-Dienst herstellen. Klicken Sie auf **Ändern** während des Setups. Sie sehen das Dialogfeld **Proxyeinstellungen** .

![Set Proxy mit Konfigurations-manager](media/data-factory-data-management-gateway/SetProxySettings.png)

Es gibt drei Optionen: 

- **Proxy nicht verwenden**: Gateway explizit verwendet keine Proxy zum Cloud-Diensten herstellen.
- **System-Proxy verwenden**: Gateway verwendet die Proxyeinstellung, der diahost.exe.config konfiguriert ist.  Wenn diahost.exe.config kein Proxy konfiguriert ist, verbindet Gateway mit Cloud-Dienst direkt und ohne Umweg über Proxy.
- **Verwenden benutzerdefinierten Proxy**: konfigurieren den HTTP-Proxy auf Gateway anstatt Konfigurationen in diahost.exe.config verwenden.  Adresse und Port sind erforderlich.  Benutzername und Kennwort sind optional, je nach Einstellung des Proxys.  Alle sind mit den Anmeldeinformationen des Gateways verschlüsselt und lokal auf dem Gateway.

Nach dem Speichern der aktualisierten Proxyeinstellungen Data Management Gateway Server Service automatisch neu gestartet. 

Nachdem Gateway möchten Sie anzeigen oder Aktualisieren von Proxyeinstellungen erfolgreich registriert wurde, verwenden Sie Data Management Gateway-Konfigurations-Manager. 

1. Data Management Gateway-Konfigurations-Manager zu starten.
2. Wechseln Sie zur Registerkarte **Einstellungen** .
3. Klicken Sie auf **Ändern** im Abschnitt **Http-Proxy** **Legen Sie HTTP-Proxy** -Dialogfeld gestartet.  
4. Nach dem Klicken auf die Schaltfläche **Weiter** um Ihre Erlaubnis zu speichern, die Proxyeinstellung Dienst für Gateway-Host Dialogfeld mit einer Warnung angezeigt.

Sie können anzeigen und Aktualisieren von HTTP-Proxy mit Configuration Manager-Tool. 

![Set Proxy mit Konfigurations-manager](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [AZURE.NOTE] Wenn Sie einen Proxy-Server mit NTLM-Authentifizierung einrichten, wird unter dem Domänenkonto Gateway Server Service ausgeführt. Wenn Sie das Kennwort für das Domänenkonto später ändern, müssen Sie update Einstellungen für den Dienst und starten Sie ihn entsprechend. Aufgrund dieser Anforderung empfehlen wir ein dediziertes Domänenkonto verwenden, um den Proxy-Server zugreifen, der nicht häufig Aktualisieren des Kennworts erforderlich ist.

### <a name="configure-proxy-server-settings-in-diahostexeconfig"></a>Konfigurieren von Proxyeinstellungen in diahost.exe.config
Bei Auswahl von **System-Proxy verwenden** für den HTTP-Proxy verwendet Gateway die Proxyeinstellung diahost.exe.config.  Wenn kein Proxy im diahost.exe.config angegeben ist, verbindet Gateway mit Cloud-Dienst direkt und ohne Umweg über Proxy. Die folgende Prozedur beschrieben Aktualisieren der Konfigurationsdatei. 

1.  Kopieren Sie im Datei-Explorer sicher C:\Program Files\Microsoft Daten Management Gateway\2.0\Shared\diahost.exe.config, um die Originaldatei zu sichern.
2.  Starten Sie Notepad.exe als Administrator, und öffnen Sie die Textdatei "C:\Program Files\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config. Sie finden das Standardtag für system.net, wie im folgenden Code dargestellt:

            <system.net>
                <defaultProxy useDefaultCredentials="true" />
            </system.net>   

    Anschließend können Sie Proxy Serverdetails hinzufügen, wie im folgenden Beispiel gezeigt:

            <system.net>
                  <defaultProxy enabled="true">
                        <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
                  </defaultProxy>
            </system.net>

    Zusätzliche Eigenschaften innerhalb des Proxy-Tags können die erforderlichen Einstellungen wie ScriptLocation. [Proxy Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) zur Syntax finden Sie unter.

            <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>

3. Speichern Sie die Konfigurationsdatei an der ursprünglichen Position und der Management-Gateway-Host Dienst die Änderungen nimmt. Dienst neu starten: Verwenden Sie Dienste-Applet vom Bedienfeld oder **Data Management Gateway-Konfigurations-Manager** > Klicken Sie auf die Schaltfläche **Beenden** den **Dienst starten**. Wenn der Dienst nicht gestartet wird, ist wahrscheinlich ein Syntaxfehler XML-Tag in der Konfigurationsdatei der Anwendung hinzugefügt wurde, das geändert wurde.     

Neben diesen Punkt müssen Sie auch sicherstellen, dass Microsoft Azure in weißen Liste Ihres Unternehmens ist. Die Liste der gültigen Microsoft Azure IP-Adressen kann im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653)heruntergeladen werden.

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mögliche Symptome für Firewall- und Proxy-Server-Probleme
Die folgenden Fehler auftreten, ist wahrscheinlich aufgrund einer fehlerhaften Konfiguration des Firewall oder Proxy-Server, Gateway mit Data Factory blockiert, um sich zu authentifizieren. Vorherigen Abschnitt Firewall sicherstellen und Proxy-Server ordnungsgemäß konfiguriert.

1.  Beim Gateway registrieren Sie die folgende Fehlermeldung: "Fehler beim Registrieren des Gateway-Schlüssels. Bevor Sie versuchen, registrieren den Gateway-Schlüssel erneut bestätigen, dass das Data Management Gateway verbunden und Data Management Gateway Server Service wird gestartet."
2.  Wenn Sie Configuration Manager öffnen, sehen Sie Status als "Getrennt" oder "Verbinden". Beim Anzeigen der Windows-Ereignisprotokolle unter "Ereignisanzeige" > "Anwendung und Dienstprotokolle" > "Data Management Gateway" Fehlermeldungen wie die folgenden angezeigt:`Unable to connect to the remote server` 
    `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Öffnen Sie Port 8050 für Anmeldeinformationen Verschlüsselung 
**Festlegen der Anmeldeinformationen** Anwendung verwendet den eingehenden Port **8050** Relay an das Gateway beim Einrichten einer lokalen Anmeldeinformationen verknüpft Dienst in Azure-Portal. Während der Installation von Gateway wird standardmäßig Daten Management Gateway-Installation auf dem Gatewaycomputer.
 
Wenn Sie eine Drittanbieter-Firewall verwenden, können Sie manuell Port 8050 öffnen. Wenn Sie Firewall-Problem beim Gateway-Setup ausführen, können Sie versuchen, mithilfe des folgenden Befehls Gateway Konfigurieren der Firewalls installieren.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Wenn Sie nicht den Port 8050 auf dem Gatewaycomputer öffnen, verwenden Sie Mechanismen als Anwendung **Anmeldeinformationen festlegen** Data Store Anmeldeinformationen konfigurieren. Beispielsweise können Sie [Neue AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) -PowerShell-Cmdlets. Finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](#set-credentials-and-securityy) Abschnitt wie Anmeldeinformationen Datenspeicher festgelegt werden kann.

## <a name="update"></a>Update 
Standardmäßig wird Data Management Gateway automatisch aktualisiert, wenn eine neuere Version des Gateways verfügbar ist. Das Gateway wird nicht aktualisiert, bis alle geplanten Aufgaben. Keine weiteren Aufgaben werden vom Gateway verarbeitet, bis der Aktualisierungsvorgang abgeschlossen ist. Schlägt die Aktualisierung fehl, wird das Gateway die alte Version zurückgesetzt. 

Geplanten Zeit der Aktualisierung in den folgenden Stellen angezeigt:

- Das Gateway Eigenschaften Blade im Azure-Portal.
- Homepage von Data Management Gateway Configuration Manager
- System Tray-Benachrichtigung. 

Startseite des Data Management Gateway-Konfigurations-Manager zeigt den Aktualisierungszeitplan und zuletzt das Gateway installiert/aktualisiert wurde. 

![Planen von updates](media/data-factory-data-management-gateway/UpdateSection.png)

Sie können das Update sofort installieren oder warten auf das Gateway zum geplanten Zeitpunkt automatisch aktualisiert werden. Das folgende Bild zeigt beispielsweise die Benachrichtigung dargestellt in Gateway Configuration Manager zusammen mit der Schaltfläche Update sofort installieren klicken. 

![Update im DMG-Konfigurations-Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Die Benachrichtigung in der Taskleiste sieht wie in der folgenden Abbildung dargestellt: 

![Systemnachricht Schacht](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Den Status des Aktualisierungsvorgangs (manuell oder automatisch) in der Taskleiste angezeigt. Gateway-Konfigurations-Manager beim nächsten Starten, wird eine Meldung auf der Benachrichtigungsleiste Gateway mit einem Link zu [Neues Thema](data-factory-gateway-release-notes.md)aktualisiert wurde angezeigt.

### <a name="to-disableenable-auto-update-feature"></a>Feature für automatische Updates aktivieren/deaktivieren
Sie können Aktivieren/Deaktivieren der AutoUpdate-Funktion durch die folgenden Schritte durchführen: 

1. Starten Sie Windows PowerShell auf dem Gatewaycomputer. 
2. Wechseln Sie zum Ordner C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript.
3. Der folgende Befehl zum Deaktivieren der AutoUpdate-Funktion deaktivieren (Disable) ausführen.   

        .\GatewayAutoUpdateToggle.ps1  -off

4. Um sie wieder einzuschalten: 
    
        .\GatewayAutoUpdateToggle.ps1  -on  

## <a name="configuration-manager"></a>Konfigurations-Manager 
Nach der Installation des Gateways können Sie Data Management Gateway Configuration Manager auf folgenden Arten starten: 

- Geben Sie im **Suchfenster** **Data Management Gateway** Zugriff auf dieses Programm. 
- Führen Sie die ausführbare Datei **ConfigManager.exe** im Ordner: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared** 
 
### <a name="home-page"></a>Startseite
Die Homepage können Sie die folgenden Aktionen ausführen: 

- Zeigen Sie Status des Gateways (verbunden mit Cloud-Dienst usw. an). 
- **Registrieren Sie** einen Schlüssel aus dem Portal.
- **Beenden** und Starten des **Data Management Gateway-Server-Dienst** auf dem Gatewaycomputer.
- **Planen von Updates** zu einem bestimmten Zeitpunkt der Tage.
- Zeigt das Datum an, wann das Gateway **aktualisiert wurde**. 

### <a name="settings-page"></a>Einstellungsseite
Die Einstellungsseite können Sie die folgenden Aktionen ausführen:

- Anzeigen, ändern und vom Gateway verwendete **Zertifikat** exportieren. Dieses Zertifikat wird zum Verschlüsseln von Datenquellen-Anmeldeinformationen verwendet.
- **HTTPS-Port** für den Endpunkt zu ändern. Das Tor öffnet einen Port für die Datenquellen-Anmeldeinformationen festlegen. 
- **Status** des Endpunkts
- **SSL-Zertifikat** anzeigen SSL-Kommunikation zwischen dem Portal und dem Gateway dient, Anmeldeinformationen für Datenquellen festzulegen.  

### <a name="diagnostics-page"></a>Diagnoseseite
Die Seite Diagnose können Sie die folgenden Aktionen ausführen:

- Aktivieren Sie die ausführliche **Protokollierung**, zeigen Sie Protokolle in der Ereignisanzeige an und senden Sie Protokolle an Microsoft, wenn ein Fehler aufgetreten.
- **Test-Verbindung** zu einer Datenquelle.  

### <a name="help-page"></a>Hilfe
Die Hilfeseite zeigt die folgenden Informationen:  

- Kurze Beschreibung des Gateways
- Versionsnummer
- Links zu online-Hilfe, Datenschutz und Lizenzvertrag.  

## <a name="troubleshooting"></a>Problembehandlung

- Sie finden Informationen in Gateway im Windows-Ereignisprotokoll protokolliert. Finden sie mithilfe der Windows **-Ereignisanzeige** unter **Anwendung und Services** > **Datenverwaltungsgateway**. Gateway-bezogenen Problemen, suchen Sie auf Fehlerereignisse im Viewer.
- Hält das Gateway nach **das Zertifikat zu ändern**, starten Sie den **Data Management Gateway Service** mit dem Microsoft Data Management Gateway Configuration Manager-Tool oder Diensten Systemsteuerungsoption. Wenn der Fehler weiterhin angezeigt wird, müssen Sie explizite Berechtigungen für Data Management Gateway Service-Benutzer das Zertifikat-Manager Zertifikate (certmgr.msc) auf.  Die Standard-Benutzerkonto für den Dienst: **NT Service\DIAHostService**. 
- Ausfall die **Anmeldeinformations-Manager** -Anwendung zum **Verschlüsseln** von Anmeldeinformationen Sie verschlüsseln Daten Factory-Editor klicken, stellen Sie sicher, dass diese Anwendung auf dem **Gatewaycomputer**ausgeführt werden. Wenn nicht, führen Sie die Anwendung auf dem Gatewaycomputer und versuchen Sie, Anmeldeinformationen zu verschlüsseln.  
- Wenn Datenspeicher Verbindung oder Treiber-Fehler angezeigt wird, gehen Sie folgendermaßen vor: 
    - Starten Sie auf dem Gatewaycomputer **Data Management Gateway-Konfigurations-Manager** .
    - Wechseln Sie zur Registerkarte " **Diagnose** "
    - Geben Sie wählen/geeignete Werte für die Felder in der Gruppe **Verbindung testen, um eine lokale Datenquelle dieses Gateway**
    - Klicken Sie auf **Verbindung testen** , um festzustellen, ob lokale Datenquelle unter Verwendung der Verbindungsinformationen und Gateway-Computer herstellen können. Das Testen der Verbindung nicht nach dem Installieren eines Treibers, starten Sie das Gateway, die neueste Änderung übernehmen.  

    ![Testverbindung](./media/data-factory-data-management-gateway/TestConnection.png)

### <a name="send-gateway-logs-to-microsoft"></a>Gateway-Protokolle an Microsoft senden
Wenn Microsoft Support mit Gateway Problembehandlung Hilfe wenden, müssen Sie eventuell die Gateway-Protokolle freigeben. Die Version des Gateways können Sie teilen erforderlich Gateway Protokolle über zwei Mausklicks in Gateway-Konfigurations-Manager.   

1. Wechseln Sie zur Registerkarte **Diagnose** des Gateway-Konfigurations-Manager.
 
    ![Management-Gateway - Registerkarte Diagnose](media/data-factory-data-management-gateway/data-management-gateway-diagnostics-tab.png)
2. Klicken Sie auf **Protokolle** Link, um das Dialogfeld anzuzeigen: 

    ![Management-Gateway - Protokolle senden](media/data-factory-data-management-gateway/data-management-gateway-send-logs-dialog.png)
3. (optional) Klicken Sie auf **Ansicht Protokolle** der Ereignisanzeige überprüfen.
4. (optional) Klicken Sie auf **Privacy** Microsoft online Services-Datenschutzrichtlinie überprüfen. 
3. Sobald Sie zufrieden mit hochzuladen, klicken Sie auf **Protokolle senden** tatsächlich Protokolle letzten sieben Tage an Microsoft zur Problembehandlung senden. Den Status der Sendevorgang Protokolle wie in der folgenden Abbildung gezeigt sollte angezeigt werden:

    ![Data Management Gateway - Protokolle senden status](media/data-factory-data-management-gateway/data-management-gateway-send-logs-status.png)
4. Sobald der Vorgang abgeschlossen ist, wird ein Dialogfeld wie in der folgenden Abbildung dargestellt angezeigt:
    
    ![Data Management Gateway - Protokolle senden status](media/data-factory-data-management-gateway/data-management-gateway-send-logs-result.png)
5. Notieren Sie die **Berichts-ID** und Microsoft Support freigeben. Die ID wird zu Ihrem Gateway-Protokolle für die Behandlung von hochgeladenen verwendet.  Die ID wird in der Ereignisanzeige für den Verweis ebenfalls gespeichert.  Sie können es anhand der Ereignis-ID "25" und überprüfen Sie Datum und Uhrzeit.
    
    ![Data Management Gateway - Protokolle senden Berichts ID](media/data-factory-data-management-gateway/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Archiv-Gateway protokolliert auf gateway
Es gibt einige Szenarien, in denen Sie Gateway Probleme und Gateway-Protokolle können nicht direkt freigegeben: 

- Manuell das Gateway installieren und registrieren das Gateway;
- Sie versuchen, das Gateway mit einem neu generierten Schlüssel Configuration Manager zu registrieren. 
- Sie versuchen, Protokolle und Hostdienst Gateway kann nicht verbunden werden;

In solchen Fällen können Sie Gateway-Protokolle als Zip-Datei speichern und Microsoft später Supportanfragen freigeben. Beispielsweise wenn eine Fehlermeldung während der Registrierung als Gateway angezeigt in der folgenden Abbildung:   

![Management-Gateway - Registrierungsfehler](media/data-factory-data-management-gateway/data-management-gateway-registration-error.png)

Klicken Sie auf **Archiv Gateway** Protokolle zum Archivieren und speichern und dann die Zip-Datei mit Microsoft Support. 

![Management-Gateway - Protokolle archivieren](media/data-factory-data-management-gateway/data-management-gateway-archive-logs.png)

### <a name="gateway-is-online-with-limited-functionality"></a>Gateway wird mit eingeschränkter Funktionalität 
Status des Gateway wird Online **mit eingeschränkten Funktionen** aus einem der folgenden Gründe angezeigt.

- Cloud-Dienst über Servicebus herstellen nicht Gateway.
- Gateway Servicebus herstellen nicht Cloud-Dienst.

Bei Gateway mit eingeschränkter Funktionalität mithilfe des Assistenten zum Kopieren von Factory Datenpipelines zum Kopieren von Daten in lokalen Datenspeichern erstellen möglicherweise nicht.

Auflösung/Workaround für dieses Problem (online mit eingeschränkter Funktionalität) basiert, ob Gateway mit Cloud-Dienst oder die Art und Weise verbinden kann. Die folgenden Abschnitte enthalten diese Abhilfen. 

#### <a name="gateway-cannot-connect-to-cloud-service-through-service-bus"></a>Cloud-Dienst über Servicebus herstellen nicht Gateway.
Folgendermaßen Sie zu Gateway wieder online 

1. Ausgehende Ports 9350 9354 sowohl die Windows-Firewall auf Gateway und Unternehmens-Firewall zu aktivieren. [Ports und Firewalls](#ports-and-firewall) Siehe Details.
2. Konfigurieren Sie auf dem Gateway Proxyeinstellungen. Siehe Einzelheiten [Proxy Server Considerations](#proxy-server-considerations) . 

Um dieses Problem zu umgehen verwenden Sie Data Factory-Editor in Azure Portal (oder) Visual Studio (oder) Azure PowerShell.

#### <a name="error-cloud-service-cannot-connect-to-gateway-through-service-bus"></a>Fehler: Cloud-Dienst nicht mit Gateway Servicebus verbinden.
Folgendermaßen Sie zu Gateway wieder online
 
1. Ausgehende Ports 5671 und 9350 9354 sowohl die Windows-Firewall auf Gateway und Unternehmens-Firewall zu aktivieren. [Ports und Firewalls](#ports-and-firewall) Siehe Details.
2. Konfigurieren Sie auf dem Gateway Proxyeinstellungen. Siehe Einzelheiten [Proxy Server Considerations](#proxy-server-considerations) .
3. Entfernen Sie statische IP-Beschränkung auf dem Proxyserver. 

Als Workaround können Sie Data Factory-Editor in Azure-Portal () Visual Studio bzw.) Azure PowerShell.
 
## <a name="move-gateway-from-a-machine-to-another"></a>Gateway von einem Computer auf einen anderen verschieben
Dieser Abschnitt enthält die Schritte zum Verschieben Gatewayclient von einem Computer auf einen anderen Computer. 

2. Im Portal **Data Factory-Homepage**navigieren Sie, und klicken Sie auf die Kachel **Verknüpften Diensten** . 

    ![Data Link-Gateways](./media/data-factory-data-management-gateway/DataGatewaysLink.png) 
3. Wählen Sie Ihr Gateway im Abschnitt **DATENGATEWAYS** **Verknüpfte** Dienste.
    
    ![Verknüpfte Dienste Blade mit Gateway ausgewählt](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
4. Klicken Sie auf Blatt **datengateway** **herunterladen und Installieren von Daten-Gateway**.
    
    ![Gateway Downloadlink](./media/data-factory-data-management-gateway/DownloadGatewayLink.png) 
5. Blatt **Konfigurieren** klicken Sie auf **herunterladen und Installieren von Daten-Gateway**und Anleitung datengateway auf dem Computer installieren. 

    ![Blade konfigurieren](./media/data-factory-data-management-gateway/ConfigureBlade.png)
6. **Microsoft Data Management Gateway Configuration Manager** geöffnet lassen. 
 
    ![Konfigurations-Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png) 
7. Blatt **Konfigurieren** im Portal auf der Befehlsleiste auf **Schlüssel erstellen** und klicken Sie auf **Ja** , um die Warnung. Klicken Sie auf die **Schaltfläche Kopieren** Sie neben Schlüssel Text, der den Schlüssel in die Zwischenablage kopiert. Das Gateway auf dem alten Computer funktioniert wie bald den Schlüssel neu erstellen.  
    
    ![Schlüssel erstellen](./media/data-factory-data-management-gateway/RecreateKey.png)
     
8. Fügen Sie den **Schlüssel** im Textfeld auf der Seite **Registrieren Gateway** **Data Management Gateway Configuration Manager** auf Ihrem Computer. (optional) Klicken Sie auf Kontrollkästchen **Gateway-Schlüssel anzeigen** , um den Schlüssel Text anzuzeigen. 
 
    ![Schlüssel kopieren und registrieren](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
9. Klicken Sie auf **Registrieren** , um Gateway mit Cloud-Dienst zu registrieren.
10. Klicken Sie auf **der Registerkarte** **Ändern** , das mit dem alten Gateway verwendet dasselbe Zertifikat aktivieren, geben das **Kennwort**und klicken Sie auf **Fertig stellen**. 
 
    ![Geben Sie](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

    Exportieren eines Zertifikats vom alten Gateway durch die folgenden Schritte: Data Management Gateway Configuration Manager auf dem alten Computer zu starten, wechseln Sie zur Registerkarte **Zertifikat** **Exportieren** klicken und gehen. 
10. Nach der erfolgreichen Registrierung von Gateway erhalten Sie **Registrierung** legen auf **registriert** und **Status** auf **gestartet** auf der Homepage des Gateway-Konfigurations-Manager. 

## <a name="encrypting-credentials"></a>Verschlüsseln der Anmeldeinformationen 
Verschlüsselung der Anmeldeinformationen im Werk-Editor folgendermaßen Sie vor:

1. Starten Sie Web-Browser auf dem **Gatewaycomputer**zu, navigieren Sie zum [Azure-Portal](http://portal.azure.com). Suchen nach Daten Werk ggf. geöffneten Factory **DATA FACTORY** Blatt und dann auf **Autor & bereitstellen** Data Factory-Editor starten.   
1. Klicken Sie auf einen vorhandenen **verknüpften Serviceartikel** in der Strukturansicht die JSON-Definition oder einen verknüpften Dienst erstellen, der eine Management-Gateway erfordert (z. B.: SQL Server oder Oracle). 
2. Geben Sie den Namen des Gateways im Editor JSON **GatewayName** -Eigenschaft. 
3. Geben Sie Server für **die Datenquelleneigenschaft** **ConnectionString**.
4. Geben Sie die Datenbanknamen für die **Initial Catalog** -Eigenschaft **ConnectionString**.    
5. Klicken Sie auf Schaltfläche in der Befehlsleiste, die auf startet **Verschlüsseln** -einmal **Anmeldeinformations-Manager** -Anwendung. Das Dialogfeld **Anmeldeinformationen festlegen** sollte angezeigt werden. 
    ![Dialogfeld für Anmeldeinformationen festlegen](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
6. Führen Sie die folgenden Schritte im Dialogfeld **Anmeldeinformationen festlegen** :  
    1.  Wählen Sie **Authentifizierung** Data Factory Dienst Verbindung zur Datenbank verwendet werden soll. 
    2.  Geben Sie die Benutzer Zugriff auf die Datenbank für **Benutzernamen** festlegen. 
    3.  Kennwort für den Benutzer für **das Kennwort** .  
    4.  Klicken Sie auf **OK** , um Anmeldeinformationen zu verschlüsseln und schließen das Dialogfeld. 
5.  **EncryptedCredential** -Eigenschaft in **ConnectionString** sollte jetzt angezeigt werden.      
        
            {
                "name": "SqlServerLinkedService",
                "properties": {
                    "type": "OnPremisesSqlServer",
                    "description": "",
                    "typeProperties": {
                        "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                        "gatewayName": "adftutorialgateway"
                    }
                }
            }

Wenn Sie das Portal auf einem Computer, die Gatewaycomputer unterscheidet zugreifen, müssen Sie sicherstellen, dass der Anmeldeinformations-Manager Application Gateway-Computer eine Verbindung herstellen kann. Wenn die Anwendung den Gatewaycomputer erreichen kann, ist es nicht Anmeldeinformationen für die Datenquelle und Verbindung mit der Datenquelle testen zulässig.  

Bei Anwendung der **Einstellung Anmeldeinformationen** verschlüsselt das Portal die Anmeldeinformationen mit dem angegebenen **Zertifikat** Registerkarte **Gateway Configuration Manager** auf dem Gatewaycomputer. 

Wenn eine API-Ansatz zum Verschlüsseln von Anmeldeinformationen suchen, können das [Neue AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) -PowerShell-Cmdlet Sie Anmeldeinformationen verschlüsseln. Das Cmdlet verwendet das Zertifikat dieses Gateway konfiguriert verwenden, um die Anmeldeinformationen zu verschlüsseln. **EncryptedCredential** Element der **ConnectionString** in JSON hinzugefügt verschlüsselte Anmeldeinformationen. Verwenden Sie die JSON mit dem [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) -Cmdlet oder im Werk-Editor. 

    "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

Es gibt eine weitere Möglichkeit zum Festlegen von Anmeldeinformationen mit Factory. Verknüpften SQL Server-Dienst mit dem Editor erstellen und Anmeldeinformationen in Klartext sind die Anmeldeinformationen verschlüsselt mit einem Zertifikat, das der Dienst Data Factory besitzt. Das Zertifikat wird nicht verwendet, Gateway wird konfiguriert. Während dieser Ansatz in einigen Fällen etwas schneller sein könnte, ist weniger sicher. Daher empfehlen wir, dass dieses nur für Entwicklung/Tests vorgehen. 


## <a name="powershell-cmdlets"></a>PowerShell-cmdlets 
Dieser Abschnitt beschreibt das Erstellen und Registrieren eines Gateways Azure PowerShell-Cmdlets verwenden. 

1. Starten Sie im Administratormodus **Azure PowerShell** . 
2. Melden Sie sich Ihre Azure-Konto durch den folgenden Befehl ausführen und Azure Anmeldeinformationen an. 

    Login-AzureRmAccount
2. Verwenden Sie das Cmdlet **Neu AzureRmDataFactoryGateway** logische Gateway wie folgt erstellen:

        $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>

    **Beispiel und Ausgabe**:


        PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

        Name              : MyGateway
        Description       : gateway for walkthrough
        Version           :
        Status            : NeedRegistration
        VersionStatus     : None
        CreateTime        : 9/28/2014 10:58:22
        RegisterTime      :
        LastConnectTime   :
        ExpiryTime        :
        ProvisioningState : Succeeded
        Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI

    
4. Wechseln Sie zum Ordner Azure PowerShell: * *C:\Program Files\Microsoft Data Management Gateway\2.0\PowerShellScript\**. Ausführen * *RegisterGateway.ps1* * zugeordnete lokale Variable * *$Key** wie im folgenden dargestellt. Dieses Skript registriert den Client-Agent auf dem Computer mit dem zuvor erstellten logischen Gateway installiert.

        PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
        
        Agent registration is successful!

    Sie können das Gateway auf einem Remotecomputer mithilfe des IsRegisterOnRemoteMachine-Parameters registrieren. Beispiel:
        
        .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true

5. Das Cmdlet " **Get-AzureRmDataFactoryGateway** " können Sie die Liste mit Gateways im Werk Daten abrufen. Wenn **Status** **online**angezeigt wird, bedeutet dies, dass das Gateway verwendet werden kann.

        Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF

Entfernen mithilfe des Cmdlets **Entfernen AzureRmDataFactoryGateway** Gateway und Beschreibung für das Gateway mit **Set-AzureRmDataFactoryGateway** -Cmdlets aktualisiert. Syntax und andere Details zu diesen Cmdlets finden Sie unter Data Factory Cmdlet Reference.  

### <a name="list-gateways-using-powershell"></a>Liste-Gateways mit PowerShell

    Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup

### <a name="remove-gateway-using-powershell"></a>Entfernen Sie Gateway mit PowerShell
    
    Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force 


## <a name="next-steps"></a>Nächste Schritte
- Siehe [Verschieben von Daten zwischen lokalen und Datenspeichern](data-factory-move-data-between-onprem-and-cloud.md) Artikel. In der exemplarischen Vorgehensweise erstellen Sie eine Pipeline, die das Gateway zum Verschieben von Daten aus einer lokalen SQL Server-Datenbank in Azure Blob verwendet.  

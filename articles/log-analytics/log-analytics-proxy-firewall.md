<properties
    pageTitle="Proxy- und Firewall konfigurieren in Protokollanalyse | Microsoft Azure"
    description="Konfigurieren Sie Proxy- und Firewall bestimmte Ports verwenden Ihre Agenten oder OMS-Dienste müssen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Konfigurieren Sie Proxy- und Firewall in Protokollanalyse

Aktionen für Proxy konfigurieren und Firewall für Protokollanalyse OMS unterscheiden sich bei Verwendung Operations Manager und dessen Agenten gegenüber Microsoft überwachen Agents, die direkt mit Servern verbunden. Überprüfen Sie die folgenden Abschnitte für den Typ des Agenten, die Sie verwenden.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Konfigurieren Sie Proxyserver und Firewall mit Microsoft Monitoring Agent

Microsoft Monitoring Agent und beim OMS-Dienst registrieren müssen sie Zugriff auf die Portnummer der Domänen und URLs. Wenn Sie einen Proxyserver für die Kommunikation zwischen dem Agent und dem OMS-Dienst verwenden, müssen Sie sicherstellen, dass geeigneten Ressourcen zugegriffen werden kann. Wenn Sie eine Firewall verwenden, den Zugriff auf das Internet beschränken, müssen Sie Ihre Firewall zugänglich OMS konfigurieren. Die folgenden Tabellen sind die Ports, die OMS.

|**Agent-Ressource**|**Anschlüsse**|**HTTPS-Überprüfung umgehen**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443|Ja|
|\*. oms.opinsights.azure.com|443|Ja|
|\*. blob.core.windows.net|443|Ja|
|ODS.systemcenteradvisor.com|443| |

Nachfolgend können Sie Proxyeinstellungen für Microsoft Monitoring Agent Systemsteuerung konfigurieren. Sie müssen das Verfahren für jeden Server verwenden. Haben Sie viele Server, die Sie konfigurieren möchten, können Sie ein Skript verwenden, um diesen Prozess zu automatisieren einfacher. Dann finden Sie im nächste Verfahren [Proxyeinstellungen für Microsoft Monitoring Agent mithilfe eines Skripts konfigurieren](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>So konfigurieren Sie Proxyeinstellungen für Microsoft Monitoring Agent mithilfe der Systemsteuerungsoption

1. **Bedienfeld**zu öffnen.

2. Öffnen Sie **Microsoft Agent überwachen**.

3. Klicken Sie auf die Registerkarte **Proxyeinstellungen** .<br>  
  ![Proxyeinstellungen (Registerkarte)](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Wählen Sie **einen Proxyserver verwenden** und geben Sie die URL und Anschlussnummer, sofern erforderlich, wie im Beispiel gezeigt. Wenn der Proxyserver Authentifizierung erfordert, geben Sie den Benutzernamen und das Kennwort für den Proxyserver zugreifen.

Gehen Sie folgendermaßen vor, um ein PowerShell-Skript zu erstellen, die Sie ausführen können, um die Proxyeinstellungen für jeden Agent festgelegt, die direkt mit Servern verbunden.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>So konfigurieren Sie Proxyeinstellungen für Microsoft Monitoring Agent mithilfe eines Skripts

Kopieren Sie im folgende Beispiel, mit Informationen zu Ihrer Umgebung aktualisieren Sie mit der Erweiterung PS1 speichern Sie und führen Sie das Skript auf jedem Computer, an den OMS-Dienst verbunden.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Proxyserver und Firewall mit Operations Manager konfigurieren

Ein Operations Manager-Verwaltungsgruppe und beim OMS-Dienst registrieren müssen sie Zugriff auf die Portnummern Ihrer Domänen und URLs. Wenn Sie einen Proxyserver für die Kommunikation zwischen dem Operations Manager-Verwaltungsserver und OMS-Dienst verwenden, müssen Sie sicherstellen, dass geeigneten Ressourcen zugegriffen werden kann. Wenn Sie eine Firewall verwenden, den Zugriff auf das Internet beschränken, müssen Sie Ihre Firewall zugänglich OMS konfigurieren. Auch wenn ein Operations Manager-Verwaltungsserver nicht hinter einem Proxyserver ist möglicherweise Beauftragten. In diesem Fall sollte der Proxyserver auf die gleiche Weise konfiguriert werden Agenten um und Sicherheit und Protokollmanagement-Lösungsdaten an, die OMS-Webdienst.

Damit Operations Manager-Agents mit OMS-Dienst kommunizieren kann müssen die Operations Manager-Infrastruktur (einschließlich Mitarbeiter) die richtigen Proxyeinstellungen und Version. Die Proxyeinstellung für Agents wird in der Operations Manager-Verwaltungskonsole angegeben. Die Version sollte eines der folgenden sein:

- Operations Manager 2012 SP1 Updaterollup 7 oder höher
- Operations Manager 2012 R2 Updaterollup 3 oder höher


Den folgenden Tabellen sind die folgenden Aufgaben-Ports.

>[AZURE.NOTE] Die folgenden Ressourcen schweigen Advisor und betriebliche Informationen wurden beide Vorgängerversionen von OMS. Ressourcen werden jedoch in Zukunft ändern.

Hier wird eine Liste der Ressourcen und Ports:<br>

|**Agent-Ressource**|**Anschlüsse**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443|
|\*. oms.opinsights.azure.com|443|
|\*.BLOB.Core.Windows.NET/\*|443|
|ODS.systemcenteradvisor.com|443|
<br>
Hier wird eine Liste der Serverressourcen und Ports:<br>

|**Management-Server-Ressource**|**Anschlüsse**|**HTTPS-Überprüfung umgehen**|
|--------------|-----|--------------|
|Service.systemcenteradvisor.com|443| |
|\*. service.opinsights.azure.com|443| |
|\*. blob.core.windows.net|443|Ja| 
|Data.systemcenteradvisor.com|443| | 
|ODS.systemcenteradvisor.com|443| | 
|\*. ods.opinsights.azure.com|443|Ja| 
<br>
Hier ist eine Liste von OMS und Operations Manager-Konsole und Ports.<br>

|**OMS und Operations Manager-Konsole Ressource**|**Anschlüsse**|
|----|----|
|Service.systemcenteradvisor.com|443|
|\*. service.opinsights.azure.com|443|
|\*. live.com|Port 80 und 443|
|\*. microsoft.com|Port 80 und 443|
|\*. microsoftonline.com|Port 80 und 443|
|\*. mms.microsoft.com|Port 80 und 443|
|Login.Windows.NET|Port 80 und 443|
<br>

Gehen Sie die Operations Manager-Verwaltungsgruppe mit OMS-Dienst registrieren. Verwenden Sie zwischen der Verwaltungsgruppe und der OMS-Dienst haben, die Validierungsverfahren Datenübertragung OMS-Dienst zu beheben.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Ausnahmen für die OMS-Endpunkte anfordern

1. Verwenden Sie die Informationen aus der ersten Tabelle sind die Mittel für den Operations Manager-Verwaltungsserver über Firewalls zuvor vorgestellten haben kann.
2. Verwenden Sie die Informationen aus der zweiten Tabelle zuvor angezeigt, um sicherzustellen, dass die Ressourcen für die Betriebskonsole in Operations Manager und OMS über Firewalls zugegriffen werden kann.
3. Wenn Sie einen Proxyserver in Internet Explorer verwenden, daß konfiguriert ist und ordnungsgemäß funktioniert. Um zu überprüfen, können Sie eine sichere Verbindung (HTTPS) beispielsweise [https://bing.com](https://bing.com)öffnen. Wenn die sichere Internet-Verbindung in einem Browser funktioniert, funktioniert es nicht wahrscheinlich in der Operations Manager-Verwaltungskonsole mit Webdiensten in der Cloud.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>So konfigurieren Sie den Proxyserver in der Operations Manager-Konsole

1. Operations Manager-Konsole und wählen Sie den Arbeitsbereich für die **Administration** .

2. **Betriebliche Informationen**erweitern und **Operative Einblicke Verbindung**wählen.<br>  
    ![Operations Manager OMS-Verbindung](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. Klicken Sie in die OMS-Verbindung **Proxyserver konfigurieren**.<br>  
    ![Operations Manager OMS Verbindung Proxyserver](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. In Betrieb Insights-Assistent: Proxyserver **Proxyserver betriebliche Insights-Webdienst auf**und geben Sie die URL mit dem Anschluss Nummer, beispielsweise **Http://myproxy:80**.<br>  
    ![Operations Manager OMS-Proxyadresse](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Anmeldeinformationen angeben, wenn der Proxyserver Authentifizierung erfordert
 Proxy Serveranmeldeinformationen und Einstellungen müssen auf verwalteten Computern übertragen, die OMS meldet. Dieser Server sollte in *Microsoft System Center Advisor Monitoring Server-Gruppe*. Anmeldeinformationen werden verschlüsselt in der Registrierung jedes Servers in der Gruppe.

1. Operations Manager-Konsole und wählen Sie den Arbeitsbereich für die **Administration** .
2. Wählen Sie unter **RunAs Configuration** **Profile**.
3. Öffnen Sie das **System Center Advisor ausführen als Profil Proxy** Profil.  
    ![System Center Advisor ausführen als Proxy-Profil](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Das Ausführen als Profil-Assistenten klicken Sie auf **Hinzufügen** , um ein Konto ausführen als verwenden. Sie können neues Ausführen als Konto erstellen oder ein vorhandenes Konto verwenden. Dieses Konto muss über ausreichende Berechtigungen zum Proxyserver durchlaufen.  
    ![Bild des Ausführen als Profil-Assistenten](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Legen Sie das Konto verwalten auch **ausgewählte Klasse, Gruppe oder Objekt** das Objektsuche öffnen.  
    ![Bild des Ausführen als Profil-Assistenten](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Suchen und wählen Sie dann **Microsoft System Center Advisor Monitoring Server-Gruppe**.  
    ![Image-Objekt Suchfeld](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Klicken Sie auf **OK** , um im Ausführen als Konto hinzufügen zu schließen.  
    ![Bild des Ausführen als Profil-Assistenten](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Führen Sie den Assistenten und speichern Sie das.  
    ![Bild des Ausführen als Profil-Assistenten](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Überprüfen, dass OMS-Management Packs heruntergeladen werden

Wenn Sie OMS Solutions hinzugefügt haben, können Sie diese managementpacks unter **Verwaltung**in der Operations Manager-Konsole anzeigen. Suche nach *System Center Advisor* schnell finden.  
    ![managementpacks heruntergeladen](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) oder, auch für OMS managementpacks mit den folgenden Windows PowerShell-Befehl in der Operations Manager-Verwaltungsserver:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Überprüfen, dass Operations Manager sendet die Daten an den OMS-Dienst

1. Operations Manager-Verwaltungsserver öffnen Sie Systemmonitor (perfmon.exe) zu, und wählen Sie **Systemmonitor**.
2. Klicken Sie auf **Hinzufügen**, und wählen Sie **Health Service-Verwaltungsgruppen**.
3. Fügen Sie die Leistungsindikatoren, die mit **HTTP**beginnen.  
    ![Leistungsindikatoren hinzufügen](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Wenn die Operations Manager-Konfiguration gut für Health Service Management Zähler Ereignisse und andere Aktivitäten angezeigt ist, basierend auf den managementpacks, die Sie in OMS und die konfigurierten Protokoll-Auflistung hinzugefügt.  
    ![Performance Monitor zeigt Aktivität](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Nächste Schritte

- [Protokollanalyse hinzufügen Lösungen Lösungskatalog](log-analytics-add-solutions.md) Funktionen und Daten.
- Vertraut mit [Protokoll suchen](log-analytics-log-searches.md) Solutions ausführlichen Informationen anzeigen.

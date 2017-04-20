<properties
    pageTitle="Verbinden von Windows-Computern mit Protokollanalyse | Microsoft Azure"
    description="In diesem Artikel werden die Schritte zum Windows-Computern in Ihrem lokalen Infrastruktur direkt OMS Verbindung eine angepasste Version des Microsoft Monitoring Agent (MMA)."
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
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# <a name="connect-windows-computers-to-log-analytics"></a>Protokollanalyse Windows-Computern herstellen

In diesem Artikel werden die Schritte zum Windows-Computern in Ihrem lokalen Infrastruktur direkt OMS Arbeitsbereiche Verbindung eine angepasste Version des Microsoft Monitoring Agent (MMA). Sie müssen installieren und Agents für alle gewünschten Computer integrierte OMS in Reihenfolge für diese OMS Daten senden, anzeigen und Bearbeiten von Daten in die OMS-Portal. Jeder Agent kann mehrere Arbeitsbereiche Bericht.

Installieren Sie Agents mithilfe von Setup-Befehlszeile oder mit gewünschten Zustand Konfiguration (DSC) in Azure Automation.  

>[AZURE.NOTE] Für virtuelle Computer kann in Azure ausgeführt Installation vereinfachen die [VM-Erweiterung](log-analytics-azure-vm-extension.md).

Auf Computern mit Internetzugang verwendet der Agent die Verbindung zum Internet OMS Daten an. Für Computer ohne Internetzugang können Sie einen Proxy oder OMS Protokoll Analytics Weiterleitung.

OMS mit Ihren Windows-Computern ist einfach 3 einfachen Schritten:

1. Agent-Setup-Datei herunterladen
2. Installieren Sie den Agenten mit der gewählten Methode
3. Konfigurieren Sie des Agenten oder fügen Sie zusätzlicher Arbeitsbereiche hinzu

Das folgende Diagramm zeigt die Beziehung zwischen Windows-Computern und OMS, nachdem Sie installiert und Agents konfiguriert haben.

![OMS-Direct-Agent-Diagramm](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Das System und die erforderliche Konfiguration
Überprüfen Sie vor der Installation oder Bereitstellung von Agenten Folgendes sicherstellen erforderlichen Anforderungen.

- Sie können nur die OMS MMA auf Computern unter Windows Server 2008 SP 1 oder höher oder Windows 7 SP1 oder höher.
- Sie benötigen ein OMS-Abonnement.  Weitere Informationen finden Sie unter [Erste Schritte mit Protokollanalyse](log-analytics-get-started.md).
- Jeder Windows-Computer muss über HTTPS mit dem Internet verbinden können. Diese Verbindung kann direkt über einen Proxy oder durch OMS Protokoll Analytics Weiterleitung.
- Sie können die OMS MMA auf eigenständigen Computern, Servern und virtuellen Maschinen. Wenn Sie OMS Azure gehosteten virtuellen Computer herstellen möchten, finden Sie unter [Verbinden Azure virtuelle Maschinen Protokollanalyse](log-analytics-azure-vm-extension.md).
- Der Agent muss TCP-Port 443 für verschiedene Ressourcen verwenden. Weitere Informationen finden Sie unter [Configure Proxy- und Firewall Settings in Protokollanalyse](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>OMS Agent-Setup-Datei herunterladen
1. Klicken Sie im Portal OMS auf **der Übersichtsseite** **Einstellungen** .  Klicken Sie oben die Registerkarte **Quellen verbunden** .  
    ![Registerkarte verbundenen Quellen](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Klicken Sie unter **Computer direkt anfügen** **Windows Agent herunterladen** auf Ihrem Computer Prozessortyp, die Setup-Datei.
3. Klicken Sie auf der rechten Seite des **Arbeitsbereichs-ID**auf das Symbol "Kopieren" und fügen Sie die ID in den Editor ein.
4. Klicken Sie auf der rechten Seite des **Primärschlüssels**auf das Symbol "Kopieren" und fügen Sie den Schlüssel in den Editor.     
    ![Arbeitsplatz-ID und Primärschlüssel kopieren](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>Installieren Sie den Agent mithilfe von setup
1. Führen Sie Setup aus, um den Agenten auf einem Computer installieren, den Sie verwalten möchten.
2. Klicken Sie auf der Seite Willkommen auf **Weiter**.
3. Lesen Sie auf der Seite Lizenzvertrag die Lizenz, und klicken Sie auf **ich stimme zu**.
4. Auf der Seite Zielordner ändern oder den Standardinstallationsordner beizubehalten, und klicken Sie auf **Weiter**.
5. Auf der Seite Agentenoptionen Setup können der Agent auf Azure Protokoll Analytics (OMS), Operations Manager, Verbindung oder können Sie die Optionen leer lassen möchten Sie den Agent später konfigurieren. Klicken Sie auf **Weiter**.   
    - Wenn Sie Verbindung zu Azure Protokoll Analytics (OMS), fügen Sie die **Arbeitsplatz-ID** und **Arbeitsbereich Schlüssel (Primary Key)** , den Sie in Editor in der vorherigen Prozedur kopiert und klicken Sie auf **Weiter**.  
        ![Arbeitsplatz-ID und Primärschlüssel einfügen](./media/log-analytics-windows-agents/connect-workspace.png)
    - Wenn Sie Operations Manager herstellen möchten, geben Sie **Namen**, **Verwaltungsservername** und **Management-Server-Port**und klicken Sie dann auf **Weiter**. Wählen Sie auf der Seite Agentaktionskonto das lokale Systemkonto oder ein lokales Domänenkonto, und klicken Sie dann auf **Weiter**.  
        ![Konfiguration](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![Agentaktionskonto](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Seite Installationsbereit überprüfen Sie Ihre Auswahl, und klicken Sie dann auf **Installieren**.
7. Die Konfiguration wurde erfolgreich abgeschlossen Seite, klicken Sie auf **Fertig stellen**.
8. Nach Abschluss des Vorgangs wird **Microsoft Monitoring Agent** im **Bedienfeld**angezeigt. Überprüfen Sie Ihre Konfiguration, und überprüfen Sie, ob der Agent auf operative Einblicke (OMS) verbunden ist. Der Agent bei OMS zeigt Meldung: **Microsoft Monitoring Agent hat Microsoft Operations Management Suite Service hergestellt.**

## <a name="install-the-agent-using-the-command-line"></a>Installieren Sie den Agenten über die Befehlszeile
- Ändern Sie, und verwenden Sie dabei den Agenten über die Befehlszeile installieren.

    >[AZURE.NOTE] Wenn Sie einen Agenten aktualisieren möchten, müssen Sie Protokollanalyse Skripting-API verwenden. Siehe nächster Abschnitt Agent aktualisieren.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>Den Agent, und fügen Sie einen Arbeitsbereich mithilfe eines Skripts
Aktualisieren ein Agenten und fügen Sie einen Arbeitsbereich mithilfe der Protokollanalyse Skripting-API mit dem folgenden PowerShell-Beispiel.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Wenn Sie haben die Befehlszeile oder Skript zuvor installieren oder Konfigurieren des Agents `EnableAzureOperationalInsights` wurde `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Installieren Sie den Agenten mit DSC in Azure Automation

>[AZURE.NOTE] Dieses Beispiel Verfahren und Skripts aktualisiert einen vorhandenen Agent nicht.

1. Azure Automation importieren Sie xPSDesiredStateConfiguration DSC-Modul aus [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) .  
2.  Erstellen Sie Azure Automation Variable Anlagen für *OPSINSIGHTS_WS_ID* und *OPSINSIGHTS_WS_KEY*. Legen Sie *OPSINSIGHTS_WS_ID* OMS-Protokollanalyse Workspace verschlüsselt und *OPSINSIGHTS_WS_KEY* auf den Primärschlüssel des Arbeitsbereichs.
3.  Verwenden Sie das folgende Skript, und speichern Sie sie als MMAgent.ps1
4.  Ändern Sie, und verwenden Sie dabei zum Installieren des Agenten mit DSC in Azure Automation. Importieren Sie MMAgent.ps1 in Azure mithilfe von Azure Automatisierungsschnittstelle oder Cmdlets.
5.  Die Konfiguration ein Knotens zuweisen. Innerhalb von 15 Minuten Überprüfen des Knotens seine Konfiguration und MMA dem Knoten abgelegt.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Konfigurieren Sie einen Agent manuell, oder fügen Sie zusätzliche Arbeitsbereiche
Wenn Sie Agents installiert haben, aber nicht sie konfiguriert haben oder wenn Sie Agent an mehrere Arbeitsbereiche, können Sie die folgende Informationen einen Agent aktivieren oder neu konfigurieren. Nachdem der Agent konfiguriert haben, wird mit der Agent-Dienst registriert und erhalten erforderlichen Konfigurationsinformationen und managementpacks, die Informationen enthalten.

1. **Nach der Installation der Microsoft Monitoring Agent Systemsteuerung.**
2. Öffnen Sie **Microsoft Monitoring Agent** und klicken Sie dann auf die Registerkarte **Azure Protokoll Analytics (OMS)** .   
3. Klicken Sie auf **Hinzufügen** , um das **Protokoll Analytics Arbeitsbereich hinzufügen** zu öffnen.
4. Fügen Sie die **Arbeitsplatz-ID** und **Arbeitsbereich Schlüssel (Primärschlüssel)** in den Editor ein zuvor für den Arbeitsbereich, die Sie hinzufügen möchten kopieren und klicken Sie dann auf **OK**.  
    ![Betriebliche Informationen konfigurieren](./media/log-analytics-windows-agents/add-workspace.png)

Nach Computer überwacht der Agent Daten wird die Anzahl der Computer, die von OMS überwacht im OMS-Portal auf der Registerkarte **Verbundene Datenquellen** **Einstellungen** als **Server verbunden**angezeigt.


## <a name="to-disable-an-agent"></a>So deaktivieren Sie einen agent
1. **Nach der Installation des Agents Systemsteuerung.**
2. Öffnen Sie Microsoft Monitoring Agent und klicken Sie dann auf die Registerkarte **Azure Protokoll Analytics (OMS)** .
3. Wählen Sie Arbeitsbereich aus, und klicken Sie dann auf **Entfernen**. Wiederholen Sie diesen Schritt für alle Arbeitsbereiche.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Konfigurieren Sie optional an eine Verwaltungsgruppe Operations Manager-agents

Wenn Sie Operations Manager in Ihrer IT-Infrastruktur verwenden, können Sie auch MMA-Agent als Operations Manager-Agent.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>MMA-Agenten an einer Verwaltungsgruppe Operations Manager konfigurieren
1.  Öffnen Sie auf dem Computer, auf dem der Agent installiert, **Bedienfeld**.
2.  Öffnen Sie **Microsoft Monitoring Agent** , und klicken Sie auf der Registerkarte **Operations Manager** .
    ![Microsoft Monitoring Agent Operations Manager-Registerkarte](./media/log-analytics-windows-agents/om-mg01.png)
3.  Die Operations Manager-Server Integration in Active Directory, klicken Sie auf **Gruppe Führungsaufgaben aus AD DS automatisch aktualisiert**.
4.  Klicken Sie auf **Hinzufügen** , um das Dialogfeld **Hinzufügen einer Verwaltungsgruppe** .  
    ![Microsoft Monitoring Agent eine Verwaltungsgruppe hinzufügen](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  Geben Sie im **Verwaltungsgruppenname** den Namen der Verwaltungsgruppe.
6.  Geben Sie im Feld **Primärer Verwaltungsserver** den Namen des primären Verwaltungsserver.
7.  Geben Sie im **Server-Verwaltungsanschluss** TCP-Portnummer.
8.  Wählen Sie unter **Agentaktionskonto über**das lokale Systemkonto oder ein lokales Domänenkonto.
9.  Klicken Sie auf **OK** zum Schließen **einer Verwaltungsgruppe hinzufügen** und klicken Sie dann auf **OK** , um das Dialogfeld **Eigenschaften von Microsoft Monitoring Agent** schließen.

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Konfigurieren Sie optional Agents OMS Protokoll Analytics Weiterleitung verwenden

Wenn Sie Server oder Clients ohne eine Verbindung zum Internet haben, können sie Daten mithilfe von OMS Protokoll Analytics Weiterleitung an OMS senden noch.  Wenn Sie die Weiterleitung verwenden, werden alle Daten von Agents durch einen einzelnen Server mit Zugriff auf das Internet gesendet. Die Weiterleitung überträgt Daten von den Agents an OMS direkt ohne Analyse der Daten, die übertragen werden.

Siehe [OMS Protokoll Analytics Weiterleitung](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) über die Weiterleitung, einschließlich Installation und Konfiguration informieren.

Informationen zum Konfigurieren der Agents um einen Proxyserver verwenden, der in diesem Fall die OMS-Weiterleitung, finden Sie unter [Proxyserver und Firewall in Protokollanalyse konfigurieren](log-analytics-proxy-firewall.md).

## <a name="optionally-configure-proxy-and-firewall-settings"></a>Optional konfigurieren Sie Proxy- und firewall
Wenn Proxyservern oder Firewalls in Ihrer Umgebung, die Zugriff auf das Internet haben, finden Sie unter [Configure Proxy- und Firewall Settings in Protokollanalyse](log-analytics-proxy-firewall.md) OMS-Dienst kommunizieren die Agenten aktivieren.

## <a name="next-steps"></a>Nächste Schritte

- [Protokollanalyse hinzufügen Lösungen Lösungskatalog](log-analytics-add-solutions.md) Funktionen und Daten.
- [Configure Proxy- und Firewall Settings in Protokollanalyse](log-analytics-proxy-firewall.md) verwendet Ihre Organisation einen Proxyserver oder eine Firewall, dass Agenten mit der Protokollanalyse Dienst kommunizieren können.

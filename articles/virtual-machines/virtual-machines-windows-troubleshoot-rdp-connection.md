<properties
    pageTitle="Kann keine RDP Azure VM | Microsoft Azure"
    description="Problembehandlung bei der virtuellen Windows-Maschine in Azure mithilfe von Remotedesktop Verbindung nicht möglich"
    keywords="Remote desktop-Fehler, Fehler Remotedesktopverbindung kann keine Verbindung mit VM Remotedesktop Problembehandlung"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Problembehandlung bei Remotedesktopverbindungen Azure virtuellen Computer

Die Remote Desktop Protocol (RDP) Verbindung zu Ihrem Windows-basierten Azure Virtual Machine (VM) kann aus verschiedenen Gründen, so dass Sie auf die VM fehl. Das Problem kann mit den Remotedesktopdienst auf die VM-Verbindung oder Remote Desktop Client auf dem Hostcomputer. Dieser Artikel führt Sie durch einige der häufigsten Methoden RDP-Verbindungsprobleme zu beheben. 

Benötigen Sie weitere Hilfe zu diesem Artikel erhalten Sie Azure-Experten auf [MSDN Azure und Foren Stapelüberlauf](https://azure.microsoft.com/support/forums/). Alternativ können Sie eine Azure Supportfall Datei. Die [Azure support-Website](https://azure.microsoft.com/support/options/) , und wählen Sie **Unterstützung erhalten**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Schritte zur Fehlerbehebung
Versuchen Sie nach jedem Schritt zur Problembehandlung VM wiederherstellen:

1. Remote Desktop-Konfiguration zurückgesetzt.
2. Überprüfen Sie Netzwerk-Sicherheitsgruppe Regeln / Cloud Services-Endpunkte.
3. Überprüfen Sie VM-Konsole.
4. Überprüfen der VM Ressource.
5. VM-Kennwort zurücksetzen.
6. Starten Sie die virtuellen Computer.
7. Die VM erneut.

Lesen Sie ggf. Weitere Schritte und erläutern.

> [AZURE.TIP] Wenn die Taste **Connect** für die VM im Portal abgeblendet und Sie sind mit Azure ein [Express Route](../expressroute/expressroute-introduction.md) oder [Standort-zu-Standort-VPN-](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) Verbindung, müssen Sie zu VM eine öffentliche IP-Adresse zuweisen, bevor Sie RDP verwenden können. Erfahren Sie mehr über [öffentliche IP-Adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Arten von RDP-Problemen
Sie können VMs erstellt, mit dem Ressourcen-Manager-Bereitstellungsmodell mithilfe einer der folgenden Methoden beheben:

- [Azure-Portal](#using-the-azure-portal) – Wenn Sie die RDP-Konfiguration oder Anmeldeinformationen und schnell zurücksetzen müssen haben nicht Azure Tools installiert.
- [Azure PowerShell](#using-azure-powershell) - Wenn Sie mit einer Aufforderung PowerShell vertraut sind schnell die RDP Konfiguration oder Anmeldeinformationen zurücksetzen mithilfe der Azure-PowerShell-Cmdlets.

Sie finden auch Schritte zur Behebung von VMs mit [klassischen Bereitstellungsmodell](#troubleshoot-vms-created-using-the-classic-deployment-model)erstellt.


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Problembehandlung bei der Verwendung des Azure-Portals
Versuchen Sie nach jedem Schritt zur Problembehandlung die VM erneut. Wenn Sie weiterhin keine Verbindung herstellen können, versuchen Sie den nächsten Schritt.

1. **RDP-Verbindung zurücksetzen**. Dieser Schritt zur Problembehandlung setzt RDP-Konfiguration bei Remoteverbindungen deaktiviert oder die Windows-Firewall-Regeln RDP, beispielsweise.

    Wählen Sie die VM in Azure-Portal. Scrollen Sie Einstellungsbereich Abschnitt **Support + Problembehandlung** unteren Bereich der Liste. Klicken Sie auf die Schaltfläche **Kennwort zurücksetzen** . Legen Sie den **Modus** **nur Konfiguration** zurücksetzen und klicken Sie dann auf die Schaltfläche **Aktualisieren** :

    ![Setzen Sie die RDP-Konfiguration im Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Überprüfen Sie Netzwerk-Sicherheitsgruppe Regeln**. Dieser Schritt zur Problembehandlung überprüft haben Sie eine Regel in der Netzwerk-Sicherheitsgruppe den RDP-Datenverkehr zulässt. Der Standardport für RDP ist TCP-Port 3389. Beim Erstellen Ihrer VM kann Regel gestatten RDP-Datenverkehr nicht automatisch erstellt.

    Wählen Sie die VM in Azure-Portal. Klicken Sie auf die **Netzwerkschnittstellen** Einstellungsbereich.

    ![Netzwerkschnittstellen für eine VM in Azure-Portal anzeigen](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Wählen Sie die Netzwerkschnittstelle aus (es ist in der Regel nur eine):

    ![Wählen Sie die Netzwerkschnittstelle im Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Wählen Sie **Network Security Group** an der Netzwerk-Sicherheitsgruppe der Netzwerkschnittstelle zugeordnet:

    ![Wählen Sie Netzwerk-Sicherheitsgruppe in Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Stellen Sie sicher, dass eine eingehende Regel vorhanden ist, die RDP-Datenverkehr über TCP-Port 3389 zulässt. Das folgende Beispiel zeigt eine gültige Sicherheitsregel, die RDP-Datenverkehr zulässt. Sehen Sie `Service` und `Action` richtig konfiguriert sind:

    ![NSG RDP-Regel im Azure-Portal überprüfen](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Wenn keine Regel gibt ermöglicht, [Erstellen Sie eine Sicherheitsgruppe Netzwerk Regel](virtual-machines-windows-nsg-quickstart-portal.md)RDP-Datenverkehr. TCP-Port 3389 zulassen

3. **Überprüfung VM Boot Diagnose**. Dieser Schritt zur Problembehandlung werden VM konsolenprotokolle, um festzustellen, ob die VM ein Problem meldet. Nicht alle VMs haben Boot Diagnose aktiviert, dieser Schritt zur Problembehandlung sind optional.
    
    Spezifische Schritte zur Problembehandlung den Rahmen dieses Artikels sprengen, aber möglicherweise größeren Problem, das RDP-Konnektivität auswirken. Weitere Informationen zum Überprüfen der Konsole anmeldet und VM Screenshot anzeigen Sie [Boot Diagnose für VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)

4. **Die VM-Ressource überprüfen**. Dieser Schritt zur Problembehandlung überprüft, gibt es keine Probleme mit der Azure-Plattform, die Verbindung zur VM auswirken können.

    Wählen Sie die VM in Azure-Portal. Scrollen Sie Einstellungsbereich Abschnitt **Support + Problembehandlung** unteren Bereich der Liste. Klicken Sie auf **Ressource Gesundheit** . Eine gesunde VM Berichte als **verfügbar**:

    ![Überprüfen Sie VM Ressource im Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. **Anmeldeinformationen zurücksetzen**. Dieser Schritt zur Problembehandlung setzt das Kennwort für ein lokales Administratorkonto, wenn Sie nicht sicher sind oder die Anmeldeinformationen vergessen haben.

    Wählen Sie die VM in Azure-Portal. Scrollen Sie Einstellungsbereich Abschnitt **Support + Problembehandlung** unteren Bereich der Liste. Klicken Sie auf die Schaltfläche **Kennwort zurücksetzen** . Sicherstellen Sie, dass der **Modus** zum **Passwort** festgelegt ist, und geben Sie dann Ihren Benutzernamen und das neue Kennwort. Klicken Sie abschließend auf die Schaltfläche **Aktualisieren** :

    ![Die Benutzeranmeldeinformationen in Azure-Portal zurücksetzen](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **VM starten**. Dieser Schritt zur Problembehandlung kann alle zugrunde liegenden Probleme korrigieren, die VM selbst.

    Wählen Sie VM in Azure-Portal, und klicken Sie auf der Registerkarte **Überblick** . Klicken Sie auf **neu starten** :

    ![Starten Sie die VM in Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Die VM erneut**. Dieser Schritt zur Problembehandlung vorhandene die VM auf einem anderen Host in Azure zugrunde liegenden Plattform oder Netzwerkproblemen.

    Wählen Sie die VM in Azure-Portal. Scrollen Sie Einstellungsbereich Abschnitt **Support + Problembehandlung** unteren Bereich der Liste. Klicken Sie **erneut** , und klicken Sie dann auf **erneut**:

    ![Erneutes Bereitstellen der VM in Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Nach Abschluss dieses Vorgangs flüchtigen Daten verloren und dynamische IP-Adressen der VM zugeordnet werden.

Wenn RDP Probleme weiterhin auftritt, können [eine Support-Anfrage öffnen](https://azure.microsoft.com/support/options/) oder Lesen Sie [Weitere Informationen zur Problembehandlung RDP-Konzepte und Schritte](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Problembehandlung bei der Verwendung von Azure PowerShell
Wenn nicht bereits geschehen, [Installieren und Konfigurieren der neuesten Azure PowerShell](../powershell-install-configure.md).

Die folgenden Beispiele verwenden Variablen wie `myResourceGroup`, `myVM`, und `myVMAccessExtension`. Ersetzen Sie diese Namen und Speicherorte mit Ihren eigenen Werten.

> [AZURE.NOTE] Zurücksetzen der Benutzeranmeldeinformationen und die RDP-Konfiguration mithilfe des [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell-Cmdlets. In den folgenden Beispielen `myVMAccessExtension` ist ein Name, den Sie als Teil des Prozesses angeben. Wenn Sie bereits mit der VMAccessAgent gearbeitet haben, erhalten Sie den Namen der vorhandenen Erweiterung mit `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` die Eigenschaften des virtuellen Computers überprüfen. Um den Namen anzuzeigen, suchen Sie im Abschnitt "Extensions" der Ausgabe.

Versuchen Sie nach jedem Schritt zur Problembehandlung die VM erneut. Wenn Sie weiterhin keine Verbindung herstellen können, versuchen Sie den nächsten Schritt.

1. **RDP-Verbindung zurücksetzen**. Dieser Schritt zur Problembehandlung setzt RDP-Konfiguration bei Remoteverbindungen deaktiviert oder die Windows-Firewall-Regeln RDP, beispielsweise.

    Das folgende Beispiel setzt RDP-Verbindung auf einem virtuellen Computer mit dem Namen `myVM` in der `WestUS` Ort und in der Ressourcengruppe mit dem Namen `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Überprüfen Sie Netzwerk-Sicherheitsgruppe Regeln**. Dieser Schritt zur Problembehandlung überprüft haben Sie eine Regel in der Netzwerk-Sicherheitsgruppe den RDP-Datenverkehr zulässt. Der Standardport für RDP ist TCP-Port 3389. Beim Erstellen Ihrer VM kann Regel gestatten RDP-Datenverkehr nicht automatisch erstellt.

    Zunächst weisen alle Konfigurationsdaten für die Netzwerk-Sicherheitsgruppe, die `$rules` Variable. Das folgende Beispiel ruft Informationen über den Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup` in der Ressourcengruppe mit dem Namen `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Sehen Sie die Regeln für diese Sicherheitsgruppe Netzwerk konfiguriert sind. Überprüfen des Vorhandenseins eine Regel zum TCP-Port 3389 für eingehende Verbindungen zu gestatten:

    ```powershell
    $rules.SecurityRules
    ```

    Das folgende Beispiel zeigt eine gültige Sicherheitsregel, die RDP-Datenverkehr zulässt. Sehen Sie `Protocol`, `DestinationPortRange`, `Access`, und `Direction` richtig konfiguriert sind:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Wenn keine Regel gibt ermöglicht, [Erstellen Sie eine Sicherheitsgruppe Netzwerk Regel](virtual-machines-windows-nsg-quickstart-powershell.md)RDP-Datenverkehr. TCP-Port 3389 zulassen

3. **Anmeldeinformationen zurücksetzen**. Dieser Schritt zur Problembehandlung setzt das Kennwort für das lokale Administratorkonto, das Sie angeben, wenn Sie unsicher sind oder vergessen haben die Anmeldeinformationen.

    Zuerst den Benutzernamen und das neue Kennwort durch Zuweisen von Anmeldeinformationen angeben der `$cred` Variable wie folgt:

    ```powershell
    $cred=Get-Credential
    ```

    Aktualisieren Sie die Anmeldeinformationen auf Ihre virtuellen Computer jetzt. Das folgende Beispiel aktualisiert die Anmeldeinformationen für einen virtuellen Computer mit dem Namen `myVM` in der `WestUS` Ort und in der Ressourcengruppe mit dem Namen `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **VM starten**. Dieser Schritt zur Problembehandlung kann alle zugrunde liegenden Probleme korrigieren, die VM selbst.

    Im folgende Beispiel startet die VM mit dem Namen `myVM` in der Ressourcengruppe mit dem Namen `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Die VM erneut**. Dieser Schritt zur Problembehandlung vorhandene die VM auf einem anderen Host in Azure zugrunde liegenden Plattform oder Netzwerkproblemen.

    Im folgende Beispiel vorhandene Bezeichnung VM `myVM` in der `WestUS` Speicherort und die Ressourcengruppe mit dem Namen `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Wenn RDP Probleme weiterhin auftritt, können [eine Support-Anfrage öffnen](https://azure.microsoft.com/support/options/) oder Lesen Sie [Weitere Informationen zur Problembehandlung RDP-Konzepte und Schritte](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Problembehandlung bei VMs mit klassischen Bereitstellungsmodell erstellt

Versuchen Sie nach jedem Schritt zur Problembehandlung VM wiederherstellen.

1. **RDP-Verbindung zurücksetzen**. Dieser Schritt zur Problembehandlung setzt RDP-Konfiguration bei Remoteverbindungen deaktiviert oder die Windows-Firewall-Regeln RDP, beispielsweise.

    Wählen Sie die VM in Azure-Portal. Klicken Sie auf das **... Weitere** und dann auf **RAS zurücksetzen**:

    ![Setzen Sie die RDP-Konfiguration im Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Endpunkte Clouddienste überprüfen**. Dieser Schritt zur Problembehandlung überprüft haben Sie Endpunkte in der Cloud Services den RDP-Datenverkehr zulässt. Der Standardport für RDP ist TCP-Port 3389. Beim Erstellen Ihrer VM kann Regel gestatten RDP-Datenverkehr nicht automatisch erstellt.

    Wählen Sie die VM in Azure-Portal. Klicken Sie auf **Endpunkte** , um die Zeit für die VM Endpunkte anzeigen. Überprüft, ob Endpunkte vorhanden, die RDP-auf TCP-Port 3389 Datenverkehr.
    
    Das folgende Beispiel zeigt gültige Endpunkte, die RDP-Datenverkehr zu ermöglichen:

    ![Überprüfen von Clouddiensten Endpunkte im Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Wenn Sie einen Endpunkt keinen ermöglicht, RDP-Datenverkehr, [Erstellen Sie eine Cloud-Diensten](virtual-machines-windows-classic-setup-endpoints.md). Privater Port 3389 erlauben Sie TCP.

3. **Überprüfung VM Boot Diagnose**. Dieser Schritt zur Problembehandlung werden VM konsolenprotokolle, um festzustellen, ob die VM ein Problem meldet. Nicht alle VMs haben Boot Diagnose aktiviert, dieser Schritt zur Problembehandlung sind optional.
    
    Spezifische Schritte zur Problembehandlung den Rahmen dieses Artikels sprengen, aber möglicherweise größeren Problem, das RDP-Konnektivität auswirken. Weitere Informationen zum Überprüfen der Konsole anmeldet und VM Screenshot anzeigen Sie [Boot Diagnose für VMs](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)

4. **Die VM-Ressource überprüfen**. Dieser Schritt zur Problembehandlung überprüft, gibt es keine Probleme mit der Azure-Plattform, die Verbindung zur VM auswirken können.

    Wählen Sie die VM in Azure-Portal. Scrollen Sie Einstellungsbereich Abschnitt **Support + Problembehandlung** unteren Bereich der Liste. Klicken Sie auf **Ressource Gesundheit** . Eine gesunde VM Berichte als **verfügbar**:

    ![Überprüfen Sie VM Ressource im Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. **Anmeldeinformationen zurücksetzen**. Dieser Schritt zur Problembehandlung setzt das Kennwort für das lokale Administratorkonto, das Sie angeben, wenn Sie nicht sicher sind oder die Anmeldeinformationen vergessen haben.

    Wählen Sie die VM in Azure-Portal. Scrollen Sie Einstellungsbereich Abschnitt **Support + Problembehandlung** unteren Bereich der Liste. Klicken Sie auf die Schaltfläche **Kennwort zurücksetzen** . Geben Sie Ihren Benutzernamen und das neue Kennwort ein. Klicken Sie abschließend auf die Schaltfläche **Speichern** :

    ![Die Benutzeranmeldeinformationen in Azure-Portal zurücksetzen](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **VM starten**. Dieser Schritt zur Problembehandlung kann alle zugrunde liegenden Probleme korrigieren, die VM selbst.

    Wählen Sie VM in Azure-Portal, und klicken Sie auf der Registerkarte **Überblick** . Klicken Sie auf **neu starten** :

    ![Starten Sie die VM in Azure-portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Wenn RDP Probleme weiterhin auftritt, können [eine Support-Anfrage öffnen](https://azure.microsoft.com/support/options/) oder Lesen Sie [Weitere Informationen zur Problembehandlung RDP-Konzepte und Schritte](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Problembehandlung bei Fehlermeldungen RDP
Eine bestimmte Fehlermeldung auftreten, wenn die VM über RDP eine Verbindung herstellen möchten. Es folgen die häufigsten Fehlermeldungen:

- [Die Remotesitzung wurde abgebrochen, weil keine Remote Desktop-Lizenzserver eine Lizenz verfügbar sind](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Remote Desktop Computer "Name" kann nicht gefunden werden](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- [Eine Authentifizierung aufgetreten. Die lokale Sicherheitsautorität kann keine Verbindung hergestellt](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Windows Sicherheitsfehler: Ihre Anmeldeinformationen funktioniert nicht](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Dieser Computer kann keine Verbindung mit dem Remotecomputer](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Zusätzliche Ressourcen
Wenn keiner dieser Fehler aufgetreten ist und noch keine zu den virtuellen Computer über Remote Desktop Verbindung, lesen Sie die detaillierte [Fehlerbehebung für Remotedesktop](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure Diagnosepaket IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Problembehandlungsschritte zugreifen auf einer VM ausgeführt, finden Sie unter [Problembehandlung bei Zugriff auf eine Anwendung auf eine Azure-VM](virtual-machines-linux-troubleshoot-app-connection.md).
- Bei Problemen mit Secure Shell (SSH) Herstellen einer Linux VM in Azure finden Sie unter [Problembehandlung SSH-Verbindung zu Linux VM in Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).

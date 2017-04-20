Gehen Sie folgendermaßen vor um ein VNet in der Ressourcen-Manager-Bereitstellungsmodell mithilfe des Azure-Portals. Die Screenshots werden als Beispiele bereitgestellt. Achten Sie darauf, dass Sie die Werte ändern. Weitere Informationen zum Arbeiten mit virtuellen Netzwerken finden Sie unter [Übersicht über Virtual Network](../articles/virtual-network/virtual-networks-overview.md).

1. In einem Browser navigieren Sie zum [Azure-Portal](http://portal.azure.com) und melden Sie sich ggf. mit Ihrem Azure-Konto.

2. **Klicken Sie auf.** Geben Sie in das Suchfeld **am Markt** "Virtuelle Netzwerk". Suchen Sie **Virtuelles Netzwerk** aus der zurückgegebenen Liste, und klicken Sie zum Öffnen von **Virtual Network** -Blade.

    ![Virtuelles Netzwerk suchen Ressource blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Suchen nach virtuellen Netzwerk Ressource blade")

3. Unteren Blade virtuelles Netzwerk aus der Liste **Wählen Sie ein Bereitstellungsmodell** wählen Sie **Ressourcenmanager aus**und dann auf **Erstellen**.


    ![Ressourcen-Manager aktivieren] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Ressourcen-Manager aktivieren")

4. Konfigurieren Sie auf VNet,-Blade **virtuelles Netzwerk erstellen** . Beim Ausfüllen der Felder wird das rote Ausrufezeichen ein Häkchen in das Feld eingegebenen Zeichen gültig sind.

    ![Überprüfung] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Überprüfung")

5. Im folgenden Beispiel ähnelt das Blade **virtuelles Netzwerk erstellen** . Möglicherweise werden Werte automatisch ausgefüllt. In diesem Fall ersetzen Sie die Werte durch Ihren eigenen.

    ![Erstellen virtuelle Netzwerk blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Erstellen virtuelle Netzwerk blade")

6. **Name**: Geben Sie den Namen für das virtuelle Netzwerk.

7. **Adressraum**: Geben Sie den Adressbereich. Haben Sie mehrere Adressräume hinzufügen, fügen Sie Ihre ersten Adressraum hinzu. Nach dem Erstellen des Vnets können Sie zusätzliche Adressräume später hinzufügen.
 
8. **Subnet-Name**: Subnetznamen und Subnetz Adressbereich hinzufügen. Sie können später weitere Subnetze hinzufügen, nach dem Erstellen des Vnets.

10. **Abonnement**: Überprüfen, ob die richtige Abonnement aufgeführt ist. Mithilfe der Dropdownliste können Sie Abonnements ändern.

11. **Gruppe**: Wählen Sie eine vorhandene Ressourcengruppe oder eine neue erstellen einen Namen für die neue Ressourcengruppe eingeben. Beim Erstellen einer neuen Gruppe name der Ressourcengruppe nach geplanten Konfigurationswerte. Weitere Informationen zu Ressourcengruppen Überblick [Azure Ressourcen-Manager](resource-group-overview.md#resource-groups).

12. **Ort**: Wählen Sie den Speicherort für das VNet. Die Position bestimmt die Bereitstellung dieser VNet Ressourcen befinden werden.

13. Wählen Sie **Pin Dashboard** , möchten Sie möglicherweise finden das VNet im Schaltpult, und klicken Sie auf **Erstellen**.
    
    ![PIN zum dashboard] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "PIN zum dashboard")

14. Nach dem Klicken auf **Erstellen**, sehen Sie eine Kachel auf dem Dashboard, die den Status der VNet widerspiegeln. Die Kachel ändert das VNet erstellt wird.

    ![Virtuelles Netzwerk erstellen nebeneinander] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Virtuelles Netzwerk erstellen nebeneinander")
## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Erstellen Sie eine klassische VNet in Azure-portal

Gehen Sie folgendermaßen vor um eine klassische VNet basierend auf dem Szenario zu erstellen.

1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie auf **neu** > **Netzwerk** > **virtuellen Netzwerk**, beachten Sie, dass die Liste **Wählen Sie ein Bereitstellungsmodell** bereits zeigt **klassische**und klicken Sie auf **Erstellen**, wie in der folgenden Abbildung dargestellt.

    ![VNet in Azure-Portal erstellen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Geben Sie auf den **virtuellen Netzwerk** den **Namen** der VNet und dann auf **Adressraum**. Konfigurieren Sie Ihre Adresse Speicherplatz für das VNet und seine erste Subnetz und dann auf **OK**. Die nachfolgende Abbildung zeigt die CIDR-Block Einstellungen Szenario.

    ![Adresse Speicherplatz blade](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Klicken Sie auf **Ressource** und klicken Sie auf **neue Ressourcengruppe erstellen** das VNet eine neue Ressourcengruppe hinzufügen VNet zum Hinzufügen wählen. Die nachfolgende Abbildung zeigt die Ressource Einstellungen für neue Ressourcengruppe **TestRG**aufgerufen. Weitere Informationen zu Ressourcengruppen Überblick [Azure Ressourcen-Manager](../articles/virtual-network/resource-group-overview.md#resource-groups).

    ![Blatt Ressource erstellen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. Ggf. ändern Sie das **Abonnement** und den **Speicherort** für das VNet. 

6. Wenn nicht das VNet als ein Element im **Startmenü**angezeigt werden soll, deaktivieren Sie **an Startmenü anheften**. 

7. Klicken Sie auf **Erstellen** , und beachten Sie die Kachel mit dem Namen **Erstellen virtuelle Netzwerk** , wie in der folgenden Abbildung dargestellt.

    ![VNet im Portal erstellen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Warten Sie VNet erstellt werden, und wenn die Kachel unten angezeigt wird, klicken Sie auf Weitere Subnetze hinzugefügt.

    ![VNet im Portal erstellen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. Die **Konfiguration** sollte für das VNet angezeigt werden, wie unten dargestellt. 

    ![VNet im Portal erstellen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Klicken Sie auf **Subnets** > **Hinzufügen**, geben Sie einen **Namen** und geben Sie einen **Adressbereich (CIDR-Block)** für Ihr Subnetz und klicken Sie dann auf **OK**. Die folgende Abbildung zeigt die Einstellungen für das aktuelle Szenario.

    ![VNet in Azure-Portal erstellen](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)
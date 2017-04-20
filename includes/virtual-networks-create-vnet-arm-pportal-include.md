## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Erstellen Sie ein VNet in Azure-portal

Erstellen Sie ein VNet auf Grundlage dieses Szenarios über mithilfe von Azure vorschauportal wie folgt.

1. Ein Browser, navigieren Sie zu http://portal.azure.com und melden Sie sich ggf. mit Ihrem Azure-Konto.
2. Klicken Sie auf **neu** > **Netzwerk** > **virtuellen Netzwerk**aus der Liste **Wählen Sie ein Bereitstellungsmodell** **Ressourcenmanager** klicken und klicken Sie auf **Erstellen**, wie in der folgenden Abbildung dargestellt.

    ![VNet in Azure-Portal erstellen](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. Konfigurieren Sie auf die **virtuelles Netzwerk erstellen** die VNet wie in der folgenden Abbildung dargestellt.

    ![Virtuelles Netzwerk Blade erstellen](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Klicken Sie auf **Gruppe** und wählen Sie VNet zum Hinzufügen oder klicken Sie auf **neu erstellen** , um die VNet eine neue Ressourcengruppe hinzufügen. Die nachfolgende Abbildung zeigt die Ressource Einstellungen für neue Ressourcengruppe **TestRG**aufgerufen. Weitere Informationen zu Ressourcengruppen Überblick [Azure Ressourcen-Manager](../articles/resource-group-overview.md#resource-groups).

    ![Ressourcengruppe](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. Ggf. ändern Sie das **Abonnement** und den **Speicherort** für das VNet. 

6. Wenn nicht das VNet als ein Element im **Startmenü**angezeigt werden soll, deaktivieren Sie **an Startmenü anheften**. 

7. Klicken Sie auf **Erstellen** , und beachten Sie die Kachel mit dem Namen **Erstellen virtuelle Netzwerk** , wie in der folgenden Abbildung dargestellt.

    ![Virtuelles Netzwerk Kachel erstellen](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Warten auf VNet erstellt werden und **virtuellen Netzwerk** Blatt klicken **Alle** > **Subnetze** > **Hinzufügen** , wie unten dargestellt.

    ![Subnetz im Azure-Portal hinzufügen](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Geben Sie Subnetz Einstellungen für *Back-End-* Subnetz an, wie unten dargestellt, und klicken Sie auf **OK**. 

    ![Subnetz settings](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. Beachten Sie die Liste der Subnetze, wie in der folgenden Abbildung dargestellt.

    ![Liste der Subnetze in VNet](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
